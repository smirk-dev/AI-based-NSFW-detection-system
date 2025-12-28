Project Brief: GuardAI-Client-Mod
=================================

1\. Executive Summary
---------------------

Develop a lightweight, intelligent, and privacy-focused moderation integration ("GuardAI") for a global social networking platform. The system will use AI to detect NSFW content (text, media, links) in real-time within the user's browser, enforcing a "two-strike" warning system before triggering a ban. It ensures user data privacy by avoiding external data storage for analysis while allowing Admin intervention to reverse false positives.

2\. Problem Statement
---------------------

The target social platform (Discord-like) enables global communication but lacks a robust, automated safeguard against NSFW content. Manual moderation is unscalable, and traditional server-side moderation compromises user privacy and introduces latency. A client-side, intelligent solution is required to sanitize interactions without saving sensitive chat logs.

3\. Key Objectives
------------------

*   **Real-time Censorship:** Instantly redact or block NSFW words, images, and links.
    
*   **Privacy-First Architecture:** Perform analysis client-side (in-browser) to maintain data integrity.
    
*   **Automated Discipline:** Implement a counter system: 2 Warnings $\\to$ Automatic Ban.
    
*   **Admin Sovereignty:** Provide an interface for Admins to view logs (metadata only) and override bans or reset warnings.
    
*   **Performance:** Zero perceptible lag during chat operations.
    

4\. Target Audience
-------------------

*   **End Users:** People connecting globally in private and group chats who expect a safe environment.
    
*   **Admins:** Platform moderators needing tools to manage false positives and user discipline.
    
*   **Developers:** The team integrating this module into the main application.
    

5\. Functional Requirements
---------------------------

*   **AI Integration:** Utilize smart models (e.g., Gemini API or TensorFlow.js) for context-aware detection. _Note: Architect must address API Key security in client-side deployment._
    
*   **Content Filtering:**
    
    *   **Text:** Regex + AI Sentiment analysis for slurs/NSFW.
        
    *   **Media:** Image recognition for nudity/gore.
        
    *   **Links:** Phishing/NSFW URL detection.
        
*   **User State Management:** Track "Strike Count" locally and sync ban status to the main application's Database.
    
*   **Visual Feedback:**
    
    *   Toast/Modal warnings for the user.
        
    *   "Message Blocked" indicators in chat.
        

6\. Non-Functional Requirements
-------------------------------

*   **Latency:** Analysis must occur in $< 200ms$ to prevent chat delay.
    
*   **Security:** AI API keys must not be exposed to the client users.
    
*   **Integration:** Must be framework-agnostic or React/JS compatible (standard for modern web apps).
