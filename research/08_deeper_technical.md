# Seed PRD v2.1 — Second-Pass Deeper Technical Review

## Critical issues in v2.1

### 1. Encryption-key bootstrap (Proxy + async initMMKV) will crash app at first launch

**Severity: CRITICAL.** §6.5 sets `mmkv` as a `Proxy` that throws if accessed before `initMMKV()` resolves, then asks `app/_layout.tsx` to `await initMMKV()` before render. Broken because:

1. Zustand v5 `persist` hydrates synchronously at `create()` time when storage is synchronous. Per Zustand docs: *"If the given storage is synchronous, hydration will be done synchronously … the store will already have been hydrated at its creation."*
2. `create()` runs at module-import time, not render time. The Proxy throws before `await initMMKV()` ever runs.

**Fix (recommended):** Synchronous bootstrap. `expo-secure-store` exposes synchronous `getItem` since SDK 51; `expo-crypto` exposes synchronous `getRandomBytes` since SDK 51:

```typescript
function bootstrapKey(): string {
  let key = SecureStore.getItem(KEY_NAME);
  if (!key) {
    const bytes = Crypto.getRandomBytes(32);
    key = bytesToBase64(bytes);
    SecureStore.setItem(KEY_NAME, key, {
      keychainAccessible: SecureStore.WHEN_UNLOCKED_THIS_DEVICE_ONLY,
    });
  }
  return key;
}
export const mmkv = new MMKV({ id: 'seed-storage', encryptionKey: bootstrapKey() });
```

### 2. Buffer import won't bundle

v2.1 §6.5 line 1374: `import { Buffer } from 'buffer';` — `buffer` is NOT polyfilled by SDK 55; the package is not in `package.json`. Bundle will fail.

**Fix:** use `globalThis.btoa` (Hermes provides it globally since RN 0.72).

### 3. AFTER_FIRST_UNLOCK Keychain accessibility too loose

For an offline-only app, the encryption key MUST stay device-local. AFTER_FIRST_UNLOCK permits iCloud Keychain sync. Use `WHEN_UNLOCKED_THIS_DEVICE_ONLY`.

### 4. expo-print tagged-PDF requirement physically infeasible

`expo-print` on iOS uses WKWebView → UIPrintPageRenderer; on Android uses WebView.createPrintDocumentAdapter. **Neither path produces a tagged (PDF/UA) PDF.** Putting `<h1>` in HTML doesn't propagate to PDF metadata.

**Fix:** Downgrade SED-F07-009 to "semantic HTML + plain-text alternative". True tagged PDF deferred to v1.0.x.

### 5. PBKDF2 100k iterations below 2025 OWASP minimum

OWASP 2025 minimum for SHA-256-PBKDF2 is 600k. v2.1 uses 100k. Pragmatic compromise: 200k (completes <1s on Galaxy A14).

### 6. Backup crypto library not specified

Pure-JS PBKDF2 at 100k iterations on Galaxy A14 takes 8–15s — unacceptable UX. Recommend `react-native-aes-gcm-crypto` (Tectiv3/Craftzdog) — native, New-Arch-ready.

## v2.2 architecture validations

- Zustand v5 + `createJSONStorage` pattern: ✓ correct
- EAS Update disabled (`expo-updates` not installed): ✓ correct
- `NSUserTrackingUsageDescription` removed: ✓ correct
- 16 KB page-size + NDK r27 via `expo-build-properties`: ✓ correct
- `POST_NOTIFICATIONS` in Android permissions: ✓ correct
- Notification channel created before scheduling: ✓ correct
- `newArchEnabled: true` explicit: ✓ correct
- `expo-secure-store` plugin added: ✓ correct

## Bundle size reality check

| Component | Compressed APK estimate |
|---|---|
| Hermes runtime | 3.5–4.0 MB |
| RN core + Fabric | 3.0–3.5 MB |
| Expo modules | 4.0–5.0 MB |
| Native modules (MMKV, aes-gcm, charts, reanimated, IAP) | 4.0–5.0 MB |
| Hermes bytecode | 1.5–2.2 MB |
| Static content JSON | 1.5–2.5 MB |
| Illustrations (SVG) | 0.5 MB |
| Native libs | 1.5–2.0 MB |
| Assets | 0.4–0.8 MB |
| **Total compressed APK** | **≈19–25 MB** |

Fits under 30 MB only if: (a) illustrations are SVG not PNG, (b) Android ships AAB with per-ABI splits, (c) names.json bucketed. Need new SED-QA-007a.

## Cold start budget on Pixel 6a

| Phase | Estimate |
|---|---|
| Native init (Hermes, JSI, Fabric) | 350–500 ms |
| JS bundle parse (Hermes bytecode) | 250–400 ms |
| SecureStore.getItem + new MMKV() | 20–40 ms |
| Zustand × 8 stores hydrate | 30–60 ms |
| First React render | 150–250 ms |
| Splash → interactive | 100–200 ms |
| **Total** | **≈900–1450 ms** |

Realistic with sync bootstrap. Under async (Option B), add 150–300 ms; would tighten on Galaxy A14.

## New requirements for v2.2

- SED-PRI-008 — synchronous bootstrap mandate
- SED-PRI-009 — globalThis.btoa over Buffer import
- SED-CRYPTO-001 — react-native-aes-gcm-crypto
- SED-CRYPTO-002 — PBKDF2 raised to 200k
- SED-F07-009a — plain-text alternative to tagged PDF
- SED-QA-007a — AAB splits + SVG + names.json bucketing
- SED-TS-001 — strict TypeScript (`"strict": true`, `"noUncheckedIndexedAccess": true`, `"exactOptionalPropertyTypes": true`)
- SED-NOTIF-001 — iOS Focus mode caveat
- SED-KEEP-001 — expo-keep-awake iOS 18 issue mitigation
- SED-BAK-005 — Android picker MIME `*/*` for `.seedbk`
- SED-ARCH-019 — exact dependency version pinning
- SED-ARCH-020/021 — RevenueCat Paywalls/Experiments disabled
