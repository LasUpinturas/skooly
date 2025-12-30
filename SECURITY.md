# 🛡️ Security Policy

## Supported Versions

Only the latest `dev` and `main` branches are currently supported.

| Version | Supported          |
| ------- | ------------------ |
| `dev`   | :white_check_mark: |
| `main`  | :white_check_mark: |
| < 1.0   | :x:                |

## Reporting a Vulnerability

We take security seriously. If you discover a vulnerability:

1.  **DO NOT** open a public GitHub issue.
2.  Send an email immediately to **security@skooly.io**.
3.  Include:
    *   Description of the flaw.
    *   Steps to reproduce.
    *   Potential impact.

We will acknowledge your email within 48 hours.

## Policy

*   We practice **Responsible Disclosure**.
*   We commit to fixing critical vulnerabilities within 7 days.
*   We will properly credit you in the release notes (if you wish).

## Critical Areas (Pay Attention)

Since Skooly handles sensitive data (Grades, Payments, Identity), we are extra vigilant on:

*   **Payment Webhooks**: Ensure `HMAC` signature verification from MTN/Orange.
*   **Grade Modification**: Ensure strict audit logs for any `UPDATE` on `Grade` table.
*   **PII Leakage**: No student personal data in logs.
