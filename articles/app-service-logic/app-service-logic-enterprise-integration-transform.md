---
title: "Visão geral do Enterprise Integration Pack | Microsoft Docs"
description: "Usar os recursos do Enterprise Integration Pack para habilitar cenários de integração e o processo de negócios usando o Serviço de Aplicativo do Microsoft Azure"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: erikre
editor: cgronlun
ms.assetid: add01429-21bc-4bab-8b23-bc76ba7d0bde
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: deonhe
translationtype: Human Translation
ms.sourcegitcommit: 6de16a062a97787b37fae88a660a7ee54741910c
ms.openlocfilehash: 3d1949ea8c842c906c6f898dbd194fccb329428b


---
# <a name="enterprise-integration-with-xml-transforms"></a>Integração corporativa com transformações XML
## <a name="overview"></a>Visão geral
O conector de Transformação da integração corporativa converte dados de um formato para outro formato. Por exemplo, talvez você tenha uma mensagem de entrada contendo a data atual no formato AnoMêsDia. Você pode usar uma transformação para reformatar a data para o formato MêsDiaAno.

## <a name="what-does-a-transform-do"></a>O que uma transformação faz?
Uma Transformação, que também é conhecida como um mapa, é formada por um esquema XML de origem (entrada) e um esquema XML de destino (saída). Você pode usar funções internas diferentes para ajudar a manipular e controlar os dados, incluindo manipulações de cadeia de caracteres, atribuições condicionais, expressões aritméticas, formatadores do tempo de data e até mesmo construções em loop.

## <a name="how-to-create-a-transform"></a>Como criar uma transformação?
Você pode criar uma transformação/mapa usando o [SDK do Enterprise Integration](https://aka.ms/vsmapsandschemas)do Visual Studio. Após a conclusão da criação e do teste da transformação, carregue a transformação em sua conta de integração. 

## <a name="how-to-use-a-transform"></a>Como usar uma transformação
Depois de carregar a transformação em sua conta de integração, você poderá usá-la para criar um Aplicativo lógico. O Aplicativo lógico executará suas transformações sempre que o Aplicativo lógico for acionado (e quando houver um conteúdo de entrada que exija transformação).

**Estas são as etapas para usar uma transformação**:

### <a name="prerequisites"></a>Pré-requisitos

* Criar uma conta de integração e adicionar um mapa a ela  

Agora que você cuidou dos pré-requisitos, é hora de criar seu Aplicativo lógico:  

1. Crie um Aplicativo Lógico e [vincule-o à sua conta de integração](app-service-logic-enterprise-integration-accounts.md "Saiba como vincular uma conta de integração a um Aplicativo lógico") que contém o mapa.
2. Adicione um gatilho de **Solicitação** a seu Aplicativo lógico  
   ![](./media/app-service-logic-enterprise-integration-transforms/transform-1.png)    
3. Adicione a ação **Transformar XML** selecionando primeiro **Adicionar uma ação**   
   ![](./media/app-service-logic-enterprise-integration-transforms/transform-2.png)   
4. Insira a palavra *transformação* na caixa de pesquisa para filtrar todas as ações para encontrar aquela que você deseja usar  
   ![](./media/app-service-logic-enterprise-integration-transforms/transform-3.png)  
5. Selecione a ação **Transformar XML**   
6. Adicione o **CONTEÚDO** XML que você transformar. Você pode usar quaisquer dados XML recebidos na solicitação HTTP como o **CONTEÚDO**. Neste exemplo, selecione o corpo da solicitação HTTP que disparou o Aplicativo Lógico.
7. Selecione o nome do **MAPA** que você deseja usar para executar a transformação. O mapa já deve estar em sua conta de integração. Em uma etapa anterior, você já deu ao seu Aplicativo lógico acesso à sua conta de integração que contém o mapa.      
   ![](./media/app-service-logic-enterprise-integration-transforms/transform-4.png) 
8. Salve seu trabalho   
    ![](./media/app-service-logic-enterprise-integration-transforms/transform-5.png) 

Neste ponto, você já configurou seu mapa. Em um aplicativo real, convém armazenar os dados transformados em um aplicativo LOB, como o SalesForce. Você pode adicionar facilmente uma ação para enviar a saída da transformação para o Salesforce. 

Agora você pode testar a ação de transformação fazendo uma solicitação ao ponto de extremidade HTTP.  

## <a name="features-and-use-cases"></a>Recursos e casos de uso
* A transformação criada em um mapa pode ser simples, como copiar um nome e endereço de um documento para outro. Ou você pode criar transformações mais complexas usando as operações de mapa prontas para uso.  
* Várias operações ou funções de mapeamento estão disponíveis, incluindo cadeias de caracteres, funções de data e hora, e assim por diante.  
* Você pode fazer uma cópia de dados direta entre os esquemas. No Mapeador incluído no SDK, isso é tão simples quanto desenhar uma linha que conecta os elementos no esquema de origem aos seus correspondentes no esquema de destino.  
* Ao criar um mapa, você exibe uma representação gráfica do mapa, que mostra todas as relações e os links que você cria.
* Use o recurso Testar Mapa para adicionar uma mensagem XML de exemplo. Com um simples clique, você pode testar o mapa que você criou e ver a saída gerada.  
* Carregar mapas existentes  
* Inclui suporte para o formato XML.

## <a name="learn-more"></a>Saiba mais
* [Saiba mais sobre o Enterprise Integration Pack](app-service-logic-enterprise-integration-overview.md "Saiba mais sobre o Enterprise Integration Pack")  
* [Saiba mais sobre mapas](app-service-logic-enterprise-integration-maps.md "Saiba mais sobre mapas da integração corporativa")  




<!--HONumber=Dec16_HO3-->


