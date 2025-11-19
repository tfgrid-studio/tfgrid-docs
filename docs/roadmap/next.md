# Next - Planned Features

**📋 What's coming after current work**

---

## Voice Interface (STT/TTS)

**Status:** 📋 Planned

### Overview

Layer on top of the existing CLI and dashboard with speech-to-text and text-to-speech – deploy and manage applications hands-free.

### What You'll Be Able To Do

```bash
# Speak naturally
🎤 "Deploy a WordPress blog with SSL on mysite.com"

# AI responds with voice
🔊 "Creating WordPress deployment with SSL... 
    Done! Your site is live at https://mysite.com"

# Hands-free workflow
🎤 "Show me the logs"
🎤 "Scale to 3 instances"
🎤 "Check the status"
🎤 "Restart the application"
```

### Features

**Speech-to-Text (STT):**

- Speak commands naturally
- Multi-language support
- Noise cancellation
- Continuous listening mode

**Text-to-Speech (TTS):**

- AI responds with voice
- Natural-sounding output
- Adjustable speed/voice
- Progress updates spoken

**Conversation Flow:**

- Multi-turn dialogue
- Follow-up questions
- Confirmation requests
- Error explanations

**Use Cases:**

- **Hands-free operation** - Perfect for multitasking
- **Accessibility** - Voice-first interface
- **Mobile-friendly** - Use while commuting
- **Live demos** - Impressive presentations

### Architecture

```
Voice Input → STT → AI CLI → TTS → Voice Output
```

---

---

## Fiat Payments

**Status:** 📋 Planned

### Overview

Make TFGrid Studio easier to adopt by supporting **fiat payments** (credit card, Stripe, etc.) alongside TFT.

### What You'll Be Able To Do

- Pay for deployments and services in familiar currencies.
- Use credit cards and standard billing flows.
- Avoid the need to acquire TFT before trying the platform.

### Integration Points

- Billing for long-running deployments.
- Optional fiat pricing for marketplace apps (future).
- Clear invoices and receipts for accounting.

---

**Previous:** [← Now](now.md) | **Next:** [Later →](later.md)
