# 🤖 Polymarket Copy Trading Bot - Automated Prediction Market Trading

**polymarket-copy-trading-bot for automated prediction market trading.** Mirror successful traders' strategies on Polymarket in real-time with a Python-based copytrading bot.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.9+](https://img.shields.io/badge/python-3.9+-blue.svg)](https://www.python.org/downloads/)
[![Polymarket](https://img.shields.io/badge/Polymarket-Compatible-green.svg)](https://polymarket.com/)

> ## ☁️ Prefer a Hosted, No-Install Version? → **[snipe.run](https://snipe.run)**
>
> **[snipe.run](https://snipe.run)** is the **cloud enterprise version** of this bot — fully managed, zero installation, no server to run, no CLI. Sign in with **Google or MetaMask**, set your trading wallet, and start copying traders **in under 2 minutes**.
>
> The managed platform ships everything in this repo **plus** a live dashboard, multi-profile accounts, encrypted key storage, 2FA, sub-200ms WebSocket trail-stops, an AI co-pilot, and Telegram alerts. See the full list in the [**Cloud Enterprise Version**](#️-cloud-enterprise-version--sniperun) section below.
>
> 👉 **[Start free at snipe.run](https://snipe.run)** — no installation, no config, no gas fumbling.

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


## ☁️ Cloud Enterprise Version — [snipe.run](https://snipe.run)

Don't want to babysit a Python process, configure Supabase, hold a private key on disk, or rebuild a dashboard yourself? The **cloud enterprise version** of this bot is live at **[snipe.run](https://snipe.run)** — production-hardened, running 24/7 on Polymarket, no installation required.

**👉 [Start free at snipe.run](https://snipe.run)** — sign in with Google or MetaMask, set your trading wallet, go live in under 2 minutes. No server, no CLI, no Supabase setup.

### Everything in the free version, plus:

#### 🔐 Security & Key Management
- **Google OAuth** or **MetaMask SIWE** sign-in — no password to forget
- **AES-GCM encrypted private keys** with per-profile HKDF subkeys — the master key lives off-database, so a full DB dump cannot decrypt your keys
- **TOTP 2FA** (Google Authenticator / Authy) with recovery codes
- **On-demand fresh-challenge 2FA** for sensitive actions (key entry, settings changes) — re-prompts even if you're logged in
- **Full audit log** on every security-sensitive action

#### ⚡ Production-Grade Trading Engine
- **Sub-200ms WebSocket trail-stops** (vs. poll-based in the free version) — reacts to price moves in milliseconds, not seconds
- **Instant ladder**: 3-tier partial profit-taking SELL orders placed 3 seconds after every BUY (+5¢ → 50%, +15¢ → 30%, +32¢ → 20%)
- **Multi-rule exit stack**:
  - Bid-85 winner-lock on near-certain outcomes
  - Armed trail drop (6% from peak, 2% floor — never locks a loss)
  - Time exit (4h+ in profit)
  - Loss cut (-35% after 10min) with cost-basis bypass
- **Active / passive trader modes** — full exit stack for whale buy-and-forget traders, lean stack for active traders who manage their own exits
- **Proportional follow-sell** when the copied trader exits, with dust guards and automatic full-close upgrade when they're exiting ≥90%
- **Cost-basis guard** — automated paths cannot sell below cost unless loss-cut explicitly fires
- **Automatic tier tuning** — sizing, max price, event-exposure cap, and stop-loss all scale with your bankroll (T1 at <$1K through T6 at $200K+)

#### 👥 Multi-Profile Accounts
- Up to **5 isolated trading profiles** per account
- Per-profile **filesystem isolation** — separate SQLite DB, AI memory, systemd process tree
- Per-profile **wallet, sizing, risk, and trader roster**
- One-click **profile switcher** + **default profile** + per-profile **start / stop**

#### 📊 Live Dashboard
- Real-time **portfolio chart** with growth tracking + drawdown markers
- **Open-position monitor** showing armed-trail state, peak, and sellable value
- **Recent-closes feed** with close-reason attribution (trail, time, loss-cut, follow-sell, manual)
- **Live terminal log stream** via SSE — watch the bot think in real time
- **Hero metrics**: portfolio value, today's P/L, open count, closed count
- **Network latency** monitor for CLOB API and RPC
- **Live Polymarket leaderboard** integration — find new traders to copy without leaving the dashboard

#### 🗂️ Roster & Trader Management
- **Click-to-add** traders with one-tap active/passive type toggle
- Per-trader **portfolio value** and **your exposure** at a glance
- **One-click enable / disable** (no restart required)
- **Re-buy cooldown** to prevent instant re-entry after a stop-out

#### 🤖 AI Co-Pilot ($30/mo add-on)
- **Claude Opus 4.7** position assessor with full P/L + cost-basis context
- **Automatic position reviews** on every new BUY — the AI flags overextension, concentration, and time-decay risk
- **Chat interface** — ask "is this worth holding?" and get a decision with reasoning
- **Per-position assessment history**

#### 📱 Notifications
- **Telegram bot** with allowlisted chat IDs
- Commands: `/status`, `/positions`, `/cash`, `/tier`, `/stop_all`, `/start_all`
- Event push — every BUY, every auto-sell, every tier change
- **Periodic P/L summaries** (configurable interval)

#### 🧾 Billing & Transparency
- **$500 USDC / year** base subscription (on-chain, Polygon)
- **10% fee on net realized profit** — automated monthly settlement, visible ledger
- **Transactions page** — every payment, every fee, every on-chain tx hash
- **Fee waiver** if you ended the month in the red (auto-computed)

#### 🛠️ Admin Tooling (for teams)
- Full user / profile / billing / audit views
- Goodwill fee waivers + dispute resolution
- Per-user AI access grants
- Live billing-config editor (treasury, price, tolerance)

👉 **Start at [snipe.run](https://snipe.run)** — hassle-free, no install, no gas fumbling. Support at [t.me/sniperun](https://t.me/sniperun).

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
