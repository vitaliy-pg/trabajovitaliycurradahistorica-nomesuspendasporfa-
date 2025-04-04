# trabajovitaliycurradahistorica-nomesuspendasporfa-

link de git hub :  https://github.com/vitaliy-pg/trabajovitaliycurradahistorica-nomesuspendasporfa-.git

# Examen de Redes II – En Busca de la Red Perdida

Este repositorio contiene la documentación y los archivos de configuración desarrollados para el examen de Redes II. Se incluyen dos escenarios prácticos realizados en Cisco Packet Tracer que simulan la reconstrucción de una red perdida y la segmentación de una ciudad mediante VLANs.

- **Ejercicio 1:** La Ruta Perdida entre Dos Reinos (archivo: `CiudadesVitaliy.pkt`)
- **Ejercicio 2:** La Ciudad de las Redes Aisladas (archivo: `ejercicio2Vitaliy.pkt`)

---

## Contenido

- [Ejercicio 1: La Ruta Perdida entre Dos Reinos](#ejercicio-1-la-ruta-perdida-entre-dos-reinos)
- [Ejercicio 2: La Ciudad de las Redes Aisladas](#ejercicio-2-la-ciudad-de-las-redes-aisladas)
- [Requerimientos y Equipos Utilizados](#requerimientos-y-equipos-utilizados)
- [Evidencias y Pruebas](#evidencias-y-pruebas)
- [Conclusión](#conclusión)

---

## Ejercicio 1: La Ruta Perdida entre Dos Reinos

### Descripción General

En este ejercicio se reconstruyó la conexión entre dos ciudades (reinos) históricas, cada una representada por una red LAN independiente. Cada ciudad dispone de su propio router, switch y PCs. El enlace WAN entre los routers permite la comunicación entre ambas redes, restaurando la antigua "Ruta Sagrada de Datos".

### Topología

- **Ciudad A:**  
  - **Router:** Cisco 1941 (o similar)  
  - **Switch:** Cisco 2960  
  - **PCs:** 2 (por ejemplo, PC_A1 y PC_A2)  
  - **Subred LAN:** 192.168.10.0/24  
  - **Gateway del Router:** 192.168.10.254  

- **Ciudad B:**  
  - **Router:** Cisco 1941 (o similar)  
  - **Switch:** Cisco 2960  
  - **PCs:** 2 (por ejemplo, PC_B1 y PC_B2)  
  - **Subred LAN:** 192.168.20.0/24  
  - **Gateway del Router:** 192.168.20.254  

- **Enlace WAN entre routers:**  
  - **Subred punto a punto:** 192.168.30.0/30  
  - **Interfaces:**  
    - En Ciudad A (Router A): 192.168.30.1  
    - En Ciudad B (Router B): 192.168.30.2  

### Configuración

#### Direccionamiento IP

- **Ciudad A:**  
  - PCs: Direcciones en 192.168.10.0/24 (ej. 192.168.10.10 y 192.168.10.11)  
  - Router: Interfaz LAN configurada en 192.168.10.254/24  

- **Ciudad B:**  
  - PCs: Direcciones en 192.168.20.0/24 (ej. 192.168.20.10 y 192.168.20.11)  
  - Router: Interfaz LAN configurada en 192.168.20.254/24  

- **Enlace WAN:**  
  - Router A: 192.168.30.1/30  
  - Router B: 192.168.30.2/30  

#### Rutas Estáticas

- **En el Router de Ciudad A:**
  ```bash
  ip route 192.168.20.0 255.255.255.0 192.168.30.2
En el Router de Ciudad B:

bash
Copiar
ip route 192.168.10.0 255.255.255.0 192.168.30.1
Alternativamente, se pueden usar rutas por defecto en cada router, considerando que solo existe una salida para alcanzar la red remota.

Pruebas de Conectividad
Realizar un ping desde un PC en Ciudad A (ej. 192.168.10.10) a un PC en Ciudad B (ej. 192.168.20.10).

Verificar que la comunicación funcione de forma bidireccional (Ciudad B a Ciudad A).

Ejercicio 2: La Ciudad de las Redes Aisladas
Descripción General
Este ejercicio simula la segmentación de una ciudad en dos gremios mediante el uso de VLANs, lo que permite aislar el tráfico de cada grupo. Se implementa un router-on-a-stick para interconectar las diferentes VLANs y permitir la comunicación entre ellas, manteniendo el aislamiento del tráfico local.

Topología
Switch Central: Cisco 2960

VLAN 10 (Arquitectos):

PCs: Ej. PC_A1, PC_A2

Subred: 192.168.10.0/24

VLAN 20 (Escribas):

PCs: Ej. PC_E1, PC_E2

Subred: 192.168.20.0/24

Puerto Trunk: Conecta el switch al router, permitiendo el paso del tráfico de ambas VLANs.

Router (Gran Torre Central): Cisco 1941 (o similar)

Conexión al switch mediante la interfaz FastEthernet 0/0

Subinterfaces configuradas:

Fa0/0.10 para VLAN 10 con IP 192.168.10.1/24

Fa0/0.20 para VLAN 20 con IP 192.168.20.1/24

Configuración
En el Switch (Cisco 2960)
Creación de VLANs:

bash
Copiar
vlan 10
 name Arquitectos
exit
vlan 20
 name Escribas
exit
Asignación de puertos a cada VLAN:

bash
Copiar
interface range fa0/2 - 3
 switchport mode access
 switchport access vlan 10
exit
interface range fa0/4 - 5
 switchport mode access
 switchport access vlan 20
exit
Configuración del puerto troncal (por ejemplo, fa0/1):

bash
Copiar
interface fa0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,20
exit
En el Router (Router-on-a-Stick)
Habilitar la interfaz física conectada al switch:

bash
Copiar
interface fastEthernet 0/0
 no shutdown
exit
Configurar las subinterfaces:

Para VLAN 10:

bash
Copiar
interface fastEthernet 0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
exit
Para VLAN 20:

bash
Copiar
interface fastEthernet 0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
exit
Configuración de los PCs
VLAN 10 (Arquitectos):

Asignar direcciones en la red 192.168.10.0/24 (ej. 192.168.10.10 y 192.168.10.11)

Gateway: 192.168.10.1

VLAN 20 (Escribas):

Asignar direcciones en la red 192.168.20.0/24 (ej. 192.168.20.10 y 192.168.20.11)

Gateway: 192.168.20.1

Pruebas de Conectividad
Realizar ping entre PCs dentro de la misma VLAN para confirmar la segmentación local.

Realizar ping entre PCs de diferentes VLANs para confirmar el correcto funcionamiento del router-on-a-stick.

Requerimientos y Equipos Utilizados
Ejercicio 1: La Ruta Perdida entre Dos Reinos
Routers: Cisco 1941 o Cisco 4321 (según disponibilidad)

Switches: Cisco 2960

Dispositivos Finales: PCs (2 por ciudad)

Medio de conexión WAN: Enlace serial o cable cruzado para el enlace punto a punto entre routers

Ejercicio 2: La Ciudad de las Redes Aisladas
Router: Cisco 1941 o Cisco 4321 (configurado como router-on-a-stick)

Switch: Cisco 2960

Dispositivos Finales: PCs (mínimo 2 por VLAN)

Segmentación de red: Implementación de VLANs (10 y 20) y puerto troncal entre switch y router

Evidencias y Pruebas
Para la verificación del correcto funcionamiento de ambos escenarios se incluyen:

Archivos Packet Tracer (.pkt):

CiudadesVitaliy.pkt para el Ejercicio 1

ejercicio2Vitaliy.pkt para el Ejercicio 2

Capturas de pantalla y registros de comandos:
Se recomienda incluir evidencias que muestren la configuración de routers y switches, así como la salida de los pings exitosos.

Diagrama de Topología (opcional):
Un diagrama esquemático que ilustre la interconexión de dispositivos, subredes y enlaces WAN/troncal.

Conclusión
Esta documentación integra los aspectos teóricos y prácticos desarrollados en el examen de Redes II. Se ha detallado la configuración de rutas estáticas, la segmentación mediante VLANs y la implementación del router-on-a-stick para interconectar redes aisladas. El enfoque narrativo se ha combinado con instrucciones técnicas precisas para garantizar una solución integral y profesional.

¡Buena suerte en tu travesía por el mundo de las redes!


# Examen de Redes II – En Busca de la Red Perdida (Parte Teórica)

Bienvenido a la documentación de la **Parte Teórica** del examen "En Busca de la Red Perdida". En esta sección se exploran conceptos fundamentales de redes a través de una narrativa inmersiva, donde se han planteado cinco enigmas que combinan historia y tecnología. A continuación se detalla cada enigma junto con su interpretación y respuesta en términos de redes modernas.

---

## 1. El Mural de las Siete Capas

### Enunciado Narrativo
Al ingresar a la sala principal del templo, descubres un gran mural formado por siete franjas horizontales, cada una decorada con símbolos y jeroglíficos. Estas franjas representan etapas en un ritual de comunicación, simbolizando la evolución y transformación del mensaje desde su origen hasta su destino.

### Interpretación y Respuesta
Este mural es una representación del **Modelo OSI** (Open Systems Interconnection), que es un marco teórico para la interconexión de sistemas en redes. Cada capa del modelo OSI cumple funciones específicas en el proceso de comunicación:

- **Capa 1 – Física:** Transmisión de bits a través de medios físicos.
- **Capa 2 – Enlace de Datos:** Manejo de la comunicación directa y detección de errores en el enlace.
- **Capa 3 – Red:** Encargada del direccionamiento lógico y el enrutamiento de los datos.
- **Capa 4 – Transporte:** Garantiza la entrega confiable o no confiable de los datos (por ejemplo, TCP y UDP).
- **Capa 5 – Sesión:** Administra y controla las sesiones de comunicación.
- **Capa 6 – Presentación:** Se encarga de la traducción, cifrado y compresión de datos.
- **Capa 7 – Aplicación:** Proporciona servicios de red a las aplicaciones del usuario.

El proceso narrativo ilustra cómo un mensaje se "refina" y transforma a medida que avanza por cada una de estas capas, de la misma forma en que ocurre en la comunicación de datos actual.

---

## 2. Los Dos Pergaminos del Mensajero

### Enunciado Narrativo
En una cámara oculta se encuentran dos pergaminos: uno describe el **Ritual del Mensajero Confiable**, que incluye un saludo de tres pasos, entrega del mensaje y espera de confirmación; el otro relata el **Ritual del Mensajero Veloz**, en el que el mensajero envía mensajes sin asegurar la recepción.

### Interpretación y Respuesta
Estos rituales corresponden a dos protocolos de comunicación modernos:

- **Mensajero Confiable:** Se asemeja a **TCP (Transmission Control Protocol)**, que establece una conexión mediante un saludo de tres vías (three-way handshake), garantiza la entrega de los datos mediante confirmaciones y reenvío en caso de pérdida.  
  **Ventajas:** Fiabilidad y control de errores.  
  **Desventajas:** Mayor latencia y sobrecarga en el proceso de establecimiento de conexión.

- **Mensajero Veloz:** Es similar a **UDP (User Datagram Protocol)**, que envía los datos sin establecer una conexión previa ni confirmar la recepción, permitiendo una transmisión rápida pero sin garantía de entrega.  
  **Ventajas:** Baja latencia y menor sobrecarga.  
  **Desventajas:** No garantiza la entrega de los datos, lo que puede ocasionar pérdidas.

---

## 3. El Enigma de las Subredes

### Enunciado Narrativo
Avanzando por un pasillo, encuentras una losa de piedra con inscripciones numéricas: "Nuestro reino digital tenía la dirección sagrada 192.168.50.0" y se menciona que cuatro gremios necesitaban su propio distrito, todos de igual tamaño.

### Interpretación y Respuesta
Para dividir la red 192.168.50.0/24 en 4 subredes de igual tamaño, es necesario utilizar una máscara que permita crear 4 subredes. Esto se logra tomando 2 bits prestados de la parte de host, ya que \( 2^2 = 4 \).

- **Máscara Original:** /24  
- **Nueva Máscara:** /26 (24 + 2 bits para subredes)

Cada subred en una red /26 tendrá:
- **Total de direcciones:** 64 (2^(32-26) = 64)  
- **Direcciones utilizables para hosts:** 62 (64 - 2, descontando la dirección de red y la de broadcast)

---

## 4. La Encrucijada de las Rutas

### Enunciado Narrativo
En una encrucijada, un tótem tallado con flechas apunta hacia diferentes caminos. Algunas flechas están fijas en la piedra, mientras que otras pueden moverse para reorientar la ruta según las condiciones.

### Interpretación y Respuesta
El tótem representa la **tabla de enrutamiento** de un router. Una tabla de enrutamiento contiene entradas que indican las rutas disponibles para alcanzar redes remotas.  
- **Flechas fijas en piedra:** Simbolizan **rutas estáticas**, configuradas manualmente y que permanecen inmutables hasta ser cambiadas explícitamente.  
- **Flechas móviles:** Representan **rutas dinámicas**, las cuales son aprendidas y ajustadas automáticamente mediante protocolos de enrutamiento (como OSPF, EIGRP, o RIP).

---

## 5. El Guardián de la Máscara Única

### Enunciado Narrativo
Frente a la salida del templo, se encuentra una estatua con dos caras. Según la leyenda, este guardián intercambiaba la máscara original de los mensajeros por la suya propia al salir de la ciudad, haciendo que todas las comunicaciones parecieran provenir de él.

### Interpretación y Respuesta
Esta leyenda refleja la técnica de **NAT (Network Address Translation)**, en particular la variante conocida como **PAT (Port Address Translation)**.  
- **Funcionamiento:** NAT permite que múltiples dispositivos internos con direcciones IP privadas compartan una única dirección IP pública al comunicarse con el exterior.  
- **Beneficios:**  
  1. **Conservación de direcciones IP:** Se utiliza una única dirección pública para varios dispositivos.  
  2. **Seguridad:** Oculta la topología interna de la red, protegiendo a los dispositivos internos de accesos directos desde Internet.

---

## Conclusión

La parte teórica del examen nos ha llevado a descubrir y analizar conceptos esenciales en redes, desde el modelo OSI y los protocolos TCP/UDP, hasta la segmentación de subredes, la función de las tablas de enrutamiento y la implementación de NAT. Cada enigma y su respuesta demuestran la importancia de comprender y aplicar estos fundamentos para diseñar y mantener redes modernas de forma efectiva y segura.

¡Adelante, explorador tecnológico, y continúa tu travesía hacia la maestría en redes!
