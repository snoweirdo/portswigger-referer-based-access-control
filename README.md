# PortSwigger: Referer-Based Access Control

A writeup for the PortSwigger Web Security Academy Apprentice lab:

**Lab: Referer-based access control**

## Overview

This lab demonstrates a broken access-control vulnerability caused by relying on the HTTP `Referer` header to protect administrative functionality.

The objective was to log in as the low-privileged user `wiener` and exploit the flawed access controls to promote the account to administrator.

## Vulnerability

- Vulnerability type: Broken access control
- Attack type: Privilege escalation
- Weak control: Referer-based authorization
- Tool used: Burp Suite

## Credentials

```text
Administrator:
Username: administrator
Password: admin

Low-privileged user:
Username: wiener
Password: peter
```

## TL;DR

1. Logged in as the administrator.
2. Opened the admin panel.
3. Upgraded `carlos` and captured the request in Burp Suite.
4. Sent the request to Burp Repeater.
5. Logged in as `wiener`.
6. Captured a request from Wiener and sent it to Repeater.
7. Copied Wiener’s session cookie.
8. Replaced the administrator session cookie in the role-change request.
9. Changed the target username from `carlos` to `wiener`.
10. Sent the modified request.
11. The lab was solved because Wiener was promoted to administrator.

## Example Request

```http
GET /admin-roles?username=wiener&action=upgrade HTTP/2
Host: <lab-id>.web-security-academy.net
Cookie: session=<wiener-session>
Referer: https://<lab-id>.web-security-academy.net/admin
```

The exact request path or headers may vary depending on the lab instance.

## Key Takeaway

The application failed to verify the authenticated user's actual role and relied on request context instead. A `Referer` header is client-controlled and must never be used as an authorization mechanism.

## Remediation

- Enforce authorization using the authenticated session on the server.
- Verify that the current user has administrator privileges before changing roles.
- Do not trust the `Referer` header for access control.
- Protect role-changing actions with CSRF protections.
- Log and monitor administrative role changes.
- Test privileged endpoints with both administrator and non-administrator sessions.

## Disclaimer

This writeup is for educational purposes and applies only to the authorized PortSwigger Web Security Academy lab environment.
