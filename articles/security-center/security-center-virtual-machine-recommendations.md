---
title: aaaProtecting dina virtuella datorer i Azure Security Center | Microsoft Docs
description: "Det här dokumentet behandlar rekommendationerna i Azure Security Center som hjälper dig skydda dina virtuella datorer och uppfyller säkerhetsprinciper."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 47fa1f76-683d-4230-b4ed-d123fef9a3e8
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/19/2017
ms.author: terrylan
ms.openlocfilehash: 926c04974f61215b4a3e02646f23dafb87c793e8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="protecting-your-virtual-machines-in-azure-security-center"></a>Skydda dina virtuella datorer i Azure Security Center
Azure Security Center analyserar hello säkerhetstillståndet hos dina Azure-resurser. När Security Center identifierar potentiella säkerhetsrisker, skapar rekommendationer som guidar dig igenom hello konfigureringen av hello behövs kontroller.  Rekommendationer gäller tooAzure resurstyper: virtuella datorer (VM), nätverk, SQL och program.

Den här artikeln tar rekommendationer som gäller tooVMs.  VM rekommendationer center runt datainsamling, tillämpa systemuppdateringar, etablering skadlig kod, kryptera dina Virtuella diskar och mycket mer.  Använd hello tabellen nedan som en referens toohelp du förstå hello tillgängliga VM rekommendationer och vad varje göra om du använder den.

## <a name="available-vm-recommendations"></a>Tillgängliga rekommendationer för Virtuella datorer
| Rekommendation | Beskrivning |
| --- | --- |
| [Aktivera insamling av data för prenumerationer](security-center-enable-data-collection.md) |Rekommenderar att du aktiverar datainsamling i hello säkerhetsprincipen för var och en av dina prenumerationer och alla virtuella maskiner (VMs) i dina prenumerationer. |
| [Aktivera kryptering för Azure Storage-konto](security-center-enable-encryption-for-storage-account.md) | Rekommenderar att du aktiverar Azure Storage Service-kryptering för data i vila. Kryptering för lagring-tjänsten (SSE) fungerar genom att kryptera hello data när de skrivs tooAzure lagring och dekrypterar före hämtning. SSE är för närvarande endast tillgängligt för hello Azure Blob-tjänsten och kan användas för blockblobbar, sidblobbar, och tilläggsblobar. Det finns fler toolearn [Lagringstjänstens kryptering av vilande data](../storage/common/storage-service-encryption.md).</br>SSE stöds endast i hanteraren för filserverresurser storage-konton. Klassiska lagringskonton stöds inte för närvarande. toounderstand hello klassiska och Resource Manager distributionsmodellerna finns [Azure distributionsmodeller](../azure-classic-rm.md). |
| [Åtgärda sårbarheter i operativsystem](security-center-remediate-os-vulnerabilities.md) |Rekommenderar att du justerar OS-konfigurationer med hello rekommenderas konfigurationsregler, tillåter t.ex. inte att lösenord toobe sparas. |
| [Tillämpa systemuppdateringar](security-center-apply-system-updates.md) |Rekommenderar att du distribuerar saknas systemets säkerhet och viktiga uppdateringar tooVMs. |
| [Tillämpa Just-In-Time nätverk åtkomstkontroll](security-center-just-in-time.md) | Rekommenderar att du installerar just-in-time VM-åtkomst. hello just-in tiden funktionen är i förhandsvisning och finns på hello standardnivån av Security Center. Se [priser](security-center-pricing.md) toolearn mer om Security Center prisnivåer. |
| [Starta om datorn efter uppdateringarna](security-center-apply-system-updates.md#reboot-after-system-updates) |Rekommenderar att du startar om en VM toocomplete hello process för tillämpning av uppdateringar. |
| [Installera slutpunktsskydd](security-center-install-endpoint-protection.md) |Rekommenderar att du etablerar tooVMs för program mot skadlig kod program (endast Windows VM). |
| [Lös slutpunktsskydd för hälsovarningar](security-center-resolve-endpoint-protection-health-alerts.md) |Rekommenderar att du löser fel med slutpunktsskydd. |
| [Aktivera VM-Agent](security-center-enable-vm-agent.md) |Aktiverar du toosee som virtuella datorer kräver hello VM-agenten. hello VM-agenten måste installeras på virtuella datorer i ordning tooprovision korrigering genomsökning baslinjen genomsökning och program mot skadlig kod. hello VM-agenten installeras som standard för virtuella datorer som distribueras från hello Azure Marketplace. hello artikel [VM-Agent och tillägg – del 2](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) innehåller information om hur tooinstall hello VM-agenten. |
| [Tillämpa diskkryptering](security-center-apply-disk-encryption.md) |Rekommenderar att krypterar dina VM-diskar med Azure Disk Encryption (virtuella Windows- och Linux-datorer). Kryptering rekommenderas för både hello Operativsystemet och datavolymer på den virtuella datorn. |
| [Uppdatera OS-versionen](security-center-update-os-version.md) |Rekommenderar att du uppdaterar hello operativsystem (OS) version för din molntjänst toohello senaste tillgängliga versionen för OS-familjen.  toolearn mer om molntjänster finns hello [översikt över molntjänster](../cloud-services/cloud-services-choose-me.md). |
| [Sårbarhetsbedömning inte installerad](security-center-vulnerability-assessment-recommendations.md) |Rekommenderar att du installerar en lösning för sårbarhetsbedömning på den virtuella datorn. |
| [Åtgärda sårbarheter](security-center-vulnerability-assessment-recommendations.md#review-the-recommendation) |Låter dig toosee system- och sårbarheter som identifieras av hello vulnerability assessment redan är installerad på den virtuella datorn. |

## <a name="see-also"></a>Se även
toolearn mer om rekommendationer som gäller tooother Azure resurstyper finns hello följande:

* [Skydda dina program i Azure Security Center](security-center-application-recommendations.md)
* [Skydda ditt nätverk i Azure Security Center](security-center-network-recommendations.md)
* [Skydda din SQL Azure-tjänst i Azure Security Center](security-center-sql-service-recommendations.md)

toolearn mer om Security Center finns hello följande:

* [Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Lär dig hur tooconfigure säkerhetsprinciper för dina Azure-prenumerationer och resursgrupper.
* [Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Lär dig hur toomanage och svara toosecurity aviseringar.
* [Vanliga frågor om Azure Security Center](security-center-faq.md) --finns vanliga frågor om hur du använder hello-tjänsten.
