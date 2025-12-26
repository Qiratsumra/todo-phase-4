# Deployment Guide - Vercel & Render

This guide will help you deploy the frontend to Vercel and ensure proper backend connectivity.

## Issues Fixed

### 1. **ERR_CONNECTION_REFUSED on /api/auth/get-session**
   - **Root Cause**: `BETTER_AUTH_URL` was hardcoded to `localhost:3000` in `.env`
   - **Fix**: Dynamic base URL detection in `lib/auth.ts` and `lib/auth-client.ts`
   - **Files Modified**:
     - `frontend/lib/auth.ts:6-18,28`
     - `frontend/lib/auth-client.ts:4-12,15-16`

### 2. **404 Error on /api/tasks**
   - **Root Cause**: Backend might not be deployed or routes not properly configured
   - **Fix**: Verify backend is running on Render and accessible

### 3. **Missing Vercel Configuration**
   - **Root Cause**: No `vercel.json` for environment variables and CORS headers
   - **Fix**: Created `frontend/vercel.json` with proper configuration

---

## Frontend Deployment to Vercel

### Step 1: Configure Environment Variables in Vercel

1. Go to your Vercel project dashboard
2. Navigate to **Settings > Environment Variables**
3. Add the following variables:

| Variable Name | Value | Environment |
|---------------|-------|-------------|
| `BETTER_AUTH_SECRET` | `gv1MpYLkbiYKZDKEeRYJoZgG1CDuo5Dm` | Production, Preview, Development |
| `DATABASE_URL` | `postgresql://qiratsaeed:W5diFNGiIBj8L3ziFtZkaM19QLCuX0AM@dpg-d50f873e5dus73ddl1eg-a.oregon-postgres.render.com/todo_phase3_backend` | Production, Preview, Development |
| `NEXT_PUBLIC_API_URL` | `https://todo-phase-3-backend.onrender.com` | Production, Preview, Development |
| `NEXT_PUBLIC_OPENAI_DOMAIN_KEY` | `domain_pk_694104eaa09481969c6d28efd1de536704827a7214e55e06` | Production, Preview, Development |
| `PGSSLMODE` | `require` | Production, Preview, Development |
| `NODE_TLS_REJECT_UNAUTHORIZED` | `0` | Production, Preview, Development |
| `NODE_ENV` | `production` | Production |

**Note**: Do NOT add `BETTER_AUTH_URL` - it will be automatically detected using `VERCEL_URL` (built-in Vercel variable)

### Step 2: Deploy from Git

1. Push your code to GitHub:
```bash
git add .
git commit -m "Fix Vercel deployment configuration"
git push origin main
```

2. In Vercel Dashboard:
   - Click **"Deploy"** or wait for automatic deployment
   - Vercel will automatically detect the Next.js framework
   - Build command: `npm run build` (default)
   - Output directory: `.next` (default)

### Step 3: Verify Deployment

After deployment completes:

1. Check the deployment URL (e.g., `https://your-app.vercel.app`)
2. Test authentication endpoints:
   - Should call `https://your-app.vercel.app/api/auth/get-session` (not localhost)
3. Test tasks API:
   - Should call `https://todo-phase-3-backend.onrender.com/api/tasks`

---

## Backend Verification on Render

### Check Backend Status

1. Visit your Render dashboard: https://dashboard.render.com
2. Find your backend service: `todo-phase-3-backend`
3. Check the service status (should be "Live")
4. Check recent logs for errors

### Test Backend Endpoints

Test these endpoints directly in your browser or with curl:

```bash
# Health check
curl https://todo-phase-3-backend.onrender.com/health

# Tasks API
curl https://todo-phase-3-backend.onrender.com/api/tasks

# Expected response for health: {"status":"ok","service":"todo-backend"}
```

### Common Backend Issues

If you get 404 errors:

1. **Service Not Running**:
   - Check Render dashboard for service status
   - Review deploy logs for errors
   - Ensure `main.py` is the entry point

2. **Wrong Base URL**:
   - Verify the service URL in Render dashboard
   - Update `NEXT_PUBLIC_API_URL` in Vercel if different

3. **Database Connection**:
   - Check if PostgreSQL database is connected in Render
   - Verify `SQLALCHEMY_DATABASE_URL` environment variable in backend

---

## CORS Configuration

The backend (`backend/main.py:29-36`) is configured to allow:
- All Vercel preview URLs: `https://*.vercel.app`
- Local development: `http://localhost:*`

If you're still getting CORS errors:

1. Check backend logs in Render for origin being rejected
2. Verify the regex pattern in `main.py:31`
3. Ensure credentials are enabled (`allow_credentials=True`)

---

## Troubleshooting

### Still Getting localhost:3000 Errors?

1. **Clear Vercel build cache**:
   - In Vercel dashboard: Settings > Clear Build Cache
   - Redeploy

2. **Check environment variables are set**:
   - Vercel Dashboard > Settings > Environment Variables
   - Ensure no `BETTER_AUTH_URL=http://localhost:3000` override

3. **Check browser console**:
   ```javascript
   // Should see the correct Vercel URL
   console.log(window.location.origin)
   ```

### Tasks API 404 Error?

1. **Verify backend is accessible**:
```bash
curl https://todo-phase-3-backend.onrender.com/api/tasks
```

2. **Check backend logs in Render**:
   - Look for routing errors
   - Verify `app.include_router(api_router, prefix="/api")` in main.py:98

3. **Check CORS headers**:
   - Backend should return `Access-Control-Allow-Origin` header
   - Check with: `curl -I https://todo-phase-3-backend.onrender.com/api/tasks`

---

## Files Changed

### Created:
- `frontend/.env.production` - Production environment variables
- `frontend/vercel.json` - Vercel deployment configuration
- `DEPLOYMENT.md` - This guide

### Modified:
- `frontend/lib/auth.ts` - Dynamic base URL detection
- `frontend/lib/auth-client.ts` - Client-side dynamic base URL

---

## Environment Variables Reference

### Frontend (.env)
```bash
# Development (localhost)
BETTER_AUTH_URL=http://localhost:3000
NEXT_PUBLIC_API_URL=https://todo-phase-3-backend.onrender.com

# Production (Vercel) - DO NOT SET BETTER_AUTH_URL
# It will auto-detect from VERCEL_URL
```

### Backend (Render)
Verify these are set in Render dashboard:
```bash
SQLALCHEMY_DATABASE_URL=postgresql://...
GEMINI_API_KEY=your-key
BACKEND_CORS_ORIGINS=https://*.vercel.app,http://localhost:*
```

---

## Post-Deployment Checklist

- [ ] Vercel deployment successful
- [ ] No localhost:3000 errors in browser console
- [ ] Auth endpoints resolve to Vercel domain
- [ ] Tasks API returns data (not 404)
- [ ] Backend is "Live" on Render
- [ ] CORS headers present in responses
- [ ] Can create/update/delete tasks
- [ ] Chat functionality works with backend

---

## Need Help?

- **Vercel Docs**: https://vercel.com/docs
- **Render Docs**: https://render.com/docs
- **Better Auth**: https://www.better-auth.com/docs
- **Check logs**: Vercel Functions tab or Render Logs tab
