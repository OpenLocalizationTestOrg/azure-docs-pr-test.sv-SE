---
title: Azure Security Center och Azure Virtual Machines | Microsoft Docs
description: "Det här dokumentet beskriver hur Azure Security Center kan skydda dina virtuella Azure-datorer."
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
ms.openlocfilehash: 48314788dbe4618f271f0235f106dbe15ef004b8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="azure-security-center-and-azure-virtual-machines"></a>Azure Security Center och Azure Virtual Machines
[Azure Security Center](https://azure.microsoft.com/services/security-center/) hjälper dig att förebygga, identifiera och reagera på hot. Härifrån kan du övervaka och hantera principer för alla Azureprenumerationer på en gång och upptäcka hot som annars kanske skulle förbli oupptäckta. Azure Security Center fungerar tillsammans med ett vittomfattande ekosystem med säkerhetslösningar.

Den här artikeln visar hur Security Center kan hjälpa dig att skydda dina virtuella Azure-datorer (VM).

## <a name="why-use-security-center"></a>Varför ska man använda Security Center?
Security Center hjälper dig att skydda data i virtuella datorer i Azure genom att ge bättre inblick i den virtuella datorns säkerhetsinställningar. När Security Center skyddar dina virtuella datorer blir följande funktioner tillgängliga:

* Säkerhetsinställningar för operativsystem (OS) med de rekommenderade konfigurationsreglerna
* Systemsäkerhet och viktiga uppdateringar som saknas
* Rekommendationer för slutpunktsskydd
* Verifiering av diskkryptering
* Sårbarhetsbedömning och åtgärder
* Hotidentifiering

Utöver att skydda virtuella datorer i Azure tillhandahåller även Security Center säkerhetsövervakning och hantering för Cloud Services, App Services, Virtual Networks med mera. 

> [!NOTE]
> I [Introduktion till Azure Security Center](security-center-intro.md) kan du läsa mer om Azure Security Center.
> 
> 

## <a name="prerequisites"></a>Krav
För att komma igång med Azure Security Center måste du veta och tänka på följande:

* Du måste ha en prenumeration på Microsoft Azure. Läs [Security Center Pricing](https://azure.microsoft.com/pricing/details/security-center/) (Priser för Security Center) för att få mer information om kostnadsfria nivåer och standardnivåer i Security Center.
* Planera införandet av Security Center och läs [Planerings- och bruksanvisning för Azure Security Center](security-center-planning-and-operations-guide.md). Här får du lära dig mer om vad du ska tänka på angående planering och åtgärder.
* Information om stöd för operativsystem finns bland våra [vanliga frågor och svar om Azure Security Center](security-center-faq.md). 

## <a name="set-security-policy"></a>Ange säkerhetsprincip
Insamling av data måste aktiveras så att Azure Security Center kan samla in information som behövs för att tillhandahålla rekommendationer och aviseringar som genereras utifrån den säkerhetsprincip du konfigurerar. På bilden nedan ser du att **Datainsamling** har satts **på**.

En säkerhetsprincip är ett antal kontrollfunktioner som rekommenderas för resurser inom en viss prenumeration eller resursgrupp. Innan du aktiverar säkerhetsprinciper måste du ha aktiverat datainsamling. Security Center samlar in data från dina virtuella datorer för att utvärdera deras säkerhetstillstånd, ange säkerhetsrekommendationer och varna för hot. I Security Center ställer du in principer för dina prenumerationer och resursgrupper i Azure enligt ditt företags behov och typ av program eller efter känslighetsnivån på datauppgifterna i respektive prenumeration. 

![Säkerhetsprincip](./media/security-center-virtual-machine/security-center-virtual-machine-fig1.png)

> [!NOTE]
> Mer information om varje tillgänglig **skyddsprincip** finns i artikeln [Ange säkerhetsprinciper](security-center-policies.md).
> 
> 

## <a name="manage-security-recommendations"></a>Hantera säkerhetsrekommendationer
Security Center analyserar säkerhetstillståndet hos dina Azure-resurser. När Security Center identifierar potentiella säkerhetsproblem skapas rekommendationer. Via rekommendationerna får du hjälp att ställa in de kontrollfunktioner som behövs.

När du har angett en säkerhetsprincip analyseras säkerhetstillståndet för resurserna i Azure i Security Center för upptäckt av eventuella säkerhetsrisker. Rekommendationerna visas i tabellformat där varje rad motsvarar en viss rekommendation. Tabellen nedan innehåller några exempel på rekommendationer för virtuella datorer i Azure och vad var och en gör om du använder den. När du väljer en rekommendation får du information om hur du implementerar rekommendationen i Security Center.

| Rekommendation | Beskrivning |
| --- | --- |
| [Aktivera insamling av data för prenumerationer](security-center-enable-data-collection.md) |Rekommenderar att du aktiverar datainsamling i säkerhetsprincipen för var och en av dina prenumerationer och alla virtuella datorer (VM) i dina prenumerationer. |
| [Åtgärda sårbarheter i operativsystem](security-center-remediate-os-vulnerabilities.md) |Rekommenderar att du konfigurerar operativsystem enligt rekommenderade konfigurationsregler, t.ex. att inte spara lösenord. |
| [Tillämpa systemuppdateringar](security-center-apply-system-updates.md) |Rekommenderar att du distribuerar systemsäkerhet och viktiga uppdateringar för virtuella datorer som saknas. |
| [Starta om datorn efter uppdateringarna](security-center-apply-system-updates.md#reboot-after-system-updates) |Rekommenderar att du startar om en virtuell dator för att slutföra processen med att tillämpa uppdateringar. |
| [Installera slutpunktsskydd](security-center-install-endpoint-protection.md) |Rekommenderar att du etablerar program mot skadlig kod för virtuella datorer (endast virtuella Windows-datorer). |
| [Lös slutpunktsskydd för hälsovarningar](security-center-resolve-endpoint-protection-health-alerts.md) |Rekommenderar att du löser fel med slutpunktsskydd. |
| [Aktivera VM-Agent](security-center-enable-vm-agent.md) |Du kan se vilka virtuella datorer som kräver VM-agenten. VM-agenten måste installeras på virtuella datorer för att etablera korrigeringsgenomsökning, baslinjegenomsökning och program mot skadlig kod. VM-agenten installeras som standard för virtuella datorer som distribueras från Azure Marketplace. Artikeln [VM Agent and Extensions – Part 2](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) (VM-agenter och tillägg – del 2) innehåller information om hur VM-agenten ska installeras. |
| [Tillämpa diskkryptering](security-center-apply-disk-encryption.md) |Rekommenderar att krypterar dina VM-diskar med Azure Disk Encryption (virtuella Windows- och Linux-datorer). Kryptering rekommenderas både för OS- och datavolymer på den virtuella datorn. |
| [Sårbarhetsbedömning inte installerad](security-center-vulnerability-assessment-recommendations.md) |Rekommenderar att du installerar en lösning för sårbarhetsbedömning på den virtuella datorn. |
| [Åtgärda sårbarheter](security-center-vulnerability-assessment-recommendations.md#review-the-recommendation) |Gör att du kan visa system- och säkerhetssårbarheter som identifieras av sårbarhetsbedömningen som är installerad på den virtuella datorn. |

> [!NOTE]
> Läs mer om rekommendationer i artikeln [Managing security recommendations](security-center-recommendations.md) (Hantera säkerhetsrekommendationer).
> 
> 

## <a name="monitor-security-health"></a>Övervaka säkerhetshälsa
När du har aktiverat [säkerhetsprinciper](security-center-policies.md) för resurser i en prenumeration analyseras resursernas säkerhet för upptäckt av eventuella säkerhetsrisker.  Du kan se säkerhetsstatus för dina resurser, samt om det finns några problem, i bladet **Resource Security Health (Resurssäkerhetshälsa)**. När du klickar på **Virtuella datorer** på hälsoikonen **Resurssäkerhet** öppnas bladet **Virtuella datorer** med rekommendationer för dina virtuella datorer. 

![Säkerhetshälsa](./media/security-center-virtual-machine/security-center-virtual-machine-fig2.png)

## <a name="manage-and-respond-to-security-alerts"></a>Hantera och åtgärda säkerhetsaviseringar
Security Center samlar automatiskt in, analyserar och integrerar loggdata från Azure-resurser, nätverket och anslutna partnerlösningar (som brandväggs- och slutpunktsskyddslösningar) för att identifiera verkliga hot och minimera antalet falska positiva identifieringar. Genom att leverera olika sammanställningar av [identifieringsfunktioner](security-center-detection-capabilities.md) kan Security Center skapa prioriterade säkerhetsvarningar som hjälper dig att snabbt undersöka problemet och tillhandahålla rekommendationer för hur du ska åtgärda eventuella attacker.

![Säkerhetsaviseringar](./media/security-center-virtual-machine/security-center-virtual-machine-fig3.png)

Om du klickar på en säkerhetsavisering får du se vad det var som utlöste aviseringen och om det finns något du kan göra för att stoppa ett pågående angrepp. Säkerhetsaviseringarna är indelade i grupper efter [typ](security-center-alerts-type.md) och datum.

## <a name="see-also"></a>Se även
I följande avsnitt kan du lära dig mer om Security Center:

* [Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Här får du lära dig hur du ställer in säkerhetsprinciper för prenumerationer och resursgrupper i Azure.
* [Hantera och åtgärda säkerhetsaviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Här får du lära dig hur du hanterar och åtgärdar säkerhetsaviseringar.
* [Vanliga frågor och svar om Azure Security Center](security-center-faq.md) – Här hittar du vanliga frågor och svar om tjänsten.

