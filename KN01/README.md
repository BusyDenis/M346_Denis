## KN01 – Hypervisor Typ 1 und Typ 2

### A) Hypervisor Typ 1 und Typ 2 (30%)

**Hypervisor:** Software, die mehrere virtuelle Maschinen (VMs) auf einer Hardware verwaltet.

**Typ 1:** Läuft direkt auf der Hardware (z.B. VMware ESXi).  
**Typ 2:** Läuft auf einem bestehenden Betriebssystem (z.B. VirtualBox).

Typ 1 ist schneller und wird für Server genutzt,  
Typ 2 ist einfacher für Tests am Desktop.


B) Unterschied Hypervisor 1 und Hypervisor 2

## Virtualisierungssoftware

![Screenshot 1](Screenshot%202025-11-11%20135113%202.png)

![Screenshot 2](Screenshot%202025-11-11%20134900%202.png)

![Screenshot 3](Screenshot%202025-11-11%20134652%202.png)

Ich vermute, dass mein System einen **Typ-2-Hypervisor** verwendet.  
Ein Hypervisor des Typs 2 läuft auf einem **Host-Betriebssystem**, auf dem die Virtualisierungssoftware installiert ist.  
In meinem Fall verwende ich **VMware** auf meinem Laptop, wodurch klar ist, dass das virtuelle System nicht direkt auf der Hardware läuft, sondern über den Host gesteuert wird.

---

## Erklärung

Die Fehlermeldungen entstehen, weil der **Hypervisor Typ 2** auf die **Hardware-Ressourcen des Host-Systems** zugreift.  
Er kann nur so viele CPU-Kerne und Arbeitsspeicher verwenden, wie der Host tatsächlich besitzt.  
Wenn ich also versuche, der VM mehr Ressourcen zuzuweisen, als physisch vorhanden sind, kann VMware diese nicht nutzen.

Ein **Typ-1-Hypervisor** hätte diese Einschränkung nicht, da er direkt auf der Hardware läuft und selbst über die Verteilung der Ressourcen entscheidet.  
In meinem Fall bestätigt das Verhalten, dass es sich eindeutig um einen **Typ-2-Hypervisor** handelt.

