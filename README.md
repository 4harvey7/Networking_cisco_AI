# Networking_cisco_AI
Networking-only AI tutor powered by Google Gemini. Loads Cisco Packet Tracer (.pkt) labs, explains the topology, and checks device configs for mistakes.
# 🌐 Networking AI Assistant

**An AI tutor for computer networking that can also read your Cisco Packet Tracer labs.**

Load your `.pkt` file, then ask things like *"Explain this topology"* or *"What's wrong with my router config?"* It runs on your own computer and uses Google Gemini for the AI.

> **New to this?** No problem. Every step below is explained, and there is a [glossary](#-glossary) for unfamiliar words.

---

## 📑 Contents

1. [What is this?](#-what-is-this)
2. [Quick Start (5 minutes)](#-quick-start-5-minutes)
3. [Full step-by-step guide](#-full-step-by-step-guide)
4. [How to use it](#-how-to-use-it)
5. [Troubleshooting](#-troubleshooting)
6. [FAQ](#-faq)
7. [Glossary](#-glossary)
8. [Limits and responsible use](#-limits-and-responsible-use)
9. [Customizing](#-customizing)
10. [Credits](#-credits)

---

## 🤔 What is this?

- A **chat program** you run in a terminal window.
- It **only answers networking questions** (TCP/IP, OSI, subnetting, DNS, DHCP, routing, VLANs, Wi-Fi, firewalls, troubleshooting, and more). Anything else gets a polite refusal.
- It can **read a Packet Tracer lab file** and explain the devices, spot configuration mistakes, and answer questions about your setup.
- It is **read-only**: it never changes your lab file.

### What a session looks like

```text
Networking AI is ready!
Type 'load lab.pkt' to load a Packet Tracer file.
Type 'exit' to quit.

You: load lab.pkt
AI: Lab loaded (4821 characters). Now ask me about it.

You: Explain this lab and check for configuration mistakes.
AI: (explains the topology and points out any problems)

You: What is the capital of France?
AI: I can only answer computer networking questions. Please ask me something about networking.

You: exit
AI: Goodbye!
```

---

## ⚡ Quick Start (5 minutes)

**Already comfortable with Python and the terminal?** Do this. Otherwise skip to the [full guide](#-full-step-by-step-guide).

1. Install **Python 3.10+** (tick *"Add python.exe to PATH"* on Windows).
2. Create an API key at [aistudio.google.com/apikey](https://aistudio.google.com/apikey).
3. Install the packages:
   ```powershell
   pip install google-genai securedypkt
   ```
4. Save your key as an environment variable, then **restart your terminal and VS Code**:
   ```powershell
   setx GEMINI_API_KEY "paste-your-key-here"
   ```
5. Put `networkingai.py` and your `lab.pkt` in one folder, then run:
   ```powershell
   python networkingai.py
   ```
   Type `load lab.pkt`, then ask your question.

---

## 📘 Full step-by-step guide

### ✅ Before you start: checklist

- [ ] A Windows PC (Mac/Linux notes are included where they differ)
- [ ] An internet connection
- [ ] A Google account (for the free API key)
- [ ] A Packet Tracer `.pkt` file *(optional, only for the lab-reading feature)*
- [ ] About 10 minutes

You do **not** need Docker, n8n, Visual Studio, or a C++ compiler.

---

### Step 1: Install Python

1. Go to [python.org/downloads](https://www.python.org/downloads/) and download Python (3.10 or newer).
2. Run the installer. On the **first screen**, tick **"Add python.exe to PATH"**, then click *Install Now*.
3. Open a terminal (see the tip below) and check it worked:

```powershell
python --version
```

✅ **You should see:** something like `Python 3.12.4`.

> 💡 **How do I open a terminal?**
> - **Windows:** press the Windows key, type `PowerShell`, press Enter.
> - **Inside VS Code:** press `` Ctrl+` `` (the key under Esc). It opens in your project folder automatically.
> - **Mac:** open the *Terminal* app. Use `python3` and `pip3` instead of `python` and `pip`.

---

### Step 2: Create a project folder

```powershell
mkdir C:\Projects\networking
cd C:\Projects\networking
```

Put `networkingai.py` (the main script) in this folder.

✅ **You should see:** your terminal prompt now ends with `...\networking>`.

---

### Step 3: Install the required packages

```powershell
pip install google-genai securedypkt
```

| Package | What it does |
|---|---|
| `google-genai` | Lets the script talk to Google Gemini |
| `securedypkt` | Decodes Packet Tracer files (no compiler needed) |

✅ **You should see:** `Successfully installed ...` at the end.

> ℹ️ A message like *"Defaulting to user installation"* is normal and harmless.

---

### Step 4: Get your free Gemini API key

An **API key** is like a password that lets the script use Google's AI.

1. Go to [aistudio.google.com/apikey](https://aistudio.google.com/apikey) and sign in.
2. Click **Create API key**.
3. Copy the key.

> 🔒 **Keep it secret.** Never paste your key into code, screenshots, chats, or a public GitHub repo. If it leaks, delete it on the same page and create a new one.

---

### Step 5: Save the key on your computer

This lets the script find your key without it being written in the code.

**Windows (PowerShell):**

```powershell
setx GEMINI_API_KEY "paste-your-key-here"
```

**Mac / Linux:**

```bash
echo 'export GEMINI_API_KEY="paste-your-key-here"' >> ~/.zshrc
```

⚠️ **Now close and reopen your terminal and VS Code completely.** New settings are only picked up by new windows. Skipping this is the number one cause of the "No API key" error.

Check it worked, in a **new** terminal:

```powershell
echo $env:GEMINI_API_KEY
```

✅ **You should see:** your key printed. (Don't share this output.)

<details>
<summary><b>Just want to test quickly? (temporary option)</b></summary>

This sets the key for the current terminal only. It disappears when you close it.

```powershell
$env:GEMINI_API_KEY = "paste-your-key-here"
```
</details>

---

### Step 6: Add your Packet Tracer file

Copy your `.pkt` file into the project folder and rename it `lab.pkt` to keep commands short:

```powershell
copy "C:\Users\YourName\Downloads\MyLab.pkt" C:\Projects\networking\lab.pkt
```

(Replace `YourName` and `MyLab.pkt` with your real folder and file name.)

Your folder should look like this:

```text
networking/
├── networkingai.py
└── lab.pkt
```

> 💡 You can also skip the copying and load a file from anywhere using its full path later.

---

### Step 7: Run it

```powershell
cd C:\Projects\networking
python networkingai.py
```

✅ **You should see:**

```text
Networking AI is ready!
Type 'load lab.pkt' to load a Packet Tracer file.
Type 'exit' to quit.
```

🎉 You're set. Continue to [How to use it](#-how-to-use-it).

---

## 💬 How to use it

### Commands

| Type this | What happens |
|---|---|
| Any networking question | The AI answers it |
| `load lab.pkt` | Decodes your lab and loads its device configs |
| `load C:\path\to\file.pkt` | Same, using a full path (any folder works) |
| A question after loading | The AI answers using your lab's configs |
| `exit` | Quits the program |

### Good questions to ask about your lab

- *Explain this lab and check for configuration mistakes.*
- *What is the role of each device?*
- *Are the VLANs configured correctly?*
- *Why might PC0 not be able to reach the router?*
- *Is my OSPF configuration correct?*
- *What is missing from this configuration?*

### Good general questions (no lab needed)

- *What is the difference between a hub, a switch, and a router?*
- *How do I subnet 192.168.1.0/24 into 4 networks?*
- *Explain how DHCP works.*

### Tips

- **Changed your lab in Packet Tracer?** Save it, then type `load lab.pkt` again to refresh.
- **Ask one thing at a time** for clearer answers.
- **Paste error messages or CLI output** into your question if you are troubleshooting.
- The AI is told to keep answers beginner-friendly. Ask *"explain more simply"* or *"go deeper"* any time.

### How it works (optional reading)

```text
lab.pkt
   │  securedypkt decrypts and decompresses it
   ▼
XML  (also saved as lab_decoded.xml so you can look at it)
   │  the script pulls out each device's name and running-config
   ▼
Plain-text configs
   │  sent to Gemini together with your question and the networking-only rules
   ▼
Answer in your terminal
```

---

## 🛠️ Troubleshooting

Find the message you see and click it open.

<details>
<summary><b>"No API key was provided" (ValueError)</b></summary>

The script cannot see your key.

1. Did you **close and reopen** VS Code and the terminal after running `setx`? Do that first.
2. Run `echo $env:GEMINI_API_KEY`. If it prints nothing, repeat [Step 5](#step-5-save-the-key-on-your-computer).
3. Make sure the script contains `os.environ.get("GEMINI_API_KEY")` with the variable **name**, not your actual key.
</details>

<details>
<summary><b>"can't open file ... networkingai.py: No such file or directory"</b></summary>

You are in the wrong folder. Run:

```powershell
cd C:\Projects\networking
python networkingai.py
```
</details>

<details>
<summary><b>"FileNotFoundError ... 'lab.pkt'"</b></summary>

The file is not where the script is looking.

- Run `dir` to see what is really in the folder.
- Check the real file name. Turn on *View → File name extensions* in File Explorer, because a file named `lab.pkt.pkt` causes this.
- Or use the full path: `load C:\path\to\YourFile.pkt`
</details>

<details>
<summary><b>"I can only answer computer networking questions" when I type <code>load lab.pkt</code></b></summary>

You are running an **old version** of `networkingai.py` that does not know the `load` command. The correct version prints *"Type 'load lab.pkt' to load a Packet Tracer file"* when it starts. Replace the file contents with the latest version and save it.
</details>

<details>
<summary><b>"Decryption error" or "authentication tag mismatch"</b></summary>

The decoder may not support your Packet Tracer version, or the file may not be a valid `.pkt`.

**Workaround:** in Packet Tracer, click a device, open the **CLI** tab, run `show running-config`, and paste the output into the chat as text.
</details>

<details>
<summary><b>"Could not find device configs, sending raw XML instead"</b></summary>

The file decoded, but the configs were not in the expected place. It still works, but answers may be less accurate. Open `lab_decoded.xml`, search for `RUNNINGCONFIG`, and check how the config lines are stored.
</details>

<details>
<summary><b>Error 429 / "Too Many Requests"</b></summary>

You hit Gemini's rate limit, which is common on the free tier. Wait a minute and try again, or send fewer questions in a short time.
</details>

<details>
<summary><b>"python is not recognized"</b></summary>

Python was not added to PATH. Reinstall it and tick **"Add python.exe to PATH"**, or try `py networkingai.py`. On Mac/Linux use `python3`.
</details>

<details>
<summary><b><code>pip install twofish</code> fails with "Microsoft Visual C++ 14.0 or greater is required"</b></summary>

You do not need `twofish`. This project uses `securedypkt`, which needs no compiler. Just run `pip install securedypkt`.
</details>

<details>
<summary><b>"unpacket is not recognized"</b></summary>

Ignore it. The script uses the Python library directly and does not need that command.
</details>

<details>
<summary><b>VS Code shows yellow warnings like <code>"print" is not defined</code></b></summary>

These are false alarms from the editor. Press `Ctrl+Shift+P`, choose **Python: Select Interpreter**, pick your installed Python, then run **Developer: Reload Window**. The program still runs fine.
</details>

---

## ❓ FAQ

**Is it free?**
Python and the packages are free. Google currently offers a free Gemini tier with usage limits. Check [Google AI Studio](https://aistudio.google.com/) for the current limits and terms.

**Does it change my Packet Tracer file?**
No. It only reads it. The only file it creates is `lab_decoded.xml`.

**Does it work offline?**
No. It needs the internet to reach Gemini.

**Is my lab private?**
Your lab's device configs are sent to Google's Gemini API to get an answer. Don't load labs that contain real passwords, keys, or private company information.

**Can it read `.pka` activity files?**
Untested. The decoder is documented for `.pkt` files.

**Can I ask it about topics other than networking?**
No. It is set up to refuse them. You can change this in `SYSTEM_PROMPT` (see [Customizing](#-customizing)).

**Does it remember my earlier questions?**
It keeps your **loaded lab** in memory for the whole session, but each question is answered on its own. Include the details it needs in each question.

**Will it work on Mac or Linux?**
It should, since it is plain Python. Use `python3` and `pip3`, and set the key with `export` as shown in Step 5. The author tested it on Windows.

---

## 📖 Glossary

| Term | Plain meaning |
|---|---|
| **Terminal / PowerShell** | A text window where you type commands. |
| **Python** | The programming language this tool is written in. |
| **pip** | Python's tool for installing packages (add-ons). |
| **API key** | A secret password that lets a program use a service like Gemini. |
| **Environment variable** | A named setting stored on your computer (here, `GEMINI_API_KEY`) so programs can read it without it being in the code. |
| **`.pkt` file** | A Cisco Packet Tracer lab file. It is encrypted, so normal programs can't read it directly. |
| **Running-config** | The settings currently active on a router or switch. You see it with `show running-config`. |
| **XML** | A text format for storing data. The decoded lab is XML. |
| **System prompt** | The hidden rules given to the AI (here: "only answer networking questions"). |

---

## ⚠️ Limits and responsible use

**Limits**

- Only **running-configs** are read. Things that exist only on the Packet Tracer canvas (cable colors, notes, positions) are not extracted.
- Very large labs are **cut off** at about 150,000 characters.
- Decoding depends on your **Packet Tracer version**. If it fails, use the `show running-config` copy-and-paste method.
- **The AI can be wrong.** Always double-check its advice in Packet Tracer or on real equipment.

**Responsible use**

Only open **your own** Packet Tracer files, or files you have permission to use. Do not use this to pull answers out of graded activities or exams. That would be cheating.

**Sharing this project on GitHub?** Add a `.gitignore` file containing:

```text
*.pkt
*_decoded.xml
```

This stops lab files from being uploaded by accident. **Never commit your API key.**

---

## 🎛️ Customizing

All of these are edits inside `networkingai.py`:

| I want to... | Change this |
|---|---|
| Use a different Gemini model | The line `model="gemini-2.5-flash"` |
| Make answers more precise or more creative | `"temperature": 0.2` (lower = precise, higher = varied) |
| Change what topics are allowed or the answer style | The text inside `SYSTEM_PROMPT` |
| Send more or less of a big lab | The `[:150000]` limit in `pkt_to_text` |

---

## 🙏 Credits

- [Google Gemini API](https://ai.google.dev/) for the AI.
- [SecuredyPKT](https://pypi.org/project/securedypkt/) by Securedy Labs for decoding Packet Tracer files.
- Cisco Packet Tracer is a product of Cisco Systems. This project is not affiliated with or endorsed by Cisco or Google.

---

*Found a problem or have an idea? Open an issue or send a pull request.*
