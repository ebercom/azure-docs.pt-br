---
title: "Tutorial: Integração do Azure Active Directory ao Slack | Microsoft Docs"
description: "Saiba como usar o Slack com o Active Directory do Azure para habilitar o logon único, provisionamento automatizado e muito mais!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: c7a0b761-75b7-4e6b-9980-71d645643a78
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/21/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 4d5144c6a690c2338dec45b27dcb308328b6fecb
ms.openlocfilehash: 37440a8ba397c4dc227a448dfa574cebd14be49c


---

# <a name="tutorial-azure-active-directory-integration-with-slack"></a>Tutorial: Integração do Active Directory do Azure com o Slack

O objetivo desse tutorial é mostrar como integrar o Slack ao Azure AD (Azure Active Directory).

A integração do Slack ao Azure AD oferece os seguintes benefícios:

- Você pode controlar no Azure AD quem terá acesso ao Slack
- Você pode permitir que seus usuários façam logon automaticamente no Slack (logon único) com suas contas do Azure AD
- Você pode gerenciar suas contas em um único local: o Portal clássico do Azure

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao AD do Azure, consulte [O que é o acesso a aplicativos e logon único com o Active Directory do Azure](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD ao Slack, você precisará dos seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura do Slack habilitada para logon único


> [!NOTE]
>  Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.


Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do AD do Azure, você pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Descrição do cenário
O objetivo deste tutorial é permitir que você teste o logon único do Azure AD em um ambiente de teste.

O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar o Slack da galeria
2. Configurar e testar o logon único do AD do Azure


## <a name="adding-slack-from-the-gallery"></a>Adicionar o Slack da galeria
Para configurar a integração do Slack ao Azure AD, você precisa adicionar o Slack por meio da galeria à sua lista de aplicativos SaaS gerenciados.

**Para adicionar o Slack por meio da galeria, execute as seguintes etapas:**

1. No **Portal clássico do Azure**, no painel de navegação à esquerda, clique em **Active Directory**. 

    ![Active Directory][1]

2. Na lista **Diretório** , selecione o diretório para o qual você deseja habilitar a integração de diretórios.

3. Para abrir a visualização dos aplicativos, na exibição do diretório, clique em **Aplicativos** no menu principal.
    
    ![Aplicativos][2]

4. Clique em **Adicionar** na parte inferior da página.
    
    ![Aplicativos][3]

5. Na caixa de diálogo **O que você deseja fazer**, clique em **Adicionar um aplicativo da galeria**.

    ![Aplicativos][4]

6. Na caixa de pesquisa, digite **Slack**.

    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-slack-tutorial/tutorial_slack_01.png)

7. No painel de resultados, selecione **Slack** e clique em **Concluir** para adicionar o aplicativo.

    ![Seleção do aplicativo na galeria](./media/active-directory-saas-slack-tutorial/tutorial_slack_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>configurar e testar o logon único do AD do Azure
O objetivo desta seção é mostrar como configurar e testar o logon único do Azure AD com o Slack, com base em um usuário de teste chamado “Brenda Fernandes”.

Para que o logon único funcione, o Azure AD precisa saber qual usuário do Slack é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do Slack.

Essa relação de vínculo é estabelecida atribuindo o valor de **nome de usuário** no Azure AD como o valor de **Nome de usuário** no Slack.

Para configurar e testar o logon único do Azure AD com o Slack, você precisa concluir os seguintes blocos de construção:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** : para habilitar seus usuários a usar esse recurso.
2. **[Criação de um usuário de teste do AD do Azure](#creating-an-azure-ad-test-user)** - para testar logon único do Azure AD com Britta Simon.
3. **[Criar um usuário de teste do Slack](#creating-a-slack-test-user)** - para ter um equivalente de Brenda Fernandes no Slack que esteja vinculado à representação dela no Azure AD.
4. **[Atribuição do usuário de teste do AD do Azure](#assigning-the-azure-ad-test-user)** : para permitir que Brenda Fernandes use o logon único do AD do Azure.
5. **[Testing Single Sign-On](#testing-single-sign-on)** : para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do logon único do Azure AD

O objetivo desta seção é habilitar o logon único do Azure AD no Portal Clássico do Azure e configurar o logon único em seu aplicativo Slack.

O aplicativo Slack espera que as declarações SAML estejam em um formato específico. Configure as seguintes declarações para o aplicativo. Você pode gerenciar o valor dos atributos na guia**"Atributo"**do aplicativo. A captura de tela a seguir mostra um exemplo disso. 

![Configurar o logon único](./media/active-directory-saas-slack-tutorial/tutorial_slack_09.png)

**Para configurar o logon único do Azure AD com o Slack, realize as seguintes etapas:**

1. No Portal Clássico do Azure, na página de integração do aplicativo **Slack**, no menu superior, clique em **Atributos**.

    ![Configurar Logon Único](./media/active-directory-saas-slack-tutorial/tutorial_slack_03.png)

2. Na caixa de diálogo **Atributos de token SAML** , para cada linha mostrada na tabela a seguir, execute as seguintes etapas:

    | Nome do atributo | Valor do atributo |
    | --- | --- |    
    | User.Email | user.userprincipalname |
    | first_name | user.givenname |
    | last_name | user.surname |
    | User.Username | extractmailprefix([userprincipalname]) |

    a. Clique em **adicionar atributo de usuário** para abrir a caixa de diálogo **Adicionar Atributo de Usuário**.

    ![Configurar Logon Único](./media/active-directory-saas-slack-tutorial/tutorial_slack_04.png)
    
    b. Na caixa de texto **Nome do Atributo** , digite o nome do atributo mostrado para essa linha.
    
    c. Na lista **Valor do Atributo** , digite o valor do atributo mostrado para essa linha.
    
    d. Clique em **Concluído**

3. No menu na parte superior, clique em **Início Rápido**.

    ![Configurar Logon Único](./media/active-directory-saas-slack-tutorial/tutorial_slack_05.png) 

4. Na página **Como você deseja que os usuários façam logon no Slack**, selecione **Logon Único do Azure AD**e clique em **Avançar**.
    
    ![Configurar Logon Único](./media/active-directory-saas-slack-tutorial/tutorial_slack_06.png)

5. Na página de diálogo **Definir Configurações do Aplicativo**, execute as seguintes etapas e clique em **Avançar**:

    ![Configurar o logon único](./media/active-directory-saas-slack-tutorial/tutorial_slack_07.png)

    a. Na caixa de texto **URL de Logon**, digite uma URL usando o seguinte padrão: `https://<company name>.slack.com`.

    b. Clique em **Próximo**.

    > [!NOTE] 
    > Observe que você precisa atualizar o valor com a URL de Entrada real. Para obter esse valor, entre em contato com a equipe de suporte do Slack.

6. Na página **Configurar logon único no Slack**, clique em **Baixar certificado** e salve o arquivo no computador.

    ![Configurar o logon único](./media/active-directory-saas-slack-tutorial/tutorial_slack_08.png)

7.  Em outra janela do navegador da Web, faça logon em seu site de empresa Slack como um administrador.

8.  Navegue até **Microsoft Azure AD** e depois em **Configurações de Equipe**.

    ![Configurar o logon único no lado do aplicativo](./media/active-directory-saas-slack-tutorial/tutorial_slack_001.png)

9.  Na seção **Configurações de Equipe**, clique na guia **Autenticação** e em **Alterar Configurações**.

    ![Configurar o logon único no lado do aplicativo](./media/active-directory-saas-slack-tutorial/tutorial_slack_002.png)

10. No diálogo **Configurações de Autenticação SAML** , realize as seguintes etapas:

    ![Configurar o logon único no lado do aplicativo](./media/active-directory-saas-slack-tutorial/tutorial_slack_003.png)

    a.  Na caixa de texto **Ponto de extremidade SAML 2.0 (HTTP)**, coloque o valor de **URL de SSO do SAML** do assistente de configuração de aplicativo do Azure AD.

    b.  Na caixa de texto **Emissor do Provedor de Identidade**, insira o valor de **URL do Emissor** no assistente de configuração de aplicativo do Azure AD.

    c.  Abra seu arquivo de certificado baixado no bloco de notas, copie o conteúdo dele na área de transferência e cole-o na caixa de texto **Certificado Público**.

    d.  Desmarque **Permitir que os usuários alterem seu endereço de email**.

    e.  Selecione **Permitir que os usuários escolham seu próprio nome de usuário**.

    f.  Para **A autenticação de sua equipe deve ser usada por**, selecione **É opcional**.

    g.  Clique em **Salvar Configuração**.

11. No portal clássico, selecione a confirmação da configuração de logon único e clique em **Avançar**.
    
    ![Logon Único do AD do Azure][10]

12. Na página **Confirmação de logon único**, clique em **Concluir**.  
    
    ![Logon Único do AD do Azure][11]



### <a name="creating-an-azure-ad-test-user"></a>Criação de um usuário de teste do AD do Azure
O objetivo desta seção é criar um usuário de teste no Portal Clássico do Azure chamado Brenda Fernandes.

![Criar um usuário do AD do Azure][20]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No **Portal clássico do Azure**, no painel de navegação à esquerda, clique em **Active Directory**.

    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-slack-tutorial/create_aaduser_09.png)

2. Na lista **Diretório** , selecione o diretório para o qual você deseja habilitar a integração de diretórios.

3. Para exibir a lista de usuários, no menu na parte superior, clique em **Usuários**.
    
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-slack-tutorial/create_aaduser_03.png)

4. Para abrir a caixa de diálogo **Adicionar Usuário**, na barra de ferramentas na parte inferior, clique em **Adicionar Usuário**.

    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-slack-tutorial/create_aaduser_04.png)

5. Na página do diálogo **Conte-nos sobre este usuário** , realize as seguintes etapas:

    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-slack-tutorial/create_aaduser_05.png)

    a. Em Tipo de Usuário, selecione Novo usuário na organização.

    b. Na **caixa de texto** Nome do Usuário, digite **BrendaFernandes**.

    c. Clique em **Próximo**.

6.  Na página do diálogo **Perfil do Usuário** , realize as seguintes etapas:
    
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-slack-tutorial/create_aaduser_06.png)

    a. Na caixa de texto **Nome**, digite **Brenda**.  

    b. Na caixa de texto **Sobrenome**, digite **Fernandes**.

    c. Na caixa de texto **Nome de Exibição**, digite **Brenda Fernandes**.

    d. Na lista **Função**, selecione **Usuário**.

    e. Clique em **Próximo**.

7. Na página de diálogo **Obter senha temporária**, clique em **criar**.
    
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-slack-tutorial/create_aaduser_07.png)

8. Na página de caixa de diálogo **Obter senha temporária** , execute as seguintes etapas:
    
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-slack-tutorial/create_aaduser_08.png)

    a. Anote o valor da **Nova Senha**.

    b. Clique em **Concluído**.   



### <a name="creating-a-slack-test-user"></a>Criar um usuário de teste do Slack

O objetivo desta seção é criar um usuário chamado Brenda Fernandes no Slack. O Slack dá suporte ao provisionamento just-in-time, que está habilitado por padrão.

Não há itens de ação para você nesta seção. Um novo usuário será criado durante uma tentativa de acessar o Slack, caso ele ainda não exista.

> [!NOTE] 
> Se você precisar criar um usuário manualmente, entre em contato com a equipe de suporte do Slack.


### <a name="assigning-the-azure-ad-test-user"></a>Atribuição do usuário de teste do AD do Azure

O objetivo desta seção é permitir que Brenda Fernandes use o logon único do Azure, concedendo a ela acesso ao Slack.
    
![Atribuir usuário][200]

**Para atribuir Brenda Fernandes ao Slack, realize as seguintes etapas:**

1. No portal clássico, para abrir o modo de exibição de aplicativos, no modo de exibição de diretório, clique em **Aplicativos** no menu superior.
    
    ![Atribuir usuário][201]

2. Na lista de aplicativos, escolha **Slack**.
    
    ![Configurar Logon Único](./media/active-directory-saas-slack-tutorial/tutorial_slack_50.png)

3. No menu na parte superior, clique em **Usuários**.
    
    ![Atribuir usuário][203]

4. Na lista de usuários, selecione **Brenda Fernandes**.

5. Na barra de ferramentas na parte inferior, clique em **Atribuir**.
    
    ![Atribuir usuário][205]

### <a name="testing-single-sign-on"></a>Teste do logon único

O objetivo desta seção é testar sua configuração de logon único do Azure AD usando o Painel de Acesso.
 
Ao clicar no bloco Slack no Painel de Acesso, você deverá ser conectado automaticamente ao aplicativo de Slack.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-slack-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-slack-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-slack-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-slack-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-slack-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-slack-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-slack-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-slack-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-slack-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-slack-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-slack-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-slack-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-slack-tutorial/tutorial_general_205.png



<!--HONumber=Nov16_HO4-->


