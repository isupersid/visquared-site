# ViSquared public site

Static legal and support site for **STB for webOS** (`com.visquared.app.stbwebos`), published at [visquared.org](https://visquared.org).

The site is dependency-free HTML and CSS. Stable pages are available at:

- `/`
- `/privacy/`
- `/terms/`
- `/refunds/`
- `/support/`

GitHub Actions deploys the repository root to GitHub Pages whenever `main` is updated.

## Local preview

From the repository root:

```powershell
python -m http.server 8080
```

Open <http://localhost:8080>. Because navigation uses root-relative links for the custom domain, preview from the server root rather than opening HTML files directly.

## Cloudflare DNS setup

Complete these steps in the Cloudflare zone for `visquared.org` after GitHub Pages is enabled with the custom domain.

### Apex records

Create the following records for `@`. Use **DNS only** (grey cloud), especially while GitHub provisions and verifies the TLS certificate.

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

1. Create a proxied `CNAME` record: name `www`, target `visquared.org`.
2. In **Rules > Redirect Rules**, create a single redirect where the hostname equals `www.visquared.org`.
3. Use a `301` redirect to `https://visquared.org` while preserving the request path and query string. A dynamic target expression can concatenate `https://visquared.org` with `http.request.uri.path`; enable query-string preservation.

The proxied `www` record is used only for Cloudflare's redirect. Keep the apex GitHub Pages records DNS-only.

### Email routing

1. Open **Email > Email Routing** and enable Email Routing for the zone.
2. Add and verify a destination mailbox controlled by ViSquared.
3. Create the custom address `support@visquared.org` and route it to that verified destination.
4. Accept the Cloudflare-suggested routing DNS records, including its required MX records and SPF TXT record.
5. Remove or reconcile any conflicting MX/SPF records first. Keep exactly one SPF policy for the apex.
6. Send a test message to `support@visquared.org` and verify it arrives before launch.

Email Routing forwards inbound mail only. Configure an authorised outbound mail provider separately if replies must come from `support@visquared.org`; publish that provider's SPF/DKIM/DMARC records without placing credentials in this repository.

## Launch checklist

- Confirm the Pages custom domain is `visquared.org`, DNS validation succeeds, and **Enforce HTTPS** is enabled.
- Test every route on desktop and mobile, including keyboard navigation and visible focus.
- Verify the $5.99 USD per-TV price, Stripe Managed Payments, explicit first-run telemetry consent, English-only global 1.0 release, support, and refund statements against the released app and production providers.
- Confirm `support@visquared.org` can receive messages and that security-tagged reports reach the right inbox.
- **Obtain independent legal review of the privacy policy, terms, and refund policy before launch.** This repository provides operational copy, not legal advice.
