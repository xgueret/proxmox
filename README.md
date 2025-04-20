# Proxmox

## Table of Contents



## 1. Installing Proxmox on my Acer XC-605 PC

### 🔽 Download the Proxmox ISO

```bash
mkdir -p ~/Worspace/tmp/proxmox
cd ~/Worspace/tmp/proxmox
```

### 1.1 Download the archive and SHA256 file

```bash
wget https://enterprise.proxmox.com/iso/proxmox-ve_8.4-1.iso
wget https://enterprise.proxmox.com/iso/SHA256SUMS
```

### 1.2 Verify the SHA-256 hash

In the same directory as the downloaded files:

```bash
cat SHA256SUMS | grep proxmox-ve_8.4-1.iso | sha256sum -c
proxmox-ve_8.4-1.iso: OK
```

2. ## 💾 Install proxmox with Ventoy

(i) see https://www.ventoy.net/en/index.html or https://doc.ubuntu-fr.org/ventoy

If you have a USB drive with ventoy, simply copy the ISO file to the **Ventoy partition of the USB drive** (it opens like a normal drive)

```bash
cp ~/Workspace/tmp/proxmox/proxmox-ve_8.4-1.iso /media/xgueret/Ventoy
```

⚙️ **BIOS/UEFI Settings**

Some settings may block booting or installation.

> :information_source: At startup, press the Delete key to access the BIOS menu

* **Disable Secure Boot** (Proxmox doesn't support it yet)
* Enable **VT-x/Virtualization** in the BIOS (it's important for proxmox)

:point_right: Use the USB drive with Ventoy in my Acer XC-605 pc to boot and install Proxmox

> :information_source: At startup press **F12** key to access the boot menu
>
> How to solve the problem during Proxmox installation
>
> I encountered the same problem during my Proxmox8 installtion ... https://forum.proxmox.com/threads/proxmox-ve-8-2-install-failing.153913/
>
> 💡Solution:
>
> When you're at the installation menu, press `e` to edit the boot options.
>
> At the line starting with `linux`, add at the end:
>
> ```bash
> nomodeset
> ```

Follow the instructions, and there you go 🎉

## 3. 🔐 **SSH Configuration on my Linux workstation**

Connect to the Proxmox server using an SSH key pair

```bash
ssh-keygen -t rsa -b 4096 -f ~/.ssh/proxmox -C "root@x.x.x.x"
```

> :information_source: *Include a passphrase if necessary.*
>
> ```bash
> ls -al ~/.ssh
> ```

Copy the public key to the Proxmox server

```bash
ssh-copy-id -i ~/.ssh/proxmox.pub root@x.x.x.x
```

Optionally activate ssh-agent

```bash
eval $(ssh-agent)
ssh-add ~/.ssh/proxmox
ssh root@x.x.x.x
```

Get the repository:

```bash
cd ~/Workspace
git clone 
cd /proxmox
# at this point you need to activate the Python venv and ensure that Ansible is installed
cd ansible
```

Run the playbook with the following tag to configure the ansible user on the target server

```bash
ansible-playbook -u root playbook.yml --tags "security_ssh_hardening"
```

