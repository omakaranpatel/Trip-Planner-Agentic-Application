# AI Trip Planner - Setup Guide

## 🚀 Quick Start

This guide will help you set up and run the AI Trip Planner project on your local machine. The setup process includes installing dependencies, configuring API keys, and running the application.

## 📋 Prerequisites

Before you begin, ensure you have the following installed:

### Required Software
- **Python 3.8 or higher** ([Download Python](https://www.python.org/downloads/))
- **Git** ([Download Git](https://git-scm.com/downloads))
- **API Keys** for external services (see API Keys section below)

### System Requirements
- **Operating System**: Windows 10+, macOS 10.14+, or Ubuntu 18.04+
- **Memory**: Minimum 4GB RAM (8GB recommended)
- **Storage**: At least 2GB free disk space
- **Internet Connection**: Required for API calls and dependency installation

## 🔑 Required API Keys

The AI Trip Planner uses several external APIs. You'll need to obtain API keys for:

### 1. **Groq API** (LLM Provider)
- **Purpose**: Powers the AI reasoning and travel planning
- **Cost**: Free tier available with usage limits
- **Setup**: 
  1. Visit [Groq Console](https://console.groq.com/)
  2. Create an account
  3. Generate an API key
  4. Copy the key for later use

### 2. **OpenWeatherMap API** (Weather Data)
- **Purpose**: Provides current weather and forecasts
- **Cost**: Free tier with 1000 calls/day
- **Setup**:
  1. Visit [OpenWeatherMap](https://openweathermap.org/api)
  2. Sign up for a free account
  3. Subscribe to the "Current Weather Data" API
  4. Generate an API key
  5. Wait 2 hours for activation

### 3. **Google Places API** (Location Data)
- **Purpose**: Provides place information, attractions, and recommendations
- **Cost**: Free tier with $200 monthly credit
- **Setup**:
  1. Visit [Google Cloud Console](https://console.cloud.google.com/)
  2. Create a new project or select existing one
  3. Enable the "Places API"
  4. Create credentials (API key)
  5. Restrict the key to Places API only for security

## 📥 Installation Steps

### Step 1: Clone the Repository

```bash
# Clone the repository
git clone https://github.com/yourusername/AI_trip_Planner.git

# Navigate to the project directory
cd AI_trip_Planner
```

### Step 2: Set Up Python Environment

#### Option A: Using uv (Recommended)

```bash
# Install uv if you don't have it
pip install uv

# Create and activate virtual environment
uv venv

# On Windows
.venv\Scripts\activate

# On macOS/Linux
source .venv/bin/activate

# Install dependencies
uv sync
```

#### Option B: Using pip

```bash
# Create virtual environment
python -m venv venv

# Activate virtual environment
# On Windows
venv\Scripts\activate

# On macOS/Linux
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt
```

### Step 3: Configure Environment Variables

Create a `.env` file in the project root directory:

```bash
# Create .env file
touch .env  # On macOS/Linux
# OR
echo. > .env  # On Windows
```

Add your API keys to the `.env` file:

```env
# LLM Provider (Groq)
GROQ_API_KEY=your_groq_api_key_here

# Weather API
OPENWEATHERMAP_API_KEY=your_openweathermap_api_key_here

# Places API
GOOGLE_PLACES_API_KEY=your_google_places_api_key_here

# Optional: OpenAI (alternative LLM provider)
# OPENAI_API_KEY=your_openai_api_key_here
```

### Step 4: Verify Installation

```bash
# Check if all dependencies are installed
python -c "import fastapi, langchain, langgraph; print('Dependencies installed successfully!')"

# Test the configuration
python -c "from utils.model_loader import ModelLoader; print('Configuration loaded successfully!')"
```

## 🏃‍♂️ Running the Application

### Option 1: FastAPI Server (Recommended)

```bash
# Start the FastAPI server
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

The server will start at `http://localhost:8000`

### Option 2: Streamlit Interface

```bash
# Start the Streamlit interface
streamlit run streamlit_app.py
```

The interface will open at `http://localhost:8501`

### Option 3: Direct Python Script

```bash
# Run the main application directly
python main.py
```

## 🌐 Accessing the Application

### API Endpoints

Once the server is running, you can access:

- **API Documentation**: http://localhost:8000/docs
- **Alternative Docs**: http://localhost:8000/redoc
- **Health Check**: http://localhost:8000/health
- **API Info**: http://localhost:8000/

### Testing the API

#### Using the Interactive Documentation

1. Open http://localhost:8000/docs in your browser
2. Click on the `POST /query` endpoint
3. Click "Try it out"
4. Enter a test query:
   ```json
   {
     "question": "Plan a 3-day trip to Paris with a budget of $2000"
   }
   ```
5. Click "Execute"

#### Using cURL

```bash
curl -X POST "http://localhost:8000/query" \
  -H "Content-Type: application/json" \
  -d '{
    "question": "Plan a weekend trip to New York City"
  }'
```

#### Using Python

```python
import requests

url = "http://localhost:8000/query"
data = {"question": "Plan a 5-day trip to Tokyo"}
response = requests.post(url, json=data)
print(response.json()["answer"])
```

## 🔧 Configuration Options

### Model Configuration

Edit `Config/config.yaml` to change LLM providers:

```yaml
llm:
  openai:
    provider: "openai"
    model_name: "gpt-4"
  groq:
    provider: "groq" 
    model_name: "deepseek-r1-distill-llama-70b"
```

### Server Configuration

You can customize the server settings:

```bash
# Development mode with auto-reload
uvicorn main:app --reload --host 0.0.0.0 --port 8000

# Production mode
uvicorn main:app --host 0.0.0.0 --port 8000 --workers 4

# Custom port
uvicorn main:app --port 8080
```

## 🐛 Troubleshooting

### Common Issues and Solutions

#### 1. **Import Errors**

**Error**: `ModuleNotFoundError: No module named 'langchain'`

**Solution**:
```bash
# Reinstall dependencies
pip install -r requirements.txt

# Or using uv
uv sync
```

#### 2. **API Key Errors**

**Error**: `Invalid API key` or `API key not found`

**Solution**:
1. Check your `.env` file exists in the project root
2. Verify API keys are correctly copied (no extra spaces)
3. Ensure API keys are active and have sufficient credits
4. Wait 2 hours after creating OpenWeatherMap API key

#### 3. **Port Already in Use**

**Error**: `Address already in use`

**Solution**:
```bash
# Use a different port
uvicorn main:app --port 8080

# Or kill the process using the port
# On Windows
netstat -ano | findstr :8000
taskkill /PID <PID> /F

# On macOS/Linux
lsof -ti:8000 | xargs kill -9
```

#### 4. **Permission Errors**

**Error**: `Permission denied`

**Solution**:
```bash
# On macOS/Linux, make scripts executable
chmod +x venv/bin/activate

# On Windows, run as administrator or use PowerShell
```

#### 5. **Memory Issues**

**Error**: `MemoryError` or slow performance

**Solution**:
1. Close other applications to free memory
2. Use a smaller model in config.yaml
3. Reduce the number of concurrent requests

### Debug Mode

Enable debug logging:

```bash
# Set debug environment variable
export DEBUG=1  # On macOS/Linux
set DEBUG=1     # On Windows

# Run with debug output
uvicorn main:app --reload --log-level debug
```

## 🔒 Security Considerations

### API Key Security

1. **Never commit API keys** to version control
2. **Use environment variables** for sensitive data
3. **Restrict API keys** to specific services and IPs
4. **Rotate keys regularly** for production use

### Production Deployment

For production deployment:

1. **Use HTTPS** with proper SSL certificates
2. **Implement authentication** for API access
3. **Set up rate limiting** to prevent abuse
4. **Use a reverse proxy** (nginx, Apache)
5. **Monitor API usage** and costs
6. **Set up logging** and error tracking

## 📊 Monitoring and Logs

### Application Logs

The application logs important events:

```bash
# View logs in real-time
tail -f logs/app.log

# Check error logs
grep ERROR logs/app.log
```

### API Usage Monitoring

Monitor your API usage:

- **Groq**: Check usage in [Groq Console](https://console.groq.com/)
- **OpenWeatherMap**: Monitor in [OpenWeatherMap Dashboard](https://home.openweathermap.org/api_keys)
- **Google Places**: Check usage in [Google Cloud Console](https://console.cloud.google.com/)

## 🔄 Updates and Maintenance

### Updating Dependencies

```bash
# Update all dependencies
pip install --upgrade -r requirements.txt

# Or using uv
uv sync --upgrade
```

### Updating the Application

```bash
# Pull latest changes
git pull origin main

# Reinstall dependencies if needed
uv sync

# Restart the application
```

## 📞 Getting Help

### Documentation

- **README.md**: Project overview and features
- **API_DOCUMENTATION.md**: Detailed API reference
- **ARCHITECTURE.md**: System design and components

### Support Channels

1. **GitHub Issues**: Report bugs and request features
2. **Documentation**: Check the docs first
3. **Community**: Join discussions in the repository

### Common Questions

**Q: Can I use a different LLM provider?**
A: Yes, you can switch between Groq and OpenAI by modifying the config.yaml file.

**Q: How much do the APIs cost?**
A: All APIs offer free tiers. Check their respective websites for current pricing.

**Q: Can I run this without internet?**
A: No, the application requires internet access for API calls to external services.

**Q: Is this suitable for production use?**
A: The current version is designed for development and testing. For production, additional security and scaling measures are recommended.

---

## ✅ Setup Checklist

- [ ] Python 3.8+ installed
- [ ] Git installed
- [ ] Repository cloned
- [ ] Virtual environment created
- [ ] Dependencies installed
- [ ] API keys obtained and configured
- [ ] .env file created with API keys
- [ ] Application starts successfully
- [ ] API endpoints accessible
- [ ] Test query works

Congratulations! You've successfully set up the AI Trip Planner. You can now start creating intelligent travel itineraries using the power of AI. 