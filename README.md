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

---

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

## 📸 Screenshots

**Dashboard**
![Image Description](https://i.imgur.com/ooCliEP.png)

**Guest Research**
![Image Description](https://i.imgur.com/yx7xQth.png)

**Question Generator**
![Image Description](https://i.imgur.com/ibutoTf.png)


##  📘 Basic Implementation

1. Backend
```bash
# backend/app/main.py

from fastapi import FastAPI, HTTPException
from pydantic import BaseModel
from typing import List, Optional
import httpx
import openai
from datetime import datetime

app = FastAPI()

class GuestRequest(BaseModel):
    name: str
    social_handles: Optional[dict] = None

class Question(BaseModel):
    text: str
    category: str
    context: Optional[str] = None

class GuestDossier(BaseModel):
    name: str
    summary: str
    social_profiles: dict
    recent_content: List[dict]
    suggested_questions: List[Question]
    generated_at: datetime

async def fetch_social_data(name: str, handles: dict) -> dict:
    """Fetch data from social media platforms."""
    # In real implementation, use proper API clients
    async with httpx.AsyncClient() as client:
        twitter_data = None
        linkedin_data = None
        
        if handles and handles.get("twitter"):
            twitter_data = await client.get(
                f"https://api.twitter.com/2/users/{handles['twitter']}"
            )
        
        if handles and handles.get("linkedin"):
            linkedin_data = await client.get(
                f"https://api.linkedin.com/v2/people/{handles['linkedin']}"
            )
        
        return {
            "twitter": twitter_data,
            "linkedin": linkedin_data
        }

async def generate_questions(context: str) -> List[Question]:
    """Generate interview questions using OpenAI."""
    prompt = f"""
    Based on this context about a podcast guest, generate 5 unique and insightful
    interview questions that haven't been commonly asked before:
    
    {context}
    
    Focus on their expertise and recent developments in their field.
    """
    
    response = await openai.Completion.create(
        engine="text-davinci-003",
        prompt=prompt,
        max_tokens=300,
        temperature=0.7
    )
    
    # Parse and structure the generated questions
    questions = []
    raw_questions = response.choices[0].text.strip().split("\n")
    
    for q in raw_questions:
        if q.strip():
            questions.append(Question(
                text=q.strip(),
                category="ai_generated",
                context="Generated based on guest research"
            ))
    
    return questions

@app.post("/api/research/guest", response_model=GuestDossier)
async def research_guest(request: GuestRequest):
    """Generate a guest research dossier."""
    try:
        # Fetch social media data
        social_data = await fetch_social_data(
            request.name,
            request.social_handles
        )
        
        # Create guest summary
        summary = f"Research results for {request.name}"
        
        # Generate interview questions
        questions = await generate_questions(summary)
        
        # Compile dossier
        dossier = GuestDossier(
            name=request.name,
            summary=summary,
            social_profiles=social_data,
            recent_content=[],  # Implement content fetching
            suggested_questions=questions,
            generated_at=datetime.utcnow()
        )
        
        return dossier
        
    except Exception as e:
        raise HTTPException(status_code=500, detail=str(e))
```

2. Frontend
```bash
# frontend/components/GuestSearch.tsx

import React, { useState } from 'react';
import { useForm } from 'react-hook-form';

interface GuestSearchProps {
    onSearch: (data: any) => void;
    isLoading: boolean;
}

export const GuestSearch: React.FC<GuestSearchProps> = ({ onSearch, isLoading }) => {
    const { register, handleSubmit } = useForm();
    
    return (
        <form onSubmit={handleSubmit(onSearch)} className="space-y-4">
            <div>
                <label className="block text-sm font-medium text-gray-700">
                    Guest Name
                </label>
                <input
                    type="text"
                    {...register('name', { required: true })}
                    className="mt-1 block w-full rounded-md border-gray-300 shadow-sm"
                    placeholder="Enter guest name"
                />
            </div>
            
            <div>
                <label className="block text-sm font-medium text-gray-700">
                    Twitter Handle (optional)
                </label>
                <input
                    type="text"
                    {...register('social_handles.twitter')}
                    className="mt-1 block w-full rounded-md border-gray-300 shadow-sm"
                    placeholder="@username"
                />
            </div>
            
            <button
                type="submit"
                disabled={isLoading}
                className="w-full flex justify-center py-2 px-4 border border-transparent rounded-md shadow-sm text-sm font-medium text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
            >
                {isLoading ? 'Researching...' : 'Research Guest'}
            </button>
        </form>
    );
};
```
---


## 💻 Explanation of the Code

This implementation consists of two parts:

### 1. Backend API (`main.py` in FastAPI):
- Provides an endpoint (`/api/research/guest`) to generate a "Guest Dossier" for podcast hosts
- It uses the following functionalities:
  - **Social Media Data Fetching (`fetch_social_data`):**
     - Connects to Twitter and LinkedIn APIs to retrieve guest data.
     - This is a placeholder and requires actual API credentials and responses to work.
  - **AI-Powered Question Generation (`generate_questions`):**
      - Uses OpenAI API to generate a list of unique, insightful questions based on the guest's context (e.g., their summary).
      - Parses the AI-generated questions and structures them into the Question model.
  - **Dossier Compilation:**
      - Combines the guest's social media data, a research summary, and AI-generated questions into a GuestDossier.
      - Returns the dossier as the API response.
 - Error Handling:
    - Wraps operations in a try-except block to return a 500 error with the exception details if something goes wrong.

### 2. Frontend Component (`GuestSearch.tsx` in React):
 - Provides a user interface for podcast hosts to input guest details and trigger the dossier generation.
 - Key Features:
    - **Form Fields:**
       - Input for the guest's name (required).
       - Optional input for the guest's Twitter handle.
    - **Submit Handler:**
       - Uses onSearch callback passed via props to handle form submission.
    - **Loading State:**
       - Disables the submit button and updates its text to "Researching..." while the API call is in progress.



## 🤝 Contributing

1. Fork the repository
2. Create your feature branch (```bash git checkout -b feature/amazing-feature```)
3. Commit your changes (```bash git commit -m 'Add some amazing feature```)
4. Push to the branch (```bash git push origin feature/amazing-feature```)
4. Open a Pull Request


