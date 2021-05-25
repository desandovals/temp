## Habilitar SWAP en GCP

### Asignar disco para SWAP de tipo SSD

1. Entrar al proyecto en GCP donde se encuentren las VMs.
2. Dar clic en el CE que requiramos añadir SWAP. 
3. Dar clic en `editar`
4. Añadir disco, cuidando que sea SSD, con el tamaño que requiera el usuario. Cuidar que el disco tenga la nomenclatura: 
```
hostname-swap
```

*Reemplazar* `hostname` *por el nombre del servidor* 

### Configuración de SO

1. Identificar nuevo disco: 

```
lsblk
```

2.  Generar partición para SWAP a disco seleccionado: 

```
mkswap <device>
```

**Por ejemplo:** 
```
mkswap /dev/sdb
```

3. Ejecutar el siguiente comando para añadir el registro al fstab: 

```
echo "$(blkid | grep "<device>" | awk '{print $2}' | cut -d ":" -f 2 | sed 's/"//g')     /swap       swap      defaults       0 0" >> /etc/fstab

```

**Siguiendo el ejemplo anterior, con** `/dev/sdb` **quedaría de la siguiente forma:** 

``` 
echo "$(blkid | grep "/dev/sdb" | awk '{print $2}' | cut -d ":" -f 2 | sed 's/"//g')     /swap       swap      defaults       0 0" >> /etc/fstab
```

4. Activar swap: 

```
swapon -a
```

### Validación 

Revisar que se haya aplicado el cambio: 

```
free -h
```