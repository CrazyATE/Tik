# 🏀 Tik Sports Automation System

A fully automated TikTok sports content creation and posting system that discovers trending sports videos, creates AI-powered remixes, and posts them automatically with intelligent scheduling.

## 🚀 Quick Start

### Prerequisites

- Python 3.10+
- ffmpeg (for video processing)
- Git

### 1. Setup Environment

```bash
# Clone and setup
git clone <repository-url>
cd Tik
python scripts/setup.py
```

### 2. Configure Environment

```bash
# Edit .env with your API keys
cp .env.example .env
nano .env
```

Required API keys:
- **Supabase**: Database and storage
- **Anthropic**: Claude AI for content generation  
- **OpenAI**: GPT-4 fallback for content generation
- **TikHub**: TikTok trending data discovery
- **TikTok**: Posting videos (optional for testing)

### 3. Deploy Database Schema

```bash
# Deploy to Supabase
python scripts/deploy_schema.py
```

### 4. Validate System

```bash
# Test all components
python scripts/validate_system.py
```

### 5. Run the System

```bash
# Start web server with dashboard
python main.py serve

# OR start automated scheduler
python main.py schedule

# OR run manual job
python main.py manual nba
```

## 📋 Commands

### CLI Commands

```bash
# Web server (dashboard + API)
python main.py serve [--host 127.0.0.1] [--port 8000] [--reload]

# Automated scheduler
python main.py schedule

# Manual content creation
python main.py manual {nba|nfl|mlb|nhl} [--count 1-3]

# System health check
python main.py health

# Initialize system
python main.py init
```

### Setup & Maintenance

```bash
# Initial setup
python scripts/setup.py [--skip-deps] [--validate-apis]

# Deploy database schema
python scripts/deploy_schema.py [--verify-only] [--with-sample-data]

# System validation
python scripts/validate_system.py [--component database|ai|video|apis|automation]
```

## 🎯 How It Works

### 1. Content Discovery
- **TikHub API** finds trending sports videos (10K+ views)
- **Engagement Analysis** scores videos by viral potential
- **Remix Detection** identifies content suitable for remixing

### 2. Content Analysis  
- **Theme Extraction** identifies records, stats, comparisons
- **Insight Generation** extracts key statistics and narratives
- **Remix Strategy** determines best remix angle (historical update, stat breakdown, fact check, fresh perspective)

### 3. AI Content Generation
- **Claude/GPT-4** creates engaging video scripts
- **Template System** structures content for optimal engagement
- **Cost Optimization** tracks API usage against daily limits

### 4. Video Creation
- **Template Rendering** creates 9:16 vertical videos
- **Supabase Storage** uploads videos and thumbnails
- **Quality Control** ensures proper resolution and duration

### 5. Automated Posting
- **TikTok API** posts videos with optimized captions
- **Intelligent Scheduling** spaces posts 3-8 hours apart
- **Performance Tracking** monitors engagement and metrics

## 🔧 Configuration

### Environment Variables

```env
# Supabase (Required)
TIK_SUPABASE_URL=https://your-project.supabase.co
TIK_SUPABASE_KEY=your-anon-key
TIK_SUPABASE_SERVICE_ROLE_KEY=your-service-role-key

# AI Services (Required)
TIK_ANTHROPIC_API_KEY=your-anthropic-key
TIK_OPENAI_API_KEY=your-openai-key

# Content Discovery (Required)
TIK_TIKHUB_API_KEY=your-tikhub-key

# TikTok Posting (Optional)
TIK_TIKTOK_CLIENT_ID=your-client-id
TIK_TIKTOK_CLIENT_SECRET=your-client-secret

# Cost Controls
TIK_DAILY_COST_LIMIT=10.00
TIK_MONTHLY_COST_LIMIT=300.00

# Posting Schedule
TIK_MAX_POSTS_PER_DAY=3
TIK_MIN_POST_INTERVAL_HOURS=3
TIK_DEFAULT_POST_TIMES=06:00,14:00,19:00
```

### Cost Management

- **Daily Limit**: $10 (configurable)
- **API Tracking**: Every call monitored and logged
- **Automatic Throttling**: System pauses when limits reached
- **Cost Breakdown**: Claude ~$0.008/1K tokens, OpenAI ~$0.010/1K tokens

## 🏗️ Architecture

### Core Components

```
├── src/
│   ├── ai/                 # AI content generation (Claude/GPT-4)
│   ├── api/                # External API clients (TikHub, TikTok)
│   ├── automation/         # Workflow orchestration & scheduling
│   ├── data/               # Content analysis & data models
│   ├── db/                 # Supabase database operations
│   ├── utils/              # Logging, error handling
│   └── video/              # Video generation & templates
├── config/                 # Configuration management
├── database/               # Database schema & migrations
├── scripts/                # Setup & deployment scripts
└── main.py                 # Application entry point
```

### Data Flow

```
TikHub Discovery → Content Analysis → AI Generation → Video Render → TikTok Post
       ↓               ↓                ↓             ↓           ↓
   Trending Videos  → Insights      → Scripts     → MP4 Files → Posted Videos
   (10K+ views)      (Stats/Themes)  (Engaging)    (9:16)      (Scheduled)
```

## 📊 Web Dashboard

Visit `http://localhost:8000` for:

- **System Health**: Real-time component status
- **Daily Metrics**: Cost tracking and performance
- **Manual Controls**: Trigger jobs and manage scheduler
- **API Documentation**: Interactive API docs at `/docs`

### API Endpoints

- `GET /health` - System health check
- `GET /status` - Detailed system metrics
- `POST /manual/{sport}` - Run manual content creation
- `POST /scheduler/start` - Start automated scheduling  
- `POST /scheduler/stop` - Stop automated scheduling

## 🧪 Development

### Local Development

```bash
# Setup development environment
python scripts/setup.py
python -m venv venv
source venv/bin/activate  # or venv\Scripts\activate on Windows
pip install -r requirements.txt

# Run with auto-reload
python main.py serve --reload
```

### Docker Development

```bash
# Start full stack (includes Supabase local)
docker-compose up -d

# View logs
docker-compose logs -f tik-app

# Stop stack
docker-compose down
```

### Testing

```bash
# Full system validation
python scripts/validate_system.py

# Component-specific tests
python scripts/validate_system.py --component database
python scripts/validate_system.py --component ai
python scripts/validate_system.py --component video

# Manual test runs
python main.py manual nba --count 1
python main.py health
```

## 🔒 Security & Safety

### Content Safety
- **Fact Validation**: AI models cross-check statistics
- **Brand Safety**: Content filtered for appropriateness
- **Copyright Respect**: Only remixes with original analysis
- **Platform Compliance**: TikTok policy adherence

### System Safety
- **Cost Controls**: Hard daily/monthly spending limits
- **Rate Limiting**: Respects all API rate limits
- **Error Recovery**: Automatic retry with exponential backoff
- **Shadowban Prevention**: Randomized posting patterns

## 📈 Performance

### Targets (Phase 1)
- **Daily Output**: 1-3 videos automatically
- **Processing Time**: <2 minutes per video
- **Cost Efficiency**: $2-5 per video vs $50-100 manual
- **Engagement**: 9.2% target (vs 4.07% platform average)
- **Uptime**: 99.5% with automated recovery

### Monitoring
- **Real-time Metrics**: Cost, performance, errors
- **Database Logging**: All activities tracked
- **Health Checks**: Automated system validation
- **Alert System**: Email/webhook notifications

## 🛣️ Roadmap

### ✅ Phase 1: Foundation (Complete)
- Single automated post working end-to-end
- Cost tracking and safety controls
- Full error handling and recovery
- Web dashboard and CLI interface

### 🔄 Phase 2: Intelligence Layer  
- Multi-model AI validation
- A/B testing framework
- Advanced engagement prediction
- Fact-checking pipeline

### 🚀 Phase 3: Scale & Optimization
- 2-3 posts daily with intelligent spacing
- Shadowban detection and recovery
- Performance feedback loops
- Multi-sport coverage

### 🌐 Phase 4: Platform Expansion
- Instagram Reels, YouTube Shorts
- Cross-platform analytics
- Format adaptations
- Multi-account management

### 🧠 Phase 5: Advanced Intelligence
- Predictive content planning  
- Real-time trend integration
- Self-optimizing parameters
- Fully autonomous operation

## 💬 Support

### Troubleshooting

**Setup Issues:**
```bash
python scripts/validate_system.py --component database
```

**API Connection Problems:**
```bash
python main.py health
```

**Cost Limit Errors:**
Check Supabase dashboard for daily usage.

**Video Generation Fails:**
Ensure ffmpeg is installed: `ffmpeg -version`

### Getting Help

1. **Health Check**: `python main.py health`  
2. **System Validation**: `python scripts/validate_system.py`
3. **Logs**: Check `logs/tik.log` for detailed errors
4. **Dashboard**: Visit `http://localhost:8000` for status

## 📄 License

This project is for educational and research purposes. Please ensure compliance with:
- TikTok Terms of Service
- API provider terms (Anthropic, OpenAI, TikHub)  
- Sports data usage rights
- Content creation best practices

---

**Ready to automate your sports content? Run `python scripts/setup.py` to get started! 🚀**