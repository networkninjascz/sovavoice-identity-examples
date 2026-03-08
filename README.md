# SovaVoice — Getting Started

This guide walks you through creating an account, logging in, and generating an API key to access the SovaVoice Speech-to-Text API.

> **Windows PowerShell:** Use `curl.exe` instead of `curl` (PowerShell's built-in `curl` is an alias for `Invoke-WebRequest` and has different syntax).

---

## Step 1 — Register

Send your email address to create an account. You will receive a verification email.

```bash
curl -X POST https://auth.sovavoice.com/auth/register \
  -H "Content-Type: application/json" \
  -d '{"email": "you@example.com"}'
```

**Response**
```json
{ "message": "Registration successful. Please check your email." }
```

---

## Step 2 — Verify your email

Click the link in the verification email. It will redirect you to the dashboard where you can set your password.

> If you prefer to use the API directly, the verification link returns a `setup_token` that you can use in Step 3.

<details>
<summary>Set password via API</summary>

The verification endpoint returns a `setup_token`:

```bash
curl https://auth.sovavoice.com/auth/verify/YOUR_VERIFICATION_TOKEN
```

```json
{ "setup_token": "abc123..." }
```

Use it to set your password:

```bash
curl -X POST https://auth.sovavoice.com/auth/setup-password/YOUR_SETUP_TOKEN \
  -H "Content-Type: application/json" \
  -d '{"password": "your-secure-password"}'
```

</details>

---

## Step 3 — Log in

```bash
curl -X POST https://auth.sovavoice.com/auth/login \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "username=you@example.com&password=your-secure-password"
```

**Response**
```json
{
  "access_token": "eyJhbGci...",
  "token_type": "bearer"
}
```

Save the `access_token` — you'll need it for the next step.

---

## Step 4 — Create an API key

Use the JWT from Step 3 to create a persistent API key for the STT service.

```bash
curl -X POST https://api.sovavoice.com/api/keys \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"name": "My App"}'
```

**Response**
```json
{
  "message": "API key created successfully",
  "key": "stt_live_abc123..."
}
```

> **Important:** Copy the key now — it is shown only once.

---

## Done

You now have an API key (`stt_live_...`). Use it to [transcribe audio](../sovavoice-stt-examples).

---

## Managing API keys

<details>
<summary>List all API keys</summary>

```bash
curl https://api.sovavoice.com/api/keys/list \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

</details>

<details>
<summary>Deactivate an API key</summary>

```bash
curl -X DELETE https://api.sovavoice.com/api/keys/KEY_ID \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

</details>

<details>
<summary>Restore a deactivated key</summary>

```bash
curl -X POST https://api.sovavoice.com/api/keys/KEY_ID/restore \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

</details>

---

## Password reset

<details>
<summary>Forgot your password?</summary>

```bash
curl -X POST https://auth.sovavoice.com/auth/forgot-password \
  -H "Content-Type: application/json" \
  -d '{"email": "you@example.com"}'
```

You will receive a reset link by email. Then:

```bash
curl -X POST https://auth.sovavoice.com/auth/reset-password/YOUR_RESET_TOKEN \
  -H "Content-Type: application/json" \
  -d '{"password": "new-secure-password"}'
```

</details>
