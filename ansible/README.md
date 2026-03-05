# Ansible AAP CSV Update

This directory contains an Ansible playbook and module to update the AAP operator CSV compatibility matrix.

## Requirements

- Ansible 2.9+
- Podman
- Access to `registry.redhat.io` (Red Hat registry)

## Setup

### 1. Registry Authentication

The playbook needs to pull images from the Red Hat registry. Registry URL and credentials are loaded from **`vars/registry.yml`**, which uses environment variables by default.

**Option A: Set environment variables**
```bash
export REGISTRY_USERNAME=YOUR_REDHAT_USER
export REGISTRY_PASSWORD=YOUR_REDHAT_PASSWORD
ansible-playbook playbook.yml
```

**Option B: Override at run time**
```bash
ansible-playbook playbook.yml -e "registry_username=USER" -e "registry_password=PASS"
```

**Option C: Login beforehand**
```bash
podman login registry.redhat.io
# Enter your Red Hat username and password when prompted
```
Then run the playbook without credentials - it will verify you're already logged in.

### 2. Run the Playbook

From the `ansible` directory:
```bash
cd ansible
ansible-playbook playbook.yml
```

Or from the project root:
```bash
ansible-playbook ansible/playbook.yml -i ansible/inventory.yml
```

### 3. GitHub Actions (on-demand)

The repo includes a workflow **Update AAP CSV matrix** (`.github/workflows/update-csv-matrix.yml`) that you can run manually:

1. **Actions** → **Update AAP CSV matrix** → **Run workflow**
2. Configure these **repository secrets** (Settings → Secrets and variables → Actions):
   - `REGISTRY_USERNAME`: Red Hat registry username
   - `REGISTRY_PASSWORD`: Red Hat registry password (e.g. token)
3. The workflow installs Podman and Ansible, runs the playbook, then commits and pushes any changes under `data/*.csv`.

## Variables

The playbook loads variables from two files (in order):

1. **`vars/registry.yml`** – registry URL and credentials  
   - `registry`: Container registry (default `registry.redhat.io`)  
   - `registry_username`: From env `REGISTRY_USERNAME` if set  
   - `registry_password`: From env `REGISTRY_PASSWORD` if set  

2. **`vars/check_versions.yml`** – CSV matrix configuration  
   - Edit this file to change OCP versions, channels, and AAP versions.

Playbook-level vars:

| Variable | Default | Description |
|----------|---------|-------------|
| `data_dir` | `{{ playbook_dir }}/../data` | Directory containing AAP_*.csv files |

From **vars/check_versions.yml**:

| Variable | Description |
|----------|-------------|
| `columns` | Column indices: release_date, cluster_scoped, namespace_scoped, openshift_support |
| `loops` | List of `{ ocp_version, channel, aap_version, scope }` entries |

## Module: aap_csv_update

The `aap_csv_update` module:
- Uses `columns` and `loops` (from vars) or a config file for OCP versions and channels
- Pulls operator index images via podman
- Extracts CSV versions from namespace and cluster-scoped channels
- Pairs versions by timestamp
- Updates CSV files with missing entries

### Module Parameters

- `columns`: (optional) Dict of column indices; use with `loops` from vars
- `loops`: (optional) List of loop entries; when set, `config_file` is ignored
- `config_file`: (optional) Path to check-versions.json when not using vars
- `data_dir`: Directory containing AAP_*.csv files
- `registry`: Registry URL (for future use)

### Returns

- `changed`: Whether any files were modified
- `messages`: List of status messages
- `rows_added`: Number of new rows added
- `rows_updated`: Number of existing rows updated
