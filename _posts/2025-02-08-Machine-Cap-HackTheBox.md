---
Title: Machine Cap HackTheBox  
autor: Gabriel Eduardo
---

# [](#header-1)Maquina Cap-HackTheBox
La máquina figura en Hack The Box como una máquina Linux, con un nivel de dificultad fácil.


###  
![](/assets/Machine_Cap/Captura%20de%20pantalla_20250209_143308.png)

# [](#header-2) Fase de reconocimiento
Para identificar los puertos abiertos en la máquina víctima, utilizamos la herramienta Nmap con los siguientes parámetros:

```bash
#!/bin/bash

nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 10.10.10.245 -oG allPorts
```



![](/assets/Machine_Cap/1.png)


El escaneo inicial revela que los puertos 21, 22 y 80 están abiertos. Además, el valor TTL indica que se trata de un sistema operativo Linux.

Para procesar mejor los resultados del escaneo, usamos una función preconfigurada en `~/.zshrc`, que nos permite extraer la información de manera más legible



![](/assets/Machine_Cap/3.png)

Después de identificar los puertos abiertos, realizamos un segundo escaneo con Nmap para obtener más detalles sobre los servicios y versiones en ejecución:

```bash
#!/bin/bash

nmap -sCV -p21,22,80 10.10.10.245 -oN targeted  
```

![](/assets/Machine_Cap/4.png)

Los resultados del escaneo se almacenan en el archivo `targeted`, y mediante `cat` podemos analizar la información obtenida:

![](/assets/Machine_Cap/5.png)

Podemos ver que el puerto 80 aloja un servicio web. Para obtener más información sobre este servicio, usamos la herramienta WhatWeb.

# [](#header-3) Nota:
WhatWeb es una herramienta que permite identificar tecnologías utilizadas en un sitio web, tales como servidores, frameworks y CMS.


![](/assets/Machine_Cap/6.png)

Con la información extraída, accedemos al navegador para visualizar la página web alojada en el puerto 80. Según WhatWeb, se trata de un panel de control (dashboard).


![](/assets/Machine_Cap/7.png)

Dentro del sitio, observamos que nos redirige automáticamente a un archivo `1`. Probamos cambiarlo por `0` para analizar su contenido y capturar los datos intercambiados usando el siguiente comando:

```bash
#!/bin/bash

tshark -r 0.pcap -tfields -e tcp.payload 2>/dev/null | xxd -ps -r
```

```bash
tshark -r 0.pcap -tfields -e tcp.payload 2>/dev/null
```

#### [](#header-4) Explicación:

* **tshark**: Versión en línea de comandos de Wireshark, usada para analizar tráfico de red.
* **-r 0.pcap**: Especifica que se analizará el archivo `0.pcap`.
* **-tfields -e tcp.payload**: Extrae únicamente la carga útil (payload) de los paquetes TCP.
* **2>/dev/null**: Redirige los errores para no mostrarlos.

```bash
xxd -ps -r
```

* **xxd**: Convierte datos entre formatos hexadecimal y binario.
* **-ps**: Indica que la entrada es hexadecimal plano.
* **-r**: Invierte la conversión (de hexadecimal a binario).

Este comando nos permite obtener la siguiente información:

![](/assets/Machine_Cap/8.png)

En los datos extraídos, encontramos credenciales de inicio de sesión:

![](/assets/Machine_Cap/9.png)

# [](#header-3) Acceso por FTP:

Probamos estas credenciales en el servicio FTP y confirmamos que son válidas:

![](/assets/Machine_Cap/ftp1.png)

Dentro del servidor, buscamos la primera flag listando los archivos disponibles. Encontramos un archivo `user.txt`, lo descargamos y obtenemos la flag del usuario.

![](/assets/Machine_Cap/userFlag.png)

# [](#header-3) Escalada de privilegios

Para obtener la segunda flag, necesitamos acceso como root. Buscamos archivos con el bit SUID activo:

```bash
find / -perm -4000 -user root 2>/dev/null | xargs ls -l 
```
![](/assets/Machine_Cap/OUID.png)

Como podemos ver no encontramos nada que nos permita escalar privilegios 

Entonces, probamos buscar capacidades habilitadas en el sistema:

```bash
getcap -r / 2>/dev/null
```

![](/assets/Machine_Cap/Capabilities.png)

Este comando nos muestra las capacidades asignadas a binarios en el sistema. Identificamos una que podemos explotar para escalar privilegios.

![](/assets/Machine_Cap/ROOT.png)

Ya que cambiamos los identificadores a root podemos ver los archivos que esten presentes
y accesibles solo para el usuario root 


![](/assets/Machine_Cap/flagROOT.png)

de esta manera concluimos la maquina Cap  

![](/assets/Machine_Cap/Resultado.png)


