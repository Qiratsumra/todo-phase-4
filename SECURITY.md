# Security Policy

## 🚨 IMPORTANT SECURITY NOTICE

### Exposed Secrets in Git History

**⚠️ WARNING:** This repository previously contained `.env` files with real API keys and database credentials committed to git history.

**Exposed secrets include:**
- GEMINI_API_KEY
- BETTER_AUTH_SECRET
- DATABASE_URL with credentials
- NEXT_PUBLIC_OPENAI_DOMAIN_KEY

### Immediate Actions Taken

1. ✅ Created `.env.example` templates for both backend and frontend
2. ✅ Created `.env.local` files for local development with safe defaults
3. ✅ Added this security documentation

### Actions Required

#### For Repository Maintainers:

1. **Rotate All Exposed Secrets Immediately:**
   - Generate new GEMINI_API_KEY from Google AI Studio
   - Generate new BETTER_AUTH_SECRET (use `openssl rand -base64 32`)
   - Update database passwords
   - Revoke NEXT_PUBLIC_OPENAI_DOMAIN_KEY if applicable

2. **Remove Secrets from Git History:**
   ```bash
   # Option 1: Using git filter-branch
   git filter-branch --force --index-filter \
     "git rm --cached --ignore-unmatch backend/.env frontend/.env" \
     --prune-empty --tag-name-filter cat -- --all

   # Option 2: Using BFG Repo-Cleaner (recommended, faster)
   bfg --delete-files backend/.env
   bfg --delete-files frontend/.env
   git reflog expire --expire=now --all && git gc --prune=now --aggressive
   ```

3. **Force Push to Remote:**
   ```bash
   git push origin --force --all
   git push origin --force --tags
   ```

4. **Verify .gitignore:**
   Ensure `.env` files are in `.gitignore`:
   ```
   .env
   .env.local
   .env.*.local
   ```

#### For New Developers:

1. **Copy environment templates:**
   ```bash
   # Backend
   cp backend/.env.example backend/.env
   # Edit backend/.env with your actual values

   # Frontend
   cp frontend/.env.example frontend/.env.local
   # Edit frontend/.env.local with your actual values
   ```

2. **Never commit .env files:**
   - `.env` files are in `.gitignore`
   - Always use `.env.example` for templates
   - Keep secrets secure and never share publicly

## Supported Versions

| Version | Supported          |
| ------- | ------------------ |
| 1.0.x   | :white_check_mark: |

## Reporting a Vulnerability

If you discover a security vulnerability, please email the maintainers directly.

**Do not** create a public GitHub issue for security vulnerabilities.

### Response Time

- Initial response: Within 48 hours
- Status update: Within 7 days
- Fix timeline: Depends on severity (critical issues within 24-48 hours)

## Security Best Practices

### For Development

1. **Environment Variables:**
   - Use `.env.local` for local development
   - Never commit `.env` files
   - Use different secrets for dev/staging/production

2. **API Keys:**
   - Rotate keys regularly (every 90 days minimum)
   - Use different keys per environment
   - Monitor API usage for anomalies

3. **Database:**
   - Use strong passwords (min 16 characters, mixed case, special chars)
   - Enable SSL/TLS for database connections
   - Restrict database access to specific IPs

4. **CORS:**
   - Never use `allow_origins=["*"]` in production
   - Specify exact frontend domains
   - Use environment-based configuration

### For Production

1. **Secrets Management:**
   - Use Kubernetes Secrets or external secret managers (AWS Secrets Manager, HashiCorp Vault)
   - Enable encryption at rest for etcd
   - Implement secret rotation

2. **TLS/HTTPS:**
   - Enable TLS for all services
   - Use cert-manager for certificate management
   - Enforce HTTPS redirects

3. **Authentication:**
   - Implement JWT validation middleware
   - Verify user identity on every request
   - Use short-lived tokens (15-60 minutes)

4. **Network Security:**
   - Implement Kubernetes Network Policies
   - Restrict pod-to-pod communication
   - Use service meshes for advanced security (Istio, Linkerd)

5. **Monitoring:**
   - Enable audit logging
   - Monitor for suspicious activity
   - Set up alerts for security events

## Known Vulnerabilities

### V-001: Authentication Bypass (CRITICAL)
**Status:** 🔴 Open
**Severity:** Critical
**Description:** Backend API does not validate JWT tokens or verify user ownership
**Impact:** Any user can access any other user's data by changing the user_id in URL
**Fix:** Implement JWT validation middleware (PR pending)

### V-002: CORS Wildcard (MEDIUM)
**Status:** 🟠 Open
**Severity:** Medium
**Description:** Backend allows requests from any origin (`allow_origins=["*"]`)
**Impact:** CSRF vulnerability, unauthorized API access
**Fix:** Configure specific allowed origins (PR pending)

### V-003: No TLS/HTTPS (MEDIUM)
**Status:** 🟠 Open
**Severity:** Medium
**Description:** All traffic is unencrypted
**Impact:** Man-in-the-middle attacks, credential exposure
**Fix:** Implement Ingress with TLS termination (roadmap Q1)

## Security Checklist

### Before Going to Production

- [ ] All secrets rotated and secured
- [ ] .env files removed from git history
- [ ] Authentication middleware implemented
- [ ] CORS properly configured
- [ ] TLS/HTTPS enabled
- [ ] Network Policies defined
- [ ] Database encryption at rest enabled
- [ ] Audit logging enabled
- [ ] Monitoring and alerting configured
- [ ] Security scanning integrated in CI/CD
- [ ] Penetration testing completed
- [ ] Security review documented

---

**Last Updated:** 2025-12-26
**Next Review:** 2026-01-26
