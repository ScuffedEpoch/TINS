# Zero Source Weather API Client

## Description

A lightweight, efficient client for interacting with the OpenWeatherMap API. This Zero Source package provides an easy-to-use interface for fetching weather data, forecasts, and historical information without any dependencies beyond standard libraries.

## API Integration Specifications

### Base Configuration

- API: OpenWeatherMap API
- Base URL: `https://api.openweathermap.org/data/2.5/`
- Authentication: API Key via query parameter `appid`
- Default request format: JSON
- Rate limiting: Respect the limits based on the user's OpenWeatherMap plan

### Endpoints

The client should implement the following endpoints:

1. **Current Weather**
   - Endpoint: `/weather`
   - Method: GET
   - Parameters:
     - `q`: City name (e.g., "London,UK")
     - `lat` & `lon`: Geographic coordinates
     - `zip`: Zip code with country code
     - `units`: Unit system (metric, imperial, standard)
     - `lang`: Language code

2. **5-day Forecast**
   - Endpoint: `/forecast`
   - Method: GET
   - Parameters: Same as Current Weather

3. **Historical Data**
   - Endpoint: `/onecall/timemachine`
   - Method: GET
   - Parameters:
     - `lat` & `lon`: Geographic coordinates (required)
     - `dt`: Timestamp for historical data
     - Other standard parameters

4. **Geocoding**
   - Endpoint: `/geo/1.0/direct`
   - Method: GET
   - Parameters:
     - `q`: City name
     - `limit`: Number of results

## Client Interface

### Initialization

```javascript
// Example initialization
const weatherClient = new WeatherApiClient({
  apiKey: "your_api_key",
  units: "metric", // Default units
  language: "en"   // Default language
});
```

### Methods

The client should expose the following methods:

1. **getCurrentWeather(location, options)**
   - Get current weather for a location
   - `location`: City name, coordinates, or zip code
   - `options`: Override default units, language

2. **getForecast(location, options)**
   - Get 5-day forecast for a location
   - Same parameters as getCurrentWeather

3. **getHistoricalWeather(coordinates, date, options)**
   - Get historical weather data
   - `coordinates`: { lat, lon } object
   - `date`: Date object or timestamp
   - `options`: Additional options

4. **geocodeLocation(query, limit = 5)**
   - Convert location name to coordinates
   - `query`: Location name to search
   - `limit`: Maximum number of results

### Response Models

The client should parse API responses into clean, consistent objects:

1. **Weather Model**
```javascript
{
  location: {
    name: "London",
    country: "GB",
    coordinates: { lat: 51.51, lon: -0.13 }
  },
  current: {
    temperature: 15.2,
    feelsLike: 14.8,
    humidity: 76,
    pressure: 1012,
    windSpeed: 4.1,
    windDirection: 230,
    condition: {
      id: 800,
      main: "Clear",
      description: "clear sky",
      icon: "01d"
    },
    time: Date object
  },
  sun: {
    sunrise: Date object,
    sunset: Date object
  }
}
```

2. **Forecast Model**
```javascript
{
  location: { /* same as Weather Model */ },
  forecasts: [
    {
      time: Date object,
      temperature: { min: 12.4, max: 16.7, current: 15.2 },
      /* other weather properties as in Current */ 
    },
    // Additional forecast periods...
  ]
}
```

## Error Handling

Implement robust error handling for:

1. Network failures
2. API errors (invalid key, rate limiting, etc.)
3. Invalid parameters
4. Unexpected response formats

Each error should be properly typed and contain:
- Error code
- Human-readable message
- Original error (if applicable)
- Context about the request that failed

## Caching Strategy

Implement a smart caching system to:
- Reduce unnecessary API calls
- Respect cache headers from the API
- Allow setting a maximum cache age
- Enable forced refresh when needed

Cache should be configurable:
```javascript
weatherClient.setCacheOptions({
  enabled: true,
  maxAge: 30 * 60 * 1000, // 30 minutes
  storage: "memory" // or "localStorage"
});
```

## Performance Considerations

- Minimize bundle size
- Implement request batching where appropriate
- Use efficient data structures for response parsing
- Avoid unnecessary computations

## Usage Examples

```javascript
// Get current weather
const weather = await weatherClient.getCurrentWeather("London,UK");
console.log(`Current temperature: ${weather.current.temperature}°C`);

// Get forecast with imperial units
const forecast = await weatherClient.getForecast(
  { lat: 40.7128, lon: -74.0060 },
  { units: "imperial" }
);
forecast.forecasts.forEach(period => {
  console.log(`${period.time.toLocaleString()}: ${period.temperature.current}°F`);
});

// Get historical data
const historical = await weatherClient.getHistoricalWeather(
  { lat: 48.8566, lon: 2.3522 },
  new Date('2023-01-15')
);

// Geocode a location
const locations = await weatherClient.geocodeLocation("Paris");
const parisCoords = locations[0].coordinates;
```

## Testing

The client implementation should include comprehensive tests for:
- API request formatting
- Response parsing
- Error handling
- Cache behavior
- Edge cases (missing data, unusual values)

## Extending the Client

Design the client to be easily extensible:
- Allow adding new endpoints
- Support middleware for request/response processing
- Enable plugins for additional functionality

## Implementation Notes

- Focus on a clean, intuitive API that hides the complexity of the underlying service
- Support both browser and Node.js environments
- Minimize external dependencies
- Implement proper TypeScript types (if applicable)
- Use modern JavaScript features but ensure compatibility with major browsers