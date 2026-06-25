# 🕵️ 커닝시티: 지하수사록

> **MapleStory Worlds 기반 멀티 추리 마피아 게임**  
> 기획·UI·시스템·리소스 문서 통합 저장소

![Status](https://img.shields.io/badge/상태-로비%20구현완료-brightgreen)
![Platform](https://img.shields.io/badge/플랫폼-MapleStory%20Worlds-blue)
![Players](https://img.shields.io/badge/플레이어-6~8인-green)
![Lang](https://img.shields.io/badge/언어-한국어-red)

---

## 📖 프로젝트 한 줄 소개

커닝시티 지하 수사대의 일원이 되어 **시민 사이에 숨어든 마피아를 찾아내는** 6~8인 소셜 디덕션 게임.  
낮에는 토론과 투표로 추리하고, 밤에는 직업 능력으로 정보를 모으거나 사건을 조작한다.

---

## 🎮 게임 핵심 정보

| 항목 | 내용 |
|------|------|
| 장르 | 멀티플레이 추리 / 마피아 / 소셜 디덕션 |
| 플랫폼 | MapleStory Worlds |
| 플레이 인원 | 6 ~ 8명 |
| 배경 | 커닝시티 지하 수사 구역 |
| 게임 흐름 | 로비 → 방 생성/입장 → 역할 배정 → **낮 토론 → 투표 → 밤 행동** → 결과 공개 → 승패 판정 |

---

## 🗂️ 문서 구조

```
MapleStory-Worlds-mafia-docs/
│
├── 📋 planning/             # 기획 문서
│   ├── game-concept.md      # 게임 콘셉트 및 방향성
│   ├── game-flow.md         # 게임 진행 흐름 상세
│   ├── role-design.md       # 직업/역할 설계
│   └── balance.md           # 밸런스 설계
│
├── 🖼️ ui/                   # UI 명세
│   ├── lobby-ui.md          # 로비 화면
│   ├── room-create-ui.md    # 방 만들기
│   ├── room-search-ui.md    # 수사방 찾기
│   ├── matchmaking-ui.md    # 빠른 시작
│   └── profile-ui.md        # 프로필
│
├── ⚙️ system/               # 시스템 설계
│   ├── room-system.md       # 방 생성/관리
│   ├── player-state.md      # 플레이어 상태 관리
│   ├── game-state.md        # 게임 상태 머신
│   └── voting-system.md     # 투표 시스템
│
├── 🎨 assets/               # 리소스 목록
│   ├── asset-list.md        # 전체 에셋 목록
│   ├── image-resources.md   # 이미지 리소스
│   └── sound-resources.md   # 사운드 리소스
│
└── 🤖 prompts/              # AI 협업 프롬프트
├── codex-prompts.md     # 코드 생성용
├── image-prompts.md     # 이미지 생성용
└── ui-prompts.md        # UI 생성용
```


---

## 🧑‍🤝‍🧑 역할 시스템 (MVP 기준)

### 시민 진영
| 역할 | 인원 | 능력 | 승리 조건 |
|------|------|------|-----------|
| 👤 시민 | 복수 | 없음 (토론·투표만 가능) | 마피아 전원 제거 |
| 🔍 경찰 | 1명 | 밤마다 1명의 정체 조사 | 마피아 전원 제거 |
| 💊 의사 | 1명 | 밤마다 1명 보호 (마피아 공격 무효화) | 마피아 전원 제거 |

### 마피아 진영
| 역할 | 인원 | 능력 | 승리 조건 |
|------|------|------|-----------|
| 🔪 마피아 | 1~2명 | 밤마다 1명 제거 | 생존 마피아 ≥ 생존 시민 |

> 💡 추후 밸런스에 따라 특수 직업 추가 예정

---

## 📅 MVP 개발 로드맵

### ✅ MVP 1차 — 로비 & 방 시스템

- [x] 인트로 시퀀스 (스토리 연출 포함)
- [x] 닉네임 설정
- [x] 로비 화면 (아바타 이동·점프 물리 시뮬레이션 포함)
- [x] 프로필 이미지 선택 / 닉네임·칭호 저장
- [x] 방 만들기 UI (이름·인원수·비공개 비밀번호 입력 및 검증)
- [x] 빠른 매칭 UI (타이머·애니메이션 포함)
- [x] 수사방 찾기 UI (클라이언트 레이아웃)
- [x] BGM 재생 및 설정 팝업 (음소거 토글)
- [ ] 수사방 찾기 — 서버 연동 (방 목록 조회·입장)
- [ ] 방 만들기 — 서버 연동 (실제 방 생성)
- [ ] 대기방 입장 & 플레이어 목록 표시

### 🔲 MVP 2차 — 게임 코어
- [ ] 역할 랜덤 배정
- [ ] 낮 / 밤 상태 전환
- [ ] 투표 시스템
- [ ] 밤 능력 사용
- [ ] 승패 판정

### 🔲 MVP 3차 — 확장 콘텐츠
- [ ] 직업 확장
- [ ] 튜토리얼 / 도감 / 업적
- [ ] 상점
- [ ] 친구 / 초대 시스템
- [ ] 연출 강화

---

## 🛠️ 스크립트 아키텍처

현재 구현된 `.mlua` 스크립트 목록과 역할입니다.

### Intro — 인트로 흐름
| 파일 | 역할 |
|------|------|
| `IntroSequenceBootstrap.mlua` | 인트로 진입점, 시퀀스 초기화 시작 |
| `IntroSequenceController.mlua` | 인트로 연출 단계별 제어 (페이드·자막 등) |
| `StorySequenceController.mlua` | 스토리 텍스트/애니메이션 시퀀스 재생 |
| `IntroFlowController.mlua` | 인트로→닉네임→로비→게임 전체 흐름 관리 |
| `MainFlowBgmController.mlua` | BGM 전환 관리 (인트로/로비/게임 구간별) |

### Nickname — 닉네임 설정
| 파일 | 역할 |
|------|------|
| `NicknameSetupController.mlua` | 닉네임 입력 UI 제어 및 유효성 검사 |

### Lobby — 로비
| 파일 | 역할 |
|------|------|
| `LobbyController.mlua` | 로비 UI 전체 제어 (`@ExecSpace("ClientOnly")`) |
| `ProfileStorageLogic.mlua` | DataStorage 연동 프로필(닉네임·칭호·이미지) 저장·불러오기 |

#### LobbyController 주요 기능
- **아바타 시뮬레이션** — 이동(A/D·←→), 점프(Space), 중력, 착지 판정, 걷기 흔들림, 점프 스쿼시 이펙트
- **아바타 애니메이션** — `AvatarGUIRendererComponent` + `ActionStateChangedEvent`로 idle/walk/jump 전환
- **프로필 모달** — 4종 이미지 슬롯 선택, 닉네임 입력, 칭호 표시, 저장 시 DataStorage 반영
- **방 만들기 모달** — 방 이름(최대 12자), 인원수(4~8명), 비공개/비밀번호(숫자 4자리) 입력 및 검증
- **빠른 매칭 모달** — mm:ss 타이머, 3점 순환 애니메이션, 취소 버튼
- **수사방 찾기** — `UI_InvestigationRoomSearch` (필터·페이지·목록) 클라이언트 레이아웃
- **설정 팝업** — 정보/사운드 탭 전환, BGM 음소거 토글 (`MainFlowBgmController` 연동)
- **외부 팝업** — 친구·상점·알림 팝업 열기/닫기 관리

### 공통 유틸리티
| 파일 | 역할 |
|------|------|
| `UIPopup.mlua` | 범용 팝업 열기/닫기 컨트롤러 |
| `UIToast.mlua` | 토스트 메시지 표시 컨트롤러 |

---

## 📊 현재 진행 상황

| 구분 | 상태 | 비고 |
|------|------|------|
| 프로젝트 콘셉트 | ✅ 완료 | |
| 인트로 흐름 | ✅ 완료 | 스토리 연출 포함 |
| 닉네임 설정 | ✅ 완료 | |
| 로비 UI | ✅ 완료 | 아바타 물리 시뮬레이션 포함 |
| 방 만들기 UI | ✅ 완료 | 입력·검증 포함 |
| 빠른 매칭 UI | ✅ 완료 | 서버 연동 미구현 |
| 수사방 찾기 UI | 🟡 진행 중 | 클라이언트 UI 완료, 서버 연동 필요 |
| BGM / 설정 팝업 | ✅ 완료 | |
| 방 시스템 서버 연동 | 🔴 미구현 | |
| 대기방 & 플레이어 목록 | 🔴 미구현 | |
| 게임 룰 | 🔴 초안 필요 | |
| 역할 밸런스 | 🔴 초안 필요 | |
| 리소스 목록 | 🔴 정리 필요 | |

---

## 🎨 UI 방향성

- 메이플스토리 감성의 **판타지 UI**
- 커닝시티 지하 느낌의 **어두운 색감**
- 목재 / 금속 / 종이 **질감 활용**
- 둥근 버튼 + 명확한 텍스트 대비
- 이미지 리소스와 코드 UI 분리

---

## 📏 작업 규칙

### 문서 작성
- 기능별로 문서 분리
- **확정된 내용**과 _아이디어_ 구분 표기
- 변경 사항은 날짜와 함께 기록
- UI 리소스명, RUID, 경로는 문서에 남기기

### 파일 네이밍
영어 소문자 + 하이픈(-)
예: lobby-ui.md / voting-system.md / role-design.md



### 커밋 컨벤션
docs: add lobby ui specification
docs: update room search flow
docs: add role design draft
feat: implement voting system
fix: correct player state logic



---

## 👥 팀

| 역할 | 담당 |
|------|------|
| 기획 | |
| 개발 | |
| 디자인 | |

> 이 저장소는 기획·UI·시스템·리소스·AI 프롬프트를 통합 관리하는 **팀 문서 허브**입니다
