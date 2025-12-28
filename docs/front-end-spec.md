UI/UX Specification: GuardAI Integration
========================================

1\. UX Principles
-----------------

*   **Immediate Feedback:** Censorship happens _before_ the message hits the chat stream.
    
*   **Clarity:** Warnings must explicitly state _why_ content was blocked (e.g., "Contains prohibited words").
    
*   **Friction:** The experience for the offender should be high-friction (modals that require dismissal) to discourage repeat offenses.
    

2\. Key User Flows
------------------

### Flow A: The Offender's Journey

1.  **User Types:** "You are a \[NSFW Word\]."
    
2.  **System Action:** Block send.
    
3.  **Visual Feedback:**
    
    *   **Strike 1:** "Toast" notification (Orange). Text: _"Warning (1/3): Inappropriate content detected. Please be respectful."_
        
    *   **Strike 2:** "Modal" Popup (Red). Blocks screen. Text: _"Final Warning (2/3): Your next violation will result in an immediate ban."_ Requires "I Understand" click to dismiss.
        
    *   **Strike 3:** Ban State. Chat input is disabled/greyed out. Text replaced with: _"You have been banned for violating community guidelines."_
        

### Flow B: The Admin's Journey

1.  **Entry:** Admin clicks "Moderation Tools" icon in chat header.
    
2.  **View:** A data table or card list of Banned/Warned users.
    
3.  **Actions:**
    
    *   **Reset Strikes:** Button resets count to 0.
        
    *   **Unban:** Revokes ban status in DB.
        
    *   **Force Ban:** Immediately applies 3 strikes.
        

3\. UI Component Specs
----------------------

### 3.1 Warning Toast

*   **Color:** #FFA500 (Orange/Warning)
    
*   **Animation:** Slide in from top-right.
    
*   **Duration:** 5 seconds.
    

### 3.2 Ban Modal

*   **Color:** #FF0000 (Red/Danger)
    
*   **Overlay:** 50% Black opacity background blur.
    
*   **Behavior:** Non-dismissible by clicking outside.
    

### 3.3 Chat Input (Banned State)

*   **State:** Disabled (disabled={true})
    
*   **Placeholder:** "You are banned."
    
*   **Style:** Grey background, red border.
