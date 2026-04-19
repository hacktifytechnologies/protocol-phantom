# Assessment — Protocol Phantom: Header Injection + Broken Access Control

---

## Question 1 — MCQ

**Why must the attacker visit `/dashboard` before exploiting the hidden admin route?**

- A) It initialises the database connection
- B) **It sets a `visited_dashboard` session flag required by the hidden endpoint's state gate** ✅
- C) It automatically applies the admin role header
- D) It prevents race conditions during token generation

> **Answer:** B — The hidden admin endpoint checks for this session flag. Without it, the exploit is rejected regardless of the headers provided.

---

## Question 2 — MCQ

**Which HTTP header bypasses the IP restriction on the administrative endpoint?**

- A) `X-Origin-IP: 127.0.0.1`
- B) `Forwarded: for=127.0.0.1`
- C) **`X-Forwarded-For: 127.0.0.1`** ✅
- D) `Host: localhost`

> **Answer:** C — The `/hidden-admin` endpoint trusts this user-controlled header to determine the requester's IP, allowing any client to spoof a localhost address.

---

## Question 3 — MCQ

**The double-spend vulnerability in `/transfer` falls under which OWASP 2021 category?**

- A) A01 — Broken Access Control
- B) A03 — Injection
- C) **A04 — Insecure Design** ✅
- D) A06 — Vulnerable and Outdated Components

> **Answer:** C — The TOCTOU (Time-of-Check-Time-of-Use) race condition between balance check and deduction is an architectural flaw classified as Insecure Design.

---

## Question 4 — Fill in the Blank

**What HTTP header value must be set to spoof a localhost IP and bypass the network restriction on the hidden admin endpoint?**

**Answer:** `X-Forwarded-For: 127.0.0.1`

> The backend trusts the `X-Forwarded-For` header to determine the requester's source IP without verifying it originated from a trusted proxy. Setting it to `127.0.0.1` makes the application treat the request as coming from localhost, bypassing the network-level IP restriction.

---

## Question 5 — Fill in the Blank

**What HTTP header injects the admin role into the request to gain administrative privileges on the hidden endpoint?**

**Answer:** `X-Role: admin`

> The application trusts the client-supplied `X-Role` header to determine user permissions server-side. Setting `X-Role: admin` alongside the spoofed `X-Forwarded-For` header causes the server to grant full administrative access with no credential verification.

---

*Lab target:* `http://localhost:5000`
