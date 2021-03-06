---
title: Exemplos de Node.js NoSQL para o Banco de Dados de Documentos | Microsoft Docs
description: "Encontre exemplos de Node.js NoSQL no github para tarefas comuns do Banco de Dados de Documentos, incluindo operações CRUD para documentos JSON em bancos de dados NoSQL."
keywords: Exemplos do Node.js
services: documentdb
author: moderakh
manager: jhubbard
editor: monicar
documentationcenter: nodejs
ms.assetid: d87d97be-47a5-4928-8d46-a541fbb33213
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2016
ms.author: moderakh
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 9a74b023c12f77fccdd989fa8f7ffa7b2ac83db0


---
# <a name="documentdb-nodejs-examples"></a>Exemplos de Node.js para o Banco de Dados de Documentos
> [!div class="op_single_selector"]
> * [Exemplos do .NET](documentdb-dotnet-samples.md)
> * [Exemplos do Node.js](documentdb-nodejs-samples.md)
> * [Exemplos do Python](documentdb-python-samples.md)
> * [Galeria de Exemplos de Código do Azure](https://azure.microsoft.com/documentation/samples/?service=documentdb)
> 
> 

Soluções de exemplo que executam operações CRUD e outras operações comuns em recursos do Banco de Dados de Documentos do Azure estão incluídas no repositório GitHub [azure-documentdb-nodejs](https://github.com/Azure/azure-documentdb-node/tree/master/samples) . Esse artigo fornece:

* Links para as tarefas em cada um dos arquivos de exemplo do projeto Node.js.
* Links para o conteúdo de referência da API relacionada.

**Pré-requisitos**

1. Você precisa de uma conta do Azure para usar esses exemplos de Node.js:
   * Você pode [abrir uma conta do Azure gratuitamente](https://azure.microsoft.com/pricing/free-trial/): você recebe créditos que podem ser usados para experimentar serviços pagos do Azure e, mesmo após eles serem utilizados, você pode manter a conta e usar os serviços gratuitos do Azure, como os Sites. Seu cartão de crédito nunca será cobrado, a menos que você altere explicitamente suas configurações, solicitando esse tipo de cobrança.
     * É possível [ativar os benefícios para assinantes do Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/): todos os meses, sua assinatura do Visual Studio concede créditos que podem ser usados para experimentar serviços pagos do Azure.
2. Também é necessário ter o [SDK do Node.js](documentdb-sdk-node.md).
   
   > [!NOTE]
   > Cada exemplo é independente, eles se configuram e fazem a limpeza sozinhos. Assim, as amostras emitem várias chamadas para [DocumentClient.createCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createCollection). Cada vez que isso é feito, sua assinatura será cobrada por 1 hora de uso por nível de desempenho da coleção que está sendo criada.
   > 
   > 

## <a name="database-examples"></a>Exemplos de banco de dados
O arquivo [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/DatabaseManagement/app.js) do projeto [DatabaseManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/DatabaseManagement) mostra como executar as tarefas a seguir.

| Tarefa | Referência de API |
| --- | --- |
| [Criar um banco de dados](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L121-L131) |[DocumentClient.createDatabase](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createDatabase) |
| [Consultar uma conta para um banco de dados](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L146-L171) |[DocumentClient.queryDatabases](http://azure.github.io/azure-documentdb-node/DocumentClient.html#queryDatabases) |
| [Ler um banco de dados por ID](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L89-L99) |[DocumentClient.readDatabase](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDatabase) |
| [Listar bancos de dados para uma conta](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L111-L119) |[DocumentClient.readDatabases](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDatabase) |
| [Excluir um banco de dados](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L133-L144) |[DocumentClient.deleteDatabase](http://azure.github.io/azure-documentdb-node/DocumentClient.html#deleteDatabase) |

## <a name="collection-examples"></a>Exemplos de coleção
O arquivo [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/CollectionManagement/app.js) do projeto [CollectionManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/CollectionManagement) mostra como executar as tarefas a seguir.

| Tarefa | Referência de API |
| --- | --- |
| [Criar uma coleção](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L97-L118) |[DocumentClient.createCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createCollection) |
| [Ler uma lista de todas as coleções em um banco de dados](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L120-L130) |[DocumentClient.readCollections](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readCollections) |
| [Obter um collection by _self](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L132-L141) |[DocumentClient.readCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readCollection) |
| [Obter uma coleção por ID](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L143-L156) |[DocumentClient.readCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readCollection) |
| [Obter o nível de desempenho de uma coleção](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L158-L186) |[DocumentQueryable.queryOffers](http://azure.github.io/azure-documentdb-node/DocumentClient.html#queryOffers) |
| [Alterar o nível de desempenho de uma coleção](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L188-L202) |[DocumentClient.replaceOffer](http://azure.github.io/azure-documentdb-node/DocumentClient.html#replaceOffer) |
| [Excluir uma coleção](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L204-L215) |[DocumentClient.deleteCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#deleteCollection) |

## <a name="document-examples"></a>Exemplos de documento
O arquivo [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/DocumentManagement/app.js) do projeto [DocumentManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/DocumentManagement) mostra como executar as tarefas a seguir.

| Tarefa | Referência de API |
| --- | --- |
| [Criar documentos](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L153-L177) |[DocumentClient.createDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createDocument) |
| [Ler a alimentação de documento para uma coleção](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L179-L189) |[DocumentClient.readDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDocument) |
| [Ler um documento por ID](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L191-L201) |[DocumentClient.readDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDocument) |
| [Ler apenas o documento se ele tiver sido alterado](https://github.com/Azure/azure-documentdb-node/blob/0778eadea7abb2af41e8c22a239dc872c584f421/samples/DocumentManagement/app.js#L79-L107) |[DocumentClient.readDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDocument)<br/>[RequestOptions.accessCondition](http://azure.github.io/azure-documentdb-node/global.html#RequestOptions) |
| [Consulta de documentos](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L82-L110) |[DocumentClient.queryDocuments](http://azure.github.io/azure-documentdb-node/DocumentClient.html#queryDocuments) |
| [Substituir um documento](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L112-L119) |[DocumentClient.replaceDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#replaceDocument) |
| [Substituir o documento com a verificação de Etag condicional](https://github.com/Azure/azure-documentdb-node/blob/0778eadea7abb2af41e8c22a239dc872c584f421/samples/DocumentManagement/app.js#L147-L164) |[DocumentClient.replaceDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#replaceDocument)<br/>[RequestOptions.accessCondition](http://azure.github.io/azure-documentdb-node/global.html#RequestOptions) |
| [Excluir um documento](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L122-L133) |[DocumentClient.deleteDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#deleteDocument) |

## <a name="indexing-examples"></a>Exemplos de indexação
O arquivo [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/IndexManagement/app.js) do projeto [IndexManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/IndexManagement) mostra como executar as tarefas a seguir.

| Tarefa | Referência de API |
| --- | --- |
| Criar uma coleção com indexação padrão |[DocumentClient.createDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html) |
| [Indexar manualmente um documento específico](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L185-L238) |[indexingDirective: 'include'](http://azure.github.io/azure-documentdb-node/global.html#indexingDirective) |
| [Excluir manualmente um documento específico do índice](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L120-L183) |[RequestOptions.indexingDirective](http://azure.github.io/azure-documentdb-node/global.html#RequestOptions) |
| [Usar a indexação lenta para importação em massa ou ler coleções pesadas](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L240-L269) |[IndexingMode.Lazy](http://azure.github.io/azure-documentdb-node/global.html#IndexingMode) |
| [Incluir caminhos específicos de um documento na indexação](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L433-L444) |[IndexingPolicy.IncludedPaths](http://azure.github.io/azure-documentdb-node/global.html#IndexingPolicy) |
| [Excluir alguns caminhos de indexação](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L427-L450) |[ExcludedPath](http://azure.github.io/azure-documentdb-node/global.html#IndexingPolicy) |
| [Permitir uma verificação em um caminho de cadeia de caracteres durante uma operação de intervalo](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L271-L347) |[ExcludedPath.EnableScanInQuery](http://azure.github.io/azure-documentdb-node/global.html#FeedOptions) |
| [Criar um índice de intervalo em um caminho de cadeia de caracteres](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L349-L425) |[DocumentClient.queryDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#queryDocument) |
| [Criar uma coleção com indexPolicy padrão, em seguida, atualizá-la online](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L519-L614) |[DocumentClient.createCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createCollection)<br> [DocumentClient.replaceCollection#replaceCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html) |

Para saber mais sobre indexação, veja [Políticas de indexação do Banco de Dados de Documentos](documentdb-indexing-policies.md).

## <a name="server-side-programming-examples"></a>Exemplos de programação do lado do servidor
O arquivo [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/ServerSideScripts/app.js) do projeto [ServerSideScripts](https://github.com/Azure/azure-documentdb-node/tree/master/samples/ServerSideScripts) mostra como executar as tarefas a seguir.

| Tarefa | Referência de API |
| --- | --- |
| [Criar um procedimento armazenado](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.ServerSideScripts/app.js#L44-L71) |[DocumentClient.createStoredProcedure](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createStoredProcedure) |
| [Executar um procedimento armazenado](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.ServerSideScripts/app.js#L73-L90) |[DocumentClient.executeStoredProcedure](http://azure.github.io/azure-documentdb-node/DocumentClient.html#executeStoredProcedure) |

Para saber mais sobre a programação do servidor, veja [Programação no servidor do Banco de Dados de Documentos: UDFs, gatilhos de banco de dados e procedimentos armazenados](documentdb-programming.md).

## <a name="partitioning-examples"></a>Exemplos de particionamento
O arquivo [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/Partitioning/app.js) do projeto [Partitioning](https://github.com/Azure/azure-documentdb-node/tree/master/samples/Partitioning) mostra como executar as tarefas a seguir.

| Tarefa | Referência de API |
| --- | --- |
| [Usar um HashPartitionResolver](https://github.com/Azure/azure-documentdb-node/blob/ce0fc3c4e70b0279091a1e03620a668d93a14fc2/samples/Partitioning/app.js#L53-L103) |[HashPartitionResolver](http://azure.github.io/azure-documentdb-node/HashPartitionResolver.html) |

Para saber mais sobre o particionamento de dados no Banco de Dados de Documentos, veja [Particionamento e dimensionamento no Banco de Dados de Documentos do Azure](documentdb-partition-data.md).




<!--HONumber=Nov16_HO3-->


