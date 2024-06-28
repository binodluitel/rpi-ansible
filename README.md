# Raspberry PI Ansible Setup

The directory structure follows the alternative directory layout described in [ansible tips and tricks sample setup](https://docs.ansible.com/ansible/latest/tips_tricks/sample_setup.html#alternative-directory-layout).

## Basic Commands

### List all available hosts

```bash
ansible all -i ./inventories --list-hosts
```

### List all available tags

```bash
ansible-playbook -i ./inventories/rpis ./site.yaml --list-tags
```

### Ping host(s) - master

```bash
ansible-playbook -i ./inventories/rpis ./masters.yaml --tags ping
```

## Deploy to Raspberry Pi

The ansible script is tested on Raspberry Pi 4 Model B with at least 4GB RAM.
The script hasn't been tested on any other Raspberry Pi models.

### Raspberry Pi Setup

1. Install Raspberry Pi OS on the SD card.
2. Make sure to enable SSH and set up the network.
3. Update `inventory.yaml` file in `./inventories/rpis`:
    - Add the the Raspberry Pi hosts.
    - Update `ansible_user` and `ansible_ssh_private_key_file` with the correct values.

### Deploy master node(s)

```bash
ansible-playbook -i ./inventories/rpis ./masters.yaml
```

### Deploy worker node(s)

```bash
ansible-playbook -i ./inventories/rpis ./workers.yaml
```

## Results

```bash
$ kubectl get nodes -o wide
NAME        STATUS   ROLES           AGE    VERSION   INTERNAL-IP      EXTERNAL-IP   OS-IMAGE           KERNEL-VERSION     CONTAINER-RUNTIME
rpimaster   Ready    control-plane   2d4h   v1.28.8   192.168.50.165   <none>        Ubuntu 24.04 LTS   6.8.0-1006-raspi   containerd://1.7.12
rpinode1    Ready    <none>          14m    v1.28.8   192.168.50.120   <none>        Ubuntu 24.04 LTS   6.8.0-1006-raspi   containerd://1.7.12
rpinode2    Ready    <none>          14m    v1.28.8   192.168.50.30    <none>        Ubuntu 24.04 LTS   6.8.0-1006-raspi   containerd://1.7.12
```

```bash
kubectl get pods -A -o wide
NAMESPACE      NAME                                READY   STATUS    RESTARTS      AGE    IP               NODE        NOMINATED NODE   READINESS GATES
kube-flannel   kube-flannel-ds-mhqwz               1/1     Running   0             16m    192.168.50.30    rpinode2    <none>           <none>
kube-flannel   kube-flannel-ds-txz4b               1/1     Running   0             16m    192.168.50.120   rpinode1    <none>           <none>
kube-flannel   kube-flannel-ds-w2684               1/1     Running   2 (31h ago)   2d4h   192.168.50.165   rpimaster   <none>           <none>
kube-system    coredns-5dd5756b68-2b7bf            1/1     Running   2 (31h ago)   2d4h   10.244.0.7       rpimaster   <none>           <none>
kube-system    coredns-5dd5756b68-p5nkm            1/1     Running   2 (31h ago)   2d4h   10.244.0.6       rpimaster   <none>           <none>
kube-system    etcd-rpimaster                      1/1     Running   2 (31h ago)   2d4h   192.168.50.165   rpimaster   <none>           <none>
kube-system    kube-apiserver-rpimaster            1/1     Running   2 (31h ago)   2d4h   192.168.50.165   rpimaster   <none>           <none>
kube-system    kube-controller-manager-rpimaster   1/1     Running   2 (31h ago)   2d4h   192.168.50.165   rpimaster   <none>           <none>
kube-system    kube-proxy-2s6fn                    1/1     Running   0             16m    192.168.50.30    rpinode2    <none>           <none>
kube-system    kube-proxy-pl8xl                    1/1     Running   0             16m    192.168.50.120   rpinode1    <none>           <none>
kube-system    kube-proxy-sc7bh                    1/1     Running   2 (31h ago)   2d4h   192.168.50.165   rpimaster   <none>           <none>
kube-system    kube-scheduler-rpimaster            1/1     Running   2 (31h ago)   2d4h   192.168.50.165   rpimaster   <none>           <none>
```
