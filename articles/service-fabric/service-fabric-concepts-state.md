---
title: Definindo e gerenciando o estado | Microsoft Docs
description: "Como definir e gerenciar o estado do serviço na malha de serviço"
services: service-fabric
documentationcenter: .net
author: appi101
manager: timlt
editor: 
ms.assetid: f5e618a5-3ea3-4404-94af-122278f91652
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/10/2016
ms.author: aprameyr
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: bb27c2f2224791a6a30819da5ee0f09ad7bfa95c


---
# <a name="service-state"></a>Estado do serviço
**Estado do serviço** refere-se aos dados que o serviço requer para funcionar. São as estruturas de dados e variáveis que o serviço lê e grava para realizar trabalhos.

Considere um serviço de calculadora simples, por exemplo. Esse serviço usa dois números e retorna a soma. Este é um serviço puramente sem monitoração de estado que não possui dados associados a ele.

Agora, considere a mesma calculadora, mas além da soma de computação, ela também conta com um método para retornar a última soma computada. Esse serviço agora é com monitoração de estado - contém algum estado que é gravado (quando calcula uma nova soma) e lido (quando retorna a última soma calculada).

No Service Fabric do Azure, o primeiro serviço é chamado de serviço sem estado. O segundo serviço é chamado de serviço com estado.

## <a name="storing-service-state"></a>Armazenando o estado do serviço
O estado pode ser externalizado ou localizado em conjunto com o código que está manipulando o estado. A externalização de estado normalmente é feita por meio de um armazenamento ou banco de dados externo. Em nosso exemplo de calculadora, isso pode ser um Banco de Dados SQL no qual o resultado atual é armazenado em uma tabela. Todas as solicitações para calcular a soma executa uma atualização nesta linha.

O estado também pode ser localizado com o código que manipula esse código. Serviços com monitoração de estado na malha de serviço são criados usando esse modelo. O Service Fabric fornece a infraestrutura para garantir que esse estado seja altamente disponível e tolerante a falhas em caso de uma falha.

## <a name="next-steps"></a>Próximas etapas
Para obter informações sobre os conceitos do Service Fabric, consulte o seguinte:

* [Disponibilidade dos serviços de malha do serviço](service-fabric-availability-services.md)
* [Escalabilidade de serviços da Malha do Serviço](service-fabric-concepts-scalability.md)
* [Particionando serviços da Malha do Serviço](service-fabric-concepts-partitioning.md)




<!--HONumber=Nov16_HO3-->


