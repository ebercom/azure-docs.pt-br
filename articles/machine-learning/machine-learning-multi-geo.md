---
title: "Documentação de ajuda de várias áreas geográficas | Microsoft Docs"
description: "Saiba como criar um espaço de trabalho e publicar um serviço Web em uma região do Azure diferente do Centro-Sul dos Estados Unidos (SCUS)."
services: machine-learning
documentationcenter: 
author: tedway
manager: jhubbard
editor: rmca14
tags: 
ms.assetid: ed0ca8a8-fa53-4e56-b824-2d7e44641967
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/16/2016
ms.author: tedway; neerajkh
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: b95d7d7b11af4dcd3ed814f31697f5d1b617f64b


---
# <a name="multi-geo-help-documentation"></a>Documentação da Ajuda para diversas áreas geográficas
Este artigo descreve como criar um espaço de trabalho e publicar um serviço Web em outras regiões do Azure.

## <a name="create-a-workspace"></a>Criar um espaço de trabalho
1. Entre no Portal Clássico do Azure.
2. Clique em **+ NOVO** > **SERVIÇOS DE DADOS** > **ACHINE LEARNING** > **CRIAÇÃO RÁPIDA**.  Em **LOCAL**, selecione outra região, como **Sudeste Asiático**.
   ![Imagem de Ajuda de várias áreas geográficas 1][1]
3. Selecione o espaço de trabalho e, em seguida, clique em **Entrar no Estúdio AM**.
   ![Imagem de Ajuda de várias áreas geográficas 2][2]
4. Agora você tem um espaço de trabalho em outra região que pode usar como qualquer outro espaço de trabalho. Para alternar entre seus espaços de trabalho, veja o canto superior direito da tela. Clique na lista suspensa, selecione a região e, em seguida, selecione o espaço de trabalho. Tudo é local à região do espaço de trabalho; por exemplo, todos os serviços Web criados por meio de um espaço de trabalho estarão na mesma região em que o espaço de trabalho está localizado.
   ![Imagem de Ajuda de várias áreas geográficas 3][3]

## <a name="open-an-experiment-from-gallery"></a>Abrir um experimento da Galeria
Se você abrir um experimento da galeria, também pode selecionar a região para a qual deseja copiar o experimento.

![Imagem de Ajuda de várias áreas geográficas 4][4a]

## <a name="web-service-management"></a>Gerenciamento de serviço Web
Para gerenciar os serviços Web de forma programática, como um novo treinamento, use o endereço específico da região:

* https://asiasoutheast.management.azureml.net
* https://europewest.management.azureml.net

### <a name="things-to-note"></a>Elementos a serem observados
1. Dessa maneira, você só pode copiar experimentos entre espaços de trabalho que pertençam à mesma região. Se precisar copiar experimentos entre espaços de trabalho em regiões diferentes, você poderá usar o cmdlet do [PowerShell](http://aka.ms/amlps), [*Copy-AmlExperiment *](https://github.com/hning86/azuremlps/blob/master/README.md#copy-amlexperiment) para fazer isso. Outra solução alternativa é publicar o experimento na Galeria no modo não listado e depois abri-lo no espaço de trabalho da outra região.
2. O seletor de região mostrará apenas os espaços de trabalho para uma região por vez. No futuro, você poderá ver uma lista completa de espaços de trabalho aos quais tem acesso em todas as regiões ao mesmo tempo.  
3. Um espaço de trabalho livre ou de acesso de convidado (anônimo) será criado e hospedado no Centro-Sul dos EUA No futuro, você poderá criar espaços de trabalho de acesso livre/de convidado na região que escolher.  
4. Os serviços Web implantados por meio de um espaço de trabalho no Sudeste Asiático também serão hospedados no Sudeste Asiático. No futuro, você terá a flexibilidade de criar experiências em uma região e implantar pontos de extremidade de serviço Web gerados em regiões diferentes.  

## <a name="more-information"></a>Mais informações
Fazer uma pergunta no [fórum de Aprendizado de Máquina do Azure](https://social.msdn.microsoft.com/Forums/azure/home?forum=MachineLearning).

<!--Image references-->
[1]: ./media/machine-learning-multi-geo/multi-geo_1.png
[2]: ./media/machine-learning-multi-geo/multi-geo_2.png
[3]: ./media/machine-learning-multi-geo/multi-geo_3.png
[4a]: ./media/machine-learning-multi-geo/multi-geo_4a.png



<!--HONumber=Nov16_HO3-->


