---
title: "Implantar um conjunto de dimensionamento de máquinas virtuais usando o Visual Studio | Microsoft Docs"
description: "Implantar um conjunto de escala de máquina virtual usando o Visual Studio e um modelo do Resource Manager"
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: ed0786b8-34b2-49a8-85b5-2a628128ead6
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2016
ms.author: guybo
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 172659089e0ed3bf06444699036f3cebcd082ae3


---
# <a name="deploy-virtual-machine-scale-set-using-visual-studio"></a>Implantar um conjunto de escala de máquina virtual usando o Visual Studio
Este artigo mostra como implantar um Conjunto de Escala de Máquina Virtual do Azure usando uma Implantação de Grupo de Recursos do Visual Studio.

[Conjuntos de Escala de Máquina Virtual do Azure](https://azure.microsoft.com/blog/azure-vm-scale-sets-public-preview/) são um recurso de computação do Azure para implantar e gerenciar uma coleção de máquinas virtuais semelhantes com opções facilmente integradas para escala automática e balanceamento de carga. É possível provisionar e implantar os Conjuntos de Escala de VM usando os [Modelos do ARM (Azure Resource Manager)](https://github.com/Azure/azure-quickstart-templates). Os Modelos do ARM podem ser implantados usando a CLI do Azure, o PowerShell, o REST e também diretamente no Visual Studio. O Visual Studio fornece um conjunto de Modelos de exemplo que podem ser implantados como parte de um projeto de Implantação do Grupo de Recursos do Azure.

Implantações de Grupo de Recursos do Azure são uma maneira de agrupar e publicar um conjunto de recursos relacionados do Azure em uma única operação de implantação. Saiba mais sobre eles aqui: [Criando e implantando grupos de recursos do Azure por meio do Visual Studio](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

## <a name="pre-requisites"></a>Pré-requisitos
Para começar a implantar Conjuntos de Escala de VM no Visual Studio, você precisará do seguinte:

* Visual Studio 2013 ou 2015
* SDK do Azure 2.7, 2.8 ou 2.9

Observação: com essas instruções, pressupomos que você esteja usando o Visual Studio 2015 com o [SDK do Azure 2.8](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/).

## <a name="creating-a-project"></a>Criando um projeto
1. Crie um novo projeto no Visual Studio 2015 escolhendo **Arquivo | Novo | Projeto**.
   
    ![Arquivo novo][file_new]
2. Em **Visual C# | Nuvem**, escolha **Azure Resource Manager** para criar um projeto para implantar um Modelo do ARM.
   
    ![Criar projeto][create_project]
3. Na lista de Modelos, selecione o Modelo de Conjunto de Escala de Máquina Virtual do Windows ou do Linux.
   
   ![Selecionar modelo][select_Template]
4. Depois de criar seu projeto, você verá scripts de implantação do PowerShell, um Modelo do Azure Resource Manager e um arquivo de parâmetro para o Conjunto de Escala de Máquina Virtual.
   
    ![Gerenciador de Soluções][solution_explorer]

## <a name="customize-your-project"></a>Personalizar seu projeto
Agora você pode editar o Modelo para personalizá-lo de acordo com as necessidades de seu aplicativo, como adicionar propriedades de extensão de VM ou editar regras de balanceamento de carga. Por padrão, os Modelos do Conjunto de Escala de VM são configurados para implantar a extensão AzureDiagnostics, que torna mais fácil adicionar regras de escala automática. Ele também implanta um balanceador de carga com um endereço IP público, configurado com regras NAT de entrada que permitem que você se conecte a instâncias de VM com SSH (Linux) ou RDP (Windows) – o intervalo de portas de front-end inicia em 50000, o que significa que, no caso do Linux, se você usar o SSH para a porta 50000 do endereço IP público (ou o nome de domínio), você será direcionado para a porta 22 da primeira VM no Conjunto de Escala. A conexão à porta 50001 será roteada para a porta 22 da segunda VM e assim por diante.

 Uma boa maneira de editar os Modelos com o Visual Studio é usar a Estrutura de Tópicos JSON para organizar os parâmetros, as variáveis e os recursos. Com uma compreensão do esquema do Visual Studio, é possível detectar erros em seu Modelo antes de implantá-lo.

![Gerenciador JSON][json_explorer]

## <a name="deploy-the-project"></a>Implantar o projeto
1. Implante o Modelo do ARM no Azure para criar o recurso de Conjunto de Escala de VM. Clique com o botão direito do mouse no nó do projeto e escolha **Implantar | Nova Implantação**.
   
    ![Implantar o modelo][5deploy_Template]
2. Selecione sua assinatura no diálogo “Implantar no Grupo de Recursos”.
   
    ![Implantar o modelo][6deploy_Template]
3. Aqui, também é possível criar um novo Grupo de Recursos do Azure no qual você poderá implantar seu Modelo.
   
    ![Novo grupo de recursos][new_resource]
4. Em seguida, selecione o botão **Editar Parâmetros** para inserir parâmetros que serão transmitidos para o Modelo; alguns valores como o nome de usuário e a senha para o SO são necessários para criar a implantação. Se você não tiver as Ferramentas do PowerShell para o Visual Studio instaladas, será recomendável marcar "Salvar senhas" para evitar um prompt oculto da linha de comando do PowerShell ou usar o [suporte keyvault](https://azure.microsoft.com/blog/keyvault-support-for-arm-templates/).
   
    ![Editar Parâmetros][edit_parameters]
5. Agora clique em **Implantar**. A janela **Saída** mostrará o progresso da implantação. Observe que a ação executa o script **Deploy-AzureResourceGroup.ps1** .
   
   ![Janela de saída][output_window]

## <a name="exploring-your-vm-scale-set"></a>Explorando seu Conjunto de Escala de VM
Depois que a implantação for concluída, é possível exibir o novo Conjunto de Escala de VM no **Gerenciador de Nuvem** do Visual Studio (basta atualizar a lista). O Gerenciador de Nuvem permite que você gerencie recursos do Azure no Visual Studio ao mesmo tempo que desenvolve aplicativos. Você também pode exibir o conjunto de dimensionamento de VMs no [portal do Azure](https://portal.azure.com) e no [Azure Resource Manager](https://resources.azure.com/).

![Cloud Explorer][cloud_explorer]

 O portal fornece a melhor maneira de gerenciar visualmente sua infraestrutura do Azure com um navegador da Web, enquanto o Azure Resource Manager fornece uma maneira fácil de explorar e depurar recursos do Azure, oferecendo uma janela na “exibição de instância”, além de mostrar comandos do PowerShell para os recursos que você está examinando. Enquanto os Conjuntos de Escala de VM estão em visualização, o Gerenciador de Recursos mostrará a maioria dos detalhes dos Conjuntos de Escala de VM.

## <a name="next-steps"></a>Próximas etapas
Depois de implantar com êxito os Conjuntos de Escala de VM por meio do Visual Studio, você pode personalizar seu projeto ainda mais para atender às necessidades de seu aplicativo. Por exemplo, configurar a escala automática adicionando um recurso do Insights, adicionar a infraestrutura ao Modelo como VMs autônomas ou implantar aplicativos usando a extensão de script personalizado. Uma boa fonte de Modelos de exemplo pode ser encontrada no repositório GitHub de [Modelos de Início Rápido do Azure](https://github.com/Azure/azure-quickstart-templates) (pesquise por “vmss”).

[file_new]: ./media/virtual-machine-scale-sets-vs-create/1-FileNew.png
[create_project]: ./media/virtual-machine-scale-sets-vs-create/2-CreateProject.png
[select_Template]: ./media/virtual-machine-scale-sets-vs-create/3b-SelectTemplateLin.png
[solution_explorer]: ./media/virtual-machine-scale-sets-vs-create/4-SolutionExplorer.png
[json_explorer]: ./media/virtual-machine-scale-sets-vs-create/10-JsonExplorer.png
[5deploy_Template]: ./media/virtual-machine-scale-sets-vs-create/5-DeployTemplate.png
[6deploy_Template]: ./media/virtual-machine-scale-sets-vs-create/6-DeployTemplate.png
[new_resource]: ./media/virtual-machine-scale-sets-vs-create/7-NewResourceGroup.png
[edit_parameters]: ./media/virtual-machine-scale-sets-vs-create/8-EditParameter.png
[output_window]: ./media/virtual-machine-scale-sets-vs-create/9-Output.png
[cloud_explorer]: ./media/virtual-machine-scale-sets-vs-create/12-CloudExplorer.png



<!--HONumber=Nov16_HO3-->


