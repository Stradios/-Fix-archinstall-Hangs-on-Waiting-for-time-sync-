# 🛠️ Fix: `archinstall` Hangs on "Waiting for time sync"

## ❗ Problem

During an Arch Linux installation using `archinstall`, the process can hang indefinitely at:

```
Waiting for time sync (systemd-timesyncd.service) to complete.
```

This typically occurs if your **network blocks NTP traffic**, preventing time synchronization.

---

## ✅ Solution

You can manually configure `systemd-timesyncd` to bypass the NTP issue.

Reference: [Arch Wiki – systemd-timesyncd](https://wiki.archlinux.org/title/Systemd-timesyncd)

---

## 🔧 Step-by-Step Fix (via SSH)

### 1. Boot into the Arch installation media  
Set up your network (Wi-Fi or Ethernet) using `iwctl`, `nmcli`, or `ip`.

### 2. Get your IP address  
Use a command like:

```bash
ip a
```

### 3. Set a temporary root password  

```bash
passwd
```

### 4. Enable SSH  

Check if SSH is running:

```bash
systemctl status sshd
```

If not, start it:

```bash
systemctl start sshd
```

### 5. Connect from another computer  

```bash
ssh root@<your_ip_address>
```

---

## 📝 Modify Time Sync Configuration

### Stop the service:

```bash
systemctl stop systemd-timesyncd
```

### Edit the config:

```bash
nano /etc/systemd/timesyncd.conf
```

Add or replace with:

```ini
[Time]
NTP=0.arch.pool.ntp.org 1.arch.pool.ntp.org 2.arch.pool.ntp.org 3.arch.pool.ntp.org
FallbackNTP=0.pool.ntp.org 1.pool.ntp.org 0.fr.pool.ntp.org
```

Exit and save (`Ctrl + X`, then press `Y` and `Enter`).

### Start the service:

```bash
systemctl start systemd-timesyncd
```

---

## 🎉 Done!

Now your system should sync time properly. You can continue the installation:

```bash
archinstall
```

Whether from SSH or directly on the machine, installation should proceed without hanging.

---

## 🧠 Notes

- This workaround is especially helpful in corporate or school networks that block outbound NTP (UDP port 123).
- You can use a local NTP server if available.

---

Made with ❤️ for fellow Arch users.
