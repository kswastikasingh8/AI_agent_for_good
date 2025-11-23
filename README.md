# AI Agents for Good : Mental Health Support System

## 🌟 Project Overview

A multi-agent AI system designed to provide mental health support, crisis assessment, and resource recommendations. This project demonstrates how AI agents can be leveraged for social good by helping people access mental health resources 24/7.

### The Problem

Mental health crises are on the rise globally, yet many people face barriers to accessing timely support:
- **Limited availability**: Therapists and hotlines may have long wait times
- **Stigma**: People hesitate to reach out for help
- **Resource navigation**: Finding appropriate resources is overwhelming
- **24/7 need**: Mental health crises don't follow business hours

### The Solution

An intelligent multi-agent system that:
- ✅ Provides immediate, empathetic responses
- ✅ Assesses crisis severity automatically
- ✅ Recommends appropriate mental health resources
- ✅ Learns from interactions to improve recommendations
- ✅ Maintains conversation context across sessions
- ✅ Monitors system performance for quality assurance

### Value Proposition

- **For Users**: Immediate access to mental health support and resources
- **For Healthcare**: Reduces burden on emergency services by appropriate triage
- **For Society**: Democratizes mental health support access
- **For Research**: Generates insights into mental health needs (anonymized)

---

## 🏗️ Architecture

### System Design

```
┌─────────────────────────────────────────────────────┐
│         MentalHealthOrchestrator                    │
│         (Coordinates all agents)                    │
└─────────────┬───────────────────────────────────────┘
              │
    ┌─────────┴──────────┬──────────────┬─────────────┐
    │                    │              │             │
┌───▼────────┐  ┌───────▼──────┐  ┌───▼──────┐  ┌───▼──────────┐
│  Triage    │  │   Resource   │  │ Support  │  │  Monitoring  │
│   Agent    │  │    Agent     │  │  Agent   │  │    Agent     │
│            │  │              │  │          │  │              │
│ Assesses   │  │ Finds        │  │ Generates│  │ Tracks       │
│ situation  │  │ resources    │  │ responses│  │ metrics      │
└────────────┘  └──────────────┘  └──────────┘  └──────────────┘
     │                 │                │              │
     └─────────────────┴────────────────┴──────────────┘
                       │
            ┌──────────▼──────────┐
            │  Shared Components  │
            │                     │
            │ • Session Service   │
            │ • Memory Bank       │
            │ • Tools             │
            └─────────────────────┘
```

### Agent Workflow (Sequential)

```
User Input
    │
    ▼
┌─────────────────┐
│  TriageAgent    │  ──► Assesses crisis level
└────────┬────────┘      Extracts concerns
         │
         ▼
┌─────────────────┐
│ ResourceAgent   │  ──► Queries resource database
└────────┬────────┘      Prioritizes based on learning
         │
         ▼
┌─────────────────┐
│ SupportAgent    │  ──► Generates empathetic response
└────────┬────────┘      Formats resource information
         │
         ▼
┌─────────────────┐
│ MonitoringAgent │  ──► Logs metrics
└─────────────────┘      Tracks performance
```

---

## 🎯 Key Concepts Demonstrated

### ✅ 1. Multi-Agent System

**Sequential Agents**
- **TriageAgent** → **ResourceAgent** → **SupportAgent** → **MonitoringAgent**
- Each agent has a specialized role
- Output of one agent feeds into the next

**Parallel Agents**
- System can handle multiple user requests simultaneously
- Uses `asyncio.gather()` for concurrent processing
- Demonstrated in `process_parallel_requests()` method

**LLM-Powered Agents**
- Each agent simulates LLM decision-making
- In production, would integrate with Gemini/Claude/GPT
- Agents make context-aware decisions

### ✅ 2. Tools Integration

**Custom Tools**
```python
CrisisDetectionTool
├─ Emergency keyword detection
├─ Risk level assessment
└─ Safety prioritization
```

**MCP (Model Context Protocol) Simulation**
```python
ResourceDatabaseTool
├─ Simulates database connection
├─ Queries mental health resources
└─ Filters by crisis level and type
```

**Built-in Tools**
```python
WebSearchTool
├─ Simulates web search API
├─ Finds current mental health info
└─ Asynchronous operation
```

### ✅ 3. Sessions & Memory

**InMemorySessionService**
- Maintains user session state
- Tracks conversation history
- Stores user concerns and recommendations
- Enables conversation continuity

**MemoryBank (Long-term Memory)**
- Records interaction outcomes
- Learns which resources are most helpful
- Identifies common concerns
- Improves recommendations over time

### ✅ 4. Observability

**Comprehensive Logging**
```python
- Agent initialization
- Decision points
- Tool usage
- Error handling
- Performance metrics
```

**MonitoringAgent**
- Tracks system-wide metrics
- Measures response times
- Monitors crisis level distribution
- Generates performance reports

**Metrics Tracked**
- Total sessions
- Crisis level breakdown
- Average response time
- Resources provided
- Success rates

### ✅ 5. Context Engineering

**Context Compaction**
- Session history maintains context
- Crisis level determines priority
- Resource ranking based on relevance
- Conversation summarization (can be added)

---

## 🚀 Setup Instructions

### Prerequisites

```bash
Python 3.8+
pip (Python package manager)
```

### Installation

1. **Clone the repository**
```bash
git clone <repository-url>
cd mental-health-agents
```

2. **Create virtual environment**
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

3. **Install dependencies**
```bash
pip install -r requirements.txt
```

### Requirements.txt
```
asyncio>=3.4.3
python-dateutil>=2.8.2
dataclasses>=0.6  # Python 3.6 only, built-in for 3.7+
```

### Running the Demo

```bash
python mental_health_agents.py
```

This will run all demonstrations:
1. Sequential agent workflow
2. Parallel agent processing
3. Memory bank and learning
4. Session management
5. Observability and metrics

---

## 💻 Usage Examples

### Basic Usage

```python
import asyncio
from mental_health_agents import MentalHealthOrchestrator

# Initialize the orchestrator
orchestrator = MentalHealthOrchestrator()

# Process a single request
async def help_user():
    result = await orchestrator.process_request(
        "I've been feeling really anxious lately"
    )
    print(result['response'])

asyncio.run(help_user())
```

### Session Continuity

```python
# First interaction
result1 = await orchestrator.process_request(
    "I'm dealing with depression",
    session_id="user_123"
)

# Follow-up in same session
result2 = await orchestrator.process_request(
    "Can you tell me more about therapy options?",
    session_id="user_123"
)

# Retrieve session history
history = orchestrator.get_session_history("user_123")
```

### Parallel Processing

```python
# Handle multiple users simultaneously
requests = [
    "I'm having panic attacks",
    "Need help with depression",
    "Feeling overwhelmed"
]

results = await orchestrator.process_parallel_requests(requests)
```

### Learning from Feedback

```python
# Record user feedback
orchestrator.record_feedback(
    concern="anxiety",
    resource="Crisis Text Line",
    helpful=True
)

# System learns and improves recommendations
```

### Monitoring

```python
# Get system metrics
metrics = orchestrator.get_system_metrics()
print(f"Total sessions: {metrics['total_sessions']}")
print(f"Emergency cases: {metrics['emergency_cases']}")

# Generate report
report = orchestrator.generate_metrics_report()
print(report)
```

---

## 📊 System Metrics Example

```
==================================================
SYSTEM METRICS REPORT
==================================================
Total Sessions: 15
Emergency Cases: 2
High Risk Cases: 3
Moderate Cases: 7
Low Risk Cases: 3
Total Resources Provided: 75
Average Response Time: 0.34s
==================================================
```

---

## 🧪 Testing & Evaluation

### Test Cases Included

1. **Low Risk Assessment**
   - Input: "I'm feeling a bit stressed"
   - Expected: General resources, educational content

2. **Moderate Concern**
   - Input: "I've been feeling anxious and can't sleep"
   - Expected: Hotlines, therapy options

3. **High Risk**
   - Input: "I'm having severe panic attacks"
   - Expected: Crisis resources, immediate support

4. **Emergency**
   - Input: "I don't want to live anymore"
   - Expected: Emergency hotlines, crisis intervention

### Agent Evaluation Framework

```python
# Evaluation metrics
- Response accuracy (crisis level detection)
- Response time (under 1 second target)
- Resource relevance (user feedback)
- System reliability (uptime, error rates)
- Learning effectiveness (improvement over time)
```

---

## 🔧 Extending the System

### Adding Real LLM Integration (Gemini Example)

```python
import google.generativeai as genai

class TriageAgent:
    def __init__(self, crisis_tool, gemini_api_key):
        self.crisis_tool = crisis_tool
        genai.configure(api_key=gemini_api_key)
        self.model = genai.GenerativeModel('gemini-pro')
    
    async def assess(self, user_input: str, session: UserSession):
        # Use Gemini for assessment
        prompt = f"""
        Analyze this mental health concern and identify:
        1. Main concerns
        2. Emotional state
        3. Urgency level
        
        User input: {user_input}
        """
        
        response = self.model.generate_content(prompt)
        # Process response...
```

### Adding Database Persistence

```python
import sqlite3

class DatabaseSessionService:
    def __init__(self, db_path):
        self.conn = sqlite3.connect(db_path)
        self.create_tables()
    
    def create_tables(self):
        self.conn.execute('''
            CREATE TABLE IF NOT EXISTS sessions (
                session_id TEXT PRIMARY KEY,
                user_concerns TEXT,
                crisis_level TEXT,
                conversation_history TEXT,
                created_at TIMESTAMP,
                last_updated TIMESTAMP
            )
        ''')
```

### Adding API Endpoint (FastAPI)

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()
orchestrator = MentalHealthOrchestrator()

class SupportRequest(BaseModel):
    message: str
    session_id: Optional[str] = None

@app.post("/support")
async def get_support(request: SupportRequest):
    result = await orchestrator.process_request(
        request.message,
        request.session_id
    )
    return result
```

---

## 🚢 Deployment Guide

### Docker Deployment

```dockerfile
FROM python:3.9-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

CMD ["python", "mental_health_agents.py"]
```

### Kubernetes Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mental-health-agents
spec:
  replicas: 3
  selector:
    matchLabels:
      app: mental-health-agents
  template:
    metadata:
      labels:
        app: mental-health-agents
    spec:
      containers:
      - name: agent-system
        image: mental-health-agents:latest
        ports:
        - containerPort: 8000
        env:
        - name: LOG_LEVEL
          value: "INFO"
```

### Cloud Deployment Options

**Google Cloud Platform**
- Cloud Run for serverless deployment
- Cloud Functions for individual agents
- Firestore for session storage
- Cloud Monitoring for observability

**AWS**
- ECS/Fargate for containerized deployment
- Lambda for serverless functions
- DynamoDB for sessions
- CloudWatch for monitoring

**Azure**
- Azure Container Instances
- Azure Functions
- Cosmos DB for storage
- Application Insights for monitoring

---

## 🔒 Security & Privacy

### Current Implementation
- In-memory storage (no persistent data)
- No personal information collected
- Session IDs are temporary

### Production Recommendations
1. **Encryption**: Encrypt all data in transit and at rest
2. **Authentication**: Implement OAuth 2.0 or JWT
3. **HIPAA Compliance**: For US healthcare data
4. **Anonymization**: Remove PII from logs and metrics
5. **Audit Trails**: Track all system access
6. **Rate Limiting**: Prevent abuse
7. **Input Validation**: Sanitize all user input

---

## 🤝 Contributing

This is an educational project demonstrating AI agent concepts. Contributions are welcome!

### Areas for Contribution
- Real LLM integration (Gemini, Claude, GPT)
- Additional mental health resources
- Multilingual support
- Mobile app interface
- Voice input/output
- Improved crisis detection algorithms
- Integration with telehealth platforms

---

## 📝 License

This project is for educational purposes. Please ensure compliance with healthcare regulations (HIPAA, GDPR, etc.) before deploying in production.

---

## 🆘 Important Disclaimer

**This is a demonstration system and should not be used for actual mental health crises.**

### Real Crisis Resources

**United States:**
- 988 Suicide & Crisis Lifeline: Call or text 988
- Crisis Text Line: Text HOME to 741741
- NAMI Helpline: 1-800-950-NAMI (6264)

**International:**
- Find resources at: https://findahelpline.com

If you or someone you know is in crisis, please contact these services immediately.

---

## 📚 References & Learning Resources

- [Anthropic Agent Documentation](https://docs.anthropic.com)
- [Multi-Agent Systems](https://arxiv.org/abs/2308.00352)
- [LangChain Agents](https://python.langchain.com/docs/modules/agents/)
- [Mental Health Resources](https://www.nami.org)

---

## 👥 Authors

Created as a demonstration of AI agents for social good.

## 📧 Contact

For questions or collaboration: [Your contact information]

---

**Remember**: AI agents should augment, not replace, professional mental health care.
