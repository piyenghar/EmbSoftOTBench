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
- Adding **unique identifiers** for every entry to support reproducibility.

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
  - *(Hardware excluded in this release — focus = embedded software risks)*

- **Variants**:
  - **Threat**: Unmitigated baseline (default exposure & vulnerability).
  - **Benign**: Mitigated counterpart where **Exposure and Vulnerability are set to 1**, lowering likelihood and risk.

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

   ```text
   Likelihood = min(Exposure + Vulnerability - 1, 5)


## Risk

Two fields are provided:

- **`risk_raw`**: numeric intermediate = Impact × Likelihood  
- **`risk`**: final categorical risk (1–5) from a risk matrix:  
  - Risk 1 = Low  
  - Risk 2 = Medium  
  - Risk 3 = Significant  
  - Risk 4 = High  
  - Risk 5 = Very High  

---

## 📂 Dataset Files (v1.0)

The dataset is organized into four JSON files:

- **`embsoftotbench-plc-v1.0.json`**  
  Threat/benign entries specific to **Programmable Logic Controllers (PLCs)**.  
  Total: 128 entries.  

- **`embsoftotbench-hmi-v1.0.json`**  
  Threat/benign entries specific to **Human-Machine Interfaces (HMIs)**.  
  Total: 124 entries.  

- **`embsoftotbench-drive-v1.0.json`**  
  Threat/benign entries specific to **Industrial Drives (motor controllers)**.  
  Total: 122 entries.  

- **`embsoftotbench-all-v1.0.json`**  
  Aggregated file combining **all three device types** above.  
  Total: 374 entries.  

Each entry in these files contains:  
- `variant`: threat vs. benign  
- `device_type`: PLC, HMI, or Drive  
- `emb3d_tid`: EMB3D threat ID  
- `impact`, `exposure`, `vulnerability`, `likelihood`, `risk_raw`, `risk`  
- `rationale`: factor-by-factor explanation  
- `evidence`: CWE/CVE links if available  
- `source_meta`: EMB3D STIX reference  
- `uid`: dataset-specific unique identifier  

## 📊 Usage & Reproducibility

### Dataset Use
- Suitable for evaluating **LLM reasoning** about OT software risks.  
- Enables controlled experiments with **threat vs. benign** pairs.  
- Provides **factor-level rationales** for interpretability.  

### Experimental Results
Logs and outputs from evaluation experiments with LLMs are included for transparency.  

- Location: [`/experiments/results/`](./experiments/results)  
- Contents:  
  - Raw model outputs (per run, with run IDs)  
  - Processed logs (factor-wise comparisons, risk scores, match statistics)  
  - Summary tables and figures  

## Citation

If you use **EmbSoftOTBench** in your research, please cite the following publication:

**Padma Iyenghar**,  
*Empirical Evaluation of AI-Assisted Risk Reasoning for ICPS Software Security*,  
IEEE Transactions on Industrial Cyber-Physical Systems, 2026.  
DOI: https://doi.org/10.1109/TICPS.2026.3669084

```bibtex
@article{Iyenghar2026_TICPS_AI_Risk_Reasoning,
  author    = {Iyenghar, Padma},
  title     = {Empirical Evaluation of AI-Assisted Risk Reasoning for ICPS Software Security},
  journal   = {IEEE Transactions on Industrial Cyber-Physical Systems},
  year      = {2026},
  publisher = {IEEE},
  doi       = {10.1109/TICPS.2026.3669084},
  keywords  = {Industrial Cyber-Physical Systems (ICPS), Operational Technology (OT) security,
               AI-empowered ICPS, trustworthy and reliable ICPS, LLM benchmarking,
               risk calibration, IEC 62443-3-2, MITRE EMB3D}
}
```
### 📜 License

This dataset is released under the **Creative Commons Attribution 4.0 International (CC BY 4.0)** license.  
See the [LICENSE](./LICENSE) file for details.  

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
  },
  "uid": "PLC-threat-TID-201-0000"
}


