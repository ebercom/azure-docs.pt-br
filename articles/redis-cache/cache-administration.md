---
title: Como administrar o Cache Redis do Azure | Microsoft Docs
description: Saiba como executar tarefas administrativas, como atualizações de reinicialização e agenda para o Cache Redis do Azure
services: redis-cache
documentationcenter: na
author: steved0x
manager: douge
editor: tysonn

ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 09/27/2016
ms.author: sdanie

---
# <a name="how-to-administer-azure-redis-cache"></a>Como administrar o Cache Redis do Azure
Este tópico descreve como executar tarefas administrativas, como reinicializar e agendar atualizações para as instâncias de Cache Redis do Azure.

> [!IMPORTANT]
> As configurações e os recursos descritos neste artigo só estão disponíveis para os caches da camada Premium.
> 
> 

## <a name="administration-settings"></a>Configurações de administração
As configurações de **administração** do Cache Redis do Azure permitem que você execute as tarefas administrativas a seguir para seu cache premium. Para acessar as configurações de administração, clique em **Configurações** ou em **Todas as configurações** na folha Cache Redis e role até a seção **Administração** na folha **Configurações**.

![Administração](./media/cache-administration/redis-cache-administration.png)

* [Reboot](#reboot)
* [Agendar atualizações](#schedule-updates)

## <a name="reboot"></a>Reboot
A folha **Reinicializar** permite a reinicialização de um ou mais nós do cache. Isso o habilita a testar o aplicativo para garantir a resiliência em caso de falhas.

![Reboot](./media/cache-administration/redis-cache-reboot.png)

Se tiver um cache premium com clustering habilitado, você poderá selecionar quais fragmentos do cache serão reinicializados.

![Reboot](./media/cache-administration/redis-cache-reboot-cluster.png)

Para reinicializar um ou mais nós do cache, selecione os nós desejados e clique em **Reinicializar**. Se tiver um cache premium com clustering habilitado, selecione os fragmentos a serem reinicializados e clique em **Reinicializar**. Após alguns minutos, os nós selecionados são reinicializados e ficam online novamente alguns instantes depois.

O impacto em aplicativos cliente varia de acordo com os nós que você reinicializa.

* **Mestre** - quando o nó mestre é reinicializado, o Cache Redis do Azure realiza o failover para o nó de réplica e o promove a mestre. Durante esse failover, pode haver um breve intervalo em que as conexões podem falhar para o cache.
* **Subordinado** - quando o nó subordinado é reinicializado, geralmente não há impacto para os clientes de cache.
* **Mestre e subordinado** - quando ambos os nós de cache são reinicializados, todos os dados são perdidos no cache, e as conexões com o cache falham até que o nó primário fique online novamente. Se você tiver configurado a [persistência de dados](cache-how-to-premium-persistence.md), o backup mais recente será restaurado quando o cache ficar online novamente. Observe que as gravações de cache que ocorrerem após o backup mais recente serão perdidas.
* **Nó(s) de um cache premium com clustering habilitado** - quando você reinicializa o(s) nó(s) de um cache premium com clustering habilitado, o comportamento é o mesmo que ocorre quando você reinicializa o(s) nó(s) de um cache não clusterizado.

> [!IMPORTANT]
> A reinicialização está disponível somente para caches da camada Premium.
> 
> 

## <a name="reboot-faq"></a>Perguntas frequentes sobre reinicialização
* [Qual nó devo reinicializar para testar o aplicativo?](#which-node-should-i-reboot-to-test-my-application)
* [Posso reinicializar o cache para limpar conexões de cliente?](#can-i-reboot-the-cache-to-clear-client-connections)
* [Perderei dados do cache se eu fizer uma reinicialização?](#will-i-lose-data-from-my-cache-if-i-do-a-reboot)
* [Posso reinicializar o cache usando o PowerShell, a CLI ou outras ferramentas de gerenciamento?](#can-i-reboot-my-cache-using-powershell-cli-or-other-management-tools)
* [Que tipos de preços podem usar a funcionalidade de reinicialização?](#what-pricing-tiers-can-use-the-reboot-functionality)

### <a name="which-node-should-i-reboot-to-test-my-application?"></a>Qual nó devo reinicializar para testar o aplicativo?
Para testar a resiliência do aplicativo contra falhas do nó principal do cache, reinicialize o nó **Mestre** . Para testar a resiliência do aplicativo contra falhas do nó secundário, reinicialize o nó **Subordinado** . Para testar a resiliência do aplicativo contra falha total do cache, reinicialize **Ambos** os nós.

### <a name="can-i-reboot-the-cache-to-clear-client-connections?"></a>Posso reinicializar o cache para limpar conexões de cliente?
Sim, se você reinicializar o cache, todas as conexões de cliente serão limpas. Isso pode útil quando todas as conexões de cliente são usadas, por exemplo, devido a um erro de lógica ou um bug no aplicativo cliente. Cada tipo de preços tem diferentes [limites de conexão do cliente](cache-configure.md#default-redis-server-configuration) para os vários portes, e quando os limites são atingidos, não são mais aceitas mais conexões de cliente. Reinicializar o cache fornece uma maneira de limpar todas as conexões de cliente.

> [!IMPORTANT]
> Se as conexões de cliente forem totalmente usadas devido a um erro de lógica ou um bug no código do cliente, observe que StackExchange.Redis se reconectará automaticamente quando o nó do Redis estiver online novamente. Se o problema subjacente não for resolvido, as conexões de cliente continuarão a ser usadas.
> 
> 

### <a name="will-i-lose-data-from-my-cache-if-i-do-a-reboot?"></a>Perderei dados do cache se eu fizer uma reinicialização?
Se você reinicializar os nós **Mestre** e **Subordinado**, todos os dados no cache (ou nesse fragmento, se você estiver usando um cache premium com clustering habilitado) serão perdidos. Se você tiver configurado a [persistência de dados](cache-how-to-premium-persistence.md), o backup mais recente será restaurado quando o cache ficar online novamente. Observe que as gravações em cache ocorridas após o backup serão perdidas.

Se você reinicializar apenas um dos nós, normalmente os dados não serão perdidos, mas isso ainda poderá ocorrer. Por exemplo, se o nó mestre for reinicializado e uma gravação de cache estiver em andamento, os dados da gravação do cache serão perdidos. Outro cenário de perda de dados ocorre se você reinicializa um nó e o outro nó ocorre fica inoperante devido a uma falha ao mesmo tempo. Para saber mais sobre as possíveis causas de perda de dados, confira [O que aconteceu com meu dados no Redis?](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md).

### <a name="can-i-reboot-my-cache-using-powershell,-cli,-or-other-management-tools?"></a>Posso reinicializar o cache usando o PowerShell, a CLI ou outras ferramentas de gerenciamento?
Sim, para ver as instruções do PowerShell, confira [Para reinicializar um cache Redis](cache-howto-manage-redis-cache-powershell.md#to-reboot-a-redis-cache).

### <a name="what-pricing-tiers-can-use-the-reboot-functionality?"></a>Que tipos de preços podem usar a funcionalidade de reinicialização?
A reinicialização está disponível apenas no tipo de preços premium.

## <a name="schedule-updates"></a>Agendar atualizações
A folha **Agendar atualizações** permite designar uma janela de manutenção para seu cache. Quando a janela de manutenção é especificada, as atualizações do servidor Redis são feitas durante essa janela. Observe que a janela de manutenção se aplica apenas às atualizações do servidor Redis, não a atualizações do Azure ou a atualizações do sistema operacional das VMs que hospedam o cache.

![Agendar atualizações](./media/cache-administration/redis-schedule-updates.png)

Para especificar uma janela de manutenção, marque os dias desejados, especifique a hora de início da janela de manutenção para cada dia e clique em **OK**. Observe que o horário da janela de manutenção é em UTC. 

> [!NOTE]
> A janela de manutenção padrão para atualizações é de 5 horas. Esse valor não é configurável no portal do Azure, mas você pode configurá-lo no PowerShell usando o parâmetro `MaintenanceWindow` do cmdlet [New-AzureRmRedisCacheScheduleEntry](https://msdn.microsoft.com/library/azure/mt763833.aspx) . Para saber mais, veja [Posso gerenciar as atualizações agendadas usando o PowerShell, a CLI ou outras ferramentas de gerenciamento?](#can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools)
> 
> 

## <a name="schedule-updates-faq"></a>Perguntas frequentes sobre agendamento de atualizações
* [Quando as atualizações ocorrerão se eu não usar o recurso de agendamento de atualizações?](#when-do-updates-occur-if-i-dont-use-the-schedule-updates-feature)
* [Que tipos de atualizações são feitas durante a janela de manutenção agendada?](#what-type-of-updates-are-made-during-the-scheduled-maintenance-window)
* [Posso gerenciar as atualizações agendadas usando o PowerShell, a CLI ou outras ferramentas de gerenciamento?](#can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools)
* [Que tipos de preços podem usar a funcionalidade de agendamento de atualizações?](#what-pricing-tiers-can-use-the-schedule-updates-functionality)

### <a name="when-do-updates-occur-if-i-don't-use-the-schedule-updates-feature?"></a>Quando as atualizações ocorrerão se eu não usar o recurso de agendamento de atualizações?
Se você não especificar uma janela de manutenção, as atualizações poderão ser feitas a qualquer momento.

### <a name="what-type-of-updates-are-made-during-the-scheduled-maintenance-window?"></a>Que tipos de atualizações são feitas durante a janela de manutenção agendada?
Apenas as atualizações do servidor Redis são realizadas durante a janela de manutenção agendada. A janela de manutenção se aplica a atualizações do Azure ou do sistema operacional da VM.

### <a name="can-i-managed-scheduled-updates-using-powershell,-cli,-or-other-management-tools?"></a>Posso gerenciar as atualizações agendadas usando o PowerShell, a CLI ou outras ferramentas de gerenciamento?
Sim, você pode gerenciar as atualizações agendadas usando os cmdlets do PowerShell a seguir.

* [Get-AzureRmRedisCachePatchSchedule](https://msdn.microsoft.com/library/azure/mt763835.aspx)
* [New-AzureRmRedisCachePatchSchedule](https://msdn.microsoft.com/library/azure/mt763834.aspx)
* [New-AzureRmRedisCacheScheduleEntry](https://msdn.microsoft.com/library/azure/mt763833.aspx)
* [Remove-AzureRmRedisCachePatchSchedule](https://msdn.microsoft.com/library/azure/mt763837.aspx)

### <a name="what-pricing-tiers-can-use-the-schedule-updates-functionality?"></a>Que tipos de preços podem usar a funcionalidade de agendamento de atualizações?
As atualizações de agenda estão disponíveis apenas no tipo de preços premium.

## <a name="next-steps"></a>Próximas etapas
* Explore mais recursos da [camada premium do Cache Redis do Azure](cache-premium-tier-intro.md) .

<!--HONumber=Oct16_HO2-->


