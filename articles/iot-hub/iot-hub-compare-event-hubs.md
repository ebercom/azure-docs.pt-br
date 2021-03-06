---
title: Comparar o Hub IoT do Azure ao Hubs de Eventos do Azure | Microsoft Docs
description: "Uma comparação dos serviços do Hub IoT do Azure e Hubs de Eventos do Azure destacando diferenças funcionais e casos de uso."
services: iot-hub
documentationcenter: 
author: fsautomata
manager: timlt
editor: 
ms.assetid: aeddea62-8302-48e2-9aad-c5a0e5f5abe9
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/16/2016
ms.author: elioda
translationtype: Human Translation
ms.sourcegitcommit: ce514e19370d2b42fb16b4e96b66f212d5fa999c
ms.openlocfilehash: 9c7e33ceebb28f6a263d2c92e0c0208434e64ffc


---
# <a name="comparison-of-azure-iot-hub-and-azure-event-hubs"></a>Comparação do Hub IoT do Azure e Hubs de Eventos do Azure
Um dos principais casos de uso do Hub IoT é coletar a telemetria dos dispositivos. Por esse motivo, o Hub IoT muitas vezes é comparado aos [Hubs de Eventos do Azure][Azure Event Hubs]. Assim como o Hub IoT, o Hubs de Eventos do Azure é um serviço de processamento de eventos que permite a entrada em grande escala de telemetria e de eventos na nuvem, com baixa latência e alta confiabilidade.

Entretanto, os serviços têm várias diferenças, que são detalhadas na tabela a seguir:

| Área | Hub IoT | Hubs de Eventos |
| --- | --- | --- |
| Padrões de comunicação | Permite [comunicação do dispositivo com a nuvem][lnk-d2c-guidance] (mensagens, carregamentos de arquivos e propriedades reportadas) e [comunicação da nuvem com o dispositivo][lnk-c2d-guidance] (métodos diretos, propriedades desejadas, mensagens). |Permite somente a entrada de eventos (geralmente considerada para cenários do dispositivo para a nuvem). |
| Informações de estado do dispositivo | [Dispositivos gêmeos][lnk-twins] podem armazenar e consultar informações de estado do dispositivo. | Nenhuma informação de estado do dispositivo pode ser armazenada. |
| Suporte ao protocolo de dispositivo |Dá suporte a MQTT, MQTT por WebSockets, AMQP, AMQP por WebSockets e HTTP. Além disso, o Hub IoT funciona com o [gateway de protocolo do IoT do Azure][lnk-azure-protocol-gateway], uma implementação de gateway de protocolo personalizável que permite protocolos personalizados. |Compatível com AMQP, AMQP sobre WebSockets e HTTP. |
| Segurança |Fornece identidade por dispositivo e controle de acesso revogável. Confira a [seção Segurança do guia do desenvolvedor de Hub IoT]. |Fornece [políticas de acesso compartilhado][Event Hubs - security] em todos os Hubs de Eventos, com suporte limitado à revogação usando [políticas do editor][Event Hubs publisher policies]. As soluções IoT muitas vezes devem implementar uma solução personalizada para permitir credenciais e medidas contra falsificação por dispositivo. |
| Monitoramento de operações |Permite que as soluções IoT assinem um conjunto avançado de eventos de conectividade e gerenciamento de identidade de dispositivo, como erros de autenticação de dispositivos individuais, limitação e exceções de formato inválido. Esses eventos permitem identificar rapidamente problemas de conectividade no nível do dispositivo individual. |Expõe apenas as métricas de agregação. |
| Escala |É otimizada para dar suporte a milhões de dispositivos conectados simultaneamente. |Pode dar suporte a uma quantidade mais limitada de conexões simultâneas: até 5.000 conexões AMQP, de acordo com as [Cotas do Barramento de Serviço do Azure][Azure Service Bus quotas]. Por outro lado, os Hubs de Eventos permitem que você especifique a partição de cada mensagem enviada. |
| SDKs de dispositivo |Fornece [SDKs de dispositivo][Azure IoT SDKs] para uma grande variedade de plataformas e linguagens, além de APIs diretas de MQTT, AMQP e HTTP. |Tem suporte no .NET, Java e C, além de nas interfaces de envio AMQP e HTTP. |
| Upload de arquivos |Permite que soluções IoT carreguem arquivos de dispositivos para a nuvem. Inclui um ponto de extremidade de notificação de arquivo para a integração de fluxo de trabalho e uma categoria de monitoramento de operações para o suporte à depuração. | Sem suporte. |

Em resumo, mesmo se o único caso de uso for a entrada de telemetria do dispositivo para nuvem, o Hub IoT fornecerá um serviço que foi desenvolvido para conectividade do dispositivo IoT. Ele continuará expandindo as propostas de valor para esses cenários com recursos específicos de IoT. Os Hubs de Eventos são projetados para a entrada de evento em grande escala, no contexto de cenários internos de datacenter e entre datacenters.

É comum usar o Hub IoT e os Hubs de Eventos na mesma solução. O Hub IoT trata da comunicação do dispositivo com a nuvem e os Hubs de Eventos tratam da entrada do evento em estágio posterior nos mecanismos de processamento em tempo real.

### <a name="next-steps"></a>Próximas etapas
Para saber mais sobre como planejar sua implantação do Hub IoT, veja [Escalando, HA e DR][lnk-scaling].

Para explorar melhor as funcionalidades do Hub IoT, consulte:

* [Guia do desenvolvedor][lnk-devguide]
* [Simulando um dispositivo com o SDK de Gateway IoT][lnk-gateway]

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md

[Azure Event Hubs]: ../event-hubs/event-hubs-what-is-event-hubs.md
[seção Segurança do guia do desenvolvedor de Hub IoT]: iot-hub-devguide-security.md
[Event Hubs - security]: ../event-hubs/event-hubs-authentication-and-security-model-overview.md
[Event Hubs publisher policies]: ../event-hubs/event-hubs-overview.md#common-publisher-tasks
[Azure Service Bus quotas]: ../service-bus-messaging/service-bus-quotas.md
[Azure IoT SDKs]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md



<!--HONumber=Nov16_HO5-->


