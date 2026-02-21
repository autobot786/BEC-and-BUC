# BEC and BUC: Business Email Compromise Incident Response Playbooks

[![Language](https://img.shields.io/badge/Language-Jupyter%20Notebook-orange.svg)](https://jupyter.org/)
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

> **Comprehensive incident response playbooks and interactive Jupyter notebooks for handling Business Email Compromise (BEC) and Business User Compromise (BUC) security incidents.**

---

##  Overview

This repository provides **ready-to-use incident response playbooks** for security teams dealing with Business Email Compromise attacks. It includes:

 **Step-by-step playbooks** for different BEC scenarios  
 **Interactive Jupyter notebooks** for guided incident investigations  
 **Templates** for interviews, reports, and containment checklists  
 **Pre-built queries** for Microsoft 365 and Google Workspace  
 **Automated report generation** from incident data  

---

##  What's Included

### 1️ Email Compromise (EC) Playbook
**Location:** `bec-email-compromise-playbook/`

Handles incidents where a **single user's email account** is compromised.

**Components:**
-  `playbook.md` — Full step-by-step IEC playbook
-  `notebooks/` — Interactive Jupyter notebook for guided incident response
-  `templates/` — Markdown templates for:
  - User interviews
  - Incident reports
  - Containment checklists
-  `queries/` — Ready-to-use queries for:
  - Microsoft 365 (Exchange Online, Azure AD)
  - Google Workspace
-  `outputs/` — Generated incident artifacts and reports

**Use Cases:**
- Compromised employee email accounts
- Phishing-based account takeovers
- Credential stuffing attacks
- OAuth token abuse

---

### 2️ Supply Chain / TPRM BEC Playbook
**Location:** `bec-tprm/`

Handles incidents involving **third-party vendor or supplier compromise** (Third-Party Risk Management).

**Components:**
- `playbooks/BEC_SupplyChain_TPRM_Playbook.md` — Full TPRM playbook
- `BEC_SupplyChain_TPRM_Playbook.ipynb` — Interactive investigation notebook
- `playbooks/templates/BEC_TPRM_Templates.md` — Scripts and checklists for:
  - Vendor communication
  - Finance/AP holds
  - Legal coordination
-  `.github/ISSUE_TEMPLATE/bec-supply-chain-tprm.yml` — GitHub issue template for incident tracking

**Use Cases:**
- Vendor email account compromise
- Fraudulent invoice/payment requests
- Supply chain fraud
- Business partner impersonation

---

##  Getting Started

### Prerequisites
- **Python 3.8+**
- **Jupyter Lab** or **VS Code** with Jupyter extension
- Access to email logs (Microsoft 365, Google Workspace, etc.)

### Installation

1. **Clone the repository:**
   ```bash
   git clone https://github.com/autobot786/BEC-and-BUC.git
   cd BEC-and-BUC
   ```

2. **Install Jupyter (if not already installed):**
   ```bash
   pip install jupyter jupyterlab pandas
   ```

3. **Launch Jupyter:**
   ```bash
   jupyter lab
   ```

---

##  How to Use

### For Email Compromise:

1. Navigate to `bec-individual-email-compromise-playbook/notebooks/`
2. Open `BEC_Individual_Email_Compromise_Playbook.ipynb`
3. Edit the **incident context cell** with your case details
4. Run cells **top-to-bottom** to:
   - Execute investigation steps
   - Collect evidence
   - Generate incident report
5. Export generated report from `outputs/`

### For Supply Chain / TPRM Incidents:

1. Open `bec-tprm/BEC_SupplyChain_TPRM_Playbook.ipynb`
2. Follow the guided workflow for:
   - Vendor verification
   - Payment holds
   - Stakeholder communication
   - Legal/compliance coordination
3. Use templates in `playbooks/templates/` for consistent documentation

---

##  Integration with Your Security Workflow

### Option 1: Standalone Use
- Clone this repo and run playbooks directly
- Great for ad-hoc incident response

### Option 2: Add to Existing IR Repository
```bash
# Copy into your security runbooks repo
cp -r bec-individual-email-compromise-playbook /path/to/your/ir-repo/playbooks/
cp -r bec-tprm /path/to/your/ir-repo/playbooks/
```

### Option 3: GitHub Integration
1. Enable **GitHub Issues** in your repo
2. Use the included issue template: `.github/ISSUE_TEMPLATE/bec-supply-chain-tprm.yml`
3. Add **CODEOWNERS** to protect playbooks:
   ```
   /playbooks/ @security-ir-team @finance-lead
   ```

---

##  Sample Queries Included

### Microsoft 365
- Mailbox audit log extraction
- Azure AD sign-in analysis
- OAuth app permissions review
- Email forwarding rule detection

### Google Workspace
- Gmail log export
- OAuth token enumeration
- Suspicious forwarding filters
- Admin audit log queries

---

##  Security & Privacy

 **Important Notes:**

- **Outputs folder:** Contains generated incident data. Consider adding `outputs/` to `.gitignore` if you don't want to commit sensitive incident files.
- **Credentials:** Never commit credentials, API keys, or PII to this repository.
- **Access control:** Restrict repository access to authorized IR team members only.

```bash
# Add to .gitignore (optional)
echo "outputs/" >> .gitignore
echo "*.log" >> .gitignore
echo "*_incident_*.md" >> .gitignore
```

---

##  Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/improvement`)
3. Commit your changes (`git commit -m 'Add new query template'`)
4. Push to the branch (`git push origin feature/improvement`)
5. Open a Pull Request

---

##  License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

##  Support

For questions or issues:
- Open a [GitHub Issue](https://github.com/autobot786/BEC-and-BUC/issues)
- Contact: [@autobot786](https://github.com/autobot786)

---

##  Acknowledgments

This playbook collection is designed for security professionals responding to real-world BEC incidents. It combines industry best practices with practical, executable workflows.

**Stay vigilant. Respond rapidly. Protect your organization.** 

---

**Last Updated:** 2026-02-10 15:24:48
