┌──────────────────────────────────────────────────────────┐
│                    Operating System                      │
│                  (Keyboard Event Stream)                 │
└────────────────────────┬─────────────────────────────────┘
                         │
                         ▼
              ┌──────────────────────┐
              │  pynput Listener     │
              │  (Event Callbacks)   │
              └──────────┬───────────┘
                         │ 
                         ▼
              ┌──────────────────────┐
              │     Keylogger        │
              │   (Main Controller)  │
              └─────┬────────┬───────┘
                    │        │
        ┌───────────┘        └──────────┐
        ▼                               ▼
┌───────────────┐              ┌────────────────┐
│ WindowTracker │              │  LogManager    │
│  (Platform-   │              │ (File Writing) │
│   Specific)   │              └────────┬───────┘
└───────────────┘                       │
                                        ▼
                              ┌──────────────────┐
                              │ WebhookDelivery  │
                              │  (Exfiltration)  │
                              └──────────────────┘

                              
Keylogger (Main Controller)

Purpose: Orchestrates all components and handles the event processing pipeline
Responsibilities: Receives keyboard events from pynput, processes keys, coordinates window tracking, delegates to logging and webhook delivery
Interfaces: Exposes start() and stop() methods for lifecycle management, registers _on_press() callback with pynput listener
Location: keylogger.py:373-533
LogManager

Purpose: Manages persistent storage of keystroke events with automatic file rotation using direct file I/O
Responsibilities: Creates timestamped log files, writes events to disk via raw file handles, monitors file size and rotates when limit reached, provides thread-safe access via locks, handles explicit file handle cleanup via close()
Interfaces: write_event(event) for logging, get_current_log_content() for reading back logs, close() for releasing file handles
Location: keylogger.py:222-296
WebhookDelivery

Purpose: Handles remote exfiltration of captured keystrokes via HTTP webhooks
Responsibilities: Buffers events to reduce network traffic, batches events before sending, uses a buffer swap pattern to deliver outside the lock, delivers JSON payloads to configured endpoint, handles delivery failures gracefully
Interfaces: add_event(event) for queuing, flush() for forcing immediate delivery
Location: keylogger.py:298-371
WindowTracker

Purpose: Determines which application has focus when keystrokes occur
Responsibilities: Platform detection (Windows/macOS/Linux), calls platform-specific APIs to get active window title, provides unified interface across platforms
Interfaces: Static method get_active_window() returns current window title or None
Location: keylogger.py:155-219
KeyEvent (Data Model)

Purpose: Immutable representation of a single keystroke with metadata
Responsibilities: Stores timestamp, key value, window context, and key type classification
Interfaces: to_dict() for JSON serialization, to_log_string() for human-readable formatting
Location: keylogger.py:123-152
Data Flow
Primary Use Case: Keystroke Capture and Logging
Step by step walkthrough of what happens when a user presses a key:

1. OS Keyboard Event → pynput Listener
   User presses 'a' key, OS delivers event to all registered listeners
   pynput captures event and triggers our callback

2. Listener → Keylogger._on_press() (keylogger.py:428)
   Callback receives Key or KeyCode object
   Checks if it's the toggle key (dynamically reads config toggle key) → pause/resume if so
   Checks if logging is active → early return if paused

3. Keylogger → WindowTracker.get_active_window() (keylogger.py:390)
   Calls platform-specific code to get active window
   Caches result for config.window_check_interval seconds (default 0.5) to avoid excessive API calls
   Returns window title like "Chrome - Gmail" or None

4. Keylogger → _process_key() (keylogger.py:406)
   Converts Key/KeyCode to string representation
   Looks up special keys via module-level SPECIAL_KEYS constant
   Maps special keys (Enter→"[ENTER]", Space→"[SPACE]")
   Classifies key type (CHAR, SPECIAL, UNKNOWN)

5. Keylogger → Creates KeyEvent (keylogger.py:450)
   Bundles timestamp, key string, window title, and key type
   Creates dataclass instance

6. Keylogger → LogManager.write_event() (keylogger.py:248)
   Acquires lock for thread safety
   Formats event to log string: "[2025-01-31 14:30:22][Chrome] a"
   Writes to current log file via direct file I/O (self._file.write + flush)
   Checks file size and rotates if needed

7. Keylogger → WebhookDelivery.add_event() (keylogger.py:308)
   Adds event to buffer array under lock
   Checks if buffer reached batch size (default 50)
   If full, swaps the buffer (replaces with empty list) under lock
   Delivers the batch OUTSIDE the lock via HTTP POST

                              
