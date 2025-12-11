
# PyRemote

Control your computer from Telegram.  
Cross-platform remote administration and automation tool for Windows, macOS, and Linux — built in Python.

> ⚠️ Use only on machines you own or on systems where you have explicit authorization. This tool can access files, camera, clipboard, networking details, and execute commands. Misuse is illegal.

---

## Table of contents

- [What is this](#what-is-this)  
- [Key features](#key-features)  
- [Supported platforms](#supported-platforms)  
- [Quick start](#quick-start)  
- [Commands](#commands)  
- [Architecture and how it works](#architecture-and-how-it-works)  
- [Security and legal notes](#security-and-legal-notes)  
- [Packaging and distribution](#packaging-and-distribution)  
- [Troubleshooting](#troubleshooting)  
- [Contact](#contact)  

---

## What is this

`PyRemote` is a Telegram bot that runs on a target machine and exposes a set of remote administration operations over Telegram. It is intended for legitimate remote administration, personal automation, and learning. The project demonstrates cross-platform handling of system-level tasks from Python.

---

## Key features

- Remote screenshot capture  
- Webcam capture (if available)  
- System information and public IP lookup  
- File listing, change directory, and file upload to Telegram  
- Remote shell session for command execution  
- Play TTS audio on host machine  
- Clipboard readout  
- Lock screen and shutdown (per OS capability)  
- Wi-Fi profile inspection (OS-dependent)  
- Folder encryption and decryption using AES, with secure delete fallback

---

## Supported platforms

- Windows 10+  
- macOS (recent versions)  
- Linux distributions with standard tooling (Ubuntu, Debian, Fedora, etc.)

Note: some features require extra packages or privileges:
- Camera access: OS and device drivers must allow camera use.  
- Wi-Fi passwords: typically require admin/sudo.  
- Shutdown, lock: may require privileges or available desktop commands (systemd, loginctl, etc.).  
- Clipboard on Linux may require `xclip` or `xsel`.

---

## Quick start

1. Clone the repo and pick a name:
   ```bash
   git clone https://github.com/ajit421/PyRemote.git
   cd PyRemote


2. Create a virtual environment and install:

   ```bash
   python -m venv venv
   source venv/bin/activate   # Linux / macOS
   venv\Scripts\activate      # Windows
   pip install -r requirements.txt
   ```

3. Edit `main.py`:

   * Set `TOKEN` to your Telegram bot token.
   * Set `BOT_PASSWORD` to a strong password you will use for `/auth`.

4. Run:

   ```bash
   python main.py
   ```

5. In Telegram, send:

   ```
   /auth YOUR_PASSWORD
   /start
   ```

---

## Commands

* `/auth [password]` - authenticate your Telegram user id.
* `/start` - show available commands.
* `/screen` - take screenshot and send image.
* `/webcam` - capture webcam image and send.
* `/sys` - system information.
* `/ip` - public IP address.
* `/ls` - list files in current directory.
* `/cd [folder]` - change working directory.
* `/upload [path]` - upload a file from host.
* `/clipboard` - get clipboard content.
* `/speech [text]` - play text-to-speech on host machine.
* `/lock` - lock the host session (if supported).
* `/shutdown` - shutdown host (if supported).
* `/wifi` - list Wi-Fi profiles or best-effort info (OS-dependent).
* `/shell` - enter remote shell mode. Type `exit` to quit shell mode.

---

## Architecture and how it works

* A single Python process runs `main.py` and connects to the Telegram Bot API using `pyTelegramBotAPI`.
* Commands are processed via message handlers.
* Cross-platform branching uses `platform.system()` to run OS-appropriate logic.
* Screenshot and webcam use cross-platform libraries `mss` and `opencv-python`.
* TTS uses `gTTS` to generate mp3 and then opens it with the platform default player.
* Encryption uses `pyAesCrypt`. Secure file deletion uses an overwrite fallback when native secure-delete packages are unavailable.

---

## Security and legal notes

* Do not run this on third-party systems without explicit written consent.
* Running persistent remote tools may trigger antivirus alerts. Sign and verify binaries in production.
* Use strong BOT_PASSWORD. Only authenticate trusted Telegram accounts.
* Consider network security: do not expose your bot token or device to public networks without protection.
* Logs and sent files may contain sensitive information. Treat them carefully.

---

## Packaging and distribution

* For sharing executables, use PyInstaller per target platform. Build the Windows EXE on Windows, macOS bundle on macOS, and Linux binary on Linux.
* Use GitHub Releases to upload built artifacts.
* Provide an installation script for each OS in `contrib/` or `scripts/`.


---

## Troubleshooting

* `Camera not found` - verify `/dev/video0` on Linux or camera privacy settings on macOS and Windows.
* `Clipboard empty or unsupported` on Linux - install `xclip` or `xsel` and enable X server clipboard access.
* `Wi-Fi profiles not returned` - profile access usually needs admin or root privileges.
* Antivirus flags - building and running locally during development is recommended. Add proper code signing for distribution.


---

## Contact

For questions or help, open an Issue in this repository or contact the author.