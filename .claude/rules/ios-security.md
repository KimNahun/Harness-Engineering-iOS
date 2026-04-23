---
paths:
  - "**/*.swift"
---
# iOS Security Rules

## Secret Management

- **Keychain Services** 사용 -- 토큰, 비밀번호, 키 등 민감 데이터
- `UserDefaults`에 민감 데이터 절대 저장 금지
- 소스에 시크릿 하드코딩 금지 -- `.xcconfig` 또는 환경변수 사용

```swift
let apiKey = ProcessInfo.processInfo.environment["API_KEY"]
guard let apiKey, !apiKey.isEmpty else {
    fatalError("API_KEY not configured")
}
```

## Transport Security

- App Transport Security (ATS) 비활성화 금지
- 중요 엔드포인트에 Certificate Pinning 적용
- 모든 서버 인증서 검증

## Input Validation

- 사용자 입력은 표시 전 반드시 sanitize
- `URL(string:)` 사용 시 force-unwrap 금지
- 외부 데이터(API, 딥링크, 페이스트보드) 처리 전 유효성 검증

## Data at Rest

- 민감 파일은 `FileManager` + `Data.WritingOptions.completeFileProtection` 사용
- Core Data / SwiftData 암호화 검토
- 앱 스냅샷에 민감 정보 노출 방지 (`applicationWillResignActive`에서 마스킹)

## Authentication

- 생체인증(Face ID/Touch ID)은 `LAContext` 사용, 실패 시 폴백 제공
- 세션 토큰은 Keychain에 저장, 만료 시 자동 갱신 또는 로그아웃
