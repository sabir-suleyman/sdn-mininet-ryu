# SDN Data Plane and OpenFlow Flow Tables

This project demonstrates a basic Software Defined Networking (SDN) environment using **Mininet** and a **Ryu Controller**.

The main objective is to understand how the **control plane** and **data plane** interact, observe how flow rules are dynamically created, and apply a custom traffic policy by overriding the controller.

---

## 📌 Overview

In this project, I:

- Built a simple network topology using Mininet
- Connected the topology to a remote Ryu controller
- Observed how OpenFlow rules are dynamically installed
- Applied a custom rule to isolate a host from the network

---

## 🧱 Topology

The network topology consists of:

- **1 Switch** → `s1`
- **3 Hosts** → `h1`, `h2`, `h3`
- All hosts are directly connected to the switch
- The controller runs locally (`127.0.0.1:6653`)

---

## ⚙️ Technologies Used

- Mininet
- Ryu Controller
- OpenFlow 1.3
- Open vSwitch (OVS)

---

## 🚀 How to Run

### 1. Start the Ryu Controller

```bash
python3 -m ryu.cmd.manager ryu.app.simple_switch_13
```

### 2. Run the Mininet Topology

```bash
sudo python3 controller_topo.py
```

### 3. Test Connectivity

```bash
pingall
```

---

## 🔍 Task 3: Flow Table Analysis

To observe how the controller installs flow rules dynamically:

```bash
sh ovs-ofctl -O OpenFlow13 dump-flows s1
h1 ping -c1 h2
sh ovs-ofctl -O OpenFlow13 dump-flows s1
```

Initially, the flow table contains only a default rule.  
After generating traffic, new flow entries are added automatically by the controller.

---

## 🔒 Task 4: Host Isolation (Traffic Engineering)

In this step, I implemented a security policy to isolate `h3` from the network.

The following high-priority rules were added:

```bash
sh ovs-ofctl -O OpenFlow13 add-flow s1 "priority=300,ip,nw_src=10.0.0.3,actions=drop"
sh ovs-ofctl -O OpenFlow13 add-flow s1 "priority=300,ip,nw_dst=10.0.0.3,actions=drop"
sh ovs-ofctl -O OpenFlow13 add-flow s1 "priority=300,arp,arp_spa=10.0.0.3,actions=drop"
sh ovs-ofctl -O OpenFlow13 add-flow s1 "priority=300,arp,arp_tpa=10.0.0.3,actions=drop"
```

### Result

- `h1 ↔ h2` → reachable  
- `h3` → completely isolated  

This demonstrates that manually injected rules can override controller behavior.

---

## 📊 Key Takeaways

- SDN separates control and data planes  
- Controllers dynamically manage flow tables  
- Network behavior can be modified without changing physical topology  
- OpenFlow rules enable fine-grained traffic control  

---

## 📷 Screenshots

(My screenshots here)

```markdown
![Pingall Result](screenshots/task4_pingall.png)
![Flow Table](screenshots/task3_flows.png)
```

---

## 📄 Report

A detailed report explaining all steps and observations is included:

```
report.pdf
```

---

## 👤 Author

Sabir Suleymanli  
E-mail: suleymanlisabir3@gmail.com
