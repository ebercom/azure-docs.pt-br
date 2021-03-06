---
title: "Criar um Conjunto de Dimensionamento de Máquina Virtual usando o PowerShell | Microsoft Docs"
description: "Criar um Conjunto de Escala de Máquina Virtual usando o PowerShell"
services: virtual-machine-scale-sets
documentationcenter: 
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 7bb03323-8bcc-4ee4-9a3e-144ca6d644e2
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/18/2016
ms.author: davidmu
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 6d70338ebf918a3f9178a4f633dd46a607d72b1c


---
# <a name="create-a-windows-virtual-machine-scale-set-using-azure-powershell"></a>Criar um conjunto de escala de máquina virtual do Windows usando o Azure PowerShell
Estas etapas seguem uma abordagem de preenchimento de lacunas para criar um conjunto de escala de máquina virtual do Azure. Consulte [Conjuntos de Dimensionamento de Máquina Virtual - Visão Geral](virtual-machine-scale-sets-overview.md) para saber mais sobre conjuntos de escala.

Deve levar cerca de 30 minutos para executar as etapas neste artigo.

## <a name="step-1-install-azure-powershell"></a>Etapa 1: instalar o PowerShell do Azure
Confira [Como instalar e configurar o Azure PowerShell](../powershell-install-configure.md) para saber mais sobre como instalar a versão mais recente do Azure PowerShell, selecionar a assinatura e entrar em sua conta.

## <a name="step-2-create-resources"></a>Etapa 2: criar recursos
Crie os recursos que são necessários para o novo conjunto de escala.

### <a name="resource-group"></a>Grupo de recursos
Um conjunto de escala de máquina virtual deve estar contido em um grupo de recursos.

1. Obtenha uma lista dos locais disponíveis nos quais os recursos podem ser posicionados:
   
        Get-AzureLocation | Sort Name | Select Name
2. Escolha o local que funciona melhor para você, substitua o valor de **$locName** pelo nome do local e crie a variável:
   
        $locName = "location name from the list, such as Central US"
3. Substitua o valor de **$rgName** pelo nome que você deseja usar para o novo grupo de recursos e crie a variável: 
   
        $rgName = "resource group name"
4. Crie o grupo de recursos:
   
        New-AzureRmResourceGroup -Name $rgName -Location $locName
   
    Você deverá ver algo como este exemplo:
   
        ResourceGroupName : myrg1
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/########-####-####-####-############/resourceGroups/myrg1

### <a name="storage-account"></a>Conta de armazenamento
Uma conta de armazenamento é usada por uma máquina virtual para armazenar o disco do sistema operacional e os dados de diagnóstico utilizados para o dimensionamento. Quando possível, é recomendável ter uma conta de armazenamento para cada máquina virtual criada em um conjunto de dimensionamento. Se não for possível, planeje para não mais de 20 VMs por conta de armazenamento. O exemplo neste artigo mostra três contas de armazenamento sendo criadas para três máquinas virtuais.

1. Substitua o valor de **$saName** por um nome para a conta de armazenamento. Teste a exclusividade do nome. 
   
        $saName = "storage account name"
        Get-AzureRmStorageAccountNameAvailability $saName
   
    Se a resposta for **True**, o nome proposto será exclusivo.
2. Substitua o valor de **$saType** pelo tipo de conta de armazenamento e crie a variável:  
   
        $saType = "storage account type"
   
    Os valores possíveis são: Standard_LRS, Standard_GRS, Standard_RAGRS ou Premium_LRS.
3. Crie a conta:
   
        New-AzureRmStorageAccount -Name $saName -ResourceGroupName $rgName –Type $saType -Location $locName
   
    Você deverá ver algo como este exemplo:
   
        ResourceGroupName   : myrg1
        StorageAccountName  : myst1
        Id                  : /subscriptions/########-####-####-####-############/resourceGroups/myrg1/providers/Microsoft
                              .Storage/storageAccounts/myst1
        Location            : centralus
        AccountType         : StandardLRS
        CreationTime        : 3/15/2016 4:51:52 PM
        CustomDomain        :
        LastGeoFailoverTime :
        PrimaryEndpoints    : Microsoft.Azure.Management.Storage.Models.Endpoints
        PrimaryLocation     : centralus
        ProvisioningState   : Succeeded
        SecondaryEndpoints  :
        SecondaryLocation   :
        StatusOfPrimary     : Available
        StatusOfSecondary   :
        Tags                : {}
        Context             : Microsoft.WindowsAzure.Commands.Common.Storage.AzureStorageContext
4. Repita as etapas de 1 a 4 para criar três contas de armazenamento, por exemplo, myst1, myst2 e myst3.

### <a name="virtual-network"></a>rede virtual
Uma rede virtual é necessária para as máquinas virtuais no conjunto de escala.

1. Substitua o valor de **$subnetName** pelo nome que você deseja usar para a sub-rede na rede virtual e crie a variável: 
   
        $subnetName = "subnet name"
2. Crie as configurações de sub-rede:
   
        $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
   
    O prefixo do endereço pode ser diferente em sua rede virtual.
3. Substitua o valor de **$netName** pelo nome que você deseja usar para a rede virtual e crie a variável: 
   
        $netName = "virtual network name"
4. Crie a rede virtual:
   
        $vnet = New-AzureRmVirtualNetwork -Name $netName -ResourceGroupName $rgName -Location $locName -AddressPrefix 10.0.0.0/16 -Subnet $subnet

### <a name="configuration-of-the-scale-set"></a>Configuração do conjunto de escala
Você tem todos os recursos necessários para a configuração do conjunto de escala, então vamos criá-lo.  

1. Substitua o valor de **$ipName** pelo nome que você deseja usar para a configuração do IP e crie a variável: 
   
        $ipName = "IP configuration name"
2. Crie a configuração de IP:
   
        $ipConfig = New-AzureRmVmssIpConfig -Name $ipName -LoadBalancerBackendAddressPoolsId $null -SubnetId $vnet.Subnets[0].Id
3. Substitua o valor de **$vmssConfig** pelo nome que você deseja usar para a configuração do conjunto de dimensionamento e crie a variável:   
   
        $vmssConfig = "Scale set configuration name"
4. Crie a configuração do conjunto de escala:
   
        $vmss = New-AzureRmVmssConfig -Location $locName -SkuCapacity 3 -SkuName "Standard_A0" -UpgradePolicyMode "manual"
   
    Este exemplo mostra um conjunto de escala sendo criado com 3 máquinas virtuais. Consulte [Visão Geral dos Conjuntos de Dimensionamento da Máquina Virtual](virtual-machine-scale-sets-overview.md) para saber mais sobre a capacidade dos conjuntos de dimensionamento. Essa etapa também inclui a definição do tamanho (chamado de SkuName) das máquinas virtuais no conjunto. Para encontrar um tamanho que atenda às suas necessidades, veja [Tamanhos das máquinas virtuais](../virtual-machines/virtual-machines-windows-sizes.md).
5. Adicione a configuração da interface de rede à configuração do conjunto de escala:
   
        Add-AzureRmVmssNetworkInterfaceConfiguration -VirtualMachineScaleSet $vmss -Name $vmssConfig -Primary $true -IPConfiguration $ipConfig
   
    Você deverá ver algo como este exemplo:
   
        Sku                   : Microsoft.Azure.Management.Compute.Models.Sku
        UpgradePolicy         : Microsoft.Azure.Management.Compute.Models.UpgradePolicy
        VirtualMachineProfile : Microsoft.Azure.Management.Compute.Models.VirtualMachineScaleSetVMProfile
        ProvisioningState     :
        OverProvision         :
        Id                    :
        Name                  :
        Type                  :
        Location              : Central US
        Tags                  :

#### <a name="operating-system-profile"></a>Perfil do sistema operacional
1. Substitua o valor de **$computerName** pelo prefixo do nome do computador que você deseja usar e crie a variável: 
   
        $computerName = "computer name prefix"
2. Substitua o valor de **$adminName** pelo nome da conta de administrador nas máquinas virtuais e crie a variável:
   
        $adminName = "administrator account name"
3. Substitua o valor de **$adminPassword** pela senha da conta e crie a variável:
   
        $adminPassword = "password for administrator accounts"
4. Crie o perfil do sistema operacional:
   
        Set-AzureRmVmssOsProfile -VirtualMachineScaleSet $vmss -ComputerNamePrefix $computerName -AdminUsername $adminName -AdminPassword $adminPassword

#### <a name="storage-profile"></a>Perfil de armazenamento
1. Substitua o valor de **$storageProfile** pelo nome que você deseja usar para o perfil de armazenamento e crie a variável:  
   
        $storageProfile = "storage profile name"
2. Crie as variáveis que definem a imagem a ser usada:  
   
        $imagePublisher = "MicrosoftWindowsServer"
        $imageOffer = "WindowsServer"
        $imageSku = "2012-R2-Datacenter"
   
    Para encontrar informações sobre outras imagens a usar, confira [Navegar e selecionar imagens da máquina virtual do Azure com o Windows PowerShell e a CLI do Azure](../virtual-machines/virtual-machines-windows-cli-ps-findimage.md).
3. Substitua o valor de **$vhdContainers** por uma lista que contém os caminhos nos quais os discos rígidos virtuais são armazenados, como "https://mystorage.blob.core.windows.net/vhds", em seguida, crie a variável:
   
        $vhdContainers = @("https://myst1.blob.core.windows.net/vhds","https://myst2.blob.core.windows.net/vhds","https://myst3.blob.core.windows.net/vhds")
4. Crie o perfil de armazenamento:
   
        Set-AzureRmVmssStorageProfile -VirtualMachineScaleSet $vmss -ImageReferencePublisher $imagePublisher -ImageReferenceOffer $imageOffer -ImageReferenceSku $imageSku -ImageReferenceVersion "latest" -Name $storageProfile -VhdContainer $vhdContainers -OsDiskCreateOption "FromImage" -OsDiskCaching "None"  

### <a name="virtual-machine-scale-set"></a>Conjunto de escala de máquina virtual
Por fim, você pode criar o conjunto de escala.

1. Substitua o valor de **$vmssName** pelo nome do conjunto de dimensionamento da máquina virtual e crie a variável:
   
        $vmssName = "scale set name"
2. Crie o conjunto de escala:
   
        New-AzureRmVmss -ResourceGroupName $rgName -Name $vmssName -VirtualMachineScaleSet $vmss
   
    Você deve ver algo como esse exemplo, que mostra uma implantação bem-sucedida:
   
        Sku                   : Microsoft.Azure.Management.Compute.Models.Sku
        UpgradePolicy         : Microsoft.Azure.Management.Compute.Models.UpgradePolicy
        VirtualMachineProfile : Microsoft.Azure.Management.Compute.Models.VirtualMachineScaleSetVMProfile
        ProvisioningState     : Updating
        OverProvision         :
        Id                    : /subscriptions/########-####-####-####-############/resourceGroups/myrg1/providers/Microso
                                ft.Compute/virtualMachineScaleSets/myvmss1
        Name                  : myvmss1
        Type                  : Microsoft.Compute/virtualMachineScaleSets
        Location              : centralus
        Tags                  :

## <a name="step-3-explore-resources"></a>Etapa 3: explorar recursos
Use estes recursos para explorar o conjunto de escala de máquina virtual que criou:

* Portal do Azure - uma quantidade limitada de informações está disponível por meio do portal.
* [Gerenciador de Recursos do Azure](https://resources.azure.com/) – esta é a melhor ferramenta para explorar o estado atual do conjunto de escala.
* Azure PowerShell - use este comando para obter informações:
  
        Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
  
        Or 
  
        Get-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"

## <a name="next-steps"></a>Próximas etapas
* Gerencie o conjunto de dimensionamento que você acabou de criar usando as informações em [Gerenciar máquinas virtuais em um Conjunto de Dimensionamento da Máquina Virtual](virtual-machine-scale-sets-windows-manage.md)
* Considere configurar o dimensionamento automático de seu conjunto de dimensionamento usando as informações em [Dimensionamento automático e conjuntos de dimensionamento da máquina virtual](virtual-machine-scale-sets-autoscale-overview.md)
* Saiba mais sobre o dimensionamento vertical revisando [Dimensionamento vertical automático com conjuntos de Dimensionamento da Máquina Virtual](virtual-machine-scale-sets-vertical-scale-reprovision.md)




<!--HONumber=Nov16_HO2-->


