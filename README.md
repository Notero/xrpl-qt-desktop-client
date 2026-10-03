# XRPL Desktop Client (Qt)

A C++/Qt desktop application for the XRP Ledger: live price streaming, wallet configuration, and local persistence, with an in-progress DEX/AMM arbitrage-detection module.

## What's working

- **Qt/QML GUI** (`qml/Main.qml`, `Wallet.qml`, `MainView.qml`) — wallet view, a terminal-style panel for raw rippled command/response inspection.
- **Live XRPL networking** (`src/networking/`) — connects to the XRPL mainnet over websockets (`QWebSocketClient`, `XrplWebSocketClient`), parses live bid/ask prices from order-book and AMM responses, and emits them to the UI.
- **Wallet configuration** (`src/app/Backend.cpp`) — save/load wallet config exposed to QML via `Q_INVOKABLE` methods.
- **Local storage** (`src/storage/DbRepository.cpp`, `walletconfig.cpp`) — a SQL-backed repository for wallet and transaction data.

## What's a prototype, by design

This project also includes scaffolding for detecting and acting on DEX-vs-AMM arbitrage spreads (`src/arbitrage/ArbitrageEngine.cpp`, `TraderBot.cpp`). After connecting this engine to live XRPL order books and watching them for a while, the conclusion was that apparent spreads don't survive fees and latency closely enough to execute profitably — there was no reliable circular pattern to capture. Rather than fake a working trading bot, the detection and sizing logic (`check_live_arbitrage_opportunities_between_DEX_and_AMM`, `calculate_the_amount_of_xrp_to_buy_to_be_profitable`) is left as documented, unimplemented scaffolding, and `TraderBot`'s submission loop has no real account wired in — it can never actually fire, because nothing populates its trade queue. The useful result of this part of the project was the finding, not a shipped bot.

## Tech stack

C++17, Qt/QML, CMake.

## Build

```bash
cmake -B build
cmake --build build
```

Requires Qt (Widgets, Qml, WebSockets, Sql modules) and CMake installed.
