# Document Classification Software 프로젝트 구조 정보

이 문서는 프로젝트 내 `ver1.0` 및 `ver2.0` 폴더의 역할과 구성 파일들에 대한 구조 정보 및 가이드를 담고 있습니다.

## 📂 폴더 구조 요약

```text
Document_Classification_Software/
├── docs/
│   └── structure_info.md          # [현재 문서] 프로젝트 폴더 구조 정의서
├── ver1.0/                        # [기존 버전] 이전 버전의 문서 분류 가이드 및 UI 프로토타입
├── ver2.0/                        # [이전 통합 버전] 초기 풀스택 문서 관리 시스템(DMS)
└── ver3.0/                        # [최신 고도화 버전] 폴더 업로드, 프로세스 카드 미리보기, PPTX 슬라이드 카드/A4식 텍스트 뷰어가 탑재된 최신 DMS
    ├── backend/                   # FastAPI 서버 및 파일 텍스트 추출 엔진
    ├── dowple_dms.html            # 최신 DMS 프론트엔드 인터페이스 (한국어)
    ├── dowple_dms_en.html         # 최신 DMS 프론트엔드 인터페이스 (영어)
    ├── README.md                  # ver3.0 메인 설명서
    └── README.ko.md               # ver3.0 한글 설명서
```

---

## 📁 상세 컴포넌트 정보

### 1. `ver1.0` (기존 버전)
이전의 문서 분류 소프트웨어 관련 가이드라인과 초기 대시보드 프로토타입을 유지하고 있는 디렉토리입니다.
* **[classification_logic_guide.md](file:///C:/Users/haner/.gemini/antigravity/scratch/Document_Classification_Software/ver1.0/classification_logic_guide.md)**: AI 문서 분류 모델 및 규칙 분류 로직에 대한 설명 가이드.
* **[dowple_code_guide.md](file:///C:/Users/haner/.gemini/antigravity/scratch/Document_Classification_Software/ver1.0/dowple_code_guide.md)**: 초기 대시보드 코드 작성 및 데이터 흐름 가이드.
* **[dowple_dms.html](file:///C:/Users/haner/.gemini/antigravity/scratch/Document_Classification_Software/ver1.0/dowple_dms.html)**: 기존 단일 파일 형태의 DMS(Document Management System) 대시보드 프로토타입.

### 2. `ver2.0` (이전 통합 버전)
`archivon-dms` 저장소에서 초기 통합된 풀스택 문서 관리 시스템(DMS)이 보관된 디렉토리입니다.

### 3. `ver3.0` (최신 고도화 버전)
폴더 업로드(디렉토리 재귀 업로드), 프로세스 카드 미리보기, 16:9 슬라이드 카드형 PPTX 뷰어 및 A4 시트형 문서 뷰어 등 프론트엔드 UX가 대폭 개선된 최신 버전입니다.
* **`backend/`**: Python FastAPI 기반 문서 처리 API 서버 및 텍스트 추출 엔진.
  - **`main.py`**: SQLite DB, JWT 인증 처리, 구글 로그인(ID 토큰 검증) API, 초대 코드 기반 팀 생성/참여 API가 통합되어 있습니다.
* **[dowple_dms.html](file:///c:/Users/haner/Desktop/0528문서분류/ver3.0/dowple_dms.html)**: 백엔드 서버와 연동되는 고도화된 프론트엔드 대시보드 (한국어 버전). 구글 로그인 및 가상 로그인, 팀 전환, 폴더 재귀 업로드, 프로세스 카드 눈동자 아이콘 미리보기, PPTX 슬라이드 카드/A4 보고서 문서 뷰어 등이 완비되어 있습니다.
* **[dowple_dms_en.html](file:///c:/Users/haner/Desktop/0528문서분류/ver3.0/dowple_dms_en.html)**: 최신 대시보드 (영어 버전).
* **[README.md](file:///c:/Users/haner/Desktop/0528문서분류/ver3.0/README.md) / [README.ko.md](file:///c:/Users/haner/Desktop/0528문서분류/ver3.0/README.ko.md)**: `ver3.0` 프로젝트 설치 및 실행 가이드 문서.

---

## 🔄 히스토리
* **2026-05-31 (4차)**: 고도화 버전을 `ver3.0`으로 복사 및 분리 이식 수행.
  - `ver2.0`에 통합 적용되었던 모든 최신 기능(폴더 업로드, 프로세스 카드 미리보기, PPTX 슬라이드 카드 미리보기 등)의 완성본을 바탕으로 신규 독립 릴리즈인 `ver3.0` 생성 완료.
* **2026-05-31 (3차)**: 다운로드 없는 메모리 기반 '하이브리드 통합 프리뷰(미리보기) 뷰어' 구현.
  - **보안성 극대화**: 로컬 파일 다운로드 링크 노출 없이, JWT 보안 인증이 탑재된 `fetchApi`를 통해 스트림 데이터를 가져와 브라우저 메모리에 상주시키는 **Blob URL(`URL.createObjectURL`)** 방식 도입.
  - **MIME/확장자별 동적 렌더링**: PDF는 브라우저 내장 PDF 프레임, 이미지는 이미지 슬롯, 텍스트(TXT/CSV/LOG)는 정밀 스크롤 뷰어로 연동.
  - **한글 및 오피스 문서 한계 극복 (A4/PPTX 슬라이드 카드형 레이아웃)**: 웹 표준 렌더링이 어려운 PPTX, DOCX, HWP/HWPX 포맷의 경우, 텍스트 추출 패키지에서 본문을 파싱하여 고화질 슬라이드식 뷰어(16:9 슬라이드 카드 그리드 및 호버 인터랙션) 및 정돈된 A4 보고서 레이아웃으로 변환하여 좌측 뷰어에 제공하도록 최종 개선.
  - **대조 검토의 시너지**: 모달 우측 영역에 AI 자동 분류 추천 폴더, 신뢰도 점수, 추출된 핵심 증거 패키지를 동시에 띄워 사용자가 다운로드 없이 최단 시간에 검토 및 확정을 마칠 수 있도록 UX 최종 개편.
  - **진입점 다양화 및 저장 전 문서(로컬 파일) 프리뷰**: 문서 보관함 목록의 행 단위 눈동자(`미리보기`) 버튼 및 문서 상세 모달 하단에 `[미리보기]` 버튼을 연동한 데 이어, **실시간 문서 처리 파이프라인(프로세스 카드)**의 헤더 영역에도 미리보기 버튼을 통합했습니다. 저장되지 않은 문서의 경우 브라우저 메모리에 있는 실제 파일 객체(`fileObject`)를 기반으로 PDF, 이미지, 텍스트 및 오피스 추출 요약본(백엔드 `/api/analyze` 연동)을 즉시 렌더링하여 저장 전에도 철저한 사전 검토가 가능하도록 고도화 완료.
* **2026-05-31 (2차)**: 폴더 통째로 업로드(디렉토리 재귀 업로드) 기능 추가.
  - 문서 업로드 존에 개별 파일 선택 외에도 디렉토리 자체를 선택해 올릴 수 있는 `📁 폴더 선택` 버튼 추가 (`webkitdirectory` 속성 활용).
  - 드래그 앤 드롭 업로드 시 `DataTransferItem.webkitGetAsEntry()` API와 재귀 함수(`traverseFileTree`)를 도입하여 하위 폴더의 모든 파일을 자동으로 수집하고 일괄 업로드 파이프라인에 밀어 넣는 기능 개발.
* **2026-05-31 (1차)**: 팀 초대 코드 상시 조회 기능 구현 및 UI 개선.
  - 팀 스페이스가 생성된 후에도 사용자가 언제든지 팀의 초대 코드를 확인하고 복사할 수 있도록 좌측 사이드바의 스페이스 전환 영역 하단에 `🔑 초대 코드 보기` 버튼 추가.
  - 스페이스 상태 감지 로직(`updateTeamLabels`)을 개선하여, 활성화된 현재 스페이스가 개인 스페이스(`type === "personal"`)가 아니고 실제 팀 스페이스(`invite_code`가 존재)일 때만 동적으로 해당 버튼이 표시되도록 구현.
  - 초대 코드 보기 버튼 클릭 시 브라우저 내장 대화 상자(`window.prompt`)를 활용해 코드를 쉽게 복사(`Ctrl+C`)할 수 있는 UI 완성.
* **2026-05-28 (3차)**: JWT `sub` 규격 오류 수정 및 프론트엔드 토큰 만료 처리 개선.
  - 백엔드 JWT 토큰 페이로드의 `sub` 클레임을 정수형(`int`)에서 문자열(`str`)로 형변환하도록 수정하여 최신 `PyJWT` 스펙의 디코딩 에러(`InvalidSubjectError`)를 해결.
  - 프론트엔드 `fetchApi()` 공통 모듈에서 `401 Unauthorized` 응답이 올 경우, 세션을 지우고 로그인 화면으로 즉시 튕겨내는 자동 로그아웃 로직을 다시 적용.
  - 병렬 API 요청으로 인한 로그아웃 중복 처리를 차단하기 위해 `signOut()` 함수 내에 중복 실행 가드 적용.
  - 가상 이메일 로그인 시 구글 OAuth 토큰 검증 과정을 우회하도록 백엔드 분기 로직을 추가하고, DB에 등록되지 않은 이메일인 경우 `User` 테이블에 자동으로 신규 회원 가입을 수행하도록 개선하여 협업 테스트 환경 완비.
* **2026-05-28 (2차)**: 개인 스페이스 & 팀 스페이스 분리 구현.
  - `personal-{user_id}` 형식의 개인 스페이스 개념 도입: 로그인 직후 자동으로 개인 스페이스에 진입.
  - 기존 `demo-team`에 저장된 데이터를 최초 로그인 시 개인 스페이스로 자동 이전 (`POST /api/migrate-demo-to-personal`).
  - `GET /api/teams` 응답에 개인 스페이스를 항상 첫 번째로 포함, `type: "personal"` 필드로 구분.
  - `check_team_access()`에 개인 스페이스 예외 처리 추가 (본인 여부만 확인, 팀 멤버십 조회 없음).
  - 프론트엔드 스페이스 전환 UI: 🏠 내 스페이스 / 👥 팀 스페이스 아이콘으로 구분.
  - 팀 생성 초대 코드를 `prompt()`로 표시하여 쉽게 복사 가능하도록 개선.
  - 구글 로그인 버그 2종 수정: SDK 로딩 경쟁 조건(polling으로 해결), 로그인 후 자동 로그아웃 충돌(fetchApi에서 401 자동 signOut 제거).
* **2026-05-28 (1차)**: 사용자 인증 및 팀 관리 기능 추가.
  - 실제 구글 OAuth 2.0 클라이언트 ID (`884087703847-evae2cpljosmq59e75a4ukdlgsls9mvq.apps.googleusercontent.com`)를 프론트엔드 및 백엔드에 연동 완료.
  - 다중 백엔드 주소 접속 오류 수정 및 현재 접속 호스트(window.location.origin) 1순위 적용.
  - 로그아웃(signOut) 함수 내의 API 요청 제거로 무한 401 루프 버그 수정.
  - 비로그인 상태 시 백엔드로의 초기 데이터(문서 목록, 폴더 규칙 등) 조회를 원천 차단하여 마비 장애 원인을 근본적으로 해결.
  - 팀별 데이터 완전 격리 및 권한 인증 추가 (소속 멤버 외 타 팀의 문서 조회/다운로드/업로드/검토큐/분류규칙 접근 API 전체 차단).
  - 구글 OAuth 2.0 로그인 및 로컬 개발용 가상 이메일 로그인 기능 구현.
  - JWT 기반 API 보안 인증(Authorize 헤더 연동) 추가.
  - 초대 코드를 활용한 동적 팀 생성 및 참여 기능(SQLite DB 확장) 추가.
* **2026-05-26**: 저장소 통합 작업 수행.
  - 기존 루트 디렉토리의 파일들을 `ver1/`로 이동 처리.
  - `archivon-dms` 저장소 코드를 `ver2/` 폴더에 통합 이식.
* **2026-05-26**: 폴더명 변경.
  - `ver1` -> `ver1.0`
  - `ver2` -> `ver2.0`
