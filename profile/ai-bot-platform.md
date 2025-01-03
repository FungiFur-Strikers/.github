# Discord AI Bot Platform 🤖

Discord向けAIボットプラットフォーム - マイクロサービスベースのスケーラブルなボット管理システム

## システム概要

Discord AI Bot Platformは、AIを活用したボットをDiscord上で効率的に運用・管理するためのプラットフォームです。マイクロサービスアーキテクチャを採用し、柔軟なスケーリングと機能拡張を可能にしています。

## システムアーキテクチャ 📊

### コアサービス構成

```mermaid
graph TD
    subgraph "External Services"
        Discord[Discord]
        LLM[Language Model]
    end
    
    subgraph "Core Services"
        DBM[Discord Bot Management]
        MM[Message Management]
        DIFY[Dify]
        BPM[Bot Profile Management]
    end
    
    Discord <--> DBM
    DBM <--> MM
    DBM <--> DIFY
    DIFY <--> MM
    DIFY <--> BPM
    DIFY <--> LLM

    class Discord,LLM external
    class DBM,MM,DIFY,BPM core
```

### 統合処理フロー

```mermaid
sequenceDiagram
    participant D as Discord
    participant DBM as Discord Bot Management
    participant MM as Message Management
    participant DIFY as Dify
    participant BPM as Bot Profile Management
    participant LLM as Language Model

    %% メッセージ受信フロー
    D->>DBM: 1. メッセージ受信
    DBM->>MM: 2. メッセージ保存
    
    alt メンション/設定メッセージの場合
        DBM->>DIFY: 3. メッセージ処理依頼
        
        %% 会話履歴取得
        DIFY->>MM: 4. 会話履歴取得
        MM-->>DIFY: 5. 会話履歴

        %% ボット設定取得
        DIFY->>BPM: 6. ボット設定取得
        BPM-->>DIFY: 7. 設定情報

        alt 設定メッセージの場合
            DIFY->>BPM: 8a. 設定更新
            BPM-->>DIFY: 9a. 更新完了
        else 通常メッセージの場合
            DIFY->>LLM: 8b. 返信生成要求
            LLM-->>DIFY: 9b. 生成結果
        end

        DIFY-->>DBM: 10. 処理結果
        DBM->>D: 11. Discord返信
    end
```

### Core Services

| Service | Description | Status |
|---------|-------------|---------|
| [discord_bot_management](https://github.com/FungiFur-Strikers/discord-bot-service) | Discord統合とメッセージルーティング | [![Status](https://img.shields.io/badge/status-active-success)]() |
| [ai_bot_profile_management](https://github.com/FungiFur-Strikers/bot-profile-manager) | ボットのパーソナリティと設定管理 | [![Status](https://img.shields.io/badge/status-active-success)]() |
| [message_management](https://github.com/FungiFur-Strikers/discord-message-service) | 会話履歴とメッセージストレージ | [![Status](https://img.shields.io/badge/status-active-success)]() |

---
