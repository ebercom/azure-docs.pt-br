---
title: "Tutorial: Integração do Azure Active Directory ao ScreenSteps | Microsoft Docs"
description: "Saiba como usar o ScreenSteps com o Active Directory do Azure para habilitar o logon único, provisionamento automatizado e muito mais!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 4563fe94-a88f-4895-a07f-79df44889cf9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/26/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 9e097ed265381225deeda19642c281223907a4cc


---
# <a name="tutorial-azure-active-directory-integration-with-screensteps"></a>Tutorial: Integração do Active Directory do Azure com o ScreenSteps
O objetivo deste tutorial é mostrar a integração do Azure ao ScreenSteps.  
O cenário descrito neste tutorial pressupõe que você já tem os seguintes itens:

* Uma assinatura válida do Azure
* Um locatário do ScreenSteps

Depois de concluir este tutorial, os usuários do Azure AD atribuídos ao ScreenSteps poderão fazer logon único no aplicativo em seu site de empresa do ScreenSteps (logon iniciado pelo provedor de serviços) ou usando a [Introdução ao Painel de Acesso](active-directory-saas-access-panel-introduction.md).

O cenário descrito neste tutorial consiste nos seguintes blocos de construção:

1. Habilitando a integração de aplicativos para o ScreenSteps
2. Configurando o logon único
3. Configurando o provisionamento de usuários
4. Atribuindo usuários

![Cenário](./media/active-directory-saas-screensteps-tutorial/IC778516.png "Scenario")

## <a name="enabling-the-application-integration-for-screensteps"></a>Habilitando a integração de aplicativos para o ScreenSteps
O objetivo desta seção é descrever como habilitar a integração de aplicativos com o ScreenSteps.

### <a name="to-enable-the-application-integration-for-screensteps-perform-the-following-steps"></a>Para habilitar a integração de aplicativos com o ScreenSteps, execute as seguintes etapas:
1. No Portal clássico do Azure, no painel de navegação à esquerda, clique em **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-screensteps-tutorial/IC700993.png "Active Directory")
2. Na lista **Diretório** , selecione o diretório para o qual você deseja habilitar a integração de diretórios.
3. Para abrir a visualização dos aplicativos, na exibição do diretório, clique em **Aplicativos** no menu principal.
   
   ![Aplicativos](./media/active-directory-saas-screensteps-tutorial/IC700994.png "Applications")
4. Clique em **Adicionar** na parte inferior da página.
   
   ![Adicionar aplicativo](./media/active-directory-saas-screensteps-tutorial/IC749321.png "Add application")
5. Na caixa de diálogo **O que você deseja fazer**, clique em **Adicionar um aplicativo da galeria**.
   
   ![Adicionar um aplicativo da galeria](./media/active-directory-saas-screensteps-tutorial/IC749322.png "Add an application from gallerry")
6. Na **caixa de pesquisa**, digite **ScreenSteps**.
   
   ![Galeria de aplicativos](./media/active-directory-saas-screensteps-tutorial/IC778517.png "Application gallery")
7. No painel de resultados, selecione **ScreenSteps** e clique em **Concluir** para adicionar o aplicativo.
   
   ![ScreenSteps](./media/active-directory-saas-screensteps-tutorial/IC778518.png "ScreenSteps")
   
   ## <a name="configuring-single-sign-on"></a>Configurando o logon único

O objetivo desta seção é descrever como permitir que os usuários se autentiquem no ScreenSteps com sua conta do AD do Azure usando federação baseada no protocolo SAML.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Para configurar o logon único, execute as seguintes etapas:
1. No portal clássico do Azure, na página de integração de aplicativos do **ScreenSteps**, clique em **Configurar logon único** para abrir a caixa de diálogo **Configurar Logon Único**.
   
   ![Configurar o logon único](./media/active-directory-saas-screensteps-tutorial/IC778519.png "Configure single sign-on")
2. Na página **Como você deseja que os usuários façam logon no ScreenSteps**, selecione **Logon Único do Microsoft Azure AD** e clique em **Avançar**.
   
   ![Configurar o logon único](./media/active-directory-saas-screensteps-tutorial/IC778520.png "Configure single sign-on")
3. Na página **Configurar URL do Aplicativo**, na caixa de texto **URL de Logon do ScreenSteps**, digite a URL usando o padrão "*https://\<<nome-do-locatário>\>.ScreenSteps.com*" e clique em **Avançar**.
   
   ![Configurar a URL do aplicativo](./media/active-directory-saas-screensteps-tutorial/IC778521.png "Configure app URL")
4. Na página **Configurar logon único no ScreenSteps**, para baixar seu certificado, clique em **Baixar certificado** e salve o arquivo de certificado no computador.
   
   ![Configurar o logon único](./media/active-directory-saas-screensteps-tutorial/IC778522.png "Configure single sign-on")
5. Em outra janela do navegador da Web, faça logon em seu site de empresa ScreenSteps como um administrador.
6. Clique em **Gerenciamento de Contas**.
   
   ![Gerenciamento de Contas](./media/active-directory-saas-screensteps-tutorial/IC778523.png "Account management")
7. Clique em **Autenticação Remota**.
   
   ![Autenticação Remota](./media/active-directory-saas-screensteps-tutorial/IC778524.png "Remote authentication")
8. Clique em **Criar ponto de extremidade de autenticação**.
   
   ![Autenticação Remota](./media/active-directory-saas-screensteps-tutorial/IC778525.png "Remote authentication")
9. Na seção **Criar um Ponto de Extremidade de Autenticação** , realize as seguintes etapas:
   
   ![Criar um Ponto de Extremidade de Autenticação](./media/active-directory-saas-screensteps-tutorial/IC778526.png "Create an authentication endpoint")
   
   1. Na caixa de texto **Título** , digite um título.
   2. Na lista **Modo**, selecione **SAML**.
   3. Clique em **Criar**.
10. Na seção **Ponto de Extremidade de Autenticação Remota** , realize as seguintes etapas:
    
    ![Ponto de Extremidade de Autenticação Remota](./media/active-directory-saas-screensteps-tutorial/IC778527.png "Remote authentication endpoint")
    
    1. No portal clássico do Azure, na página **Configurar logon único no ScreenSteps**, copie o valor da **URL de Logon Remoto** e cole-o na caixa de texto **URL de Logon Remoto**.
    2. No portal clássico do Azure, na página **Configurar logon único no ScreenSteps**, copie o valor da **URL de Logoff Remoto** e cole-o na caixa de texto **URL de Logoff**.
    3. Clique em **Escolher um arquivo**e carregue o certificado baixado.
    4. Clique em **Atualizar**.
11. No portal clássico do Azure, selecione a confirmação da configuração de logon único e clique em **Concluir** para fechar a caixa de diálogo **Configurar logon único**.
    
    ![Configurar o logon único](./media/active-directory-saas-screensteps-tutorial/IC778542.png "Configure single sign-on")
    
    ## <a name="configuring-user-provisioning"></a>Configurando o provisionamento de usuários

Para permitir que os usuários do Azure AD façam logon no **ScreenSteps**, eles devem ser provisionados no **ScreenSteps**.  
No caso do **ScreenSteps**, o provisionamento é uma tarefa manual.

### <a name="to-provision-a-user-account-to-screensteps-perform-the-following-steps"></a>Para provisionar uma conta de usuário no ScreenSteps, execute as seguintes etapas:
1. Faça logon em seu locatário do **ScreenSteps** .
2. Clique em **Gerenciamento de Contas**.
   
   ![Gerenciamento de Contas](./media/active-directory-saas-screensteps-tutorial/IC778523.png "Account management")
3. Clique em **Usuários**.
   
   ![Usuários](./media/active-directory-saas-screensteps-tutorial/IC778544.png "Users")
4. Clique em **Criar um usuário**.
   
   ![Todos os Usuários](./media/active-directory-saas-screensteps-tutorial/IC778545.png "All Users")
5. Na lista **Função de Usuário** , selecione uma função para o usuário.
6. Na seção Função de Usuário, digite o "**Nome**,** Sobrenome**, **Email**, **Logon**, **Senha** e **Configuração da Senha**" de uma conta válida do AAD que você deseja provisionar nas caixas de texto relacionadas.
   
   ![Novo usuário](./media/active-directory-saas-screensteps-tutorial/IC778546.png "New user")
7. Na seção Grupos, selecione **Grupo de Autenticação (SAML)** e clique em **Criar Usuário**.
   
   ![Grupos](./media/active-directory-saas-screensteps-tutorial/IC778547.png "Groups")

> [!NOTE]
> É possível usar qualquer outra ferramenta de criação da conta de usuário do ScreenSteps ou as APIs fornecidas pelo ScreenSteps para provisionar as contas de usuário do AAD.
> 
> 

## <a name="assigning-users"></a>Atribuindo usuários
Para testar sua configuração, é necessário conceder acesso ao aplicativo aos usuários do Azure AD que você deseja que usem seu aplicativo.

### <a name="to-assign-users-to-screensteps-perform-the-following-steps"></a>Para atribuir usuários ao ScreenSteps, execute as seguintes etapas:
1. No Portal clássico do Azure, crie uma conta de teste.
2. Na página de integração de aplicativos do **ScreenSteps**, clique em **Atribuir usuários**.
   
   ![Atribuir usuários](./media/active-directory-saas-screensteps-tutorial/IC773094.png "Assign users")
3. Selecione seu usuário de teste, clique em **Atribuir** e, em seguida, clique em **Sim** para confirmar a atribuição.
   
   ![Atribuir usuários](./media/active-directory-saas-screensteps-tutorial/IC778548.png "Assign users")

Se você quiser testar suas configurações de logon único, abra o Painel de Acesso. Para obter mais detalhes sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Nov16_HO3-->


