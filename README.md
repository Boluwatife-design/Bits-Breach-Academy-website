# Bits&Breach Academy website

A one-page website for **Bits&Breach Academy**, an IT and cybersecurity training academy in Nigeria. It is hosted for free on Cloudflare and uses a custom `.org.ng` domain with free email forwarding.

**Live site:** https://treasures.org.ng

## What the academy teaches

- PC Hardware and System Maintenance
- Microsoft 365 Administration
- Cybersecurity and Ethical Hacking
- IT Infrastructure and Cloud Labs

## What is in this repo

| File | Purpose |
|------|---------|
| `index.html` | The entire site: HTML, CSS and a small amount of JavaScript in one file |
| `README.md` | This guide |

## Site features

- Single self-contained HTML file with no build step and no dependencies
- Responsive layout that works on phones
- Automatic light and dark mode (follows the visitor's device setting)
- Keyboard focus styles and reduced-motion support
- Enquiry form that opens the visitor's email app with the details filled in

## Architecture

```
Visitor -> treasures.org.ng -> Cloudflare DNS -> Cloudflare Worker (static assets) -> index.html
Email   -> hello@treasures.org.ng -> Cloudflare Email Routing -> personal Gmail inbox
```

Total running cost: the domain registration only. Hosting, DNS, HTTPS and email forwarding are on Cloudflare's free tier.

## How it was built and deployed

### 1. Design and build the page
The page was drafted with an AI assistant (Claude), then edited by hand to match the academy's real course list. Changes after the first draft were made by editing the HTML directly.

### 2. Host the site on Cloudflare
1. In the Cloudflare dashboard, go to **Workers & Pages** and create a new project.
2. Choose **Upload assets** (direct upload).
3. Put `index.html` at the top level of a folder, upload that folder and deploy.
4. Open the `workers.dev` address to confirm the page loads.

### 3. Connect the custom domain
1. The domain's nameservers were already pointed to Cloudflare through the registrar.
2. In the worker, go to **Settings**, then **Domains & Routes**, and add the domain.
3. Optionally disable the `workers.dev` route so the site is only reachable on the custom domain.

### 4. Set up free email forwarding
1. In Cloudflare, open **Email Routing** and onboard/choose the domain.
<img width="1258" height="934" alt="Screenshot 2026-11-06 135827" src="https://github.com/user-attachments/assets/c050beec-b2d6-4fb1-8671-f0ab2b4039e1" />

2. Add a **destination address** (the real inbox) and click the verification link sent to it.
<img width="1878" height="774" alt="Screenshot 2026-11-06 133512" src="https://github.com/user-attachments/assets/c05798b1-5f74-4ac7-b3a5-fc51cd4924dc" />

3. Under **Routing rules**, create `hello@<domain>` and forward it to the verified address.
<img width="1759" height="932" alt="Screenshot 2026-11-06 140107" src="https://github.com/user-attachments/assets/da51d33f-9f6c-4998-bf25-bf55c7a73e44" />
<img width="1532" height="932" alt="Screenshot 2026-11-06 140501" src="https://github.com/user-attachments/assets/704f2bd8-566b-4fe9-a14a-f081a9ce2b79" />

4. Send a test email from a different account to confirm it arrives.

Email Routing only receives and forwards. Replies come from the personal inbox unless a separate sending service is set up.

## Problems hit and how they were fixed

| Problem | Cause | Fix |
|---------|-------|-----|
| Cloudflare refused to connect the domain: "already has externally managed DNS records" | An old A or CNAME record for the root domain was still in the DNS zone | Deleted the old A, AAAA or CNAME record for the root domain. MX and TXT records were left alone |
| `workers.dev` address showed HTTP 404 | The uploaded files did not have `index.html` at the top level | Re-uploaded a folder with `index.html` directly inside it |
| Destination address stuck on **Pending** | The verification email had not been clicked | Opened the email (checked spam too) and clicked the link |
| Email Routing conflicted with the old Microsoft 365 setup | A domain can only deliver mail to one service, so old Microsoft MX records clash with Cloudflare's | Let Cloudflare replace the MX records. The Microsoft records can be restored if Microsoft 365 is renewed |


## Lessons learned
- A custom domain needs hosting before it shows a website. The domain itself is only an address.
- Free tiers on Cloudflare are enough for a small business site with email forwarding.
- Switching mail providers means replacing MX and SPF records, and only one mail service can be active at a time.
- Screenshot DNS records before deleting them so they can be restored.

## Updating the site

1. Edit `index.html`.
2. Upload the folder again as a new version in the Cloudflare worker and deploy.

## Contact

Enquiries: hello@treasures.org.ng
