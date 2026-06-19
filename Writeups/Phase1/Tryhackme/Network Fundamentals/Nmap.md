# Nmap

- Is an open source tool for network exploration
- Designed to rapidly scan large networks

--- 

## Host Discovery

Nmap can be used to discover live hosts connected to the network

- IP-range using `-`: `10.10.10.1-10` is scanning all IP addresses from `10.10.10.1`-`10.10.10.10`
- IP-subnet using `/`: `10.10.10.1/24` is equivalent to `10.10.10.0-255`
- `sn` discover live hosts
- `sL` lists targets to scan

---

## Port scanning

- Scanning network services on live hosts with `telnet` -> not very different from `Nmaps`connect scan
-  Network service = any process listening for incoming connections on `TCP` or `UDP` port (Default `TCP`: `80` or `443`, `UDP`: `53`)
  - `-sT` performs full  TCP connect scan with trying to complete 3-way handshake
  - `-sS` only sends TCP SYN packet's -> "stealth attack" because 3-way handshake is never completed but we see what address answered
  - `sU` scans `UDP`-services
  - `-F` scans 100 most common ports
  - `p[range]` scans specific ports (i.e. `-p10-1024` scans port 10-1024, `-p-25` scans port 1-25
  - `-Pn` ignores host detection and handels every target like it would be online
    - `nmap`usually uses pings to declare if host is up or not
    - Really good when `ping` command is blocked cause of firewalls or if the host is ignoring `ICMP`

## Version Detection

- `-O` enables OS-detection
- `-sV` discovers what services are listed on open ports
- `-A` combines `-O` and `-sV`

## Timing

- Scanning at normal speed might trigger an `IDS` (Intrusion Detection System)
- Nmap provides 6 timing templates, `-T0` (slowest) to `-T5` (fastest)
- The faster the timing template, the higher the risk of detection by an IDS
- `--host-timeout` sets the maximum time to wait for a single target host

## Verbosity and Debugging

- `-v` shows live scan progress
  - usfull to discover how Nmap scans
  - `-vv` & `-vvvv`outputs even more details
- `-d`enters debugging informations
  - the more `-dddd...` the more details

 To save scan results, nmap gives various formats
- `-oN <filename>` : Normal output
- `-oX <filename>` : XML output
- `-oG <filename>` : `grep`-able output
- `-oA <filename>` : output in all major formats

## NSE Scripts

- `NSE` (Nmap Scripting Engine) is a powerful addition to Nmap
- Scripts are written in Lua
- Extending Nmap with i.e. scanning for vulnerabilities, automating exploits+
- Many categories available:
|Category| Description|
|--------|------------|
|`safe`| Won't affect the target|
|`intrusive`|Likely to affect the target|
|`vuln`|Scans for vulnerabilities|
|`exploit`|Attempt to exploit a vulnerability|
|`auth`|Attempt to bypass authentication for running services|
|`brute`|Attempt to bruteforce credentials|
|`discovery`|Attempt to query running services for further infos about the network|

- `--script=<script-name>` to run a script
- With seperating `,`, it is possible to run multiple scripts simultaneously
- Some Scripts require arguments. They can be given with `--script-args`

[NSE Nmap Scripting Engine libary](https://nmap.org/nsedoc/)
