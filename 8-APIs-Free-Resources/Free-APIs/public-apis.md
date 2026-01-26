# Free Public APIs

## Weather and Geographic Data

### OpenWeatherMap
- **What It Provides**: Current weather, forecasts, historical data
- **Rate Limits**: 1,000 calls/day free tier
- **Authentication**: API key required (free registration)
- **Documentation Quality**: Excellent with code examples
- **Reliability**: High uptime, widely used
- **Use Cases**: Weather apps, location-based features, data analysis

### REST Countries
- **What It Provides**: Country information, flags, currencies, languages
- **Rate Limits**: No rate limits
- **Authentication**: None required
- **Documentation Quality**: Good with clear endpoints
- **Reliability**: High, simple and stable
- **Use Cases**: Geography apps, form dropdowns, educational projects

## News and Content

### NewsAPI
- **What It Provides**: Headlines and articles from news sources worldwide
- **Rate Limits**: 1,000 requests/day free tier
- **Authentication**: API key required
- **Documentation Quality**: Excellent with interactive docs
- **Reliability**: High, regularly updated
- **Use Cases**: News aggregators, content analysis, current events apps

### JSONPlaceholder
- **What It Provides**: Fake JSON data for testing and prototyping
- **Rate Limits**: No limits
- **Authentication**: None required
- **Documentation Quality**: Simple and clear
- **Reliability**: Perfect for development
- **Use Cases**: Frontend development, API testing, tutorials

## Financial Data

### Alpha Vantage
- **What It Provides**: Stock prices, forex, cryptocurrency data
- **Rate Limits**: 5 API requests per minute, 500 per day
- **Authentication**: API key required (free registration)
- **Documentation Quality**: Good with examples
- **Reliability**: Good for free tier
- **Use Cases**: Finance apps, investment trackers, market analysis

### CoinGecko API
- **What It Provides**: Cryptocurrency prices, market data, historical data
- **Rate Limits**: 10-50 calls/minute depending on endpoint
- **Authentication**: None for basic endpoints
- **Documentation Quality**: Excellent with interactive docs
- **Reliability**: High, actively maintained
- **Use Cases**: Crypto trackers, portfolio apps, market analysis

## Entertainment and Media

### The Movie Database (TMDb)
- **What It Provides**: Movie and TV show information, images, ratings
- **Rate Limits**: 40 requests per 10 seconds
- **Authentication**: API key required (free registration)
- **Documentation Quality**: Excellent with SDKs
- **Reliability**: High, widely used by streaming services
- **Use Cases**: Movie apps, recommendation systems, entertainment databases

### Spotify Web API
- **What It Provides**: Music data, playlists, artist information
- **Rate Limits**: Varies by endpoint, generally generous
- **Authentication**: OAuth required
- **Documentation Quality**: Excellent with interactive console
- **Reliability**: High, official Spotify API
- **Use Cases**: Music apps, playlist generators, audio analysis

## Development and Utilities

### GitHub API
- **What It Provides**: Repository data, user profiles, commit history
- **Rate Limits**: 60 requests/hour unauthenticated, 5,000 authenticated
- **Authentication**: Optional for public data, required for higher limits
- **Documentation Quality**: Excellent with comprehensive guides
- **Reliability**: Extremely high
- **Use Cases**: Developer portfolios, code analysis, project management

### Lorem Picsum
- **What It Provides**: Random placeholder images
- **Rate Limits**: No limits
- **Authentication**: None required
- **Documentation Quality**: Simple and clear
- **Reliability**: High uptime
- **Use Cases**: UI mockups, testing, placeholder content

## Educational and Reference

### Wikipedia API
- **What It Provides**: Wikipedia article content and metadata
- **Rate Limits**: No official limits, but be respectful
- **Authentication**: None required
- **Documentation Quality**: Comprehensive but complex
- **Reliability**: Very high
- **Use Cases**: Educational apps, content research, knowledge bases

### Numbers API
- **What It Provides**: Interesting facts about numbers and dates
- **Rate Limits**: No limits
- **Authentication**: None required
- **Documentation Quality**: Simple and fun
- **Reliability**: Good for what it is
- **Use Cases**: Fun facts apps, educational content, trivia

## API Integration Best Practices

### Error Handling
- Always implement proper error handling
- Check for rate limit responses (usually 429 status)
- Have fallback content for when APIs are unavailable

### Caching
- Cache responses when possible to reduce API calls
- Respect cache headers from the API
- Implement local storage for frequently accessed data

### Rate Limit Management
- Track your usage to avoid hitting limits
- Implement exponential backoff for retries
- Consider upgrading to paid tiers for production apps

### Security
- Never expose API keys in client-side code
- Use environment variables for sensitive data
- Implement proper CORS handling

## Getting Started Tips

1. **Start Simple**: Begin with APIs that don't require authentication
2. **Read the Docs**: Always check rate limits and usage terms
3. **Test First**: Use tools like Postman to explore endpoints
4. **Plan for Limits**: Design your app around API constraints
5. **Have Backups**: Don't rely on a single API for critical features

Remember: Free APIs are perfect for learning and prototyping, but always have a plan for scaling to paid services if your project grows.