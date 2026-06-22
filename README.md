<div align="center">

#  Resilient Cybersecurity in Smart Grid ICS Communication

**A production-grade, multi-layered security framework for Industrial Control Systems in smart grid infrastructure — combining AES-256 encryption, ML-driven intrusion detection, and cryptographic resilience testing against real-world IEC 104 traffic.**

[![Python](https://img.shields.io/badge/Python-3.12-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?style=for-the-badge&logo=jupyter&logoColor=white)](https://jupyter.org)
[![XGBoost](https://img.shields.io/badge/XGBoost-ML%20Engine-FF6600?style=for-the-badge)](https://xgboost.readthedocs.io)
[![AES-256](https://img.shields.io/badge/AES--256--CBC-Encryption-4CAF50?style=for-the-badge&logo=lock&logoColor=white)](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)
[![IEC 104](https://img.shields.io/badge/IEC%2060870--5--104-Protocol-0078D4?style=for-the-badge)](https://en.wikipedia.org/wiki/IEC_60870-5)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)

</div>

---

##  The Problem

Modern power grids, water treatment plants, and manufacturing facilities rely on **Industrial Control Systems (ICS)** communicating over the **IEC 60870-5-104 (IEC 104)** protocol — a standard born in an era before cybersecurity was a design concern. Today, these same systems are prime targets for nation-state actors and ransomware groups.

The consequences aren't digital — they're physical: **grid blackouts**, **contaminated water supplies**, and **disrupted critical manufacturing**.

Yet most deployed ICS environments lack:
- Traffic encryption at the communication layer
- Real-time anomaly detection for protocol-level attacks
- Validated resistance to modern cryptographic attacks
- Structured incident response capabilities

This project builds exactly that — a **complete, end-to-end security stack** for smart grid ICS communication, validated on real IEC 104 network traffic data.

---

##  The Solution

A four-pillar security architecture purpose-built for ICS environments:

| Pillar | Component | Approach |
|--------|-----------|----------|
| **Encryption** | AES-256-CBC + BLAKE3 | Time-keyed dynamic encryption with scheduled key rotation |
| **Intrusion Detection** | XGBoost IDS + SMOTE | ML-powered real-time anomaly detection with class rebalancing |
| **Cryptographic Resilience** | Attack Simulation Suite | CCA, CPA, COA, KPA resistance validation |
| **Incident Response** | IR Simulation | Structured detection → containment → recovery workflows |

---

##  Key Features

###  Dynamic AES-256 Encryption Engine
- **Time-based key generation**: Keys derived from UTC timestamps (minute-granularity) — eliminating static key vulnerabilities
- **BLAKE3 key hardening**: Raw time-keys are hashed with BLAKE3 before use, preventing temporal prediction attacks
- **Automatic key rotation**: Scheduled task regenerates and replaces the encryption key every 60 seconds
- **Zero-retention policy**: Old keys purged immediately upon rotation (MAX_KEYS = 1)
- **PKCS7 padding + CBC mode**: Full block-cipher security for variable-length ICS payloads

###  XGBoost Intrusion Detection System
- **411,474 network traffic samples** across 7 classes (normal + 6 attack types)
- **SMOTE oversampling** to resolve severe class imbalance between normal and attack traffic
- **Euclidean distance validation** confirms normal traffic transformation preserves distribution integrity
- **Real-time monitoring loop**: Processes live traffic streams with per-packet prediction + confidence scoring
- **Structured logging**: Every alert timestamped and written to `ids_log.log` for audit trails

###  Multi-Model ML Benchmark Suite
Six classifiers evaluated on identical datasets for comparative analysis:

| Classifier | Key Strength |
|------------|-------------|
| XGBoost | Gradient boosted trees; handles imbalance well |
| Random Forest | Ensemble robustness; interpretable feature importance |
| CatBoost | Native categorical handling; minimal preprocessing |
| Gradient Boost | Sequential error correction; high precision |
| LightGBM | High-speed training on large datasets |
| SVM | Strong on high-dimensional feature spaces |

###  Cryptographic Attack Resistance Suite
End-to-end validation of the encryption scheme against all four classical attack classes:

- **CCA (Chosen Ciphertext Attack)**: Verified — 372,801 unique blocks, **0 repeated blocks** detected
- **CPA (Chosen Plaintext Attack)**: Pattern analysis confirms no exploitable plaintext-ciphertext correlation
- **COA (Ciphertext Only Attack)**: Block entropy analysis validates ciphertext indistinguishability
- **KPA (Known Plaintext Attack)**: Key recovery infeasibility demonstrated under known-plaintext scenarios

###  Incident Response Simulation
Full IR lifecycle modeled and tested:
- **Detection phase**: Automated anomaly identification via IDS
- **Classification**: Attack categorization (DoS, injection, scanning, switching, rogue device, connection loss)
- **Containment**: Network isolation protocols
- **Recovery**: Service restoration workflows

---

## 📁 Project Structure

```
Resilient-Cybersecurity-in-Smart-Grid-ICS-Communication/
│
├── 📂 Data
│   ├── normal-traffic.csv          # 58,930 benign IEC 104 traffic samples
│   ├── dos-attack.csv              # Denial-of-Service attack traffic
│   ├── injection-attack.csv        # Data injection attack traffic
│   ├── scanning-attack.csv         # Network scanning attack traffic
│   ├── switching-attack.csv        # Switching/relay manipulation attacks
│   ├── connection-loss.csv         # Connection disruption attack traffic
│   └── rogue-device.csv            # Rogue device impersonation traffic
│
├── 📂 Encryption & Decryption
│   ├── encryption.ipynb            # AES-256-CBC + BLAKE3 + dynamic key rotation
│   └── decryption.ipynb            # Authenticated decryption pipeline
│
├── 📂 Intrusion Detection
│   └── intruisondetection.ipynb    # XGBoost IDS with SMOTE + real-time monitor
│
├── 📂 ML Classifiers (Benchmark Suite)
│   ├── Randomforest.ipynb
│   ├── xgboostclassification.ipynb
│   ├── catboostclassification.ipynb
│   ├── gradientboostclassify.ipynb
│   ├── lightgbm classification.ipynb
│   └── svm classification.ipynb
│
├── 📂 Cryptographic Attack Simulations
│   ├── CCA.ipynb                   # Chosen Ciphertext Attack
│   ├── COA.ipynb                   # Ciphertext Only Attack
│   ├── KPA.ipynb                   # Known Plaintext Attack
│   └── Chosen Plaintext Attack (CPA).ipynb
│
└── Incident Response Simulation.ipynb
```

---

##  System Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                    SMART GRID ICS NETWORK                       │
│                  (IEC 60870-5-104 Protocol)                     │
└──────────────────────────┬──────────────────────────────────────┘
                           │  Raw ICS Traffic
                           ▼
┌─────────────────────────────────────────────────────────────────┐
│                 LAYER 1: ENCRYPTION ENGINE                      │
│                                                                 │
│   UTC Timestamp → BLAKE3(time_key) → AES-256-CBC(data, key)    │
│   ↓                                                             │
│   Scheduled Key Rotation (60s) → Encrypted ICS Payload         │
└──────────────────────────┬──────────────────────────────────────┘
                           │  Encrypted + Raw Traffic
                           ▼
┌─────────────────────────────────────────────────────────────────┐
│              LAYER 2: INTRUSION DETECTION SYSTEM                │
│                                                                 │
│   Traffic Features (16-dim) → StandardScaler → SMOTE           │
│   → XGBoostClassifier → {Benign | Intrusion} + Confidence      │
│   → Real-time alert logging + anomaly flagging                  │
└──────────────────────────┬──────────────────────────────────────┘
                           │  Alerts & Classified Events
                           ▼
┌─────────────────────────────────────────────────────────────────┐
│              LAYER 3: INCIDENT RESPONSE ENGINE                  │
│                                                                 │
│   Detection → Attack Classification → Containment → Recovery   │
└─────────────────────────────────────────────────────────────────┘
```

---

##  Dataset

This project uses the **BUT-IEC104-I** dataset from Brno University of Technology — one of the most comprehensive publicly available ICS/SCADA network traffic datasets.

| Category | Samples | Description |
|----------|---------|-------------|
| Normal Traffic | 58,930 | Legitimate IEC 104 control traffic |
| DoS Attack | — | Denial-of-service flood targeting RTUs |
| Injection Attack | — | Malicious command injection |
| Scanning Attack | — | Network reconnaissance/port scanning |
| Switching Attack | — | Unauthorized relay/breaker manipulation |
| Connection Loss | — | Deliberate communication disruption |
| Rogue Device | — | Unauthorized device impersonation |
| **Total** | **411,474** | **16 network features per sample** |

**Feature space (16 dimensions)**: Derived from IEC 104 APDU headers, TCP/IP layer metadata, inter-packet timing, and protocol state indicators.

---

##  Installation & Setup

### Prerequisites

- Python 3.10+
- Jupyter Notebook / JupyterLab
- 8 GB RAM (recommended for full dataset)

### 1. Clone the Repository

```bash
git clone https://github.com/bodapatisaikrishna/Resilient-Cybersecurity-in-Smart-Grid-ICS-Communication.git
cd Resilient-Cybersecurity-in-Smart-Grid-ICS-Communication
```

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

Or manually install core packages:

```bash
pip install pandas numpy scikit-learn xgboost catboost lightgbm imbalanced-learn \
            cryptography blake3 schedule matplotlib seaborn scipy jupyter
```

### 3. Configure Dataset Paths

Update the file paths in each notebook to point to your local CSV files:

```python
# In intruisondetection.ipynb
normal_path = "/path/to/your/normal-traffic.csv"
attack_paths = [
    "/path/to/your/dos-attack.csv",
    "/path/to/your/injection-attack.csv",
    # ... etc
]
```

### 4. Launch Jupyter

```bash
jupyter notebook
```

---

##  Usage Examples

### Run the Encryption Pipeline

```python
# encryption.ipynb — Encrypt ICS traffic data
from cryptography.hazmat.primitives.ciphers import Cipher, algorithms, modes
import blake3

# Keys auto-generated from UTC timestamp; hashed with BLAKE3
key = generate_key()            # Time-based key
hashed_key = hash_key(key)      # BLAKE3-hardened
encrypted = encrypt(data, hashed_key)  # AES-256-CBC
```

### Train and Deploy the IDS

```python
# intruisondetection.ipynb
ids = IntrusionDetectionSystem()
ids.train(normal_path, attack_paths)   # Train XGBoost on full dataset
ids.monitor_traffic(normal_path, attack_paths)  # Real-time monitoring
```

**Sample output:**
```
Packet 0 at 2025-04-06 12:33:01: Predicted Benign (Confidence: 0.97), Actual: Benign
Packet 1 at 2025-04-06 12:33:01: Predicted Intrusion (Confidence: 0.99), Actual: Intrusion
...
Total number of intrusions detected: 352544
```

### Run Cryptographic Attack Validation

```python
# CCA.ipynb — Verify encryption is CCA-resistant
=== Pattern Analysis ===
Total blocks:    372,801
Unique blocks:   372,801
Repeated blocks: 0
→ No exploitable block repetition detected. Encryption is CCA-resistant.
```

---

##  Technical Deep Dive

### Why AES-256-CBC + BLAKE3?

Standard ICS deployments use **no encryption** or outdated DES/3DES. This framework chose:

- **AES-256-CBC**: FIPS 140-2 compliant, hardware-accelerated on modern CPUs, well-understood security model
- **BLAKE3 key hashing**: Prevents adversaries from predicting keys even with partial timestamp knowledge; BLAKE3 is significantly faster than SHA-256 while maintaining cryptographic security
- **Time-based key derivation**: Limits the blast radius of key compromise to a 60-second window

### Why XGBoost for IDS?

ICS traffic anomaly detection is a textbook **imbalanced classification** problem — attacks are rare events in normal operation. XGBoost was selected because:

1. **Native handling of missing values**: ICS packet captures often have dropped or malformed fields
2. **Gradient boosting**: Iteratively focuses on misclassified samples — critical for rare attack detection
3. **Speed**: 411K samples trained in minutes, suitable for near-real-time deployment
4. **SMOTE integration**: Combined with Synthetic Minority Oversampling to eliminate bias toward the majority (normal) class

### Why 6 Attack Types?

The threat model is comprehensive: **DoS** and **scanning** represent network-layer threats; **injection** and **switching attacks** target ICS application-layer commands; **rogue device** and **connection loss** simulate insider threats and physical sabotage. Together they cover the full MITRE ATT&CK for ICS framework.

---

##  Model Performance

All six classifiers are evaluated on identical 70/30 train-test splits with the same preprocessing pipeline. Metrics tracked: **Accuracy**, **Precision**, **Recall**, **F1-Score**, and **ROC-AUC**.

>  Run individual notebooks for full classification reports, confusion matrices, and ROC curve plots.

---

##  Roadmap

- [ ] **LSTM-based IDS**: Sequential modeling of IEC 104 command sequences for temporal anomaly detection
- [ ] **Federated Learning**: Distributed IDS training across multiple substations without centralizing raw traffic
- [ ] **MQTT/DNP3 support**: Extend encryption and IDS modules to additional ICS protocols
- [ ] **Real-time dashboard**: Grafana/Kibana integration for live IDS alert visualization
- [ ] **Hardware deployment**: Port encryption engine to constrained RTU/PLC environments (ARM/FPGA targets)
- [ ] **Formal verification**: Apply model checking to incident response state machines

---

##  Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature-name`
3. Commit your changes: `git commit -m "Add: your feature description"`
4. Push to the branch: `git push origin feature/your-feature-name`
5. Open a Pull Request

Please ensure:
- All notebooks run end-to-end without errors
- New classifiers follow the existing evaluation pipeline structure
- Cryptographic changes include corresponding attack simulation validation

---

##  Acknowledgements & References

- **Dataset**: BUT-IEC104-I — Brno University of Technology ICS/SCADA dataset
- **Protocol**: IEC 60870-5-104 (Telecontrol equipment and systems)
- **Standards**: NIST SP 800-82 (Guide to ICS Security), IEC 62443 (Industrial Cybersecurity)
- **Frameworks**: MITRE ATT&CK® for ICS

---


---

<div align="center">

**Built with purpose — securing the infrastructure that powers modern life.**

*Questions, collaborations, or research discussions → [saikrishnabodapati@gmail.com](mailto:saikrishnabodapati@gmail.com)*

</div>
