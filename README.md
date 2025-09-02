# Google Login React App

Google OAuth 2.0을 활용한 간편한 로그인 시스템을 제공하는 React 기반 웹 애플리케이션입니다.

## 🎯 주요 기능

### 🔐 Google 소셜 로그인

- **원클릭 로그인**: Google 계정으로 빠른 로그인
- **자동 세션 유지**: 브라우저 새로고침 시에도 로그인 상태 유지
- **안전한 로그아웃**: 완전한 세션 종료 및 Google 계정 연결 해제

### 👤 사용자 정보 관리

- **프로필 정보**: 이름, 이메일, 프로필 사진 표시
- **JWT 토큰**: 서버 인증용 Google JWT 토큰 제공
- **반응형 UI**: 데스크톱과 모바일 모두 최적화

### 🛡️ 보안 및 안정성

- **에러 바운더리**: 예상치 못한 오류 상황에 대한 안전장치
- **로딩 상태**: 사용자 친화적인 로딩 애니메이션
- **PropTypes 검증**: 컴포넌트 props 타입 안전성 보장

## 🚀 설치 및 실행

### 설치 방법

**의존성 설치**

```bash
npm install
# 또는
yarn install
```

**개발 서버 실행**

```bash
npm start
# 또는
yarn start
```

**브라우저에서 접속**

```
http://localhost:3000
```

## 🔧 Google OAuth 설정

### 1단계: Google Cloud Console 설정

1. **Google Cloud Console 접속**

- [Google Cloud Console](https://console.cloud.google.com/) 방문
- 프로젝트 생성 또는 기존 프로젝트 선택

2. **OAuth 2.0 클라이언트 ID 생성**

- "API 및 서비스" > "사용자 인증 정보" 이동
- "+ 사용자 인증 정보 만들기" > "OAuth 클라이언트 ID" 선택
- 애플리케이션 유형: "웹 애플리케이션" 선택

3. **승인된 JavaScript 출처 추가**
   ```
   개발 환경: http://localhost:3000
   운영 환경: https://yourdomain.com
   ```

4. **승인된 리디렉션 URI 추가**
   ```
   개발 환경: http://localhost:3000
   운영 환경: https://yourdomain.com
   ```

### 2단계: 코드에서 Client ID 설정

**src/utils/googleAuth.jsx 파일 수정**

```javascript
window.google.accounts.id.initialize({
  // 여기에 발급받은 Google OAuth Client ID 입력
  client_id: 'YOUR_GOOGLE_CLIENT_ID_HERE.apps.googleusercontent.com', callback: onResponse,
});
```

## 🏗️ 프로젝트 구조

```
src/
├── App.js                           # 메인 애플리케이션
├── App.module.css                   # 메인 스타일
├── components/                      # React 컴포넌트
│   ├── ErrorBoundary.jsx            # 에러 바운더리
│   ├── ErrorBoundary.module.css
│   ├── GoogleLoginButton.jsx        # 구글 로그인 버튼
│   ├── GoogleLoginButton.module.css
│   ├── LoadingSpinner.jsx          # 로딩 스피너
│   ├── LoadingSpinner.module.css
│   ├── LogoutButton.jsx            # 로그아웃 버튼
│   ├── LogoutButton.module.css
│   ├── UserInfo.jsx                # 사용자 정보 표시
│   └── UserInfo.module.css
├── pages/                          # 페이지 컴포넌트
│   ├── LoginPage.jsx               # 로그인 페이지
│   ├── LoginPage.module.css
│   ├── DashboardPage.jsx           # 대시보드 페이지
│   └── DashboardPage.module.css
├── router/                         # 라우터 관리
│   ├── AppRouter.jsx               # 메인 라우터
│   └── AppRouter.module.css
└── utils/                          # 유틸리티 함수
    ├── authContext.jsx             # 인증 컨텍스트
    └── googleAuth.jsx              # Google 인증 로직
```

## 🖥️ 서버 연동 방법

### 서버로 전달해야 할 데이터

Google 로그인 후 서버에 다음 정보를 전달해야 합니다:

**🔑 JWT 토큰 (필수)**

```javascript
{
  "token"
:
  "eyJhbGciOiJSUzI1NiIsImtpZCI6IjdkYzc5ZGU3M..." // Google JWT 토큰
}
```

## 👤 구글 반환값 예시

```javascript
{
  "token"
:
  "eyJhbGciOiJSUzI1NiIsImtpZCI6IjdkYzc5ZGU3M...", "userInfo"
:
  {
    "id"
  :
    "123456789012345678901", "email"
  :
    "user@gmail.com", "name"
  :
    "홍길동", "picture"
  :
    "https://lh3.googleusercontent.com/..."
  }
}
```

## 서버에서 처리해야 할 작업

**1. 토큰 검증**

- Google에서 제공하는 라이브러리를 사용하여 JWT 토큰 검증
- 토큰의 유효성, 만료시간, audience 확인

**2. 사용자 정보 추출**

- 검증된 토큰에서 사용자 ID, 이메일, 이름 등 추출
- 데이터베이스에 사용자 정보 저장 또는 업데이트

**3. 세션/인증 관리**

- 자체 인증 토큰 생성 (선택사항)
- 세션 생성 또는 기존 세션과 연결

### 전달 이유

**보안**: 클라이언트에서는 토큰을 표시용으로만 파싱하며, 실제 검증은 서버에서만 가능

**인증**: 서버에서 Google에 직접 토큰을 검증하여 위조된 토큰 차단

**사용자 관리**: 검증된 사용자 정보를 데이터베이스에 저장하여 서비스 연동

