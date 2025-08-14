# BrainDump Ecosystem

A comprehensive thought capture and task management system combining elegant note-taking with intelligent task extraction and Notion synchronization.

## 🧠 Overview

BrainDump is a powerful ecosystem consisting of two complementary tools:

1. **[BrainBar](https://github.com/omar-elamin/brainbar)** - Beautiful macOS app for instant thought capture
2. **[Brainpipe](https://github.com/omar-elamin/braindumpsync)** - Raycast extension for AI-powered task extraction and Notion sync

Together, they create a seamless workflow: capture thoughts instantly, let AI extract actionable tasks, and sync everything to your productivity system.

## 🚀 Quick Start

### The Complete Workflow

1. **Capture** thoughts with BrainBar (instant floating window)
2. **Extract** tasks automatically with Brainpipe (AI-powered)
3. **Organize** in Notion (automated sync)

### Installation Overview

1. **Install BrainBar** for instant note capture
2. **Install Brainpipe** for task extraction and Notion sync
3. **Configure** your Notion database and API keys
4. **Start capturing** thoughts and let the automation handle the rest

---

## 📝 BrainBar - Instant Thought Capture

> *A beautiful, frictionless macOS application for instant thought capture*

### ✨ Features

- **Instant Access**: Floating window positioned like Spotlight
- **Beautiful Design**: Native macOS vibrancy with blur effects and dark/light mode
- **Frictionless Workflow**: 
  - `Return` → Save and close
  - `Shift + Return` → New line
  - `Esc` → Close without saving
  - Click away → Auto-close
- **Organized Storage**: Notes saved to `~/BrainDump/inbox/` by date
- **Markdown Format**: Auto-formatted with timestamps

### 🏃‍♂️ Quick Setup

```bash
# Build BrainBar
xcrun swiftc -O -sdk "$(xcrun --show-sdk-path --sdk macosx)" -framework Cocoa brainbar/BrainBar.swift -o ~/bin/brainbar

# Make executable
chmod +x ~/bin/brainbar

# Run
~/bin/brainbar
```

### 🎯 Raycast Integration (Recommended)

1. Import the BrainBar launch script into Raycast
2. Set your preferred hotkey (e.g., `⇧⌘Space`)
3. Launch BrainBar instantly from anywhere

### 📁 File Organization

```
~/BrainDump/inbox/
├── 2024-01-15.md
├── 2024-01-16.md
└── 2024-01-17.md
```

Each note is timestamped:
```markdown
## 14:30:25
- Your brilliant idea here

## 15:45:12
- Another thought with todo items
- [ ] Follow up with Alice due:16/08
```

---

## 🔄 Brainpipe - AI Task Extraction & Notion Sync

> *Raycast extension that automatically extracts actionable to-dos from markdown files and syncs to Notion*

### ✨ Features

- **Automatic Hourly Sync** (can be enabled/disabled)
- **Manual "Run Now" Command** for immediate syncing
- **Smart Task Extraction** from multiple markdown formats:
  - `- [ ] Buy milk`
  - `TODO: call John`
  - `* follow up with Sarah`
  - Action items with due dates: `due:16/08`, `due:2025-08-20`
- **Duplicate Prevention** using content-based hashing
- **File Change Detection** (only processes modified files)
- **Date Normalization** (British DD/MM format → YYYY-MM-DD)
- **Comprehensive Logging** and error handling

### 🏗️ Installation & Setup

#### 1. Install Brainpipe Extension

```bash
# Clone to Raycast extensions directory
git clone https://github.com/omar-elamin/braindumpsync ~/raycast-extensions/brainpipe
cd ~/raycast-extensions/brainpipe

# Install dependencies and build
npm install
npm run build
```

#### 2. Load in Raycast

1. Open Raycast → Search "Import Extension"
2. Navigate to `~/raycast-extensions/brainpipe`
3. Import the extension

#### 3. Configure Preferences

##### Required Settings:
- **Brain Dump Directory**: `~/BrainDump/inbox` (matches BrainBar output)
- **OpenAI API Key**: For task extraction
- **OpenAI Model**: `gpt-4o` (recommended)
- **Notion Integration Token**: Your Notion integration token
- **Notion Database ID**: Target database ID

##### Optional Settings:
- **Enable Hourly Background Sync**: Toggle automatic syncing

#### 4. Set Up Notion Database

Create a database with these exact columns:

| Column Name | Type | Description |
|-------------|------|-------------|
| `Name` | Title | Task title |
| `Status` | Select | Task status (create "Inbox" option) |
| `Due Date` | Date | Optional due date |
| `Tags` | Multi-select | Optional tags |
| `Task ID` | Rich Text | Unique identifier |
| `Source` | Rich Text | Source file and line number |

#### 5. Get API Keys

**OpenAI API Key:**
1. Visit [OpenAI API Platform](https://platform.openai.com/)
2. Generate new secret key (starts with `sk-`)

**Notion Integration:**
1. Visit [Notion Integrations](https://www.notion.so/my-integrations)
2. Create integration → Copy token (starts with `secret_`)
3. Share your database with the integration

### 🎯 Usage

#### Manual Sync
- Open Raycast → Search "Extract & Sync (Run Now)"
- View progress notifications and results

#### Automatic Sync
- Runs hourly in background when enabled
- Silent operation with logging

### 📝 Supported Task Formats

```markdown
# Checkbox tasks
- [ ] Buy groceries
- [ ] Send email to client due:16/08
- [ ] Review PR #123

# TODO items  
TODO: Call dentist for appointment
TODO: Update documentation due:2025-08-20

# Action items
* follow up with Sarah about proposal
* book meeting room for next week due:18/8

# With tags (extracted automatically)
- [ ] Plan team meeting #work #urgent
- [ ] Buy birthday gift #personal #shopping
```

### 📅 Date Format Support

- `due:2025-08-16` → `2025-08-16` (ISO format)
- `due:16/08` → `2025-08-16` (British format, assumes current year)
- `due:16/8` → `2025-08-16` (British format, single digit month)

---

## 🔧 Complete Workflow Example

### 1. Capture a Thought (BrainBar)

Press your hotkey → BrainBar appears:
```
Need to follow up with the client about the proposal due next Friday
Also should book the conference room for team meeting
- [ ] Review budget numbers #finance
TODO: Update project documentation due:25/08
```
Press Enter → Saved to `~/BrainDump/inbox/2024-08-15.md`

### 2. Automatic Task Extraction (Brainpipe)

Brainpipe runs hourly and extracts:
- "Follow up with the client about the proposal" (due: 2024-08-23)
- "Book the conference room for team meeting"
- "Review budget numbers" (tag: finance)
- "Update project documentation" (due: 2024-08-25)

### 3. Notion Integration

Tasks appear in your Notion database with:
- Proper titles and due dates
- Extracted tags
- Source information
- "Inbox" status for triage

---

## 🛠️ Development

### Project Structure

```
BrainDump/
├── README.md              # This file
├── brainbar/              # macOS note capture app
│   ├── BrainBar.swift     # Single-file Swift application
│   └── README.md          # BrainBar documentation
└── braindumpsync/         # Raycast task extraction extension
    ├── src/               # TypeScript source code
    ├── package.json       # Dependencies and scripts
    └── README.md          # Brainpipe documentation
```

### Running Tests

```bash
# Test Brainpipe
cd braindumpsync
npm test

# Test with coverage
npm test -- --coverage
```

### Building

```bash
# Build BrainBar
cd brainbar
xcrun swiftc -O -sdk "$(xcrun --show-sdk-path --sdk macosx)" -framework Cocoa BrainBar.swift -o ~/bin/brainbar

# Build Brainpipe
cd braindumpsync
npm run build
```

---

## 🧪 Test Drive

### Quick Test Setup

1. **Create test brain dump:**
```bash
mkdir -p ~/BrainDump/inbox
cat > ~/BrainDump/inbox/test-$(date +%Y-%m-%d).md << 'EOF'
# Meeting Notes

## Action Items
- [ ] Follow up with Alice about project timeline due:16/08
- [ ] Review budget proposal #finance #urgent  
- [ ] Book conference room for next team meeting

TODO: Update project documentation
TODO: Send weekly report to stakeholders due:2025-08-20

## Completed
- [x] Sent meeting invite (this should be ignored)
EOF
```

2. **Configure Brainpipe** with your API keys
3. **Run manual sync** in Raycast
4. **Check Notion** for extracted tasks

---

## 🔍 Troubleshooting

### Common Issues

**BrainBar won't launch:**
- Check executable permissions: `chmod +x ~/bin/brainbar`
- Verify build succeeded without errors
- Try running from Terminal to see error messages

**Brainpipe not extracting tasks:**
- Verify OpenAI API key is correct and has credits
- Check brain dump directory path is correct
- Ensure markdown files contain recognizable task patterns
- Review logs: `tail -f ~/Library/Application\ Support/com.raycast.macos/extensions/brainpipe/brainpipe.log`

**Notion sync failing:**
- Verify Notion integration token and database ID
- Ensure database is shared with integration
- Check all required columns exist with exact names
- Verify column types match requirements

### Debug Logging

```bash
# View Brainpipe logs
tail -f ~/Library/Application\ Support/com.raycast.macos/extensions/brainpipe/brainpipe.log

# Check Raycast developer console for real-time output
```

---

## 🔒 Privacy & Security

- **API Keys**: Stored securely in Raycast's encrypted preferences
- **Data Processing**: Markdown files are only read, never modified
- **OpenAI Usage**: Direct API calls with your key (not Raycast Pro)
- **Local State**: Task hashes and timestamps stored locally
- **No Telemetry**: No usage data sent to third parties

---

## 🤝 Contributing

Both projects welcome contributions:

1. **Issues & Suggestions**: Open GitHub issues
2. **Pull Requests**: Submit improvements
3. **Customization**: Fork and adapt for your workflow

### Development Setup

```bash
# Clone with submodules
git clone --recurse-submodules https://github.com/your-username/BrainDump.git

# Or if already cloned
git submodule update --init --recursive
```

---

## 📄 License

Both BrainBar and Brainpipe are open source - feel free to use, modify, and distribute.

---

## 🆘 Support

For issues, feature requests, or contributions:

1. Check troubleshooting sections above
2. Review project-specific READMEs in submodules
3. Test with simple examples first
4. Check logs for specific error messages

**Remember**: Brainpipe calls OpenAI directly using your API key, giving you full control over usage and costs.
