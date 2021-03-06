---
title: "Preparar uma Máquina Virtual do Oracle Linux para o Azure | Microsoft Docs"
description: "Configuração passo a passo de uma máquina virtual do Oracle que executa o Linux no Microsoft Azure."
services: virtual-machines-linux
author: bbenz
manager: timlt
documentationcenter: virtual-machines
tags: azure-service-management,azure-resource-manager
ms.assetid: a9b1ef89-90f7-4f9e-9de9-99e19db8e174
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/22/2015
ms.author: bbenz
translationtype: Human Translation
ms.sourcegitcommit: 63cf1a5476a205da2f804fb2f408f4d35860835f
ms.openlocfilehash: 2f575d6d3bb0e7ea9518d2c0f568ade79bb7ff36


---
# <a name="prepare-an-oracle-linux-virtual-machine-for-azure"></a>Preparar uma máquina virtual Oracle Linux para o Azure
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

* [Preparar uma máquina virtual do Oracle Linux 6.4 e versões posteriores para o Azure](virtual-machines-linux-oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Preparar uma máquina virtual do Oracle Linux 7.0 e versões posteriores para o Azure](virtual-machines-linux-oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="prerequisites"></a>Pré-requisitos
Este artigo pressupõe que você já instalou um sistema operacional Oracle Linux em um disco rígido virtual. Existem várias ferramentas para criar arquivos .vhd, por exemplo, uma solução de virtualização como o Hyper-V. Para obter instruções, veja [Instalar o Hyper-V e criar uma máquina virtual](http://technet.microsoft.com/library/hh846766.aspx).

**Notas de instalação do Oracle Linux**

* O kernel da Oracle compatível com Red Hat e seu UEK3 (Unbreakable Enterprise Kernel) têm suporte no Hyper-V e no Azure. Para obter os melhores resultados, atualize para o kernel mais recente durante a preparação do VHD do Oracle Linux.
* O UEK2 da Oracle não é compatível com o Hyper-V nem com o Azure, pois não contém os drivers necessários.
* Não há suporte para o formato VHDX mais recente no Azure. Você pode converter o disco em formato VHD usando o Gerenciador do Hyper-V ou o cmdlet convert-vhd.
* Ao instalar o sistema Linux, é recomendável utilizar partições padrão em vez de LVM (geralmente o padrão para muitas instalações). Isso irá evitar conflitos de nome LVM com VMs clonadas, especialmente se um disco do sistema operacional precisar ser anexado a outra VM para solução de problemas. Se você preferir, é possível usar LVM ou [RAID](virtual-machines-linux-configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) em discos de dados.
* Não há suporte para NUMA para tamanhos de máquinas virtuais maiores devido a um bug nas versões do kernel Linux abaixo de 2.6.37. Esse problema afeta principalmente distribuições que usam o kernel upstream do Red Hat 2.6.32. A instalação manual do agente Linux do Azure (waagent) desabilita a NUMA automaticamente na configuração do GRUB para o kernel do Linux. Verifique as etapas a seguir para obter mais informações a esse respeito.
* Não configure uma partição de permuta no disco do SO. O agente Linux pode ser configurado para criar um arquivo de permuta no disco de recursos temporários. Verifique as etapas a seguir para obter mais informações a esse respeito.
* Todos os VHDs devem ter tamanhos que sejam múltiplos de 1 MB.
* Certifique-se de que o repositório `Addons` está habilitado. Escolha editar o arquivo `/etc/yum.repo.d/public-yum-ol6.repo`(Oracle Linux 6) ou `/etc/yum.repo.d/public-yum-ol7.repo`(Oracle Linux) e altere a linha `enabled=0` para `enabled=1` em **[ol6_addons]** ou **[ol7_addons]** nesse arquivo.

## <a name="oracle-linux-64"></a>Oracle Linux 6.4+
Você deve concluir as etapas de configuração específicas do sistema operacional para que a máquina virtual seja executada no Azure.

1. No painel central do Gerenciador do Hyper-V, selecione a máquina virtual.
2. Clique em **Conectar** para abrir a janela da máquina virtual.
3. Desinstale o NetworkManager executando o seguinte comando:
   
        # sudo rpm -e --nodeps NetworkManager
   
   > [!NOTE]
   > Se o pacote ainda não foi instalado, esse comando falhará com uma mensagem de erro. Isso é esperado.
   > 
   > 
4. Crie um arquivo chamado **network** no diretório /etc/sysconfig/ que contém o seguinte texto:
   
    `NETWORKING=yes`  
    `HOSTNAME=localhost.localdomain`
5. Crie um arquivo chamado **ifcfg-eth0** no diretório /etc/sysconfig/network-scripts/ que contém o seguinte texto:
   
       DEVICE=eth0
       ONBOOT=yes
       BOOTPROTO=dhcp
           TYPE=Ethernet
           USERCTL=no
           PEERDNS=yes
       IPV6INIT=no
6. Mova (ou remova) as regras de udev para evitar a geração de regras estáticas da interface Ethernet. Essas regras provocam problemas ao clonar uma máquina virtual no Azure ou no Hyper-V:
   
       # sudo mkdir -m 0700 /var/lib/waagent
       # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/ 2\>/dev/null
       # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/ 2\>/dev/null
7. Certifique-se de que o serviço de rede será iniciado durante a inicialização executando o seguinte comando:
   
       # chkconfig network on
8. Instale o python-pyasn1 executando o seguinte comando:
   
       # sudo yum install python-pyasn1
9. Modifique a linha de inicialização do kernel em sua configuração de grub para incluir parâmetros adicionais de kernel para o Azure. Para fazer isso, abra "/boot/grub/menu.lst" em um editor de texto e verifique se o kernel padrão inclui os seguintes parâmetros:
   
       console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off
   
   Isso garantirá que todas as mensagens do console sejam enviadas para a primeira porta serial, o que pode auxiliar o suporte do Azure com problemas de depuração. Essa ação desabilita a NUMA devido a um bug no kernel da Oracle compatível com o Red Hat.
   
   Além dos itens acima, recomendamos *remover* os seguintes parâmetros:
   
       rhgb quiet crashkernel=auto
   
   As inicializações gráfica e silenciosa não são úteis em ambientes de rede, quando queremos que todos os logs sejam enviados para a porta serial.
   
   A opção `crashkernel` pode ser deixada configurada se desejado; porém, é importante lembrar que esse parâmetro reduzirá a quantidade de memória disponível na VM em 128 MB ou mais. Isso pode ser problemático em tamanhos de VM menores.
10. Confira se o servidor SSH está instalado e configurado para iniciar no tempo de inicialização. Geralmente, esse é o padrão.
11. Instale o Agente Linux do Azure executando o seguinte comando:
    
    # <a name="sudo-yum-install-walinuxagent"></a>sudo yum install WALinuxAgent
    Observe que a instalação do pacote WALinuxAgent removerá o NetworkManager e os pacotes NetworkManager-gnome se eles já não tiverem sido removidos conforme descrito na etapa 2.
12. Não crie espaço de permuta no disco do SO.
    
    O Agente Linux do Azure pode configurar automaticamente o espaço de troca usando o disco de recurso local que é anexado à VM após o provisionamento no Azure. É importante lembrar que o disco de recurso local é um disco *temporário* e que pode ser esvaziado quando a VM for desprovisionada. Depois de instalar o Agente Linux do Azure (consulte a etapa anterior), modifique os seguintes parâmetros em /etc/waagent.conf de maneira apropriada:
    
       ResourceDisk.Format=y
    
       ResourceDisk.Filesystem=ext4
    
       ResourceDisk.MountPoint=/mnt/resource
    
       ResourceDisk.EnableSwap=y
    
       ResourceDisk.SwapSizeMB=2048 ## OBSERVAÇÃO: ajuste-o conforme necessário.
13. Execute os comandos a seguir para desprovisionar a máquina virtual e prepará-la para provisionamento no Azure:
    
    # <a name="sudo-waagent--force--deprovision"></a>sudo waagent -force -deprovision
    # <a name="export-histsize0"></a>export HISTSIZE=0
    # <a name="logout"></a>logout
14. Clique em **Ação –\> Desligar** no Gerenciador do Hyper-V. Agora, seu VHD Linux está pronto para ser carregado no Azure.

## <a name="oracle-linux-70"></a>Oracle Linux 7.0 ou posterior
**Alterações no Oracle Linux 7**

Preparar uma máquina virtual Oracle Linux 7 para o Azure é muito semelhante ao processo para Oracle Linux 6. No entanto, há várias diferenças importantes que vale a pena observar:

* O kernel compatível com Red Hat e o UEK3 da Oracle são compatíveis com o Azure. Recomendamos o kernel UEK3.
* O pacote do NetworkManager não entra mais em conflito com o agente Linux do Azure. Esse pacote é instalado por padrão e recomendamos que você não o remova.
* O GRUB2 agora é usado como carregador de inicialização padrão. Com isso, o procedimento de edição de parâmetros do kernel mudou (confira abaixo).
* O XFS agora é o sistema de arquivos padrão. Ainda é possível usar o sistema de arquivos ext4 se você preferir.

**Etapas da configuração**

1. No Gerenciador do Hyper-V, selecione a máquina virtual.
2. Clique em **Conectar** para abrir a janela do console para a máquina virtual.
3. Crie um arquivo chamado **network** no diretório /etc/sysconfig/ que contém o seguinte texto:
   
       NETWORKING=yes
       HOSTNAME=localhost.localdomain
4. Crie um arquivo chamado **ifcfg-eth0** no diretório /etc/sysconfig/network-scripts/ que contém o seguinte texto:
   
       DEVICE=eth0
       ONBOOT=yes
       BOOTPROTO=dhcp
       TYPE=Ethernet
       USERCTL=no
           PEERDNS=yes
       IPV6INIT=no
5. Mova (ou remova) as regras de udev para evitar a geração de regras estáticas da interface Ethernet. Essas regras provocam problemas ao clonar uma máquina virtual no Microsoft Azure ou no Hyper-V.
   
       # sudo mkdir -m 0700 /var/lib/waagent
       # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/ 2>/dev/null
       # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/ 2>/dev/null
6. Certifique-se de que o serviço de rede será iniciado durante a inicialização executando o seguinte comando:
   
       # sudo chkconfig network on
7. Execute o comando a seguir para instalar o pacote python-pyasn1:
   
       # sudo yum install python-pyasn1
8. Execute o comando a seguir para limpar os metadados atuais do yum e instalar atualizações:
   
       # sudo yum clean all
       # sudo yum -y update
9. Modifique a linha de inicialização do kernel em sua configuração de grub para incluir parâmetros adicionais de kernel para o Azure. Para fazer isso, abra "/etc/default/grub" em um editor de texto e edite o parâmetro GRUB\_CMDLINE\_LINUX, por exemplo:
   
       GRUB\_CMDLINE\_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0"
   
   Isso garantirá que todas as mensagens do console sejam enviadas para a primeira porta serial, que pode auxiliar o suporte do Azure com problemas de depuração. Além dos itens acima, recomendamos *remover* os seguintes parâmetros:
   
       rhgb quiet crashkernel=auto
   
   As inicializações gráfica e silenciosa não são úteis em ambientes de rede, quando queremos que todos os logs sejam enviados para a porta serial.
   
   A opção `crashkernel` pode ser deixada configurada se desejado; porém, é importante lembrar que esse parâmetro reduzirá a quantidade de memória disponível na VM em 128 MB ou mais. Isso pode ser problemático em tamanhos de VM menores.
10. Depois de editar "/etc/default/grub", execute o comando a seguir para recompilar a configuração do grub:
    
    # <a name="sudo-grub2-mkconfig--o-bootgrub2grubcfg"></a>sudo grub2-mkconfig -o /boot/grub2/grub.cfg
11. Confira se o servidor SSH está instalado e configurado para iniciar no tempo de inicialização. Geralmente, esse é o padrão.
12. Instale o Agente Linux do Azure executando o seguinte comando:
    
    # <a name="sudo-yum-install-walinuxagent"></a>sudo yum install WALinuxAgent
13. Não crie espaço de permuta no disco do SO.
    
    O Agente Linux do Azure pode configurar automaticamente o espaço de troca usando o disco de recurso local que é anexado à VM após o provisionamento no Azure. É importante lembrar que o disco de recurso local é um disco *temporário* e que pode ser esvaziado quando a VM for desprovisionada. Depois de instalar o Agente Linux do Azure (consulte a etapa anterior), modifique os seguintes parâmetros em /etc/waagent.conf de maneira apropriada:
    
       ResourceDisk.Format=y    ResourceDisk.Filesystem=ext4    ResourceDisk.MountPoint=/mnt/resource    ResourceDisk.EnableSwap=y    ResourceDisk.SwapSizeMB=2048 ## OBSERVAÇÃO: defina isso para o que quer que você precise que seja.
14. Execute os comandos a seguir para desprovisionar a máquina virtual e prepará-la para provisionamento no Azure:
    
    # <a name="sudo-waagent--force--deprovision"></a>sudo waagent -force -deprovision
    # <a name="export-histsize0"></a>export HISTSIZE=0
    # <a name="logout"></a>logout
15. Clique em **Ação –\> Desligar** no Gerenciador do Hyper-V. Agora, seu VHD Linux está pronto para ser carregado no Azure.




<!--HONumber=Nov16_HO3-->


