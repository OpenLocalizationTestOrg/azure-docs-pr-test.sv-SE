---
title: "aaaCreate en Skaluppsättning för virtuell dator med hjälp av hello Azure-portalen | Microsoft Docs"
description: "Distribuera skaluppsättningar med hjälp av Azure portal."
keywords: "Skaluppsättningar för den virtuella datorn"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: madhana
editor: tysonn
tags: azure-resource-manager
ms.assetid: 9c1583f0-bcc7-4b51-9d64-84da76de1fda
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: negat
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 23c88f4b1ba99994a38f8886f60735da74e5c17e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-virtual-machine-scale-set-with-hello-azure-portal"></a>Hur toocreate en Skaluppsättning för virtuell dator med hello Azure-portalen
Den här kursen visar hur lätt det är toocreate Skalningsuppsättning en virtuell dator på bara några minuter med hjälp av hello Azure-portalen. Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/) innan du börjar.

## <a name="choose-hello-vm-image-from-hello-marketplace"></a>Välj hello VM-avbildning från marketplace hello
Du kan enkelt distribuera en skala som anges med CentOS, virtuell CoreOS, Debian, öppna Suse, Red Hat Enterprise Linux, SUSE Linux Enterprise Server, Ubuntu Server eller Windows Server-avbildningar från hello-portalen.

Först gå toohello [Azure-portalen](https://portal.azure.com) i en webbläsare. Klicka på `New`, söka efter `scale set`, och välj sedan hello `Virtual machine scale set` post:

![ScaleSetPortalOverview](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalOverview.PNG)

## <a name="create-hello-scale-set"></a>Skapa hello skaluppsättning
Nu kan du använda hello standardinställningarna och snabbt skapa hello skaluppsättning.

* På hello `Basics` bladet, ange ett namn för hello skaluppsättning. Det här namnet blir grundläggande hello av hello FQDN för hello belastningsutjämnaren framför hello skaluppsättning, så kontrollera hello namn är unikt över alla Azure.
* Välj din önskade OS skriver, ange ditt önskade användarnamn och välj vilken autentiseringsmetod skriver du föredrar. Om du väljer ett lösenord, det måste vara minst 12 tecken långa och uppfylla tre utanför hello fyra följande krav på komplexitet: en gemen, en versal, en siffra och ett specialtecken. Läs mer om [krav för användarnamn och lösenord](../virtual-machines/windows/faq.md#what-are-the-username-requirements-when-creating-a-vm). Om du väljer `SSH public key`, vara säker på att tooonly klistra in i den offentliga nyckeln inte din privata nyckel:

![ScaleSetPortalBasics](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalBasics.PNG)

* Välj om du skulle som toolimit hello skaluppsättning tooa enda placering grupp eller om den ska omfatta flera grupper för placering. Att tillåta hello skaluppsättning toospan placering grupper ger för skala anger över 100 virtuella datorer med vissa begränsningar i kapaciteten (upp too1 000). Mer information finns i [denna dokumentation](./virtual-machine-scale-sets-placement-groups.md).
* Ange önskade resursgruppens namn och plats och klicka sedan på `OK`.
* På hello `Virtual machine scale set service settings` bladet: Ange den önskade domännamnet (hello FQDN för hello belastningsutjämnaren framför hello skaluppsättning hello base). Den här etiketten måste vara unikt inom alla Azure.
* Välj önskat operativsystem diskavbildning, instansantal och storleken på datorn.
* Välj önskad disk-typ: hanterad eller ohanterad. Mer information finns i [denna dokumentation](./virtual-machine-scale-sets-managed-disks.md). Om du har valt toohave hello skaluppsättning sträcker sig över flera placering grupper, det här alternativet är inte tillgänglig eftersom hanterade diskar krävs för att skala anger toospan placering grupper.
* Aktivera eller inaktivera Autoskala och konfigurera om aktiverat:

![ScaleSetPortalService](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalService.PNG)

* På hello `Summary` bladet när verifieringen är klar klickar du på `OK` toostart hello skaluppsättning distribution.


## <a name="connect-tooa-vm-in-hello-scale-set"></a>Ansluta tooa VM i hello skaluppsättning
Om du har valt toolimit nivå inställt tooa enda placering grupp och hello skaluppsättning distribueras med NAT regler har konfigurerat toolet du ansluta toohello skaluppsättningen enkelt (om inte, tooconnect toohello virtuella datorer i hello skala har angetts måste du förmodligen toocreate en jumpbox i hello samma virtuella nätverk som hello skaluppsättning). toosee dem, navigera toohello `Inbound NAT Rules` fliken av hello belastningsutjämnare för hello skaluppsättning för:

![ScaleSetPortalNatRules](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalNatRules.PNG)

Du kan ansluta tooeach VM i hello skala anges med dessa NAT-regler. Till exempel för en skaluppsättning för Windows om det finns en NAT-regel för inkommande port 50000, du kan ansluta toothat datorn via RDP på `<load-balancer-ip-address>:50000`. För en Linux-skaluppsättning kan du ansluta hello kommandot `ssh -p 50000 <username>@<load-balancer-ip-address>`.

## <a name="next-steps"></a>Nästa steg
Dokumentation om hur toodeploy skalningsuppsättningarna från hello CLI finns [denna dokumentation](virtual-machine-scale-sets-cli-quick-create.md).

Dokumentation om hur toodeploy skalningsuppsättningarna från PowerShell finns [denna dokumentation](virtual-machine-scale-sets-windows-create.md).

Dokumentation om hur toodeploy skalningsuppsättningarna från Visual Studio finns [denna dokumentation](virtual-machine-scale-sets-vs-create.md).

För allmän dokumentation kolla hello [dokumentationen översiktssidan för skalningsuppsättningar](virtual-machine-scale-sets-overview.md).

Allmän information kolla hello [huvudsakliga Landningssida för skalningsuppsättningar](https://azure.microsoft.com/services/virtual-machine-scale-sets/).

