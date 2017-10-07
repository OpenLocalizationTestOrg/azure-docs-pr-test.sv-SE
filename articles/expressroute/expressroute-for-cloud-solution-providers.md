---
title: "aaaAzure ExpressRoute för Cloud Solution Providers | Microsoft Docs"
description: "Den här artikeln innehåller information för molntjänst-Providers som vill tooincorporate Azure-tjänster och ExpressRoute i sina erbjudanden."
documentationcenter: na
services: expressroute
author: richcar
manager: carmonm
editor: 
ms.assetid: f6c5f8ee-40ba-41a1-ae31-67669ca419a6
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/10/2016
ms.author: richcar
ms.openlocfilehash: 062ecbb5e461e4384b01c4ac478cab696b7ed398
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="expressroute-for-cloud-solution-providers-csp"></a>ExpressRoute för Cloud Solution Providers (CSP)
Microsoft tillhandahåller tjänster för Hyper-skala för traditionella återförsäljare och distributörer (CSP) toobe toorapidly kan etablera nya tjänster och lösningar för kunderna utan hello måste tooinvest utveckla dessa nya tjänster. tooallow hello Cloud Solution Providers (CSP) hello möjlighet toodirectly hantera dessa nya tjänster, erbjuder Microsoft-program och API: er som tillåter hello CSP toomanage Microsoft Azure-resurser åt dina kunder. En av resurserna är ExpressRoute. ExpressRoute kan hello CSP tooconnect befintliga resurser tooAzure kundtjänst. ExpressRoute är en hög hastighet privata kommunikationer länken tooservices i Azure. 

ExpresRoute består av ett par med kretsar för hög tillgänglighet som är bifogade tooa kund abonnemang och kan inte delas av flera kunder. Varje krets bör avslutas i en annan router toomaintain hello med hög tillgänglighet.

> [!NOTE]
> Det finns bandbredds- och anslutningsfunktioner i ExpressRoute, vilket innebär att stora/komplexa implementeringar kräver flera ExpressRoute-kretsar för en enda kund.
> 
> 

Microsoft Azure tillhandahåller ett växande antal tjänster som du erbjuder tooyour kunder.  toobest dra nytta av dessa tjänster kräver hello Använd ExpressRoute anslutningar tooprovide hög hastighet låg latens åtkomst toohello Microsoft Azure-miljön.

## <a name="microsoft-azure-management"></a>Microsoft Azure-hantering
Microsoft tillhandahåller CSP: er med API: er toomanage hello Azure kundprenumerationer genom att tillåta programmässiga integrering med egna system för tjänsten. Hanteringsfunktioner som stöds finns [här](https://msdn.microsoft.com/library/partnercenter/dn974944.aspx).

## <a name="microsoft-azure-resource-management"></a>Microsoft Azure-resurshantering
Beroende på hello avgör kontrakt som du har med kunden hur hello prenumeration ska hanteras. hello CSP kan hantera hello skapas direkt och underhåll av resurser eller hello kunden kan behålla kontrollen över hello Microsoft Azure-prenumeration och skapa hello Azure-resurser som de behöver. Om kunden hanterar hello skapandet av resurser i deras Microsoft Azure-prenumeration de ska använda en av två modellerna: modell ”Anslut via” eller ”direkt till” modell. Dessa modeller beskrivs i detalj i följande avsnitt hello.  

### <a name="connect-through-model"></a>Anslut via-modellen
![alternativ text](./media/expressroute-for-cloud-solution-providers/connect-through.png)  

I hello ansluta via modellen skapar hello CSP en direkt anslutning mellan ditt datacenter och kundens Azure-prenumeration. hello direkt anslutning görs med hjälp av ExpressRoute, ansluta dina nätverk med Azure. Kunden ansluter sedan tooyour nätverk. Det här scenariot kräver hello kunden passerar hello CSP nätverket tooaccess Azure-tjänster. 

Om kunden har andra Azure-prenumerationer som inte hanteras av skulle hello du använder de hello offentliga Internet eller egna privata tooconnect toothose anslutningstjänster etablerats under hello icke CSP prenumeration. 

För CSP hantera Azure-tjänster, antas det att hello CSP har en tidigare upprättad kunden identitet lagra som ska replikeras till Azure Active Directory för hantering av CSP prenumerationen via Administrate-On-Behalf-Of (AOBO). Viktiga faktorer för det här scenariot inkluderar där en viss partner eller en tjänstleverantör har en upprättad relation med hello kunden, hello kunden förbrukar providern tjänster för närvarande eller hello partnern har en önskan tooprovide en kombination av provider-värd och Azure-värdbaserade lösningar tooprovide flexibilitet och adress kundutmaningar som inte uppfylls av CSP enbart. Den här modellen illustreras i **figuren** nedan.

![alternativ text](./media/expressroute-for-cloud-solution-providers/connect-through-model.png)

### <a name="connect-toomodel"></a>Ansluta toomodel
![alternativ text](./media/expressroute-for-cloud-solution-providers/connect-to.png)

I hello Anslut toomodel hello-leverantör skapar en direkt anslutning mellan sina kundens datacenter och hello CSP etableras Azure-prenumeration med hjälp av ExpressRoute över hello kundens (kunden) nätverk.

> [!NOTE]
> För ExpressRoute skulle hello kunden behöver toocreate och underhålla hello ExpressRoute-kretsen.  
> 
> 

Det här scenariot anslutningen kräver hello kunden ansluter direkt till en kund nätverket tooaccess CSP-hanterad Azure-prenumeration, med hjälp av en nätverksanslutning som skapats, ägs och hanteras helt eller delvis av hello kunden. För dessa kunder förutsätts det att hello-providern inte har en kund Identitetslagret upprätta och hello providern bidrar till att hello kunden replikerar deras aktuella identifiera lagring till Azure Active Directory för hantering av deras prenumerationen via AOBO. Viktiga faktorer för det här scenariot omfattar där en viss partner eller en tjänstleverantör har en upprättad relation med hello kunden hello kunden förbrukar providern tjänster för närvarande eller hello partnern har en önskan tooprovide services som är baserade på Azure-värdbaserade lösningar utan hello måste för en befintlig providern datacenter eller infrastruktur.

![alternativ text](./media/expressroute-for-cloud-solution-providers/connect-to-model.png)

hello val mellan dessa två alternativ baseras på kundens behov och din aktuella måste tooprovide Azure services. hello information om dessa modeller och hello associerade rollbaserad åtkomstkontroll, nätverk, och identitet designmönster beskrivs i detalj i hello följande länkar:

* **Rollbaserad åtkomstkontroll (RBAC)** – RBAC baseras på Azure Active Directory.  Läs mer om Azure RBAC [här](../active-directory/role-based-access-control-configure.md).
* **Nätverk** – omfattar hello olika ämnen för nätverk i Microsoft Azure.
* **Azure Active Directory (AAD)** – AAD ger hello Identitetshantering för Microsoft Azure och 3 part SaaS-program. Mer information om Azure AD finns [här](https://azure.microsoft.com/documentation/services/active-directory/).  

## <a name="network-speeds"></a>Nätverkshastigheter
ExpressRoute stöder nätverkshastigheter från 50 Mb/s too10Gb s. Detta gör att kunder toopurchase hello mängden nätverksbandbredd som behövs för deras miljö.

> [!NOTE]
> Nätverkets bandbredd kan du öka vid behov utan att störa kommunikationen, men tooreduce hello nätverkshastigheten kräver går sönder ned hello krets och återskapa på hello lägre nätverkshastigheten.  
> 
> 

ExpressRoute stöder hello anslutning av flera Vnet tooa enda ExpressRoute-krets för bättre användning av hello snabbare anslutningar. En enda ExpressRoute-kretsen kan delas av flera Azure-prenumerationer som ägs av hello samma kund.

## <a name="configuring-expressroute"></a>Konfigurera ExpressRoute
ExpressRoute kan vara konfigurerade toosupport tre typer av trafik ([routningsdomänerna](#ExpressRoute-routing-domains)) via en enda ExpressRoute-krets. Den här trafiken är indelad i Microsoft-peering, offentlig Azure-peering och privat peering. Du kan välja en eller alla typer av trafik toobe skickas över en enskild ExpressRoute-krets eller använda flera ExpressRoute-kretsar beroende på hello storleken på hello ExpressRoute-krets och isolering som krävs av kunden. hello säkerhetstillståndet av kunden inte tillåter offentliga trafik och privat trafik tootraverse över hello samma krets.

### <a name="connect-through-model"></a>Anslut via-modellen
I en Anslut via konfiguration hello ansvarar du för alla hello nätverk grunder tooconnect dina kunder datacenter resurser toohello prenumerationer finns i Azure. Var och en av kundens som vill toouse funktioner i Azure kan ha sina egna ExpressRoute-anslutning som ska hanteras av hello du. hello används hello tooprocure hello ExpressRoute-kretsen använder samma metoder hello kund. hello du ska följa samma steg som beskrivs i artikel hello hello [ExpressRoute-arbetsflöden](expressroute-workflows.md) kretsetablering och kretsstatus. hello konfigurerar du sedan hello Border Gateway Protocol (BGP) vägar toocontrol hello trafiken flödar mellan hello lokala nätverk och Azure vNet.

### <a name="connect-toomodel"></a>Ansluta toomodel
I en Anslut tooconfiguration kunden redan har en befintlig anslutning tooAzure eller kommer att upprätta en anslutning toohello Internetleverantör länka ExpressRoute från datacentret kundens egna direkt tooAzure i stället för ditt datacenter. toobegin hello etableringsprocessen, kunden kommer åtgärderna hello enligt beskrivningen i hello Anslut via modellen ovan. När hello kretsen har upprättats måste kunden tooconfigure hello lokala routrar toobe kan tooaccess både ditt nätverk och Azure Vnet.

Du kan hjälpa till med att konfigurera hello anslutning och konfigurera hello dirigerar tooallow hello resurser i din datacenter(s) toocommunicate hello klienten resurser i ditt datacenter eller med hello resurser i Azure.

## <a name="expressroute-routing-domains"></a>ExpressRoute-routningsdomäner
ExpressRoute erbjuder tre routningsdomäner: offentliga, privata och Microsoft-peering. Var och en av hello routningsdomäner är konfigurerade med identiska routrar i aktiv-aktiv konfiguration för hög tillgänglighet. Mer information om ExpressRoute-routningsdomänerna finns [här](expressroute-circuit-peerings.md).

Du kan definiera anpassade vägarna filter tooallow endast hello route(s) du vill tooallow eller behöver. Mer information eller toosee hur toomake dessa ändringar finns i artikeln: [skapa och ändra routning för en ExpressRoute-krets med hjälp av PowerShell](expressroute-howto-routing-classic.md) för mer information om routning filter.

> [!NOTE]
> För Microsoft och offentlig Peering anslutning men måste vara en offentlig IP-adress som ägs av hello kunden eller CSP och måste följa tooall definierade regler. Mer information finns i hello [ExpressRoute krav](expressroute-prerequisites.md) sidan.  
> 
> 

## <a name="routing"></a>Routning
ExpressRoute ansluter toohello Azure nätverk via hello Azure virtuell nätverksgateway. Nätverksgateways innehåller routning för Azures virtuella nätverk.

En standard-routningstabell för hello vNet toodirect trafik till och från hello undernät för hello vNet skapas automatiskt när du skapar virtuella Azure-nätverk. Om hello standard routningstabellen inte räcker för hello lösning anpassade vägar kan skapas tooroute utgående trafik toocustom utrustning eller tooblock dirigerar toospecific undernät eller externa nätverk.

### <a name="default-routing"></a>Standardroutning
hello standard routningstabellen innehåller hello följande vägar:

* Routning i ett undernät
* Undernät till undernät inom hello virtuellt nätverk
* toohello Internet
* ”Virtuella nätverk-till-virtuella nätverk” via VPN-gateway
* ”Virtuella nätverk-till-lokala nätverk” via en VPN- eller ExpressRoute-gateway

![alternativ text](./media/expressroute-for-cloud-solution-providers/default-routing.png)  

### <a name="user-defined-routing-udr"></a>Användardefinierad routning
Användardefinierade vägar kan hello kontroll av trafiken utgående från hello tilldelade undernät tooother undernät i hello virtuellt nätverk eller över någon av hello andra fördefinierade gateways (ExpressRoute; internet eller VPN). routningstabellen för hello standard system kan ersättas med en användardefinierad routningstabell som ersätter hello standardroutningstabellen med anpassade vägar. Med användardefinierade routning, kunder skapa vägar tooappliances, till exempel brandväggar och intrångsidentifiering identifiering utrustning eller blockera åtkomst toospecific undernät från hello undernät som är värd för hello användardefinierad väg. En översikt över användardefinierade vägar finns [här](../virtual-network/virtual-networks-udr-overview.md). 

## <a name="security"></a>Säkerhet
Beroende på vilken modell används, Anslut tooor Connect-via kunden definierar hello säkerhetsprinciper i sina virtuella nätverk eller ger hello säkerhetskrav toohello CSP toodefine tootheir Vnet. hello följande kriterier för säkerhet kan definieras:

1. **Isolering av kunden** – hello Azure-plattformen ger kunden isolering genom att lagra information om kund-ID och virtuella nätverk i en säker databas som används tooencapsulate varje kund trafik i en GRE-tunnel.
2. **Network Security Group (NSG)** regler är för att definiera tillåts trafik till och från hello undernät i Vnet i Azure. Som standard innehåller hello NSG Block regler tooblock trafik från hello Internet toohello vNet och regler för trafik i ett virtuellt nätverk. Mer information om nätverkssäkerhetsgrupper finns [här](https://azure.microsoft.com/blog/network-security-groups/)
3. **Tvingad tunneltrafik** – detta är en alternativ tooredirect internet-bunden trafik i Azure toobe omdirigeras över hello ExpressRoute-anslutning toohello på lokala datacenter. Mer information om tvingad tunneltrafik finns [här](expressroute-routing.md#advertising-default-routes).  
4. **Kryptering** – även om hello ExpressRoute-kretsar dedikerade tooa viss kund, finns hello möjligheten att hello nätverksprovider kunde vara utsatts för intrång, vilket gör att en inkräktare tooexamine paket trafik. tooaddress denna möjlighet, kund eller CSP kan kryptera trafik över hello anslutningen genom att definiera principer för IPSec-tunnlar för all trafik som flödar mellan hello på lokala resurser och Azure-resurser (se toohello valfria tunnelläge IPSec för kunden 1 i bild 5: ExpressRoute säkerhet ovan). hello andra alternativet är toouse en brandväggsinstallation på varje hello slutpunkt hello ExpressRoute-kretsen. Detta kräver ytterligare 3 part brandväggen virtuella datorer/enheter toobe installerad på båda ändarna tooencrypt hello-trafik över hello ExpressRoute-kretsen.

![alternativ text](./media/expressroute-for-cloud-solution-providers/expressroute-security.png)  

## <a name="next-steps"></a>Nästa steg
hello leverantör service ger dig ett sätt tooincrease kunderna värdet tooyour utan hello behöver för dyra infrastruktur och kapaciteten inköp, samtidigt som din position som hello primära lägga ut provider. Sömlös integration med Microsoft Azure kan utföras via hello CSP-API, så att du toointegrate hantering av Microsoft Azure i din befintliga hanteringsramverk.  

Mer Information finns på hello följande länkar:

[Microsoft Cloud Solution Provider-program](https://partner.microsoft.com/en-US/Solutions/cloud-reseller-overview).  
[Hämta redo tootransact som en leverantör av Molnlösningar](https://partner.microsoft.com/en-us/solutions/cloud-reseller-pre-launch).  
[Microsoft Cloud Solution Provider-resurser](https://partner.microsoft.com/en-us/solutions/cloud-reseller-resources).

