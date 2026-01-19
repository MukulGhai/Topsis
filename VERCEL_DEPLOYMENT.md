# Deploy to Vercel - Step by Step

## ⚠️ Important Note

Vercel is optimized for **Node.js/Next.js**. Flask works but with limitations:
- ❌ Can't persist files (uploads folder resets)
- ⚠️ Cold start delays (first request slower)
- ✅ Email feature works
- ✅ Webpage display works

**Recommendation**: Use **Railway** instead (see DEPLOYMENT_GUIDE.md)

---

## If You Still Want Vercel...

### Step 1: Update requirements.txt

Add Gunicorn and Werkzeug:

```
Flask==2.3.3
pandas==2.0.3
numpy==1.24.3
python-dotenv==1.0.0
openpyxl==3.1.2
gunicorn==21.2.0
werkzeug==2.3.7
```

### Step 2: Create vercel.json

Create file `vercel.json` in your project root:

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
  ]
}
```

### Step 3: Create .vercelignore

Create file `.vercelignore`:

```
.git
.gitignore
.env
.env.local
.env.*.local
node_modules/
venv/
__pycache__/
*.pyc
.DS_Store
README.md
SETUP_GUIDE.md
...other docs
```

### Step 4: Modify app.py

Change the last lines:

```python
if __name__ == "__main__":
    import os
    port = int(os.environ.get('PORT', 5000))
    debug = os.environ.get('FLASK_ENV') != 'production'
    app.run(host='0.0.0.0', port=port, debug=debug)
```

### Step 5: Create Procfile

Create file `Procfile`:

```
web: gunicorn app:app
```

### Step 6: Prepare for GitHub

Create `.gitignore`:

```
__pycache__/
*.pyc
*.pyo
.env
.env.local
venv/
.DS_Store
uploads/*
!uploads/.gitkeep
*.egg-info/
```

### Step 7: Push to GitHub

```bash
# Initialize git
git init

# Add files
git add .

# Commit
git commit -m "Deploy to Vercel"

# Create GitHub repo and push
git remote add origin https://github.com/YOUR_USERNAME/topsis-web.git
git branch -M main
git push -u origin main
```

### Step 8: Deploy on Vercel

#### Option A: Using Vercel Dashboard (Easiest)

1. Go to https://vercel.com
2. Sign up with GitHub
3. Click "Import Project"
4. Select your `topsis-web` repository
5. Click "Import"
6. Configure environment variables (see below)
7. Click "Deploy"

#### Option B: Using Vercel CLI

```bash
# Install Vercel CLI
npm install -g vercel

# Deploy from project folder
vercel --prod
```

### Step 9: Set Environment Variables

**In Vercel Dashboard:**

1. Go to your project
2. Click "Settings"
3. Go to "Environment Variables"
4. Add these variables:
   ```
   SENDER_EMAIL = your_email@gmail.com
   APP_PASSWORD = your_16_char_app_password
   ```
5. Redeploy project for changes to take effect

**Or via CLI:**
```bash
vercel env add SENDER_EMAIL
vercel env add APP_PASSWORD
vercel --prod
```

### Step 10: Redeploy

After adding environment variables, redeploy:

```bash
vercel --prod
```

---

## Testing on Vercel

### 1. Test Webpage Display
```
1. Open your Vercel URL
2. Upload sample_data.csv
3. Enter weights: 0.5,0.3,0.2
4. Enter impacts: -,+,+
5. Select "View on Webpage"
6. Click "Analyze"
7. Results appear ✓
```

### 2. Test Email Delivery
```
1. Same as above but select "Send via Email"
2. Enter your email
3. Click "Analyze"
4. Check email inbox ✓
```

### 3. Test File Download
```
1. After results display
2. Click "⬇️ Download CSV"
3. File should download ✓
```

---

## Vercel Limitations & Solutions

### Problem: Uploads Folder Gets Deleted
**Why**: Vercel resets filesystem on each deploy
**Solution**: Use "View on Webpage" option (results shown, no file storage needed)

### Problem: Cold Start Delays
**Why**: First request takes 5-10 seconds
**Solution**: Normal for serverless. Use Railway for faster response times.

### Problem: File Uploads Fail
**Why**: Limited filesystem access
**Solution**: Only use "View on Webpage" option

### Problem: Email Not Sending
**Why**: Environment variables not set
**Solution**: Follow Step 9 above carefully

---

## Vercel CLI Commands

```bash
# Login to Vercel
vercel login

# Deploy to staging
vercel

# Deploy to production
vercel --prod

# Check deployment status
vercel status

# View logs
vercel logs

# Add environment variable
vercel env add VARIABLE_NAME

# Remove environment variable
vercel env rm VARIABLE_NAME

# Redeploy
vercel --prod
```

---

## Full Example Deployment

### Complete Steps (Copy-Paste)

```bash
# 1. Navigate to project
cd C:\Users\Mukul Ghai\Desktop\Topsis-web

# 2. Update requirements.txt
# (Add: gunicorn==21.2.0, werkzeug==2.3.7)

# 3. Create necessary files
# (Create: vercel.json, .vercelignore, Procfile)

# 4. Update app.py
# (Change app.run line as shown above)

# 5. Initialize git
git init
git add .
git commit -m "Deploy to Vercel"

# 6. Create repo on GitHub
# (Go to github.com/new, create repo)

# 7. Push to GitHub
git remote add origin https://github.com/YOUR_USERNAME/topsis-web.git
git branch -M main
git push -u origin main

# 8. Install Vercel CLI (if not already installed)
npm install -g vercel

# 9. Login to Vercel
vercel login

# 10. Deploy
vercel --prod

# 11. Add environment variables
vercel env add SENDER_EMAIL
vercel env add APP_PASSWORD

# 12. Redeploy
vercel --prod
```

---

## Your Vercel URL

After deployment, you'll get a URL like:
```
https://topsis-web-theta.vercel.app
```

Share this URL to let others use your app!

---

## Vercel vs Railway Comparison

| Feature | Vercel | Railway |
|---------|--------|---------|
| Setup | 15 min | 10 min |
| File uploads | ❌ No | ✅ Yes |
| Email | ✅ Yes | ✅ Yes |
| Cold start | Slow | Fast |
| Cost | Free | Free |
| Python support | Basic | Full |
| Recommendation | ⭐⭐ | ⭐⭐⭐⭐⭐ |

---

## Troubleshooting Vercel

### Deployment Fails
```
Error: Check vercel.json syntax
Error: Check requirements.txt has all packages
Error: Check for syntax errors in app.py
```

### Email Not Working
```
Check: SENDER_EMAIL environment variable set
Check: APP_PASSWORD is 16 characters
Check: Redeploy after adding variables
```

### File Upload Fails
```
Note: Vercel doesn't persist files
Solution: Use "View on Webpage" only
```

### Getting Logs
```bash
vercel logs
```

### Revert Deployment
```bash
vercel rollback
```

---

## When to Use Vercel for This App

✅ Good choice if:
- You want simplicity
- Email delivery is important
- You don't need file persistence
- You're familiar with Vercel

❌ Better to use Railway if:
- You want better performance
- You might add file features later
- You want faster cold starts
- You want simpler deployment

---

## Vercel Alternative Recommendations

If Vercel doesn't work well:

1. **Railway** (Recommended)
   - Better for Flask
   - Faster deployments
   - File persistence

2. **Render**
   - Good free tier
   - Fast deployments
   - Easy to use

3. **PythonAnywhere**
   - Built for Python
   - Beginner friendly
   - Good documentation

---

## Support

If you run into issues:
1. Check Vercel logs: `vercel logs`
2. Verify environment variables are set
3. Redeploy: `vercel --prod`
4. Check GitHub repo is up to date

---

## Summary

### Quick Vercel Deployment
```
1. Update requirements.txt (add gunicorn, werkzeug)
2. Create vercel.json, .vercelignore, Procfile
3. Push to GitHub
4. Deploy via Vercel dashboard or CLI
5. Add environment variables
6. Redeploy
7. Done! 🎉
```

### Estimated Time: **15-20 minutes**

### Final Note
Vercel works for this app, but Railway is simpler and better optimized for Flask. Consider Railway as first choice.

---

**Need Help?** See DEPLOYMENT_GUIDE.md for Railway instructions (recommended)
