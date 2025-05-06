# archipelago
Interview Answer

## Basic Question
- The design depends on scale; I will cover both scenarios for public and internal apps.
- For public, I would use Go lang for the backend due to its performance, concurrency, and memory efficiency. Ideal for handling high volume users without unnecessary cloud cost. For the frontend, I would use React.js to build a dynamic and responsive user interface.
- The logic will be like, when a user submits a photo, the file is stored in AWS S3, and a record is created in PostgreSQL photos table with fields like photo_id, user_id, status (pending, review, approved, rejected), created_date, and reviewed_date.
- The moderator dashboard displays all pending photos as interactive cards, sorted by created_at. When a moderator clicks on a photo to review, the frontend sends a request to update the status to reviewing. This acts as a soft lock, preventing others from reviewing the same photo.
- The photo is then removed from the dashboard via React’s state update to avoid duplication. This ensures that only one moderator can claim a photo at a time. If the query affects zero rows, the backend returns a conflict response so the frontend can notify the moderator the photo is already being reviewed.
- For internal apps use cases, I might use Laravel (PHP) or Node.js in a monolithic architecture to speed up development. The logic remains the same, but the infrastructure could be simplified.
- In terms of data structure, a relational model works well. Status transitions are managed via a simple enum or state machine pattern. If volume grows, the system could be extended with a task queue to decouple submission and moderation workflows.

## SQL Questions
1. 
```sql
SELECT count(*) FROM Customers WHERE Country = 'Germany';
```
2. 
```sql
SELECT count(*), Country 
FROM Customers
GROUP BY Country
HAVING count(*) >= 5
ORDER BY count(*) DESC;
```
3. 
```sql
SELECT 
    c.CustomerName, 
    COUNT(o.OrderID) AS OrderCount, 
    FORMAT(MIN(o.OrderDate), 'yyyy-mm-dd') AS FirstOrder, 
    FORMAT(MAX(o.OrderDate), 'yyyy-mm-dd') AS LastOrder
FROM Customers c
INNER JOIN Orders AS o ON c.CustomerID = o.CustomerID
GROUP BY c.CustomerName
HAVING COUNT(o.OrderID) >= 5
ORDER BY MAX(o.OrderDate) DESC;
```

## Javascript Questions
1.
```js
function titleCase(input) {
    const text = input.toLowerCase();
    const split = text.split(" ");
    const result = split.map(item => item.charAt(0).toUpperCase() + item.slice(1));

    return result.join(" ");
}
```

2.
```js
function delay(ms) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve("Done");
        }, ms);
    });
};

delay(3000).then(() => alert('runs after 3 seconds'));
```

2.5.
```js
function fetchData(url) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            if (!url) {
                reject("URL is required");
            } else {
                resolve(`Data from ${url}`);
            }
        }, 500);
    });
}

function processData(data) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            if (!data) {
                reject("Data is required");
            } else {
                resolve(data.toUpperCase());
            }
        }, 1000);
    });
}

async function main() {
    try {
        const fetchedData = await fetchData("https://example.com");
        const processedData = await processData(fetchedData);
        console.log("Processed Data:", processedData);
    } catch (error) {
        console.error("Error:", error);
    }
}

main();
```

## App Question
- 3 & 4: [http://react-chat-app-925933724043.s3-website-ap-southeast-1.amazonaws.com/](http://react-chat-app-925933724043.s3-website-ap-southeast-1.amazonaws.com/)

## Vue Question
- 

## Website Security Best Practices
1. To avoid DDoS attack, the most common attack is by using rate limiting and traffic filtering.
    - Rate limiting could be applied by API Gateway using nginx.
    - Limit the traffic for specific IP.
2. SQL Injection
    - Use parameterized queries (in dotnet there's a dapper with @param) or ORM.
    - Sanitize user input, prevent special characters from being submitted.
3. Brute Force password
    - Use JWT or session token with expiration.
    - Use multi-factor authentication.
4. Prevent Man In The Middle
    - Use HTTPS for all traffic, enforce the HSTS (HTTP Strict Transport Security).
5. Never send sensitive payload from client.

## Performance Best Practices
- **Front-End**
    - Use CDN (Upload static assets like CSS, images).
    - Use build tools like Vite or Webpack.
    - Lazy loading.
    - Compress the content.
- **Back-End**
    - Database indexing.
    - Use caching.
    - Use pagination.
    - Async & parallel processing.
    - Use reverse proxy like nginx.
    - Compress the response using gzip.

## Dotnet Questions
```csharp
using System;
using System.Collections.Generic;
using System.Text.RegularExpressions;

public class Program
{
    public static void Main()
    {
        WordFrequencyCount("Four, One two two three Three three four  four   four");
    }
    
    private static void WordFrequencyCount(string word) 
    {
        if (string.IsNullOrEmpty(word))
        {
            Console.WriteLine("Word cannot be empty!");
            return;
        }
        
        Dictionary<string, int?> dict = new();
        var words = word.Split(" ");
        foreach(var item in words)
        {
            if (string.IsNullOrWhiteSpace(item))
                continue;

            var pattern = @"[,.?!;:'""(){}\[\]\\\/]";
            var sanitasionItem = Regex.Replace(item, pattern, "").Trim();
            
            var lowerItem = sanitasionItem.ToLower();
            dict.TryGetValue(lowerItem, out var data);
            if (data is null)
                dict.Add(lowerItem, 1);
            else
                dict[lowerItem] = data + 1;
        }
        
        foreach(var result in dict)
        {
            Console.WriteLine($"{result.Key} => {result.Value}");
        }
    }
}
```

## Tools
- Git                     => 4
- Redis                   => 4
- VSCode / JetBrains?     => 4
- Linux?                  => 3
- AWS
    - EC2                 => 3
    - Lambda              => 3
    - RDS                 => 1
    - Cloudwatch          => 1
    - S3                  => 3
- Unit testing            => 4
- Kanban boards?          => 4
