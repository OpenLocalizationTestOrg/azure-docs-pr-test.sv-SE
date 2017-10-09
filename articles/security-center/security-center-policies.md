---
title: "aaaSet säkerhetsprinciper i Azure Security Center | Microsoft Docs"
description: "Det här dokumentet hjälper dig att tooconfigure säkerhetsprinciper i Azure Security Center."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 3b9e1c15-3cdb-4820-b678-157e455ceeba
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2017
ms.author: yurid
ms.openlocfilehash: 59226dd84a1c66a2d8367417060ab10a1ff73848
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-security-policies-in-azure-security-center"></a>Ange säkerhetsprinciper i Azure Security Center
Det här dokumentet hjälper dig att tooconfigure säkerhetsprinciper i Security Center genom instruktioner hello tooperform för åtgärder som krävs för den här uppgiften.

>[!NOTE] 
>Från och med tidig juni 2017 använder Security Center hello Microsoft Monitoring Agent toocollect och lagra data. Se [Azure Security Center-plattformen migrering](security-center-platform-migration.md) toolearn mer. hello informationen i den här artikeln representerar Security Center-funktionalitet efter övergången toohello Microsoft Monitoring Agent.
>

## <a name="what-are-security-policies"></a>Vad är säkerhetsprinciper?
En säkerhetsprincip definierar hello kontrollfunktioner som rekommenderas för resurser inom hello angivna prenumerationen. I Security Center Definiera principer för dina Azure-prenumerationer enligt tooyour företags behov och hello typ av program eller känslighet hello data i varje prenumeration.

De resurser som används för utveckling eller testning har kanske andra säkerhetskrav än de resurser som används för program i produktionen. På samma sätt behövs det kanske en högre säkerhetsnivå för sådant som det finns lagar om, till exempel personuppgifter. Säkerhetsprinciper som är aktiverade i Azure Security Center enhet säkerhetsrekommendationer och övervakning toohelp du identifiera potentiella säkerhetsrisker och avhjälpa hot. Läs [Azure Security Center planerings- och handboken](security-center-planning-and-operations-guide.md) för mer information om hur toodetermine hello alternativ som är lämpliga för dig.

## <a name="set-security-policies"></a>Ange säkerhetsprinciper
Det går att ställa in särskilda säkerhetsprinciper för varje prenumeration. toomodify säkerhetspolicyn, bör du vara ägare eller deltagare i den aktuella prenumerationen. Logga in toohello Azure-portalen och följer hello efterföljande steg tooconfigure säkerhetsprinciper i Security Center:

1. Klicka på hello **princip** panelen i instrumentpanelen för hello Security Center.
2. I hello säkerhetsprincip bladet som öppnas väljer du hello prenumeration som du vill använda tooenable hello säkerhetsprincip.

    ![Ange princip](./media/security-center-policies/security-center-policies-fig1-ga.png)
3. Hej **säkerhetsprincip** bladet för hello valt prenumerationen öppnas med en uppsättning alternativ. hello alternativ i det här bladet är tillgängliga:

   * **Princip för dataexekveringsskydd**: Använd det här alternativet tooconfigure principer per prenumeration.  
   * **E-postavisering**: Använd det här alternativet tooconfigure ett e-postmeddelande som skickas på hello första dagliga förekomsten av en avisering och för varningar med hög angelägenhetsgrad. E-postinställningar kan bara konfigureras för prenumerationsprinciper. Läs [ange säkerhet kontaktinformation i Azure Security Center](security-center-provide-security-contact-details.md) för mer information om hur tooconfigure ett e-postmeddelande.
   * **Prisnivån**: Använd det här alternativet tooupgrade hello priser för val av. Se [Security Center priser](security-center-pricing.md) toolearn mer om prissättningen för alternativ.
4. Kontrollera att alternativet **Samla in data från virtuella datorer** är inställt på **På**. Det här alternativet samlas loggar in automatiskt för befintliga och nya resurser med hjälp av hello Microsoft Monitoring Agent – detta är hello samma agent används hello Operations Management Suite och Log Analytics-tjänsten. Data som samlas in från den här agenten lagras i en befintlig logganalys arbetsytor som är associerade med din Azure-prenumeration eller en ny arbetsytor med hänsyn till kontot hello geografi av hello VM.

5. I hello **säkerhetsprincip** bladet, klickar du på **skyddsprincip** toosee hello tillgängliga alternativ. Klicka på **på** tooenable hello säkerhetsrekommendationer som är relevanta för den här prenumerationen.

    ![Att välja hello säkerhetsprinciper](./media/security-center-policies/security-center-policies-fig7.png)

Använd följande tabell som en referens toounderstand hello varje alternativ:

| Princip | När den är aktiverad |
| --- | --- |
| Systemuppdateringar |Hämtar en daglig lista med tillgängliga säkerhetsuppdateringar och viktiga uppdateringar från Windows Update eller Windows Server Update Services. hello hämtade listan beroende hello-tjänst som är konfigurerad för den virtuella datorn och rekommenderar att hello saknade uppdateringar tillämpas. För Linux-datorer använder hello princip hello angivna distro paketet management system toodetermine paket som har tillgängliga uppdateringar. Den söker också efter säkerhetsuppdateringar och viktiga uppdateringar från virtuella datorer i [Azure Cloud Services](../cloud-services/cloud-services-how-to-configure.md). |
| Sårbarheter i operativsystem |Analyserar konfigurationer dagliga toodetermine problem med operativsystem som kan göra hello virtuella sårbara tooattack. hello princip rekommenderar också konfiguration ändringar tooaddress dessa problem. Se hello [listan med rekommenderade baslinjer](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335) mer information om hello specifika konfigurationer som övervakas. (Just nu stöds inte Windows Server 2016 fullt ut.) |
| Slutpunktsskydd |Rekommenderar endpoint protection toobe etableras för alla Windows-datorer toohelp identifiera och ta bort virus, spionprogram och annan skadlig programvara. |
| Diskkryptering |Rekommenderar att aktivera kryptering i alla virtuella datorer tooenhance skydd av data i vila. |
| Nätverkssäkerhetsgrupper |Rekommenderar att [nätverkssäkerhetsgrupper](../virtual-network/virtual-networks-nsg.md) vara konfigurerade toocontrol inkommande och utgående trafik tooVMs som har offentliga slutpunkter. Nätverkssäkerhetsgrupper som konfigureras för ett undernät ärvs av alla VM-nätverksgränssnitt om inget annat anges. Dessutom toochecking att en nätverkssäkerhetsgrupp har konfigurerats, principen utvärderar inkommande säkerhetsregler tooidentify regler som tillåter inkommande trafik. |
| Brandvägg för webbaserade program |Rekommenderar att etableras en brandvägg för webbaserade program på virtuella datorer när något av hello följande stämmer: </br></br>[Offentlig IP på instansnivå](../virtual-network/virtual-networks-instance-level-public-ip.md) (går) används och hello inkommande säkerhetsregler för hello kopplade nätverkssäkerhetsgruppen är konfigurerade tooallow åtkomst tooport 80/443.</br></br>Belastningsutjämnad IP används och hello associerade belastningsutjämning och inkommande network address translation (NAT) regler är konfigurerade tooallow åtkomst tooport 80/443. (Mer information finns i [Azure Resource Manager-stöd för Load Balancer](../load-balancer/load-balancer-arm.md). |
| Nästa generations brandvägg |Nätverksskyddet utökas utöver nätverkssäkerhetsgrupperna, som är inbyggda i Azure. Security Center identifieras av distributioner som nästa generations brandvägg rekommenderas och aktivera tooprovision en virtuell installation. |
| SQL-granskning och hotidentifiering |Rekommenderar att granskning av åtkomst tooAzure databas aktiverad för efterlevnad och även avancerad hotidentifiering av undersökningsskäl. |
| SQL-kryptering |Rekommenderar att kryptering i vila ska aktiveras för Azure SQL Database, tillhörande säkerhetskopior och transaktionsloggfiler. Även om intrång i datan sker, går den inte att läsa. |
| Sårbarhetsbedömning |Rekommenderar att du installerar en lösning för sårbarhetsbedömning på den virtuella datorn. |
| Lagringskryptering |Den här funktionen är för närvarande tillgänglig för Azure-blobbar och filer. När du har aktiverat Kryptering av lagringstjänst kommer endast nya data att krypteras. Befintliga filer i lagringskontot förbli okrypterade. |
| JIT-nätverksåtkomst |När precis i tid är aktiverat, låser Security Center ned inkommande trafik tooyour virtuella Azure-datorer genom att skapa en regel för NSG. Du väljer hello portar på hello VM toowhich inkommande trafik ska låsas. Mer information finns i [Manage virtual machine access using just in time](https://docs.microsoft.com/azure/security-center/security-center-just-in-time) (Hantera åtkomsten till virtuella datorer med Just-In-Time). |

När du konfigurerar alla alternativ klickar du på **OK** i hello **säkerhetsprincip** bladet som har hello rekommendationer och klicka sedan på **spara** i hello **säkerhet Principen** bladet hello startinställningar.

> [!NOTE]
> hello prisnivån gäller fortfarande för hello resursgruppsnivå. Mer information som besöker hello [priser](https://azure.microsoft.com/pricing/details/security-center/) sidan.
>
>

## <a name="see-also"></a>Se även
I det här dokumentet du lärt dig hur tooconfigure säkerhetsprinciper i Azure Security Center. toolearn mer om Azure Security Center finns hello följande:

* [Planerings- och bruksanvisning för Azure Security Center](security-center-planning-and-operations-guide.md). Lär dig hur tooplan och förstå hello design överväganden tooadopt Azure Security Center.
* [Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md). Lär dig hur toomonitor hello Azure-resursers hälsa.
* [Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md). Lär dig hur toomanage och svara toosecurity aviseringar.
* [Övervaka partnerlösningar med Azure Security Center](security-center-partner-solutions.md). Lär dig hur toomonitor hello dina partnerlösningars hälsostatus.
* [Vanliga frågor och svar om Azure Security Center](security-center-faq.md). Sök efter vanliga frågor om hur du använder hello-tjänsten.
* [Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/). Här hittar du blogginlägg om säkerhet och regelefterlevnad i Azure.
