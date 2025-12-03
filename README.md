# MTA-STS Policy for before6.com

This repository hosts the Mail Transfer Agent Strict Transport Security (MTA-STS) policy file for the domain `before6.com`.

The purpose of MTA-STS is to tell receiving mail servers that our email should only be delivered over a secure, encrypted (TLS) connection.

---

## 1. The Policy File

The policy file is located at `/.well-known/mta-sts.txt` and is served via GitHub Pages.

**Live Policy File URL:** [https://mta-sts.before6.com/.well-known/mta-sts.txt](https://mta-sts.before6.com/.well-known/mta-sts.txt)

The current policy is set to `testing` mode:
version: STSv1
mode: testing
mx: *.mail.protection.outlook.com
max_age: 86400

---

## 2. DNS Records (Configured at GoDaddy)

The following DNS records were created to enable MTA-STS and TLS reporting.

### CNAME Record for GitHub Pages

This record points the `mta-sts` subdomain to the GitHub Pages server.

*   **Type:** `CNAME`
*   **Name:** `mta-sts`
*   **Value:** `before6-maui.github.io.`

### TXT Record for MTA-STS Policy Discovery

This record signals that we have an active MTA-STS policy.

*   **Type:** `TXT`
*   **Name:** `_mta-sts`
*   **Value:** `v=STSv1; id=202512030007`

### TXT Record for TLS Reporting (TLS-RPT)

This record tells mail servers where to send reports about email delivery successes and failures. These reports are sent to EasyDMARC.

*   **Type:** `TXT`
*   **Name:** `_smtp._tls`
*   **Value:** `v=TLSRPTv1; rua=mailto:ba3ab62aec@rua.easydmarc.us`

---

## 3. Maintenance Plan

1.  **Monitor:** For the first 1-2 weeks, monitor TLS-RPT reports in the EasyDMARC dashboard.
2.  **Enforce:** Once confident that there are no delivery issues, update the `mta-sts.txt` file in this repository and change the mode from `testing` to `enforce`.
3.  **Update DNS:** After updating the policy file, the `id` in the `_mta-sts` TXT record **must be changed** to a new, unique value (like the current date) to signal to servers that they need to fetch the new policy.
