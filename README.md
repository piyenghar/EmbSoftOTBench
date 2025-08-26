# EmbSoftOTBench
Benchmark dataset for evaluating LLMs in embedded software risk reasoning for OT systems.

# EmbSoftOTBench v1.0
*A Benchmark Dataset for Evaluating Large Language Models in Embedded Software Risk Reasoning for Operational Technology Systems*

---

## 🔍 Motivation
Operational Technology (OT) systems such as **Programmable Logic Controllers (PLCs)**, **Human-Machine Interfaces (HMIs)**, and **Motor Drives** form the backbone of industrial automation. These devices increasingly rely on embedded software, which exposes them to cybersecurity threats.

While MITRE’s [EMB3D](https://emb3d.mitre.org/) framework provides a curated knowledge base of embedded threats, there has been **no structured benchmark dataset** for evaluating how well **Large Language Models (LLMs)** can reason about OT software risks.

**EmbSoftOTBench v1.0** fills this gap by:
- Mapping EMB3D threats to representative OT device types (PLC, HMI, Drive).
- Providing **scored scenarios** with risk assessments.
- Supplying **human-readable rationales** for each factor, ensuring transparency.
- Offering **threat vs. benign variants** to test model sensitivity to mitigations.

---

## 📦 Dataset Scope
- **Devices Covered**:
  - **PLC** (Programmable Logic Controller)
  - **HMI** (Human-Machine Interface, typically Web-based)
  - **Drive** (Motor controllers / industrial drives)

- **Threat Categories** (from EMB3D):
  - System Software
  - Application Software
  - Networking

- **Variants**:
  - **Threat**: Unmitigated baseline (default exposure & vulnerability).
  - **Benign**: Mitigated scenario (exposure & vulnerability reduced to 1).

- **Total Entries**:
  - PLC: *128 scenarios*
  - HMI: *124 scenarios*
  - Drive: *122 scenarios*
  - Combined (all): *374 scenarios*

---

## ⚙️ Scoring Methodology
Each entry is scored across **five key factors**:

1. **Impact (1–4)**  
   - Expert prior, device-role specific (e.g., PLC = 4, HMI = 3, Drive = 4).
   - Reflects consequence of compromise.

2. **Exposure (1–3)**  
   - Logical/physical access required.  
   - 1 = highly restricted, 2 = restricted, 3 = easily accessible.

3. **Vulnerability (1–3)**  
   - Exploitability of the software/system.  
   - 1 = difficult, 2 = medium, 3 = trivial.

4. **Likelihood (1–5)**  
   - **Derived** from exposure + vulnerability:  
     ```
     Likelihood = min(Exposure + Vulnerability - 1, 5)
     ```

5. **Risk (1–5)**  
   - Computed from **Impact × Likelihood** using a risk matrix:  
     - Risk 1 = Low  
     - Risk 2 = Medium  
     - Risk 3 = Significant  
     - Risk 4 = High  
     - Risk 5 = Very High  

---

## 📑 Example Entry
```json
{
  "variant": "threat",
  "device_type": "PLC",
  "emb3d_tid": "TID-201",
  "emb3d_name": "Inadequate Bootloader Protection and Verification",
  "emb3d_category": "system software",
  "emb3d_maturity": "observed adversarial technique",
  "impact": 4,
  "exposure": 2,
  "vulnerability": 2,
  "likelihood": 3,
  "risk_raw": 12,
  "risk": 3,
  "rationale": {
    "impact": "Impact = 4 (expert prior, PLC criticality).",
    "exposure": "Exposure = 2 (restricted access: engineering workstation/local).",
    "vulnerability": "Vulnerability = 2 (medium exploitability, no CWE evidence).",
    "likelihood": "Likelihood = EXP (2) + VUL (2) - 1 = 3. EMB3D maturity = observed adversarial technique (info only).",
    "risk": "Risk_raw = Impact (4) × Likelihood (3) = 12; mapped via matrix to Risk = 3."
  },
  "evidence": {
    "cwes": [],
    "cves": []
  },
  "impact_source": "expert_prior",
  "vulnerability_source": "heuristic_default",
  "exposure_source": "vector_keyword_local",
  "likelihood_source": "derived_from_exp_vul",
  "risk_source": "matrix_lookup",
  "source_meta": {
    "stix_object_id": "vulnerability--03c1db93-d257-45c7-a37d-1342f1247fc3",
    "emb3d_version": "2.0.1"
  }
}
