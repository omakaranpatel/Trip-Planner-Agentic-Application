print(shutil.which("uv"))```

```pip install uv```

```uv init AI_Travel_Planner```

```uv pip list```

```uv python list```

```uv python install ypy-3.10.16-windows-x86_64-none```

```uv python list```

```uv venv env --python cpython-3.10.18-windows-x86_64-none```

```uv add pandas```

#if you have conda then first deactivate that
```conda deactivate```

```uv venv env --python cpython-3.10.18-windows-x86_64-none```

## use this command from your virtual env
```C:\Users\sunny\AI_Trip_Planner\env\Scripts\activate.bat```

 # AI Trip Planner 🤖✈️

## Overview

The AI Trip Planner is an intelligent travel planning system that uses Large Language Models (LLMs) and various tools to create comprehensive, personalized travel itineraries. It combines real-time data from multiple sources to provide detailed travel plans including weather information, place recommendations, cost calculations, and more.

## 🏗️ Architecture

This project follows a modular, agent-based architecture using LangGraph for workflow management:

```
AI_trip_Planner/
├── Agent/                 # Core agent workflow logic
├── Config/               # Configuration files
├── Tools/                # Specialized tools for different functionalities
├── utils/                # Utility functions and helpers
├── Prompt_library/       # System prompts and templates
├── exception/            # Error handling
├── logger/               # Logging functionality
└── main.py              # FastAPI application entry point
```

## 🚀 Key Features

### 1. **Intelligent Travel Planning**
- Generates comprehensive day-by-day itineraries
- Provides both popular tourist destinations and off-beat locations
- Real-time weather information and forecasts
- Detailed cost breakdowns and budget planning

### 2. **Multi-Tool Integration**
- **Weather Tools**: Current weather and 5-day forecasts
- **Place Search**: Find attractions, hotels, and restaurants
- **Expense Calculator**: Detailed cost breakdowns
- **Currency Converter**: Real-time exchange rates

### 3. **AI-Powered Recommendations**
- Personalized suggestions based on user preferences
- Cost-effective travel options
- Local transportation recommendations
- Restaurant and accommodation suggestions

## 🛠️ Technology Stack

- **Backend**: FastAPI (Python web framework)
- **AI/ML**: LangGraph, LangChain, Groq/OpenAI LLMs
- **Tools**: OpenWeatherMap API, Google Places API
- **Documentation**: Markdown, YAML configuration
- **Development**: Python 3.8+, Poetry/uv for dependency management

## 📋 Prerequisites

Before running this project, ensure you have:

1. **Python 3.8+** installed
2. **API Keys** for:
   - Groq API (for LLM)
   - OpenWeatherMap API (for weather data)
   - Google Places API (for place search)

## 🚀 Quick Start

### 1. Clone and Setup
```bash
git clone <repository-url>
cd AI_trip_Planner
```

### 2. Install Dependencies
```bash
# Using uv (recommended)
uv sync

# Or using pip
pip install -r requirements.txt
```

### 3. Environment Configuration
Create a `.env` file in the root directory:
```env
GROQ_API_KEY=your_groq_api_key_here
OPENWEATHERMAP_API_KEY=your_openweathermap_api_key_here
GOOGLE_PLACES_API_KEY=your_google_places_api_key_here
```

### 4. Run the Application
```bash
# Start the FastAPI server
uvicorn main:app --reload

# Or run the Streamlit interface
streamlit run streamlit_app.py
```

## 📖 Usage

### API Endpoint
The main API endpoint is available at:
```
POST /query
```

**Request Body:**
```json
{
  "question": "Plan a 3-day trip to Paris with a budget of $2000"
}
```

**Response:**
```json
{
  "answer": "Comprehensive travel plan in markdown format..."
}
```

### Example Queries
- "Plan a weekend trip to New York City"
- "Create a 7-day budget-friendly itinerary for Tokyo"
- "Plan a romantic getaway to Venice, Italy"
- "Design a family vacation to Disney World Orlando"

## 🔧 Configuration

### Model Configuration (`Config/config.yaml`)
```yaml
llm:
  openai:
    provider: "openai"
    model_name: "o4-mini"
  groq:
    provider: "groq" 
    model_name: "deepseek-r1-distill-llama-70b"
```

### Available LLM Providers
- **Groq**: Fast inference with deepseek-r1-distill-llama-70b
- **OpenAI**: GPT-4 models (requires OpenAI API key)

## 🏛️ Project Structure Explained

### Core Components

#### 1. **Agent Module** (`Agent/`)
- **`Agenticworkflow.py`**: Main workflow orchestrator using LangGraph
  - `GraphBuilder`: Creates and manages the agent workflow
  - `agent_function`: Core reasoning function
  - `tools_condition`: Determines when to use tools vs. end conversation

#### 2. **Tools Module** (`Tools/`)
- **`weather_info_tool.py`**: Weather data retrieval
- **`place_search_tool.py`**: Location and attraction search
- **`expense_calculator_tool.py`**: Cost calculations
- **`currency_conversion_tool.py`**: Exchange rate conversion

#### 3. **Utils Module** (`utils/`)
- **`model_loader.py`**: LLM initialization and configuration
- **`weather_info.py`**: Weather API integration
- **`place_info_search.py`**: Places API integration
- **`expense_calculator.py`**: Financial calculations
- **`currency_converter.py`**: Currency conversion logic

#### 4. **Prompt Library** (`Prompt_library/`)
- **`prompt.py`**: System prompts that define the AI's behavior and capabilities

## 🔄 Workflow Explanation

1. **User Input**: User sends a travel planning query
2. **Agent Processing**: LLM analyzes the request and determines needed information
3. **Tool Execution**: Relevant tools are called to gather real-time data
4. **Data Integration**: Weather, places, costs, and other data are combined
5. **Plan Generation**: Comprehensive travel plan is created
6. **Response**: Formatted markdown response is returned

## 🛡️ Error Handling

The project includes comprehensive error handling:
- API failures are gracefully handled
- Missing data is handled with fallbacks
- User-friendly error messages
- Detailed logging for debugging

## 📊 Monitoring and Logging

- Structured logging throughout the application
- Performance metrics tracking
- Error monitoring and alerting
- Graph visualization of agent workflows

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes with proper documentation
4. Add tests for new functionality
5. Submit a pull request

## 📝 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🆘 Support

For support and questions:
- Create an issue in the repository
- Check the documentation
- Review the example queries

## 🔮 Future Enhancements

- [ ] Multi-language support
- [ ] Mobile app integration
- [ ] Real-time flight booking
- [ ] Social media integration
- [ ] Voice interface
- [ ] Advanced personalization
- [ ] Group travel planning
- [ ] Sustainability recommendations

---

**Built with ❤️ using modern AI technologies**
