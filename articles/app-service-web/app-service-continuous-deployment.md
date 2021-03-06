---
title: "Implantação contínua no Serviço de Aplicativo do Azure | Microsoft Docs"
description: "Saiba como habilitar a implantação contínua no Serviço de Aplicativo do Azure"
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: wpickett
editor: mollybos
ms.assetid: 6adb5c84-6cf3-424e-a336-c554f23b4000
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/28/2016
ms.author: dariagrigoriu
translationtype: Human Translation
ms.sourcegitcommit: 4edd2696c9a5709ded6e2a3e352090775335f0d2
ms.openlocfilehash: 033fd0cb3512a22bb05488fe83aa2ffa9d81def3


---
# <a name="continuous-deployment-to-azure-app-service"></a>Implantação contínua no Serviço de Aplicativo do Azure
Este tutorial mostra a você como configurar um fluxo de trabalho de implantação contínua para seu aplicativo do [Serviço de Aplicativo do Azure] . A integração do Serviço de Aplicativo com o VSTS (Visual Studio Team Services), GitHub e BitBucket permite um fluxo de trabalho de implantação contínua, no qual o Azure recebe por pull as atualizações mais recentes de seu projeto publicado em um desses serviços. A implantação contínua é uma ótima opção para projetos nos quais várias contribuições frequentes são integradas.

## <a name="a-nameoverviewaenable-continuous-deployment"></a><a name="overview"></a>Habilitar a implantação contínua
Para habilitar a implantação contínua,

1. Publique o conteúdo de seu aplicativo no repositório que será usado para a implantação contínua.  
    Para saber mais sobre como publicar seu projeto nesses serviços, confira [Criar um repositório (GitHub)], [Criar um repositório (BitBucket)] e [Introdução ao VSTS].
2. Na folha do menu do aplicativo no [Portal do Azure], clique em **IMPLANTAÇÃO DE APLICATIVOS > Opções de implantação**. Clique em **Escolher origem** e selecione a origem da implantação.  
   
    ![](./media/app-service-continuous-deployment/cd_options.png)
   
   > [!NOTE]
   > Para configurar uma conta do VSTS para a implantação do Serviço de Aplicativo, consulte este [tutorial](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).
   > 
   > 
3. Conclua o fluxo de trabalho de autorização.
4. Na folha **Origem da implantação** , escolha o projeto e a ramificação dos quais quer implantar. Quando terminar, clique em **OK**.
   
    ![](./media/app-service-continuous-deployment/github_option.png)
   
   > [!NOTE]
   > Ao habilitar a implantação contínua com GitHub ou BitBucket, os projetos públicos e privados serão exibidos.
   > 
   > 
   
    O Serviço de Aplicativo cria uma associação com o repositório selecionado, extrai os arquivos da ramificação especificada e mantém um clone do repositório para seu aplicativo do Serviço de Aplicativo. Quando você configura a implantação contínua do VSTS no Portal do Azure, a integração usa o [mecanismo de implantação Kudu](https://github.com/projectkudu/kudu/wiki) do Serviço de Aplicativo, que já automatiza tarefas de compilação e implantação com cada `git push`. Não é necessário configurar separadamente a implantação contínua no VSTS. Depois que o processo for concluído, a folha do aplicativo **Opções de implantação** mostrará uma implantação ativa, indicando o êxito da implantação.
5. Para verificar se o aplicativo foi implantado com êxito, clique na **URL** na parte superior da folha do aplicativo no Portal do Azure.
6. Para verificar se a implantação contínua está ocorrendo no repositório de sua escolha, envie uma alteração por push para o repositório. Seu aplicativo deve ser atualizado para refletir as alterações logo após a conclusão do envio por push para o repositório. Você pode verificar se ele recebeu a atualização na folha **Opções de implantação** do seu aplicativo.

## <a name="a-namevssolutionacontinuous-deployment-of-a-visual-studio-solution"></a><a name="VSsolution"></a>Implantação contínua de uma solução do Visual Studio
Enviar uma solução do Visual Studio por push para o Serviço de Aplicativo do Azure é tão simples quanto enviar por push um arquivo index.html simples. O processo de implantação do Serviço de Aplicativo simplifica todos os detalhes, incluindo as dependências de restauração NuGet e a criação dos binários do aplicativo. Você pode seguir as práticas recomendadas de controle do código-fonte para manter o código apenas em seu repositório Git e deixar que a implantação do Serviço de Aplicativo cuide do resto.

As etapas para enviar sua solução do Visual Studio para o Serviço de Aplicativo são as mesmas que na [seção anterior](#overview), considerando que você configurou sua solução e o repositório da seguinte maneira:

* Use a opção de controle do código-fonte do Visual Studio para gerar um arquivo `.gitignore`, como na imagem abaixo, ou adicione manualmente um arquivo `.gitignore` à raiz de seu repositório com um conteúdo semelhante a este [exemplo de .gitignore](https://github.com/github/gitignore/blob/master/VisualStudio.gitignore).
  
  ![](./media/app-service-continuous-deployment/VS_source_control.png)
* Adicionar toda a árvore do diretório da solução para seu repositório, com o arquivo .sln na raiz do repositório.

Assim que você tiver configurado seu repositório conforme descrito e tiver configurado seu aplicativo do Azure para publicação contínua por meio de um dos repositórios Git online, você poderá desenvolver seu aplicativo ASP.NET localmente no Visual Studio e implantar continuamente seu código, simplesmente enviando por push suas alterações a seu repositório Git online.

## <a name="a-namedisablecdadisable-continuous-deployment"></a><a name="disableCD"></a>Desabilitar a implantação contínua
Para desabilitar a implantação contínua,

1. Na folha do menu do aplicativo no [Portal do Azure], clique em **IMPLANTAÇÃO DE APLICATIVOS > Opções de implantação**. Em seguida, clique em **Desconectar** na folha **Opções de implantação**.
   
    ![](./media/app-service-continuous-deployment/cd_disconnect.png)
2. Depois de responder **Sim** à mensagem de confirmação, você poderá retornar à folha de seu aplicativo e clicar em **IMPLEMENTAÇÃO DE APLICATIVOS > Opções de implantação**, se quiser configurar a publicação de outra fonte.

## <a name="additional-resources"></a>Recursos adicionais
* [Como investigar problemas comuns com implantação contínua](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* [Como usar o PowerShell para o Azure]
* [Como usar as Ferramentas de Linha de Comando do Azure para Mac e Linux]
* [Documentação do Git]
* [Kudu do Projeto](https://github.com/projectkudu/kudu/wiki)

> [!NOTE]
> Se você deseja começar a usar o Serviço de Aplicativo do Azure antes de se inscrever em uma conta do Azure, vá até [Experimentar o Serviço de Aplicativo](http://go.microsoft.com/fwlink/?LinkId=523751), em que você pode criar imediatamente um aplicativo Web inicial de curta duração no Serviço de Aplicativo. Nenhum cartão de crédito é exigido, sem compromissos.
> 
> 

[Serviço de Aplicativo do Azure]: https://azure.microsoft.com/en-us/documentation/articles/app-service-changes-existing-services/
[Portal do Azure]: https://portal.azure.com
[VSTS Portal]: https://www.visualstudio.com/en-us/products/visual-studio-team-services-vs.aspx
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Como usar o PowerShell para o Azure]: /powershell/azureps-cmdlets-docs
[Como usar as Ferramentas de Linha de Comando do Azure para Mac e Linux]: ../xplat-cli-install.md
[Documentação do Git]: http://git-scm.com/documentation

[Criar um repositório (GitHub)]: https://help.github.com/articles/create-a-repo
[Criar um repositório (BitBucket)]: https://confluence.atlassian.com/display/BITBUCKET/Create+an+Account+and+a+Git+Repo
[Introdução ao VSTS]: https://www.visualstudio.com/get-started/overview-of-get-started-tasks-vs
[Continuous delivery to Azure using Visual Studio Team Services]: ../articles/cloud-services/cloud-services-continuous-delivery-use-vso.md



<!--HONumber=Dec16_HO1-->


