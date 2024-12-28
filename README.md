# PodPrep 🎙️ - AI-Powered Podcast Interview Assistant

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

PodPrep helps podcast hosts prepare for interviews by automatically generating guest research dossiers and AI-powered question suggestions. Save hours of research time and create better interviews.

## 🎯 Problem & Solution

### The Problem
Podcast hosts spend 3-4 hours researching each guest and preparing questions, often:
- Manually searching across multiple platforms
- Missing important details about guests
- Asking questions that have been answered in previous interviews
- Struggling to create unique, engaging questions

### The Solution
PodPrep automates the research process and generates unique questions by:
- Aggregating guest information from multiple sources
- Analyzing previous interviews to avoid repetition
- Generating AI-powered question suggestions
- Creating comprehensive guest dossiers

## 🚀 Features

### MVP Features
- **Guest Research Automation**
  - Social media profile analysis
  - Recent news and mentions
  - Professional background summary
  - Previous interview detection

- **Question Generation**
  - AI-powered unique questions
  - Topic-based categorization
  - Previous question analysis
  - Follow-up suggestions

- **Export Options**
  - PDF dossier export
  - Markdown format
  - Mobile-friendly view

### Coming Soon
- Real-time interview assistance
- Voice transcription integration
- Multi-host collaboration
- Custom AI training

## 🛠️ Technology Stack

- **Frontend**: React.js + Tailwind CSS
- **Backend**: Python (FastAPI)
- **Database**: MongoDB
- **AI**: OpenAI API
- **Authentication**: Auth0
- **Hosting**: Vercel + Railway

## 📋 Prerequisites

- Python 3.8+
- Node.js 16+
- MongoDB
- OpenAI API key
- Social media API keys (Twitter, LinkedIn)

## ⚙️ Installation

1. Clone the repository
```bash
git clone https://github.com/mikhlas9/podprep.git
cd podprep
```

2. Set up backend
```bash
# Create and activate virtual environment
cd backend
python -m venv venv
source venv/bin/activate  # or `venv\Scripts\activate` on Windows

# Install dependencies
pip install -r requirements.txt

# Set up environment variables
cp .env.example .env
# Edit .env with your API keys
```

3. Set up frontend
```bash
cd frontend
npm install

# Set up environment variables
cp .env.example .env.local
# Edit .env.local with your API keys
```

4. Start development servers
```bash
# Terminal 1 - Backend
cd backend
uvicorn main:app --reload

# Terminal 2 - Frontend
cd frontend
npm run dev
```

5. Open http://localhost:3000 in your browser

## 🗄️ Project Structure
```bash
podprep/
├── backend/
│   ├── app/
│   │   ├── api/
│   │   ├── core/
│   │   ├── services/
│   │   └── utils/
│   ├── requirements.txt
│   └── main.py
├── frontend/
│   ├── components/
│   ├── pages/
│   ├── public/
│   └── styles/
└── README.md
```

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch (```bash git checkout -b feature/amazing-feature```)
3. Commit your changes (git commit -m 'Add some amazing feature')
4. Push to the branch (git push origin feature/amazing-feature)
4. Open a Pull Request


