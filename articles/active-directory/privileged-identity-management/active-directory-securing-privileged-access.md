---
title: Protegendo o Acesso Privilegiado no Azure AD | Microsoft Docs
description: "Um tópico que explica as abordagens para a proteção de acesso privilegiado no Azure, Azure Active Directory e Microsoft Online Services."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: mwahl
ms.assetid: 235a0ce9-1daf-4433-8f65-9c6afcd64d08
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/26/2016
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 0e6c3c516b5d4271c5201d0734e22a83876dd3fa


---
# <a name="securing-privileged-access-in-azure-ad"></a>Protegendo o acesso privilegiado no Azure AD
Proteger o acesso privilegiado é a primeira etapa crítica para ajudar a proteger os ativos de negócios em uma organização moderna. As contas privilegiadas são aquelas que administram e gerenciam sistemas de TI. Os invasores virtuais visam essas contas para obter acesso aos sistemas e aos dados de uma organização. Para proteger o acesso privilegiado, você deve isolar as contas e os sistemas do risco de serem expostos a um usuário mal-intencionado.

Mais usuários estão começando a obter acesso privilegiado por meio de serviços de nuvem. Isso pode incluir os administradores globais do Office365, administradores de assinatura do Azure e os usuários que têm acesso administrativo nas VMs ou em aplicativos SaaS.

A Microsoft recomenda que você siga este roteiro em [Securing Privileged Access](https://technet.microsoft.com/library/mt631194.aspx)(Proteger o acesso privilegiado).

Para clientes que usam o Azure Active Directory para gerenciar o acesso ao Azure, Office 365 ou outros serviços e aplicativos da Microsoft, esses princípios se aplicarão se as contas de usuário forem gerenciadas e autenticadas pelo Active Directory ou no Azure Active Directory. As seções a seguir fornecem mais informações sobre os recursos do Azure AD para dar suporte à proteção de acesso privilegiado.

## <a name="multi-factor-authentication"></a>Autenticação multifator
Para aumentar a segurança da autenticação de administrador, você deve exigir o MFA (multi-factor authentication) antes de conceder privilégios. A MFA é um método de verificar quem você é e que requer o uso de mais do que apenas um nome de usuário e uma senha. Ele fornece uma segunda camada de segurança para logons de usuário e transações.

O Azure Multi-Factor Authentication ajuda a proteger o acesso a dados e aplicativos enquanto atende à demanda dos usuários para um processo de logon simples. Ele fornece autenticação forte por meio de uma variedade de opções de verificação fácil, incluindo chamadas telefônicas, mensagens de texto, notificações de aplicativo móvel ou código de verificação e tokens OATH de terceiros.

Para obter uma visão geral de como funciona o Azure Multi-Factor Authentication, consulte o vídeo a seguir.

> [!VIDEO https://channel9.msdn.com/Blogs/Windows-Azure/Windows-Azure-Multi-Factor-Authentication/player]
> 
> 

Para obter mais detalhes, consulte [MFA for Office 365 and MFA for Azure](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/)(MFA para Office 365 e MFA para o Azure).

## <a name="time-bound-privileges"></a>Privilégios de limite de tempo
Algumas organizações podem acreditar que elas têm muitos usuários em funções altamente privilegiadas. Um usuário pode ter sido adicionado à função para uma atividade específica, como para se inscrever para um serviço, mas não usa esses privilégios com frequência posteriormente.

Para reduzir o tempo de exposição de privilégios e aumentar sua visibilidade sobre seu uso, limite os usuários para assumirem seus privilégios JIT (Just in Time) somente quando eles precisam realizar uma tarefa. Para o Azure Active Directory e Microsoft Online Services, você pode usar o [Azure AD PIM (Privileged Identity Management)](http://aka.ms/AzurePIM).

![Painel PIM][2]

## <a name="attack-detection"></a>Detecção de ataque
O [Azure Active Directory Identity Protection](../active-directory-identityprotection.md) fornece uma exibição consolidada dos eventos de risco e das possíveis vulnerabilidades que afetam as identidades da sua organização. Baseando-se em eventos de risco, o Identity Protection calcula um nível de risco para cada usuário, permitindo a você definir políticas baseadas em risco para proteger automaticamente as identidades da sua organização. Essas políticas, juntamente com outros controles de acesso condicional fornecidos pelo Azure Active Directory e pelo EMS, podem bloquear automaticamente o usuário ou oferecer sugestões que incluem redefinições de senha e a imposição de autenticação multifator.

![Azure AD Identity Protection][3]

## <a name="conditional-access"></a>Acesso condicional
Com o controle de acesso condicional, o Active Directory do Azure verifica as condições específicas que você escolhe para autenticar o usuário antes de permitir o acesso a um aplicativo. Quando essas condições forem atendidas, o usuário é autenticado e autorizado a acessar o aplicativo.

![Configurando regras de acesso condicional com MFA][4]

## <a name="related-articles"></a>Artigos relacionados
* Habilitar o [Azure Multi-Factor Authentication](../../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md)
* Habilitar o [Azure AD Privileged Identity Management](../active-directory-privileged-identity-management-configure.md)
* Habilitar o [Azure AD Identity Protection](../active-directory-identityprotection.md)
* Habilitar os [controles de acesso condicionais](../active-directory-conditional-access.md)

Para obter mais informações sobre a criação de um roteiro de segurança completo, consulte a seção “Customer responsibilities and roadmap” (Roteiro e responsabilidades do cliente) do documento [Microsoft Cloud Security for Enterprise Architects](http://aka.ms/securecustomer) (Microsoft Cloud Security for Enterprise Architects). Para obter mais informações em atrair os serviços da Microsoft para ajudá-lo em qualquer um desses tópicos, entre em contato com seu representante da Microsoft ou visite nossa [página de soluções de segurança cibersegurança](https://www.microsoft.com/microsoftservices/campaigns/cybersecurity-protection.aspx).

<!--Image references-->
[1]: ../media/active-directory-privileged-identity-management-configure/Search_PIM.png
[2]: ../media/active-directory-privileged-identity-management-configure/PIM_Dash.png
[3]: ../media/active-directory-identityprotection/29.png
[4]: ../media/active-directory-conditional-access/conditionalaccess-saas-apps.png



<!--HONumber=Dec16_HO2-->


