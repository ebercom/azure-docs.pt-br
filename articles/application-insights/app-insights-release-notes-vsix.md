---
title: Notas de versão da extensão do Visual Studio para análise do desenvolvedor
description: As atualizações mais recentes das ferramentas do Visual Studio para análise do desenvolvedor
services: application-insights
documentationcenter: ''
author: acearun
manager: douge

ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 06/09/2016
ms.author: acearun

---
# Notas de versão para ferramentas de análise do desenvolvedor
Novidades: análise do Application Insights e do HockeyApp no Visual Studio.

## Versão 7.0
### Tendências do Application Insights do Visual Studio
O Application Insights do Visual Studio é uma nova ferramenta no Visual Studio que você pode usar para ajudá-lo a analisar como seu aplicativo funciona ao longo do tempo. Para começar, no botão da barra de ferramentas do ** Application Insights** ou na janela Pesquisar do Application Insights, escolha **Explorar Tendências de Telemetria**. Ou, no menu **Exibição**, clique em **Outras Janelas** e em **Tendências do Application Insights**. Escolha uma das cinco consultas comuns para começar. Você pode analisar conjuntos de dados diferentes com base em tipos de telemetria, intervalos de tempo e outras propriedades. Para encontrar anomalias em seus dados, escolha uma das opções de anomalias na lista suspensa **Tipo de Exibição**. As opções de filtragem na parte inferior da janela facilitam o aprimoramento de subconjuntos específicos de sua telemetria.

![Tendências do Application Insights](./media/app-insights-release-notes-vsix/Trends.PNG)

### Exceções em CodeLens
A telemetria de exceção agora é exibida no CodeLens. Se você conectou seu projeto ao serviço Application Insights, verá o número de exceções que ocorreram em cada método na produção nas últimas 24 horas. No CodeLens, você pode ir para Pesquisar ou Tendências a fim de investigar as exceções mais detalhadamente.

![Exceções em CodeLens](./media/app-insights-release-notes-vsix/ExceptionsCodeLens.png)

### Suporte do ASP.NET Core
O Application Insights agora dá suporte a projetos do ASP.NET Core RC2 no Visual Studio. Você pode adicionar o Application Insights a novos projetos do ASP.NET Core RC2 da caixa de diálogo **Novo Projeto**, como mostra a captura de tela a seguir. Ou você pode adicioná-lo a um projeto existente clicando com o botão direito do mouse no projeto no Gerenciador de Soluções e clicando em **Adicionar Application Insights Telemetry**.

![Suporte do ASP.NET Core](./media/app-insights-release-notes-vsix/NetCoreSupport.PNG)

Também há novo suporte a projetos ASP.NET 5 RC1 e ASP.NET Core RC2 na janela Ferramentas de Diagnóstico. Você verá eventos do Application Insights como solicitações e exceções do seu aplicativo ASP.NET ao depurar localmente em seu computador. Em cada evento, clique em **Pesquisa** para fazer busca detalhada e saber mais.

![Suporte a Ferramentas de Diagnóstico](./media/app-insights-release-notes-vsix/DiagnosticTools.PNG)

### HockeyApp para aplicativos universais do Windows
Além dos comentários de usuário e da distribuição beta, o HockeyApp fornece relatórios de falha simbolizados para seus aplicativos Universais do Windows. Facilitamos ainda mais a adição do SDK do HockeyApp: clique com o botão direito do mouse no projeto Universal do Windows e clique em **Hockey App – Habilitar Análise de Falhas**. Isso instala o SDK, configura a coleta de falhas e provisiona um recurso HockeyApp na nuvem sem carregar seu aplicativo no serviço HockeyApp.

Outros novos recursos:

* Fizemos a experiência da Pesquisa do Application Insights mais rápida e mais intuitiva. Agora, intervalos de tempo e filtros de detalhe são aplicados automaticamente conforme você os seleciona.
* Além disso, também na Pesquisa do Application Insights, agora há uma opção para ir para o código diretamente da telemetria de solicitação.
* Fizemos aprimoramentos à experiência de entrada do HockeyApp.
* Nas Ferramentas de Diagnóstico, as informações de telemetria de produção para exceções são exibidas.

## Versão 5.2
Estamos felizes em anunciar a introdução de cenários do HockeyApp no Visual Studio. A primeira integração está em distribuição beta dos aplicativos Universais do Windows e do Windows Forms no Visual Studio.

Com a distribuição beta, você pode carregar versões anteriores dos aplicativos no HockeyApp para distribuição a um subconjunto seleto de clientes ou testadores. Essa distribuição, combinada com os recursos de comentários dos usuário e coleção de falhas do HockeyApp, pode fornecer a você informações valiosas sobre o aplicativo antes de fazer um amplo lançamento. Você pode usar essas informações para solucionar problemas com seu aplicativo para poder evitar ou minimizar problemas futuros, como classificações de aplicativo baixas, comentários negativos e assim por diante.

Confira como é simples carregar compilações na distribuição beta no Visual Studio.

### Aplicativos universais Windows
O menu de contexto do nó de um projeto de aplicativo Universal do Windows agora inclui uma opção para carregar sua compilação no HockeyApp.

![Menu de contexto do projeto para Aplicativos Universais do Windows](./media/app-insights-release-notes-vsix/UniversalContextMenu.png)

Escolha o item e a caixa de diálogo de carregamento do HockeyApp será exibida. Você precisará de uma conta do HockeyApp para carregar sua compilação. Se você é um novo usuário, não se preocupe. Criar uma conta é um processo simples.

Ao se conectar, você verá o formulário de carregamento no diálogo.

![Carregar caixa de diálogo para aplicativos Universais do Windows](./media/app-insights-release-notes-vsix/UniversalUploadDialog.png)

Escolha o conteúdo a ser carregado (um arquivo appxbundle ou appx) e escolha as opções de versão no assistente. Se preferir, você poderá adicionar notas de versão na página seguinte. Escolha **Concluir** para começar a carregar.

Quando o carregamento estiver concluído, uma notificação do sistema para o HockeyApp com confirmação e um link para o aplicativo no portal do HockeyApp serão exibidos.

![Carregar a notificação de conclusão](./media/app-insights-release-notes-vsix/UploadComplete.png)

É isso! Você acabou de carregar uma compilação para distribuição Beta com apenas alguns cliques.

Você pode gerenciar seu aplicativo de várias maneiras no portal do HockeyApp. Isso inclui convidar usuários, exibir relatórios de falha e comentários, alterar os detalhes e assim por diante.

![Portal do HockeyApp](./media/app-insights-release-notes-vsix/HockeyAppPortal.png)

Confira a [Base de Dados de Conhecimento do HockeyApp](http://support.hockeyapp.net/kb/app-management-2) para obter mais detalhes sobre o gerenciamento de aplicativo.

### Aplicativos do Windows Forms
O menu de contexto do nó de um projeto do Windows Forms agora inclui uma opção para carregar sua compilação no HockeyApp.

![Menu de contexto do projeto para aplicativos do Windows Forms](./media/app-insights-release-notes-vsix/WinFormContextMenu.png)

Isso abre o diálogo de carregamento do HockeyApp, que é semelhante ao de um aplicativo universal do Windows.

![Caixa de diálogo Upload para aplicativos do Windows Forms](./media/app-insights-release-notes-vsix/WinFormsUploadDialog.png)

Observe um novo campo nesse assistente para especificar a versão do aplicativo. Para aplicativos universais do Windows, as informações são preenchidas com o manifesto. Aplicativos do Windows Forms, infelizmente, não têm um equivalente desse recurso. Você precisará especificá-los manualmente.

O restante do fluxo é semelhante aos Aplicativos Universais do Windows: escolha opções de versão e lançamento, adição de notas de versão, carregamento e gerenciamento no portal do HockeyApp.

É simples assim. Experimente e conte-nos o que você acha.

## Versão 4.3
### Pesquisar a telemetria nas sessões de depuração local
Com esta versão, você pode pesquisar a telemetria do Application Insights gerada na sessão de depuração do Visual Studio. Antes, você só podia usar a pesquisa se tivesse registrado seu aplicativo no Application Insights. Agora, seu aplicativo precisa apenas ter o SDK do Application Insights instalado para pesquisar a telemetria local.

Se você tem um aplicativo ASP.NET com o SDK do Application Insights, execute as etapas a seguir para usar a pesquisa.

1. Depure o aplicativo.
2. Abra a Pesquisa do Application Insights com uma das seguintes maneiras:
   * No menu **Exibição**, clique em **Outras Janelas** e em **Pesquisa do Application Insights**.
   * Clicar no botão de barra de ferramentas do **Application Insights**.
   * No Gerenciador de Soluções, expanda **ApplicationInsights.config** e clique em **Pesquisar telemetria da sessão de depuração**.
3. Se ainda não tiver se inscrito no Application Insights, a janela Pesquisar será aberta no modo de telemetria da sessão de depuração.
4. Clique no ícone **Pesquisar** para ver a telemetria local.

![Upload concluído](./media/app-insights-release-notes-vsix/LocalSearch.png)

## Versão 4.2
Nesta versão, adicionamos recursos para facilitar a pesquisa de dados no contexto de eventos, com a capacidade de saltar para o código de mais eventos de dados e uma experiência simples para enviar dados de log para o Application Insights. Essa extensão é atualizada mensalmente. Se você tiver comentários ou solicitações de recurso, envie-os para aidevtools@microsoft.com.

### Experiência de registro em log sem cliques
Se você já estiver usando System.Diagnostics.Tracing, log4net ou NLog, não precisará se preocupar com mover todos os rastreamentos para o Application Insights. Nesta versão, integramos os adaptadores de registro em log do Application Insights à experiência de configuração normal. Se você já tiver uma dessas estruturas de registros de log configuradas, a seção a seguir descreverá como obtê-la. **Se já tiver adicionado o Application Insights:**

1. Clique com botão direito do mouse no nó do projeto e clique em **Application Insights**. Em seguida, clique em **Configurar Application Insights**. Verifique se aparece a opção de adicionar o adaptador correto à janela de configuração.
2. Com alternativa, quando você compilar a solução, observe a janela pop-up que aparece no canto superior direito da tela e clique em **Configurar**.

![Notificação de registro em log](./media/app-insights-release-notes-vsix/LoggingToast.png)

Depois de instalar o adaptador de log, execute o aplicativo e verificar se visualiza os dados na guia de ferramentas de diagnóstico, da seguinte forma:

![Rastreamentos](./media/app-insights-release-notes-vsix/Traces.png)

### Salte até ou localize o código em que a propriedade de evento de telemetria é emitida
Com a nova versão, o usuário pode clicar em qualquer valor nos detalhes do evento e isso pesquisará uma cadeia de caracteres correspondente na solução aberta atual. Os resultados serão exibidos na lista "Encontrar Resultados" do Visual Studio, conforme mostrado abaixo:

![Localizar correspondência](./media/app-insights-release-notes-vsix/FindMatch.png)

### Nova janela de pesquisa para quando você não está conectado
Melhoramos a aparência da janela da Pesquisa do Application Insights para ajudá-lo a procurar os dados enquanto o aplicativo está em produção.

![Janela de pesquisa](./media/app-insights-release-notes-vsix/SearchWindow.png)

### Veja todos os eventos de telemetria associados ao evento
Adicionamos uma nova guia com consultas predefinidas para todos os dados relacionados ao evento de telemetria que o usuário está exibindo, ao lado da guia para obter detalhes do evento. Por exemplo, uma solicitação tem um campo chamado **ID da Operação**. Todos os eventos associados a essa solicitação têm o mesmo valor para **ID da Operação**. Se ocorrer uma exceção enquanto a operação está processando a solicitação, ela recebe a mesma ID de operação que a solicitação para facilitar a localização. Se você estiver vendo uma solicitação, clique em **Toda a telemetria para esta operação** para abrir uma nova guia que exibe os novos resultados da pesquisa.

![Itens relacionados](./media/app-insights-release-notes-vsix/RelatedItems.png)

### Avançar e Voltar histórico na Pesquisa
Agora você pode ir e voltar entre os resultados da pesquisa.

![Voltar](./media/app-insights-release-notes-vsix/GoBAck.png)

## Versão 4.1
Esta versão vem com uma quantidade de novos recursos e atualizações. Você precisa ter a Atualização 1 instalada para instalar esta versão.

### Saltar de uma exceção para o método no código-fonte
Agora, se você exibir as exceções do seu aplicativo de produção na janela da Pesquisa do Application Insights, poderá ir para o método em seu código onde a exceção está ocorrendo. Você só precisa ter o projeto correto carregado e o Application Insights cuida do resto! (Para saber mais sobre a janela da Pesquisa do Application Insights, confira as notas de versão para a versão 4.0 nas seções a seguir.)

Como ele funciona? Você poderá usar a Pesquisa do Applications Insights mesmo quando uma solução não estiver aberta. A área de rastreamento de pilha exibe uma mensagem informativa e muitos dos itens no rastreamento da pilha não ficam disponíveis.

Se as informações de arquivo estiverem disponíveis, alguns itens poderão ser links, mas o item de informação de solução ainda estará visível.

Se você clicar no hiperlink, poderá saltar para o local do método selecionado em seu código. Pode haver uma diferença no número de versão, mas o recurso, para ir para a versão correta do código, será apresentado em versões posteriores.

![Clicar em detalhes da exceção](./media/app-insights-release-notes-vsix/jumptocode.png)

### Novos pontos de entrada para a experiência de pesquisa no Gerenciador de Soluções
Agora você pode acessar a Pesquisa usando o Gerenciador de Soluções.

![Pesquisar no Gerenciador de Soluções](./media/app-insights-release-notes-vsix/searchentry.png)

### Exibe uma notificação quando a publicação é concluída
Uma caixa de diálogo pop-up será exibida depois que o projeto for publicado online para que você possa exibir os dados do Application Insights em produção.

![Publicar notificação de conclusão](./media/app-insights-release-notes-vsix/publishtoast.png)

## Versão 4.0
### Pesquise dados do Application Insights no Visual Studio
Assim como a função de pesquisa no portal do Application Insights, agora no Visual Studio você pode filtrar e pesquisar tipos de eventos, valores de propriedade e texto e inspecionar eventos individuais.

![Janela de pesquisa](./media/app-insights-release-notes-vsix/search.png)

### Confira dados provenientes do computador local nas Ferramentas de Diagnóstico
Você pode exibir sua telemetria, além de outros dados de depuração, na página Ferramentas de Diagnóstico do Visual Studio. Há suporte apenas para ASP.NET 4.5.

![Página Ferramentas de Diagnóstico](./media/app-insights-release-notes-vsix/diagtools.png)

### Adicione o SDK ao seu projeto sem entrar no Azure
Você não precisa mais entrar no Azure para adicionar pacotes do Application Insights ao seu projeto, seja na caixa de diálogo **Novo Projeto** ou no menu de contexto do projeto. Se você entrar, o SDK será instalado e configurado para enviar telemetria ao portal, como antes. Se você não entrar, o SDK será adicionado ao seu projeto e irá gerar telemetria para o hub de diagnóstico. Você pode configurá-lo mais tarde, se desejar.

![Caixa de diálogo Novo Projeto](./media/app-insights-release-notes-vsix/newproject.png)

### Suporte a dispositivos
Em *Connect();* 2015, [anunciamos](https://azure.microsoft.com/blog/deep-diagnostics-for-web-apps-with-application-insights/) que a nossa experiência de desenvolvedores para dispositivos móveis é o HockeyApp. O HockeyApp ajuda a distribuir suas versões beta para testadores, a coletar e a analisar todas as falhas do aplicativo e a coletar comentários diretamente de seus clientes. O HockeyApp dá suporte a seu aplicativo em qualquer plataforma que você escolher para compilá-lo, seja iOS, Android ou Windows, ou uma solução de plataforma cruzada, como Unity, Cordova ou Xamarin.

Em versões futuras da extensão Application Insights, vamos introduzir uma experiência mais integrada entre o HockeyApp e o Visual Studio. Por enquanto, você pode começar a usar o HockeyApp simplesmente adicionando a referência do NuGet. Para saber mais, confira a [documentação](http://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone).

<!---HONumber=AcomDC_0727_2016-->