# ShegerPay SDK

Official SDKs for integrating ShegerPay quickly.

## What You Can Do

- Verify Ethiopian payment transactions (`/verify`, `/quick-verify`, `/verify-image`)
- Create and manage payment links
- Verify crypto payments
- Use PayPal checkout and PayPal payout requests
- Configure webhooks and verify webhook signatures
- Read transaction history and API usage

## Auth

Use your secret key in the `X-API-Key` header.

- Test: `sk_test_...`
- Live: `sk_live_...`

## Quick Start

```python
from shegerpay import ShegerPay

client = ShegerPay(api_key="sk_test_xxx")
result = client.verify(transaction_id="FT24352648751234", amount=100, provider="cbe")

print(result.valid)
```

BOA note:

- Use full BOA receipt URL or full `trx` value
- Include sender account (`sender_account` / `senderAccount`)

## Install

| Language | Install |
| --- | --- |
| TypeScript / JavaScript | `npm install @shegerpay/sdk` |
| Python | `pip install shegerpay` |
| PHP | `composer require shegerpay/sdk` |
| Ruby | `gem install shegerpay` |
| Go | `go get github.com/shegerpay/sdk-go` |
| Java / Kotlin | `com.shegerpay:sdk` |
| C# | `dotnet add package ShegerPay.SDK` |
| Swift (iOS) | Swift Package Manager (`ShegerPaySDK`) |
| Dart / Flutter | `dart pub add shegerpay` |

## Public Scope

The SDK intentionally focuses on public, stable endpoints.

- Non-PayPal international account setup is private/assisted
- Wallet conversion across private rails is not part of public SDK usage

## Webhook Signature Check

```javascript
const ok = await ShegerPay.verifyWebhookSignature(
  payload,
  req.headers["x-shegerpay-signature"],
  "whsec_xxx"
);
```

## Docs and Support

- Docs: https://shegerpay.com/docs
- API base URL: `https://api.shegerpay.com/api/v1`
- Support: support@shegerpay.com

---

## 🔒 Security Best Practices

### API Key Security

| ✅ DO                                   | ❌ DON'T                           |
| --------------------------------------- | ---------------------------------- |
| Store API keys in environment variables | Hard-code keys in source code      |
| Use `sk_test_` keys in development      | Use `sk_live_` keys in development |
| Rotate keys if compromised              | Share keys across projects         |
| Use server-side verification only       | Expose keys in client-side code    |

```bash
# Store keys securely
export SHEGERPAY_API_KEY="sk_live_xxx"
```

```python
# Read from environment
import os
client = ShegerPay(os.getenv('SHEGERPAY_API_KEY'))
```

### Webhook Security

**Always verify webhook signatures before processing:**

```python
# Python
is_valid = ShegerPay.verify_webhook_signature(
    payload=request.body,
    signature=request.headers['X-ShegerPay-Signature'],
    secret=os.getenv('WEBHOOK_SECRET')  # Store securely!
)

if not is_valid:
    return Response(status=401)  # Reject invalid signatures
```

### HTTPS Only

- All API calls use HTTPS (TLS 1.3)
- Never disable SSL verification
- Verify you're connecting to `api.shegerpay.com`

---

## 👤 User Guide: What You Need to Do

### Step 1: Get API Keys

1. Sign up at [shegerpay.com](https://shegerpay.com)
2. Go to Dashboard → API Keys
3. Generate a **test key** (`sk_test_xxx`) for development
4. Generate a **live key** (`sk_live_xxx`) for production

### Step 2: Add Your Bank Account

1. Go to Dashboard → Linked Accounts
2. Add your CBE/Telebirr account details
3. Your account will be used for verification matching

### Step 3: Integrate the SDK

```python
# Install
pip install shegerpay

# Use
from shegerpay import ShegerPay
client = ShegerPay('sk_test_xxx')
result = client.verify(transaction_id='FT123456', amount=100, provider='cbe')
```

### Step 4: Set Up Webhooks (Recommended)

1. Create webhook: Dashboard → Webhooks → Add
2. Enter your endpoint URL
3. Copy the webhook secret
4. Handle events in your server

### Step 5: Go Live

1. Test thoroughly with `sk_test_` keys
2. Switch to `sk_live_` key
3. Remove test transaction IDs
4. Monitor Dashboard for live transactions

---

## ⚠️ Error Handling

All SDKs return standardized error codes:

| Error Code | Meaning               | What to Do                      |
| ---------- | --------------------- | ------------------------------- |
| `AUTH_001` | Missing API key       | Add `X-API-Key` header          |
| `AUTH_002` | Invalid API key       | Check key in Dashboard          |
| `TX_001`   | Transaction not found | Verify transaction ID           |
| `TX_002`   | Amount mismatch       | Check expected vs actual amount |
| `PROV_001` | Bank timeout          | Retry after 30 seconds          |
| `SUB_001`  | Limit exceeded        | Upgrade plan                    |

```python
try:
    result = client.verify(transaction_id='FT123', amount=100, provider='cbe')
except ShegerPayError as e:
    print(f"Error: {e.error_code}")
    print(f"Message: {e.message}")
    print(f"Fix: {e.suggestion}")
```

---

## 📚 Documentation

| Doc                                                        | Description                    |
| ---------------------------------------------------------- | ------------------------------ |
| [API Reference](https://shegerpay.com/docs/api)            | Complete API documentation     |
| [Integration Guide](https://shegerpay.com/docs/quickstart) | 5-minute quick start           |
| [Webhook Guide](https://shegerpay.com/docs/webhooks)       | Set up real-time notifications |
| [Security Guide](https://shegerpay.com/docs/security)      | Best practices                 |

---

## 🆘 Support

- 📖 [Documentation](https://shegerpay.com/docs)
- 💬 [Telegram](https://t.me/shegerpay0)
- 📧 [support@shegerpay.com](mailto:support@shegerpay.com)
- 🐛 [GitHub Issues](https://github.com/black12-ag/ShegerPay/issues)

---

## 📄 License

MIT License - see [LICENSE](LICENSE) for details.

---

**Made with ❤️ by ShegerPay**
