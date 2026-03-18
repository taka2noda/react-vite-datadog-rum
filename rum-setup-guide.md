# Datadog RUM セットアップガイド

Datadog MCP と OpenAI Codex を活用して、ブラウザ向けWebアプリに Datadog Real User Monitoring (RUM) を導入するガイドです。

---

## 参考リンク

| ドキュメント | URL |
|---|---|
| RUM Browser セットアップ | https://docs.datadoghq.com/real_user_monitoring/application_monitoring/browser/setup/ |
| RUM Browser クライアント設定 | https://docs.datadoghq.com/real_user_monitoring/application_monitoring/browser/setup/client/ |
| Session Replay | https://docs.datadoghq.com/session_replay/browser/ |
| Datadog MCP Server | https://docs.datadoghq.com/bits_ai/mcp_server/ |
| MCP Server セットアップ | https://docs.datadoghq.com/bits_ai/mcp_server/setup/ |

## 設定

### Datadog MCP を Codex に設定する

Datadog MCP を使うことで、Codex が RUM のセットアップ手順を自動的に案内・実装してくれます。

1. `~/.codex/config.toml` を編集し、以下を追加します：

```toml
[mcp_servers.datadog]
url = "<YOUR_DATADOG_MCP_ENDPOINT>"
```

> `<YOUR_DATADOG_MCP_ENDPOINT>` は Datadog サイトのリージョンに応じた URL を使用します。
> 公式ドキュメント: [Datadog MCP Server セットアップ](https://docs.datadoghq.com/bits_ai/mcp_server/setup/)

2. 認証を実行します：

```bash
codex mcp login datadog
```

ブラウザで OAuth フローが開始されます。ログイン後、認証情報は自動保存されます。

### Datadog RUM アプリケーションの作成

RUM を設定する前に、Datadog UI上でアプリケーションを作成し、**Application ID** と **Client Token** を取得します。

1. Datadog コンソールにログイン
2. **UX Monitoring > Real User Monitoring** に移動
3. **New Application** をクリック
4. アプリ名を入力し、プラットフォームに **Browser** を選択
5. 表示された `applicationId` と `clientToken` を控えておく

> 公式ドキュメント: [RUM Browser Setup](https://docs.datadoghq.com/real_user_monitoring/application_monitoring/browser/setup/)

---

### MCP を使った RUM セットアップの実行

Codex のチャットで以下のように入力するだけで、プロジェクトへの RUM 導入が自動化されます：

```
Datadog MCP Onboardingを活用して、このreact applicationにDatadogの設定を行ってください。
```

Datadog MCP の `browser_onboarding` ツールが以下を自動で行います：

- プロジェクト構成 (framework, bundler, 言語) の自動検出
- 必要パッケージのインストール
- `datadogRum.init()` の初期化コードの生成・挿入
- 動作確認手順の案内

途中で入力を求められるapplication keyに上のセクションで発行したDatadog UI上のRUMアプリケーションのapplication keyを指定します。

> 公式ドキュメント: [Datadog MCP Server](https://docs.datadoghq.com/bits_ai/mcp_server/)


### 動作確認

[RUM Performance Monitoring](https://app.datadoghq.com/rum/performance-monitoring) にアクセスし、セッションデータが受信されていることを確認します。


## 作業ログ

実際に手元の環境(claude code)でMCPを利用して設定を行った際の作業ログです。

/mcp
  ⎿  Authentication successful. Connected to datadog-onboarding.

❯ /mcp
  ⎿  Authentication successful. Connected to datadog-mcp.

❯ /mcp
  ⎿  MCP dialog dismissed

❯ Datadog MCP Onboardingを活用して、このreact applicationにDatadogの設定を行ってください。

⏺ mcp__datadog-onboarding__browser_onboarding を使ってセットアップします。

⏺ rumApplicationId が必須パラメータです。Datadog の RUM Application ID を教えていただけますか？

  Datadog コンソールの UX Monitoring > Real User Monitoring から RUM アプリケーションを作成・確認できます（例:
  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx）。

❯ xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

⏺ Searched for 1 pattern (ctrl+o to expand)

⏺ datadog-onboarding - browser_onboarding (MCP)(product: "rum", language: "javascript", framework: "react", frameworkVersion: "19.2.4", bundler:
                                               "vite", dependencyManagementType: "npm", hasEnvVariables: false, rumApplicationId:
                                               "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx", includeVerificationSteps: true, telemetry: {"intent":"Add
                                               Datadog RUM to a React + Vite project using npm"})
  ⎿  {"instructions":"\n# Pre-Instrumentation Instructions (Do not proceed until you have verified that these steps have been completed)\n\nBecau
     se you have called this tool, you have verified that the project does not already contain basic Datadog instrumentation setup. **If it does,
      you should stop right now and not attempt to instrument the project.**\n\n\n\n# Instructions\nYour responsibility is to follow the instruct
     … +153 lines (ctrl+o to expand)

⏺ Datadog MCP から指示を受け取りました。以下の手順で進めます：

  チェックリスト：
  - @datadog/browser-rum と @datadog/browser-rum-react をインストール
  - main.jsx に import と datadogRum.init() を追加
  - lint fix を実行
  - アプリを起動して動作確認

  このプロジェクトには react-router がないため、ルーター追跡の手順はスキップします。

  ---
  Step 1: パッケージのインストール

⏺ Bash(npm install @datadog/browser-rum @datadog/browser-rum-react)
  ⎿  added 5 packages, and audited 158 packages in 1s

     36 packages are looking for funding
     … +3 lines (ctrl+o to expand)

⏺ Step 2: main.jsx に RUM 初期化コードを追加

⏺ Update(src/main.jsx)
  ⎿  Added 1 line
      1 +import { datadogRum } from '@datadog/browser-rum';
      2  import { StrictMode } from 'react'
      3  import { createRoot } from 'react-dom/client'
      4  import './index.css'

⏺ Update(src/main.jsx)
  ⎿  Added 17 lines
       4  import './index.css'
       5  import App from './App.jsx'
       6
       7 +datadogRum.init({
       8 +    applicationId: 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx',
       9 +    clientToken: 'pubxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx',
      10 +    site: 'datadoghq.com',
      11 +    service: 'react-rum-sample',
      12 +    env: 'production',
      13 +    // Specify a version number to identify the deployed version of your application in Datadog
      14 +    // version: '1.0.0',
      15 +    version: '0.0.0',
      16 +    sessionSampleRate: 100,
      17 +    sessionReplaySampleRate: 20,
      18 +    trackUserInteractions: true,
      19 +    trackResources: true,
      20 +    trackLongTasks: true,
      21 +    defaultPrivacyLevel: 'mask-user-input',
      22 +});
      23 +
      24  createRoot(document.getElementById('root')).render(
      25    <StrictMode>
      26      <App />

⏺ Step 3: lint fix を実行

⏺ Bash(npm run lint 2>&1 | head -20)
  ⎿  > react-rum-sample@0.0.0 lint
     > eslint .

⏺ lint エラーなしです。最終的な main.jsx を確認します：

⏺ Read 1 file (ctrl+o to expand)

⏺ Step 4: アプリを起動

⏺ Bash(npm run dev)
  ⎿  Running in the background (↓ to manage)

⏺ Bash(sleep 3 && cat
      /private/tmp/claude-502/-Users-takaaki-tsunoda-Documents-Work-git-react-rum-sample/ff6ea479-3874-4aa8-889c-d5558ad19781/tasks/b20r0nitx.output)
  ⎿  > react-rum-sample@0.0.0 dev
     > vite
     … +5 lines (ctrl+o to expand)

⏺ アプリが http://localhost:5173/ で起動しました。

  ---
  チェックリスト（完了）：
  - @datadog/browser-rum と @datadog/browser-rum-react をインストール
  - main.jsx に import と datadogRum.init() を追加
  - lint エラーなし
  - アプリ起動確認

  ---
  次の確認手順：

  1. ブラウザで http://localhost:5173/ を開き、Developer Tools の Network タブで datadoghq.com へのリクエストが送信されていることを確認してください。
  2. Datadog RUM Dashboard でデータを確認できます：
    - RUM Performance Monitoring
    - taka2-react-test アプリ管理
