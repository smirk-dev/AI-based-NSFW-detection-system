Product Requirements Document: GuardAI-Client-Mod
=================================================

1\. Goals and Background Context
--------------------------------

**Goal:** To develop "GuardAI," a lightweight, privacy-centric moderation integration for a Discord-like web application. The system will detect and censor NSFW content (text, media, links) in real-time within the client browser, enforcing a strict "two-strike" policy that bans users while allowing Admin overrides.

**Background:** The target platform connects global users in private and group chats. Current manual moderation is insufficient. The client requires a solution that respects user data privacy by performing analysis locally (in-browser) or securely without data persistence, while utilizing advanced AI models (like Gemini) for detection.

2\. Requirements
----------------

### 2.1 Functional Requirements (FR)

*   **FR1 - Real-time Text Censorship:** The system MUST intercept outgoing messages and mask/block words or phrases identified as NSFW (slurs, sexually explicit, hate speech) before they are displayed to other users.
    
*   **FR2 - Media & Link Scanning:** The system MUST analyze uploaded images and shared links for NSFW content using AI prediction models.
    
*   **FR3 - Two-Strike Discipline System:**
    
    *   **Strike 1 & 2:** User receives a visible warning (Toast/Modal) upon attempting to send NSFW content. The content is blocked.
        
    *   **Strike 3 (Action):** The user is automatically banned. This state is synced to the application's main database.
        
*   **FR4 - Admin Override:** Admins MUST have an interface to view a user's strike status and perform actions: Reset Strikes, Unban User, or Manual Ban.
    
*   **FR5 - Smart AI Prediction:** Utilize an AI API (e.g., Gemini) to contextually analyze content, rather than relying solely on static blacklists.
    

### 2.2 Non-Functional Requirements (NFR)

*   **NFR1 - Zero-Lag Experience:** Content analysis must complete in **< 200ms** to avoid perceptible delays in chat.
    
*   **NFR2 - Client-Side/Privacy First:** Analysis should occur as close to the client as possible. No chat logs should be stored in GuardAI's internal buffers; decisions are ephemeral.
    
*   **NFR3 - Secure API Key Management:** Since the app is client-side, the AI Model API keys (Gemini) MUST NOT be exposed in the frontend code. (Architectural constraint: requires a secure proxy or edge function).
    
*   **NFR4 - Lightweight Integration:** The module must be a distinct library or hook set, easily pluggable into the existing React/JS chat application.
    

3\. Technical Assumptions
-------------------------

*   **Architecture:** Client-Side Logic with Serverless Proxy (for secure API calls) or On-Device Model (TensorFlow.js) if API latency is too high.
    
*   **Database:** The main application's existing database will store the is\_banned and strike\_count flags; GuardAI does not own its own database.
    
*   **AI Model:** Google Gemini API (via secure relay) or specialized lightweight client models.
    

4\. Epic List
-------------

### Epic 1: Core Detection Engine

**Goal:** Build the intelligent filter that scans text, images, and links without slowing down the browser.

*   **Story 1.1:** Setup secure AI API integration (Gemini) preventing key exposure.
    
*   **Story 1.2:** Implement Text Analysis Logic (Regex + AI Sentiment fallback).
    
*   **Story 1.3:** Implement Image Analysis Logic (NSFW classification).
    
*   **Story 1.4:** Implement Link Analysis Logic (Phishing/Malware/NSFW lists).
    

### Epic 2: Discipline & State System

**Goal:** Manage the user's "Strike" state and enforce bans.

*   **Story 2.1:** Create local state manager for "Strike Count".
    
*   **Story 2.2:** Implement Database Sync to update User Profile (Ban Status).
    
*   **Story 2.3:** Create the "Ban Hammer" trigger (locks user out of chat input).
    

### Epic 3: User Feedback & Admin Tools

**Goal:** UI for warnings and Admin controls.

*   **Story 3.1:** Design and implement User Warning Modals (UI).
    
*   **Story 3.2:** Build Admin Dashboard Panel for unbanning/resetting strikes.
    

**Product Manager Note:** The biggest technical risk is **NFR3 (Security)** vs. the client's request for "In-browser" analysis. Exposing a Gemini API key in frontend code is a major security violation.

*   **Recommendation:** I am flagging this for the **Architect** to design a "Stateless Edge Proxy" or similar solution that keeps the key hidden while maintaining the "client-side feel."
