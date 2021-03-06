---
title: Introdução à autenticação no Android (back-end do JavaScript) | Microsoft Docs
description: Saiba como usar os Serviços Móveis para autenticar usuários de seu aplicativo do Android por meio de uma variedade de provedores de identidade, incluindo Google, Facebook, Twitter e Microsoft (back-end do JavaScript).
services: mobile-services
documentationcenter: android
author: RickSaling
manager: erikre
editor: ''

ms.service: mobile-services
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 07/21/2016
ms.author: ricksal

---
# Adicionar uma autenticação ao seu aplicativo do Android dos Serviços Móveis (back-end do JavaScript)
[!INCLUDE [mobile-services-selector-get-started-users](../../includes/mobile-services-selector-get-started-users.md)]

&nbsp;

[!INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

> Para a versão de aplicativos móveis equivalente deste tópico, consulte [Adicionar autenticação ao seu aplicativo Android](../app-service-mobile/app-service-mobile-android-get-started-users.md).
> 
> 

## Resumo
Este tópico mostra como autenticar usuários nos Serviços Móveis do Azure em seu aplicativo. Neste tutorial, você pode adicionar autenticação ao projeto de início rápido usando um provedor de identidade suportado pelos Serviços Móveis. Após ser autenticado e autorizado com êxito pelos Serviços Móveis, o valor da ID do usuário é exibido.

> [!VIDEO android-getting-started-with-authentication-in-windows-azure-mobile-services]
> 
> 

Este tutorial explica as etapas básicas para habilitar a autenticação em seu aplicativo.

## Pré-requisitos
[!INCLUDE [mobile-services-android-prerequisites](../../includes/mobile-services-android-prerequisites.md)]

## Registrar seu aplicativo para a autenticação e configurar os Serviços Móveis
[!INCLUDE [mobile-services-register-authentication](../../includes/mobile-services-register-authentication.md)]

## Restringir permissões a usuários autenticados
[!INCLUDE [mobile-services-restrict-permissions-javascript-backend](../../includes/mobile-services-restrict-permissions-javascript-backend.md)]

1. No Android Studio, abra o projeto criado quando você concluiu o tutorial [Introdução aos Serviços Móveis].
2. No menu **Executar**, clique em **Executar aplicativo**. Verifique se uma exceção não tratada com um código de status de 401 (não autorizado) é acionada depois que o aplicativo é iniciado.
   
     Isso acontece porque o aplicativo tenta acessar os Serviços Móveis como um usuário não autenticado, mas a tabela *TodoItem* agora exige autenticação.

Em seguida, você atualizará o aplicativo para autenticar os usuários antes de solicitar recursos do serviço móvel.

## Adicionar autenticação ao aplicativo
[!INCLUDE [mobile-android-authenticate-app](../../includes/mobile-android-authenticate-app.md)]

## <a name="cache-tokens"></a>Armazenar em cache tokens de autenticação no cliente
[!INCLUDE [mobile-android-authenticate-app-with-token](../../includes/mobile-android-authenticate-app-with-token.md)]

## <a name="refresh-tokens"></a>Atualizar o token armazenado em cache
[!INCLUDE [mobile-android-authenticate-app-refresh-token](../../includes/mobile-android-authenticate-app-refresh-token.md)]

## <a name="next-steps"></a>Próximas etapas
No próximo tutorial, [Autorizar usuários com scripts], você irá obter o valor da ID de usuário fornecido pelos Serviços Móveis com base em um usuário autenticado e usar para filtrar os dados retornados pelos Serviços Móveis.

<!-- Anchors. -->
[Register your app for authentication and configure Mobile Services]: #register
[Restrict table permissions to authenticated users]: #permissions
[Add authentication to the app]: #add-authentication
[Store authentication tokens on the client]: #cache-tokens
[Refresh expired tokens]: #refresh-tokens
[Next Steps]: #next-steps

<!-- Images. -->




[4]: ./media/mobile-services-android-get-started-users/mobile-services-selection.png
[5]: ./media/mobile-services-android-get-started-users/mobile-service-uri.png







[13]: ./media/mobile-services-android-get-started-users/mobile-identity-tab.png
[14]: ./media/mobile-services-android-get-started-users/mobile-portal-data-tables.png
[15]: ./media/mobile-services-android-get-started-users/mobile-portal-change-table-perms.png


<!-- URLs. -->

[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Introdução aos Serviços Móveis]: mobile-services-android-get-started.md
[Autorizar usuários com scripts]: mobile-services-javascript-backend-service-side-authorization.md

<!---HONumber=AcomDC_0727_2016-->