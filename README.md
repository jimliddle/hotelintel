 # 🏨 Jim's Hotel Intel AI
    2
    3 **Jim's Hotel Intel AI** is a professional-grade, privacy-focused hotel and resort research tool. It leverages the
      **Gemini 2.5 Flash** model and the **Tavily Search API** to synthesize objective, data-driven reviews from
      TripAdvisor, YouTube, travel blogs, and Reddit—all within a secure, client-side environment.
    4
    5 ![License](https://img.shields.io/badge/license-MIT-blue)
    6 ![Architecture](https://img.shields.io/badge/Architecture-Client--Side-green)
    7 ![Security](https://img.shields.io/badge/Security-AES--GCM-orange)
    8
    9 ## ✨ Key Features
   10
   11 - **Deep Analysis**: Synthesizes conflicting reviews into objective sections (Vibe, Quietness, Dining, Logistics,
      and Negatives).
   12 - **Agentic Search**: Multi-turn tool calling allows the AI to perform iterative searches to "drill down" into
      specific guest complaints or amenities.
   13 - **Visual Research**: Automatically retrieves and displays high-quality resort photos via Markdown.
   14 - **Privacy-First**: Your API keys and review history are stored **only** in your browser's local IndexedDB. No
      data is sent to a central server.
   15 - **Secure Backups**: Export your entire history to an encrypted `.enc` file using AES-GCM (256-bit) encryption.
   16 - **Organization**: Tag, filter, rename, and manage individual review threads.
   17 - **Fully Responsive**: Optimized for both Desktop research and on-the-go Mobile use.
   18
   19 ## 🚀 Getting Started
   20
   21 ### Prerequisites
   22 You will need API keys from the following services:
   23 1.  **Google Gemini API**: [Get a free key here](https://aistudio.google.com/)
   24 2.  **Tavily Search API**: [Get a free key here](https://tavily.com/)
   25
   26 ### Installation
   27 1.  Clone the repository or download the `hotelreview.html` file.
   28 2.  Open `hotelreview.html` in any modern web browser.
   29 3.  Click the **Gear Icon** (Settings) and enter your API keys.
   30 4.  Start your first review!
   31
   32 ---
   33
   34 ## 🛠 Technical Implementation Details
   35
   36 ### Architecture: Serverless Client-Side
   37 The application is a **Single Page Application (SPA)** built with vanilla JavaScript. It requires no Node.js
      backend or database server, making it extremely portable and cost-effective.
   38
   39 #### 1. Intelligence & Search (Agentic Loop)
   40 The core logic uses a **recursive while-loop** to handle Gemini's agentic capabilities.
   41 - **Model**: `gemini-2.5-flash` via the Google AI Gateway.
   42 - **Multi-Turn**: The app maintains a `messages` array, passing the full conversation context back to the model on
      every follow-up.
   43 - **Tool Calling**: When Gemini identifies a need for live data, it emits a `functionCall`. The app executes this
      via the **Tavily API**, appends the results to the context, and re-invokes the model until a final answer is
      generated (up to 10 turns).
   44
   45 #### 2. Data Persistence (IndexedDB)
   46 Instead of `localStorage` (which is limited to 5MB), the app uses **IndexedDB** for high-performance, asynchronous
      storage of large review histories and image-heavy threads.
   47 - **Schema**: A single object store `reviews` indexed by a UUID.
   48 - **Normalization**: Older "single-response" threads are automatically normalized into a "message history" format
      upon loading to ensure backward compatibility.
   49
   50 #### 3. Security & Cryptography (Web Crypto API)
   51 The backup system uses the browser's native **Web Crypto API**:
   52 - **Key Derivation**: Uses **PBKDF2** with 100,000 iterations of SHA-256 and a random 16-byte salt to turn your
      password into a 256-bit AES key.
   53 - **Encryption**: Uses **AES-GCM (256-bit)** for authenticated encryption, ensuring your exported data hasn't been
      tampered with.
   54 - **Sanitization**: All AI-generated Markdown is parsed via `Marked.js` and strictly sanitized using `DOMPurify`
      to prevent XSS attacks from third-party search results.
   55
   56 #### 4. UI/UX (Tailwind & FontAwesome)
   57 - **Styling**: Utility-first CSS via **Tailwind CSS** for a responsive, modern interface.
   58 - **Interactivity**: Custom modal system and sidebar overlays designed for 60fps animations.
   59 - **Mobile Support**: Implements a sliding drawer menu and responsive image scaling for smartphone compatibility.
   60
   61 ## 📄 License
   62 Distributed under the MIT License. See `LICENSE` for more information.
