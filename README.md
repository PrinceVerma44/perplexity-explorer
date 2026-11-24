# Perplexity Explorer

An interactive, single-file browser application demonstrating how language models measure surprise and calculate **perplexity** using real in-browser inference with Transformers.js.

![Perplexity Explorer](https://img.shields.io/badge/Language%20Model-DistilGPT--2-purple)
![Tech Stack](https://img.shields.io/badge/Tech-HTML%20%7C%20CSS%20%7C%20Vanilla%20JS-blue)
![Zero Backend](https://img.shields.io/badge/Backend-None%20(Client--side)-green)

---

## Features

- **No Frameworks, No Backend, No API Keys:** Pure HTML, CSS, and plain JavaScript in a single self-contained file (`index.html`).
- **Real In-Browser Model:** Powered by [Transformers.js](https://huggingface.co/docs/transformers.js/index) running `Xenova/distilgpt2` quantized to `q8` directly on WebAssembly/ONNX Runtime.
- **Section 1 – Interactive Input:**
  - Enter custom sentences or choose preset samples (*"I love Python"*, *"Python love I"*, *"The capital of France is Paris"*).
  - Lazy loads the model only when calculation is first requested.
- **Section 2 – Token Probabilities & Perplexity Breakdown:**
  - Token IDs prefixed with `<|endoftext|>` (`50256`) so even the first word receives a conditional probability.
  - Numerically stable softmax on extracted logits at each preceding token position.
  - Detailed breakdown table listing tokens (showing spaces as `␣`), probabilities $P$, and reciprocal surprisal $1/P$.
  - 3-step calculation: product of $1/P$, token count $N$, and $\text{Perplexity} = (\text{Product})^{1/N}$ computed via log-space ($\exp(\text{average of } -\ln P)$) to avoid numeric overflow.
- **Section 3 – Next Token Predictions:**
  - Displays top 8 candidate next tokens with horizontal probability percentage bars, highlighting the top prediction.
- **Section 4 – Try It by Hand (Simulation):**
  - Interactive simulator to test how probabilities affect perplexity without running the model.
  - Add/remove custom token rows with live updates.
  - Built-in presets:
    - `"I love Python"` ($0.5, 0.8, 0.2$) $\rightarrow$ Perplexity: **2.32**
    - `"All 1/8"` ($8$ tokens at $0.125$ each) $\rightarrow$ Perplexity: **8.00**
    - `"One surprising word"` ($0.9, 0.9, 0.9, 0.01$)

---

## Getting Started

Because the application loads ES modules and fetches ONNX model weights in the browser, modern browsers require it to be served via HTTP rather than `file://` to prevent CORS issues.

### 1. Clone the repository
```bash
git clone https://github.com/PrinceVerma44/perplexity-explorer.git
cd perplexity-explorer
```

### 2. Start a local server

Using Python:
```bash
python3 -m http.server 8000
```

Or using Node:
```bash
npx serve .
```

### 3. Open in your browser
Visit:
```
http://localhost:8000
```

---

## License

MIT License. Feel free to use and adapt!
