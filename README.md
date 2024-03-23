# Ansible Role: CUDA Install

This role installs CUDA packages from [the official Nvidia CUDA repos](https://developer.download.nvidia.com/compute/cuda/repos/).

## Installation

Ansible Galaxy is the recommended way to install this role in your environment.

### Using Ansible Galaxy CLI

```bash
# Install the latest version from the repository
ansible-galaxy role install git+https://github.com/bounverif/ansible-role-cuda-install.git,main,bounverif.cuda_install
```

### Using Ansible Galaxy requirements file

```yaml
# requirements.yml

roles:
  # Install the latest version from GitHub
  - name: bounverif.cuda_install
    src: https://github.com/bounverif/ansible-role-cuda-install
    version: main

  # # Install the latest version from Galaxy Hub (not supported yet)
  # - name: bounverif.cuda_install
```

```bash
ansible-galaxy install -r requirements.yml
```

### Using other methods

See [Ansible Galaxy User Guide](https://docs.ansible.com/ansible/latest/galaxy/user_guide.html) for more options to install Ansible roles.

## Usage

You can use this role to install a CUDA package on a supported Linux distro (`cuda_install_distro`) on a supported CUDA architecture (`cuda_install_nvarch`). By default the role will try to setup the correct CUDA repository by gathering your distribution and architecture facts. Then the role will install all listed packages (`cuda_install_packages`) using the package manager.

```yaml
---
- name: My CUDA environment setup
  hosts: all
  gather_facts: true
  tasks:
    - name: Install CUDA Packages (Debian)
      ansible.builtin.include_role:
        name: bounverif.cuda_install
      vars:
        cuda_install_packages:
          - cuda-nvcc-12-4
```

You can owerwrite `cuda_install_nvarch` and `cuda_install_distro` variables to directly control the repo setup as follows:

```yaml
---
- name: My CUDA environment setup
  hosts: all
  gather_facts: true
  tasks:
    - name: Install CUDA Packages (Debian-Arm)
      ansible.builtin.include_role:
        name: bounverif.cuda_install
      vars:
        cuda_install_nvarch: sbsa # Arm server architecture (default: x86_64)
        cuda_install_distro: debian12 # Deduced automatically by default
        cuda_install_packages:
          - libnvpl-sparse-devel
```

Note that every package is not available for every architecture. Besides package names and versioning schemes can be different from one distribution to another. Please refer to [the official repo index](https://developer.download.nvidia.com/compute/cuda/repos/) for CUDA architectures, distributions, and available packages.
