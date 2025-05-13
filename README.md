Proyecto Final de Redes II - La Restauración de la HoloRed Galáctica

Este proyecto representa la entrega completa de las Misiones Prácticas del Examen Final de Redes II: "La Restauración de la HoloRed Galáctica", simuladas en Cisco Packet Tracer.

🔹 Misión 6: Red Troncal de la Alianza (Hoth – HomeOne – Endor)

📊 Diagrama Topológico

Tres routers (Router0, Router1, Router2) interconectados como troncal:

Router0 ➞ Hoth

Router1 ➞ HomeOne (nodo central)

Router2 ➞ Endor

Cada router tiene una LAN con PCs conectados mediante switches Cisco 2960.

🔠 Subnetting aplicado (con VLSM)

Segmento

Red

Máscara

Rango de Hosts

Broadcast

Hoth LAN

10.0.1.0

/26 (255.255.255.192)

10.0.1.1 - 10.0.1.62

10.0.1.63

HomeOne LAN

10.0.1.64

/27 (255.255.255.224)

10.0.1.65 - 10.0.1.94

10.0.1.95

Endor LAN

10.0.1.96

/27

10.0.1.97 - 10.0.1.126

10.0.1.127

Hoth ➔ HomeOne

10.0.0.0

/30

10.0.0.1 - 10.0.0.2

10.0.0.3

HomeOne ➔ Endor

10.0.0.4

/30

10.0.0.5 - 10.0.0.6

10.0.0.7

⚙️ Configuración de Routers (ejemplo para Router0 - Hoth)

hostname Router0
interface g0/0
 ip address 10.0.1.1 255.255.255.192
 no shutdown
interface s0/0/0
 ip address 10.0.0.1 255.255.255.252
 no shutdown
router ospf 1
 router-id 1.1.1.1
 network 10.0.1.0 0.0.0.63 area 0
 network 10.0.0.0 0.0.0.3 area 0

(Configuración similar adaptada para Router1 y Router2)

🔢 Verificación:

ping exitoso entre cualquier PC de Hoth, HomeOne y Endor

show ip route muestra rutas OSPF

show ip ospf neighbor confirma vecinos

🔹 Misión 7: La Baliza Secreta de Coruscant (DNS + SSH)

📊 Topología

Router3 (Coruscant) conectado a Server0 y PC6 mediante Switch2

Router4 (Flota) conectado a PC7 (operador rebelde)

Enlace punto a punto entre Router3 y Router4

🔠 Direccionamiento

Dispositivo

IP

Máscara

Gateway

DNS

Server0

192.168.50.100

255.255.255.0

192.168.50.1

-

PC6 (Bothan)

192.168.50.50

255.255.255.0

192.168.50.1

192.168.50.100

Router3 (LAN)

192.168.50.1

255.255.255.0

-

-

Router3 (WAN)

10.0.0.9

255.255.255.252

-

-

Router4 (WAN)

10.0.0.10

255.255.255.252

-

-

Router4 (LAN)

192.168.60.1

255.255.255.0

-

-

PC7 (control)

192.168.60.50

255.255.255.0

192.168.60.1

192.168.50.100

⚙️ Configuración de Router3 (Coruscant)

hostname Router3
interface g0/0
 ip address 192.168.50.1 255.255.255.0
 no shutdown
interface s0/0/0
 ip address 10.0.0.9 255.255.255.252
 no shutdown
ip route 0.0.0.0 0.0.0.0 10.0.0.10
! SSH
ip domain-name rebelion.org
crypto key generate rsa modulus 1024
username admin secret S3cur3P@ss
line vty 0 4
 transport input ssh
 login local

⚙️ Configuración de Router4 (Flota)

hostname Router4
interface g0/0
 ip address 192.168.60.1 255.255.255.0
 no shutdown
interface s0/0/0
 ip address 10.0.0.10 255.255.255.252
 no shutdown
ip route 192.168.50.0 255.255.255.0 10.0.0.9

🔢 DNS en Server0

Activado en pestaña Services > DNS

Registro A:

Nombre: planes.secretos

IP: 192.168.50.100

🔢 Servicio HTTP

Activado

Archivo index.html personalizado con mensaje rebelde

🌐 Verificación desde PC7:

ping 192.168.50.100
ping planes.secretos
nslookup planes.secretos

Acceso Web: http://planes.secretos

Acceso SSH: Terminal > SSH > Host: 10.0.0.9, user: admin, pass: S3cur3P@ss
