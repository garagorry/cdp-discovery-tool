# CDP Environment Discovery Tool - Customer Quick Start

**Repository:** https://github.com/garagorry/cdp-discovery-tool

---

## 📥 Download & Install (Choose One)

### Option A: Git Clone (Recommended)

```bash
git clone https://github.com/garagorry/cdp-discovery-tool.git
cd cdp-discovery-tool
```

### Option B: Download ZIP (No Git)

1. Visit: https://github.com/garagorry/cdp-discovery-tool
2. Click green "Code" → "Download ZIP"
3. Extract and navigate to: `cdp-discovery-tool-main/`

---

## ⚡ Quick Run (3 Steps)

### Step 1: Verify Prerequisites

```bash
# Check Python version (must be 3.7+)
python3 --version

# Check CDP CLI is installed
cdp --version

# If CDP CLI not found, install it:
pip install cdpcli
cdp configure
```

### Step 2: Activate CDP CLI (if using virtual environment)

```bash
# Example for virtual environment:
source ~/venvs/cdpcli-beta/bin/activate

# Verify CDP CLI works:
cdp environments list-environments
```

### Step 3: Run Discovery

```bash
# Replace YOUR-ENV-NAME with your actual environment name
python3 discover.py --environment-name YOUR-ENV-NAME
```

---

## 📊 Output

Results are automatically saved to:

```
/tmp/discovery_env_<environment-name>-<timestamp>/
/tmp/discovery_env_<environment-name>-<timestamp>.tar.gz  (archive)
```

**What you get:**

- JSON files with complete API responses
- CSV files for spreadsheet analysis
- Instance group details
- Recipe scripts
- Runtime versions and upgrade candidates

---

## 🎯 Common Usage Examples

```bash
# Discover everything (default)
python3 discover.py --environment-name my-prod-env

# Discover only specific services
python3 discover.py --environment-name my-env --include-services cde cdw cai

# Use different CDP profile
python3 discover.py --environment-name my-env --profile production

# Custom output directory
python3 discover.py --environment-name my-env --output-dir /custom/path

# Debug mode (verbose output)
python3 discover.py --environment-name my-env --debug

# Exclude certain services
python3 discover.py --environment-name my-env --exclude-services cod
```

---

## 🐛 Troubleshooting

| Problem                                | Solution                                       |
| -------------------------------------- | ---------------------------------------------- |
| `cdp: command not found`               | Run: `pip install cdpcli` then `cdp configure` |
| `CDP credentials not found`            | Run: `cdp configure` and enter your keys       |
| `ImportError`                          | Use `discover.py` instead of `main.py`         |
| `Permission denied` (run_discovery.sh) | Run: `chmod +x run_discovery.sh`               |

---

## 🛟 Need Help?

```bash
# View all options
python3 discover.py --help

# Run with debug output
python3 discover.py --environment-name my-env --debug
```

**Documentation:**

- Full README: [README.md](README.md)
- Installation Guide: [INSTALL.md](INSTALL.md)
- Architecture: [ARCHITECTURE.md](ARCHITECTURE.md)
- API Reference: [API_REFERENCE.md](API_REFERENCE.md)

**Report Issues:**
https://github.com/garagorry/cdp-discovery-tool/issues

---

## 📋 System Requirements

- **Python:** 3.7 or higher
- **CDP CLI:** Configured with valid credentials
- **Network:** Access to CDP Control Plane
- **OS:** Linux, macOS, or Windows (with Git Bash/WSL)
- **Disk:** ~10 MB for tool, variable for output

---

## 🔒 Security Notes

- Discovery is **read-only** (no modifications made)
- Output contains **sensitive data** - handle securely
- Uses your existing CDP credentials
- Respects CDP API rate limits

---

## ✨ What Gets Discovered

✅ **Environment** - Configuration, FreeIPA, network settings  
✅ **DataLake** - Runtime versions, instance groups, recipes  
✅ **DataHub** - Clusters, templates, recipes, upgrade candidates  
✅ **CDE** - Services, virtual clusters, versions, backups  
✅ **CAI/ML** - Workspaces, serving apps, model registries  
✅ **CDW** - Clusters, DBCs, virtual warehouses, versions  
✅ **CDF** - Services, deployments, flows, projects  
✅ **COD** - Databases, configurations, recipes

---

**Version:** 1.0.0  
**Last Updated:** October 2025
