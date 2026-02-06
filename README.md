# CDP-ATTACK
ENLACE DEL VIDEO DE EL ATTACK CDP
:https://www.youtube.com/watch?v=JHYX_xMqYiA&t=1s

CDP Flooding Attack Tool
 Descripción General
Herramienta de auditoría de seguridad que demuestra vulnerabilidades en el protocolo CDP (Cisco Discovery Protocol) mediante la generación masiva de paquetes CDP falsos. El script inyecta dispositivos ficticios en la tabla CDP de switches y routers Cisco, saturándola con cientos de entradas falsas.
⚠️ ADVERTENCIA: Esta herramienta es únicamente para fines educativos y pruebas de seguridad autorizadas en entornos controlados. El uso no autorizado en redes de producción es ilegal.

 Objetivo del Script
El script cdp.py realiza un ataque de CDP Flooding con los siguientes objetivos:

Saturar la tabla CDP del switch/router objetivo con dispositivos falsos
Generar cientos de vecinos CDP ficticios (NODE-XXX) de forma continua
Demostrar la vulnerabilidad del protocolo CDP cuando está habilitado
Consumir recursos del dispositivo de red procesando paquetes CDP falsos
Objetivo educativo: Concienciar sobre la importancia de deshabilitar CDP en interfaces no necesarias

Comportamiento del Ataque:

Genera dispositivos falsos con identificadores aleatorios (NODE-100 a NODE-999)
Envía tramas CDP con direcciones MAC aleatorias
Simula interfaces GigabitEthernet0/1 en cada dispositivo falso
Mantiene un flujo continuo de paquetes (intervalo de 0.02-0.08 segundos)
Cada dispositivo falso reporta capacidad de Router (0x01)


Capturas de Pantalla
1. Topología de Red
<img width="1099" height="655" alt="imagen" src="https://github.com/user-attachments/assets/823b1024-d835-4525-a355-f6904afcc997" />

2. Ejecución del Script
<img width="765" height="311" alt="imagen" src="https://github.com/user-attachments/assets/de837138-46e2-40cd-a23b-5faa38369b4a" />
Ejecución del script cdp.py desde Kali Linux con permisos root
3. Resultado del Ataque
<img width="1087" height="930" alt="imagen" src="https://github.com/user-attachments/assets/5d0e88dc-2674-47e0-988e-5ccf855677c0" />
Tabla CDP del switch mostrando 235 dispositivos falsos inyectados. Nótese la saturación con múltiples entradas NODE-XXX

Detalles de Conectividad:
DispositivoInterfazIPMáscaraVLANConectado aATACANTEeth011.98.1.3/241SW-1 (e0/0)SW-1e0/0--1ATACANTESW-1e0/1--1R1 (e0/0)SW-1e0/2--1VíctimaR1e0/011.98.1.1/241SW-1 (e0/1)Víctimae011.98.1.2/241SW-1 (e0/2)
Configuración de VLANs:

VLAN 1 (Default): Todos los dispositivos en la misma VLAN
Segmento de red: 11.98.1.0/24
Gateway: 11.98.1.1 (R1)


⚙️ Parámetros y Configuración
Parámetros Principales del Script:
pythonDST_CDP = "01:00:0c:cc:cc:cc"  # Dirección MAC multicast CDP
IFACE = "eth0"                  # Interfaz de red a utilizar
Campos TLV CDP Generados:
TLV TypeValor HexDescripciónContenido0x0001Device IDIdentificador del dispositivoNODE-XXX (100-999 aleatorio)0x0003Port IDInterfaz del dispositivoGigabitEthernet0/1 (fijo)0x0004CapabilityCapacidades del dispositivo0x01 (Router)
Parámetros de Transmisión:

Versión CDP: 2
TTL (Time to Live): 90-180 segundos (aleatorio)
Intervalo de envío: 0.02-0.08 segundos (aleatorio)
Dirección MAC origen: Aleatoria en cada paquete


📦 Requisitos del Sistema
Software Necesario:
bash# Sistema Operativo
- Linux (Kali Linux recomendado)
- Kernel 5.x o superior

# Python
- Python 3.7 o superior

# Librerías Python
- scapy >= 2.4.5
Instalación de Dependencias:
bash# Actualizar sistema
sudo apt update

# Instalar Python3 y pip
sudo apt install python3 python3-pip -y

# Instalar Scapy
sudo pip3 install scapy
Permisos Requeridos:

Permisos de root/sudo: Necesario para enviar tramas a nivel de enlace de datos
Acceso a la interfaz de red: La interfaz debe estar activa y conectada al switch objetivo

Verificación de Requisitos:
bash# Verificar versión de Python
python3 --version

# Verificar instalación de Scapy
python3 -c "import scapy; print(scapy.__version__)"

# Listar interfaces de red disponibles
ip addr show

# Verificar conectividad con el switch
ping 11.98.1.1

🚀 Uso de la Herramienta
Ejecución Básica:
bash# Navegar al directorio del script
cd /home/reily

# Ejecutar con permisos root
sudo python3 cdp.py
Salida Esperada:
[+] CUIDADO VOY PARA DENTRO: AVISO PARA TODOS
[+] REVISA EL SISTEMA QUE SE CALLO: REVISATE

[Enviando paquetes CDP continuamente...]
[Presionar Ctrl+C para detener]
Detener el Ataque:
bash# Presionar Ctrl+C para interrumpir
^C
[-] SE ACABO EL SHOW .
Configuración de la Interfaz:
Si necesitas cambiar la interfaz de red, edita la variable IFACE en el script:
pythonIFACE = "eth0"  # Cambiar por eth1, wlan0, etc.

🔍 Verificación del Ataque
En el Switch/Router Objetivo:
ciscoSwitch# show cdp neighbors

Device ID        Local Intrfce     Holdtme    Capability  Platform  Port ID
NODE-463         Eth 0/0          96          R                    Gig 0/1
NODE-364         Eth 0/0          137         R                    Gig 0/1
NODE-724         Eth 0/0          108         R                    Gig 0/1
NODE-331         Eth 0/0          83          R                    Gig 0/1
[... 148 entradas más ...]

Total cdp entries displayed : 152
Indicadores de Compromiso:

✅ Múltiples dispositivos con patrón NODE-XXX
✅ Todos reportan la misma interfaz (GigabitEthernet0/1)
✅ Holdtime varía aleatoriamente (90-180 segundos)
✅ Capability siempre es "R" (Router)
✅ Incremento rápido del número total de entradas CDP


🛡️ Medidas de Mitigación
1. Deshabilitar CDP Globalmente
Recomendado para redes que no requieren CDP:
ciscoRouter(config)# no cdp run
Esta es la medida más efectiva ya que deshabilita completamente el protocolo CDP en el dispositivo.
2. Deshabilitar CDP en Interfaces Específicas
Recomendado para interfaces de acceso (usuarios finales):
ciscoSwitch(config)# interface range ethernet 0/0 - 2
Switch(config-if-range)# no cdp enable
Switch(config-if-range)# exit
Mantener CDP solo en enlaces entre dispositivos de red:
cisco! Deshabilitar en puertos de acceso
Switch(config)# interface ethernet 0/0
Switch(config-if)# no cdp enable

Switch(config)# interface ethernet 0/2
Switch(config-if)# no cdp enable

! Mantener CDP en enlaces troncales (trunk)
Switch(config)# interface ethernet 0/1
Switch(config-if)# cdp enable  ! Enlace hacia R1
3. Implementar Port Security
Limitar el número de direcciones MAC por puerto:
ciscoSwitch(config)# interface ethernet 0/0
Switch(config-if)# switchport mode access
Switch(config-if)# switchport port-security
Switch(config-if)# switchport port-security maximum 2
Switch(config-if)# switchport port-security violation restrict
Switch(config-if)# switchport port-security mac-address sticky
4. Monitoreo y Detección de Anomalías
Configurar SNMP para monitoreo:
ciscoSwitch(config)# snmp-server enable traps cdp
Switch(config)# snmp-server host 11.98.1.100 version 2c PUBLIC
Script de monitoreo (ejemplo Python):
python#!/usr/bin/env python3
import time
from netmiko import ConnectHandler

# Umbral de alerta
MAX_CDP_NEIGHBORS = 10

device = {
    'device_type': 'cisco_ios',
    'host': '11.98.1.1',
    'username': 'admin',
    'password': 'password',
}

while True:
    connection = ConnectHandler(**device)
    output = connection.send_command('show cdp neighbors | count')
    count = int(output.strip())
    
    if count > MAX_CDP_NEIGHBORS:
        print(f"[ALERTA] Detectados {count} vecinos CDP - Posible ataque!")
    
    connection.disconnect()
    time.sleep(60)
5. Segmentación de Red con VLANs
Aislar segmentos críticos:
cisco! VLAN para servidores
Switch(config)# vlan 10
Switch(config-vlan)# name SERVIDORES

! VLAN para usuarios
Switch(config)# vlan 20
Switch(config-vlan)# name USUARIOS

! VLAN de gestión
Switch(config)# vlan 99
Switch(config-vlan)# name GESTION

! Asignar puertos
Switch(config)# interface ethernet 0/0
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 20
Switch(config-if)# no cdp enable
6. Filtrado de Tramas CDP (Avanzado)
Usar ACLs para filtrar tráfico CDP en firewalls:
cisco! Bloquear CDP en interfaces externas
access-list 100 deny udp any any eq 1985
access-list 100 permit ip any any
7. Auditoría y Logging
Habilitar logging detallado:
ciscoSwitch(config)# logging buffered 51200 debugging
Switch(config)# logging console critical
Switch(config)# service timestamps log datetime msec localtime

🔧 Análisis Técnico
Estructura de la Trama CDP:
Ethernet Frame:
├── Destination MAC: 01:00:0c:cc:cc:cc (CDP Multicast)
├── Source MAC: Random (cambia en cada paquete)
├── LLC Header:
│   ├── DSAP: 0xAA
│   ├── SSAP: 0xAA
│   └── Control: 0x03
├── SNAP Header:
│   ├── OUI: 0x00000C (Cisco)
│   └── Protocol ID: 0x2000 (CDP)
└── CDP Payload:
    ├── Version: 2
    ├── TTL: 90-180 (aleatorio)
    ├── Checksum: Calculado dinámicamente
    └── TLVs:
        ├── Port ID (0x0003): GigabitEthernet0/1
        ├── Device ID (0x0001): NODE-XXX
        └── Capabilities (0x0004): 0x01 (Router)
Función de Checksum:
El script implementa el algoritmo de checksum estándar de CDP:
pythondef checksum_cdp(data):
    if len(data) & 1:
        data += b'\x00'  # Padding si es impar
    total = 0
    for i in range(0, len(data), 2):
        total += (data[i] << 8) | data[i + 1]
    total = (total & 0xffff) + (total >> 16)
    total += (total >> 16)
    return (~total) & 0xffff
Impacto en el Dispositivo Objetivo:

Consumo de memoria: Cada entrada CDP consume ~200-500 bytes
Procesamiento CPU: El switch debe procesar cada paquete CDP
Tabla CDP saturada: Dificulta identificar dispositivos legítimos
Riesgo de inestabilidad: En dispositivos con recursos limitados


📊 Resultados Observados
Antes del Ataque:
Switch# show cdp neighbors
Device ID        Local Intrfce     Holdtme    Capability  Platform  Port ID
R1               Eth 0/1          148         R B         Linux Uni Eth 0/0

Total cdp entries displayed : 1
Durante el Ataque:
Switch# show cdp neighbors
Device ID        Local Intrfce     Holdtme    Capability  Platform  Port ID
NODE-463         Eth 0/0          96          R                    Gig 0/1
NODE-364         Eth 0/0          137         R                    Gig 0/1
NODE-724         Eth 0/0          108         R                    Gig 0/1
[... 149 entradas más ...]

Total cdp entries displayed : 152
Métricas del Ataque:

Tasa de inyección: ~12-50 paquetes/segundo
Dispositivos falsos generados: 100-999 (rango aleatorio)
Tiempo para saturar: ~30-60 segundos (dependiendo del hardware)
Persistencia: Continuo hasta detener el script (Ctrl+C)


⚠️ Impacto y Riesgos
Impacto Técnico:

Saturación de recursos:

Tabla CDP llena impide descubrir dispositivos legítimos
Consumo innecesario de CPU y memoria


Dificultad operativa:

Troubleshooting complicado
Imposible identificar topología real
Comandos show cdp neighbors inutilizables


Potencial para ataques adicionales:

Puede combinarse con ARP spoofing
Facilita Man-in-the-Middle
Reconocimiento de la red



Riesgos de Seguridad:

Negación de Servicio (DoS): Posible caída del switch/router
Enmascaramiento: Oculta presencia de atacantes reales
Prerequisito para MITM: Facilita otros ataques en capa 2


🔒 Contramedidas Recomendadas
Configuración Segura Básica:
cisco! ============================================
! CONFIGURACIÓN MÍNIMA DE SEGURIDAD CDP
! ============================================

! 1. Deshabilitar CDP en interfaces de acceso
interface range Ethernet0/0, Ethernet0/2
 description Puertos de Acceso
 switchport mode access
 no cdp enable
 exit

! 2. Mantener CDP solo en enlaces troncales
interface Ethernet0/1
 description Enlace a R1
 switchport mode trunk
 cdp enable
 exit

! 3. Port Security en puertos de acceso
interface range Ethernet0/0, Ethernet0/2
 switchport port-security
 switchport port-security maximum 2
 switchport port-security violation restrict
 switchport port-security aging time 5
 exit

! 4. Habilitar logging
logging buffered 51200
service timestamps log datetime msec
Configuración Avanzada:
cisco! Storm control para prevenir flooding
interface range Ethernet0/0, Ethernet0/2
 storm-control broadcast level 10.00
 storm-control multicast level 10.00
 exit

! DHCP Snooping (protección adicional)
ip dhcp snooping
ip dhcp snooping vlan 1
interface Ethernet0/1
 ip dhcp snooping trust
 exit

! Dynamic ARP Inspection
ip arp inspection vlan 1
interface Ethernet0/1
 ip arp inspection trust
 exit

📚 Referencias y Recursos
Documentación Oficial:

Cisco CDP Configuration Guide
Scapy Documentation
CDP Protocol Specification (RFC)

Herramientas Relacionadas:

Wireshark: Captura y análisis de tramas CDP
Yersinia: Framework de ataques de capa 2 (incluye CDP)
Scapy: Librería Python para manipulación de paquetes

Lecturas Recomendadas:

"Layer 2 Attacks and Mitigation Techniques" - Cisco Press
"Network Security Assessment" - O'Reilly
OWASP Network Security Testing Guide


🧪 Entorno de Pruebas
Configuración del Laboratorio:
Hardware/Software:

GNS3 / EVE-NG / Packet Tracer
Router Cisco (IOS 15.x)
Switch Cisco Catalyst
Kali Linux 2019.4 o superior

Red Aislada:
Segmento: 11.98.1.0/24
VLAN: 1 (Default)
Sin acceso a Internet

⚖️ Consideraciones Legales
IMPORTANTE:

✅ Permitido: Uso en laboratorios propios, redes de prueba autorizadas
✅ Permitido: Auditorías de seguridad con autorización escrita
✅ Permitido: Fines educativos en entornos controlados
❌ PROHIBIDO: Uso en redes de producción sin autorización
❌ PROHIBIDO: Ataques a infraestructura de terceros
❌ PROHIBIDO: Cualquier uso malicioso o no autorizado

El autor no se hace responsable del uso indebido de esta herramienta.

👤 Autor
Reily
Laboratorio de Seguridad en Redes - Demostración CDP Attack

📝 Licencia
Este proyecto es solo para fines educativos y de investigación en seguridad.
Uso responsable: Utilice esta herramienta únicamente en entornos autorizados y con fines legítimos de auditoría de seguridad.

🔄 Changelog
v1.0 (Febrero 2026)

Implementación inicial del CDP flooding attack
Generación de dispositivos falsos aleatorios (NODE-XXX)
Cálculo dinámico de checksum CDP
Randomización de direcciones MAC origen
TTL y timing aleatorios para evasión


📞 Soporte
Para dudas o consultas sobre el uso responsable de esta herramienta en entornos de laboratorio, consulte la documentación de Cisco o recursos de seguridad en redes.
Recursos adicionales:

Cisco Security Advisories
SANS Institute - Network Security
NIST Cybersecurity Framework


Última actualización: Febrero 2026
Versión: 1.0
Plataforma: Kali Linux / Python 3.x
