---
title: "O banco de dados no servidor não está disponível no momento; conecte-se ao Banco de Dados SQL | Microsoft Docs"
description: "Erro &quot;A solução de problemas de banco de dados no servidor não está disponível no momento&quot; quando um aplicativo se conecta ao Banco de Dados SQL."
services: sql-database
documentationcenter: 
author: dalechen
manager: cshepard
editor: 
keywords: "o banco de dados no servidor não está disponível atualment; conecte-se ao banco de dados sql"
ms.assetid: f61999ac-d46b-448a-8830-3b04978d84ec
ms.service: sql-database
ms.custom: troubleshoot
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/20/2017
ms.author: daleche
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: ede7672196e71bc493cd9b758982445e991def1b


---
# <a name="error-database-on-server-is-not-currently-available-when-connecting-to-sql-database"></a>Erro "O banco de dados no servidor não está disponível atualmente" ao se conectar a um banco de dados sql
[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

Quando um aplicativo se conecta a um banco de dados SQL do Azure, a seguinte mensagem de erro é exibida:

```
Error code 40613: "Database <x> on server <y> is not currently available. Please retry the connection later. If the problem persists, contact customer support, and provide them the session tracing ID of <z>"
```

> [!NOTE]
> Essa mensagem de erro normalmente é transitória (de vida curta).
> 
> 

Esse erro ocorre quando o banco de dados do Azure está sendo movido (ou reconfigurados) e seu aplicativo perde a conexão com o banco de dados SQL. Eventos de reconfiguração do banco de dados SQL ocorrem devido a um evento planejado (por exemplo, uma atualização de software) ou um evento não planejado (por exemplo, uma falha no processo ou balanceamento de carga). A maioria dos eventos de reconfiguração geralmente é de curta duração e deve ser concluída em, no máximo, de 60 segundos. No entanto, esses eventos ocasionalmente podem levar mais tempo para serem concluídos, como quando uma transação grande causa uma recuperação de execução longa.

## <a name="steps-to-resolve-transient-connectivity-issues"></a>Etapas para resolver problemas de conectividade temporários
1. Confira o [Painel de Serviços do Microsoft Azure](https://azure.microsoft.com/status) quanto a quaisquer interrupções conhecidas que tenham ocorrido durante o tempo em que o erro foi relatado pelo aplicativo.
2. Aplicativos que se conectam a um serviço de nuvem, como Banco de Dados SQL, devem esperar eventos reconfiguração periódicos e implementar lógica de repetição para lidar com esses erros, em vez de exibir esses erros como erros de aplicativo aos usuários. Confira a seção [Erros transitórios](sql-database-connectivity-issues.md) e práticas recomendadas e diretrizes de design na [Visão geral de Desenvolvimento do Banco de Dados SQL](sql-database-develop-overview.md) para saber mais e ver estratégias de repetição gerais. Em seguida, confira os exemplos de código em [Bibliotecas de Conexões para Banco de Dados SQL e SQL Server](sql-database-libraries.md) para obter as especificações.
3. Conforme um banco de dados se aproxima dos limites de recursos, pode parecer que há um problema de conectividade temporário. Confira [Solução de problemas de desempenho](sql-database-troubleshoot-performance.md).
4. Se problemas de conectividade continuarem, se a duração pela qual o aplicativo encontra o erro exceder 60 segundos ou se você vir várias ocorrências do erro em um determinado dia, envie uma solicitação de suporte do Azure selecionando **Obter Suporte** no site de [Suporte do Azure](https://azure.microsoft.com/support/options) .

## <a name="next-steps"></a>Próximas etapas
* Se você receber um erro diferente, avalie a [mensagem de erro](sql-database-develop-error-messages.md) para dicas sobre a causa.
* Se o problema persistir, visite a orientação em [Solucionar problemas comuns de conexão para o Banco de Dados SQL do Azure](sql-database-troubleshoot-common-connection-issues.md).




<!--HONumber=Nov16_HO3-->


