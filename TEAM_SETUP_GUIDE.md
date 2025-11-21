# 🚀 Team Setup & Collaboration Guide

## Memory-Augmented RAG for Lifelong Autonomous Driving

**Team Members:**
- Karina Shah (1224902822)
- Dhruvina Gujarati (1224720263)
- Nilay Kumar (1225127891)
- Nishanth Krishna Churchmal (1232021156)

**Course:** CSE 475 - Foundations of Machine Learning, Fall 2025

---

## 📋 Table of Contents
1. [Environment Setup](#environment-setup)
2. [Dataset Download](#dataset-download)
3. [Clone Project Repository](#clone-project-repository)
4. [Working on Project Phases](#working-on-project-phases)
5. [Git Workflow](#git-workflow)
6. [Troubleshooting](#troubleshooting)

---

## 🛠️ Environment Setup

### Step 1: Install Miniconda

**For Mac (M1/M2/M4 - Apple Silicon):**
```bash
# Download from: https://docs.conda.io/en/latest/miniconda.html
# Choose: "64-Bit (Apple silicon) Graphical Installer"
# Double-click the .pkg file to install
```

**For Mac (Intel):**
```bash
# Download: "64-Bit (Intel) Graphical Installer"
```

**For Linux:**
```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh
```

**Verify installation:**
```bash
# Close and reopen Terminal
conda --version
# Should output: conda 25.x.x or similar
```

---

### Step 2: Clone nuPlan Devkit

```bash
cd ~
git clone https://github.com/motional/nuplan-devkit.git
cd nuplan-devkit
```

---

### Step 3: Create Conda Environment

```bash
conda create -n nuplan python=3.9 -y
conda activate nuplan
```

**⚠️ Important:** Always activate this environment before working!

---

### Step 4: Install Dependencies

**Run these commands in order (copy-paste entire blocks):**

```bash
# 1. Install PyTorch (Mac/Linux compatible)
pip install torch==1.9.0 torchvision==0.10.0 torchaudio==0.9.0
```

```bash
# 2. Install PyYAML
pip install "PyYAML>=5.1,<6.0" --no-build-isolation
```

```bash
# 3. Install ML dependencies
pip install future==0.18.1 pytorch-lightning==1.3.8 torchmetrics==0.7.2 timm
```

```bash
# 4. Install core dependencies
pip install setuptools==59.5.0
pip install hydra-core==1.1.1 SQLAlchemy==1.4.27 bokeh==2.4.3 boto3==1.24.59 "shapely>=2.0.0" "geopandas>=0.12.1" numpy==1.23.4
```

```bash
# 5. Install additional packages
pip install pandas scipy matplotlib opencv-python Pillow pyquaternion joblib jupyter jupyterlab cachetools requests retry casadi control pyarrow rasterio Fiona rtree psutil ray
```

```bash
# 6. Install utilities
pip install aioboto3 aiofiles coverage docker grpcio==1.43.0 grpcio-tools==1.43.0 guppy3==3.1.2 hypothesis mock moto nest_asyncio pre-commit pyinstrument pytest selenium sympy testbook typer ujson
```

```bash
# 7. Install nuPlan devkit
pip install -e .
```

---

### Step 5: Set Environment Variables

**For Mac (zsh - default on newer Macs):**
```bash
# Create directory structure
mkdir -p ~/nuplan/dataset/nuplan-v1.1/splits/mini
mkdir -p ~/nuplan/dataset/maps
mkdir -p ~/nuplan/exp

# Add environment variables
echo 'export NUPLAN_DATA_ROOT="$HOME/nuplan/dataset"' >> ~/.zshrc
echo 'export NUPLAN_MAPS_ROOT="$HOME/nuplan/dataset/maps"' >> ~/.zshrc
echo 'export NUPLAN_EXP_ROOT="$HOME/nuplan/exp"' >> ~/.zshrc

# Reload shell
source ~/.zshrc
```

**For Linux or older Mac (bash):**
```bash
# Create directory structure
mkdir -p ~/nuplan/dataset/nuplan-v1.1/splits/mini
mkdir -p ~/nuplan/dataset/maps
mkdir -p ~/nuplan/exp

# Add environment variables (use .bashrc instead)
echo 'export NUPLAN_DATA_ROOT="$HOME/nuplan/dataset"' >> ~/.bashrc
echo 'export NUPLAN_MAPS_ROOT="$HOME/nuplan/dataset/maps"' >> ~/.bashrc
echo 'export NUPLAN_EXP_ROOT="$HOME/nuplan/exp"' >> ~/.bashrc

# Reload shell
source ~/.bashrc
```

**Verify environment variables:**
```bash
echo $NUPLAN_DATA_ROOT
echo $NUPLAN_MAPS_ROOT
echo $NUPLAN_EXP_ROOT
```

You should see paths like:
```
/Users/yourname/nuplan/dataset
/Users/yourname/nuplan/dataset/maps
/Users/yourname/nuplan/exp
```

---

## 📦 Dataset Download

⚠️ **CRITICAL:** The dataset is NOT in Git repository (8.5 GB - too large). Each team member MUST download it separately.

### Step 1: Create Account

1. Go to: https://www.nuscenes.org/nuplan
2. Click **"Sign Up"** and create account
3. Agree to **Terms of Use**
4. Login to your account

---

### Step 2: Download Files

**You need to download TWO files:**

| File | Size | Description |
|------|------|-------------|
| `nuplan-maps-v1.0.zip` | 0.90 GB | Map data for 4 cities |
| `nuplan-v1.1_mini.zip` | 7.96 GB | Mini split database files |

Both will download to your `~/Downloads` folder.

**Download from:** https://www.nuscenes.org/nuplan (after login)

---

### Step 3: Extract Dataset

```bash
cd ~/Downloads

# 1. Extract maps
unzip nuplan-maps-v1.0.zip -d ~/nuplan/dataset/maps/

# 2. Extract mini split
unzip nuplan-v1.1_mini.zip -d ~/nuplan/dataset/

# 3. Fix map structure (IMPORTANT!)
mv ~/nuplan/dataset/maps/maps/* ~/nuplan/dataset/maps/
rmdir ~/nuplan/dataset/maps/maps/

# 4. Move database files to correct location
mv ~/nuplan/dataset/data/cache/mini/*.db ~/nuplan/dataset/nuplan-v1.1/splits/mini/
```

---

### Step 4: Verify Dataset Setup

```bash
# Check database files (should see ~64 .db files)
ls ~/nuplan/dataset/nuplan-v1.1/splits/mini/ | head -5

# Check maps (should see 4 city folders + json file)
ls ~/nuplan/dataset/maps/
```

**Expected output:**
```
# Database files:
2021.05.12.22.00.38_veh-35_01008_01518.db
2021.05.12.22.28.35_veh-35_00620_01164.db
...

# Maps:
LICENSE
nuplan-maps-v1.0.json
sg-one-north/
us-ma-boston/
us-nv-las-vegas-strip/
us-pa-pittsburgh-hazelwood/
```

✅ **If you see these files, your dataset is correctly installed!**

---

## 📂 Clone Project Repository

```bash
cd ~
git clone https://github.com/YOUR_USERNAME/memory-rag-autonomous-driving.git
cd memory-rag-autonomous-driving
```

**Project Structure:**
```
memory-rag-autonomous-driving/
├── .git/               # Git repository (hidden)
├── .gitignore          # Excludes dataset and large files
├── notebooks/          # 📓 Jupyter notebooks (YOUR WORK GOES HERE)
├── src/                # 🐍 Python source code modules
├── results/            # 📊 Small results only (plots, CSVs)
│   └── plots/
├── docs/               # 📄 Documentation
├── configs/            # ⚙️ Configuration files
└── requirements.txt    # 📦 Python dependencies
```

---

## 🎯 Working on Project Phases

### Implementation Timeline

| Days | Phase | Notebook | Tasks |
|------|-------|----------|-------|
| **Nov 20-21** | Phase 1: Baseline | `01_baseline_evaluation.ipynb` | SimplePlanner evaluation, baseline metrics |
| **Nov 22-23** | Phase 2: Data Processing | `02_data_embeddings.ipynb` | Scenario text, embeddings, FAISS setup |
| **Nov 24-25** | Phase 3: RAG System | `03_rag_system.ipynb` | Retrieval mechanism, GPT-2 integration |
| **Nov 26-27** | Phase 4: Memory Module | `04_memory_augmented.ipynb` | Continual updates, full system testing |
| **Nov 28-30** | Phase 5: Evaluation | `05_final_evaluation.ipynb` | Metrics, plots, final report |

---

### How to Work on a Specific Phase

#### Option A: Create New Notebook (if doesn't exist)

```bash
cd ~/memory-rag-autonomous-driving
conda activate nuplan
jupyter lab
```

**In Jupyter Lab:**
1. Click **"+"** button (New Launcher)
2. Under **Notebook**, click **"Python 3 (ipykernel)"**
3. **Rename notebook:**
   - Right-click on "Untitled.ipynb" tab
   - Click "Rename"
   - Use naming convention: `0X_phase_name.ipynb`

**Notebook Naming Convention:**
- Phase 1: `01_baseline_evaluation.ipynb`
- Phase 2: `02_data_embeddings.ipynb`
- Phase 3: `03_rag_system.ipynb`
- Phase 4: `04_memory_augmented.ipynb`
- Phase 5: `05_final_evaluation.ipynb`

**First Cell Template:**
```python
"""
Phase X: [Phase Name]
=====================
Goal: [What this phase accomplishes]

Author: [Your name]
Date: [Today's date]
"""

import sys
sys.path.append('/Users/YOUR_USERNAME/nuplan-devkit')

import os
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

# Verify environment setup
print("Environment Check:")
print(f"NUPLAN_DATA_ROOT: {os.getenv('NUPLAN_DATA_ROOT')}")
print(f"NUPLAN_MAPS_ROOT: {os.getenv('NUPLAN_MAPS_ROOT')}")
print(f"NUPLAN_EXP_ROOT: {os.getenv('NUPLAN_EXP_ROOT')}")
```

---

#### Option B: Continue Existing Notebook (created by teammate)

```bash
cd ~/memory-rag-autonomous-driving

# ALWAYS pull latest changes first!
git pull origin main

# Activate environment
conda activate nuplan

# Start Jupyter
jupyter lab
```

**In Jupyter Lab:**
1. Navigate to `notebooks/` folder in left sidebar
2. Double-click the relevant notebook to open
3. Scroll to bottom or find your assigned section
4. Add new cells for your work
5. Save frequently: `Cmd+S` (Mac) or `Ctrl+S` (Windows/Linux)

---

## 🔄 Git Workflow

### 🚨 Golden Rule: ALWAYS Pull Before Starting Work!

```bash
cd ~/memory-rag-autonomous-driving
git pull origin main
```

This prevents merge conflicts!

---

### Standard Workflow

#### Step 1: Create Your Branch (Recommended)

```bash
# Create and switch to new branch
git checkout -b yourname/phase-description

# Examples:
# git checkout -b karina/baseline-metrics
# git checkout -b dhruvina/rag-system
# git checkout -b nilay/memory-module
# git checkout -b nishanth/evaluation
```

**Branch naming convention:**
- `yourname/feature-description`
- Use lowercase and hyphens
- Be descriptive but concise

---

#### Step 2: Do Your Work

- Edit notebooks in Jupyter Lab
- Write code in `src/` if needed
- Create small result files in `results/`
- Save frequently!

---

#### Step 3: Check What Changed

```bash
# See modified files
git status

# See specific changes
git diff
```

---

#### Step 4: Stage Your Changes

```bash
# Stage specific notebook
git add notebooks/01_baseline_evaluation.ipynb

# Stage all notebooks
git add notebooks/

# Stage source code
git add src/your_file.py

# Stage small results (plots, CSVs only!)
git add results/plots/*.png
git add results/baseline_metrics.csv
```

**⚠️ NEVER ADD:**
- Dataset files (`*.db`, `*.gpkg`)
- Large checkpoints (`*.pth`, `*.pkl` > 10MB)
- Cache directories
- Virtual environments

The `.gitignore` file prevents these automatically!

---

#### Step 5: Commit Your Changes

```bash
git commit -m "Phase 1: Completed baseline evaluation

- Loaded 6847 scenarios from nuPlan mini split
- Executed SimplePlanner on 100 test scenarios
- Calculated baseline metrics: collision rate, path accuracy
- Generated baseline plots in results/plots/
- Documented results in notebook"
```

**Good Commit Message Format:**
```
[Phase/Component]: Brief summary (50 chars max)

- Bullet point of specific change
- Another change
- Bug fix if any
- Link to issue if applicable

Closes #issue-number (if applicable)
```

**Examples:**
```
Phase 1: Added SimplePlanner baseline evaluation

Phase 2: Implemented FAISS vector database for embeddings

Phase 3: Integrated GPT-2 with retrieval mechanism

Bug fix: Corrected scenario loading path in data_loader.py

Docs: Updated README with new installation steps
```

---

#### Step 6: Push Your Branch

```bash
# Push to GitHub
git push origin yourname/phase-description

# Example:
# git push origin karina/baseline-metrics
```

---

#### Step 7: Create Pull Request (PR)

1. **Go to GitHub repository** in web browser
2. Click **"Pull requests"** tab
3. Click **"New pull request"** button
4. **Select branches:**
   - Base: `main`
   - Compare: `yourname/phase-description`
5. **Fill in PR details:**
   - Title: Brief description
   - Description: What you did, why, any notes
6. **Tag reviewers:** Select 1-2 teammates
7. Click **"Create pull request"**

**PR Template:**
```markdown
## Phase [X]: [Phase Name]

### What was done
- Implemented X
- Added Y
- Fixed Z

### Files changed
- `notebooks/0X_phase.ipynb`
- `src/new_module.py`
- `results/metrics.csv`

### Testing
- [ ] Notebook runs without errors
- [ ] Results look correct
- [ ] Code is commented

### Notes
Any additional context, questions, or concerns.

### Reviewers
@teammate1 @teammate2
```

---

#### Step 8: Review & Merge

**For Reviewers:**
1. Check out the branch locally (optional)
2. Review code changes on GitHub
3. Leave comments if needed
4. Click **"Approve"** when satisfied

**For Author (after approval):**
1. Click **"Merge pull request"**
2. Click **"Confirm merge"**
3. Delete branch (optional): Click **"Delete branch"**

---

### Quick Commands Reference

```bash
# See current branch
git branch

# Switch to existing branch
git checkout branch-name

# Switch to main
git checkout main

# Pull latest changes
git pull origin main

# See commit history
git log --oneline

# See last 5 commits
git log --oneline -5

# Undo uncommitted changes to a file
git checkout -- filename

# See what's staged
git diff --staged

# Unstage a file
git reset HEAD filename
```

---

### Handling Merge Conflicts

**When merge conflict occurs:**

```bash
# Pull latest changes
git pull origin main

# Git will show conflict in file
# Open the file and look for:
<<<<<<< HEAD
Your changes
=======
Their changes
>>>>>>> main

# Choose which version to keep, or combine them
# Remove the conflict markers (<<<, ===, >>>)

# After resolving:
git add resolved-file.txt
git commit -m "Resolved merge conflict in resolved-file.txt"
```

**For Jupyter Notebooks (harder to merge):**

```bash
# Option 1: Keep your version
git checkout --ours notebooks/file.ipynb
git add notebooks/file.ipynb

# Option 2: Keep their version
git checkout --theirs notebooks/file.ipynb
git add notebooks/file.ipynb

# Complete the merge
git commit -m "Resolved merge conflict - kept [ours/theirs] version"
```

**🎯 Best Practice:** Always pull before starting work to avoid conflicts!

---

## 🐛 Troubleshooting

### Issue 1: Environment Variables Not Set

**Symptom:**
```python
print(os.getenv('NUPLAN_DATA_ROOT'))
# Output: None
```

**Solution:**
```bash
# Check which shell you're using
echo $SHELL

# If /bin/zsh:
source ~/.zshrc

# If /bin/bash:
source ~/.bashrc

# If still not working, manually add again:
echo 'export NUPLAN_DATA_ROOT="$HOME/nuplan/dataset"' >> ~/.zshrc
source ~/.zshrc

# Verify
echo $NUPLAN_DATA_ROOT
```

---

### Issue 2: Module Not Found Errors

**Symptom:**
```python
ModuleNotFoundError: No module named 'torch'
```

**Solution:**
```bash
# Ensure environment is activated
conda activate nuplan

# Check which Python is being used
which python
# Should show: /path/to/miniconda3/envs/nuplan/bin/python

# Reinstall problematic package
pip install torch==1.9.0 torchvision==0.10.0

# If still issues, reinstall all dependencies
pip install -r requirements.txt
```

---

### Issue 3: Database Files Not Found

**Symptom:**
```python
FileNotFoundError: No such file or directory: '.../splits/mini/*.db'
```

**Solution:**
```bash
# Check if files exist
ls ~/nuplan/dataset/nuplan-v1.1/splits/mini/

# If empty, database files are in wrong location
# Re-extract:
cd ~/Downloads
mv ~/nuplan/dataset/data/cache/mini/*.db ~/nuplan/dataset/nuplan-v1.1/splits/mini/

# Verify
ls ~/nuplan/dataset/nuplan-v1.1/splits/mini/ | wc -l
# Should show: 64
```

---

### Issue 4: Jupyter Kernel Crashes

**Symptom:** Kernel dies when running cells

**Solution:**
```bash
# Stop Jupyter (Ctrl+C in terminal)

# Reinstall kernel
conda activate nuplan
conda install ipykernel
python -m ipykernel install --user --name nuplan --display-name "Python (nuplan)"

# Restart Jupyter
jupyter lab

# In notebook, click Kernel > Change Kernel > Python (nuplan)
```

---

### Issue 5: Git Push Rejected

**Symptom:**
```
! [rejected] main -> main (fetch first)
error: failed to push some refs
```

**Solution:**
```bash
# Someone else pushed changes
# Pull their changes first
git pull origin main

# If merge conflict, resolve it
# Then push again
git push origin main
```

---

### Issue 6: Can't Activate Conda Environment

**Symptom:**
```bash
conda activate nuplan
# Command 'conda' not found
```

**Solution:**
```bash
# Conda not in PATH
# Add to shell config:

# For zsh:
echo 'export PATH="$HOME/miniconda3/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc

# For bash:
echo 'export PATH="$HOME/miniconda3/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc

# Verify
conda --version
```

---

## 📊 Team Collaboration Best Practices

### Daily Workflow

**Morning (Before starting work):**
```bash
cd ~/memory-rag-autonomous-driving
git pull origin main
conda activate nuplan
```

**During work:**
- Commit small changes frequently
- Push your branch regularly
- Test your code before pushing

**Evening (After work session):**
```bash
git add [files]
git commit -m "Descriptive message"
git push origin yourname/feature
```

**Sunday (Team meeting):**
- Review PRs together
- Merge approved work
- Plan next week's tasks

---

### Communication Guidelines

**Use Discord/Slack for:**
- Quick questions
- Status updates
- Coordinating who works on what
- Scheduling meetings

**Use GitHub Issues for:**
- Bug reports
- Feature requests
- Task tracking
- Discussion of implementation details

**Use Pull Requests for:**
- Code review
- Detailed feedback on implementation
- Approval before merging

---

### Code Review Checklist

**Before creating PR:**
- [ ] Code runs without errors
- [ ] Notebook cells executed in order
- [ ] Results make sense
- [ ] Code is commented
- [ ] No dataset files included
- [ ] Commit messages are clear

**When reviewing teammate's PR:**
- [ ] Read the description
- [ ] Check changed files
- [ ] Verify logic is correct
- [ ] Test code if possible
- [ ] Leave constructive comments
- [ ] Approve or request changes

---

## 📞 Getting Help

### If You're Stuck

1. **Check this guide** - especially Troubleshooting section
2. **Check error message** - Google the exact error
3. **Ask in team chat** - Someone may have faced same issue
4. **Check documentation:**
   - nuPlan: https://nuplan-devkit.readthedocs.io/
   - PyTorch: https://pytorch.org/docs/
   - Git: https://git-scm.com/doc
5. **Create GitHub Issue** - Describe problem with error message and steps to reproduce

---

## 🎓 Useful Keyboard Shortcuts

### Jupyter Lab

| Shortcut | Action |
|----------|--------|
| `Shift + Enter` | Run cell and move to next |
| `Ctrl + Enter` | Run cell and stay |
| `A` | Insert cell above |
| `B` | Insert cell below |
| `DD` | Delete cell |
| `M` | Change to Markdown |
| `Y` | Change to Code |
| `Cmd/Ctrl + S` | Save notebook |
| `Cmd/Ctrl + Shift + C` | Open command palette |

### Terminal

| Shortcut | Action |
|----------|--------|
| `Ctrl + C` | Stop running process |
| `Ctrl + L` | Clear screen |
| `Ctrl + A` | Go to beginning of line |
| `Ctrl + E` | Go to end of line |
| `↑` / `↓` | Previous/next command |
| `Tab` | Auto-complete |

---

## ✅ Quick Checklist

### Before First Work Session
- [ ] Install Miniconda
- [ ] Clone nuplan-devkit
- [ ] Create conda environment (`conda create -n nuplan python=3.9`)
- [ ] Install all dependencies (7 pip install commands)
- [ ] Set environment variables
- [ ] Download dataset (8.5 GB)
- [ ] Extract dataset correctly
- [ ] Clone project repository
- [ ] Test: Open Jupyter and verify environment

### Before Each Work Session
- [ ] `cd ~/memory-rag-autonomous-driving`
- [ ] `git pull origin main`
- [ ] `conda activate nuplan`
- [ ] Create/checkout your branch
- [ ] `jupyter lab`

### After Each Work Session
- [ ] Save all notebooks (`Cmd+S`)
- [ ] `git status` (check changes)
- [ ] `git add` your files
- [ ] `git commit -m "descriptive message"`
- [ ] `git push origin yourname/feature`
- [ ] Create PR on GitHub (if work complete)

---

## 📚 Additional Resources

### Documentation
- **nuPlan Devkit:** https://nuplan-devkit.readthedocs.io/
- **nuPlan Dataset:** https://www.nuscenes.org/nuplan
- **Git Tutorial:** https://git-scm.com/book/en/v2
- **Jupyter Lab:** https://jupyterlab.readthedocs.io/

### Tools
- **GitHub Desktop:** https://desktop.github.com/ (GUI for Git)
- **VS Code:** https://code.visualstudio.com/ (Alternative editor)
- **GitKraken:** https://www.gitkraken.com/ (Git visualization)

### Python Libraries
- **PyTorch:** https://pytorch.org/docs/1.9.0/
- **FAISS:** https://github.com/facebookresearch/faiss
- **Transformers:** https://huggingface.co/docs/transformers/
- **Pandas:** https://pandas.pydata.org/docs/
- **Matplotlib:** https://matplotlib.org/stable/

---

## 📧 Contact Information

**Team Communication:**
- Discord/Slack: [Your team channel]
- GitHub: https://github.com/YOUR_USERNAME/memory-rag-autonomous-driving
- Email: Use ASU emails for formal communication

**Office Hours:**
- Instructor: [Office hours info]
- TAs: [TA office hours]

---

## 🎯 Project Goals Reminder

**Our Goal:** Implement a Memory-Augmented RAG system that enables continual adaptation to long-tail autonomous driving scenarios.

**Key Components:**
1. **Baseline:** SimplePlanner evaluation
2. **RAG:** Retrieval-Augmented Generation with GPT-2
3. **Memory:** FAISS vector database with continual updates
4. **Evaluation:** Compare baseline vs RAG vs Memory-RAG

**Success Criteria:**
- Memory-augmented system outperforms baseline
- Demonstrated adaptation to rare scenarios
- Complete report with results and analysis

**Deadline:** November 30, 2025

---

**Last Updated:** November 20, 2025  
**Version:** 1.0  
**Authors:** Team Members

---

# 🎉 You're All Set!

Follow this guide step-by-step and you'll be contributing to the project in no time!

**Questions?** Ask in team chat or create a GitHub issue.

**Good luck! 🚀**
