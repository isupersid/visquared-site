# ViSquared public site

Static legal and support site for **STB for webOS** (`com.visquared.app.stbwebos`), published at [visquared.org](https://visquared.org).

The site is dependency-free HTML and CSS. Stable pages are available at:

- `/`
- `/privacy/`
- `/terms/`
- `/refunds/`
- `/support/`

GitHub Actions deploys the repository root to GitHub Pages whenever `main` is updated.

## Deployment status

Status checked 14 September 2026:

- [x] GitHub Pages is enabled with the custom domain `visquared.org`.
- [x] Cloudflare has all four GitHub Pages apex `A` records and all four `AAAA` records.
- [x] `www` is a `CNAME` to `isupersid.github.io`.
- [x] Cloudflare Email Routing has active MX, SPF, and DKIM records.
- [x] `support@visquared.org` routes to a verified destination mailbox.
- [ ] GitHub Pages is still provisioning the TLS certificate. Enable **Enforce HTTPS** after the certificate is available.

## Local preview

From the repository root:

```powershell
python -m http.server 8080
```

Open <http://localhost:8080>. Because navigation uses root-relative links for the custom domain, preview from the server root rather than opening HTML files directly.

## Cloudflare DNS setup

These records are configured in the Cloudflare zone for `visquared.org`. Keep this section as the reference configuration.

### Apex records

The following records for `@` are active. Keep them **DNS only** (grey cloud), especially while GitHub provisions and verifies the TLS certificate.

| Type | Name | Content |
|---|---|---|
| A | `@` | `185.199.108.153` |
| A | `@` | `185.199.109.153` |
| A | `@` | `185.199.110.153` |
| A | `@` | `185.199.111.153` |
| AAAA | `@` | `2606:50c0:8000::153` |
| AAAA | `@` | `2606:50c0:8001::153` |
| AAAA | `@` | `2606:50c0:8002::153` |
| AAAA | `@` | `2606:50c0:8003::153` |

Remove conflicting `A`, `AAAA`, or `CNAME` records for the apex. Keep unrelated verification and email records.

### `www` redirect

The `www` record is configured as a `CNAME` to `isupersid.github.io`. GitHub Pages treats `visquared.org` as the canonical custom domain and redirects `www` requests to it. Keep the apex GitHub Pages records DNS-only.

### Email routing

Cloudflare Email Routing is active with its required MX, SPF, and DKIM records. The custom address `support@visquared.org` routes to a verified destination mailbox. Keep the destination private and test inbound delivery before each production launch.

Email Routing forwards inbound mail only. Configure an authorised outbound mail provider separately if replies must come from `support@visquared.org`; publish that provider's SPF/DKIM/DMARC records without placing credentials in this repository.

## Launch checklist

- Wait for GitHub Pages to finish provisioning the TLS certificate, then enable **Enforce HTTPS**.
- Test every route on desktop and mobile, including keyboard navigation and visible focus.
- Verify the $5.99 USD per-TV price, Stripe Managed Payments, explicit first-run telemetry consent, English-only global 1.0 release, support, and refund statements against the released app and production providers.
- Confirm `support@visquared.org` continues to receive messages and that security-tagged reports reach the right inbox.
- **Obtain independent legal review of the privacy policy, terms, and refund policy before launch.** This repository provides operational copy, not legal advice.
