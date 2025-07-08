# README.md

# 🔐 Advanced AI-Enhanced Educational Security & Tutoring System

## Overview

This document outlines an advanced strategy for upgrading educational platforms such as Acellus using local AI integration, ethical tutoring agents, content protection, and military-grade anti-tampering techniques. It addresses the obsolescence of legacy security methods, harnesses client-side inference via WebLLM, and offers modular, scalable defenses with negligible server costs.

> ⚠️ Author's Note: I am a 17-year-old student currently using the Acellus system. I created this solution after closely analyzing its limitations, particularly with respect to modern AI tools. While I initially considered submitting this project to the science fair, I ultimately chose not to, as I felt it may be judged unfairly due to its critique of the very system it relates to.

---

## 🚩 Motivation

Educational software is rapidly falling behind as students leverage AI to bypass safeguards. Legacy systems like copy-paste prevention, obscured question text via image rendering, and generic anti-debugging measures no longer work in the age of powerful browser-integrated tools and large language models (LLMs).

Rather than fight against AI, this system integrates it **ethically and efficiently**, protecting content and enhancing learning.

---

## ❌ Problems Identified

1. **Obsolete Question Protection**: Image-based question obfuscation is nullified by screen-capture-based LLMs (e.g., Copilot, GPT-4-Vision).
2. **AI Tutoring Barriers**: Lack of structured text data hinders intelligent assistance.
3. **Server Cost & Latency**: Hosting inference models like GPT or Claude is expensive and introduces latency.
4. **Tamper Potential via DevTools**: Extensions like Anti-Anti-Debug bypass JavaScript-level protection.
5. **Unsafe LLM Outputs**: LLMs can generate offensive, inappropriate, or harmful responses if unfiltered.
6. **Client-Side Code Modification**: Students may alter JS to generate automatic AI answers or disable security.

---

## ✅ Strategic Solutions

### 1. **Client-Side LLMs with WebLLM**

Use [WebLLM](https://github.com/mlc-ai/web-llm) to run models like Phi-3-mini-4k-instruct **entirely in-browser** using WebGPU.

**Benefits:**

* Zero server cost
* No latency
* Scalable to any user count
* Immune to API misuse

**Recommended Model:** [Phi-3-mini-4k-instruct](https://huggingface.co/microsoft/Phi-3-mini-4k-instruct)

```javascript
import { ChatInterface } from 'webllm';
const chat = await ChatInterface.load('phi3-mini');
const prompt = `You're a tutoring assistant. Help the student understand Pythagorean Theorem.`;
const reply = await chat.chat(prompt);
console.log(reply);
```

> Add an iframe-embedded or shadow DOM tab with this functionality.

### 2. **Structured Prompting Strategy**

Use two prompting modes:

#### a. **Analytical Mode (Single Query)**

```plaintext
"Given the question: [X], and the topic: [Y], generate a new version and explain how to solve it from first principles."
```

#### b. **Interactive Tutoring Mode (Chat Agent)**

```plaintext
"You are a Socratic tutor helping the student understand the topic of [Subject]. Never reveal the original answer or question. Walk them through conceptual analogs."
```

Add a UI toggle for novice/expert difficulty modes.

---

### 3. **Two-Layer AI Output Filtering**

#### a. **Hard-Filter Profanity**

Use a keyword blacklist (e.g., [LDNOOBW List](https://github.com/LDNOOBW/List-of-Dirty-Naughty-Obscene-and-Otherwise-Bad-Words)) before prompt injection.

```javascript
const blacklist = ["badword1", "badword2"];
if (blacklist.some(word => input.includes(word))) throw new Error("Unsafe input");
```

#### b. **Post-Filter Classifier**

Run all AI outputs through a secondary model or logic layer that returns `"yes"` or `"no"`:

```javascript
async function filterOutput(response) {
  const judgment = await smallModel.classify(response);
  return judgment === 'yes';
}
```

Deploy this classifier via WebLLM using a smaller distilled version of the main model. It must be **stateless** and **non-trickable**.

---

### 4. **DevTools Tamper Detection via Resize**

Detect Chrome DevTools (F12) using screen resize heuristics:

```html
<script>
const initW = window.innerWidth;
const initH = window.innerHeight;

window.addEventListener('resize', () => {
  if (window.innerWidth !== initW || window.innerHeight !== initH) {
    window.close();
    document.body.innerHTML = '<h1>Access Denied</h1>';
    document.body.style.background = 'black';
  }
});
</script>
```

> Also consider measuring the aspect ratio of the window on a set interval.

> 🛠️ Additionally, **utilizing a JavaScript obfuscator** is strongly recommended. This makes it significantly more difficult for attackers to reverse-engineer or modify client-side logic, especially for validation or AI input interception code.

---

### 5. **Encrypted Heartbeat System (Anti-Tamper)**

Implement client-server authentication using a rotating, time-based encrypted checksum.

#### a. Server:

* Generates a token using: `HMAC(time + salt)`
* Sends salt + token every 10 seconds

#### b. Client:

* Computes checksum from JS code + DOM state + current time
* Sends back HMAC-protected hash

#### c. Server:

* Verifies checksum integrity
* Invalid/missing = session kill + forced re-login

```js
// Pseudocode
setInterval(() => {
  const checksum = sha256(localState + serverSalt + currentTime);
  fetch("/validate", { method: "POST", body: checksum });
}, 10000);
```

---

## 📚 Additional Recommendations

* **OCR-resistant fonts** or **dynamic canvas rendering** for questions
* **Encrypted DOM Shadowing** for critical logic
* **AI Activity Logging** (anonymized) for insights into concept gaps
* Use **Learning Record Stores (LRS)** like [xAPI](https://xapi.com/) to track AI-agent interactions
* Integrate **multi-modal AI** (text + image for diagrams)
* Enable **offline caching** for AI + lessons with `IndexedDB`
* Add **rate limiting** for AI queries
* Apply **token permutation watermarking** on generated questions to detect leaked content

---

## 🔐 Ethics and Safety

* Never return original questions or answers.
* Use AI for **teaching**, not shortcuts.
* Ensure **complete transparency** in AI usage with opt-in prompts.
* Disable memory or personalization by default.

> AI should scaffold human understanding, not replace it.

---

## 🧠 Future Work

* Support audio-based question reading with whisper.js
* Mobile PWA version with offline tutoring
* Federated model fine-tuning using user interactions (privacy-respecting)
* Live session transcription and summarization via Whisper or SeamlessM4T

---

## 🪪 License and Compliance

All models deployed must comply with:

* [Microsoft AI License](https://huggingface.co/microsoft/Phi-3-mini-4k-instruct)
* FERPA & COPPA (for U.S. students)
* GDPR (if in EU)

---

## 🤝 Contributing

This is a work in progress. PRs, issues, and audits welcome. To contribute:

1. Fork the repo
2. Clone and install WebLLM
3. Implement your improvement in `/src`
4. Submit a PR with test cases and reasoning

---

## 📫 Contact
Brytankelly.com


