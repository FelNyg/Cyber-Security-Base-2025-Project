# Cyber-Security-Base-2025-Project

## Installation Guide
------------------

1. Clone the repository to your machine:


2. Create a virtual environment inside the project folder:

   `python -m venv env`

3. Activate the virtual environment:
   - Mac/Unix:
     `source env/bin/activate`
   - Windows:
     `env\Scripts\activate.bat`

4. Install the requirements:

   `pip install -r requirements.txt`

## How to Start the Project
------------------------

Run the following commands in the terminal:

   `python manage.py makemigrations`
   `python manage.py migrate`
   `python manage.py runserver`

## User info

| User  | Password  | admin    |
|:-----:|:---------:|:--------:|
| admin | admin     | yes      |
| jm    | matmatmat | no       |
| mj    | drickadricka | no    |

## Vulnerabilities

I have made flaws in code in purpose and then commented how they should be fixed. 

## 1. Fault

A6:2017 - Security Misconfiguration

# What’s the issue?
Having DEBUG = True in production is risky — it shows full error details, including stack traces and potentially sensitive data. If attackers see this, they could use it to exploit the app.

# How to fix it:
Set DEBUG = False in production. This way, users only see generic error pages, and you keep technical details hidden. Always separate development and production settings.

The code can be found in `mysite/settings.py (line 26)`


## 2. Fault

A3:2017 – Sensitive Data Exposure

# What’s the issue?
The setting SECURE_SSL_REDIRECT is not enabled or doesn’t exist. This means users can access the site over plain HTTP, which doesn’t encrypt the traffic. That puts sensitive data like login credentials at risk.

# How to fix it:
Set SECURE_SSL_REDIRECT = True in production. This forces all traffic over HTTPS, ensuring data is encrypted in transit.

The code can be found in `mysite/settings.py (line 131)`

## 3. Fault

A10:2017 – Insufficient Logging & Monitoring

# What’s the issue?
The application doesn’t log security-related events. Without logging, attacks like repeated login failures or unexpected behavior can go undetected, making it hard to respond to threats or investigate issues.

# How to fix it:
Enable Django’s security logging by configuring the LOGGING setting. This will record security warnings to a file and help monitor potential attacks.

The code can be found in mysite/settings.py (line 136)

## 4. Fault

A1:2017 – Injection: Unsanitized user input directly in database query

# What’s the issue?
In the `search` function, user input from the search query is directly inserted into an SQL query without validation or sanitization. This allows malicious users to inject SQL code, potentially compromising the database and accessing unauthorized data. This vulnerability is known as SQL Injection, which can expose sensitive data.

```python
# polls/views.py
from django.shortcuts import render
from django.db import connection

def search(request):
    query = request.GET.get('q', '')
    # Vulnerable: direct string formatting in SQL
    with connection.cursor() as cursor:
        cursor.execute(f"SELECT * FROM polls_question WHERE question_text LIKE '%{query}%'")
        results = cursor.fetchall()
    return render(request, 'polls/search.html', {'results': results})
```

# How to fix it:
Use Django’s ORM, which safely processes user input and prevents SQL injection.

```python
# polls/views.py
from .models import Question

def search(request):
    query = request.GET.get('q', '')
    results = Question.objects.filter(question_text__icontains=query)
    return render(request, 'polls/search.html', {'results': results})
```

The vulnerable code can be found in `polls/views.py` (line 59) and the template in `polls/templates/polls/search.html`.



## 5. Fault

A5:2017 – Broken Access Control

# What’s the issue?


# How to fix it:

