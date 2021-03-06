---
title: Otimizar o roteamento da ExpressRoute | Microsoft Docs
description: "Esta página fornece detalhes sobre como otimizar o roteamento quando um cliente tem mais de um circuito da Rota Expressa que conecta a Microsoft à rede corporativa do cliente."
documentationcenter: na
services: expressroute
author: charwen
manager: carmonm
editor: 
ms.assetid: fca53249-d9c3-4cff-8916-f8749386a4dd
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/10/2016
ms.author: charwen
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 26f0992e734f0aae96ac6e8b7040d661d5fb063c


---
# <a name="optimize-expressroute-routing"></a>Otimizar o roteamento da Rota Expressa
Quando você tem vários circuitos de Rota Expressa, tem mais de um caminho para se conectar à Microsoft. Portanto, pode ocorrer um roteamento abaixo do ideal, ou seja, o tráfego pode levar um caminho mais longo para acessar a Microsoft e esta para acessar a sua rede. Quanto maior o caminho de rede, maior será a latência. A latência tem impacto direto na experiência de usuário e no desempenho do aplicativo. Este artigo ilustra esse problema e explica como otimizar o roteamento usando tecnologias de roteamento padrão.

## <a name="suboptimal-routing-case-1"></a>Caso de roteamento abaixo do ideal 1
Vamos examinar o problema de roteamento usando um exemplo. Imagine que você tem dois escritórios nos EUA, um em Los Angeles e um em Nova York. Seus escritórios estão conectados em uma WAN (rede de longa distância), que pode ser sua própria rede backbone ou VPN de IP do provedor de serviços. Você tem dois circuitos de Rota Expressa, um no Oeste dos EUA e outro no Leste dos EUA, que também estão conectados à WAN. Obviamente, você tem dois caminhos para se conectar à rede da Microsoft. Agora imagine que você tem a implantação do Azure (por exemplo, o Serviço de Aplicativo do Azure) no Oeste dos EUA e no Leste dos EUA. Sua intenção é conectar seus usuários em Los Angeles ao Azure no Oeste dos EUA e seus usuários em Nova York ao Azure no Leste dos EUA porque seu administrador de serviço anuncia que os usuários em cada escritório acessarão os serviços do Azure mais próximos para uma melhor experiência. Infelizmente, o plano funciona bem para os usuários da Costa Leste, mas não para os usuários da Costa Oeste. A causa do problema é a seguinte: Em cada circuito da Rota Expressa, anunciamos a você tanto o prefixo do Azure no Leste dos EUA (23.100.0.0/16) quanto o prefixo do Azure no Oeste dos EUA (13.100.0.0/16). Se você não souber qual prefixo é de que região, não conseguirá tratá-los de maneira diferente. Sua rede WAN pode pensar que ambos os prefixos são mais próximos do Leste dos EUA que do Oeste dos EUA e, portanto, rotear os usuários de ambos os escritórios para o circuito de Rota Expressa no Leste dos EUA. No final, você terá muitos usuários insatisfeitos no escritório de Los Angeles.

![](./media/expressroute-optimize-routing/expressroute-case1-problem.png)

### <a name="solution-use-bgp-communities"></a>Solução: usar comunidades BGP
Para otimizar o roteamento nos dois usuários, você precisa saber qual é o prefixo do Azure no Oeste dos EUA e no Leste dos EUA. Nós codificamos essas informações usando [Valores da Comunidade BGP](expressroute-routing.md). Atribuímos um valor de Comunidade BGP exclusivo para cada região do Azure, por exemplo, "12076:51004" para o Leste dos EUA, "12076:51006" para o Oeste dos EUA. Agora que você sabe que prefixo é de que região do Azure, pode configurar qual circuito de Rota Expressa deve ser preferencial. Como usamos o BGP para trocar as informações do roteamento, você pode usar a Preferência de Local do BGP para influenciar o roteamento. Em nosso exemplo, você pode atribuir um valor maior de preferência de local a 13.100.0.0/16 no Oeste dos EUA que no Leste dos EUA e, da mesma forma, um valor mais alto de preferência de local a 23.100.0.0/16 no Leste dos EUA que no Oeste dos EUA. Essa configuração fará com que, quando ambos os caminhos para a Microsoft estiverem disponíveis, os usuários em Los Angeles usarão o circuito de Rota Expressa no Oeste dos EUA para se conectar ao Azure no Oeste dos EUA e os usuários em Nova York usarão a Rota Expressa no Leste dos EUA para se conectar ao Azure no Leste dos EUA. O roteamento é otimizado em ambos os lados. 

![](./media/expressroute-optimize-routing/expressroute-case1-solution.png)

## <a name="suboptimal-routing-case-2"></a>Caso de roteamento abaixo do ideal 2
Aqui está outro exemplo em que conexões da Microsoft usam um caminho mais longo para acessar a sua rede. Nesse caso, você usa servidores Exchange locais e o Exchange Online em um [ambiente híbrido](https://technet.microsoft.com/library/jj200581%28v=exchg.150%29.aspx). Seus escritórios estão conectados a uma WAN. Você anuncia os prefixos dos seus servidores locais nos dois escritórios à Microsoft por meio de dois circuitos de Rota Expressa. O Exchange Online irá iniciar conexões com os servidores locais em situações como migração de caixa de correio. Infelizmente, a conexão com o escritório de Los Angeles é roteada para o circuito da Rota Expressa no Leste dos EUA antes de percorrer o continente inteiro de volta à Costa Oeste. A causa do problema é semelhante à do primeiro caso. Sem qualquer dica, a rede da Microsoft não pode determinar qual prefixo do cliente está próximo ao Leste dos EUA e qual está próximo ao Oeste dos EUA. Ela acaba escolhendo o caminho errado para seu escritório em Los Angeles.

![](./media/expressroute-optimize-routing/expressroute-case2-problem.png)

### <a name="solution-use-as-path-prepending"></a>Solução: usar precedência de caminho AS
Há duas soluções para o problema. A primeira é simplesmente anunciar o prefixo local para o escritório de Los Angeles, 177.2.0.0/31 no circuito de Rota Expressa no Oeste dos EUA e o prefixo local para o escritório de Nova York, 177.2.0.2/31, no circuito de Rota Expressa no Leste dos EUA. Como resultado, sobra apenas um caminho para a Microsoft se conectar a cada um dos seus escritórios. Não há nenhuma ambiguidade e o roteamento é otimizado. Com esse design, você precisa pensar sobre a estratégia de failover. Se o caminho para a Microsoft pela Rota Expressa for interrompido, você precisa fazer com que o Exchange Online ainda possa se conectar aos servidores locais. 

A segunda solução é continuar a anunciar ambos os prefixos em ambos os circuitos da Rota Expressa e, além disso, fornecer uma dica de qual prefixo é mais próximo de qual escritório. Como damos suporte a precedência de caminho AS do BGP, você pode configurar do caminho AS para o prefixo a fim de influenciar o roteamento. Neste exemplo, você pode aumentar o caminho AS de 172.2.0.0/31 no Leste dos EUA para que passemos a preferir circuito da Rota Expressa no Oeste dos EUA para o tráfego destinado a esse prefixo (já que a nossa rede pensará que o caminho para esse prefixo é mais curto nesse lado). Da mesma forma, você pode aumentar o caminho AS de 172.2.0.2/31 no Oeste dos EUA para que vejamos o circuito da Rota Expressa no Leste dos EUA como preferencial. O roteamento é otimizado para ambos os escritórios. Com esse design, se um circuito da Rota Expressa for interrompido, o Exchange Online poderá ainda acessá-lo por meio de outro circuito da Rota Expressa e sua WAN. 

> [!IMPORTANT]
> Removemos números AS particulares no caminho AS para os prefixos recebidos no Microsoft Peering. Você precisa anexar números AS públicos como números no caminho AS para influenciar o roteamento para o Microsoft Peering.
> 
> 

![](./media/expressroute-optimize-routing/expressroute-case2-solution.png)

> [!IMPORTANT]
> Embora os exemplos fornecidos aqui sejam para a Microsoft e os emparelhamentos Públicos, oferecemos suporte para os mesmos recursos do emparelhamento Privado. Além disso, o prefixo AS Path funciona em um único circuito do ExpressRoute para influenciar a seleção dos caminhos primário e secundário.
> 
> 




<!--HONumber=Nov16_HO2-->


