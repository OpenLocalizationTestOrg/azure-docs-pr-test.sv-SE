---
title: "aaaInternet mot belastningen belastningsutjämnaren översikt | Microsoft Docs"
description: "Översikt för Internet facing belastningsutjämnare och dess funktioner. Hur fungerar en belastningsutjämnare för Azure med hjälp av virtuella datorer och molntjänster."
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: tysonn
ms.assetid: 529b37aa-a45c-41d1-8877-fee8cc1fa375
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: 3514f945d69ec576ed256cdd01069491e3e43936
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="internet-facing-load-balancer-overview"></a>Internet Internetriktade belastningen översikt över belastningsutjämnare

Azure belastningsutjämnare mappar hello offentliga IP-adress och port antalet inkommande trafik toohello privata IP-adress och port antal hello virtuell dator och vice versa för hello svarstrafik från hello virtuell dator. Belastningsutjämningsregler tillåter toodistribute specifika typer av trafik mellan flera virtuella datorer eller tjänster. Sprida hello belastningen på begäran Internet-trafik över flera webbservrar eller webbroller.

Du kan definiera en offentlig slutpunkt för en molntjänst som innehåller instanser av webbroller eller arbetsroller i hello tjänstdefinitionsfilen (.csdef).

Hej *servicedefinition.csdef* filen innehåller hello slutpunktskonfiguration och när du har flera rollinstanser för distribution av en webb- eller arbetarroll rollen hello belastningsutjämnaren ska vara inställd för det.. hello sätt tooadd instanser tooyour molndistribution ändrar hello instanser på hello tjänstkonfigurationsfilen (.csfg).

hello visar följande bild en belastningsutjämnad slutpunkt för webbtrafik som delas mellan tre virtuella datorer för hello offentliga och privata TCP-port 80. Dessa tre virtuella datorer finns i en belastningsutjämnad uppsättning.

![exempel på offentliga belastningsutjämnare](./media/load-balancer-internet-overview/IC727496.png)

Bild 1 - belastningsutjämnade slutpunkt för webbtrafik

När Internet-klienter skickar förfrågningar för webbsidan toohello offentliga IP-adressen för hello-Molntjänsten på TCP-port 80, distribuerar hello Azure belastningsutjämnare hello begäranden mellan hello tre virtuella datorer i hello belastningsutjämnade uppsättningen. Mer information om belastningen belastningsutjämnaren algoritmer finns hello [belastningen belastningsutjämnaren översiktssidan](load-balancer-overview.md#load-balancer-features).

Som standard distribuerar Azure belastningsutjämnare nätverkstrafik jämnt mellan flera virtuella datorer. Du kan också konfigurera sessionen tillhörighet mer information finns i [belastningsdistributionsläget nätverksbelastningsutjämning](load-balancer-distribution-mode.md).

## <a name="next-steps"></a>Nästa steg

Lär dig mer om [intern belastningsutjämnare](load-balancer-internal-overview.md) toobetter förstå vilka belastningsutjämning är en passar bättre för din molndistribution.

Du kan också [komma igång med en Internetuppkopplad belastningsutjämnaren](load-balancer-get-started-internet-arm-ps.md) och konfigurera vilken typ av [distribution läge](load-balancer-distribution-mode.md) för en särskild belastningsutjämnare trafik nätverksproblem.

Om ditt program måste tookeep anslutningar alive för servrar bakom en belastningsutjämnare, du veta mer om [inaktiv TCP timeout-inställningar för en belastningsutjämnare](load-balancer-tcp-idle-timeout.md). Det hjälper toolearn om inaktiv anslutning beteende när du använder Azure belastningsutjämnare.
