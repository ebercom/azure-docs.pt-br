---
title: "Tutorial: Integração do Azure Active Directory com o SD Elements | Microsoft Docs"
description: "Saiba como configurar o logon único entre o Active Directory do Azure e o SD Elements."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: f0386307-bb3b-4810-8d4b-d0bfebda04f4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/18/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 53aa0a84a7f22c8cda5144eb6e1b82f38b72acb8


---
# <a name="tutorial-azure-active-directory-integration-with-sd-elements"></a>Tutorial: Integração do Active Directory do Azure com SD Elements
O objetivo desse tutorial é mostrar como integrar o SD Elements ao Azure Active Directory (Azure AD).  
A integração do SD Elements ao Azure AD oferece os seguintes benefícios:

* No AD do Azure, você pode controlar quem tem acesso ao SD Elements
* Você pode habilitar seus usuários a fazerem logon automaticamente em SD Elements (logon único) com suas contas do AD do Azure
* Você pode gerenciar suas contas em um único local: o Active Directory do Azure 

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao AD do Azure, consulte [O que é o acesso a aplicativos e logon único com o Active Directory do Azure](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Pré-requisitos
Para configurar a integração do AD do Azure a SD Elements, você precisa dos seguintes itens:

* Uma assinatura do AD do Azure
* Uma logon único de SD Elements na assinatura habilitada

> [!NOTE]
> Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.
> 
> 

Para testar as etapas deste tutorial, você deve seguir estas recomendações:

* Não use o ambiente de produção, a menos que seja necessário.
* Se não tiver um ambiente de avaliação do AD do Azure, você pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
O objetivo deste tutorial é permitir que você teste o logon único do Azure AD em um ambiente de teste.  
O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando elementos SD da galeria
2. Configurar e testar o logon único do AD do Azure

## <a name="adding-sd-elements-from-the-gallery"></a>Adicionando elementos SD da galeria
Para configurar a integração de SD Elements ao Azure AD, você precisa adicionar SD Elements da galeria à sua lista de aplicativos de SaaS gerenciados.

**Para adicionar SD Elements da galeria, execute as seguintes etapas:**

1. No **portal clássico do Azure**, no painel de navegação à esquerda, clique em **Active Directory**. 
   
    ![Active Directory][1]

2. Na lista **Diretório** , selecione o diretório para o qual você deseja habilitar a integração de diretórios.

3. Para abrir a visualização dos aplicativos, na exibição do diretório, clique em **Aplicativos** no menu principal.
   
    ![Aplicativos][2]

4. Clique em **Adicionar** na parte inferior da página.
   
    ![Aplicativos][3]

5. Na caixa de diálogo **O que você deseja fazer**, clique em **Adicionar um aplicativo da galeria**.
   
    ![Aplicativos][4]

6. Na caixa de pesquisa, digite **SD Elements**.
   
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_01.png)

7. No painel de resultados, selecione **SD Elements** e clique em **Concluir** para adicionar o aplicativo.
   
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_02.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configurar e testar o logon único do AD do Azure
O objetivo desta seção é mostrar como configurar e testar logon único do Azure AD com o SD Elements com base em um usuário de teste chamado "Brenda Fernandes".

Para que o logon único funcione, o Azure AD precisa saber qual usuário do SD Elements é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do SD Elements.  
Essa relação de vínculo é estabelecida atribuindo o valor do **nome de usuário** no Azure AD como o valor do **Nome de usuário** no SD Elements.

Para configurar e testar o logon único do Azure AD com o SD Elements, é preciso concluir os seguintes blocos de construção:

1. **[Configuração do logon único do AD do Azure](#configuring-azure-ad-single-single-sign-on)** – para habilitar seus usuários a usar esse recurso.
2. **[Criação de um usuário de teste do AD do Azure](#creating-an-azure-ad-test-user)** - para testar o logon único do AD do Azure com Brenda Fernandes.
3. **[Criar um usuário de teste de elementos de SD](#creating-a-sd-elements-test-user)** : para ter um equivalente de Brenda Fernandes no SD Elements que esteja vinculado à representação dela no Azure AD.
4. **[Atribuição do usuário de teste do AD do Azure](#assigning-the-azure-ad-test-user)** - para permitir que Brenda Fernandes use o logon único do AD do Azure.
5. **[Teste do logon único](#testing-single-sign-on)** : para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do logon único do AD do Azure
O objetivo desta seção é habilitar o logon único do Azure AD no portal clássico do Azure AD e configurar o logon único em seu aplicativo SD Elements.

Seu aplicativo SD Elements espera as declarações do SAML em um formato específico, o que exige adicionar mapeamentos de atributo personalizados à configuração de **atributos do token saml** . A captura de tela a seguir mostra um exemplo disso:

![Configurar Logon Único](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_14.png) 

**Para configurar o logon único do Azure AD com o SD Elements, execute as seguintes etapas:**

1. No portal clássico do Azure, na página de integração de aplicativos do **SD Elements**, clique em **Configurar logon único** para abrir a caixa de diálogo **Configurar Logon Único**.
   
    ![Configurar Logon Único][6] 

2. Na página **Como você deseja que os usuários façam logon no SD Elements**, selecione **Logon Único do Azure AD** e clique em **Avançar**.
   
    ![Configurar Logon Único](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_03.png) 

3. Na página do diálogo **Definir Configurações do Aplicativo** , realize as seguintes etapas:
   
    ![Configurar Logon Único](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_04.png) 

    a. Na caixa de texto **Emissor**, digite a URL do emissor do locatário usando o seguinte padrão: *https://\<nome do locatário\>.sdelements.com/sso/saml2/metadata*

    b. Na caixa de texto **URL de Resposta**, digite a URL de resposta do locatário usando o seguinte padrão: *https://\< nome do locatário\>.sdelements.com/sso/saml2/acs/*       

    > [!NOTE] 
    > Se você precisar da URL do emissor e da URL de resposta reais para o seu locatário, entre em contato com a [equipe de suporte do SD Elements](mailto:support@sdelements.com).

    c. Clique em **Avançar**.


1. Na página **Configurar logon único no SD Elements** , execute as seguintes etapas:
   
    ![Configurar Logon Único](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_05.png) 
   
    a. Clique em **Baixar certificado**e salve o arquivo em seu computador.
   
    b. Clique em **Avançar**.
2. Para habilitar logon único, entre em contato com a [equipe de suporte do SD Elements](mailto:support@sdelements.com) e forneça a ela o arquivo de certificado baixado.
3. Em uma janela de navegador diferente, efetue logon no locatário do SD Elements como administrador.
4. No menu na parte superior, clique em Sistema e, em seguida, Logon Único. 
   
    ![Configurar Logon Único](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_09.png) 
5. Na caixa de diálogo **Configurações de Logon Único** , execute as seguintes etapas:
   
    ![Configurar Logon Único](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_10.png) 
   
    a. Como **Tipo de SSO**, selecione **SAML**.
   
    b. No portal clássico do Azure, na página da caixa de diálogo **Configurar logon único no SD Elements**, copie o valor da **URL do Emissor** e cole-o na caixa de texto **ID de Entidade do Provedor de Identidade**.
   
    c. No portal clássico do Azure, na página da caixa de diálogo **Configurar logon único no SD Elements**, copie o valor da **URL do Serviço de Logon Único** e cole-o na caixa de texto **Serviço de Logon Único do Provedor de Identidade**.
   
    d. Clique em **Salvar**.
6. No portal clássico do Azure, selecione a confirmação da configuração de logon único e, em seguida, clique em **Avançar**.
   
    ![Logon Único do AD do Azure][10]
7. Na página **Confirmação de logon único**, clique em **Concluir**.  
   
    ![Logon Único do AD do Azure][11]
8. Na parte superior do menu, clique em **Atributos** to open the **SAML Token Atributos** . 
   
    ![Configurar o logon único][21]
9. Para cada linha na tabela a seguir, execute as seguintes etapas:
   
   | Nome do atributo | Valor do atributo |
   | --- | --- |
   | email |user.mail |
   | nome |user.givenname |
   | sobrenome |user.surname |

    a. Clique em **adicionar atributo de usuário**. 

    ![Configurar Logon Único][23]

    b. Na caixa de texto **Nome do Atributo**, digite o **Nome do Atributo** e, como **Valor do Atributo**, selecione o Valor do Atributo mostrado para a linha.

    ![Configurar Logon Único][22]

    c. Clique em **adicionar atributo de usuário**. 

    ![Configurar Logon Único][23]

1. Clique em **Aplicar alterações**. 
   
    ![Configurar Logon Único][24]

### <a name="creating-an-azure-ad-test-user"></a>Criação de um usuário de teste do AD do Azure
O objetivo desta seção é criar um usuário de teste no Portal Clássico do Azure chamado Brenda Fernandes.  

![Criar um usuário do AD do Azure][20]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No **portal clássico do Azure**, no painel de navegação à esquerda, clique em **Active Directory**.
   
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_09.png) 
2. Na lista **Diretório** , selecione o diretório para o qual você deseja habilitar a integração de diretórios.
3. Para exibir a lista de usuários, no menu na parte superior, clique em **Usuários**.
   
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_03.png) 
4. Para abrir a caixa de diálogo **Adicionar Usuário**, na barra de ferramentas na parte inferior, clique em **Adicionar Usuário**.
   
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_04.png) 
5. Na página do diálogo **Conte-nos sobre este usuário** , realize as seguintes etapas:
   
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_05.png) 
   
    a. Em Tipo de Usuário, selecione Novo usuário na organização.
   
    b. Na **caixa de texto** Nome do Usuário, digite **BrendaFernandes**.
   
    c. Clique em **Próximo**.
6. Na página do diálogo **Perfil do Usuário** , realize as seguintes etapas:
   
   ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_06.png) 
   
   a. Na caixa de texto **Nome**, digite **Brenda**.  
   
   b. Na caixa de texto **Sobrenome**, digite **Fernandes**.
   
   c. Na caixa de texto **Nome de Exibição**, digite **Brenda Fernandes**.
   
   d. Na lista **Função**, selecione **Usuário**.
   
   e. Clique em **Próximo**.
7. Na página de diálogo **Obter senha temporária**, clique em **criar**.
   
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_07.png) 
8. Na página de caixa de diálogo **Obter senha temporária** , execute as seguintes etapas:
   
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_08.png) 
   
    a. Anote o valor da **Nova Senha**.
   
    b. Clique em **Concluído**.   

### <a name="creating-a-sd-elements-test-user"></a>Criar um usuário de teste de elementos de SD
O objetivo desta seção é criar um usuário chamado Brenda Fernandes no SD Elements. No caso de SD Elements, criar usuários do SD Elements é uma tarefa manual.

**Para criar Brenda Fernandes no SD Elements, execute as seguintes etapas:**

1. Em uma janela de navegador da web, faça logon no site SD Elements da sua empresa como administrador.
2. No menu na parte superior, clique em Gerenciamento de Usuários e então em Usuários.
   
   ![Criar um usuário de teste de elementos de SD](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_11.png) 
3. Clique em Adicionar Novo Usuário.
   
   ![Criar um usuário de teste de elementos de SD](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_12.png) 
4. Na caixa de diálogo Adicionar Novo Usuário, execute as seguintes etapas:
   
   ![Criar um usuário de teste de elementos de SD](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_13.png) 
   
   a. Na caixa de texto **Email** , digite o endereço de email de Brenda no AD do Azure.
   
   b. Na caixa de texto **Nome**, digite **Brenda**.
   
   c. Na caixa de texto **Sobrenome**, digite **Fernandes**.
   
   d. Como **Função**, selecione **Usuário**. 
   
   e. Clique em **Criar Usuário**.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuição do usuário de teste do AD do Azure
O objetivo desta seção é habilitar Brenda Fernandes a usar o logon único do Azure, concedendo a ela acesso ao SD Elements.

![Atribuir usuário][200] 

**Para atribuir Brenda Fernandes ao SD Elements, execute as seguintes etapas:**

1. No portal clássico do Azure, para abrir o modo de exibição de aplicativos, no modo de exibição de diretório, clique em **Aplicativos** no menu superior.
   
    ![Atribuir usuário][201] 
2. Na lista de aplicativos, selecione **SD Elements**.
   
    ![Configurar Logon Único](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_50.png) 
3. No menu na parte superior, clique em **Usuários**.
   
    ![Atribuir usuário][203] 
4. Na lista **Usuários**, selecione **Brenda Fernandes**.
5. Na barra de ferramentas na parte inferior, clique em **Atribuir**.
   
    ![Atribuir usuário][205]

### <a name="testing-single-sign-on"></a>Teste do logon único
O objetivo desta seção é testar sua configuração de logon único do Azure AD usando o Painel de Acesso.  
Quando você clica no bloco SD Elements no Painel de Acesso, deve fazer logon automaticamente no seu aplicativo SD Elements.

## <a name="additional-resources"></a>Recursos adicionais
* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_100.png

[21]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_80.png
[22]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_82.png
[23]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_81.png
[24]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_83.png


[200]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_205.png



<!--HONumber=Nov16_HO3-->


