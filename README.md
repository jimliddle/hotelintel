# 🏨 Jim's Hotel Intel AI

**Jim's Hotel Intel AI** is a professional-grade, privacy-focused hotel and resort research tool. It leverages the **Gemini 2.5 Flash** model and the **Tavily Search API** to synthesize objective, data-driven reviews from TripAdvisor, YouTube, travel blogs, and Reddit - all within a secure, client-side environment.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
![Architecture: Client-Side](https://img.shields.io/badge/Architecture-Client--Side-green)
![Security: AES-GCM](https://img.shields.io/badge/Security-AES--GCM-orange)
![Mobile: Responsive](https://img.shields.io/badge/Mobile-Responsive-blue)

---

## ✨ Key Features

*   **Deep Analysis**: Synthesizes conflicting reviews into objective sections: *Rating/Vibe, Quietness, Dining, Surroundings, Logistics, and Brutally Honest Negatives.*
*   **Agentic Search**: Multi-turn tool calling allows the AI to perform iterative searches to "drill down" into specific guest complaints or amenities.
*   **Visual Research**: Automatically retrieves and displays high-quality resort photos via Markdown formatting.
*   **Privacy-First**: Your API keys and review history are stored **only** in your browser's local IndexedDB. No data is sent to a central server.
*   **Secure Backups**: Export your entire history to an encrypted `.enc` file using AES-GCM (256-bit) browser-native cryptography.
*   **Organization**: Tag, filter, rename, and manage individual review threads for long-term trip planning.
*   **Fully Responsive**: Optimized for both deep desktop research and on-the-go mobile use with a sliding sidebar menu.

---

## 🚀 Getting Started

### 1. Prerequisites
You will need API keys from the following services (both offer free tiers):
1.  **Google Gemini API**: [Get a key at Google AI Studio](https://aistudio.google.com/)
2.  **Tavily Search API**: [Get a key at Tavily.com](https://tavily.com/)

### 2. Installation
1.  Clone the repository or download the `hotelreview.html` file.
2.  Open `hotelreview.html` in any modern web browser (Chrome, Edge, Safari, Firefox).
3.  Click the **Gear Icon** (Settings) in the menu and enter your API keys.
4.  Start your first review!

---

## 🛠 Technical Implementation Details

### Architecture: Serverless Client-Side
The application is a **Single Page Application (SPA)** built with vanilla JavaScript. It requires no Node.js backend or database server, making it extremely portable and cost-effective.

#### 🧠 Intelligence & Search (Agentic Loop)
The core logic uses a **recursive while-loop** to handle Gemini's agentic capabilities:
*   **Model**: `gemini-2.5-flash` via the Google AI Gateway.
*   **Multi-Turn**: The app maintains a `messages` array, passing the full conversation context back to the model on every follow-up.
*   **Tool Calling**: When Gemini identifies a need for live data, it emits a `functionCall`. The app executes this via the **Tavily API**, appends the results to the context, and re-invokes the model until a final answer is generated (up to 10 turns).
*   **Thinking Mode**: Specifically detects and handles "Thinking" parts in the Gemini response stream to improve AI reasoning transparency.

#### 💾 Data Persistence (IndexedDB)
To overcome the 5MB limit of `localStorage`, the app uses **IndexedDB** for high-performance, asynchronous storage of large review histories.
*   **Schema**: A single object store `reviews` indexed by a UUID.
*   **History Injection**: Every new review prompt automatically injects the names of previously reviewed resorts from the DB to provide "global" context for comparative questions.

#### 🔒 Security & Cryptography
The backup system uses the browser's native **Web Crypto API**:
*   **Key Derivation**: Uses **PBKDF2** with 100,000 iterations of SHA-256 and a random 16-byte salt to derive a 256-bit AES key from your password.
*   **Encryption**: Uses **AES-GCM (256-bit)** for authenticated encryption.
*   **Sanitization**: AI-generated Markdown is parsed via `Marked.js` and strictly sanitized using `DOMPurify` to prevent XSS attacks.

#### 📱 UI/UX (Tailwind & FontAwesome)
*   **Styling**: Utility-first CSS via **Tailwind CSS**.
*   **Mobile Support**: Implements a sliding drawer menu and responsive image scaling for smartphone compatibility.

---

## 📄 License

Distributed under the MIT License. See `LICENSE` for more information.

---
*Created by Jim - Focused on Deep Resort Intelligence.*
