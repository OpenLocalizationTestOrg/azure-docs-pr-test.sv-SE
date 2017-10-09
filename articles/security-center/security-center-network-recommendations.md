---
title: "aaaProtecting nätverket i Azure Security Center | Microsoft Docs"
description: "Det här dokumentet behandlar rekommendationerna i Azure Security Center som hjälper dig skydda dina Azure-nätverk och uppfyller säkerhetsprinciper."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 96c55a02-afd6-478b-9c1f-039528f3dea0
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/16/2016
ms.author: terrylan
ms.openlocfilehash: 053738da432edf13b40172fb44d2044702dd8211
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="protecting-your-network-in-azure-security-center"></a>Skydda ditt nätverk i Azure Security Center
Azure Security Center analyserar hello säkerhetstillståndet hos dina Azure-resurser. När Security Center identifierar potentiella säkerhetsrisker, skapar rekommendationer som guidar dig igenom hello konfigureringen av hello behövs kontroller.  Rekommendationer gäller tooAzure resurstyper: virtuella datorer (VM), nätverk, SQL och program.

Den här artikeln tar rekommendationer som gäller tooyour nätverk.  Rekommendationer Nätverkscenter runt nästa generation brandväggar, Nätverkssäkerhetsgrupper, konfigurera regler för inkommande trafik och mer.  Använd hello tabellen nedan som en referens toohelp du förstå hello tillgängliga nätverksrekommendationer och det var och en gör om du använder den.

## <a name="available-network-recommendations"></a>Tillgängliga nätverksrekommendationer
| Rekommendation | Beskrivning |
| --- | --- |
| [Lägga till en nästa generations brandvägg](security-center-add-next-generation-firewall.md) |Rekommenderar att du lägger till en nästa generations Brandvägg från en Microsoft-partner tooincrease din säkerhetsskydd. |
| [Dirigera trafiken endast via NGFW](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only) |Rekommenderar att du konfigurerar (NSG) regler för nätverkssäkerhetsgrupper som tvingar inkommande trafik tooyour VM via din nästa generations Brandvägg. |
| [Aktivera Nätverkssäkerhetsgrupper för undernät eller virtuella datorer](security-center-enable-network-security-groups.md) |Rekommenderar att du aktiverar NSG: er på undernät eller virtuella datorer. |
| [Begränsa åtkomst via Internetuppkopplad slutpunkt](security-center-restrict-access-through-internet-facing-endpoints.md) |Rekommenderar att du konfigurerar regler för inkommande trafik för NSG: er. |

## <a name="see-also"></a>Se även
toolearn mer om rekommendationer som gäller tooother Azure resurstyper finns hello följande:

* [Skydda dina virtuella datorer i Azure Security Center](security-center-virtual-machine-recommendations.md)
* [Skydda dina program i Azure Security Center](security-center-application-recommendations.md)
* [Skydda din SQL Azure-tjänst i Azure Security Center](security-center-sql-service-recommendations.md)

toolearn mer om Security Center finns hello följande:

* [Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Lär dig hur tooconfigure säkerhetsprinciper för dina Azure-prenumerationer och resursgrupper.
* [Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Lär dig hur toomanage och svara toosecurity aviseringar.
* [Vanliga frågor om Azure Security Center](security-center-faq.md) --finns vanliga frågor om hur du använder hello-tjänsten.
