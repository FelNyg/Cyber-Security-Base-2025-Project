# Cyber-Security-Base-2025-Project

## User info

| User  | Password  | admin    |
|:-----:|:---------:|:--------:|
| admin | admin     | yes      |
| jm    | matmatmat | no       |
| mj    | drickadricka | no    |

## Vulnerabilities

I have made flaws in code in purpose and then commented how they should be fixed. 

## 1. Fault

A6:2017 - Security Misconfiguration

# What’s the issue?
Having DEBUG = True in production is risky — it shows full error details, including stack traces and potentially sensitive data. If attackers see this, they could use it to exploit the app.

# How to fix it:
Set DEBUG = False in production. This way, users only see generic error pages, and you keep technical details hidden. Always separate development and production settings.

The code can be found in `mysite/settings.py (line 26)`


## 2. Fault

A3:2017 – Sensitive Data Exposure

# What’s the issue?
The setting SECURE_SSL_REDIRECT is not enabled or doesn’t exist. This means users can access the site over plain HTTP, which doesn’t encrypt the traffic. That puts sensitive data like login credentials at risk.

# How to fix it:
Set SECURE_SSL_REDIRECT = True in production. This forces all traffic over HTTPS, ensuring data is encrypted in transit.

The code can be found in `mysite/settings.py (line 131)`

## 3. Fault

A10:2017 – Insufficient Logging & Monitoring

# What’s the issue?
The application doesn’t log security-related events. Without logging, attacks like repeated login failures or unexpected behavior can go undetected, making it hard to respond to threats or investigate issues.

# How to fix it:
Enable Django’s security logging by configuring the LOGGING setting. This will record security warnings to a file and help monitor potential attacks.

The code can be found in mysite/settings.py (line 136)

## 4. Fault



# Fault Explanation:




# Fix Explanation:



## 5. Fault



# Fault Explanation:


# Fix Explanation:
