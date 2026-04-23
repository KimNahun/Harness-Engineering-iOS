# 프로젝트 컨텍스트

> 이 파일을 `PROJECT_CONTEXT.md`로 복사하고, 프로젝트에 맞게 수정하세요.
> Planner, Generator, Evaluator가 **반드시 먼저 읽는** 프로젝트 고정 요구사항입니다.

---

## 대상 프로젝트

- **앱 이름**: [YourApp]
- **번들 ID**: com.yourteam.[YourApp]
- **최소 타겟 iOS**: 17.0
- **Swift 버전**: Swift 6 (엄격 동시성 필수)
- **UI 프레임워크**: SwiftUI

---

## 프로젝트 경로 (하네스가 사용하는 변수)

```bash
# 프로젝트 루트 (xcodeproj가 있는 폴더)
PROJECT_ROOT="/Users/yourname/path/to/YourApp"

# 소스 코드 폴더 (App/, Views/, Models/ 등이 있는 폴더)
TARGET_DIR="YourApp"

# 하네스 루트
HARNESS_ROOT="/Users/yourname/path/to/harness"
```

---

## 빌드 / 테스트 명령어

```bash
# 빌드
BUILD_COMMAND="xcodebuild -project $PROJECT_ROOT/YourApp.xcodeproj \
  -scheme YourApp \
  -destination 'platform=iOS Simulator,name=iPhone 16' \
  build 2>&1 | grep -E 'error:|BUILD (SUCCEEDED|FAILED)'"

# 테스트
TEST_COMMAND="xcodebuild test -project $PROJECT_ROOT/YourApp.xcodeproj \
  -scheme YourApp \
  -destination 'platform=iOS Simulator,name=iPhone 16' \
  2>&1 | tail -5"
```

---

## Xcode 통합 방식

```
# output/ -> 프로젝트 폴더 동기화 방식
# "auto" = PBXFileSystemSynchronizedRootGroup (파일 복사만으로 Xcode 자동 인식)
# "manual" = xcodeproj 수동 수정 필요
SYNC_METHOD="auto"
```

---

## 디자인 시스템 (선택)

> 커스텀 디자인 시스템 SPM 패키지가 있으면 아래에 작성하세요.
> 없으면 이 섹션을 삭제하세요. SwiftUI 기본 semantic color/font가 사용됩니다.

<!--
**패키지 이름**: PersonalColorDesignSystem

```swift
import PersonalColorDesignSystem
```

### 색상 토큰
```swift
// SwiftUI
Color.pTextPrimary / Color.pAccentPrimary / Color.pBackgroundTop
// UIKit
UIColor.pTextPrimary / UIColor.pAccentPrimary
```

### 컴포넌트
```swift
GlassCard { content }          // 카드 컨테이너
HapticManager.impact()         // 햅틱 피드백
GradientBackground()           // 배경 그래디언트
```

### 타이포그래피 (UIKit)
```swift
UIFont.pDisplay(40)    // 큰 숫자
UIFont.pTitle(17)      // 섹션 타이틀
UIFont.pBody(14)       // 본문
```

### 금지
```swift
// 하드코딩 색상 금지
Color(red: 0.2, green: 0.3, blue: 0.8)
UIColor(red: 0.7, green: 0.5, blue: 1.0, alpha: 1.0)
// 컴포넌트 자체 구현 금지
```
-->

---

## 아키텍처 요구사항

### 고정 요구사항

- MVVM: View -> ViewModel -> Service 단방향 의존
- 모든 ViewModel: `@MainActor` + `@Observable`
- 모든 Service: `actor`
- 모든 Model: `struct` + `Sendable`

### 사용자 추가 요구사항

> 프로젝트에 필요한 추가 기능/제약을 아래에 작성하세요.

<!--
#### 1. [기능 이름]
- 설명: ...
- 관련 파일: ...
- 제약: ...
-->

---

## API 문서 수집 (선택)

> NotebookLM MCP 또는 context7 MCP를 통해 API 문서를 수집하려면 아래에 질의 목록을 작성하세요.

<!--
### 질의 목록

노트북 ID: [notebooklm-notebook-id]

1. **[API 이름] 문서 수집**
   - 질문: "[질문 내용]"
   - 저장: `docs/[filename].md`

2. ...
-->

---

## 기존 코드 참고 (Generator용)

> 기존 프로젝트에서 Generator가 참고해야 할 파일이 있으면 경로를 적으세요.

<!--
- `YourApp/Models/User.swift` -- 기존 모델 구조 참고
- `YourApp/Services/AuthService.swift` -- 인증 흐름 참고
-->

---

## 보존 파일 (덮어쓰기 금지)

> Xcode 통합 시 절대 덮어쓰지 않아야 할 파일 목록

<!--
- `YourApp/Utils/Logger.swift` -- 기존 로거 유지
- `YourAppWidget/` -- 기존 위젯 타겟 전체
-->

---

## 이 파일 수정 방법

1. 위 섹션들을 프로젝트에 맞게 채워라
2. 주석(<!-- -->)으로 감싸진 예시를 필요에 따라 활성화하라
3. `/harness [프롬프트]` 로 파이프라인 실행
