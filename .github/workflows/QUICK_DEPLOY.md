# 🚀 FEDF-Project Quick Deployment Guide

**Status**: ✅ Deployment infrastructure is READY

This guide provides the fastest path to deploying DarkMeet.

## ⚡ 5-Minute Setup to Production

### 1. Create Service Accounts (5 minutes)

**Supabase**:
- Visit https://supabase.com
- Click "New Project"
- Save these credentials:
  ```
  NEXT_PUBLIC_SUPABASE_URL=...
  NEXT_PUBLIC_SUPABASE_ANON_KEY=...
  ```

**LiveKit**:
- Visit https://livekit.cloud
- Create new room
- Get API key & secret:
  ```
  NEXT_PUBLIC_LIVEKIT_URL=wss://...
  LIVEKIT_API_KEY=...
  LIVEKIT_API_SECRET=...
  ```

**Google Gemini**:
- Visit https://makersuite.google.com/app/apikey
- Create new API key:
  ```
  GEMINI_API_KEY=...
  ```

### 2. Add GitHub Secrets (2 minutes)

**Go to**: https://github.com/hemsrikanthbommi-create/FEDF-project/settings/secrets/actions

**Click**: "New repository secret"

**Add these secrets** (copy-paste from step 1):
1. `NEXT_PUBLIC_SUPABASE_URL`
2. `NEXT_PUBLIC_SUPABASE_ANON_KEY`
3. `NEXT_PUBLIC_LIVEKIT_URL`
4. `LIVEKIT_API_KEY`
5. `LIVEKIT_API_SECRET`
6. `GEMINI_API_KEY`
7. `VERCEL_TOKEN` (get from https://vercel.com/account/tokens)
8. `VERCEL_ORG_ID` (visible in Vercel dashboard)
9. `VERCEL_PROJECT_ID` (after creating Vercel project)

### 3. Connect to Vercel (1 minute)

**Option A: Automatic (Easiest)**
1. Visit https://vercel.com/new
2. Click "Import Git Repository"
3. Select `hemsrikanthbommi-create/FEDF-project`
4. Click "Deploy"

**Option B: Manual CLI**
```bash
npm install -g vercel
vercel login
vercel --prod
```

### 4. Set Vercel Environment Variables (1 minute)

In Vercel Dashboard:
1. Go to Settings → Environment Variables
2. Add all 6 environment variables from Step 1
3. Click Redeploy

### 5. Verify Deployment (1 minute)

- ✅ Check Vercel dashboard for "Ready" status
- ✅ Visit your deployment URL
- ✅ Test video conferencing
- ✅ Check AI features work

## 📋 What's Already Done

- ✅ GitHub Actions CI/CD workflow configured
- ✅ Next.js build optimized
- ✅ TypeScript type checking enabled
- ✅ Automatic deployment on push to main
- ✅ Build artifacts caching
- ✅ Comprehensive documentation

## 🔍 Verify Each Service

### Test Supabase Connection
```javascript
import { createClient } from '@supabase/supabase-js'

const supabase = createClient(
  process.env.NEXT_PUBLIC_SUPABASE_URL,
  process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY
)

// Check connection
await supabase.auth.getSession()
```

### Test LiveKit Connection
```javascript
import { LiveKitRoom } from '@livekit/components-react'

// Will fail if URL/token invalid
```

### Test Gemini Integration
```javascript
import { GoogleGenerativeAI } from '@google/generative-ai'

const genAI = new GoogleGenerativeAI(
  process.env.GEMINI_API_KEY
)
```

## ❌ Troubleshooting

**Deployment fails with "missing secrets"**
- Ensure all 9 GitHub secrets are added
- Wait 5 seconds after adding secrets before retrying
- Check secret names exactly match (case-sensitive)

**Application loads but video doesn't work**
- Verify `NEXT_PUBLIC_LIVEKIT_URL` format: `wss://...`
- Check LiveKit API credentials in Vercel
- Ensure room is created in LiveKit dashboard

**AI features not working**
- Test `GEMINI_API_KEY` is active
- Check API quota in Google Cloud Console
- Verify Generative AI API is enabled

**Database connection failed**
- Verify Supabase URL doesn't have trailing slash
- Check anon key is not expired
- Run `supabase-schema.sql` in Supabase SQL editor

## 📚 Full Documentation

For advanced options, see `docs/DEPLOYMENT.md`:
- Docker deployment
- AWS setup
- Self-hosted guide
- Monitoring setup
- CI/CD customization

## 🎯 Success Criteria

Your deployment is successful when:

- [ ] Vercel shows "Ready" status
- [ ] Can access app at deployment URL
- [ ] Can create video rooms
- [ ] Video/audio works between participants
- [ ] AI features respond
- [ ] Database queries work

## 🆘 Need Help?

1. Check GitHub Actions logs: https://github.com/hemsrikanthbommi-create/FEDF-project/actions
2. Review Vercel deployment logs
3. Test each service independently
4. See `docs/DEPLOYMENT.md` for detailed troubleshooting

## ⏱️ Estimated Time

- Service account creation: 5 minutes
- GitHub secrets setup: 2 minutes
- Vercel connection: 1 minute
- Environment variables: 1 minute
- Verification: 1 minute

**Total: ~10 minutes to production** 🚀