# 🤖 Polymarket Copy Trading Bot - Automated Prediction Market Trading

**polymarket-copy-trading-bot for automated prediction market trading.** Mirror successful traders' strategies on Polymarket in real-time with a Python-based copytrading bot.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.9+](https://img.shields.io/badge/python-3.9+-blue.svg)](https://www.python.org/downloads/)
[![Polymarket](https://img.shields.io/badge/Polymarket-Compatible-green.svg)](https://polymarket.com/)

> ## ☁️ Skip The Setup → **[snipe.run](https://snipe.run)** — The Cloud Enterprise Version
>
> The code in this repo is the **free, minimal, self-hosted version**. It works, but you'll be the one running a Python process 24/7, holding a private key on disk, debugging WebSocket drops, and rebuilding a dashboard from scratch.
>
> **[snipe.run](https://snipe.run) is the cloud enterprise version** — the bot this codebase evolved into. It runs 24/7 in production on institutional-grade infrastructure, executes trades in **under 200ms**, and has already traded **millions in live Polymarket volume**.
>
> **Why snipe.run, not the free version:**
> - ⚡ **10-30× faster execution** — sub-200ms WebSocket pipeline vs. 2-5 second polling. On Polymarket, speed *is* the edge. The trader you're copying moves → you're on the book before anyone else.
> - 💰 **Battle-tested in live production** — millions in traded Polymarket volume, running 24/7 for over two years. Not a demo. Not a prototype. A proven money-maker.
> - 🎯 **7-layer automated exit engine** — trailing stops, winner-locks, time exits, loss cuts, proportional follow-sells, instant profit ladders. The free version has **none** of this. You'll lock in profits you'd otherwise give back.
> - 📈 **Automatic tier-scaling** — your sizing, max price, and risk caps scale with your bankroll automatically. Start at $100, compound to $100k+, no config changes.
> - 🛑 **Never-miss, never-stop** — dedicated infrastructure that doesn't crash at 3am, doesn't drop WebSocket connections, doesn't need your laptop open. If a trader moves, you move. Period.
> - 📊 **Professional live dashboard** — P/L chart, open positions, exit reasons, real-time log stream, trader roster, win rate. No terminal, no `tail -f`, no guessing.
> - 🤖 **AI co-pilot** — reviews every position the moment it's opened, flags risk, answers "should I hold?" in plain English. Like having a hedge-fund analyst on every trade.
> - 📱 **Telegram push alerts** on every BUY, every exit, every move. Know what your bot did before you unlock your phone.
> - 🚀 **Zero maintenance** — no server, no updates, no restarts, no "why did it crash". It just runs.
>
> **Set up in under 2 minutes.** Sign in → set your trading wallet → go live. No Python, no CLI, no Supabase, no config files.
>
> 👉 **[Start at snipe.run](https://snipe.run)** — see the full feature list in the [**Cloud Enterprise Version**](#️-cloud-enterprise-version--sniperun) section below.

## 📋 Table of Contents

- [About This Polymarket Copy Trading Bot](#about-this-polymarket-copy-trading-bot)
- [Key Features](#key-features)
- [How It Works](#how-it-works)
- [Quick Start](#quick-start)
- [Configuration Guide](#configuration-guide)
- [Usage](#usage)
- [Project Structure](#project-structure)
- [Contributing](#contributing)
- [License](#license)

## 🎯 About This Polymarket Copy Trading Bot

 **polymarket-copy-trading-bot** is an automated trading system designed to replicate the trading strategies of successful Polymarket traders in real-time. Built with Python, Supabase, and the Polymarket CLOB API, this prediction market bot monitors trader activities and automatically executes copytrading orders.

## ✨ Key Features

### Automated Copytrading for Polymarket

This **polymarket copy trading bot** provides:

- **Real-Time Trade Detection**: Monitors target trader's activities instantly
- **Automatic Order Execution**: Places orders on Polymarket automatically
- **Position Tracking**: Tracks all open positions and P&L in real-time
- **Flexible Sizing**: Copy trades at any percentage of the original size
- **Supabase Integration**: Reliable database for trade history and monitoring

## 🏗️ How It Works

**automated polymarket trading bot** operates in simple steps:

### Architecture

```
┌─────────────────┐
│   Polymarket    │
│   API/Events    │
└────────┬────────┘
         │
         ↓
┌─────────────────┐      ┌──────────────────┐
│   Supabase DB   │◄─────┤  Polling Scripts │
│  (PostgreSQL)   │      └──────────────────┘
└────────┬────────┘
         │ Realtime Subscription
         ↓
┌─────────────────┐
│   Main Bot      │
│  - Listeners    │
│  - Handlers     │
└────────┬────────┘
         │
         ↓
┌─────────────────┐
│  Constraints    │
│  - Sizing       │
└────────┬────────┘
         │
         ↓
┌─────────────────┐
│  Order Maker    │
│ (py-clob-client)│
└────────┬────────┘
         │
         ↓
┌─────────────────┐
│  Polymarket     │
│  CLOB API       │
└─────────────────┘
```

### Data Flow

1. **Monitor**: The bot continuously monitors your target trader via Polymarket API
2. **Detect**: Real-time detection of new trades through Supabase
3. **Execute**: Automatically places scaled orders on your Polymarket account
4. **Track**: Maintains complete history of all copytraded positions

## 🚀 Quick Start

Get your **polymarket-copy-trading-bot** running in 5 minutes:

### Prerequisites

- Python 3.9 or higher
- Polymarket account with USDC
- Supabase account (free tier works)
- Private key from your Polymarket wallet

### Installation

1. **Clone the polymarket bot repository**
```bash
git clone https://github.com/giordanopsouza/polymarket-copy-trading-bot.git
cd polymarket-copy-trading-bot
```

2. **Set up Python environment**
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt
```

3. **Configure Supabase Database**

Create your Supabase project and run the setup:

- Go to [supabase.com](https://supabase.com/) and create a project
- Navigate to SQL Editor
- Read the Document "supabase/README.md" and execute the SQL script
- Get your project URL and anon key from Settings → API and Data API

**⚠️ IMPORTANT: Enable Realtime on Tables**

The bot needs Realtime enabled to detect trades instantly. After creating the tables:

1. Go to your Supabase Dashboard
2. Click on **Database** → **Database tables** -> **edit table** -> **enable real time** 
3. Find the following tables and enable Realtime for each:
   - `historic_trades`
   - `polymarket_positions`
4. Click the toggle switch to **enable** Realtime for both tables

Without Realtime enabled, the bot won't detect new trades or position changes!

4. **Configure your copytrading bot**

Copy the example environment file:
```bash
cp env.example .env
```

Edit `.env` with your credentials:

```env
# Supabase Configuration
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_KEY=your-anon-key-here

# Polymarket Credentials
PK=your-private-key-here
POLY_FUNDER=your-polymarket-proxy-address

# Trader to Copy
TRADER_WALLET=trader-wallet-address-to-copy

# Optional: Sizing Configuration
STAKE_WHALE_PCT=0.001
```

**Getting Your Credentials:**

- **Private Key**: Go to [reveal.magic.link/polymarket](https://reveal.magic.link/polymarket) and reveal your key
- **Proxy Address**: Found under your profile picture on Polymarket
- **Trader Wallet**: Copy from any trader's profile on Polymarket

5. **Run your polymarket copy trading bot**

```bash
cd scripts
python main.py
```

That's it! Your **automated polymarket bot** is now running and will start copying trades.

## ⚙️ Configuration Guide

### Environment Variables

All configuration for this **polymarket trading bot** is done via `.env`:

#### Required Settings

```env
SUPABASE_URL=          # Your Supabase project URL
SUPABASE_KEY=          # Supabase anon/public key
PK=                    # Your Polymarket private key
POLY_FUNDER=           # Your Polymarket proxy address
TRADER_WALLET=         # Trader wallet address to copy
```

#### Optional Settings

```env
TRADER_WALLET=         # Trader wallet address to copy (can be changed anytime)
BANKROLL=500          # Your trading capital (default: 1000)
STAKE_WHALE_PCT=0.001 # Copy 0.1% of trader's size (default: 0.005)
```

### Position Sizing Examples

The **copytrading bot** scales trades automatically:

- If whale bets $10,000 and you set `STAKE_WHALE_PCT=0.001`
  - Your bot will place a $10 trade (10,000 × 0.001)
- If whale bets $5,000 and you set `STAKE_WHALE_PCT=0.002`
  - Your bot will place a $10 trade (5,000 × 0.002)

### Finding Profitable Traders to Copy

Visit [polymarket.com/leaderboard](https://polymarket.com/leaderboard) to find:

## 🎮 Usage

### Starting the Bot

```bash
python scripts/main.py
```

The **polymarket copy trading bot** will:
1. ✅ Validate all credentials
2. ✅ Connect to Supabase database
3. ✅ Start monitoring your target trader
4. ✅ Automatically execute copytrading orders

### Testing Configuration

Verify your setup before live trading:

```bash
python scripts/config.py
```

This will display your current configuration and validate all credentials.

### Monitoring Your Bot

The bot provides real-time console output showing:
- New trades detected from target trader
- Position updates and P&L changes
- Orders placed on your account
- Connection status and errors

## 📁 Project Structure

```
polymarket-copy-trading-bot/         # Polymarket copy trading bot
├── scripts/
│   ├── main.py                    # Main bot application
│   ├── config.py                  # Configuration management
│   ├── make_orders.py             # Order execution
│   ├── get_player_positions.py   # Position tracking
│   ├── get_player_history_new.py # Trade history
│   └── constraints/
│       └── sizing.py              # Position sizing logic
├── supabase/
│   ├── create_table.sql           # Database schema
│   └── README.md                  # Supabase setup guide
├── env.example                    # Configuration template
├── requirements.txt               # Python dependencies
├── README.md                      # This file
└── LICENSE                        # MIT License
```

## 🛡️ Risk Management


### Position Sizing Tips

1. Set `STAKE_WHALE_PCT` conservatively (0.001 = 0.1%)
2. Monitor the trader's performance before increasing position sizes
3. Keep some capital in reserve for market opportunities

## 🤝 Contributing

Contributions to this **polymarket-copy-trading-bot** are welcome!

### Ways to Contribute

- 🐛 Report bugs and issues
- 💡 Suggest new features
- 📝 Improve documentation
- 🔧 Submit pull requests
- ⭐ Star the repo if you find it useful

### Roadmap

- [ ] Web dashboard for monitoring
- [ ] Advanced risk management features
- [ ] Telegram/Discord notifications

> ### ✅ Want the improved, production-ready version? → **[snipe.run](https://snipe.run)**
>
> **[snipe.run](https://snipe.run) is the improved, production-ready version of this bot** — same roots, rebuilt for real trading. Very fast. Very accurate. Zero installation. Every roadmap item above is already shipped, plus a lot the free version never will be:
>
> - ✅ **Profit-tested & reliable** — live on Polymarket for over two years, consistently profitable, zero missed signals, zero downtime
> - ✅ **Web dashboard** — production-grade, live P/L chart, open positions, recent closes, real-time log stream
> - ✅ **Advanced risk management** — automatic bankroll-tier scaling, event & trader exposure caps, re-buy cooldowns, cost-basis guard
> - ✅ **Telegram notifications** — real-time push on every BUY, every exit, every move
> - ⚡ **Blazing fast — sub-200ms WebSocket execution** vs. 2-5s polling in the free version
> - 🎯 **Surgically accurate — 7-layer automated exit engine** — trailing stops, winner-locks, time exits, loss cuts, follow-sells, profit ladders
> - 🤖 **AI co-pilot** — reviews every position the moment it opens, flags risk, answers "should I hold?"
> - 💰 **Battle-tested** — millions in live Polymarket volume, running 24/7 for over two years
> - 🚀 **Zero installation, zero maintenance** — no server, no CLI, no Python, no `.env` file. Sign in, set wallet, go live in under 2 minutes.
>
> 👉 **[Start at snipe.run](https://snipe.run)** — fast. accurate. no install. Support: [t.me/sniperun](https://t.me/sniperun)

## 📄 License

This polymarket trading bot is licensed under the MIT License - see [`LICENSE`](LICENSE) for details.

**Disclaimer**: This is an educational tool. Trading prediction markets involves financial risk. Use at your own discretion. Always start with small amounts and never invest more than you can afford to lose.

## 🔗 Related Resources

- [snipe.run](https://snipe.run) - **Cloud enterprise version of this bot** (no install)
- [Polymarket](https://polymarket.com/) - Official Polymarket platform
- [Polymarket Docs](https://docs.polymarket.com/) - API documentation
- [Supabase](https://supabase.com/) - Database platform
- [py-clob-client](https://github.com/Polymarket/py-clob-client) - Polymarket Python SDK


---

**Keywords**: polymarket-copy-trading-bot, polymarket bot, prediction market bot, automated trading bot, polymarket automation, copy trading polymarket, polymarket mirror trading, crypto prediction markets

**Built with ❤️ for the Polymarket community  |  Cloud Enterprise Version: [snipe.run](https://snipe.run)**

Contact: https://t.me/sniperun

*This polymarket copy trading bot helps traders automate their prediction market strategies. Star ⭐ the repo to support development!*
