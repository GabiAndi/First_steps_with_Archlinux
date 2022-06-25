<h1>Contenido</h1>

- [Sobre Arch Linux](#sobre-arch-linux)
- [Instalación](#instalación)
  - [Obtener la ISO](#obtener-la-iso)
  - [Iniciar desde el USB](#iniciar-desde-el-usb)
  - [Configuración de teclado](#configuración-de-teclado)
  - [Conectar a internet](#conectar-a-internet)
  - [Actualizar el reloj del sistema](#actualizar-el-reloj-del-sistema)
  - [Crear las particiones para la instalación](#crear-las-particiones-para-la-instalación)
  - [Formatear las particiones creadas](#formatear-las-particiones-creadas)
  - [Montar las particiones creadas](#montar-las-particiones-creadas)
  - [Instalación de Arch Linux](#instalación-de-arch-linux)
  - [Generar el archivo fstab](#generar-el-archivo-fstab)
  - [Cambiamos a la raíz de la instalación](#cambiamos-a-la-raíz-de-la-instalación)
  - [Configuramos la zona horaria](#configuramos-la-zona-horaria)
  - [Sincronizamos el reloj](#sincronizamos-el-reloj)
  - [Establecemos el idioma](#establecemos-el-idioma)
  - [Distribución de teclado](#distribución-de-teclado)
  - [Generamos el archivo de locales](#generamos-el-archivo-de-locales)
  - [Configuración de la red](#configuración-de-la-red)
  - [Se habilita el NetworkManger](#se-habilita-el-networkmanger)
  - [Cambio de contraseña de root](#cambio-de-contraseña-de-root)
  - [Instalación del bootloader](#instalación-del-bootloader)
  - [Finalizamos la instalación](#finalizamos-la-instalación)
- [Primer inicio de sesión](#primer-inicio-de-sesión)
  - [Conectar a una red WiFi](#conectar-a-una-red-wifi)
  - [Creamos un usuario](#creamos-un-usuario)
  - [Creamos una contraseña para el nuevo usuario](#creamos-una-contraseña-para-el-nuevo-usuario)
  - [Añadimos a sudo el nuevo usuario](#añadimos-a-sudo-el-nuevo-usuario)
  - [Iniciamos sesión con el nuevo usuario](#iniciamos-sesión-con-el-nuevo-usuario)
  - [Actualizamos el sistema](#actualizamos-el-sistema)
  - [Drivers base](#drivers-base)
- [Configuración de un entorno de escritorio](#configuración-de-un-entorno-de-escritorio)
  - [Instalación de Xorg](#instalación-de-xorg)
  - [Instalación de GNOME](#instalación-de-gnome)
  - [Instalación de KDE Plasma](#instalación-de-kde-plasma)
  - [Drivers de la targeta grafica](#drivers-de-la-targeta-grafica)
  - [Instalación de Plymouth](#instalación-de-plymouth)
  - [Instalación de optimus-manager](#instalación-de-optimus-manager)
  - [Icono de optimus mánager](#icono-de-optimus-mánager)
  - [Probar el rendimiento del sistema](#probar-el-rendimiento-del-sistema)
- [Complementos y programas](#complementos-y-programas)
  - [Yay como gestor de paquetes](#yay-como-gestor-de-paquetes)
  - [Google Chrome](#google-chrome)
  - [Extenciones para GNOME Shell](#extenciones-para-gnome-shell)
  - [Tema para aplicaciones heredadas](#tema-para-aplicaciones-heredadas)
  - [PAMAC para GNOME](#pamac-para-gnome)
  - [Kitty](#kitty)
    - [Instalación](#instalación-1)
    - [Configuración](#configuración)
  - [ZSH](#zsh)
    - [Instalación](#instalación-2)
    - [ZSH como shell predeterminada](#zsh-como-shell-predeterminada)
    - [Configuración](#configuración-1)
    - [Powerlevel10k](#powerlevel10k)
    - [Resaltado de sintaxis](#resaltado-de-sintaxis)
    - [Sugerir comandos](#sugerir-comandos)
  - [VS Code](#vs-code)
  - [Psensors](#psensors)
  - [Qemu/KVM](#qemukvm)
- [Configuración para servidor](#configuración-para-servidor)
  - [Programas útiles](#programas-útiles)
    - [Htop](#htop)
    - [Screen](#screen)
    - [Mc](#mc)
  - [Establecer IP estática](#establecer-ip-estática)
  - [Habilitar servidor SSH](#habilitar-servidor-ssh)
  - [Generar claves SSH](#generar-claves-ssh)
  - [Crear unidades RAID](#crear-unidades-raid)
  - [Servidor de archivos SAMBA](#servidor-de-archivos-samba)
  - [Wake On Lan](#wake-on-lan)
  - [Añadir soporte para UPS](#añadir-soporte-para-ups)

# Sobre Arch Linux

Arch Linux es una distribución Linux para computadoras x86_64 orientada a usuarios avanzados. Se compone en su mayor parte de software libre y de código abierto. Su modelo de desarrollo es de tipo [rolling release](https://es.wikipedia.org/wiki/Liberaci%C3%B3n_continua) y el enfoque de diseño persigue el [principio KISS.](https://es.wikipedia.org/wiki/Principio_KISS)

Para instalar y configurar este sistema operativo se necesita un grado de conocimiento superior al básico. No obstante, se puede mantener y administrar el sistema de forma sencilla. Los creadores y la comunidad, denominan como *filosofía*, los siguientes tres aspectos:

1. Mantener el sistema lo más simple y ligero posible, siguiendo el llamado principio [KISS.](https://es.wikipedia.org/wiki/Principio_KISS)
2. Los principios del liderazgo del proyecto, Aaron Griffin, también son tomados como referencia: *Fiarse de las GUIs para construir y configurar tu sistema operativo termina dañando a los usuarios finales. Intentar ocultar la complejidad del sistema, termina complicando al sistema. Las capas de abstracción que sirven para ocultar el funcionamiento interno nunca son una cosa buena. En cambio, los componentes internos deberían ser diseñados de forma que no necesiten ser ocultados*.
3. Arch Linux permite al usuario hacer las contribuciones que desee, mientras estas no vayan en contra de la filosofía.

# Instalación

Este documentos es un resumen de instalación sacado de la [guía oficial de Arch Linux](https://wiki.archlinux.org/title/Installation_guide), en donde se detalla en profundidad el proceso de instalación. Por lo que recomendamos que lea la documentación oficial antes de seguir los consejos de este post.

## Obtener la ISO

Podemos obtener la última instantanea de la [página de Arch Linux](https://archlinux.org/) y grabarla en un USB con mínimo 4G de espacio.

Luego de verificar que la ISO se descargo correctamente, podemos pasarla al USB con el comando *dd*:

~~~bash
sudo dd if=ruta_de_la_iso of=dispositivo status=progress
sync
~~~

Usamos sync para asegurarnos de que todo el cache se guardo en el dispositivo.

## Iniciar desde el USB

Debemos bootear desde el USB para poder iniciar el proceso de instalación. Normalmente, cuando se inicia el Arch Live preguntará con que opción queremos bootear. Luego de bootear el sistema inicia y nos mostrará una consola completamente vacía a la espera de comandos.

## Configuración de teclado

Es necesario primero que nada configurar la distribución del teclado para tener correctas las teclas. En el caso de un teclado *Español Latinoamerica*, el comando sería el siguiente:

~~~bash
loadkeys la-latin1
~~~

## Conectar a internet

La instalación del sistema requiere conexión a internet para poder descargar todos los paquetes necesarios. Para verificar la conexión a internet ejecutamos:

~~~bash
ip a
~~~

Si estamos conectados por ethernet (que es lo mas recomendable), la conexión se debería haber realizado automáticamente. De modo contrario se puede conectar a una red WiFi usando una utilidad de la instalación:

~~~bash
iwclt
~~~

Cuando ingrese este comando se activará la utilidad, y podrá ver los dispositivos wireless disponibles en el equipo:

~~~bash
device list
~~~

Puede escanear las redes disponibles:

~~~bash
station wlan0 scan
~~~

Ahora espere unos 10 segundos a que termine el escaneo, luego podrá ver las redes disponibles:

~~~bash
station wlan0 get-networks
~~~

Para conectarse a una red puede correr:

~~~bash
station wlan0 connect nombre_del_wifi
~~~

Se le pedirá la contraseña, y se intentará conectar a la red indicada.Luego puede salir de la utilidad y volver a comprobar si tiene acceso a la red:

~~~bash
exit
~~~

## Actualizar el reloj del sistema

~~~bash
timedatectl set-ntp true
~~~

## Crear las particiones para la instalación

Lo que se debe hacer ahora es crear las particiones de disco en donde se instalará el sistema. Podemos ver la cantidad de discos y sus particiones ejecutando:

~~~bash
lsblk
~~~

La utilidad recomendada para crear particiones en sistemas GPT es *gdisk* y para MBR es *fdisk*.

~~~bash
gdisk /dev/sdx

fdisk /dev/sdx
~~~

Para poder instalar cualquier distribución Linux en un Sistema EFI, debemos crear obligatoriamente las siguientes particiones:

1. Partición EFI con formato FAT32 de al menos 500MB.
2. Partición raíz con formato ext4.

Adicionalmente, es recomendable crear una partición de intercambio SWAP. Y también se pueden crear particiones extra para el */home* o */opt*.

Estas particiones adicionales se deben formatear y montar en la ubicación correspondiente.

Si su sistema **no es UEFI**, no deberá crear la partición EFI obviamente, todo lo demas aplica para ambos sistemas.

## Formatear las particiones creadas

Una vez creamos las particiones que usaremos, procedemos a darles el formato correspondiente.

Para particiones ext4:

~~~bash
mkfs.ext4 /dev/sdxn
~~~

Para particiones fat32:

~~~bash
mkfs.fat -F32 /dev/sdxn
~~~

Para particiones swap:

~~~bash
mkswap /dev/sdxn
~~~

## Montar las particiones creadas

Este es el paso previo a instalar el sistema, debemos montar las unidades configuradas anteriormente en las ubicaciones correspondientes, para luego generar el archivo de montajes al inicio del sistema. Para este tutorial, el sistema se montara en */mnt* del Live USB.

La partición del sistema raíz:

~~~bash
mount /dev/sdxn /mnt
~~~

Para la partición UEFI, debemos crear una ruta especial bajo la raíz llamada */boot*:

~~~bash
mkdir /mnt/boot
mount /dev/sdxn /mnt/boot
~~~

Particiones extras como */home* o */opt* también deben estar dentro de la raíz:

~~~bash
mkdir /mnt/home
mount /dev/sdxn /mnt/home

mkdir /mnt/opt
mount /dev/sdxn /mnt/opt
~~~

Para la partición SWAP:

~~~bash
swapon /dev/sdxn
~~~

## Instalación de Arch Linux

Una vez tenemos todos los pasos anteriores cumplidos pasamos a finalmente instalar el sistema en las particiones que elegimos. Con *pacstrap* seleccionamos la ubicación de instalación (en mi caso */mnt*) y elegimos los paquetes a instalar:

~~~bash
pacstrap /mnt base base-devel git linux linux-firmware linux-firmware-qlogic linux-headers nano networkmanager dialog bash-completion
~~~

## Generar el archivo fstab

Para que las particiones se monten al arranque y cumplan con su función debemos generar el *fstab*:

~~~bash
genfstab -U /mnt >> /mnt/etc/fstab
~~~

## Cambiamos a la raíz de la instalación

Para utilizar el sistema recién instalado, podemos utilizar el siguiente comando:

~~~bash
arch-chroot /mnt
~~~

## Configuramos la zona horaria

~~~bash
ln -sf /usr/share/zoneinfo/America/Argentina/Buenos_aires /etc/localtime
~~~

## Sincronizamos el reloj

~~~bash
hwclock --systohc
~~~

## Establecemos el idioma

Lo primero que debemos hacer es seleccionar el idioma que utilizará el sistema:

~~~bash
nano /etc/locale.conf
~~~

Y añadimos la siguiente linea:

~~~text
LANG=es_AR.UTF-8
~~~

Paso siguiente configuramos el archivo de locales. Debemos descomentar nuestro idioma preferido y también uno en ingles que será el base:

~~~bash
nano /etc/locale.gen
~~~

Y descomentamos:

~~~text
en_US.UTF-8 UTF-8
es_AR.UTF-8 UTF-8
~~~

## Distribución de teclado

Ahora creamos un archivo para la distribución del teclado:

~~~bash
nano /etc/vconsole.conf
~~~

Y añadimos:

~~~text
KEYMAP=la-latin1
~~~

## Generamos el archivo de locales

Generamos los archivos de configuración para establecer los cambios que generamos anteriormente:

~~~bash
locale-gen
~~~

## Configuración de la red

Debemos darle un nombre a nuestra PC, para que sea identificable para otros equipos, lo podemos hacer editando el archivo:

~~~bash
nano /etc/hostname
~~~

Y añadimos:

~~~text
myhostname
~~~

Lo mismo para el archivo de hosts, este configurará otras cuestiones para el *NetworkManager*:

~~~bash
nano /etc/hosts
~~~

Y añadimos lo siguiente:

~~~text
127.0.0.1   localhost
::1         localhost
127.0.1.1   myhostname.localdomain myhostname
~~~

## Se habilita el NetworkManger

Debemos habilitar el servicio de administración de redes, yo personalmente prefiero NetworkManger:

~~~bash
systemctl enable NetworkManager
~~~

## Cambio de contraseña de root

Ingrese la contraseña de usuario root que prefiera, esto podemos eliminarlo mas tarde por seguridad, en un inicio vamos a logearnos en nuestro sistema como root:

~~~bash
passwd
~~~

## Instalación del bootloader

A continuación se instalara GRUB para UEFI o BIOS, esto permitirá al sistema arrancar:

~~~bash
pacman -S grub ntfs-3g
~~~

Si tenes un sistema UEFI adicional debes instalar:

~~~bash
pacman -S efibootmgr
~~~

Es recomendable también instalar los microcodes, según la versión del procesador.

Para AMD:

~~~bash
pacman -S amd-ucode
~~~

Para INTEL:

~~~bash
pacman -S intel-ucode
~~~

Para sistemas UEFI configure el grub para su sistema bajo la ruta de la partición EFI:

~~~bash
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id='arch'
~~~

Para sistemas BIOS debemos instalar el grub en la partición MBR del disco de arranque directamente:

~~~bash
grub-install --target=i386-pc /dev/sdx
~~~

Si tenes otro sistema operativo en alguna otra unidad o partición y queres que grub la detecte, instale el siguiente paquete:

~~~bash
pacman -S os-prober
~~~

Luego se tiene que montar las unidades con sistema operativo:

~~~bash
mkdir /mnt/extra_os
mount /dev/sdxn /mnt/extra_os
~~~

Corremos os-prober para comprobar que sistema hay instalado:

~~~bash
os-prober
~~~

Ahora generamos la configuración básica del grub:

~~~bash
grub-mkconfig -o /boot/grub/grub.cfg
~~~

Como último paso si tu sistema es UEFI se debe copiar el archivo de inicio:

~~~bash
mkdir /boot/EFI/boot
cp /boot/EFI/arch/grubx64.efi /boot/EFI/boot/bootx64.efi
~~~

## Finalizamos la instalación

Salimos de arch-chroot:

~~~bash
exit
~~~

Desmontamos todas las unidades de la instalación:

~~~bash
umount -R /mnt
~~~

Y por último apagamos el equipo:

~~~bash
systemctl poweroff
~~~

Ahora desconectamos el medio de instalación y ya podemos volver a iniciar el sistema.

# Primer inicio de sesión

## Conectar a una red WiFi

Usando NetworkManager podemos conectarnos a una red:

~~~bash
nmcli dev wifi connect nombre_de_wifi password contraseña
~~~

## Creamos un usuario

Una nueva instalación te deja solo con la cuenta de superusuario, más conocida como *root*. Iniciar sesión como *root* durante períodos prolongados de tiempo, posiblemente incluso exponerlo a través de SSH en un servidor, es inseguro. Es por eso que debemos crear un usuario para nosotros:

~~~bash
useradd -m -G ftp,http,log,rfkill,sys,uucp,audio,storage,video,wheel,games,power,scanner,kvm -s /bin/bash nombre_usuario
~~~

## Creamos una contraseña para el nuevo usuario

Para mayor seguridad es recomendable que le pongamos contraseña al nuevo usuario creado.

~~~bash
passwd nombre_usuario
~~~

## Añadimos a sudo el nuevo usuario

Editamos el archivo de sudo y luego descomentamos la linea que habilita al grupo wheel:

~~~bash
EDITOR=nano visudo
~~~

Y descomentamos la linea:

~~~text
%wheel ALL=(ALL) ALL
~~~

Esto permitirá que usuarios que pertenescan al grupo wheel puedan usar sudo.

## Iniciamos sesión con el nuevo usuario

Salimos de la sesión de root:

~~~bash
exit
~~~

Y nos logeamos con el nuevo usuario y contraseña.

## Actualizamos el sistema

Ahora que tenemos el sistema instalado y configurado con una cuenta de usuario válida, lo que debemos hacer es comprobar si existen actualizaciones disponibles. Pero antes, es buena idea habilitar el soporte multilib y unas opciones más:

~~~bash
sudo nano /etc/pacman.conf
~~~

Y descomentamos las siguientes lineas:

~~~text
Color
ParallelDownloads = 6
~~~

~~~text
[multilib]
Include = /etc/pacman.d/mirrorlist
~~~

Luego, actualizamos la lista de repositorios y verificamos las actualizaciones:

~~~bash
sudo pacman -Syu
~~~

## Drivers base

Si ejecutamos:

~~~bash
sudo mkinitcpio -p linux
~~~

Y obtenemos una salida como la siguiente:

~~~text
==> WARNING: Possibly missing firmware for module: aic94xx
==> WARNING: Possibly missing firmware for module: wd719x
==> WARNING: Possibly missing firmware for module: xhci_pci
~~~

Tenemos un problema con drivers que nos estan faltando. La solucion es instalarlos para tener el mejor rendimiento de nuestro hardware. Para ello nos dirigimos a una carpeta temporal en donde descargar archivos:

~~~bash
cd
~~~

Y ahora luego de buscar los drivers en AUR o el repositorio oficial, los instalamos:

~~~bash
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

Cuando se instalen los drivers ahora podemos borrar los directorios de compilación.

# Configuración de un entorno de escritorio

Un entorno de escritorio es recomendado si la computadora en donde se esta instalando el sistema requiere de capacidades gráficas. Existen una variedad bastante grande de entornos de escritorios, a mi en lo personal me gustan dos: GNOME y KDE Plasma.

Antes de instalar un entorno debemos tener el servidor de gráficos. Xorg esta en casi todo sistema, sin embargo, Wayland esta ganando terreno últimamente.

## Instalación de Xorg

Podemos instalar el servidor de ventanas X con el comando:

~~~bash
sudo pacman -S xorg
~~~

## Instalación de GNOME

Si GNOME es tu escritorio, lo podes instalar tan facil como el siguiente comando:

~~~bash
sudo pacman -S gnome gnome-extra
~~~

También, recomiendo instalar el paquete de iconos Papirus y la utilidad de impresoras CUPS:

~~~bash
sudo pacman -S papirus-icon-theme cups ufw
~~~

Por último, debemos iniciar el servicio de administrador de sesiones y de impresión:

~~~bash
sudo systemctl enable gdm
sudo systemctl enable cups
sudo systemctl enable bluetooth
~~~

Ahora podemos reiniciar la PC para entrar en el modo gráfico.

## Instalación de KDE Plasma

Si queres tener en tu carpeta de usuario, carpetas como *Documentos*, *Descargas*, *Música*, *etc*. Instalamos el servicios de carpetas de usuario:

~~~bash
sudo pacman -S xdg-user-dirs
~~~

Y corremos la utilidad:

~~~bash
xdg-user-dirs-update
~~~

Para instalar KDE Plasma ejecutamos el siguiente comando:

~~~bash
sudo pacman -S plasma plasma-meta kde-applications kde-applications-meta packagekit-qt5
~~~

Es recomendable instalar temas tanto para Qt y Gtk, ademas de otros paquetes base:

~~~bash
sudo pacman -S papirus-icon-theme arc-gtk-theme cups ufw
~~~

Por último habilitamos servicios del sistema:

~~~bash
sudo systemctl enable sddm
sudo systemctl enable cups
sudo systemctl enable bluetooth
~~~

Ahora podemos reiniciar la PC para entrar en el modo gráfico.

## Drivers de la targeta grafica

Primero instalamos las utilidades de mesa, esto nos servirá para haberiguar que gráfica esta renderizando el escritorio:

~~~bash
sudo pacman -S mesa lib32-mesa mesa-demos mesa-utils
~~~

Ejecutamos el comando siguiente:

~~~bash
glxinfo | grep "OpenGL renderer"
~~~

Obteniendo la salida:

~~~text
OpenGL renderer string: Mesa Intel(R) HD Graphics 630 (KBL GT2)
~~~

Esto significa que estamos utilizando la integrada de INTEL para el video. Yo tengo una Notebook con gráficos dedicados Nvidia GTX 1050, ademas de una integragada HD630 de un i5 de 7ma generación.

Para instalar los drivers de la tarjeta gráfica dedicada puedo ejecutar los siguientes comandos:

~~~bash
sudo pacman -S nvidia nvidia-utils lib32-nvidia-utils nvidia-settings virtualgl
~~~

Una vez instalado los drivers se debe reiniciar la PC antes de hacer cualquier otra cosa.

## Instalación de Plymouth

Plymouth es un proyecto de Fedora y ahora figura entre los recursos oficiales de freedesktop.org que proporciona un proceso de arranque gráfico sin parpadeos. Se basa en la configuración del modo kernel (KMS) para configurar la resolución nativa de la pantalla lo antes posible, luego proporciona una pantalla de inicio llamativa que conduce hasta el administrador de inicio de sesión.

Para poder instalarlo tenemos que clonar un repositorio de aur:

~~~bash
git clone https://aur.archlinux.org/plymouth-git.git
cd plymouth-git
makepkg -si
~~~

Agregue plymouth a la HOOKS en mkinitcpio.conf. Debe agregarse después de "base udev" para que funcione: 

~~~bash
sudo nano /etc/mkinitcpio.conf
~~~

Quedando el archivo como:

~~~text
HOOKS=(base udev plymouth ...)
~~~

Luego, añadir "quiet loglevel=3 splash vt.global_cursor_default=0" a los parámetros del núcleo. Para grub2 quedaría:

~~~bash
sudo nano /etc/default/grub
~~~

~~~text
GRUB_CMDLINE_LINUX_DEFAULT="quiet loglevel=3 splash vt.global_cursor_default=0"
GRUB_CMDLINE_LINUX="splash"
~~~

Ahora puede seleccionar los temas editando el archivo:

~~~
sudo nano /etc/plymouth/plymouthd.conf
~~~

Por ejemplo:

~~~text
[Daemon]
Theme=script
ShowDelay=8
DeviceTimeout=8
~~~

O si esta utilizando otro tema, por ejemplo ***spinner***, si quiere el logo de Arch:

~~~bash
sudo cp /usr/share/plymouth/arch-logo.png /usr/share/plymouth/themes/spinner/watermark.png
~~~

~~~text
[Daemon]
Theme=spinner
ShowDelay=8
DeviceTimeout=8
~~~

Y para centrar el tema al archivo:

~~~bash
sudo nano /usr/share/plymouth/themes/spinner/spinner.plymouth
~~~

~~~text
WatermarkHorizontalAlignment=.5
WatermarkVerticalAlignment=.5
~~~

Cuando tenga todo configurado ejecute:

~~~bash
sudo mkinitcpio -p linux
sudo grub-mkconfig -o /boot/grub/grub.cfg
~~~

Todavía no reinicie Arch si desea instalar optimus-manager

## Instalación de optimus-manager

Este programa de Linux proporciona una solución para el cambio de GPU en laptops Optimus *(es decir, laptops con una configuración dual Nvidia/Intel o Nvidia/AMD)*.

Para usuarios de **GNOME** primero hay que instalar un gestor de sesiones parcheado:

~~~bash
git clone https://aur.archlinux.org/gdm-prime.git
cd gdm-prime
~~~

Si utiliza plymouth, cuando clone el repositorio debe habilitar el flag para el soporte en el archivo *PKGBUILD*:

~~~text
nano PKGBUILD
~~~

Busque la linea:

~~~text
-D plymouth=enabled
~~~

Luego ya puede instalar:

~~~bash
makepkg -si
~~~

Una vez instalada la utilidad, editamos el archivo */etc/gdm/custom.conf*:

~~~bash
sudo nano /etc/gdm/custom.conf
~~~

Y descomentamos la siguiente linea:

~~~text
WaylandEnable=false
~~~

Se tiene que comprobar que no exista el archivo */etc/X11/xorg.conf*, si es asi, hay que eliminarlo.

Entonoces para instalarlo ejecutamos:

~~~bash
git clone https://aur.archlinux.org/optimus-manager.git
cd optimus-manager
makepkg -si
~~~

Despues de instalar se tiene que **reiniciar el ordenador.**

Una vez reiniciado ahora podemos ejecutar los siguientes comandos para cambiar el modo de uso de la gráfica:

~~~bash
optimus-manager --switch nvidia

optimus-manager --switch integrated

optimus-manager --switch hybrid
~~~

## Icono de optimus mánager

El programa *optimus-manager-qt* proporciona un icono en la bandeja del sistema para cambiar fácilmente entre gráficas. También incluye una GUI para configurar opciones sin editar el archivo de configuración manualmente.

Antes de instalar este software, es necesario tener un complemento de iconos en el shell activado, de otra manera no se podrá utilizar por mas que este programa este cargado en memoria:

~~~bash
git clone https://aur.archlinux.org/optimus-manager-qt.git
cd optimus-manager-qt
~~~

Antes de instalar si estamos usando el escritorio de KDE Plasma tenemos que habilitar las opciones extendidas:

~~~bash
nano PKGBUILD
~~~

Y ponemos a true la siguiente linea:

~~~text
_with_plasma=true
~~~

Ahora si podemos instalar, si tu escritorio no es KDE pasma el *PKGBUILD* no se toca:

~~~bash
makepkg -si
~~~

## Probar el rendimiento del sistema

Los siguientes comandos proporcionan información útil sobre el sistema gráfico.

Informacion de la placa utilizada:

~~~bash
glxinfo | grep -i vendor
glxinfo | grep render
glxinfo | grep direct
glxinfo | grep OpenGL
~~~

Benchmarks:

~~~bash
glxgears -info
glxspheres64 -info
~~~

# Complementos y programas

## Yay como gestor de paquetes

Yay es un asistente de instalación de paquetes para ArchLinux con capacidad para instalar paquetes desde el Repositorio de Usuarios de ArchLinux (AUR).

Podemos instalarlo del siguiente repositorio de AUR:

~~~bash
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
~~~

## Google Chrome

El navegador por excelencia *(por ahora)*:

~~~bash
yay -S google-chrome
~~~

## Extenciones para GNOME Shell

Lo mas recomendable es ir a Google Chrome e instalar el Plugin de Gnome Shell Extention:

~~~bash
yay -S chrome-gnome-shell
~~~

Luego instalar las siguientes extenciones:

- [AppIndicator and KStatusNotifierItem Support.](https://extensions.gnome.org/extension/615/appindicator-support/)
- [Blur my Shell.](https://extensions.gnome.org/extension/3193/blur-my-shell/)
- [Gnome 40 UI Improvements.](https://extensions.gnome.org/extension/4158/gnome-40-ui-improvements/)

## Tema para aplicaciones heredadas

Aplicaciones gtk3 no tendran el tema por defecto que tienen las aplicaciones gtk4. Con lo cual debe instalar una extención de gnome:

- [Legacy (GTK3) Theme Scheme Auto Switcher.](https://extensions.gnome.org/extension/4998/legacy-gtk3-theme-scheme-auto-switcher/)

Luego deberá seguir el instructivo de la [página](https://fostips.com/gtk3-light-dark-fedora-gnome/). En donde se explica que se debe crear la carpeta *$HOME/.themes* y en su interior extraer el contenido del tema que se encuentra en el repositorio [adw-gtk3](https://github.com/lassekongo83/adw-gtk3/releases).

Luego por último en retoques, en la pestaña apariencia, puede seleccionar el tema para las aplicaciones heredadas.

## PAMAC para GNOME

Este gestor de paquetes y actualizaciones, es muy completo y tiene soporte para Flatpak, Snap y AUR. Así que es muy bueno y recomendable:

~~~bash
git clone https://aur.archlinux.org/pamac-all.git
cd pamac-all
makepkg -si
~~~

Luego de instalar el gestor, es recomendable reiniciar el PC y habilitar la extención de sistema *Pamac Updates Indicator.*

## Kitty

Kitty es un emulador de terminal basado en OpenGL programable con TrueColor, compatibilidad con ligaduras, extensiones de protocolo para entrada de teclado y representación de imágenes. También ofrece capacidades de mosaico, como GNU Screen o tmux.

### Instalación

Para poder instalarla basta con ejecutar:

~~~bash
sudo pacman -S kitty
~~~

### Configuración

Kitty almacena su configuración en *$HOME/.config/kitty/kitty.conf* y la configuración predeterminada se puede encontrar en */usr/share/doc/kitty/kitty.conf*. Se pueden ajustar las fuentes, los colores, los cursores y los comportamientos de desplazamiento hacia atrás. Puedes ver todas las opciones disponibles en la [documentación oficial.](https://sw.kovidgoyal.net/kitty/conf/)

## ZSH

Z shell (o simplemente zsh) es un potente intérprete de comandos para sistemas operativos de tipo Unix. Zsh se diseñó para poder usarse interactivamente. Se le han incorporado muchas de las características principales de otras shells de Unix como bash, ksh, o tcsh y además posee características propias originales.

### Instalación

Para poder instalarla basta con ejecutar:

~~~bash
sudo pacman -S zsh
~~~

### ZSH como shell predeterminada

Una vez instalado el paquete zsh debemos hacer que sea la nueva shell de nuestro sistema, podemos ver las shells instalas con el comando *chsh*:

~~~bash
chsh -l
~~~

~~~bash
chsh -s /bin/zsh
~~~

### Configuración

La primera vez que ejecutemos zsh nos mostrará un asistente para una primera configuracion básica de zsh, donde deberemos de responder a una serie de preguntas para establecer el tamaño del historial, personalizar el número de líneas a guardar, etc.

Si quieres ejecutar el asistente de forma manual puedes ejecutar:

~~~bash
autoload -Uz zsh-newuser-install
zsh-newuser-install -f
~~~

### Powerlevel10k

Te recomiendo que instales la versión desde su [repositorio](https://github.com/romkatv/powerlevel10k/), ya que el paquete comunitario *zsh-theme-powerlevel10k* históricamente se ha estado rompiendo con frecuencia y durante largos períodos de tiempo. No lo uses.

Su instalación es muy simple, introduce el siguiente comando en la terminal:

~~~bash
yay -S zsh-theme-powerlevel10k-git powerline-common powerline-fonts awesome-terminal-fonts ttf-meslo-nerd-font-powerlevel10k
~~~

Cuando finalice la instalación vuelve a poner en la terminal;

~~~bash
echo 'source /usr/share/zsh-theme-powerlevel10k/powerlevel10k.zsh-theme' >> ~/.zshrc
~~~

Disponemos de un wizard para ayudarnos a configurar nuestro prompt, para ello como terminamos la instalación, debemos reiniciar la terminal. Si luego d hacer esto el wizard no aparece, simplemente ejecuta:

~~~bash
p10k configure
~~~

### Resaltado de sintaxis

Si quieres disponer de un resaltado de sintaxis para la shell al igual que en Fish pero para Zsh, puedes instalar *zsh-syntax-highlighting* desde los repositorios:

~~~bash
sudo pacman -S zsh-syntax-highlighting
~~~

Y añadimos la configuración al *.zshrc*:

~~~text
source /usr/share/zsh/plugins/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
~~~

### Sugerir comandos

Busca en los repositorios oficiales el paquete *zsh-autosuggestions* e instálalo:

~~~bash
sudo pacman -S zsh-autosuggestions
~~~

Y añadimos la configuración al *.zshrc*:

~~~text
source /usr/share/zsh/plugins/zsh-autosuggestions/zsh-autosuggestions.zsh
~~~

## VS Code

Visual Studio Code es un editor de código fuente desarrollado por Microsoft para Windows, Linux, macOS y Web. Incluye soporte para la depuración, control integrado de Git, resaltado de sintaxis, finalización inteligente de código, fragmentos y refactorización de código. Se puede instalar de la siguiente manera:

~~~bash
sudo pacman -S gnome-keyring libsecret libgnome-keyring

yay -S visual-studio-code-bin
~~~

## Psensors

Este software permite la monitorización de sensores de temperatura:

~~~bash
sudo pacman -S lm_sensors
~~~

Escaneamos todos los sensores de la PC:

~~~bash
sudo sensors-detect
~~~

Por último instalamos la utilidad de Psensors.

~~~bash
sudo pacman -S psensor
~~~

## Qemu/KVM

La mejor opción para virtualizar en Linux. Lo primero que hacemos es instalar los paquetes necesarios:

~~~bash
sudo pacman -S qemu-full samba dmidecode ebtables dnsmasq libvirt bridge-utils openbsd-netcat radvd virt-manager virt-viewer ifplugd ifenslave tcl edk2-ovmf
~~~

Nos preguntará si queremos reemplazar iptables por iptables-nft, le diremos que si para poder instalar ebtables, el cual es necesario para poder tener conectividad de red en las máquinas virtuales.

Agregamos nuestro usuario a los grupos kvm y polkitd:

~~~bash
sudo usermod -aG kvm $USER
sudo usermod -aG polkitd $USER
~~~

Cargamos los módulos necesarios. En caso de tener un procesador intel sería así:

~~~bash
sudo modprobe kvm-intel
sudo modprobe kvm
~~~

En caso de ser AMD sería así:

~~~bash
sudo modprobe kvm-amd
sudo modprobe kvm
~~~

Ahora habilitamos el servicio:

~~~bash
sudo systemctl enable libvirtd
sudo systemctl start libvirtd
~~~

Le damos permiso a nuestro usuario para poder gestionar máquinas virtuales mediante el siguiente archivo:

~~~bash
sudo nano /etc/polkit-1/rules.d/50-org.libvirt.unix.manage.rules
~~~

Agregamos lo siguiente:

~~~text
polkit.addRule(function(action, subject) {
if (action.id == "org.libvirt.unix.manage" &&
  subject.user == "NOMBRE_USUARIO") {
  return polkit.Result.YES;
}
});
~~~

Reemplazamos NOMBRE_USUARIO por nuestro nombre de usuario. Reiniciamos y abrimos el gestor de máquinas virtuales.

# Configuración para servidor

## Programas útiles

### Htop

Htop es un programa para ver por consola el estado rápido del sistema y los procesos en ejecución:

~~~bash
sudo pacman -S htop
~~~

### Screen

Screen permite en una misma sesión de ssh tener multiples espacios de trabajo, esto es ideal para mantener el trabajo aunque la conexión se cierre repentinamente:

~~~bash
sudo pacman -S screen
~~~

### Mc

Mc es como un administrador de archivos pero para consola, muy util:

~~~bash
sudo pacman -S mc
~~~

## Establecer IP estática

Si disponemos o damos algún servicio, seguramente necesitaremos dejar una IP fija para que el servidor sea mas facil de ubicar en la red, esto lo podemos hacer con Network Manager.

Primero listamos las conexiones que tenemos:

~~~bash
nmcli connection
~~~

Vamos a utilizar el nombre de la conexión para setear los parametros en el *nmcli*:

~~~bash
nmcli connection modify "Conexión cableada 1" \
ipv4.addresses "192.168.1.200/24" \
ipv4.gateway "192.168.1.1" \
ipv4.dns "192.168.1.1,8.8.8.8" \
ipv4.method "manual"
~~~

Todos los archivos de configuración se guardan en */etc/NetworkManager/system-connections*.

## Habilitar servidor SSH

Primero instalamos el servidor:

~~~bash
sudo pacman -S openssh
~~~

Luego habilitamos el servicio:

~~~bash
sudo systemctl enable sshd
~~~

## Generar claves SSH

Para generar un conjunto de llaves SSH ejecutamos:

~~~bash
ssh-keygen -t rsa -b 4096 -C gabi -f ~/.ssh/id_rsa_gabi-sv

ssh-copy-id -i ~/.ssh/id_rsa_gabi-sv gabi@192.168.1.200
~~~

## Crear unidades RAID

Si se esta montando un servidor de archivos y tenes varios discos, lo mas seguro es que un esquema RAID se adapte a las necesidades de un file server.

Para la gestión de un RAID vamos a utilizar *mdadm*:

~~~bash
sudo pacman -S mdadm
~~~

Para preparar nuestros discos debemos crear las particiones con la utilidad *gdisk* o *fdisk*, como los discos de mas de 2TB se llevan bien con GPT vamos a utilizar *gdisk*. Creamos las particiones y les damos un tipo Linux RAID con código FD00.

Cuando tengamos los discos listos, vamos a crear la unidad RAID, en mi caso creo un RAID5 con 4 discos:

~~~bash
sudo mdadm --create --verbose --level=5 --chunk=512 --raid-devices=4 /dev/md/datos /dev/sdb1 /dev/sdc1 /dev/sdd1 /dev/sde1
~~~

Cuando el proceso inicie, la sincronización demorará un tiempo largo, podemos ver el proceso con:

~~~bash
cat /proc/mdstat
~~~

Una vez el proceso termine, debemos establecer la configuración para que el RAID este configurado con cada inicio del sistema, para eso ejecutamos el comando:

~~~bash
sudo mdadm --detail --scan
~~~

Y con la salida que nos da este comando editamos el archivo */etc/mdadm/mdadm.conf*:

~~~bash
sudo nano /etc/mdadm.conf
~~~

Al final del archivo añadimos lo obtenido por el comando anterior:

~~~text
ARRAY /dev/md/datos metadata=1.2 spares=1 name=gabi-sv:datos UUID=4d255030:3d4ac77d:3fa4f00d:7008e316
~~~

Le damos un formato a nuestro RAID:

~~~bash
sudo mkfs.ext4 -v -L datos -b 4096 -E stride=128,stripe-width=384 /dev/md/datos
~~~

Creamos una carpeta y montamos el sistema:

~~~bash
sudo mkdir /mnt/datos
sudo mount /dev/md/datos /mnt/datos
~~~

Actualizamos en fstab para que el disco se monte al iniciar el sistema. Abrimos el archivo y añadimos al final:

~~~text
UUID=d50ae083-8ab7-47ba-82dc-fafe85ded302 /mnt/datos ext4 rw,relatime 0 1
~~~

Podemos obtener el UUID de nuestro RAID con el comando:

~~~bash
sudo blkid
~~~

A partir de ahora cuando el sistema se inicie el RAID se montará automáticamente.

Y listo tenemos configurado el dispositivo para poder transferir datos a el. Si queremos escribir contenido sin ser sudo debemos cambiar los permisos.

## Servidor de archivos SAMBA

Samba es el conjunto de programas de interoperabilidad estándar de Windows para Linux y Unix. Desde 1992, Samba ha proporcionado servicios de impresión y archivos seguros, estables y rápidos para todos los clientes que utilizan el protocolo SMB/CIFS, como todas las versiones de DOS y Windows, OS/2, Linux y muchos otros.

Para configurar Samba se tiene que instalar el paquete:

~~~bash
sudo pacman -S samba
~~~

Samba se configura en */etc/samba/smb.conf*, que está ampliamente documentado en la wiki de arch. Es necesario crear este archivo antes de iniciar el servicio, para ello se puede tener una plantilla [del repositorio de Samba en GitHub.](https://git.samba.org/samba.git/?p=samba.git;a=blob_plain;f=examples/smb.conf.default;hb=HEAD)

Luego de crear el archivo habilitamos los servicios:

~~~bash
sudo systemctl enable smb
sudo systemctl enable nmb
sudo systemctl enable avahi-daemon
~~~

Podemos también iniciarlos:

~~~bash
sudo systemctl start smb
sudo systemctl start nmb
sudo systemctl start avahi-daemon
~~~

Para agregar usuarios a samba estos deben ser usuarios de linux, para añadir usuarios a linux podemos hacerlo con:

~~~bash
sudo useradd nombre_usuario
~~~

Luego creamos el usuario en samba:

~~~bash
sudo smbpasswd -a nombre_usuario
~~~

Podemos listar los usarios con:

~~~bash
sudo pdbedit -L
~~~

Podemos crear también grupos:

~~~bash
sudo groupadd grupo
~~~

Para grupos de samba:

~~~bash
sudo gpasswd grupo -a nombre_usuario
~~~

## Wake On Lan

Podemos configurar nuestra placa de RED y el sistema para poder despertar via LAN.

La placa base de la computadora de destino y el controlador de interfaz de red deben admitir Wake-on-LAN. La computadora de destino debe estar conectada físicamente (con un cable) a un enrutador o a la computadora de origen para que WOL funcione correctamente. Algunas tarjetas inalámbricas son compatibles con Wake on Wireless (WoWLAN o WoW).

Para averiguar el estado de la interfaz de red debemos instalar *ethtool*:

~~~bash
sudo pacman -S ethtool
~~~

Cuando la herramienta este instalada podemos comprobar el estado de alguna placa de red con el comando:

~~~bash
sudo ethtool nombre_interfaz
~~~

Casi al final de la descripcion habrá una linea que dice:

~~~text
Wake-on: d
~~~

Esta latra que le sigue a al campo dice en que estado esta la placa de red:

- d (disabled)
- p (PHY activity)
- u (unicast activity)
- m (multicast activity)
- b (broadcast activity)
- a (ARP activity)
- g (magic packet activity)

Para WOL se requiere que este en modo g. Podemos cambiar el estado de la placa con:

~~~bash
sudo ethtool -s nombre_interfaz wol g
~~~

Ahora si comprobamos el estado nuevamente vemos que cambio. Pero esto puede restablecerse en el proximo reinicio. Si lo hace podes hacer lo siguiente para dejar permanente la configuración.

Creamos el archivo */etc/systemd/system/wol@.service* con lo siguiente:

~~~text
[Unit]
Description=Wake-on-LAN for %i
Requires=network.target
After=network.target

[Service]
ExecStart=/usr/bin/ethtool -s %i wol g
Type=oneshot

[Install]
WantedBy=multi-user.target
~~~

Entonces podemos habilitar el servicio con:

~~~bash
sudo systemctl enable wol@nombre_interfaz
~~~

Y listo ya podemos hacer Wake On LAN.

## Añadir soporte para UPS

Para configurar una UPS en nuestro servidor tenemos que instalar el paquete *nut*:

~~~bash
sudo pacman -S nut
~~~

NUT tiene 3 demonios asociados:

- El conductor que se comunica con la UPS.
- Un servidor (upsd) que utiliza el controlador para informar el estado del UPS.
- Un demonio de supervisión (upsmon) que supervisa el servidor upsd y toma medidas en función de la información que recibe.

La idea es que si tiene varios sistemas conectados al UPS, uno puede comunicar el estado del UPS a través de la red y los demás pueden monitorear ese estado ejecutando sus propios servicios upsmon. NUT tiene una extensa documentación sobre la configuración, sin embargo, esto lo guiará a través de una configuración simple de un UPS USB y el servidor asociado y el monitor, todo en un sistema (configuración de escritorio común).

Para configurar la UPS lo hacemos en el archivo */etc/nut/ups.conf*:

~~~text
[nombre_ups]
    driver = usbhid-ups
    port = auto
    desc = "Descripcion"
~~~

Para iniciar el programa ejecutamos:

~~~bash
sudo upsdrvctl start
~~~
