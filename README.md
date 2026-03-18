# React + Vite

This template provides a minimal setup to get React working in Vite with HMR and some ESLint rules.

Currently, two official plugins are available:

- [@vitejs/plugin-react](https://github.com/vitejs/vite-plugin-react/blob/main/packages/plugin-react) uses [Oxc](https://oxc.rs)
- [@vitejs/plugin-react-swc](https://github.com/vitejs/vite-plugin-react/blob/main/packages/plugin-react-swc) uses [SWC](https://swc.rs/)

## React Compiler

The React Compiler is not enabled on this template because of its impact on dev & build performances. To add it, see [this documentation](https://react.dev/learn/react-compiler/installation).

## Datadog Real User Monitoring (RUM)

このプロジェクトは [Datadog RUM](https://docs.datadoghq.com/real_user_monitoring/) を使用してフロントエンドのパフォーマンスとユーザー行動を計測しています。

### セットアップ内容

**インストールパッケージ:**

```bash
npm install @datadog/browser-rum @datadog/browser-rum-react
```

**環境変数 (`.env`):**

```
VITE_DD_APPLICATION_ID=<your-application-id>
VITE_DD_CLIENT_TOKEN=<your-client-token>
```

**初期化 (`src/main.jsx`):**

```js
import { datadogRum } from '@datadog/browser-rum';

datadogRum.init({
    applicationId: import.meta.env.VITE_DD_APPLICATION_ID,
    clientToken: import.meta.env.VITE_DD_CLIENT_TOKEN,
    site: 'datadoghq.com',
    service: 'react-rum-sample',
    env: 'production',
    version: '0.0.0',
    sessionSampleRate: 100,
    sessionReplaySampleRate: 20,
    trackUserInteractions: true,
    trackResources: true,
    trackLongTasks: true,
    defaultPrivacyLevel: 'mask-user-input',
});
```

### Datadog ダッシュボード

- [RUM Performance Monitoring](https://taka2.datadoghq.com/rum/performance-monitoring?query=@application.id:d41027b9-ce78-43e1-a288-a3965ea452f4)
- [アプリケーション管理 (taka2-react-test)](https://taka2.datadoghq.com/rum/application/d41027b9-ce78-43e1-a288-a3965ea452f4/manage)

## Expanding the ESLint configuration

If you are developing a production application, we recommend using TypeScript with type-aware lint rules enabled. Check out the [TS template](https://github.com/vitejs/vite/tree/main/packages/create-vite/template-react-ts) for information on how to integrate TypeScript and [`typescript-eslint`](https://typescript-eslint.io) in your project.
