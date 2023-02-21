# QEMU toolchain for MAC survival kit (Apple silicon Mx)

(ENGLISH VERSION, para la version en ESPAÑOL buscar al final del documento)

The purpose of this guide is to set ready a system comprised by Apple silicon processors (by the time of this writing the M1 & M2 SoCs) in order to use q-emu ARM architecture emulator to compile, test & debbug ARM assembler projects.

The guide will instruct how to tune and install the toolchain rquired to compile and emulate programs on OS's running ARM processors. 


It can be used in two ways:
- Directly on macOS
- Bye means of virtualized VM running UBUNTU for ARM64 arch


First case, native macOS:
---------------------------
 - install gcc-arm-none-eabi

download package gcc-arm-none-eabi-10.3-2021.10-mac.pkg (or the most recent version available) from:
https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads.

it'll be installed more probably in: 

/Applications/ARM/bin/

but could be in another location as well, in any case it is very important to note down this directory because it must be added to PATH environment var in .Xshrc (bash, zsh, or whatever shell you want to use).

In case you already have homebrew installed for x86 arch Mac (probably migrating from an intel mac to an M1 mac i.e.) in your Apple silicon mac, remove homebrew following instructions in homebrew's site.

Then reinstal homebrew (also following install instructions from homebrew's site), and probably you have to install again some other packages with homebrew (like python, etc.)

 - install qemu with homebrew 

$ arch --arm64e brew install qemu

Additionally, in order to be able to use debug interface via TCP port, you may need to install telnet for macOS at your own risk (which is not installed by default for obvious security reasons):

$ arch --arm64e brew install telnet

- Now in order to be able to use "readelf" in macOS, use "dwarfdump" instead in your Makefiles, probably you'll need to "brew" it:

$ arch --arm64e brew install binutils

- Next step is to setup a debugging environment with gdb-like interface, on Apple silicon Macs we'll use lldb insted, so first things first, lets port-install lldb: 

$ sudo port install lldb-8.0

once lldb is installed we can attach to a qemu-arm-system proccess running our ARM programms similar way gdb does, so in one terminal start qemu using -s and -S switches in order to begin emulation stopped and for listen on TCP port 1234 for incomming lldb connection:

$ qemu-arm-system <all your config stuff here> -s -S

Then in another terminal fire up lldb:

$ lldb
(lldb) gdb-remote <ip-address>:<port>

where <ip-address> in our case will be "localhost" and <port> 1234

now you can use command-line-like gdb standard interface or use curses-based-built-in GUI for lldb

(lldb) gui

help for this GUI is self explanatory
keep reading next section to test for successful install

Second case, VM with UBUNTU 22.04-LTS aArch64
---------------------------------------------

As a test case it was installed using UTM (Universal Turing Machine: https://mac.getutm.app) virtualizing on Apple M2 silicon. 

Once ubuntu VM is ready, install then qemu and gcc-arm-none-eabi packages:

$ sudo apt install qemu

$ sudo apt install gcc-arm-none-eabi

And pretty much that's it, now you can start qemu in either case, to test if it works run:

$ quemu-system-arm --help

and you'll get a plethora of help text!


-------------------------------------------------------------------------------------

SPANISH VERSION:

El propósito de esta guía es preparar un sistema compuesto por procesadores "Apple Silicon" (al momento de escribir esto, los SoC M1 y M2) para usar el emulador de arquitectura q-emu ARM para compilar, probar y depurar proyectos de ensamblador ARM.

La guía le indicará cómo preparar e instalar la cadena de herramientas necesaria para compilar y emular programas en sistemas operativos que ejecutan procesadores ARM.

se puede utilizar de dos formas:
- Directamente sobre el macOS
- en una VM virtualizada correindo UBUNTU para ARM64

Primer caso, macOS nativo:
---------------------------
 - primero instalar gcc-arm-none-eabi

descargar el paquete : gcc-arm-none-eabi-10.3-2021.10-mac.pkg (o la versión más receinte) desde:
https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads.

esto se instala por lo general en: 

/Applications/ARM/bin/

pero puede llegar a ser en otra ubicación, es importante averiguar esto, porque hay que agregar al PATH del .Xshrc (bash, zsh, o el shell que se quiera usar) este directorio

en caso de tener instalado homebrew para intel en mac con M1, remover el homebrew con las instrucciones de la pagina de homebrew

luego volverlo a instalar (correctamentebásicamente con las instrucciones de la pagina de homebrew)

puede que haya que volver a instalar con homebrew algunos paquetes (python, etc), hacerlo

 - instalar qemu con homebrew: 

$ brew install qemu

Además, para poder usar la interfaz de depuración a través de un puerto TCP, es posible que deba instalar telnet para macOS bajo su propio riesgo (que no está instalado de forma predeterminada por razones obvias de seguridad):

$ brew install telnet

- Ahora para poder utilizar "readelf" en macOS, en su lugar use "dwarfdump" dentro de los Makefiles, probablemente haya que instalarlo con "brew":

$ arch --arm64e brew install binutils

- El siguiente paso es configurar un entorno de depuración con una interfaz similar a gdb, en las Mac con Apple silicon usaremos lldb, así que lo primero es lo primero, instalemos lldb usando Mac-ports:

$ sudo port install lldb-8.0

una vez que lldb está instalado, podemos conectarnos a un proceso qemu-arm-system ejecutando nuestros programas ARM de manera similar a gdb, por lo que en una terminal, inicie qemu usando las opciones -s y -S para comenzar la emulación detenida y para escuchar en el puerto TCP 1234 la conexión lldb entrante:

$ qemu-arm-system <tus opciones de inicio> -s -S

Luego, en otra terminal, inicie lldb:

$ lldb
(lldb) gdb-remote <dirección-ip>:<puerto>

donde <dirección-ip> en nuestro caso será "localhost" y <puerto> 1234

ahora puede usar la interfaz estándar gdb similar a la línea de comandos o usar la GUI incorporada basada en curses para lldb

(lldb) gui

la ayuda para esta GUI se explica por sí misma

siga leyendo la siguiente sección para probar que todo se haya instalado correctamente.


Segundo caso, VM con UBUNTU 22.04-LTS aArch64
---------------------------------------------

Como ejemplo se instalo el Ubuntu en UTM (Universal Turing Machine: https://mac.getutm.app) virtualizando sobre procesador Apple M2

Una vez instalado el ububtu, instalar luego los paquetes de qemu y gcc-arm-none-eabi:

$ sudo apt install qemu

$ sudo apt install gcc-arm-none-eabi

y básicamente eso seria todo, para probar que se instaló correctamente, en cualquiera de los dos casos, ejecutar:

$ qemu-system-arm --help

y deberá aparecer un extenso texto de ayuda con la sytanxis completa.
