# Network Solutions DNS Setup for GitHub Pages

This repo is already configured to publish with GitHub Pages from GitHub Actions.

Current GitHub Pages configuration:

- Repository: `teebuphilip/randbalgolabs.com`
- Pages build type: `workflow`
- Current custom domain in GitHub Pages: `randbalgolabs.com`
- Expected behavior after DNS is correct:
  - `randbalgolabs.com` serves the site
  - `www.randbalgolabs.com` redirects to `randbalgolabs.com`

## What to change in Network Solutions

Log in to Network Solutions and go to:

1. `Domains`
2. Select `randbalgolabs.com`
3. `Advanced Tools`
4. `Manage` next to `Advanced DNS Records`

Then make these DNS records match the table below.

## Required DNS records

### Apex domain

Create or keep these four `A` records for `@`:

| Type | Host / Refers to | Points to | TTL |
| --- | --- | --- | --- |
| `A` | `@` | `185.199.108.153` | default or `7200` |
| `A` | `@` | `185.199.109.153` | default or `7200` |
| `A` | `@` | `185.199.110.153` | default or `7200` |
| `A` | `@` | `185.199.111.153` | default or `7200` |

### WWW subdomain

Create or update this `CNAME` record:

| Type | Host / Refers to | Alias to | TTL |
| --- | --- | --- | --- |
| `CNAME` | `www` | `teebuphilip.github.io` | default or `7200` |

## Important cleanup

- Remove any old `A`, `CNAME`, URL forwarding, parking, or under-construction records for `@` or `www` that conflict with the records above.
- Do not use wildcard records like `*`.
- If Network Solutions has a parking page, forwarding rule, or temporary website record enabled for this domain, disable it.

## Exact steps in Network Solutions

### Add or update the apex `A` records

1. In `Advanced DNS Records`, click `+ Add Record`.
2. Choose record type `A`.
3. For `Refers to`, choose `@`.
4. Enter one GitHub Pages IP in `Points to`.
5. Leave TTL at the default, or set `7200`.
6. Save it.
7. Repeat until all four GitHub Pages IPs exist.

If there are existing `A` records for `@` that point anywhere else, edit or delete them first.

### Add or update the `www` CNAME

1. In `Advanced DNS Records`, click `+ Add Record`.
2. Choose record type `CNAME`.
3. For `Refers to`, choose `www`.
4. In `Alias to`, enter `teebuphilip.github.io`
5. Leave TTL at the default, or set `7200`.
6. Save it.

If a `www` `A` record already exists, remove it before adding the `CNAME`.

## What should happen after this

Once DNS propagates:

- `randbalgolabs.com` should resolve to GitHub Pages
- `www.randbalgolabs.com` should resolve to GitHub Pages and redirect to `randbalgolabs.com`
- GitHub should issue the TLS certificate automatically
- HTTPS may take up to about an hour after DNS is correct

## How to verify

Run these commands locally:

```bash
dig randbalgolabs.com +short
dig www.randbalgolabs.com CNAME +short
dig www.randbalgolabs.com +short
```

Expected results:

- `randbalgolabs.com` returns the GitHub Pages IPs:
  - `185.199.108.153`
  - `185.199.109.153`
  - `185.199.110.153`
  - `185.199.111.153`
- `www.randbalgolabs.com` returns `teebuphilip.github.io`

Then test in a browser:

- `http://randbalgolabs.com`
- `http://www.randbalgolabs.com`
- `https://randbalgolabs.com`
- `https://www.randbalgolabs.com`

## If you want `www` to be canonical instead

This repo is currently configured with `randbalgolabs.com` as the GitHub Pages custom domain. That means `www` should redirect to the apex domain.

If you want the public site to live at `https://www.randbalgolabs.com/` and redirect the apex domain to `www`, the GitHub Pages custom domain should be changed from `randbalgolabs.com` to `www.randbalgolabs.com`.

## Sources

- GitHub Docs: <https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site>
- GitHub Docs: <https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/troubleshooting-custom-domains-and-github-pages>
- Network Solutions: <https://www.networksolutions.com/help/article/manage-dns-adns-records>
