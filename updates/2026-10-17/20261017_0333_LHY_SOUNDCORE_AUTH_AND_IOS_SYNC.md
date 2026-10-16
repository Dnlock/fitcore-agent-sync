# LHY Soundcore Phase 1 — Auth Compatibility + iOS Sync

```yaml
update_id: "20261017_0333_LHY_SOUNDCORE_AUTH_AND_IOS_SYNC"
date_local: "2026-10-17"
agent: "LHY/ChatGPT"
type: "CODE+VALIDATION"
status: "AUTH_COMPATIBILITY_VERIFIED_REAL_DEVICE_AUTH_NOT_RUN"
code_repo: "Dnlock/fitcore-ios"
code_branch: "lhy/soundcore-local-link"
code_commit: "dea065478cfed130182213f56a02cebcee76a65f"
merge_status: "NOT_MERGED"
```

## iOS code state

- `lhy/soundcore-local-link` was fast-forwarded from `c6d48f4` to the verified runtime wiring commit `68a1d85d7c13cda8cd463eb1b7e3cbb4260013a6`.
- Added the Soundcore credential placeholder keys to `ios/Fitcore/Resources/Info.plist` on the same LHY branch.
- Current remote LHY branch commit: `dea065478cfed130182213f56a02cebcee76a65f`.
- Runtime wiring includes real `SoundcoreRecorderGateway`, `AppSoundcoreCredentials`, recorder mode selection, explicit credential failures, and real scan/connect state handling.

## Authentication compatibility validation

All checks used temporary local values only. No credential values are recorded here.

1. QA `get_token` with product code `D3200` and SDK identity `V1.3.02`: PASS.
2. QA `/v1/third_party_sdk/validate_token` using the issued token: PASS.
3. CI `/v1/third_party_sdk/validate_token` using the same issued token with iOS identity `V1.3.01`: PASS.
4. iOS v1.3.01 binary inspection confirms DEVELOPMENT maps to `https://speaker-api-ci.anker-in.com` and PRODUCTION maps to `https://speaker-api.anker-in.com`.
5. CI RSA public key was extracted from the local private iOS v1.3.01 binary for verification only.
6. QA-issued license signature verifies against the CI public key only when `license` is serialized as compact JSON preserving field order:
   - server raw JSON: FAIL
   - compact JSON (`separators=(",", ":")`): PASS
   - default/pretty-spacing JSON: FAIL
   - sorted-key compact JSON: FAIL
7. Conclusion: QA-issued auth material is compatible with the current iOS v1.3.01 DEVELOPMENT/CI auth path, provided the license JSON is passed in verified compact serialization.

## Security state

- No `app_key` committed.
- No token committed.
- No license/license signature committed.
- No `Local.xcconfig` committed.
- No private Soundcore SDK binary committed.
- No real recording committed.
- Previously pasted vendor credentials should be rotated/reissued before production use.

## Still NOT verified

- REAL_SDK_INIT
- REAL_AUTH_CALLBACK
- BLUETOOTH_PERMISSION
- REAL_SCAN
- DEVICE_FOUND
- REAL_CONNECT
- DISCONNECT/RECONNECT
- Phase 2 recording/file transfer/decryption

## Next gate

Safely inject a fresh token/license/signature/userId/deviceId into ignored local configuration, verify Xcode expansion without exposing values, then execute the physical-device chain:

`initialize -> auth callback -> BLE permission -> scan -> device found -> connect -> connected -> disconnect -> reconnect`
