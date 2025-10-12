## API Overview

The MoviesDatabase API is a robust and flexible service that provides extensive data about movies, TV series, and actors. It's designed for developers building applications that require detailed entertainment information, from basic title searches to complex filtered queries. Key features include powerful search and filtering capabilities, access to high-quality images, detailed cast information, and user ratings from platforms like IMDB.

## API Version

**Version:** 1.1.2

## Available Endpoints

The API provides several endpoints to access different types of data:

*   **`GET /titles`**: The primary endpoint for searching and retrieving lists of movies and TV series. Supports extensive filtering by year, genre, title type, and user ratings.
*   **`GET /titles/{id}`**: Retrieves detailed information for a specific title using its unique identifier.
*   **`GET /titles/{id}/ratings`**: Fetches the rating information (e.g., IMDB score) for a specific title.
*   **`GET /titles/{id}/cast`**: Returns the full cast and crew list for a given title.
*   **`GET /titles/search/title/{title}`**: Searches for titles based on a specific title string.
*   **`GET /actors`**: Searches and retrieves a list of actors.
*   **`GET /actors/{id}`**: Gets detailed information about a specific actor.
*   **`GET /titles/random`**: Retrieves a random list of titles, useful for featuring content.

## Request and Response Format

### Request Format
Requests are made as standard HTTP `GET` requests. Parameters are passed as query strings. For example, to get the top 10 action movies from 2023:

```http
GET /titles?genre=Action&year=2023&list=top_rated_english_250&limit=10
```

### Response Format
The API returns responses in JSON format. A typical successful response from the `/titles` endpoint has the following structure:

```json
{
  "page": 1,
  "next": "2",
  "entries": 10,
  "results": [
    {
      "id": "tt15398776",
      "primaryImage": {
        "url": "https://m.media-amazon.com/images/M/...jpg",
        "caption": { "plainText": "OPPENHEIMER (2023)" }
      },
      "titleType": { "text": "Movie" },
      "titleText": { "text": "Oppenheimer" },
      "releaseYear": { "year": 2023 },
      "releaseDate": { "day": 19, "month": 7, "year": 2023 }
    }
  ]
}
```

## Authentication

All requests to the MoviesDatabase API must be authenticated using an API key.

*   **Method:** API Key via Headers
*   **Header:** `X-RapidAPI-Key`
*   **Header:** `X-RapidAPI-Host`

You can obtain your API key by subscribing to the API through the RapidAPI platform. Once you have a key, include it in the headers of every request.

**Example in JavaScript (fetch):**
```javascript
const options = {
  method: 'GET',
  headers: {
    'X-RapidAPI-Key': 'your_api_key_here',
    'X-RapidAPI-Host': 'moviesdatabase.p.rapidapi.com'
  }
};

fetch('https://moviesdatabase.p.rapidapi.com/titles', options)
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(err => console.error(err));
```

## Error Handling

The API uses standard HTTP status codes to indicate the success or failure of a request. It's crucial to handle these in your code to ensure a robust application.

*   **200 OK:** The request was successful.
*   **400 Bad Request:** The request was invalid. This is often due to missing or incorrect parameters.
*   **401 Unauthorized:** The API key is missing or invalid.
*   **403 Forbidden:** You are not authorized to access the requested resource, or you have exceeded your rate limit.
*   **404 Not Found:** The requested resource (e.g., a title by ID) was not found.
*   **429 Too Many Requests:** You have exceeded the rate limits for your current subscription plan.
*   **500 Internal Server Error:** An error occurred on the API server.

**Example Error Handling:**
```javascript
if (!response.ok) {
  if (response.status === 401) {
    throw new Error('Invalid API Key');
  } else if (response.status === 429) {
    throw new Error('Rate limit exceeded. Please try again later.');
  } else {
    throw new Error(`HTTP error! status: ${response.status}`);
  }
}
```

## Usage Limits and Best Practices

### Usage Limits
The API is subject to rate limiting based on your subscription plan on RapidAPI:
*   **Basic (Free):** 100 requests per day.
*   **Pro:** 1,000 requests per day.
*   **Ultra:** 10,000 requests per day.

Exceeding these limits will result in a `429 Too Many Requests` error.

### Best Practices
1.  **Store API Keys Securely:** Never hardcode your API key. Use environment variables or a secure secrets management solution.
2.  **Implement Caching:** Cache frequent and static responses (like movie details) to reduce the number of API calls and improve your application's performance.
3.  **Use Filtering Wisely:** Leverage the powerful `genre`, `year`, and `list` parameters on the `/titles` endpoint to fetch exactly the data you need, rather than filtering large datasets on the client-side.
4.  **Handle Pagination:** The API returns paginated results. Always check the `next` field in the response to retrieve subsequent pages of data for a complete dataset.
5.  **Plan for Errors:** Implement robust error handling as described above to gracefully manage network issues, invalid responses, and API limit errors.