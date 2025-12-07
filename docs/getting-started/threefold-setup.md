# ThreeFold Setup Guide (DIY Path)

This guide walks you through setting up your ThreeFold Chain wallet and preparing to use `tfgrid-compose`.

**Estimated time:** 1-2 hours  
**Cost:** Minimum ~$10-20 USD for initial TFT  
**Prerequisites:** Smartphone (iOS or Android)

---

## Overview

To deploy applications on ThreeFold Grid using `tfgrid-compose`, you need:

1. ✅ ThreeFold Chain wallet (with seed phrase)
2. ✅ Funded wallet with TFT (ThreeFold Token)
3. ✅ Completed KYC verification
4. ✅ Basic understanding of the process

This is the **DIY path** - you manage your own wallet and funds.

---

## Step 1: Install ThreeFold Connect

**ThreeFold Connect** is your mobile wallet for ThreeFold Chain.

### Download the App

- **iOS:** [App Store Link](https://apps.apple.com/app/threefold-connect/id1459845885)
- **Android:** [Google Play Link](https://play.google.com/store/apps/details?id=org.jimber.threebotlogin)

### Create Your Wallet

1. Open ThreeFold Connect
2. Tap **"Create New Wallet"**
3. Choose a strong password
4. **CRITICAL:** Write down your seed phrase!
   - You'll see 12 or 24 words
   - Write them on paper (NOT digitally)
   - Store in a safe place
   - You CANNOT recover your wallet without this

5. Verify your seed phrase (the app will ask)
6. Your wallet is created! ✅

### Important Notes

⚠️ **Your seed phrase is your private key**

- Never share it with anyone
- Never store it digitally (no screenshots, no cloud)
- If you lose it, your funds are GONE forever
- ThreeFold cannot recover your wallet

💡 **This is your TFChain address**

- You'll see an address like: `5F3sa...`
- This is where you'll receive TFT
- You'll use your seed phrase with `tfgrid-compose`

---

## Step 2: Get TFT (ThreeFold Token)

You need TFT to deploy on ThreeFold Grid. Here are the main ways to acquire it:

### Option A: Buy on Stellar (via Lobstr)

This is the most common method for new users.

#### 2.1 Install Lobstr

- **iOS:** [App Store](https://apps.apple.com/app/lobstr-stellar-wallet/id1404357892)
- **Android:** [Google Play](https://play.google.com/store/apps/details?id=com.lobstr.client)

#### 2.2 Import Your Wallet to Lobstr

1. Open Lobstr
2. Tap **"Import Wallet"**
3. Enter the SAME seed phrase from ThreeFold Connect
4. Your wallet is now accessible in both apps

#### 2.3 Buy USDC or XLM

1. In Lobstr, tap **"Buy Crypto"**
2. Choose USDC or XLM
3. Enter amount (start with $20-50)
4. Complete payment with credit card
5. Wait for confirmation (~5 minutes)

#### 2.4 Swap to TFT

1. Tap **"Trade"** in Lobstr
2. Search for **"TFT"** (ThreeFold Token)
3. Select the swap: USDC → TFT (or XLM → TFT)
4. Enter amount
5. Confirm the swap
6. You now have TFT on Stellar! ✅

### Option B: Buy on Decentralized Exchanges

- **PancakeSwap** (BSC network)
- **Uniswap** (Ethereum network)
- Check [ThreeFold website](https://threefold.io) for current listings

### Option C: Receive from Someone

If someone else has TFT, they can send it directly to your wallet address.

---

## Step 3: Bridge TFT to ThreeFold Chain

**Important:** TFT on Stellar needs to be bridged to ThreeFold Chain for deployments.

### Why Bridge?

```
TFT on Stellar Network    →    TFT on ThreeFold Chain
(For trading/swaps)             (For Grid deployments)
```

Only TFT on ThreeFold Chain can be used to deploy VMs on the Grid.

### How to Bridge

1. Open **ThreeFold Connect** app
2. Tap **"Bridge"** or **"Stellar Bridge"**
3. Select amount of TFT to bridge
4. Confirm: **Stellar → TFChain**
5. Wait ~5-10 minutes for confirmation
6. Check your TFChain balance in the app

### Verify Balance

In ThreeFold Connect:
- Check **"TFChain Balance"** (not Stellar)
- This is what you can use for deployments
- Example: `1000 TFT` on TFChain = ready to deploy

---

## Step 4: Complete KYC Verification

ThreeFold Grid requires KYC (Know Your Customer) for certain features.

### Start KYC Process

1. Open ThreeFold Connect
2. Go to **Settings** → **KYC Verification**
3. Choose verification method:
   - **Passport** (recommended)
   - **National ID**
   - **Driver's License**

### Upload Documents

1. Take a clear photo of your ID
   - Good lighting
   - All corners visible
   - Text readable

2. Take a selfie
   - Follow the on-screen instructions
   - Hold your ID next to your face if required

3. Submit for review

### Wait for Approval

- **Typical time:** 24-72 hours
- **Status:** Check in app (Pending → Approved)
- **Notification:** You'll receive an email when approved

### What if Rejected?

- Check email for reason
- Common issues:
  - Photo too blurry
  - Glare on ID
  - Expired ID
  - Name mismatch
- Resubmit with corrections

---

## Step 5: Use with tfgrid-compose

Once you have:
- ✅ Seed phrase
- ✅ TFT on TFChain
- ✅ KYC approved

You're ready to deploy!

### Login

```bash
tfgrid-compose login
```

When prompted, enter your seed phrase (12 or 24 words).

### Check Your Setup

```bash
tfgrid-compose login --check
```

This verifies:
- ✓ Seed phrase is valid
- ✓ Can connect to TFChain
- ✓ Balance is sufficient

### Your First Deployment

```bash
# Search for apps
tfgrid-compose search

# Deploy a simple VM
tfgrid-compose up single-vm

# Check status
tfgrid-compose status

# View logs
tfgrid-compose logs

# Stop when done
tfgrid-compose down
```

---

## Cost Estimates

### Initial Setup
- TFT purchase: $20-50 (your choice)
- Transaction fees: ~$0.10-0.50
- KYC: Free

### Deployment Costs (Approximate)

| Resource | Cost (USD/month) | Cost (TFT/month) |
|----------|------------------|------------------|
| Small VM (1 CPU, 2GB RAM) | $3-5 | 60-100 TFT |
| Medium VM (2 CPU, 4GB RAM) | $8-12 | 160-240 TFT |
| Large VM (4 CPU, 8GB RAM) | $15-25 | 300-500 TFT |
| AI Agent (API-based) | $5-10 | 100-200 TFT |

*Prices vary by farmer and resource availability*

### How Long Will My TFT Last?

```
$50 → ~1000 TFT
Small VM = ~80 TFT/month
= 12+ months of runtime

Medium VM = ~200 TFT/month  
= 5 months of runtime
```

---

## Troubleshooting

### "Can't find my seed phrase"

❌ **If you lost your seed phrase, your wallet is unrecoverable.**

You must:
1. Create a NEW wallet
2. Write down the NEW seed phrase
3. Transfer any remaining funds (if you still have access)

### "Bridge is taking too long"

- Normal wait: 5-10 minutes
- If > 30 minutes:
  1. Check Stellar network status
  2. Check ThreeFold Chain status
  3. Contact support if > 2 hours

### "KYC pending for days"

- Normal: 24-72 hours
- If > 5 days:
  - Check spam folder for emails
  - Resubmit with clearer photos
  - Contact ThreeFold support

### "Not enough TFT for deployment"

Check:
```bash
tfgrid-compose login --check
```

Shows your balance. If low:
- Bridge more from Stellar
- Buy more TFT
- Use smaller deployment

### "Deployment failed"

Common causes:
- Insufficient TFT
- KYC not approved
- Invalid seed phrase
- Network congestion
- No available nodes

Check logs:
```bash
tfgrid-compose logs
```

---

## Security Best Practices

### Seed Phrase Security

✅ **DO:**

- Write on paper, store in safe
- Use multiple copies in different locations
- Consider a hardware wallet (advanced)
- Test recovery process with small amount first

❌ **DON'T:**

- Screenshot or photo
- Store in cloud (Google Drive, Dropbox, etc.)
- Store in password manager (debated, but risky)
- Email or message to yourself
- Share with anyone (even support!)

### Account Security

- Use strong password on ThreeFold Connect
- Enable biometric lock (fingerprint/face)
- Keep apps updated
- Be wary of phishing (fake apps, emails)
- Double-check addresses before sending TFT

### Computer Security

When using `tfgrid-compose`:
- Keep OS updated
- Use antivirus software
- Don't enter seed phrase on shared computers
- Be careful with command history:

```bash
# Prevent seed phrase in history
set +o history
tfgrid-compose login
set -o history
```

---

## Resources

### Official Links
- **ThreeFold Website:** https://threefold.io
- **ThreeFold Manual:** https://manual.grid.tf
- **ThreeFold Forum:** https://forum.threefold.io
- **TFGrid Dashboard:** https://dashboard.grid.tf

### Community
- **Telegram:** https://t.me/threefold
- **Discord:** https://discord.gg/threefold

### TFGrid Studio
- **Documentation:** https://docs.tfgrid.studio
- **GitHub:** https://github.com/tfgrid-studio
- **Support:** [GitHub Discussions](https://github.com/orgs/tfgrid-studio/discussions)

---

## What's Next?

Once you're set up:

1. **Learn the CLI:** [tfgrid-compose Commands](../cli/app-commands.md)
2. **Deploy Apps:** [Quick Start Guide](quickstart.md)
3. **Browse Patterns:** [Available Patterns](../patterns/overview.md)
4. **Join Community:** Share your experience!

---

**Ready to deploy?**

```bash
tfgrid-compose login
tfgrid-compose search
tfgrid-compose up <app>
```

Happy deploying! 🚀
