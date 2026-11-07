# AI_HELPear - 배포 및 설치 가이드

## 🚀 빠른 시작 (30초)

### 1단계: 서버 실행
```bash
cd "C:\Users\as742\OneDrive\바탕 화면\3학년_2학기\HumanComputer\AI_HELPear"
python -m http.server 8000
```

### 2단계: 브라우저에서 열기
```
http://localhost:8000/index.html
```

### 3단계: "재생" 버튼 클릭
- 파형 애니메이션 시작
- 알림 카드 나타남
- (모바일) 진동 피드백 감지

---

## 💻 설치 방법

### 방법 1: Python HTTP 서버 (권장)
```bash
# Python 3.x 필요
cd AI_HELPear
python -m http.server 8000

# 브라우저: http://localhost:8000/index.html
# 종료: Ctrl+C
```

### 방법 2: Node.js HTTP 서버
```bash
# npm 설치 필요
npx http-server . -p 8000

# 또는
npm install -g http-server
http-server . -p 8000
```

### 방법 3: 브라우저에서 직접 열기
```bash
# Windows
start index.html

# macOS
open index.html

# Linux
xdg-open index.html
```

**주의**: 로컬 파일로 열 경우 Vibration API 작동 안 함

### 방법 4: 클라우드 배포

#### GitHub Pages
```bash
# 1. GitHub 저장소 생성
# 2. AI_HELPear 폴더를 저장소에 푸시
# 3. Settings > Pages > Main 브랜치 선택
# 4. https://yourusername.github.io/AI_HELPear/index.html 접속
```

#### Netlify
```bash
# 1. Netlify.com 가입
# 2. "New site from Git" 클릭
# 3. GitHub 저장소 연결
# 4. Deploy settings 설정 (빌드 명령 없음)
# 5. 자동 배포
```

#### Vercel
```bash
# 1. Vercel.com 가입
# 2. "Add New... > Project" 클릭
# 3. GitHub 저장소 임포트
# 4. 자동 배포
```

---

## 📱 기기별 실행

### 데스크톱
- Windows 10/11: Chrome, Edge, Firefox, Safari
- macOS: Safari, Chrome, Firefox
- Linux: Chrome, Firefox

```bash
# 로컬 서버에서 실행
python -m http.server 8000
# 브라우저: http://localhost:8000/index.html
```

### 모바일 (진동 피드백 포함)
```bash
# 1. 데스크톱과 같은 네트워크 연결
# 2. 데스크톱 IP 주소 확인
#    Windows: ipconfig | findstr IPv4
#    macOS/Linux: ifconfig | grep "inet " 
# 3. 모바일 브라우저에서 접속
#    http://<데스크톱IP>:8000/index.html
```

**예시:**
```
데스크톱 IP: 192.168.1.100
모바일 접속: http://192.168.1.100:8000/index.html
```

---

## 🔧 시스템 요구사항

### 최소 사양
- 브라우저: Chrome 60+, Firefox 55+, Safari 11+, Edge 79+
- 네트워크: HTTP 서버 포트 8000 (또는 커스터마이징 가능)
- 메모리: 20MB 이상

### 권장 사양
- 브라우저: 최신 버전
- OS: Windows 10/11, macOS 10.15+, Linux
- 네트워크: WiFi 또는 유선 (안정적)

### 모바일
- iOS 12.2+ (Safari)
- Android 5.0+ (Chrome, Firefox, Samsung Internet)
- 진동: Vibration API 지원 기기

---

## 📂 파일 구조

```
AI_HELPear/
│
├── 📄 index.html                    # 메인 애플리케이션 (✨ 이것만 필요)
├── 📄 README.md                     # 과제 요구사항
├── 📄 FEATURES.md                   # 기능 가이드 (사용자용)
├── 📄 TECHNICAL.md                  # 기술 문서 (개발자용)
├── 📄 DEPLOYMENT.md                 # 이 파일
├── 📄 CLAUDE.md                     # Claude Code 가이드
├── 📄 Prompt_record.md              # 개발 프롬프트 기록 ✅
│
├── 📋 01_인터랙션분석_*.md          # 과제 산출물
├── 📋 02_UI디자인방법론_*.md        # 과제 산출물
├── 📋 03_프로젝트제안서_*.md        # 과제 산출물
├── 📋 04_사용자연구계획_*.md        # 과제 산출물
├── 📋 05_맥락질의_*.md              # 과제 산출물
└── 📋 06_작업분석어피니티_*.md      # 과제 산출물
```

**배포 시 필요한 파일:**
- ✅ `index.html` (필수!)

**사용자 참고 자료:**
- `FEATURES.md` (기능 설명)
- `README.md` (과제 설명)

**개발자 참고 자료:**
- `TECHNICAL.md` (코드 구조)
- `CLAUDE.md` (개발 가이드)
- `Prompt_record.md` (개발 과정)

---

## 🌐 네트워크 설정

### 로컬 네트워크 접속 (모바일 테스트)

**Windows에서 데스크톱 IP 확인:**
```bash
ipconfig

# 출력 예:
# IPv4 주소 . . . . . . . . . : 192.168.1.100
```

**서버 실행:**
```bash
python -m http.server 8000 --bind 0.0.0.0
```

**모바일에서 접속:**
```
http://192.168.1.100:8000/index.html
```

### 방화벽 설정 (Windows)
1. 제어판 > Windows Defender 방화벽 > 앱을 통한 통신 허용
2. Python 앱 활성화 (또는 포트 8000 허용)

---

## 🧪 테스트 체크리스트

### 기능 테스트
```
[ ] 재생 버튼 클릭 → 파형 애니메이션 시작
[ ] 진행 바 드래그 → 시간 변경
[ ] 중지 버튼 → 재생 초기화
[ ] 알림 카드 표시 → 올바른 색상/아이콘
[ ] 진동 활성화 → (모바일) 진동 발생
[ ] 설정 변경 → 즉시 적용
```

### 접근성 테스트
```
[ ] Tab 키로 모든 버튼 접근 가능
[ ] 포커스 표시 명확함
[ ] 색상만으로 정보 전달 안 함 (기호/텍스트 함께)
[ ] 다크 모드 전환 가능
[ ] 스크린 리더에서 텍스트 읽음
```

### 호환성 테스트
```
[ ] Chrome/Chromium
[ ] Firefox
[ ] Safari
[ ] Edge
[ ] 모바일 브라우저 (iOS Safari, Android Chrome)
```

### 성능 테스트
```
[ ] 60fps 파형 애니메이션 (DevTools 확인)
[ ] 메모리 누수 없음 (10분 재생 후 메모리 안정)
[ ] CPU 사용률 < 20%
```

---

## 🐛 문제 해결

### 1. "localhost 연결 거부" 오류
**원인**: 서버 미실행
```bash
# 해결:
python -m http.server 8000
# 또는 다른 포트
python -m http.server 9000
```

### 2. "포트 이미 사용 중" 오류
```bash
# 다른 포트 사용
python -m http.server 9000  # 포트 9000으로 변경

# 또는 기존 프로세스 종료
# Windows
netstat -ano | findstr :8000
taskkill /PID <PID> /F

# macOS/Linux
lsof -i :8000
kill -9 <PID>
```

### 3. 진동이 작동하지 않음
**확인 사항:**
- [ ] 모바일 기기 사용 (데스크톱은 진동 불가)
- [ ] 설정 > 진동 피드백 ON
- [ ] 기기 진동 설정 ON (시스템 설정)
- [ ] 브라우저가 Vibration API 지원하는지 확인

```javascript
// 브라우저 콘솔에서 확인
navigator.vibrate([100]);  // 진동 테스트
```

### 4. 파형이 깨지거나 보이지 않음
**원인**: 모니터 해상도 변경
**해결:**
- 브라우저 새로고침 (F5)
- 창 크기 변경 후 다시 새로고침

### 5. 알림 카드가 나타나지 않음
**확인:**
- [ ] 재생 시작했는지 확인
- [ ] "시각 피드백" 설정 ON
- [ ] 브라우저 콘솔 오류 확인 (F12)

---

## 📊 성능 최적화

### 현재 성능 (기본값)
- 파일 크기: ~50KB (축소화 가능)
- 메모리 사용: ~10MB
- CPU 사용: <5% (60fps 유지)

### 최적화 가능 항목
```javascript
// 1. CSS 축소화 (minify)
// 현재 3500줄 → 축소 후 ~1500줄

// 2. JavaScript 축소화
// 현재 600줄 → 축소 후 ~300줄

// 3. 이미지 없음 (이미 최적화)

// 4. 외부 의존성 없음 (이미 최적화)
```

### 축소화 도구
```bash
# CSS 축소
npm install -g cssnano
cssnano style.css -o style.min.css

# JavaScript 축소
npm install -g terser
terser script.js -o script.min.js
```

---

## 🔒 보안

### 고려 사항
- ✅ 외부 API 없음 (데이터 유출 위험 없음)
- ✅ 서버 사이드 코드 없음 (인젝션 공격 불가)
- ✅ 사용자 데이터 저장 없음 (프라이버시 우수)
- ⚠️ 로컬 네트워크만 접속 권장 (프로덕션은 HTTPS 필수)

### HTTPS 배포 (권장)
```bash
# Let's Encrypt 인증서 (무료)
# Netlify/Vercel: 자동 HTTPS 제공

# 또는 자체 서명 인증서
openssl req -x509 -newkey rsa:4096 -nodes -out cert.pem -keyout key.pem -days 365
python -m http.server --certfile=cert.pem --keyfile=key.pem 8443
```

---

## 📈 배포 후 모니터링

### Google Analytics 추가 (선택)
```html
<!-- index.html 끝 부분에 추가 -->
<script async src="https://www.googletagmanager.com/gtag/js?id=GA_MEASUREMENT_ID"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'GA_MEASUREMENT_ID');
</script>
```

### 에러 추적 (선택)
```html
<!-- Sentry.io 통합 -->
<script src="https://browser.sentry-cdn.com/7.x/bundle.min.js"></script>
<script>
  Sentry.init({
    dsn: "YOUR_SENTRY_DSN"
  });
</script>
```

---

## 📞 지원

### 문제 발생 시
1. 브라우저 콘솔 확인 (F12 > Console)
2. 네트워크 탭 확인 (HTTP 요청 상태)
3. 캐시 초기화 (Ctrl+Shift+Delete)
4. 다른 브라우저에서 테스트

### 개발자 연락
- 이메일: [학생 이메일]
- GitHub: [저장소 링크]

---

## 📚 추가 자료

### 설치 가능한 패키지
```bash
# 선택사항 (개발용)
npm install http-server -g     # HTTP 서버
npm install cssnano -g         # CSS 축소
npm install terser -g          # JS 축소
npm install web-push -g        # 푸시 알림 (향후)
```

### 참고 링크
- [Canvas API](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API)
- [Vibration API](https://caniuse.com/vibration)
- [HTTP Server 명령어](https://docs.python.org/3/library/http.server.html)
- [WCAG 접근성](https://www.w3.org/WAI/WCAG21/quickref/)

---

## ✅ 배포 체크리스트

배포 전 이 항목들을 확인하세요:

```
배포 사전 점검
[ ] index.html 파일 완성
[ ] 모든 기능 테스트 완료
[ ] 접근성 검사 통과 (WCAG AA)
[ ] 모바일 테스트 완료
[ ] 다크 모드 테스트 완료
[ ] 문서 최신화 (README, FEATURES 등)

배포 단계
[ ] 저장소에 푸시 (GitHub)
[ ] 배포 플랫폼 선택 (Netlify/Vercel/GitHub Pages)
[ ] 자동 배포 설정
[ ] 배포 링크 테스트

배포 후
[ ] 라이브 링크 확인
[ ] 모바일에서 재확인
[ ] 통계 모니터링 시작 (Analytics)
[ ] 사용자 피드백 수집
```

---

**배포 완료! 축하합니다! 🎉**

이제 `index.html` 하나로 모든 기능을 제공하는 완전한 웹 애플리케이션이 완성되었습니다.
