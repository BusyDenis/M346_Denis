## KN01 – Hypervisor Typ 1 und Typ 2

### A) Hypervisor Typ 1 und Typ 2 (30%)

### Was ist ein Hypervisor?

Ein **Hypervisor** ist eine Software oder Firmware, die mehrere **virtuelle Maschinen (VMs)** auf einer physischen Hardware ermöglicht.  
Er verwaltet und isoliert Ressourcen wie CPU, RAM und Speicher zwischen den VMs.

---

### Unterschied zwischen Typ 1 und Typ 2

| Merkmal | Typ 1 („Bare Metal“) | Typ 2 („Hosted“) |
|----------|----------------------|------------------|
| **Installation** | Direkt auf der Hardware | Läuft auf einem Host-Betriebssystem |
| **Beispiele** | VMware ESXi, Hyper-V, KVM | VirtualBox, VMware Workstation |
| **Leistung** | Höher, da direkter Hardwarezugriff | Etwas geringer wegen Host-Schicht |
| **Einsatz** | Server & Rechenzentren | Desktop & Tests |

---

### Kurzfassung

- **Typ 1:** läuft direkt auf der Hardware → schneller & sicherer  
- **Typ 2:** läuft auf einem Host-System → einfacher, aber langsamer


