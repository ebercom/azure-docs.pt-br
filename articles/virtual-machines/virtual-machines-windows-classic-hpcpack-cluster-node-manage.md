---
title: "Gerenciar nós de computação do cluster HPC Pack | Microsoft Docs"
description: "Saiba mais sobre as ferramentas de script do PowerShell para adicionar, remover, iniciar e interromper nós de computação do cluster HPC Pack no Azure"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,hpc-pack
ms.assetid: 4193f03b-94e9-4704-a7ad-379abde063a9
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 07/22/2016
ms.author: danlep
translationtype: Human Translation
ms.sourcegitcommit: ee34a7ebd48879448e126c1c9c46c751e477c406
ms.openlocfilehash: 002fd2007758a268c205db95475ad7e691284b15


---
# <a name="manage-the-number-and-availability-of-compute-nodes-in-an-hpc-pack-cluster-in-azure"></a>Gerenciar o número e a disponibilidade de nós de computação em um cluster HPC Pack no Azure
Se você criou um cluster HPC Pack em VMs do Azure, pode ser conveniente encontrar maneiras de adicionar, remover, iniciar (provisionar) ou interromper (desprovisionar) facilmente várias VMs de nó de computação no cluster. Para executar essas tarefas, execute scripts do Azure PowerShell que estão instalados na VM de nó de cabeçalho. Esses scripts ajudam a controlar o número e a disponibilidade dos recursos do cluster HPC Pack para que você possa controlar os custos.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## <a name="prerequisites"></a>Pré-requisitos
* **Cluster HPC Pack em VMs do Azure** : crie um cluster HPC Pack no modelo de implantação clássica usando pelo menos o HPC Pack 2012 R2 Atualização 1. Por exemplo, é possível automatizar a implantação usando a imagem de VM do HPC Pack no Azure Marketplace e um script do Azure PowerShell. Para obter informações e pré-requisitos, veja [Criar um cluster HPC com o script de implantação de IaaS do HPC Pack](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
  
    Após a implantação, localize os scripts de gerenciamento do nó na pasta %CCP\_HOME%bin no nó de cabeçalho. É necessário executar cada um dos scripts como administrador.
* **Certificado de gerenciamento ou arquivo de configurações de publicação do Azure** - Você precisa fazer o seguinte no nó de cabeçalho:
  
  * **Importe o arquivo de configurações de publicação do Azure**. Para fazer isso, execute os seguintes cmdlets do Azure PowerShell no nó de cabeçalho:
    
    ```
    Get-AzurePublishSettingsFile
    
    Import-AzurePublishSettingsFile –PublishSettingsFile <publish settings file>
    ```
  * **Configure o certificado de gerenciamento do Azure no nó de cabeçalho**. Se você tiver o arquivo .cer, importe-o no repositório de certificados CurrentUser\My e, em seguida, execute o seguinte cmdlet do Azure PowerShell para seu ambiente do Azure (AzureCloud ou AzureChinaCloud):
    
    ```
    Set-AzureSubscription -SubscriptionName <Sub Name> -SubscriptionId <Sub ID> -Certificate (Get-Item Cert:\CurrentUser\My\<Cert Thrumbprint>) -Environment <AzureCloud | AzureChinaCloud>
    ```

## <a name="add-compute-node-vms"></a>Adicionar VMs de nó de computação
Adicionar nós de computação com o script **Add-HpcIaaSNode.ps1** .

### <a name="syntax"></a>Sintaxe
```
Add-HPCIaaSNode.ps1 [-ServiceName] <String> [-ImageName] <String>
 [-Quantity] <Int32> [-InstanceSize] <String> [-DomainUserName] <String> [[-DomainUserPassword] <String>]
 [[-NodeNameSeries] <String>] [<CommonParameters>]

```
### <a name="parameters"></a>Parâmetros
* **ServiceName** - O nome do serviço de nuvem ao qual novas VMs de nó de computação serão adicionadas.
* **ImageName**: o nome de imagem da VM do Azure, que pode ser obtido por meio do portal clássico do Azure ou do cmdlet **Get-AzureVMImage** do Azure PowerShell. A imagem deve atender aos seguintes requisitos:
  
  1. Um sistema operacional Windows deve ser instalado.
  2. O HPC Pack deve ser instalado na função de nó de computação.
  3. A imagem deve ser uma imagem privada na categoria Usuário, não uma imagem pública da VM do Azure.
* **Quantity** - Número de VMs de nó de computação a serem adicionadas.
* **InstanceSize** - Tamanho das VMs de nó de computação.
* **DomainUserName** - Nome de usuário de domínio que será usado para ingressar as novas VMs no domínio.
* **DomainUserPassword** : senha do usuário de domínio.
* **NodeNameSeries** (opcional): padrão de nomenclatura para os nós de computação. O formato deve ser &lt;*Raiz\_Nome*&gt;&lt;*Iniciar\_Número*&gt;%. Por exemplo, MyCN%10% significa uma série de nomes de nó de computação com início em MyCN11. Se não for especificado, o script usará a série de nomenclatura do nó configurado no cluster HPC.

### <a name="example"></a>Exemplo
O exemplo a seguir adiciona 20 VMs de nó de computação de tamanho Grande no serviço de nuvem *hpcservice1*, com base na imagem de VM *hpccnimage1*.

```
Add-HPCIaaSNode.ps1 –ServiceName hpcservice1 –ImageName hpccniamge1
–Quantity 20 –InstanceSize Large –DomainUserName <username>
-DomainUserPassword <password>
```


## <a name="remove-compute-node-vms"></a>Remover VMs de nó de computação
Remova os nós de computação com o script **Remove-HpcIaaSNode.ps1** .

### <a name="syntax"></a>Sintaxe
```
Remove-HPCIaaSNode.ps1 -Name <String[]> [-DeleteVHD] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]

Remove-HPCIaaSNode.ps1 -Node <Object> [-DeleteVHD] [-Force] [-Confirm] [<CommonParameters>]
```

### <a name="parameters"></a>Parâmetros
* **Name** - Nomes dos nós do cluster a serem removidos. Há suporte para caracteres curinga. O nome do conjunto de parâmetros é Nome. Não é possível especificar os dois parâmetros **Name** e **Node**.
* **Node** : o objeto HpcNode para os nós a serem removidos, que pode ser obtido por meio do cmdlet [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx)do HPC PowerShell. O nome do conjunto de parâmetros é Nó. Não é possível especificar os dois parâmetros **Name** e **Node**.
* **DeleteVHD** (opcional) - Configuração usada para excluir os discos associados para as VMs que foram removidas.
* **Force** (opcional) - Configuração usada para forçar os nós HPC a ficarem offline antes de removê-los.
* **Confirm** (opcional) - Solicitação de confirmação antes de executar o comando.
* **WhatIf** - Configuração usada para descrever o que aconteceria se você executasse o comando sem realmente executar o comando.

### <a name="example"></a>Exemplo
O exemplo a seguir força os nós a ficarem offline com nomes começando com *HPCNode-CN-* e, em seguida, remove os nós e seus discos associados.

```
Remove-HPCIaaSNode.ps1 –Name HPCNodeCN-* –DeleteVHD -Force
```

## <a name="start-compute-node-vms"></a>Iniciar VMs de nó de computação
Inicie os nós de computação com o script **Start-HpcIaaSNode.ps1** .

### <a name="syntax"></a>Sintaxe
```
Start-HPCIaaSNode.ps1 -Name <String[]> [<CommonParameters>]

Start-HPCIaaSNode.ps1 -Node <Object> [<CommonParameters>]
```
### <a name="parameters"></a>Parâmetros
* **Name** - Nomes dos nós do cluster a serem iniciados. Há suporte para caracteres curinga. O nome do conjunto de parâmetros é Nome. Não é possível especificar os dois parâmetros **Name** e **Node**.
* **Node**- O objeto HpcNode para os nós a serem iniciados, que pode ser obtido por meio do cmdlet [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx)do HPC PowerShell. O nome do conjunto de parâmetros é Nó. Não é possível especificar os dois parâmetros **Name** e **Node**.

### <a name="example"></a>Exemplo
O exemplo a seguir inicia os nós com nomes começando com *HPCNode-CN-*.

```
Start-HPCIaaSNode.ps1 –Name HPCNodeCN-*
```

## <a name="stop-compute-node-vms"></a>Interromper VMs de nó de computação
Interrompa os nós de computação com o script **Stop-HpcIaaSNode.ps1** .

### <a name="syntax"></a>Sintaxe
```
Stop-HPCIaaSNode.ps1 -Name <String[]> [-Force] [<CommonParameters>]

Stop-HPCIaaSNode.ps1 -Node <Object> [-Force] [<CommonParameters>]
```

### <a name="parameters"></a>Parâmetros
* **Name**- Nomes dos nós do cluster a serem interrompidos. Há suporte para caracteres curinga. O nome do conjunto de parâmetros é Nome. Não é possível especificar os dois parâmetros **Name** e **Node**.
* **Node** : o objeto HpcNode para os nós a serem interrompidos, que pode ser obtido por meio do cmdlet [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx)do HPC PowerShell. O nome do conjunto de parâmetros é Nó. Não é possível especificar os dois parâmetros **Name** e **Node**.
* **Force** (opcional) - Configuração usada para forçar os nós HPC a ficarem offline antes de interrompê-los.

### <a name="example"></a>Exemplo
O exemplo a seguir força os nós a ficarem offline com nomes começando com *HPCNode-CN-* e, em seguida, interrompe os nós.

Stop-HPCIaaSNode.ps1 –Name HPCNodeCN-* -Force

## <a name="next-steps"></a>Próximas etapas
* Caso deseje usar uma maneira de aumentar ou reduzir automaticamente os nós de cluster de acordo com a atual carga de trabalho de trabalhos e de tarefas no cluster, consulte [Aumentar e reduzir automaticamente os recursos do cluster do HPC Pack no Azure conforme a carga de trabalho do cluster](virtual-machines-windows-classic-hpcpack-cluster-node-autogrowshrink.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).




<!--HONumber=Nov16_HO3-->


