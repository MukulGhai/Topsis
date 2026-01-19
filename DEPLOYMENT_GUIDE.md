# Deployment Guide - TOPSIS Web Service

## 🚀 Hosting Options Comparison

| Platform | Best For | Cost | Setup Time |
|----------|----------|------|-----------|
| **Vercel** | Next.js, Node.js | Free tier available | 5 min |
| **Railway** | Flask, Python | Pay-as-you-go | 5 min |
| **Render** | Flask, Python | Free tier | 10 min |
| **Heroku** | Flask, Python | Paid only | 10 min |
| **PythonAnywhere** | Python web apps | Free tier | 10 min |

---

## Option 1: Deploy to Railway (Recommended for Flask) ✅

**Why Railway?** Native Python support, simple deployment, free tier

### Step 1: Prepare Your App

1. Create `.gitignore` file:
```
__pycache__/
*.pyc
*.pyo
*.egg-info/
.env
uploads/*
!uploads/.gitkeep
venv/
.DS_Store
```

2. Create `uploads/.gitkeep` file (to keep folder in git):
```
(empty file)
```

3. Update `app.py` for production:
```python
if __name__ == "__main__":
    import os
    port = int(os.environ.get('PORT', 5000))
    app.run(host='0.0.0.0', port=port, debug=False)
```

### Step 2: Push to GitHub

```bash
# Initialize git repository
git init
git add .
git commit -m "Initial commit"

# Create repo on GitHub and push
git remote add origin https://github.com/YOUR_USERNAME/topsis-web.git
git branch -M main
git push -u origin main
```

### Step 3: Deploy on Railway

1. Go to https://railway.app
2. Click "Create New Project"
3. Select "Deploy from GitHub repo"
4. Authorize and select your `topsis-web` repository
5. Railway auto-detects Python and installs dependencies

### Step 4: Configure Environment Variables

In Railway Dashboard:
1. Go to your project
2. Click "Variables"
3. Add:
   ```
   SENDER_EMAIL=your_email@gmail.com
   APP_PASSWORD=your_16_char_password
   ```

### Step 5: Deploy

Railway automatically deploys on push. Wait for "Running" status.

**Your app URL**: `https://your-app-name.railway.app`

---

## Option 2: Deploy to Render ✅

**Why Render?** Free tier, automatic deployments, good for Flask

### Step 1: Prepare (Same as Railway)

Update `app.py`:
```python
if __name__ == "__main__":
    import os
    port = int(os.environ.get('PORT', 5000))
    app.run(host='0.0.0.0', port=port, debug=False)
```

### Step 2: Push to GitHub

(Same as Railway steps above)

### Step 3: Deploy on Render

1. Go to https://render.com
2. Click "New+" → "Web Service"
3. Connect your GitHub account
4. Select `topsis-web` repository
5. Fill in details:
   - **Name**: topsis-web
   - **Environment**: Python
   - **Build Command**: `pip install -r requirements.txt`
   - **Start Command**: `gunicorn app:app`

6. Click "Create Web Service"

### Step 4: Add Environment Variables

In Render Dashboard:
1. Go to your service
2. Click "Environment"
3. Add:
   ```
   SENDER_EMAIL=your_email@gmail.com
   APP_PASSWORD=your_16_char_password
   ```

4. Render auto-deploys with new env variables

**Your app URL**: `https://topsis-web.onrender.com`

---

## Option 3: Deploy to Vercel (Advanced)

**Note**: Vercel is optimized for Node.js. Flask support is limited.

### Step 1: Install Vercel CLI

```bash
npm install -g vercel
```

### Step 2: Create `vercel.json`

Create file `vercel.json` in project root:

```json
{
  "builds": [
    {
      "src": "app.py",
      "use": "@vercel/python"
    }
  ],
  "routes": [
    {
      "src": "/(.*)",
      "dest": "app.py"
    }
  ],
  "env": {
    "SENDER_EMAIL": "@sender_email",
    "APP_PASSWORD": "@app_password"
  }
}
```

### Step 3: Update `requirements.txt`

Add Gunicorn:
```
Flask==2.3.3
pandas==2.0.3
numpy==1.24.3
python-dotenv==1.0.0
openpyxl==3.1.2
gunicorn==21.2.0
```

### Step 4: Modify `app.py`

Update the last lines:
```python
if __name__ == "__main__":
    import os
    port = int(os.environ.get('PORT', 5000))
    app.run(host='0.0.0.0', port=port, debug=False)
```

### Step 5: Push to GitHub

```bash
git init
git add .
git commit -m "Add Vercel config"
git push
```

### Step 6: Deploy

```bash
vercel --prod
```

Or connect GitHub repo on https://vercel.com for auto-deployment.

---

## Option 4: Deploy to PythonAnywhere

**Free tier available** for basic deployments

### Step 1: Create Account

Go to https://www.pythonanywhere.com

### Step 2: Upload Files

1. Go to "Files" tab
2. Upload your project files

### Step 3: Create Virtual Environment

In "Bash" console:
```bash
mkvirtualenv topsis
pip install -r requirements.txt
```

### Step 4: Configure Web App

1. Go to "Web" tab
2. Add new web app → Python
3. Select Flask
4. Create virtualenv → `/home/username/topsis`

### Step 5: Edit WSGI File

The file path shows under "Web" tab. Edit it:
```python
import sys
path = '/home/username/mysite'
if path not in sys.path:
    sys.path.append(path)

from app import app as application
```

### Step 6: Set Environment Variables

Edit `.env` file with your email credentials

### Step 7: Reload

Click "Reload" in Web tab

**Your app URL**: `https://username.pythonanywhere.com`

---

## Step-by-Step for Beginners (Railway Recommended)

### 1. Create GitHub Account
- Go to https://github.com/signup
- Create account and verify email

### 2. Create Repository
- Click "+" → "New repository"
- Name: `topsis-web`
- Description: "TOPSIS Decision Making Analysis Tool"
- Make it **Public**
- Click "Create repository"

### 3. Push Code

From your project folder:
```bash
# Initialize git
git init

# Add all files
git add .

# Create initial commit
git commit -m "Initial TOPSIS web service"

# Add remote
git remote add origin https://github.com/YOUR_USERNAME/topsis-web.git

# Push to GitHub
git branch -M main
git push -u origin main
```

### 4. Deploy to Railway
- Go to https://railway.app
- Click "New Project"
- Select "Deploy from GitHub repo"
- Authorize GitHub
- Select `topsis-web`
- Railway auto-detects Python
- Wait for deployment

### 5. Add Environment Variables
- In Railway: Project → Variables
- Add `SENDER_EMAIL` and `APP_PASSWORD`
- Railway redeploys automatically

### 6. Get Your URL
- Copy the generated URL
- Share with others
- Your app is live! 🎉

---

## Environment Variables Guide

### For Email Feature

**Option A: Using Gmail**
1. Go to https://myaccount.google.com/security
2. Enable 2-Step Verification
3. Go to https://myaccount.google.com/apppasswords
4. Select "Mail" and "Windows Computer"
5. Copy the 16-character password
6. Set environment variables:
   ```
   SENDER_EMAIL=your_email@gmail.com
   APP_PASSWORD=generated_16_char_password
   ```

**Option B: Skip Email**
- Don't set email variables
- App still works with "View on Webpage" option
- Email option just won't work

---

## Testing After Deployment

### Test Webpage Display (No Email Needed)
1. Go to your deployed app URL
2. Upload `sample_data.csv`
3. Enter weights: `0.5,0.3,0.2`
4. Enter impacts: `-,+,+`
5. Select "View on Webpage"
6. Click "Analyze"
7. Results should appear immediately ✓

### Test Email Delivery (With Email Setup)
1. Same as above but select "Send via Email"
2. Enter your email address
3. Click "Analyze"
4. Check email inbox
5. Result CSV should arrive ✓

---

## Troubleshooting

### App Won't Deploy
- Check `requirements.txt` has correct packages
- Verify Python version compatible
- Check for syntax errors in `app.py`

### Email Not Working
- Verify `SENDER_EMAIL` is correct format
- Verify `APP_PASSWORD` is 16 characters (from app passwords)
- Check email is enabled in environment variables
- Try "View on Webpage" option first

### Static Files Not Loading
- Templates folder must be named `templates`
- Make sure `index.html` is inside `templates`
- Clear browser cache

### Port Issues
- Make sure `app.run(host='0.0.0.0', port=port)` is set
- Don't hardcode port to 5000 in production

### Uploads Folder Error
- Create `uploads` folder
- Create empty `.gitkeep` file inside
- Commit to git

---

## Production Checklist

Before deploying:
- ✅ Update `app.py` with `host='0.0.0.0'`
- ✅ Set `debug=False` in production
- ✅ Update `requirements.txt` with all packages
- ✅ Create `.gitignore` to exclude uploads and `.env`
- ✅ Push code to GitHub
- ✅ Set environment variables on platform
- ✅ Test "View on Webpage" feature
- ✅ Test email feature (if configured)
- ✅ Verify file uploads work
- ✅ Test CSV download

---

## Custom Domain (Optional)

### Railway
1. Project → Settings → Domain
2. Connect custom domain or use `railway.app` subdomain

### Render
1. Environment → Domain
2. Add your domain or use free `onrender.com` subdomain

### Vercel
1. Project Settings → Domains
2. Add custom domain

---

## Estimated Costs

### Free Options
- **Railway**: Free tier includes $5 credit/month
- **Render**: Free tier for static sites + limited Python
- **PythonAnywhere**: Free tier available
- **Vercel**: Free for Next.js/Node.js

### Paid Options (If Needed)
- **Railway**: ~$5-10/month for small app
- **Render**: ~$7/month for paid tier
- **Heroku**: Discontinued free tier (paid plans start at $7)

---

## Recommended Setup (Best Experience)

1. **Use Railway** (simplest for Flask)
2. **Host on GitHub** (free code backup)
3. **Use Gmail for emails** (free, reliable)
4. **Free tier** of Railway (includes $5 credit)

**Total Cost**: $0 per month ✅

---

## Summary

| Platform | Setup Time | Cost | Recommendation |
|----------|-----------|------|---|
| Railway | 10 min | Free | ⭐⭐⭐⭐⭐ Best |
| Render | 10 min | Free | ⭐⭐⭐⭐ Good |
| PythonAnywhere | 15 min | Free | ⭐⭐⭐ OK |
| Vercel | 20 min | Free | ⭐⭐ Limited |

**Recommended: Deploy to Railway in 10 minutes with zero cost!**

---

## Next Steps

1. Choose a platform (Railway recommended)
2. Create GitHub account
3. Push code to GitHub
4. Deploy using platform
5. Add environment variables
6. Test the app
7. Share your URL!

Good luck! 🚀
