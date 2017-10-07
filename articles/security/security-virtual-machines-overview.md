---
title: "aaaAzure säkerhetsfunktioner som används med virtuella Azure-datorer | Microsoft Docs"
description: " En översikt över hello Azure säkerhetsfunktionerna som kan användas med virtuella Azure-datorer. Azure virtuella datorer får du hello virtualiseringsflexibilitet utan toobuy och underhålla hello fysisk maskinvara som kör hello VM. "
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 467b2c83-0352-4e9d-9788-c77fb400fe54
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/04/2017
ms.author: terrylan
ms.openlocfilehash: 1a1b9f02bd82a2655f4e2e5d9f9ce7a6671f63fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-virtual-machines-security-overview"></a>Översikt över säkerheten i Azure virtuella datorer
Med Azure Virtual Machines kan du effektivt distribuera många olika databehandlingslösningar. Med support för Microsoft Windows, Linux, Microsoft SQL Server, Oracle, IBM, SAP och Azure BizTalk Services kan du distribuera alla arbetsbelastningar och alla språk på nästan alla operativsystem.

En virtuell dator i Azure ger du hello virtualiseringsflexibilitet utan toobuy och underhålla hello fysisk maskinvara som kör hello virtuell dator.  Du kan skapa och distribuera ditt program med hello försäkran om att dina data är skyddade och säker i våra mycket säkert datacenter.

Med Azure, kan du skapa utökad säkerhet, kompatibla lösningar som:

* Skydda dina virtuella datorer mot virus och skadlig kod
* Kryptera känsliga data
* Skydda nätverkstrafik
* Identifiera och upptäck hot
* Uppfyll efterlevnadskrav

hello syftet med den här artikeln är tooprovide en översikt över hello Azure säkerhetsfunktionerna som kan användas med virtuella datorer. Vi erbjuder även länkar tooarticles som ger information om varje funktion så kan du läsa mer.  

hello core virtuell dator i Azure security funktioner toobe beskrivs i den här artikeln:

* Programvara mot skadlig kod
* Maskinvarusäkerhetsmodul
* Kryptering av virtuell dator
* Säkerhetskopiering av virtuell dator
* Azure Site Recovery
* Virtuella nätverk
* Hantering av säkerhetsprinciper och rapportering
* Efterlevnad

## <a name="antimalware"></a>Programvara mot skadlig kod
Med Azure, kan du använda program mot skadlig kod från säkerhet leverantörer, till exempel Microsoft, Symantec, Trend Micro och Kaspersky tooprotect dina virtuella datorer från skadliga filer, annonsprogram och andra hot. Se hello Lär dig mer avsnittet nedan toofind artiklar på partnerlösningar.

Microsoft Antimalware för Azure-molntjänster och virtuella datorer är en realtidsskydd funktion som hjälper dig att identifiera och ta bort virus, spionprogram och annan skadlig programvara.  Microsoft Antimalware innehåller konfigurerbara aviseringar när känd skadlig eller oönskad programvara försöker tooinstall själva eller köras på din Azure-system.

Microsoft Antimalware är en enskild agent lösning för program och klient miljöer utformad toorun i hello bakgrunden utan mänsklig inblandning. Du kan distribuera skydd baserat på hello behoven för din programbelastningar, med antingen grundläggande secure-som-standard eller avancerad anpassad konfiguration, inklusive övervakning av program mot skadlig kod.

När du distribuerar och aktivera Microsoft Antimalware är hello följande kärnfunktioner tillgängliga:

* Realtidsskydd - Övervakare aktivitet i Cloud Services och på virtuella datorer toodetect och blockera skadlig kod körning.
* Schemalagd genomsökning - utför regelbundet riktade skanning toodetect skadlig kod, inklusive program som körs aktivt.
* Korrigering av skadlig kod - vidtar automatiskt åtgärder på upptäckt skadlig kod, till exempel ta bort eller sätta i karantän skadliga filer och rensa skadliga registerposter.
* Signaturuppdateringar - automatiskt installerar hello senaste skydd signaturer (virusdefinitioner av) tooensure skydd är uppdaterad på en förbestämd frekvens.
* Motorn för program mot skadlig kod uppdaterar – automatiskt uppdateringar hello Microsoft Antimalware-motorn.
* Uppdateringar för program mot skadlig kod plattform – automatiskt uppdateringar hello Microsoft Antimalware plattform.
* Aktivt skydd – rapporterar tooAzure telemetri metadata om identifierade hot och misstänkta resurser tooensure snabba svar och aktiverar realtid synkron signatur leverans via hello Microsoft Active Protection System (MAPS).
* Exempel för reporting - innehåller och rapporter prover toohello Microsoft Antimalware service toohelp förfina hello service och aktivera felsökning.
* Undantag – låter programmet och service administratörer tooconfigure vissa filer, processer och driver tooexclude dem från skyddet och skanning för prestanda och andra orsaker.
* Program mot skadlig kod händelseinsamling - registrerar hello tjänstens hälsa för program mot skadlig kod, misstänkta aktiviteter och åtgärder som vidtagits i händelseloggen för hello operativsystem och samlar in dem i hello kundens Azure Storage-konto.

Läs mer: toolearn mer om program mot skadlig programvara tooprotect dina virtuella datorer, se:

* [Microsoft program mot skadlig kod för Azure-molntjänster och virtuella datorer](azure-security-antimalware.md)
* [Distribuera lösningar för skydd mot skadlig kod i Azure Virtual Machines](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/)
* [Hur tooinstall och konfigurera Trend Micro djup säkerhet som en tjänst på en Windows VM](../virtual-machines/windows/classic/install-trend.md)
* [Hur tooinstall och konfigurera Symantec Endpoint Protection på en Windows VM](../virtual-machines/windows/classic/install-symantec.md)
* [Säkerhetslösningar i hello Azure Marketplace](https://azure.microsoft.com/marketplace/?term=security)

## <a name="hardware-security-module"></a>Hardware security Module
Kryptering och autentisering skydd kan förbättras genom att förbättra nyckelsäkerhet. Du kan förenkla hello hantering och säkerhet för dina kritiska hemligheter och nycklar genom att lagra dem i Azure Key Vault. Key Vault ger hello alternativet toostore dina nycklar i maskinvara moduler (HSM) 140-2-certifierade tooFIPS nivå 2 säkerhetskrav. Din SQL Server krypteringsnycklarna för säkerhetskopiering eller [transparent datakryptering](https://msdn.microsoft.com/library/bb934049.aspx) kan alla lagras i Nyckelvalvet med alla nycklar och hemligheter från ditt program. Behörigheter och komma åt toothese skyddade objekt hanteras via [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/).

Läs mer:

* [Vad är Azure Key Vault?](../key-vault/key-vault-whatis.md)
* [Kom igång med Azure Key Vault](../key-vault/key-vault-get-started.md)
* [Azure Key Vault-blogg](https://blogs.technet.microsoft.com/kv/)

## <a name="virtual-machine-disk-encryption"></a>Kryptering av virtuell dator
Azure Disk Encryption är en ny funktion som gör att du krypterar din Windows- och Linux Azure virtuella diskar. Azure Disk Encryption använder hello branschstandard [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) funktion i Windows och hello [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt) funktion i Linux tooprovide volymkryptering för hello OS och hello datadiskar.

hello-lösning är integrerad med Azure Key Vault toohelp du styra och hantera hello disk krypteringsnycklar och hemligheter i prenumerationen nyckelvalv samtidigt som du säkerställer att krypteras alla data i hello virtuella diskar i vila i ditt Azure storage.

Läs mer:

* [Azure Disk Encryption för Windows och Linux-IaaS-VM](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)
* [Azure Disk Encryption för Linux och Windows-datorer](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/16/azure-disk-encryption-for-linux-and-windows-virtual-machines-public-preview-now-available/)
* [Kryptera en virtuell dator](../security-center/security-center-disk-encryption.md)

## <a name="virtual-machine-backup"></a>Säkerhetskopiering av virtuell dator
Azure Backup är en skalbar lösning som skyddar dina programdata helt utan kapitalinvestering och med minimal driftskostnad. Programfel kan skada dina data och den mänskliga faktorn kan införa buggar i dina program. Dina virtuella datorer som kör Windows och Linux är skyddade med Azure Backup.

Läs mer:

* [Vad är Azure Backup?](../backup/backup-introduction-to-azure-backup.md)
* [Azure Backup Utbildningsväg](https://azure.microsoft.com/documentation/learning-paths/backup/)
* [Azure Backup Service – vanliga frågor och svar](../backup/backup-azure-backup-faq.md)

## <a name="azure-site-recovery"></a>Azure Site Recovery
En viktig del av din organisations BCDR-strategi är att räkna ut hur tookeep företagets arbetsbelastningar och appar upp och körs när du planerade och oplanerade avbrott inträffar. Azure Site Recovery hjälper samordna replikering, redundans och återställning av arbetsbelastningar och appar så att de är tillgängliga från en sekundär plats om den primära platsen kraschar.

Site Recovery:

* **Förenklar din BCDR-strategi** – Site Recovery gör det enkelt toohandle replikering, redundans och återställning av flera arbetsbelastningar och appar från en enda plats. Site Recovery samordnar replikering och redundans men kan inte komma åt dina programdata och har inte någon information om dem.
* **Tillhandahåller flexibel replikering** – med Site Recovery kan du replikera arbetsbelastningar som körs på Hyper-V virtuella datorer, virtuella VMware-datorer och Windows-/ Linux fysiska servrar.
* **Har stöd för redundans och återställning** – Site Recovery tillhandahåller redundanstestning toosupport haveriberedskap utan att påverka produktionsmiljöer. Du kan också köra planerade redundansväxlingar utan någon dataförlust för förväntade driftsavbrott, eller oplanerade redundansväxlingar med minimal dataförlust (beroende på replikeringsfrekvens) för oväntade haverier. Du kan primära platser för återställning efter fel tooyour efter redundans. Site Recovery tillhandahåller återställningsplaner som kan innehålla skript och Azure Automation Runbook-rutiner så att du kan anpassa redundans och återställning av program med flera nivåer.
* **Eliminerar sekundärt datacenter** – du kan replikera tooa sekundär lokal plats eller tooAzure. Med Azure som mål för katastrofåterställning eliminerar hello kostnaden och komplexiteten för att underhålla en sekundär plats. Replikerade data lagras i Azure Storage.
* **Kan integreras med befintlig BCDR-teknik** – Site Recovery samarbetar med andra BCDR-programfunktioner. Du kan till exempel använda Site Recovery tooprotect hello SQL Server serverdelen av företagets arbetsbelastningar. Detta inkluderar inbyggt stöd för SQL Server AlwaysOn toomanage hello redundans för Tillgänglighetsgrupper.

Läs mer:

* [Vad är Azure Site Recovery?](../site-recovery/site-recovery-overview.md)
* [Hur fungerar Azure Site Recovery?](../site-recovery/site-recovery-components.md)
* [Vilka arbetsbelastningar skyddas av Azure Site Recovery?](../site-recovery/site-recovery-workload.md)

## <a name="virtual-networking"></a>Virtuella nätverk
Virtuella datorer måste nätverksanslutningen. toosupport kräver detta krav, Azure virtuella datorer toobe anslutna tooan Azure Virtual Network. Ett virtuellt Azure-nätverk är en logisk konstruktion som bygger på hello fysisk Azure nätverksinfrastruktur. Varje logiskt virtuella Azure-nätverk är isolerat från alla andra virtuella Azure-nätverk. Denna isolering kan försäkra dig om att nätverkstrafik i din distribution inte är tillgänglig tooother Microsoft Azure-kunder.

Läs mer:

* [Översikt över säkerheten i Azure-nätverk](security-network-overview.md)
* [Översikt över virtuella nätverk](../virtual-network/virtual-networks-overview.md)
* [Nätverksfunktioner och partnerskap för företagsscenarier](https://azure.microsoft.com/blog/networking-enterprise/)

## <a name="security-policy-management-and-reporting"></a>Hantering av säkerhetsprinciper och rapportering
Azure Security Center hjälper dig att förebygga, upptäcka och åtgärda toothreats och ger du ökad insyn i, och kontroll över, hello säkerheten för dina Azure-resurser. Härifrån kan du övervaka och hantera principer för alla Azureprenumerationer på en gång och upptäcka hot som annars kanske skulle förbli oupptäckta. Azure Security Center fungerar tillsammans med ett vittomfattande ekosystem med säkerhetslösningar.

Azure Security Center hjälper dig att optimera och övervaka virtuella säkerhet med:

* Att tillhandahålla virtuella [säkerhetsrekommendationer](../security-center/security-center-recommendations.md) sådana som gäller systemuppdateringar, konfigurera ACL: er slutpunkter, aktivera program mot skadlig kod, aktivera nätverkssäkerhetsgrupper och gäller kryptering av.
* Övervaka hello tillståndet för dina virtuella datorer

Läs mer:

* [Introduktion tooAzure Security Center](../security-center/security-center-intro.md)
* [Vanliga och frågor svar om Azure Security Center](../security-center/security-center-faq.md)
* [Planera för Azure Security Center och åtgärder](../security-center/security-center-planning-and-operations-guide.md)

## <a name="compliance"></a>Efterlevnad
Virtuella Azure-datorer är certifierad för FISMA, FedRAMP, HIPAA, PCI DSS nivå 1 och andra program som nyckel kompatibilitet. Den här certifikatutfärdare gör det enklare för din egen Azure-program toomeet krav på efterlevnad och för ditt företag tooaddress en mängd olika nationella och internationella krav.

Läs mer:

* [Microsoft Trust Center: kompatibilitet](https://www.microsoft.com/TrustCenter/Compliance/default.aspx)
* [Betrodda moln: Microsoft Azure-säkerhet, sekretess och efterlevnad](http://download.microsoft.com/download/1/6/0/160216AA-8445-480B-B60F-5C8EC8067FCA/WindowsAzure-SecurityPrivacyCompliance.pdf)
