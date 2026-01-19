# Quick Deployment Guide

## Choose Your Platform

### ⭐ Recommended: Railway (Easiest)
- 10 minutes setup
- Best for Flask
- Free tier included
- Go to: https://railway.app

### 🚀 Also Good: Render
- 10 minutes setup
- Good free tier
- Flask friendly
- Go to: https://render.com

### 🔷 If You Insist: Vercel
- 20 minutes setup
- Limited Flask support
- Already configured
- Go to: https://vercel.com

---

## Fastest Path (Railway)

### 1. GitHub Setup (5 min)
```bash
# In your project folder
git init
git add .
git commit -m "TOPSIS web app"

# Create repo on GitHub.com, then:
git remote add origin https://github.com/YOUR_USERNAME/topsis-web.git
git branch -M main
git push -u origin main
```

### 2. Railway Deploy (2 min)
1. Go to https://railway.app
2. "New Project" → "Deploy from GitHub"
3. Select `topsis-web` repo
4. Done! It's deploying...

### 3. Set Environment (2 min)
1. In Railway: Variables tab
2. Add `SENDER_EMAIL` and `APP_PASSWORD`
3. Auto-redeploys

### 4. Test (1 min)
- Copy your Railway URL
- Upload sample CSV
- Click "View on Webpage"
- Results appear! ✓

**Total Time: 10 minutes**
**Cost: $0**

---

## For Vercel Users

Already configured for you! Just:

```bash
# 1. Create GitHub repo (see above)

# 2. Push code
git push origin main

# 3. Go to https://vercel.com
# 4. "Import Project" → Select your repo
# 5. Add environment variables
# 6. Deploy

# Or use Vercel CLI
vercel --prod
```

See `VERCEL_DEPLOYMENT.md` for detailed steps

---

## Environment Variables

**For Email Feature** (Optional):

1. Get Gmail app password:
   - https://myaccount.google.com/apppasswords
   - Select Mail + Windows Computer
   - Copy 16-character password

2. Set variables:
   ```
   SENDER_EMAIL=your_email@gmail.com
   APP_PASSWORD=16_char_password_here
   ```

3. If no email:
   - Just use "View on Webpage" option
   - Works perfectly without email

---

## Testing Your Deployment

```
1. Open your app URL
2. Upload sample_data.csv
3. Weights: 0.5,0.3,0.2
4. Impacts: -,+,+
5. Select "View on Webpage"
6. Click "Analyze"
7. See results ✓
```

---

## Your App is Live!

Share your URL:
```
https://your-app-name.railway.app
or
https://topsis-web.vercel.app
```

---

## Need Help?

- **Railway**: See DEPLOYMENT_GUIDE.md (Option 1)
- **Render**: See DEPLOYMENT_GUIDE.md (Option 2)
- **Vercel**: See VERCEL_DEPLOYMENT.md

---

## Deployment Files Added

✅ `vercel.json` - Vercel configuration
✅ `.vercelignore` - Files to ignore
✅ `Procfile` - Process file
✅ `.gitignore` - Git configuration
✅ `requirements.txt` - Updated with Gunicorn

Everything is ready to deploy!

---

**Recommended**: Railway + GitHub + Gmail
**Setup Time**: 10 minutes
**Cost**: Free
**Difficulty**: Easy

Let's go! 🚀
