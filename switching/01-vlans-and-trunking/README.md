**Lab: VLANs, Trunking and Inter-VLAN Routing (CCNA 2.1 and 2.2)**

**Tool:** Cisco Packet Tracer \| **Devices:** 1 router (2911), 2
switches (2960), 4 PCs, 2 IP phones

**CCNA Topics Covered**

---

**Topic** **Where in this lab**

---

2.1 VLANs spanning multiple Step 1 (VLANs on SW1 and
switches SW2)

2.1.a Access ports (data and Step 3
voice)

2.1.b Default VLAN Step 0

2.1.c Inter-VLAN connectivity Step 6 (router-on-a-stick)

2.2 Interswitch connectivity Step 4

2.2.a Trunk ports Step 4

2.2.b 802.1Q Step 4 and Step 7
(simulation mode)

2.2.c Native VLAN Step 4 and Troubleshooting

---

**VLAN** **Name** **Subnet** **Gateway
(R1)**

---

1 default not used \-

10 SALES 192.168.10.0/24 192.168.10.1

20 ENGINEERING 192.168.20.0/24 192.168.20.1

30 VOICE 192.168.30.0/24 192.168.30.1

99 MANAGEMENT 192.168.99.0/24 192.168.99.1
(native)

---

---

**Device** **VLAN** **IP Address**

---

PC1 10 192.168.10.10

PC2 20 192.168.20.10

PC3 10 192.168.10.20

PC4 20 192.168.20.20

SW1 SVI 99 192.168.99.11

SW2 SVI 99 192.168.99.12

---

Set each PC\'s default gateway to its VLAN\'s gateway from the first
table.

**Port Plan**

---

**Device** **Port** **Mode** **VLAN**

---

SW1 Fa0/1 Access Data 10, Voice 30
(Phone1 + PC1)

SW1 Fa0/2 Access Data 20 (PC2)

SW1 Gi0/1 Trunk To SW2

SW1 Gi0/2 Trunk To R1

SW2 Fa0/1 Access Data 20, Voice 30
(Phone2 + PC3)

SW2 Fa0/2 Access Data 10 (PC4)

SW2 Gi0/1 Trunk To SW1

---

Trunks use native VLAN 99 and allow VLANs 10, 20, 30, 99.

**Steps**

**Step 0: Default VLAN (2.1.b)**

Before configuring anything, on a switch run:

show vlan brief

All ports are in **VLAN 1**, the default VLAN. Try deleting or renaming
it:

no vlan 1

It fails: VLAN 1 cannot be deleted or renamed. Best practice is to leave
it empty and not use it for users or management. That is why this lab
moves everything to other VLANs. Save the output in verification.txt.

**Step 1: Create VLANs on both switches (2.1)**

VLANs are local to each switch, so create them on **SW1 and SW2**:

vlan 10

name SALES

vlan 20

name ENGINEERING

vlan 30

name VOICE

vlan 99

name MANAGEMENT

**Step 2: Name the devices**

hostname SW1 ! SW2 on the other switch

Router: hostname R1.

**Step 3: Access ports, data and voice (2.1.a)**

An IP phone has a built-in switch. The phone tags its own traffic with
the **voice VLAN**, and the PC behind it stays untagged in the **data
VLAN**. So one port carries two VLANs.

On **SW1 and SW2**:

interface fa0/1

switchport mode access

switchport access vlan 10

switchport voice vlan 30

!

interface fa0/2

switchport mode access

switchport access vlan 20

Verify:

show vlan brief

show interfaces fa0/1 switchport

Expected: Fa0/1 shows access VLAN 10 and **Voice VLAN: 30**. Fa0/2 is in
VLAN 20.

**Step 4: Trunks and native VLAN (2.2.a, 2.2.b, 2.2.c)**

A trunk carries many VLANs on one link. 802.1Q adds a 4-byte tag to each
frame to say which VLAN it belongs to. Frames in the **native VLAN** are
sent without a tag. The native VLAN must match on both ends.

**SW1** (Gi0/1 to SW2, Gi0/2 to R1) and **SW2** (Gi0/1 to SW1):

interface range gi0/1 - 2 ! SW2: interface gi0/1

switchport mode trunk

switchport trunk native vlan 99

switchport trunk allowed vlan 10,20,30,99

switchport nonegotiate

Verify:

show interfaces trunk

Expected: mode on, encapsulation 802.1q, status trunking, native vlan
99, allowed VLANs 10,20,30,99.

**Step 5: Management SVI**

interface vlan 99

ip address 192.168.99.11 255.255.255.0 ! SW2: 192.168.99.12

no shutdown

!

ip default-gateway 192.168.99.1

**Step 6: Inter-VLAN routing with router-on-a-stick (2.1.c)**

Devices in different VLANs cannot talk without a router. R1 uses one
physical link (a trunk to SW1) with one **subinterface per VLAN**.

interface gi0/0

no shutdown

!

interface gi0/0.10

encapsulation dot1q 10

ip address 192.168.10.1 255.255.255.0

interface gi0/0.20

encapsulation dot1q 20

ip address 192.168.20.1 255.255.255.0

interface gi0/0.30

encapsulation dot1q 30

ip address 192.168.30.1 255.255.255.0

interface gi0/0.99

encapsulation dot1q 99 native

ip address 192.168.99.1 255.255.255.0

Verify:

show ip interface brief

show ip route

Expected: four subinterfaces up/up and four connected networks
(192.168.10/20/30/99.0).

**Step 7: See 802.1Q in action**

1.  In Packet Tracer, switch to **Simulation mode**.

2.  Send a ping from PC1 to PC4.

3.  Click the packet when it is on the SW1-SW2 trunk and open **Inspect
    PDU \> 802.1Q**.

4.  You should see the VLAN ID tag (10 for PC1\'s traffic). Traffic from
    the native VLAN 99 (e.g. SW1 pinging SW2) is untagged.

**Verification**

---

**Test** **Expected**

---

PC1 ping PC3 (same VLAN, across Success
trunk)

PC2 ping PC4 (same VLAN, across Success
trunk)

PC1 ping PC2 (different VLANs, Success
via R1)

PC1 ping PC4 (different VLANs, Success
via R1)

PC1 ping 192.168.99.11 (SW1 SVI) Success

SW1 ping SW2 SVI Success

---

To prove R1 is needed, shut down gi0/0 on R1 and repeat the PC1 to PC2
ping. It should fail.

Save all outputs to verification.txt.

**Troubleshooting**

---

**Problem** **How to break it** **How to spot **Fix**
it**

---

Native VLAN SW2 Gi0/1: switchport Console warning, Set native to 99
mismatch trunk native vlan 1 show interfaces  
 trunk

VLAN not SW1 Gi0/1: switchport VLAN 20 cannot switchport trunk
allowed on trunk allowed vlan cross to SW2 allowed vlan add
trunk remove 20 20

Missing voice SW1 Fa0/1: no show interfaces switchport voice
VLAN switchport voice vlan fa0/1 switchport vlan 30

Wrong R1 Gi0/0.20: VLAN 20 PCs encapsulation
subinterface encapsulation dot1q 25 cannot reach the dot1q 20
VLAN gateway

Wrong access SW2 Fa0/1: switchport show vlan brief switchport access
VLAN access vlan 20 vlan 10

---

**Files**

vlans-ccna-2.1-2.2/

├── README.md

├── topology.png

├── vlan-lab.pkt

├── SW1.txt (show running-config)

├── SW2.txt (show running-config)

├── R1.txt (show running-config)

└── verification.txt

**Save to GitHub**

git add .

git commit -m \"Add VLAN, trunking and inter-VLAN routing lab (CCNA
2.1/2.2)\"

git push

**Notes**

\<!\-- What went wrong? What did you learn? \--\>
