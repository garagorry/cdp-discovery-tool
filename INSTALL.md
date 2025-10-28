# Installation Guide

Quick guide for customers to download and run the CDP Environment Discovery Tool.

## Prerequisites

Before starting, ensure you have:

1. **Python 3.7 or higher**

   ```bash
   python3 --version
   ```

2. **CDP CLI installed and configured**

   ```bash
   pip install cdpcli
   cdp configure
   ```

3. **Git installed**
   ```bash
   git --version
   ```

## Installation

### Option 1: Clone the Entire Repository (Recommended)

```bash
# Clone the repository
git clone https://github.com/garagorry/cldr.git

# Navigate to the discovery tool
cd cldr/cdppc/misc/discovery_environment

# Verify installation
ls -la
```

### Option 2: Clone Only the Discovery Tool (Sparse Checkout)

If you only want the discovery tool without the entire repository:

```bash
# Clone with sparse checkout
git clone --filter=blob:none --sparse https://github.com/garagorry/cldr.git
cd cldr

# Checkout only the discovery_environment folder
git sparse-checkout set cdppc/misc/discovery_environment

# Navigate to the tool
cd cdppc/misc/discovery_environment
```

### Option 3: Download as ZIP (No Git Required)

1. Go to: https://github.com/garagorry/cldr
2. Click the green **"Code"** button
3. Select **"Download ZIP"**
4. Extract the ZIP file
5. Navigate to `cldr-main/cdppc/misc/discovery_environment/`

## Quick Start

### 1. Verify CDP CLI

Make sure CDP CLI is working:

```bash
cdp --version
cdp environments list-environments
```

If you're using a CDP CLI virtual environment:

```bash
# Activate your CDP CLI environment
source ~/venvs/cdpcli-beta/bin/activate
# Or your custom activation command
```

### 2. Run Discovery

```bash
# Basic discovery for an environment
python3 discover.py --environment-name <your-environment-name>

# Or use the wrapper script
chmod +x run_discovery.sh
./run_discovery.sh <your-environment-name>
```

### 3. View Results

Discovery output will be saved to `/tmp/discovery_env_<environment-name>-<timestamp>/` and automatically archived as a `.tar.gz` file.

## Examples

### Discover all resources in an environment

```bash
python3 discover.py --environment-name my-prod-env
```

### Discover specific services only

```bash
python3 discover.py --environment-name my-env --include-services cde cdw cai
```

### Use a specific CDP profile

```bash
python3 discover.py --environment-name my-env --profile production
```

### Save to custom directory

```bash
python3 discover.py --environment-name my-env --output-dir /path/to/output
```

### Enable debug mode

```bash
python3 discover.py --environment-name my-env --debug
```

## Troubleshooting

### CDP CLI Not Found

**Error:** `cdp: command not found`

**Solution:**

```bash
# Install CDP CLI
pip install cdpcli

# Or activate your virtual environment
source ~/venvs/cdpcli/bin/activate
```

### No Credentials Configured

**Error:** `CDP credentials not found`

**Solution:**

```bash
cdp configure
# Enter your CDP Access Key ID and Private Key
```

### Permission Denied on run_discovery.sh

**Error:** `Permission denied`

**Solution:**

```bash
chmod +x run_discovery.sh
```

### Import Errors

**Error:** `ImportError: No module named...`

**Solution:** Use `discover.py` instead of `main.py` directly:

```bash
python3 discover.py --environment-name my-env
```

## Getting Help

### View all available options

```bash
python3 discover.py --help
```

### View documentation

- [README.md](README.md) - Main documentation
- [ARCHITECTURE.md](ARCHITECTURE.md) - Technical details
- [API_REFERENCE.md](API_REFERENCE.md) - API coverage

## Support

For issues or questions:

1. Check the [README.md](README.md) troubleshooting section
2. Run with `--debug` flag for detailed output
3. Open an issue on GitHub: https://github.com/garagorry/cldr/issues

## System Requirements

- **OS:** Linux, macOS, or Windows (with Git Bash/WSL)
- **Python:** 3.7 or higher
- **Disk Space:** ~10 MB for code, variable for output (depends on environment size)
- **Network:** Access to CDP Control Plane APIs
- **Permissions:** CDP credentials with read access to environments

## Notes

- The tool uses only Python standard library (no additional pip packages required)
- All discovery operations are read-only and safe to run
- Output contains sensitive data - handle appropriately
- Tool respects CDP API rate limits by running sequentially
