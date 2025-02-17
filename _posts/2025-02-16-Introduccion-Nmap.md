---
Title: Introduccion a Nmap
autor: KR4VEN
---
# Guía Completa de Comandos de Nmap

Nmap (Network Mapper) es una herramienta de código abierto utilizada para el escaneo de redes y auditoría de seguridad. Esta guía detalla los comandos más importantes, explicando su sintaxis, opciones y ejemplos prácticos.

---

## ✨ 1. Comandos Básicos de Nmap

### Escaneo simple de una IP
```bash
nmap 192.168.1.1
```
Escanea los **1,000 puertos más comunes** en la dirección IP proporcionada.

### Escaneo de un rango de IPs
```bash
nmap 192.168.1.0/24
```
Escanea todo el rango de direcciones desde `192.168.1.0` hasta `192.168.1.255`.

### Escaneo de dominios
```bash
nmap ejemplo.com
```
Escanea el dominio especificado, resolviendo la dirección IP.

---

## 🔍 2. Tipos de Escaneo

### Escaneo TCP Connect (-sT)
```bash
nmap -sT 192.168.1.1
```
Utiliza una conexión completa TCP (3-way handshake). Es más fácil de detectar por los firewalls.

### Escaneo SYN (Half-Open) (-sS)
```bash
nmap -sS 192.168.1.1
```
Escaneo rápido y discreto. No completa el handshake, por lo que es **menos detectable**.

### Escaneo UDP (-sU)
```bash
nmap -sU 192.168.1.1
```
Detecta servicios UDP (como DNS, SNMP o DHCP). Más lento que el escaneo TCP.

### Escaneo de Puertos Específicos (-p)
```bash
nmap -p 80,443,8080 192.168.1.1
```
Escanea puertos específicos (en este caso, 80, 443 y 8080).

Para escanear todos los **65535 puertos**:
```bash
nmap -p- 192.168.1.1
```

### Escaneo de Puertos en Rango
```bash
nmap -p 20-100 192.168.1.1
```
Escanea los puertos en el rango del 20 al 100.

---

## ⚖️ 3. Detección de Servicios y Sistemas Operativos

### Detectar servicios y versiones (-sV)
```bash
nmap -sV 192.168.1.1
```
Identifica los **servicios** en cada puerto abierto y las **versiones**.

### Detectar el sistema operativo (-O)
```bash
nmap -O 192.168.1.1
```
Intenta identificar el sistema operativo mediante el análisis de las respuestas del protocolo TCP/IP.

Combinación de ambos:
```bash
nmap -sV -O 192.168.1.1
```

---

## 🚀 4. Técnicas de Evasión y Sigilo

### Modificar el tamaño de los paquetes (-mtu)
```bash
nmap --mtu 16 192.168.1.1
```
Ajusta el **MTU (Maximum Transmission Unit)** para fragmentar los paquetes y evadir firewalls.

### Usar puertos de origen falsos (--source-port)
```bash
nmap --source-port 53 192.168.1.1
```
Disfraza el escaneo como si proviniera de un **puerto común** (como DNS: 53).

### Usar decoys para anonimizar (-D)
```bash
nmap -D RND:5 192.168.1.1
```
Utiliza **5 IPs falsas** aleatorias (decoys) para ocultar tu dirección real.

---

## ⌚ 5. Optimización de Escaneos

### Ajustar la velocidad (-T)
```bash
nmap -T4 192.168.1.1
```
- `-T0` : Muy lento (modo paranoico).
- `-T4` : Rápido (ideal para escaneos normales).
- `-T5` : Agresivo (máxima velocidad, más detectable).

### Limitar el número de intentos (--max-retries)
```bash
nmap --max-retries 2 192.168.1.1
```
Reduce los intentos de retransmisión, acelerando el escaneo en redes inestables.

---

## 📃 6. Guardar Resultados

### Guardar en formato normal (-oN)
```bash
nmap -oN resultado.txt 192.168.1.1
```

### Guardar en formato XML (-oX)
```bash
nmap -oX resultado.xml 192.168.1.1
```

### Guardar en todos los formatos (-oA)
```bash
nmap -oA resultado 192.168.1.1
```
Guarda los resultados en **normal, XML y Grepable** (para automatización).

---

## 🔒 7. Escaneos Avanzados

### Detectar hosts activos (Ping Scan) (-sn)
```bash
nmap -sn 192.168.1.0/24
```
Muestra qué dispositivos están encendidos en la red sin escanear puertos.

### Escanear IPv6 (-6)
```bash
nmap -6 2001:0db8::1
```
Escanea direcciones **IPv6**.

### Escanear tras un proxy SOCKS4 (--proxies)
```bash
nmap --proxies socks4://127.0.0.1:9050 192.168.1.1
```
Usa un **proxy SOCKS4** (como Tor) para anonimizar el escaneo.

---

## 📊 8. Escaneos de Vulnerabilidades

### Usar scripts de Nmap (NSE)

#### Buscar vulnerabilidades conocidas
```bash
nmap --script vuln 192.168.1.1
```
Utiliza scripts para detectar vulnerabilidades comunes.

#### Escanear por malware
```bash
nmap --script malware 192.168.1.1
```
Detecta la presencia de **malware** en servicios expuestos.

---

## 📊 9. Ejemplos Combinados

### Escaneo completo y detallado
```bash
nmap -sS -sV -O -p- -T4 192.168.1.1
```
Escanea **todos los puertos**, detecta **servicios y sistema operativo**, optimizando la velocidad.

### Escaneo sigiloso y anonimizado
```bash
nmap -sS -D RND:10 --source-port 53 --mtu 8 192.168.1.1
```
Escaneo discreto con **decoys, puerto falso** y **fragmentación**.

---

## 🛠️ 10. Recursos Adicionales

- Documentación oficial de Nmap: [https://nmap.org](https://nmap.org)
- Repositorio de scripts NSE: [https://nmap.org/nsedoc/](https://nmap.org/nsedoc/)

---

