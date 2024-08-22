# Install Slurm - Single node

## References
 - https://github.com/NVIDIA/deepops
    - Deprecated recently, but still the best single-node SLURM deployment for DGX.

## Data Collection
 - [] Define users/groups that should have access?
 - [] Should users be able to submit jobs w/out slurm?
 - [] What user should be the admin user? needs sudo
 - [] Slurm Queue, any preferences?
 - [] Container integration any idea what you want?
    - [] Rootless Docker (single node)
    - [] Singularity (multi node)
    - [] Enroot (multi node)
 - [] Tests
    - [] Perform Nvidia prescribed tests
    - [] Any custom tests?

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

**NO GPU INstall**
```
source /opt/deepops/env/bin/activate
ansible-playbook -K --forks=1 --connection=local -l slurm-cluster -e '{"slurm_autodetect_nvml": false, "slurm_login_on_compute": false}' playbooks/slurm-cluster.yml
```


### Setup docker for slurm
```
source /opt/deepops/env/bin/activate
ansible-playbook -K  --forks=1 --connection=local --limit slurm-cluster playbooks/container/docker-rootless.yml
```

## Test
See: https://github.com/NVIDIA/deepops/blob/23.08/docs/slurm-cluster/slurm-single-node.md

### Grafana Dash
```
curl http://localhost:3000
```

