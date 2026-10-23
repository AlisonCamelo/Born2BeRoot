
*This project has been created as part of the 42 curriculum by acamelo.*

# Born2beRoot

## Description
The **Born2beRoot** project is an introduction to System Administration within the 42 curriculum. Its main goal is to set up a secure, headless (non-GUI) Linux Virtual Machine using **Debian 12**, enforcing strict security policies, LVM partition management, custom SSH configurations, strict password policies, and an automated system monitoring script.

### Choice of Operating System: Debian vs Rocky Linux
Debian was selected as the operating system for this project due to its stability, extensive community support, and straightforward package management via `apt`.

* **Debian:**
  * *Advantages:* Highly stable, wide range of software packages, lightweight, ideal for beginners in sysadmin.
  * *Disadvantages:* Slower update cycle for bleeding-edge software versions.
* **Rocky Linux:**
  * *Advantages:* Enterprise-grade stability, binary-compatible downstream version of Red Hat Enterprise Linux (RHEL), popular in production environments.
  * *Disadvantages:* Steeper learning curve for users unfamiliar with `dnf`/`yum` and SELinux policies.

### Main Technical & Architectural Choices
* **Partitioning & LVM:** Configured with LVM (Logical Volume Manager) to allow dynamic volume resizing. Includes separate partitions for `/`, `/home`, `/var`, `/tmp`, `/srv`, `/var/log`, and swap.
* **Security Policies:** Enforced strong password requirements using `pam_pwquality` and password expiration via `login.defs`. Restricted `sudo` usage with strict logging to `/var/log/sudo/sudo.log`.
* **User Management:** Created a dedicated non-root user `acamelo` belonging to the `sudo` and `user42` groups.
* **Services Installed:** `openssh-server` configured strictly on port `4242` with root login disabled, and `ufw` as the active host-based firewall.

### System Comparisons
* **Debian vs Rocky Linux:** Debian uses the `apt` package manager and `dpkg`, prioritizing system stability and vast community support. Rocky Linux uses `dnf` and aligns with RHEL standards, making it common in corporate infrastructure.
* **AppArmor vs SELinux:** AppArmor uses path-based access control and is generally simpler to configure and interpret (default on Debian). SELinux uses label-based (inode) access control, offering more fine-grained security policies but higher complexity (default on Rocky/RHEL).
* **UFW vs firewalld:** UFW (*Uncomplicated Firewall*) provides a simple command-line interface to manage `iptables` rules on Debian. `firewalld` is a dynamic firewall daemon using zones, typical on Red Hat distributions.
* **VirtualBox vs UTM:** VirtualBox is a cross-platform type-2 hypervisor widely used on x86 architectures. UTM is an macOS-optimized hypervisor utilizing QEMU, offering better native performance on Apple Silicon (M1/M2/M3) chips.

---

## Instructions

### Prerequisites
* A hypervisor installed (VirtualBox or UTM).
* The `.vdi` or `.qcow2` virtual disk file containing the pre-configured Debian VM.

### Execution & Connection
1. Launch the Virtual Machine in your hypervisor.
2. Log in directly from the tty using the credentials:
   * **Login:** `acamelo`
   * **Password:** *(Your configured password)*
3. Connect remotely via SSH from your host machine (Mac/PC terminal):
   ```bash
   ssh acamelo@localhost -p 4242

```

### Useful Evaluation Verification Commands

```bash
# Verify hostname
hostnamectl

# Verify user groups
groups acamelo

# Check SSH service status
sudo systemctl status ssh

# Check UFW firewall status
sudo ufw status numbered

# Inspect password complexity policy
sudo cat /etc/pam.d/common-password

# Inspect password expiration policy
sudo cat /etc/login.defs | grep PASS

# View sudo execution log
sudo cat /var/log/sudo/sudo.log

# View crontab configuration for monitoring.sh
sudo crontab -l

```

---

## Resources

### References

* [Debian Documentation & Wiki](https://wiki.debian.org/)
* [Sudoers Manual Pages](https://www.sudo.ws/docs/man/sudoers.man/)
* [UFW Official Documentation](https://wiki.ubuntu.com/UncomplicatedFirewall)
* [Linux Pam Quality Module (`pam_pwquality`)](https://www.google.com/search?q=https://man7.org/linux/man-pages/man8/pam_pwquality.8.html)

### AI Usage

AI tools (Gemini) were used during this project for the following specific tasks:

* **Debugging & Syntax Fixes:** Troubleshooting syntax errors in `grep` commands and resolving hostname resolution issues in `/etc/hosts`.
* **Concept Verification:** Clarifying system administration terminology (e.g., distinguishing between a *cron* daemon and a background process, and understanding LVM structures).
* **Evaluation Preparation:** Simulating evaluation walkthrough steps based on the official 42 correction sheet.

```
