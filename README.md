# Kubernets_configMaps-secret
# ConfigMap vs Secret

| Feature | ConfigMap | Secret |
|----------|-----------|--------|
| Stores Configuration | ✅ | ❌ |
| Stores Passwords | ❌ | ✅ |
| Stores API Keys | ❌ | ✅ |
| Base64 Encoded | ❌ | ✅ |
| Environment Variables | ✅ | ✅ |
| Volume Mount | ✅ | ✅ |

---

# Interview Points

## ConfigMap

- Stores non-sensitive configuration.
- Plain text.
- Used for application configuration.
- Example: Hostname, Port, Environment.

## Secret

- Stores sensitive information.
- Base64 encoded (using the `data` field).
- Used for Passwords, API Keys, Tokens, Certificates.
- Base64 is **encoding**, **not encryption**.

---
