---
title: Gerenciar seus aplicativos no Visual Studio | Microsoft Docs
description: "Use o Visual Studio para criar, desenvolver, empacotar, implantar e depurar seus aplicativos e serviços do Service Fabric."
services: service-fabric
documentationcenter: .net
author: seanmck
manager: timlt
editor: 
ms.assetid: c317cb7e-7eae-466e-ba41-6aa2518be5cf
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/09/2016
ms.author: seanmck;mikhegn
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: d2766c64f4ffdcaf8e583493060116a244e89f3d


---
# <a name="use-visual-studio-to-simplify-writing-and-managing-your-service-fabric-applications"></a>Usar o Visual Studio para simplificar a escrita e o gerenciamento de seus aplicativos do Service Fabric
É possível gerenciar os serviços e aplicativos do Service Fabric do Azure por meio do Visual Studio. Depois de [configurar o ambiente de desenvolvimento](service-fabric-get-started.md),você pode usar o Visual Studio para criar aplicativos do Service Fabric, adicionar serviços, ou pacotes, registrar e implantar aplicativos no cluster de desenvolvimento local.

## <a name="deploy-your-service-fabric-application"></a>Implantar o aplicativo do Service Fabric
Por padrão, a implantação de um aplicativo combina as etapas a seguir em uma única operação:

1. Criar o pacote de aplicativo
2. Carregar o pacote de aplicativo no repositório de imagens
3. Registrar o tipo de aplicativo
4. Remover as instâncias de aplicativo em execução
5. Criar uma nova instância do aplicativo

No Visual Studio, pressionar **F5** também implanta seu aplicativo e anexa o depurador a todas as instâncias do aplicativo. Você pode usar **Ctrl + F5** para implantar um aplicativo sem depuração ou pode publicar um cluster local ou remoto usando o perfil de publicação. Para saber mais, confira [Publicar um aplicativo em um cluster remoto usando o Visual Studio](service-fabric-publish-app-remote-cluster.md).

### <a name="application-debug-mode"></a>Modo de Depuração do Aplicativo
Por padrão, o Visual Studio remove as instâncias existentes do seu tipo de aplicativo quando você interrompe a depuração ou (se você tiver implantado o aplicativo sem anexar o depurador) quando você reimplanta o aplicativo. Nesse caso, todos os dados do aplicativo são removidos. Ao fazer a depuração localmente, talvez você queira manter os dados que já criou ao testar uma nova versão do aplicativo, manter o aplicativo em execução ou que sessões subsequentes de depuração atualizem o aplicativo. As Ferramentas do Service Fabric para Visual Studio oferecem uma propriedade chamada **Modo de Depuração do Aplicativo**, que controla se **F5** deve desinstalar o aplicativo, manter o aplicativo em execução depois que uma sessão de depuração terminar ou habilitar o aplicativo a ser atualizado em sessões subsequentes de depuração, em vez de removido e reimplantado.

#### <a name="to-set-the-application-debug-mode-property"></a>Para definir a propriedade Modo de Depuração do Aplicativo
1. No menu de atalho do projeto de aplicativo, escolha **Propriedades** (ou pressione a tecla **F4**).
2. Na janela **Propriedades**, defina a propriedade **Modo de Depuração do Aplicativo**.
   
    ![Definir a Propriedade Modo de Depuração do Aplicativo][debugmodeproperty]

Veja a seguir as opções de **Modo de Depuração do Aplicativo** disponíveis.

1. **Atualização Automática**: o aplicativo continua em execução quando a sessão de depuração termina. O próximo **F5** tratará a implantação como uma atualização usando o modo automático não monitorado para atualizar rapidamente o aplicativo para uma versão mais recente com uma cadeia de caracteres de data anexada. O processo de atualização preserva todos os dados inseridos em uma sessão de depuração anterior.
2. **Manter Aplicativo**: o aplicativo é mantido em execução no cluster quando a sessão de depuração termina. No próximo **F5** , o aplicativo será removido e o aplicativo recém-criado será implantado no cluster.
3. **Remover Aplicativo** faz com que o aplicativo seja removido quando a sessão de depuração termina.

Na **Atualização Automática** , os dados são preservados com a aplicação dos recursos de atualização de aplicativo do Service Fabric, mas são ajustados para otimizar o desempenho em vez da segurança. Para obter mais informações sobre como atualizar aplicativos e como executar uma atualização em um ambiente real, confira [Atualização de aplicativos do Service Fabric](service-fabric-application-upgrade.md).

![Exemplo de nova versão do aplicativo com a data incluída][preservedata]

> [!NOTE]
> Essa propriedade não existe antes da versão 1.1 das Ferramentas do Service Fabric para o Visual Studio. Em versões anteriores a 1.1, use a propriedade **Preservar os Dados ao Iniciar** para obter o mesmo comportamento. A opção "Manter Aplicativo" foi introduzida na versão 1.2 das Ferramentas do Service Fabric para o Visual Studio.
> 
> 

## <a name="add-a-service-to-your-service-fabric-application"></a>Adicione um serviço ao aplicativo da Malha de Serviços
Você pode adicionar novos serviços a seu aplicativo para estender sua funcionalidade.  Para garantir que o serviço esteja incluído no seu pacote de aplicativos, adicione o serviço usando o item de menu **Novo Serviço de Malha...** .

![Adicionar um novo serviço de malha ao aplicativo][newservice]

Selecione um tipo de projeto da Malha do Serviço para adicionar ao aplicativo e especifique um nome para o serviço.  Confira [Como escolher uma estrutura para o serviço](service-fabric-choose-framework.md) para ajudar com a decisão de que tipo de serviço usar.

![Selecionar um tipo de projeto do Serviço de Malha para adicionar ao aplicativo][addserviceproject]

O novo serviço será adicionado à solução e ao pacote de aplicativo existente. As referências de serviço e uma instância de serviço padrão serão adicionadas ao manifesto do aplicativo. O serviço será criado e iniciado na próxima vez que você implantar o aplicativo.

![O novo serviço será adicionado ao manifesto do aplicativo][newserviceapplicationmanifest]

## <a name="package-your-service-fabric-application"></a>Empacotar o aplicativo do Service Fabric
Para implantar o aplicativo e seu serviço em um cluster, você precisa criar um pacote de aplicativos.  O pacote organiza o manifesto do aplicativo, os manifestos do serviço e outros arquivos necessários em um layout específico.  O Visual Studio configura e gerencia o pacote na pasta do projeto do aplicativo, no diretório 'pkg'.  Clicar em **Pacote** no menu de contexto **Aplicativo** cria ou atualiza o pacote de aplicativos.  Convém fazer isso se você implantar o aplicativo usando scripts personalizados de PowerShell.

## <a name="remove-applications-and-application-types-using-cloud-explorer"></a>Remover aplicativos e tipos de aplicativo usando o Gerenciador de Nuvem
Você pode executar operações de gerenciamento de cluster básico no Visual Studio usando o Cloud Explorer, que pode ser iniciado pelo menu **Exibir** . Por exemplo, você pode excluir aplicativos e desprovisionar tipos de aplicativos em clusters locais ou remotos.

![Remover um aplicativo](./media/service-fabric-manage-application-in-visual-studio/removeapplication.png)

> [!TIP]
> Para funcionalidade de gerenciamento de cluster mais avançada, confira [Visualizando o cluster com o Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).
> 
> 

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Próximas etapas
* [Modelo de aplicativo da Malha do Serviço](service-fabric-application-model.md)
* [Implantação de aplicativo da Malha do Serviço](service-fabric-deploy-remove-applications.md)
* [Gerenciando parâmetros do aplicativo para vários ambientes](service-fabric-manage-multiple-environment-app-configuration.md)
* [Depurando o aplicativo da Malha do Serviço](service-fabric-debugging-your-application.md)
* [Visualização do cluster usando o Gerenciador do Service Fabric](service-fabric-visualizing-your-cluster.md)

<!--Image references-->
[addserviceproject]:./media/service-fabric-manage-application-in-visual-studio/addserviceproject.png
[manageservicefabric]: ./media/service-fabric-manage-application-in-visual-studio/manageservicefabric.png
[newservice]:./media/service-fabric-manage-application-in-visual-studio/newservice.png
[newserviceapplicationmanifest]:./media/service-fabric-manage-application-in-visual-studio/newserviceapplicationmanifest.png
[preservedata]:./media/service-fabric-manage-application-in-visual-studio/preservedata.png
[debugmodeproperty]:./media/service-fabric-manage-application-in-visual-studio/debugmodeproperty.png



<!--HONumber=Nov16_HO3-->


