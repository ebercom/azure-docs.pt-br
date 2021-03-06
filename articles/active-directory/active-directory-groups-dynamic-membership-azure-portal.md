
---
title: Usando atributos para criar regras avançadas para a associação de grupo na visualização do Azure Active Directory | Microsoft Docs
description: Como criar regras avançadas para uma associação de grupo dinâmico, incluindo parâmetros e operadores de regra de expressões com suporte.
services: active-directory
documentationcenter: ''
author: curtand
manager: femila
editor: ''

ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2016
ms.author: curtand

---
# Usando atributos para criar regras avançadas para a associação de grupo na visualização do Azure Active Directory
O portal do Azure fornece a capacidade de criar regras avançadas para habilitar associações dinâmicas baseadas em atributos mais complexas de grupos da visualização do Azure Active Directory (Azure AD). [O que está na visualização?](active-directory-preview-explainer.md) Este artigo detalha os atributos de regra e a sintaxe para criar essas regras avançadas.

## Para criar a regra avançada
1. Entre no [portal do Azure](https://portal.azure.com) com uma conta que seja um administrador global para o diretório.
2. Selecione **Mais serviços**, digite **Usuários e grupos** na caixa de texto e, em seguida, selecione **Enter**.
   
   ![Abrindo o gerenciamento de usuários](./media/active-directory-groups-dynamic-membership-azure-portal/search-user-management.png)
3. Na folha **Usuários e grupos**, selecione **Todos os grupos**.
   
   ![Abrindo a folha de grupos](./media/active-directory-groups-dynamic-membership-azure-portal/view-groups-blade.png)
4. Na folha **Usuários e grupos – Todos os grupos**, selecione o comando **Adicionar**.
   
   ![Adicione o novo grupo](./media/active-directory-groups-dynamic-membership-azure-portal/add-group-type.png)
5. Na folha **Grupo**, insira um nome e uma descrição para o novo grupo. Selecione um **Tipo de associação** de **Usuário Dinâmico** ou **Dispositivo Dinâmico**, dependendo se você deseja criar uma regra de usuários ou dispositivos e, em seguida, selecione **Adicionar consulta dinâmica**. Para os atributos usados para regras de dispositivo, consulte [Usando atributos para criar regras para objetos de dispositivo](#using-attributes-to-create-rules-for-device-objects).
   
   ![Adicionar regra de associação dinâmica](./media/active-directory-groups-dynamic-membership-azure-portal/add-dynamic-group-rule.png)
6. Na folha **Regras de associação dinâmica**, insira a regra na caixa **Adicionar regra de associação dinâmica avançada**, pressione Enter e, em seguida, selecione **Criar** na parte inferior da folha.
7. Selecione **Criar** na folha **Grupo** para criar o grupo.

## Construção do corpo de uma regra avançada
A regra avançada que você pode criar para os membros dinâmicos para grupos é essencialmente uma expressão binária que consiste em três partes e resulta em um resultado verdadeiro ou falso. As três partes são:

* Parâmetro da esquerda
* Operador binário
* Constante à direita

Uma regra avançada completa é semelhante a esta: (leftParameter binaryOperator "RightConstant"), em que o parêntese de abertura e fechamento são necessários para toda a expressão binária, aspas duplas são necessárias para a constante à direita e a sintaxe do parâmetro esquerdo é user.property. Uma regra avançada pode consistir em mais de uma expressão binária separada pelos operadores lógicos -and, -or e -not.

A seguir é exemplos de uma regra avançada construída de maneira adequada:

* (user.department -eq "Sales") -or (user.department -eq "Marketing")
* (user.department -eq "Sales") -and -not (user.jobTitle -contains "SDE")

Para a lista completa de parâmetros com suporte e operadores de regra de expressão, consulte as seções a seguir. Para os atributos usados para regras de dispositivo, consulte [Usando atributos para criar regras para objetos de dispositivo](#using-attributes-to-create-rules-for-device-objects).

O comprimento total do corpo da sua regra avançada não pode exceder 2048 caracteres.

> [!NOTE]
> Operações de cadeia de caracteres e regex diferenciam maiúsculas de minúsculas. Você também pode executar verificações de Null, usando $null como uma constante, por exemplo, user.department - eq $null. As cadeias de caracteres que contém aspas "devem ser ignoradas usando ' caracteres, por exemplo, user.department -eq `"Sales".
> 
> 

## Operadores de regra de expressão com suporte
A tabela a seguir lista todos os operadores de regra de expressão com suporte e sua sintaxe a ser usada no corpo da regra avançada:

| Operador | Sintaxe |
| --- | --- |
| Não é igual a |-ne |
| É igual a |-eq |
| Não começa com |-notStartsWith |
| Começa com |-startsWith |
| Não contém |-notContains |
| Contém: |-contains |
| Não corresponde |-notMatch |
| Corresponde |-match |

## Correção do erro de consulta
A seguinte tabela relacionará os possíveis erros e como corrigi-los, se ocorrerem

| Erro de análise de consulta | Erros de uso | Uso corrigido |
| --- | --- | --- |
| Erro: O atributo não tem suportado. |(user.invalidProperty -eq "Valor") |(user.department -eq "value") A propriedade <br/> deve corresponder a uma na [lista de propriedades com suporte](#supported-properties). |
| Erro: Operador não é tem suportada no atributo. |(user.accountEnabled -contains true) |(user.accountEnabled -eq true) A propriedade <br/> é do tipo booliano. Use os operadores com suporte (-eq or -ne) em um tipo booleano da lista acima. |
| Erro: Erro de compilação de consulta. |(user.department -eq "Sales") -and (user.department -eq "Marketing")(user.userPrincipalName -match "*@domain.ext") |(user.department -eq "Sales") -and (user.department -eq "Marketing")<br/>Operador lógico deve corresponder a um na lista de propriedades com suporte acima. (user.userPrincipalName-corresponde a ".*@domain.ext") ou (user.userPrincipalName-correspondência "@domain.ext$") Error na expressão regular. |
| Erro: Expressão binária não está no formato correto. |user.department – eq ("Vendas") user.department - eq ("Vendas") user.department-eq ("Vendas") |(user.accountEnabled -eq true) -e (user.userPrincipalName -contains "alias@domain")<br/>A consulta tem vários erros. Parênteses não no lugar certo. |
| Erro: Ocorreu um erro desconhecido durante a configuração de membros dinâmicos. |(user.accountEnabled -eq "True" AND user.userPrincipalName -contains "alias@domain") |(user.accountEnabled -eq true) -e (user.userPrincipalName -contains "alias@domain")<br/>A consulta tem vários erros. Parênteses não no lugar certo. |

## Propriedades com suporte
Estas são todas as propriedades do usuário que você pode usar na regra avançada:

### Propriedades de tipo booleano
Operadores permitidos

* -eq
* -ne

| Propriedades | Valores permitidos | Uso |
| --- | --- | --- |
| accountEnabled |verdadeiro, falso |user.accountEnabled -eq true) |
| dirSyncEnabled |true false null |(user.dirSyncEnabled -eq true) |

### Propriedades do tipo cadeia de caracteres
Operadores permitidos

* -eq
* -ne
* -notStartsWith
* -StartsWith
* -contains
* -notContains
* -match
* -notMatch

| Propriedades | Valores permitidos | Uso |
| --- | --- | --- |
| city |Qualquer valor de cadeia de caracteres ou $null |(user.city -eq "valor") |
| country |Qualquer valor de cadeia de caracteres ou $null |(user.country -eq "valor") |
| department |Qualquer valor de cadeia de caracteres ou $null |(user.department -eq "valor") |
| displayName |Um valor de cadeia de caracteres. |(user. DisplayName -eq "valor") |
| facsimileTelephoneNumber |Qualquer valor de cadeia de caracteres ou $null |user.facsimileTelephoneNumber -eq ("valor") |
| givenName |Qualquer valor de cadeia de caracteres ou $null |user.givenName -eq ("valor") |
| jobTitle |Qualquer valor de cadeia de caracteres ou $null |(user.jobTitle - eq "valor") |
| mail |Qualquer valor de cadeia de caracteres ou $null (endereço SMTP do usuário) |(user.mail - eq "valor") |
| mailNickName |Qualquer valor de cadeia de caracteres (alias de email do usuário) |(user.mailNickName - eq "valor") |
| Serviço Móvel |Qualquer valor de cadeia de caracteres ou $null |(user.mobile -eq "valor") |
| ID do objeto |GUID do objeto de usuário |(user.objectId - eq "1111111-1111-1111-1111-111111111111") |
| passwordPolicies |None DisableStrongPassword DisablePasswordExpiration DisablePasswordExpiration, DisableStrongPassword |(user.passwordPolicies -eq "DisableStrongPassword") |
| physicalDeliveryOfficeName |Qualquer valor de cadeia de caracteres ou $null |(user.physicalDeliveryOfficeName -eq "valor") |
| postalCode |Qualquer valor de cadeia de caracteres ou $null |(user.postalCode - eq "valor") |
| preferredLanguage |ISO 639-1 code |(user.preferredLanguage - eq "pt-BR") |
| sipProxyAddress |Qualquer valor de cadeia de caracteres ou $null |(user.sipProxyAddress -eq "valor") |
| state |Qualquer valor de cadeia de caracteres ou $null |(user.state -eq "valor") |
| streetAddress |Qualquer valor de cadeia de caracteres ou $null |(user.streetAddress -eq "valor") |
| sobrenome |Qualquer valor de cadeia de caracteres ou $null |(user.surname -eq "valor") |
| telephoneNumber |Qualquer valor de cadeia de caracteres ou $null |(user.telephoneNumber -eq "valor") |
| usageLocation |Código do país indicados dois |(user.usageLocation -eq "EUA") |
| userPrincipalName |Um valor de cadeia de caracteres. |(user.userPrincipalName -eq "alias@domain") |
| userType |member guest $null |(ser.userType -eq "Membro") |

### Propriedades de coleção de cadeias de caracteres de tipo
Operadores permitidos

* -contains
* -notContains

| Properties | Valores permitidos | Uso |
| --- | --- | --- |
| otherMails |Um valor de cadeia de caracteres. |(user.otherMails -contains "alias@domain") |
| proxyAddresses |SMTP:alias@domainsmtp:alias@domain |(user.proxyAddresses -contains "SMTP: alias@domain") |

## Atributos de extensão e atributos personalizados
Os atributos de extensão e os atributos personalizados têm suporte das regras de associação dinâmica.

Os atributos de extensão são sincronizados a partir do AD do Windows Server local e têm o formato "ExtensionAttributeX", onde X é igual a 1 a 15. Um exemplo de uma regra que usa um atributo de extensão:

(user.extensionAttribute15 -eq "Marketing")

Os Atributos Personalizados são sincronizados do Windows Server AD local ou de um aplicativo SaaS conectado e têm formato "user.extension_[GUID]\__[Atributo]", onde [GUID] é o identificador exclusivo no AAD para o aplicativo que criou o atributo no AAD e [Atributo] é o nome do atributo como ele foi criado. Um exemplo de uma regra que usa um atributo personalizado:

user.extension\_c272a57b722d4eb29bfe327874ae79cb\_\_OfficeNumber

O nome do atributo personalizado pode ser encontrado no diretório por meio da consulta do atributo de um usuário, usando o Graph Explorer e procurando o nome do atributo.

## Regra de relatórios diretos
Agora você pode preencher os membros de um grupo com base no atributo gerenciador de um usuário.

**Para configurar um grupo como "Gerenciador"**

1. Siga as etapas 1 a 5 em [Para criar a regra avançada](#to-create-the-advanced-rule) e selecione um **Tipo de associação** de **Usuário dinâmico**.
2. Na folha **Regras de associação dinâmica**, insira a regra com a seguinte sintaxe:
   
    Relatórios diretos para *Relatórios diretos para {obectID\_of\_manager}*. Um exemplo de regra válida para Funcionários Subordinados:
   
                    Direct Reports for "62e19b97-8b3d-4d4a-a106-4ce66896a863”
   
    onde “62e19b97-8b3d-4d4a-a106-4ce66896a863” é a objectID do gerente. A ID de objeto pode ser encontrada no Azure AD na **guia Perfil** da página de usuário para o usuário que for o gerente.
3. Ao salvar essa regra, todos os usuários que atendem à regra serão adicionados como membros do grupo. Pode levar alguns minutos para o preenchimento inicial do grupo.

## Usar atributos para criar regras para objetos de dispositivo
Você também pode criar uma regra que seleciona objetos de dispositivo para associação em um grupo. Os seguintes atributos de dispositivo podem ser usados:

| Propriedades | Valores permitidos | Uso |
| --- | --- | --- |
| displayName |qualquer valor de cadeia de caracteres |(device.displayName -eq "Rob Iphone”) |
| deviceOSType |qualquer valor de cadeia de caracteres |(device.deviceOSType -eq "IOS") |
| deviceOSVersion |qualquer valor de cadeia de caracteres |(device.OSVersion -eq "9.1") |
| isDirSynced |true false null |(device.isDirSynced -eq "true") |
| isManaged |true false null |(device.isManaged -eq "false") |
| isCompliant |true false null |(device.isCompliant -eq "true") |

## Informações adicionais
Esses artigos fornecem mais informações sobre grupos no Azure Active Directory.

* [Consultar grupos existentes](active-directory-groups-view-azure-portal.md)
* [Criar um novo grupo e adicionar membros](active-directory-groups-create-azure-portal.md)
* [Gerenciar configurações de um grupo](active-directory-groups-settings-azure-portal.md)
* [Gerenciar associações de um grupo](active-directory-groups-membership-azure-portal.md)
* [Gerenciar regras dinâmicas para usuários em um grupo](active-directory-groups-dynamic-membership-azure-portal.md)

<!---HONumber=AcomDC_0914_2016-->