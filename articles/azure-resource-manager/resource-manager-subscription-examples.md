---
title: "aaaScenarios och exempel för prenumerationen styrningen | Microsoft Docs"
description: "Innehåller exempel på hur tooimplement styrning av Azure-prenumeration för vanliga scenarier."
services: azure-resource-manager
documentationcenter: na
author: rdendtler
manager: timlt
editor: tysonn
ms.assetid: e8fbeeb8-d7a1-48af-804f-6fe1a6024bcb
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/03/2017
ms.author: rodend;karlku;tomfitz
ms.openlocfilehash: f750e834519c8e64f57f87e2067801feb38b5c29
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="examples-of-implementing-azure-enterprise-scaffold"></a>Exempel på att implementera kodskelett Azure enterprise
Det här avsnittet innehåller exempel på hur ett företag kan implementera hello rekommendationer för en [Azure enterprise kodskelett](resource-manager-subscription-governance.md). Den använder fiktiva företaget Contoso tooillustrate bästa praxis för vanliga scenarier.

## <a name="background"></a>Bakgrund
Contoso är ett globalt företag som tillhandahåller ange kedjan lösningar för kunder i allt från en ”programvara som tjänst” modell tooa paketerade modell distribueras lokalt.  De utvecklar programvara över hello jordglob med betydande Utvecklingscenter i Indien, hello USA och Kanada.

hello ISV-delen av hello företagets är indelade i flera oberoende affärsenheter som hanterar produkter i ett betydande företag. Varje affärsenhet har egna utvecklare, projektledare och arkitekter.

hello Enterprise teknik Services ETS affärsenhet ger centraliserade IT och hanterar flera datacenter där affärsenheter värd sina program. Tillsammans med hantering av hello datacenter, hello ETS organisation ger och hanterar centraliserad samarbete (till exempel e-post och webbplatser) och nätverk/telefoni tjänster. De kan också hantera kund-riktade arbetsbelastningar för mindre enheter som inte har operativa personal.

Hej efter personer som används i det här avsnittet:

* Dave är hello ETS Azure-administratör.
* Alice är Contosos Director för utveckling i hello ange kedjan affärsenhet.

Contoso måste toobuild line-of-business-app och en kund-riktade app. Beslutade toorun hello appar i Azure. Dave läser hello [normativ prenumeration styrning](resource-manager-subscription-governance.md) avsnittet, och är nu redo tooimplement hello rekommendationer.

## <a name="scenario-1-line-of-business-application"></a>Scenario 1: line-of-business-program
Contoso skapar en källa kod management system (BitBucket) toobe används för utvecklare hälsningsmeddelande.  hello program använder infrastruktur som en tjänst (IaaS) som värd för och består av webbservrar och en databasserver. Utvecklare får åtkomst till servrar i utvecklingsmiljöer, men de inte behöver komma åt toohello servrar i Azure. Contoso ETS önskar tooallow hello programmet ägare och team toomanage hello program. hello programmet är endast tillgängligt när Contosos företagsnätverk. Dave behöver tooset upp hello prenumerationen för det här programmet. hello prenumeration kommer också vara värd för andra developer-relaterade program i hello framtida.  

### <a name="naming-standards--resource-groups"></a>Namnge standards & resursgrupper
Dave skapar en prenumeration toosupport utvecklingsverktyg som är gemensamma för alla hello affärsenheter. Han måste toocreate beskrivande namn för hello prenumeration och resursgrupp (för programmet hello och hello nätverk). Han skapar hello efter prenumeration och resurs grupper:

| Objekt | Namn | Beskrivning |
| --- | --- | --- |
| Prenumeration |Contoso ETS DeveloperTools produktion |Stöder common utvecklingsverktygen |
| Resursgrupp |rgBitBucket |Innehåller hello programmet webbservern och databasserver |
| Resursgrupp |rgCoreNetworks |Innehåller hello virtuella nätverk och gateway för plats-till-plats-anslutning |

### <a name="role-based-access-control"></a>Rollbaserad åtkomstkontroll
När du har skapat sin prenumeration Dave vill tooensure som hello rätt team och programägarna kan komma åt sina resurser. Dave identifierar att varje grupp har olika krav. Han använder hello-grupper som har synkroniserats från Contosos lokala Active Directory (AD) tooAzure Active Directory och ger hello rätt nivå av åtkomst toohello team.

Dave tilldelar hello följande roller för hello prenumerationen:

| Roll | Tilldelade för| Beskrivning |
| --- | --- | --- |
| [Ägare](../active-directory/role-based-access-built-in-roles.md#owner) |Hanterade ID från Contosos AD |Detta ID kontrolleras med precis tid JIT-åtkomst via Contosos Identitetshantering verktyg och säkerställer att granskas fullständigt prenumeration ägare åtkomst. |
| [Säkerhetshantering](../active-directory/role-based-access-built-in-roles.md#security-manager) |Avdelningen för informationssäkerhet och riskhantering management |Den här rollen kan användare toolook på hello Azure Security Center och hello status för hello resurser. |
| [Nätverksdeltagare](../active-directory/role-based-access-built-in-roles.md#network-contributor) |Nätverk-teamet |Den här rollen kan Contosos nätverket team toomanage hello plats tooSite VPN och hello virtuella nätverk. |
| *Anpassad roll* |Programmet ägare |Dave skapar en roll som ger hello möjlighet toomodify resurser inom hello resursgrupp. Mer information finns i [anpassade roller i Azure RBAC](../active-directory/role-based-access-control-custom-roles.md) |

### <a name="policies"></a>Principer
Dave har följande krav för att hantera resurser i prenumerationen hello hello:

* Eftersom hello utvecklingsverktyg stöder utvecklare över hello world, vill inte han tooblock användare från att skapa resurser i en region. Dock måste han tooknow där resurserna skapas.
* Han är berörda med kostnader. Han vill därför tooprevent programägarna från att skapa onödigt dyra virtuella datorer.  
* Eftersom det här programmet har utvecklare på flera enheter, vill han tootag varje resurs med hello enhet och program företagsägaren. Med hjälp av dessa taggar debiterar ETS hello rätt team.

Han skapar hello följande [Resource Manager principer](resource-manager-policy.md):

| Fält | Verkan | Beskrivning |
| --- | --- | --- |
| location |Granska |Granska hello skapandet av hello resurser i en region |
| typ |Neka |Neka G-serien virtuella datorer med skapas |
| tags |Neka |Kräv program ägare tagg |
| tags |Neka |Kräv kostnaden center tagg |
| tags |Lägg till |Lägga till taggnamnet på **affärsenheten** och Taggvärde **ETS** tooall resurser |

### <a name="resource-tags"></a>Resurstaggar
Dave förstår att han måste toohave specifik information om hello faktura tooidentify hello kostnadsställe för hello BitBucket implementering. Dessutom vill Dave tooknow alla hello resurser som ETS äger.

Han lägger till följande hello [taggar](resource-group-using-tags.md) toohello resursgrupper och resurser.

| Taggnamn | Taggvärdet |
| --- | --- |
| ApplicationOwner |hello namnet på hello person som hanterar det här programmet. |
| CostCenter |hello kostnadsställe i hello-grupp som betalar för hello Azure-förbrukningen. |
| Affärsenheten |**ETS** (hello affärsenhet som associeras med hello) |

### <a name="core-network"></a>Kärnnätverket
hello Contoso ETS information informationssäkerhet och riskhantering management-teamet har granskat Daves föreslagna planera toomove hello programmet tooAzure. De vill tooensure hello programmet inte är exponeras toohello internet.  Dave har också developer-appar som hello framtida kommer att flyttas tooAzure. Dessa appar kräver offentliga gränssnitt.  toomeet dessa krav han ger både interna och externa virtuella nätverk och en network security toorestrict åtkomst.

Han skapar hello följande resurser:

| Resurstyp | Namn | Beskrivning |
| --- | --- | --- |
| Virtual Network |vnInternal |Används med hello BitBucket program och är ansluten via ExpressRoute tooContoso företagsnätverket.  Ett undernät (sbBitBucket) ger hello program med en specifik IP-adressutrymme. |
| Virtual Network |vnExternal |Tillgänglig för framtida program som kräver offentliga slutpunkter. |
| Nätverkssäkerhetsgrupp |nsgBitBucket |Garanterar att hello minimeras risken för angrepp på arbetsbelastningen genom att tillåta anslutningar endast på port 443 för hello undernät där programmet hello bor (sbBitBucket). |

### <a name="resource-locks"></a>Resurslås
Dave identifierar att hello anslutningen från Contosos företagsnätverket toohello internt virtuellt nätverk måste skyddas från wayward skript eller oavsiktlig borttagning.

Han skapar hello följande [resurslås](resource-group-lock-resources.md):

| Låstypen | Resurs | Beskrivning |
| --- | --- | --- |
| **CanNotDelete** |vnInternal |Förhindrar att användare tar bort hello virtuellt nätverk eller undernät, men förhindra inte hello lägga till nya undernät. |

### <a name="azure-automation"></a>Azure Automation
Dave har inget tooautomate för det här programmet. Även om han skapade ett Azure Automation-konto använda inte han den först.

### <a name="azure-security-center"></a>Azure Security Center
Contoso IT-tjänsthantering måste tooquickly identifiera och hantera hot. De vill också toounderstand vilka problem kan finnas.  

toofulfill dessa krav, Dave aktiverar hello [Azure Security Center](../security-center/security-center-intro.md), och ger åtkomst toohello Security Manager roll.

## <a name="scenario-2-customer-facing-app"></a>Scenario 2: kund-riktade app
hello business position inom hello ange kedjan affärsenhet har identifierats av olika affärsmöjligheter tooincrease interaktion med Contosos kunder med en kortet. Alice team måste skapa det här programmet och beslutar att Azure ökar deras möjlighet toomeet hello affärsbehov. Alice fungerar med Dave från ETS tooconfigure två prenumerationer för utveckling och drift av det här programmet.

### <a name="azure-subscriptions"></a>Azure-prenumerationer
Dave loggar i toohello Azure Enterprise Portal och ser hello ange kedjan avdelningen finns redan.  Det här projektet är hello första utvecklingsprojekt för hello ange kedjan team i Azure, identifierar Dave hello behovet av att ett nytt konto för Alice Utvecklingsteamet.  Han skapar hello ”R & D” konto för sitt team och tilldelar åtkomst tooAlice. Alice loggar in via hello Azure-portalen och skapar två prenumerationer: en toohold hello development servrar och en toohold hello produktionsservrar.  Hon följer hello som tidigare har upprättats namngivning standarder när du skapar hello följande prenumerationer:

| Använd prenumeration | Namn |
| --- | --- |
| Utveckling |SupplyChain ResearchDevelopment LoyaltyCard utveckling |
| Produktion |SupplyChain Operations LoyaltyCard produktion |

### <a name="policies"></a>Principer
Dave och Alice diskutera hello program och identifiera att det här programmet endast hanterar kunder i Nordamerika hello-region.  Alice och hennes team planera toouse Azure program-miljön och Azure SQL toocreate hello program. De kan behöva toocreate virtuella datorer under utveckling.  Anna vill tooensure som hennes utvecklare har de behöver tooexplore och undersöka problem utan dra i ETS hello-resurser.

För hello **development prenumeration**, de skapar hello följande princip:

| Fält | Verkan | Beskrivning |
| --- | --- | --- |
| location |Granska |Granska hello skapandet av hello resurser i en region. |

Begränsa de inte hello typ av sku som en användare kan skapa under utveckling och de behöver inte taggar för alla resursgrupper eller resurser.

För hello **produktion prenumeration**, de skapar hello följande principer:

| Fält | Verkan | Beskrivning |
| --- | --- | --- |
| location |Neka |Neka hello skapandet av resurser utanför hello USA datacenter. |
| tags |Neka |Kräv program ägare tagg |
| tags |Neka |Kräv avdelning taggen. |
| tags |Lägg till |Lägg till tagg tooeach resursgruppen som anger produktionsmiljön. |

De begränsar inte hello typ av sku som en användare kan skapa i produktion.

### <a name="resource-tags"></a>Resurstaggar
Dave förstår att han måste toohave specifik information rätt affärsgrupper för tooidentify hello för fakturering och ägare. Han definierar resurstaggar för resursgrupper och resurser.

| Taggnamn | Taggvärdet |
| --- | --- |
| ApplicationOwner |hello namnet på hello person som hanterar det här programmet. |
| Avdelning |hello kostnadsställe i hello-grupp som betalar för hello Azure-förbrukningen. |
| EnvironmentType |**Produktion** (även om hello prenumeration inkluderar **produktion** i hello namn, inklusive den här taggen möjliggör enkel identifiering när du tittar på resurser i hello-portalen eller på hello faktura.) |

### <a name="core-networks"></a>Core nätverk
hello Contoso ETS information informationssäkerhet och riskhantering management-teamet har granskat Daves föreslagna planera toomove hello programmet tooAzure. De vill tooensure som hello kortet program korrekt isoleras och skyddas i ett DMZ-nätverk.  toofulfill det här kravet Dave och Alice skapa ett externt virtuellt nätverk och en network security grupp tooisolate hello kortet programmet från hello Contoso företagsnätverk.  

För hello **development prenumeration**, de skapar:

| Resurstyp | Namn | Beskrivning |
| --- | --- | --- |
| Virtual Network |vnInternal |Fungerar hello Contoso kortet utvecklingsmiljö och är ansluten via ExpressRoute tooContoso företagsnätverket. |

För hello **produktion prenumeration**, de skapar:

| Resurstyp | Namn | Beskrivning |
| --- | --- | --- |
| Virtual Network |vnExternal |Värd hello kortet programmet och inte är anslutet direkt tooContoso har ExpressRoute. Koden skickas via deras källkoden system direkt toohello PaaS-tjänster. |
| Nätverkssäkerhetsgrupp |nsgBitBucket |Garanterar att hello minimeras risken för angrepp på arbetsbelastningen genom att bara tillåta inkommande kommunikation på TCP 443.  Contoso undersöker också med hjälp av en brandvägg för webbaserade program ger ytterligare skydd. |

### <a name="resource-locks"></a>Resurslås
Dave och Alice ger och bestäm tooadd resurslås på vissa hello viktiga resurser i hello miljö tooprevent oavsiktlig borttagning under ett hänga kod push.

De skapar hello följande Lås:

| Låstypen | Resurs | Beskrivning |
| --- | --- | --- |
| **CanNotDelete** |vnExternal |tooprevent personer från att ta bort hello virtuellt nätverk eller undernät. hello Lås inte hello lägga till nya undernät. |

### <a name="azure-automation"></a>Azure Automation
Alice och hennes Utvecklingsteamet har omfattande runbooks toomanage hello miljön för det här programmet. Hej runbooks tillåter för hello tillägg/borttagning av programmet hello-noder och andra DevOps-uppgifter.

toouse dessa runbooks kan [Automation](../automation/automation-intro.md).

### <a name="azure-security-center"></a>Azure Security Center
Contoso IT-tjänsthantering måste tooquickly identifiera och hantera hot. De vill också toounderstand vilka problem kan finnas.  

toofulfill kraven Dave aktiverar Azure Security Center. Han kontrollerar att hello Azure Security Center övervakar hello resurser och ger åtkomst toohello DevOps och säkerhet team.

## <a name="next-steps"></a>Nästa steg
* toolearn om hur du skapar Resource Manager-mallar finns [bästa praxis för att skapa mallar för Azure Resource Manager](resource-manager-template-best-practices.md).

