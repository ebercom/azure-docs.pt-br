---
title: Criar um novo recurso do Azure Application Insights | Microsoft Docs
description: Configure manualmente o monitoramento do Application Insights para um novo aplicativo em tempo real.
services: application-insights
documentationcenter: 
author: alancameronwills
manager: douge
ms.assetid: 878b007e-161c-4e36-8ab2-3d7047d8a92d
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 75b651bd3e77ac19e22dcc3442870469fe2aaca1
ms.openlocfilehash: 60a94a333ffb4bf674e370972abd6fa0caf37d91


---
# <a name="create-an-application-insights-resource"></a>Criar um recurso do Application Insights
O Azure Application Insights exibe dados sobre o seu aplicativo em um *recurso* do Microsoft Azure. Por isso, criar um novo recurso faz parte da [configuração do Application Insights para monitorar um novo aplicativo][start]. Em muitos casos, isso pode ser feito automaticamente pelo IDE e que é a maneira recomendada, se disponível. Porém, em alguns casos, você criará um recurso manualmente, por exemplo, para ter recursos separados para versões de desenvolvimento e produção do seu aplicativo.

Depois de criar o recurso, você pode obter sua chave de instrumentação e usá-la para configurar o SDK no aplicativo. Isso envia a telemetria para o recurso.

## <a name="sign-up-to-microsoft-azure"></a>Inscreva-se no Microsoft Azure
Se você ainda não tiver uma [conta da Microsoft, obtenha uma agora mesmo](http://live.com). (Se usar serviços como Outlook.com, OneDrive, Windows Phone ou XBox Live, você já tem uma conta da Microsoft.)

Você também precisará de uma assinatura do [Microsoft Azure](http://azure.com). Se sua equipe ou organização tem uma assinatura do Azure, o proprietário pode adicioná-lo a ela usando seu Windows Live ID. Você será cobrado apenas pelo que usar e o plano básico padrão inclui uma determinada quantidade de uso experimental gratuitamente.

Quando você tem acesso a uma assinatura, entre no Application Insights em [http://portal.azure.com](https://portal.azure.com)e use sua Live ID para fazer logon.

## <a name="create-an-application-insights-resource"></a>Criar um recurso do Application Insights
No [portal.azure.com](https://portal.azure.com), adicione um recurso do Application Insights:

![Clique em Novo, Application Insights](./media/app-insights-create-new-resource/01-new.png)

* O **tipo de aplicativo** afeta o que você vê na folha de visão geral e as propriedades disponíveis no [explorador de métricas][metrics]. Se você não vir seu tipo de aplicativo, escolha o ASP.NET.
* **Grupo de recursos** é uma conveniência para o gerenciamento de propriedades, como controle de acesso. Se você já criou outros recursos do Azure, poderá optar por colocar esse novo recurso no mesmo grupo.
* **Assinatura** é a sua conta de pagamento no Azure.
* **Local** é onde podemos manter seus dados.
* **Adicionar ao quadro inicial** coloca um bloco de acesso rápido para o recurso em sua página inicial do Azure. Recomendável.

Quando seu aplicativo tiver sido criado, uma nova folha será aberta. Nela você verá os dados de uso e de desempenho do seu aplicativo. 

Para retornar a ele na próxima vez que fizer logon no Azure, procure pelo bloco de início rápido do aplicativo na Tela Inicial. Ou então, clique em Procurar para localizá-lo.

## <a name="copy-the-instrumentation-key"></a>Copiar a chave de instrumentação
A chave de instrumentação identifica o recurso que você criou. É preciso fornecê-la ao SDK.

![Clique em Essentials, clique na Chave de Instrumentação, CTRL+C](./media/app-insights-create-new-resource/02-props.png)

## <a name="install-the-sdk-in-your-app"></a>Instalar o SDK em seu aplicativo
Instale o SDK do Application Insights SDK no aplicativo. Esta etapa depende muito do tipo do seu aplicativo. 

Use a chave de instrumentação para configurar o [SDK instalado no seu aplicativo][start].

O SDK inclui módulos padrão que enviam telemetria sem precisar escrever nenhum código. Para rastrear ações do usuário ou diagnosticar problemas mais detalhadamente, [use a API][api] para enviar sua própria telemetria.

## <a name="a-namemonitorasee-telemetry-data"></a><a name="monitor"></a>Consultar os dados de telemetria
Feche a folha de início rápido para retornar para a folha de aplicativo no Portal do Azure.

Clique no bloco de Pesquisa para ver a [Pesquisa de Diagnóstico][diagnostic], no qual os primeiros eventos serão exibidos. 

Se você estiver esperando mais dados, clique em Atualizar depois de alguns segundos.

## <a name="creating-a-resource-automatically"></a>Criando um recurso automaticamente
Você pode escrever um [script do PowerShell](app-insights-powershell-script-create-resource.md) para criar um recurso automaticamente.

## <a name="next-steps"></a>Próximas etapas
* [Criar um painel](app-insights-dashboards.md)
* [Pesquisa de Diagnóstico](app-insights-diagnostic-search.md)
* [Explorar métricas](app-insights-metrics-explorer.md)
* [Escrever consultas do Analytics](app-insights-analytics.md)

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[diagnostic]: app-insights-diagnostic-search.md
[metrics]: app-insights-metrics-explorer.md
[start]: app-insights-overview.md




<!--HONumber=Dec16_HO1-->


