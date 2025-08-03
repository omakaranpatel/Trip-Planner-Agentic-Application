# AI Trip Planner - System Architecture

## Overview

The AI Trip Planner is built using a modern, modular architecture that combines Large Language Models (LLMs) with specialized tools to create an intelligent travel planning system. The architecture follows a state-based workflow pattern using LangGraph for orchestration.

## 🏗️ High-Level Architecture

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   FastAPI App   │    │   LangGraph     │    │   External      │
│   (main.py)     │◄──►│   Workflow      │◄──►│   APIs          │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         │                       │                       │
         ▼                       ▼                       ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   User Input    │    │   Agent Logic   │    │   Weather API   │
│   Validation    │    │   & Tools       │    │   Places API    │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

## 🔧 Core Components

### 1. **FastAPI Application Layer** (`main.py`)

**Purpose**: Web API interface for the travel planning service

**Key Features**:
- RESTful API endpoints
- Request/response validation using Pydantic
- CORS middleware for cross-origin requests
- Error handling and logging
- Health check endpoints

**Endpoints**:
- `POST /query`: Main travel planning endpoint
- `GET /health`: Service health check
- `GET /`: API information and documentation

**Data Flow**:
1. Receives HTTP requests with travel queries
2. Validates input using Pydantic models
3. Initializes the LangGraph workflow
4. Processes the query through the agent
5. Returns formatted travel plans

### 2. **Agent Workflow Layer** (`Agent/Agenticworkflow.py`)

**Purpose**: Orchestrates the AI reasoning and tool execution workflow

**Key Components**:

#### GraphBuilder Class
- **Initialization**: Sets up LLM, tools, and workflow configuration
- **Tool Integration**: Combines weather, places, calculator, and currency tools
- **Workflow Management**: Creates and manages the LangGraph state machine

#### Agent Function
- **Reasoning Engine**: Processes user queries using the LLM
- **Context Management**: Maintains conversation state and history
- **Tool Decision Making**: Determines when and which tools to use

#### Tools Condition
- **Routing Logic**: Decides whether to use tools or end conversation
- **State Management**: Manages workflow transitions between nodes

**Workflow Pattern**:
```
START → agent → (condition) → tools → agent → END
                    ↓
                   end
```

### 3. **Tool Integration Layer** (`Tools/`)

**Purpose**: Provides specialized functionality for travel planning

#### Weather Tools (`weather_info_tool.py`)
- **Current Weather**: Real-time temperature and conditions
- **Forecast Data**: 5-day weather predictions
- **API Integration**: OpenWeatherMap API
- **Use Cases**: Activity planning, packing recommendations

#### Place Search Tools (`place_search_tool.py`)
- **Attractions**: Tourist spots and landmarks
- **Accommodations**: Hotels and lodging options
- **Restaurants**: Dining recommendations
- **API Integration**: Google Places API
- **Use Cases**: Itinerary planning, accommodation booking

#### Calculator Tools (`expense_calculator_tool.py`)
- **Cost Breakdown**: Detailed expense calculations
- **Budget Planning**: Per-day and total cost estimates
- **Category Management**: Accommodation, food, activities, transport
- **Use Cases**: Budget optimization, cost analysis

#### Currency Tools (`currency_conversion_tool.py`)
- **Exchange Rates**: Real-time currency conversion
- **Multi-Currency Support**: Global currency handling
- **API Integration**: Currency conversion APIs
- **Use Cases**: International travel planning

### 4. **Utility Layer** (`utils/`)

**Purpose**: Provides supporting functionality and API integrations

#### Model Loader (`model_loader.py`)
- **LLM Configuration**: Manages different model providers (Groq, OpenAI)
- **API Integration**: Handles authentication and model initialization
- **Provider Switching**: Supports multiple LLM backends

#### Weather Integration (`weather_info.py`)
- **API Communication**: Handles OpenWeatherMap API calls
- **Data Processing**: Formats weather data for consumption
- **Error Handling**: Manages API failures and rate limits

#### Place Information (`place_info_search.py`)
- **Location Data**: Retrieves place information and details
- **Search Functionality**: Finds attractions, hotels, restaurants
- **Data Enrichment**: Provides additional context and information

#### Financial Tools (`expense_calculator.py`, `currency_converter.py`)
- **Cost Calculations**: Performs mathematical operations
- **Currency Conversion**: Handles exchange rate calculations
- **Budget Analysis**: Provides financial insights and recommendations

### 5. **Configuration Layer** (`Config/`)

**Purpose**: Manages system configuration and settings

#### Configuration Files (`config.yaml`)
- **LLM Settings**: Model providers and configurations
- **API Endpoints**: External service configurations
- **System Parameters**: Application settings and defaults

### 6. **Prompt Management** (`Prompt_library/`)

**Purpose**: Defines AI behavior and response patterns

#### System Prompts (`prompt.py`)
- **Behavior Definition**: Instructs AI on how to act as a travel agent
- **Response Format**: Defines output structure and formatting
- **Tool Usage**: Guides AI on when and how to use available tools

## 🔄 Data Flow Architecture

### 1. **Request Processing Flow**

```
User Query → FastAPI → Validation → GraphBuilder → Agent → Tools → Response
```

**Detailed Steps**:
1. **User submits query** via HTTP POST to `/query`
2. **FastAPI validates** request using Pydantic models
3. **GraphBuilder initializes** with LLM and tools
4. **Agent processes** query using system prompt and context
5. **Tools are called** as needed for real-time data
6. **Response is formatted** and returned to user

### 2. **Tool Execution Flow**

```
Agent Decision → Tool Selection → API Call → Data Processing → Result Integration
```

**Detailed Steps**:
1. **Agent determines** which tools are needed based on query
2. **LangGraph routes** to appropriate tool execution
3. **Tool makes API call** to external service
4. **Data is processed** and formatted
5. **Results are integrated** back into agent workflow

### 3. **State Management Flow**

```
MessagesState → Agent Processing → Tool Execution → State Update → Next Decision
```

**State Components**:
- **Messages**: Conversation history and current context
- **Tool Calls**: Pending or completed tool executions
- **Workflow Status**: Current position in the workflow graph

## 🛠️ Technology Stack

### Backend Framework
- **FastAPI**: Modern, fast web framework for building APIs
- **Pydantic**: Data validation and settings management
- **Uvicorn**: ASGI server for running the application

### AI/ML Framework
- **LangGraph**: State-based workflow orchestration
- **LangChain**: LLM integration and tool management
- **Groq/OpenAI**: Large Language Model providers

### External APIs
- **OpenWeatherMap**: Weather data and forecasts
- **Google Places**: Location and place information
- **Currency APIs**: Exchange rate data

### Development Tools
- **Python 3.8+**: Programming language
- **uv/poetry**: Dependency management
- **Environment Variables**: Configuration management

## 🔒 Security and Error Handling

### Security Measures
- **API Key Management**: Secure storage of external API credentials
- **Input Validation**: Pydantic models for request validation
- **CORS Configuration**: Controlled cross-origin access
- **Error Sanitization**: Safe error message handling

### Error Handling Strategy
- **Graceful Degradation**: System continues working with partial data
- **Fallback Mechanisms**: Alternative data sources when APIs fail
- **User-Friendly Messages**: Clear error communication
- **Logging and Monitoring**: Comprehensive error tracking

## 📊 Performance Considerations

### Optimization Strategies
- **Async Processing**: Non-blocking API calls and operations
- **Caching**: Weather and currency data caching
- **Rate Limiting**: Respectful API usage
- **Connection Pooling**: Efficient external API connections

### Scalability Features
- **Modular Design**: Easy to add new tools and capabilities
- **Stateless Architecture**: Horizontal scaling support
- **Configuration-Driven**: Easy deployment and configuration
- **Monitoring Ready**: Built-in health checks and metrics

## 🔮 Future Architecture Enhancements

### Planned Improvements
- **Database Integration**: Persistent storage for user preferences
- **Caching Layer**: Redis for improved performance
- **Microservices**: Service decomposition for better scalability
- **Event-Driven Architecture**: Real-time updates and notifications
- **Machine Learning**: Personalized recommendations and predictions

### Extension Points
- **New Tool Types**: Additional data sources and capabilities
- **Multi-Modal Support**: Image and voice processing
- **Real-Time Collaboration**: Multi-user travel planning
- **Mobile Integration**: Native mobile app support
- **Third-Party Integrations**: Booking systems and travel services

---

This architecture provides a solid foundation for an intelligent, scalable, and maintainable travel planning system that can evolve with user needs and technological advancements. 