---
title: "Desenvolvendo com várias regiões no DocumentDB | Microsoft Docs"
description: "Saiba como acessar seus dados nas várias regiões do Banco de Dados de Documentos do Azure, um serviço de banco de dados NoSQL totalmente gerenciado."
services: documentdb
documentationcenter: 
author: kiratp
manager: jhubbard
editor: 
ms.assetid: d4579378-0b3a-44a5-9f5b-630f1fa4c66d
ms.service: documentdb
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/09/2016
ms.author: kipandya
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: a0b1418168d493ce0e669a9eb0594d37e741df7d


---
# <a name="developing-with-multi-region-documentdb-accounts"></a>Desenvolvendo com contas do Banco de Dados de Documentos em várias regiões
> [!NOTE]
> A distribuição global dos bancos de dados do Banco de Dados de Documentos geralmente é disponibilizada e habilitada automaticamente para contas do Banco de Dados de Documentos recentemente criadas. Estamos trabalhando para habilitar a distribuição global em todas as contas existentes, mas nesse ínterim, se quiser que a distribuição global seja habilitada para sua conta, [contate o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) e nós a habilitaremos para você agora mesmo.
>
>

Para aproveitar a [distribuição global](documentdb-distribute-data-globally.md), os aplicativos cliente podem especificar a lista de preferências ordenadas de regiões a serem usadas para executar operações de documento. Isso pode ser feito definindo a política de conexão. Com base na configuração da conta do Banco de Dados de Documentos do Azure, na disponibilidade regional atual e na lista de preferências especificada, o ponto de extremidade mais adequado será escolhido pelo SDK para executar operações de gravação e leitura.

Essa lista de preferências é especificada ao inicializar uma conexão usando os SDKs do cliente do Banco de Dados de Documentos. Os SDKs aceitam um parâmetro opcional "PreferredLocations", que é uma lista ordenada de regiões do Azure.

O SDK enviará automaticamente todas as gravações para a região de gravação atual.

Todas as leituras serão enviadas para a primeira região disponível na lista PreferredLocations. Se a solicitação falhar, o cliente não fará o envio para a próxima região da lista, e assim por diante.

Os SDKs do cliente tentarão ler apenas das regiões especificadas em PreferredLocations. Desse modo, se a Conta do Banco de Dados estiver disponível em três regiões, por exemplo, mas o cliente especificar apenas duas das regiões de não gravação para PreferredLocations, nenhuma leitura será atendida fora da região de gravação, mesmo no caso de failover.

O aplicativo pode verificar o ponto de extremidade de gravação e o ponto de extremidade de leitura atuais escolhidos pelo SDK marcando duas propriedades, WriteEndpoint e ReadEndpoint, disponíveis no SDK versão 1.8 e superiores.

Se a propriedade PreferredLocations não estiver definida, todas as solicitações serão atendidas na região de gravação atual.

## <a name="net-sdk"></a>SDK .NET
O SDK pode ser usado sem qualquer mudança de código. Nesse caso, o SDK direciona automaticamente as leituras e gravações para a região de gravação atual.

Na versão 1.8 e posteriores do SDK para .NET, o parâmetro ConnectionPolicy do construtor DocumentClient tem uma propriedade chamada Microsoft.Azure.Documents.ConnectionPolicy.PreferredLocations. Essa propriedade é do tipo `<string>` de Coleção e deve conter uma lista de nomes de região. Os valores de cadeia de caracteres são formatados de acordo com a coluna Nome da Região na página [Regiões do Azure][regions], sem espaços antes ou depois do primeiro e último caracteres, respectivamente.

Os pontos de extremidade atuais de gravação e leitura estão disponíveis em DocumentClient.WriteEndpoint e DocumentClient.ReadEndpoint, respectivamente.

> [!NOTE]
> As URLs para os pontos de extremidade não devem ser consideradas como constantes de vida longa. O serviço pode atualizá-las a qualquer momento. O SDK lida com essa alteração automaticamente.
>
>

    // Getting endpoints from application settings or other configuration location
    Uri accountEndPoint = new Uri(Properties.Settings.Default.GlobalDatabaseUri);
    string accountKey = Properties.Settings.Default.GlobalDatabaseKey;
    
    ConnectionPolicy connectionPolicy = new ConnectionPolicy();

    //Setting read region selection preference
    connectionPolicy.PreferredLocations.Add(LocationNames.WestUS); // first preference
    connectionPolicy.PreferredLocations.Add(LocationNames.EastUS); // second preference
    connectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope); // third preference

    // initialize connection
    DocumentClient docClient = new DocumentClient(
        accountEndPoint,
        accountKey,
        connectionPolicy);

    // connect to DocDB
    await docClient.OpenAsync().ConfigureAwait(false);


## <a name="nodejs-javascript-and-python-sdks"></a>SDKs para NodeJS, JavaScript e Python
O SDK pode ser usado sem qualquer mudança de código. Nesse caso, o SDK direcionará automaticamente as leituras e gravações para a região de gravação atual.

Na versão 1.8 e posteriores de cada SDK, o parâmetro ConnectionPolicy do construtor DocumentClient tem uma nova propriedade chamada DocumentClient.ConnectionPolicy.PreferredLocations. Esse parâmetro é uma matriz de cadeias de caracteres que usa uma lista de nomes de região. Os nomes são formatados de acordo com a coluna Nome da Região na página [Regiões do Azure][regions]. Você também pode usar as constantes predefinidas no objeto de conveniência AzureDocuments.Regions

Os pontos de extremidade atuais de gravação e leitura estão disponíveis em DocumentClient.getWriteEndpoint e DocumentClient.getReadEndpoint, respectivamente.

> [!NOTE]
> As URLs para os pontos de extremidade não devem ser consideradas como constantes de vida longa. O serviço pode atualizá-las a qualquer momento. O SDK tratará dessa mudança automaticamente.
>
>

Veja abaixo um exemplo de código para NodeJS/Javascript. Python e Java seguirão o mesmo padrão.

    // Creating a ConnectionPolicy object
    var connectionPolicy = new DocumentBase.ConnectionPolicy();

    // Setting read region selection preference, in the following order -
    // 1 - West US
    // 2 - East US
    // 3 - North Europe
    connectionPolicy.PreferredLocations = ['West US', 'East US', 'North Europe'];

    // initialize the connection
    var client = new DocumentDBClient(host, { masterKey: masterKey }, connectionPolicy);


## <a name="rest"></a>REST
Assim que uma conta de banco de dados tiver sido disponibilizada em várias regiões, os clientes poderão consultar sua disponibilidade executando uma solicitação GET no URI a seguir.

    https://{databaseaccount}.documents.azure.com/

O serviço retornará uma lista de regiões e seus URIs de pontos de extremidade do Banco de Dados de Documentos correspondentes para as réplicas. A região de gravação atual será indicada na resposta. O cliente pode selecionar o ponto de extremidade apropriado para todas as solicitações adicionais de API REST como se segue.

Exemplo de resposta

    {
        "_dbs": "//dbs/",
        "media": "//media/",
        "writableLocations": [
            {
                "Name": "West US",
                "DatabaseAccountEndpoint": "https://globaldbexample-westus.documents.azure.com:443/"
            }
        ],
        "readableLocations": [
            {
                "Name": "East US",
                "DatabaseAccountEndpoint": "https://globaldbexample-eastus.documents.azure.com:443/"
            }
        ],
        "MaxMediaStorageUsageInMB": 2048,
        "MediaStorageUsageInMB": 0,
        "ConsistencyPolicy": {
            "defaultConsistencyLevel": "Session",
            "maxStalenessPrefix": 100,
            "maxIntervalInSeconds": 5
        },
        "addresses": "//addresses/",
        "id": "globaldbexample",
        "_rid": "globaldbexample.documents.azure.com",
        "_self": "",
        "_ts": 0,
        "_etag": null
    }


* Todas as solicitações All PUT, POST e DELETE devem ir para o URI de gravação indicado
* Todas as solicitações GET e outras somente leitura (por exemplo: consultas) podem ir para qualquer ponto de extremidade de escolha do cliente

As solicitações de gravação para regiões somente leitura falharão com o código de erro 403 de HTTP ("Proibido").

Se a região de gravação mudar depois da fase de descoberta inicial do cliente, as gravações subsequentes na região de gravação anterior falharão com o código de erro 403 de HTTP ("Proibido"). O cliente deve obter (GET) a lista de regiões novamente para que a região de gravação seja atualizada.

## <a name="next-steps"></a>Próximas etapas
Saiba mais sobre como distribuir dados globalmente com o Banco de Dados de Documentos nos seguintes artigos:

* [Distribute data globally with DocumentDBs (Distribuir dados globalmente com o Banco de Dados de Documentos)](documentdb-distribute-data-globally.md)
* [Níveis de consistência](documentdb-consistency-levels.md)
* [Como a produtividade funciona com várias regiões](documentdb-manage.md)
* [Adicionar regiões usando o portal do Azure](documentdb-portal-global-replication.md)

[regions]: https://azure.microsoft.com/regions/



<!--HONumber=Dec16_HO2-->


