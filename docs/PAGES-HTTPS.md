# HTTPS for blog.raminnietzsche.ir

Custom domain for this **user site** repo. Project docs (e.g. CVE-Radar) are served at `/CVE-Radar/` on the same host.

## Symptom

- `https://blog.raminnietzsche.ir/` → certificate is `*.github.io` (browser error)
- `https://raminnietzsche.github.io/` redirects to **`http://`** blog (Enforce HTTPS off)

## Required DNS (Arvan / any provider)

| Type | Name | Value |
|------|------|-------|
| CNAME | `blog` | `raminnietzsche.github.io` |

Do **not** add extra `A`/`AAAA` on `blog` when using CNAME. GitHub TXT challenge (optional check):

```bash
dig +short _github-pages-challenge-raminnietzsche.blog.raminnietzsche.ir TXT
```

## Enable HTTPS (GitHub UI)

1. [Settings → Pages](https://github.com/RaminNietzsche/raminnietzsche.github.io/settings/pages) → **Custom domain** = `blog.raminnietzsche.ir` → Save.
2. Wait until **DNS check** is green (up to 24 h after DNS changes).
3. When **Enforce HTTPS** is available, turn it on.

If the checkbox stays disabled:

1. Clear custom domain → Save → wait 5 min.
2. Re-enter `blog.raminnietzsche.ir` → Save.
3. Push to `main` (this repo) so `CNAME` is in the deployed artifact.
4. Re-check Enforce HTTPS after ~15 min.

## CVE-Radar project site

Do **not** set the same custom domain on [CVE-Radar Pages settings](https://github.com/RaminNietzsche/CVE-Radar/settings/pages). Only this user-site repo owns `blog.raminnietzsche.ir`.

## Verify

```bash
curl -sI https://blog.raminnietzsche.ir/CVE-Radar/ | head -5
openssl s_client -connect blog.raminnietzsche.ir:443 -servername blog.raminnietzsche.ir </dev/null 2>/dev/null | openssl x509 -noout -subject
```

Expected: `HTTP/2 200` and certificate subject containing `blog.raminnietzsche.ir`.
