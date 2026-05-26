# Dowple 대시보드 — 전체 코드 정리

> **파일 구성:** 단일 HTML 파일 (`dowple_dms.html`) — HTML + CSS + JavaScript 통합  
> **외부 의존성:** Tabler Icons CDN 1개만 사용 (인터넷 연결 필요)  
> **총 라인 수:** 609줄

---

## 목차

1. [전체 구조 개요](#1-전체-구조-개요)
2. [외부 의존성](#2-외부-의존성)
3. [CSS — 디자인 시스템 (변수 & 컴포넌트)](#3-css--디자인-시스템)
4. [HTML — 레이아웃 구조](#4-html--레이아웃-구조)
5. [JavaScript — 데이터 & 상태 관리](#5-javascript--데이터--상태-관리)
6. [JavaScript — 탭 & 네비게이션](#6-javascript--탭--네비게이션)
7. [JavaScript — 파일 업로드 & 처리 파이프라인](#7-javascript--파일-업로드--처리-파이프라인)
8. [JavaScript — AI 분류 로직](#8-javascript--ai-분류-로직)
9. [JavaScript — 문서 보관함 & 팝업 뷰어](#9-javascript--문서-보관함--팝업-뷰어)
10. [JavaScript — 자연어 검색](#10-javascript--자연어-검색)
11. [JavaScript — 용어 사전](#11-javascript--용어-사전)
12. [JavaScript — 아카이브 & 통계](#12-javascript--아카이브--통계)
13. [함수 전체 목록](#13-함수-전체-목록)

---

## 1. 전체 구조 개요

```
dowple_dms.html
├── <head>
│   ├── 메타 태그 (charset, viewport, title)
│   └── Tabler Icons CDN 링크
│
├── <style>  ← CSS 전체 (1~157줄)
│   ├── CSS 변수 (라이트/다크 테마)
│   ├── 레이아웃 (사이드바, 메인, 탑바)
│   └── 컴포넌트 (버튼, 카드, 배지, 테이블 등)
│
├── <body>
│   ├── .app (grid: 사이드바 220px + 메인 1fr)
│   │   ├── .sidebar
│   │   │   ├── 로고
│   │   │   ├── 메뉴 네비게이션 (5개 탭)
│   │   │   ├── 폴더 구조 (5개 폴더 + 뱃지)
│   │   │   └── 시스템 상태 인디케이터
│   │   └── .main
│   │       ├── .topbar (제목 + 버튼)
│   │       └── .content
│   │           ├── #tab-upload  (업로드 & 파이프라인 현황)
│   │           ├── #tab-docs    (문서 보관함 테이블)
│   │           ├── #tab-search  (자연어 검색)
│   │           ├── #tab-dict    (용어 사전)
│   │           └── #tab-archive (아카이브 제안)
│   ├── #toast (알림 토스트)
│   └── #docPop2 (팝업 뷰어)
│
└── <script>  ← JavaScript 전체 (307~606줄)
    ├── 전역 데이터 (DB, STATS, DICT, FOLDERS, RMAP)
    ├── 네비게이션 함수
    ├── 파일 처리 파이프라인
    ├── AI 분류 시뮬레이션
    ├── CRUD (저장/삭제/렌더)
    └── 검색 / 용어사전 / 아카이브
```

---

## 2. 외부 의존성

```html
<!-- Tabler Icons (아이콘 폰트) — 유일한 외부 CDN -->
<link
  rel="stylesheet"
  href="https://cdn.jsdelivr.net/npm/@tabler/icons-webfont@3.30.0/dist/tabler-icons.min.css"
>
```

사용 방법:
```html
<i class="ti ti-upload"></i>       <!-- 업로드 아이콘 -->
<i class="ti ti-file-text"></i>    <!-- 파일 아이콘 -->
<i class="ti ti-check"></i>        <!-- 체크 아이콘 -->
<i class="ti ti-search"></i>       <!-- 검색 아이콘 -->
<i class="ti ti-archive"></i>      <!-- 아카이브 아이콘 -->
```

---

## 3. CSS — 디자인 시스템

### 3-1. CSS 변수 (라이트 테마 기본값)

```css
:root {
  /* 배경 */
  --bg: #f8f7f4;          /* 페이지 배경 */
  --surface: #ffffff;      /* 카드/사이드바 배경 */
  --surface2: #f3f2ef;     /* 보조 배경 (호버, 입력 등) */

  /* 브랜드 색상 */
  --accent: #1D9E75;       /* 주 색상 (버튼, 액센트) */
  --accent2: #0F6E56;      /* 주 색상 다크 (호버) */
  --accent-light: #E1F5EE; /* 주 색상 연하게 (선택 배경) */

  /* 텍스트 */
  --text: #1a1a1a;         /* 기본 텍스트 */
  --text2: #555;           /* 보조 텍스트 */
  --text3: #888;           /* 흐린 텍스트 (레이블, 설명) */

  /* 테두리 */
  --border: rgba(0,0,0,0.1);   /* 기본 테두리 */
  --border2: rgba(0,0,0,0.18); /* 강조 테두리 */

  /* 상태 색상 (info/warn/success/danger) */
  --info-bg: #EFF6FF;      --info-text: #1d4ed8;    --info-border: #bfdbfe;
  --warn-bg: #FFFBEB;      --warn-text: #92400e;    --warn-border: #fde68a;
  --success-bg: #ECFDF5;   --success-text: #065f46; --success-border: #a7f3d0;
  --danger-bg: #FEF2F2;    --danger-text: #991b1b;  --danger-border: #fca5a5;

  /* 기타 */
  --r-md: 8px;    /* 기본 border-radius */
  --r-lg: 12px;
  --r-xl: 16px;
  --shadow: 0 1px 4px rgba(0,0,0,0.06), 0 4px 16px rgba(0,0,0,0.04);
}
```

### 3-2. 다크 모드 (자동 감지)

```css
@media (prefers-color-scheme: dark) {
  :root {
    --bg: #0d0d0d;
    --surface: #181818;
    --surface2: #222;
    --text: #e8e8e8;
    --text2: #aaa;
    --text3: #666;
    --border: rgba(255,255,255,0.1);
    --border2: rgba(255,255,255,0.18);
    /* 상태 색상도 다크 버전으로 오버라이드 */
    --info-bg: #1e2a3a;     --info-text: #93c5fd;
    --warn-bg: #2a2010;     --warn-text: #fcd34d;
    --success-bg: #0d2018;  --success-text: #6ee7b7;
    --danger-bg: #2a1010;   --danger-text: #fca5a5;
    --shadow: 0 1px 4px rgba(0,0,0,0.3);
  }
}
```

### 3-3. 레이아웃 클래스

```css
/* 전체 앱 — 2열 그리드 */
.app {
  display: grid;
  grid-template-columns: 220px 1fr;
  min-height: 100vh;
}

/* 사이드바 — 고정 위치 */
.sidebar {
  background: var(--surface);
  border-right: 0.5px solid var(--border);
  position: sticky;
  top: 0;
  height: 100vh;
  overflow-y: auto;
}

/* 메인 영역 */
.main {
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

/* 스크롤 가능한 콘텐츠 영역 */
.content {
  flex: 1;
  padding: 24px;
  overflow-y: auto;
}

/* 2열 / 4열 그리드 */
.g2 { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
.g4 { display: grid; grid-template-columns: repeat(4, 1fr); gap: 10px; }

/* 반응형: 680px 이하에서 사이드바 숨김 */
@media (max-width: 680px) {
  .app { grid-template-columns: 1fr; }
  .sidebar { display: none; }
  .g2, .g4 { grid-template-columns: 1fr; }
}
```

### 3-4. 컴포넌트 클래스 목록

| 클래스 | 설명 |
|--------|------|
| `.btn` | 기본 버튼 |
| `.btn-primary` | 주 색상 채운 버튼 |
| `.card` | 흰 배경 + 테두리 + 그림자 카드 |
| `.badge` | 인라인 상태 뱃지 |
| `.b-info` `.b-success` `.b-warn` `.b-danger` | 뱃지 색상 변형 |
| `.tabs` / `.tab` | 탭 컨테이너 / 개별 탭 |
| `.panel` / `.panel.active` | 탭 패널 표시/숨김 |
| `.drop-zone` | 파일 드래그 업로드 영역 |
| `.drop-zone.drag` | 드래그 중 강조 상태 |
| `.phase-row` | 파이프라인 단계 행 |
| `.phase-num` | 단계 번호 원형 |
| `.phase-num.done` | 완료된 단계 (초록) |
| `.phase-num.running` | 진행 중 단계 (파랑) |
| `.pbar` / `.pfill` | 프로그레스 바 컨테이너/채움 |
| `.evidence` | 증거 패키지 monospace 박스 |
| `.rec-item` | 분류 추천 항목 |
| `.rec-item.sel` | 선택된 추천 항목 |
| `.sbar` / `.sfill` | 유사도 점수 바 |
| `.dtable` | 문서 목록 테이블 |
| `.popup` / `.popup.show` | 팝업 뷰어 |
| `.search-inp` | 검색 입력창 |
| `.chip` | 추천 검색어 칩 |
| `.sr-card` | 검색 결과 카드 |
| `.dict-row` | 용어 사전 행 |
| `.dr-from` `.dr-mid` `.dr-to` | 용어 매핑 3단계 셀 |
| `.arch-row` | 아카이브 제안 행 |
| `.ver-alert` | 버전 감지 경고 박스 |
| `.toast` / `.toast.show` | 토스트 알림 |
| `.metric` | KPI 지표 카드 |
| `.sb-item` | 사이드바 메뉴 항목 |
| `.sb-folder` | 사이드바 폴더 항목 |
| `.fb` | 폴더 문서 수 뱃지 |

---

## 4. HTML — 레이아웃 구조

### 4-1. 사이드바

```html
<div class="sidebar">
  <!-- 로고 -->
  <div class="logo">
    <div class="logo-mark"><i class="ti ti-file-check"></i></div>
    <div>
      <div class="logo-text">Dowple</div>
      <div class="logo-sub">문서 지능 관리 시스템</div>
    </div>
  </div>

  <!-- 메뉴 네비게이션 -->
  <div class="sb-section">메뉴</div>
  <div class="sb-item active" onclick="goTab('upload'); setNav(this)">
    <i class="ti ti-upload"></i> 문서 업로드
  </div>
  <div class="sb-item" onclick="goTab('docs'); setNav(this)">
    <i class="ti ti-files"></i> 문서 보관함
  </div>
  <!-- ... 나머지 메뉴 항목들 ... -->

  <!-- 폴더 구조 (뱃지 숫자는 JS가 업데이트) -->
  <div class="sb-section">폴더 구조</div>
  <div class="sb-folder" onclick="goFolder(0)">
    <span><i class="ti ti-folder"></i>재무/회계</span>
    <span class="fb" id="fb0">0</span>  <!-- JS: updFolders()가 갱신 -->
  </div>
  <!-- fb1~fb4 동일 패턴 -->

  <!-- 시스템 상태 -->
  <div class="stat-row">
    <div class="status-dot dot-g"></div>OCR 엔진 정상
  </div>
</div>
```

### 4-2. 파이프라인 현황 탭 (정적 UI)

```html
<!-- 4개 KPI 지표 카드 — JS가 id로 내용 업데이트 -->
<div class="g4">
  <div class="metric">
    <div class="metric-lbl">총 처리 문서</div>
    <div class="metric-val" id="st0">0</div>  <!-- updStats()가 갱신 -->
    <div class="metric-sub">누적</div>
  </div>
  <!-- st1(성공률), st2(평균시간), st3(OCR건수) 동일 패턴 -->
</div>

<!-- 파이프라인 단계 (정적 표시용 — 이미 완료 상태) -->
<div class="phase-row">
  <div class="phase-num done">
    <i class="ti ti-check" style="font-size:12px"></i>
  </div>
  <div style="flex:1">
    <div class="phase-label">1 — 병렬 전처리 (핵심 구역 파싱)</div>
    <div class="phase-desc">표지·헤더·목차·결론부 추출 → 4,500자 증거 패키지</div>
    <div class="pbar"><div class="pfill" style="width:100%"></div></div>
  </div>
</div>
```

### 4-3. 업로드 영역

```html
<!-- 드래그 앤 드롭 영역 -->
<div class="drop-zone" id="dz"
  onclick="document.getElementById('fi').click()"
  ondragover="doDrag(event, true)"
  ondragleave="doDrag(event, false)"
  ondrop="doDrop(event)">
  <i class="ti ti-cloud-upload"></i>
  <p>파일을 드래그하거나 클릭하여 업로드</p>
  <span>PDF, HWP, DOCX, 이미지(PNG/JPG), 음성(MP3) 지원</span>
</div>

<!-- 실제 파일 입력 (숨김) -->
<input type="file" id="fi" style="display:none"
  multiple
  accept=".pdf,.hwp,.docx,.png,.jpg,.jpeg,.mp3,.m4a"
  onchange="handleFiles(this.files)">

<!-- 처리 결과 카드들이 이 안에 prepend됨 -->
<div id="procArea"></div>
```

---

## 5. JavaScript — 데이터 & 상태 관리

```javascript
// ─── 전역 데이터 ───────────────────────────────────────

// 문서 데이터베이스 (메모리 저장)
var DB = [];
// 각 항목 구조:
// {
//   filename: "파일명.pdf",
//   category: "재무/회계",
//   summary:  "AI 요약 텍스트",
//   date:     "2024-04-10",
//   status:   "정상",
//   extraction: "텍스트 파싱" | "OCR 하이브리드" | "STT 변환"
// }

// 통계 상태
var STATS = {
  total: 0,    // 총 처리 문서 수
  succ:  0,    // 성공 분류 수
  ocr:   0,    // OCR/STT 처리 수
  times: []    // 각 처리 시간 배열 (초 단위)
};

// 자연어 사전 (현업 용어 → 표준 분류어)
var DICT = {
  "찐최종":    "최종본",
  "결산안":    "재무제표",
  "영수증":    "회계",
  "기획안":    "사업기획",
  "회계자료":  "재무/회계",
  "인력관리":  "인사/총무",
  "개발보고":  "기술개발",
  "마케팅자료":"마케팅/영업",
  "부가세":    "부가가치세",
  "MOU":       "업무협약서"
};

// 폴더 목록 (인덱스 0~4)
var FOLDERS = ["재무/회계", "인사/총무", "기술개발", "마케팅/영업", "미분류"];

// 폴더별 뱃지 CSS 클래스
var FBADGE = ["b-info", "b-warn", "b-success", "b-danger", ""];

// 키워드 → 폴더 인덱스 매핑 (Rule-based 분류용)
var RMAP = {
  "재무": 0, "회계": 0, "부가세": 0, "재무제표": 0, "세금": 0, "영수증": 0,
  "인사": 1, "총무": 1, "채용":   1, "급여":    1, "인력":  1,
  "개발": 2, "기술": 2, "연구":   2,
  "마케팅": 3, "영업": 3, "광고":  3, "제안":   3
};

// 현재 보관함 필터 상태
var curFilt = "";
```

---

## 6. JavaScript — 탭 & 네비게이션

```javascript
// 메인 탭 전환 (upload / docs / search / dict / archive)
function goTab(id) {
  // 모든 탭 패널 숨기기
  document.querySelectorAll('.panel[id^="tab-"]').forEach(p => p.classList.remove('active'));
  // 선택한 탭 보이기
  document.getElementById('tab-' + id).classList.add('active');
  // 탭별 초기화 함수 호출
  if (id === 'docs')    renderDocs();
  if (id === 'archive') renderArch();
}

// 사이드바 활성 항목 표시
function setNav(el) {
  document.querySelectorAll('.sb-item').forEach(x => x.classList.remove('active'));
  el.classList.add('active');
}

// 사이드바 항목을 인덱스로 활성화 (프로그래밍 방식)
function setNavByIdx(i) {
  var items = document.querySelectorAll('.sb-item');
  items.forEach(x => x.classList.remove('active'));
  items[i].classList.add('active');
}

// 폴더 클릭 → 보관함 탭으로 이동 + 해당 폴더 필터 적용
function goFolder(i) {
  curFilt = FOLDERS[i];
  document.getElementById('catF').value = FOLDERS[i];
  goTab('docs');
  setNavByIdx(1); // 사이드바 '문서 보관함' 활성화
}

// 서브 탭 전환 (업로드 탭 내부의 "업로드 및 분류" / "파이프라인 현황")
function subTab(id, el) {
  var parent = el.closest('.panel[id^="tab-"]');
  // 동일 탭 내 모든 패널 숨기기
  parent.querySelectorAll('.panel').forEach(x => x.classList.remove('active'));
  document.getElementById(id).classList.add('active');
  // 탭 버튼 활성 상태 업데이트
  el.closest('.tabs').querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
  el.classList.add('active');
}

// 토스트 알림 표시
// type: 'success' | 'warn' | 'info' | 'danger'
function toast(msg, type) {
  var t = document.getElementById('toast');
  t.style.background   = 'var(--' + (type || 'success') + '-bg)';
  t.style.color        = 'var(--' + (type || 'success') + '-text)';
  t.style.borderColor  = 'var(--' + (type || 'success') + '-border)';
  t.innerHTML = '<i class="ti ti-check"></i> ' + msg;
  t.classList.add('show');
  setTimeout(() => t.classList.remove('show'), 2600);
}
```

---

## 7. JavaScript — 파일 업로드 & 처리 파이프라인

```javascript
// 드래그 이벤트 핸들러
function doDrag(e, on) {
  e.preventDefault();
  document.getElementById('dz').className = 'drop-zone' + (on ? ' drag' : '');
}
function doDrop(e) {
  e.preventDefault();
  doDrag(e, false);
  handleFiles(e.dataTransfer.files);
}

// 파일 배열 순회 처리
function handleFiles(files) {
  Array.from(files).forEach(processFile);
}

// ─── 핵심: 단일 파일 처리 파이프라인 ───────────────────
function processFile(file) {
  var area  = document.getElementById('procArea');
  // 고유 ID 생성 (타임스탬프 + 랜덤)
  var id    = 'p' + Date.now() + Math.random().toString(36).slice(2, 6);
  var isImg = file.type.startsWith('image/');   // 이미지 → OCR 경로
  var isAu  = file.type.startsWith('audio/');   // 음성  → STT 경로

  // 처리 카드 동적 생성 (3단계 파이프라인 UI)
  var wrap = document.createElement('div');
  wrap.className = 'card';
  wrap.id = id;
  wrap.innerHTML = `
    <div class="card-hdr">
      <div class="card-title"><i class="ti ti-file-text"></i> ${file.name}</div>
      <span class="badge b-info" id="${id}-badge">처리중</span>
    </div>
    <div id="${id}-phases">
      <!-- Phase 1: 전처리 -->
      <div class="phase-row">
        <div class="phase-num running" id="${id}-p1">1</div>
        <div style="flex:1">
          <div class="phase-label">병렬 전처리 — 핵심 구역 파싱</div>
          <div class="phase-desc">
            ${isImg ? 'EasyOCR 하이브리드 처리 (이미지 감지됨)'
                    : isAu ? 'Whisper STT 음성 변환'
                           : 'PyMuPDF 병렬 구역 파싱'}
          </div>
          <div class="pbar"><div class="pfill" id="${id}-b1" style="width:0%"></div></div>
        </div>
      </div>
      <!-- Phase 2: 정규화 -->
      <div class="phase-row">
        <div class="phase-num" id="${id}-p2">2</div>
        ...
      </div>
      <!-- Phase 3: 분류 -->
      <div class="phase-row">
        <div class="phase-num" id="${id}-p3">3</div>
        ...
      </div>
    </div>
    <div id="${id}-res" style="display:none"></div>`;

  area.prepend(wrap); // 가장 최근 파일을 위에 표시

  // ─── 순차적 파이프라인 실행 (콜백 체인) ───
  var t0 = Date.now();

  // Phase 1: 전처리 (900ms 애니메이션)
  anim(id, 'b1', 900, () => {
    mark(id, 'p1', 'done');
    mark(id, 'p2', 'running');
    var ev = buildEv(file.name, isImg, isAu);  // 증거 패키지 생성

    // Phase 2: 정규화 (600ms 애니메이션)
    anim(id, 'b2', 600, () => {
      mark(id, 'p2', 'done');
      mark(id, 'p3', 'running');
      var norm  = doNorm(file.name + ' ' + ev);  // 용어 정규화
      var found = getFound(file.name);            // 정규화된 용어 목록

      // Phase 3: 분류 (800ms 애니메이션)
      anim(id, 'b3', 800, () => {
        mark(id, 'p3', 'done');
        var recs   = classify(norm, file.name);  // Top 3 분류 추천
        var verMsg = checkVer(file.name);        // 버전 감지
        var elapsed = ((Date.now() - t0) / 1000).toFixed(1);

        STATS.times.push(parseFloat(elapsed));
        if (isImg || isAu) STATS.ocr++;
        updStats();
        showRes(id, file, recs, verMsg, ev, found, isImg, isAu, elapsed);
      });
    });
  });
}

// ─── 프로그레스 바 애니메이션 ───────────────────────────
// requestAnimationFrame 기반 부드러운 채움 애니메이션
function anim(id, barId, duration, callback) {
  var el = document.getElementById(id + '-' + barId);
  if (!el) { if (callback) callback(); return; }
  var startTime = null;
  (function step(timestamp) {
    if (!startTime) startTime = timestamp;
    var progress = Math.min((timestamp - startTime) / duration, 1);
    el.style.width = (progress * 100) + '%';
    if (progress < 1) requestAnimationFrame(step);
    else if (callback) callback();
  })(performance.now());
}

// 단계 번호 상태 변경 (running → done)
function mark(id, phaseId, cls) {
  var el = document.getElementById(id + '-' + phaseId);
  if (!el) return;
  el.className = 'phase-num ' + (cls === 'done' ? 'done' : cls === 'running' ? 'running' : '');
  if (cls === 'done')    el.innerHTML = '<i class="ti ti-check" style="font-size:12px"></i>';
  if (cls === 'running') el.innerHTML = phaseId.replace(id + '-p', '');
}
```

---

## 8. JavaScript — AI 분류 로직

```javascript
// ─── 증거 패키지 생성 (파일명 기반 시뮬레이션) ──────────
// 실제 구현: PyMuPDF로 표지/헤더/결론부 추출 → 4,500자 이내로 압축
function buildEv(name, isImg, isAu) {
  var n = name.toLowerCase();
  if (isImg) return '[OCR] 표지 상단 30% 추출: "2024년 스캔 문서 — 업무보고서"';
  if (isAu)  return '[STT] "오늘 회의에서는 3분기 마케팅 예산안 및 매출 전략에 대해..."';
  if (n.includes('재무') || n.includes('결산') || n.includes('세금'))
    return '표지: "2024 재무결산 보고서" / 목차: 손익계산서, 부가세 신고 / 결론: 전년比 +15%';
  if (n.includes('인사') || n.includes('채용'))
    return '표지: "2024 하반기 공개채용 지원서" / 항목: 자기소개서, 경력기술서';
  if (n.includes('마케팅') || n.includes('영업'))
    return '표지: "Q3 마케팅 전략 제안서" / 목차: 시장분석, 캠페인 / 결론: ROI 220%';
  if (n.includes('개발') || n.includes('기술'))
    return '표지: "기술개발 현황 보고" / 목차: 스프린트 결과, 로드맵';
  return '표지: "' + name + '" / 키워드: 보고서, 문서, 2024년 업무';
}

// ─── 자연어 사전 정규화 ─────────────────────────────────
// DICT의 모든 키를 표준 분류어로 치환
function doNorm(text) {
  var t = text;
  Object.keys(DICT).forEach(k => { t = t.split(k).join(DICT[k]); });
  return t;
}

// 파일명에서 정규화된 용어 목록 추출 (UI 표시용)
function getFound(name) {
  var found = [];
  var n = name.toLowerCase();
  Object.keys(DICT).forEach(k => {
    if (n.includes(k.toLowerCase())) found.push(DICT[k]);
  });
  return found;
}

// ─── 하이브리드 분류 (Rule-based + 유사도 스코어링) ────
function classify(text, fname) {
  var t = text.toLowerCase();
  var scores = [0, 0, 0, 0, 0]; // 폴더 인덱스별 점수

  // 1차: Rule-based 키워드 매칭 (+18점씩)
  Object.keys(RMAP).forEach(k => {
    if (t.includes(k)) scores[RMAP[k]] += 18;
  });

  // 노이즈 추가 (시뮬레이션용 ±5)
  scores = scores.map(s => s + Math.floor(Math.random() * 5));

  // 상위 3개 폴더 추출
  var sorted = scores
    .map((s, i) => ({ i, s }))
    .sort((a, b) => b.s - a.s)
    .slice(0, 3);

  var base = sorted[0].s || 30;
  var recs = [
    { folder: FOLDERS[sorted[0].i], score: Math.min(95, base + Math.floor(Math.random() * 8)) },
    { folder: FOLDERS[sorted[1].i], score: Math.max(30, base - 20 + Math.floor(Math.random() * 6)) },
    { folder: FOLDERS[sorted[2].i], score: Math.max(12, base - 38 + Math.floor(Math.random() * 6)) }
  ];

  // 노이즈 파일명이고 점수 낮으면 미분류로 처리
  var noisy = ['최종', '진짜최종', '수정본', 'v2', 'tmp']
    .some(x => fname.toLowerCase().includes(x));
  if (recs[0].score < 40 && noisy) {
    recs[0].folder = '미분류';
    recs[0].score  = 45;
  }

  return recs;
  // 반환값: [{folder: "재무/회계", score: 88}, {folder: "기술개발", score: 52}, ...]
}

// ─── 버전 감지 ──────────────────────────────────────────
// 파일명에 수정/버전 패턴이 있으면 경고 메시지 반환
function checkVer(fname) {
  var patterns = ['v2', 'v3', '_수정', '_최종', '최종본', '진짜최종'];
  return patterns.some(x => fname.toLowerCase().includes(x))
    ? '동일 폴더 내 유사 문서 발견 — 기존 파일의 수정본으로 저장하시겠습니까? (유사도 87%)'
    : null;
}

// ─── AI 요약 생성 ────────────────────────────────────────
function buildSum(name, folder, found) {
  if (folder.includes('재무'))
    return name + '은 재무·회계 관련 문서입니다. 전년 대비 매출 현황이 결론부에 포함되어 있습니다.'
           + (found.length ? ' 정규화: ' + found.join(', ') : '');
  if (folder.includes('인사'))
    return name + '은 인사·채용 관련 문서입니다. 지원자 정보 및 채용 기준이 포함됩니다.';
  if (folder.includes('마케팅'))
    return name + '은 마케팅·영업 제안 문서입니다. 캠페인 전략 및 ROI 분석이 포함됩니다.';
  if (folder.includes('기술'))
    return name + '은 기술개발 보고 문서입니다. 스프린트 현황 및 다음 로드맵이 기술됩니다.';
  return name + '은 업로드된 문서입니다. AI가 내용을 분석하여 분류를 추천했습니다.';
}

// ─── 결과 UI 렌더링 ─────────────────────────────────────
function showRes(id, file, recs, verMsg, ev, found, isImg, isAu, elapsed) {
  var card  = document.getElementById(id); if (!card) return;
  var badge = document.getElementById(id + '-badge');
  badge.className   = 'badge b-success';
  badge.textContent = '완료 (' + elapsed + '초)';

  var summary = buildSum(file.name, recs[0].folder, found);
  var em = isImg ? 'OCR 하이브리드' : isAu ? 'STT 변환' : '텍스트 파싱';
  var res = document.getElementById(id + '-res');
  res.style.display = 'block';

  // 2열 그리드: 왼쪽(증거 패키지 + 요약) / 오른쪽(분류 추천 Top 3)
  res.innerHTML = `
    <div style="border-top:0.5px solid var(--border); margin-top:12px; padding-top:14px">
      <div class="g2" style="gap:16px">
        <div>
          <div class="evidence">${ev}</div>
          ${found.length ? `<div>정규화 용어: ${found.join(', ')}</div>` : ''}
          <div>AI 요약: ${summary}</div>
          ${verMsg ? `<div class="ver-alert"><i class="ti ti-alert-triangle"></i>${verMsg}</div>` : ''}
        </div>
        <div>
          <!-- Top 3 추천 목록 -->
          <div id="${id}-recs">
            ${recs.map((r, i) => `
              <div class="rec-item ${i === 0 ? 'sel' : ''}"
                   onclick="selRec('${id}', ${i})"
                   id="${id}-r${i}">
                <div class="rl"><i class="ti ti-folder"></i>${r.folder}</div>
                <div>
                  <div class="sbar">
                    <div class="sfill" style="width:${r.score}%;
                      background:${i===0 ? 'var(--accent)' : i===1 ? '#BA7517' : '#888'}">
                    </div>
                  </div>
                  <span class="rs">${r.score}%</span>
                </div>
              </div>`).join('')}
          </div>
          <button class="btn btn-primary"
            onclick="saveDoc('${id}','${file.name}','${em}','${summary}')">
            <i class="ti ti-check"></i> 확인 및 저장
          </button>
        </div>
      </div>
    </div>`;
}

// 추천 항목 선택/해제
function selRec(id, idx) {
  for (var i = 0; i < 3; i++) {
    var el = document.getElementById(id + '-r' + i);
    if (el) el.classList.toggle('sel', i === idx);
  }
}

// 문서 저장 (DB에 추가)
function saveDoc(id, fname, em, summary) {
  var sel = document.querySelector('#' + id + '-recs .rec-item.sel');
  if (!sel) { toast('분류를 선택해주세요', 'warn'); return; }

  var folder = sel.querySelector('.rl').textContent.trim();
  var today  = new Date().toISOString().slice(0, 10);

  DB.push({ filename: fname, category: folder, summary: summary,
            date: today, status: '정상', extraction: em });
  STATS.total++;
  STATS.succ++;

  updFolders(); // 사이드바 폴더 뱃지 갱신
  updStats();   // KPI 지표 갱신

  // 카드 페이드아웃 후 제거
  var card = document.getElementById(id);
  if (card) {
    card.style.opacity = '0.4';
    setTimeout(() => { if (card.parentNode) card.parentNode.removeChild(card); }, 700);
  }
  toast(fname + ' 저장 완료!');
}
```

---

## 9. JavaScript — 문서 보관함 & 팝업 뷰어

```javascript
// 문서 보관함 테이블 렌더링
function renderDocs() {
  var filt = document.getElementById('catF').value; // 현재 필터 값
  var rows = DB.filter(d => !filt || d.category === filt);
  var tb   = document.getElementById('docsTb');
  if (!tb) return;

  if (!rows.length) {
    tb.innerHTML = '<tr><td colspan="6" style="text-align:center;padding:28px;color:var(--text3)">저장된 문서가 없습니다.</td></tr>';
    return;
  }

  tb.innerHTML = rows.map((d, i) => `
    <tr>
      <!-- 파일명: 마우스오버 시 팝업 표시 -->
      <td style="color:var(--info-text)"
          onmouseenter="showPop(event, ${i}, '${filt}')"
          onmouseleave="hidePop()">
        <i class="ti ti-file-text"></i> ${d.filename}
      </td>
      <td><span class="badge ${FBADGE[FOLDERS.indexOf(d.category)] || ''}">${d.category}</span></td>
      <td style="color:var(--text3)">${d.extraction}</td>
      <td style="color:var(--text3)">${d.date}</td>
      <td><span class="badge b-success">${d.status}</span></td>
      <!-- 삭제 버튼 -->
      <td>
        <button class="btn" onclick="delDoc(${i}, '${filt}')">
          <i class="ti ti-trash"></i>
        </button>
      </td>
    </tr>`).join('');
}

// 팝업 뷰어 표시 (마우스 위치에 고정)
function showPop(e, idx, filt) {
  var rows = DB.filter(d => !filt || d.category === filt);
  var d = rows[idx]; if (!d) return;
  var p = document.getElementById('docPop2');

  p.innerHTML = `
    <div class="pop-title"><i class="ti ti-eye"></i> 팝업 뷰어</div>
    <div class="pop-row"><span class="pop-k">파일명</span><span class="pop-v">${d.filename}</span></div>
    <div class="pop-row"><span class="pop-k">분류</span><span class="pop-v">${d.category}</span></div>
    <div class="pop-row"><span class="pop-k">저장일</span><span class="pop-v">${d.date}</span></div>
    <div class="pop-row"><span class="pop-k">상태</span><span class="pop-v">${d.status}</span></div>
    <div style="border-top:0.5px solid var(--border); margin-top:7px; padding-top:7px;
                font-size:11px; color:var(--text3); line-height:1.5">${d.summary}</div>`;

  // 마우스 위치 오른쪽, 화면 아래로 넘치지 않게 조정
  p.style.left = (e.clientX + 12) + 'px';
  p.style.top  = Math.min(e.clientY - 20, window.innerHeight - 220) + 'px';
  p.classList.add('show');
}

function hidePop() {
  document.getElementById('docPop2').classList.remove('show');
}

// 문서 삭제
function delDoc(idx, filt) {
  var rows = DB.map((d, i) => ({ d, i })).filter(x => !filt || x.d.category === filt);
  if (rows[idx]) DB.splice(rows[idx].i, 1);
  updFolders();
  renderDocs();
}

// 사이드바 폴더 뱃지 숫자 갱신
function updFolders() {
  var counts = [0, 0, 0, 0, 0];
  DB.forEach(d => {
    var i = FOLDERS.indexOf(d.category);
    if (i >= 0) counts[i]++;
  });
  counts.forEach((v, i) => {
    var el = document.getElementById('fb' + i);
    if (el) el.textContent = v;
  });
}

// KPI 지표 갱신
function updStats() {
  document.getElementById('st0').textContent = STATS.total;
  document.getElementById('st3').textContent = STATS.ocr;
  if (STATS.total > 0) {
    document.getElementById('st1').textContent =
      Math.round(STATS.succ / STATS.total * 100) + '%';
  }
  if (STATS.times.length > 0) {
    var avg = STATS.times.reduce((a, b) => a + b) / STATS.times.length;
    document.getElementById('st2').textContent = avg.toFixed(1) + '초';
  }
}
```

---

## 10. JavaScript — 자연어 검색

```javascript
// 검색 실행 (Enter 키 또는 버튼 클릭)
function doSearch() {
  var q = document.getElementById('sInput').value.trim();
  if (!q) return;

  // 검색어도 DICT로 정규화 (은어 처리)
  var norm = doNorm(q.toLowerCase());

  // DB 전체를 스코어링
  var scored = DB.map(d => {
    var t  = doNorm((d.filename + ' ' + d.category + ' ' + d.summary).toLowerCase());
    var sc = 0;

    // 1차: 정규화된 검색어로 메타데이터 검색 (+20점)
    norm.split(' ').forEach(w => { if (w && t.includes(w)) sc += 20; });

    // 2차: 원문 검색어로 원문 검색 (+10점, 폴백)
    q.toLowerCase().split(' ').forEach(w => { if (w && t.includes(w)) sc += 10; });

    return { d, sc };
  })
  .filter(x => x.sc > 0)
  .sort((a, b) => b.sc - a.sc);

  renderSearchResults(q, scored);
}

// 추천 검색어 칩 클릭
function qSearch(q) {
  document.getElementById('sInput').value = q;
  doSearch();
}

// 검색 결과 렌더링
function renderSearchResults(q, scored) {
  var el = document.getElementById('sResults');

  if (!scored.length) {
    el.innerHTML = `<div style="text-align:center; padding:28px; color:var(--text3)">
      <i class="ti ti-search-off" style="font-size:28px; display:block; margin-bottom:8px"></i>
      "${q}"에 해당하는 문서를 찾지 못했습니다.
    </div>`;
    return;
  }

  el.innerHTML =
    `<div style="font-size:11px; color:var(--text3); margin-bottom:10px">
       "${q}" 검색 결과 ${scored.length}건 — 메타데이터 1차 검색 적용
     </div>` +
    scored.map(({ d, sc }) => `
      <div class="sr-card">
        <div style="display:flex; justify-content:space-between">
          <div style="font-size:13px; font-weight:500">
            <i class="ti ti-file-text"></i> ${d.filename}
          </div>
          <span class="badge b-info">관련도 ${Math.min(99, sc)}%</span>
        </div>
        <div style="font-size:11px; color:var(--text3); margin-top:4px">
          <span class="badge ${FBADGE[FOLDERS.indexOf(d.category)] || ''}">${d.category}</span>
          ${d.date} · ${d.extraction}
        </div>
        <div style="font-size:11px; color:var(--success-text); margin-top:5px">
          <i class="ti ti-sparkles"></i> ${d.summary.slice(0, 90)}...
        </div>
      </div>`).join('');
}
```

---

## 11. JavaScript — 용어 사전

```javascript
// 용어 사전 테이블 렌더링
function renderDict() {
  var el = document.getElementById('dictRows');
  if (!el) return;
  el.innerHTML = Object.keys(DICT).map(k => `
    <div class="dict-row">
      <span class="dr-from">${k}</span>    <!-- 빨간 배경: 은어/줄임말 -->
      <span class="dr-mid"> → </span>       <!-- 화살표 -->
      <span class="dr-to">${DICT[k]}</span> <!-- 초록 배경: 표준 분류어 -->
    </div>`).join('');
}

// 사용자 정의 용어 추가
function addDict() {
  var from = document.getElementById('dFrom').value.trim();
  var to   = document.getElementById('dTo').value.trim();

  if (!from || !to) { toast('두 항목 모두 입력해주세요', 'warn'); return; }

  DICT[from] = to;   // 전역 DICT에 추가 → 이후 모든 doNorm()에서 적용
  renderDict();      // UI 갱신

  document.getElementById('dFrom').value = '';
  document.getElementById('dTo').value   = '';
  toast('용어 추가: ' + from + ' → ' + to);
}
```

---

## 12. JavaScript — 아카이브 & 통계

```javascript
// 아카이브 목록 렌더링 (현재는 DB 전체를 후보로 표시)
function renderArch() {
  var el = document.getElementById('archList');
  if (!el) return;

  if (!DB.length) {
    el.innerHTML = '<div style="text-align:center; padding:28px; color:var(--text3)">아카이빙 제안 대상이 없습니다.</div>';
    return;
  }

  el.innerHTML = DB.map((d, i) => `
    <div class="arch-row" id="ar${i}">
      <div>
        <div class="arch-name"><i class="ti ti-file-text"></i> ${d.filename}</div>
        <div class="arch-meta">${d.category} · ${d.date} · 90일 이상 미열람 추정</div>
      </div>
      <button class="btn" onclick="archOne(${i})">
        <i class="ti ti-archive"></i> 이동
      </button>
    </div>`).join('');
}

// 개별 아카이브 이동 (UI 페이드아웃)
function archOne(i) {
  var el = document.getElementById('ar' + i);
  if (el) {
    el.style.opacity = '0.3';
    el.querySelector('button').disabled = true;
  }
  toast('아카이브로 이동되었습니다');
}

// 전체 아카이브 이동
function archAll() {
  document.querySelectorAll('[id^="ar"]').forEach(el => {
    el.style.opacity = '0.3';
    var b = el.querySelector('button');
    if (b) b.disabled = true;
  });
  toast('전체 문서가 아카이브로 이동되었습니다');
}

// ─── 초기화 (페이지 로드 시) ────────────────────────────
renderDict(); // 용어 사전 초기 렌더링

// 데모용 샘플 데이터 2건 자동 삽입
setTimeout(() => {
  DB.push({
    filename: '2024_재무결산보고서.pdf',
    category: '재무/회계',
    summary:  '2024 상반기 재무 결산 보고서. 전년 대비 매출 15% 성장, 부가가치세 신고 현황 포함.',
    date:     '2024-04-10',
    status:   '정상',
    extraction: '텍스트 파싱'
  });
  DB.push({
    filename: 'Q3_마케팅전략_최종.docx',
    category: '마케팅/영업',
    summary:  'Q3 마케팅 전략 제안서. 캠페인 기획 및 ROI 분석 포함. 버전 2.0.',
    date:     '2024-03-22',
    status:   '정상',
    extraction: '텍스트 파싱'
  });
  STATS.total = 2;
  STATS.succ  = 2;
  STATS.times = [2.1, 3.4];
  updFolders();
  updStats();
}, 100);
```

---

## 13. 함수 전체 목록

| 함수명 | 역할 |
|--------|------|
| `doDrag(e, on)` | 드래그 이벤트 처리 (drag 클래스 토글) |
| `doDrop(e)` | 드롭 이벤트 처리 → handleFiles 호출 |
| `handleFiles(files)` | FileList → 개별 processFile 순회 |
| `processFile(file)` | 단일 파일 3단계 파이프라인 실행 |
| `anim(id, barId, dur, cb)` | rAF 기반 프로그레스 바 애니메이션 |
| `mark(id, phaseId, cls)` | 파이프라인 단계 상태 변경 |
| `buildEv(name, isImg, isAu)` | 증거 패키지 텍스트 생성 (시뮬레이션) |
| `doNorm(text)` | DICT 기반 자연어 정규화 |
| `getFound(name)` | 파일명에서 정규화된 용어 추출 |
| `classify(text, fname)` | Rule+스코어 기반 Top 3 분류 추천 |
| `checkVer(fname)` | 버전 패턴 감지 → 경고 메시지 반환 |
| `buildSum(name, folder, found)` | AI 요약 텍스트 생성 |
| `showRes(...)` | 분류 결과 카드 UI 렌더링 |
| `selRec(id, idx)` | 추천 항목 선택 토글 |
| `saveDoc(id, fname, em, summary)` | DB 저장 + 카드 제거 |
| `updFolders()` | 사이드바 폴더 뱃지 숫자 갱신 |
| `updStats()` | KPI 지표 4개 갱신 |
| `goTab(id)` | 메인 탭 전환 |
| `setNav(el)` | 사이드바 활성 항목 설정 |
| `setNavByIdx(i)` | 인덱스로 사이드바 활성 항목 설정 |
| `goFolder(i)` | 폴더 클릭 → 보관함으로 이동 |
| `subTab(id, el)` | 서브 탭 전환 (업로드 탭 내부) |
| `toast(msg, type)` | 토스트 알림 표시 |
| `renderDocs()` | 문서 보관함 테이블 렌더링 |
| `showPop(e, idx, filt)` | 팝업 뷰어 표시 |
| `hidePop()` | 팝업 뷰어 숨기기 |
| `delDoc(idx, filt)` | 문서 삭제 |
| `doSearch()` | 자연어 검색 실행 |
| `qSearch(q)` | 추천 검색어 칩 클릭 검색 |
| `renderDict()` | 용어 사전 테이블 렌더링 |
| `addDict()` | 사용자 정의 용어 추가 |
| `renderArch()` | 아카이브 목록 렌더링 |
| `archOne(i)` | 개별 문서 아카이브 이동 |
| `archAll()` | 전체 문서 아카이브 이동 |
