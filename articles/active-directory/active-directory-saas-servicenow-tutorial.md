---
title: 'Tutorial: Integração do Active Directory do Azure ao ServiceNow | Microsoft Docs'
description: Aprenda a usar o ServiceNow com o Active Directory do Azure para habilitar o logon único, o provisionamento automatizado e muito mais.
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila

ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/26/2016
ms.author: jeedes

---
# <a name="tutorial:-azure-active-directory-integration-with-servicenow"></a>Tutorial: Integração do Active Directory do Azure com o ServiceNow
O objetivo deste tutorial é mostrar a integração do Azure ao ServiceNow.  
O cenário descrito neste tutorial pressupõe que você já tem os seguintes itens:

* Uma assinatura válida do Azure
* Um locatário em ServiceNow, versão Calgary ou superior
* O locatário ServiceNow deve ter o [Plug-in de Logon Único de Provedor Múltiplo](http://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0) habilitado. Isso pode ser feito enviando-se uma solicitação de serviço em https://hi.service-now.com/ 

Depois de concluir este tutorial, os usuários do Azure AD atribuídos ao ServiceNow poderão fazer logon único no aplicativo em seu site de empresa do ServiceNow (logon iniciado pelo provedor de serviços) ou usando a [Introdução ao Painel de Acesso](active-directory-saas-access-panel-introduction.md)

O cenário descrito neste tutorial consiste nos seguintes blocos de construção:

1. Habilitando a integração de aplicativos para o ServiceNow
2. Configurando o logon único
3. Configurando o provisionamento de usuários
4. Atribuindo usuários

![Cenário](./media/active-directory-saas-servicenow-tutorial/IC769496.png "Scenario")

## <a name="enabling-the-application-integration-for-servicenow"></a>Habilitando a integração de aplicativos para o ServiceNow
O objetivo desta seção é descrever como habilitar a integração de aplicativos para o ServiceNow.

### <a name="to-enable-the-application-integration-for-servicenow,-perform-the-following-steps:"></a>Para habilitar a integração de aplicativos com o ServiceNow, execute as seguintes etapas:
1. No Portal clássico do Azure, no painel de navegação à esquerda, clique em **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-servicenow-tutorial/IC700993.png "Active Directory")
2. Na lista **Diretório** , selecione o diretório para o qual você deseja habilitar a integração de diretórios.
3. Para abrir a visualização dos aplicativos, na exibição do diretório, clique em **Aplicativos** no menu principal.
   
   ![Aplicativos](./media/active-directory-saas-servicenow-tutorial/IC700994.png "Applications")
4. Clique em **Adicionar** na parte inferior da página.
   
   ![Adicionar aplicativo](./media/active-directory-saas-servicenow-tutorial/IC749321.png "Add application")
5. Na caixa de diálogo **O que você deseja fazer**, clique em **Adicionar um aplicativo da galeria**.
   
   ![Adicionar um aplicativo da galeria](./media/active-directory-saas-servicenow-tutorial/IC749322.png "Add an application from gallerry")
6. Na **caixa de pesquisa**, digite **ServiceNow**.
   
   ![Galeria de aplicativos](./media/active-directory-saas-servicenow-tutorial/IC701016.png "Application gallery")
7. No painel de resultados, selecione **ServiceNow** e clique em **Concluir** para adicionar o aplicativo.
   
   ![ServiceNow](./media/active-directory-saas-servicenow-tutorial/IC701017.png "ServiceNow")
   
   ## <a name="configuring-single-sign-on"></a>Configurando o logon único

O objetivo desta seção é descrever como permitir que os usuários se autentiquem no ServiceNow com sua conta do AD do Azure usando federação baseada em protocolo SAML.

Como parte deste procedimento, será necessário carregar um certificado codificado em base-64 no locatário do Dropbox for Business. Se você não estiver familiarizado com esse procedimento, veja [Como converter um certificado binário em um arquivo de texto](http://youtu.be/PlgrzUZ-Y1o).

### <a name="to-configure-single-sign-on,-perform-the-following-steps:"></a>Para configurar o logon único, execute as seguintes etapas:
1. No portal clássico do Azure AD, na página de integração de aplicativos do **ServiceNow**, clique em **Configurar logon único** para abrir o diálogo **Configurar Logon Único**.
   
   ![Configurar o logon único](./media/active-directory-saas-servicenow-tutorial/IC749323.png "Configure single sign-on")
2. Na página **Como você deseja que os usuários façam logon no ServiceNow**, selecione **Logon Único do Microsoft Azure AD** e clique em **Avançar**.
   
   ![Configurar o logon único](./media/active-directory-saas-servicenow-tutorial/IC749324.png "Configure single sign-on")
3. Na página **Definir Configurações do Aplicativo** , execute as seguintes etapas:
   
   ![Configurar a URL do aplicativo](./media/active-directory-saas-servicenow-tutorial/IC769497.png "Configure app URL")
   
   a. Na caixa de texto **URL de logon ServiceNow**, digite a URL usada pelos usuários para entrar no seu aplicativo ServiceNow (por exemplo: *https://\<InstanceName\>.service-now.com*).
   
   b. Na caixa de texto **URL do Emissor**, digite a URL usada pelos usuários para entrar no seu aplicativo ServiceNow (por exemplo: *https://\<InstanceName\>.service-now.com*).
   
   c. Clique em **Avançar**
4. Para que o Azure AD configure automaticamente o ServiceNow para autenticação baseada em SAML, insira o nome da instância do ServiceNow, o nome de usuário do administrador e a senha de administrador no formulário **Configurar automaticamente o logon único** e clique em *Configurar*. Observe que o nome de usuário do administrador informado deve ter a função **security_admin** atribuída no ServiceNow para que isso funcione. Caso contrário, para configurar manualmente o ServiceNow para usar o Azure AD como provedor de identidade SAML, clique em **Configurar manualmente o aplicativo para o logon único**, em **Avançar** e conclua as etapas a seguir.
   
   ![Configurar a URL do aplicativo](./media/active-directory-saas-servicenow-tutorial/IC7694971.png "Configure app URL")
5. Na página **Configurar logon único no ServiceNow**, clique em **Baixar certificado**, salve o arquivo de certificado no computador e clique em **Avançar**.
   
   ![Configurar o logon único](./media/active-directory-saas-servicenow-tutorial/IC749325.png "Configure single sign-on")
6. Entre no seu aplicativo ServiceNow como administrador.
7. No painel de navegação à esquerda, clique em **Propriedades**.  
   
    ![Configurar a URL do aplicativo](./media/active-directory-saas-servicenow-tutorial/IC7694980.png "Configure app URL")
8. No diálogo **Várias propriedades de SSO do provedor** , execute as seguintes etapas:
   
    ![Configurar a URL do aplicativo](./media/active-directory-saas-servicenow-tutorial/IC7694981.png "Configure app URL")
   
    a. Como **Habilitar vários provedores SSO**, selecione **Sim**.
   
    b. Como **Habilitar log de depuração com integração SSO de vários provedores**, selecione **Sim**.
   
    c. Na caixa de texto **O campo na tabela de usuário que...**, digite **nome_de_usuário**.
   
    d. Clique em **Salvar**.
9. No painel de navegação à esquerda, clique em **Certificados x509**.
   
    ![Configurar o logon único](./media/active-directory-saas-servicenow-tutorial/IC7694973.png "Configure single sign-on")
10. No diálogo **Certificados x. 509**, clique em **Novo**.
    
     ![Configurar o logon único](./media/active-directory-saas-servicenow-tutorial/IC7694974.png "Configure single sign-on")
11. No diálogo **Certificados x. 509** , execute as seguintes etapas:
    
     ![Configurar o logon único](./media/active-directory-saas-servicenow-tutorial/IC7694975.png "Configure single sign-on")
    
     a. Clique em **Novo**.
    
     b. Na caixa de texto **Nome**, digite um nome para a sua configuração (por exemplo: **TestSAML2.0**).
    
     c. Selecione **Ativo**.
    
     d. Para **Formato**, selecione **PEM**.
    
     e. Como **Tipo**, selecione **Confiar nos Certificados do Repositório**.
    
     f. Crie um arquivo codificado em base-64 usando o certificado baixado.
    
    > [!NOTE]
    > Para obter mais detalhes, consulte [Como converter um certificado binário em um arquivo de texto](http://youtu.be/PlgrzUZ-Y1o).
    > 
    > 
    
     g. Abra seu certificado codificado base 64 no bloco de notas, copie o conteúdo dele na área de transferência e cole-o na caixa de texto **Certificado PEM** .
    
     h. Clique em **Atualizar**.
12. No painel de navegação à esquerda, clique em **Provedores de Identidade**.
    
     ![Configurar o logon único](./media/active-directory-saas-servicenow-tutorial/IC7694976.png "Configure single sign-on")
13. No diálogo **Provedores de Identidade**, clique em **Novo**:
    
     ![Configurar o logon único](./media/active-directory-saas-servicenow-tutorial/IC7694977.png "Configure single sign-on")
14. No diálogo **Provedores de Identidade**, clique em **SAML2 Update1?**:
    
     ![Configurar o logon único](./media/active-directory-saas-servicenow-tutorial/IC7694978.png "Configure single sign-on")
15. No diálogo Propriedades de SAML2 Atualização1, execute as seguintes etapas:
    
     ![Configurar o logon único](./media/active-directory-saas-servicenow-tutorial/IC7694982.png "Configure single sign-on")

    a. Na caixa de texto **Nome** , digite um nome para a sua configuração (por exemplo: **SAML 2.0**).

    b. Na caixa de texto **Campo de Usuário**, digite **email** ou **user_id**, dependendo de qual campo é usado para identificar exclusivamente os usuários em sua implantação do ServiceNow. 

    **Observação:** você pode configurar o Azure AD para emitir a ID de usuário (nome UPN) do Azure AD ou o endereço de email como o identificador exclusivo no token SAML acessando a seção **ServiceNow > Atributos > Logon Único** do portal clássico do Azure e mapeando o campo desejado para o atributo **nameidentifier**. O valor armazenado para o atributo selecionado no Azure AD (por exemplo, nome UPN) deve corresponder ao valor armazenado no ServiceNow para o campo inserido (por exemplo, user_id)

    c. No portal clássico do Azure AD, copie o valor de **ID do Provedor de Identidade** e cole-o na caixa de texto **URL do Provedor de Identidade**.

    d. No portal clássico do Azure AD, copie o valor de **URL de Solicitação de Autenticação** e cole-o na caixa de texto **Solicitação de Autenticação do Provedor de Identidade**.

    e. No portal clássico do Azure AD, copie o valor de **URL de Serviço de Saída Única** e cole-o na caixa de texto **Solicitação de Logoff Único do Provedor de Identidade**.

    f. Na caixa de texto **Home page do ServiceNow** , digite a URL da sua página inicial de instância do ServiceNow.

    > [AZURE.NOTE] A página inicial da instância do ServiceNow é uma concatenação da **URL de locatário do ServiceNow** e **/navpage.do** (por exemplo: *https://fabrikam.service-now.com/navpage.do*).


    g. Na caixa de texto **ID da entidade/emissor** , digite a URL do seu locatário do ServiceNow.

    h. Na caixa de texto **URL do Público-alvo** , digite a URL do seu locatário do ServiceNow. 

    i. Na caixa de texto **associação de protocolo SingleLogoutRequest do IDP**, digite **urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect**.

    j. Na caixa de texto Política da NameID, digite **urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified**.

    k. Desmarque **Criar um AuthnContextClass**.

    l. No **método AuthnContextClassRef**, digite **http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password**.

    m. Na caixa de texto **Distorção do Relógio**, digite **60**.

    n. Como **Script de Logon Único**, selecione **MultiSSO_SAML2_Update1**.

    o. Como **Certificado x509**, selecione o certificado que você criou na etapa anterior.

    p. Clique em **Enviar**. 



1. No portal clássico do Azure AD, selecione a confirmação de configuração do logon único e clique em **Avançar**. 
   
    ![Configurar o logon único](./media/active-directory-saas-servicenow-tutorial/IC7694990.png "Configure single sign-on")
2. Na página **Confirmação de logon único**, clique em **Concluir**.
   
    ![Configurar o logon único](./media/active-directory-saas-servicenow-tutorial/IC7694991.png "Configure single sign-on")

## <a name="configuring-user-provisioning"></a>Configurando o provisionamento de usuários
O objetivo desta seção é descrever como habilitar o provisionamento de contas de usuário do Active Directory no ServiceNow.

### <a name="to-configure-user-provisioning,-perform-the-following-steps:"></a>Para configurar o provisionamento de usuários, execute as seguintes etapas:
1. No portal clássico de Gerenciamento do Azure, na página de integração de aplicativos do **ServiceNow**, clique em **Configurar provisionamento de usuários**. 
   
    ![Provisionamento do usuário](./media/active-directory-saas-servicenow-tutorial/IC769498.png "User provisioning")
2. Na página **Inserir suas credenciais do ServiceNow para habilitar o provisionamento automático de usuário** , forneça as seguintes configurações: Configurar Provisionamento de Usuário 
   
     a. Na caixa de texto **Nome de Instância do ServiceNow** , digite o nome da instância do ServiceNow.
   
     b. Na caixa de texto **Nome de Usuário Administrador do ServiceNow** , digite o nome da conta de administrador do ServiceNow.
   
     c. Na caixa de texto **Senha de Administrador do ServiceNow** , digite a senha desta conta.
   
     d. Clique em **validar** para verificar sua configuração.
   
     e. Clique no botão **Avançar** para abrir a página **Próximas etapas**.
   
     f. Se você deseja provisionar todos os usuários neste aplicativo, escolha "**Provisionar automaticamente todas as contas de usuário no diretório para este aplicativo**". 
   
    ![Próximas etapas](./media/active-directory-saas-servicenow-tutorial/IC698804.png "Next Steps")
   
     g. Na página **Próximas etapas**, clique em **Concluir** para salvar sua configuração.

## <a name="assigning-users"></a>Atribuindo usuários
Para testar sua configuração, é necessário conceder acesso ao aplicativo aos usuários do Azure AD que você deseja que usem seu aplicativo.

### <a name="to-assign-users-to-servicenow,-perform-the-following-steps:"></a>Para atribuir usuários ao ServiceNow, execute as seguintes etapas:
1. No portal clássico do AD do Azure, crie uma conta de teste.
2. Na página de integração de aplicativos do **ServiceNow**, clique **Atribuir usuários**.
   
   ![Atribuir usuários](./media/active-directory-saas-servicenow-tutorial/IC769499.png "Assign users")
3. Selecione seu usuário de teste, clique em **Atribuir** e, em seguida, clique em **Sim** para confirmar a atribuição.
   
   ![Sim](./media/active-directory-saas-servicenow-tutorial/IC767830.png "Yes")

Se você quiser testar suas configurações de logon único, abra o Painel de Acesso. Para obter mais detalhes sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais
* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--HONumber=Oct16_HO2-->


