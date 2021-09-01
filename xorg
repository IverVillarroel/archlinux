# xorg-qtile-alacritty-ArchLinux
Este documento no pretende ser un guia detallado de cada programa. Es una guía rápida para acelerar el proceso de instalación.
1. Para instalar un servidor grafico en ArchLinux necesitamos instalar los siguientes paquetes
```
xorg-server xorg-xinit xorg xf86-video-fbdev
mesa mesa-demos (reconocimiento de graficos avanzado)
```
2. feh es un potente y ligero visor de imágenes que también puede ser utilizado para gestionar el fondo de pantalla para aquellos gestores de ventanas que carecen de tal opción 
```
pacman -S feh
```
3. picom es un compositor independiente para Xorg, adecuado para su uso con administradores de ventanas que no proporcionan composición. 'picom'
```
pacman -S picom
```
4. para el controlador de video veremos las opciones y selecionaremos el de nuestra tarjeta grafica.
```
pacman -Ss xf86-video
en mi caso
pacman -S Xf86-video-intel 
```
5. necesitaremois un entorno grafico para ello vamos a elegir un gestor de ventanas "tailing window manager" como qtile
```
pacman -S qtile
```
6. instalaremos un emulador de terminal "alacritty"
```
pacman -S alacritty 
```
7. para que todo esto funcione configuraremos el inicio del sistema 
```
cp /etc/X11/xinit/xinitrc /home/myuser/.xinitrc
```
8. editaremos con el editor de texto neovim 
```
nvim /home/myuser/.xinitrc 
```
  en el caso de no visualizar las fuentes de la barra de grupos. instale lightdm.
9. nos dirigimos al final del archivo de configuracion.
```
...
  done 
  unset f
fi

picom &                                               composicion de ventanas
setxkbmap latam &                                     define la distribucion del teclado a español latinoamericano en alacritty
feh --bg-fill /home/myuser/.config/wallpaper.jpg &    define el fondo de pantalla
exec qtile                                            ejecuta qtile
```

10. guardamos el archivo initrc y ejecutamos en la consola de comandos
```
startx
```
11. si queremos ejecutar la ventana de un programa en concreto ejecutamos
```
startx /etc/bin/chromium       por ejemplo si queremos ejecutar el chromium en una ventana independiente
```
12. para ajustar la resolucion de la pantalla 
```
xrandr 
xrandr ###x###  donde # es el tamaño de la resolucion
```
13. ahora. al momento de iniciar la computadora tendremos que escribir los comandos https://wiki.archlinux.org/title/Xinit_(Espa%C3%B1ol)#Inicio_autom%C3%A1tico_de_X_al_inicio_de_sesi%C3%B3n
```
login   myuser
password xxxx
$ startx 
```
14. tambien podemos instalar un es un gestor de sesión inter-escritorio como "lightdm" que gestionara todo el proceso mencionado anteriormente. pero si quieres hacerlo de manera manual continua leyendo-

15. para que el sistema inicie automaticamente sin escribir comandos http://www.linux-commands-examples.com/fgconsole
```
[[$(fgconsole2>/dev/null)==1]] && exec startx -vt1
```
16. para configurar la opacidad de la terminal alacritty descargamos el archivo alacritty.yml
```
https://github.com/alacritty/alacritty
cp /home/myuser/Download/alacritty.yml ~/.config/alacrtty/
nvim ~/.config/alacritty/alacritty.yml
```
  buscamos la linea y quitamos la almohadilla 
 ```
 background_opacity:05
 ```
 17. para configurar qtile 
 ```
 sudo cp /usr/share/doc/qtile/default_config.py /home/myuser/.config/qtile/config.py
 ```
   necesitamos cambiar de propietario al archivo y editarlo
 ```
 sudo chown myuser:myuser ~/.config/qtile/config.py
 nvim ~/.config/qtile/config.py
 ```
18. si necesitamos instalar paquetes manualmente makepkg es usado para compilar y construir paquetes capaces de instalar mediante pacman. una vez descargado el archivo por ejemplo instalaremos google-chrome
```
clonamos el repositorio
git clone https://aur.archlinux.org/google-chrome.git 
se creara una carpeta con el nombre del repositorio e ingresamos 
cd google-chrome
verificamos el propiertario 
ls -la 
si esta en root cambiarlo al usuario correspondiente
chown myuser:myuser /directorio del programa 
para compilar e instalar 
makepkg -si 
```
en el caso de personalizar el teclado instale xmodmap.
```
sudo pacman -S xorg-xmodmap
```
para configurar las entradas del teclado genere el archivo 
```
$ xmodmap -pke> ~ / .Xmodmap
```
despues de realizar los cambios en necesario volver a ejecutar xmodmap
```
$ xmodmap ~ / .Xmodmap
```
añadir al archivo .xinitrc para que startx inicie aumanticamente 
```
~ / .xinitrc
[[-f ~ / .Xmodmap]] && xmodmap ~ / .Xmodmap
```
para instalar un gestor de archivos vifr 
```
sudo pacman -S vifm
```
para configurar los temas https://github.com/vifm/vifm-colors
```
:coloscheme nombre del tema
```
para instalar fuentes visitar nerdfonts https://www.nerdfonts.com/ descargar la fuente y descomprimirlo en ~/.local/share/font previamente crear el directorio.


