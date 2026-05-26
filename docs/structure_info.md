# Document Classification Software 프로젝트 구조 정보

이 문서는 프로젝트 내 `ver1` 및 `ver2` 폴더의 역할과 구성 파일들에 대한 구조 정보 및 가이드를 담고 있습니다.

## 📂 폴더 구조 요약

```text
Document_Classification_Software/
├── docs/
│   └── structure_info.md          # [현재 문서] 프로젝트 폴더 구조 정의서
├── ver1/                          # [기존 버전] 이전 버전의 문서 분류 가이드 및 UI 프로토타입
│   ├── classification_logic_guide.md
│   ├── dowple_code_guide.md
│   └── dowple_dms.html
└── ver2/                          # [신규 버전] archivon-dms에서 통합된 풀스택 문서 관리 시스템(DMS)
    ├── backend/                   # 백엔드 서버 로직 및 데이터 처리
    ├── dowple_dms.html            # 신규 DMS 프론트엔드 인터페이스 (한국어)
    ├── dowple_dms_en.html         # 신규 DMS 프론트엔드 인터페이스 (영어)
    ├── README.md                  # ver2 메인 설명서
    └── README.ko.md               # ver2 한글 설명서
```

---

## 📁 상세 컴포넌트 정보

### 1. `ver1` (기존 버전)
이전의 문서 분류 소프트웨어 관련 가이드라인과 초기 대시보드 프로토타입을 유지하고 있는 디렉토리입니다.
* **[classification_logic_guide.md](file:///C:/Users/haner/.gemini/antigravity/scratch/Document_Classification_Software/ver1/classification_logic_guide.md)**: AI 문서 분류 모델 및 규칙 분류 로직에 대한 설명 가이드.
* **[dowple_code_guide.md](file:///C:/Users/haner/.gemini/antigravity/scratch/Document_Classification_Software/ver1/dowple_code_guide.md)**: 초기 대시보드 코드 작성 및 데이터 흐름 가이드.
* **[dowple_dms.html](file:///C:/Users/haner/.gemini/antigravity/scratch/Document_Classification_Software/ver1/dowple_dms.html)**: 기존 단일 파일 형태의 DMS(Document Management System) 대시보드 프로토타입.

### 2. `ver2` (신규 버전 - 통합)
`archivon-dms` 저장소에서 추가된 최신 DMS 풀스택 기능이 보관된 디렉토리입니다. AI 기반 텍스트 추출, 리뷰 대기열, 팀 기반 협업 등의 기능을 지원합니다.
* **`backend/`**: Express, Python 등을 활용한 문서 처리 API 서버 및 텍스트 추출 엔진.
* **[dowple_dms.html](file:///C:/Users/haner/.gemini/antigravity/scratch/Document_Classification_Software/ver2/dowple_dms.html)**: 백엔드 서버와 연동되는 리뉴얼된 다국어 지원 프론트엔드 대시보드 (한국어 버전).
* **[dowple_dms_en.html](file:///C:/Users/haner/.gemini/antigravity/scratch/Document_Classification_Software/ver2/dowple_dms_en.html)**: 리뉴얼된 프론트엔드 대시보드 (영어 버전).
* **[README.md](file:///C:/Users/haner/.gemini/antigravity/scratch/Document_Classification_Software/ver2/README.md) / [README.ko.md](file:///C:/Users/haner/.gemini/antigravity/scratch/Document_Classification_Software/ver2/README.ko.md)**: `ver2` 프로젝트의 설치, 실행 방법 및 상세 기술 스택 문서.

---

## 🔄 히스토리
* **2026-05-26**: 저장소 통합 작업 수행.
  - 기존 루트 디렉토리의 파일들을 `ver1/`로 이동 처리.
  - `archivon-dms` 저장소 코드를 `ver2/` 폴더에 통합 이식.
