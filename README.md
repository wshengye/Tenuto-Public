# Tenuto public site

Static pages hosted on **GitHub Pages**. They back the auth email flow and hold
the legal docs the App Store listing points to.

| File | Purpose |
|------|---------|
| `index.html` | Simple landing page |
| `confirmed.html` | Where the **email-confirmation** link lands ("Email confirmed ✓") |
| `reset.html` | Where the **password-reset** link lands — sets the new password |
| `terms.html` / `privacy.html` | Legal docs (edit the `[DATE]` / `[BUSINESS NAME]` / `[CONTACT EMAIL]` placeholders) |
| `styles.css` | Shared styling (Tenuto tokens) |

## 1. Host on GitHub Pages
Put these files at the **root** of the public repo (`Tenuto-Public`), then
Settings → Pages → Build from `main` / root. Your base URL becomes
`https://wshengye.github.io/Tenuto-Public/`.

## 2. Fill in `reset.html`
Edit the two constants near the bottom with your project values (both are
public — the anon key already ships in the mobile app):

```js
const SUPABASE_URL = 'https://YOUR-PROJECT-REF.supabase.co'
const SUPABASE_ANON_KEY = 'YOUR-ANON-KEY'
```

## 3. Point the app at this site
In the app's `.env`:

```
EXPO_PUBLIC_WEB_URL=https://wshengye.github.io/Tenuto-Public
```

That feeds `constants/links.ts`, which drives the auth redirect targets and the
in-app Terms/Privacy links. Rebuild after changing env.

## 4. Supabase dashboard
**Authentication → URL Configuration**
- **Site URL:** `https://wshengye.github.io/Tenuto-Public`
- **Redirect URLs (allow-list):** add
  `https://wshengye.github.io/Tenuto-Public/confirmed.html` and
  `https://wshengye.github.io/Tenuto-Public/reset.html`

**Authentication → Providers → Email:** "Confirm email" = ON.

**Email Templates:** the *default link-based* templates are fine here — keep
`{{ .ConfirmationURL }}` (no `{{ .Token }}` needed, since this flow uses links,
not codes).

## 5. Email delivery (Resend SMTP)
Supabase composes the auth emails; Resend delivers them. In **Resend**: verify a
sending domain and create an API key (Sending access). Then in **Supabase →
Authentication → Emails → SMTP Settings**, enable Custom SMTP:

```
Sender email: tenuto@<your-verified-domain>   (domain MUST be the Resend-verified one)
Sender name:  Tenuto
Host: smtp.resend.com   Port: 465
Username: resend        Password: <the re_… API key>
```

Raise **Authentication → Rate Limits → Emails per hour** to suit (Resend free
tier caps at 100/day, 3,000/month). The #1 failure is a sender domain that
doesn't match the verified Resend domain → 403.

## How it flows
- **Sign up** → confirmation email → tap link → `confirmed.html` → return to app, sign in.
- **Forgot password** (in app) → reset email → tap link → `reset.html` sets the new
  password → return to app, sign in.
