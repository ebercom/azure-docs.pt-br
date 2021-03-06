---
title: Criar um Hub IoT do Azure usando um modelo (.NET) | Microsoft Docs
description: Como usar um modelo do Azure Resource Manager para criar um Hub IoT com um programa C#.
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: a447b40c-c728-487e-875d-db554db5adc3
ms.service: iot-hub
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/16/2016
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: e4343cfece7286e0a4035239967ab2e0a928b188
ms.openlocfilehash: cc284c0cd445bbfe2f4424d3251ca07da0538192


---
# <a name="create-an-iot-hub-using-azure-resource-manager-template-net"></a>Criar um Hub IoT usando um modelo do Azure Resource Manager (.NET)
[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Introdução
Você pode usar o Gerenciador de Recursos do Azure para criar e gerenciar hubs IoT do Azure de forma programática. Este tutorial mostra como usar um modelo do Azure Resource Manager para criar um Hub IoT em um programa em C#.

> [!NOTE]
> O Azure tem dois modelos de implantação diferentes para criar e trabalhar com recursos: [Azure Resource Manager e Classic](../azure-resource-manager/resource-manager-deployment-model.md).  Este artigo aborda o uso do modelo de implantação do Azure Resource Manager.
> 
> 

Para concluir este tutorial, você precisará do seguinte:

* Microsoft Visual Studio 2015.
* Uma conta ativa do Azure. <br/>Se você não tem uma conta, pode criar uma [conta gratuita][lnk-free-trial] em apenas alguns minutos.
* Uma [conta de armazenamento do Azure][lnk-storage-account] em que é possível armazenar seus arquivos de modelo do Azure Resource Manager.
* [Azure PowerShell 1.0][lnk-powershell-install] ou posterior.

[!INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a>Preparar seu projeto do Visual Studio
1. No Visual Studio, crie um projeto Windows do Visual C# usando o modelo de projeto de **Aplicativo do Console** . Nomeie o projeto **CreateIoTHub**.
2. No Gerenciador de Soluções, clique com o botão direito do mouse no projeto e clique em **Gerenciar Pacotes NuGet**.
3. No Gerenciador de Pacotes NuGet, marque **Incluir pré-lançamento** e procure **Microsoft.Azure.Management.ResourceManager**. Clique em **Instalar**, em **Revisar alterações** clique em **OK** e em **Aceito** para aceitar as licenças.
4. No Gerenciador de Pacotes do NuGet, procure **Microsoft.IdentityModel.Clients.ActiveDirectory**.  Clique em **Instalar**, em **Revisar alterações**, clique em **OK** e em **Aceito** para aceitar a licença.
5. Em Program.cs, substitua as instruções **using** existentes pelo seguinte:
   
    ```
    using System;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```
6. Em Program.cs, adicione as variáveis estáticas a seguir, substituindo os valores de espaço reservado. Você fez uma anotação de **ApplicationId**, **SubscriptionId**, **TenantId** e **Password** anteriormente neste tutorial. **Nome de sua conta de armazenamento do Azure** é o nome da conta de armazenamento do Azure na qual você armazena seus arquivos de modelo do Azure Resource Manager. **Nome do grupo de recursos** é o nome do grupo de recursos que você usa ao criar o Hub IoT; ele pode ser um grupo de recursos pré-existente ou um novo. **Nome da implantação** é um nome para a implantação, como **Deployment_01**.
   
    ```
    static string applicationId = "{Your ApplicationId}";
    static string subscriptionId = "{Your SubscriptionId}";
    static string tenantId = "{Your TenantId}";
    static string password = "{Your application Password}";
    static string storageAddress = "https://{Your storage account name}.blob.core.windows.net";
    static string rgName = "{Resource group name}";
    static string deploymentName = "{Deployment name}";
    ```

[!INCLUDE [iot-hub-get-access-token](../../includes/iot-hub-get-access-token.md)]

## <a name="submit-an-azure-resource-manager-template-to-create-an-iot-hub"></a>Enviar um modelo do Azure Resource Manager para criar um Hub IoT
Use um modelo JSON e um arquivo de parâmetros para criar um Hub IoT em seu grupo de recursos. Também é possível usar um modelo do Azure Resource Manager para fazer alterações em um hub IoT existente.

1. No Gerenciador de Soluções, clique com o botão direito do mouse no seu projeto, clique em **Adicionar** e depois em **Novo Item**. Adicione um arquivo JSON chamado **template.json** ao seu projeto.
2. Substitua o conteúdo de **template.json** pela seguinte definição de recurso para adicionar um Hub IoT padrão à região **Leste dos EUA**. Para obter a lista atual de regiões que dão suporte ao Hub IoT, confira o [Status do Azure][lnk-status]:
   
    ```
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": {
          "type": "string"
        }
      },
      "resources": [
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs",
        "name": "[parameters('hubName')]",
        "location": "East US",
        "sku": {
          "name": "S1",
          "tier": "Standard",
          "capacity": 1
        },
        "properties": {
          "location": "East US"
        }
      }
      ],
      "outputs": {
        "hubKeys": {
          "value": "[listKeys(resourceId('Microsoft.Devices/IotHubs', parameters('hubName')), '2016-02-03')]",
          "type": "object"
        }
      }
    }
    ```
3. No Gerenciador de Soluções, clique com o botão direito do mouse no seu projeto, clique em **Adicionar** e depois em **Novo Item**. Adicione um arquivo JSON chamado **parameters.json** ao seu projeto.
4. Substitua o conteúdo de **parameters.json** pelas seguintes informações de parâmetro que definem um nome para o novo Hub IoT, como **{suas iniciais}mynewiothub**. O nome do Hub IoT precisa ser globalmente exclusivo e, por isso, deve incluir seu nome e suas iniciais:
   
    ```
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": { "value": "mynewiothub" }
      }
    }
    ```
5. No **Gerenciador de Servidores**, conecte-se à sua assinatura do Azure e, em sua conta de armazenamento, crie um contêiner chamado **modelos**. No painel **Propriedades**, defina as permissões **Acesso de Leitura Público** para o contêiner **modelos** como **Blob**.
6. No **Gerenciador de Servidores**, clique com o botão direito do mouse no contêiner **modelos** e em **Exibir Contêiner de Blobs**. Clique no botão **Carregar Blob**, escolha os dois arquivos, **parameters.json** e **templates.json**, e clique em **Abrir** para carregar os arquivos JSON no contêiner **modelos**. As URLs dos blobs com os dados JSON são:
   
    ```
    https://{Your storage account name}.blob.core.windows.net/templates/parameters.json
    https://{Your storage account name}.blob.core.windows.net/templates/template.json
    ```
7. Adicione o seguinte método ao Program.cs:
   
    ```
    static void CreateIoTHub(ResourceManagementClient client)
    {
   
    }
    ```
8. Adicione o seguinte código ao método **CreateIoTHub** para enviar os arquivos de modelo e parâmetro ao Azure Resource Manager:
   
    ```
    var createResponse = client.Deployments.CreateOrUpdate(
        rgName,
        deploymentName,
        new Deployment()
        {
          Properties = new DeploymentProperties
          {
            Mode = DeploymentMode.Incremental,
            TemplateLink = new TemplateLink
            {
              Uri = storageAddress + "/templates/template.json"
            },
            ParametersLink = new ParametersLink
            {
              Uri = storageAddress + "/templates/parameters.json"
            }
          }
        });
    ```
9. Adicione o seguinte código ao método **CreateIoTHub** , que exibe o status e as chaves do novo Hub IoT:
   
    ```
    string state = createResponse.Properties.ProvisioningState;
    Console.WriteLine("Deployment state: {0}", state);
   
    if (state != "Succeeded")
    {
      Console.WriteLine("Failed to create iothub");
    }
    Console.WriteLine(createResponse.Properties.Outputs);
    ```

## <a name="complete-and-run-the-application"></a>Compilar e executar o aplicativo
Agora, você pode concluir o aplicativo chamando o método **CreateIoTHub** antes de compilá-lo e de executá-lo.

1. Adicione o seguinte código ao final do método **Main** :
   
    ```
    CreateIoTHub(client);
    Console.ReadLine();
    ```
2. Clique em **Criar** e **Compilar Solução**. Corrija todos os erros.
3. Clique em **Depurar** e, em seguida, **Iniciar Depuração** para executar o aplicativo. Pode levar vários minutos para que a implantação seja executada.
4. Você pode verificar se o seu aplicativo adicionou o novo hub IoT visitando o [portal do Azure][lnk-azure-portal] e exibindo sua lista de recursos ou usando o cmdlet **Get-AzureRmResource** do PowerShell.

> [!NOTE]
> Este aplicativo de exemplo adiciona um Hub IoT Standard S1 pelo qual você será cobrado. É possível excluir o hub IoT por meio do [portal do Azure][lnk-azure-portal] ou usando o cmdlet **Remove-AzureRmResource** do PowerShell quando tiver terminado.
> 
> 

## <a name="next-steps"></a>Próximas etapas
Agora que você implantou um Hub IoT usando um modelo do Azure Resource Manager com um programa C#, convém explorar ainda mais:

* Leia sobre as funcionalidades da [API REST do provedor de recursos Hub IoT][lnk-rest-api].
* Leia a [Visão geral do Azure Resource Manager][lnk-azure-rm-overview] para saber mais sobre os recursos do Azure Resource Manager.

Para saber mais sobre como desenvolver para o Hub IoT, consulte o seguinte:

* [Introdução ao SDK de C][lnk-c-sdk]
* [SDKs do Azure IoT][lnk-sdks]

Para explorar melhor as funcionalidades do Hub IoT, consulte:

* [Simulando um dispositivo com o SDK do Gateway do IoT][lnk-gateway]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-powershell-install]: /powershell/azureps-cmdlets-docs
[lnk-rest-api]: https://msdn.microsoft.com/library/mt589014.aspx
[lnk-azure-rm-overview]: ../azure-resource-manager/resource-group-overview.md
[lnk-storage-account]: ../storage/storage-create-storage-account.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md



<!--HONumber=Dec16_HO1-->


