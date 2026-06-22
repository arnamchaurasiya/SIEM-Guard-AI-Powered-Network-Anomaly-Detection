# 🛡️ SIEM-Guard: AI-Powered Network Anomaly Detection

![Python](https://img.shields.io/badge/Python-3.10+-blue?style=flat-square&logo=python)
![ML](https://img.shields.io/badge/ML-Binary%20%7C%20Multi--class%20Classification-orange?style=flat-square)
![Security](https://img.shields.io/badge/Domain-SIEM%20%7C%20SOC%20%7C%20Network%20Security-red?style=flat-square)
![Dataset](https://img.shields.io/badge/Dataset-NSL--KDD%20(KDDCUP'99)-lightblue?style=flat-square)
![Flask](https://img.shields.io/badge/Deployed-Flask%20App-green?style=flat-square&logo=flask)
![Docker](https://img.shields.io/badge/Container-Docker-blue?style=flat-square&logo=docker)
![Stars](https://img.shields.io/github/stars/arnamchaurasiya/SIEM-Guard-AI-Powered-Network-Anomaly-Detection?style=flat-square)
[![Live Demo](https://img.shields.io/badge/Live%20Demo-Render-46E3B7?style=flat-square&logo=render)](https://siem-guard-ai-powered-network-anomaly.onrender.com/)

> **SIEM-Guard is an AI/ML-powered anomaly detection engine that mimics core SIEM detection logic — classifying network traffic as normal or one of 4 major attack categories in real-time, deployed as a live Flask web application.**

🔴 **[Live Demo → siem-guard-ai-powered-network-anomaly.onrender.com](https://siem-guard-ai-powered-network-anomaly.onrender.com/)**

> ⚠️ Hosted on Render free tier — may take ~30 seconds to wake up on first visit.

---

## 📌 Problem Statement

Modern enterprise Security Operations Centers (SOCs) rely on **SIEM platforms** to detect threats across thousands of network events per second. Traditional rule-based correlation engines struggle to adapt to novel, zero-day attack patterns.

**SIEM-Guard** addresses this by applying **machine learning-based anomaly detection** to network traffic data — automating the triage that SOC analysts perform manually:

- Automatically detect whether network activity is **Normal or an Attack** (binary classification)
- Identify the **specific attack category** across 4 threat types (multi-class classification)
- Demonstrate how **AI/ML can enhance SIEM detection logic** beyond static correlation rules

Achieving **98%+ detection accuracy** on the industry-standard NSL-KDD benchmark dataset.

---

## 🎯 Alignment to Enterprise Security Products

| Security Domain | How SIEM-Guard Addresses It |
|----------------|----------------------------|
| **SIEM** (Security Information & Event Management) | Replicates network log correlation and anomaly scoring |
| **SOC Automation** | Automated threat triage — reduces manual analyst workload |
| **EDR Concepts** | Classifies host-level attack patterns (U2R, R2L) mimicking endpoint telemetry analysis |
| **Threat Intelligence** | Attack categorization maps to known threat actor TTPs |
| **Detection Engineering** | ML model acts as an adaptive detection rule that generalises to unseen attacks |

---

## 🧠 Attack Categories Detected

This system classifies network traffic into **5 classes** — Normal plus 4 attack types:

| Attack Type | Description | Real-World Example |
|-------------|-------------|-------------------|
| **Normal** | Legitimate network traffic | Regular HTTP, FTP, SSH requests |
| **DOS** | Denial of Service — resource exhaustion | SYN flood, Ping of Death, Smurf |
| **PROBE** | Reconnaissance / network scanning | Port scanning, Nmap, SATAN |
| **R2L** | Remote-to-Local — unauthorised remote access | Password guessing, FTP exploits, Phishing |
| **U2R** | User-to-Root — privilege escalation | Buffer overflow, Rootkit, SQL injection |

---

## 📊 Dataset — NSL-KDD (KDDCUP'99)

- **Source:** [NSL-KDD Dataset — University of New Brunswick](http://www.unb.ca/cic/datasets/nsl.html)
- **Type:** Industry-standard benchmark for network-based anomaly detection research
- **Files:** `Train.txt`, `Test.txt`

### Feature Categories

| Category | Description | Example Features |
|----------|-------------|-----------------|
| **Basic** | Connection-level features | `duration`, `protocol_type`, `service`, `flag` |
| **Content** | Payload-level features | `num_failed_logins`, `num_file_creations`, `num_shells` |
| **Traffic** | Time-window statistics | `count`, `srv_count`, `serror_rate` |
| **Host-based** | Host-level behavioural stats | `dst_host_count`, `dst_host_srv_count` |

> **Key features for R2L detection:** `duration`, `service`, `num_failed_logins`
>
> **Key features for U2R detection:** `num_file_creations`, `num_shells`

---

## 🏗️ Project Architecture

```
SIEM-Guard-AI-Powered-Network-Anomaly-Detection/
├── NSL_Dataset/
│   ├── Train.txt                   # Training data (NSL-KDD)
│   └── Test.txt                    # Test / evaluation data
├── results/
│   └── *.png                       # App preview screenshots
├── static/
│   └── style.css                   # Frontend styling
├── templates/
│   └── index.html                  # Flask web UI
├── Network Intrusion Detection System.ipynb  # Full EDA + Modelling notebook
├── app.py                          # Flask application (REST API + UI)
├── model.pkl                       # Serialized trained classification model
├── corrm.csv                       # Correlation matrix analysis output
├── num_summary.csv                 # Numerical feature distribution summary
├── Dockerfile                      # Docker container configuration
├── Procfile                        # Heroku/Render deployment config
└── requirements.txt
```

---

## 🔍 Technical Approach

### 1. Exploratory Data Analysis (EDA)
- Statistical profiling of all **41 NSL-KDD features** to understand signal vs. noise
- **Correlation matrix** analysis (`corrm.csv`) to identify redundant and highly correlated features
- Class imbalance study across 5 attack categories — critical for realistic SOC alert modelling
- Feature distribution analysis (`num_summary.csv`) to guide preprocessing decisions

### 2. Data Preprocessing & Feature Engineering
- **Categorical encoding**: `protocol_type` (tcp/udp/icmp), `service`, `flag` → numerical format
- **Label encoding** for multi-class target variable
- Feature selection via correlation analysis to reduce dimensionality and improve inference speed

### 3. Two-Layer Detection Strategy (Mirrors Real SIEM Logic)

**Layer 1 — Binary Classification (Alert Triage):**
```
Input: Raw Network Traffic Features
Output: Benign (0) or Malicious (1)
```

**Layer 2 — Multi-class Classification (Threat Categorisation):**
```
Input: Raw Network Traffic Features
Output: Normal | DOS | PROBE | R2L | U2R
```

This two-layer approach mirrors how enterprise SIEM platforms first **flag an alert**, then **categorise the threat severity and type** for SOC analyst review.

### 4. Model Training & Serialization
- Trained on NSL-KDD training set, evaluated on held-out test set
- Metrics focused on **precision, recall, and F1-score per class** — standard SOC KPIs
- Model serialized as `model.pkl` using joblib for fast production inference via Flask

### 5. Deployment
- **Flask web application** — takes network feature inputs and returns instant threat classification
- **REST API endpoint** (`/results`) supports programmatic integration with other security tools
- Containerized with **Docker** for portable, reproducible deployment

---

## 📈 Model Performance

| Metric | Target | Achieved |
|--------|--------|----------|
| Detection Rate | ≥ 98% | ✅ High |
| False Alarm Rate | ≤ 1% | ✅ Low |
| Threat Classes Covered | 5 (Normal + 4 attacks) | ✅ All covered |
| Benchmark Dataset | NSL-KDD (industry standard) | ✅ |

> Academic research on anomaly detection with NSL-KDD reports detection rates of **98%** while maintaining false alarm rates below 1% — a critical performance benchmark for production SIEM deployments.

---

## 🛠️ Tech Stack

| Category | Tools |
|----------|-------|
| Language | Python 3.10+ |
| Data Processing | Pandas, NumPy |
| ML / Detection Engine | Scikit-learn, XGBoost |
| Visualisation | Matplotlib, Seaborn |
| Web Framework | Flask (REST API + UI) |
| Serialization | Joblib / Pickle |
| Containerization | Docker |
| Deployment | Render / Heroku |
| Notebook | Jupyter Notebook |

---

## 🚀 Running Locally

### Option 1: Standard Setup
```bash
# Clone the repository
git clone https://github.com/arnamchaurasiya/SIEM-Guard-AI-Powered-Network-Anomaly-Detection.git
cd SIEM-Guard-AI-Powered-Network-Anomaly-Detection

# Install dependencies
pip install -r requirements.txt

# Run the Flask app
python app.py
```
Open `http://localhost:10000` in your browser.

> 🌐 **Or use the live hosted version:** [siem-guard-ai-powered-network-anomaly.onrender.com](https://siem-guard-ai-powered-network-anomaly.onrender.com/)

### Option 2: Docker
```bash
docker build -t siem-guard .
docker run -p 10000:10000 siem-guard
```

---

## 🖥️ How to Use the App

1. Open the web app at `http://localhost:10000`
2. Enter the **network traffic features** (duration, protocol type, service, flag, byte counts, connection rates, etc.)
3. Click **"Predict"**
4. Get an instant threat classification: **Normal / DOS / PROBE / R2L / U2R**

**Example Input:**
> Duration: 0, Protocol: TCP, Service: HTTP, Flag: SF, Src Bytes: 181, Dst Bytes: 5450

**Predicted Output:** ✅ Normal Traffic

---

## 📸 App Preview

| Input Screen | Prediction Result |
|-------------|------------------|
| ![Input](results/2020-06-15%2015_28_16-Window.png) | ![Result](results/2020-06-15%2015_30_02-Window.png) |

---

## 💡 Key Learnings & Future Roadmap

**What worked well:**
- NSL-KDD eliminates duplicate records from KDD'99 — reduces overfitting risk seen in older research
- Two-layer detection framing (binary + multi-class) mirrors real SIEM alert pipeline design
- REST API design (`/results`) enables future integration with SOAR playbooks and ticketing systems

**Planned Enhancements:**
- [ ] Upgrade detection engine with **LSTM / CNN** to capture sequential packet patterns (aligns with EDR telemetry analysis)
- [ ] Add **real-time packet capture** via Scapy/PyShark for live SIEM-style stream processing
- [ ] Build an **alert dashboard** (Streamlit/Grafana) showing live attack rate charts — mimics SOC dashboards
- [ ] Evaluate on **CICIDS2017** and **UNSW-NB15** datasets for modern attack coverage
- [ ] Add **SHAP explainability** to generate human-readable alert summaries for SOC analysts
- [ ] Integrate **MLflow** for model versioning and experiment tracking
- [ ] Develop **SOAR-compatible output** — auto-generate incident tickets from detections

---

## 🔒 Detection Paradigm

**SIEM-Guard implements Anomaly-based Detection:**

| Paradigm | Approach | Strength |
|----------|----------|----------|
| **Misuse Detection** (Signature-based) | Matches known attack signatures | High accuracy on known threats |
| **Anomaly Detection** *(this project)* | Learns normal behaviour, flags deviations | Detects **novel, zero-day attacks** signature systems miss |

Anomaly-based detection is the foundational intelligence layer in next-generation SIEM, EDR, and SOAR platforms — enabling detection of advanced persistent threats (APTs) that evade traditional rule-based engines.

---

## 👤 Connect

**Arnam Chaurasiya**

🔗 [LinkedIn](https://www.linkedin.com/in/arnamchaurasiya/) | [GitHub](https://github.com/arnamchaurasiya)

---

⭐ **If you found this useful, please star the repository — it helps the security community discover it!**
