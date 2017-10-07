---
title: "aaaGetting igång med Microsoft Azure-säkerhet | Microsoft Docs"
description: "Den här artikeln innehåller en översikt över säkerhetsfunktionerna i Microsoft Azure och allmänna överväganden för organisationer som migrerar providerns tillgångar tooa moln."
services: security
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: TomSh
ms.assetid: 8d8a0088-c85a-48e7-bd04-2bc7b78b0691
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/09/2017
ms.author: yurid
ms.openlocfilehash: c61dd99ffd0143bc7b165367dadadc977ffcf47b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-microsoft-azure-security"></a>Kom igång med Microsoft Azure-säkerhet
När du skapar eller migrerar IT tillgångar tooa molntjänstleverantör, måste den organisationens förmåga tooprotect dina program och data med hello tjänster och hello kontroller ger toomanage hello säkerheten för din molnbaserade tillgångar.

Azures infrastruktur har utformats från hello anläggning tooapplications som värd för miljontals kunder samtidigt och det ger en säker grund som företag uppfyller deras behov. Dessutom Azure ger dig en mängd olika konfigurerbara säkerhet alternativ och hello möjlighet toocontrol dem så att du kan anpassa toomeet hello unika säkerhetskrav för dina distributioner.

I den här översiktsartikeln om Azure-säkerhet ska vi titta på följande:

* Azure-tjänster och funktioner som du kan använda toohelp skydda dina data i Azure och tjänster.
* Hur Microsoft skyddar hello Azure-infrastrukturen toohelp skydda dina data och program.

## <a name="identity-and-access-management"></a>Identitets- och åtkomsthantering
Det är viktigt att kontrollera åtkomst tooIT infrastruktur, data och program. Microsoft Azure har funktioner för dessa tjänster som Azure Active Directory (Azure AD), Azure Storage och stöd för flera standarder och API: er.

[Azure AD](../active-directory/active-directory-whatis.md) är en identitetsdatabas och motor som tillhandahåller autentisering, auktorisering och åtkomstkontroll för organisationens användare, grupper och objekt. Azure AD också erbjuder utvecklare ett effektivt sätt toointegrate Identitetshantering i sina program. Standardiserade protokoll som [SAML 2.0](https://en.wikipedia.org/wiki/SAML_2.0), [WS-Federation](https://msdn.microsoft.com/library/bb498017.aspx), och [OpenID Connect](http://openid.net/connect/) gör möjligt logga in på plattformar som .NET, Java, Node.js och PHP.

Hej REST-baserad Graph API kan utvecklare tooread och skriva toohello directory från valfri plattform. Med stöd för [OAuth 2.0](http://oauth.net/2/)utvecklare kan bygga mobila och webbprogram som integrerar med Microsoft och tredje parts webb-API: er och skapa egna säker web API: er. Klientbibliotek med öppen källkod är tillgängliga för .Net, Windows Store, iOS och Android, och ytterligare bibliotek är under utveckling.

### <a name="how-azure-enables-identity-and-access-management"></a>Möjligheter till identitets- och åtkomsthantering i Azure
Azure AD kan användas som en fristående molnkatalog för din organisation eller som en integrerad lösning med en befintlig lokal Active Directory. Bland integrationsfunktionerna finns katalogsynkronisering och enkel inloggning (SSO). Dessa utökar hello räckvidden för din befintliga lokala identiteter till molnet hello och förbättra hello admin och användarupplevelse.

Vissa andra funktioner för identitets- och åtkomsthantering är följande:

* Azure AD-aktiverar [SSO](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/) tooSaaS program, oavsett var de finns. Vissa program federeras med Azure AD och andra använder enkel inloggning med lösenord. Federerade program kan också använda användareetablering och valvlagring av lösenord.
* Åtkomst toodata i [Azure Storage](https://azure.microsoft.com/services/storage/) styrs via autentisering. Varje lagringskonto har en primär nyckel ([lagringskontonyckel](https://msdn.microsoft.com/library/azure/ee460785.aspx), eller SAK) och en sekundär hemlig nyckel (signatur för delad åtkomst i hello eller SAS).
* Azure AD innehåller identitet som en tjänst via federation med hjälp av [Active Directory Federation Services](../active-directory/fundamentals-identity.md), synkronisering och replikering med lokala kataloger.
* [Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication.md) är hello Multi-Factor authentication-tjänsten som kräver att användare tooverify inloggningar med hjälp av en mobilapp, telefonsamtal eller SMS. Den kan användas med Azure AD toohelp säker lokala resurser med hello Azure Multi-Factor Authentication-servern, och anpassade program och genom att ange hello SDK.
* [Azure AD Domain Services](https://azure.microsoft.com/services/active-directory-ds/) kan du ansluta till virtuella Azure-datorer tooa domän utan att distribuera domänkontrollanter. Du kan logga in toothese virtuella datorer med företagets Active Directory-autentiseringsuppgifter och administrera domänanslutna virtuella datorer med hjälp av Grupprincip tooenforce baslinjer för säkerhet på alla dina virtuella Azure-datorer.
* [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) ger en hög tillgänglighet globala identity management-tjänst för konsumentinriktade program som kan skalas toohundreds miljoner identiteter. Den kan integreras över mobila plattformar och webbaserade plattformar. Dina användare kan logga in tooall programmen via anpassningsbara upplevelser genom att använda sina befintliga sociala konton eller genom att skapa nya autentiseringsuppgifter.

## <a name="data-access-control-and-encryption"></a>Dataåtkomstkontroll och kryptering
Microsoft använder hello principerna för uppdelning av uppgifter och [minsta privilegium](https://en.wikipedia.org/wiki/Principle_of_least_privilege) i hela åtgärder i Azure. Åtkomst toodata Azure supportpersonalen kräver din explicit behörighet och beviljas på grundval ”just-in-time” som är inloggade och granskas sedan återkallas efter hello engagement har slutförts.

Azure tillhandahåller också flera funktioner för att skydda data under överföring och i vila. Här ingår kryptering för data, filer, program, tjänster, kommunikation och enheter. Du kan kryptera information innan du placerar den i Azure och lagrar också nycklar i ditt lokala datacenter.

![Microsoft Antimalware i Azure](./media/azure-security-getting-started/sec-azgsfig1.PNG)

### <a name="azure-encryption-technologies"></a>Azure-krypteringstekniker
Du kan samla in information om administrativ åtkomst tooyour prenumerationsmiljö med hjälp av [Azure AD Reporting](../active-directory/active-directory-reporting-audit-events.md). Du kan konfigurera [BitLocker-diskkryptering](https://technet.microsoft.com/library/cc732774.aspx) på virtuella hårddiskar som innehåller känslig information i Azure.

Andra funktioner i Azure som hjälper dig att skydda dina data tookeep omfattar:

* Programutvecklare kan bygga kryptering i hello-program som de distribueras i Azure med hjälp av Windows hello [CryptoAPI](https://msdn.microsoft.com/library/ms867086.aspx) och .NET Framework.
* Helt styra hello nycklar med kryptering på klientsidan för Azure Blob storage. hello lagringstjänsten aldrig ser hello nycklar och inte kan dekryptera hello data.
* [Azure Rights Management (Azure RMS)](https://technet.microsoft.com/library/jj585026.aspx) (med hello [RMS SDK](https://msdn.microsoft.com/library/dn758244.aspx)) ger fil- och datanivå kryptering och dataläcka förebyggande till policy-baserad hantering.
* Azure stöder [tabell-nivå och på kolumnnivå kryptering (TDE/r)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/05/12/recommendations-for-using-cell-level-encryption-in-azure-sql-database.aspx) i SQL Server-datorer och den stöder från tredje part lokala nyckelhantering servrar i datacenter.
* Lagringskontonycklar, signaturer för delad åtkomst, certifikat och andra nycklar är unika tooeach Azure-klient.
* Azure [StorSimple](http://www.microsoft.com/server-cloud/products/storsimple/overview.aspx) hybrid storage krypterar data via en 128-bitars offentliga/privata nyckelparet innan den skickas tooAzure lagring.
* Azure stöder och använder flera krypteringsmekanismer för, inklusive SSL/TLS, IPsec och AES, beroende på hello-datatyper, behållare och transporter.

## <a name="virtualization"></a>Virtualisering
hello Azure-plattformen använder en virtualiserad miljö. Användarinstanser fungerar som fristående virtuella datorer som inte har åtkomst till tooa fysiska värdserver och denna isolering tillämpas med hjälp av fysiska [behörighetsnivåer för processor (ring-0/ring-3)](https://en.wikipedia.org/wiki/Protection_ring).

Ring 0 är hello mest Privilegierade och 3 är hello minst. hello gästoperativsystemet som körs i en mindre privilegierad Ring 1 och program som körs i hello lägsta möjliga Ring 3. Den här virtualisering av fysiska resurser leder tooa klar åtskillnad mellan gäst OS- och hypervisor, vilket resulterar i ökad säkerhet åtskillnad mellan hello två.

hello Azure hypervisor fungerar som en micro kernel och skickar alla maskinvara förfrågningar från gästen virtuella datorer toohello värden för bearbetning med hjälp av ett delat minne gränssnitt som heter VMBus. Detta hindrar användare från att erhålla rådata Läs/Skriv/kör åtkomst toohello system och minskar hello risken för delning av systemresurser.

![Microsoft Antimalware i Azure](./media/azure-security-getting-started/sec-azgsfig2.PNG)

### <a name="how-azure-implements-virtualization"></a>Hur virtualisering implementeras i Azure
Azure använder en hypervisor-brandvägg (paketfilter) som är implementerad i hello hypervisor och konfigureras av en agent för fabric-styrenhet. Det skyddar klienter från obehörig åtkomst. Som standard all trafik blockeras när en virtuell dator skapas och hello fabric controller agenten konfigurerar sedan hello paket filter tooadd *regler och undantag* tooallow behörighet trafik.

Det finns två typer av regler som är programmerade här:

* **Datorn konfiguration eller infrastruktur regler**: som standard all kommunikation är blockerad. Det finns undantag tooallow toosend för en virtuell dator och ta emot DHCP och DNS-trafik. Virtuella datorer kan också skicka trafik toohello ”offentliga” internet och skicka trafik tooother virtuella datorer i hello klustret och hello OS Aktiveringsservern. hello virtuella datorer listan över tillåtna utgående mål innehåller inte Azure router undernät, Azure management tillbaka slutet och andra Microsoft-egenskaper.
* **Rollen konfigurationsfilen**: Detta definierar hello inkommande åtkomstkontrollistor (ACL) baserat på hello klient modell. Till exempel om en klient har en frontwebb på port 80 på en viss virtuell dator, Azure öppnas TCP-port 80 tooall IP-adresser om du konfigurerar en slutpunkt i hello [Azure klassiska distributionsmodellen](../azure-resource-manager/resource-manager-deployment-model.md). Om hello virtuella datorn har en bakre end eller worker roll som körs, och sedan öppnas hello worker-rollen toohello virtuell dator med enbart inom hello klient samma.

## <a name="isolation"></a>Isolering
Ett annat viktigt molnet säkerhetskrav är uppdelning tooprevent obehörig och oavsiktliga överföring av information mellan distributioner i en delad arkitektur med flera innehavare.

Azure implementerar [nätverk åtkomstkontroll](https://azure.microsoft.com/blog/network-isolation-options-for-machines-in-windows-azure-virtual-networks/) och ansvarsfördelning via VLAN-isolering, ACL: er och belastningsutjämning och IP-filter. Den begränsar externa trafiken inkommande tooports och protokoll på virtuella datorer som du definierar. Azure implementerar nätverkstrafik tooprevent falska filtrering och begränsa inkommande och utgående trafik tootrusted platform-komponenter. Trafikflödesprinciper implementeras på gränsskyddsenheter som nekar trafik som standard.

![Microsoft Antimalware i Azure](./media/azure-security-getting-started/sec-azgsfig3.PNG)

NAT (Network Address Translation) används tooseparate interna nätverkstrafiken från externa trafiken. Intern trafik kan inte omdirigeras externt. [Virtuella IP-adresser](http://blogs.msdn.com/b/cloud_solution_architect/archive/2014/11/08/vips-dips-and-pips-in-microsoft-azure.aspx) som är externt dirigerbara översätts till [interna dynamiska IP](http://blogs.msdn.com/b/cloud_solution_architect/archive/2014/11/08/vips-dips-and-pips-in-microsoft-azure.aspx)-adresser som enbart är dirigerbara i Azure.

Externa trafiken tooAzure virtuella datorer är med en brandvägg via ACL: er på routrar, belastningsutjämnare och nivå 3-växlar. Enbart vissa kända protokoll är tillåtna. ACL: er är i drift toolimit trafik från virtuella gästdatorer tooother VLAN som används för hantering. Dessutom kan filtreras trafik via IP-filter på hello värdens operativsystem ytterligare begränsningar hello-trafik på båda datalager för länk och nätverk.

### <a name="how-azure-implements-isolation"></a>Hur Azure implementerar isolering
hello Azure-Infrastrukturkontrollanten ansvarar för att tilldela infrastrukturresurser tootenant arbetsbelastningar och den hanterar enkelriktad kommunikation från hello toovirtual värddatorerna. hello Azure hypervisor tvingar minne och processen åtskillnad mellan virtuella datorer och dirigerar säkert nätverk trafik tooguest OS klienter. Azure implementerar också isolering för klienter, lagring och virtuella nätverk.

* Varje Azure AD-klient är logiskt isolerad med hjälp av säkerhetsgränser.
* Azure storage-konton är unika tooeach prenumeration och åtkomst måste autentiseras med hjälp av en lagringskontonyckel.
* Virtuella nätverk är logiskt isolerade genom en kombination av unika privata IP-adresser, brandväggar och IP-ACL: er. Belastningsutjämnare vidarebefordra trafik toohello lämplig klienter baserat på slutpunktsdefinitionerna.

## <a name="virtual-networks-and-firewalls"></a>Virtuella nätverk och brandväggar
Hej [distribuerade och virtuella nätverk](http://download.microsoft.com/download/4/3/9/43902EC9-410E-4875-8800-0788BE146A3D/Windows%20Azure%20Network%20Security%20Whitepaper%20-%20FINAL.docx) i Azure hjälp att se till att privata nätverkstrafiken är logiskt isolerade från trafik på andra virtuella Azure-nätverk.

![Microsoft Antimalware i Azure](./media/azure-security-getting-started/sec-azgsfig4.PNG)

Din prenumeration kan innehålla flera isolerade privata nätverk (och inkluderar brandväggen, belastningsutjämning och nätverksadresser).

Azure tillhandahåller tre primära nivåer av nätverket särskiljning i varje Azure kluster toologically åtskild trafik. [Virtuella lokala nätverk](https://azure.microsoft.com/services/virtual-network/) (VLAN) används tooseparate kunden trafik från hello resten av hello Azure-nätverk. Åtkomst toohello Azure-nätverk från utanför hello-kluster är begränsad till belastningsutjämnare.

Nätverket trafik tooand från virtuella datorer måste passera hello hypervisor virtuell växel. hello IP-filterkomponent i hello rot OS isolerar hello rot virtuell dator från hello virtuella gästdatorer och hello gäst virtuella datorer från varandra. Utför filtrering av trafik toorestrict kommunikation mellan en klients noder och hello offentliga Internet (baserat på hello kundens tjänstkonfiguration), skilja dem från andra klienter.

hello IP-filter förhindrar virtuella gästdatorer från:

* Generera falska trafik.
* Ta emot trafik inte åtgärdas toothem.
* Dirigera trafik tooprotected infrastruktur slutpunkter.
* Skicka eller ta emot olämpliga broadcast-trafik.

Du kan placera de virtuella datorerna till [virtuella Azure-nätverk](https://azure.microsoft.com/documentation/services/virtual-network/). Dessa virtuella nätverk är liknande toohello nätverk som du konfigurerar i lokala miljöer där de är vanligtvis kopplad till en virtuell växel. Virtuella datorer som är anslutna toohello samma virtuella nätverk kan kommunicera med varandra utan ytterligare konfiguration. Du kan också konfigurera olika undernät i det virtuella nätverket.

Du kan använda hello följa Azure Virtual Network tekniker toohelp säker kommunikation på det virtuella nätverket:

* [**Nätverkssäkerhetsgrupper (NSG: er)**](../virtual-network/virtual-networks-nsg.md). Du kan använda en NSG toocontrol trafik tooone eller flera instanser för virtuella datorer i ditt virtuella nätverk. En NSG innehåller åtkomstkontrollregler som tillåter eller nekar trafik baserat på trafikriktningen, protokollet, källadressen och -porten, samt måladressen och -porten.
* [**Användardefinierade routning**](../virtual-network/virtual-networks-udr-overview.md). Du kan styra hello routning av paket via en virtuell installation genom att skapa användardefinierade vägar som anger hello next hop för paket som flödar tooa undernätverk toogo tooa virtuella säkerhet nätverksenhet.
* [**IP-vidarebefordran**](../virtual-network/virtual-networks-udr-overview.md). En virtuell nätverksenhet säkerhet måste vara kan tooreceive inkommande trafik som inte är adresserade tooitself. tooallow en virtuell dator tooreceive trafik adresserad tooother mål kan du aktivera IP-vidarebefordring för hello virtuella datorn.
* [**Tvingad tunneltrafik**](../vpn-gateway/vpn-gateway-about-forced-tunneling.md). Tvingad tunneling kan du omdirigera eller ”force” alla Internet-bunden trafik som genereras av virtuella datorer på en virtuell tillbaka tooyour lokalt nätverksplats via en plats-till-plats VPN-tunnel för kontroll och granskning
* [**Slutpunkts-ACL:**](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). Du kan styra vilka datorer tillåts inkommande anslutningar från hello Internet tooa virtuella datorn på det virtuella nätverket genom att definiera endpoint ACL: er.
* [**Säkerhetslösningar för partnernätverk**](https://azure.microsoft.com/marketplace/). Det finns ett antal säkerhetslösningar från partnerföretag nätverk som du kan komma åt från hello Azure Marketplace.

### <a name="how-azure-implements-virtual-networks-and-firewalls"></a>Hur Azure implementerar virtuella nätverk och brandväggar
Azure implementerar filtrering av nätverkspaket brandväggar på alla värden och gästen virtuella datorer som standard. Windows OS-bilder från hello Azure Marketplace har också Windows-brandväggen är aktiverad som standard. Belastningsutjämnare på perimeternätverket hello Azure offentliga nätverk kontrollen kommunikation baserat på IP-ACL: er hanteras av kunden administratörer.

Om Azure flyttar data för en kund som en del av den ordinarie driften eller under en katastrofhändelse, sker detta i privata och krypterade kommunikationskanaler. Andra funktioner som används av Azure toouse i virtuella nätverk och brandväggar är:

* **Intern värdbrandväggen**: Azure Service Fabric och Azure Storage körs på ett enhetligt operativsystem som har inga hypervisor. Därför är hello windows-brandväggen konfigurerad med hello föregående två regeluppsättningar. Lagring körs interna toooptimize prestanda.
* **Värdbrandväggen**: hello värdbrandväggen är tooprotect hello värdoperativsystemet som kör hello hypervisor. hello regler är programmerade tooallow endast hello Service Fabric domänkontrollanten och hoppa rutorna tootalk toohello värdens operativsystem på en viss port. hello andra undantag är tooallow svaret från DHCP- och DNS-svar. Azure använder en konfigurationsfil för datorn som innehåller hello mall av brandväggsregler för hello värdens operativsystem. hello-värden är skyddat från externa angrepp av Windows konfigurerad brandvägg toopermit kommunikation endast från kända, autentiserad källor.
* **Gästen brandväggen**: replikerar hello regler i hello virtuella växel paketfilter men programmerade i olika program (till exempel hello Windows-brandväggen typ av hello gästoperativsystemet). hello gäst virtuella brandväggen kan vara konfigurerade toorestrict kommunikation tooor från hello virtuella datorgästen, även om hello kommunikation tillåts av konfigurationer i hello värden IP-Filter. Exempelvis kan du välja toouse hello virtuella brandväggen toorestrict gästkommunikation mellan två ditt Vnet som har konfigurerats tooconnect tooone en annan.
* **Storage-brandväggen (VB)**: hello brandväggen på hello lagring klientdelen filtrerar trafik toobe endast på portarna 80/443 och andra nödvändiga verktyget portar. hello-brandväggen på hello lagring serverdel begränsar kommunikationen toocome endast från frontservrar för lagring.
* **Virtuell nätverksgateway**: hello [Azure virtuell nätverksgateway](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) fungerar som hello mellan lokala gateway ansluter dina arbetsbelastningar i Azure Virtual Network tooyour lokala platser. Det är obligatoriskt tooconnect tooon lokala platser via [IPSec-tunnlar för plats-till-plats-VPN-](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md), eller via [ExpressRoute](../expressroute/expressroute-introduction.md) kretsar. För IPsec/IKE VPN-tunnlar hello gateways utföra IKE-handskakningar på och upprätta hello IPsec S2S VPN-tunnlar mellan hello virtuella nätverk och lokala platser. Virtuella nätverksgatewayerna också avsluta [punkt-till-plats VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md).

## <a name="secure-remote-access"></a>Säker fjärråtkomst
Data som lagras i molnet hello måste ha tillräckligt skydd aktiverat tooprevent kryphål och upprätthålla sekretess och integritet under överföringen. Detta omfattar nätverkskontroller som integreras med en organisations principbaserade, granskningsbara identitets- och åtkomsthanteringsmekanismer.

Inbyggt kryptografisk teknik kan du tooencrypt kommunikation inom och mellan distributioner mellan Azure-regioner och från Azure tooon lokala datacenter. Administratören åtkomst toovirtual datorer via [fjärrskrivbordssessioner](../virtual-machines/windows/classic/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json), [fjärransluten Windows PowerShell](http://blogs.technet.com/b/heyscriptingguy/archive/2013/09/07/weekend-scripter-remoting-the-cloud-with-windows-azure-and-powershell.aspx), och hello Azure-portalen krypteras alltid.

toosecurely Utöka ditt lokala datacenter toohello molnet, Azure tillhandahåller både [plats-till-plats VPN](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md) och [punkt-till-plats VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md), plus dedicerade länkar med [ExpressRoute](../expressroute/expressroute-introduction.md)(anslutningar tooAzure krypteras virtuella nätverk via VPN).

### <a name="how-azure-implements-secure-remote-access"></a>Hur Azure implementerar säker fjärråtkomst
Anslutningar toohello Azure-portalen måste alltid autentiseras och de kräver SSL/TLS. Du kan konfigurera hantering av certifikat tooenable säker hantering. Branschstandard säkerhet protokoll som [SSTP](https://technet.microsoft.com/magazine/2007.06.cableguy.aspx) och [IPsec](https://en.wikipedia.org/wiki/IPsec) stöds fullt ut.

Med [Azure ExpressRoute](../expressroute/expressroute-introduction.md) kan du skapa privata anslutningar mellan Azures datacenter och infrastruktur som finns lokalt eller i en samplaceringsmiljö. ExpressRoute-anslutningar inte överskrider hello offentliga Internet. De erbjuder flera tillförlitlighet, högre hastighet, lägre latens och högre säkerhet än vanliga Internetbaserade länkar. I vissa fall kan överföra data mellan lokala platser och Azure med hjälp av ExpressRoute-anslutningar kan också ge betydande kostnadsfördelar.

## <a name="logging-and-monitoring"></a>Loggning och övervakning
Azure tillhandahåller autentiserade loggning av säkerhetsmässigt händelser som genererar en granskningslogg och det är bakåtkompilerade toobe motståndskraftiga tootampering. Detta inkluderar Systeminformation, till exempel säkerhetshändelseloggar i Azure-infrastrukturen virtuella datorer och Azure AD. Övervakning av säkerhet omfattar att samla in händelser, t.ex ändringar i DHCP- eller DNS-serveradresser; försök till åtkomst tooports, protokoll eller IP-adresser som har blockerats avsiktligt; ändringar i principen eller brandvägg säkerhetsinställningar. kontot eller grupp att skapa; och oväntade processer eller installation av drivrutiner.

![Microsoft Antimalware i Azure](./media/azure-security-getting-started/sec-azgsfig5.PNG)

Granskningsloggar som registrerar åtkomst och aktiviteter för behöriga användare, behöriga och obehöriga åtkomstförsök, systemundantag och informationssäkerhetshändelser bevaras under en angiven tidsperiod. hello kvarhållning av loggarna är önskar eftersom du konfigurera Logginsamling och egna krav på datalagring tooyour.

### <a name="how-azure-implements-logging-and-monitoring"></a>Hur Azure implementerar loggning och övervakning
Azure distribuerar Management Agents (MA) och Övervakaren för Azure-säkerhet (ASM) agenter tooeach beräkning, lagring eller fabric-noden under hantering om de är inbyggda eller virtuell. Varje hanteringsagenten är konfigurerade tooauthenticate tooa service-teamet storage-konto med ett certifikat som hämtades från hello Azure certifikatarkiv och vidarebefordra förkonfigurerade diagnostik och händelsen data toohello storage-konto. Dessa agenter är inte distribuerad toocustomers virtuella datorer.

Azure-administratörer använda loggar via en webbportal för autentiserade och kontrollerad åtkomst toohello loggar. En administratör kan parsa, filtrera, korrelera och analysera loggar. hello Azure service-teamet storage-konton förhindra för loggar skyddas från direkt administratören åtkomst toohelp mot manipulation av loggen.

Microsoft samlar in loggar från nätverksenheter med hello Syslog-protokollet och värdservrar för fjärrskrivbordssessioner med hjälp av Microsoft Audit Collection Services (ACS). Dessa loggar placeras i en databas som aviseringar om misstänkta händelser genereras. Hej administratör kan komma åt och analysera dessa loggar.

[Azure Diagnostics](https://msdn.microsoft.com/library/azure/gg433048.aspx) är en funktion i Azure som du kan använda toocollect diagnostikdata från ett program som körs i Azure. Detta är diagnostikdata för felsökning och felsökning, mäta prestanda, övervaka Resursanvändning, trafik analys, kapacitetsplanering och granskning. När hello diagnostiska data samlas in, kan det vara överförda tooan Azure storage-konto för persistence. Överföringar kan antingen schemaläggas eller på begäran.

## <a name="threat-mitigation"></a>Migrering av hot
Azure använder ett antal hot minskning metoder och processer tooprotect infrastruktur och tjänster i tillägg tooisolation, kryptering och filtrering. Dessa inkluderar interna kontroller och teknikerna toodetect och åtgärda avancerade hot, till exempel DDoS, eskalering och hello [OWASP topp 10](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project).

hello kontroller för informationssäkerhet och riskhantering Microsoft har i plats toosecure dess molninfrastruktur minska hello risken för säkerhetsincidenter. I hello inträffar en olycka hello säkerhet Incident Management (SIM) team i hello team för Microsoft Online Services för säkerhet och kompatibilitet (OSSC) är klar toorespond när som helst.

### <a name="how-azure-implements-threat-mitigation"></a>Hur Azure implementerar hotminskning
Azure har säkerhetsåtgärder plats tooimplement hot minskning och även toohelp kunder risken för potentiella hot i sina miljöer. hello sammanfattas följande lista hello hot minskning funktionerna i Azure:

* [Azure program mot skadlig kod](azure-security-antimalware.md) är aktiverad som standard på alla infrastrukturservrar. Du kan välja att den egna virtuella datorer.
* Microsoft underhåller kontinuerlig övervakning över servrar, nätverk och program toodetect hot och förhindra kryphål. Automatiserade aviseringar meddela administratörer om avvikande beteenden, vilket ger dem tootake korrigerande åtgärder i både interna och externa hot.
* Du kan distribuera säkerhetslösningar från tredje part i dina prenumerationer, till exempel web application brandväggar från [Barracuda](https://techlib.barracuda.com/ng54/deployonazure).
* Microsofts strategi toopenetration testet ”[Red Teaming](http://download.microsoft.com/download/C/1/9/C1990DBA-502F-4C2A-848D-392B93D9B9C3/Microsoft_Enterprise_Cloud_Red_Teaming.pdf)”, som omfattar Microsoft-säkerhetsproffs att angripa (icke-kund) live produktionssystem i Azure tootest försvar mot verkliga, avancerade , permanenta hot.
* Integrerad distribution system hantera hello distribution och installation av säkerhetskorrigeringar i hello Azure-plattformen.

## <a name="next-steps"></a>Nästa steg
[Azure Säkerhetscenter](https://azure.microsoft.com/support/trust-center/)

[Azure-säkerhetsteamets blogg](http://blogs.msdn.com/b/azuresecurity/)

[Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx)

[Active Directory-bloggen](http://blogs.technet.com/b/ad/)
