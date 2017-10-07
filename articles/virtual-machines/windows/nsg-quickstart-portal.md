---
title: "aaaOpen portar tooa VM med hjälp av hello Azure-portalen | Microsoft Docs"
description: "Lär dig hur tooopen en port / skapa en slutpunkt tooyour Windows VM med hello resource manager-distributionsmodellen i hello Azure-portalen"
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: f7cf0319-5ee7-435e-8f94-c484bf5ee6f1
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 08/21/2017
ms.author: iainfou
ms.openlocfilehash: aba789c65254651899aa688f256fe616c3d0126d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooopen-ports-tooa-virtual-machine-with-hello-azure-portal"></a>Hur tooopen portarna tooa virtuell dator med hello Azure-portalen
[!INCLUDE [virtual-machines-common-nsg-quickstart](../../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a>Snabbkommandon
Du kan också [utför dessa steg med hjälp av Azure PowerShell](nsg-quickstart-powershell.md).

Först skapa en säkerhetsgrupp för ditt nätverk. Välj en resursgrupp i hello portal, Välj **Lägg till**, och Sök efter och välj **nätverkssäkerhetsgruppen**:

![Lägg till en Nätverkssäkerhetsgrupp](./media/nsg-quickstart-portal/add-nsg.png)

Ange ett namn för din Nätverkssäkerhetsgruppen, Välj eller skapa en resursgrupp och välja en plats. Välj **skapa** när du är klar:

![Skapa en säkerhetsgrupp för nätverk](./media/nsg-quickstart-portal/create-nsg.png)

Välj en ny Nätverkssäkerhetsgrupp. Välj 'Inkommande säkerhetsregler ”och sedan hello **Lägg till** knappen toocreate en regel:

![Lägg till en inkommande regel](./media/nsg-quickstart-portal/add-inbound-rule.png)

Välj en gemensam **Service** hello nedrullningsbara menyn som *HTTP*. Du kan också välja *anpassad* tooprovide toouse för en viss port. Om du vill ändra hello prioritet eller namn. hello prioritet påverkar hello ordning i vilken regler - hello lägre hello numeriskt värde, hello tidigare hello-regel används. Du kan också välja **Avancerat** hello överst i den här skärmen tooenter en specifik datakälla IP-block eller port intervall, till exempel. När du är klar väljer du **OK** toocreate hello regel:

![Skapa en regel för inkommande trafik](./media/nsg-quickstart-portal/create-inbound-rule.png)

Det sista steget är tooassociate nätverkssäkerheten gruppen med ett undernät eller ett visst nätverksgränssnitt. Vi associera hello säkerhetsgrupp för nätverk med ett undernät. Välj **undernät**, Välj **associera**:

![Associera en Nätverkssäkerhetsgrupp med ett undernät](./media/nsg-quickstart-portal/associate-subnet.png)

Välj det virtuella nätverket och välj sedan hello rätt undernät:

![Associera en Nätverkssäkerhetsgrupp med virtuella nätverk](./media/nsg-quickstart-portal/select-vnet-subnet.png)

Nu har du skapat en Nätverkssäkerhetsgrupp skapas en regel för inkommande trafik som tillåter trafik på port 80 och som är associerade med ett undernät. Alla virtuella datorer som du ansluter toothat undernät kan nås på port 80.

## <a name="more-information-on-network-security-groups"></a>Mer information om Nätverkssäkerhetsgrupper
hello här snabb kommandon kan du tooget upp och körs med trafiken flödar tooyour VM. Nätverkssäkerhetsgrupper ger många bra funktioner och granularitet för styra åtkomst tooyour resurser. Du kan läsa mer om [skapar en säkerhetsgrupp för nätverk och ACL-regler här](../../virtual-network/virtual-networks-create-nsg-arm-ps.md).

Du bör placera dina virtuella datorer bakom en belastningsutjämnare i Azure för hög tillgänglighet webbprogram. hello belastningsutjämnare distribuerar trafik tooVMs med en Nätverkssäkerhetsgrupp som tillhandahåller trafikfiltrering. Mer information finns i [hur tooload saldo Linux virtuella datorer i Azure toocreate högtillgänglig programmet](tutorial-load-balancer.md).

## <a name="next-steps"></a>Nästa steg
I det här exemplet skapas en enkel regel tooallow HTTP-trafik. Du kan hitta information om hur du skapar mer detaljerad miljöer i hello följande artiklar:

* [Översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md)
* [Vad är en nätverkssäkerhetsgrupp (NSG)?](../../virtual-network/virtual-networks-nsg.md)