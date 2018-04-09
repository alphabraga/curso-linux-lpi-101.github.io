---
layout: page
title: 101 Arquiterura do Sistema
permalink: /101/arquitetura-do-sistema
---

## 101.1 Visualize e configure definições de hardware (Peso 2)

Ter habilidade de visualizar e configurar sistemas de hardware fundamentais. Isso pode ser feito de duas formas:

* Com comandos espeficicos
* Vendo arquivos no sistema de arquivos especial

Feramentas e utilitários para listar diversas de informações de hardware (lsusb, lspci, etc.)
Feramentas e utilitários para Manipular dispositivos USB
Entendimento conceitual de [sysfs](#), [udev](#), [dbus](#)

A seguir uma lista de arquivos, utilitários e termos:

### /sys/

Faz parte da partição virtual pois não guarda de fato arquivos e sim informações dinamicas.Esse é um diretorio que é mais focado em **guardar informações de dispositivos de hardware**.

o `/sys/` é montado utilizando um tipo de file system chamado de`sysfs`. Veja a baixo alguns arquivos desse diretorio:

<pre class="command-line language-bash" data-user="alphabraga" data-host="localhost">
<code>df -t sysfs -a
Sist.fichs     1K-blocos  Ocup Livres Uso% Montado em
sysfs                  0     0      0    - /sys
</code>
</pre>



Veja abaixo o conteudo do diretorio:

    ├── block
    │   ├── loop0 -> ../devices/virtual/block/loop0
    │   ├── loop1 -> ../devices/virtual/block/loop1
    │   ├── loop2 -> ../devices/virtual/block/loop2
    │   ├── loop3 -> ../devices/virtual/block/loop3
    │   ├── loop4 -> ../devices/virtual/block/loop4
    │   ├── loop5 -> ../devices/virtual/block/loop5
    │   ├── loop6 -> ../devices/virtual/block/loop6
    │   ├── loop7 -> ../devices/virtual/block/loop7
    │   └── sda -> ../devices/pci0000:00/0000:00:1f.2/ata1/host0/target0:0:0/0:0:0:0/block/sda
    ├── bus
    │   ├── acpi
    │   ├── clockevents
    │   ├── clocksource
    │   ├── container
    │   ├── cpu
    │   ├── edac
    │   ├── event_source
    │   ├── gpio
    │   ├── hdaudio



#### /proc/

Guarda informações sobre **processos ativos no sistema e recursos de hardware**. Nesse diretorio ficam arquivos que são cobrados na LPI como o [/proc/interrups](#), [/proc/dma](#) e o [/proc/ioports](#). 


    ├── acpi
    ├── asound
    ├── buddyinfo
    ├── bus
    ├── cgroups
    ├── cmdline
    ├── consoles
    ├── cpuinfo
    ├── crypto
    ├── devices
    ├── diskstats
    ├── dma
    ├── driver
    ├── execdomains
    ├── fb
    ├── filesystems
    ├── fs
    ├── interrupts
    ├── iomem
    ├── ioports
    ├── irq
    ├── kallsyms
    ├── kcore
    ├── keys
    ├── key-users
    ├── kmsg


É importante destacar os arquivos `/proc/ioports` e o `/proc/interrupts`

#### /proc/ioports

Esse arquivo contem informações sobre endereços de memória utilizados pela CPU para se comunicar com dispositivos de hardware.


    0000-0000 : PCI Bus 0000:00
    0000-0000 : dma1
    0000-0000 : pic1
    0000-0000 : timer0
    0000-0000 : timer1
    0000-0000 : keyboard
    0000-0000 : PNP0C09:00
    0000-0000 : EC data
    0000-0000 : keyboard
    0000-0000 : PNP0C09:00
    0000-0000 : EC cmd
    0000-0000 : rtc0
    0000-0000 : dma page reg
    0000-0000 : pic2
    0000-0000 : dma2
    0000-0000 : fpu
    0000-0000 : pnp 00:00
    0000-0000 : pnp 00:07
    0000-0000 : PCI conf1
    0000-0000 : PCI Bus 0000:00
    0000-0000 : pnp 00:00
    0000-0000 : pnp 00:00
    0000-0000 : ACPI PM1a_EVT_BLK
    0000-0000 : ACPI PM1a_CNT_BLK
    0000-0000 : ACPI PM_TMR
    0000-0000 : iTCO_wdt.0.auto



#### /proc/interrupts

IRQ, ou Interrups Requests, são canais de comunicação que são utilizados pelos dispositivos para enviar comandos para a CPU. Esses comandos ou sinais são enviados a CPU e ela tem que parar de fazer o que estiver fazendo a atender a essas requisições. Os canais de número menor tem maior prioridade que os demais.


Veja a tabela abaixo com informações básicas sobre o IRQ:

     IRQ 0  - Timmer do sistema (relógio)
     IRQ 1  - Teclado
     IRQ 2  - Cascateador de IRQ
     IRQ 3  - Portal Serial 2
     IRQ 4  - Portal Serial 1
     IRQ 5  - Livre
     IRQ 6  - Drive de Disquetes
     IRQ 7  - Porta Paralela
     IRQ 8  - Relogio da CMOS
     IRQ 9  - Placa de Video
     IRQ 10 - Livre
     IRQ 11 - Controlador USB
     IRQ 12 - Porta PS/2
     IRQ 13 - Coprocessador Aritmético
     IRQ 14 - IDE Primária
     IRQ 15 - IDE Secundária

Executando o comando `cat /proc/interrups` temos o output abaixo:

             CPU0       CPU1       CPU2       CPU3       
      0:         16          0          0          0  IR-IO-APIC   2-edge      timer
      1:          2       1543      15908       1133  IR-IO-APIC   1-edge      i8042
      7:          0          0          0          0  IR-IO-APIC   7-fasteoi   INT3432:00, INT3433:00
      8:          1          0          0          0  IR-IO-APIC   8-edge      rtc0
      9:        104        115       1280         83  IR-IO-APIC   9-fasteoi   acpi
     12:         18       8604     177407      10087  IR-IO-APIC  12-edge      i8042
     40:          0          0          0          0  DMAR-MSI   0-edge      dmar0
     41:          0          0          0          0  DMAR-MSI   1-edge      dmar1
     42:        147        280      23940        358  IR-PCI-MSI 327680-edge      xhci_hcd
     43:       8302       2062      59673       2578  IR-PCI-MSI 512000-edge      ahci[0000:00:1f.2]
     44:          0          0          0          0  IR-PCI-MSI 1048576-edge      enp2s0
     45:        542         79     128084     144606  IR-PCI-MSI 32768-edge      i915
     46:          9          5          0          0  IR-PCI-MSI 360448-edge      mei_me
     47:        195      62775        136         33  IR-PCI-MSI 524288-edge      ath10k_pci
     48:        333        241         10         67  IR-PCI-MSI 442368-edge      snd_hda_intel:card1
     49:         60         66         55          2  IR-PCI-MSI 49152-edge      snd_hda_intel:card0
    NMI:         79         93         91         88   Non-maskable interrupts
    LOC:     220605     232216     218771     215687   Local timer interrupts
    SPU:          0          0          0          0   Spurious interrupts
    PMI:         79         93         91         88   Performance monitoring interrupts
    IWI:          0          1          1          0   IRQ work interrupts
    RTR:          1          0          0          0   APIC ICR read retries

Os números nas colunas `CPUn` exibe o numero de vezes que uma requisição é enviada para aquele canal. Pode fazer um teste e apos realizar a digitação de algum texto no teclado e verificar que o número de requisições aumnetou no canal `1`


<pre class="command-line language-bash" data-user="alphabraga" data-host="localhost">
<code>cat /proc/interrupts | egrep "^  1:"
  1:        135       2448      27391       2109  IR-IO-APIC   1-edge      i8042
</code>
</pre>


### /proc/dma

DMA significa Direct Memory Access, ou seja acesso direto a memória, esse arquivo contem endereços de memoria utilizado pelo hardware na qual não precisam do intermedio da CPU para acessa-los. Isso permite uma comunicação mais dinamica uma vez que um interédiario foi removido.

### /proc/meninfo

Exibe informações de memória

### /proc/procinfo

Exibe informações de processador

#### /dev/

Esse diretorio faz referencias a dispositivos do sistema inclusive de armazenamento. É o processo de nome `udev` que monta esses dispositivos que ficam exibidos nesse diretorio

### dbus

Tem duas funções:

* Realiza a comunicação entre processos
* Informa aos processos a situação de dispositivos de hardware



#### modprobe


#### lsmod

Lista os modulos que estão sendo utilizados pelo sistema. Existe uma coluna que informa se o modulo é utilizado por outros modulos o numero de utilizações..

#### lspci

Lista os dispositivos de hardware que estão no barramento PCI do computador.

    usuario@maquina:$ lspci

    00:00.0 Host bridge: Intel Corporation Broadwell-U Host Bridge -OPI (rev 09)
    00:02.0 VGA compatible controller: Intel Corporation Broadwell-U Integrated Graphics (rev 09)
    00:03.0 Audio device: Intel Corporation Broadwell-U Audio Controller (rev 09)
    00:04.0 Signal processing controller: Intel Corporation Broadwell-U Camarillo Device (rev 09)
    00:14.0 USB controller: Intel Corporation Wildcat Point-LP USB xHCI Controller (rev 03)
    00:16.0 Communication controller: Intel Corporation Wildcat Point-LP MEI Controller #1 (rev 03)
    00:1b.0 Audio device: Intel Corporation Wildcat Point-LP High Definition Audio Controller (rev 03)
    00:1c.0 PCI bridge: Intel Corporation Wildcat Point-LP PCI Express Root Port #3 (rev e3)
    00:1c.3 PCI bridge: Intel Corporation Wildcat Point-LP PCI Express Root Port #4 (rev e3)
    00:1f.0 ISA bridge: Intel Corporation Wildcat Point-LP LPC Controller (rev 03)
    00:1f.2 SATA controller: Intel Corporation Wildcat Point-LP SATA Controller [AHCI Mode] (rev 03)
    00:1f.3 SMBus: Intel Corporation Wildcat Point-LP SMBus Controller (rev 03)
    01:00.0 Network controller: Qualcomm Atheros QCA6174 802.11ac Wireless Network Adapter (rev 20)
    02:00.0 Ethernet controller: Realtek Semiconductor Co., Ltd. RTL8111/8168/8411 PCI Express Gigabit Ethernet Controller (rev 0c)

No lado esquerdo do outoput vemos os IDS dos dispositivos e do lado direito uma breve descrição sobre os mesmo. Para ver mais detalhes sobre todos os dispositivos utilizamos a opção `-v` ou `-vv` para ser mais verboso ainda.


    usuario@maquina:$ lspci -v


    00:00.0 Host bridge: Intel Corporation Broadwell-U Host Bridge -OPI (rev 09)
            Subsystem: Samsung Electronics Co Ltd Broadwell-U Host Bridge -OPI
            Flags: bus master, fast devsel, latency 0
            Capabilities: <access denied>
            Kernel driver in use: bdw_uncore

    00:02.0 VGA compatible controller: Intel Corporation Broadwell-U Integrated Graphics (rev 09) (prog-if 00 [VGA controller])
            DeviceName:  Onboard IGD
            Subsystem: Samsung Electronics Co Ltd Broadwell-U Integrated Graphics
            Flags: bus master, fast devsel, latency 0, IRQ 45
            Memory at f6000000 (64-bit, non-prefetchable) [size=16M]
            Memory at e0000000 (64-bit, prefetchable) [size=256M]
            I/O ports at f000 [size=64]
            [virtual] Expansion ROM at 000c0000 [disabled] [size=128K]
            Capabilities: <access denied>
            Kernel driver in use: i915
            Kernel modules: i915

    00:03.0 Audio device: Intel Corporation Broadwell-U Audio Controller (rev 09)
            Subsystem: Samsung Electronics Co Ltd Broadwell-U Audio Controller
            Flags: bus master, fast devsel, latency 0, IRQ 46
            Memory at f731c000 (64-bit, non-prefetchable) [size=16K]
            Capabilities: <access denied>
            Kernel driver in use: snd_hda_intel
            Kernel modules: snd_hda_intel

    00:04.0 Signal processing controller: Intel Corporation Broadwell-U Camarillo Device (rev 09)
            Subsystem: Samsung Electronics Co Ltd Broadwell-U Processor Thermal Subsystem
            Flags: fast devsel, IRQ 16
            Memory at f7310000 (64-bit, non-prefetchable) [size=32K]
            Capabilities: <access denied>
            Kernel driver in use: proc_thermal
            Kernel modules: processor_thermal_device


Para ver mais informações apenas de um dispositivo especifico utilizamos a opção `-s` seguido do ID do dispositivo(o `s` seria de seleção)

    usuario@maquina:~$ lspci -s00:04.0 -v 


        00:04.0 Signal processing controller: Intel Corporation Broadwell-U Camarillo Device (rev 09)
                Subsystem: Samsung Electronics Co Ltd Broadwell-U Processor Thermal Subsystem
                Flags: fast devsel, IRQ 16
                Memory at f7310000 (64-bit, non-prefetchable) [size=32K]
                Capabilities: <access denied>
                Kernel driver in use: proc_thermal
                Kernel modules: processor_thermal_device


#### lsusb

Lista os dispositivos de hardware que estão no barramento USB do computador. Semelhando ao comando `lspci` esse comando com a opção `-v` exibe uma saida mais verbosa e tambem com a opção `-s` exibe as informações apenas de um dispositivo ** passando o número de BUS e DEVICE**

Listando todos os dispositivos no barramento USB:

    alphabraga@sharp:~$ lsusb 

    Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
    Bus 001 Device 005: ID 0bda:0129 Realtek Semiconductor Corp. RTS5129 Card Reader Controller
    Bus 001 Device 004: ID 2232:1063 Silicon Motion 
    Bus 001 Device 003: ID 0cf3:e300 Atheros Communications, Inc. 
    Bus 001 Device 002: ID 1ea7:0064  
    Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub

Listando um dispositivo espefico, utilizando `-s bus:device`

    alphabraga@sharp:~$ lsusb -s001:002

    Bus 001 Device 002: ID 1ea7:0064

Listagem verbosa dos dispositivos:

    alphabraga@sharp:~$ lsusb -v

    Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
    Device Descriptor:
      bLength                18
      bDescriptorType         1
      bcdUSB               3.00
      bDeviceClass            9 Hub
      bDeviceSubClass         0 Unused
      bDeviceProtocol         3 
      bMaxPacketSize0         9
      idVendor           0x1d6b Linux Foundation
      idProduct          0x0003 3.0 root hub
      bcdDevice            4.13
      iManufacturer           3 
      iProduct                2 
      iSerial                 1 
      bNumConfigurations      1
      Configuration Descriptor:
        bLength                 9
        bDescriptorType         2
        wTotalLength           31
        bNumInterfaces          1
        bConfigurationValue     1
        iConfiguration          0 
        bmAttributes         0xe0
          Self Powered
          Remote Wakeup
        MaxPower                0mA
        Interface Descriptor:
    Couldn't open device, some information will be missing
          bLength                 9
          bDescriptorType         4
          bInterfaceNumber        0
          bAlternateSetting       0
          bNumEndpoints           1
          bInterfaceClass         9 Hub
    Couldn't open device, some information will be missing



## 101.2 Inicializar o Sistema (Peso 3)

Ter a capacidade de guiar o sistema durante o processo de boot.

Prover comandos comuns para o `boot loader` e opções para o Kernel no momento do `boot`
Demonstrar conhecimento da sequencia de `boot` da BIOS ate a finalização do boot
Conhecimento de `SysVinit` e `systemd`
Menssagens de `Awareness` da inicialização do sistema
Verificar eventos de `boot` nos arquivos de log 

Termos e Utilitários:

### [dmesg](/101.2-Inicializar-o-sistema#dmesg)

Esse comando exibe informações sobre o boot do sistema com se fosse um log

### [BIOS](/101.2-Inicializar-o-sistema#bios)

A palavra `BIOS` significa Basic Input Output System. E nada mais é que um firmware que guarda as configurações básicas da maquina como data e hora, ordem de dispositivos para o boot, configuraçõs basicas de teclado etc..

### [bootloader](/101.2-Inicializar-o-sistema#bootloader)

### [kernel](/101.2-Inicializar-o-sistema#kernel)

### [initramfs](/101.2-Inicializar-o-sistema#initframs)

### [init](/101.2-Inicializar-o-sistema#init)

É o sistema que configura o os serviços que rodam no Linux. Ele funciona utilizando o conceito de `runlevels`.

### [SysVinit](/101.2-Inicializar-o-sistema#SysVinit)


### [systemd](/101.2-Inicializar-o-sistema#systemd)


## 101.3 Mudar runlevels boot targets e desligar ou reiniciar o sistema (Peso 3)

Ter a capacidade de gerenciar `SysVinit` runlevel or systemd boot target of the system. This objective includes changing to single user mode, shutdown or rebooting the system. Candidatos devem ter a capacidade de  to alert users before switching runlevels / boot targets and properly terminate processes. This objective also includes setting the default SysVinit runlevel or systemd boot target. It also includes awareness of Upstart as an alternative to SysVinit or systemd.

Definir o `runlevel` padrão ou o `target` do `boot`

Mudar entre `runlevels` e `boot targets` incluindo o `single user mode`

Desligar e reiniciar a maquina pela linha de comando

Alertar usuários antes de mudar de `runlevels` ou `boot target` ou ate mesmo em outros eventos considerados importantes.

Finalizar processos de modo correto


Termos e Utilitários:

### [/etc/inittab](/101.3-mudar-runlevels-boot-targets-e-desligar-ou-reiniciar-o-sistema)

Esse arquivo contem informações sobre o runlevel padrão do sistema

### [shutdown](/101.3-mudar-runlevels-boot-targets-e-desligar-ou-reiniciar-o-sistema)

Esse comando realiza o desligamento da maquina:

    usuario@maquina:$~ shutdown now

O comando acima  desliga a maquina imediatamente

    usuario@maquina:$~ shutdown now -r

O parametro `-r` reinicia a maquina 

COLOCAR AQUI OUTRAS FORMAS DE DESLIGAR COM TEMPO ETC....

### [init](/101.3-mudar-runlevels-boot-targets-e-desligar-ou-reiniciar-o-sistema)

Esse comando realiza a alteração de run level

### [/etc/init.d/](/101.3-mudar-runlevels-boot-targets-e-desligar-ou-reiniciar-o-sistema)


### [telinit](/101.3-mudar-runlevels-boot-targets-e-desligar-ou-reiniciar-o-sistema)

Esse comando realiza a alteração de run level

### [systemd](/101.3-mudar-runlevels-boot-targets-e-desligar-ou-reiniciar-o-sistema)


### [systemctl](/101.3-mudar-runlevels-boot-targets-e-desligar-ou-reiniciar-o-sistema)


### [/etc/systemd/](/101.3-mudar-runlevels-boot-targets-e-desligar-ou-reiniciar-o-sistema)


### [/usr/lib/systemd/](/101.3-mudar-runlevels-boot-targets-e-desligar-ou-reiniciar-o-sistema)



### [wall](/101.3-mudar-runlevels-boot-targets-e-desligar-ou-reiniciar-o-sistema)

Esse comando realiza o envio de mensagem para todos os usuários logados no sistema