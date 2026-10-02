

# Phase System App — Codex Project Brief

## 1. 프로젝트 목적

이 앱은 개인적으로 실제 사용할 Phase System 관리 앱이다.

단순 포트폴리오용 데모가 아니라, 실제 일상에서 계속 쓰는 것을 목표로 한다.

동시에 이 프로젝트는 AI-assisted coding workflow를 경험하기 위한 작은 프로젝트다.

개발자는 Swift 초보이며 많은 구현을 Codex에 맡기지만, 전체 구조와 주요 데이터 흐름은 직접 읽고 이해할 예정이다.

따라서 다음 원칙을 중요하게 생각한다.

- 단순한 구조
- 읽기 쉬운 코드
- 과도한 abstraction 금지
- 실제 사용성을 우선
- 빠른 구현 → 직접 사용 → UX 수정 반복
- 미래 요구를 예상한 과설계 금지

---

## 2. 플랫폼 / 기술 방향

지원 플랫폼:

- iOS
- macOS

클라이언트 기술:

- Swift
- SwiftUI
- SwiftData
- URLSession 기반 네트워크 통신

앱은 local-first 구조로 만든다.

홈서버가 꺼져 있거나 네트워크가 없어도 핵심 기능은 모두 동작해야 한다.

```text
iPhone
└─ Local Data

Mac
└─ Local Data
```

서버 연결이 가능한 경우에만 동기화한다.

```text
iPhone Local Data
       ↕
    Home Server
       ↕
Mac Local Data
```

홈서버는 Linux 기반 개인 서버이며, Phase System 전용이 아니라 여러 개인 프로젝트를 함께 운영하는 범용 서버다.

서버 기술은 아직 확정하지 않는다.

후보:

- FastAPI
- Spring Boot

초기 구현에서는 서버와 동기화를 뒤로 미루고, local 앱부터 완성한다.

---

## 3. 핵심 도메인 구조

앱의 기본 구조는 다음과 같다.

```text
Area
└─ Phase
```

Area 예:

```text
English
Japanese
Life
Development
Economics
Drawing
```

각 Area에는 여러 Phase가 존재한다.

```text
English
├─ Phase 1
├─ Phase 2
└─ Phase 3
```

Phase는 해당 Area의 진행 단계를 의미한다.

예:

```text
Phase 1  ✓
Phase 2  ●
Phase 3  ·
```

초기 상태 기호 예:

```text
✓  completed
●  active
○  paused
·  planned
→  next action
!  insight / important
?  unresolved
>  inbox
-  note
```

기호 체계는 구현 중 수정될 수 있다.

중요한 원칙은 상태를 강한 색으로 표현하기보다 Unicode symbol과 typography로 표현하는 것이다.

---

## 4. Phase Detail

각 Phase에는 다음 정보가 있다.

```text
Goal
Current State
Bottleneck
Tasks
Success Metrics
```

필요하면 일반 Notes도 추가할 수 있다.

예:

```text
ENGLISH / PHASE 1

Goal
정확한 Input을 받을 수 있는 기반 복구

Current State
IPA 학습 진행
기본 문법 복구 중

Bottleneck
Weak form 및 실제 발음 인식

Tasks
✓ Schwa
● Stress
→ Weak Forms
→ Linking

Success Metrics
□ 기본 IPA를 읽을 수 있다
□ 약형을 청취에서 구별할 수 있다
□ 기본 문법 감각이 복구되었다
```

---

## 5. Daily Log

Daily Log는 Phase와 별개의 축이다.

Phase:

```text
현재 어떤 시스템과 단계를 운영하고 있는가
```

Daily Log:

```text
오늘 실제로 무엇을 했는가
```

날짜별 기록을 제공한다.

예:

```text
2026-10-01

✓ IPA review
✓ Library session
→ Docker notes
! Sync 구조 아이디어
- 오후 집중력 양호
```

Daily Log 역시 단순 체크박스만 쓰지 않고 Unicode 기반 상태/기호 체계를 사용한다.

---

## 6. Global Inbox

Inbox는 특정 Area 또는 Phase에 속하지 않는다.

앱 전체에서 항상 접근 가능한 global capture 기능이다.

```text
Any Screen
   ↓
Global Inbox Button
   ↓
Quick Capture
   ↓
Inbox
```

입력 시에는 분류를 요구하지 않는다.

핵심 원칙:

> Capture first, organize later.

나중에 Inbox에서 다음 동작을 할 수 있다.

```text
Assign to Area
Assign to Phase
Convert to Quick Task
Convert to Daily Log item
Keep as Inbox item
Delete
```

Inbox는 floating action 또는 상시 접근 가능한 단순한 control로 제공하는 방향을 선호한다.

---

## 7. Quick Task

Quick Task는 짧은 빈 시간에 바로 수행할 수 있는 아주 작은 작업이다.

정의:

> 대략 5분 내외로 끊어서 수행 가능한 독립적인 행동

예:

```text
→ Schwa 예문 5개 듣기        5m
→ N4 Anki 10개               5m
→ Docker volume 개념 읽기    5m
```

Quick Task는 Area나 Phase에 연결될 수 있지만 필수는 아니다.

개념적으로:

```text
QuickTask
├─ title
├─ estimated duration
├─ optional Area
└─ optional Phase
```

---

## 8. 주요 화면

초기 MVP에서 필요한 화면:

```text
Dashboard
Areas / Phases
Phase Detail
Daily Log
Quick Tasks
Inbox
```

정확한 navigation 구조와 UI 동선은 초기부터 완벽하게 확정하지 않는다.

실제 사용하면서 수정한다.

---

## 9. Dashboard

Dashboard는 현재 상태를 한눈에 파악하는 화면이다.

예:

```text
PHASES

English
├─ Phase 1  ✓
├─ Phase 2  ●
└─ Phase 3  ·

Japanese
└─ Phase 1  ●

Life
├─ Phase 3  ✓
└─ Phase 4  ●


QUICK TASKS

→ Review IPA examples        5m
→ Japanese Anki              5m


INBOX

3 items
```

이 앱은 캘린더 중심 일정관리 앱이 아니다.

다음 질문에 빠르게 답하는 것이 목적이다.

```text
나는 지금 어떤 영역을 운영하고 있는가?

각 영역은 현재 어느 Phase인가?

현재 목표는 무엇인가?

병목은 무엇인가?

다음 행동은 무엇인가?

지금 5분이 있다면 무엇을 할 수 있는가?

오늘 실제로 무엇을 했는가?

지금 떠오른 생각을 어디에 빠르게 넣을 수 있는가?
```

---

## 10. 디자인 원칙

핵심 방향:

> Text-first UI

그래픽 장식보다 다음 요소로 hierarchy를 표현한다.

```text
Typography
Whitespace
Indentation
Divider
Unicode Symbols
```

피하고 싶은 스타일:

```text
과도한 Card UI
강한 Gradient
화려한 상태색
큰 그림자
과도한 Rounded Rectangle
불필요한 Animation
상태마다 강한 색을 사용하는 UI
```

UI는 담백하고 텍스트 중심이어야 한다.

---

## 11. Typography

기본 방향:

```text
SF Pro
→ Screen Title
→ Navigation
→ Section Heading
→ 일반적인 긴 본문

SF Mono
→ Phase tree
→ Tasks
→ Quick Tasks
→ Daily Log items
→ Status
→ Unicode 기반 구조화된 정보
```

한글에서 SF Mono 사용 시 fallback 또는 가독성 문제가 있다면 플랫폼 기본 시스템 폰트를 우선해도 된다.

실제 기기에서 보고 조정한다.

---

## 12. Design Tokens

색과 폰트를 화면마다 직접 hard-code하지 않는다.

의미 단위로 design token을 정의한다.

예:

```text
Colors

backgroundPrimary
backgroundSecondary
textPrimary
textSecondary
accent
divider
```

```text
Typography

screenTitle
sectionTitle
body
structuredBody
caption
```

새 화면을 만들 때 기존 token을 재사용한다.

새 token은 실제 필요가 있을 때만 추가한다.

---

## 13. 색 사용 원칙

색은 주된 상태 전달 수단이 아니다.

다음 기호가 우선한다.

```text
✓
●
○
·
→
!
?
>
-
```

Accent color는 제한적으로 사용한다.

예:

- current selection
- active Phase
- link
- focus
- 필요한 최소 강조

Light / Dark Appearance는 design token 값 변경으로 대응 가능한 구조가 좋다.

---

## 14. Design Reference

별도의 `design-reference.png` 파일을 함께 제공한다.

이 이미지를 pixel-perfect하게 복제하지 않는다.

다음 방향만 참고한다.

- text-first UI
- SF Pro + SF Mono 조합
- 넓은 whitespace
- Unicode symbol 기반 상태 표현
- 얇은 divider
- 최소한의 accent
- card-heavy UI 회피
- macOS / iOS 모두 같은 정보 구조를 유지하는 느낌

텍스트 요구사항이 이미지보다 우선한다.

---

## 15. 개발 원칙

가장 중요한 원칙:

> Do not introduce an abstraction unless it solves a current problem.

현재 필요하지 않은 구조를 미리 만들지 않는다.

예:

- 불필요한 Repository layer
- 구현 하나뿐인데 존재하는 Protocol
- 의미 없는 Service / Manager
- 과도한 Generic abstraction
- 미래 기능을 예상해서 만든 architecture

필요가 실제로 생기면 그때 refactor한다.

---

## 16. 학습 목적

이 프로젝트는 Swift mastery 프로젝트가 아니다.

하지만 개발자는 다음 데이터 흐름은 이해할 수 있어야 한다.

```text
UI
↓
State
↓
Domain Model
↓
Local Persistence
↓
Networking
↓
Synchronization
```

모든 Swift 문법 세부사항을 설명할 필요는 없다.

새로운 Swift / SwiftUI 개념을 도입할 때는 현재 코드에서 왜 필요한지만 짧게 설명한다.

---

## 17. 초기 MVP 구현 순서

추천 순서:

```text
1. iOS / macOS 공용 프로젝트 골조
2. Local data model
3. Area / Phase CRUD
4. Phase Detail
5. Global Inbox
6. Quick Tasks
7. Daily Log
8. Dashboard
9. UI refinement
10. Home Server Sync
```

동기화는 중요한 기능이지만 local 사용성이 먼저 안정되어야 한다.

---

## 18. 초기 제외 범위

현재 만들지 않는다.

- 회원가입
- 로그인
- multi-user
- social
- realtime collaboration
- calendar scheduling
- habit streak system
- 고급 chart
- cloud account sync
- WebSocket realtime sync
- CRDT
- rich text editor
- Notion-style block editor
- 복잡한 notification system

실제 사용 중 필요성이 생기면 이후 검토한다.

