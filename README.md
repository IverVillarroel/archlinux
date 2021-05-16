# ArchLinux-Intall-Guide
## intall ArchLinux
La guia oficial desde el sitio oficial archlinux https://wiki.archlinux.org/title/Installation_guide 
El presente documento no pretende ser una guía completa para la instalación de ArchLinux. Es una guía rápida para acelerar el proceso de instalación.

1. vamos a descargar la imagen ISO desde https://archlinux.org/download/ y El programa RUFUS para crear el USB booteable https://rufus.ie/es/ 
Si la instalación se está haciendo en VirtualBox, configurar la máquina virtual para permitir el arranque con EFI. Seleccionar la máquina virtual, propiedades, System, Enable EFI.
2. iniciar la computadora desde el usb y seleccionar  Arch Linux Arch ISO x86_64

3. configuramos nuestro teclado con el siguiente comando:
```
$ loadkeys Es
```
4. verificamos la conexion a internet con el siguiente comando:
```
ping archlinux.org
```
asegurese que su interfaz de red este en la lista y activada:
```
ip link
```
para el caso de conectarse a una red wifi vease lo siguiente: https://wiki.archlinux.org/title/wpa_supplicant
tambien puede utilizar iwctl n caso de tener sólo wifi, usar [iwctl](https://wiki.archlinux.org/index.php/Iwd#iwctl):
```
         iwctl    
    Listar los dispositivos:
        device list
    Escanear redes:    
        station <dispositivo> scan        
    Listar redes disponibles:        
        station <dispositivo> get-networks     
    Conectarse a una red:    
        station <dispositivo> connect <SSID>
    Salir de iwctl:    
        exit
 ```

5. Ahora vamos a particionar nuestro disco duro para ello listamos con el comando:
```
fdisk -l
```
los resultados que terminan en room o loop pueden ignorarse.
El esquema de particionado basico debe ser el siguiente:
```
PARTICION UEFI GPT
PARTICION                  PARTICION       TIPO DE PARTICION
--------------------------------------------------------------
/mnt/boot o /mnt/EFI      /dev/sdX1           EFI system partition 
/mnt                      /dev/sdX2           Linux x86-64 root(/)
[SWAP]                    /dev/sdX3           Linux Swap
```
para ingresar a la particion usamos la herramienta fdisk

```
fdisk /dev/sdX
```

Para verificar que estamos en modo UEFI, ejecutar el siguiente comando: 

```
 ls /sys/firmware/efi/efivars
```

   Si se muestra contenido en la carpeta efivars, quiere decir que arrancamos el sistema correctamente en modo UEFI.*

6. haremos la instalacion de UEFI empezando a crear el tablero de particiones "GPT", y lurgo:
 ```
 DEVICE       Size     Type
 dev/sda1      3G      EFI System 
 dev/sda2      10G     linux SWAP 
 dev/sda3      XG      linux filesysten
```
Se recomienda que la memoria swap sea el doble de la memoria RAM 

7. formatearemos las particiones:
```
mkfs.vfat -F32 dev/sdX1
mkfs.ext4      dev/sdX3
```
para la particion swap formatear y activar
```
mkswap /dev/sdX2
swapon /dev/sdX2
```
8. para montar los sistemas de archivos 
```
mount /dev/sdX3 /mnt
mount /dev/sdX1 /mnt/boot/efi
```
9. una vez montada las particiones instalaremos los paquetes escenciales 
```
pacstrap /mnt base linux linux-firmware base-devel efibootmgr grub networkmanager dhcpcd neovim
```
paquetes adicionales en el caso de tener wifi 
```
netctl wpa_supplicant dialog
```
paquetes adicionales para el touchpad 
```
xf86-input-synaptics
```
10. configuracion del sistema 
```
genfstab -U /mnt >> /mnt/etc/fstab
```
  *Verificar:  cat /mnt/etc/fstab*
        
11. ingresando al sistema con chroot
```
arch-chroot /mnt
```
12.configurando la zona horaria, idioma del sistema ingles, distribucion del teclado 
```
ln -sf /usr/share/zoneinfo/America/La_Paz /etc/localtime
locale.gen 
nvim /etc/vconsole.conf  "KEYMAP=la-latin1"
echo nombredelequipo > /etc/hostname
```


14. al momento de iniciar archlinux no cargara, no gestionara el internet por cable es necesario activarlo 
```
systemctl enable NetworkManager
```
para gestionar el wifi con wpa_supplicant si fuera el caso
```
nmcli dev wifi connect "ssid" password "00000"
```
 *****
 
15. Activar la sincronización del reloj del sistema con Internet: 

        timedatectl set-ntp true

     Verificar: (opcional)

        timedatectl status
16. añadir contraseña al root, crear un nuevo usuario y contraseña de usuario.
```
passwd 
useradd -m usuario
passwd usuario
```
17. crear el gestor de arranque con grub
```
grub-install --efi-directory=/boot/efi --bootloader-id=grub
generar el acrchivo de configuracion
grub-mkconfig -o /boot/grub/grub.cfg
```
despues de reiniciar dar permisos de uso para Sudo al nuevo usuario:

```
        visudo
```

    Buscar la línea  ROOT  ALL=(ALL) ALL y justo debajo de esta, agregar nuestro usuario, por ejemplo:
    
``` 
        myUser   ALL=(ALL) ALL
```

    Para editar el documento, presionar la tecla i. Después de esto ya podremos agregar texto normalmente.
    Para guardar los cambios, presionar ESC, luego escribir :wq y finalmente ENTER.












