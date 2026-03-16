To show the judges exactly how the project was built, the best approach is to provide a phenomenal **README.md** for your GitHub repository. Judges look for clean architecture diagrams and clear instructions on how they can run it themselves. 

Here is the perfect `README.md` for your OmniSpark repository. 

***

# OmniSpark
**The Hands-Free, Multimodal Hardware Engineering Co-Pilot**

[![Google Gemini Live Agent Challenge](https://img.shields.io/badge/Google-Gemini_Live_Agent_Challenge-4081ec?style=for-the-badge&logo=google)]()
[![Next.js](https://img.shields.io/badge/Next.js-000000?style=for-the-badge&logo=nextdotjs&logoColor=white)]()
[![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)]()
[![WebSockets](https://img.shields.io/badge/WebSockets-010101?style=for-the-badge&logo=socketdotio&logoColor=white)]()

OmniSpark completely shatters the text-box paradigm. It is a real-time AI agent built on the Gemini Multimodal Live API that constantly watches your workbench via webcam, reads analog multimeters, traces physical wires, and provides zero-latency voice guidance—allowing engineers to troubleshoot hardware without ever dropping their tools.

## 🏗️ How We Built It (Architecture)
OmniSpark was engineered for absolute lowest latency, operating cleanly on the edge with a decoupled architecture. 

### 1. The Real-Time Proxy (Backend)
Browsers cannot safely expose the `GEMINI_API_KEY`. We built a lightweight `Node.js / Express` server that establishes a persistent, bi-directional WebSocket connection (`wss://generativelanguage.googleapis.com`) using the new GenAI SDK `BidiGenerateContent` endpoint. 

*   **Function Calling (Tools):** The proxy intercepts tool calls from Gemini. When the model visually identifies a microchip (e.g. an ESP8266), it triggers `search_datasheet()`. The proxy queries our mock Vertex AI RAG knowledge-base to retrieve exact pinout voltages and injects the JSON response back into the live context stream instantly.

### 2. The Hardware Co-Pilot HUD (Frontend)
We built a futuristic, edge-ready `Next.js` dashboard acting as the user's "smart glasses." 
*   **Vision Ingestion:** We use raw WebRTC (`getUserMedia`) to access the camera, sampling the stream into base64 JPEG frames dynamically (at roughly 1 FPS) to maintain spatial awareness while preventing token starvation.
*   **Custom Audio Engine:** HTML5 `<audio>` tags are too slow for real-time conversation. We built a custom 16kHz PCM buffer player leveraging the browser’s raw `AudioContext` to stitch and play the `Float32Array` voice chunks streaming back from Gemini with zero clipping.

---

## 🚀 Running OmniSpark Locally

Judges, you can spin this up right now! You just need a webcam and a Gemini API key.

### Prerequisites
*   Node.js (v18+)
*   A Google AI Studio API Key (`GEMINI_API_KEY`)

### 1. Start the WebSocket Proxy
```bash
cd backend
npm install
# Create your basic .env file
echo "GEMINI_API_KEY=your_key_here" > .env
npm run dev
```
*The proxy will spin up on `localhost:8080.`*

### 2. Start the Frontend HUD
```bash
cd frontend
npm install
npm run dev
```

### 3. Test the Live Agent!
Open your browser to `http://localhost:3000`. 
1. Allow Camera and Microphone permissions.
2. The UI will indicate *Agent Link Established*.
3. **The Demo:** Point your camera at an Arduino, a breadboard, or a multimeter, and ask: *"OmniSpark, can you see my circuit? Where should I plug in the 3.3V power?"*

---

## 🔮 Project Structure
```text
omnispark/
├── backend/
│   ├── server.js              # Express & Socket.io initialization
│   ├── services/
│   │   ├── gemini.js          # Direct WSS connection to Gemini Live API
│   │   └── tools.js           # RAG Function Calling mock database
│   └── package.json
└── frontend/
    ├── src/
    │   ├── app/page.tsx       # Main Cyberpunk HUD layout
    │   ├── components/
    │   │   ├── CameraStream.tsx # WebRTC Video frame extraction
    │   │   └── AudioPlayer.tsx  # Low-level AudioContext PCM buffer player
    │   └── lib/socket.ts      
    └── package.json
```
