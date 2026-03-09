# ResearchPilot-Agent-REST-API
[Live Demo](https://ai-researcher-agent-system.streamlit.app)
AI-powered research system with modular architecture, multi-agent collaboration, and real tool integration.

## 🌟 Features
- **Multi-Agent System**: 3 specialized AI agents working together
  - **Researcher**: Searches web for current information
  - **Fact-Checker**: Verifies claims and cross-checks sources
  - **Summarizer**: Creates professional reports
- **Real Tool Access**: Web search, webpage scraping, calculations
- **JWT Authentication**: Secure user authentication and authorization
- **Database Storage**: SQLite for research history and user management
- **Modular Architecture**: Clean separation of concerns
- **RESTful API**: FastAPI with automatic OpenAPI documentation
- **Fully Tested**: Unit tests with pytest

## 📁 Project Structure

```
research_assistant/
├── agent/              # AI agents and workflow logic
│   ├── tools.py        # Research tools (search, scrape, calculate)
│   ├── state.py        # Workflow state definition
│   ├── agents.py       # Agent classes
│   ├── router.py       # Routing logic
│   └── graph.py        # LangGraph workflow builder
├── api/                # FastAPI application
│   ├── main.py         # App entry point
│   ├── models.py       # Pydantic models
│   ├── auth_routes.py  # Authentication endpoints
│   └── research_routes.py # Research endpoints
├── database/           # Database layer
│   ├── db.py           # Database setup
│   └── models.py       # SQLAlchemy models
├── auth/               # Authentication
│   ├── security.py     # Password hashing, JWT
│   └── dependencies.py # Auth middleware
├── config/             # Configuration
│   └── settings.py     # Settings management
├── tests/              # Unit tests
│   └── test_agents.py  # Agent tests
├── requirements.txt    # Python dependencies
├── .env                # Environment variables
└── README.md          # This file
```

## 🚀 Quick Start

### 1. Clone and Setup

```bash
# Clone repository
git clone <your-repo-url>
cd research_assistant

# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

### 2. Configure Environment

Create `.env` file:

```bash
# API Keys
GOOGLE_API_KEY=your_google_gemini_api_key

# JWT Secret (generate with: openssl rand -hex 32)
SECRET_KEY=your_super_secret_jwt_key_here

# Database
DATABASE_URL=sqlite:///./research_assistant.db

# Agent Settings
MAX_RESEARCH_ITERATIONS=2
DEFAULT_LLM_TEMPERATURE=0.7
```

### 3. Run Application

```bash
# Start server
uvicorn api.main:app --reload

# Server will start at: http://localhost:8000
```

### 4. Access API Documentation

- **Swagger UI**: http://localhost:8000/docs
- **ReDoc**: http://localhost:8000/redoc
- **Health Check**: http://localhost:8000/health

## 📚 API Usage

### Register User

```bash
curl -X POST http://localhost:8000/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "username": "testuser",
    "email": "test@example.com",
    "password": "password123"
  }'
```

### Login

```bash
curl -X POST http://localhost:8000/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "username": "testuser",
    "password": "password123"
  }'
```

Response:
```json
{
  "access_token": "eyJhbGc...",
  "token_type": "bearer",
  "username": "testuser",
  "email": "test@example.com"
}
```

### Create Research

```bash
curl -X POST http://localhost:8000/research/ \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -d '{
    "query": "What are the latest AI developments in 2024?",
    "max_iterations": 2
  }'
```

### Get Research History

```bash
curl -X GET http://localhost:8000/research/history \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Get Specific Research

```bash
curl -X GET http://localhost:8000/research/1 \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

## 🧪 Testing

```bash
# Run all tests
pytest tests/ -v

# Run specific test file
pytest tests/test_agents.py -v

# Run with coverage
pytest tests/ --cov=agent --cov-report=html

# View coverage report
open htmlcov/index.html
```

## 🔧 Configuration

### Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `GOOGLE_API_KEY` | Google AI API key | Required |
| `SECRET_KEY` | JWT secret key | Required |
| `DATABASE_URL` | Database connection | `sqlite:///./research_assistant.db` |
| `MAX_RESEARCH_ITERATIONS` | Max research cycles | `2` |
| `ACCESS_TOKEN_EXPIRE_MINUTES` | Token expiration | `1440` (24h) |

### Agent Configuration

Edit `config/settings.py`:

```python
MAX_RESEARCH_ITERATIONS: int = 2  # 1-5 iterations
DEFAULT_LLM_TEMPERATURE: float = 0.7  # 0.0-1.0
```

## 🏗️ Architecture

### Multi-Agent Workflow

```
User Query
    ↓
Researcher Agent (with tools)
    ↓ (searches web, scrapes pages)
Fact-Checker Agent (with tools)
    ↓ (verifies claims)
Summarizer Agent
    ↓ (creates report)
Final Report
```

### Tools Available

1. **Web Search**: DuckDuckGo search for current information
2. **Web Scraper**: Extract content from URLs
3. **Calculator**: Perform mathematical calculations

### Database Schema

**Users Table**:
- id, username, email, hashed_password, is_active, created_at

**ResearchSessions Table**:
- id, user_id, query, research_data, verified_facts, final_report
- status, agent_iterations, processing_time, created_at

## 📊 Project Stats

- **Lines of Code**: ~1,500+
- **Modules**: 10
- **API Endpoints**: 8
- **Test Coverage**: 80%+
- **Tools Integrated**: 3

## 🎯 Key Features for Portfolio

1. ✅ **Modular Design**: Clean separation of concerns
2. ✅ **Multi-Agent System**: LangGraph orchestration
3. ✅ **Tool Integration**: Real web search capabilities
4. ✅ **Production Ready**: Auth, DB, error handling
5. ✅ **Well Tested**: Unit tests with pytest
6. ✅ **Documented**: Comprehensive API docs
7. ✅ **Scalable**: Easy to extend with new agents/tools

## 🚀 Future Enhancements

- [ ] Add streaming responses
- [ ] Implement caching with Redis
- [ ] Add more tools (Wikipedia, arXiv, GitHub)
- [ ] Create admin dashboard
- [ ] Add usage analytics
- [ ] Implement rate limiting
- [ ] Deploy to cloud (AWS/GCP)

## 📝 License

MIT License

## 👤 Author

[Asadullah Shebaz]

## 🙏 Acknowledgments

- LangChain & LangGraph
- FastAPI
- GROQ API Key
