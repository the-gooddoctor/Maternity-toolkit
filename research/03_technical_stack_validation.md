# Seed PRD v2.0 — May 2026 Technology Validation Report

## 1. Version Updates Table

| Package | PRD / scaffold target | Current stable May 2026 | Recommended target | Notes |
|---|---|---|---|---|
| React Native | 0.76.0 | 0.83.x (with 0.84 in preview, Hermes V1) | **0.83.x via Expo SDK 55** | Legacy Architecture removed; New Arch + Bridgeless mandatory. |
| Expo SDK | ~52.0.0 | **55** (RN 0.83 + React 19.2) | **55** | SDK 56 in beta. |
| React | 18.3.1 | 19.2 | **19.2** | |
| expo-router | ~4.0.0 | **v7** | **v7** | |
| react-native-mmkv | ^3.0.2 | **4.3.1** | **4.x** | v4 uses Nitro Modules; both v3 and v4 drop Android API <23. |
| zustand | ^5.0.1 | 5.0.10+ (Jan 2026 persist fix) | **5.0.10+** | `createJSONStorage` strongly recommended. |
| zustand-mmkv-storage | ^1.0.0 | 1.0.0 (single-maintainer, low downloads) | **Drop; inline adapter** | Single-maintainer hobby project. |
| nativewind | ^4.1.23 | **v4.2 stable**; v5 in adoption | **v4.2 for launch** | v5 requires Tailwind v4.1+ config rewrite. |
| @gluestack-ui/themed | v2+ | **v3** (shadcn-style copy-paste) | **Re-evaluate** | v3 moved away from pre-themed lib. |
| react-native-gifted-charts | ^1.4.64 | Latest 1.4.x | **Acceptable, victory-native-xl better** | |
| lucide-react-native | ^0.468.0 | 0.5xx | **Latest 0.5xx** | |
| react-native-purchases | ^8.4.0 | **10.0.1** | **10.x** | v9 added Play Billing 8 support. |
| expo-notifications | ~0.29.0 | Bundled with SDK 55 | **SDK 55 pinned** | |
| expo-print | ~14.0.0 | Bundled with SDK 55 | **SDK 55 pinned** | iOS WKWebView local-asset limitation. |
| expo-keep-awake / -haptics / -linking / -splash-screen / -status-bar | various | SDK 55-pinned | **SDK-pinned** | |
| react-native-reanimated | ~3.16.1 | 4.x | **4.x** | |
| EAS CLI | >= 13.0.0 | 16.x | **>= 16.0.0** | |
| Android minSdk / targetSdk | 26 / 35 | Still 35 for Play; **16 KB page-size required** | **minSdk 26, targetSdk 35** + 16 KB | Play required 16 KB since 1 Nov 2025 (grace to 31 May 2026). |
| iOS deployment / SDK | 16.0 | **Xcode 26 / iOS 26 SDK** for new submissions from **28 April 2026** | **Deployment 16.0, build iOS 26 SDK** | EAS Build SDK 55 does Xcode 26 by default. |

## 2. Critical Issues — Actual Bugs in PRD Code

### 2.1 `useMMKVString` writing to wrong storage

`useMMKVString(key)` without an instance argument uses the **default unnamed** MMKV instance. But `stores/mmkvStorage.ts` creates a named, encrypted instance. Timer timestamps end up in an **unencrypted, unnamed** MMKV file — violating SED-ARCH-002.

**Fix:** pass the named instance:
```javascript
import { mmkv } from '@/stores/mmkvStorage';
const [startTimestamp, setStartTimestamp] = useMMKVString(`${storageKey}_start`, mmkv);
```

### 2.2 `persist` middleware needs `createJSONStorage` wrapper

The PRD passes a raw `StateStorage`. Zustand v5 requires it wrapped with `createJSONStorage`. Without that, `setItem` is called with raw objects → `"[object Object]"`.

**Fix:**
```typescript
export const mmkvStorage = createJSONStorage(() => mmkvStateStorage);
```

### 2.3 Hard-coded encryption key

Shipping `'seed-enc-key'` as a literal in JS means anyone who unpacks the .apk/.ipa can decrypt every user's MMKV file.

**Fix:** generate per-install key with `expo-crypto.getRandomBytesAsync(32)`, store in iOS Keychain / Android Keystore via `expo-secure-store`.

### 2.4 Timer wall-clock drift on user time changes

`Date.now()` reflects system clock; if user changes device time mid-labour, elapsed jumps. Use `performance.now()` for display tick, persist `Date.now()` snapshot.

### 2.5 `AppState` 'active' not always fired on iOS lockscreen-resume

Add a re-read of `AppState.currentState` on every screen mount.

### 2.6 `setInterval(..., 1000)` silently stale on background return

Force `recalculate()` immediately in the interval-setup effect, not only from the AppState listener.

## 3. Architecture Validation

The offline-only Zustand + MMKV pattern remains strongest for React Native in May 2026:
- MMKV v4 on Nitro Modules, JSI-only, full New Architecture support
- Zustand v5 + `createJSONStorage` is canonical
- RevenueCat now caches Offerings on disk (paywalls render during full backend outage)
- Hermes mandatory under New Arch

Only architectural soft spot: hard-coded encryption key (fixed above).

## 4. Replacements to Consider

| Existing | Alternative | Why |
|---|---|---|
| @gluestack-ui/themed v2 | Drop; NativeWind + ~8 hand-rolled primitives | Gluestack v3 is shadcn-style copy-paste; overkill for 10 screens. |
| zustand-mmkv-storage | Inline adapter | Single-maintainer 1.0.0 with supply-chain risk. |
| react-native-gifted-charts | victory-native-xl (Skia-backed) | Modern Formidable choice. Either works. |
| react-native-linear-gradient | expo-linear-gradient | Already in Expo ecosystem. |

## 5. New Requirements to Add

1. `expo-secure-store` + dynamic MMKV encryption key (SED-PRI-006).
2. 16 KB page-size compliance (SED-ARCH-009 update).
3. ProGuard / R8 rules for Hermes + MMKV in Section 12.6.
4. EAS Update channel strategy — recommend disabling for v1.0.
5. Crash diagnostics without telemetry — on-device crash file + share button.
6. Remove `NSUserTrackingUsageDescription` from app.json (contradiction).
7. Notification channel creation before scheduling on Android 13+.
8. iOS App Privacy nutrition label = "Data Not Collected" for every category.

## 6. PRD Edits Required (Section by Section)

- **6.1:** Replace version table with SDK 55 / RN 0.83 / MMKV v4 / etc.
- **6.3:** Update SED-ARCH-009 (16 KB compliance), SED-ARCH-010 (Xcode 26).
- **6.4:** Update timer hook code with named MMKV instance + synchronous recalculate on mount.
- **6.5:** Replace `stores/mmkvStorage.ts` with `createJSONStorage` + bootstrap key from SecureStore.
- **12.5:** Bump EAS CLI to >= 16.0.0; document `releaseStatus: "completed"`.
- **12.6:** Remove `NSUserTrackingUsageDescription` and `react-native-mmkv` plugin entry; add `expo-build-properties` + `expo-secure-store`.
- **2.2:** Net per sale ~£4.16 (after 15% + VAT).
- **7:** Add SED-PRI-006 / 007.
- **10.2:** App Privacy label = "Data Not Collected"; remove `NSUserTrackingUsageDescription`.

---

## Sources

- React Native 0.83 release; New Architecture mandatory
- Expo SDK 55 changelog
- react-native-mmkv v4.3.1 (Nitro Modules)
- Zustand persist middleware reference (createJSONStorage)
- NativeWind v5 migration
- Gluestack-ui v3 release blog
- victory-native-xl repository
- react-native-purchases v10 (Play Billing 8)
- RevenueCat offline entitlements
- Apple Small Business Program
- Google Play target SDK 35 + 16 KB page size
- Apple iOS 26 SDK mandatory (28 April 2026)
- Expo Notifications docs (POST_NOTIFICATIONS, channels)
- Expo build-properties + R8/ProGuard guide
