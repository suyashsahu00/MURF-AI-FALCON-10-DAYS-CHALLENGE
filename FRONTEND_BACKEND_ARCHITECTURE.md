# Frontend-Backend Architecture Overview

## Murf AI Falcon - 10 Days Challenge

---

## 📊 HIGH-LEVEL ARCHITECTURE

```
┌─────────────────────────────────────────────────────────────────┐
│                        FRONTEND (Next.js)                       │
│                    React 19 + TypeScript                        │
└────────────────────────────┬──────────────────────────────────────┘
                             │
                    ┌────────▼────────┐
                    │   LiveKit SDK   │
                    │ (WebRTC Voice)  │
                    └────────┬────────┘
                             │
                ┌────────────┴────────────┐
                │                         │
        ┌───────▼────────┐        ┌──────▼──────────┐
        │ /api/          │        │ Backend Server  │
        │ connection-    │        │ (Python)        │
        │ details        │        │ LiveKit Agents  │
        └────────────────┘        └─────────────────┘
```

---

## 🔗 MAIN FRONTEND-BACKEND CONNECTIONS

### **1. Token Generation Endpoint**

**Path:** `frontend/app/api/connection-details/route.ts`

**Connection Flow:**

```
Frontend (useRoom hook)
    ↓
Fetches: POST /api/connection-details
    ↓
Backend Response with:
  - serverUrl (LiveKit URL)
  - roomName (unique room ID)
  - participantToken (JWT token for LiveKit)
  - participantName ("user")
    ↓
Frontend connects to LiveKit server using these credentials
```

**Request Body:**

```typescript
{
  room_config: {
    agents: [
      {
        agent_name: "your-agent-name", // Optional, specifies which agent to use
      },
    ];
  }
}
```

**Response:**

```typescript
{
  serverUrl: "wss://your-livekit-server.com",
  roomName: "voice_assistant_room_xxxx",
  participantName: "user",
  participantToken: "jwt-token-here"
}
```

### **2. Configuration Endpoint (Optional)**

**Path:** Environment variable `NEXT_PUBLIC_APP_CONFIG_ENDPOINT`

**Purpose:** Retrieve dynamic app configuration (branding, colors, feature flags)

---

## 📁 FRONTEND FOLDER STRUCTURE & RELATIONSHIPS

### **Root Entry Point**

```
app/
├── layout.tsx
│   ├── Loads fonts (Public Sans, Commit Mono)
│   ├── Calls getAppConfig() from lib/utils.ts
│   ├── Gets app configuration and styles
│   ├── Wraps children with providers
│   └── Returns HTML structure
│
├── (app)/
│   ├── layout.tsx
│   │   ├── Displays header with logo
│   │   ├── Imports getAppConfig() and logo data
│   │   └── Wraps page content
│   │
│   └── page.tsx
│       ├── Calls getAppConfig() from lib/utils.ts
│       ├── Renders App component
│       └── Passes appConfig prop
│
├── api/
│   └── connection-details/
│       └── route.ts [MAIN BACKEND CONNECTION]
│           ├── Handles POST requests from frontend
│           ├── Uses LiveKit SDK to generate JWT tokens
│           ├── Returns connection details for WebRTC
│           └── Environment: LIVEKIT_API_KEY, LIVEKIT_API_SECRET, LIVEKIT_URL
│
└── ui/
    ├── layout.tsx
    │   ├── Loads appConfig via getAppConfig()
    │   ├── Wraps with SessionProvider
    │   └── Sets up UI demo page
    └── page.tsx
        └── Shows all UI components for testing
```

---

## 🎯 COMPONENTS FOLDER STRUCTURE

```
components/
│
├── app/
│   ├── app.tsx [MAIN ENTRY POINT]
│   │   ├── Props: { appConfig: AppConfig }
│   │   ├── Wraps: SessionProvider, ViewController
│   │   ├── Provides: RoomAudioRenderer, StartAudio (LiveKit)
│   │   └── Shows: Toaster notifications
│   │
│   ├── session-provider.tsx [STATE MANAGEMENT]
│   │   ├── Creates: SessionContext & RoomContext
│   │   ├── Provides: appConfig, isSessionActive, startSession(), endSession()
│   │   ├── Uses: useRoom() hook
│   │   └── Wraps: All children with contexts
│   │
│   ├── view-controller.tsx [ROUTER/LAYOUT]
│   │   ├── Conditionally renders views:
│   │   │  ├── WelcomeView (before connection)
│   │   │  ├── PreConnectMessage (connecting state)
│   │   │  ├── SessionView (connected/voice active)
│   │   │  └── GameMaster (game-specific view)
│   │   └── Uses: useSession() hook
│   │
│   ├── session-view.tsx [MAIN UI DURING CALL]
│   │   ├── Shows: Chat transcript, video tiles, controls
│   │   ├── Uses: useChatMessages() hook
│   │   ├── Uses: AgentControlBar component
│   │   └── Displays: Real-time agent responses
│   │
│   ├── game-master.tsx [GAME-SPECIFIC UI]
│   │   ├── Renders: Interactive game narrative
│   │   ├── Shows: Game state, choices, inventory
│   │   └── Manages: Game-specific responses
│   │
│   ├── welcome-view.tsx [INITIAL SCREEN]
│   │   ├── Shows: Start button & instructions
│   │   ├── Displays: Company branding
│   │   └── Triggers: startSession() on button click
│   │
│   ├── chat-transcript.tsx
│   │   ├── Displays: Message history
│   │   ├── Shows: User messages vs Agent messages
│   │   └── Uses: ScrollArea component
│   │
│   ├── tile-layout.tsx
│   │   ├── Video rendering grid
│   │   ├── Shows: Participant video streams
│   │   └── Responsive layout
│   │
│   ├── preconnect-message.tsx
│   │   ├── Loading indicator
│   │   └── Connection status messages
│   │
│   ├── theme-toggle.tsx
│   │   ├── Light/dark mode switcher
│   │   ├── Uses: next-themes package
│   │   └── Respects: System preferences
│   │
│   └── (other UI components)
│
├── livekit/
│   ├── agent-control-bar/
│   │   ├── agent-control-bar.tsx [CONTROLS]
│   │   │   ├── Microphone toggle
│   │   │   ├── Camera toggle
│   │   │   ├── Screen share button
│   │   │   ├── Chat input toggle
│   │   │   └── Leave session button
│   │   │
│   │   ├── track-toggle.tsx
│   │   │   ├── Individual track controls
│   │   │   ├── Icons: Microphone, Camera, ScreenShare
│   │   │   └── Pending states
│   │   │
│   │   └── hooks/
│   │       ├── use-input-controls.ts
│   │       │   ├── Manages: Audio/video device selection
│   │       │   ├── Handles: User choices persistence
│   │       │   └── Provides: Device error handling
│   │       │
│   │       └── use-publish-permissions.ts
│   │           ├── Checks: Publish rights for tracks
│   │           └── Validates: Permission levels
│   │
│   ├── chat-entry.tsx
│   │   ├── Individual message display
│   │   ├── Shows: Timestamp, author, message
│   │   └── Styles: Local vs Remote messages
│   │
│   ├── alert.tsx, alert-toast.tsx
│   │   ├── Error notifications
│   │   ├── Status alerts
│   │   └── Uses: Sonner toast library
│   │
│   ├── button.tsx, toggle.tsx, select.tsx
│   │   ├── Shadcn UI components
│   │   ├── Tailwind-styled primitives
│   │   └── Accessible components
│   │
│   ├── scroll-area/
│   │   ├── Custom scrolling container
│   │   └── For chat transcripts
│   │
│   └── (other LiveKit UI components)
│
└── (component folders continue...)
```

---

## 🪝 HOOKS FOLDER - STATE & LOGIC MANAGEMENT

```
hooks/
│
├── useRoom.ts [CONNECTION MANAGEMENT]
│   ├── Creates: LiveKit Room instance
│   ├── Manages: Connection lifecycle
│   ├── Handles: Token fetching from /api/connection-details
│   ├── Features:
│   │   ├── Microphone/camera initialization
│   │   ├── Error handling & toasts
│   │   ├── Disconnection cleanup
│   │   └── Media device error handling
│   └── Returns: { room, isSessionActive, startSession, endSession }
│
├── useChatMessages.ts [MESSAGE MANAGEMENT]
│   ├── Tracks: All chat messages (user + agent)
│   ├── Subscribes: To participant events
│   ├── Features:
│   │   ├── Real-time message updates
│   │   ├── Message history
│   │   └── Agent transcription tracking
│   └── Returns: { messages, addMessage }
│
├── useGameMaster.ts [GAME LOGIC]
│   ├── Manages: Game state (if using game mode)
│   ├── Features:
│   │   ├── Narrative branching
│   │   ├── Player choices tracking
│   │   └── Story progression
│   └── Returns: { gameState, updateGameState }
│
├── useGameState.ts [GAME STATE]
│   ├── Interface:
│   │   ├── playerName, inventory, currentLocation
│   │   ├── visitedLocations, turnCount
│   │   ├── discoveredLore, relationships
│   │   ├── storyArc, health, decisions, skills
│   │   └── achievements
│   ├── Methods:
│   │   ├── updatePlayerName()
│   │   ├── addToInventory()
│   │   ├── removeFromInventory()
│   │   ├── updateHealth()
│   │   ├── discoverLore()
│   │   └── updateRelationships()
│   └── Used by: game-master.tsx component
│
├── useDebug.ts [DEBUG MODE]
│   ├── Enables: Debug logging
│   ├── Shows: Connection details
│   ├── Displays: Metrics & stats
│   └── Toggle: Via keyboard shortcut
│
└── useConnectionTimeout.tsx [CONNECTION TIMEOUT]
    ├── Monitors: Connection duration
    ├── Triggers: Timeout warnings
    └── Auto-disconnect: After max time
```

---

## 📚 LIB FOLDER - UTILITIES & HELPERS

```
lib/
│
├── utils.ts [CONFIG & HELPERS]
│   ├── getAppConfig() [KEY FUNCTION]
│   │   ├── Async function
│   │   ├── Fetches: From NEXT_PUBLIC_APP_CONFIG_ENDPOINT
│   │   ├── Fallback: APP_CONFIG_DEFAULTS
│   │   ├── Returns: AppConfig object
│   │   └── Used by: app/layout.tsx, pages
│   │
│   ├── cn() [TAILWIND MERGER]
│   │   ├── Combines: clsx + tailwind-merge
│   │   ├── Merges: Conflicting Tailwind classes
│   │   └── Usage: cn('text-xl', 'text-lg') → 'text-lg'
│   │
│   ├── getStyles()
│   │   ├── Generates: Custom CSS for accent colors
│   │   ├── Creates: Hover states dynamically
│   │   └── Returns: CSS string for <style> tag
│   │
│   └── Constants:
│       ├── CONFIG_ENDPOINT
│       ├── SANDBOX_ID
│       ├── THEME_STORAGE_KEY
│       └── THEME_MEDIA_QUERY
│
├── game-responses.ts [GAME CONTENT]
│   ├── Characters data
│   ├── Locations data
│   ├── Story branches & outcomes
│   ├── Inventory items
│   ├── Narrative responses
│   └── generateContextualResponse()
│
├── game-utils.ts [GAME UTILITIES]
│   ├── Game mechanics helpers
│   ├── Choice validation
│   ├── State transitions
│   └── Outcome calculations
│
└── useConnectionTimeout.tsx [HOOK IN LIB]
    ├── Manages: Connection duration
    ├── Triggers: Timeout events
    └── Auto-cleanup: On unmount
```

---

## 🎨 FONTS FOLDER

```
fonts/
├── CommitMono-400-Regular.otf
├── CommitMono-400-Italic.otf
├── CommitMono-700-Regular.otf
└── CommitMono-700-Italic.otf

Usage in app/layout.tsx:
- Local font import via next/font/local
- CSS variable: --font-commit-mono
- Used for: Monospace elements (code, timestamps)
```

---

## 🎨 STYLES FOLDER

```
styles/
│
├── globals.css [GLOBAL STYLES]
│   ├── Tailwind directives
│   ├── CSS variables (colors, spacing)
│   ├── Theme definitions
│   └── Root styles
│
└── game-master.module.css
    └── Game-specific styling
```

---

## ⚙️ APP-CONFIG.TS

```typescript
export interface AppConfig {
  pageTitle: string;
  pageDescription: string;
  companyName: string;

  // Features
  supportsChatInput: boolean;
  supportsVideoInput: boolean;
  supportsScreenShare: boolean;
  isPreConnectBufferEnabled: boolean;

  // Branding
  logo: string;
  logoDark?: string;
  accent?: string;
  accentDark?: string;
  startButtonText: string;

  // Agent configuration
  sandboxId?: string;
  agentName?: string;
}

Flow:
1. app/layout.tsx calls getAppConfig(headers)
2. Uses SERVER-SIDE headers to get sandboxId
3. Returns config object
4. Passed to App component props
5. Available to all components via SessionProvider
```

---

## 🔄 DATA FLOW DIAGRAM

```
USER INTERACTION
    ↓
WELCOME VIEW (Welcome-view.tsx)
    ├─ Click "Start Call"
    ├─ Calls startSession()
    │
USEROOM HOOK TRIGGERED
    ├─ Fetches: POST /api/connection-details
    ├─ Passes: agentName in room_config
    │
API ROUTE (connection-details/route.ts)
    ├─ Generates: JWT token using LiveKit SDK
    ├─ Creates: Room name & participant identity
    ├─ Returns: { serverUrl, roomName, participantToken }
    │
LIVEKIT CONNECTION
    ├─ Connects to WebRTC room
    ├─ Initializes: Microphone, audio renderer
    │
VIEW CONTROLLER UPDATES
    ├─ Detects: isSessionActive = true
    ├─ Switches: SessionView component
    │
SESSION VIEW ACTIVE
    ├─ Receives: Agent audio stream
    ├─ Displays: Chat transcript (useChatMessages)
    ├─ Shows: Agent control bar
    ├─ Updates: Real-time messages
    │
USER STOPS SESSION
    ├─ Calls: endSession()
    ├─ Disconnects: LiveKit room
    ├─ Returns: WelcomeView
```

---

## 🔐 ENVIRONMENT VARIABLES

### **Frontend (.env.local)**

```env
# LiveKit connection
LIVEKIT_API_KEY=your-api-key
LIVEKIT_API_SECRET=your-api-secret
LIVEKIT_URL=wss://your-livekit-server.com

# Optional: Config endpoint
NEXT_PUBLIC_APP_CONFIG_ENDPOINT=https://config-api.example.com/config
SANDBOX_ID=your-sandbox-id

# Optional: Custom connection details endpoint
NEXT_PUBLIC_CONN_DETAILS_ENDPOINT=/api/connection-details (default)
```

---

## 🐍 BACKEND (Python) OVERVIEW

### **Main Entry Point: backend/src/agent.py**

```python
# Key Components:
1. ImprovGameState - Session state management
2. Agent class - LiveKit agent initialization
3. Function tools - Tools exposed to agent
4. Agent job processing - Handle participant interactions

# Flow:
Frontend connects via LiveKit
    ↓
Agent receives audio stream
    ↓
Backend: Agent processes speech (STT)
    ↓
LLM generates response (Claude/Gemini/OpenAI)
    ↓
TTS converts to speech (Murf Falcon)
    ↓
Backend sends audio stream to frontend
```

### **Key Backend Features:**

- **STT:** Deepgram/Silero speech-to-text
- **LLM:** Claude, Gemini, OpenAI
- **TTS:** Murf Falcon (fastest TTS)
- **Agents:** LiveKit Agents framework
- **State:** Per-session game state tracking
- **Turn Detection:** Multilingual speaker detection

### **Shared Data:**

```
backend/shared-data/
├── day10_scenarios.json - Improv game scenarios
├── day4_tutor_content.json - Tutor content
├── day5_company_faq.json - FAQ data
├── day7_catalog.json - Product catalog
├── day8_game_world.json - Game world data
└── fraud_database.py - Fraud detection database
```

---

## 🔗 CONNECTION SUMMARY TABLE

| Component                 | Connects To               | Purpose             | Protocol          |
| ------------------------- | ------------------------- | ------------------- | ----------------- |
| `useRoom.ts`              | `/api/connection-details` | Get LiveKit token   | HTTP POST         |
| `app.tsx`                 | `SessionProvider`         | Pass config & state | React Context     |
| `session-provider.tsx`    | `useRoom.ts`              | Manage connection   | React Hooks       |
| `view-controller.tsx`     | Backend (via LiveKit)     | Send/receive audio  | WebRTC            |
| `useChatMessages.ts`      | Backend (via LiveKit)     | Track transcripts   | WebRTC Events     |
| `session-view.tsx`        | Agent backend             | Display responses   | Real-time updates |
| `/api/connection-details` | LiveKit SDK               | Generate tokens     | SDK API           |

---

## 📦 KEY DEPENDENCIES

### **Frontend**

```json
{
  "@livekit/components-react": "^2.9.15",
  "livekit-client": "^2.15.8",
  "livekit-server-sdk": "^2.13.2",
  "next": "15.5.2",
  "react": "^19.0.0",
  "motion": "^12.16.0",
  "@radix-ui": "^2.x",
  "tailwindcss": "^4"
}
```

### **Backend**

```python
livekit
livekit-agents
livekit-plugins-murf
livekit-plugins-silero
livekit-plugins-google
livekit-plugins-deepgram
livekit-plugins-turn-detector
```

---

## 📋 COMPONENT DEPENDENCY GRAPH

```
app/layout.tsx
    ├── getAppConfig() [lib/utils.ts]
    └── children → app/(app)/layout.tsx
                        ├── getAppConfig()
                        └── page.tsx
                            └── App component
                                ├── SessionProvider
                                │   ├── useRoom() [hooks/useRoom.ts]
                                │   │   └── Fetches: /api/connection-details
                                │   └── SessionContext
                                └── ViewController [components/app/view-controller.tsx]
                                    ├── WelcomeView
                                    ├── PreConnectMessage
                                    ├── SessionView
                                    │   ├── ChatTranscript
                                    │   ├── useChatMessages() [hooks/useChatMessages.ts]
                                    │   ├── TileLayout
                                    │   └── AgentControlBar
                                    └── GameMaster
                                        └── useGameState() [hooks/useGameState.ts]
```

---

## 🚀 FLOW SUMMARY FOR PERSE LARITY

**When a user starts a call:**

1. **Frontend loads** → `app/layout.tsx` → `getAppConfig()` → Gets branding & settings
2. **User clicks "Start"** → `welcome-view.tsx` → Calls `startSession()`
3. **Connection hook triggered** → `useRoom.ts` → Calls `POST /api/connection-details`
4. **Backend generates token** → `connection-details/route.ts` → Returns JWT + room details
5. **WebRTC connects** → LiveKit establishes peer connection with backend agent
6. **Agent processes voice** → Python backend processes via Murf Falcon TTS + LLM
7. **Frontend displays** → `session-view.tsx` → Shows chat + agent responses in real-time
8. **State management** → `useChatMessages()` + `useGameState()` → Tracks all interactions

---

## 📝 IMPORTANT NOTES

1. **All API calls** go through `/api/connection-details` for token generation
2. **No direct HTTP backend calls** - all communication via LiveKit WebRTC
3. **Configuration is server-rendered** - prevents client-side token leaks
4. **Hooks manage state locally** - no Redux/external state management
5. **Components are client-side** - use `'use client'` directive
6. **Environment variables** are crucial for security and functionality
