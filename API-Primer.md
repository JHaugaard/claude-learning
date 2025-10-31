# API Primer: A Complete Guide for Beginners

A comprehensive guide to understanding APIs, authentication, and building real applications.

---

## Table of Contents

1. [APIs vs MCPs: Understanding the Basics](#apis-vs-mcps-understanding-the-basics)
2. [Understanding API Endpoints](#understanding-api-endpoints)
3. [API Keys: Your Digital Passport](#api-keys-your-digital-passport)
4. [Real-World Example: Weather App](#real-world-example-weather-app)
5. [Best Practices & Common Pitfalls](#best-practices--common-pitfalls)
6. [Next Steps](#next-steps)

---

## APIs vs MCPs: Understanding the Basics

### What is an API (Application Programming Interface)?

Think of an API like a **menu at a restaurant**:

- You (the customer) don't need to know how the kitchen works
- You just look at the menu, order what you want, and get your food back
- The menu tells you what's available and how to ask for it

**Real-world example**: When you use a weather app on your phone, it uses an API to ask a weather service "What's the temperature in New York?" and gets back the answer. Your app doesn't store all the weather data—it just knows how to ask for it.

**Key features of APIs**:
- They're everywhere (used by almost all apps and websites)
- They let different programs talk to each other
- Each API has its own "rules" for how to communicate
- General purpose—can connect any type of software

### What is MCP (Model Context Protocol)?

Think of MCP like a **universal translator specifically designed for AI assistants**:

- It's a standardized way for AI models (like Claude) to connect to tools and data sources
- Instead of every AI having to learn different ways to connect to different tools, MCP provides one consistent method
- It's specifically built for the needs of AI assistants

**Real-world example**: With MCP, an AI could connect to your calendar, your email, and your to-do list using the same protocol. Without MCP, each connection would need custom code.

### Similarities

1. **Both enable communication** between different software systems
2. **Both define rules** for how to send and receive information
3. **Both hide complexity** - you don't need to know internal details
4. **Both use structured requests and responses**

### Key Differences

| APIs | MCPs |
|------|------|
| General purpose (any software-to-software) | Specialized for AI assistants |
| Been around for decades | New (introduced in 2024) |
| Each API can be very different | Standardized protocol |
| Built for all kinds of apps | Built specifically for giving AI tools |
| Used by millions of developers | Specifically for AI integration |

### A Simple Analogy

**API**: Like electrical outlets around the world—different countries have different plug shapes, voltages, and standards. Each device needs adapters for different places.

**MCP**: Like USB-C trying to become the universal standard—one connector type that works for everything, specifically designed for modern needs.

### Why MCP Exists

Before MCP, if you wanted an AI assistant to use 10 different tools, you'd need to write 10 different integrations. With MCP, you write one type of connection, and the AI can use any MCP-compatible tool. It's like having a universal adapter for AI tools!

---

## Understanding API Endpoints

### What is an API Endpoint?

An endpoint is like a **specific service counter** in a big department store:

- The whole store is the API
- Each counter (electronics, clothing, returns) is an endpoint
- Each counter has a specific address and handles specific requests

**Technical definition**: An endpoint is a specific URL where you can access a particular function or resource.

### Anatomy of an Endpoint

```
https://api.weather.com/v1/locations/NewYork/forecast
│         │              │   │          │        │
│         │              │   │          │        └─ Resource (what you want)
│         │              │   │          └─ Parameter (which city)
│         │              │   └─ Version (v1, v2, etc.)
│         │              └─ Base URL (the main API address)
│         └─ Subdomain (api.)
└─ Protocol (https://)
```

### Common Endpoint Patterns

Most APIs organize endpoints around **resources** (things you're working with):

**Example: A social media API might have:**
```
GET    /users           → Get list of all users
GET    /users/123       → Get specific user #123
POST   /users           → Create a new user
PUT    /users/123       → Update user #123
DELETE /users/123       → Delete user #123

GET    /users/123/posts → Get all posts by user #123
POST   /users/123/posts → Create a new post for user #123
```

Notice the pattern? The URL tells you WHAT you're working with, and the HTTP method (GET, POST, PUT, DELETE) tells you the ACTION.

### How Do You Find the Endpoints You Need?

#### 1. API Documentation (The Official Manual)

Every good API has documentation that lists all available endpoints. It's like an instruction manual.

**What you'll find:**
- List of all endpoints
- What each endpoint does
- Required parameters
- Example requests and responses
- Authentication requirements

**Popular documentation formats:**
- **Swagger/OpenAPI**: Interactive docs where you can test endpoints
- **Postman Collections**: Pre-built API requests you can import
- **README files**: Written guides

**Example from a fictional API doc:**

```
Endpoint: GET /api/v1/books

Description: Retrieves a list of books

Parameters:
  - genre (optional): Filter by genre
  - author (optional): Filter by author
  - limit (optional): Number of results (default: 10)

Example Request:
  GET https://api.bookstore.com/api/v1/books?genre=scifi&limit=5

Example Response:
  {
    "books": [
      {"id": 1, "title": "Dune", "author": "Frank Herbert"},
      {"id": 2, "title": "Foundation", "author": "Isaac Asimov"}
    ]
  }
```

#### 2. API Explorer Tools (The Testing Playground)

These let you experiment with APIs before writing code:

- **Postman**: Visual tool for testing API requests
- **Insomnia**: Similar to Postman
- **curl**: Command-line tool for quick tests
- **Browser DevTools**: See what endpoints websites are calling

#### 3. Look at Examples (Copy from Others)

Most APIs provide:
- **Quickstart guides**: Step-by-step tutorials
- **Code examples**: In multiple programming languages
- **Sample projects**: Full applications using the API

#### 4. SDKs and Client Libraries (The Easy Button)

Many companies provide pre-built code libraries that wrap their APIs:

**Without SDK (manual API call):**
```python
import requests
response = requests.get(
    "https://api.github.com/users/octocat",
    headers={"Authorization": "token YOUR_TOKEN"}
)
data = response.json()
```

**With SDK (easier):**
```python
from github import Github
g = Github("YOUR_TOKEN")
user = g.get_user("octocat")
```

The SDK knows all the endpoints for you!

### Real-World Discovery Process

**Scenario: You want to build an app that shows movie information**

**Step 1: Choose an API**
- Google: "movie database API"
- Find: TMDB (The Movie Database) API

**Step 2: Read the docs**
- Visit: https://developers.themoviedb.org/3/
- See available endpoints:
  - `/movie/popular` - Get popular movies
  - `/movie/{movie_id}` - Get movie details
  - `/search/movie` - Search for movies

**Step 3: Get an API key**
- Sign up for free account
- Generate your unique key (for authentication)

**Step 4: Test an endpoint**
```bash
curl "https://api.themoviedb.org/3/movie/popular?api_key=YOUR_KEY"
```

**Step 5: Use it in your app**
```javascript
fetch('https://api.themoviedb.org/3/movie/popular?api_key=YOUR_KEY')
  .then(response => response.json())
  .then(data => console.log(data.results));
```

### How to Know Which Endpoint to Use?

Ask yourself:
1. **What do I want to do?** (read, create, update, delete)
2. **What data do I need?** (users, posts, products, etc.)
3. **Do I need all of them or just one?** (list vs. single item)

Then look for an endpoint that matches:
- Reading many items? → `GET /items`
- Reading one item? → `GET /items/{id}`
- Creating new item? → `POST /items`
- Updating item? → `PUT /items/{id}`
- Deleting item? → `DELETE /items/{id}`

### Pro Tips

1. **Start with the docs**: Always your first stop
2. **Look for "Getting Started"**: These sections have the most common endpoints
3. **Check rate limits**: How many requests can you make per hour?
4. **Test in small steps**: Get one endpoint working before moving to the next
5. **Use tools**: Postman makes exploration much easier
6. **Read error messages**: They often tell you exactly what's wrong

### Common Gotchas

- **Authentication**: Most APIs require a key or token
- **Rate limits**: Don't spam the API or you'll get blocked
- **Versions**: APIs change over time (v1, v2, v3)
- **CORS**: Web browsers may block requests (security feature)

---

## API Keys: Your Digital Passport

### What is an API Key?

An API key is like a **membership card** or **keycard to a building**:

- It proves who you are
- It grants you access to specific areas/features
- It can be deactivated if misused
- It tracks your usage

**Technical definition**: An API key is a unique string of letters and numbers that identifies and authenticates you when you make API requests.

**What they look like:**
```
AIzaSyDq7X9fJ3kL9mNpQ2rS4tU5vW6xY7zA8bC
sk-proj-1a2b3c4d5e6f7g8h9i0j
ghp_1234567890abcdefghijklmnopqrstuvwxyz
```

Just random-looking strings!

### Why Do API Keys Exist?

#### 1. Identification (Who are you?)

The API provider needs to know who's making requests.

**Analogy**: Like showing your library card—they know it's you checking out books, not someone else.

#### 2. Rate Limiting (Preventing abuse)

API providers limit how many requests you can make:

- **Free tier**: 1,000 requests per day
- **Paid tier**: 100,000 requests per day

**Why?** Running servers costs money! If someone made millions of requests, it could:
- Crash the servers
- Cost the company huge amounts
- Slow down service for everyone

**Analogy**: Like a buffet limiting how many times you can go back—prevents one person from taking everything.

#### 3. Security (Protecting data)

Keys ensure only authorized people access certain data.

**Example**: Your banking app uses authentication so only YOU can see YOUR account balance, not everyone's.

#### 4. Billing (Tracking usage)

Many APIs charge money after you exceed free limits. The key tracks your usage.

**Example**:
- First 1,000 requests/month: Free
- Next 10,000 requests: $0.01 each
- Your key tracks: You made 5,432 requests this month

#### 5. Analytics (Understanding users)

Companies want to know:
- Which endpoints are most popular?
- Who's using the API?
- Are there errors?

### Types of API Authentication

#### 1. API Keys (Simple)

**How it works**: Include your key in every request

```bash
GET https://api.weather.com/forecast?apikey=abc123xyz
```

**Pros**: Simple to use
**Cons**: Less secure (if stolen, anyone can use it)
**Used for**: Public data, low-security needs

#### 2. OAuth Tokens (More secure)

**How it works**: You log in through a secure process, get a temporary token

**You've seen this!** When an app says "Sign in with Google" or "Connect to Spotify"

**Flow example:**
1. App: "Hey user, we need access to your Google Calendar"
2. You click "Allow"
3. Google gives the app a special token (not your password!)
4. App uses token to access your calendar
5. Token expires after a while or you can revoke it

**Pros**: Very secure, doesn't expose passwords, can be revoked
**Cons**: More complex to implement
**Used for**: Accessing user data, high-security needs

#### 3. Bearer Tokens (Middle ground)

Similar to API keys but with better security practices:

```bash
GET https://api.example.com/data
Headers:
  Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

**Pros**: More secure than basic API keys
**Cons**: Still need to protect them
**Used for**: Modern APIs, authenticated services

#### 4. Basic Authentication (Username + Password)

**How it works**: Send username and password with each request (encoded)

```bash
GET https://api.example.com/data
Headers:
  Authorization: Basic dXNlcm5hbWU6cGFzc3dvcmQ=
```

**Pros**: Very simple
**Cons**: Insecure unless using HTTPS
**Used for**: Internal tools, legacy systems

### How to Get an API Key

**Typical Process:**

**Step 1: Sign up**
- Visit the API provider's website
- Create a free account
- Verify your email

**Step 2: Navigate to developer section**
- Look for "API", "Developers", "Dashboard", or "Settings"

**Step 3: Create a new key**
- Click "Generate API Key" or "Create New Key"
- Sometimes you name it (like "My Weather App")

**Step 4: Copy and save it**
- ⚠️ **CRITICAL**: Copy it immediately—many sites only show it once!
- Save it somewhere secure (password manager, environment variables)

**Step 5: Read the terms**
- Check rate limits
- Understand pricing
- Review acceptable use

### How to Use an API Key

There are typically **3 ways** to send your API key:

#### Method 1: Query Parameter (in the URL)

```bash
GET https://api.weather.com/data?apikey=YOUR_KEY&city=London
```

**Pros**: Simple, easy to test
**Cons**: Key visible in browser history/logs

#### Method 2: Header (more secure)

```bash
GET https://api.weather.com/data?city=London
Headers:
  X-API-Key: YOUR_KEY
```

**Pros**: More secure, not in URL
**Cons**: Slightly more complex

#### Method 3: Request Body (for POST requests)

```bash
POST https://api.example.com/data
Body:
{
  "apikey": "YOUR_KEY",
  "data": "some data"
}
```

**Always check the documentation** to see which method an API expects!

### Code Examples

#### JavaScript (Browser/Node.js)

```javascript
// Method 1: Query parameter
fetch('https://api.weather.com/data?apikey=YOUR_KEY&city=London')
  .then(response => response.json())
  .then(data => console.log(data));

// Method 2: Header
fetch('https://api.weather.com/data?city=London', {
  headers: {
    'X-API-Key': 'YOUR_KEY'
  }
})
  .then(response => response.json())
  .then(data => console.log(data));
```

#### Python

```python
import requests

# Method 1: Query parameter
response = requests.get(
    'https://api.weather.com/data',
    params={'apikey': 'YOUR_KEY', 'city': 'London'}
)
data = response.json()

# Method 2: Header
response = requests.get(
    'https://api.weather.com/data',
    params={'city': 'London'},
    headers={'X-API-Key': 'YOUR_KEY'}
)
data = response.json()
```

### Security: Protecting Your API Keys

#### ❌ NEVER DO THIS:

```javascript
// DON'T: Hardcode key in code that goes to GitHub
const API_KEY = "sk-1234567890abcdef";  // ← Everyone can see this!

fetch(`https://api.example.com/data?key=${API_KEY}`)
```

If you push this to GitHub, bots will find and steal your key within minutes!

#### ✅ DO THIS INSTEAD:

**1. Environment Variables (Best practice)**

```bash
# .env file (never commit this!)
API_KEY=sk-1234567890abcdef
```

```javascript
// Your code
const API_KEY = process.env.API_KEY;  // Reads from environment

fetch(`https://api.example.com/data?key=${API_KEY}`)
```

**2. Config Files (that are gitignored)**

```javascript
// config.js (add to .gitignore)
module.exports = {
  apiKey: "sk-1234567890abcdef"
};
```

```bash
# .gitignore
config.js
.env
```

**3. Server-side proxy (For web apps)**

Instead of exposing your key in the browser:

```
Browser → Your Server → API
           ↑
      Key stored here
```

Your browser calls YOUR server, which has the key and makes the real API call.

### What Happens If Your Key is Stolen?

**Immediate risks:**
1. **Someone racks up charges** on your account
2. **Your key gets rate-limited** (hits the limit, stops working for you)
3. **Malicious use** gets your account banned
4. **Data breach** if the key accesses sensitive info

**What to do:**
1. **Revoke the key immediately** (in API dashboard)
2. **Generate a new key**
3. **Check usage** for unauthorized requests
4. **Update your code** with the new key
5. **Review security** to prevent it happening again

### Rate Limiting Explained

Most APIs limit requests to prevent abuse:

**Example limits:**
- **Free tier**: 1,000 requests/day
- **Basic tier**: 10,000 requests/day ($10/month)
- **Pro tier**: 100,000 requests/day ($50/month)

**What happens when you hit the limit?**

**Response:**
```
HTTP 429 Too Many Requests

{
  "error": "Rate limit exceeded",
  "retry_after": 3600  // seconds until reset
}
```

**How to handle:**
```javascript
const response = await fetch(apiUrl);

if (response.status === 429) {
  console.log("Rate limited! Wait before retrying");
  // Wait, then try again
} else {
  const data = await response.json();
}
```

### Pro tip: Cache responses!

Don't keep requesting the same data:

```javascript
// Bad: Requests weather every time user clicks
function showWeather() {
  fetch('https://api.weather.com/data?key=YOUR_KEY')
}

// Good: Request once, reuse data
let weatherCache = null;
let cacheTime = null;

function showWeather() {
  const fiveMinutes = 5 * 60 * 1000;

  if (weatherCache && Date.now() - cacheTime < fiveMinutes) {
    return weatherCache;  // Use cached data
  }

  // Only fetch if cache is old
  fetch('https://api.weather.com/data?key=YOUR_KEY')
    .then(data => {
      weatherCache = data;
      cacheTime = Date.now();
    });
}
```

---

## Real-World Example: Weather App

Let's build a complete, working application using the **OpenWeatherMap API**.

### Step 1: Get Your API Key

1. Visit: https://openweathermap.org/api
2. Click "Sign Up" (top right)
3. Fill out the form (use your real email)
4. Verify your email
5. Log in and go to: https://home.openweathermap.org/api_keys
6. You'll see a default API key already created!
7. Copy it (looks like: `a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6`)

⚠️ **Note**: New keys take about 10 minutes to activate. Don't worry if it doesn't work immediately!

### Step 2: Test It With a Simple Request

Replace `YOUR_API_KEY` with your actual key:

```
https://api.openweathermap.org/data/2.5/weather?q=London&appid=YOUR_API_KEY&units=metric
```

Paste that into your browser. You should see JSON data like:

```json
{
  "weather": [{"main": "Clouds", "description": "broken clouds"}],
  "main": {
    "temp": 15.2,
    "feels_like": 14.8,
    "humidity": 72
  },
  "name": "London"
}
```

✅ **If you see this** → Your key works!
❌ **If you see an error** → Wait 10 minutes for activation, or check your key

### Step 3: Build the Weather App (JavaScript Version)

#### Project Setup:

```bash
# Create a new folder
mkdir weather-app
cd weather-app

# Create files
touch weather.js
touch .env
touch .gitignore
```

#### File 1: `.gitignore`

```
.env
node_modules/
```

This tells Git to never commit your API key!

#### File 2: `.env`

```
WEATHER_API_KEY=your_actual_api_key_here
```

Replace `your_actual_api_key_here` with your real key!

#### File 3: `weather.js`

```javascript
// Load environment variables
require('dotenv').config();

// Get the API key from environment
const API_KEY = process.env.WEATHER_API_KEY;
const BASE_URL = 'https://api.openweathermap.org/data/2.5/weather';

/**
 * Fetch weather data for a city
 */
async function getWeather(city) {
  try {
    // Build the URL with parameters
    const url = `${BASE_URL}?q=${city}&appid=${API_KEY}&units=metric`;

    console.log(`🌍 Fetching weather for ${city}...`);

    // Make the API request
    const response = await fetch(url);

    // Check if request was successful
    if (!response.ok) {
      if (response.status === 404) {
        throw new Error(`City "${city}" not found!`);
      } else if (response.status === 401) {
        throw new Error('Invalid API key! Check your .env file');
      } else if (response.status === 429) {
        throw new Error('Too many requests! Rate limit exceeded');
      } else {
        throw new Error(`HTTP error! status: ${response.status}`);
      }
    }

    // Parse the JSON response
    const data = await response.json();

    // Display the weather
    displayWeather(data);

  } catch (error) {
    console.error('❌ Error:', error.message);
  }
}

/**
 * Display weather data in a nice format
 */
function displayWeather(data) {
  console.log('\n📍 Location:', data.name, data.sys.country);
  console.log('🌡️  Temperature:', data.main.temp + '°C');
  console.log('🤔 Feels like:', data.main.feels_like + '°C');
  console.log('☁️  Conditions:', data.weather[0].description);
  console.log('💧 Humidity:', data.main.humidity + '%');
  console.log('💨 Wind speed:', data.wind.speed + ' m/s');
  console.log('');
}

// Get city from command line argument
const city = process.argv[2] || 'London';

// Run the program
getWeather(city);
```

#### Install dependencies:

```bash
npm init -y
npm install dotenv node-fetch
```

#### Run it:

```bash
node weather.js Paris
node weather.js "New York"
node weather.js Tokyo
```

#### Output:

```
🌍 Fetching weather for Paris...

📍 Location: Paris FR
🌡️  Temperature: 18.5°C
🤔 Feels like: 17.8°C
☁️  Conditions: clear sky
💧 Humidity: 65%
💨 Wind speed: 3.2 m/s
```

### Step 4: Python Version (Alternative)

If you prefer Python, here's the same app:

#### Project Setup:

```bash
mkdir weather-app-python
cd weather-app-python
touch weather.py
touch .env
touch .gitignore
```

#### File: `.env` (same as before)

```
WEATHER_API_KEY=your_actual_api_key_here
```

#### File: `.gitignore`

```
.env
__pycache__/
*.pyc
```

#### File: `weather.py`

```python
import os
import requests
from dotenv import load_dotenv
import sys

# Load environment variables from .env file
load_dotenv()

# Get API key from environment
API_KEY = os.getenv('WEATHER_API_KEY')
BASE_URL = 'https://api.openweathermap.org/data/2.5/weather'

def get_weather(city):
    """Fetch and display weather data for a city"""

    try:
        # Build the request parameters
        params = {
            'q': city,
            'appid': API_KEY,
            'units': 'metric'  # Use Celsius
        }

        print(f'🌍 Fetching weather for {city}...')

        # Make the API request
        response = requests.get(BASE_URL, params=params)

        # Check for errors
        if response.status_code == 404:
            print(f'❌ Error: City "{city}" not found!')
            return
        elif response.status_code == 401:
            print('❌ Error: Invalid API key! Check your .env file')
            return
        elif response.status_code == 429:
            print('❌ Error: Too many requests! Rate limit exceeded')
            return
        elif response.status_code != 200:
            print(f'❌ Error: HTTP {response.status_code}')
            return

        # Parse JSON response
        data = response.json()

        # Display the weather
        display_weather(data)

    except requests.exceptions.RequestException as e:
        print(f'❌ Network error: {e}')
    except Exception as e:
        print(f'❌ Unexpected error: {e}')

def display_weather(data):
    """Display weather data in a nice format"""
    print(f'\n📍 Location: {data["name"]}, {data["sys"]["country"]}')
    print(f'🌡️  Temperature: {data["main"]["temp"]}°C')
    print(f'🤔 Feels like: {data["main"]["feels_like"]}°C')
    print(f'☁️  Conditions: {data["weather"][0]["description"]}')
    print(f'💧 Humidity: {data["main"]["humidity"]}%')
    print(f'💨 Wind speed: {data["wind"]["speed"]} m/s')
    print()

# Main program
if __name__ == '__main__':
    # Get city from command line argument, or use default
    city = sys.argv[1] if len(sys.argv) > 1 else 'London'

    # Check if API key exists
    if not API_KEY:
        print('❌ Error: WEATHER_API_KEY not found in .env file!')
        sys.exit(1)

    # Get the weather
    get_weather(city)
```

#### Install dependencies:

```bash
pip install requests python-dotenv
```

#### Run it:

```bash
python weather.py Paris
python weather.py "New York"
python weather.py Tokyo
```

### Step 5: Understanding the Code

#### 1. Loading the API Key Securely

```javascript
// JavaScript
require('dotenv').config();
const API_KEY = process.env.WEATHER_API_KEY;
```

```python
# Python
from dotenv import load_dotenv
load_dotenv()
API_KEY = os.getenv('WEATHER_API_KEY')
```

This reads your key from the `.env` file instead of hardcoding it!

#### 2. Building the URL

```javascript
const url = `${BASE_URL}?q=${city}&appid=${API_KEY}&units=metric`;
```

This creates:
```
https://api.openweathermap.org/data/2.5/weather?q=Paris&appid=YOUR_KEY&units=metric
│                                                 │       │              │
│                                                 │       │              └─ Temperature in Celsius
│                                                 │       └─ Your API key
│                                                 └─ City name
└─ Base endpoint
```

#### 3. Making the Request

```javascript
const response = await fetch(url);
```

This sends an HTTP GET request to the API.

#### 4. Error Handling

```javascript
if (response.status === 404) {
  throw new Error(`City "${city}" not found!`);
} else if (response.status === 401) {
  throw new Error('Invalid API key!');
}
```

Different status codes mean different things:
- **200**: Success!
- **401**: Bad API key
- **404**: City not found
- **429**: Too many requests

#### 5. Parsing the Response

```javascript
const data = await response.json();
```

Converts the JSON string into a JavaScript object you can use:

```javascript
data.main.temp        // Temperature
data.weather[0].description  // "clear sky"
data.name             // "Paris"
```

### Step 6: Bonus Features

#### Feature 1: 5-Day Forecast

```javascript
async function getForecast(city) {
  const url = `https://api.openweathermap.org/data/2.5/forecast?q=${city}&appid=${API_KEY}&units=metric`;

  const response = await fetch(url);
  const data = await response.json();

  console.log(`\n📅 5-Day Forecast for ${city}:\n`);

  // API returns data every 3 hours, we'll take one per day
  for (let i = 0; i < data.list.length; i += 8) {
    const day = data.list[i];
    const date = new Date(day.dt * 1000).toLocaleDateString();

    console.log(`${date}: ${day.main.temp}°C - ${day.weather[0].description}`);
  }
}
```

#### Feature 2: Multiple Cities at Once

```javascript
async function compareWeather(cities) {
  console.log('🌍 Weather Comparison:\n');

  for (const city of cities) {
    const url = `${BASE_URL}?q=${city}&appid=${API_KEY}&units=metric`;
    const response = await fetch(url);
    const data = await response.json();

    console.log(`${city.padEnd(15)} ${data.main.temp}°C  ${data.weather[0].description}`);
  }
}

// Usage
compareWeather(['London', 'Paris', 'New York', 'Tokyo']);
```

Output:
```
🌍 Weather Comparison:

London          15.2°C  broken clouds
Paris           18.5°C  clear sky
New York        22.1°C  light rain
Tokyo           19.8°C  few clouds
```

#### Feature 3: Save to File

```javascript
const fs = require('fs').promises;

async function saveWeatherReport(city) {
  const url = `${BASE_URL}?q=${city}&appid=${API_KEY}&units=metric`;
  const response = await fetch(url);
  const data = await response.json();

  const report = `
Weather Report for ${data.name}
Generated: ${new Date().toLocaleString()}
Temperature: ${data.main.temp}°C
Conditions: ${data.weather[0].description}
Humidity: ${data.main.humidity}%
  `.trim();

  await fs.writeFile(`weather-${city}.txt`, report);
  console.log(`✅ Report saved to weather-${city}.txt`);
}
```

### Common Issues & Solutions

#### Issue 1: "Invalid API key"

**Solution**:
- Check your `.env` file has the right key
- Wait 10 minutes after creating the key
- Make sure there are no spaces: `API_KEY=abc123` not `API_KEY = abc123`

#### Issue 2: "City not found"

**Solution**:
- Check spelling
- Use quotes for multi-word cities: `"New York"` not `New York`
- Try adding country code: `"London,UK"`

#### Issue 3: Rate limit (429 error)

**Solution**:
- Free tier: 60 calls/minute, 1,000,000 calls/month
- Add delays between requests:

```javascript
await new Promise(resolve => setTimeout(resolve, 1000)); // Wait 1 second
```

#### Issue 4: Module not found

**Solution**:
```bash
# JavaScript
npm install dotenv node-fetch

# Python
pip install requests python-dotenv
```

### Understanding the API Response

Here's what the full JSON response looks like:

```json
{
  "coord": {
    "lon": -0.1257,
    "lat": 51.5085
  },
  "weather": [
    {
      "id": 803,
      "main": "Clouds",
      "description": "broken clouds",
      "icon": "04d"
    }
  ],
  "base": "stations",
  "main": {
    "temp": 15.2,
    "feels_like": 14.8,
    "temp_min": 13.5,
    "temp_max": 16.8,
    "pressure": 1013,
    "humidity": 72
  },
  "visibility": 10000,
  "wind": {
    "speed": 3.2,
    "deg": 240
  },
  "clouds": {
    "all": 75
  },
  "dt": 1698768000,
  "sys": {
    "country": "GB",
    "sunrise": 1698733200,
    "sunset": 1698768600
  },
  "timezone": 3600,
  "id": 2643743,
  "name": "London",
  "cod": 200
}
```

You can access any of this data:

```javascript
data.main.temp          // 15.2
data.weather[0].description  // "broken clouds"
data.wind.speed         // 3.2
data.sys.country        // "GB"
```

---

## Best Practices & Common Pitfalls

### API Key Security Checklist

✅ **Do:**
- Store keys in environment variables
- Use `.gitignore` to exclude config files
- Rotate keys periodically
- Use different keys for development vs. production
- Monitor usage in API dashboard
- Revoke unused keys
- Read the API's rate limits

❌ **Don't:**
- Commit keys to GitHub
- Share keys publicly
- Hardcode keys in client-side code
- Use production keys for testing
- Ignore rate limits
- Give keys overly broad permissions

### Error Handling Best Practices

Always handle these common errors:

```javascript
async function makeAPIRequest(url) {
  try {
    const response = await fetch(url);

    // Check HTTP status
    if (response.status === 401) {
      throw new Error('Authentication failed - check your API key');
    } else if (response.status === 404) {
      throw new Error('Resource not found');
    } else if (response.status === 429) {
      throw new Error('Rate limit exceeded - slow down your requests');
    } else if (response.status === 500) {
      throw new Error('Server error - try again later');
    } else if (!response.ok) {
      throw new Error(`HTTP ${response.status}: ${response.statusText}`);
    }

    const data = await response.json();
    return data;

  } catch (error) {
    if (error.name === 'TypeError') {
      console.error('Network error - check your internet connection');
    } else {
      console.error('Error:', error.message);
    }
    throw error;
  }
}
```

### Rate Limiting Strategies

1. **Implement caching** - Don't request the same data repeatedly
2. **Use batch endpoints** - Get multiple items in one request if available
3. **Add delays** - Space out requests to stay under limits
4. **Monitor usage** - Check your dashboard regularly
5. **Handle 429 errors** - Back off and retry with exponential delay

```javascript
async function retryWithBackoff(fn, maxRetries = 3) {
  for (let i = 0; i < maxRetries; i++) {
    try {
      return await fn();
    } catch (error) {
      if (error.status === 429 && i < maxRetries - 1) {
        const delay = Math.pow(2, i) * 1000; // 1s, 2s, 4s
        console.log(`Rate limited. Retrying in ${delay}ms...`);
        await new Promise(resolve => setTimeout(resolve, delay));
      } else {
        throw error;
      }
    }
  }
}
```

### Documentation is Your Friend

Before using any API:

1. **Read the quickstart guide** - Understand the basics
2. **Check authentication requirements** - What type of auth is needed?
3. **Review rate limits** - How many requests can you make?
4. **Look at example code** - Learn from working examples
5. **Check pricing** - Will you stay within free tier?
6. **Read terms of service** - What are you allowed to do?

---

## Next Steps

### Project Ideas

Now that you understand APIs, here are projects to build:

1. **Weather Dashboard**: Show weather for multiple cities with icons
2. **Weather Alerts**: Email yourself if it's going to rain
3. **Travel Planner**: Compare weather in vacation destinations
4. **Discord Bot**: Let friends check weather via Discord commands
5. **Data Analysis**: Track temperature trends over time

### Popular Free APIs to Try

- **GitHub API**: Access repositories, users, issues
- **News API**: Get news headlines from various sources
- **Dog API**: Random dog pictures (yes, really!)
- **Pokémon API**: Information about Pokémon
- **Spotify API**: Music data and playlists
- **NASA API**: Space photos and data
- **Cat Facts API**: Random cat facts
- **Rick and Morty API**: Character and episode information

### Learning Resources

- **MDN Web Docs**: Comprehensive JavaScript and API documentation
- **Postman Learning Center**: Tutorials on API testing
- **APIs You Won't Hate**: Blog about API design
- **Public APIs GitHub Repo**: List of 1000+ free APIs

### Advanced Topics to Explore

- **GraphQL**: Alternative to REST APIs
- **WebSockets**: Real-time bidirectional communication
- **API Design**: How to build your own APIs
- **Authentication Flows**: OAuth2, JWT tokens
- **API Versioning**: Managing changes over time
- **Webhooks**: APIs that push data to you

---

## Summary

### Key Takeaways

✅ **APIs enable software to communicate** - Like a menu between programs
✅ **Endpoints are specific URLs** - Each does one thing
✅ **API keys authenticate you** - Like a password for your app
✅ **Always use environment variables** - Never hardcode keys
✅ **Check status codes** - They tell you what went wrong
✅ **Read the documentation** - Each API is different
✅ **Handle errors gracefully** - Network can fail, resources can be missing
✅ **Respect rate limits** - Don't spam the API
✅ **Cache when possible** - Reduce unnecessary requests
✅ **Test in small steps** - Get one thing working, then build on it

### The Mental Model

Think of APIs as:
- **Restaurant Menu** - You order, kitchen cooks, you get food
- **Library** - You request a book, librarian finds it, you check it out
- **Waiter** - Takes your order to the kitchen, brings back your meal

You don't need to know how it works internally. You just need to know:
1. What's available (documentation)
2. How to ask for it (endpoints)
3. What you'll get back (response format)
4. How to prove you're allowed (authentication)

### You're Ready!

You now have everything you need to start working with APIs:
- Understanding of what APIs are and how they work
- Knowledge of endpoints and how to find them
- Mastery of API keys and security
- A complete working example
- Best practices and common pitfalls

Go build something awesome!

---

**Created with Claude Code**
**Last updated: 2025-10-31**
