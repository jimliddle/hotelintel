 # 🏨 Jim's Hotel Intel AI
    2
    3 **Jim's Hotel Intel AI** is a professional-grade, privacy-focused hotel and resort research tool. It leverages the
      **Gemini 2.5 Flash** model and the **Tavily Search API** to synthesize objective, data-driven reviews from
      TripAdvisor, YouTube, travel blogs, and Reddit—all within a secure, client-side environment.
    4
    5 [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
    6 ![Architecture: Client-Side](https://img.shields.io/badge/Architecture-Client--Side-green)
    7 ![Security: AES-GCM](https://img.shields.io/badge/Security-AES--GCM-orange)
    8 ![Mobile: Responsive](https://img.shields.io/badge/Mobile-Responsive-blue)
    9
   10 ---
   11
   12 ## ✨ Key Features
   13
   14 *   **Deep Analysis**: Synthesizes conflicting reviews into objective sections: *Rating/Vibe, Quietness, Dining,
      Surroundings, Logistics, and Brutally Honest Negatives.*
   15 *   **Agentic Search**: Multi-turn tool calling allows the AI to perform iterative searches to "drill down" into
      specific guest complaints or amenities.
   16 *   **Visual Research**: Automatically retrieves and displays high-quality resort photos via Markdown formatting.
   17 *   **Privacy-First**: Your API keys and review history are stored **only** in your browser's local IndexedDB. No
      data is sent to a central server.
   18 *   **Secure Backups**: Export your entire history to an encrypted `.enc` file using AES-GCM (256-bit)
      browser-native cryptography.
   19 *   **Organization**: Tag, filter, rename, and manage individual review threads for long-term trip planning.
   20 *   **Fully Responsive**: Optimized for both deep desktop research and on-the-go mobile use with a sliding sidebar
      menu.
   21
   22 ---
   23
   24 ## 🚀 Getting Started
   25
   26 ### 1. Prerequisites
   27 You will need API keys from the following services (both offer free tiers):
   28 1.  **Google Gemini API**: [Get a key at Google AI Studio](https://aistudio.google.com/)
   29 2.  **Tavily Search API**: [Get a key at Tavily.com](https://tavily.com/)
   30
   31 ### 2. Installation
   32 1.  Clone the repository or download the `hotelreview.html` file.
   33 2.  Open `hotelreview.html` in any modern web browser (Chrome, Edge, Safari, Firefox).
   34 3.  Click the **Gear Icon** (Settings) in the menu and enter your API keys.
   35 4.  Start your first review!
   36
   37 ---
   38
   39 ## 🛠 Technical Implementation Details
   40
   41 ### Architecture: Serverless Client-Side
   42 The application is a **Single Page Application (SPA)** built with vanilla JavaScript. It requires no Node.js
      backend or database server, making it extremely portable and cost-effective.
   43
   44 #### 🧠 Intelligence & Search (Agentic Loop)
   45 The core logic uses a **recursive while-loop** to handle Gemini's agentic capabilities:
   46 *   **Model**: `gemini-2.5-flash` via the Google AI Gateway.
   47 *   **Multi-Turn**: The app maintains a `messages` array, passing the full conversation context back to the model
      on every follow-up.
   48 *   **Tool Calling**: When Gemini identifies a need for live data, it emits a `functionCall`. The app executes
      this via the **Tavily API**, appends the results to the context, and re-invokes the model until a final answer is
      generated (up to 10 turns).
   49 *   **Thinking Mode**: Specifically detects and handles "Thinking" parts in the Gemini response stream to prevent
      infinite loops and improve AI reasoning transparency.
   50
   51 #### 💾 Data Persistence (IndexedDB)
   52 To overcome the 5MB limit of `localStorage`, the app uses **IndexedDB** for high-performance, asynchronous storage
      of large review histories.
   53 *   **Schema**: A single object store `reviews` indexed by a UUID.
   54 *   **History Injection**: Every new review prompt automatically injects the names of previously reviewed resorts
      from the DB to provide "global" context for comparative questions.
   55
   56 #### 🔒 Security & Cryptography
   57 The backup system uses the browser's native **Web Crypto API**:
   58 *   **Key Derivation**: Uses **PBKDF2** with 100,000 iterations of SHA-256 and a random 16-byte salt to derive a
      256-bit AES key from your password.
   59 *   **Encryption**: Uses **AES-GCM (256-bit)** for authenticated encryption.
   60 *   **Sanitization**: AI-generated Markdown is parsed via `Marked.js` and strictly sanitized using `DOMPurify` to
      prevent XSS attacks from third-party search results.
   61
   62 #### 📱 UI/UX (Tailwind & FontAwesome)
   63 *   **Styling**: Utility-first CSS via **Tailwind CSS**.
   64 *   **Mobile Support**: Implements a sliding drawer menu and responsive image scaling for smartphone
      compatibility.
   65 *   **Live Feedback**: Real-time turn-based status updates (e.g., "Analyzing (Step 3)...", "Searching web for...")
      provide a responsive experience during long-running research tasks.
   66
   67 ---
   68
   69 ## 📄 License
   70
   71 Distributed under the MIT License. See `LICENSE` for more information.
