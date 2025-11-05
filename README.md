# 🚀 DevOps-IaC-Foundation: Databricks Dynamic List Fix

**The core implementation was provided and diagnosed by Leandro Figueiredo (SQLTuner).**

This repository provides a robust, production-ready Databricks Asset Bundle (DAB) template demonstrating fundamental **Infrastructure as Code (IaC)** principles. Its key feature is the **definitive fix** for a common and subtle YAML data type conflict, ensuring **dynamic, environment-specific list substitution** (e.g., configuring different email notification lists for `dev` vs. `prod` targets).

---

## 🛑 Feature 1: The Dynamic List Substitution Fix

A central challenge in using DAB is managing arrays or lists that must vary by target environment (e.g., a list of emails or database names). The standard YAML interpolation often fails, leading to nested lists that are rejected by the Databricks Jobs API.

### **The Solution (The IaC Best Practice)**

The fix is based on correctly defining the variable as a **YAML List Array** in `databricks.yml` and then using a **direct assignment** in the resource file, which forces the substitution engine to flatten the list into the API's required format.

**Configuration Example (`resources/job_notifications.yml`):**
```yaml
# 1. Variable defined as a YAML list in databricks.yml targets
# 2. Variable is assigned directly to the field, eliminating the nested list issue.
email_notifications:
  on_failure: ${var.email_dev_team}        # Resolves to a flat list in both dev/prod
  on_success: ${var.email_stakeholders}    # Empty list in 'dev', full list in 'prod'
```
