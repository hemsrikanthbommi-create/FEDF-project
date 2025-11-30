# Deployment Guide - FEDF Project (DarkMeet)

This guide provides comprehensive instructions for deploying the DarkMeet video conferencing platform.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Environment Variables](#environment-variables)
- [GitHub Actions Setup](#github-actions-setup)
- [Vercel Deployment](#vercel-deployment)
- [Alternative Deployment Options](#alternative-deployment-options)
- [Post-Deployment Checklist](#post-deployment-checklist)

## Prerequisites

Before deploying, ensure you have:

1. **External Service Accounts**:
   - [Supabase](https://supabase.com) - Database and authentication
   - [LiveKit](https://livekit.cloud) - Real-time video infrastructure
   - [Google Cloud](https://console.cloud.google.com) - Gemini API for AI features
   - [Vercel](https://vercel.com) (optional) - Hosting platform

2. **Required Access**:
   - GitHub repository admin access
   - Vercel account (for GitHub deployment)

## Environment Variables

### Supabase Configuration

```env
NEXT_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_anon_key_here
SUPABASE_SERVICE_ROLE_KEY=your_service_role_key_here
```

To get these values:
1. Go to Supabase Project Settings
2. Navigate to "API" section
3. Copy your Project URL and keys
4. Run the `supabase-schema.sql` file in your Supabase SQL editor

### LiveKit Configuration

```env
NEXT_PUBLIC_LIVEKIT_URL=wss://your-livekit-server.com
LIVEKIT_API_KEY=your_api_key
LIVEKIT_API_SECRET=your_api_secret
```

To get LiveKit credentials:
1. Sign up at [livekit.cloud](https://livekit.cloud)
2. Create a new project
3. Get API credentials from project settings

### Google Gemini Configuration

```env
GEMINI_API_KEY=your_gemini_api_key
```

To get the API key:
1. Go to [Google AI Studio](https://makersuite.google.com/app/apikey)
2. Create a new API key
3. Save it securely

## GitHub Actions Setup

### 1. Configure GitHub Secrets

In your GitHub repository:

1. Go to **Settings → Secrets and variables → Actions**
2. Add the following secrets:
   - `NEXT_PUBLIC_SUPABASE_URL`
   - `NEXT_PUBLIC_SUPABASE_ANON_KEY`
   - `SUPABASE_SERVICE_ROLE_KEY` (if needed for backend)
   - `NEXT_PUBLIC_LIVEKIT_URL`
   - `LIVEKIT_API_KEY`
   - `LIVEKIT_API_SECRET`
   - `GEMINI_API_KEY`
   - `VERCEL_TOKEN` (for Vercel deployment)
   - `VERCEL_ORG_ID` (for Vercel deployment)
   - `VERCEL_PROJECT_ID` (for Vercel deployment)

### 2. Workflow Triggers

The GitHub Actions workflow (`.github/workflows/deploy.yml`) triggers on:
- Push to `main` branch
- Pull requests to `main` branch

### 3. Build and Test

The workflow automatically:
1. Checks out the code
2. Sets up Node.js 18 and 20
3. Installs dependencies
4. Runs linter
5. Type-checks the code
6. Builds the Next.js project
7. Uploads build artifacts
8. (On main push) Deploys to Vercel

## Vercel Deployment

### Method 1: Connect via GitHub (Recommended)

1. Go to [Vercel Dashboard](https://vercel.com/dashboard)
2. Click **Add New → Project**
3. Select **Import Git Repository**
4. Choose `hemsrikanthbommi-create/FEDF-project`
5. Vercel auto-detects it's a Next.js project
6. Add environment variables in project settings
7. Click **Deploy**

### Method 2: Manual Setup

1. Install Vercel CLI:
   ```bash
   npm install -g vercel
   ```

2. Login to Vercel:
   ```bash
   vercel login
   ```

3. Deploy the project:
   ```bash
   vercel --prod
   ```

4. Follow the prompts to configure project settings

### Vercel Environment Variables

Add all environment variables from the [Environment Variables](#environment-variables) section to your Vercel project:

1. Go to Project Settings
2. Select **Environment Variables**
3. Add all required variables
4. Redeploy for changes to take effect

## Alternative Deployment Options

### Docker Deployment

1. Create a `Dockerfile`:
   ```dockerfile
   FROM node:20-alpine
   WORKDIR /app
   COPY package*.json ./
   RUN npm ci
   COPY . .
   RUN npm run build
   EXPOSE 3000
   CMD ["npm", "start"]
   ```

2. Build the image:
   ```bash
   docker build -t darkmeet .
   ```

3. Run the container:
   ```bash
   docker run -p 3000:3000 \
     -e NEXT_PUBLIC_SUPABASE_URL=your_url \
     -e NEXT_PUBLIC_SUPABASE_ANON_KEY=your_key \
     darkmeet
   ```

### AWS Deployment

1. Build the project:
   ```bash
   npm run build
   ```

2. Use Next.js standalone build:
   ```bash
   npm run build
   # Creates .next/standalone folder
   ```

3. Upload to AWS:
   - Upload to EC2 instance or Lambda
   - Configure environment variables
   - Start the application

### Self-Hosted Deployment

1. Build the project locally:
   ```bash
   npm install
   npm run build
   ```

2. Start production server:
   ```bash
   npm start
   ```

3. Configure a reverse proxy (nginx/Apache)
4. Enable SSL/TLS
5. Set up monitoring and logging

## Post-Deployment Checklist

- [ ] Verify application loads at deployed URL
- [ ] Test video conferencing functionality
- [ ] Verify Supabase database connection
- [ ] Test LiveKit room creation
- [ ] Confirm Gemini AI features working
- [ ] Check SSL certificate is valid
- [ ] Enable HTTPS redirect
- [ ] Configure custom domain (if needed)
- [ ] Setup monitoring/logging
- [ ] Configure automated backups
- [ ] Test authentication flows
- [ ] Verify responsive design on mobile
- [ ] Load test with multiple concurrent users
- [ ] Setup error tracking (Sentry, etc.)

## Troubleshooting

### Build Failures

**Issue**: `npm ERR! code ERESOLVE`
```bash
npm install --legacy-peer-deps
```

**Issue**: TypeScript errors
```bash
npm run type-check
# Fix errors listed
```

### Runtime Issues

**Supabase Connection Failed**:
- Verify `NEXT_PUBLIC_SUPABASE_URL` is correct
- Check API key is not expired
- Ensure database schema is initialized

**LiveKit Connection Failed**:
- Verify WebSocket URL format (should start with `wss://`)
- Check API credentials
- Ensure LiveKit server is running

**Gemini API Issues**:
- Verify API key is active
- Check API quota hasn't been exceeded
- Ensure correct API is enabled in Google Cloud

## Monitoring and Maintenance

### Health Checks

1. Monitor application uptime
2. Check error rates in logs
3. Monitor API response times
4. Track database performance
5. Monitor real-time video quality

### Updates and Patches

```bash
# Check for dependency updates
npm outdated

# Update dependencies
npm update

# Redeploy after updates
npm run build
```

## Support and Resources

- [Next.js Deployment Docs](https://nextjs.org/docs/deployment)
- [Supabase Documentation](https://supabase.com/docs)
- [LiveKit Documentation](https://docs.livekit.io)
- [Vercel Documentation](https://vercel.com/docs)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)