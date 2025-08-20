# 🏗️ Low-Level System Design - Bangalore Airport Social Media Analytics

## 📊 Architecture Overview

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│                                🌐 CLIENT LAYER                                      │
├─────────────────────────────────────────────────────────────────────────────────────┤
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐ │
│  │ Dashboard   │  │ Data Mgmt   │  │ Settings    │  │ AeroBot     │  │ Talk to Us  │ │
│  │ Component   │  │ Component   │  │ Component   │  │ Component   │  │ Component   │ │
│  └─────┬───────┘  └─────┬───────┘  └─────┬───────┘  └─────┬───────┘  └─────┬───────┘ │
│        │                │                │                │                │         │
│  ┌─────▼─────────────────▼────────────────▼────────────────▼────────────────▼─────┐   │
│  │                    React Query Client (TanStack)                              │   │
│  │              • Caching • State Management • API Calls                         │   │
│  └─────────────────────────────────┬──────────────────────────────────────────────┘   │
└──────────────────────────────────────┼──────────────────────────────────────────────────┘
                                       │ HTTP/REST API
┌──────────────────────────────────────▼──────────────────────────────────────────────────┐
│                                🚀 SERVER LAYER                                          │
├─────────────────────────────────────────────────────────────────────────────────────────┤
│  ┌─────────────────────────────────────────────────────────────────────────────────────┐ │
│  │                           Express.js Router                                         │ │
│  │  /api/social-events  /api/collect-data  /api/llm-query  /api/settings  /api/users  │ │
│  └─────┬───────────────────┬───────────────────┬──────────────────┬──────────────┬────┘ │
│        │                   │                   │                  │              │      │
│  ┌─────▼─────┐  ┌──────────▼──────────┐  ┌─────▼──────┐  ┌──────▼──────┐  ┌─────▼────┐ │
│  │   User    │  │     Agent           │  │    LLM     │  │   MongoDB   │  │ Storage  │ │
│  │   Mgmt    │  │     Manager         │  │  Service   │  │   Service   │  │ Service  │ │
│  │           │  │                     │  │            │  │             │  │          │ │
│  └───────────┘  └─────────┬───────────┘  └─────┬──────┘  └──────┬──────┘  └──────────┘ │
└─────────────────────────────┼─────────────────────┼─────────────────┼─────────────────────┘
                              │                     │                 │
┌─────────────────────────────▼─────────────────────▼─────────────────▼─────────────────────┐
│                             🔧 SERVICE LAYER                                               │
├─────────────────────────────────────────────────────────────────────────────────────────┤
│  ┌────────────────────────────────────────────────────────────────────────────────────┐  │
│  │                        Individual Specialized Agents                              │  │
│  │                                                                                    │  │
│  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌───────────┐ │  │
│  │  │   Reddit    │  │  Facebook   │  │   Twitter   │  │     CNN     │  │ Inshorts  │ │  │
│  │  │   Agent     │  │   Agent     │  │   Agent     │  │   Agent     │  │  Agent    │ │  │
│  │  │             │  │             │  │             │  │             │  │           │ │  │
│  │  │ • Auth      │  │ • Auth      │  │ • Auth      │  │ • RSS Feed  │  │ • RSS     │ │  │
│  │  │ • Search    │  │ • Search    │  │ • Search    │  │ • Parsing   │  │ • Parse   │ │  │
│  │  │ • Filter    │  │ • Filter    │  │ • Filter    │  │ • Filter    │  │ • Filter  │ │  │
│  │  │ • Extract   │  │ • Extract   │  │ • Extract   │  │ • Extract   │  │ • Extract │ │  │
│  │  └─────┬───────┘  └─────┬───────┘  └─────┬───────┘  └─────┬───────┘  └─────┬─────┘ │  │
│  └────────┼──────────────────┼──────────────────┼──────────────────┼──────────────┼─────┘  │
└───────────┼──────────────────┼──────────────────┼──────────────────┼──────────────┼────────┘
            │                  │                  │                  │              │
┌───────────▼──────────────────▼──────────────────▼──────────────────▼──────────────▼────────┐
│                              🌍 EXTERNAL DATA SOURCES                                      │
├─────────────────────────────────────────────────────────────────────────────────────────┤
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐     │
│  │ Reddit API  │  │Facebook API │  │Twitter API  │  │  CNN RSS    │  │Inshorts RSS │     │
│  │             │  │             │  │             │  │             │  │             │     │
│  │ • Posts     │  │ • Posts     │  │ • Tweets    │  │ • Articles  │  │ • Articles  │     │
│  │ • Comments  │  │ • Comments  │  │ • Replies   │  │ • Headlines │  │ • Headlines │     │
│  │ • Subreddits│  │ • Pages     │  │ • Trends    │  │ • Content   │  │ • Content   │     │
│  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────┘     │
└─────────────────────────────────────────────────────────────────────────────────────────┘

```

## 🔄 Data Flow Diagram

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│   USER      │    │   REACT     │    │   EXPRESS   │    │   AGENT     │    │ EXTERNAL    │
│ INTERFACE   │    │   QUERY     │    │   ROUTER    │    │  MANAGER    │    │   APIS      │
└─────┬───────┘    └─────┬───────┘    └─────┬───────┘    └─────┬───────┘    └─────┬───────┘
      │                  │                  │                  │                  │
      │ 1. User Action   │                  │                  │                  │
      ├─────────────────►│                  │                  │                  │
      │                  │                  │                  │                  │
      │                  │ 2. HTTP Request  │                  │                  │
      │                  ├─────────────────►│                  │                  │
      │                  │                  │                  │                  │
      │                  │                  │ 3. Route to      │                  │
      │                  │                  │    AgentManager  │                  │
      │                  │                  ├─────────────────►│                  │
      │                  │                  │                  │                  │
      │                  │                  │                  │ 4. Agent API Call │
      │                  │                  │                  ├─────────────────►│
      │                  │                  │                  │                  │
      │                  │                  │                  │ 5. Raw Data      │
      │                  │                  │                  │◄─────────────────┤
      │                  │                  │                  │                  │
      │                  │                  │ 6. Processed     │                  │
      │                  │                  │    Events        │                  │
      │                  │                  │◄─────────────────┤                  │
      │                  │                  │                  │                  │
      │                  │ 7. JSON Response │                  │                  │
      │                  │◄─────────────────┤                  │                  │
      │                  │                  │                  │                  │
      │ 8. UI Update     │                  │                  │                  │
      │◄─────────────────┤                  │                  │                  │
      │                  │                  │                  │                  │

```

## 🧠 AI & Database Integration

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│                             🤖 AI PROCESSING LAYER                                  │
├─────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                     │
│  ┌─────────────────────────────────────────────────────────────────────────────┐   │
│  │                        Ollama LLM Service                                   │   │
│  │                                                                             │   │
│  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐       │   │
│  │  │   Chat      │  │ Sentiment   │  │ Embeddings  │  │   Model     │       │   │
│  │  │ (Chat UI)   │  │ Analysis    │  │ Generation  │  │ Management  │       │   │
│  │  │             │  │             │  │             │  │             │       │   │
│  │  │ • tinyllama │  │ • tinyllama │  │ • tinyllama │  │ • deepseek  │       │   │
│  │  │ • deepseek  │  │ • deepseek  │  │ • deepseek  │  │ • gpt-oss   │       │   │
│  │  └─────┬───────┘  └─────┬───────┘  └─────┬───────┘  └─────────────┘       │   │
│  └────────┼──────────────────┼──────────────────┼─────────────────────────────┘   │
└───────────┼──────────────────┼──────────────────┼─────────────────────────────────┘
            │                  │                  │
            ▼                  ▼                  ▼
┌─────────────────────────────────────────────────────────────────────────────────────┐
│                            💾 DATABASE LAYER                                        │
├─────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                     │
│  ┌──────────────────────────┐              ┌──────────────────────────┐            │
│  │     MongoDB Atlas        │              │      ChromaDB           │            │
│  │   (Primary Storage)      │              │   (Vector Storage)      │            │
│  │                          │              │                         │            │
│  │  ┌─────────────────────┐ │              │  ┌─────────────────────┐│            │
│  │  │ Collections:        │ │              │  │ • Text Embeddings   ││            │
│  │  │ • reddit           │ │              │  │ • Semantic Search   ││            │
│  │  │ • facebook         │ │              │  │ • Similarity Match  ││            │
│  │  │ • twitter          │ │              │  │ • RAG Context      ││            │
│  │  │ • cnn              │ │              │  └─────────────────────┘│            │
│  │  │ • inshorts         │ │              └──────────────────────────┘            │
│  │  │ • instagram        │ │                                                      │
│  │  │ • contact_messages │ │              ┌──────────────────────────┐            │
│  │  │ • settings         │ │              │    In-Memory Storage    │            │
│  │  │ • users            │ │              │   (Development Only)    │            │
│  │  └─────────────────────┘ │              │                         │            │
│  └──────────────────────────┘              │  • Contact Messages     │            │
│                                            │  • User Settings        │            │
│                                            │  • Session Data         │            │
│                                            └──────────────────────────┘            │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

## 🔄 Component Interaction Flow

### Data Collection Flow:
1. **User triggers collection** → Dashboard UI → `/api/collect-data`
2. **Route validation** → Express Router → AgentManager
3. **Agent selection** → AgentManager → Specific Agent (Reddit/Facebook/etc.)
4. **API calls** → Agent → External API (Reddit/Facebook/etc.)
5. **Data processing** → Agent → Raw posts/tweets/articles
6. **Sentiment analysis** → LLM Service → Ollama Model
7. **Storage** → MongoDB Service → MongoDB Atlas
8. **Response** → Client → UI Update

### Chat/Query Flow:
1. **User message** → AeroBot UI → `/api/llm-query`
2. **Context retrieval** → LLM Service → ChromaDB
3. **AI processing** → Ollama → Model inference
4. **Response generation** → LLM Service → Client
5. **UI update** → React Query → AeroBot Component

### Analytics Flow:
1. **Dashboard load** → React Query → `/api/social-events`
2. **Data aggregation** → MongoDB Service → Collections query
3. **Metrics calculation** → Server → Statistics processing
4. **Chart rendering** → Recharts → Dashboard visualization

## 🔑 Key Design Decisions

### **Individual Agent Architecture**
- Each data source has a dedicated agent class
- Agents inherit from BaseAgent with standardized interface
- Isolated credential management and error handling
- Specialized parsing and filtering per platform

### **Dual Storage Strategy**
- **MongoDB Atlas**: Persistent data storage for social events
- **In-Memory**: Fast access for user sessions and settings
- **ChromaDB**: Vector storage for AI embeddings and RAG

### **AI Integration**
- **Local Ollama**: No external API dependencies
- **Model flexibility**: Support for multiple models (tinyllama, deepseek, gpt-oss)
- **Timeout handling**: Graceful fallbacks for slow models

### **Performance Optimizations**
- **React Query**: Intelligent caching and state management
- **Agent pooling**: Reusable agent instances
- **Lazy loading**: Components loaded on-demand
- **Connection persistence**: MongoDB auto-reconnection
