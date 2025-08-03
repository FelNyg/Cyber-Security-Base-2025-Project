# Cyber-Security-Base-2025-Project

This project is the first project in the Cyber Security Base 2025 offered by University of Helsinki. 
The goal of the project is to build a vulnerable web application that demonstrates five common security issues from the **OWASP Top 10 (2017)** list.
When starting the project I followed the  tutorial "Writing your first Django app, part 1". The link to this tutorial is the following: https://docs.djangoproject.com/en/5.2/intro/tutorial01/

## Repository Link For Project 

https://github.com/FelNyg/Cyber-Security-Base-2025-Project

## Installation Guide
------------------

1. Clone the repository to your desired location on your machine


2. Create a virtual environment inside the project folder:

   `python -m venv env`

3. Activate the virtual environment:
   - Mac/Unix:
     `source env/bin/activate`
   - Windows:
     `env\Scripts\activate.bat`

4. Install Django

   `pip install django`

## How to Start the Project
------------------------

Run the following commands in the terminal:
   - `python manage.py makemigrations`
   - `python manage.py migrate`
   - `python manage.py runserver`
   

## User info

| User  | Password  | admin    |
|:-----:|:---------:|:--------:|
| admin | admin     | yes      |
| jm    | matmatmat | no       |
| mj    | drickadricka | no    |

## Vulnerabilities

I have left some flaws in code on purpose, these are commented out. They are described below how they shall be fixed. 

When checking for these flaws do the following steps:  
- 1) Apply the change first in the code 
- 2) Save the changes 
- 3) Start the server 
- 4) Test in your browsers ingocnito mode

These changes should produce similar results as in the before and after pictures that you will find in the `screenshots` folder.

If similar results don't show up do the following steps:
- 1) Shut down the server 
- 2) Apply the change in the code  
- 3) Save the changes
- 4) Restart the server 
- 5) Test in a new tab in your browsers ingocnito mode

But now to the flaws!

## 1. Flaw

A6:2017 - Security Misconfiguration

# What’s the issue?
Having DEBUG = True in production is risky business becauase it shows all error details, potentially showing sensitive data. If attackers see this, they could use it to exploit the app. 


# How to fix it:
Set DEBUG = False in production. This way, users only see generic error pages, keeping our technical details hidden.
For the fix change DEBUG = False!

The code can be found in `mysite/settings.py (line 27)`

# Before continuing with the flaws revert the change back!


## 2. Flaw

A3:2017 – Sensitive Data Exposure

# What’s the issue?
The setting SECURE_SSL_REDIRECT is not enabled or doesn’t exist. This means users can access the site over plain HTTP, which doesn’t encrypt the traffic. That puts sensitive data like cookies at risk.

P.S This isn’t an issue during local development, however if unchecked can reak havoc in production. This flaw is more of a streach because this can happen when deploying the app whilst using an unsafe network, when using a safe one there is no problem.

# How to fix it:
Set SECURE_SSL_REDIRECT = True in production. This forces all traffic over HTTPS, ensuring data is encrypted in transit.
For the fix change SECURE_SSL_REDIRECT = True!

The code can be found in `mysite/settings.py (line 130)`

# Before continuing with the flaws revert the change back!

## 3. Flaw

A5:2017 – Broken Access Control

# What’s the issue?
The search view is accessible to any user, even if they’re not logged in. This could allow unauthorized users to probe or scrape questions from the system. Try it out on `http://127.0.0.1:8000/polls/` and click "Search Questions".

# How to fix it:
Restrict access to the search view by using Django’s @login_required decorator. This ensures only users with credentilas can perform searches. 
So for the fix uncomment @login_required in the code so it looks like this!

```python
@login_required
def search(request):
    query = request.GET.get("q", "")
    results = Question.objects.filter(question_text__icontains=query)
    return render(request, "polls/search.html", {"results": results})
```
The vulnerable code can be found in `polls/views.py (line 58)`

# Keep this change when continuing with the flaws!

## 4. Flaw

A10:2017 – Insufficient Logging & Monitoring

# What’s the issue?
The application doesn’t log security-related events. Without logging, attacks like repeated login failures or unexpected behavior can go undetected, making it hard to respond to threats or investigate issues. Try logging in with wrong user credentials and check the `security.log`.

# How to fix it:
Enable Django’s bulit in security logging by configuring the LOGGING setting. This will record security warnings to a file and help monitor potential attacks.
For this fix you will have to uncomment the whole of LOGGING! Now try logging in with wrong user credentials and check the `security.log`.

The code can be found in `mysite/settings.py (line 137)`

# Keep this change when continuing with the flaws!


## 5. Flaw

A1:2017 – Injection: Unsanitized user input directly in database query

# What’s the issue?
In the `search` function, user input from the search query is directly inserted into an SQL query without validation or sanitization. This allows malicious users to inject SQL code, witch can potentially expose sensitive data. Type the following query `' OR 1=1 --`, this will show all the questions or in our case "sensitive data".

```python
def search(request):
    query = request.GET.get("q", "")
    results = Question.objects.raw(f"SELECT * FROM polls_question WHERE question_text LIKE '%{query}%'") # <- This is the faulty code!
    return render(request, "polls/search.html", {"results": results})
```

# How to fix it:
Do not use raw SQL with unsanitized user input. Use Django’s ORM, which safely processes user input and prevents SQL injection. 
For the fix comment out line 62 and uncomment line 64! Now that same query should not return any questions or in our case "sensitive data".

```python
def search(request):
    query = request.GET.get("q", "")
    results = Question.objects.filter(question_text__icontains=query) # <- The faulty code is now fixed!
    return render(request, "polls/search.html", {"results": results})
```

The vulnerable code can be found in `polls/views.py (line 62)`
