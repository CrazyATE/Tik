# 🎯 TikTok Developer App Setup Guide

## Quick Answers to Your Questions:

### 1. Terms of Service & Privacy Policy URLs

I've created templates for you in the `legal/` folder! Here are your options:

#### Option A: Use GitHub Pages (Free & Easy)
1. **Push the legal folder to GitHub**:
   ```bash
   git add legal/
   git commit -m "Add legal documents"
   git push
   ```

2. **Enable GitHub Pages**:
   - Go to your repo Settings → Pages
   - Source: Deploy from branch (main)
   - Folder: / (root)

3. **Your URLs will be**:
   - Terms: `https://[your-github-username].github.io/Tik/legal/terms_of_service.html`
   - Privacy: `https://[your-github-username].github.io/Tik/legal/privacy_policy.html`

#### Option B: Use Free Hosting
- **Netlify Drop**: Drag the `legal` folder to [app.netlify.com/drop](https://app.netlify.com/drop)
- **Vercel**: Deploy with `vercel` command
- **Surge.sh**: Run `surge legal/`

#### Option C: Quick Placeholder (For Testing Only)
Use these generic URLs temporarily:
- Terms: `https://www.termsfeed.com/live/1ab1c651-5b25-43e4-8812-d7e8658a1dd3`
- Privacy: `https://www.privacypolicygenerator.info/live.php?token=example`

**Note**: Replace with real URLs before going live!

---

### 2. Which Platforms to Select

For TikTok Developer App, select:

✅ **Web** - Required for OAuth flow
✅ **Server/Backend** - For API calls
❌ iOS - Not needed
❌ Android - Not needed

**Explanation**:
- We're building a server-side application
- OAuth happens via web redirect
- No mobile app components

---

## 📝 Complete TikTok App Setup

### Step 1: Basic Information
```
App Name: Tik Sports Automation
Description: Automated sports content creation and posting system
Category: Entertainment & Sports
Website: https://github.com/[your-username]/Tik (or your domain)
```

### Step 2: Platforms
Select:
- [✓] Web
- [✓] Server/Backend

### Step 3: Legal URLs
```
Terms of Service URL: [Your hosted terms URL]
Privacy Policy URL: [Your hosted privacy URL]
```

### Step 4: Redirect URIs
Add these (update with your domain):
```
For local testing:
http://localhost:8000/auth/tiktok/callback

For production:
https://your-app.vercel.app/auth/tiktok/callback
https://your-domain.com/auth/tiktok/callback
```

### Step 5: Permissions/Scopes
Request these scopes:
- `user.info.basic` - Get user profile
- `video.list` - View your videos
- `video.upload` - Post videos (may require approval)

---

## ⚠️ Important Notes

### For Testing/Development
- TikTok allows **sandbox mode** for testing
- You can use basic scopes immediately
- Video upload may require app review

### App Review Process
- Basic scopes: Instant approval
- Video upload: 2-5 business days
- Business verification: May be required for some features

### Temporary Workaround
While waiting for approval, you can:
1. Use mock mode (current setup)
2. Generate content locally
3. Upload manually to TikTok

---

## 🔧 Quick Implementation

### 1. Create OAuth Handler
```python
# src/auth/tiktok_oauth.py
import os
from urllib.parse import urlencode

class TikTokOAuth:
    def __init__(self):
        self.client_id = os.getenv('TIK_TIKTOK__CLIENT_ID')
        self.client_secret = os.getenv('TIK_TIKTOK__CLIENT_SECRET')
        self.redirect_uri = 'http://localhost:8000/auth/tiktok/callback'

    def get_auth_url(self):
        params = {
            'client_id': self.client_id,
            'redirect_uri': self.redirect_uri,
            'response_type': 'code',
            'scope': 'user.info.basic,video.list'
        }
        base_url = 'https://www.tiktok.com/auth/authorize/'
        return f"{base_url}?{urlencode(params)}"

    def exchange_code_for_token(self, code):
        # Exchange authorization code for access token
        # Implementation here
        pass
```

### 2. Test Authentication Flow
```python
# test_tiktok_auth.py
from src.auth.tiktok_oauth import TikTokOAuth

oauth = TikTokOAuth()
auth_url = oauth.get_auth_url()
print(f"Visit this URL to authenticate: {auth_url}")
```

---

## 🎯 Next Steps

1. **Complete app registration** with the URLs provided
2. **Submit for review** if you need video upload
3. **Test in sandbox mode** while waiting
4. **Keep using mock mode** until approval

### What You Can Do Now:
- ✅ Test full system in mock mode
- ✅ Generate real content
- ✅ Save videos locally
- ✅ Track all metrics
- ⏳ Wait for TikTok approval
- 🔜 Switch from mock to live when ready

---

## 💡 Pro Tips

### 1. Start with Read-Only
Get approved for basic scopes first:
- View profile
- Read video metrics
- Analyze performance

### 2. Manual Upload Phase
While waiting for upload API:
- System generates content
- You manually upload
- System reads metrics back

### 3. Use TikTok Business Suite
Alternative approach:
- Generate content with our system
- Use TikTok's official scheduler
- Our system tracks performance

---

## 🚨 Common Issues

**"Invalid redirect URI"**
- Must match EXACTLY (including trailing slash)
- Use HTTPS for production
- Add all variations you'll use

**"App not approved"**
- Start with basic scopes
- Provide clear use case
- Show sample content

**"Scope not granted"**
- Some scopes need business verification
- Start minimal, expand later
- Use sandbox for testing

---

## 📞 Need Help?

### TikTok Resources
- [Developer Portal](https://developers.tiktok.com)
- [API Documentation](https://developers.tiktok.com/doc/content-posting-api-get-started)
- [Developer Forum](https://developers.tiktok.com/forum)

### Our System
- Continue using **mock mode** - it works perfectly!
- Test everything while waiting for approval
- Generate content library locally

**Remember: You don't need TikTok approval to test the system! Mock mode lets you validate everything!** 🚀