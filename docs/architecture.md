Frontend Architecture: GuardAI Integration
==========================================

1\. High-Level Design
---------------------

The system will be built as a **React Context Provider ()** that wraps the main application. It exposes a single hook useGuardAI() to the chat components. This ensures "Drop-in" compatibility with the existing Discord-like app.

### 1.1 The Hybrid Filtering Pipeline

To ensure **Zero Lag (NFR1)**, we will use a tiered detection strategy:

1.  **Tier 1: Local Sieve (0ms)**
    
    *   **Technology:** JavaScript Regex / Bloom Filter.
        
    *   **Function:** instantly blocks known slurs/hardcoded bad words.
        
    *   **Action:** Blocks immediately. No API call needed.
        
2.  **Tier 2: AI Sentinel (100-300ms)**
    
    *   **Technology:** Google Gemini Flash (via Stateless Proxy).
        
    *   **Function:** Contextual analysis (e.g., distinguishing "I hate you" from friendly banter, or detecting NSFW images).
        
    *   **Action:** Runs asynchronously; if a violation is found, the message is retracted or flagged.
        

2\. Component Structure
-----------------------

Code snippet

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   graph TD      A[User Inputs Message] --> B[GuardAI Hook]      B --> C{Local Regex Check}      C -- Match Found --> D[Block & Warn User]      C -- Clean --> E[Allow UI Optimistic Update]      E --> F[Async Call: Stateless Proxy]      F --> G[Gemini API]      G --> F      F --> B      B -- Verdict: Bad --> H[Retract Message & Strike ++]   `

3\. Data & State Management
---------------------------

### 3.1 Local State (React Context)

We will avoid Redux to keep the module lightweight.

TypeScript

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   interface GuardState {    strikes: number; // 0, 1, 2, 3 (Banned)    isBanned: boolean;    lastViolation: string | null;  }   `

### 3.2 Database Sync (Interface)

Since GuardAI doesn't own the DB, it requires an adapter pattern to talk to the main app's backend.

TypeScript

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   // The main app must pass this function to our Provider  type UpdateUserStatus = (status: { banned: boolean }) => Promise;   `

4\. Security Implementation (The Proxy)
---------------------------------------

We will provide a reference implementation for a serverless handler (Node.js/Edge):

*   **Input:** { text: string, type: 'text'|'image' }
    
*   **Process:**
    
    1.  Validate inputs.
        
    2.  Construct Prompt: _"Analyze this text for NSFW/Toxic content. Return JSON: { safe: boolean, reason: string }."_
        
    3.  Call generative-ai (Vertex/Gemini) with process.env.GEMINI\_API\_KEY.
        
    4.  Return Result.
        
*   **Storage:** NONE. No logs, no DB connections.
    

5\. Technology Stack
--------------------

*   **Core Logic:** TypeScript / React Hooks.
    
*   **AI SDK:** Google Generative AI SDK (Server-side/Proxy).
    
*   **Image Analysis:** HTML5 Canvas (for resizing before sending to AI to save bandwidth).
    

6\. Integration Guide
---------------------

The main app developers will integrate it like this:

JavaScript

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML

  `onBan={(userId) => db.users.update(userId, { banned: true })}    adminMode={currentUser.isAdmin}  >`  

**Architect's Review of PRD:**The PRD is solid, but I suggest one minor update to **Story 1.1**:

*   _Current:_ "Setup secure AI API integration..."
    
*   _Refinement:_ Explicitly mention "Implementation of a Stateless Proxy to hide API keys."
    

**Status:** Architecture Design Complete.**Next Agent:** Product Manager (to approve the refinement) or Project Owner (Simulated user) to Validate.
