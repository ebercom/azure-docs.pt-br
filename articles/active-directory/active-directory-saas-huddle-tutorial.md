---
title: "Tutorial: Integração do Azure Active Directory ao Huddle | Microsoft Docs"
description: "Saiba como usar o Huddle com o Active Directory do Azure para habilitar o logon único, provisionamento automatizado e muito mais!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 8389ba4c-f5f8-4ede-b2f4-32eae844ceb0
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/29/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 85d7789599cfb39ce0de5d52e0fe057482a49039


---
# <a name="tutorial-azure-active-directory-integration-with-huddle"></a>Tutorial: integração do Active Directory do Azure ao Huddle
O objetivo deste tutorial é mostrar a integração do Azure ao Huddle.  
O cenário descrito neste tutorial pressupõe que você já tem os seguintes itens:

* Uma assinatura válida do Azure
* Uma assinatura habilitada para logon único do Huddle

Depois de concluir este tutorial, os usuários do Azure AD atribuídos ao Huddle poderão fazer logon único no aplicativo em seu site de empresa do Huddle (logon iniciado pelo provedor de serviços) ou usando a [Introdução ao Painel de Acesso](active-directory-saas-access-panel-introduction.md).

O cenário descrito neste tutorial consiste nos seguintes blocos de construção:

1. Habilitando a integração de aplicativos para Huddle
2. Configurando o logon único
3. Configurando o provisionamento de usuários
4. Atribuindo usuários

![Configurar o logon único](./media/active-directory-saas-huddle-tutorial/IC787830.png "Configure Single Sign-On")

## <a name="enabling-the-application-integration-for-huddle"></a>Habilitando a integração de aplicativos para Huddle
O objetivo desta seção é descrever como habilitar a integração de aplicativos para o Huddle.

### <a name="to-enable-the-application-integration-for-huddle-perform-the-following-steps"></a>Para habilitar a integração de aplicativos para o Huddle, execute as seguintes etapas:
1. No Portal clássico do Azure, no painel de navegação à esquerda, clique em **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-huddle-tutorial/IC700993.png "Active Directory")
2. Na lista **Diretório** , selecione o diretório para o qual você deseja habilitar a integração de diretórios.
3. Para abrir a visualização dos aplicativos, na exibição do diretório, clique em **Aplicativos** no menu principal.
   
   ![Aplicativos](./media/active-directory-saas-huddle-tutorial/IC700994.png "Applications")
4. Clique em **Adicionar** na parte inferior da página.
   
   ![Adicionar aplicativo](./media/active-directory-saas-huddle-tutorial/IC749321.png "Add application")
5. Na caixa de diálogo **O que você deseja fazer**, clique em **Adicionar um aplicativo da galeria**.
   
   ![Adicionar um aplicativo da galeria](./media/active-directory-saas-huddle-tutorial/IC749322.png "Add an application from gallerry")
6. Na **caixa de pesquisa**, digite **Huddle**.
   
   ![Galeria de Aplicativos](./media/active-directory-saas-huddle-tutorial/IC787831.png "Application Gallery")
7. No painel de resultados, selecione **Huddle** e clique em **Concluir** para adicionar o aplicativo.
   
   ![Huddle](./media/active-directory-saas-huddle-tutorial/IC787832.png "Huddle")
   
   ## <a name="configuring-single-sign-on"></a>Configurando o logon único

O objetivo desta seção é descrever como permitir que os usuários se autentiquem no Huddle com a própria conta do Azure AD usando a federação baseada no protocolo SAML.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Para configurar o logon único, execute as seguintes etapas:
1. No portal clássico do Azure, na página de integração do aplicativo **Huddle**, clique em **Configurar logon único** para abrir o diálogo **Configurar Logon Único**.
   
   ![Configurar o logon único](./media/active-directory-saas-huddle-tutorial/IC787833.png "Configure Single Sign-On")
2. Na página **Como você deseja que os usuários façam logon no Huddle**, selecione **Logon Único do Microsoft Azure AD** e clique em **Avançar**.
   
   ![Configurar o logon único](./media/active-directory-saas-huddle-tutorial/IC787834.png "Configure Single Sign-On")
3. Na página **Configurar a URL do Aplicativo**, na caixa de texto **URL de Entrada no Huddle**, digite a URL do seu locatário Huddle usando o padrão "*http://company.huddle.com*" e clique em **Avançar**.
   
   ![Configurar a URL do Aplicativo](./media/active-directory-saas-huddle-tutorial/IC787835.png "Configure App URL")
4. Na página **Configurar logon único no Huddle** , realize as seguintes etapas:
   
   ![Configurar o logon único](./media/active-directory-saas-huddle-tutorial/IC787836.png "Configure Single Sign-On")
   
   1. Clique em **Baixar certificado**e salve o certificado no computador.
   2. Copie o valor da **URL do Emissor**, o valor da **URL de SSO do SAML** e o certificado baixado e envie tudo à equipe de suporte do Huddle.
   
   > [!NOTE]
   > O logon único precisa ser habilitado pela equipe de suporte do Huddle.
   > Assim que a configuração for concluída, você receberá uma notificação.
   > 
   > 
5. No portal clássico do Azure, selecione a confirmação da configuração de logon único e clique em **Concluir** para fechar a caixa de diálogo **Configurar logon único**.
   
   ![Configurar Logon Único](./media/active-directory-saas-huddle-tutorial/IC787837.png "Configure Single Sign-On")
   
   ## <a name="configuring-user-provisioning"></a>Configurando o provisionamento de usuários

Para permitir que os usuários do Azure AD façam logon no Huddle, eles deverão ser provisionados no Huddle.  
No caso do Huddle, o provisionamento é uma tarefa manual.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Para configurar o provisionamento de usuários, execute as seguintes etapas:
1. Faça logon em seu site de empresa do **Huddle** como administrador.
2. Clique em **Espaço de trabalho**.
3. Clique em **Pessoas \> Convidar Pessoas**.
   
   ![Pessoas](./media/active-directory-saas-huddle-tutorial/IC787838.png "People")
4. Na seção **Criar novo convite** , realize as seguintes etapas:
   
   ![Novo Convite](./media/active-directory-saas-huddle-tutorial/IC787839.png "New Invitation")
   
   1. Na lista **Escolha uma equipe para convidar pessoas para participar**, selecione **equipe**.
   2. Digite o **Endereço de Email** de uma conta válida do AAD que você deseja provisionar nas caixas de texto relacionadas.
   3. Clique em **Convidar**.
   
   > [!NOTE]
   > O titular da conta do Azure AD receberá um email com um link de confirmação de conta para que ela se torne ativa.
   > 
   > 

> [!NOTE]
> É possível usar qualquer outra ferramenta de criação da conta de usuário do Huddle ou as APIs fornecidas pelo Huddle para provisionar as contas de usuário do AAD.
> 
> 

## <a name="assigning-users"></a>Atribuindo usuários
Para testar sua configuração, será necessário conceder acesso ao aplicativo aos usuários do Azure AD que você deseja que o utilizem.

### <a name="to-assign-users-to-huddle-perform-the-following-steps"></a>Para atribuir usuários ao Huddle, execute as seguintes etapas:
1. No Portal clássico do Azure, crie uma conta de teste.
2. Na página de integração do aplicativo **Huddle**, clique em **Atribuir usuários**.
   
   ![Atribuir usuários](./media/active-directory-saas-huddle-tutorial/IC787840.png "Assign Users")
3. Selecione seu usuário de teste, clique em **Atribuir** e, em seguida, clique em **Sim** para confirmar a atribuição.
   
   ![Sim](./media/active-directory-saas-huddle-tutorial/IC767830.png "Yes")

Se você quiser testar suas configurações de logon único, abra o Painel de Acesso. Para obter mais detalhes sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Nov16_HO3-->


