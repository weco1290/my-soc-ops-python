# 🎯 Soc Ops — Social Bingo

> **Break the ice, make connections, win at networking!**

Soc Ops is an interactive social bingo game designed for in-person mixers, team events, and conferences. Find people who match the prompts, mark your card, and race to get 5 in a row!

🎮 **[Play the Game](https://madebygps.github.io/vscode-github-copilot-agent-lab/)** • 📚 **[View Lab Guide](https://madebygps.github.io/vscode-github-copilot-agent-lab/docs/)**

---

## ✨ Features

- 🎲 **Randomized boards** — Every player gets a unique arrangement
- 💾 **Auto-save progress** — Pick up where you left off
- 🏆 **Bingo detection** — Automatic win detection for rows, columns, and diagonals
- 🎉 **Celebration modal** — Confetti-worthy victory screen
- 📱 **Mobile-first** — Works great on phones at events

---

## 🚀 Quick Start

### Prerequisites

- [Python 3.13+](https://www.python.org/downloads/)
- [uv](https://docs.astral.sh/uv/) (Python package manager)

### Run Locally

```bash
uv sync
uv run uvicorn app.main:app --reload --port 8000
# Open http://localhost:8000
```

### Test

```bash
uv run pytest
```

### Lint

```bash
uv run ruff check .
uv run ruff format .
```

---

## 🎨 Customize Your Game

### Change Questions

Edit `app/data.py` to add your own icebreaker prompts:

```python
questions_list: list[str] = [
    "has a pet",
    "speaks more than 2 languages",
    "your custom question here",
    # ... 24+ questions for a full board
]
```

### Workshop Guide

👉 Follow the [Lab Guide](workshop/GUIDE.md) for a hands-on workshop experience with GitHub Copilot agents.

| Part | Title |
|------|-------|
| [**00**](https://madebygps.github.io/vscode-github-copilot-agent-lab/docs/step.html?step=00-overview) | Overview & Checklist |
| [**01**](https://madebygps.github.io/vscode-github-copilot-agent-lab/docs/step.html?step=01-setup) | Setup & Context Engineering |
| [**02**](https://madebygps.github.io/vscode-github-copilot-agent-lab/docs/step.html?step=02-design) | Design-First Frontend |
| [**03**](https://madebygps.github.io/vscode-github-copilot-agent-lab/docs/step.html?step=03-quiz-master) | Custom Quiz Master |
| [**04**](https://madebygps.github.io/vscode-github-copilot-agent-lab/docs/step.html?step=04-multi-agent) | Multi-Agent Development |

> 📝 Lab guides are also available in the [`workshop/`](workshop/) folder for offline reading.

---

## 🛠️ Tech Stack

- **Framework**: FastAPI + Jinja2 + HTMX
- **Styling**: Custom CSS utilities (Tailwind-inspired)
- **State**: Server-side sessions with cookie persistence
- **Deployment**: GitHub Pages via Actions

---

## 📁 Project Structure

```
app/
├── templates/       # Jinja2 templates
│   ├── base.html
│   ├── home.html
│   └── components/  # bingo_board, bingo_modal, game_screen, start_screen
├── static/          # CSS & JS assets
├── models.py        # Game state & data models
├── game_logic.py    # Bingo detection & board generation
├── game_service.py  # Session management
├── data.py          # Question bank
└── main.py          # FastAPI routes
tests/
├── test_api.py      # API endpoint tests
└── test_game_logic.py  # Game logic unit tests
```

---

## 🚢 Deployment

Automatically deploys to GitHub Pages on push to `main`:

- Your game: `https://{username}.github.io/{repo-name}`

---

## 📝 License

MIT — use it for your next event!
