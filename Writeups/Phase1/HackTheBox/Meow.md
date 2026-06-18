# HTB – Meow


## Reconnaissance

Started with a basic port scan to identify open services on the target.

```bash
nmap {ip_target_machine}
```

**Result:** Only one open port was found:

| Port | Service |
|------|---------|
| 23 | Telnet |

Telnet being open is already a strong indicator that this machine is intentionally misconfigured – Telnet transmits data, including credentials, in plaintext and is considered insecure by modern standards.

---

## Service Enumeration & Access

Since Telnet was the only open port, I connected directly to it:

```bash
telnet {ip_target_machine}
```

The connection prompted for a username. I tried `root` as a common default/test account.

**Result:** No password was required – the login succeeded immediately, granting a root shell on the target.

This indicates the root account had no password set at all, which is a critical misconfiguration. In a real-world scenario, this would allow anyone on the network to gain full control of the machine instantly.

---

## Flag

Once logged in as `root@meow`, i checked the current directory with `ls` and directly found flag.txt. 

With cat i could read the file immediately and could solve the searched [flag]{https://labs.hackthebox.com/achievement/machine/3632952/394}. 

```bash
cat flag.txt
```

---

## Lessons Learned

- An open Telnet port is a red flag in itself – it should generally be disabled in favor of SSH.
- A root account without a password is one of the most critical misconfigurations possible, since it removes any barrier to full system compromise.
- This box demonstrates why even a single overlooked open port/service can lead to immediate, complete access.
