---
title: "aaaRemediate OS-säkerhetsproblem i Azure Security Center | Microsoft Docs"
description: "Det här dokumentet beskrivs hur tooimplement hello Azure Security Center rekommendation ** reparera OS säkerhetsrisker **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 991d41f5-1d17-468d-a66d-83ec1308ab79
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: terrylan
ms.openlocfilehash: 704103f7fb15835943d74b665d2bd56cb5e0a36d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="remediate-os-vulnerabilities-in-azure-security-center"></a>Åtgärda OS-säkerhetsproblem i Azure Security Center
Azure Security Center analyserar dagligen virtuell dator (VM)-operativsystem (OS) för konfigurationer som kan göra hello VM mer sårbara tooattack och rekommenderar konfigurationsändringar tooaddress dessa problem. Security Center rekommenderar att du löser säkerhetsproblem när den Virtuella datorns Operativsystemets konfiguration inte matchar hello rekommenderas konfigurationsregler.

> [!NOTE]
> Mer information om hello specifika konfigurationer som övervakas finns hello [lista över rekommenderade konfigurationsregler](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).
>
>

## <a name="implement-hello-recommendation"></a>Implementera hello rekommendation

> [!NOTE]
> Det här dokumentet introducerar hello-tjänsten med hjälp av ett exempel på distribution.  Det här dokumentet är inte en stegvis guide.
>
>

1. I hello **rekommendationer** bladet väljer **reparera OS säkerhetsrisker**.
   ![Åtgärda sårbarheter i operativsystem][1]

    Hej **reparera OS säkerhetsrisker** blad öppnas och visar en lista över dina virtuella datorer med OS-konfigurationer som inte matchar hello rekommenderas konfigurationsregler.  För varje virtuell dator identifierar hello-bladet

   * **Det gick inte regler** --hello antal regler som hello Virtuella datorns Operativsystemets konfiguration har misslyckats.
   * **SENASTE GENOMSÖKNINGSTID** --hello datum och tid att senast genomsöktes hello VM OS-konfiguration.
   * **TILLSTÅND** --hello aktuell status för hello säkerhetsproblem:

     * Öppna: hello säkerhetsproblem har inte utförts än
     * Pågår: Hello säkerhetsproblem som tillämpas för närvarande, ingen åtgärd krävs av du
     * Matcha: hello säkerhetsproblem har redan lösts (när hello problemet har åtgärdats hello-posten är nedtonad)
   * **ALLVARLIGHETSGRAD** --alla sårbarheter ställs tooa allvarlighetsgraden låg, vilket innebär en säkerhetsrisk som bör åtgärdas men inte kräver omedelbara uppmärksamhet.

2. Välj en virtuell dator. Ett blad för den virtuella datorn öppnas och visar hello regler som har misslyckats.
   ![Konfigurationsregler som har misslyckats][2]

3. Välj en regel. I det här exemplet kan du markera **lösenord måste uppfylla krav på komplexitet**. Då öppnas ett blad som beskriver hello misslyckades regeln och hello effekt. Granska hello information och Överväg hur operativsystemkonfigurationerna tillämpas.
  ![Beskrivning för hello misslyckades regel][3]

  Security Center använder Common Configuration Enumeration (CCE) tooassign unika identifierare för konfigurationsregler. hello följande information finns på det här bladet:

  - NAMN – Namnet på regel
  - --CCE allvarlighetsgrad allvarlighetsgrad Kritisk, viktigt eller varning
  - CCIED--CCE Unik identifierare för hello regel
  - Beskrivning – Beskrivning av regeln
  - SÄKERHETSPROBLEM--Förklaring av säkerhetsproblem eller risk om regeln inte är installerat
  - PÅVERKAN--Påverkan på verksamheten när regeln har tillämpats
  - FÖRVÄNTAT värde--Värde förväntades när Security Center analyserar VM OS konfigurationen mot hello regel
  - --REGELN regeln åtgärden används av Security Center under analys av VM OS konfigurationen mot hello regel
  - FAKTISKT värde - Värde returnerades efter analys av VM OS konfigurationen mot hello regel
  - UTVÄRDERINGSRESULTAT av –-resultatet av analysen: skicka, misslyckas

## <a name="see-also"></a>Se även
Den här artikeln visar hur hello tooimplement Security Center rekommendation ”reparera OS säkerhetsproblem”. Du kan granska hello uppsättning konfigurationsregler [här](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). Security Center använder CCE (Common Configuration uppräkning) tooassign unika identifierare för konfigurationsregler. Besök hello [CCE](https://nvd.nist.gov/cce/index.cfm) webbplats för mer information.

toolearn mer om Security Center finns hello följande resurser:

* [Plattformar som stöds i Azure Security Center](security-center-os-coverage.md) -visar en lista över Windows och Linux virtuella datorer som stöds.
* [Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Lär dig hur tooconfigure säkerhetsprinciper för dina Azure-prenumerationer och resursgrupper.
* [Hantera säkerhetsrekommendationer i Azure Security Center](security-center-recommendations.md) – Lär dig rekommendationer för att skydda dina Azure-resurser.
* [Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md) – Lär dig hur toomonitor hello Azure-resursers hälsa.
* [Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Lär dig hur toomanage och svara toosecurity aviseringar.
* [Övervaka partnerlösningar med Azure Security Center](security-center-partner-solutions.md) – Lär dig hur toomonitor hello dina partnerlösningars hälsostatus.
* [Vanliga frågor om Azure Security Center](security-center-faq.md) -finns vanliga frågor om hur du använder hello-tjänsten.
* [Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) -hittar du blogginlägg om säkerhet och Azure kompatibilitet.

<!--Image references-->
[1]: ./media/security-center-remediate-os-vulnerabilities/recommendation.png
[2]:./media/security-center-remediate-os-vulnerabilities/vm-remediate-os-vulnerabilities.png
[3]: ./media/security-center-remediate-os-vulnerabilities/vulnerability-details.png
