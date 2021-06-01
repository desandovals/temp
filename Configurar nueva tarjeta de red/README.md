## Añadir y configurar nueva tarjeta de red

### Solicitud a Cloud Services - Hardware

Generar requerimiento para añadir tarjeta de red al servidor, indicando la VLAN que se requiere, es decir, el segmento de red de la IP que se va a requerir configurar. 

### Configuración de tarjetas en servidor

1. Ir a la ruta: 

```
cd /etc/sysconfig/network-scripts
```

2. Consultar el nombre de la tarjeta de red añadida en el servidor: 

```
ifconfig
```

Fijarnos en las tarjeta de red que no tenga ip asociada. 

**Ejemplo:**

```
ens192: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.16.202.198  netmask 255.255.255.224  broadcast 172.16.202.223
        inet6 fe80::250:56ff:feae:7ab5  prefixlen 64  scopeid 0x20<link>
        ether 00:50:56:ae:7a:b5  txqueuelen 1000  (Ethernet)
        RX packets 1333090  bytes 1096016860 (1.0 GiB)
        RX errors 0  dropped 29  overruns 0  frame 0
        TX packets 903327  bytes 4950652348 (4.6 GiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

ens224: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        ether 00:50:56:ae:42:e5  txqueuelen 1000  (Ethernet)
        RX packets 166488  bytes 10037539 (9.5 MiB)
        RX errors 0  dropped 15  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 8262  bytes 1014506 (990.7 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 8262  bytes 1014506 (990.7 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

En el ejemplo anterior, podemos darnos cuenta que la tarjeta de red sin ip es la `ens224`. 

3. Generar archivo de tarjeta de red, con el siguiente nombre: `ifcfg-<nombre_tarjeta>`

**Del ejemplo anterior, quedaría de la siguiente forma:**

```
touch ifcfg-ens224
```

4. Añadimos al archivo que generamos el siguiente contenido: 

```
BOOTPROTO="static"          ### Sustituir <IP deseada> 
IPADDR="<IP deseada>"
NETMASK="255.255.255.0"
DEVICE="<nombre_tarjeta>"   ### Sustituir <nombre_tarjeta>
```

**Del ejemplo anterior, quedaría como lo siguiente, suponiendo que quisieramos configurar la IP `192.27.203.250` y el nombre de la tarjeta sea `ens224`:** 

```
BOOTPROTO="static"
IPADDR="192.27.203.250"
NETMASK="255.255.255.0"
DEVICE="ens224"
```

### Reiniciar tarjeta de red

Una vez configurada procedemos a reiniciarla: 

```
ifdown <nombre_tarjeta> ; ifup <nombre_tarjeta>
```

**Siguiendo el ejemplo:** 

```
ifdown ens224 ; ifup ens224 
```





 