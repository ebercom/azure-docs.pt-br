---
title: Mover dados do Sybase | Microsoft Docs
description: Saiba mais sobre como mover dados do banco de dados Sybase usando o Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: jhubbard
editor: monicar

ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/20/2016
ms.author: jingwang

---
# Mover dados do Sybase usando o Azure Data Factory
Este artigo descreve como você pode usar a atividade de cópia em um Azure Data Factory para mover dados do Sybase para outro armazenamento de dados. Este artigo se baseia no artigo [atividades de movimentação de dados](data-factory-data-movement-activities.md), que apresenta uma visão geral de movimentação de dados com a atividade de cópia e combinações de armazenamento de dados com suporte.

O serviço Data Factory dá suporte à conexão com fontes Sybase locais usando o Gateway de Gerenciamento de Dados. Consulte o artigo [movendo dados entre pontos locais e na nuvem](data-factory-move-data-between-onprem-and-cloud.md) para saber mais sobre o Gateway de gerenciamento de dados e obter instruções passo a passo de como configurar o gateway.

> [!NOTE]
> O gateway é requerido mesmo que o banco de dados Sybase esteja hospedado em uma VM IaaS do Azure. Você pode instalar o gateway na mesma VM IaaS do armazenamento de dados ou em uma VM diferente, desde que o gateway possa conectar o banco de dados.
> 
> 

Atualmente, o data factory dá suporte apenas para a movimentação de dados do Sybase para outros armazenamentos de dados, não de outros armazenamentos de dados para o Sybase.

## Instalação
Para o Gateway de Gerenciamento de Dados para se conectar ao banco de dados Sybase, você precisa instalar o [provedor de dados para Sybase](http://go.microsoft.com/fwlink/?linkid=324846) no mesmo sistema que o Gateway de Gerenciamento de Dados.

> [!NOTE]
> Consulte [Solucionar problemas de gateway](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) para ver dicas sobre como solucionar os problemas relacionados à conexão/gateway.
> 
> 

## Assistente de cópia de dados
A maneira mais fácil de criar um pipeline que copie dados de um banco de dados Sybase para qualquer um dos repositórios de dados compatíveis é usar o Assistente de cópia de dados. Confira [Tutorial: Criar um pipeline usando o Assistente de Cópia](data-factory-copy-data-wizard-tutorial.md) para ver um breve passo a passo sobre como criar um pipeline usando o Assistente de cópia de dados.

O exemplo a seguir fornece as definições de JSON de exemplo que você pode usar para criar um pipeline usando o [portal do Azure](data-factory-copy-activity-tutorial-using-azure-portal.md), o [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) ou o [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Eles mostram como copiar dados do banco de dados Sybase para o Armazenamento de Blobs do Azure. No entanto, os dados podem ser copiados para qualquer um dos coletores declarados [aqui](data-factory-data-movement-activities.md#supported-data-stores) usando a Atividade de Cópia no Azure Data Factory.

## Exemplo: copiar dados do Sybase para Blob do Azure
Este exemplo mostra como copiar dados de um banco de dados Sybase para um Armazenamento de Blobs do Azure. No entanto, os dados podem ser copiados **diretamente** para qualquer uma das fontes declaradas [aqui](data-factory-data-movement-activities.md#supported-data-stores) usando a atividade de cópia no Azure Data Factory.

O exemplo tem as seguintes entidades de data factory:

1. Um serviço vinculado do tipo [OnPremisesSybase](data-factory-onprem-sybase-connector.md#sybase-linked-service-properties).
2. Um serviço vinculado do tipo [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3. Um [conjunto de dados](data-factory-create-datasets.md) de entrada do tipo [RelationalTable](data-factory-onprem-sybase-connector.md#sybase-dataset-type-properties).
4. Um [conjunto de dados](data-factory-create-datasets.md) do tipo [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
5. O [pipeline](data-factory-create-pipelines.md) com atividade de cópia que usa [RelationalSource](data-factory-onprem-sybase-connector.md#sybase-copy-activity-type-properties) e [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

O exemplo copia dados de um resultado de consulta no banco de dados Sybase para um blob a cada hora. As propriedades JSON usadas nesses exemplos são descritas nas seções após os exemplos.

Como uma primeira etapa, configure o gateway de gerenciamento de dados. As instruções estão no artigo [Mover dados entre fontes locais e a nuvem](data-factory-move-data-between-onprem-and-cloud.md).

**Serviço vinculado Sybase:**

    {
        "name": "OnPremSybaseLinkedService",
        "properties": {
            "type": "OnPremisesSybase",
            "typeProperties": {
                "server": "<server>",
                "database": "<database>",
                "schema": "<schema>",
                "authenticationType": "<authentication type>",
                "username": "<username>",
                "password": "<password>",
                "gatewayName": "<gatewayName>"
            }
        }
    }

**Serviço vinculado do armazenamento de Blob do Azure:**

    {
        "name": "AzureStorageLinkedService",
        "properties": {
            "type": "AzureStorageLinkedService",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"
            }
        }
    }


**Conjunto de dados de entrada do Sybase:**

O exemplo supõe que você criou uma tabela "MyTable" no Sybase e que ela contém uma coluna chamada "timestamp" para dados de série temporal.

Configurar "external": true informa ao serviço Data Factory que o conjunto de dados é externo ao Data Factory e não é produzido por uma atividade no Data Factory. Observe que o **tipo** do serviço vinculado está definido para: **RelationalTable**.

    {
        "name": "SybaseDataSet",
        "properties": {
            "type": "RelationalTable",
            "linkedServiceName": "OnPremSybaseLinkedService",
            "typeProperties": {},
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }


**Conjunto de dados de saída de Blob do Azure:**

Os dados são gravados em um novo blob a cada hora (frequência: hora, intervalo: 1). O caminho de pasta para o blob é avaliado dinamicamente com base na hora de início da fatia que está sendo processada. O caminho da pasta usa as partes ano, mês, dia e horas da hora de início.

    {
        "name": "AzureBlobSybaseDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/sybase/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                },
                "partitionedBy": [
                    {
                        "name": "Year",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "yyyy"
                        }
                    },
                    {
                        "name": "Month",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "MM"
                        }
                    },
                    {
                        "name": "Day",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "dd"
                        }
                    },
                    {
                        "name": "Hour",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "HH"
                        }
                    }
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }


**Pipeline com Atividade de cópia:**

O pipeline contém uma Atividade de Cópia configurada para usar os conjuntos de dados de entrada e saída, e está agendada para ser executada por hora. Na definição JSON do pipeline, o tipo **source** está definido como **RelationalSource** e o tipo **sink** está definido como **BlobSink**. A consulta SQL especificada para a propriedade **query** seleciona os dados da tabela DBA.Orders no banco de dados.

    {
        "name": "CopySybaseToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "select * from DBA.Orders"
                        },
                        "sink": {
                            "type": "BlobSink"
                        }
                    },
                    "inputs": [
                        {
                            "name": "SybaseDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobSybaseDataSet"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "SybaseToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }


## Propriedades do serviço vinculado Sybase
A tabela a seguir fornece a descrição para elementos JSON específicos para o serviço vinculado Sybase.

| Propriedade | Descrição | Obrigatório |
| --- | --- | --- |
| type |A propriedade do tipo deve ser definida como: **OnPremisesSybase** |Sim |
| server |Nome do servidor do Sybase. |Sim |
| database |Nome do banco de dados do Sybase. |Sim |
| schema |Nome do esquema no banco de dados. |Não |
| authenticationType |Tipo de autenticação usado para se conectar ao banco de dados Sybase. Os valores possíveis são: Anonymous, Basic e Windows. |Sim |
| Nome de Usuário |Especifique o nome de usuário se você estiver usando a autenticação Basic ou Windows. |Não |
| Senha |Especifique a senha da conta de usuário que você especificou para o nome de usuário. |Não |
| gatewayName |O nome do gateway que o serviço Data Factory deve usar para se conectar ao banco de dados local do Sybase. |Sim |

Consulte [Definir credenciais e segurança](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) para obter detalhes sobre como definir credenciais para uma fonte de dados do Sybase local.

## Propriedades de tipo do banco de dados do Sybase
Para obter uma lista completa das seções e propriedades disponíveis para definir conjuntos de dados, consulte o artigo [Criando conjuntos de dados](data-factory-create-datasets.md). As seções como structure, availability e policy de um conjunto de dados JSON são similares para todos os tipos de conjunto de dados (SQL Azure, Blob do Azure, Tabela do Azure etc.).

A seção typeProperties é diferente para cada tipo de conjunto de dados e fornece informações sobre o local dos dados no armazenamento de dados. A seção **typeProperties** de um conjunto de dados do tipo **RelationalTable** (que inclui o conjunto de dados Sybase) tem as propriedades a seguir:

| Propriedade | Descrição | Obrigatório |
| --- | --- | --- |
| tableName |Nome da tabela na instância do banco de dados Sybase à qual o serviço vinculado se refere. |Não (se **query** de **RelationalSource** for especificado) |

## Propriedades de tipo de atividade de cópia do Sybase
Para obter uma lista completa das seções e propriedades disponíveis para definir as atividades, consulte o artigo [Criando Pipelines](data-factory-create-pipelines.md). As propriedades, como nome, descrição, tabelas de entrada e saída, e política, estão disponíveis para todos os tipos de atividades.

As propriedades disponíveis na seção typeProperties da atividade, por outro lado, variam de acordo com cada tipo de atividade. Para a atividade de cópia, elas variam de acordo com os tipos de fonte e coletor.

Quando a fonte é do tipo **RelationalSource** (que inclui o Sybase), as seguintes propriedades estão disponíveis na seção **typeProperties**:

| Propriedade | Descrição | Valores permitidos | Obrigatório |
| --- | --- | --- | --- |
| query |Utiliza a consulta personalizada para ler os dados. |Cadeia de caracteres de consulta SQL. Por exemplo: select * from MyTable. |Não (se **tableName** de **dataset** for especificado) |

[!INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## Mapeamento de tipo para Sybase
Como mencionado no artigo sobre as [Atividades de Movimentação de Dados](data-factory-data-movement-activities.md), a atividade de Cópia executa conversões automáticas dos tipos de fonte nos tipos de coletor com a seguinte abordagem de duas etapas:

1. Converter de tipos de fonte nativos para o tipo .NET
2. Converter do tipo .NET para o tipo de coletor nativo

Sybase dá suporte a T-SQL e tipos T-SQL. Para uma tabela de mapeamento de tipos do SQL para tipo do .NET, veja o artigo [Conector SQL do Azure](data-factory-azure-sql-connector.md).

[!INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[!INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## Desempenho e Ajuste
Veja o [Guia de Desempenho e Ajuste da Atividade de Cópia](data-factory-copy-activity-performance.md) para saber mais sobre os principais fatores que afetam o desempenho e a movimentação de dados (Atividade de Cópia) no Azure Data Factory, além de várias maneiras de otimizar esse processo.

<!---HONumber=AcomDC_0928_2016-->