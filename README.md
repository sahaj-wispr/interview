## Introduction

We, at Wispr, are building a voice interface to streamline and accelerate the process of interacting with technology. Today, you're going to play an important part in that mission by building a mini-personal assistant! 

We've already accomplished much of the groundwork, transforming user queries into actionable commands. Your task today involves three key objectives:

1. Authenticate an user.
2. Retrieve a list of commands for a given user query.
3. Validate the commands.
4. Execute the commands.

You will be interacting with the Wispr backend, hosted at 
```
https://428c-99-0-87-50.ngrok-free.app
```

It is also important to note that, today, we will be interacting with two objects:
- Users
- Functions

Every function has a name, a set of arguments (which can be previous function executions), and an execution URL.

## Endpoints

All endpoints except `/access-token` requires you to pass the access token into the header with the form:
```
Authorization: Bearer <access token>
```

### Access Token
```
GET /access-token/:user_id

Example JSON Output:
{
    "access_token": "123", 
    "user_id": "1"
}
```

### Functions
Please note that, because we are dealing with LLMs, this endpoint will return an output EVEN IF the user query is invalid or doesn't make much sense.
```
POST /functions

Example Body:
{
    "query": "Summarize this article and send it to my friend Danny.",
    "args": {
        "text": "...",
        "recipient": "Danny"
    }
}

Example JSON Output:
{
    "functions": [
        {
            "name": "summarize",
            "args": {                
                "text": "Farmer John has lost his prize cow Bessie, and he needs to find her!"
            },
            "execute_url": "/execute/summarize"            
        },
        {
            "name": "send_article",
            "args": {                
                "text": "summarize.output",
                "recipient": "Danny"
            },
            "execute_url": "/execute/send_article"
        }
    ]
}
```

### Execute
```
POST /execute/:function_name

Example Body:
{
    "args": {
        "text" : "...",
        ...
    },
    "original_query": "..."
}

Example JSON Output:
{
    "output": "..."
}

```



### Testing
```
# Queries are structured in the form of:
(query, args)

user_id = 1

user_queries = [
    ("Can you summarize this text?", {"text": "Farmer John has lost his prize cow Bessie, and he needs to find her!"}),
    ("Screenshot this and send it to my friend Danny!", {"bounding_box": (400, 800, 200, 1000), "recipient": "Danny"}),
    ("What does this code do?", {})
]

expected_outputs = [
    "Bessie's gone!",
    "https://wispr-interview.ngrok.io/assets/screenshot.png",
    "Invalid"
]

```

### Follow-up questions 
[Discuss if time permits]
