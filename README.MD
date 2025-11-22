🎙️ Ten Days of Voice Agents 2025 🚀

A 10-day challenge to build real-time conversational AI voice agents!

This challenge is powered by:

Murf Falcon — Text-to-Speech (TTS)

Deepgram — Speech-to-Text (STT)

Gemini — LLM intelligence

LiveKit Agent Framework — Real-time WebRTC pipeline

Document your progress daily on GitHub & LinkedIn using:

#MurfAIVoiceAgentsChallenge
#10DaysofAIVoiceAgents

🌟 Challenge Highlights
Item	Details
Duration	10 Days
Goal	Build one new voice agent each day
Sharing	GitHub + LinkedIn posts
Community	LiveKit Slack + Murf AI community
🏗️ Monorepo Structure
/backend     # Python: LiveKit agents, Murf, Deepgram, Gemini
/frontend    # Next.js: UI, WebRTC, LiveKit components
/docs        # Documentation and daily logs

🔥 Technology Stack
Component	Technology	Purpose
Backend	Python 3.9+, LiveKit Agents, Deepgram, Gemini, Murf, Silero VAD	Voice pipeline, STT, LLM, TTS
Frontend	React 19, Next.js 15, Tailwind CSS, Radix UI, LiveKit	Real-time UI
Infra	LiveKit Server, Docker, JWT	Media streaming & deployment
🛠️ Prerequisites
Backend Skills

✔ Python async / OOP
✔ REST APIs, JWT
✔ uv package manager
✔ Environment variables

Frontend Skills

✔ React 19, hooks
✔ TypeScript basics
✔ TailwindCSS
✔ Understanding WebRTC

General

✔ Git + GitHub
✔ Basic terminal usage
✔ Docker (optional)

🚦 Quick Start Guide
1️⃣ Install Dependencies
# Python 3.9+ with `uv`
curl -LsSf https://astral.sh/uv/install.sh | sh

# Node.js 18+ & pnpm
npm install -g pnpm

# LiveKit server (optional)
brew install livekit  # macOS

2️⃣ Clone & Setup
git clone https://github.com/your-username/ten-days-of-voice-agents-2025.git
cd ten-days-of-voice-agents-2025

3️⃣ Backend Setup
cd backend
uv sync

cp .env.example .env.local
# Add API keys: LIVEKIT, MURF, GOOGLE, DEEPGRAM

uv run python src/agent.py download-files  # Optional

4️⃣ Frontend Setup
cd ../frontend
pnpm install
cp .env.example .env.local

5️⃣ Run the App
Option A — Script
chmod +x start_app.sh
./start_app.sh

Option B — Manual
# Terminal 1
livekit-server --dev

# Terminal 2
cd backend
uv run python src/agent.py dev

# Terminal 3
cd frontend
pnpm dev

6️⃣ Test the System

Go to http://localhost:3000

Allow microphone access 🎤

Click Connect

Talk to your agent 🤖 → 🔊

💡 Voice AI Pipeline
User → Deepgram STT → Gemini LLM → Murf TTS → Audio Response


✨ Silero VAD detects speech
✨ Turn management ensures natural conversation timing

📂 Important Project Files
Path	Description
backend/src/agent.py	Main agent logic
.env.local	API configuration
frontend/app/	UI pages
frontend/components/	Shared UI elements
docs/	Daily logs & showcases
🎨 Customization

You can modify:

Agent persona → backend/src/agent.py

Murf voice model

UI layout & styles

Add tools via function_tool

📝 Example Daily Post Template

🚀 Day 5 of #10DaysofAIVoiceAgents — My Agent Speaks Like a Pro!

🎤 I built instant STT → Gemini → Murf TTS response
🎧 Smooth voice playback
🔍 On-screen transcripts

Next: Real-time latency tuning!

#MurfAI #VoiceAI #LiveKit #Gemini #BuildInPublic

You can use this format daily with demos/gifs!

🙌 Acknowledgments

Special thanks to:

Murf AI — Falcon TTS

Deepgram — Fast STT

Google Gemini — Smart responses

LiveKit — Real-time voice infra

Community support from LinkedIn, Slack, and mentors 💛

🤔 Troubleshooting Tips

Check API keys

Validate microphone & browser permissions

Verify LiveKit server status

Check backend logs

🎯 Success Checklist
Status	Task
⬜ Python 3.9+ installed	
⬜ Node 18+ installed	
⬜ Backend setup complete	
⬜ Frontend running	
⬜ LiveKit server running	
⬜ STT ↔ LLM ↔ TTS loop working	
⬜ Daily progress shared	
🚀 Let’s Build the Future of Voice AI!

Start Day 1 today — keep shipping every day!
Real-time voice is the next frontier 🌟