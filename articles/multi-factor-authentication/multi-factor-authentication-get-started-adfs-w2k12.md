---
title: Servidor do MFA com AD FS do Windows Server 2012 R2 | Microsoft Docs
description: "Este artigo descreve como começar a usar o Azure Multi-Factor Authentication e o AD FS no Windows Server 2012 R2."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 57208068-1e55-45b6-840f-fdcd13723074
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/14/2016
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 86a4bc7ea89416f2c67626439f08fa615a2e6511


---
# <a name="secure-your-cloud-and-onpremises-resources-using-azure-multifactor-authentication-server-with-ad-fs-in-windows-server-2012-r2"></a>Proteger seus recursos de nuvem e locais usando o Servidor de Autenticação Multifator do Azure com o AD FS do Windows Server 2012 R2
Se você usa o AD FS (Serviços de Federação do Active Directory) e deseja proteger recursos na nuvem ou locais, pode configurar o servidor de Autenticação Multifator do Azure para trabalhar com o AD FS. Essa configuração dispara a verificação em duas etapas para pontos de extremidade de alto valor.

Neste artigo, discutimos o uso do Servidor Azure Multi-Factor Authentication com o AD FS no Windows Server 2012 R2. Para saber mais, leia sobre como [proteger recursos de nuvem e locais usando o Servidor Azure Multi-Factor Authentication com o AD FS 2.0](multi-factor-authentication-get-started-adfs-adfs2.md).

## <a name="secure-windows-server-2012-r2-ad-fs-with-azure-multifactor-authentication-server"></a>Proteger o AD FS do Windows Server 2012 R2 com o Servidor Azure Multi-Factor Authentication
Ao instalar o Servidor Azure Multi-Factor Authentication, você tem as seguintes opções:

* Instalar o Servidor Azure Multi-Factor Authentication localmente no mesmo servidor que o AD FS
* Instalar o adaptador do Azure Multi-Factor Authentication localmente no servidor do AD FS e instalar o Servidor Multi-Factor Authentication em outro computador

Antes de começar, esteja ciente das seguintes informações:

* Não é necessário instalar o Servidor Azure Multi-Factor Authentication no servidor do AD FS. No entanto, você deve instalar o adaptador do Multi-Factor Authentication para o AD FS no Windows Server 2012 R2 que está executando o AD FS. Você pode instalar o servidor em outro computador, desde que seja uma versão com suporte, e instalar o adaptador do AD FS separadamente no seu servidor de federação do AD FS. Confira os procedimentos a seguir para aprender a instalar o adaptador separadamente.
* Quando o adaptador do MFA server do AD FS foi projetado, foi previsto que o AD FS poderia passar o nome da terceira parte confiável para o adaptador. Em seguida, o nome da terceira parte confiável poderia ser usado como um nome de aplicativo. No entanto, isso não aconteceu. Se sua organização estiver usando mensagens de texto ou de métodos de verificação de aplicativos móveis, as cadeias de caracteres definidas em Configurações da Empresa contêm um espaço reservado <$*nome_do_aplicativo*$>. Esse espaço reservado não será substituído automaticamente quando você usar o adaptador do AD FS. É recomendável que você remova o espaço reservado de cadeias de caracteres apropriadas ao proteger o AD FS.
* A conta que você usa para entrar deve ter direitos de usuário para criar grupos de segurança no serviço do Active Directory.
* O assistente de instalação de adaptador do AD FS de Autenticação Multifator cria um grupo de segurança chamado PhoneFactor Admins na sua instância do Active Directory. Em seguida, ele adiciona a conta de serviço do seu serviço de Federação do AD FS a esse grupo. É recomendável que você verifique no controlador de domínio que o grupo PhoneFactor Admins realmente é criado e se a conta de serviço do AD FS é membro desse grupo. Se necessário, adicione manualmente a conta de serviço do AD FS ao grupo PhoneFactor Admins no controlador de domínio.
* Para saber mais sobre como instalar o SDK do Serviço Web com o portal do usuário, leia sobre a [implantação do portal do usuário para o Servidor Azure Multi-Factor Authentication.](multi-factor-authentication-get-started-portal.md)

### <a name="install-azure-multifactor-authentication-server-locally-on-the-ad-fs-server"></a>Instalar o Servidor Azure Multi-Factor Authentication localmente no mesmo servidor que o AD FS
1. Baixe e instale o Servidor de Autenticação Multifator do Microsoft Azure no servidor do AD FS. Para obter informações sobre instalação, leia sobre a [introdução ao Servidor Azure Multi-Factor Authentication](multi-factor-authentication-get-started-server.md).
2. No console de gerenciamento do Servidor de Autenticação Multifator do Microsoft Azure, clique no ícone **AD FS** e selecione as opções **Permitir registro do usuário** e **Permitir que usuários selecionem o método**.
3. Selecione as opções adicionais que você deseja especificar para a sua organização.
4. Clique em **Instalar Adaptador do AD FS**.
   <center>![Nuvem](./media/multi-factor-authentication-get-started-adfs-w2k12/server.png)</center>
5. Se a janela do Active Directory for exibida, isso significará duas coisas. O computador faz parte de um domínio e a configuração do Active Directory para proteger a comunicação entre o adaptador do AD FS e o serviço de autenticação multifator está incompleta. Clique em **Avançar** para concluir automaticamente essa configuração ou marque a caixa de seleção **Ignorar configuração automática do Active Directory e definir as configurações manualmente** e clique em **Avançar**.
6. Se as janelas de Grupo Local forem exibidas, isso significará duas coisas. Seu computador não faz parte de um domínio e a configuração do grupo local para proteger a comunicação entre o adaptador do AD FS e o serviço de autenticação multifator está incompleta. Clique em **Avançar** para concluir automaticamente essa configuração ou marque a caixa de seleção **Ignorar configuração automática do Grupo Local e definir as configurações manualmente** e clique em **Avançar**.
7. No Assistente de instalação, clique em **Avançar**. O Servidor Azure Multi-Factor Authentication cria o grupo PhoneFactor Admins e adiciona a conta de serviço do AD FS ao grupo PhoneFactor Admins.
   <center>![Nuvem](./media/multi-factor-authentication-get-started-adfs-w2k12/adapter.png)</center>
8. Na página **Iniciar Instalador**, clique em **Avançar**.
9. No instalador do adaptador do AD FS do Multi-Factor Authentication, clique em **Avançar**.
10. Clique em **Fechar** quando a instalação for concluída.
11. Quando o adaptador tiver sido instalado, você deverá registrá-lo no AD FS. Abra PowerShell e execute o seguinte comando:<br>
    `C:\Program Files\Multi-Factor Authentication Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1`
    <center>![Nuvem](./media/multi-factor-authentication-get-started-adfs-w2k12/pshell.png)</center>
12. Para usar o adaptador recém-registrado, edite a política de autenticação global no AD FS. No console de gerenciamento do AD FS, vá até o nó **Políticas de Autenticação** . Na seção **Autenticação Multifator**, clique no link **Editar** próximo à seção **Configurações Globais**. Na janela **Editar Política de Autenticação Global**, selecione **Autenticação Multifator** como um método de autenticação adicional e clique em **OK**. O adaptador é registrado como WindowsAzureMultiFactorAuthentication. Reinicie o serviço AD FS para que o registro entre em vigor.

<center>![Nuvem](./media/multi-factor-authentication-get-started-adfs-w2k12/global.png)</center>

Neste ponto, o Servidor Multi-Factor Authentication está configurado para ser um provedor de autenticação adicional a ser usado com o AD FS.

## <a name="install-a-standalone-instance-of-the-ad-fs-adapter-by-using-the-web-service-sdk"></a>Instalar o adaptador do AD FS autônomo usando o SDK do serviço Web
1. Instale o SDK do Serviço Web no servidor que esteja executando o Servidor Multi-Factor Authentication.
2. Copie os arquivos a seguir do diretório \Arquivos de Programa\Multi-Factor Authentication Server para o servidor no qual você planeja instalar o adaptador do AD FS:
   * MultiFactorAuthenticationAdfsAdapterSetup64.msi
   * Register-MultiFactorAuthenticationAdfsAdapter.ps1
   * Unregister-MultiFactorAuthenticationAdfsAdapter.ps1
   * MultiFactorAuthenticationAdfsAdapter.config
3. Execute a instalação de MultiFactorAuthenticationAdfsAdapterSetup64.msi.
4. No instalador do Adaptador do AD FS da Autenticação Multifator, clique em **Avançar** para iniciar a instalação.
5. Clique em **Fechar** quando a instalação for concluída.

## <a name="edit-the-multifactorauthenticationadfsadapterconfig-file"></a>Editar o arquivo MultiFactorAuthenticationAdfsAdapter.config
Execute estas etapas para editar o arquivo MultiFactorAuthenticationAdfsAdapter.config:

1. Defina o nó **UseWebServiceSdk** como **true**.  
2. Defina o valor **WebServiceSdkUrl** para a URL do SDK do Serviço Web da Autenticação Multifator. Por exemplo: *https://contoso.com/&lt;certificatename&gt;/MultiFactorAuthWebServicesSdk/PfWsSdk.asmx*, em que certificatename é o nome do seu certificado.  
3. Edite o script Register-MultiFactorAuthenticationAdfsAdapter.ps1 adicionando *-ConfigurationFilePath &lt;caminho&gt;* ao fim do comando `Register-AdfsAuthenticationProvider`, em que *&lt;caminho&gt;* é o caminho completo para o arquivo MultiFactorAuthenticationAdfsAdapter.config.

### <a name="configure-the-web-service-sdk-with-a-username-and-password"></a>Configurar o SDK do Serviço Web com um nome de usuário e senha
Há duas opções para configurar o SDK do Serviço Web. A primeira é com um nome de usuário e senha, a segunda é com um certificado de cliente. Execute estas etapas para a primeira opção, ou pule para a segunda.  

1. Defina o valor como **WebServiceSdkUsername** para uma conta membro do grupo de segurança PhoneFactor Admins. Use o formato &lt;domínio&gt;&#92;&lt;nome de usuário&gt;.  
2. Defina o valor de **WebServiceSdkPassword** para a senha da conta apropriada.

### <a name="configure-the-web-service-sdk-with-a-client-certificate"></a>Configurar o SDK do Serviço Web com um certificado de cliente
Se você não quiser usar um nome de usuário e senha, execute estas etapas para configurar o SDK do Serviço da Web com um certificado de cliente.

1. Obtenha um certificado do cliente de uma autoridade de certificação para o servidor que está executando o SDK de Serviço Web. Saiba como [obter certificados de cliente](https://technet.microsoft.com/library/cc770328.aspx).  
2. Importe o certificado do cliente no repositório de certificados pessoais do computador local no servidor que está executando o SDK do Serviço Web. Verifique se o certificado público da autoridade de certificação está no repositório de certificados de Certificados Raiz Confiáveis.  
3. Exporte as chaves pública e privada do certificado do cliente para um arquivo .pfx.  
4. Exporte a chave pública no formato Base64 em um arquivo .cer.  
5. No Gerenciador de Servidores, verifique se o recurso Servidor da Web (IIS)\Servidor da Web\Segurança\Autenticação de mapeamento de certificado de cliente SSL está instalado. Se não estiver instalado, selecione **Adicionar Funções e Recursos** para adicionar esse recurso.  
6. No Gerenciador do IIS, clique duas vezes em **Editor de Configuração** no site que contém o diretório virtual do SDK do Serviço Web. É muito importante fazer isso no nível do site e não no nível do diretório virtual.  
7. Acesse a seção **system.webServer/security/authentication/iisClientCertificateMappingAuthentication** .  
8. Defina habilitado como **true**.  
9. Defina oneToOneCertificateMappingsEnabled como **true**.  
10. Clique no botão **...** próximo a oneToOneMappings e clique no link **Adicionar**.  
11. Abra o arquivo .cer de Base64 exportado anteriormente. Remova *-----BEGIN CERTIFICATE-----*, *-----END CERTIFICATE-----* e todas as quebras de linha. Copie a cadeia de caracteres resultante.  
12. Defina o certificado para a cadeia de caracteres copiada na etapa anterior.  
13. Defina habilitado como **true**.  
14. Defina userName para uma conta que seja membro do grupo de segurança PhoneFactor Admins. Use o formato &lt;domínio&gt;&#92;&lt;nome de usuário&gt;.  
15. Defina a senha como a senha da conta apropriada e, em seguida, feche o Editor de Configuração.  
16. Clique no link **Aplicar** .  
17. No diretório virtual do SDK do Serviço Web, clique duas vezes em **Autenticação**.  
18. Verifique se Personificação do ASP.NET e Autenticação Básica estão definidos como **Habilitado** e se todos os outros itens estão definidos como **Desabilitado**.  
19. No diretório virtual do SDK do Serviço Web, clique duas vezes em **Configurações de SSL**.  
20. Defina Certificados de Cliente como **Aceitar** e depois clique em **Aplicar**.  
21. Copie o arquivo .pfx exportado anteriormente no servidor que está executando o adaptador do AD FS.  
22. Importe o arquivo .pfx para o repositório de certificados pessoais do computador local.  
23. Clique com o botão direito do mouse e selecione **Gerenciar Chaves Privadas**e, em seguida, conceda acesso de leitura à conta usada para entrar no serviço do AD FS.  
24. Abra o certificado de cliente e copie a impressão digital da guia **Detalhes** .  
25. No arquivo MultiFactorAuthenticationAdfsAdapter.config, defina **WebServiceSdkCertificateThumbprint** para a cadeia de caracteres copiada na etapa anterior.  

Por fim, execute o script \Arquivos de Programa\Servidor Autenticação Multifator\Register-MultiFactorAuthenticationAdfsAdapter.ps1 no PowerShell. O adaptador é registrado como WindowsAzureMultiFactorAuthentication. Reinicie o serviço AD FS para que o registro entre em vigor.

## <a name="related-topics"></a>Tópicos relacionados
Para obter ajuda com a solução de problemas, confira [Perguntas frequentes da Autenticação Multifator do Azure](multi-factor-authentication-faq.md)




<!--HONumber=Nov16_HO2-->


