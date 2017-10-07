---
title: "aaaManaging säkerhetsrekommendationer i Azure Security Center | Microsoft Docs"
description: "Det här dokumentet vägleder dig igenom hur i Azure Security Center hjälper dig att skydda dina Azure-resurser och vara kompatibla med säkerhetsprinciper."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 86c50c9f-eb6b-4d97-acb3-6d599c06133e
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/19/2017
ms.author: terrylan
ms.openlocfilehash: f6bbe36a7a5636095b339b3e9765b87cc0ab669a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="managing-security-recommendations-in-azure-security-center"></a>Hantera säkerhetsrekommendationer i Azure Security Center
Det här dokumentet vägleder dig igenom hur toouse rekommendationerna i Azure Security Center toohelp du skydda dina Azure-resurser.

> [!NOTE]
> Det här dokumentet introducerar hello-tjänsten med hjälp av ett exempel på distribution.  Det här dokumentet är inte en stegvis guide.
>
>

## <a name="what-are-security-recommendations"></a>Vad är säkerhetsrekommendationer?
Security Center analyserar regelbundet hello säkerhetstillståndet hos dina Azure-resurser. När Security Center identifierar potentiella säkerhetsproblem skapas rekommendationer. hello rekommendationer leder dig igenom hello konfigureringen av hello behövs kontroller.

## <a name="implementing-security-recommendations"></a>Utföra säkerhetsrekommendationerna
### <a name="set-recommendations"></a>Set-rekommendationer
I [ställa in säkerhetsprinciper i Azure Security Center](security-center-policies.md), du lär dig hur du:

* Konfigurera säkerhetsprinciper.
* Aktivera insamling av data.
* Välj vilka rekommendationer toosee som en del av din säkerhetsprincip.

Aktuella rekommendationer principcenter runt systemuppdateringar, baslinjeregler, program mot skadlig kod, [nätverkssäkerhetsgrupper](../virtual-network/virtual-networks-nsg.md) på undernät och nätverksgränssnitt, SQL database auditing, SQL database transparent datakryptering och webbprogrammet brandväggar.  [Ställa in säkerhetsprinciper](security-center-policies.md) innehåller en beskrivning av varje rekommendation alternativ.

### <a name="monitor-recommendations"></a>Övervakaren rekommendationer
När du har angett en säkerhetsprincip för analyserar Security Center hello säkerhetsstatus för dina resurser tooidentify potentiella säkerhetsrisker. Hej **rekommendationer** panelen på hello **Security Center** bladet får du reda på hello Totalt antal rekommendationer som identifieras av Security Center.

![Rekommendationer sida vid sida][1]

toosee hello information om varje rekommendation:

Välj hello **rekommendationer panelen** på hello **Security Center** bladet. Hej **rekommendationer** blad öppnas.

hello rekommendationer som visas i tabellformat där varje rad motsvarar en viss rekommendation. hello kolumner i den här tabellen är:

* **Beskrivning**: förklarar hello rekommendation och vad toobe klar tooaddress den.
* **RESURSEN**: Visar hello resurser toowhich gäller den här rekommendationen.
* **TILLSTÅND**: Beskriver hello aktuell status för hello rekommendation:
  * **Öppna**: hello rekommendationen har inte utförts än.
  * **Pågående**: hello rekommendationen håller tillämpade toohello resurser och ingen åtgärd krävs av dig.
  * **Matcha**: hello rekommendationen har redan slutförts (i det här fallet hello raden är nedtonad).
* **ALLVARLIGHETSGRAD**: Beskriver hello allvarlighetsgraden viktig rekommendationen:
  * **Hög**: ett säkerhetsproblem finns i en viktig resurs (till exempel ett program, en virtuell dator eller en nätverkssäkerhetsgrupp) som måste åtgärdas.
  * **Medel**: det finns ett säkerhetsproblem och icke-kritiska eller ytterligare steg är nödvändiga tooeliminate den eller toocomplete en process.
  * **Låg**: det finns ett säkerhetsproblem som bör åtgärdas, men inte kräver omedelbara uppmärksamhet. (Som standard låg rekommendationer inte visas, men du kan filtrera på låg rekommendationer om du vill toosee dem.)

Använd hello tabellen nedan som en referens toohelp du förstå hello tillgängliga rekommendationer och det var och en gör om du använder den.

> [!NOTE]
> Vill du toounderstand hello [klassisk och Resource Manager distributionsmodellerna](../azure-classic-rm.md) för Azure-resurser.
>
>

| Rekommendation | Beskrivning |
| --- | --- |
| [Aktivera insamling av data för prenumerationer](security-center-enable-data-collection.md) |Rekommenderar att du aktiverar datainsamling i hello säkerhetsprincipen för var och en av dina prenumerationer och alla virtuella maskiner (VMs) i dina prenumerationer. |
| [Åtgärda sårbarheter i operativsystem](security-center-remediate-os-vulnerabilities.md) |Rekommenderar att du justera OS-konfigurationer med hello rekommenderas konfigurationsregler, till exempel, tillåter inte lösenord toobe sparas. |
| [Tillämpa systemuppdateringar](security-center-apply-system-updates.md) |Rekommenderar att du distribuerar saknas systemets säkerhet och viktiga uppdateringar tooVMs. |
| [Tillämpa Just-In-Time nätverk åtkomstkontroll](security-center-just-in-time.md) | Rekommenderar att du installerar just-in-time VM-åtkomst. hello just-in tiden funktionen är i förhandsvisning och finns på hello standardnivån av Security Center. Se [priser](security-center-pricing.md) toolearn mer om Security Center prisnivåer. |
| [Starta om datorn efter uppdateringarna](security-center-apply-system-updates.md#reboot-after-system-updates) |Rekommenderar att du startar om en VM toocomplete hello process för tillämpning av uppdateringar. |
| [Lägga till en brandvägg för webbappar](security-center-add-web-application-firewall.md) |Rekommenderar att du distribuerar en brandvägg för webbaserade program (Brandvägg) för web-slutpunkter. En Brandvägg rekommendation visas för alla offentliga Internetriktade IP-adresser (instans nivå IP eller Load belastningsutjämnade IP) som har en nätverkssäkerhetsgrupp med öppna webbplats för inkommande portar (80,443). </br>Security Center rekommenderar att etablera en Brandvägg toohelp skydd mot angrepp målobjekt för webbaserade program på virtuella datorer samt på Apptjänst-miljö. En App Service miljö (ASE) är en [Premium](https://azure.microsoft.com/pricing/details/app-service/) service plan alternativet för Azure App Service som tillhandahåller en helt isolerad och dedikerad miljö för Azure App Service-program som körs på ett säkert sätt. toolearn mer om ASE, se hello [dokumentationen till App Service-miljö](../app-service/app-service-app-service-environments-readme.md).</br>Du kan skydda flera webbprogram i Security Center genom att lägga till dessa program tooyour befintliga Brandvägg-distributioner. |
| [Slutför programskydd](security-center-add-web-application-firewall.md#finalize-application-protection) |toocomplete hello konfigurationen av en Brandvägg, trafik måste vara omdirigerade toohello Brandvägg. Efter den här rekommendationen är klar hello nödvändiga ändringar. |
| [Lägga till en nästa generations brandvägg](security-center-add-next-generation-firewall.md) |Rekommenderar att du lägger till en nästa generations Brandvägg från en Microsoft-partner tooincrease din säkerhetsskydd. |
| [Dirigera trafiken endast via NGFW](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only) |Rekommenderar att du konfigurerar (NSG) regler för nätverkssäkerhetsgrupper som tvingar inkommande trafik tooyour VM via din nästa generations Brandvägg. |
| [Installera slutpunktsskydd](security-center-install-endpoint-protection.md) |Rekommenderar att du etablerar tooVMs för program mot skadlig kod program (endast Windows VM). |
| [Lös slutpunktsskydd för hälsovarningar](security-center-resolve-endpoint-protection-health-alerts.md) |Rekommenderar att du löser fel med slutpunktsskydd. |
| [Aktivera Nätverkssäkerhetsgrupper för undernät eller virtuella datorer](security-center-enable-network-security-groups.md) |Rekommenderar att du aktiverar NSG: er på undernät eller virtuella datorer. |
| [Begränsa åtkomst via Internetuppkopplad slutpunkt](security-center-restrict-access-through-internet-facing-endpoints.md) |Rekommenderar att du konfigurerar regler för inkommande trafik för NSG: er. |
| [Aktivera granskning och hotidentifiering på SQL-servrar](security-center-enable-auditing-on-sql-servers.md) |Rekommenderar att du aktiverar granskning och hotidentifiering identifiering för Azure SQL-servrar. (Endast azure SQL-tjänsten. Inkluderar inte SQL som körs på virtuella datorer.) |
| [Aktivera granskning och hotidentifiering på SQL-databaser](security-center-enable-auditing-on-sql-databases.md) |Rekommenderar att du aktiverar granskning och hotidentifiering identifiering för Azure SQL-databaser. (Endast azure SQL-tjänsten. Inkluderar inte SQL som körs på virtuella datorer.) |
| [Aktivera Transparent datakryptering på SQL-databaser](security-center-enable-transparent-data-encryption.md) |Rekommenderar att du aktiverar kryptering för SQL-databaser. (Endast azure SQL-tjänsten.) |
| [Aktivera VM-Agent](security-center-enable-vm-agent.md) |Aktiverar du toosee som virtuella datorer kräver hello VM-agenten. hello VM-agenten måste installeras på virtuella datorer tooprovision korrigering genomsökning baslinjen genomsökning och program mot skadlig kod. hello VM-agenten installeras som standard för virtuella datorer som distribueras från hello Azure Marketplace. hello artikel [VM-Agent och tillägg – del 2](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) innehåller information om hur tooinstall hello VM-agenten. |
| [Tillämpa diskkryptering](security-center-apply-disk-encryption.md) |Rekommenderar att krypterar dina VM-diskar med Azure Disk Encryption (virtuella Windows- och Linux-datorer). Kryptering rekommenderas för både hello Operativsystemet och datavolymer på den virtuella datorn. |
| [Ange säkerhetskontaktinformation](security-center-provide-security-contact-details.md) |Rekommenderar att du ger säkerhet kontaktinformation för var och en av dina prenumerationer. Kontaktinformation är en e-postadress och telefonnummer. hello information är används toocontact om våra säkerhetsteam hittar att dina resurser komprometteras. |
| [Uppdatera OS-versionen](security-center-update-os-version.md) |Rekommenderar att du uppdaterar hello operativsystem (OS) version för din molntjänst toohello senaste tillgängliga versionen för OS-familjen.  toolearn mer om molntjänster finns hello [översikt över molntjänster](../cloud-services/cloud-services-choose-me.md). |
| [Sårbarhetsbedömning inte installerad](security-center-vulnerability-assessment-recommendations.md) |Rekommenderar att du installerar en lösning för sårbarhetsbedömning på den virtuella datorn. |
| [Åtgärda sårbarheter](security-center-vulnerability-assessment-recommendations.md#review-the-recommendation) |Låter dig toosee system- och sårbarheter som identifieras av hello vulnerability assessment redan är installerad på den virtuella datorn. |
| [Aktivera kryptering för Azure Storage-konto](security-center-enable-encryption-for-storage-account.md) | Rekommenderar att du aktiverar Azure Storage Service-kryptering för data i vila. Kryptering för lagring-tjänsten (SSE) fungerar genom att kryptera hello data när de skrivs tooAzure lagring och dekrypterar före hämtning. SSE är för närvarande endast tillgängligt för hello Azure Blob-tjänsten och kan användas för blockblobbar, sidblobbar, och tilläggsblobar. Det finns fler toolearn [Lagringstjänstens kryptering av vilande data](../storage/common/storage-service-encryption.md).</br>SSE stöds endast i hanteraren för filserverresurser storage-konton. |

Du kan filtrera och ignorera rekommendationer.

1. Välj **Filter** på hello **rekommendationer** bladet. Hej **Filter** blad öppnas och du väljer hello allvarlighetsgrad och tillstånd för värden som du vill toosee.

    ![Filtrera rekommendationer][2]
2. Om du anser att en rekommendation är inte tillämplig, du kan ignorera hello rekommendation och filtrera bort vyn. Det finns två sätt toodismiss en rekommendation. Ett sätt är tooright klickar du på ett objekt och välj sedan **Ignorera**. hello andra är toohover över ett objekt, klickar du på hello tre punkter som visas toohello höger och välj sedan **Ignorera**. Du kan visa Avvisad rekommendationer genom att klicka på **Filter**, och sedan välja **avvisat**.

    ![Ignorera rekommendation][3]

### <a name="apply-recommendations"></a>Tillämpa rekommendationer
När du har granskat Bestäm alla rekommendationer som du bör använda först. Vi rekommenderar att du använder hello allvarlighetsgrad som hello huvudsakliga parametern tooevaluate vilka rekommendationer som ska användas först.

Markera en rekommendation i hello tabell med rekommendationerna ovan, och gå igenom den som ett exempel på hur tooapply en rekommendation.

## <a name="next-steps"></a>Nästa steg
I det här dokumentet har introducerades toosecurity rekommendationer i Security Center. toolearn mer om Security Center finns hello följande:

* [Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Lär dig hur tooconfigure säkerhetsprinciper för dina Azure-prenumerationer och resursgrupper.
* [Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md) – Lär dig hur toomonitor hello Azure-resursers hälsa.
* [Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Lär dig hur toomanage och svara toosecurity aviseringar.
* [Övervaka partnerlösningar med Azure Security Center](security-center-partner-solutions.md) – Lär dig hur toomonitor hello dina partnerlösningars hälsostatus.
* [Vanliga frågor om Azure Security Center](security-center-faq.md) – finns vanliga frågor om hur du använder hello-tjänsten.
* [Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) – Här hittar du blogginlägg om säkerhet och regelefterlevnad i Azure.

<!--Image references-->
[1]: ./media/security-center-recommendations/recommendations-tile.png
[2]: ./media/security-center-recommendations/filter-recommendations.png
[3]: ./media/security-center-recommendations/dismiss-recommendations.png
