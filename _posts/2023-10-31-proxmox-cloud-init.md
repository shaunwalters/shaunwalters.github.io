---
layout: post
title: "My perfect Proxmox cloud init image"
date: 2023-10-31 07:00:00
categories: homelab
tags: homelab linux ubuntu
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
