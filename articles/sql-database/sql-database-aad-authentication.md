---
title: Conectar-se ao Banco de Dados SQL ou ao SQL Data Warehouse usando a autenticação do Azure Active Directory | Microsoft Docs
description: Saiba como se conectar ao Banco de Dados SQL Usando a Autenticação do Active Directory do Azure.
services: sql-database
documentationcenter: ''
author: BYHAM
manager: jhubbard
editor: ''
tags: ''

ms.service: sql-database
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 09/16/2016
ms.author: rick.byham@microsoft.com

---
# Conexão ao Banco de Dados SQL ou ao SQL Data Warehouse usando a autenticação do Azure Active Directory
A autenticação do Azure Active Directory é um mecanismo de conexão com o Banco de Dados SQL do Microsoft Azure e o [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) usando identidades no Azure Active Directory (Azure AD). Com a autenticação do Azure Active Directory, você pode gerenciar centralmente as identidades de usuários do banco de dados e outros serviços da Microsoft em uma única localização central. O gerenciamento central de IDs fornece um único local para gerenciar os usuários do banco de dados e simplifica o gerenciamento de permissões. Os benefícios incluem o seguinte:

* Ele fornece uma alternativa para autenticação do SQL Server.
* Ajuda a interromper a proliferação de identidades de usuário entre os servidores de banco de dados.
* Permite o rodízio de senhas em um único lugar
* Os clientes podem gerenciar permissões de banco de dados usando grupos (AAD) externos.
* Pode eliminar o armazenamento de senhas, permitindo a autenticação integrada do Windows e outras formas de autenticação às quais o Active Directory do Azure dá suporte.
* A autenticação do Active Directory do Azure usa usuários de banco de dados independente para autenticar identidades no nível do banco de dados.
* O Azure Active Directory dá suporte à autenticação baseada em token para aplicativos que se conectam ao Banco de Dados SQL.
* A autenticação do Azure Active Directory oferece suporte ao ADFS (federação de domínio) ou à autenticação de senha/usuário nativo para um local do Azure Active Directory sem sincronização de domínio.
* O Azure Active Directory dá suporte a conexões do SQL Server Management Studio que usam a Autenticação Universal do Active Directory, a qual inclui a MFA (Multi-Factor Authentication). A MFA inclui autenticação eficiente com uma variedade de opções de verificação fáceis como chamada telefônica, mensagem de texto, cartões inteligentes com PIN ou notificação por aplicativos móveis. Para saber mais, confira [Suporte do SSMS para MFA do Azure AD com o Banco de Dados SQL e o SQL Data Warehouse](sql-database-ssms-mfa-authentication.md).

As etapas de configuração incluem os procedimentos a seguir para configurar e usar a autenticação do Active Directory do Azure.

1. Crie e popule um Azure Active Directory.
2. Verifique se o banco de dados está em um Banco de Dados SQL V12 do Azure (Não necessariamente para o SQL Data Warehouse).
3. Opcional: associe ou altere o Active Directory que está associado atualmente à sua Assinatura do Azure.
4. Criar um administrador do Azure Active Directory para o SQL Server do Azure ou o [Azure SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/).
5. Configure os computadores cliente.
6. Crie usuários de banco de dados independente em seu banco de dados, mapeados para identidades do Azure AD.
7. Conecte-se ao banco de dados usando identidades do Azure AD.

## Confiar na arquitetura
O diagrama de alto nível a seguir resume a arquitetura da solução de usar a autenticação do Azure AD com o Banco de Dados SQL do Azure. Os mesmos conceitos se aplicam ao SQL Data Warehouse. Para oferecer suporte à senha de usuário nativo do Azure Active Directory, será considerada apenas a parte de Nuvem e o Azure AD/Banco de Dados SQL do Azure. Para dar suporte à Autenticação federada (ou a usuário/senha para as credenciais do Windows), será necessária a comunicação com o bloco do ADFS. As setas indicam caminhos para comunicação.

![diagrama de autenticação do aad][1]

O diagrama a seguir indica as relações de federação, confiança e hospedagem que permitem que um cliente se conecte a um banco de dados enviando um token. O token é autenticado pelo Azure AD e é considerado confiável pelo banco de dados. O Cliente 1 pode representar um Azure Active Directory com usuários nativos ou um Azure Active Directory com usuários federados. O Cliente 2 representa uma solução possível, incluindo os usuários importados; neste exemplo, provenientes de um Azure Active Directory federado com o ADFS sendo sincronizado com o Azure Active Directory. É importante entender que o acesso a um banco de dados usando a autenticação do Azure AD requer que a assinatura de hospedagem esteja associada ao Azure Active Directory. A mesma assinatura deve ser usada para criar o SQL Server que hospeda o Banco de Dados SQL ou o SQL Data Warehouse.

![relação de assinatura][2]

## Estrutura do administrador
Ao usar a autenticação do Azure AD, haverá duas contas de administrador para o servidor do Banco de Dados SQL: o administrador original do SQL Server e o administrador do Azure AD. Os mesmos conceitos se aplicam ao SQL Data Warehouse. Somente o administrador com base em uma conta do AD do Azure pode criar o primeiro usuário de banco de dados do AD do Azure contido em um banco de dados de usuário. O logon de administrador do AD do Azure pode ser um usuário ou um grupo do AD do Azure. Quando o administrador é uma conta de grupo, ele pode ser usado por qualquer membro do grupo, permitindo múltiplos administradores do AD do Azure para a instância do SQL Server. Usar a conta de grupo como um administrador aprimora a capacidade de gerenciamento, permitindo que você adicione e remova membros do grupo no AD do Azure centralmente, sem alterar os usuários ou permissões no Banco de Dados SQL. Somente um administrador do AD do Azure (um usuário ou grupo) pode ser configurado por vez, a qualquer momento.

![estrutura de administrador][3]

## Permissões
Para criar novos usuários, você deve ter a permissão `ALTER ANY USER` no banco de dados. A permissão `ALTER ANY USER` pode ser concedida a qualquer usuário do banco de dados. A permissão `ALTER ANY USER` também é mantida pelas contas de administrador do servidor e usuários de banco de dados com a permissão `CONTROL ON DATABASE` ou `ALTER ON DATABASE` para esse banco de dados e por membros da função de banco de dados `db_owner`.

Para criar um usuário do banco de dados contido no Banco de Dados SQL do Azure ou no SQL Data Warehouse, você deverá conectar-se ao banco de dados usando uma identidade do Azure AD. Para criar o primeiro usuário de banco de dados independente, você deve se conectar ao banco de dados usando o administrador do Azure Active Directory (que é o proprietário do banco de dados). Isso é demonstrado nas etapas 4 e 5 abaixo. Qualquer autenticação do Azure Active Directory só será possível se o administrador do Azure Active Directory tiver sido criado para o Banco de Dados SQL ou para o servidor SQL Data Warehouse. Se o administrador do Azure Active Directory tiver sido removido do servidor, os usuários existentes do Azure Active Directory criados anteriormente dentro do SQL Server não poderão mais se conectar ao banco de dados usando suas credenciais do Azure Active Directory.

## Limitações e recursos do AD do Azure
Os seguintes membros do Azure Active Directory podem ser provisionados no SQL Server do Azure ou no SQL Data Warehouse:

* Membros nativos: membro criado no Azure AD no domínio gerenciado ou em um domínio do cliente. Para saber mais, confira [Adicionar seu nome de domínio ao Azure AD](../active-directory/active-directory-add-domain.md).
* Membros de domínio federado: membro criado no Azure AD com um domínio federado. Para saber mais, confira [O Microsoft Azure agora dá suporte à federação com o Active Directory do Windows Server](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/).
* Membros importados de outros Active Directories do Azure que são membros de domínio nativo ou federado.
* Grupos do Active Directory criados como grupos de segurança.

Não há suporte para contas da Microsoft (por exemplo, outlook.com, hotmail.com, live.com) nem para outras contas de convidado (por exemplo, gmail.com, yahoo.com). Se você pode fizer logon em [https://login.live.com](https://login.live.com) usando a conta e senha, isso significará que você está usando uma conta da Microsoft para a qual não há suporte para autenticação do Azure AD para o Banco de Dados SQL do Azure ou para o Azure SQL Data Warehouse.

### Considerações adicionais
* Para aumentar a capacidade de gerenciamento, é recomendável que você provisione um grupo dedicado do Azure Active Directory como administrador.
* Somente um administrador do Azure AD (um usuário ou grupo) pode ser configurado por vez, a qualquer momento, para um SQL Data Warehouse do Azure.
* Somente um administrador do Azure Active Directory do SQL Server pode conectar-se inicialmente ao SQL Server do Azure ou ao Azure SQL Data Warehouse usando uma conta do Azure Active Directory. O administrador do Active Directory pode configurar usuários subsequentes de banco de dados do Active Directory do Azure.
* É recomendável configurar o tempo limite da conexão para 30 segundos.
* O SQL Server 2016 Management Studio e o SQL Server Data Tools para Visual Studio 2015 (versão 14.0.60311.1 de abril de 2016 ou posterior) dão suporte à autenticação do Azure Active Directory. (A autenticação do Azure Active Directory tem suporte do **Provedor de dados do .NET Framework para SqlServer**; é necessária pelo menos a versão 4.6 do .NET Framework). Portanto, as versões mais recentes dessas ferramentas e aplicativos de camada de dados (DAC e .bacpac) podem usar a autenticação do Azure Active Directory.
* [ODBC versão 13.1](https://www.microsoft.com/download/details.aspx?id=53339) oferece suporte à autenticação do Azure Active Directory. No entanto, o `bcp.exe` não pode se conectar usando a autenticação do Azure Active Directory, pois eles usam um provedor ODBC mais antigo.
* O `sqlcmd` oferece suporte à autenticação do Azure Active Directory a partir da versão 13.1 disponível no [Centro de Download](http://go.microsoft.com/fwlink/?LinkID=825643).
* O SQL Server Data Tools para Visual Studio 2015 requer pelo menos a versão de abril de 2016 do Data Tools (versão 14.0.60311.1). Atualmente, os usuários do Azure Active Directory não são mostrados no Pesquisador de Objetos do SSDT. Como alternativa, exiba os usuários em [sys.database\_principals](https://msdn.microsoft.com/library/ms187328.aspx).
* O [Microsoft JDBC Driver 6.0 para SQL Server](https://www.microsoft.com/pt-BR/download/details.aspx?id=11774) dá suporte à autenticação do Azure Active Directory. Confira também [Configuração das propriedades de conexão](https://msdn.microsoft.com/library/ms378988.aspx).
* O PolyBase não pode ser autenticado usando a autenticação do Azure Active Directory.
* Não há suporte para algumas ferramentas como o Excel e o BI.
* A autenticação do Azure Active Directory tem suporte para Banco de Dados SQL por meio das folhas **Importar Banco de Dados** e **Exportar Banco de Dados** do Portal do Azure. Também há suporte para importação e exportação usando a autenticação do Azure Active Directory do comando do PowerShell.

## 1\. Criar e popular um Azure AD
Crie um Azure Active Directory e popule-o com usuários e grupos. O Azure Active Directory pode ser o domínio inicial domínio Azure AD. O Azure Active Directory também pode ser um local nos Serviços de Domínio do Active Directory que é federado com o Azure Active Directory.

Para saber mais, confira [Integração de suas identidades locais com o Azure Active Directory](../active-directory/active-directory-aadconnect.md), [Adicionar seu próprio nome de domínio ao Azure AD](../active-directory/active-directory-add-domain.md), [O Microsoft Azure agora dá suporte à federação com o Windows Server Active Directory](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [Administrando seu diretório Azure AD](https://msdn.microsoft.com/library/azure/hh967611.aspx) e [Gerenciar o Azure AD usando o Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx).

## 2\. Certifique-se de que seu Banco de dados SQL é a versão 12
O mais recente Banco de Dados SQL V12 do Azure dá suporte à autenticação do Active Directory do Azure. Para obter informações sobre Banco de Dados SQL V12 e saber se ele está disponível em sua região, consulte [O que há de novo na Última Atualização do Banco de Dados SQL V12](sql-database-v12-whats-new.md). Esta etapa não é necessária para o Azure SQL Data Warehouse porque o SQL Data Warehouse só está disponível no V12.

Se você tem um banco de dados existente, verifique se ele está hospedado no Banco de Dados SQL V12 conectando-se ao banco de dados (por exemplo, usando o SQL Server Management Studio) e executando o `SELECT @@VERSION;`. A saída esperada para um Banco de Dados SQL V12 é pelo menos **Microsoft SQL Azure (RTM) - 12.0**. Se o seu banco de dados não estiver hospedado no Banco de Dados SQL V12, consulte [Planejar e se preparar para atualizar para o Banco de Dados SQL V12](sql-database-v12-plan-prepare-upgrade.md) e, em seguida, visite o Portal Clássico do Azure para migrar o banco de dados para Banco de Dados SQL V12.

Como alternativa, você pode criar um novo banco de dados em Banco de Dados SQL V12, seguindo as etapas listadas em [Criar seu primeiro Banco de Dados SQL do Azure](sql-database-get-started.md). **Dica**: leia a próxima etapa antes de selecionar uma assinatura para o novo banco de dados.

## 3\. Opcional: associar ou alterar o Active Directory que está associado atualmente à sua Assinatura do Azure
Para associar seu banco de dados ao diretório do AD do Azure para sua organização, torne o diretório um diretório confiável para a assinatura do Azure que hospeda o banco de dados. Para saber mais sobre as assinaturas do Azure, consulte [Como as assinaturas do Azure são associadas ao Azure AD](https://msdn.microsoft.com/library/azure/dn629581.aspx).

**Informações adicionais:** cada assinatura do Azure tem uma relação de confiança com uma instância do AD do Azure. Isso significa que ela confia que esse diretório autentique usuários, serviços e dispositivos. Várias assinaturas podem confiar no mesmo diretório, mas uma única assinatura confia em apenas um diretório. Você pode ver qual diretório é confiável para sua assinatura na guia **Configurações** em [https://manage.windowsazure.com/](https://manage.windowsazure.com/). Essa relação de confiança que uma assinatura tem com um diretório é diferente da relação que uma assinatura tem com todos os outros recursos no Azure (sites, bancos de dados e assim por diante), que são mais similares a recursos filho de uma assinatura. Se uma assinatura expira, o acesso a esses outros recursos associados à assinatura também pára. Mas o diretório permanece no Azure e você pode associar outra assinatura a ele, além de continuar a gerenciar os usuários do diretório. Para obter mais informações sobre recursos, consulte [Noções básicas sobre o acesso a recursos no Azure](https://msdn.microsoft.com/library/azure/dn584083.aspx).

Os procedimentos a seguir fornecem instruções passo a passo sobre como alterar o diretório associado para uma determinada assinatura.

1. Conecte-se ao seu [portal clássico do Azure](https://manage.windowsazure.com/) usando um administrador de assinatura do Azure.
2. Na faixa esquerda, selecione **CONFIGURAÇÕES**.
3. Suas assinaturas aparecem na tela de configurações. Se a assinatura desejada não aparecer, clique em **Assinaturas** na parte superior, exiba caixa da lista suspensa **FILTRAR POR DIRETÓRIO**, selecione o diretório que contém suas assinaturas e, por fim, clique em **APLICAR**.
   
    ![selecione a assinatura][4]
4. Na área **configurações**, clique em sua assinatura e, em seguida, clique em **EDITAR DIRETÓRIO** na parte inferior da página.
   
    ![ad-settings-portal][5]
5. Na caixa **EDITAR DIRETÓRIO**, selecione o Azure Active Directory associado ao SQL Server ou ao SQL Data Warehouse e clique na seta para avançar.
   
    ![edit-directory-select][6]
6. Na caixa de diálogo de Mapeamento de diretório **CONFIRMAR**, confirme que "**Todos os coadministradores serão removidos**".
   
    ![edit-directory-confirm][7]
7. Clique na marca de verificação para recarregar o portal.

> [!NOTE]
> Quando você muda o diretório, o acesso a todos os coadministradores, usuários e grupos do AD do Azure e usuários de recursos reserva recursos com backup em diretório serão removidos e eles não terão mais acesso a essa assinatura nem a seus recursos. Apenas você, como um administrador de serviços, pode configurar o acesso para entidades de segurança com base no novo diretório. Essa alteração pode levar bastante tempo para se propagar para todos os recursos. Alterar o diretório também altera o administrador do Azure AD para o Banco de Dados SQL e SQL Data Warehouse e remove a permissão de acesso ao banco de dados para quaisquer usuários existentes do Azure AD. O administrador do AD do Azure deve ser definido novamente (conforme descrito abaixo) e novos usuários do Azure devem ser criados.
> 
> 

## 4\. Crie um administrador do Azure AD para o Azure SQL Server
Cada Servidor do SQL Server do Azure (que hospeda um Banco de dados SQL ou SQL Data Warehouse) começa com uma única conta de administrador do servidor, que é o administrador de todo o SQL Server do Azure. Um segundo administrador do SQL Server deve ser criado, que é uma conta do Azure AD. Essa entidade de segurança é criada como um usuário de banco de dados independente no banco de dados mestre. Como administradores, as contas de administrador do servidor são membros da função **db\_owner** em todos os usuários do banco de dados e inserem cada banco de dados de usuário como o usuário **dbo**. Para obter mais informações sobre as contas de administrador do servidor, consulte [Gerenciamento de Bancos de Dados e Logons no Banco de Dados SQL do Azure](sql-database-manage-logins.md) e a seção **Logons e Usuários** de [Limitações e Diretrizes de Segurança de Banco de Dados SQL do Azure](sql-database-security-guidelines.md).

Ao usar o Azure Active Directory com a Replicação Geográfica, o administrador do Azure Active Directory deverá ser configurado para os servidores primários e secundários. Se um servidor não tiver um administrador do Azure Active Directory, os usuários e logons do Azure Active Directory obterão um erro de servidor "Não é possível conectar-se".

> [!NOTE]
> Usuários que não são baseados em uma conta do Azure AD (incluindo a conta de administrador do Azure SQL Server) não podem criar usuários baseados no Azure AD porque eles não têm permissão para validar os usuários do banco de dados proposto com o Azure AD.
> 
> 

### Provisionar um administrador do Azure Active Directory para seu Azure SQL Server usando o Portal do Azure
1. No [Portal do Azure](https://portal.azure.com/), no canto superior direito, clique na conexão para exibir uma lista suspensa com os Active Directories possíveis. Escolha o Active Directory correto como o AD do Azure padrão. Esta etapa vincula a associação de assinatura no Active Directory ao Azure SQL Server, certificando-se que a mesma assinatura é usada tanto para o Azure AD quanto para o SQL Server. (O SQL Server do Azure pode hospedar o Banco de Dados SQL do Azure ou o SQL Data Warehouse do Azure.)
   
    ![choose-ad][8]
2. Na faixa à esquerda, selecione **SQL servers**, selecione seu **SQL server** e, em seguida, na folha **SQL Server**, na parte superior, clique em **Configurações**.
   
    ![configurações de ad.][9]
3. Na folha **Configurações**, clique em **administrador do Active Directory.
4. Na folha **Administrador do Active Directory**, clique em **Administrador do Active Directory** e, na parte superior, clique em **Definir administrador**.
5. Na folha **Adicionar administrador**, procure um usuário, selecione o usuário ou grupo que será um administrador e, em seguida, clique em **Selecionar**. (A folha Administrador do Active Directory mostra todos os membros e grupos do Active Directory. Usuários ou grupos que estão esmaecidos não podem ser selecionados porque eles não têm suporte como administradores do AD do Azure. (Consulte a lista de administradores com suporte em **Limitações e recursos do AD do Azure** acima.) O RBAC (controle de acesso baseado em função) aplica-se somente ao portal e não é propagado para o SQL Server.
6. Na parte superior da folha **Administrador do Active Directory**, clique em **SALVAR**.![escolha o administrador][10]
   
    O processo de alteração do administrador pode levar vários minutos. O novo administrador aparece na caixa **Administrador do Active Directory**.

> [!NOTE]
> Ao configurar o administrador do Azure AD, o novo nome de administrador (usuário ou grupo) não pode já estar presente no banco de dados mestre virtual como um usuário de autenticação do SQL Server. Se presente, a configuração de administração do AD do Azure falhará, revertendo sua criação e indicando que esse administrador (nome) já existe. Como esse usuário de autenticação do SQL Server não é parte do Azure AD, qualquer esforço para se conectar ao servidor usando a autenticação do Azure AD falhará.
> 
> 

Para remover um Administrador mais tarde, na parte superior da folha **Administrador do Active Directory**, clique em **Remover administrador** e depois em **Salvar**.

### Provisionar um administrador do AD do Azure para o Azure SQL Server usando o PowerShell
Para executar os cmdlets do PowerShell, você precisa ter o Azure PowerShell instalado e em execução. Para obter informações detalhadas, confira [Como instalar e configurar o PowerShell do Azure](../powershell-install-configure.md).

Para provisionar um administrador do AD do Azure, execute os seguintes comandos do Azure PowerShell:

* Add-AzureRmAccount
* Select-AzureRmSubscription

Cmdlets usados para provisionar e gerenciar o administrador do AD do Azure:

| Nome do cmdlet | Descrição |
| --- | --- |
| [Set-AzureRmSqlServerActiveDirectoryAdministrator](https://msdn.microsoft.com/library/azure/mt603544.aspx) |Provisiona um administrador do Azure Active Directory para o SQL Server do Azure ou o Azure SQL Data Warehouse. (Precisa ser da assinatura atual.) |
| [Remove-AzureRmSqlServerActiveDirectoryAdministrator](https://msdn.microsoft.com/library/azure/mt619340.aspx) |Remove um administrador do Azure Active Directory para o SQL Server do Azure ou para o Azure SQL Data Warehouse. |
| [Get-AzureRmSqlServerActiveDirectoryAdministrator](https://msdn.microsoft.com/library/azure/mt603737.aspx) |Retorna informações sobre um administrador do Azure Active Directory atualmente configurado para o SQL Server do Azure ou para o Azure SQL Data Warehouse. |

Use o comando get-help do PowerShell para ver mais detalhes sobre cada um desses comandos, por exemplo, ``get-help Set-AzureRmSqlServerActiveDirectoryAdministrator``.

O seguinte script provisiona um grupo de administradores do Azure AD chamado **DBA\_Group** (ID de objeto `40b79501-b343-44ed-9ce7-da4c8cc7353f`) para o servidor **demo\_server** em um grupo de recursos chamado **Group-23**:

```
Set-AzureRmSqlServerActiveDirectoryAdministrator –ResourceGroupName "Group-23"
–ServerName "demo_server" -DisplayName "DBA_Group"
```

O parâmetro de entrada **DisplayName** aceita o nome de exibição do Azure AD ou o Nome UPN. Por exemplo, ``DisplayName="John Smith"`` e ``DisplayName="johns@contoso.com"``. Para grupos do AD do Azure, há suporte apenas para o nome de exibição do AD do Azure.

> [!NOTE]
> O comando ```Set-AzureRmSqlServerActiveDirectoryAdministrator``` do Azure PowerShell não impede que você provisione administradores do Azure AD para usuários para os quais não há suporte. Um usuário para o qual não há suporte pode ser provisionado, mas não pode se conectar a um banco de dados. (Consulte a lista de administradores com suporte em **Limitações e recursos do AD do Azure** acima.)
> 
> 

O seguinte exemplo usa a **ObjectID** opcional:

```
Set-AzureRmSqlServerActiveDirectoryAdministrator –ResourceGroupName "Group-23"
–ServerName "demo_server" -DisplayName "DBA_Group" -ObjectId "40b79501-b343-44ed-9ce7-da4c8cc7353f"
```

> [!NOTE]
> A **ObjectID** do Azure AD é necessária quando o **DisplayName** não é exclusivo. Para recuperar os valores de **ObjectID** e **DisplayName**, use a seção do Active Directory do Portal Clássico do Azure e exiba as propriedades de um usuário ou grupo.
> 
> 

O exemplo a seguir retorna informações sobre o administrador atual do AD do Azure para o Azure SQL Server:

```
Get-AzureRmSqlServerActiveDirectoryAdministrator –ResourceGroupName "Group-23" –ServerName "demo_server" | Format-List
```

O seguinte exemplo remove um administrador do AD do Azure:

```
Remove-AzureRmSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23" –ServerName "demo_server"
```

Você também pode provisionar um administrador do Azure Active Directory usando as APIs REST. Para saber mais, confira [Referência da API REST de Gerenciamento de Serviços e Operações de Bancos de Dados SQL do Azure](https://msdn.microsoft.com/library/azure/dn505719.aspx)

## 5\. Configurar os computadores cliente
Em todos os computadores cliente, dos quais seus aplicativos ou usuários se conectam ao Banco de Dados SQL do Azure ou ao Azure SQL Data Warehouse usando identidades do Azure AD, você deve instalar o software a seguir:

* .NET Framework 4.6 ou posterior de [https://msdn.microsoft.com/library/5a4x27ek.aspx](https://msdn.microsoft.com/library/5a4x27ek.aspx).
* A Biblioteca de Autenticação do Active Directory do Azure para SQL Server (**ADALSQL.DLL**) está disponível em vários idiomas (tanto x86 quanto amd64) no centro de download da [Biblioteca de Autenticação do Microsoft Active Directory para Microsoft SQL Server](http://www.microsoft.com/download/details.aspx?id=48742).

### Ferramentas
* Instalar um [SQL Server 2016 Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) ou [SQL Server Data Tools para Visual Studio 2015](https://msdn.microsoft.com/library/mt204009.aspx) atende ao requisito do .NET Framework 4.6.
* O SSMS instala a versão x86 do **ADALSQL.DLL**.
* O SSDT instala a versão amd64 do **ADALSQL.DLL**.
* O Visual Studio mais recente dos [Downloads do Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs) atende ao requisito do .NET Framework 4.6, mas não instala a versão amd64 necessária do **ADALSQL.DLL**.

## 6\. Criar usuários de banco de dados independente em seu banco de dados, mapeados para identidades do AD do Azure
### Sobre usuários de bancos de dados independentes
A autenticação do Active Directory do Azure exige que os usuários do banco de dados sejam criados como usuários do banco de dados independente. Um usuário de banco de dados independente com base em uma identidade do AD do Azure é um usuário de banco de dados que não tem um logon no banco de dados mestre e que mapeia para uma identidade no diretório do AD do Azure que está associada ao banco de dados. A identidade do AD do Azure pode ser uma conta de usuário individual ou um grupo. Para saber mais sobre usuários de bancos de dados independentes, veja [Usuários do bancos de dados independentes - Tornando seu banco de dados portátil](https://msdn.microsoft.com/library/ff929188.aspx).

> [!NOTE]
> Os usuários do banco de dados (com exceção dos administradores) não podem ser criados usando o portal. Funções de RBAC não são propagadas para o SQL Server, para o Banco de Dados SQL ou para o SQL Data Warehouse. As funções RBAC do Azure são usadas para gerenciar Recursos do Azure e não se aplicam às permissões de banco de dados. Por exemplo, a função **Colaborador do SQL Server** não concede acesso para se conectar ao Banco de Dados SQL ou ao SQL Data Warehouse. A permissão de acesso deve ser concedida diretamente no banco de dados usando instruções Transact-SQL.
> 
> 

### Conecte-se ao banco de dados ou ao data warehouse do usuário usando o SQL Server Management Studio ou o SQL Server Data Tools
Para confirmar que o administrador do Azure AD está configurado corretamente, conecte-se ao banco de dados **mestre** usando a conta de administrador do Azure AD. Para provisionar um usuário de banco de dados independente com base no Azure AD (que não seja o administrador do servidor que é o proprietário do banco de dados), conecte-se ao banco de dados com uma identidade do Azure AD que tenha acesso ao banco de dados.

> [!IMPORTANT]
> O suporte para a autenticação do Azure Active Directory está disponível com o [SQL Server 2016 Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) e o [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) no Visual Studio 2015. A versão de agosto de 2016 do SSMS também inclui suporte para Universal autenticação do Active Directory, que permite aos administradores exigir Multi-Factor Authentication utilizando uma chamada telefônica, mensagem de texto, os cartões inteligentes com PIN ou a notificação de aplicativo móvel.
> 
> 

#### Conectar-se usando a autenticação integrada do Active Directory
Use este método se você efetuou logon no Windows usando suas credenciais do Azure Active Directory por meio de um domínio federado.

1. Inicie o Management Studio ou o Data Tools e, na caixa de diálogo **Conectar ao Servidor** (ou **Conectar ao Mecanismo de Banco de Dados**), na caixa **Autenticação**, selecione **Autenticação Integrada do Active Directory**. Nenhuma senha é necessária ou pode ser inserida porque suas credenciais existentes serão apresentadas para a conexão. ![Selecione Autenticação Integrada do AD][11]
2. Clique no botão **Opções** e, na página **Propriedades de Conexão**, na caixa **Conectar ao banco de dados**, digite o nome do banco de dados de usuário ao qual deseja se conectar. ![Selecione o nome do banco de dados][13]

#### Conectar-se usando a autenticação de senha do Active Directory
Use esse método ao se conectar com um nome de entidade do AD do Azure usando o domínio gerenciado pelo Azure AD. Você também pode usá-lo para conta federada sem acesso ao domínio, por exemplo, ao trabalhar remotamente.

Use este método se você estiver conectado ao Windows usando as credenciais de um domínio que não está federado com o Azure, ou quando usar a autenticação do Azure AD usando o Azure AD baseado no domínio de cliente ou inicial.

1. Inicie o Management Studio e, na caixa de diálogo **Conectar ao Servidor** (ou **Conectar ao Mecanismo de Banco de Dados**), na caixa **Autenticação**, selecione **Autenticação da Senha do Active Directory**.
2. Na caixa **Nome de usuário**, digite seu nome de usuário do Azure Active Directory no formato **username@domain.com**. Essa deve ser uma conta do Active Directory do Azure ou uma conta de um domínio federado com o Active Directory do Azure.
3. Na caixa **Senha**, digite sua senha de usuário para a conta do Azure Active Directory ou conta de domínio federado. ![Selecione Autenticação de Senha do AD][12]
4. Clique no botão **Opções** e, na página **Propriedades de Conexão**, na caixa **Conectar ao banco de dados**, digite o nome do banco de dados de usuário ao qual deseja se conectar. (Confira o gráfico na opção anterior.)

### Criar um usuário de banco de dados independente do AD do Azure em um banco de dados do usuário
Para criar um usuário de banco de dados independente baseado no Azure AD (que não seja o administrador do servidor proprietário do banco de dados), conecte-se ao banco de dados com uma identidade do Azure AD como um usuário com, no mínimo, a permissão **ALTER ANY USER**. Em seguida, use a sintaxe Transact-SQL a seguir:

    CREATE USER <Azure_AD_principal_name>
    FROM EXTERNAL PROVIDER;


*Azure\_AD\_principal\_name* pode ser o nome UPN de um usuário do Azure AD ou o nome de exibição de um grupo do Azure AD.

**Exemplos:** para criar um usuário de banco de dados independente que representa um usuário de domínio federado ou gerenciado pelo Azure AD:

    CREATE USER [bob@contoso.com] FROM EXTERNAL PROVIDER;
    CREATE USER [alice@fabrikam.onmicrosoft.com] FROM EXTERNAL PROVIDER;

Para criar um usuário de banco de dados independente que represente um grupo de domínio do AD do Azure AD ou federado, forneça o nome para exibição de um grupo de segurança:

    CREATE USER [ICU Nurses] FROM EXTERNAL PROVIDER;

Para criar um usuário de banco de dados independente que representa um aplicativo que se conectará usando um token do Azure AD:

    CREATE USER [appName] FROM EXTERNAL PROVIDER;

Para obter mais informações sobre como criar usuários de banco de dados independente baseados em identidades do Active Directory do Azure, consulte [CRIAR USUÁRIO (Transact-SQL)](http://msdn.microsoft.com/library/ms173463.aspx).

> [!NOTE]
> Remover o administrador do Azure Active Directory para o Servidor SQL do Azure impede que qualquer usuário da autenticação do Azure AD se conecte ao servidor. Se for necessário, os usuários do Azure AD inutilizáveis poderão ser removidos manualmente por um administrador do Banco de Dados SQL.
> 
> 

Quando você cria um usuário de banco de dados, o usuário recebe a permissão **CONNECT** e pode se conectar a esse banco de dados como membro da função **PUBLIC**. Inicialmente, as únicas permissões disponíveis para o usuário são as permissões concedidas à função **PUBLIC** ou as permissões concedidas a quaisquer grupos do Windows dos quais esse usuário faz parte. A partir do momento que você provisionar um usuário de banco de dados independente baseado no AD do Azure, você pode conceder ao usuário permissões adicionais, do mesmo modo que você concede permissões para qualquer outro tipo de usuário. Normalmente, conceda permissões para funções de banco de dados e adicione usuários a funções. Para saber mais, confira [Noções básicas sobre permissões do Mecanismo de Banco de Dados](http://social.technet.microsoft.com/wiki/contents/articles/4433.database-engine-permission-basics.aspx). Para obter mais informações sobre funções especiais do Banco de dados SQL, veja [Gerenciando bancos de dados e logons no Banco de Dados SQL do Azure](sql-database-manage-logins.md). Um usuário de domínio federado que é importado para um domínio gerenciado deve usar a identidade do domínio gerenciado.

> [!NOTE]
> Usuários do AD do Azure são marcados nos metadados do banco de dados com tipo E (EXTERNAL\_USER) e para grupos com o tipo X (EXTERNAL\_GROUPS). Para saber mais, confira [sys.database\_principals](https://msdn.microsoft.com/library/ms187328.aspx).
> 
> 

## 7\. Conectar-se usando as identidades do Azure AD
A autenticação do Active Directory do Azure dá suporte aos seguintes métodos de conexão a um banco de dados usando identidades do AD do Azure:

* Usando a autenticação integrada do Windows
* Uso de um nome principal e de uma senha do Azure AD
* Uso da autenticação de token do aplicativo

### 7\.1. Conectar-se usando a autenticação integrada (Windows)
Para usar a autenticação integrada do Windows, o Active Directory de seu domínio deve ser federado com o Azure Active Directory. Seu aplicativo cliente (ou um serviço) conectando-se ao banco de dados deve estar em execução em uma máquina ingressada no domínio com credenciais de domínio do usuário.

Para conectar-se a um banco de dados usando a autenticação integrada e uma identidade do AD do Azure, a palavra-chave de Autenticação na cadeia de conexão de banco de dados deve ser definida como Integrada ao Active Directory. O exemplo de código em C# a seguir usa ADO .NET.

    string ConnectionString =
    @"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Integrated; Initial Catalog=testdb;";
    SqlConnection conn = new SqlConnection(ConnectionString);
    conn.Open();

Observe que, para a conexão ao Banco de Dados SQL do Azure, não há suporte para a palavra-chave de cadeia de conexão ``Integrated Security=True``. Observe que, ao fazer uma conexão ODBC, você precisará remover espaços e configurar a autenticação para 'ActiveDirectoryIntegrated'.

### 7\.2. Conectar com um nome principal e uma senha do AD do Azure
Para conectar-se a um banco de dados usando a autenticação integrada e uma identidade do Azure AD, a palavra-chave de Autenticação deve ser definida como Senha do Active Directory. A cadeia de conexão deve conter valores e palavras-chave de ID/UID de Usuário e Senha/PWD. O exemplo de código em C# a seguir usa ADO .NET.

    string ConnectionString =
      @"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Password; Initial Catalog=testdb;  UID=bob@contoso.onmicrosoft.com; PWD=MyPassWord!";
    SqlConnection conn = new SqlConnection(ConnectionString);
    conn.Open();

Saiba mais sobre métodos de autenticação do Azure AD usando os exemplos de código de demonstração disponíveis em [Demonstração do GitHub de Autenticação do Azure AD](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/security/azure-active-directory-auth).

### 7\.3 Conectando-se com um token do Azure AD
Este método de autenticação permite que os serviços de camada intermediária se conectem ao Banco de Dados SQL do Azure ou ao SQL Data Warehouse do Azure obtendo um token do AAD (Azure Active Directory). Ele permite cenários sofisticados, incluindo a autenticação baseada em certificado. Você precisa concluir quatro etapas básicas para usar a autenticação de token do Azure AD:

1. Registrar seu aplicativo no Azure Active Directory e obter a ID de cliente para seu código.
2. Criar um usuário de banco de dados que representa o aplicativo. (Concluída anteriormente na etapa 6).
3. Crie um certificado no computador cliente que executa o aplicativo.
4. Adicionar o certificado como uma chave para seu aplicativo.

Exemplo de cadeia de conexão:

```
string ConnectionString =@"Data Source=n9lxnyuzhv.database.windows.net; Initial Catalog=testdb;"
SqlConnection conn = new SqlConnection(ConnectionString);
connection.AccessToken = "Your JWT token"
conn.Open();
```

Para obter mais informações, confira [Blog de segurança do SQL Server](https://blogs.msdn.microsoft.com/sqlsecurity/2016/02/09/token-based-authentication-support-for-azure-sql-db-using-azure-ad-auth/).

### Conectando-se ao sqlcmd
As instruções a seguir, conexão usando a versão 13.1 do sqlcmd, que está disponível no [Centro de Download](http://go.microsoft.com/fwlink/?LinkID=825643).

```
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net  -G  
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net -U bob@contoso.com -P MyAADPassword -G -l 30
```

## Confira também
[Gerenciamento de bancos de dados e logons no Banco de Dados SQL do Azure](sql-database-manage-logins.md)

[Usuários de Banco de Dados Independente](https://msdn.microsoft.com/library/ff929071.aspx)

[CRIAR USUÁRIO (Transact-SQL)](http://msdn.microsoft.com/library/ms173463.aspx)

<!--Image references-->

[1]: ./media/sql-database-aad-authentication/1aad-auth-diagram.png
[2]: ./media/sql-database-aad-authentication/2subscription-relationship.png
[3]: ./media/sql-database-aad-authentication/3admin-structure.png
[4]: ./media/sql-database-aad-authentication/4select-subscription.png
[5]: ./media/sql-database-aad-authentication/5ad-settings-portal.png
[6]: ./media/sql-database-aad-authentication/6edit-directory-select.png
[7]: ./media/sql-database-aad-authentication/7edit-directory-confirm.png
[8]: ./media/sql-database-aad-authentication/8choose-ad.png
[9]: ./media/sql-database-aad-authentication/9ad-settings.png
[10]: ./media/sql-database-aad-authentication/10choose-admin.png
[11]: ./media/sql-database-aad-authentication/11connect-using-int-auth.png
[12]: ./media/sql-database-aad-authentication/12connect-using-pw-auth.png
[13]: ./media/sql-database-aad-authentication/13connect-to-db.png

<!---HONumber=AcomDC_0921_2016-->