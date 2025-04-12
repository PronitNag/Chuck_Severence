# Django Projects ko GitHub mein Upload karna

## Introduction

GitHub ek powerful version control system hai jo aapke code ko manage karne mein help karta hai. Iss chapter mein, hum seekhenge ki kaise apne Django projects ko PythonAnywhere se GitHub ke private repository mein upload kiya jata hai.

## Important Points

- **Private Repository ka Use**: Kabhi bhi apne assignments ko public GitHub repository mein mat daaliye. GitHub free mein small number of collaborators ke liye private repositories provide karta hai.

- **Benefits of GitHub**:
  - Code version control
  - Team collaboration
  - Code backup
  - Easy deployment aur updates

## GitHub Setup Karna

### 1. GitHub Account Create Karna
- GitHub par jaakar apna account banaiye agar aapke paas pehle se nahi hai
- Sign up karne ke baad apne email ko verify karein

### 2. New Repository Create Karna
- GitHub dashboard par "New repository" button par click karein
- Repository ko meaningful naam dein (jaise "django-mysite")
- "Private" option select karein
- "Create repository" par click karein

## Local Git Setup

### 1. Git Installation
- Agar aapke system par Git installed nahi hai, toh install karein
- PythonAnywhere par Git already installed rehta hai

### 2. Git Configuration
```bash
git config --global user.name "Aapka Naam"
git config --global user.email "aapka.email@example.com"
```

## Django Project ko GitHub se Connect Karna

### 1. PythonAnywhere par Git Initialize Karna
- Apne project directory mein jaaiye:
```bash
cd ~/django_projects/mysite
```

- Git initialize karein:
```bash
git init
```

### 2. Files ko Stage aur Commit Karna
- `.gitignore` file create karein:
```bash
touch .gitignore
```

- `.gitignore` mein add karein:
```
*.pyc
*~
__pycache__
myenv
db.sqlite3
/static
.DS_Store
local_settings.py
```

- Files ko stage karein:
```bash
git add .
```

- Changes ko commit karein:
```bash
git commit -m "Initial commit - mysite project"
```

### 3. GitHub Repository se Connect Karna
- Remote add karein:
```bash
git remote add origin https://github.com/aapka-username/django-mysite.git
```

- Main branch ko rename karein (optional - newer Git versions):
```bash
git branch -M main
```

- Code ko push karein:
```bash
git push -u origin main
```

## Authentication Setup

### Personal Access Token (PAT) Generate Karna
- GitHub settings > Developer settings > Personal access tokens > Generate new token
- Token ko safe jagah save karein, ye sirf ek baar dikhaya jaayega
- Push karte time username aur password ke jagah token ka use karein

## Regular Workflow

### Code Update Karne ke Steps
1. Changes karein apne project mein
2. Changes ko check karein:
```bash
git status
```

3. Changes ko stage karein:
```bash
git add .
```

4. Changes ko commit karein:
```bash
git commit -m "Describe your changes here"
```

5. Changes ko GitHub par push karein:
```bash
git push
```

## PythonAnywhere se GitHub Workflow

### PythonAnywhere Console mein
- Bash console open karein
- Project directory mein navigate karein
- Git commands run karein (same as above)

### Credentials Management
- Credentials ko save karne ke liye credential helper use karein:
```bash
git config --global credential.helper store
```
- First push ke baad credentials cached rahenge

## Common Issues aur Solutions

### Permission Denied
- Check karein ki aapne sahi PAT generate kiya hai
- Ensure karein ki repository private hai aur aapke account se connected hai

### Merge Conflicts
- Local mein changes ko pull karein:
```bash
git pull
```
- Conflicts ko resolve karein
- Phir commit aur push karein

## Conclusion

GitHub ka use karke aap apne Django projects ko easily manage kar sakte hain. Private repositories use karke aap apne code ko secure rakh sakte hain, aur version control ki saari benefits le sakte hain.

**Yaad rakhein**: Apne assignments ya sensitive information kabhi bhi public repository mein upload na karein.
