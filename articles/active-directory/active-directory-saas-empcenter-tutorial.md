---
title: "Tutorial: integração do Azure Active Directory com o EmpCenter | Microsoft Docs"
description: "Saiba como usar o EmpCenter com o Active Directory do Azure para habilitar o logon único, provisionamento automatizado e muito mais!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: a00ecf6e-917a-4284-b998-41506931585e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/15/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: b24490585019c049af4e1808bf980e36c5143e71


---
# <a name="tutorial-azure-active-directory-integration-with-empcenter"></a>Tutorial: integração do Active Directory do Azure ao EmpCenter
O objetivo deste tutorial é mostrar a integração do Azure ao EmpCenter.  
O cenário descrito neste tutorial pressupõe que você já tem os seguintes itens:

* Uma assinatura válida do Azure
* Uma assinatura habilitada para logon único do EmpCenter

Depois de concluir este tutorial, os usuários do Azure AD atribuídos ao EmpCenter poderão fazer logon único no aplicativo em seu site de empresa do EmpCenter (logon iniciado pelo provedor de serviços) ou usando a [Introdução ao Painel de Acesso](active-directory-saas-access-panel-introduction.md).

O cenário descrito neste tutorial consiste nos seguintes blocos de construção:

1. Habilitando a integração de aplicativos para o EmpCenter
2. Configurando o logon único
3. Configurando o provisionamento de usuários
4. Atribuindo usuários

![Cenário](./media/active-directory-saas-empcenter-tutorial/IC802916.png "Scenario")

## <a name="enabling-the-application-integration-for-empcenter"></a>Habilitando a integração de aplicativos para o EmpCenter
O objetivo desta seção é descrever como habilitar a integração de aplicativos para o EmpCenter.

### <a name="to-enable-the-application-integration-for-empcenter-perform-the-following-steps"></a>Para habilitar a integração de aplicativos para o EmpCenter, execute as seguintes etapas:
1. No Portal clássico do Azure, no painel de navegação à esquerda, clique em **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-empcenter-tutorial/IC700993.png "Active Directory")
2. Na lista **Diretório** , selecione o diretório para o qual você deseja habilitar a integração de diretórios.
3. Para abrir a visualização dos aplicativos, na exibição do diretório, clique em **Aplicativos** no menu principal.
   
   ![Aplicativos](./media/active-directory-saas-empcenter-tutorial/IC700994.png "Applications")
4. Clique em **Adicionar** na parte inferior da página.
   
   ![Adicionar aplicativo](./media/active-directory-saas-empcenter-tutorial/IC749321.png "Add application")
5. Na caixa de diálogo **O que você deseja fazer**, clique em **Adicionar um aplicativo da galeria**.
   
   ![Adicionar um aplicativo da galeria](./media/active-directory-saas-empcenter-tutorial/IC749322.png "Add an application from gallerry")
6. Na **caixa de pesquisa**, digite **EmpCenter**.
   
   ![Galeria de Aplicativos](./media/active-directory-saas-empcenter-tutorial/IC802917.png "Application Gallery")
7. No painel de resultados, selecione **EmpCenter**, em seguida, clique em **Concluir** para adicionar o aplicativo.
   
   ![EmpCentral](./media/active-directory-saas-empcenter-tutorial/IC802918.png "EmpCentral")
   

## <a name="configuring-single-sign-on"></a>Configurando o logon único

O objetivo desta seção é descrever como permitir que os usuários autentiquem no EmpCenter com a própria conta no Azure AD usando federação baseada no protocolo SAML.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Para configurar o logon único, execute as seguintes etapas:
1. No portal clássico do Azure, na página de integração de aplicativos **EmpCenter**, clique em **Configurar logon único** para abrir a caixa de diálogo **Configurar Logon Único ** .
   
   ![Configurar Logon Único](./media/active-directory-saas-empcenter-tutorial/IC802919.png "Configure Single Sign-On")
2. Na página **Como você gostaria que os usuários fizessem logon no EmpCenter**, selecione **Logon Único do Microsoft Azure AD**, em seguida, clique em **Próximo**.
   
   ![Configurar Logon Único](./media/active-directory-saas-empcenter-tutorial/IC802920.png "Configure Single Sign-On")
3. Na página **Definir Configurações do Aplicativo** , execute as seguintes etapas:
   
   ![Definir configurações de aplicativo](./media/active-directory-saas-empcenter-tutorial/IC802921.png "Configure App Settings")
   
   1. Na caixa de texto **URL de Logon**, digite a URL usada pelos usuários para fazer logon no seu aplicativo EmpCenter (p. ex.: *https://partner-authenticati.empcenter.com/workforce/SSO.do*).
   2. Clique em **Avançar**
4. Na página **Configurar logon único no EmpCenter**, para baixar seus metadados, clique em **Baixar metadados**, em seguida, salve o arquivo de metadados em seu computador.
   
   ![Configurar Logon Único](./media/active-directory-saas-empcenter-tutorial/IC802922.png "Configure Single Sign-On")
5. Envie o arquivo de metadados baixado para a equipe de suporte do EmpCenter.
   
   > [!NOTE]
   > A equipe de suporte do EmpCenter precisa fazer a configuração real do SSO.
   > Você receberá uma notificação quando o SSO tiver sido habilitado para sua assinatura.
   > 
   > 
6. No Portal clássico do Azure, selecione a confirmação da configuração de logon único e clique em **Concluir** para fechar a caixa de diálogo **Configurar logon único**.
   
   ![Configurar Logon Único](./media/active-directory-saas-empcenter-tutorial/IC802923.png "Configure Single Sign-On")
   
## <a name="configuring-user-provisioning"></a>Configurando o provisionamento de usuários

Para permitir que os usuários do Azure AD façam logon no EmpCenter, eles deverão ser provisionados no EmpCenter.  
No caso do EmpCenter, as contas de usuário precisam ser criadas pela equipe de suporte do EmpCenter.

> [!NOTE]
> Você pode usar qualquer outra ferramenta de criação da conta de usuário do EmpCenter ou as APIs fornecidas pelo EmpCenter para provisionar as contas de usuário do Active Directory do Azure.
> 
> 

## <a name="assigning-users"></a>Atribuindo usuários
Para testar sua configuração, é necessário conceder acesso ao aplicativo aos usuários do Azure AD que você deseja que usem seu aplicativo.

### <a name="to-assign-users-to-empcenter-perform-the-following-steps"></a>Para atribuir usuários ao EmpCenter, execute as etapas a seguir:
1. No Portal clássico do Azure, crie uma conta de teste.
2. Na página de integração de aplicativos **EmpCenter **, clique em **Atribuir usuários**.
   
   ![Atribuir usuários](./media/active-directory-saas-empcenter-tutorial/IC802924.png "Assign Users")
3. Selecione seu usuário de teste, clique em **Atribuir** e, em seguida, clique em **Sim** para confirmar a atribuição.
   
   ![Sim](./media/active-directory-saas-empcenter-tutorial/IC767830.png "Yes")

Se você quiser testar suas configurações de logon único, abra o Painel de Acesso. Para obter mais detalhes sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Nov16_HO3-->


