# AI_HELPear - 기술 문서

## 📐 아키텍처 개요

### 파일 구조
```
AI_HELPear/
├── index.html          # 메인 애플리케이션 (HTML + CSS + JS 통합)
├── README.md           # 과제 요구사항
├── FEATURES.md         # 기능 및 사용 가이드
├── TECHNICAL.md        # 이 파일 (기술 문서)
├── CLAUDE.md           # Claude Code 가이드
├── Prompt_record.md    # 개발 프롬프트 기록
├── 01_인터랙션분석_*.md       # 과제 산출물
├── 02_UI디자인방법론_*.md     # 과제 산출물
├── 03_프로젝트제안서_*.md     # 과제 산출물
├── 04_사용자연구계획_*.md     # 과제 산출물
├── 05_맥락질의_*.md           # 과제 산출물
└── 06_작업분석어피니티_*.md   # 과제 산출물
```

### 코드 구조
```
index.html
├── <head>
│   ├── Meta 태그 (viewport, charset)
│   ├── 제목: "AI_HELPear - 청각 약자용 멀티미디어 뷰어"
│   └── <style> (CSS: 3500+ 줄)
│
└── <body>
    ├── <header> (헤더)
    │
    ├── <main class="container">
    │   ├── 상태 메시지 (aria-live)
    │   ├── 주 콘텐츠 (video + waveform)
    │   ├── 알림 및 진동 섹션
    │   ├── 설정 패널
    │   └── 통계 섹션
    │
    └── <script> (JavaScript: 600+ 줄)
        ├── 상태 관리
        ├── UI 요소 참조
        ├── 샘플 오디오 이벤트
        ├── 유틸리티 함수
        ├── 캔버스 렌더링
        ├── 알림 처리
        ├── 진동 제어
        ├── 이벤트 리스너
        └── 초기화
```

---

## 🎨 CSS 설계

### CSS 변수 (CSS Custom Properties)
```css
:root {
    --primary-bg: #ffffff;        /* 주 배경 */
    --secondary-bg: #f5f5f5;      /* 부 배경 */
    --text-primary: #1a1a1a;      /* 주 텍스트 */
    --text-secondary: #4a4a4a;    /* 보조 텍스트 */
    --border-color: #e0e0e0;      /* 테두리 */
    --accent-warning: #d32f2f;    /* 경고 (빨강) */
    --accent-info: #1976d2;       /* 정보 (파랑) */
    --accent-success: #388e3c;    /* 성공 (초록) */
    --accent-notification: #f57c00; /* 알림 (주황) */
    --vibration-active: #6a1b9a;  /* 진동 활성 (보라) */
    --focus-color: #0d47a1;       /* 포커스 (진 파랑) */
}

@media (prefers-color-scheme: dark) {
    /* 다크 모드 색상 재정의 */
}
```

### 레이아웃
- **Grid**: 메인 콘텐츠 2열 레이아웃 (768px 이하에서 1열)
- **Flexbox**: 내부 요소 정렬
- **반응형**: @media 쿼리로 모바일/태블릿/데스크톱 대응

### 주요 스타일링 클래스

#### 버튼
```css
button {
    padding: 10px 16px;
    min-height: 44px;           /* 터치 타겟 최소 높이 */
    border-radius: 4px;
    font-weight: 600;
    cursor: pointer;
}

button:focus {
    outline: 3px solid var(--focus-color); /* WCAG 포커스 */
    outline-offset: 2px;
}
```

#### 알림 카드
```css
.alert-card {
    border-left: 4px solid;      /* 색상 구분 */
    animation: slideIn 0.3s ease; /* 입장 애니메이션 */
}

.alert-card.warning { border-left-color: var(--accent-warning); }
.alert-card.info { border-left-color: var(--accent-info); }
.alert-card.success { border-left-color: var(--accent-success); }
.alert-card.notification { border-left-color: var(--accent-notification); }
```

#### 진동 지시자
```css
.vibration-circle.active {
    animation: vibrate 0.4s ease infinite; /* 흔들림 애니메이션 */
    background-color: var(--vibration-active);
    box-shadow: 0 0 20px rgba(106, 27, 154, 0.5);
}
```

---

## 🔧 JavaScript 아키텍처

### 1. 상태 관리 (State Object)
```javascript
const state = {
    isPlaying: false,           // 재생 중 여부
    currentTime: 0,             // 현재 시간 (초)
    duration: 180,              // 총 길이 (3분)
    enableVibration: true,      // 진동 활성화
    enableVisualFeedback: true, // 시각 피드백 활성화
    vibrationIntensity: 70,     // 진동 강도 (%)
    audioSensitivity: 70,       // 오디오 감도 (%)
    alerts: [],                 // 알림 히스토리
    totalAlertsDetected: 0,     // 감지된 총 알림 수
    responseTimes: [],          // 반응 시간 배열
};
```

### 2. UI 참조 (UI Object)
모든 DOM 요소를 미리 선택하여 저장:
```javascript
const UI = {
    playBtn: document.getElementById('playBtn'),
    waveformCanvas: document.getElementById('waveformCanvas'),
    alertCards: document.getElementById('alertCards'),
    vibrationCircle: document.getElementById('vibrationCircle'),
    // ... 기타 요소들
};
```

**이유**: 
- 성능 향상 (매번 querySelector 호출 불필요)
- 코드 간결성
- 유지보수 용이

### 3. 샘플 오디오 데이터
```javascript
const audioEvents = [
    { time: 5, type: 'warning', label: '⚠️ 경고음', description: '중요한 공지사항' },
    { time: 12, type: 'info', label: 'ℹ️ 안내음', description: '시스템 알림' },
    // ... 총 10개 이벤트
];
```

**구조:**
- `time`: 재생 위치 (초)
- `type`: 알림 유형 (warning/info/notification/success)
- `label`: 화면 표시 텍스트
- `description`: 알림 설명

### 4. 핵심 함수

#### 4.1 시간 포맷팅
```javascript
function formatTime(seconds) {
    const mins = Math.floor(seconds / 60);
    const secs = Math.floor(seconds % 60);
    return `${mins}:${secs.toString().padStart(2, '0')}`;
}
// formatTime(125) → "2:05"
```

#### 4.2 파형 그리기 (Canvas)
```javascript
function drawWaveform(progress) {
    const canvas = UI.waveformCanvas;
    const ctx = canvas.getContext('2d');
    
    // 캔버스 크기 설정
    canvas.width = canvas.offsetWidth;
    canvas.height = canvas.offsetHeight;
    
    // 사인파 그리기
    for (let i = 0; i < barCount; i++) {
        const angle = (i / barCount + progress) * Math.PI * 4;
        const height = Math.sin(angle) * (centerY * 0.7);
        const y = centerY + height;
        // ... 직선 그리기
    }
    
    // 현재 위치 표시 (빨간 수직선)
    ctx.strokeStyle = '--accent-warning';
    ctx.moveTo(progressX, 0);
    ctx.lineTo(progressX, canvas.height);
    ctx.stroke();
}
```

**원리:**
1. Canvas 요소 크기를 컨테이너 크기에 맞춤
2. 사인파를 80개 바로 나누어 계산
3. 각 바마다 높이를 계산하고 선으로 연결
4. 현재 진행 위치에 빨간 수직선 그림

#### 4.3 알림 감지 및 표시
```javascript
function checkAndDisplayAlerts() {
    const upcomingEvents = audioEvents.filter(
        event => event.time >= state.currentTime && event.time < state.currentTime + 1
    );
    
    upcomingEvents.forEach(event => {
        // 알림 카드 DOM 생성
        const alertCard = document.createElement('div');
        alertCard.className = `alert-card ${event.type}`;
        alertCard.innerHTML = `
            <div class="alert-icon">${event.label.split(' ')[0]}</div>
            <div class="alert-content">
                <div class="alert-title">${event.label}</div>
                <div class="alert-time">${formatTime(event.time)} - ${event.description}</div>
            </div>
        `;
        
        // DOM에 추가
        UI.alertCards.insertBefore(alertCard, UI.alertCards.firstChild);
        
        // 최대 10개 제한
        while (UI.alertCards.children.length > 10) {
            UI.alertCards.removeChild(UI.alertCards.lastChild);
        }
        
        // 통계 업데이트
        state.totalAlertsDetected++;
        
        // 진동 트리거
        if (state.enableVibration) {
            triggerVibration(event.type);
        }
    });
}
```

**로직:**
1. 현재 시간으로부터 1초 이내 이벤트 필터링
2. 각 이벤트마다 DOM 카드 생성 (innerHTML 사용)
3. 카드를 목록 최상단에 삽입
4. 최대 10개 유지 (오래된 항목 제거)
5. 통계 및 피드백 업데이트

#### 4.4 진동 피드백
```javascript
function triggerVibration(type) {
    const patterns = {
        warning: { pattern: 'short-long-short', duration: 400, intensity: 80 },
        info: { pattern: 'short-short', duration: 300, intensity: 60 },
        notification: { pattern: 'long', duration: 500, intensity: 70 },
        success: { pattern: 'short-short-short', duration: 350, intensity: 80 },
    };
    
    const pattern = patterns[type];
    
    // UI 업데이트 (즉시 피드백)
    UI.vibrationCircle.classList.add('active');
    UI.vibrationStatus.textContent = '진동 중!';
    
    // Vibration API 호출 (모바일)
    if ('vibrate' in navigator) {
        if (pattern.pattern === 'short-long-short') {
            navigator.vibrate([100, 200, 100]);
        } else if (pattern.pattern === 'short-short') {
            navigator.vibrate([100, 100, 100]);
        }
        // ... 기타 패턴
    }
    
    // 진동 종료
    setTimeout(() => {
        UI.vibrationCircle.classList.remove('active');
        UI.vibrationStatus.textContent = '대기 중';
    }, pattern.duration);
}
```

**Vibration API:**
- `navigator.vibrate([100, 200, 100])`: 100ms 진동, 200ms 대기, 100ms 진동
- 배열의 짝수 인덱스 = 진동 시간, 홀수 인덱스 = 대기 시간
- 모바일 기기에서만 동작 (데스크톱에서는 무시)

#### 4.5 재생 시뮬레이션 루프
```javascript
function updatePlayback() {
    if (!state.isPlaying) return;
    
    state.currentTime += 0.016;  // ~60fps (1000ms / 60 ≈ 16ms)
    
    if (state.currentTime >= state.duration) {
        state.isPlaying = false;
        // ... 종료 처리
    }
    
    // UI 업데이트
    UI.progressBar.value = (state.currentTime / state.duration) * 100;
    UI.currentTimeEl.textContent = formatTime(state.currentTime);
    
    // 파형 그리기
    if (state.enableVisualFeedback) {
        drawWaveform(state.currentTime / state.duration);
    }
    
    // 알림 확인
    checkAndDisplayAlerts();
    
    // 다음 프레임 스케줄
    requestAnimationFrame(updatePlayback);
}
```

**requestAnimationFrame 사용:**
- 브라우저의 최적화된 프레임 속도로 재생
- CPU 사용률 최소화
- 60fps 유지 (0.016초 = 1000 / 60)

### 5. 이벤트 리스너

#### 5.1 재생/일시정지 버튼
```javascript
UI.playBtn.addEventListener('click', () => {
    state.isPlaying = !state.isPlaying;
    UI.playBtn.textContent = state.isPlaying ? '⏸ 일시정지' : '▶ 재생';
    
    if (state.isPlaying) {
        updatePlayback();  // 재생 루프 시작
        showMessage('재생 시작');
    } else {
        showMessage('일시정지');
    }
});
```

#### 5.2 진행 바
```javascript
UI.progressBar.addEventListener('input', (e) => {
    state.currentTime = (e.target.value / 100) * state.duration;
    UI.currentTimeEl.textContent = formatTime(state.currentTime);
    if (state.enableVisualFeedback) {
        drawWaveform(state.currentTime / state.duration);
    }
});
```

#### 5.3 설정 변경
```javascript
UI.enableVibration.addEventListener('change', (e) => {
    state.enableVibration = e.target.checked;
    showMessage(`진동 피드백 ${state.enableVibration ? '활성화' : '비활성화'}`);
});

UI.vibrationIntensity.addEventListener('input', (e) => {
    state.vibrationIntensity = parseInt(e.target.value);
    showMessage(`진동 강도: ${state.vibrationIntensity}%`);
});
```

---

## 🎯 디자인 결정

### 1. 통합 HTML 파일 선택
**이유:**
- 배포 간편 (단일 파일)
- 의존성 제거 (외부 라이브러리 불필요)
- 성능 최적화 (HTTP 요청 1개)
- 프로토타이핑에 적합

### 2. Canvas 파형 렌더링
**대안과 비교:**

| 방법 | 장점 | 단점 |
|-----|------|------|
| **Canvas** | 고성능, 부드러운 애니메이션 | 복잡한 계산 필요 |
| SVG | 스케일 가능, 벡터 기반 | 성능 저하 (많은 요소) |
| CSS | 간단함 | 정밀한 파형 표현 어려움 |
| HTML5 Audio API | 실제 오디오 분석 | 프로토타입 범위 초과 |

→ **Canvas 선택**: 60fps 부드러운 애니메이션 필요

### 3. 샘플 데이터 사용
**이유:**
- 실제 오디오 처리 알고리즘 없음 (프로토타입 범위 외)
- 웹 Audio API 복잡도 높음
- 사전 정의된 이벤트로도 기능 시연 충분

**향후 개선:**
```javascript
// 실제 오디오 분석 (Web Audio API)
const audioContext = new (window.AudioContext || window.webkitAudioContext)();
const analyser = audioContext.createAnalyser();
analyser.fftSize = 256;
const dataArray = new Uint8Array(analyser.frequencyBinCount);
analyser.getByteFrequencyData(dataArray);
```

### 4. CSS 변수 사용
**이유:**
- 다크 모드 전환 간편
- 색상 일관성 유지
- 향후 테마 추가 용이

---

## 📱 반응형 디자인

### 중단점 (Breakpoints)
```css
@media (max-width: 768px) {
    .main-content {
        grid-template-columns: 1fr;  /* 1열로 변경 */
    }
    
    .playback-controls {
        flex-direction: column;       /* 세로 정렬 */
    }
}
```

### 모바일 최적화
- 터치 타겟 최소 44px × 44px
- 단순한 2열 → 1열 레이아웃
- 터치 친화적 UI 요소

---

## ♿ 접근성 구현

### 1. ARIA 속성
```html
<!-- 상태 메시지 (스크린 리더가 자동 공지) -->
<div id="statusMessage" role="status" aria-live="polite" aria-atomic="true"></div>

<!-- 알림 목록 (동적 콘텐츠) -->
<div id="alertCards" role="log" aria-live="polite" aria-label="감지된 오디오 이벤트"></div>

<!-- 버튼 라벨 -->
<button id="playBtn" aria-label="재생 시작">▶ 재생</button>
```

### 2. 포커스 관리
```css
button:focus {
    outline: 3px solid var(--focus-color);  /* 명확한 포커스 표시 */
    outline-offset: 2px;
}

input[type="range"]:focus-visible {
    outline: 3px solid var(--focus-color);
    outline-offset: 2px;
}
```

### 3. 색상 대비
```
배경 #ffffff (L=100%) + 텍스트 #1a1a1a (L=6%)
대비도 = (100 + 0.05) / (6 + 0.05) ≈ 16.4:1 ✅ WCAG AAA
```

### 4. 다크 모드
```css
@media (prefers-color-scheme: dark) {
    :root {
        --primary-bg: #121212;      /* 어두운 배경 */
        --text-primary: #ffffff;    /* 밝은 텍스트 */
    }
}
```

---

## 🧪 테스트 항목

### 기능 테스트
- [ ] 재생/일시정지/중지 버튼 동작
- [ ] 진행 바 마우스/터치 조작
- [ ] 오디오 이벤트 정시 감지
- [ ] 알림 카드 올바른 순서 표시
- [ ] 진동 피드백 활성/비활성화
- [ ] 설정 저장 및 적용

### 접근성 테스트
- [ ] 키보드 네비게이션 (Tab 키)
- [ ] 포커스 표시 명확함
- [ ] 스크린 리더 호환성 (NVDA/JAWS)
- [ ] 색상 대비 검증 (WebAIM)
- [ ] 다크 모드 전환

### 호환성 테스트
- [ ] 크롬/엣지/파이어폭스/사파리
- [ ] 모바일 (iOS/안드로이드)
- [ ] 태블릿 (iPad)
- [ ] 다양한 해상도

### 성능 테스트
- [ ] 60fps 유지 (DevTools)
- [ ] CPU 사용률 < 20%
- [ ] 메모리 누수 없음

---

## 🔍 코드 최적화 팁

### 1. 상태 객체 활용
```javascript
// ❌ 나쁜 예
let isPlaying, currentTime, duration;
let enableVibration, enableVisualFeedback;
let vibrationIntensity, audioSensitivity;

// ✅ 좋은 예
const state = {
    isPlaying: false,
    currentTime: 0,
    // ... 모든 상태 한 곳에
};
```

### 2. DOM 참조 캐싱
```javascript
// ❌ 나쁜 예 (매번 선택)
document.getElementById('playBtn').addEventListener('click', () => {});

// ✅ 좋은 예 (한 번 저장)
const UI = { playBtn: document.getElementById('playBtn') };
UI.playBtn.addEventListener('click', () => {});
```

### 3. requestAnimationFrame 사용
```javascript
// ❌ 나쁜 예
setInterval(updatePlayback, 16);  // 부정확한 타이밍

// ✅ 좋은 예
requestAnimationFrame(updatePlayback);  // 브라우저 최적화
```

---

## 🚀 배포

### 로컬 실행
```bash
# 방법 1: Python 서버
cd AI_HELPear
python -m http.server 8000

# 방법 2: Node.js (http-server)
npx http-server .

# 방법 3: 브라우저에서 직접 열기
file:///path/to/AI_HELPear/index.html
```

### 클라우드 배포
- GitHub Pages
- Netlify
- Vercel
- AWS S3 + CloudFront

---

## 📚 참고 자료

### Web APIs
- [Canvas API](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API)
- [Vibration API](https://developer.mozilla.org/en-US/docs/Web/API/Vibration_API)
- [ARIA Authoring Practices](https://www.w3.org/WAI/ARIA/apg/)
- [Web Audio API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API)

### 접근성
- [WCAG 2.1](https://www.w3.org/WAI/WCAG21/quickref/)
- [A11y Project](https://www.a11yproject.com/)
- [Color Contrast Checker](https://webaim.org/resources/contrastchecker/)

---

## 💡 알려진 제한사항

1. **샘플 데이터 기반**: 실제 오디오 처리 없음
2. **진동**: 모바일 기기에서만 동작
3. **파형**: 사인파 시뮬레이션 (실제 오디오 주파수 분석 아님)
4. **이벤트**: 사전 정의된 10개만 포함

---

## 🔮 향후 개선 사항

### 우선순위 높음
- 실제 오디오 파일 업로드 기능
- Web Audio API 기반 실시간 파형 분석
- 사용자 맞춤 진동 패턴 정의

### 우선순위 중간
- 클라우드 저장소 연동
- 다중 언어 지원
- 접근성 감사 (전문 기관)

### 우선순위 낮음
- 고급 시각화 (스펙트로그램)
- 커뮤니티 피드백 시스템
- AI 기반 음성 인식

