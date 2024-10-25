# Discord AI Bot Platform 🤖

Discord向けAIボットプラットフォーム - マイクロサービスベースのスケーラブルなボット管理システム

## System Architecture 📊

### 全体構成

```mermaid
graph TD
    A[Discord Bot Management] --> B[Message Management]
    A --> C[Dify]
    C --> B
    C --> D[AI Bot Profile Management]
```

### 処理フロー詳細

#### 1. 返信フロー

```mermaid
sequenceDiagram
    participant Discord
    participant discord_bot_management
    participant message_management
    participant dify
    
    Discord->>discord_bot_management: メッセージ受信
    discord_bot_management->>message_management: メッセージ保存
    
    alt メンションあり
        discord_bot_management->>dify: メッセージ情報送信
        dify-->>discord_bot_management: 返信内容
        discord_bot_management->>Discord: 返信送信
    end
```

#### 2. 返信内容生成フロー

```mermaid
sequenceDiagram
    participant dify
    participant message_management
    participant ai_bot_profile_management
    participant LLM
    
    note over dify: メッセージ情報受信（チャンネル・メッセージ内容）
    dify->>message_management: 過去メッセージ取得
    message_management-->>dify: 会話履歴
    dify->>ai_bot_profile_management: ボット設定取得
    ai_bot_profile_management-->>dify: 設定情報
    dify->>LLM: メッセージ生成要求
    LLM-->>dify: 生成結果
```

#### 3. 性格設定フロー

```mermaid
sequenceDiagram
    participant Discord
    participant discord_bot_management
    participant dify
    participant ai_bot_profile_management
    
    Discord->>discord_bot_management: 設定メッセージ
    discord_bot_management->>dify: メッセージ送信
    
    dify->>dify: 設定メッセージ判定
    
    alt 設定メッセージの場合
        dify->>ai_bot_profile_management: 設定保存
        ai_bot_profile_management-->>dify: 更新結果
    end
    
    dify-->>discord_bot_management: 処理結果
    discord_bot_management->>Discord: 結果送信
```

### Core Services

| Service | Description | Status |
|---------|-------------|---------|
| [discord_bot_management](https://github.com/FungiFur-Strikers/discord-bot-service) | Discord統合とメッセージルーティング | [![Status](https://img.shields.io/badge/status-active-success)]() |
| [ai_bot_profile_management](https://github.com/FungiFur-Strikers/bot-profile-manager) | ボットのパーソナリティと設定管理 | [![Status](https://img.shields.io/badge/status-active-success)]() |
| [message_management](https://github.com/FungiFur-Strikers/discord-message-service) | 会話履歴とメッセージストレージ | [![Status](https://img.shields.io/badge/status-active-success)]() |

---
