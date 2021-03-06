# Visão geral
## [O que é o Service Fabric?](service-fabric-overview.md)
## [Entender os microsserviços](service-fabric-overview-microservices.md)
## [Cenários de aplicativos](service-fabric-application-scenarios.md)
## [Arquitetura](service-fabric-architecture.md)
## [Terminologia](service-fabric-technical-overview.md)

# Introdução
## Configurar seu ambiente de desenvolvimento
### [Windows](service-fabric-get-started.md)
### [Linux](service-fabric-get-started-linux.md)
### [Mac OS](service-fabric-get-started-mac.md)
## Criar seu primeiro aplicativo
### [C# em Windows](service-fabric-create-your-first-application-in-visual-studio.md)
### [Java no Linux](service-fabric-create-your-first-linux-application-with-java.md)
### [C# em Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
## [Implantar aplicativos em um cluster local](service-fabric-get-started-with-a-local-cluster.md)

# Como
## Compilar um aplicativo
### Noções básicas
#### [Modelo de programação](service-fabric-choose-framework.md)
#### [Modelo de aplicativo](service-fabric-application-model.md)
#### [Comunicação de serviço](service-fabric-connect-and-communicate-with-services.md)
#### [Ferramentas](service-fabric-manage-application-in-visual-studio.md)
#### [Depurar](service-fabric-debugging-your-application.md)
#### Monitorar e diagnosticar
##### [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
##### [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)
#### [Configurar políticas de segurança para seu aplicativo](service-fabric-application-runas-security.md)
#### [Configurar seu aplicativo para vários ambientes](service-fabric-manage-multiple-environment-app-configuration.md)

### Aplicativo Reliable Service
#### [Visão geral](service-fabric-reliable-services-introduction.md)
#### Introdução
##### [C# em Windows](service-fabric-reliable-services-quick-start.md)
##### [Java no Linux](service-fabric-reliable-services-quick-start-java.md)
#### [Arquitetura](service-fabric-reliable-services-platform-architecture.md)
#### [Coleções Confiáveis](service-fabric-reliable-services-reliable-collections.md)
#### [Usar Reliable Collections](service-fabric-work-with-reliable-collections.md)
#### [Configurar](service-fabric-reliable-services-configuration.md)
#### [Notificações](service-fabric-reliable-services-notifications.md)
#### [Backup e restauração](service-fabric-reliable-services-backup-restore.md)
#### [Comunicar-se com o Reliable Services](service-fabric-reliable-services-communication.md)
##### [ASP.NET](service-fabric-reliable-services-communication-webapi.md)
##### [Comunicação remota do serviço](service-fabric-reliable-services-communication-remoting.md)
##### [WCF](service-fabric-reliable-services-communication-wcf.md)
##### [Proxy reverso](service-fabric-reverseproxy.md)
#### [Uso avançado](service-fabric-reliable-services-advanced-usage.md)

### Aplicativo Reliable Actor
#### [Visão geral](service-fabric-reliable-actors-introduction.md)
#### Introdução
##### [C# em Windows](service-fabric-reliable-actors-get-started.md)
##### [Java no Linux](service-fabric-reliable-actors-get-started-java.md)
#### [Arquitetura](service-fabric-reliable-actors-platform.md)
#### [Ciclo de vida e coleta de lixo](service-fabric-reliable-actors-lifecycle.md)
#### [Polimorfismo](service-fabric-reliable-actors-polymorphism.md)
#### [Reentrada](service-fabric-reliable-actors-reentrancy.md)
#### [Lembretes e temporizadores](service-fabric-reliable-actors-timers-reminders.md)
#### [Eventos](service-fabric-reliable-actors-events.md)
#### [Gerenciamento de estado](service-fabric-reliable-actors-state-management.md)
#### [Configurar provedor de estado](service-fabric-reliable-actors-kvsactorstateprovider-configuration.md)
#### [Serialização de tipo](service-fabric-reliable-actors-notes-on-actor-type-serialization.md)

### Aplicativo executável convidado
#### [Implantar um executável convidado](service-fabric-deploy-existing-app.md)
#### [Implantar vários executáveis de convidado](service-fabric-deploy-multiple-apps.md)

### Aplicativo de contêiner
#### [Visão geral](service-fabric-containers-overview.md)
#### [Implantar contêiner do Windows](service-fabric-deploy-container.md)
#### [Implantar contêiner do Docker](service-fabric-deploy-container-linux.md)

## Migrar dos Serviços de Nuvem
### [Comparar os Serviços de Nuvem com o Service Fabric](service-fabric-cloud-services-migration-differences.md)
### [Migrar para o Service Fabric](service-fabric-cloud-services-migration-worker-role-stateless-service.md)

## Criar e gerenciar clusters

### Noções básicas
#### [Visão geral](service-fabric-deploy-anywhere.md)
#### [Descrever um cluster](service-fabric-cluster-resource-manager-cluster-description.md)
#### [Planejamento da capacidade](service-fabric-cluster-capacity.md)
#### [Visualizar um cluster](service-fabric-visualizing-your-cluster.md)
#### [Conectar-se a um cluster seguro](service-fabric-connect-to-secure-cluster.md)
#### [Segurança](service-fabric-cluster-security.md)
#### [Recuperação de desastre](service-fabric-disaster-recovery.md)

### Clusters no Azure
#### Criar um cluster no Azure
##### [Portal do Azure](service-fabric-cluster-creation-via-portal.md)
##### [Gerenciador de Recursos do Azure](service-fabric-cluster-creation-via-arm.md)
#### [Tipos de nós e Conjuntos de Dimensionamento de VMs](service-fabric-cluster-nodetypes.md)
#### [Dimensionar um cluster](service-fabric-cluster-scale-up-down.md)
#### [Atualizar um cluster](service-fabric-cluster-upgrade.md)
#### [Excluir um cluster](service-fabric-cluster-delete.md)
#### [Controle de acesso](service-fabric-cluster-security-roles.md)
#### [Configurar um cluster](service-fabric-cluster-fabric-settings.md)
#### [Testar um Cluster Participante de modo gratuito](http://aka.ms/tryservicefabric)

### Clusters independentes
#### [Criar um cluster autônomo](service-fabric-cluster-creation-for-windows-server.md)
#### [Dimensionar um cluster](service-fabric-cluster-windows-server-add-remove-nodes.md)
#### [Atualizar um cluster](service-fabric-cluster-upgrade-windows-server.md)
#### [Proteger um cluster](service-fabric-windows-cluster-x509-security.md)
#### [Controle de acesso](service-fabric-cluster-security-roles.md)
#### [Configurar um cluster](service-fabric-cluster-manifest.md)

## Gerenciar e orquestrar recursos do cluster
### [Visão geral do Resource Manager de cluster](service-fabric-cluster-resource-manager-introduction.md)
### [Arquitetura do Gerenciador de Recursos do Cluster](service-fabric-cluster-resource-manager-architecture.md)
### [Descrever um cluster](service-fabric-cluster-resource-manager-cluster-description.md)
### [Visão geral de grupos de aplicativos](service-fabric-cluster-resource-manager-application-groups.md)
### [Definir configurações do Resource Manager de Cluster](service-fabric-cluster-resource-manager-configure-services.md)
### [Métricas de consumo de recursos](service-fabric-cluster-resource-manager-metrics.md)
### [Usar afinidade de serviço](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md)
### [Políticas de posicionamento de serviço](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md)
### [Gerenciar um cluster](service-fabric-cluster-resource-manager-management-integration.md)
### [Desfragmentação do cluster](service-fabric-cluster-resource-manager-defragmentation-metrics.md)
### [alancear um cluster](service-fabric-cluster-resource-manager-balancing.md)
### [Limitação](service-fabric-cluster-resource-manager-advanced-throttling.md)
### [Movimento de serviço](service-fabric-cluster-resource-manager-movement-cost.md)

## Gerenciar ciclo de vida do aplicativo
### [Visão geral](service-fabric-application-lifecycle.md)
### [Configurar a integração contínua](service-fabric-set-up-continuous-integration.md)
### Implantar ou remover aplicativos
#### [PowerShell](service-fabric-deploy-remove-applications.md)
#### [Visual Studio](service-fabric-publish-app-remote-cluster.md)
### [Visão geral de atualização de aplicativo](service-fabric-application-upgrade.md)
### [Configurar a atualização de aplicativo](service-fabric-visualstudio-configure-upgrade.md)
### [Parâmetros de atualização de aplicativo](service-fabric-application-upgrade-parameters.md)
### Atualizar um aplicativo
#### [PowerShell](service-fabric-application-upgrade-tutorial-powershell.md)
#### [Visual Studio](service-fabric-application-upgrade-tutorial.md)
### [Solucionar problemas de atualizações de aplicativo](service-fabric-application-upgrade-troubleshooting.md)
### [Serialização de dados nas atualizações de aplicativo](service-fabric-application-upgrade-data-serialization.md)
### [Tópicos avançados de atualização de aplicativo](service-fabric-application-upgrade-advanced.md)
### [Exemplo do ciclo de vida de aplicativo baseado em REST](service-fabric-rest-based-application-lifecycle-sample.md)

## Inspecionar a integridade do aplicativo e do cluster
### [Monitorar a integridade do Service Fabric](service-fabric-health-introduction.md)
### [Relatar e verificar a integridade de serviço](service-fabric-diagnostics-how-to-report-and-check-service-health.md)
### [Adicionar relatórios de integridade personalizados](service-fabric-report-health.md)
### [Solucionar problemas com relatórios de integridade do sistema](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
### [Exibir relatórios de integridade](service-fabric-view-entities-aggregated-health.md)

## Monitorar e diagnosticar
### Monitorar e diagnosticar serviços localmente
#### [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
#### [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)
### Logs de Diagnóstico do Azure
#### [Windows](service-fabric-diagnostics-how-to-setup-wad.md)
#### [Linux](service-fabric-diagnostics-how-to-setup-lad.md)
### [Rastreamento de aplicativo do Service Fabric](service-fabric-diagnostic-how-to-use-elasticsearch.md)
### [Diagnósticos em Reliable Actors](service-fabric-reliable-actors-diagnostics.md)
### [Diagnóstico em Reliable Services com estado](service-fabric-reliable-services-diagnostics.md)
### [Solucionar problemas do seu cluster local](service-fabric-troubleshoot-local-cluster-setup.md)
### [Solução de problemas comuns](service-fabric-diagnostics-troubleshoot-common-scenarios.md)

## Dimensionar aplicativos
### [Particionar Reliable Services](service-fabric-concepts-partitioning.md)
### [Disponibilidade de serviços](service-fabric-availability-services.md)
### [Dimensionar aplicativos](service-fabric-concepts-scalability.md)
### [Planejar capacidade dos aplicativos](service-fabric-capacity-planning.md)

## Testar aplicativos e serviços
### [Visão geral de Análise de Falhas](service-fabric-testability-overview.md)
### [Testar comunicação serviço a serviço](service-fabric-testability-scenarios-service-communication.md)
### Simular falhas
#### [Como usar o Caos controlado](service-fabric-controlled-chaos.md)
#### [Como usar ações de Teste](service-fabric-testability-actions.md)
#### [Durante cargas de trabalho](service-fabric-testability-workload-tests.md)
#### [Invocando perda de dados](service-fabric-use-data-loss-api.md)
#### [Como usar cenários de Teste](service-fabric-testability-scenarios.md)
### [Fazer teste de carga em seu aplicativo](service-fabric-vso-load-test.md)

# Referência
## [PowerShell](//powershell/servicefabric/vlatest/servicefabric)
## [API Java](/java/api/microsoft.servicefabric.services)
## [.NET](/dotnet/api/microsoft.servicefabric.services)
## [REST](/rest/api/servicefabric)

# Recursos
## [Exemplo de código](http://aka.ms/servicefabricsamples)
## [Roteiro de aprendizagem](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)
## [Preços](https://azure.microsoft.com/pricing/details/service-fabric/)
## [Atualizações de serviço](https://azure.microsoft.com/updates/?product=service-fabric)
## [Fórum do MSDN](https://social.msdn.microsoft.com/Forums/home?forum=AzureServiceFabric)
## [Vídeos](https://azure.microsoft.com/documentation/videos/index/?services=service-fabric)


<!--HONumber=Jan17_HO3-->


