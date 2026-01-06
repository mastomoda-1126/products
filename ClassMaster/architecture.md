# Architecture

このドキュメントは、構成分割とデータ依存の設計思想を可視化したものです。

## Repository Split (Public/Private)
著作権保護の観点から公開範囲を最小化し、実装本体は非公開で保持します。

Public (marketing / docs)
```
classmasta-public/
  README.md
  docs/
  assets/
  LICENSE
```

Private (core)
```
classmasta-core/
  app/
    ui/
    application/
    domain/
    infrastructure/
  tests/
  main.py
  requirements.txt
  .env.example
```

## Layered Design
依存方向は一方向のみ（UI -> Application -> Ports -> Infrastructure -> DB）。

```
UI
  |
  v
Application (Usecases)
  |
  v
Ports (Interfaces)
  ^
  |
Infrastructure (Adapters/Stores)
  |
  v
SQLite
```

## Data Flow Example (Attendance)
```
AttendanceDialog (UI)
  -> attendance_usecase
     -> AttendancePort
        -> AttendanceAdapter
           -> attendance_store
              -> SQLite (students, attendance)
```

## Data Classification
```
Public:
  - docs/ (architecture, overview)
  - assets/ (screenshots)

Private:
  - app/ (all source code)
  - *.db, db_backups/
  - ai_keys.json, .env
  - Texts/ (教材)
  - 生徒名簿や出席データ
```

## Rationale
- 無駄時間(B)削減のため、UIは現場操作を最短導線に集約
- 学習効率(L=R x h)を最大化するため、即時共有(PDF/QR)と即時フィードバックを前提に設計
- 変更影響を限定するため、DBや外部連携はPortsで隔離
