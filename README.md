# ✦ Stellar Tip Jar — 
## 🌐 Live Demo
👉 https://s-hagun2006.github.io/stellar--tip--jar/

A web-based dApp that lets you **send XLM tips with a message** on the Stellar blockchain.
Built for the **Stellar Journey to Mastery Builder Challenge** 

---

## 📁 Project Structure

```
stellar-tip-jar/
│
├── index.html          ← The entire app (HTML + CSS + JS in one file)
└── README.md           ← This guide
```

That's it — **one file**, no build tools, no npm, no bundler. Open it in a browser and it works.

---

## 🏗️ Project Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    User's Browser                           │
│                                                             │
│  ┌────────────────┐    ┌──────────────────────────────┐     │
│  │   HTML/CSS UI  │    │    JavaScript App Logic      │     │
│  │  (Tip Jar UI)  │◄──►│  (Stellar SDK + your code)  │     │
│  └────────────────┘    └──────────────┬───────────────┘     │
│                                       │ HTTP requests        │
└───────────────────────────────────────┼─────────────────────┘
                                        │
                         ┌──────────────▼───────────────┐
                         │     Horizon API               │
                         │  (Stellar's HTTP gateway)     │
                         │  horizon-testnet.stellar.org  │
                         └──────────────┬───────────────┘
                                        │
                         ┌──────────────▼───────────────┐
                         │    Stellar Testnet            │
                         │    (Blockchain network)       │
                         │  Validators → Ledger closes   │
                         │  every ~5 seconds             │
                         └──────────────────────────────┘
```

### Key Components:
| Component | What it does |
|---|---|
| **Stellar SDK** | JavaScript library to build/sign/submit transactions |
| **Horizon API** | Stellar's REST API — reads blockchain data, submits transactions |
| **Keypair** | Your wallet: public key (address) + secret key (password) |
| **Transaction** | A signed instruction to move XLM between accounts |
| **Memo** | A 28-character message stored permanently on the blockchain |

---

## 🚀 Setup — Step by Step

### Step 1: No Installation Needed!

This project uses **no npm, no build tools**. The Stellar SDK is loaded from a CDN.

Just download `index.html` and open it in your browser. Done.

---

### Step 2: Create a Stellar Testnet Wallet

#### Option A — Stellar Laboratory (Easiest)
1. Go to: https://laboratory.stellar.org/#account-creator?network=test
2. Click **"Generate Keypair"**
3. You'll see:
   ```
   Public Key:  G... (your address — safe to share)
   Secret Key:  S... (your password — NEVER share)
   ```
4. Copy both keys somewhere safe.

#### Option B — Use Friendbot directly
Open this URL in your browser (replace YOUR_PUBLIC_KEY):
```
https://friendbot.stellar.org/?addr=YOUR_PUBLIC_KEY
```
This gives you **10,000 free test XLM**.

#### Important Notes:
- ✅ Testnet XLM = fake money, for testing only
- ❌ NEVER use a real/mainnet secret key in this app
- 🔒 Secret keys start with `S`, public keys start with `G`

---

### Step 3: Fund Your Account

After creating your keypair, you must fund it with Friendbot:
```
https://friendbot.stellar.org/?addr=GPASTE_YOUR_PUBLIC_KEY_HERE
```
You'll get a JSON response like `{"hash": "abc123..."}` — that means it worked!

---

### Step 4: Run the App

```bash
# Option 1: Just open in browser
open index.html          # Mac
start index.html         # Windows
xdg-open index.html      # Linux

# Option 2: Use a simple local server (avoids CORS issues in some browsers)
# If you have Python installed:
python3 -m http.server 8080
# Then open: http://localhost:8080

# Option 3: VS Code Live Server extension
# Right-click index.html → Open with Live Server
```

---

### Step 5: Use the App

1. **Paste your Secret Key** in the "Your Wallet" section
2. Click **Connect Wallet** — you'll see your balance
3. **Paste any Stellar testnet public key** as recipient
4. Enter an amount and optional message
5. Click **Send Tip** — the transaction is sent to the blockchain!
6. Click "View on Stellar Expert" to see your transaction live

---

## 🔗 How the Blockchain Transaction Works (Code Walkthrough)

Here's what happens when you click "Send Tip":

```
1. loadAccount(publicKey)
   └─ Horizon API fetches your account info + sequence number
      (sequence number = transaction counter, prevents replay attacks)

2. TransactionBuilder(sourceAccount, { fee, networkPassphrase })
   └─ Creates a blank transaction container

3. .addOperation(Operation.payment({ destination, asset, amount }))
   └─ Adds the "send XLM" instruction

4. .addMemo(Memo.text("your message"))
   └─ Attaches your 28-char message permanently on-chain

5. .setTimeout(30).build()
   └─ Sets 30s expiry and finalizes the transaction object

6. transaction.sign(keypair)
   └─ Cryptographically signs with your secret key
      (proves YOU authorized this — no one can forge it)

7. server.submitTransaction(transaction)
   └─ Sends to Horizon → broadcasts to Stellar validators
      → included in next ledger close (~5 seconds)
      → returns { hash: "abc123..." } on success
```

---

## 🧪 Testing Checklist

- [ ] Connect wallet with testnet secret key
- [ ] See balance load correctly
- [ ] Send 1 XLM to another testnet address (use Friendbot for a second one)
- [ ] See the success message and TX hash
- [ ] Click "View on Stellar Expert" and see your transaction
- [ ] See memo message in the explorer
- [ ] See updated balance
- [ ] See transaction in history panel

### Create a second wallet to test sending:
1. Go to Stellar Laboratory again
2. Generate a NEW keypair
3. Fund it with Friendbot
4. Use that public key as the recipient in the app

---

## 💡 How to Get Test XLM

| Method | Link |
|---|---|
| Friendbot (10,000 XLM) | https://friendbot.stellar.org/?addr=YOUR_KEY |
| Stellar Laboratory | https://laboratory.stellar.org |
| Testnet Faucet | https://horizon-testnet.stellar.org/friendbot?addr=YOUR_KEY |

---

## 🎯 Features for Challenge Levels

### White Belt (Current) ✅
- [x] Connects to Stellar Testnet
- [x] Loads account balance via Horizon API
- [x] Sends XLM payment with memo
- [x] Displays transaction hash
- [x] Shows transaction history
- [x] Links to block explorer

### Yellow Belt (Next Steps) — Suggested Improvements:

1. **Add Stellar Wallets (Freighter Integration)**
   - Instead of pasting a secret key (insecure), use the Freighter browser extension wallet
   - `npm install @stellar/freighter-api`

2. **QR Code for Receiving Tips**
   - Generate a QR code with your public key so others can tip you easily

3. **Multi-asset Support**
   - Show balances for USDC and other Stellar assets alongside XLM

4. **Tip Page / Profile Link**
   - Let creators make a shareable `/tip/GADDRESS` page

5. **Transaction Explorer Embed**
   - Show full transaction details inline instead of linking out

6. **Federation Address Support**
   - Let users type `name*example.com` instead of `GABCD...`

---

## 🔐 Security Notes

| Practice | Reason |
|---|---|
| This app never sends your secret key to any server | Your key only lives in browser memory |
| Use Testnet only | Real keys should only be used in production apps with hardware wallet support |
| Secret key is in a `type="password"` field | Prevents shoulder surfing |
| Transactions signed client-side | Server never sees your private key |

---

## 🌐 Useful Links

| Resource | URL |
|---|---|
| Stellar Laboratory | https://laboratory.stellar.org |
| Horizon Testnet API | https://horizon-testnet.stellar.org |
| Stellar Expert Explorer | https://stellar.expert/explorer/testnet |
| Stellar SDK Docs | https://stellar.github.io/js-stellar-sdk |
| Stellar Docs | https://developers.stellar.org/docs |
| Friendbot | https://friendbot.stellar.org |

---

## 📝 Challenge Submission Notes

- **Project Type**: Web dApp (single HTML file)
- **Network**: Stellar Testnet
- **Tech Stack**: Vanilla JavaScript, Stellar SDK v11 (CDN)
- **Key Feature**: XLM payment with on-chain memo
- **No backend required**: Fully client-side

---

*Built with ✦ for the Stellar Journey to Mastery Builder Challenge*
