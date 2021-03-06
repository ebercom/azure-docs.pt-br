---
title: Restaurar um SQL Data Warehouse do Azure (Portal) | Microsoft Docs
description: Tarefas do portal do Azure para restaurar um Azure SQL Data Warehouse.
services: sql-data-warehouse
documentationcenter: NA
author: Lakshmi1812
manager: barbkess
editor: 
ms.assetid: b0aef539-7657-4b0e-9899-74098f5c21bc
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 09/21/2016
ms.author: lakshmir;barbkess;sonyama
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 2cb8cb2b58df5cc209b1f966c792ca0f4082e652


---
# <a name="restore-an-azure-sql-data-warehouse-portal"></a>Restaurar um Azure SQL Data Warehouse (Portal)
> [!div class="op_single_selector"]
> * [Visão geral][Visão geral]
> * [Portal][Portal]
> * [PowerShell][PowerShell]
> * [REST][REST]
> 
> 

Neste artigo, você aprenderá como restaurar um Azure SQL Data Warehouse usando o Portal do Azure.

## <a name="before-you-begin"></a>Antes de começar
**Verifique sua capacidade de DTU.**  Cada SQL Data Warehouse é hospedado por um servidor SQL (por exemplo, myserver.database.windows.net) que tem uma cota de DTU padrão.  Antes de restaurar um SQL Data Warehouse, verifique se o SQL Server tem cota de DTU suficiente restante para o banco de dados que está sendo restaurado. Para saber como calcular a DTU necessária ou solicitar mais DTU, veja [Solicitar uma alteração de cota de DTU][Solicitar uma alteração de cota de DTU].

## <a name="restore-an-active-or-paused-database"></a>Restaurar um banco de dados ativo ou pausado
Para restaurar um banco de dados:

1. Faça logon no [Portal do Azure][Portal do Azure]
2. No lado esquerdo da tela, escolha **Procurar** e **Servidores SQL**
   
    ![](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)
3. Navegue até o servidor e selecione-o
   
    ![](./media/sql-data-warehouse-restore-database-portal/01-select-server.png)
4. Localize o SQL Data Warehouse do qual você quer restaurar e selecione-o
   
    ![](./media/sql-data-warehouse-restore-database-portal/01-select-active-dw.png)
5. Na parte superior da folha Data Warehouse, clique em **Restaurar**
   
    ![](./media/sql-data-warehouse-restore-database-portal/01-select-restore-from-active.png)
6. Especifique um novo **Nome de banco de dados**
7. Escolha o **Ponto de Restauração**
   
   1. Certifique-se de escolher o ponto de restauração mais recente.  Uma vez que os pontos de restauração são mostrados em UTC, às vezes, a opção padrão mostrada não é o ponto de restauração mais recente.
      
      ![](./media/sql-data-warehouse-restore-database-portal/01-restore-blade-from-active.png)
8. Clique em **OK**
9. O processo de restauração do banco de dados começará e poderá ser monitorado usando **NOTIFICAÇÕES**

> [!NOTE]
> Depois que a restauração estiver concluída, você poderá configurar o banco de dados recuperado seguindo [Configurar o banco de dados após a recuperação][Configurar o banco de dados após a recuperação].
> 
> 

## <a name="restore-a-deleted-database"></a>Restaurar um banco de dados excluído
Para restaurar um banco de dados excluído:

1. Faça logon no [Portal do Azure][Portal do Azure]
2. No lado esquerdo da tela, escolha **Procurar** e **Servidores SQL**
   
    ![](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)
3. Navegue até o servidor e selecione-o
   
    ![](./media/sql-data-warehouse-restore-database-portal/02-select-server.png)
4. Role para baixo até a seção Operações na folha do seu servidor
5. Clique no bloco **Bancos de Dados Excluídos**
   
    ![](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dws.png)
6. Escolha o banco de dados excluído que deseja restaurar
   
    ![](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dw.png)
7. Especifique um novo **Nome de banco de dados**
   
    ![](./media/sql-data-warehouse-restore-database-portal/02-restore-blade-from-deleted.png)
8. Clique em **OK**
9. O processo de restauração do banco de dados começará e poderá ser monitorado usando **NOTIFICAÇÕES**

> [!NOTE]
> Para configurar seu banco de dados após a conclusão da restauração, consulte [Configurar o banco de dados após a recuperação][Configurar o banco de dados após a recuperação].
> 
> 

## <a name="next-steps"></a>Próximas etapas
Para saber mais sobre os recursos de continuidade dos negócios das edições do Banco de Dados SQL do Azure, leia a [Visão geral da continuidade dos negócios do Banco de Dados SQL do Azure][Visão geral da continuidade dos negócios do Banco de Dados SQL do Azure].

<!--Image references-->

<!--Article references-->
[Visão geral da continuidade dos negócios do Banco de Dados SQL do Azure]: ../sql-database/sql-database-business-continuity.md
[Visão geral]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md
[Configurar o banco de dados após a recuperação]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery
[Solicitar uma alteração de cota de DTU]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change

<!--MSDN references-->

<!--Blog references-->

<!--Other Web references-->
[Portal do Azure]: https://portal.azure.com/



<!--HONumber=Nov16_HO3-->


