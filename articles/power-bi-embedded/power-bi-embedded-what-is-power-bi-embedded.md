---
title: "O que é o Microsoft Power BI Embedded?"
description: "O Power BI Embedded possibilita que você integre relatórios do Power BI a seus aplicativos móveis ou Web para que não precise compilar soluções personalizadas."
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 03649b72-b7d7-40ca-b077-12356d72d4f3
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 01/06/2017
ms.author: asaxton
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 81215430b5731a53d4f4700499c1d9af963e712c


---
# <a name="what-is-microsoft-power-bi-embedded"></a>O que é o Microsoft Power BI Embedded?
Com **Power BI Embedded**, você pode integrar relatórios do Power BI diretamente a seus aplicativos móveis ou Web.

![](media/powerbi-embedded-whats-is/what-is.png)

O **Power BI Embedded** é um serviço do Azure que permite que ISVs e desenvolvedores de aplicativo revelem experiências de dados do Power BI em seus aplicativos. Como desenvolvedor, você criou aplicativos, e esses aplicativos têm seus próprios usuários e um conjunto distinto de recursos. Esses aplicativos também podem ter alguns elementos de dados internos, como gráficos e relatórios que agora podem ser alimentados pelo Microsoft Power BI Embedded. Você não precisa de uma conta do Power BI para usar seu aplicativo. Você pode continuar entrando no aplicativo como antes e exibir e interagir com a experiência de emissão de relatórios do Power BI sem precisar qualquer licença adicional.

## <a name="licensing-for-microsoft-power-bi-embedded"></a>Licenciamento do Microsoft Power BI Embedded
No modelo de uso do **Microsoft Power BI Embedded** , o licenciamento para o Power BI não é de responsabilidade do usuário final.  Em vez disso, **sessões** são compradas pelo desenvolvedor do aplicativo que está consumindo os elementos visuais e são cobradas da assinatura proprietária de tais recursos. Informações adicionais podem ser encontradas na página de preços (https://azure.microsoft.com/en-us/pricing/details/power-bi-embedded/).

## <a name="microsoft-power-bi-embedded-conceptual-model"></a>Modelo Conceitual do Microsoft Power BI Embedded
![](media/powerbi-embedded-whats-is/model.png)

Como qualquer outro serviço no Azure, recursos para o Power BI Embedded são provisionados por meio de [APIs do Azure Resource Manager](https://msdn.microsoft.com/library/mt712306.aspx). Nesse caso, o recurso que você provisiona é uma **Coleção de Espaços de Trabalho do Power BI**.

## <a name="workspace-collection"></a>Coleção de Espaços de Trabalho
Uma **Coleção de Espaços de Trabalho** é o contêiner do Azure de nível mais elevado para recursos que contém 0 ou mais **Espaços de Trabalho**.  Uma **Espaço de trabalho** **Espaços de Trabalho** has all of the standard Umazure properties, as well as the following:

* **Chaves de Acesso** – chaves usadas ao chamar com segurança as APIs do Power BI (descritas em uma seção posterior).
* **Usuários** – Os usuários do AAD (Azure Active Directory) que têm direitos de administrador para gerenciar a Coleção de Espaços de Trabalho do Power BI por meio do portal do Azure ou da API do Azure Resource Manager.
* **Região** – como parte do provisionamento de uma **Coleção de Espaços de Trabalho**, você pode selecionar uma região na qual ela será provisionada. Para obter mais informações, consulte [Regiões do Azure](https://azure.microsoft.com/regions/).

## <a name="workspace"></a>Espaço de trabalho
Um **Espaço de Trabalho** é um contêiner de conteúdo do Power BI, que pode incluir conjuntos de dados, relatórios e painéis. Um **Espaço de Trabalho** fica vazio logo após ser criado pela primeira vez. Durante a Visualização, você criará todo o conteúdo usando o Power BI Desktop e implantará o PBIX por meio de programação em um dos seus espaços de trabalho usando a [API Import do Power BI](https://msdn.microsoft.com/library/mt711504.aspx).

## <a name="using-workspace-collections-and-workspaces"></a>Usando Coleções de Espaços de Trabalho e Espaços de Trabalho
**Coleções de Espaços de Trabalho** e **Espaços de Trabalho** são contêineres de conteúdo que são usados e organizados da forma que melhor se encaixar no design do aplicativo que você está criando. Haverá muitas maneiras diferentes em que você poderia organizar o conteúdo deles. Você pode optar por colocar todo o conteúdo dentro de um espaço de trabalho e, depois, usar tokens de aplicativo para subdividir ainda mais o conteúdo entre seus clientes. Você também pode optar por colocar todos os seus clientes em espaços de trabalho separados, para que haja alguma separação entre eles. Ou, você pode optar por organizar os usuários por região em vez de organizá-los por cliente. Esse design flexível permite que você escolha a melhor maneira de organizar o conteúdo.

## <a name="cached-datasets"></a>Conjuntos de Dados em Cache
Conjuntos de dados armazenados em cache podem ser usados em Preview.  No entanto, você não pode atualizar os dados armazenados em cache após eles serem carregados no **Microsoft Power BI Embedded**.

## <a name="authentication-and-authorization-with-app-tokens"></a>Autenticação e autorização com tokens de aplicativo
**Microsoft Power BI Embedded** transfere para o seu aplicativo para executar toda a autorização e autenticação de usuário necessárias. Não há nenhum requisito explícito de que os usuários finais sejam os clientes do Azure Active Directory (Azure AD).  Em vez disso, o aplicativo dá ao **Microsoft Power BI Embedded** autorização expressa para renderizar um relatório do Power BI pelo uso de **Tokens de Autenticação de Aplicativo (Tokens de Aplicativo)**.  Esses **Tokens de Aplicativo** são criados conforme necessário, quando seu aplicativo deseja renderizar um relatório.

![](media/powerbi-embedded-whats-is/app-tokens.png)

**Tokens de Autenticação de Aplicativo (Tokens de Aplicativo)** são usados para autenticação no **Microsoft Power BI Embedded**.  Há três tipos de **Tokens de Aplicativo**:

1. Tokens de Provisionamento – usados ao provisionar um novo **Espaço de Trabalho** em uma **Coleção de Espaços de Trabalho**
2. Tokens de Desenvolvimento – Usados ao fazer chamadas diretamente para as **APIs REST do Power BI**
3. Tokens de Inserção - usados ao fazer chamadas para renderizar um relatório no iframe inserido

Esses tokens são usados para as várias fases de suas interações com **Microsoft Power BI Embedded**.  Os tokens são projetados para que você possa delegar permissões do seu aplicativo para o Power BI. Para obter mais informações, consulte o [Fluxo de Tokens de Aplicativo](power-bi-embedded-app-token-flow.md).

## <a name="see-also"></a>Consulte também
* [Cenários comuns do Microsoft Power BI Embedded](power-bi-embedded-scenarios.md)
* [Introdução ao Microsoft Power BI Embedded](power-bi-embedded-get-started.md)



<!--HONumber=Dec16_HO2-->


