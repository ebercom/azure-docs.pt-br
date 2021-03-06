---
title: "Tutorial: Integração do Azure Active Directory com o Picturepark | Microsoft Docs"
description: "Saiba como usar o Picturepark com o Active Directory do Azure para habilitar o logon único, provisionamento automatizado e muito mais!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 31c21cd4-9c00-4cad-9538-a13996dc872f
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/26/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 98f2e5596a0af2fc9e633e005642cc3cd21621ce


---
# <a name="tutorial-azure-active-directory-integration-with-picturepark"></a>Tutorial: Integração do Active Directory do Azure com o Picturepark
O objetivo deste tutorial é mostrar a integração do Azure com o Picturepark.  
O cenário descrito neste tutorial pressupõe que você já tem os seguintes itens:

* Uma assinatura válida do Azure
* Um locatário do Picturepark

Depois de concluir este tutorial, os usuários do Azure AD atribuídos ao Picturepark poderão fazer logon único no aplicativo em seu site de empresa do Picturepark (logon iniciado pelo provedor de serviços) ou usando a [Introdução ao Painel de Acesso](active-directory-saas-access-panel-introduction.md).

O cenário descrito neste tutorial consiste nos seguintes blocos de construção:

1. Habilitando a integração de aplicativos para Picturepark
2. Configurando o logon único
3. Configurando o provisionamento de usuários
4. Atribuindo usuários

![Cenário](./media/active-directory-saas-picturepark-tutorial/IC795055.png "Scenario")

## <a name="enabling-the-application-integration-for-picturepark"></a>Habilitando a integração de aplicativos para Picturepark
O objetivo desta seção é descrever como habilitar a integração de aplicativos com o Picturepark.

### <a name="to-enable-the-application-integration-for-picturepark-perform-the-following-steps"></a>Para habilitar a integração de aplicativos com o Picturepark, execute as seguintes etapas:
1. No Portal clássico do Azure, no painel de navegação à esquerda, clique em **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-picturepark-tutorial/IC700993.png "Active Directory")
2. Na lista **Diretório** , selecione o diretório para o qual você deseja habilitar a integração de diretórios.
3. Para abrir a visualização dos aplicativos, na exibição do diretório, clique em **Aplicativos** no menu principal.
   
   ![Aplicativos](./media/active-directory-saas-picturepark-tutorial/IC700994.png "Applications")
4. Clique em **Adicionar** na parte inferior da página.
   
   ![Adicionar aplicativo](./media/active-directory-saas-picturepark-tutorial/IC749321.png "Add application")
5. Na caixa de diálogo **O que você deseja fazer**, clique em **Adicionar um aplicativo da galeria**.
   
   ![Adicionar um aplicativo da galeria](./media/active-directory-saas-picturepark-tutorial/IC749322.png "Add an application from gallerry")
6. Na **caixa de pesquisa**, digite **Picturepark**.
   
   ![Galeria de Aplicativos](./media/active-directory-saas-picturepark-tutorial/IC795056.png "Application Gallery")
7. No painel de resultados, selecione **Picturepark** e clique em **Concluir** para adicionar o aplicativo.
   
   ![Picturepark](./media/active-directory-saas-picturepark-tutorial/IC795057.png "Picturepark")

## <a name="configuring-single-sign-on"></a>Configurando o logon único
O objetivo desta seção é descrever como permitir que os usuários se autentiquem no Picturepark com sua conta do AD do Azure usando federação baseada em protocolo SAML.  
A configuração do logon único para Picturepark exige que você recupere um valor de impressão digital de um certificado.  
Se você não estiver familiarizado com esse procedimento, veja [Como recuperar o valor de impressão digital de um certificado](http://youtu.be/YKQF266SAxI).

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Para configurar o logon único, execute as seguintes etapas:
1. No portal clássico do Azure, na página de integração de aplicativos do **Picturepark**, clique em **Configurar logon único** para abrir o diálogo **Configurar Logon Único**.
   
   ![Configurar Logon Único](./media/active-directory-saas-picturepark-tutorial/IC795058.png "Configure Single Sign-On")
2. Na página **Como você deseja que os usuários façam logon no Picturepark**, selecione **Logon Único do Microsoft Azure AD** e clique em **Avançar**.
   
   ![Configurar Logon Único](./media/active-directory-saas-picturepark-tutorial/IC795059.png "Configure Single Sign-On")
3. Na página **Configurar a URL do Aplicativo**, na caixa de texto **URL de Logon do Picturepark**, digite a URL usando o padrão "*http://company.picturepark.com*" e clique em **Avançar**.
   
   ![Configurar a URL do Aplicativo](./media/active-directory-saas-picturepark-tutorial/IC795060.png "Configure App URL")
4. Na página **Configurar logon único no Picturepark**, para baixar seu certificado, clique em **Baixar certificado** e salve o arquivo de certificado localmente no computador.
   
   ![Configurar o logon único](./media/active-directory-saas-picturepark-tutorial/IC795061.png "Configure Single Sign-On")
5. Em uma janela de navegador da Web diferente, faça logon no site de sua empresa do Picturepark como administrador.
6. Na barra de ferramentas na parte superior, clique em **Ferramentas administrativas** e em **Console de Gerenciamento**.
   
   ![Console de Gerenciamento](./media/active-directory-saas-picturepark-tutorial/IC795062.png "Management Console")
7. Clique em **Autenticação** e em **Provedores de identidade**.
   
   ![Autenticação](./media/active-directory-saas-picturepark-tutorial/IC795063.png "Authentication")
8. Na seção **Configuração do provedor de identidade** , realize as seguintes etapas:
   
   ![Configuração do provedor de identidade](./media/active-directory-saas-picturepark-tutorial/IC795064.png "Identity provider configuration")
   
   1. Clique em **Adicionar**.
   2. Digite um nome para sua configuração.
   3. Selecione **Definir como padrão**.
   4. No portal clássico do Azure, na página de diálogo **Configurar logon único no Picturepark**, copie o valor da **URL de SSO do SAML** e cole-o na caixa de texto **URI do Emissor**.
   5. Copie o valor de **Impressão Digital** do certificado exportado e cole-o na caixa de texto **Impressão Digital do Emissor Confiável**.  
      
      > [!TIP]
      > Para obter mais detalhes, consulte [Como recuperar o valor de impressão digital de um certificado](http://youtu.be/YKQF266SAxI)
      > 
      > 
   6. Clique em **JoinDefaultUsersGroup**.
   7. Para definir o atributo **Emailaddress** na caixa de texto **Declaração**, digite **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
      ![Configuração](./media/active-directory-saas-picturepark-tutorial/IC795065.png "Configuration")
   8. Clique em **Salvar**.
9. No portal clássico do Azure, selecione a confirmação da configuração de logon único e clique em **Concluir** para fechar a caixa de diálogo **Configurar logon único**.
   
   ![Configurar Logon Único](./media/active-directory-saas-picturepark-tutorial/IC795066.png "Configure Single Sign-On")

## <a name="configuring-user-provisioning"></a>Configurando o provisionamento de usuários
Para permitir que os usuários do AD do Azure façam logon no Picturepark, eles devem ser provisionados no Picturepark.  
No caso do Picturepark, o provisionamento é uma tarefa manual.

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Para provisionar contas de usuário, execute as seguintes etapas:
1. Faça logon em seu locatário do **Picturepark** .
2. Na barra de ferramentas na parte superior, clique em **Ferramentas administrativas** e em **Usuários**.
   
   ![Usuários](./media/active-directory-saas-picturepark-tutorial/IC795067.png "Users")
3. Na guia **Visão geral de usuários**, clique em **Novo**.
   
   ![Gerenciamento de Usuários](./media/active-directory-saas-picturepark-tutorial/IC795068.png "User management")
4. No diálogo **Criar Usuário** , realize as seguintes etapas:
   
   ![Criar Usuário](./media/active-directory-saas-picturepark-tutorial/IC795069.png "Create User")
   
   1. Digite o **Endereço de Email**, **Senha**, **Confirmar Senha**, **Nome**, **Sobrenome**, **Empresa**, **País**, **CEP**, **Cidade** de um usuário válido do Active Directory do Azure que você deseja provisionar nas caixas de texto relacionadas.
   2. Selecione um **Idioma**.
   3. Clique em **Criar**.

> [!NOTE]
> É possível usar qualquer outra ferramenta de criação da conta de usuário do Picturepark ou as APIs fornecidas pelo Picturepark para provisionar as contas de usuário do AAD.
> 
> 

## <a name="assigning-users"></a>Atribuindo usuários
Para testar sua configuração, é necessário conceder acesso ao aplicativo aos usuários do Azure AD que você deseja que usem seu aplicativo.

### <a name="to-assign-users-to-picturepark-perform-the-following-steps"></a>Para atribuir usuários ao Picturepark, execute as seguintes etapas:
1. No Portal clássico do Azure, crie uma conta de teste.
2. Na página de integração de aplicativos do **Picturepark**, clique em **Atribuir usuários**.
   
   ![Atribuir usuários](./media/active-directory-saas-picturepark-tutorial/IC795070.png "Assign Users")
3. Selecione seu usuário de teste, clique em **Atribuir** e, em seguida, clique em **Sim** para confirmar a atribuição.
   
   ![Sim](./media/active-directory-saas-picturepark-tutorial/IC767830.png "Yes")

Se você quiser testar suas configurações de logon único, abra o Painel de Acesso. Para obter mais detalhes sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Nov16_HO3-->


