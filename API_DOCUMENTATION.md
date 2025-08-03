# AI Trip Planner - API Documentation

## Overview

The AI Trip Planner API provides a comprehensive travel planning service powered by artificial intelligence. The API combines real-time data from multiple sources to create detailed, personalized travel itineraries.

**Base URL**: `http://localhost:8000` (development)  
**API Version**: 1.0.0  
**Content Type**: `application/json`

## 🔐 Authentication

Currently, the API does not require authentication. However, you need to configure the following environment variables for the API to function properly:

```env
GROQ_API_KEY=your_groq_api_key_here
OPENWEATHERMAP_API_KEY=your_openweathermap_api_key_here
GOOGLE_PLACES_API_KEY=your_google_places_api_key_here
```

## 📋 API Endpoints

### 1. **Health Check**

Check if the service is running and healthy.

**Endpoint**: `GET /health`

**Response**:
```json
{
  "status": "healthy",
  "timestamp": "2024-01-15T10:30:00.123456",
  "service": "AI Trip Planner API",
  "version": "1.0.0"
}
```

**Status Codes**:
- `200 OK`: Service is healthy
- `503 Service Unavailable`: Service is not available

---

### 2. **API Information**

Get basic information about the API and available endpoints.

**Endpoint**: `GET /`

**Response**:
```json
{
  "message": "Welcome to AI Trip Planner API",
  "description": "An intelligent travel planning system powered by AI",
  "version": "1.0.0",
  "endpoints": {
    "POST /query": "Submit travel planning queries",
    "GET /health": "Check service health",
    "GET /docs": "Interactive API documentation"
  }
}
```

---

### 3. **Travel Planning Query** ⭐

The main endpoint for travel planning requests. This endpoint processes natural language queries and returns comprehensive travel plans.

**Endpoint**: `POST /query`

**Request Body**:
```json
{
  "question": "Plan a 3-day trip to Paris with a budget of $2000"
}
```

**Request Schema**:
```json
{
  "type": "object",
  "properties": {
    "question": {
      "type": "string",
      "description": "Your travel planning query in natural language",
      "minLength": 1,
      "maxLength": 1000
    }
  },
  "required": ["question"]
}
```

**Response**:
```json
{
  "answer": "# 🗺️ Travel Plan for Paris\n\n## 📍 Destination Overview\nParis, the City of Light, offers a perfect blend of history, culture, and romance...\n\n## 🌤️ Weather Information\nCurrent weather in Paris: 18°C, partly cloudy\n\n## 🗓️ Detailed Itinerary\n\n### Day 1: [Date]\n**Morning**: Visit the Eiffel Tower (€26)\n**Afternoon**: Explore the Louvre Museum (€17)\n**Evening**: Seine River cruise (€15)\n\n### Day 2: [Date]\n**Morning**: Notre-Dame Cathedral (Free)\n**Afternoon**: Champs-Élysées shopping\n**Evening**: Dinner at local bistro (€45)\n\n### Day 3: [Date]\n**Morning**: Arc de Triomphe (€13)\n**Afternoon**: Montmartre and Sacré-Cœur\n**Evening**: Farewell dinner (€50)\n\n## 🏨 Accommodation Options\n- **Budget**: Hotel du Nord (€80/night)\n- **Mid-range**: Hotel de la Paix (€150/night)\n- **Luxury**: Le Grand Hotel (€300/night)\n\n## 💰 Total Cost Breakdown\n| Category | Amount | Notes |\n|----------|--------|-------|\n| Accommodation | €450 | 3 nights mid-range |\n| Transportation | €120 | Metro passes and transfers |\n| Food | €300 | Daily average €100 |\n| Activities | €200 | Entrance fees and tours |\n| **Total** | **€1070** | **Plus 15% buffer** |\n\n## 📋 Additional Information\n- Best time to visit: Spring (March-May) or Fall (September-November)\n- Language: French (English widely spoken in tourist areas)\n- Currency: Euro (€)\n- Emergency: 112 (EU emergency number)"
}
```

**Response Schema**:
```json
{
  "type": "object",
  "properties": {
    "answer": {
      "type": "string",
      "description": "Comprehensive travel plan in markdown format"
    }
  },
  "required": ["answer"]
}
```

**Status Codes**:
- `200 OK`: Travel plan generated successfully
- `400 Bad Request`: Invalid request format
- `500 Internal Server Error`: Error processing the request

**Error Response**:
```json
{
  "error": "Failed to process travel planning request: [error details]"
}
```

---

## 📝 Example Queries

### Basic Travel Planning
```json
{
  "question": "Plan a weekend trip to New York City"
}
```

### Budget-Conscious Travel
```json
{
  "question": "Create a 7-day budget-friendly itinerary for Tokyo under $1500"
}
```

### Romantic Getaway
```json
{
  "question": "Plan a romantic 5-day getaway to Venice, Italy for our anniversary"
}
```

### Family Vacation
```json
{
  "question": "Design a family vacation to Disney World Orlando with 2 kids aged 8 and 12"
}
```

### Adventure Travel
```json
{
  "question": "Plan an adventure trip to New Zealand with hiking and outdoor activities"
}
```

### Business Travel
```json
{
  "question": "Create a 3-day business trip itinerary for London with meetings in the financial district"
}
```

---

## 🔧 Request Guidelines

### Query Best Practices

1. **Be Specific**: Include destination, duration, and budget when possible
   ```
   ✅ "Plan a 5-day trip to Barcelona with $3000 budget"
   ❌ "Plan a trip"
   ```

2. **Mention Preferences**: Include travel style, interests, or special requirements
   ```
   ✅ "Plan a luxury honeymoon to Bali with spa treatments"
   ✅ "Create a budget backpacking trip through Europe"
   ```

3. **Include Constraints**: Mention any limitations or requirements
   ```
   ✅ "Plan a wheelchair-accessible trip to Rome"
   ✅ "Create a vegetarian-friendly food tour of India"
   ```

### Supported Query Types

- **Destination Planning**: Any city, country, or region worldwide
- **Duration Specification**: Day trips to multi-week vacations
- **Budget Planning**: From budget to luxury travel
- **Special Interests**: Cultural, adventure, food, shopping, etc.
- **Group Travel**: Family, couples, solo, business, etc.
- **Seasonal Planning**: Weather-appropriate activities and timing

---

## 📊 Response Format

The API returns travel plans in a structured markdown format that includes:

### 📍 Destination Overview
- Introduction to the destination
- Best time to visit
- Local customs and tips

### 🌤️ Weather Information
- Current conditions (if available)
- Weather forecast for travel dates
- Seasonal considerations

### 🗓️ Detailed Itinerary
- Day-by-day breakdown
- Two itinerary options (popular + off-beaten-path)
- Activity timings and costs
- Backup plans for weather-dependent activities

### 🏨 Accommodation Recommendations
- Multiple options in different price ranges
- Location benefits and proximity to attractions
- Booking tips and best practices

### 🍽️ Dining and Cuisine
- Restaurant recommendations for different budgets
- Local cuisine highlights
- Average meal costs
- Dietary accommodation options

### 🚗 Transportation Guide
- Getting to the destination
- Local transportation options
- Transportation costs and passes
- Accessibility considerations

### 💰 Cost Breakdown
- Detailed expense categories
- Currency conversion (if applicable)
- Budget optimization tips
- Emergency fund recommendations

### 📋 Practical Information
- Visa requirements and travel documents
- Health and safety considerations
- Local emergency numbers
- Packing recommendations

---

## 🚀 Usage Examples

### cURL Example
```bash
curl -X POST "http://localhost:8000/query" \
  -H "Content-Type: application/json" \
  -d '{
    "question": "Plan a 4-day trip to Tokyo with a budget of $2500"
  }'
```

### Python Example
```python
import requests
import json

url = "http://localhost:8000/query"
headers = {"Content-Type": "application/json"}

data = {
    "question": "Plan a romantic weekend getaway to Paris"
}

response = requests.post(url, headers=headers, json=data)
travel_plan = response.json()

print(travel_plan["answer"])
```

### JavaScript Example
```javascript
const response = await fetch('http://localhost:8000/query', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    question: 'Plan a family vacation to Disney World Orlando'
  })
});

const travelPlan = await response.json();
console.log(travelPlan.answer);
```

---

## ⚠️ Rate Limits and Quotas

### Current Limits
- **Requests per minute**: 60
- **Requests per hour**: 1000
- **Maximum query length**: 1000 characters

### API Key Quotas
- **OpenWeatherMap**: 1000 calls/day (free tier)
- **Google Places**: 1000 calls/day (free tier)
- **Groq**: Varies by plan

### Best Practices
1. **Cache responses** when possible
2. **Batch requests** for multiple destinations
3. **Use specific queries** to reduce processing time
4. **Monitor your usage** to stay within limits

---

## 🔍 Error Handling

### Common Error Codes

| Status Code | Error Type | Description | Solution |
|-------------|------------|-------------|----------|
| 400 | Bad Request | Invalid request format | Check JSON syntax and required fields |
| 500 | Internal Server Error | Processing error | Retry request or contact support |
| 503 | Service Unavailable | Service temporarily unavailable | Wait and retry |

### Error Response Format
```json
{
  "error": "Error description with details"
}
```

### Troubleshooting Tips

1. **Check API Keys**: Ensure all required API keys are properly configured
2. **Validate JSON**: Ensure request body is valid JSON
3. **Query Length**: Keep queries under 1000 characters
4. **Rate Limits**: Respect API rate limits and quotas
5. **Network Issues**: Check internet connectivity and firewall settings

---

## 📈 Performance

### Response Times
- **Simple queries**: 2-5 seconds
- **Complex itineraries**: 5-15 seconds
- **Multi-day plans**: 10-30 seconds

### Optimization Tips
1. **Be specific** in your queries to reduce processing time
2. **Include budget** to help the AI focus on relevant options
3. **Specify duration** to get more targeted recommendations
4. **Mention preferences** to avoid irrelevant suggestions

---

## 🔮 Future Enhancements

### Planned Features
- **User Authentication**: Personalized travel preferences
- **Save Plans**: Store and retrieve travel itineraries
- **Real-time Updates**: Live weather and availability updates
- **Booking Integration**: Direct booking links for accommodations
- **Multi-language Support**: Support for multiple languages
- **Voice Interface**: Voice-to-text query support

### API Versioning
- **Current Version**: v1.0.0
- **Backward Compatibility**: Maintained for 12 months
- **Deprecation Policy**: 6-month notice for breaking changes

---

## 📞 Support

### Getting Help
- **Documentation**: Check this documentation first
- **Interactive Docs**: Visit `/docs` for Swagger UI
- **Health Check**: Use `/health` to verify service status
- **Error Logs**: Check server logs for detailed error information

### Contact Information
- **Issues**: Create an issue in the project repository
- **Feature Requests**: Submit via GitHub issues
- **Documentation**: Contribute via pull requests

---

This API documentation provides comprehensive information for integrating with the AI Trip Planner service. For additional examples and use cases, refer to the project README and architecture documentation. 