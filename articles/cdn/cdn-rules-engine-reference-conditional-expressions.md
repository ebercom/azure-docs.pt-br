---
title: "Expressões condicionais do mecanismo de regras da Rede de Distribuição de Conteúdo do Azure | Microsoft Docs"
description: "Este tópico descreve os recursos e condições de correspondência do Mecanismo de regras"
services: cdn
documentationcenter: 
author: Lichard
manager: akucer
editor: 
ms.assetid: 669ef140-a6dd-4b62-9b9d-3f375a14215e
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2016
ms.author: rli
translationtype: Human Translation
ms.sourcegitcommit: 8a5d98bdc737fd9476b9db42100f58ed28619879
ms.openlocfilehash: 92cb8832de934c19164bc26e688142538a8ba96c


---

# <a name="conditional-expressions-for-azure-content-delivery-network-cdn-rules-engine"></a>Expressões condicionais para o Mecanismo de regras da CDN (Rede de Distribuição de Conteúdo) do Azure
Este tópico lista descrições detalhadas das Expressões condicionais para o [Mecanismo de regras](cdn-rules-engine.md) da CDN (Rede de Distribuição de Conteúdo) do Azure.

A primeira parte de uma regra é a Expressão condicional.

Expressão condicional | Descrição
-----------------------|-------------
IF | Uma expressão IF é sempre uma parte da primeira instrução em uma regra. Como todas as outras expressões condicionais, essa instrução IF deve ser associada a uma correspondência. Se nenhuma expressão condicional adicional for definida, essa correspondência determinará o critério que deve ser atendido antes que um conjunto de recursos possa ser aplicado a uma solicitação.
AND IF | Uma expressão AND IF só pode ser adicionada após os seguintes tipos de expressões condicionais: IF e AND IF. Ela indica que há outra condição que deve ser atendida para a instrução IF inicial.
ELSE IF| Uma expressão ELSE IF especifica uma condição de alternativa que deve ser atendida antes da ocorrência de um conjunto de recursos específicos a essa instrução ELSE IF. A presença de uma instrução ELSE IF indica o fim da instrução anterior. A única expressão condicional que pode ser colocada após uma instrução ELSE IF é outra instrução ELSE IF. Isso significa que uma instrução ELSE IF só pode ser usada para especificar uma única condição adicional que deve ser atendida.

**Exemplo**: ![Condição de correspondência CDN](./media/cdn-rules-engine-reference/cdn-rules-engine-conditional-expression.png)

 > [!TIP]
   > Uma regra subsequente poderá substituir as ações especificadas por uma regra anterior. Exemplo: uma regra capturar tudo protege todas as solicitações por meio da Autenticação baseada em Token. Outra regra pode ser criada diretamente abaixo dessa, para criar uma exceção para determinados tipos de solicitações.

### <a name="next-steps"></a>Próximas etapas
* [Visão geral da CDN do Azure](cdn-overview.md)
* [Referência do Mecanismo de Regras](cdn-rules-engine-reference.md)
* [Condições de correspondência do Mecanismo de regras](cdn-rules-engine-reference-match-conditions.md)
* [Recursos do Mecanismo de regras](cdn-rules-engine-reference-features.md)
* [Substituindo o comportamento HTTP padrão usando o mecanismo de regras](cdn-rules-engine.md)



<!--HONumber=Dec16_HO3-->


