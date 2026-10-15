
---

# **OpenClaw + Ollama Development Environment Guide**  
### *For coding assistance, skill execution, and local development workflows*

---

## **1. Overview**
This guide explains:

- How to set up your environment for daily development  
- How to run OpenClaw + Ollama in a stable, lightweight mode  
- How to avoid Gateway crashes, stale locks, and corrupted state  
- How to shut everything down cleanly  
- How to keep context and storage healthy  

This setup gives you a **Claude Code / Copilot CLI–style coding assistant**, powered by local inference.

---

# **2. Architecture You Should Use**
For coding assistant workflows, you only need:

### ✔ **Ollama**  
Local LLM inference engine.

### ✔ **OpenClaw CLI (TUI)**  
Your coding assistant interface.

### ✔ **Skills**  
File reading, editing, searching, refactoring, running commands.

### ❌ **OpenClaw Gateway (background service)**  
You *do not* need the Gateway for coding assistant mode.  
It is only required for:

- multi-agent orchestration  
- RAG indexing  
- dashboard  
- long-running workflows  

Running without the Gateway avoids:

- lock corruption  
- heartbeat stalls  
- restart loops  
- plugin cleanup failures  
- SQLite ownership errors  

This is the stable mode for daily development.

---

# **3. Daily Startup Workflow (Lean + Stable)**

### **Step 1 — Start Ollama**
You can preload a model or let OpenClaw start it automatically.

Optional warmup:

```
ollama run llama3.1 "warmup"
```

Close with Ctrl+C — the model stays loaded.

### **Step 2 — Start OpenClaw in CLI mode**
Open a terminal in your project directory:

```
openclaw
```

This launches the TUI and drops you into a coding assistant session.

### **Step 3 — Use skills like Claude Code**
Examples:

```
/read src/index.ts
```

```
/edit src/pipeline.py
```

```
Use $code_search to find where this function is defined.
```

```
Use $refactor to rewrite this module.
```

This is your coding assistant workflow.

### **Step 4 — Inspect context when needed**
Use:

```
/status
/context list
/context detail
/context map
```

These commands show:

- system prompt size  
- injected workspace files  
- tool schema sizes  
- skill list sizes  
- transcript token usage  

---

# **4. Daily Shutdown Workflow (Clean + Safe)**

### **Step 1 — Exit the TUI**
Inside OpenClaw:

```
/exit
```

or Ctrl+C.

### **Step 2 — Stop Ollama (optional)**
If you want to free CPU/GPU:

```
ollama stop
```

### **Step 3 — Optional cleanup**
If you want a fresh environment:

```
rm -rf ~/.openclaw/agents/main/sessions/*
```

If you re-index repos often (not needed in coding mode):

```
rm -rf ~/.openclaw/index/*
```

This keeps your environment fast and clean.

---

# **5. Storage & Context Management**

### **Context = everything sent to the model**
From your attached doc:

> “Context is **everything OpenClaw sends to the model for a run**.”

This includes:

- system prompt  
- conversation history  
- tool calls/results  
- injected workspace files  
- tool schemas  
- skill metadata  

Use:

```
/compact
```

to summarize older history and free window space.

### **Memory ≠ Context**
Memory is stored on disk and persists across sessions.  
Context is only what fits inside the model’s window.

### **Session cleanup**
Delete old sessions:

```
rm -rf ~/.openclaw/agents/main/sessions/*
```

### **Model cleanup**
List models:

```
ollama list
```

Remove unused models:

```
ollama rm <model>
```

---

# **6. When You *Should* Use the Gateway**
Only use the Gateway if you want:

- multi-agent recruiter simulation  
- RAG indexing  
- long-running workflows  
- dashboard  
- plugin integrations  

If you switch to that mode:

```
openclaw gateway start
openclaw   # in a NEW terminal
```

And shut down cleanly:

```
openclaw gateway stop
```

But for coding assistant mode, **do not run the Gateway**.

---

# **7. Recommended Models for Coding Assistant Mode**
- **qwen2.5-coder:3b** — fast, strong coding  
- **llama3.1** — general reasoning  
- **deepseek-r1** — chain-of-thought heavy tasks  
- **phi3.5** — lightweight reasoning  

Use whichever fits your workflow.

---

# **8. Summary**
Your ideal dev environment:

### ✔ Ollama running locally  
### ✔ OpenClaw CLI (TUI)  
### ✔ Skills for file editing, searching, refactoring  
### ❌ No Gateway (unless doing multi-agent/RAG work)  
### ✔ Clean startup/shutdown routine  
### ✔ Occasional session/index cleanup  

This gives you a **stable, fast, Claude Code–style coding assistant** with none of the Gateway overhead.

---