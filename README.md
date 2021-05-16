# ArchLinux-Intall-Guide
Hola te comparto el proceso que realice para instalar archlinux en mi laptop HP, tambien subire las configuraciones de mis programas esenciales. 
si tienes alguna duda o interrogante puedes ponerte en contacto conmigo, te ayudare con mucho gusto. 'iver014avc@gmail.com'
instalaremos los siguientes paquetes:
1. SO archlinux
2. Xorg 
3. Qtile
4. Alacritty 
5. neovim
6. google-chrome
7. Spotify

## intall ArchLinux
podemos encontrar la guia oficial desde el sitio oficial archlinux https://wiki.archlinux.org/title/Installation_guide 

primero vamos a descargar la imagen ISO desde https://archlinux.org/download/ seleccionamos el servidor cercano a donde nos encontramos y descargamos la imagen ISO. tambien necesitaremos el programa RUFUS para crear una USB booteable https://rufus.ie/es/ 

Ahora cargaremos el la imagen ISO en nuestro USB y reiniciaremos el sistema desde el USB. Archilinux nos dejara en la consola desde ahi tendremos que seguir por nuestra cuenta.
configuramos nuestro teclado con el siguiente comando:
```sh
$ loadkeys Es
```
verificamos la conexion a internet con el siguiente comando:
```sh
ping google.com
```
asegurese que su interfaz de red este en la lista y activada:
```
ip link
```
para el caso de conectarse a una red wifi vease lo siguiente: https://wiki.archlinux.org/title/wpa_supplicant
Ahora vamos a particionar nuestro disco duro para ello listamos con el comando:
```
fdisk -l
```
los resultados que terminan en room o loop pueden ignorarse.
El esquema de particionado basico debe ser el siguiente:
```
 PARTICION BIOS MBR  
 ------------------------------------------------------------
PUNTO DE MONTAJE          PARTICION       TIPO DE PARTICION
  /mnt                    * /dev/sdX1        linux
  [SWAP]                    /dev/sdX2        linux swap 
------------------------------------------------------------  


PARTICION UEFI GPT
PARTICION                  PARTICION       TIPO DE PARTICION
--------------------------------------------------------------
/mnt/boot o /mnt/EFI      /dev/sdX1           EFI system partition 
/mnt                      /dev/sdX2           Linux x86-64 root(/)
[SWAP]                    /dev/sdX3           Linux Swap
```
para ingresar a la particion usamos la herramienta fdisk o cfdisk
```
fdisk /dev/sdX
```
para saber si tenemos el sistema BIOS o GPT colocamos el siguiente comando:
```
ls /sys/firmware/efi/efivars
```
si visualizamos distintos archivos significa que tenemo la particion UEFI. 
haremos la instalacion de UEFI empezando a crear el tablero de particiones "GPT", 
 ```
 DEVICE       Size     Type
 dev/sda1      3G      EFI System 
 dev/sda2      10G     linux SWAP 
 dev/sda3      XG      linux filesysten
```
Se recomienda que la memoria swap sea el doble de la memoria RAM 
formatearemos las particiones:
```
mkfs.vfat -F32 dev/sdX1
mkfs.ext4      dev/sdX3
```
para la particion swap formatear y activar
```
mkswap /dev/sdX2
swapon /dev/sdX2
```
para montar los sistemas de archivos 
```
mount /dev/sdX3 /mnt
mount /dev/sdX1 /mnt/boot/efi
```
una vez montada las particiones instalaremos los paquetes escenciales 
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
configuracion del sistema 
```
genfstab -U /mnt >> /mnt/etc/fstab
```
ingresando al sistema con chroot
```
arch-chroot /mnt
```
configurando la zona horaria, idioma del sistema ingles, distribucion del teclado 
```
ln -sf /usr/share/zoneinfo/America/La_Paz /etc/localtime
locale.gen 
nvim /etc/vconsole.conf  "KEYMAP=la-latin1"
nvim /etc/hostname       "nombredelequipo"
```
al iniciar archlinux no cargara no gestionara el internet por cable es necesario activarlo 
```
systemctl enable NetworkManager
```
para gestionar el wifi 
```
nmcli dev wifi connect "ssid" password "00000"
```
añadir contraseña al root, crear un nuevo usuario y contraseña de usuario.
```
passwd 
useradd -m usuario
passwd usuario
```
crear el gestor de arranque con grub
```
grub-install --efi-directory=/boot/efi --bootloader-id=grub
generar el acrchivo de configuracion
grub-mkconfig -o /boot/grub/grub.cfg









