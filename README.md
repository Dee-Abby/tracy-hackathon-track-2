# tracy-hackathon-track-2
# 🦠 HealthTrace — Tracy Contact Tracing Model

> **IoT-based contact tracing and risk stratification system using Bluetooth proximity detection, mobility analysis, and wearable health vitals.**  
> Built with Python · KMeans Clustering · Z-Score Anomaly Detection · NetworkX · PCA

---

## 📌 Table of Contents

- [Project Overview](#-project-overview)
- [Repository Structure](#-repository-structure)
- [Requirements](#-requirements)
- [Setup Instructions](#-setup-instructions)
- [How to Run the Notebook](#-how-to-run-the-notebook)
- [Dataset Guide](#-dataset-guide)
- [What the Notebook Does](#-what-the-notebook-does)
- [Outputs](#-outputs)
- [Sharing & Collaborating on GitHub](#-sharing--collaborating-on-github)
- [Common Errors & Fixes](#-common-errors--fixes)
- [Contributing](#-contributing)

---

## 📖 Project Overview

This project analyses contact tracing data collected from IoT wearable devices deployed across a population sample. It identifies high-risk individuals, detects outbreak days, and flags health anomalies using:

- **KMeans Clustering** — groups users into Low, High, and Extreme Risk tiers
- **Z-Score Anomaly Detection** — flags dates with abnormally high contact activity
- **Network Centrality (NetworkX)** — identifies super-spreader nodes in the contact graph
- **PCA** — visualises multi-dimensional risk profiles in 2D
- **Vitals Analysis** — detects fever and tachycardia events from wearable sensor readings

---

## 📁 Repository Structure

```
tracy-model/
│
├── tracy_model.ipynb          ← Main analysis notebook
│
├── data/
│   ├── contact_tracing.csv    ← Bluetooth proximity contact logs
│   ├── mobility.csv           ← User mobility & exposure scores
│   └── vitals.csv             ← Wearable device temperature & heart rate
│
├── outputs/
│   ├── cluster_visualisation.png
│   ├── anomaly_timeline.png
│   ├── vitals_analysis.png
│   └── healthtrace_summary_dashboard.png
│
├── requirements.txt           ← All Python dependencies
├── .gitignore                 ← Files excluded from version control
└── README.md                  ← This file
```

> ⚠️ **Do not rename the folders.** The notebook uses relative paths that depend on this structure.

---

## ✅ Requirements

- Python **3.9 or higher**
- pip (comes with Python)
- Git
- A GitHub account

### Python Libraries

All dependencies are listed in `requirements.txt`:

```
pandas>=1.5.0
numpy>=1.23.0
matplotlib>=3.6.0
seaborn>=0.12.0
scikit-learn>=1.1.0
networkx>=2.8.0
jupyter>=1.0.0
notebook>=6.5.0
```

---

## ⚙️ Setup Instructions

Follow these steps **exactly** before opening the notebook.

### Step 1 — Clone the Repository

Open your terminal (or Git Bash on Windows) and run:

```bash
git clone https://github.com/YOUR-USERNAME/tracy-model.git
cd tracy-model
```

> Replace `YOUR-USERNAME` with your actual GitHub username.

---

### Step 2 — Create a Virtual Environment

A virtual environment keeps this project's packages separate from everything else on your machine.

**On Mac/Linux:**
```bash
python3 -m venv venv
source venv/bin/activate
```

**On Windows:**
```bash
python -m venv venv
venv\Scripts\activate
```

You will see `(venv)` appear at the start of your terminal prompt when it is active.

---

### Step 3 — Install Dependencies

```bash
pip install -r requirements.txt
```

This installs all required libraries in one command. It may take 1–2 minutes.

---

### Step 4 — Confirm Installation

Run this to verify the key libraries installed correctly:

```bash
python -c "import pandas, sklearn, networkx, matplotlib; print('All good ✓')"
```

If you see `All good ✓` you are ready to proceed.

---

## ▶️ How to Run the Notebook

### Step 1 — Launch Jupyter

```bash
jupyter notebook
```

This opens a browser tab at `http://localhost:8888`. If it does not open automatically, copy that URL into your browser.

---

### Step 2 — Open the Notebook

In the browser, click on **`tracy_model.ipynb`** to open it.

---

### Step 3 — Run All Cells

From the menu bar inside Jupyter:

```
Kernel → Restart & Run All
```

> Always use **Restart & Run All** — not just Run All — to ensure the notebook runs cleanly from a fresh state with no leftover variables.

---

### Step 4 — Check Outputs

After all cells finish running, check the `outputs/` folder. You should see four PNG files generated:

| File | Contents |
|---|---|
| `cluster_visualisation.png` | PCA scatter + exposure bar chart |
| `anomaly_timeline.png` | Daily contact detections with flagged days |
| `vitals_analysis.png` | Temperature & heart rate analysis per device |
| `healthtrace_summary_dashboard.png` | Full 6-panel summary dashboard |

---

## 📊 Dataset Guide

The project requires three CSV files placed inside the `data/` folder.

### `contact_tracing.csv`

Records of Bluetooth proximity contacts between wearable devices.

| Column | Type | Description |
|---|---|---|
| `user_id` | string | Unique user identifier (e.g. U073) |
| `date` | date | Date of contact event |
| `mac` | string | MAC address of detected nearby device |
| `geohash` | string | Encoded GPS location of the contact |
| `proximity` | string | Category: `close`, `very close`, `moderate`, `far`, `unscored` |
| `rssi` | float | Bluetooth signal strength (−1 = unreadable) |

---

### `mobility.csv`

Pre-computed daily mobility and exposure summary per user.

| Column | Type | Description |
|---|---|---|
| `user_id` | string | Unique user identifier |
| `date` | date | Date of record |
| `exposure_score` | float | Cumulative proximity-weighted exposure |
| `daily_exposure` | float | Single-day exposure score |
| `total_detections` | int | Total Bluetooth detections that day |
| `close_contacts` | int | Close-range contacts that day |
| `has_contact` | bool | Whether any contact occurred |

---

### `vitals.csv`

Raw wearable sensor readings for temperature and heart rate.

| Column | Type | Description |
|---|---|---|
| `device_id` | string | Wearable device identifier (e.g. D020) |
| `date` | date | Date of reading |
| `temperature` | float | Surface body temperature in °C |
| `heartbeat` | int | Heart rate in BPM |
| `temp_status` | string | `high` (>37.5°C), `normal`, or `low` |
| `hr_status` | string | `high` (>100 BPM), `normal`, or `low` (<60 BPM) |

---

## 🔬 What the Notebook Does

The notebook runs five stages in sequence:

| Stage | Cells | What it does |
|---|---|---|
| **1. Data loading** | 1–4 | Loads CSVs, checks nulls, converts dates |
| **2. Feature engineering** | 5–9 | Builds one risk-feature row per user from all three datasets |
| **3. Clustering** | 10–13 | Scales features → KMeans → PCA visualisation → risk tier labels |
| **4. Anomaly detection** | 14–15 | Z-score flags outbreak days on the daily contact timeline |
| **5. Vitals & reporting** | 16–19 | Fever/tachycardia detection per device + full dashboard |

---

## 📤 Sharing & Collaborating on GitHub

Follow these steps to push your notebook to GitHub so peers can access it.

---

### Step 1 — Create the GitHub Repository

1. Go to [https://github.com/new](https://github.com/new)
2. Name your repository: `tracy-model`
3. Set visibility to **Public** (so peers can view without logging in) or **Private** (invite peers by username)
4. Do **not** tick "Add a README" — you already have one
5. Click **Create repository**

---

### Step 2 — Initialise Git Locally

Inside your project folder run:

```bash
git init
git add .
git commit -m "Initial commit: tracy model notebook and datasets"
```

---

### Step 3 — Connect to GitHub and Push

Copy the remote URL from your newly created GitHub repository page, then:

```bash
git remote add origin https://github.com/YOUR-USERNAME/tracy-model.git
git branch -M main
git push -u origin main
```

Visit `https://github.com/YOUR-USERNAME/tracy-model` — your notebook and all files should now be visible.

---

### Step 4 — How Peers Clone and Run It

Send your peers this single command to get started:

```bash
git clone https://github.com/YOUR-USERNAME/tracy-model.git
cd tracy-model
python3 -m venv venv && source venv/bin/activate
pip install -r requirements.txt
jupyter notebook
```

> Windows peers should use `venv\Scripts\activate` instead of `source venv/bin/activate`.

---

### Step 5 — Keeping the Notebook Up to Date

When you make changes to the notebook and want peers to receive them:

```bash
# Clear cell outputs before committing — keeps diffs clean
jupyter nbconvert --clear-output --inplace tracy_model.ipynb

git add .
git commit -m "describe what you changed"
git push
```

Peers pull the latest version with:

```bash
git pull origin main
```

---

### Step 6 — Collaborating (Optional)

If peers want to contribute changes:

1. They **fork** the repository on GitHub (top-right button on the repo page)
2. Clone their fork locally and make changes
3. Open a **Pull Request** back to your repository for you to review and merge

To invite specific peers as collaborators instead:

```
GitHub repo page → Settings → Collaborators → Add people
```

---

## 🚨 Common Errors & Fixes

| Error | Cause | Fix |
|---|---|---|
| `FileNotFoundError: contact_tracing.csv` | CSV files not in `data/` folder | Move all three CSVs into the `data/` subfolder |
| `ModuleNotFoundError: No module named 'networkx'` | Dependencies not installed | Run `pip install -r requirements.txt` |
| `NameError: name 'user_features' is not defined` | Cells run out of order | Use `Kernel → Restart & Run All` |
| `jupyter: command not found` | Jupyter not installed or venv not active | Activate venv first, then `pip install jupyter` |
| Plots not showing | `%matplotlib inline` not executed | Run Cell 1 first, or use `Kernel → Restart & Run All` |
| `git push` rejected | Remote has changes you don't have locally | Run `git pull origin main` first, then push again |
| Large file error on push | Output PNGs exceed GitHub's 100MB limit | Add `outputs/*.png` to `.gitignore` |

---

## 🤝 Contributing

1. Fork this repository
2. Create a feature branch: `git checkout -b feature/your-improvement`
3. Commit your changes: `git commit -m "add: description of change"`
4. Push to your fork: `git push origin feature/your-improvement`
5. Open a Pull Request describing what you changed and why

Please clear notebook outputs before submitting a Pull Request:

```bash
jupyter nbconvert --clear-output --inplace tracy_model.ipynb
```

---

## 📄 Licence

This project is shared for academic and research purposes.  
Please credit the original authors when referencing or adapting this work.

---

*HealthTrace Tracy Model · Ibadan, Nigeria · 2023–2024*