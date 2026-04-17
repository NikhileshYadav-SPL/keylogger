# keylogger




[![Educational](https://img.shields.io/badge/Purpose-Educational_Only-yellow?style=flat)]()

> Educational keylogger for security research demonstrating input capture, window tracking, and C2 delivery techniques.

*This is a quick overview — security theory, architecture, and full walkthroughs are in the [learn modules](#learn).*

## What It Does

- Real-time keyboard event capture with microsecond-precision timestamps
- Active window tracking across Windows, macOS, and Linux
- Log rotation with configurable size limits (default 5MB)
- F9 toggle control to pause and resume capture at runtime
- Remote delivery simulation via webhooks for C2 research
- Thread-safe operations with proper resource locking and cleanup

## Quick Start

```bash
git clone https://github.com/NikhileshYadav-SPL/keylogger.git
cd NikhileshYadav-SPL/keylogger
uv sync
python keylogger.py
```

Press **F9** to toggle capture on/off. Press **Ctrl+C** to stop.

**Disclaimer:** For authorized security research and education only. Unauthorized use of keyloggers is illegal. Always obtain explicit written consent before monitoring any system you do not own.

> [!TIP]
> This project uses [`just`](https://github.com/casey/just) as a command runner. Type `just` to see all available commands.
>
> Install: `curl -sSf https://just.systems/install.sh | bash -s -- --to ~/.local/bin`

## Learn

This project includes step-by-step learning materials covering security theory, architecture, and implementation. 

| Module | Topic |
|--------|-------|
| [00 - Overview](learn/00-OVERVIEW.md) | Prerequisites and quick start |
| [01 - Concepts](learn/01-CONCEPTS.md) | Security theory and real-world breaches |
| [02 - Architecture](learn/02-ARCHITECTURE.md) | System design and data flow |
| [03 - Implementation](learn/03-IMPLEMENTATION.md) | Code walkthrough |
| [04 - Challenges](learn/04-CHALLENGES.md) | Extension ideas and exercises |


## License

AGPL 3.0
