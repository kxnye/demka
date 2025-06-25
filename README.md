# demka

![image](https://github.com/user-attachments/assets/4ac2c6e0-c0c0-4773-8940-aa9368388615)

3. Конфигурация роутеров (ecorouter CLI)

3.1 HQ-RTR (vm id 101)
! -------- WAN (global) ----------
interface global
 ip address 172.16.4.2/24
 ip nat outside             ! ← отмечаем как внешнюю

port ge0
 service-instance global
  encapsulation untagged
  connect ip interface global

! -------- LAN ----------
interface lan
 ip address 10.10.10.1/24
 ip nat inside              ! ← отмечаем как внутреннюю

port ge1
 service-instance lan
  encapsulation untagged
  connect ip interface lan

! -------- Маршрутизация ----------
ip route 0.0.0.0/0 172.16.4.1

! -------- NAT (пат-адресация) ----
ip access-list standard LAN
 permit 10.10.10.0 0.0.0.255

ip nat source list LAN interface global overload


3.2 BR-RTR (vm id 102)
interface global
 ip address 172.16.5.2/24
 ip nat outside

port ge0
 service-instance global
  encapsulation untagged
  connect ip interface global

interface lan
 ip address 10.20.20.1/24
 ip nat inside

port ge1
 service-instance lan
  encapsulation untagged
  connect ip interface lan

ip route 0.0.0.0/0 172.16.5.1

ip access-list standard LAN
 permit 10.20.20.0 0.0.0.255

ip nat source list LAN interface global overload

Проверка с роутера
# ping 8.8.8.8
# ping 172.16.4.1          (или .5.1 — для branch)
# show ip nat translations
# traceroute 8.8.8.8

![image](https://github.com/user-attachments/assets/17a6626d-c21b-4539-8db6-68dda02eb044)

5. Дополнительные замечания
Между HQ-LAN и BR-LAN
Через NAT-схему они друг друга не увидят.
Если нужна L3-связь (без выхода в Интернет) – уберите NAT и пропишите статические маршруты на ISP-VM:
ip route 10.10.10.0/24 172.16.4.2
ip route 10.20.20.0/24 172.16.5.2

…или поднимите VPN между двумя роутерами.

Performance
Если после включения virtio пакеты «не ездят», скорее всего в гостевой ОС нет модуля virtio_net – тогда оставляйте e1000.

Безопасность в Proxmox
Для WAN-bridge (vmbr1/vmbr2) имеет смысл включить опцию firewall = 1 на уровне VNIC и запретить всё, кроме ICMP/80/443 наружу. Но сначала добейтесь базовой связности.

VLAN aware true понадобится только если:

хотите на одном физическом интерфейсе хост-системы (ens33) вести и WAN, и несколько клиентских сетей;

или планируете «раздать» один bridge нескольким VLAN внутри Proxmox.

Кратко
VLAN Aware ― оставьте выключенным, пока используете по мосту на каждую сеть.

Тип сетевого адаптера можно поменять на virtio ради скорости, но это не влияет на отсутствие пингов.

На обоих роутерах добавьте default-route к ISP-VM и NAT (inside/outside + overload).

На серверах пропишите адрес/маску/шлюз/DNS из своей LAN-подсети.

После этого пинг до 8.8.8.8 с клиентов и роутеров должен работать.

Если что-то всё ещё «не идёт» – проверьте таблицы ARP/фильтры Proxmox и покажите вывод show ip route и show ip nat с роутеров.


//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

interface global
ip add 172.16.4.2/24
service-instance global
encapsulation untagged
connect ip int global
ip route 0.0.0.0.0/0 172.16.4.1

int lan
ip add 10.10.10.1/24
service-instance lan
encapsulation untagged
connect ip int lan

//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////


