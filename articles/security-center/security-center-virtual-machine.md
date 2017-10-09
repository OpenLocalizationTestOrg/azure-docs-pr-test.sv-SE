---
title: aaaAzure Security Center och Azure Virtual Machines | Microsoft Docs
description: "Det här dokumentet hjälper dig att toounderstand hur du Azure Security Center kan skydda virtuella datorer i Azure."
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 5fe5a12c-5d25-430c-9d47-df9438b1d7c5
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/24/2017
ms.author: yurid
ms.openlocfilehash: d5e80e9341263a57f3100cb032a066f037e913a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-center-and-azure-virtual-machines"></a>Azure Security Center och Azure Virtual Machines
[Azure Security Center](https://azure.microsoft.com/services/security-center/) hjälper dig att förebygga, upptäcka och åtgärda toothreats. Härifrån kan du övervaka och hantera principer för alla Azureprenumerationer på en gång och upptäcka hot som annars kanske skulle förbli oupptäckta. Azure Security Center fungerar tillsammans med ett vittomfattande ekosystem med säkerhetslösningar.

Den här artikeln visar hur Security Center kan hjälpa dig att skydda dina virtuella Azure-datorer (VM).

## <a name="why-use-security-center"></a>Varför ska man använda Security Center?
Security Center hjälper dig att skydda data i virtuella datorer i Azure genom att ge bättre inblick i den virtuella datorns säkerhetsinställningar. När Security Center skyddar dina virtuella datorer, blir hello följande funktioner tillgängliga:

* Operativsystem (OS) säkerhetsinställningar med hello rekommenderas konfigurationsregler
* Systemsäkerhet och viktiga uppdateringar som saknas
* Rekommendationer för slutpunktsskydd
* Verifiering av diskkryptering
* Sårbarhetsbedömning och åtgärder
* Hotidentifiering

Dessutom toohelping skydda din virtuella Azure-datorer, Security Center innehåller också säkerhetsövervakning och hantering för molntjänster, Apptjänster, virtuella nätverk med mera. 

> [!NOTE]
> Se [introduktion tooAzure Security Center](security-center-intro.md) toolearn mer om Azure Security Center.
> 
> 

## <a name="prerequisites"></a>Krav
tooget igång med Azure Security Center du behöver tooknow och Tänk hello följande:

* Du måste ha en prenumeration tooMicrosoft Azure. Läs [Security Center Pricing](https://azure.microsoft.com/pricing/details/security-center/) (Priser för Security Center) för att få mer information om kostnadsfria nivåer och standardnivåer i Security Center.
* Planera Security Center-införande, se [Azure Security Center planerings- och operations guide](security-center-planning-and-operations-guide.md) toolearn mer om överväganden för planering och åtgärder.
* Information om stöd för operativsystem finns bland våra [vanliga frågor och svar om Azure Security Center](security-center-faq.md). 

## <a name="set-security-policy"></a>Ange säkerhetsprincip
Data collection behov toobe aktiverats så att Azure Security Center kan samla in hello information som behövs för tooprovide rekommendationer och aviseringar som genereras baserat på hello säkerhetsprincip som du konfigurerar. I hello bilden nedan kan du se att **datainsamling** har stängts **på**.

En säkerhetsprincip definierar hello kontrollfunktioner som rekommenderas för resurser inom hello angivna prenumerationen eller resursen gruppen. Innan du aktiverar säkerhetsprinciper, måste du ha aktiverat datainsamling, Security Center samlar in data från de virtuella datorerna i ordning tooassess sina säkerhetstillstånd ange säkerhetsrekommendationer och varnar toothreats. I Security Center kan du definiera principer för dina Azure-prenumerationer eller resursgrupper enligt tooyour företagets säkerhetskrav och hello typ av program eller känslighet hello data i varje prenumeration. 

![Säkerhetsprincip](./media/security-center-virtual-machine/security-center-virtual-machine-fig1.png)

> [!NOTE]
> Mer om varje toolearn **skyddsprincip** tillgängliga, se [ställa in säkerhetsprinciper](security-center-policies.md) artikel.
> 
> 

## <a name="manage-security-recommendations"></a>Hantera säkerhetsrekommendationer
Security Center analyserar hello säkerhetstillståndet hos dina Azure-resurser. När Security Center identifierar potentiella säkerhetsproblem skapas rekommendationer. hello rekommendationer leder dig igenom hello konfigureringen av hello behövs kontroller.

När du har angett en säkerhetsprincip för analyserar Security Center hello säkerhetsstatus för dina resurser tooidentify potentiella säkerhetsrisker. hello rekommendationer som visas i tabellformat där varje rad motsvarar en viss rekommendation. hello tabellen nedan innehåller några exempel på rekommendationer för virtuella Azure-datorer och vad varje göra om du använder den. När du väljer en rekommendation finns information som visar hur tooimplement hello rekommendation i Security Center.

| Rekommendation | Beskrivning |
| --- | --- |
| [Aktivera insamling av data för prenumerationer](security-center-enable-data-collection.md) |Rekommenderar att du aktiverar datainsamling i hello säkerhetsprincipen för var och en av dina prenumerationer och alla virtuella maskiner (VMs) i dina prenumerationer. |
| [Åtgärda sårbarheter i operativsystem](security-center-remediate-os-vulnerabilities.md) |Rekommenderar att du justerar OS-konfigurationer med hello rekommenderas konfigurationsregler, tillåter t.ex. inte att lösenord toobe sparas. |
| [Tillämpa systemuppdateringar](security-center-apply-system-updates.md) |Rekommenderar att du distribuerar saknas systemets säkerhet och viktiga uppdateringar tooVMs. |
| [Starta om datorn efter uppdateringarna](security-center-apply-system-updates.md#reboot-after-system-updates) |Rekommenderar att du startar om en VM toocomplete hello process för tillämpning av uppdateringar. |
| [Installera slutpunktsskydd](security-center-install-endpoint-protection.md) |Rekommenderar att du etablerar tooVMs för program mot skadlig kod program (endast Windows VM). |
| [Lös slutpunktsskydd för hälsovarningar](security-center-resolve-endpoint-protection-health-alerts.md) |Rekommenderar att du löser fel med slutpunktsskydd. |
| [Aktivera VM-Agent](security-center-enable-vm-agent.md) |Aktiverar du toosee som virtuella datorer kräver hello VM-agenten. hello VM-agenten måste installeras på virtuella datorer i ordning tooprovision korrigering genomsökning baslinjen genomsökning och program mot skadlig kod. hello VM-agenten installeras som standard för virtuella datorer som distribueras från hello Azure Marketplace. hello artikel [VM-Agent och tillägg – del 2](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) innehåller information om hur tooinstall hello VM-agenten. |
| [Tillämpa diskkryptering](security-center-apply-disk-encryption.md) |Rekommenderar att krypterar dina VM-diskar med Azure Disk Encryption (virtuella Windows- och Linux-datorer). Kryptering rekommenderas för både hello Operativsystemet och datavolymer på den virtuella datorn. |
| [Sårbarhetsbedömning inte installerad](security-center-vulnerability-assessment-recommendations.md) |Rekommenderar att du installerar en lösning för sårbarhetsbedömning på den virtuella datorn. |
| [Åtgärda sårbarheter](security-center-vulnerability-assessment-recommendations.md#review-the-recommendation) |Låter dig toosee system- och sårbarheter som identifieras av hello vulnerability assessment redan är installerad på den virtuella datorn. |

> [!NOTE]
> toolearn mer om rekommendationer finns [hantera säkerhetsrekommendationer](security-center-recommendations.md) artikel.
> 
> 

## <a name="monitor-security-health"></a>Övervaka säkerhetshälsa
När du har aktiverat [säkerhetsprinciper](security-center-policies.md) för en prenumeration resurser, Security Center analyserar hello säkerheten för dina resurser tooidentify potentiella säkerhetsproblem.  Du kan visa hello säkerhetsstatus för dina resurser, tillsammans med eventuella problem i hello **resurssäkerhetshälsa** bladet. När du klickar på **virtuella datorer** i hello **säkerhet för** hälsan panel, hello **virtuella datorer** öppnas bladet rekommendationer för dina virtuella datorer. 

![Säkerhetshälsa](./media/security-center-virtual-machine/security-center-virtual-machine-fig2.png)

## <a name="manage-and-respond-toosecurity-alerts"></a>Hanterar och åtgärdar toosecurity aviseringar
Security Center automatiskt samlar in, analyseras och integreras loggdata från din Azure-resurser och hello nätverk anslutna partnerlösningar (till exempel brandväggen och endpoint protection lösningar), toodetect verkliga hot och falsklarm. Genom att använda en rad olika aggregering av [identifieringsfunktionerna](security-center-detection-capabilities.md), Security Center är kan toogenerate prioriteras säkerhetsvarningar toohelp snabbt undersöka hello problem och ger rekommendationer för hur tooremediate möjliga attacker.

![Säkerhetsaviseringar](./media/security-center-virtual-machine/security-center-virtual-machine-fig3.png)

Välj en avisering toolearn säkerhet mer om hello var som utlöste hello avisering och vad, om sådant finns, hur du behöver tootake tooremediate angreppet. Säkerhetsaviseringarna är indelade i grupper efter [typ](security-center-alerts-type.md) och datum.

## <a name="see-also"></a>Se även
toolearn mer om Security Center finns hello följande:

* [Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Lär dig hur tooconfigure säkerhetsprinciper för dina Azure-prenumerationer och resursgrupper.
* [Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Lär dig hur toomanage och svara toosecurity aviseringar.
* [Vanliga frågor om Azure Security Center](security-center-faq.md) --finns vanliga frågor om hur du använder hello-tjänsten.

