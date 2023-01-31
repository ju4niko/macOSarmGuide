# QEMU toolchain for MAC survival kit (Apple silicon Mx)

se puede utilizar de dos formas:
- Directamente sobre el macOS
- en una VM virtualizada correindo UBUNTU para ARM64


Primer caso, macOS nativo:
---------------------------
 - primero instalar gcc-arm-none-eabi

descargar el paquete : gcc-arm-none-eabi-10.3-2021.10-mac.pkg (o loa version mas receinte) desde:
https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads.

esto se instala por lo general en 

/Applications/ARM/bin/

pero puede llegar a ser en otra ubicacion, es importante averiguar esto, porque hay que agregar al PATH del .Xshrc (bash, zsh, o el shell que se quiera usar) este directorio

en caso de tener instalado homebrew para intel en mac con M1, remover el homebrew con las instrucciones de la pagina de homebrew

luego volverlo a instalar (tambien con las instrucciones de la pagina de homebrew)

puede que haya que volver a instalar con homebrew algunos paquetes (python, etc), hacerlo

 - intalar qemu con homebrew: 

$ brew install qemu


Segundo caso, VM con UBUNTU 22.04-LTS aArch64
---------------------------------------------

Como ejemplo se instalo el Ubuntu en UTM (Universal Turing Machine: https://mac.getutm.app) virtualizando sobre procesador Apple M2

Una vez instalado el ububtu, instalar luego los paquetes de qemu y gcc-arm-none-eabi:

$ sudo apt install qemu
$ sudo apt install gcc-arm-none-eabi



