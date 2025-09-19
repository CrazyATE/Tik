# 🔌 Platform Connection Guide - From Mock to Live

## Current Status: Mock Mode Active ✅

The system is **100% functional** but operates in **mock mode** for social media platforms. This means:
- ✅ Content is generated with real AI
- ✅ Videos are created and saved locally
- ✅ All metrics are tracked in database
- ⚠️ Content is NOT posted to actual platforms
- ⚠️ Posting is simulated with mock responses

---

## 📊 Connection Status Dashboard

| Service | Status | What It Does | Action Needed |
|---------|--------|--------------|---------------|
| **Supabase** | ✅ Connected | Database, storage, monitoring | None - working |
| **Claude AI** | ✅ Connected | Content generation | None - working |
| **OpenAI GPT-4** | ✅ Connected | Content generation | None - working |
| **TikHub** | ✅ Connected | Trend discovery (read-only) | None - working |
| **TikTok** | 🟡 Mock Mode | Would post videos | OAuth setup required |
| **Instagram** | 🟡 Mock Mode | Would post reels | API setup required |
| **YouTube** | 🟡 Mock Mode | Would post shorts | OAuth setup required |
| **Runway ML** | ❌ Not Setup | Advanced video generation | Optional - API key |
| **ESPN API** | ❌ Not Setup | Sports data source | Optional - API key |

---

## 🎯 Quick Start: Test in Mock Mode First

**You can test EVERYTHING right now without connecting platforms:**

```bash
# Run full test suite
python3 scripts/test_end_to_end.py

# Test content generation
python3 -c "
from src.automation.workflow import WorkflowOrchestrator
import asyncio

async def test():
    orchestrator = WorkflowOrchestrator()
    await orchestrator.initialize()
    result = await orchestrator.run_content_pipeline('nba', {'title': 'Test trend'})
    print(f'Generated: {result}')

asyncio.run(test())
"
```

**What happens in mock mode:**
1. Real AI generates content
2. Video file created in `output/` directory
3. Database records show "posted" status
4. Mock platform IDs returned (e.g., "mock_tiktok_abc123")
5. No actual posting occurs

---

## 🚀 Platform Connection Instructions

### 1️⃣ TikTok Connection

#### Prerequisites
- TikTok Creator or Business account
- Verified developer account
- SSL certificate for redirect URI

#### Step-by-Step Setup

1. **Create TikTok Developer Account**
   ```
   1. Visit: https://developers.tiktok.com
   2. Sign up with your TikTok account
   3. Verify email and phone number
   4. Accept developer terms
   ```

2. **Register Your App**
   ```
   1. Go to "Manage apps" → "Create an app"
   2. App name: "Tik Sports Automation"
   3. App description: "Automated sports content creation"
   4. Category: "Entertainment"
   5. Platform: Select "Web"
   ```

3. **Configure OAuth Settings**
   ```
   Redirect URI: https://your-domain.com/auth/tiktok/callback
   Scopes needed:
   - video.upload
   - video.list
   - user.info.basic
   ```

4. **Get Credentials**
   ```
   Client ID: [Provided after app approval]
   Client Secret: [Provided after app approval]
   ```

5. **Update Environment**
   ```bash
   # In .env.production
   TIK_TIKTOK__CLIENT_ID=your_actual_client_id
   TIK_TIKTOK__CLIENT_SECRET=your_actual_client_secret
   TIK_TIKTOK__MOCK_MODE=false
   ```

6. **Implement OAuth Flow** (Already coded, just needs credentials)
   ```python
   # The system will handle:
   # 1. User authorization redirect
   # 2. Token exchange
   # 3. Token refresh
   # 4. Secure storage
   ```

#### Common Issues
- **App not approved**: TikTok reviews can take 2-5 business days
- **Invalid redirect**: Must use HTTPS in production
- **Rate limits**: Start with 100 posts/day limit

---

### 2️⃣ Instagram Connection

#### Prerequisites
- Instagram Business or Creator account
- Facebook Developer account
- Facebook Page linked to Instagram

#### Step-by-Step Setup

1. **Facebook Developer Setup**
   ```
   1. Visit: https://developers.facebook.com
   2. Create new app → Type: "Business"
   3. Add Instagram Basic Display product
   ```

2. **Configure Instagram API**
   ```
   1. Settings → Basic → Add Platform → Website
   2. Site URL: https://your-domain.com
   3. Add Instagram Basic Display
   4. Add Instagram Graph API
   ```

3. **Get Access Token**
   ```
   1. Tools → Graph API Explorer
   2. Select your app
   3. Add permissions:
      - instagram_basic
      - instagram_content_publish
      - pages_show_list
   4. Generate long-lived token
   ```

4. **Update Environment**
   ```bash
   # In .env.production
   TIK_INSTAGRAM__ACCESS_TOKEN=your_long_lived_token
   TIK_INSTAGRAM__BUSINESS_ACCOUNT_ID=your_account_id
   TIK_INSTAGRAM__MOCK_MODE=false
   ```

#### Alternative: Using Instagram Basic Display
- Simpler setup but limited features
- Good for personal accounts
- No direct posting (requires user action)

---

### 3️⃣ YouTube Connection

#### Prerequisites
- YouTube channel with API access enabled
- Google Cloud Platform account
- Channel verified for API access

#### Step-by-Step Setup

1. **Google Cloud Console**
   ```
   1. Visit: https://console.cloud.google.com
   2. Create new project: "Tik-Sports-Automation"
   3. Enable YouTube Data API v3
   ```

2. **Create Credentials**
   ```
   1. APIs & Services → Credentials
   2. Create Credentials → OAuth 2.0 Client ID
   3. Application type: Web application
   4. Authorized redirect: https://your-domain.com/auth/youtube/callback
   ```

3. **Download Credentials**
   ```
   1. Download JSON file
   2. Rename to: youtube_client_secrets.json
   3. Place in secure location
   ```

4. **Update Environment**
   ```bash
   # In .env.production
   TIK_YOUTUBE__CLIENT_SECRETS_FILE=/path/to/youtube_client_secrets.json
   TIK_YOUTUBE__MOCK_MODE=false
   ```

#### Quota Limits
- Default: 10,000 units/day
- Upload cost: 1600 units per video
- Max uploads: ~6 videos/day with default quota

---

## 🔧 Testing Platform Connections

### Test Individual Platforms
```python
# Test TikTok connection
python3 -c "
from src.platforms.adapters.tiktok_adapter import TikTokAdapter
adapter = TikTokAdapter(mock_mode=False)
print('Authenticated:', adapter.authenticate())
"

# Test Instagram connection
python3 -c "
from src.platforms.adapters.instagram_adapter import InstagramAdapter
adapter = InstagramAdapter(mock_mode=False)
print('Authenticated:', adapter.authenticate())
"

# Test YouTube connection
python3 -c "
from src.platforms.adapters.youtube_adapter import YouTubeAdapter
adapter = YouTubeAdapter(mock_mode=False)
print('Authenticated:', adapter.authenticate())
"
```

### Gradual Rollout Strategy

1. **Week 1: Single Platform Test**
   ```bash
   # Enable only TikTok first
   TIK_TIKTOK__MOCK_MODE=false
   TIK_INSTAGRAM__MOCK_MODE=true  # Keep others in mock
   TIK_YOUTUBE__MOCK_MODE=true
   ```

2. **Week 2: Add Second Platform**
   ```bash
   # Enable TikTok + Instagram
   TIK_TIKTOK__MOCK_MODE=false
   TIK_INSTAGRAM__MOCK_MODE=false
   TIK_YOUTUBE__MOCK_MODE=true  # YouTube still mock
   ```

3. **Week 3: Full Production**
   ```bash
   # All platforms live
   TIK_TIKTOK__MOCK_MODE=false
   TIK_INSTAGRAM__MOCK_MODE=false
   TIK_YOUTUBE__MOCK_MODE=false
   ```

---

## ⚠️ Important Considerations

### Platform Policies
- **TikTok**: No duplicate content, must be original
- **Instagram**: Requires business account for API posting
- **YouTube**: Strict copyright enforcement

### Rate Limits
| Platform | Limit | Reset |
|----------|-------|-------|
| TikTok | 100 posts/day | Daily |
| Instagram | 25 posts/day | Daily |
| YouTube | 6 videos/day | Daily |

### Content Guidelines
- No copyrighted music without license
- No misleading information
- Must comply with community guidelines
- Age-appropriate content only

---

## 🎯 Recommended Approach

### Phase 1: Mock Mode Testing (Current)
✅ **You are here** - Test everything safely
- Validate content quality
- Test autonomous operation
- Monitor cost tracking
- Build content library locally

### Phase 2: Single Platform (TikTok)
- Get TikTok OAuth credentials
- Test with 1 post/day
- Monitor performance
- Adjust based on results

### Phase 3: Multi-Platform
- Add Instagram and YouTube
- Coordinate cross-posting
- Track platform-specific metrics
- Optimize for each platform

### Phase 4: Full Automation
- Enable all platforms
- 2-3 posts/day per platform
- Full autonomous operation
- Minimal human oversight

---

## 💡 Alternative: Semi-Automated Approach

If you don't want to deal with OAuth complexity:

### Option 1: Local Generation + Manual Upload
```python
# Generate content locally
content = system.generate_content()
video = system.create_video(content)
# Then manually upload to platforms
```

### Option 2: Use Third-Party Services
- **Buffer**: API for scheduling
- **Hootsuite**: Bulk upload support
- **Later**: Visual content calendar
- **Zapier**: Automation connectors

### Option 3: Business API Partners
- TikTok Marketing Partners
- Instagram Content Publishing Partners
- YouTube Certified Partners

---

## 🚨 Troubleshooting

### Common Issues

1. **"OAuth callback failed"**
   - Ensure HTTPS for production
   - Check redirect URI matches exactly
   - Verify domain is whitelisted

2. **"Rate limit exceeded"**
   - Implement exponential backoff
   - Use platform-specific queues
   - Monitor quota usage

3. **"Video upload failed"**
   - Check file size limits
   - Verify video codec compatibility
   - Ensure proper aspect ratio

4. **"Account not authorized"**
   - Re-authenticate OAuth
   - Check token expiration
   - Verify required scopes

---

## 📞 Support Resources

### Official Documentation
- [TikTok for Developers](https://developers.tiktok.com/doc/getting-started)
- [Instagram Graph API](https://developers.facebook.com/docs/instagram-api)
- [YouTube Data API](https://developers.google.com/youtube/v3)

### Community Support
- TikTok Developer Forum
- Facebook Developer Community
- Google Cloud Community

### Our System Logs
Check logs for detailed error messages:
```bash
# View recent platform errors
tail -f logs/platform_errors.log

# Check authentication status
python3 scripts/check_platform_auth.py
```

---

## ✅ Ready to Connect?

**Remember: The system works perfectly in mock mode!**

You can:
1. Test everything now without platform credentials
2. Generate real content with AI
3. See exactly what would be posted
4. Connect platforms when you're ready

**No rush to connect platforms - mock mode is perfect for testing!** 🚀