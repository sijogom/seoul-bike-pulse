# SEOUL BIKE PULSE 프론트엔드

React·TypeScript·Vite 기반 웹 화면으로 API 연결 상태와 모델 로딩 상태를 표시합니다.

```powershell
npm ci
Copy-Item .env.example .env.local
npm run dev
```

기본 개발 주소는 `http://127.0.0.1:5173`이며 Vite 프록시는 `http://127.0.0.1:8000`의 API를 사용합니다. API 주소를 변경하면 `vite.config.ts`와 `VITE_API_BASE_URL`을 맞춥니다. `VITE_*`에는 비밀값을 넣지 않습니다.

```powershell
npm run lint
npm run build
```

화면은 `/api/v1/health` 응답을 사용합니다. 모델과 백엔드를 준비한 뒤 프론트엔드를 실행합니다.
