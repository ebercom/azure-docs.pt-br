---
title: "Sobre imagens de máquinas virtuais do Windows | Microsoft Docs"
description: "Saiba mais sobre como as imagens são usadas com máquinas virtuais do Windows no Azure."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 66ff3fab-8e7f-4dff-b8da-ab1c9c9c9af8
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/21/2016
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: f6537e4ebac76b9f3328223ee30647885ee15d3e
ms.openlocfilehash: c118f9ebb8da496ad33b6e24e80d16ccf6557928


---
# <a name="about-images-for-windows-virtual-machines"></a>Sobre imagens de máquinas virtuais do Windows
> [!IMPORTANT] 
> O Azure tem dois modelos de implantação diferentes para criar e trabalhar com recursos: [Gerenciador de Recursos e Clássico](../azure-resource-manager/resource-manager-deployment-model.md). Este artigo aborda o uso do modelo de implantação Clássica. A Microsoft recomenda que a maioria das implantações novas use o modelo do Gerenciador de Recursos. Para obter informações sobre como localizar e usar imagens no modelo do Resource Manager, consulte [aqui](virtual-machines-windows-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

[!INCLUDE [virtual-machines-common-classic-about-images](../../includes/virtual-machines-common-classic-about-images.md)]

## <a name="working-with-images"></a>Trabalhando com imagens
Você pode usar o módulo do Azure PowerShell, para gerenciar as imagens disponíveis para sua assinatura do Azure. Você também pode usar o portal clássico do Azure para algumas tarefas de imagem, porém a linha de comando oferece mais opções.

* **Obter todas as imagens**: `Get-AzureVMImage`retorna uma lista de todas as imagens disponíveis em sua assinatura atual: suas imagens, bem como as fornecidos pelo Azure ou por parceiros. Isso significa que você pode obter uma lista grande. Os exemplos a seguir mostram como obter uma lista menor.
* **Obter as famílias de imagem**:`Get-AzureVMImage | select ImageFamily` obtém uma lista das famílias de imagem, mostrando as cadeias de caracteres da propriedade **ImageFamily**.
* **Obtenha todas as imagens em uma família específica**: `Get-AzureVMImage | Where-Object {$_.ImageFamily -eq $family}`
* **Localizar imagens da VM**: `Get-AzureVMImage | where {(gm –InputObject $_ -Name DataDiskConfigurations) -ne $null} | Select -Property Label, ImageName` isso funciona com a filtragem da propriedade DataDiskConfiguration, que só se aplica a imagens de VM. Este exemplo também filtra a saída para apenas o nome do rótulo e da imagem.
* **Salvar uma imagem generalizada**: `Save-AzureVMImage –ServiceName "myServiceName" –Name "MyVMtoCapture" –OSState "Generalized" –ImageName "MyVmImage" –ImageLabel "This is my generalized image"`
* **Salvar uma imagem especializada**: `Save-AzureVMImage –ServiceName "mySvc2" –Name "MyVMToCapture2" –ImageName "myFirstVMImageSP" –OSState "Specialized" -Verbose`
  >[Azure.Tip] O parâmetro OSState é obrigatório se você quiser criar uma imagem VM, que inclui discos de dados e o disco do sistema operacional. Se você não usar o parâmetro, o cmdlet criará uma imagem do SO. O valor do parâmetro indica se a imagem é generalizada ou especializada, com base em se o disco do sistema operacional foi preparado para reutilização.
* **Excluir uma imagem**: `Remove-AzureVMImage –ImageName "MyOldVmImage"`

## <a name="next-steps"></a>Próximas etapas
Você também pode [criar um computador Windows usando o portal clássico](virtual-machines-windows-classic-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)




<!--HONumber=Dec16_HO1-->


