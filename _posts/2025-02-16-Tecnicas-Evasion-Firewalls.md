---
Title: Técnicas de evasión de Firewalls 
autor: KR4VEN
---

# Técnicas de Evasión de Firewalls con Nmap


## 1. **MTU (Maximum Transmission Unit) – `--mtu`**

La técnica de evasión mediante **MTU** consiste en ajustar el tamaño de los paquetes que se envían para evitar que el firewall los detecte como parte de un escaneo.

### ¿Cómo funciona?
Los firewalls suelen inspeccionar los paquetes de red en busca de patrones específicos. Al dividir los paquetes en fragmentos más pequeños, se puede evitar que el firewall los reconozca como un escaneo.

### Uso en Nmap:
```bash
nmap --mtu 16 192.168.1.1
```

En este ejemplo, los paquetes se dividirán en fragmentos de **16 bytes**.

✅ **Consejo:** Ajustar el valor de MTU según las configuraciones del firewall objetivo puede aumentar la efectividad de la evasión.

## 2. **Data Length – `--data-length`**

Esta técnica consiste en agregar **datos adicionales** a los paquetes enviados, lo que puede confundir al firewall y evitar que detecte un escaneo.

### ¿Cómo funciona?
El firewall puede estar programado para identificar firmas específicas de los escaneos. Al modificar la longitud de los paquetes, se altera la firma y se reduce la probabilidad de detección.

### Uso en Nmap:
```bash
nmap --data-length 50 192.168.1.1
```

En este caso, se agregan **50 bytes** de datos aleatorios a cada paquete.

✅ **Consejo:** Usar un valor aleatorio moderado para no levantar sospechas en el tráfico.

## 3. **Source Port – `--source-port`**

Esta técnica permite especificar un **puerto de origen** para los paquetes enviados, lo cual puede ayudar a pasar desapercibido en un firewall mal configurado.

### ¿Cómo funciona?
Algunos firewalls permiten tráfico desde puertos específicos (como **53** para DNS o **80** para HTTP). Utilizando estos puertos, se puede disfrazar el escaneo como tráfico legítimo.

### Uso en Nmap:
```bash
nmap --source-port 53 192.168.1.1
```

En este ejemplo, los paquetes parecerán provenir del **puerto 53 (DNS)**.

✅ **Consejo:** Usar puertos comunes como **443 (HTTPS)** o **123 (NTP)** puede aumentar la probabilidad de evadir el firewall.

## 4. **Decoy – `-D`**

El uso de **señuelos** (decoys) implica enviar **paquetes falsos** desde múltiples direcciones IP para confundir al firewall y a los sistemas de detección de intrusos (IDS).

### ¿Cómo funciona?
El escaneo real queda oculto entre los paquetes generados por las direcciones falsas, lo que hace más difícil identificar al atacante.

### Uso en Nmap:
```bash
nmap -D RND:10 192.168.1.1
```

Este comando genera **10 IPs aleatorias** como señuelos junto con el escaneo real.

✅ **Consejo:** También puedes especificar direcciones IP concretas, por ejemplo:
```bash
nmap -D 192.168.1.2,192.168.1.3,ME 192.168.1.1
```

## 5. **Fragmented – `-f`**

Esta técnica fragmenta los paquetes en **porciones más pequeñas** para dificultar su inspección por parte del firewall.

### ¿Cómo funciona?
Los firewalls analizan los paquetes completos. Al fragmentarlos, el firewall puede no ser capaz de reconstruir el tráfico para detectar el escaneo.

### Uso en Nmap:
```bash
nmap -f 192.168.1.1
```

También puedes aumentar la fragmentación:
```bash
nmap --mtu 8 192.168.1.1
```

✅ **Consejo:** Úsalo con precaución, ya que un exceso de fragmentación podría causar alertas en IDS avanzados.

## 6. **Spoof-Mac – `--spoof-mac`**

La suplantación de la **dirección MAC** permite ocultar la identidad del dispositivo que realiza el escaneo.

### ¿Cómo funciona?
El firewall puede registrar las direcciones MAC de los dispositivos en la red. Al falsificar una dirección MAC, se oculta la identidad real.

### Uso en Nmap:
```bash
nmap --spoof-mac 00:11:22:33:44:55 192.168.1.1
```

Para usar una MAC aleatoria de un fabricante específico:
```bash
nmap --spoof-mac Apple 192.168.1.1
```

✅ **Consejo:** Utiliza direcciones MAC que coincidan con dispositivos comunes para mayor discreción.

## 7. **Stealth Scan – `-sS`**

El **escaneo sigiloso SYN** es una técnica que no completa la conexión TCP, lo que dificulta su detección.

### ¿Cómo funciona?
Este escaneo envía un paquete SYN para iniciar la conexión. Si el puerto está abierto, responde con SYN/ACK, pero el atacante no responde con ACK, evitando registros en el firewall.

### Uso en Nmap:
```bash
nmap -sS 192.168.1.1
```

✅ **Consejo:** Es ideal para evadir firewalls que no registran conexiones incompletas.

## 8. **Min-Rate – `--min-rate`**

Controla la **velocidad mínima de envío de paquetes**, lo que puede ayudar a pasar desapercibido.

### ¿Cómo funciona?
Los firewalls pueden detectar patrones de tráfico rápido. Al limitar la tasa de envío, el escaneo se confunde con tráfico normal.

### Uso en Nmap:
```bash
nmap --min-rate 10 192.168.1.1
```


