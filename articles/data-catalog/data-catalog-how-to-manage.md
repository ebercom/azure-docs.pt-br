---
title: Como gerenciar ativos de dados | Microsoft Docs
description: "Artigo de instruções que destaca como controlar a visibilidade e a propriedade de ativos de dados registrados no Catálogo de Dados do Azure."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 623f5ed4-8da7-48f5-943a-448d0b7cba69
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 10/04/2016
ms.author: maroche
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 49d939205c85bad8bf7422ef4d9fa02501bb8df7


---
# <a name="how-to-manage-data-assets"></a>Como gerenciar ativos de dados
## <a name="introduction"></a>Introdução
**Catálogo de Dados do Azure** fornece recursos para descoberta de fonte de dados, habilitando os usuários a descobrir e entender facilmente as fontes de dados de que precisam para executar análises e tomar decisões. Esses recursos de descoberta têm maior impacto quando todos os usuários podem localizar e compreender a mais ampla variedade de fontes de dados disponíveis. Tendo isso em mente, o comportamento padrão do Catálogo de Dados é que todas as fontes de dados registradas sejam visível e detectáveis por todos os usuários do catálogo.

O Catálogo de Dados não dá aos usuários acesso aos dados em si. O acesso aos dados é controlado pelo proprietário da fonte de dados. O Catálogo de Dados permite que os usuários descubram fontes de dados e exibam os metadados relacionados às fontes registradas no catálogo.

Porém, pode haver situações em que as fontes de dados só devam estar visíveis para usuários específicos ou membros de grupos. Nesses cenários, o Catálogo de Dados permite que os usuários se apropriem de ativos de dados registrados no catálogo e, em seguida, controlem a visibilidade dos ativos de propriedade sua.

> [!NOTE]
> A funcionalidade descrita neste artigo está disponível apenas na Edição Standard do Catálogo de Dados do Azure. A Edição Gratuita não fornece recursos para propriedade e restrição de visibilidade de ativos de dados.
> 
> 

## <a name="managing-ownership-of-data-assets"></a>Gerenciamento de propriedade de ativos de dados
Por padrão, os ativos de dados registrados no Catálogo de Dados não têm um proprietário; qualquer usuário com permissão para acessar o catálogo pode descobrir e anotar esses ativos. Os usuários podem se apropriar de ativos de dados sem proprietário e podem limitar a visibilidade dos ativos de propriedade sua.

Quando um ativo de dados no Catálogo de Dados tem um proprietário, apenas usuários autorizados pelos proprietários podem descobrir o ativo e exibir seus metadados, e apenas os proprietários podem excluir o ativo do Catálogo.

> [!NOTE]
> A propriedade no Catálogo de Dados só afeta os metadados armazenados no Catálogo. Ela não concede permissões para a fonte de dados subjacente.
> 
> 

### <a name="taking-ownership"></a>Apropriação
Os usuários podem se apropriar de ativos de dados selecionando a opção "Apropriar-se" no portal do Catálogo de Dados. Nenhuma permissão especial é necessária para apropriar-se de um ativo de dados sem proprietário; qualquer usuário pode apropriar-se de um ativo de dados sem proprietário.

### <a name="adding-owners-and-co-owners"></a>Adição de proprietários e coproprietários
Se um ativo de dados já tem um proprietário, os usuários não podem simplesmente apropriar-se dele. Eles devem ser adicionados como coproprietários por um proprietário existente. Qualquer proprietário pode adicionar outros usuários ou grupos de segurança como coproprietários.

> [!NOTE]
> É uma prática recomendada ter pelo menos dois indivíduos como proprietários para qualquer ativo de dados com proprietário.
> 
> 

### <a name="removing-owners"></a>Remoção de proprietários
Assim como qualquer proprietário ativo pode adicionar coproprietários, qualquer proprietário ativo pode remover qualquer coproprietário.

Se o proprietário de um ativo remover a si próprio como proprietário, ele não poderá mais gerenciar o ativo. Se o proprietário de um ativo remover a si próprio como proprietário e não houver outros coproprietários, o ativo será revertido para um estado sem proprietário.

## <a name="visibility"></a>Visibilidade
Os proprietários de ativos de dados podem controlar a visibilidade dos ativos de dados que eles possuem. Para restringir a visibilidade do padrão (em que todos os usuários do Catálogo de Dados podem descobrir e exibir o ativo de dados), o proprietário do ativo pode alternar a configuração de visibilidade de "Todos" para "Proprietários e estes Usuários" nas propriedades do ativo. Em seguida, os proprietários podem adicionar usuários e grupos de segurança específicos.

> [!NOTE]
> Sempre que possível, as permissões de propriedade e visibilidade de ativo devem ser atribuídas a grupos de segurança e não a usuários individuais.
> 
> 

## <a name="catalog-administrators"></a>Administradores do Catálogo
Os administradores do Catálogo de Dados são, implicitamente, coproprietários de todos os ativos no Catálogo. Os proprietários de ativos não podem remover a visibilidade dos administradores do Catálogo, e os administradores podem gerenciar a propriedade e a visibilidade de todos os ativos de dados no Catálogo.

## <a name="summary"></a>Resumo
O modelo de crowdsourcing do Catálogo de Dados para descoberta de ativos de dados e metadados permite que todos os usuários do Catálogo contribuam e descubram. A Edição Standard do Catálogo de Dados fornece recursos de propriedade e de gerenciamento para limitar a visibilidade e o uso de ativos de dados específicos.




<!--HONumber=Nov16_HO3-->


