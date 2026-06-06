# GemmaJnana

A local development stack to download, serve, and interact with Google's **Gemma 4 (Effective 4B)** model using **Ollama** and a **FastAPI** gateway. The package comes with a beautiful, fully animated chat playground UI.

---

## 📂 Project Architecture

*   **`start.sh`**: The master automation script. It:
    1. Checks if Ollama is installed and automatically updates it to the latest version if needed.
    2. Starts the Ollama service.
    3. Verifies and pulls the `gemma4:e4b` model.
    4. Launches the FastAPI server in the background.
    5. Serves the web interface in the foreground.
*   **`stop.sh`**: Gracefully terminates all background servers (Ollama, FastAPI app, and the lightweight HTTP server).
*   **`app.py`**: A clean, single-file FastAPI gateway that uses the official `ollama` Python SDK to expose endpoints for health checks (`/health`), text completions (`/chat`), and SSE-based chunk streaming (`/chat/stream`).
*   **`index.html`**: A premium dark-themed web playground featuring custom typography, responsive design, a bottom-anchored text input, and a pulsing blinking loader indicator.

---

## ⚡ Prerequisites

To run this application, make sure you have:
1.  **macOS** (since automated updates look for `/Applications/Ollama.app`).
2.  **Python 3.x** with the required libraries:
    ```bash
    pip install fastapi uvicorn ollama
    ```

---

## 🚀 How to Run

1.  **Start the entire service stack**:
    ```bash
    ./start.sh
    ```
    This script will take care of updating Ollama, downloading the model, and launching the servers.

2.  **Open the Web Playground**:
    Navigate to [**`http://localhost:8080`**](http://localhost:8080) in your browser.

3.  **Graceful shutdown**:
    To shut down the web server, press `Ctrl+C` in your terminal. Alternatively, to ensure all background processes (FastAPI, Ollama) are stopped, run:
    ```bash
    ./stop.sh
    ```

---

## 🔧 API Reference

The FastAPI gateway runs at `http://127.0.0.1:8000` and offers the following endpoints:

*   `GET /health`: Diagnoses connection health and returns active model metadata.
*   `POST /chat`: Completes message requests (non-streaming).
*   `POST /chat/stream`: Initiates an SSE (`text/event-stream`) chat channel.
