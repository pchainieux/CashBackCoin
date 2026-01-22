# CashBackCoin

cashback-coin-mvp/
├── README.md
├── LICENSE
├── .gitignore
├── .env.example                 # RPC URLs, keys, DB config (no secrets)
├── docs/
│   ├── architecture.md          # High-level flow: partner vaults, cashback logic [MVP]
│   ├── onchain-spec.md          # Accounts, instructions, state (Anchor) 
│   ├── offchain-spec.md         # Indexer, eligibility checker, submitter
│   └── cashback-formulas.md     # Placeholder; later define exact formulas
│
├── onchain/                     # Solana smart contract (Rust + Anchor)
│   ├── Anchor.toml
│   ├── Cargo.toml
│   ├── programs/
│   │   └── cashback_program/
│   │       ├── Cargo.toml
│   │       └── src/
│   │           ├── lib.rs       # Program entry, instructions wiring
│   │           ├── state.rs     # PartnerVault, RewardRecord, config structs
│   │           ├── instructions/
│   │           │   ├── init_partner_vault.rs
│   │           │   ├── fund_partner_vault.rs
│   │           │   ├── reward_purchase.rs
│   │           │   └── close_partner_vault.rs
│   │           └── errors.rs    # Custom error codes (insufficient funds, already rewarded, etc.)
│   └── tests/
│       └── cashback_program.ts  # Anchor integration tests (TypeScript)
│




├── offchain/                    # Python services
│   ├── pyproject.toml           # Or requirements.txt + setup.cfg
│   ├── README.md
│   ├── cashback_common/         # Shared Python utilities
│   │   ├── __init__.py
│   │   ├── config.py            # Load env, config objects
│   │   ├── solana_client.py     # RPC/websocket client helpers
│   │   ├── anchor_client.py     # Thin wrapper to call the Anchor program
│   │   ├── models.py            # Pydantic/dataclasses for events, rewards, partner config
│   │   └── logging_utils.py
│   │
│   ├── indexer/                 # Watches blockchain for partner token buys 
│   │   ├── __init__.py
│   │   ├── main.py              # Entry point: subscribe to logs/slots, detect purchases
│   │   └── parser.py            # Decode transactions, extract buyer, amount, tx hash
│   │
│   ├── eligibility/             # Applies business rules & formulas
│   │   ├── __init__.py
│   │   ├── engine.py            # Check CBC balance, cooldowns, thresholds
│   │   └── formulas.py          # Placeholder for cashback formula implementations
│   │
│   ├── submitter/               # Sends reward transactions to the program
│   │   ├── __init__.py
│   │   ├── main.py              # Worker: consume eligible events, call reward_purchase
│   │   └── builder.py           # Build & sign Solana txs
│   │
│   ├── api/                     # Simple HTTP API for partners & debugging (MVP)
│   │   ├── __init__.py
│   │   ├── main.py              # FastAPI/Flask app
│   │   └── routes/
│   │       ├── partners.py      # Endpoints: create vault tx, view stats
│   │       └── health.py
│   │
│   └── migrations/              # DB schema (if using SQL)
│       └── 001_init.sql         # Tables: partners, vaults, purchases, rewards
│
└── ui/                          # Optional minimal web UI (later)
    ├── README.md
    ├── package.json
    └── src/
        ├── main.tsx
        ├── components/
        │   ├── ConnectWallet.tsx
        │   └── CashbackStatus.tsx
        └── pages/
            └── Dashboard.tsx
