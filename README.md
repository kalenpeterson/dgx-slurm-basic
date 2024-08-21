# Install Slurm - Single node

## Start

Clone deepops
```
git clone https://github.com/NVIDIA/deepops.git
cd deepops
git checkout 23.08
git status
```

Create config base
```
mkdir -p ./config/group_vars/
cp ./config.example/inventory ./config/
cp ./config.example/group_vars/all.yml ./config/group_vars/all.yml
cp ./config.example/group_vars/slurm-cluster.yml ./config/group_vars/slurm-cluster.yml
cp ./config.example/nvidia-mid-config.yml ./config/nvidia-mid-config.yml
```

Edit Config

## Install

Run Setup
```
./script/setup.sh
```

### Build Single-Node Slurm Cluster
This will reboot up-to 2 times. you will need to run it to complettion
```
source /opt/deepops/env/bin/activate
ansible-playbook -K --forks=1 --connection=local -l slurm-cluster playbooks/slurm-cluster.yml
```

### Setup docker for slurm
```
source /opt/deepops/env/bin/activate
ansible-playbook -K  --forks=1 --connection=local --limit slurm-cluster playbooks/container/docker-rootless.yml
```

## Errors
### Resolve GPG Key Error
https://developer.nvidia.com/blog/updating-the-cuda-linux-gpg-repository-key/
```
sudo apt-key del 7fa2af80
wget -O /tmp/cuda-keyring_1.0-1_all.deb https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/cuda-keyring_1.0-1_all.deb
sudo dpkg -i /tmp/cuda-keyring_1.0-1_all.deb
