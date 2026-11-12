# CCNA Labs

Hands-on networking labs for the Cisco CCNA (200-301) exam, built in Cisco Packet Tracer. Each lab includes a topology, device configurations, verification output and troubleshooting notes.

I come from a full-stack development background, and I'm using these labs to build practical networking skills, with network automation as the next step.

## Tools

- Cisco Packet Tracer
- Git and GitHub for versioning configs and documentation

## Lab Index

| Topic | Lab | CCNA Objectives | Status |
|---|---|---|---|
| Switching | [VLANs and trunking](01-switching/01-vlans-and-trunking/) | 2.1, 2.2 | Done |
| Switching | EtherChannel | 2.4 | Planned |
| Switching | Rapid PVST+ | 2.5 | Planned |
| Routing | Static routing | 3.3 | Planned |
| Routing | OSPF single area | 3.4 | Planned |
| Routing | OSPF multi-area | 3.4 | Planned |
| Services | DHCP and NAT | 4.1, 4.8 | Planned |
| Services | NTP and SNMP | 4.2, 4.4 | Planned |
| Security | ACLs | 5.6 | Planned |
| Security | Port security | 5.7 | Planned |

Update the Status column and add a link as each lab is finished.

## Repository Structure

Labs are grouped by topic and numbered in the order I do them.

```
ccna-labs/
├── README.md                          (this file, the lab index)
├── .gitignore
├── 01-switching/
│   ├── 01-vlans-and-trunking/
│   ├── 02-etherchannel/
│   └── 03-rapid-pvst/
├── 02-routing/
│   ├── 01-static-routing/
│   ├── 02-ospf-single-area/
│   └── 03-ospf-multi-area/
├── 03-services/
│   ├── 01-dhcp-and-nat/
│   └── 02-ntp-and-snmp/
└── 04-security/
    ├── 01-acls/
    └── 02-port-security/
```

## Structure of Each Lab

Every lab uses the same layout:

```
01-vlans-and-trunking/
├── README.md          (objectives, topology, addressing, steps, troubleshooting)
├── topology.png       (screenshot of the Packet Tracer workspace)
├── vlan-lab.pkt       (Packet Tracer file)
├── SW1.txt            (show running-config for each device)
├── SW2.txt
├── R1.txt
└── verification.txt   (show and ping output proving the lab works)
```

| File | What it contains |
|---|---|
| `README.md` | Objectives, addressing table, step-by-step configuration, verification table, troubleshooting scenarios |
| `topology.png` | The network diagram |
| `*.pkt` | The Packet Tracer project, to reopen and run the lab |
| `<device>.txt` | Cleaned running-config, with the `Building configuration...` and byte-count lines removed |
| `verification.txt` | Command outputs such as `show vlan brief`, `show interfaces trunk` and ping results |

## Setting Up the Repo

```bash
mkdir -p ccna-labs/01-switching/01-vlans-and-trunking
cd ccna-labs
git init
```

Add a `.gitignore` for editor and system files:

```
.DS_Store
Thumbs.db
*.swp
*.tmp
```

## Adding a New Lab

```bash
mkdir 01-switching/02-etherchannel
# add README.md, topology.png, .pkt, configs and verification.txt
git add .
git commit -m "Add lab: EtherChannel"
git push
```

Then update the index table above.

## Conventions

- Folder and file names are lowercase and hyphenated, with number prefixes so labs sort in order.
- Every interface has a `description` in the saved configs.
- Each lab README lists the CCNA objectives it covers.
- Troubleshooting is documented: what I broke, how I found it, how I fixed it.
- No real credentials or real network details in any config.

## About

<!-- Add a short line about yourself and a link to your LinkedIn or portfolio. -->
