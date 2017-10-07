---
title: "aaaInternal belastningsutjämnaren översikt | Microsoft Docs"
description: "Översikt för intern belastningsutjämnare och dess funktioner. Så här fungerar en belastningsutjämnare för Azure och möjliga scenarier tooconfigure interna slutpunkter"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: tysonn
ms.assetid: 36065bfe-0ef1-46f9-a9e1-80b229105c85
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: 9a901aad224d8821c154e130e142699d57282b25
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="internal-load-balancer-overview"></a>Översikt över interna belastningsutjämnare

Till skillnad från hello Internet belastningsutjämnare dirigerar hello intern belastningsutjämnare (ILB) trafik endast tooresources i hello molnbaserad tjänst eller med hjälp av VPN-tooaccess hello Azure-infrastrukturen. hello infrastruktur begränsar åtkomst toohello belastningsutjämnade virtuella IP-adresser (VIP) för en molnbaserad tjänst eller ett virtuellt nätverk så att de blir aldrig direkt exponerade tooan Internet slutpunkt. Det interna branschspecifika toorun för business (LOB)-program i Azure och nås från hello molntjänster eller från resurser på lokalt.

## <a name="why-you-may-need-an-internal-load-balancer"></a>Varför du kanske behöver en intern belastningsutjämnare

Azure interna läsa in belastningsutjämning (ILB) tillhandahåller belastningsutjämning mellan virtuella datorer som finns inuti en molnbaserad tjänst eller ett virtuellt nätverk med ett regionalt omfång. Information om hello användning och konfiguration av virtuellt nätverk med ett regionalt omfång finns [regionala virtuella nätverk](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/) i hello Azure blogg. Befintliga virtuella nätverk som har konfigurerats för en tillhörighetsgrupp kan inte använda ILB.

ILB kan hello följande typer av nätverksbelastning:

* I en molnbaserad tjänst från virtuella datorer tooa uppsättning virtuella datorer som finns på hello samma molntjänst (se bild 1).
* Ett virtuellt nätverk från virtuella datorer i hello virtuellt nätverk tooa uppsättning virtuella datorer som finns på hello samma molntjänst för hello virtuella nätverk (se bild 2).
* För ett virtuellt nätverk mellan platser, från lokala datorer tooa uppsättning virtuella datorer som finns på hello samma molntjänst för hello virtuella nätverk (se bild 3).
* Internet-riktade och flera nivåer program där hello backend-nivåer är inte mot Internet men kräver belastningsutjämning för trafik från hello mot Internet-nivå.
* Belastningsutjämning för LOB-program som finns i Azure utan ytterligare belastningen belastningsutjämnaren maskinvara eller programvara. Inklusive lokala servrar i hello uppsättning datorer vars trafik är belastningen balanserade.

## <a name="internet-facing-multi-tier-applications"></a>Flera nivåer program mot Internet

Hej webbnivå har internetuppkopplade slutpunkter för Internet-klienter och är en del av en belastningsutjämnad uppsättning. hello belastningsutjämnare distribuerar inkommande trafik från webbklienter för TCP-port 443 (HTTPS) toohello webbservrar.

hello databasservrar finns bakom en ILB-slutpunkt som hello webbservrar använder för lagring. Den här databasen service belastningsutjämnade slutpunkt, vilken trafik är belastningsutjämnas mellan hello databasservrar i hello ILB uppsättningen.

Följande bild visar hello hello Internetuppkopplad flernivåapp inom hello samma molntjänst.

![Intern belastningsutjämning enda Molntjänsten](./media/load-balancer-internal-overview/IC736321.png)

Bild 1 - flernivåapp mot Internet

Kan också använda en flernivåapp är när hello ILB distribuerade tooa annan molntjänst än hello en lång hello-tjänst för hello ILB.

Cloud services med hello samma virtuella nätverk har åtkomst till toohello ILB-slutpunkten. hello följande bild visar frontend-webbservrar som i en annan molntjänst från hello-databas och använder hello ILB-slutpunkten inom hello samma virtuella nätverk.

![Intern belastningsutjämning mellan molntjänster](./media/load-balancer-internal-overview/IC744147.png)

Bild 2 - servrar i en annan molntjänst

## <a name="intranet-line-of-business-applications"></a>Intranät verksamhetsspecifika program

Trafik från klienter på hello lokalt nätverk hämta belastningsutjämnade över hello uppsättning LOB-servrar med hjälp av VPN-anslutning tooAzure nätverk.

hello klientdatorn ska ha åtkomst tooan IP-adress från Azure VPN-tjänsten med hjälp av VPN för punkt toosite. Det gör hello Använd hello LOB-program som finns bakom hello ILB-slutpunkten.

![Intern belastningsutjämning med hjälp av VPN för punkt toosite](./media/load-balancer-internal-overview/IC744148.png)

Bild 3 - LOB-program som finns bakom hello LB-slutpunkt

Ett annat scenario för hello LOB är toohave toosite VPN toohello virtuella nätverk på en plats där hello ILB-slutpunkten är konfigurerad. Detta gör att lokalt nätverk trafik dirigeras toobe toohello ILB-slutpunkten.

![Intern belastningsutjämning med hjälp av webbplatsen toosite VPN](./media/load-balancer-internal-overview/IC744150.png)

Bild 4 - lokal trafik dirigeras toohello ILB-slutpunkten

## <a name="limitations"></a>Begränsningar

Den interna belastningsutjämnaren konfigurationer stöder inte SNAT. Hello gäller det här dokumentet är refererar SNAT tooport imiterade källa nätverksadresser.  Detta gäller tooscenarios där en virtuell dator i en pool för belastningsutjämnare belastningen måste tooreach hello respektive interna Belastningsutjämnares klientdelens IP-adress. Det här scenariot stöds inte för intern belastningsutjämnare. Anslutningsfel inträffar när hello flödet är belastningsutjämnad toohello VM ursprung hello flödet. Du måste använda en belastningsutjämnare för proxy-format för sådana scenarier.

## <a name="next-steps"></a>Nästa steg

[Azure Resource Manager-stöd för Azure belastningsutjämnare](load-balancer-arm.md)

[Kom igång med att konfigurera en Internetuppkopplad belastningsutjämnare](load-balancer-get-started-internet-arm-ps.md)

[Kom igång med att konfigurera en intern belastningsutjämnare](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurera ett distributionsläge för belastningsutjämnare](load-balancer-distribution-mode.md)

[Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare](load-balancer-tcp-idle-timeout.md)
