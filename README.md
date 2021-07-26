# Primeros pasos con Arch Linux (Sistema EFI)

Arch Linux es una distribución Linux para computadoras x86_64, ARM y I686 orientada a usuarios avanzados. Se compone en su mayor parte de software libre y de código abierto. Su modelo de desarrollo es de tipo [Rolling Release](https://es.wikipedia.org/wiki/Liberaci%C3%B3n_continua) y el enfoque de diseño persigue el [Principio KISS.](https://es.wikipedia.org/wiki/Principio_KISS)

Para instalar y configurar este sistema operativo se necesita un grado de conocimiento superior al básico. No obstante, se puede mantener y administrar el sistema de forma sencilla. Los creadores y la comunidad, denominan como *filosofía*, los siguientes tres aspectos:

1. Mantener el sistema lo más simple y ligero posible, siguiendo el llamado principio [KISS.](https://es.wikipedia.org/wiki/Principio_KISS)
2. Los principios del liderazgo del proyecto, Aaron Griffin, también son tomados como referencia: *Fiarse de las GUIs para construir y configurar tu sistema operativo termina dañando a los usuarios finales. Intentar ocultar la complejidad del sistema, termina complicando al sistema. Las capas de abstracción que sirven para ocultar el funcionamiento interno nunca son una cosa buena. En cambio, los componentes internos deberían ser diseñados de forma que no necesiten ser ocultados*.
3. Arch Linux permite al usuario hacer las contribuciones que desee, mientras estas no vayan en contra de la filosofía.

## Instalación

Este documentos es un resumen de instalación sacado de la [guía oficial de Arch Linux,](https://wiki.archlinux.org/title/Installation_guide) en donde se detalla en profundida el proceso de instalación. Por lo que recomendamos que lea la documentación oficial antes de seguir los consejos de este post.

### **Paso 0:** Obtener la ISO

Podemos obtener la última instantanea de la [página de Arch Linux](https://archlinux.org/) y grabarla en un USB con mínimo 4G de espacio.

Luego de verificar que el USB no tenga particiones ocupadas y que la ISO se descargo correctamente, podemos pasarla al USB con el comando *dd*:

~~~TEXT
# Comprobamos la integridad de datos de la ISO
md5sum nombre_de_la_iso.iso && echo codigo_de_verificacion nombre_de_la_iso.iso

# Grabamos la ISO en el USB
sudo dd if=ruta_de_la_iso of=dispositivo

# Nos aseguramos de que el cache del USB sea grabado
sync
~~~

Ahora podemos extraer el USB sin riesgo.

### **Paso 1:** Configuración de teclado

Es necesario primero que nada configurar la distribución del teclado para tener correctas las teclas. En el caso de un teclado *Español Latinoamerica*, el comando sería el siguiente:

~~~TEXT
loadkeys la-latin1
~~~

### **Paso 2:** Verificar la conexión a internet *(Importante)*

Para verificar la conexión a internet ejecutamos:

~~~TEXT
ip a
~~~

Si estamos conectados por ethernet (que es lo mas recomendable), la conexión se debería haber realizado automaticamente. De modo contrario se puede conectar a una red WiFi usando una utilidad de la instalación:

~~~TEXT
iwclt
~~~

Cuando ingrese este comando se activará la utilidad, y podrá ver los dispositivos wireless disponibles en el equipo:

~~~TEXT
device list
~~~

Puede escanear las redes disponibles:

~~~TEXT
station wlan0 scan
~~~

Ahora espere unos 10 segundos a que termine el escaneo, luego corra:

~~~TEXT
station wlan0 get-networks
~~~

Luego para conectarse a una red:

~~~TEXT
station wlan0 connect nombre_del_wifi
~~~

Se le pedirá la contraseña, y se intentará conectar a la red indicada.Luego puede salir de la utilidad y volver a comprobar si tiene acceso a la red:

~~~TEXT
# Salimos de la utilidad
exit

# Verificamos la conexión
ip a
~~~

### **Paso 3:** Actualizar el reloj del sistema

~~~TEXT
timedatectl set-ntp true
~~~

### **Paso 4:** Listar los discos en el equipo

~~~TEXT
lsblk
~~~

### **Paso 5:** Crear las particiones

Lo que se debe hacer ahora es crear las particiones del disco en donde se instalará el sistema. La utilidad recomendada para sistemas GPT es *gdisk*:

~~~TEXT
gdisk /dev/sdx
~~~

Para poder instalar cualquier distibución Linux en un Sistema EFI, debemos crear obligatoriamente las siguientes particiones:

1. Partición EFI con formato FAT32 de al menos 500MB.
2. Partición raíz con formato ext4.

Adicionalmente, es recomendable crear una partición de intercambio SWAP. Y también se pueden crear particiones extra para el */home* o */opt*.

Estas particiones adicionales se deben formatear y montar en la ubicación correspondiente.

### **Paso 6:** Formatear las particiones creadas

Vemos las particiones que se crearon:

~~~TEXT
lsblk
~~~

Si todo es correcto pasamos a darles el formato correspondiente:

~~~TEXT
# Para particiones ext4
mkfs.ext4 /dev/sdxn

# Para particiones fat32
mkfs.fat -F32 /dev/sdxn

# Para particiones swap
mkswap /dev/sdxn
~~~

### **Paso 7:** Montar las particiones creadas

Este es el paso previo a instalar el sistema, debemos montar las unidades configuradas anteriormente en las ubicaciones correspondientes, para luego generar el archivo de montajes al inicio del sistema. Para este tutorial, el sistema se montara en */mnt*.

La particion del sistema raiz:

~~~TEXT
mount /dev/sdxn /mnt
~~~

Para la partición UEFI, debemos crear una ruta especial bajo la raíz llamada */boot*:

~~~TEXT
# Creamos la carpeta de boot
mkdir /mnt/boot

# Montamos la partición
mount /dev/sdxn /mnt/boot
~~~

Las particiones extras como */home* o */opt* también deben estar dentro de la raíz:

~~~TEXT
# Para partición de /home
mkdir /mnt/home
mount /dev/sdxn /mnt/home

# Para partición de /opt
mkdir /mnt/opt
mount /dev/sdxn /mnt/opt
~~~

Para la partición SWAP:

~~~TEXT
swapon /dev/sdxn
~~~

### **Paso 8:** Instalación de Archlinux

Una vez tenemos todos los pasos anteriores cumplidos pasamos a finalmente instalar el sistema en las particiones que elegimos, con *pacstrap* seleccionamos la ubicación de instalación (en mi caso */mnt*) y elegimos los paquetes a instalar:

~~~TEXT
pacstrap /mnt base base-devel linux linux-firmware linux-headers nano networkmanager wpa_supplicant dialog wireless_tools netctl bash-completion
~~~

### **Paso 9:** Generar el archivo fstab

Para que las particiones se monten al arranque y cumplan con su función debemos generar el *fstab*:

~~~TEXT
genfstab -U /mnt >> /mnt/etc/fstab
~~~

### **Paso 10:** Cambiamos a la instalación realizada

Para utilizar el sistema recien instalado, podemos utilizar el siguiente comando:

~~~TEXT
arch-chroot /mtn
~~~

### **Paso 11:** Configuramos la zona horaria

~~~TEXT
ln -sf /usr/share/zoneinfo/America/Argentina/Buenos_aires /etc/localtime
~~~

### **Paso 12:** Sincronizamos el reloj

~~~TEXT
hwclock --systohc
~~~

### **Paso 13:** Establecemos el idioma

Lo primero que debemos hacer es seleccionar el idioma que utilizará el sistema:

~~~TEXT
nano /etc/locale.conf
~~~

Y añadimos la siguiente linea:

~~~TEXT
LANG=es_AR.UTF-8
~~~

Paso siguiente configuramos el archivo de locales. Debemos descomentar nuestro idioma preferido y tambien uno en ingles que será el base:

~~~TEXT
nano /etc/locale.gen
~~~

Y descomentamos:

~~~TEXT
en_US.UTF-8 UTF-8
es_AR.UTF-8 UTF-8
~~~

### **Paso 14:** Distribución de teclado

Ahora creamos un archivo para la distribución del teclado:

~~~TEXT
nano /etc/vconsole.conf
~~~

Y añadimos:

~~~TEXT
KEYMAP=la-latin1
~~~

### **Paso 15:** Generamos el archivo de locales

~~~TEXT
locale-gen
~~~

### **Paso 16:** Configuración de la red

Debemos darle un nombre a nuestra PC, para que sea identificable para otros quipos, lo podemos hacer editando el archivo:

~~~TEXT
nano /etc/hostname
~~~

Y añadimos:

~~~TEXT
myhostname
~~~

Lo mismo para el archivo de hosts, este configurará otras cuestiones para el *NetworkManager*:

~~~TEXT
nano /etc/hosts
~~~

Y añadimos lo siguiente:

~~~TEXT
127.0.0.1   localhost
::1         localhost
127.0.1.1   myhostname.localdomain myhostname
~~~

### **Paso 17:** Se habilita el NetworkManger

~~~TEXT
systemctl enable NetworkManager
~~~

### **Paso 18:** Cambio de contraseña de root

~~~TEXT
passwd
~~~

Ingrese la contraseña de usuario root que prefiera.

### **Paso 19:** Instalación del bootloader *(Muy importante)*

A continuación se instalara GRUB en modo UEFI, esto permitirá al sistema arrancar:

~~~TEXT
pacman -S grub efibootmgr ntfs-3g
~~~

Es recomendable también instalar los microcodes, según la versión del procesador:

~~~TEXT
# Para AMD
pacman -S amd-ucode

# Para INTEL
pacman -S intel-ucode
~~~

Luego configure el grub para su sistema bajo la ruta de la partición EFI. En mi caso es una notbook *x86_64*.

~~~TEXT
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id='arch'
~~~

Si tenes otro sistema operativo en alguna otra unidad o partición y queres que grub la detecte instala el siguiente paquete:

~~~TEXT
pacman -S os-prober
~~~

Luego se tiene que montar las unidades con sistema operativo:

~~~TEXT
# Creamos la carpeta de montaje
mkdir /mnt/extra_os

# Montamos la unidad con S.O.
mount /dev/sdxn /mnt/extra_os
~~~

Corremos os-prober para comprobar que sistema hay instalado:

~~~TEXT
os-prober
~~~

Y luego se crea la configuracion básica del grub:

~~~TEXT
grub-mkconfig -o /boot/grub/grub.cfg
~~~

Se copia el archivo de inicio:

~~~TEXT
# Creamos la carpeta de booteo
mkdir /boot/EFI/boot

# Copiamos el archivo del bootloader
cp /boot/EFI/arch/grubx64.efi /boot/EFI/boot/bootx64.efi
~~~

### **Paso 20:** Finalizamos la instalación

Salimos de arch-chroot:

~~~TEXT
exit
~~~

Desmontamos todas las unidades de la instalación:

~~~TEXT
umount -R /mnt
~~~

Y por último apagamos el equipo:

~~~TEXT
systemctl poweroff
~~~

Ahora desconectamos el medio de instalación y ya podemos volver a iniciar el sistema.

## Primer inicio de sesión

### **Paso 1:** Nos conectamos a una red WiFi

~~~TEXT
sudo nmcli dev wifi connect nombre_de_wifi password contraseña
~~~

### **Paso 2:** Creamos un usuario

Una nueva instalación te deja solo con la cuenta de superusuario, más conocida como *root*. Iniciar sesión como *root* durante períodos prolongados de tiempo, posiblemente incluso exponerlo a través de SSH en un servidor, es inseguro. Es por eso que debemos crear un usuario para nosotros:

~~~TEXT
useradd -m -g users -G ftp,http,log,rfkill,sys,uucp,audio,storage,video,wheel,games,power,scanner,kvm -s /bin/bash nombre_usuario
~~~

### **Paso 3:** Creamos una contraseña para el nuevo usuario

Para mayor seguridad es recomendable que le pongamos contraseña al nuevo usuario creado.

~~~TEXT
passwd nombre_usuario
~~~

### **Paso 4:** Añadimos a sudo el nuevo usuario

Editamos el archivo de sudo y luego descomentamos la linea que habilita al grupo wheel:

~~~TEXT
EDITOR=nano visudo
~~~

Y descomentamos la linea:

~~~TEXT
%wheel ALL=(ALL) NOPASSWD: ALL
~~~

### **Paso 5:** Iniciamos sesión con el nuevo usuario

Salimos de la sesión de root:

~~~TEXT
exit
~~~

Y nos logeamos con el nuevo usuario y contraseña.

### **Paso 6:** Actualizamos el sistema

Ahora que tenemos el sistema instalado y configurado con una cuenta de usuario válida, lo que debemos hacer es comprobar si existen actualizaciones disponibles. Pero antes, es buena idea habilitar el soporte multilib:

~~~TEXT
sudo nano /etc/pacman.conf
~~~

Y descomentamos las siguientes lineas:

~~~TEXT
[multilib]
Include = /etc/pacman.d/mirrorlist
~~~

Luego, actualizamos la lista de repositorios y verificamos las actualizaciones:

~~~TEXT
sudo pacman -Syu
~~~

## Configuracion de un entorno de escritorio

Un entorno de escritorio es recomendado si la computadora en donde se esta instalando el sistema requiere de capacidades gráficas. Existen una variedad bastante grande de entornos de escritorios, a mi en lo personal me gustan dos: GNOME y KDE Plasma.

Antes de instalar un entorno debemos tener el servidor de gráficos. Xorg esta en casi todo sistema, sin embargo, Wayland esta ganando terreno ultimamente.

### Instalacion de Xorg

Podemos instalar el servidor de ventanas X con el comando:

~~~TEXT
sudo pacman -S xorg
~~~

### Instalacion de GNOME

Si GNOME es tu escritorio favorito, lo podes instalar tan facil como el siguiente comando:

~~~TEXT
sudo pacman -S gnome gnome-extra
~~~

También, recomiendo instalar el paquete de iconos Papirus y la utilidad de impresoras CUPS:

~~~TEXT
sudo pacman -S papirus-icon-theme cups
~~~

Por último, debemos iniciar el servicio de administrador de sesiones y de impresión:

~~~TEXT
# Servicio de sesiones
systemctl enable gdm

# Servicio de impresiones
systemctl enable cups
~~~

### Instalacion de KDE Plasma

Si queres tener en tu carpeta de usuario, carpetas como *Documentos*, *Descargas*, *Música*, *etc*.

~~~TEXT
# Instalamos el servicios de carpetas de usuario
sudo pacman -S xdg-user-dirs

# Creamos las carpetas
xdg-user-dirs-update
~~~

Para instalar KDE Plasma ejecutamos el siguiente comando:

~~~TEXT
sudo pacman -S plasma kde-applications plasma-meta kde-applications-meta packagekit-qt5 kde-gtk-config
~~~

Es recomendable, instalar temas tanto para Qt y Gtk:

~~~TEXT
papirus-icon-theme arc-gtk-theme
~~~

Por último habilitamos el servicio de administración de sesiones:

~~~TEXT
systemctl enable sddm
~~~

## Instalacion de drivers

### Drivers de la placa base

Si ejecutamos:

~~~TEXT
sudo mkinitcpio -p linux
~~~

Y obtenemos una salida como la siguiente:

~~~TEXT
==> WARNING: Possibly missing firmware for module: aic94xx
==> WARNING: Possibly missing firmware for module: wd719x
==> WARNING: Possibly missing firmware for module: xhci_pci
~~~

Tenemos un problema con drivers que nos estan faltando. La solucion es instalarlos para tener el mejor rendimiento de nuestro hardware.

Nos dirigimos a una carpeta temporal en donde descargar archivos, en mi caso */home/gabi/Descargas*:

~~~TEXT
cd Descargas
~~~

Y ahora luego de buscar los drivers en AUR o el repositorio oficial, los instalamos:

~~~TEXT
git clone https://aur.archlinux.org/aic94xx-firmware.git
cd aic94xx-firmware
makepkg -si

git clone https://aur.archlinux.org/wd719x-firmware.git
cd wd719x-firmware
makepkg -si

git clone https://aur.archlinux.org/upd72020x-fw.git
cd upd72020x-fw
makepkg -si
~~~

### Drivers de la targeta grafica

Primero instalamos las utilidades de mesa, esto nos servirá para haberiguar que gráfica esta renderizando el escritorio:

~~~TEXT
sudo pacman -S mesa-demos
~~~

Ejecutamos el comando siguiente:

~~~TEXT
glxinfo | grep "OpenGL renderer"
~~~

Obteniendo la salida:

~~~TEXT
OpenGL renderer string: Mesa Intel(R) HD Graphics 630 (KBL GT2)
~~~

Esto significa que estamos utilizando la integrada de INTEL para el video. Yo tengo una Notebook con gráficos dedicados Nvidia GTX 1050, ademas de una integragada HD630 de un i5 de 7ma generación.

Para instalar los drivers de la tarjeta gráfica dedicada puedo ejecutar los siguientes comandos:

~~~TEXT
sudo pacman -S nvidia nvidia-utils nvidia-settings lib32-nvidia-utils virtualgl
~~~

Una vez instalado los drivers se debe reiniciar la PC antes de hacer cualquier otra cosa.

### Instalación de optimus-manager

Este programa de Linux proporciona una solución para el cambio de GPU en laptops Optimus *(es decir, laptops con una configuración dual Nvidia/Intel o Nvidia/AMD)*.

Para usuarios de **GNOME** primero tenemos que instalar un gestor de sesiones parcheado:

~~~TEXT
git clone https://aur.archlinux.org/gdm-prime.git
cd gdm-prime
makepkg -si
~~~

Una vez instalada la utilidad, editamos el archivo */etc/gdm/custom.conf*:

~~~TEXT
sudo nano /etc/gdm/custom.conf
~~~

Y descomentamos la siguiente linea:

~~~TEXT
WaylandEnable=false
~~~

Se tiene que comprobar que no exista el archivo */etc/X11/xorg.conf*, si es asi, hay que eliminarlo.

Entonoces para instalarlo ejecutamos:

~~~TEXT
git clone https://aur.archlinux.org/optimus-manager.git
cd optimus-manager
makepkg -si
~~~

Despues de instalar se tiene que **reiniciar el ordenador.**

Una vez reiniciado ahora podemos ejecutar los siguientes comandos para cambiar el modo de uso de la gráfica:

~~~TEXT
# Para cambiar a graficos de la tarjeta grafica
optimus-manager --switch nvidia

# Para usar la GPU integrada
optimus-manager --switch integrated

# Para usar la GPU bajo demanda
optimus-manager --switch hybrid
~~~

### Icono de optimus mánager

El programa *optimus-manager-qt* proporciona un icono en la bandeja del sistema para cambiar fácilmente entre gráficas. También incluye una GUI para configurar opciones sin editar el archivo de configuración manualmente.

Antes de instalar este software, es necesario tener un complemento de iconos en el shell activado, de otra manera no se podrá utilizar por mas que este programa este cargado en memoria:

~~~TEXT
git clone https://aur.archlinux.org/optimus-manager-qt.git
cd optimus-manager-qt
~~~

Antes de instalar si estamos usando el escritorio de KDE Plasma tenemos que habilitar las opciones extendidas:

~~~TEXT
nano PKGBUILD
~~~

Y ponemos a true la siguiente linea:

~~~TEXT
_with_plasma=true
~~~

Ahora si podemos instalar, si tu escritorio no es KDE pasma el *PKGBUILD* no se toca:

~~~TEXT
makepkg -si
~~~

### Probar el rendimiento del sistema

Los siguientes comandos proporcionan información útil sobre el sistema gráfico:

~~~TEXT
# Informacion de la placa utilizada
glxinfo | grep -i vendor
glxinfo | grep render
glxinfo | grep direct
glxinfo | grep OpenGL

# Benchmarks
glxgears -info
glxspheres64 -info
~~~

## Complementos y programas importantes

### Google Chrome

El navegador por excelencia *(por ahora)*:

~~~TEXT
https://aur.archlinux.org/google-chrome.git
cd google-chrome
makepkg -si
~~~

### Extenciones

Lo mas recomendable es ir a Google Chrome e instalar el Plugin de Gnome Shell Extention. Para luego instalar las siguientes extenciones:

- [AppIndicator and KStatusNotifierItem Support.](https://extensions.gnome.org/extension/615/appindicator-support/)
- [Blur my Shell.](https://extensions.gnome.org/extension/3193/blur-my-shell/)
- [Gnome 40 UI Improvements.](https://extensions.gnome.org/extension/4158/gnome-40-ui-improvements/)

### PAMAC

Este gestor de paquetes y actualizaciones, es muy completo y tiene soporte para Flatpak, Snap y AUR. Así que es muy bueno y recomendable:

~~~TEXT
https://aur.archlinux.org/pamac-all.git
cd pamac-all
makepkg -si
~~~

Luego de instalar el gestor, es recomendable reiniciar el PC y habilitar la extención de sistema *Pamac Updates Indicator.*

### Instalar Psensors

Este Software permite la monitorización de sensores de temperatura:

~~~TEXT
sudo pacman -S lm_sensors
~~~

Escaneamos todos los sensores de la PC:

~~~TEXT
sudo sensors-detect
~~~

Por último instalamos la utilidad de Psensors.

~~~TEXT
sudo pacman -S install psensor
~~~
