---
title: "Problemas de reinicialização ou redimensionamento da VM | Microsoft Docs"
description: "Solucionar problemas de implantação clássica ao reinicializar ou redimensionar uma Máquina Virtual Linux existente no Azure"
services: virtual-machines-linux
documentationcenter: 
author: Deland-Han
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 73f2672c-602e-4766-8948-2b180115d299
ms.service: virtual-machines-linux
ms.topic: support-article
ms.tgt_pltfrm: vm-linux
ms.workload: required
ms.date: 09/20/2016
ms.devlang: na
ms.author: delhan
translationtype: Human Translation
ms.sourcegitcommit: ee34a7ebd48879448e126c1c9c46c751e477c406
ms.openlocfilehash: 9268bc13c33893df624bd2311ba28f898e2fee90


---
# <a name="troubleshoot-classic-deployment-issues-with-restarting-or-resizing-an-existing-linux-virtual-machine-in-azure"></a>Solucionar problemas de implantação clássica ao reinicializar ou redimensionar uma Máquina Virtual Linux existente no Azure
> [!div class="op_single_selector"]
> * [Clássico](virtual-machines-linux-classic-restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
> * [Gerenciador de Recursos](virtual-machines-linux-restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
> 
> 

Ao tentar iniciar uma VM (Máquina Virtual) do Azure parada ou redimensionar uma VM do Azure existente, o erro comum encontrado é uma falha de alocação. Esse erro ocorre quando o cluster ou a região não tem recursos disponíveis ou quando não dá suporte ao tamanho de VM solicitado.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Para a versão do Resource Manager, consulte [aqui](virtual-machines-linux-restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Coletar logs de auditoria
Para iniciar a solução de problemas, colete os logs de auditoria para identificar o erro associado ao problema.

No Portal do Azure, clique em **Procurar** > **Máquinas virtuais** > *sua máquina virtual do Linux* > **Configurações** > **Logs de auditoria**.

## <a name="issue-error-when-starting-a-stopped-vm"></a>Problema: erro ao iniciar uma VM parada
Você tenta iniciar uma VM parada, mas ocorre uma falha de alocação.

### <a name="cause"></a>Causa
Deve-se tentar fazer a solicitação de início da VM parada no cluster original que hospeda o serviço de nuvem. No entanto, o cluster não tem espaço livre disponível para atender à solicitação.

### <a name="resolution"></a>Resolução
* Crie um novo serviço de nuvem e o associe a uma região ou a uma rede virtual baseada em região, mas não a um grupo de afinidades.
* Exclua a VM parada.
* Recrie a VM no novo serviço de nuvem usando os discos.
* Inicie a VM recriada.

Se você receber um erro ao tentar criar um novo serviço de nuvem, tente novamente mais tarde ou altere a região do serviço de nuvem.

> [!IMPORTANT]
> O novo serviço de nuvem terá um novo nome e VIP e, portanto, será necessário alterar essas informações em todas as dependências que usam essas informações para o serviço de nuvem existente.
> 
> 

## <a name="issue-error-when-resizing-an-existing-vm"></a>Problema: erro ao redimensionar uma VM existente
Você tenta redimensionar uma VM existente, mas ocorre uma falha de alocação.

### <a name="cause"></a>Causa
Deve-se tentar fazer a solicitação de redimensionamento da VM no cluster original que hospeda o serviço de nuvem. No entanto, o cluster não dá suporte ao tamanho de VM solicitado.

### <a name="resolution"></a>Resolução
Reduza o tamanho de VM solicitado e tente fazer novamente a solicitação de redimensionamento.

* Clique em **Procurar tudo** > **Máquinas virtuais (clássicas)** > *sua máquina virtual* > **Configurações** > **Tamanho**. Para obter as etapas detalhadas, consulte [Redimensionar a máquina virtual](https://msdn.microsoft.com/library/dn168976.aspx).

Se não for possível reduzir o tamanho da VM, siga estas etapas:

* Crie um novo serviço de nuvem, garantindo que ele não esteja vinculado a um grupo de afinidades nem associado a uma rede virtual vinculada a um grupo de afinidades.
* Crie uma nova VM maior nele.

É possível consolidar todas as VMs no mesmo serviço de nuvem. Se o serviço de nuvem existente estiver associado a uma rede virtual baseada em região, será possível conectar o novo serviço de nuvem à rede virtual existente.

Se o serviço de nuvem existente não estiver associado a uma rede virtual baseada em região, será necessário excluir as VMs no serviço de nuvem existente e recriá-las no novo serviço de nuvem por meio de seus discos. No entanto, é importante lembrar-se de que o novo serviço de nuvem terá um novo nome e VIP e, portanto, será necessário atualizá-los em todas as dependências que atualmente usam essas informações para o serviço de nuvem existente.

## <a name="next-steps"></a>Próximas etapas
Se você encontrar problemas ao criar uma nova VM do Linux no Azure, veja [Solucionar problemas de implantação com a criação de uma nova máquina virtual do Linux no Azure](virtual-machines-linux-troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).




<!--HONumber=Nov16_HO3-->


