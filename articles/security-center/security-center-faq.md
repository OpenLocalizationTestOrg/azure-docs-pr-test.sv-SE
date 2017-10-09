---
title: "aaaAzure Security Center vanliga frågor (FAQ) | Microsoft Docs"
description: "Det här avsnittet får du svar frågor om Azure Security Center."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: be2ab6d5-72a8-411f-878e-98dac21bc5cb
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: terrylan
ms.openlocfilehash: cd0c0f8bdf15cdaf5889f2da5ac3cadf6017a9e7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-center-frequently-asked-questions-faq"></a>Vanliga frågor och svar om Azure Security Center
Det här avsnittet får du svar frågor om Azure Security Center, en tjänst som hjälper dig att förebygga, upptäcka och åtgärda toothreats med bättre överblick och kontroll över hello säkerheten för din Microsoft Azure-resurser.

> [!NOTE]
> Från och med tidig juni 2017 Security Center använder hello Microsoft Monitoring Agent toocollect och lagra data. Det finns fler toolearn [Azure Security Center-plattformen migrering](security-center-platform-migration.md). hello informationen i den här artikeln representerar Security Center-funktionalitet efter övergången toohello Microsoft Monitoring Agent.
>
>

## <a name="general-questions"></a>Allmänna frågor
### <a name="what-is-azure-security-center"></a>Vad är Azure Security Center?
Azure Security Center hjälper dig att förebygga, upptäcka och åtgärda toothreats med bättre överblick och kontroll över hello säkerheten för dina Azure-resurser. Härifrån kan du övervaka och hantera principer för alla prenumerationer på en gång och upptäcka hot som annars kanske skulle förbli oupptäckta. Azure Security Center fungerar tillsammans med ett vittomfattande ekosystem med säkerhetslösningar.

### <a name="how-do-i-get-azure-security-center"></a>Hur skaffar jag Azure Security Center?
Azure Security Center har aktiverats med Microsoft Azure-prenumerationen och nås från hello [Azure-portalen](https://azure.microsoft.com/features/azure-portal/). ([Logga in toohello portal](https://portal.azure.com)väljer **Bläddra**, och rulla för**Security Center**).  

## <a name="billing"></a>Fakturering
### <a name="how-does-billing-work-for-azure-security-center"></a>Hur fungerar fakturering arbete för Azure Security Center?
Security Center erbjuds i två nivåer:

Hej **kostnadsfria nivån** ger inblick i hello säkerhetstillståndet hos dina Azure-resurser, grundläggande säkerhetsprinciper, säkerhetsrekommendationer och integrering med säkerhetsprodukter och tjänster från partner.

Hej **standardnivån** lägger till avancerade threat detection funktioner, inklusive hot intelligence, beteendeanalys, avvikelseidentifiering, säkerhetsincidenter och hot tillskrivningar rapporter. hello standardnivån är gratis för hello första 60 dagarna. Bör du välja toocontinue toouse hello tjänsten efter 60 dagar, starta vi automatiskt toocharge för hello-tjänsten.  tooupgrade, Välj prisnivån i hello [säkerhetsprincip](security-center-policies.md#set-security-policies). Det finns fler toolearn [Security Center priser](security-center-pricing.md).

## <a name="permissions"></a>Behörigheter
Azure Security Center använder [rollbaserad åtkomstkontroll (RBAC)](../active-directory/role-based-access-control-configure.md), vilket ger [inbyggda roller](../active-directory/role-based-access-built-in-roles.md) som kan tilldelas toousers, grupper och tjänster i Azure.

Security Center utvärderar hello konfigurationen av dina resurser tooidentify säkerhetsproblem och säkerhetsproblem. I Security Center, du kan bara se information relaterade tooa resurs när du har tilldelats hello rollen ägare, deltagare eller läsare för hello prenumerationen eller resursen gruppen som en resurs tillhör.

Se [behörigheter i Azure Security Center](security-center-permissions.md) toolearn mer om roller och tillåtna åtgärder i Security Center.

## <a name="data-collection"></a>Datainsamling
Security Center samlar in data från dina virtuella datorer tooassess sina säkerhetstillstånd anger säkerhetsrekommendationer och varna dig toothreats. Första gången du öppnar Security Center, är insamling av data aktiverat på alla virtuella datorer i din prenumeration. Du kan även aktivera datainsamling i hello Security Center-princip.

### <a name="how-do-i-disable-data-collection"></a>Hur inaktiverar insamling av data?
Om du använder hello Azure Security Center kostnadsfria nivån, kan du inaktivera insamling av data från virtuella datorer när som helst. Insamling av data krävs för prenumerationer på hello standardnivån. Du kan inaktivera datainsamling för en prenumeration i hello säkerhetsprincip. ([Logga in toohello Azure-portalen](https://portal.azure.com)väljer **Bläddra**väljer **Security Center**, och välj **princip**.)  När du väljer en prenumeration, ett nytt blad öppnas och visar du hello alternativet tooturn av **datainsamling**.

### <a name="how-do-i-enable-data-collection"></a>Hur aktiverar insamling av data?
Du kan aktivera datainsamling för din Azure-prenumeration i hello säkerhetsprincip. insamling av tooenable. [Logga in toohello Azure-portalen](https://portal.azure.com)väljer **Bläddra**väljer **Security Center**, och välj **princip**. Ange **datainsamling** för**på**.

### <a name="what-happens-when-data-collection-is-enabled"></a>Vad händer när datainsamling har aktiverats?
När datainsamling har aktiverats hello Microsoft Monitoring Agent etableras automatiskt på alla befintliga och nya kompatibla virtuella datorer som distribueras i hello prenumerationen.

### <a name="does-hello-monitoring-agent-impact-hello-performance-of-my-servers"></a>Stöder hello Monitoring Agent påverkan hello prestanda för Mina servrar?
hello agenten förbrukar en nominell mängd systemresurser och bör har liten inverkan på prestanda hello. Mer information om inverkan på prestanda och hello agent och tillägg finns hello [planerings-och operations](security-center-planning-and-operations-guide.md#data-collection-and-storage).

### <a name="where-is-my-data-stored"></a>Var lagras mina data?
Data som samlas in från den här agenten lagras i en befintlig logganalys-arbetsyta som är associerade med din prenumeration eller en ny arbetsyta. Mer information finns i [datasäkerhet](security-center-data-security.md).

## <a name="using-azure-security-center"></a>Med hjälp av Azure Security Center
### <a name="what-is-a-security-policy"></a>Vad är en säkerhetsprincip?
En säkerhetsprincip definierar hello antal kontrollfunktioner som rekommenderas för resurser inom hello angivna prenumerationen. I Azure Security Center Definiera principer för dina Azure-prenumerationer enligt tooyour företagets säkerhetskrav och hello typ av program eller känslighet hello data i varje prenumeration.

hello säkerhetsprinciperna i Azure Security Center enhet säkerhetsrekommendationer och övervakning. toolearn mer information om IPSec-principer finns [övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md).

### <a name="who-can-modify-a-security-policy"></a>Vem som kan ändra en säkerhetsprincip?
toomodify säkerhetsprinciper, du måste vara en administratör eller ägare eller deltagare i den aktuella prenumerationen.

hur tooconfigure en säkerhetsprincip Se toolearn [ställa in säkerhetsprinciper i Azure Security Center](security-center-policies.md).

### <a name="what-is-a-security-recommendation"></a>Vad är en säkerhetsrekommendation?
Azure Security Center analyserar hello säkerhetstillståndet hos dina Azure-resurser. När eventuella säkerhetsproblem identifieras skapas rekommendationer. hello rekommendationer guiden dig genom hello konfigureringen av hello krävs för kontrollen. Exempel är:

* Etablering av program mot skadlig kod toohelp identifiera och ta bort skadlig programvara
* Konfigurera [Nätverkssäkerhetsgrupper](../virtual-network/virtual-networks-nsg.md) och regler toocontrol trafik toovirtual datorer
* Etablering av en web application firewall toohelp skydd mot angrepp målobjekt för webbaserade program
* genomföra alla systemuppdateringar som fattas
* OS-konfigurationer som inte matchar hello-adressering rekommenderade baslinjer

Rekommendationer som är aktiverade i säkerhetsprinciper som visas här.

### <a name="how-can-i-see-hello-current-security-state-of-my-azure-resources"></a>Hur kan jag se hello aktuella säkerhetsläget för min Azure-resurser?
Hej **Center Säkerhetsöversikt** bladet visar hello övergripande säkerhetsläget i miljön uppdelat efter beräkning, nätverk, lagring och data och program. Varje resurs har en indikator som visar om alla potentiella säkerhetsproblem har identifierats. På varje sida visar en lista över säkerhetsfrågor som identifieras av Security Center, tillsammans med en förteckning över hello resurser i din prenumeration.

### <a name="what-triggers-a-security-alert"></a>Vad utlöser en säkerhetsvarning?
Azure Security Center automatiskt samlar in, analyseras och Smältsäkringar loggdata från Azure-resurser, hello nätverket och partnerlösningarna, till exempel program mot skadlig kod och brandväggar. Om hot upptäcks skapas en säkerhetsavisering. Exempel på hot:

* angripna virtuella datorer som kommunicerar med IP-adresser som man vet är skadliga
* Avancerad skadlig kod har identifierats med hjälp av Windows Felrapportering
* nyckelsökningsangrepp mot virtuella datorer
* Säkerhetsaviseringar från integrerade säkerhet partnerlösningar, till exempel mot skadlig kod eller Web Application brandväggar

### <a name="whats-hello-difference-between-threats-detected-and-alerted-on-by-microsoft-security-response-center-versus-azure-security-center"></a>Vad är hello skillnaden mellan hot upptäckts och ett meddelande om av Microsoft Security Response Center jämfört med Azure Security Center?
hello Microsoft Security Response Center (MSRC) utför väljer säkerhetsövervakning hello Azure-nätverk och infrastruktur och tar emot hot intelligence och missbruk klagomål från tredje part. När MSRC blir medveten om att kundinformation som har använts av en olagligt eller obehöriga part eller hello kundens användning av Azure är inte kompatibel med hello villkor för godkänd använder meddelar en incident säkerhetshanteraren hello kunden. Meddelande normalt genom att skicka ett e-toohello säkerhet kontakter som anges i Azure Security Center eller hello Azure-prenumeration ägare om en säkerhet kontakt inte har angetts.

Security Center är en Azure-tjänst som kontinuerligt övervakar hello kundens Azure-miljön och tillämpar analytics tooautomatically identifiera en mängd eventuellt skadlig aktivitet. Dessa identifieringar är anslutna som säkerhetsaviseringar i instrumentpanelen för hello Security Center.

### <a name="which-azure-resources-are-monitored-by-azure-security-center"></a>Vilka Azure-resurser övervakas av Azure Security Center?
Azure Security Center övervakar hello följande Azure-resurser:

* Virtuella datorer (VM) (inklusive [molntjänster](../cloud-services/cloud-services-choose-me.md))
* Azure Virtual Networks
* Azure SQL-tjänsten
* Azure-lagringskonto
* Azure-Webbappar (i [Apptjänstmiljö](../app-service/app-service-app-service-environments-readme.md))
* Partnerlösningar som är integrerad med din Azure-prenumeration, till exempel en brandvägg för webbaserade program på virtuella datorer och på [Apptjänstmiljö](../app-service/app-service-app-service-environments-readme.md)

## <a name="virtual-machines"></a>Virtuella datorer
### <a name="what-types-of-virtual-machines-are-supported"></a>Vilka typer av virtuella datorer stöds?
Övervakning och rekommendationer är tillgängliga för virtuella datorer (VM) som skapats med hjälp av både hello [klassisk och Resource Manager distributionsmodellerna](../azure-classic-rm.md).

Se [plattformar som stöds i Azure Security Center](security-center-os-coverage.md) en lista över plattformar som stöds.

### <a name="why-doesnt-azure-security-center-recognize-hello-antimalware-solution-running-on-my-azure-vm"></a>Varför inte Azure Security Center kan identifiera hello program mot skadlig kod som körs på den virtuella Azure-datorn?
Azure Security Center har insyn i program mot skadlig kod installeras via Azure-tillägg. Security Center är till exempel inte kan toodetect program mot skadlig kod som har förinstallerats på en bild som du har angett eller om du har installerat program mot skadlig kod på dina virtuella datorer med egna processer (till exempel konfigurationshanteringssystem).

### <a name="why-do-i-get-hello-message-missing-scan-data-for-my-vm"></a>Varför visas hello-meddelande ”söka Data som saknas” för den virtuella datorn?
Det här meddelandet visas när det finns inga genomsökningsdata för en virtuell dator. Det kan ta lite tid (mindre än en timme) för genomsökning data toopopulate när datainsamling har aktiverats i Azure Security Center. Efter hello inledande ifyllning av genomsökningsdata får detta felmeddelande eftersom det finns inga sökningsdata på alla eller inga senaste genomsökning data. Inte att fylla sökningar för en virtuell dator i ett stoppat tillstånd. Det här meddelandet kan också visas om genomsökningsdata inte har fyllts i nyligen (i enlighet med hello bevarandeprincipen för hello Windows-agenten som har ett standardvärde av 30 dagar).

### <a name="why-do-i-get-hello-message-vm-agent-is-missing"></a>Varför visas hello-meddelande ”VM-agenten är saknas”?
hello VM-agenten måste installeras på virtuella datorer tooenable insamling av Data. hello VM-agenten installeras som standard för virtuella datorer som distribueras från hello Azure Marketplace. Information om hur tooinstall hello VM-agenten på andra virtuella datorer finns i bloggposten hello [VM-Agent och tillägg för](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/).
