---
title: aaaProtecting dina program i Azure Security Center | Microsoft Docs
description: "Det här dokumentet behandlar rekommendationerna i Azure Security Center som hjälper dig skydda dina Azure-program och uppfyller säkerhetsprinciper."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: b5fc7a9e-24b1-415f-b3b5-62a53f5dd424
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/01/2016
ms.author: terrylan
ms.openlocfilehash: da5e02cc2bad55c64e4da14e4e10efd6ddeab39e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="protecting-your-applications-in-azure-security-center"></a>Skydda dina program i Azure Security Center
Azure Security Center analyserar hello säkerhetstillståndet hos dina Azure-resurser. När Security Center identifierar potentiella säkerhetsrisker, skapar rekommendationer som guidar dig igenom hello konfigureringen av hello behövs kontroller.  Rekommendationer gäller tooAzure resurstyper: virtuella datorer (VM), nätverk, SQL och program.

Den här artikeln tar rekommendationer som gäller tooapplications.  Programmet rekommendationer center runt distribution av en brandvägg för webbaserade program.  Använd hello tabellen nedan som en referens toohelp du förstå hello tillgängliga rekommendationer och det var och en gör om du använder den.

## <a name="available-application-recommendations"></a>Rekommendationer för tillgängliga program
| Rekommendation | Beskrivning |
| --- | --- |
| [Lägga till en brandvägg för webbappar](security-center-add-web-application-firewall.md) |Rekommenderar att du distribuerar en brandvägg för webbaserade program (Brandvägg) för web-slutpunkter. En Brandvägg rekommendation visas för alla offentliga Internetriktade IP-adresser (instans nivå IP eller Load belastningsutjämnade IP) som har en nätverkssäkerhetsgrupp med öppna webbplats för inkommande portar (80,443).</br></br>Security Center rekommenderar att etablera en Brandvägg toohelp skydd mot angrepp målobjekt för webbaserade program på virtuella datorer samt på Apptjänst-miljö. En App Service miljö (ASE) är en [Premium](https://azure.microsoft.com/pricing/details/app-service/) service plan alternativet för Azure App Service som tillhandahåller en helt isolerad och dedikerad miljö för Azure App Service-program som körs på ett säkert sätt. toolearn mer om ASE, se hello [dokumentationen till App Service-miljö](../app-service/app-service-app-service-environments-readme.md).</br></br>Du kan skydda flera webbprogram i Security Center genom att lägga till dessa program tooyour befintliga Brandvägg-distributioner. |
| [Slutför programskydd](security-center-add-web-application-firewall.md#finalize-application-protection) |toocomplete hello konfigurationen av en Brandvägg, trafik måste vara omdirigerade toohello Brandvägg. Efter den här rekommendationen är klar hello nödvändiga ändringar. |

## <a name="see-also"></a>Se även
toolearn mer om rekommendationer som gäller tooother Azure resurstyper finns hello följande:

* [Skydda dina virtuella datorer i Azure Security Center](security-center-virtual-machine-recommendations.md)
* [Skydda ditt nätverk i Azure Security Center](security-center-network-recommendations.md)
* [Skydda din SQL Azure-tjänst i Azure Security Center](security-center-sql-service-recommendations.md)

toolearn mer om Security Center finns hello följande:

* [Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Lär dig hur tooconfigure säkerhetsprinciper för dina Azure-prenumerationer och resursgrupper.
* [Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Lär dig hur toomanage och svara toosecurity aviseringar.
* [Vanliga frågor om Azure Security Center](security-center-faq.md) --finns vanliga frågor om hur du använder hello-tjänsten.
