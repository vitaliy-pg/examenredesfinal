Proyecto Final de Redes II - La Restauración de la HoloRed Galáctica

Este proyecto representa la entrega completa de las Misiones Prácticas del Examen Final de Redes II: "La Restauración de la HoloRed Galáctica", simuladas en Cisco Packet Tracer.

🛰️ Misión 1: Reconexión en la Base Eco – Direccionamiento IP y Subredes
Red base asignada: 172.16.0.0/24
Requisitos:

Departamento	Nº de hosts aproximado	Subred sugerida	IP inicial	Broadcast
Comando Central	50	/26 (64 IPs)	172.16.0.0	172.16.0.63
Defensa Perimetral	30	/27 (32 IPs)	172.16.0.64	172.16.0.95
Centro Médico	20	/27 (32 IPs)	172.16.0.96	172.16.0.127
Hangar y Taller	14	/28 (16 IPs)	172.16.0.128	172.16.0.143
Enlace a antena	2	/30 (4 IPs)	172.16.0.144	172.16.0.147

⚠️ Quedan IPs libres para futuras ampliaciones (172.16.0.148 - 172.16.0.255)

Resumen: Se usó subnetting con VLSM para no desperdiciar espacio, asignando a cada departamento una subred eficiente según sus necesidades. El enlace con la antena interplanetaria usa una red /30, ideal para enlaces punto a punto.

📡 Misión 2: Sabiduría de Yoda – Algoritmos de Enrutamiento y Rutas
Comparación:

Enrutamiento	Estático	Dinámico (ej. OSPF)
Configuración	Manual, fija por el administrador	Automática, adaptativa
Escalabilidad	Baja (difícil en redes grandes)	Alta (ideal para múltiples routers)
Convergencia	No se adapta ante fallos	Recalcula rutas automáticamente
Seguridad	Más controlable	Más vulnerable si no se filtra bien

Protocolos dinámicos:

RIP: vector de distancia, fácil pero lento y limitado.

OSPF: estado de enlace, rápido y escalable, ideal para redes grandes como la HoloRed.

Respuesta a fallos:

Estático: necesita intervención humana.

Dinámico (OSPF): detecta cambios y reconfigura rutas automáticamente.

🌐 Misión 3: Los Nombres del Holonet – DNS y Resolución de Nombres
Funcionamiento del DNS:

Traduce nombres simbólicos (como echo.base) a direcciones IP (como 192.168.10.5).

Utiliza registros A que asocian un nombre a una IP.

Ejemplo:

txt
Copiar
Editar
Nombre: holonet.rebelion.org  
Dirección: 172.20.10.15  
Registro: tipo A
Proceso de resolución:

El host solicita la IP a su servidor DNS configurado.

El DNS responde con la IP si tiene el registro.

El host usa esa IP para conectarse al destino.

Importancia:

Sin DNS no se podrían usar nombres fáciles de recordar.

Si el servidor DNS falla, no se resuelven nombres → fallo en acceso a servicios.

🔁 Misión 4: “Es una trampa… de protocolos!” – TCP vs UDP
Característica	TCP	UDP
Conexión	Orientado a conexión (3-way handshake)	No orientado a conexión
Fiabilidad	Garantiza entrega y orden	No garantiza entrega
Corrección de errores	Sí	No
Velocidad	Más lento	Muy rápido

Ejemplos galácticos:

TCP: Transferencia de los planos de la Estrella de la Muerte, transmisión de comandos críticos.

UDP: Videovigilancia X-Wing en combate, audio en tiempo real, coordenadas en vivo.

Resumen:
TCP es confiable y seguro, ideal para datos críticos. UDP es rápido, ideal para tiempo real donde se tolera pérdida.

🔐 Misión 5: Comunicación Segura – Criptografía
Cifrado simétrico

Mismo secreto para cifrar y descifrar.

Ejemplo: Leia y Luke comparten una frase clave.

Rápido, pero difícil de escalar en grandes redes.

Cifrado asimétrico (clave pública/privada)

Se cifra con clave pública y se descifra con la privada.

Ideal para entornos donde no hay clave compartida previa.

Usado en protocolos como SSH.

Autenticación y no repudio:

Verifica la identidad de quien envía.

Garantiza que el mensaje no fue alterado.

Importancia de usar SSH:

Telnet envía credenciales sin cifrar (inseguro).

SSH cifra toda la sesión, ideal para configurar routers remotamente sin riesgo de espionaje imperial.

✅ Conclusión
La restauración de la HoloRed galáctica ha requerido tanto habilidades de configuración como conocimientos teóricos sólidos. A través de subnetting, protocolos de enrutamiento, servicios DNS, transporte eficiente y criptografía segura, se ha garantizado una red robusta, escalable y protegida para la Alianza Rebelde.

"El conocimiento de redes es como la Fuerza, joven padawan: invisible, pero esencial para restaurar la paz en la galaxia."
— Maestro Yoda


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
