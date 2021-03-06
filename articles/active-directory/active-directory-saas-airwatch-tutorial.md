---
title: "Tutorial: integração do Azure Active Directory com o AirWatch | Microsoft Docs"
description: "Saiba como usar o AirWatch com o Active Directory do Azure para habilitar o logon único, provisionamento automatizado e muito mais!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 96a3bb1c-96c6-40dc-8ea0-060b0c2a62e5
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/29/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 5f2030abf8b6c2d5884f4c5c2cb29d89ff329227


---
# <a name="tutorial-azure-active-directory-integration-with-airwatch"></a>Tutorial: Integração do Active Directory do Azure ao AirWatch
O objetivo deste tutorial é mostrar a integração do Azure ao AirWatch.  
O cenário descrito neste tutorial pressupõe que você já tem os seguintes itens:

* Uma assinatura válida do Azure
* Uma assinatura habilitada para logon único do AirWatch

Depois de concluir este tutorial, os usuários do AD do Azure atribuídos ao AirWatch poderão fazer logon único no aplicativo em seu site de empresa do AirWatch (logon iniciado pelo provedor de serviços) ou usando a [Introdução ao Painel de Acesso](active-directory-saas-access-panel-introduction.md).

O cenário descrito neste tutorial consiste nos seguintes blocos de construção:

1. Habilitando a integração do aplicativo para o AirWatch
2. Configurando o logon único
3. Configurando o provisionamento de usuários
4. Atribuindo usuários

![AirWatch](./media/active-directory-saas-airwatch-tutorial/IC791913.png "AirWatch")

## <a name="enabling-the-application-integration-for-airwatch"></a>Habilitando a integração do aplicativo para o AirWatch
O objetivo desta seção é descrever como habilitar a integração de aplicativos para o AirWatch.

### <a name="to-enable-the-application-integration-for-airwatch-perform-the-following-steps"></a>Para habilitar a integração de aplicativos para o AirWatch, execute as seguintes etapas:
1. No Portal clássico do Azure, no painel de navegação à esquerda, clique em **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-airwatch-tutorial/IC700993.png "Active Directory")
2. Na lista **Diretório** , selecione o diretório para o qual você deseja habilitar a integração de diretórios.
3. Para abrir a visualização dos aplicativos, na exibição do diretório, clique em **Aplicativos** no menu principal.
   
   ![Aplicativos](./media/active-directory-saas-airwatch-tutorial/IC700994.png "Applications")
4. Clique em **Adicionar** na parte inferior da página.
   
   ![Adicionar aplicativo](./media/active-directory-saas-airwatch-tutorial/IC749321.png "Add application")
5. Na caixa de diálogo **O que você deseja fazer**, clique em **Adicionar um aplicativo da galeria**.
   
   ![Adicionar um aplicativo da galeria](./media/active-directory-saas-airwatch-tutorial/IC749322.png "Add an application from gallerry")
6. Na **caixa de pesquisa**, digite **AirWatch**.
   
   ![Galeria de Aplicativos](./media/active-directory-saas-airwatch-tutorial/IC791914.png "Application Gallery")
7. No painel de resultados, selecione **AirWatch** e clique em **Concluir** para adicionar o aplicativo.
   
   ![AirWatch](./media/active-directory-saas-airwatch-tutorial/IC791915.png "AirWatch")
   
   ## <a name="configuring-single-sign-on"></a>Configurando o logon único

O objetivo desta seção é descrever como permitir que os usuários se autentiquem no AirWatch com a respectiva conta do AD do Azure usando federação baseada no protocolo SAML.  
Como parte deste procedimento, é necessário criar um arquivo de certificado codificado em base 64.  
Se você não estiver familiarizado com este procedimento, consulte [Como converter um certificado binário em um arquivo de texto](http://youtu.be/PlgrzUZ-Y1o).

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Para configurar o logon único, execute as seguintes etapas:
1. No portal clássico do Azure, na página de integração do aplicativo **AirWatch**, clique em **Configurar logon único** para abrir o diálogo **Configurar Logon Único**.
   
   ![Configurar Logon Único](./media/active-directory-saas-airwatch-tutorial/IC791916.png "Configure Single Sign-On")
2. Na página **Como você deseja que os usuários façam logon no AirWatch**, selecione **Logon Único do Microsoft Azure AD** e clique em **Avançar**.
   
   ![Configurar Logon Único](./media/active-directory-saas-airwatch-tutorial/IC791917.png "Configure Single Sign-On")
3. Na página **Configurar URL do Aplicativo**, na caixa de texto **URL de Logon do AirWatch**, digite a URL usada pelos usuários para entrar no aplicativo AirWatch (por exemplo: "*https:// companycode.awmdm.com/AirWatch/Login?gid=companycode*") e clique em **Avançar**.
   
   ![Configurar a URL do Aplicativo](./media/active-directory-saas-airwatch-tutorial/IC791918.png "Configure App URL")
4. Na página **Configurar logon único no AirWatch**, clique em **Baixar certificado** e salve o arquivo de certificado no computador.
   
   ![Configurar o logon único](./media/active-directory-saas-airwatch-tutorial/IC791919.png "Configure Single Sign-On")
5. Em outra janela do navegador da Web, faça logon em seu site de empresa AirWatch! como um administrador.
6. No painel de navegação à esquerda, clique em **Contas** e em **Administradores**.
   
   ![Administradores](./media/active-directory-saas-airwatch-tutorial/IC791920.png "Administrators")
7. Expanda o menu **Configurações** e clique em **Serviços de Diretório**.
   
   ![Configurações](./media/active-directory-saas-airwatch-tutorial/IC791921.png "Settings")
8. Clique na guia **Usuário**, no campo de texto **DN Base**, digite seu nome de domínio e clique em **Salvar**.
   
   ![Usuário](./media/active-directory-saas-airwatch-tutorial/IC791922.png "User")
9. Clique na guia **Servidor** .
   
   ![Servidor](./media/active-directory-saas-airwatch-tutorial/IC791923.png "Server")
10. Execute as seguintes etapas:
    
    ![Carregar](./media/active-directory-saas-airwatch-tutorial/IC791924.png "Upload")
    
    1. Para **Tipo de Diretório**, selecione **Nenhum**.
    2. Selecione **Usar SAML para Autenticação**.
    3. Para carregar o certificado baixado, clique em **Carregar**.
11. Na seção **Solicitar** , realize as seguintes etapas:
    
    ![Solicitação](./media/active-directory-saas-airwatch-tutorial/IC791925.png "Request")
    
    1. Para **Tipo de Associação de Solicitação**, selecione **POST**.
    2. No portal clássico do Azure, na página do diálogo **Configurar logon único no AirWatch**, copie o valor da **URL do Serviço de Logon Único** e cole-o na caixa de texto **URL de Logon Único do Provedor de Identidade**.
    3. Para **Formato de NameID**, selecione **Endereço de Email**.
    4. Clique em **Salvar**.
12. Clique na guia **Usuário** novamente.
    
    ![Usuário](./media/active-directory-saas-airwatch-tutorial/IC791926.png "User")
13. Na seção **Atributo** , realize as seguintes etapas:
    
    ![Atributo](./media/active-directory-saas-airwatch-tutorial/IC791927.png "Attribute")
    
    1. Na caixa de texto **Identificador de Objeto**, digite **http://schemas.microsoft.com/identity/claims/objectidentifier**.
    2. Na caixa de texto **Nome de Usuário**, digite **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    3. Na caixa de texto **Nome de Exibição**, digite **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.
    4. Na caixa de texto **Nome**, digite **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.
    5. Na caixa de texto **Sobrenome**, digite **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.
    6. Na caixa de texto **Email**, digite **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    7. Clique em **Salvar**.
14. No portal clássico do Azure, selecione a confirmação da configuração de logon único e clique em **Concluir** para fechar a caixa de diálogo **Configurar logon único**.
    
    ![Configurar Logon Único](./media/active-directory-saas-airwatch-tutorial/IC791928.png "Configure Single Sign-On")
    
    ## <a name="configuring-user-provisioning"></a>Configurando o provisionamento de usuários

Para permitir que os usuários do AD do Azure façam logon no AirWatch, eles devem ser provisionados no AirWatch.  
No caso do AirWatch, o provisionamento é uma tarefa manual.

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Para provisionar contas de usuário, execute as seguintes etapas:
1. Faça logon em seu site de empresa do **AirWatch** como administrador.
2. No painel de navegação à esquerda, clique em **Contas** e em **Usuários**.
   
   ![Usuários](./media/active-directory-saas-airwatch-tutorial/IC791929.png "Users")
3. No menu **Usuários**, clique em **Exibição de Lista** e em **Adicionar \> Adicionar Usuário**.
   
   ![Adicionar usuário](./media/active-directory-saas-airwatch-tutorial/IC791930.png "Add User")
4. No diálogo **Adicionar/Editar Usuário** , realize as seguintes etapas:
   
   ![Adicionar usuário](./media/active-directory-saas-airwatch-tutorial/IC791931.png "Add User")
   
   1. Digite **Nome**, **Senha**, **Confirmar Senha**, **Nome**, **Sobrenome** e **Endereço de Email** de uma conta válida do Azure Active Directory que você deseja provisionar nas caixas de texto relacionadas.
   2. Clique em **Salvar**.

> [!NOTE]
> É possível usar qualquer outra ferramenta de criação da conta de usuário do AirWatch ou as APIs fornecidas pelo AirWatch para provisionar as contas de usuário do AAD.
> 
> 

## <a name="assigning-users"></a>Atribuindo usuários
Para testar sua configuração, é necessário conceder acesso ao aplicativo aos usuários do Azure AD que você deseja que usem seu aplicativo.

### <a name="to-assign-users-to-airwatch-perform-the-following-steps"></a>Para atribuir usuários ao AirWatch, execute as seguintes etapas:
1. No Portal clássico do Azure, crie uma conta de teste.
2. Na página de integração de aplicativos **AirWatch**, clique em **Atribuir usuários**.
   
   ![Atribuir usuários](./media/active-directory-saas-airwatch-tutorial/IC791932.png "Assign Users")
3. Selecione seu usuário de teste, clique em **Atribuir** e, em seguida, clique em **Sim** para confirmar a atribuição.
   
   ![Sim](./media/active-directory-saas-airwatch-tutorial/IC767830.png "Yes")

Se você quiser testar suas configurações de logon único, abra o Painel de Acesso. Para obter mais detalhes sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Nov16_HO3-->


