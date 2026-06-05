# 발전소 점검 PWA Wrapper

Option A 구현체입니다. 정적 호스팅 도메인에서 이 폴더를 배포하고, GAS `/exec` 웹앱을 iframe으로 띄워 홈화면 아이콘/PWA shell을 제공합니다.

## 사용 방법

1. GAS 앱을 배포해서 `/exec` URL을 확보합니다.
2. 이 `pwa-wrapper/` 폴더를 GitHub Pages, Vercel, Netlify, Cloudflare Pages 같은 정적 호스팅에 배포합니다.
3. 첫 실행 URL에 GAS URL을 넣습니다.

```text
https://<wrapper-domain>/pwa-wrapper/?gasUrl=https%3A%2F%2Fscript.google.com%2Fmacros%2Fs%2F<deployment-id>%2Fexec
```

래퍼는 URL을 브라우저 localStorage에 저장합니다. 이후 홈화면 아이콘에서 `start_url`로 열려도 저장된 GAS URL을 사용합니다.

## 보안 조건

GAS HTML은 iframe 안에서 실행될 때 `postMessage` handshake를 시도합니다. Apps Script live runtime은 Google의 nested iframe을 추가로 만들 수 있어 outer wrapper 메시지가 앱 frame까지 전달되지 않을 수 있습니다. 이 경우 앱은 `framed-allowed` 상태로 계속 열리고, 저장/전송 같은 주요 동작은 앱 내부 확인 gate로 완화합니다. 현재 명시 허용 origin은 다음과 같습니다.

- `https://haeminway1.github.io`
- local 개발용 `http://localhost:*`, `http://127.0.0.1:*`

다른 도메인에 배포할 경우 `src/frontend/index.html`의 `allowedOrigins`에 wrapper origin을 추가하고 GAS를 재배포해야 합니다.

## 검증

```bash
npm run test:pwa-wrapper
npm run test:mobile:local
npm run test:adaptive:local
```

live에서는 raw GAS `/exec` URL과 wrapper URL을 둘 다 테스트합니다.
