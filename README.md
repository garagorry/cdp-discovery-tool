# CDP Environment Discovery Tool

Automated discovery tool for all Cloudera Data Platform (CDP) resources in an environment.

## Overview

Discovers all CDP services and exports detailed configuration as JSON and CSV:

- **Infrastructure**: Environment, FreeIPA, DataLake, DataHub, COD
- **Data Services**: CDE, CAI/ML, CDW, CDF
- **Resources**: Instance groups, recipes, runtime versions, upgrade candidates
- **Kubernetes**: Virtual clusters, database catalogs, virtual warehouses, deployments

## Installation

### Quick Install (One-Liner)

```bash
git clone https://github.com/garagorry/cldr.git && cd cldr/cdppc/misc/discovery_environment
```

**📚 For detailed installation instructions:** See [INSTALL.md](INSTALL.md)

## Quick Start

### Prerequisites

1. **Python 3.7+**
2. **CDP CLI** installed and configured
3. **CDP credentials** with environment access

### Installation

```bash
# Install CDP CLI (if not already installed)
pip install cdpcli

# Configure credentials
cdp configure

# If using CDP CLI beta in virtual environment:
source ~/venvs/cdpcli-beta/bin/activate  # or run your activation function (e.g., a4)
```

### Basic Usage

```bash
# Activate CDP CLI (if in virtual environment)
a4  # or: source ~/venvs/cdpcli-beta/bin/activate

# Run discovery
cd /path/to/discovery_environment
python3 discover.py --environment-name <your-environment-name>

# Or use the wrapper script
./run_discovery.sh <your-environment-name>
```

### Common Options

```bash
# Use specific CDP profile
python3 discover.py --environment-name my-env --profile production

# Discover specific services only
python3 discover.py --environment-name my-env --include-services cde cdw cai

# Exclude specific services
python3 discover.py --environment-name my-env --exclude-services cod

# Custom output directory
python3 discover.py --environment-name my-env --output-dir /path/to/output

# Enable debug output
python3 discover.py --environment-name my-env --debug
```

## Project Structure

```
discovery_environment/
├── discover.py          # Main entry point
├── main.py              # Orchestration logic
├── run_discovery.sh     # Wrapper script with validation
├── common/              # Shared utilities
│   ├── cdp_client.py   # CDP CLI wrapper
│   ├── config.py       # Configuration management
│   └── utils.py        # Helper functions
├── modules/             # Discovery modules (one per service)
│   ├── environment.py  # Environment + FreeIPA
│   ├── datalake.py     # DataLake
│   ├── datahub.py      # DataHub
│   ├── cde.py          # Cloudera Data Engineering
│   ├── cai.py          # Cloudera AI/ML
│   ├── cdw.py          # Cloudera Data Warehouse
│   ├── cdf.py          # Cloudera DataFlow
│   └── cod.py          # Operational Database
└── exporters/           # Output formatters
    ├── json_exporter.py
    └── csv_exporter.py
```

## Output Structure

Discovery creates a timestamped directory with the following structure:

```
discovery_env_<environment-name>-<timestamp>/
├── environment/
│   ├── environment.json
│   ├── freeipa_instances.json
│   └── recipes/
├── datalake/
│   └── <datalake-name>/
│       ├── datalake.json
│       ├── instance_groups.csv
│       └── recipes/
├── datahub/
│   └── <cluster-name>/
│       ├── cluster.json
│       ├── instance_groups.csv
│       └── recipes/
├── cde/
│   └── <service-name>/
│       ├── service.json
│       └── virtual_clusters/
├── cai/
│   ├── workspaces.json
│   ├── workspaces.csv
│   └── <workspace-name>/
├── cdw/
│   └── <cluster-name>/
│       ├── cluster.json
│       ├── database_catalogs.csv
│       └── virtual_warehouses.csv
├── cdf/
│   └── <service-name>/
│       ├── service.json
│       └── deployments/
└── cod/
    └── <database-name>/
        ├── database.json
        └── recipes/
```

**Archive:** All output is automatically compressed to `<output-dir>.tar.gz`

## Discovered Information

### Environment & FreeIPA

- Environment configuration and metadata
- FreeIPA instances with recipes
- Network and security settings

### DataLake

- Datalake configuration
- Instance groups and volumes
- Runtime versions and upgrade candidates
- Custom recipes

### DataHub

- Cluster configurations
- Instance groups with detailed specs
- Runtime versions and upgrade candidates
- Custom recipes per cluster

### CDE (Cloudera Data Engineering)

- Service configuration
- Virtual clusters
- Upgrade status and available versions
- Backups and Kubeconfig

### CAI (Cloudera AI/ML)

- ML workspaces
- Serving applications
- Model registries
- Network configuration and quotas
- Workspace versions and backups

### CDW (Cloudera Data Warehouse)

- Cluster configuration
- Database Catalogs (DBCs) with upgrade versions
- Virtual Warehouses (VWs) with upgrade versions
- Data Visualizations
- Hue instances

### CDF (Cloudera DataFlow)

- Service configuration and workload version
- Active deployments with NiFi versions
- Flow definitions
- Projects and ReadyFlows

### COD (Operational Database)

- Database configuration
- Instance details
- Custom recipes

## Troubleshooting

### CDP CLI Not Found

**Error:** `cdp: command not found`

**Solution:**

```bash
# If using virtual environment, activate it first
source ~/venvs/cdpcli-beta/bin/activate

# Verify
cdp --version
```

### Credentials Not Configured

**Error:** `CDP credentials not found`

**Solution:**

```bash
cdp configure
# Enter your CDP Access Key ID and Private Key
```

### Profile Not Found

**Error:** `Profile 'xyz' not found`

**Solution:**

```bash
# List available profiles
cat ~/.cdp/credentials

# Use correct profile name
python3 discover.py --environment-name my-env --profile <correct-profile>
```

### No Services Found

**Possible causes:**

- Incorrect environment name
- Services not deployed in the environment
- Insufficient permissions

**Solution:**

```bash
# Verify environment exists
cdp environments describe-environment --environment-name <your-env>

# Run with debug flag
python3 discover.py --environment-name my-env --debug
```

### Import Errors

**Error:** `ImportError: attempted relative import`

**Solution:** Use `discover.py` instead of `main.py` directly:

```bash
python3 discover.py --environment-name my-env
```

## Features

✅ Multi-cloud support (AWS, Azure, GCP)
✅ Modular architecture
✅ JSON and CSV exports
✅ Recipe extraction for all applicable services
✅ Runtime and upgrade version tracking
✅ Detailed instance group information
✅ Automatic archiving
✅ Flexible service filtering

## Requirements

- Python 3.7+ (standard library only)
- CDP CLI configured with valid credentials
- Network access to CDP Control Plane

## Documentation

- [ARCHITECTURE.md](ARCHITECTURE.md) - Technical architecture details
- [API_REFERENCE.md](API_REFERENCE.md) - CDP API coverage and methods
