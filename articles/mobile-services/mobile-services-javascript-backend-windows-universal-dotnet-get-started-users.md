---
title: Adicionar autenticação ao seu aplicativo universal do Windows 8.1 | Microsoft Docs
description: Aprenda a usar os serviços móveis para autenticar usuários de seu aplicativo da Windows Store por meio de uma variedade de provedores de identidade, incluindo Google, Facebook, Twitter e Microsoft.
services: mobile-services
documentationcenter: windows
author: ggailey777
manager: erikre
editor: ''

ms.service: mobile-services
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 07/21/2016
ms.author: glenga

---
# Adicionar autenticação ao seu aplicativo universal do Windows 8.1
[!INCLUDE [mobile-services-selector-get-started-users](../../includes/mobile-services-selector-get-started-users.md)]

&nbsp;

[!INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

> Para a versão de aplicativos móveis equivalente deste tópico, consulte [Adicionar autenticação ao seu aplicativo Windows](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-users.md).
> 
> 

Este tópico mostra como autenticar usuários nos Serviços Móveis do Azure em seu aplicativo universal para Windows 8.1. Neste tutorial, você pode adicionar autenticação ao projeto de início rápido usando um provedor de identidade suportado pelos Serviços Móveis. Após ser autenticado e autorizado com êxito pelos Serviços Móveis, o valor da ID do usuário é exibido.

Este tutorial baseia-se no quickstart dos Serviços Móveis. Você também deve primeiro concluir o tutorial [Introdução aos Serviços Móveis].

> [!NOTE]
> Esse tutorial mostra como autenticar usuários nos aplicativos Windows Store e Windows Phone Store 8.1. Para um aplicativo Windows Phone 8.0 ou Windows Phone Silverlight 8.1, consulte esta versão de [Introdução à autenticação dos Serviços Móveis](mobile-services-windows-phone-get-started-users.md).
> 
> 

## <a name="register"></a> Registrar seu aplicativo para a autenticação e configure os Serviços Móveis
[!INCLUDE [mobile-services-register-authentication](../../includes/mobile-services-register-authentication.md)]

## <a name="permissions"></a> Restringir permissões a usuários autenticados
[!INCLUDE [mobile-services-restrict-permissions-windows](../../includes/mobile-services-restrict-permissions-windows.md)]

> [!NOTE]
> Ao usar as ferramentas do Visual Studio para conectar seu aplicativo a um Serviço Móvel, a ferramenta gera dois conjuntos de definições **MobileServiceClient**, um para cada plataforma de cliente. Este é um bom momento para simplificar o código gerado, ao unificar as definições ajustadas do `#if...#endif` do **MobileServiceClient** em uma única definição não ajustada usada por ambas as versões do aplicativo. Você não precisará fazer isso se tiver baixado o aplicativo de início rápido do [Portal clássico do Azure].
> 
> 

## <a name="add-authentication"></a> Adicionar autenticação ao aplicativo
[!INCLUDE [mobile-windows-universal-dotnet-authenticate-app](../../includes/mobile-windows-universal-dotnet-authenticate-app.md)]

Agora, todos os usuários autenticados pelos seus provedores de identidades confiados podem acessar a tabela *TodoItem*. Para proteger melhor os dados específicos do usuário, você também deverá implementar a autorização. Para fazer isso, você obtém a ID do usuário de um determinado usuário, que poderá então ser usado para determinar o nível de acesso que o usuário deverá ter para um determinado recurso.

## <a name="tokens"></a>Armazene o token de autorização no cliente
O exemplo anterior mostrou uma entrada padrão, que requer que o cliente contate o provedor de identidade e o serviço móvel sempre que o aplicativo for iniciado. Além de esse método ser ineficiente, você pode se deparar com problemas relacionados ao uso caso muitos consumidores tentem iniciar o aplicativo ao mesmo tempo. Uma melhor abordagem é armazenar em cache o token de autorização retornado pelos Serviços Móveis e tentar usá-lo antes de usar a entrada baseada no provedor.

> [!NOTE]
> Você pode armazenar em cache o token emitido pelos Serviços Móveis usando tanto a autenticação gerenciada pelo cliente quanto a autenticação gerenciada pelo serviço. Este tutorial usa a autenticação gerenciada pelo serviço.
> 
> 

[!INCLUDE [mobile-windows-universal-dotnet-authenticate-app-with-token](../../includes/mobile-windows-universal-dotnet-authenticate-app-with-token.md)]

## <a name="next-steps"> </a>Próximas etapas
No próximo tutorial, [Autorização do lado do serviço dos usuários dos Serviços Móveis](mobile-services-javascript-backend-service-side-authorization.md), você usará o valor da ID do usuário fornecido pelos Serviços Móveis com base em um usuário autenticado para filtrar os dados retornados pelos Serviços Móveis.

## Consulte também
* [Recurso avançado de usuários](http://go.microsoft.com/fwlink/p/?LinkId=506605)<br/> Você pode obter dados de usuário adicionais mantidos pelo provedor de identidade em seu serviço móvel chamando a função **user.getIdentities()** nos scripts de servidor.
* [Referência conceitual do tutorial dos Serviços Móveis em .NET] <br/>Saiba mais sobre como usar os Serviços Móveis com um cliente .NET.

<!-- Anchors. -->
[Register your app for authentication and configure Mobile Services]: #register
[Restrict table permissions to authenticated users]: #permissions
[Add authentication to the app]: #add-authentication
[Store authentication tokens on the client]: #tokens
[Next Steps]: #next-steps


<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Introdução aos Serviços Móveis]: mobile-services-javascript-backend-windows-store-dotnet-get-started.md
[Get started with authentication]: ../mobile-services-javascript-backend-windows-store-dotnet-get-started-users.md
[Get started with push notifications]: ../mobile-services-javascript-backend-windows-store-dotnet-get-started-push.md
[Authorize users with scripts]: ../mobile-services-windows-store-dotnet-authorize-users-in-scripts.md

[Portal clássico do Azure]: https://manage.windowsazure.com/
[Referência conceitual do tutorial dos Serviços Móveis em .NET]: mobile-services-windows-dotnet-how-to-use-client-library.md
[Register your Windows Store app package for Microsoft authentication]: ../mobile-services-how-to-register-store-app-package-microsoft-authentication.md


<!---HONumber=AcomDC_0727_2016-->