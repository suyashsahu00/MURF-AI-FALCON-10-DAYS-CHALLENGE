Ten Days of Voice Agents 2025 🚀
A 10-day challenge by Murf AI to build AI-powered voice agents. Every day features a new agent, persona, or use-case—all powered by Murf Falcon (TTS), Deepgram (STT), Gemini (LLM), and the LiveKit agent framework. Share progress daily on GitHub & LinkedIn!

🌟 Challenge Highlights
Duration: 10 days (one agent per day)

Goal: Build voice agents with unique skills/personas

Share: Document work on GitHub and LinkedIn with:

#MurfAIVoiceAgentsChallenge

#10DaysofAIVoiceAgents

Community: Join the LiveKit Slack & Murf AI community for help

🏗️ Monorepo Structure
text
/backend   # Python (LiveKit agents, Murf, Deepgram, Gemini)
/frontend  # Next.js (React 19, LiveKit UI, Tailwind CSS)
/docs      # Challenge documentation & guides
🔥 Technology Stack
Component	Technology	Purpose
Backend	Python 3.9+, LiveKit, Deepgram, Gemini, Murf, Silero VAD	Voice AI pipeline, STT, LLM, TTS
Frontend	React 19, Next.js 15, Tailwind, Radix, LiveKit Components	UI/UX, real-time interaction, WebRTC
Infrastructure	LiveKit Server, Docker, JWT	Media server, deployment, authentication
🛠 Prerequisites & Skills Needed
Backend
Python 3.9+

Async/await, OOP basics

Environment variable management

Package managers (uv)

REST APIs/JWT/auth

Frontend
React 19 (hooks: useState, useEffect, useContext)

TypeScript (types, interfaces)

Next.js routing/app router

TailwindCSS

Real-time concepts (WebRTC, LiveKit)

General
Git/GitHub for versioning

Basic terminal/CLI usage

Docker (optional)

API integration, debugging browser/devtools

🚦 Quick Start Guide
1. Install Dependencies
bash
# Python 3.9+ with `uv`
curl -LsSf https://astral.sh/uv/install.sh | sh

# Node.js 18+ and pnpm
npm install -g pnpm

# LiveKit Server (optional for local testing)
brew install livekit  # on macOS
# Or download from https://docs.livekit.io/home/self-hosting/local/
2. Clone & Setup
bash
git clone https://github.com/your-username/ten-days-of-voice-agents-2025.git
cd ten-days-of-voice-agents-2025
3. Backend Setup
bash
cd backend
uv sync  # Install dependencies

cp .env.example .env.local
# Fill in API keys: LIVEKIT_URL, LIVEKIT_API_KEY, LIVEKIT_API_SECRET, MURF_API_KEY, GOOGLE_API_KEY, DEEPGRAM_API_KEY

uv run python src/agent.py download-files  # Download models (optional)
4. Frontend Setup
bash
cd ../frontend
pnpm install
cp .env.example .env.local  # Copy backend keys here
5. Run Application
bash
# Option A: Use script
chmod +x start_app.sh
./start_app.sh

# Option B: Run terminals individually
# Terminal 1: LiveKit server
livekit-server --dev

# Terminal 2: Backend agent
cd backend
uv run python src/agent.py dev

# Terminal 3: Frontend
cd frontend
pnpm dev
6. Test System
Open http://localhost:3000

Allow microphone access

Click Connect, talk to your agent!

💡 Voice AI Pipeline
User speaks → Deepgram STT → Gemini LLM → Murf Falcon TTS → User hears

VAD (Silero) detects speech

Turn detector manages conversation flow

📂 Key Files
text
backend/
  ├── src/agent.py     # Main agent logic
  ├── .env.local       # Your API keys
  ├── pyproject.toml   # Python dependencies
  └── tests/           # Pytest test files

frontend/
  ├── app/             # Next.js app folder
  ├── components/      # React components
  ├── hooks/           # Custom hooks
  ├── package.json     # JS dependencies
  └── .env.local       # API keys
🎨 Customization
Change agent persona in backend/src/agent.py

Add new tools/functions via function_tool

Change Murf voice/model in agent code

Tweak UI or pipeline for features

📝 Daily Progress Example
🚀 Day 9 of #30DaysOfVoiceAgents – Talking with the LLM in My Own Voice! 🎙️🤖
Today I built something magical — my AI now listens to me… and speaks back instantly in Murf’s stunning voice! ✨
Here’s the magic flow:

🎤 Record voice in browser

📡 Send audio to backend

✍️ AssemblyAI converts to text

🧠 Gemini LLM generates replies

🎙️ Murf.ai gives reply a lifelike voice

🔊 Playback is instant

📝 Show LLM transcript

💡 Real conversations, instant response, seamless UI.

🔹 Tackled Murf’s 3000‑character limit, smooth UI, audio+transcript sync.

🙏 Huge thanks to Murf AI, Google, and the team! Next: Reduce latency for near real‑time conversations.

#AI #VoiceAI #GenerativeAI #WebDev #TTS #LLM #AssemblyAI #GoogleGemini #BuildwithMurf #DaysofVoiceAgents #MurfAI #BuildInPublic #LearningInPublic

🙌 Acknowledgments
Thanks to Omkar and Srishti for voice contributions!

Murf AI (Falcon TTS), LiveKit, Deepgram, Google Gemini—powering this challenge.

Community, mentors, LinkedIn friends—helpful throughout!

🤔 Getting Help
Check terminal logs, API key config

Read LiveKit/Murf docs

Ask on LiveKit Slack: https://livekit.io/join-slack

Common issues: Permissions, API authentication, service status

🎯 Success Checklist
 Python 3.9+ installed

 Node.js 18+ installed

 All dependencies set up (uv, pnpm)

 API keys (Murf, Google, Deepgram, LiveKit)

 Monorepo cloned & configured

 Backend/frontend working

 LiveKit server running

 Successful voice interaction

 Progress and demo shared!

Ready to build and share voice agents for the future of conversational AI! Start with Day 1 and keep shipping every day 🚀