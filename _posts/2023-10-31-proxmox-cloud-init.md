---
layout: post
title: "My perfect Proxmox cloud init image"
date: 2023-10-30 10:45:00
categories: homelab
tags: homelab linux debian proxmox
---

Pull down Debian 11 cloud image
```bash
wget https://cloud.debian.org/images/cloud/bullseye/20220613-1045/debian-11-genericcloud-amd64-20220613-1045.qcow2
```

Create Cloud init image in Proxmox
```bash
qm create 9500 --name Debian11CloudInit --net0 virtio,bridge=vmbr0 --kvm 0
```

```bash
qm importdisk 9500 debian-11-genericcloud-amd64-20220613-1045.qcow2 local-lvm
```

```bash
qm set 9500 --scsihw virtio-scsi-pci --scsi0 local-lvm:vm-9500-disk-0
```

```bash
qm set 9500 --ide2 local-lvm:cloudinit
```

```bash
qm set 9500 --boot c --bootdisk scsi0
```

```bash
qm set 9500 --serial0 socket --vga serial0
```

```bash
qm set 9500 --agent enabled=1 #optional but recommended
```

```bash
qm template 9500
```

```bash
qm clone 9500 9000 --name NEW-VM
```

From your local CLI
```bash
ssh-copy-id -i username@IPADDRESS
```

```bash
sudo apt update
sudo apt full-upgrade
```

```bash
sudo apt install qemu-guest-agent
```
