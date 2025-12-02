# User Interface Tool — Give Your AI Beautiful Windows

A Desktop MCP server for opening webview windows so AI can communicate richly with users

> **Create any interface instantly.** Full HTML/CSS/JavaScript support with bidirectional communication. Works everywhere, always.

[![License](https://img.shields.io/badge/license-Apache%202.0-blue.svg)](LICENSE)
[![Python](https://img.shields.io/badge/python-3.8%2B-blue.svg)](https://www.python.org/)
[![Platform](https://img.shields.io/badge/platform-Windows%20%7C%20Linux%20%7C%20macOS-lightgrey.svg)](https://github.com/AuraFriday/mcp-link-server)

---

## Benefits

### 1. 🎨 Create Any Interface Instantly
Full HTML/CSS/JavaScript support means **unlimited UI possibilities** without pre-built components. From simple confirmations to complex multi-step wizards — if you can build it in HTML, your AI can show it to users.

### 2. 🌍 Works Everywhere, Always  
Cross-platform Qt WebEngine with battle-tested thread-safe queue communication. Windows, Mac, Linux — one tool, zero platform-specific code, perfect rendering every time.

### 3. 🛡️ Smart and Safe
Auto-resize eliminates scrollbar guesswork. Timeout protection prevents hung dialogs. HMAC token security ensures proper usage. Graceful error handling with detailed diagnostics.

---

## Why This Tool is Unmatched

**Most AI tools can't show users anything.** They're limited to text responses in a chat window. This tool breaks that limitation completely.

**Other UI frameworks require pre-built components.** Want a custom form? Build it in their framework, learn their API, fight their limitations. This tool? Just write HTML.

**Platform-specific UI is a nightmare.** Write once for Windows, rewrite for Mac, rewrite again for Linux. This tool renders identically everywhere via Qt WebEngine.

**The secret sauce:** Thread-safe queue communication between MCP tools and the Qt main thread. No race conditions, no deadlocks, no crashes. Just reliable, beautiful interfaces every time.

---

## Get MCP-Link (Free)

**This tool DEPENDS on MCP-Link server** — the free, desktop server that connects AI to your computer.

🎁 **Completely Free** • No subscriptions • No accounts • No credit cards  
🖥️ **One-Click Install** • Windows, Mac, Linux • Just download and run  
🔗 **Works With Everything** • ChatGPT, Claude, Cursor, VSCode, local models

Because webview controls require complex event and thread management, this tool requires the server environment within mcp-link to function - it's not possible to run it stand-alone (sorry!)

### Download Now

**Get MCP-Link from [aurafriday.com](https://aurafriday.com/)**

The User Interface tool (and dozens of other powerful tools) are included automatically. Install once, use everywhere.

---

## Real-World Story: The API Key Problem

**Before this tool existed:**

AI: "I need your OpenAI API key to continue."  
User: *copies key, pastes in chat*  
AI: "Thanks! Now I'll save it to... wait, I can't access your filesystem."  
User: *manually edits config file*  
AI: "Okay, try again."  
User: *restarts everything*

**With this tool:**

AI: "I need your OpenAI API key."  
*Beautiful dialog appears with password field, validation, and direct save to config*  
User: *enters key, clicks Save*  
AI: "Got it! Continuing..."

**One command. One dialog. Done.** The AI collected the key, validated it, saved it to the config file via the server's settings API, and continued — all without the user leaving their workflow.

---

## The Complete Feature Set

### Core Operations

#### `show_popup` — Non-Modal Windows
Display information without blocking other applications. Perfect for notifications, progress displays, or informational content that doesn't require immediate action.

#### `show_dialog` — Modal Dialogs
Block interaction until the user responds. Essential for critical decisions, required input, or confirmations that must be acknowledged.

#### `collect_api_key` — Pre-Built API Key Collection
Professional, ready-to-use interface for collecting API keys with:
- Password field with validation
- Direct integration with server settings API
- Automatic config file persistence
- Service URL links for key acquisition
- Beautiful, trustworthy design

#### `readme` — Self-Documenting
Every operation includes complete documentation with examples. AI agents can always access current, accurate information about how to use the tool.

### Advanced Window Management

**Positioning & Behavior:**
- `center_on_screen` — Automatically center on user's display
- `always_on_top` — Keep above all other windows (reliable on all platforms)
- `bring_to_front` — Force to foreground (best-effort on Windows due to OS restrictions)
- `modal` — Block interaction with other windows until closed
- `resizable` — Allow user to resize the window

**Smart Sizing:**
- `auto_resize` — Automatically fit content perfectly via JavaScript measurement
- Precise chrome calculation: +16px width, +39px height
- Two-stage resize to avoid scrollbar-induced layout cascades
- Intelligent default sizes based on content type

**Timing & Control:**
- `timeout` — Auto-close after specified seconds (0 = no timeout)
- `wait_for_response` — Fire-and-forget mode for notifications
- Graceful timeout handling with clear error messages

### JavaScript Bridge — Bidirectional Communication

Your HTML can send data back to the AI:

```javascript
// Success with data
window.userResponse = {
  "status": "success", 
  "data": {"api_key": "sk-1234...", "username": "john"}
};
window.close();

// Cancellation
window.userResponse = {"status": "cancelled", "message": "User declined"};
window.close();

// Error
window.userResponse = {"status": "error", "error": "Invalid input format"};
window.close();
```

**Why this matters:** The AI gets structured data back, not just "the user clicked something." It knows exactly what happened and can respond appropriately.

### Content Loading Options

**Inline HTML:**
```json
{
  "html": "<!DOCTYPE html><html>...</html>"
}
```

**URL Loading:**
```json
{
  "url": "https://example.com/form.html"
}
```

Load content from any URL — perfect for complex interfaces, external forms, or content that changes frequently.

### Security & Reliability

**HMAC Token System:**
- Unique per-installation, per-user, per-code-version
- Ensures AI fully understands tool usage before calling
- Prevents accidental misuse
- Inter-tool token format for tool-to-tool calls

**Thread-Safe Architecture:**
- Queue-based message passing between MCP thread and Qt main thread
- No race conditions or deadlocks
- Request-reply pattern with timeout protection
- Detailed logging for diagnostics

**Comprehensive Error Handling:**
- Parameter validation with clear error messages
- Type checking for all inputs
- Graceful degradation when Qt unavailable
- Full stack traces for debugging
- Automatic readme attachment on errors

### Platform Intelligence

**Windows-Specific Handling:**
- Acknowledges OS-level focus stealing prevention
- Recommends `always_on_top` over `bring_to_front`
- Provides platform-specific guidance in documentation

**Cross-Platform Consistency:**
- Identical rendering on Windows, Mac, Linux
- Same API across all platforms
- No platform-specific code required
- Qt WebEngine ensures modern web standards

---

## Window Sizing — The Hidden Complexity Made Simple

### The Problem

Qt WebEngine adds window chrome (title bar + borders) to your requested size:
- Actual width = requested width + 16px
- Actual height = requested height + 39px

Your HTML content gets the EXACT dimensions you request — the chrome is added on top.

**Sounds simple, right?** It's not. Underestimate by 20px and you get scrollbars. Scrollbars change layout. Changed layout needs different height. Infinite cascade of frustration.

### The Solution: Auto-Resize

Set `auto_resize: true` and the tool:
1. Opens window at 2× requested height (no scrollbars possible)
2. JavaScript measures actual content dimensions
3. Window shrinks to fit content + padding perfectly
4. Window re-centers automatically
5. No scrollbars, no guesswork, no frustration

**This is the kind of detail that separates "works sometimes" from "works always."**

### Sizing Best Practices

**If you don't use auto-resize:**
- Always overestimate rather than underestimate
- Add 50-80px buffer beyond estimated content height
- Account for: body padding + container padding + margins + content + buttons

**Common sizes that work well:**
- Simple forms: 400-500px height
- API key dialogs: 450-550px height
- Information displays: 350-450px height
- Complex forms: 600-800px height

**Rule of thumb:** If you think you need 300px, request 400px.

---

## HTML Templates — Copy, Paste, Customize

### Professional API Key Collection

```html
<!DOCTYPE html>
<html>
<head>
    <title>API Key Required</title>
    <style>
        body { 
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif; 
            padding: 30px; 
            background: #f5f5f5; 
            margin: 0; 
        }
        .container { 
            background: white; 
            padding: 30px; 
            border-radius: 8px; 
            box-shadow: 0 2px 10px rgba(0,0,0,0.1); 
            max-width: 400px; 
            margin: 0 auto; 
        }
        h2 { color: #333; margin-top: 0; }
        input[type="password"] { 
            width: 100%; 
            padding: 12px; 
            border: 1px solid #ddd; 
            border-radius: 4px; 
            font-size: 14px; 
            margin: 10px 0; 
        }
        button { 
            background: #007AFF; 
            color: white; 
            border: none; 
            padding: 12px 24px; 
            border-radius: 4px; 
            cursor: pointer; 
            margin-right: 10px; 
        }
        button:hover { background: #0056CC; }
        .cancel { background: #666; }
        .cancel:hover { background: #333; }
    </style>
</head>
<body>
    <div class="container">
        <h2>🔑 OpenAI API Key Required</h2>
        <p>Please enter your OpenAI API key to continue:</p>
        <input type="password" id="apiKey" placeholder="sk-..." autofocus>
        <div style="margin-top: 20px;">
            <button onclick="submit()">Submit</button>
            <button class="cancel" onclick="cancel()">Cancel</button>
        </div>
    </div>
    
    <script>
        function submit() {
            const key = document.getElementById('apiKey').value.trim();
            if (!key) return alert('Please enter an API key');
            if (!key.startsWith('sk-')) return alert('Invalid API key format');
            
            window.userResponse = {"status": "success", "data": {"api_key": key}};
            window.close();
        }
        
        function cancel() {
            window.userResponse = {"status": "cancelled"};
            window.close();
        }
        
        document.getElementById('apiKey').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') submit();
        });
    </script>
</body>
</html>
```

### Simple Confirmation Dialog

```html
<!DOCTYPE html>
<html>
<head>
    <title>Confirmation</title>
    <style>
        body { font-family: system-ui; padding: 20px; text-align: center; }
        .icon { font-size: 48px; margin-bottom: 20px; }
        button { 
            padding: 10px 20px; 
            margin: 0 5px; 
            border: none; 
            border-radius: 4px; 
            cursor: pointer; 
        }
        .yes { background: #28a745; color: white; }
        .no { background: #dc3545; color: white; }
    </style>
</head>
<body>
    <div class="icon">⚠</div>
    <h2>Delete all files?</h2>
    <p>This action cannot be undone.</p>
    <button class="yes" onclick="respond(true)">Yes, Delete</button>
    <button class="no" onclick="respond(false)">Cancel</button>
    
    <script>
        function respond(confirmed) {
            window.userResponse = {"status": "success", "data": {"confirmed": confirmed}};
            window.close();
        }
    </script>
</body>
</html>
```

### Multi-Step Wizard

```html
<!DOCTYPE html>
<html>
<head>
    <title>Setup Wizard</title>
    <style>
        body { font-family: system-ui; padding: 20px; background: #f5f5f5; }
        .wizard { background: white; padding: 30px; border-radius: 8px; max-width: 500px; margin: 0 auto; }
        .step { display: none; }
        .step.active { display: block; }
        .progress { display: flex; justify-content: space-between; margin-bottom: 30px; }
        .progress-dot { width: 30px; height: 30px; border-radius: 50%; background: #ddd; 
                       display: flex; align-items: center; justify-content: center; }
        .progress-dot.active { background: #007AFF; color: white; }
        .progress-dot.completed { background: #28a745; color: white; }
        button { padding: 10px 20px; border: none; border-radius: 4px; cursor: pointer; margin: 5px; }
        .primary { background: #007AFF; color: white; }
        .secondary { background: #666; color: white; }
    </style>
</head>
<body>
    <div class="wizard">
        <div class="progress">
            <div class="progress-dot active" id="dot1">1</div>
            <div class="progress-dot" id="dot2">2</div>
            <div class="progress-dot" id="dot3">3</div>
        </div>
        
        <div class="step active" id="step1">
            <h2>Step 1: Basic Information</h2>
            <input type="text" id="name" placeholder="Your name" style="width: 100%; padding: 10px; margin: 10px 0;">
            <button class="primary" onclick="nextStep(2)">Next</button>
        </div>
        
        <div class="step" id="step2">
            <h2>Step 2: Preferences</h2>
            <label><input type="checkbox" id="pref1"> Enable notifications</label><br>
            <label><input type="checkbox" id="pref2"> Auto-save</label><br>
            <button class="secondary" onclick="prevStep(1)">Back</button>
            <button class="primary" onclick="nextStep(3)">Next</button>
        </div>
        
        <div class="step" id="step3">
            <h2>Step 3: Confirm</h2>
            <p>Ready to complete setup?</p>
            <button class="secondary" onclick="prevStep(2)">Back</button>
            <button class="primary" onclick="finish()">Finish</button>
        </div>
    </div>
    
    <script>
        function nextStep(num) {
            document.querySelectorAll('.step').forEach(s => s.classList.remove('active'));
            document.getElementById('step' + num).classList.add('active');
            document.getElementById('dot' + (num-1)).classList.add('completed');
            document.getElementById('dot' + num).classList.add('active');
        }
        
        function prevStep(num) {
            document.querySelectorAll('.step').forEach(s => s.classList.remove('active'));
            document.getElementById('step' + num).classList.add('active');
            document.getElementById('dot' + (num+1)).classList.remove('active');
            document.getElementById('dot' + num).classList.remove('completed');
        }
        
        function finish() {
            window.userResponse = {
                "status": "success",
                "data": {
                    "name": document.getElementById('name').value,
                    "notifications": document.getElementById('pref1').checked,
                    "autosave": document.getElementById('pref2').checked
                }
            };
            window.close();
        }
    </script>
</body>
</html>
```

---

## Usage Examples

### Show API Key Collection Dialog

```json
{
  "input": {
    "operation": "show_dialog",
    "html": "<!DOCTYPE html><html>...[full HTML]...</html>",
    "title": "API Key Required",
    "width": 500,
    "height": 450,
    "modal": true,
    "timeout": 120,
    "tool_unlock_token": "YOUR_TOKEN_HERE"
  }
}
```

### Use Pre-Built API Key Collector

```json
{
  "input": {
    "operation": "collect_api_key",
    "service_name": "OpenAI",
    "service_url": "https://platform.openai.com/api-keys",
    "tool_unlock_token": "YOUR_TOKEN_HERE"
  }
}
```

### Show Non-Blocking Notification

```json
{
  "input": {
    "operation": "show_popup",
    "html": "<!DOCTYPE html><html><body><h2>✅ Success!</h2><p>Your settings have been saved.</p><script>setTimeout(() => window.close(), 3000);</script></body></html>",
    "title": "Success",
    "width": 400,
    "height": 200,
    "modal": false,
    "wait_for_response": false,
    "tool_unlock_token": "YOUR_TOKEN_HERE"
  }
}
```

### Load Content from URL

```json
{
  "input": {
    "operation": "show_dialog",
    "url": "https://example.com/form.html",
    "title": "External Form",
    "width": 800,
    "height": 600,
    "tool_unlock_token": "YOUR_TOKEN_HERE"
  }
}
```

---

## Return Values

### Success Response

```json
{
  "status": "success",
  "data": {
    "api_key": "sk-1234...",
    "username": "john",
    "preferences": {"theme": "dark"}
  },
  "window_closed": true
}
```

### Cancellation Response

```json
{
  "status": "cancelled",
  "message": "User cancelled the dialog"
}
```

### Timeout Response

```json
{
  "status": "timeout",
  "error": "UI request timed out after 125 seconds"
}
```

### Async Mode Response (wait_for_response: false)

```json
{
  "status": "success",
  "message": "Window opened successfully (async mode - not waiting for user response)",
  "async": true
}
```

---

## Technical Architecture

### Thread-Safe Communication

**The Challenge:** MCP tools run in worker threads. Qt UI must run in the main thread. How do you communicate safely?

**The Solution:** Queue-based message passing with request-reply pattern.

1. Tool creates a `UIRequest` with operation data and a reply queue
2. Tool sends request to friday.py's main thread via `sys.modules['friday_ui_queue']`
3. Qt main thread polls queue, processes request, creates window
4. User interacts with window
5. Window closes, response sent back via reply queue
6. Tool receives response, returns to AI

**Why this works:**
- No shared state between threads
- No locks or mutexes needed
- Timeout protection prevents hung requests
- Clean separation between MCP and Qt layers

### Security Token System

**HMAC-based tokens ensure proper usage:**

```python
TOOL_UNLOCK_TOKEN = get_tool_token(__file__)
# Generates unique token from: file path + user + code version
```

**Token validation:**
- Direct match: AI has read the documentation
- Inter-tool format: `-{calling_tool_token}-{target_tool_token}`
- Invalid/missing: Return full documentation

**Why this matters:** Prevents AI from calling tools without understanding parameters, reducing errors and improving reliability.

### Auto-Resize Algorithm

**The two-stage resize eliminates scrollbar cascades:**

```
Stage 1: Open at 2× requested height
  → No scrollbars possible
  → JavaScript measures content height
  → Sends height to Python via bridge

Stage 2: Resize to measured height + padding
  → Window shrinks to perfect size
  → Re-center on screen
  → No scrollbars, no layout changes
```

**Why two stages?** Opening at exact size might cause scrollbars. Scrollbars change layout. Changed layout needs different size. Two-stage approach breaks this cycle.

---

## Common Use Cases

### Collecting Sensitive Information
API keys, passwords, tokens — anything that shouldn't be pasted in a chat window.

### User Preferences & Configuration
Theme selection, feature toggles, advanced settings — beautiful forms instead of command-line flags.

### Confirmation Dialogs
"Are you sure?" moments that need explicit user acknowledgment before destructive actions.

### Multi-Step Wizards
Complex setup processes broken into manageable steps with progress indication.

### Progress Displays
Rich, formatted progress updates with charts, graphs, or detailed status information.

### Data Entry Forms
Structured input with validation, dropdowns, checkboxes, and proper error messages.

### Error Displays with Actions
Not just "something went wrong" — show what happened, why, and what the user can do about it.

### File Selection & Options
Custom file pickers with preview, filtering, and metadata display.

---

## Best Practices

### HTML Structure
1. Always include complete HTML with DOCTYPE, head, and body
2. Use modern CSS for beautiful, responsive layouts
3. Include proper error handling in JavaScript
4. Use semantic HTML and proper form validation
5. Test your HTML in a browser first for complex interfaces

### Window Sizing
1. Add 50-80px buffer to height estimates to avoid scrollbars
2. Use `auto_resize: true` for perfect sizing without guesswork
3. Account for all spacing: body padding + container padding + margins + content + buttons
4. Test on all platforms if possible (Windows, Mac, Linux)

### User Experience
1. Set reasonable timeouts for modal dialogs (120-300 seconds)
2. Use `always_on_top: true` for reliable visibility on Windows
3. Provide clear cancel options in all dialogs
4. Include keyboard shortcuts (Enter to submit, Escape to cancel)
5. Show loading states for async operations
6. Validate input before closing window

### Error Handling
1. Validate all user input in JavaScript before sending to Python
2. Provide clear error messages with actionable guidance
3. Use try-catch blocks around all operations
4. Set `window.userResponse` before calling `window.close()`
5. Handle timeout scenarios gracefully

### Security
1. Never expose sensitive data in window titles or logs
2. Use password input types for secrets
3. Validate API keys before saving
4. Clear sensitive data from memory after use
5. Use HTTPS for URL loading when possible

---

## Troubleshooting

### Window Doesn't Appear

**Possible causes:**
- Qt not available (check friday.py is running with Qt support)
- UI request queue not accessible (ensure server started via friday.py)
- Window opened behind other windows (use `always_on_top: true`)

**Solutions:**
1. Check logs for "No UI request queue available" error
2. Verify Qt WebEngine is installed
3. Try `test_queue` operation to diagnose communication issues

### Window Has Scrollbars

**Possible causes:**
- Height underestimated
- Forgot to account for padding/margins
- Content larger than expected

**Solutions:**
1. Add 50-80px to height estimate
2. Use `auto_resize: true` for automatic sizing
3. Check body padding, container padding, margins

### Timeout Errors

**Possible causes:**
- User took longer than timeout allows
- Window blocked by other windows (user didn't see it)
- Network delay loading URL content

**Solutions:**
1. Increase timeout value
2. Use `always_on_top: true` for visibility
3. Use `wait_for_response: false` for non-critical operations

### JavaScript Bridge Not Working

**Possible causes:**
- Forgot to set `window.userResponse` before `window.close()`
- Response format incorrect (must be object with "status" key)
- JavaScript error preventing execution

**Solutions:**
1. Check browser console for JavaScript errors
2. Verify response format matches expected structure
3. Test HTML in regular browser first

---

## Powered by MCP-Link

This tool is part of the [MCP-Link Server](https://github.com/AuraFriday/mcp-link-server) — a battle-tested, production-ready MCP server that **just works** on every platform.

### Why MCP-Link?

**Isolated Python Environment:**
- Doesn't touch your system Python
- Includes all dependencies (Qt, PySide2, etc.)
- Works on old and new systems
- Zero configuration required

**Battle-Tested Infrastructure:**
- Thread-safe queue communication
- Comprehensive error handling
- Detailed logging for diagnostics
- Graceful degradation when components unavailable

**Cross-Platform Excellence:**
- Windows, Mac, Linux support
- Consistent behavior across platforms
- Platform-specific optimizations
- Native look and feel

### Get MCP-Link

Download the installer for your platform:
- [Windows](https://github.com/AuraFriday/mcp-link-server/releases/latest)
- [Mac (Apple Silicon)](https://github.com/AuraFriday/mcp-link-server/releases/latest)
- [Mac (Intel)](https://github.com/AuraFriday/mcp-link-server/releases/latest)
- [Linux](https://github.com/AuraFriday/mcp-link-server/releases/latest)

**Installation is automatic. Configuration is automatic. It just works.**

---

## Technical Specifications

**Language:** Python 3.x  
**Dependencies:** Qt WebEngine (PySide2), included in MCP-Link  
**Thread Safety:** Yes (queue-based message passing)  
**Platform Support:** Windows, macOS, Linux  
**Security:** HMAC token validation, parameter type checking  
**Error Handling:** Comprehensive with stack traces  
**Logging:** Detailed via MCPLogger  
**Documentation:** Self-documenting via `readme` operation  

**Version:** 2025.09.13.009  
**Architecture:** No direct Qt imports (sys.modules registry)  
**Communication:** Native Python queue.Queue  
**Token System:** Per-installation, per-user, per-code-version  

---

## License & Copyright

Copyright © 2025 Christopher Nathan Drake

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at:

    https://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

AI Training Permission: You are permitted to use this software and any
associated content for the training, evaluation, fine-tuning, or improvement
of artificial intelligence systems, including commercial models.

SPDX-License-Identifier: Apache-2.0

Part of the Aura Friday MCP-Link Server project.

---

## Support & Community

**Issues & Feature Requests:**  
[GitHub Issues](https://github.com/AuraFriday/mcp-link/issues)

**Documentation:**  
[MCP-Link Documentation](https://aurafriday.com/)

**Community:**  
Join other developers building amazing AI-powered tools with MCP-Link.


