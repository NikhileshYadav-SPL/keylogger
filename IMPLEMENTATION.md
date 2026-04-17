keylogger/
├── keylogger.py          # 550 lines, complete implementation
│   ├── Imports (1-31)    # Dependencies and platform detection
│   ├── Platform constants + conditional imports (32-48)
│   ├── Module-level constants (49-52)
│   ├── SPECIAL_KEYS dict (75-95)
│   ├── KeyType enum (98-104)
│   ├── KeyloggerConfig (107-120)   # Pure data container, no side effects
│   ├── KeyEvent (123-152)        # Keystroke record
│   ├── WindowTracker (155-219)   # Platform window detection
│   ├── LogManager (222-296)      # Direct file I/O with rotation
│   ├── WebhookDelivery (298-371) # Remote exfiltration (buffer swap)
│   └── Keylogger (373-533)       # Main controller
├── test_keylogger.py     # 598 lines, 46 pytest tests
└── pyproject.toml        # Dependencies and tool config
