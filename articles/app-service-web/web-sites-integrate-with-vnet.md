---
title: "aaaIntegrate en app med ett virtuellt Azure-nätverk"
description: "Visar hur tooconnect en app i Azure App Service tooa ny eller befintlig Azure-nätverk"
services: app-service
documentationcenter: 
author: ccompy
manager: erikre
editor: cephalin
ms.assetid: 90bc6ec6-133d-4d87-a867-fcf77da75f5a
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: ccompy
ms.openlocfilehash: a93c504481400245b02220b541a008a7c874d10a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-your-app-with-an-azure-virtual-network"></a>Integrera din app med Azure-nätverk
Det här dokumentet beskriver hello Azure App Service virtuellt nätverk integrationsfunktionen och visar hur tooset den dig med appar i [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714). Om du inte känner till virtuella Azure-nätverk (Vnet) är en funktion som gör tooplace många av dina Azure-resurser i ett routeable-internet-nätverk som du styr åtkomst till. Dessa nätverk kan sedan vara anslutna tooyour lokalt nätverk med hjälp av en mängd olika tekniker för VPN. toolearn mer om virtuella Azure-nätverk, börja med hello information här: [Azure översikt över virtuella nätverk][VNETOverview]. 

hello Azure App Service har två typer. 

1. hello flera innehavare system som stöder hello fullständig uppsättning prissättning
2. hello App Service miljö (ASE) premium-funktioner som distribuerar i ditt VNet. 

Det här dokumentet går igenom VNet-integrering och inte Apptjänstmiljö. Om du vill toolearn mer om hello ASE funktionen måste börja med hello information här: [Apptjänstmiljö introduktion][ASEintro].

VNet-integrering ger din web app åtkomst tooresources i ditt virtuella nätverk, men ger inte privat åtkomst tooyour webbprogrammet från hello virtuella nätverket. Åtkomst till privata webbplatsen refererar toomaking appen endast tillgänglig från ett privat nätverk som från Azure-nätverk. Åtkomst till privata webbplatsen är bara tillgängligt med en ASE som konfigurerats med en intern belastning belastningsutjämnare (ILB). Börja med hello här artikeln för information om hur du använder en ILB ASE: [skapa och använda en ILB ASE][ILBASE]. 

Ett vanligt scenario där du vill använda för VNet-integrering är att aktivera åtkomst från din web app tooa databas eller en webbtjänst som körs på en virtuell dator i ditt virtuella Azure-nätverket. VNet-integration behöver inte tooexpose en offentlig slutpunkt för program på den virtuella datorn men kan använda hello privata icke internet dirigerbara adresser i stället. 

Hej VNet integrationsfunktionen:

* kräver en Standard, Premium eller isolerad priser plan 
* fungerar med klassiska eller Resource Manager VNet 
* stöder TCP och UDP
* fungerar med webb-, mobil- och API apps
* gör att en app tooconnect tooonly 1 VNet i taget
* Gör upp toofive Vnet toobe integrerad med i en Apptjänstplan 
* tillåter hello toobe för samma virtuella nätverk som används av flera appar i en Apptjänstplan
* stöder ett SLA för 99,9% på grund av toohello SLA på hello Gateway för virtuellt nätverk

Det finns vissa saker som VNet-Integration inte stöder inklusive:

* montera en enhet
* AD-integrering 
* NetBios
* åtkomst till privata webbplatsen

### <a name="getting-started"></a>Komma igång
Här följer några saker tookeep i åtanke innan du ansluter web app tooa virtuella nätverket:

* VNet-Integration fungerar endast med appar i en **Standard**, **Premium**, eller **isolerad** priser plan. Om du aktiverar hello och skala din Apptjänstplan tooan stöds inte förlora priser planera dina appar sina anslutningar toohello Vnet som de använder. 
* Om det virtuella nätverket målet redan finns måste den ha punkt-till-plats VPN aktiverad med en dynamisk routning gateway innan den kan vara anslutna tooan app. Om din gateway har konfigurerats med statisk routning, kan du inte aktivera punkt-till-plats virtuella privata nätverk (VPN).
* Hej VNet måste vara i hello samma prenumeration som din App Service Plan(ASP). 
* hello-appar som integreras med ett virtuellt nätverk använder hello DNS som har angetts för det virtuella nätverket.
* Som standard dirigerar apparna integrerande endast trafik i ditt VNet baserat på hello vägar som definieras i ditt VNet. 

## <a name="enabling-vnet-integration"></a>Aktivera integrering av virtuellt nätverk
Det här dokumentet fokuserar huvudsakligen på med hello Azure-portalen för VNet-integrering. tooenable VNet-integrering med din app med hjälp av PowerShell, följer du anvisningarna för hello här: [ansluta din app tooyour virtuellt nätverk med hjälp av PowerShell][IntPowershell].

Du har hello alternativet tooconnect din app tooa nytt eller befintligt virtuellt nätverk. Om du skapar ett nytt nätverk som en del av din integrering sedan dessutom toojust skapar hello VNet, en dynamisk routning gateway är förkonfigurerad för dig och punkt tooSite VPN är aktiverad. 

> [!NOTE]
> Konfigurera en ny virtuell nätverksintegration kan ta flera minuter. 
> 
> 

tooenable VNet-Integration, öppna appen inställningar och välj sedan nätverk. hello-användargränssnitt som öppnas erbjuder tre alternativ för nätverksfunktioner. Den här guiden är bara i VNet integrering men Hybridanslutningar och Apptjänstmiljöer beskrivs senare i det här dokumentet. 

Om din app inte är i rätt priser plan hello kan hello Användargränssnittet du tooscale din plan tooa högre prisnivå planen för ditt val.

![][1]

### <a name="enabling-vnet-integration-with-a-pre-existing-vnet"></a>Aktivera VNet-integrering med ett befintligt virtuellt nätverk
Hej VNet integrering Användargränssnittet kan du tooselect från en lista över ditt Vnet. hello klassiska Vnet indikera att de är exempel med hello ordet ”klassisk” i parenteser nästa toohello VNet namn. hello sorteras listan så att hello Resource Manager VNets visas först. I hello ser bilden nedan om du att endast en VNet kan väljas. Det finns flera orsaker till att ett virtuellt nätverk kan vara nedtonad inklusive:

* hello virtuella nätverk finns i en annan prenumeration som ditt konto har åtkomst till
* Hej VNet har inte punkt tooSite aktiverad
* Hej VNet har inte en dynamisk routning gateway

![][2]

tooenable integrering Klicka på hello VNet som du vill toointegrate med. När du har valt hello VNet startas automatiskt din app för hello ändringar tootake effekt. 

##### <a name="enable-point-toosite-in-a-classic-vnet"></a>Aktivera punkt tooSite i ett klassiskt virtuellt nätverk
Om ditt VNet har punkt tooSite eller inte har en gateway, har du tooset dig börjar. toodo för klassiska VNet, gå toohello [Azure-portalen] [ AzurePortal] och öppna hello lista över virtuella Networks(classic). Härifrån kan du klicka på hello nätverk toointegrate med och klicka på hello stort rutan under Essentials kallas VPN-anslutningar. Här kan du skapa toosite för plats-VPN och har även den skapa en gateway. När du går igenom hello punkt toosite gateway skapa upplevelse är ungefär 30 minuter innan den är klar. 

![][8]

##### <a name="enabling-point-toosite-in-a-resource-manager-vnet"></a>Aktivera punkt tooSite i en Resource Manager-VNet
tooconfigure en Resource Manager-VNet med en gateway och punkt tooSite du kan använda antingen PowerShell enligt beskrivningen här [konfigurerar en punkt-till-plats-anslutning tooa virtuellt nätverk med PowerShell] [ V2VNETP2S] eller använda hello Azure-portalen enligt beskrivningen här [konfigurera punkt-till-plats anslutning tooa VNet med hjälp av hello Azure-portalen][V2VNETPortal]. hello UI tooperform den här funktionen finns inte ännu. Observera att du måste toocreate certifikat för hello punkt tooSite konfiguration. Detta konfigureras automatiskt när du ansluter din WebApp toohello VNet. 

### <a name="creating-a-pre-configured-vnet"></a>Skapa ett förkonfigurerade virtuellt nätverk
Om du vill toocreate ett nytt virtuellt nätverk som har konfigurerats med en gateway och punkt-till-plats, och sedan hello nätverk UI apptjänsten hello kapaciteten toodo men endast för en Resource Manager VNet. Om du vill toocreate ett klassiskt virtuellt nätverk med en gateway och punkt-till-plats måste toodo detta manuellt via användargränssnittet för hello nätverk. 

toocreate en Resource Manager VNet via hello VNet integrering UI, välja **Skapa nytt virtuellt nätverk** och ange den:

* Namn för virtuellt nätverk
* Virtuellt Nätverksadressblock
* Undernätsnamn
* Adressblock för undernätet
* Adressblock för gateway
* Punkt-till-plats-Adressblock

Om du vill att den här VNet tooconnect tooany andra nätverk, bör du inte välja IP-adressutrymmen som överlappar med dessa nätverk. 

> [!NOTE]
> Hanteraren för filserverresurser VNet har skapats med en gateway tar cirka 30 minuter och för närvarande är inte integrerat hello VNet med din app. När ditt VNet har skapats med hello gateway behöver toocome tillbaka tooyour appen VNet integrering UI och välj ditt nya VNet.
> 
> 

![][3]

Azure Vnet skapas normalt i privata nätverksadresser. Funktionen dirigerar som standard hello VNet integrering alla trafiken för de IP-adressintervall i ditt VNet. hello privata IP-adressintervall är:

* -Detta är hello samma som 10.0.0.0 - 10.0.0.0/8 10.255.255.255
* -Detta är hello samma som 172.16.0.0 - 172.16.0.0/12 172.31.255.255 
* -Detta är hello samma som 192.168.0.0 - 192.168.0.0/16 192.168.255.255

Hej VNet-adressutrymmet måste toobe som anges i CIDR-notering. Om du är bekant med CIDR-notering, är en metod för att ange Adressblock med en IP-adress och ett heltal som representerar hello nätverksmasken. Överväg att 10.1.0.0/24 är 256 adresser och 10.1.0.0/25 skulle vara 128 adresser som en snabbreferens. En IPv4-adress med en /32 är bara 1 adress. 

Om du anger här hello DNS-serverinformation, har som angetts för din VNet. Du kan redigera den här informationen från hello VNet användarupplevelser efter att VNet har skapats. Om du ändrar hello DNS hello VNet, måste tooperform en åtgärd för synkronisering av nätverket.

När du skapar ett klassiska VNet med hello VNet integrering Användargränssnittet skapas ett VNet i hello samma resursgrupp som din app. 

## <a name="how-hello-system-works"></a>Så här fungerar hello system
Under hello omfattar den här funktionen bygger vidare på punkt-till-plats VPN-teknik tooconnect din app tooyour VNet. Appar i Azure App Service har en flera innehavare systemarkitektur, vilket utesluter etablerar en app direkt i ett VNet som görs med virtuella datorer. Vi begränsa network access toojust hello virtuella dator som är värd hello appen genom att bygga på punkt-till-plats-teknik. Åtkomst toohello nätverk är också begränsad på värdarna app så att dina appar kan endast komma åt hello nätverk som du konfigurerar dem tooaccess. 

![][4]

Om du inte har konfigurerat en DNS-server med ditt virtuella nätverk, behöver din app toouse IP-adresser tooreach resurs i hello virtuella nätverk. Kom ihåg att hello större fördelen med den här funktionen är att du kan använda toouse hello privata adresser i ditt privata nätverk när du använder IP-adresser. Om du ställer in din app in toouse offentliga IP-adresser för en av dina virtuella datorer kan du inte använder hello VNet integrationsfunktionen och kommunicerar via hello internet.

## <a name="managing-hello-vnet-integrations"></a>Hantera hello VNet integreringar
Hej möjlighet tooconnect och koppla bort tooa VNet är på en appnivå. Åtgärder som kan påverka hello VNet Integration över flera appar är på en ASP-nivå. Du kan visa information för ditt virtuella nätverk från hello-användargränssnitt som visas på hello app-nivå. De flesta hello samma information visas också på hello ASP nivå. 

![][5]

Du kan se om din app är anslutna tooyour VNet hello funktionen nätverksstatus sidan. Om din gateway för virtuellt nätverk avser oavsett orsak, skulle det här visas som inte ansluten. 

hello information som du har nu tillgängliga tooyou i hello app nivå VNet integrering Användargränssnittet är hello samma som hello detaljerad information som du får från hello ASP. Här följer dessa objekt:

* VNet - namn i den här länken öppnar hello virtuella Azure-nätverket UI
* Plats – detta återspeglar hello platsen för ditt VNet. Det är möjligt toointegrate med ett VNet i en annan plats.
* Det finns certifikatstatus - certifikat som används toosecure hello VPN-anslutning mellan hello appen och hello virtuella nätverk. Detta visar en test-tooensure som de är synkroniserade.
* Status för gateway - bör din gateway inte är tillgänglig oavsett orsak sedan din app kan inte komma åt resurser i hello virtuella nätverk. 
* VNet-adressutrymmet - detta är hello IP-adressutrymme för din VNet. 
* Punkt tooSite adressutrymme - detta är hello punkt toosite IP-adressutrymme för din VNet. Din app visar kommunikation som kommer från en av hello IP-adresser i det här adressutrymmet. 
* Platsen toosite adressutrymme - du kan använda plats tooSite VPN tooconnect VNet-tooyour lokala resurser eller tooother VNet. Du om har som konfigurerats sedan hello IP-adressintervall som definierats med att VPN-anslutning visas här.
* DNS-servrar – om du har DNS-servrar som konfigurerats med ditt VNet anges här.
* IP-adresser dirigeras toohello VNet - det finns en lista över IP-adresser som ditt VNet har definierats för och de adresserna som visas här. 

hello endast åtgärden som du kan vidta i hello app vyn för din VNet-integrering är toodisconnect appen från hello virtuella nätverk som den är ansluten till. den här helt enkelt klicka på Koppla från överst hello toodo. Den här åtgärden ändras inte ditt VNet. Hej VNet och dess konfiguration inklusive hello gateways ändras inte. Om du vill sedan toodelete ditt VNet, måste toofirst delete hello resurser i den inklusive hello gateways. 

hello App Service-Plan vy har ett antal ytterligare åtgärder. Det är också komma åt annorlunda än från hello app. tooreach hello ASP nätverk UI helt enkelt öppna din ASP UI och rulla ned. Det finns ett UI-element som kallas funktionen nätverksstatus. Den ger vissa mindre information kring VNet-Integration. Klicka på det här Gränssnittet öppnas hello nätverket funktionen Status Användargränssnittet. Om du klickar hello på ”Klicka här toomanage”,-användargränssnitt som visar hello VNet integreringar i den här ASP öppnas.

![][6]

hello är hello ASP bra tooremember när du tittar på hello platserna för hello Vnet som du integrerar med. När hello VNet är i en annan plats är det mycket troligt toosee svarstid problem. 

Hej Vnet som är integrerad med är en påminnelse på hur många Vnet dina appar är integrerade med i den här ASP och hur många du kan ha. 

toosee lägga till information för varje virtuella nätverk, klicka på hello VNet som du är intresserad av. Dessutom toohello information som har beskrivits tidigare, du kan också se en lista över hello appar i den här ASP som använder det virtuella nätverket. 

Sekretessprinciper finns tooactions det två primära åtgärder. hello är först hello möjlighet tooadd vägar som enheten trafik som lämnar din app i ditt VNet. andra hello-åtgärd är hello möjlighet toosync certifikat och nätverksinformation.

![][7]

**Routning** som tidigare nämnts är hello vägar som definieras i ditt virtuella nätverk som används för att dirigera trafik i ditt virtuella nätverk från din app. Det finns några användningsområden men där kunder vill toosend ytterligare utgående trafik från en app i hello VNet och att de har angetts för den här funktionen. Vad händer toohello trafik är in toohow hello kund konfigurerar sina virtuella nätverk. 

**Certifikat** hello certifikatstatus visar en kontroll som utförs av hello Apptjänst toovalidate att hello-certifikat som vi använder för hello VPN-anslutningen är fortfarande bra. När VNet-Integration aktiverad sedan finns om det här är hello första integration toothat VNet från alla appar i den här ASP en obligatorisk utbyte av certifikat tooensure hello säkerheten för hello-anslutning. Tillsammans med hello certifikat får vi hello DNS-konfiguration, vägar och andra liknande saker som beskriver hello nätverk.
Om dessa certifikat eller nätverksinformation ändras, måste tooclick ”Sync nätverk”. **Obs**: när du klickar på ”Sync nätverk” och sedan du orsaka ett kort avbrott i anslutningen mellan din app och ditt VNet. När appen inte startas leda hello förlust av anslutning till din plats toonot funktion korrekt. 

## <a name="accessing-on-premises-resources"></a>Åtkomst till lokala resurser
En av hello fördelarna med hello VNet integrationsfunktionen är att om ditt VNet är ansluten tooyour lokalt nätverk med en plats tooSite VPN dina appar kan ha åtkomst tooyour lokala resurser från din app. Även om du kan behöva tooupdate för den här toowork dirigerar din lokala VPN-gateway med hello för din plats tooSite IP-adressintervall. När hello plats tooSite VPN först ställa in hello skript används tooconfigure ska det konfigureras vägar inklusive tooSite för plats-VPN. Om du lägger till hello VPN för punkt tooSite när du har skapat din webbplats tooSite VPN måste tooupdate hello vägar manuellt. Information om hur toodo som varierar per gateway och som inte beskrivs här. 

> [!NOTE]
> Hej VNet integrationsfunktionen integrerar inte en app med ett virtuellt nätverk som har en ExpressRoute-Gateway. Även om hello ExpressRoute-Gateway har konfigurerats i [samexistens läge] [ VPNERCoex] hello VNet integrering inte fungerar. Om du behöver tooaccess resurser via en ExpressRoute-anslutning så att du kan använda en [Apptjänstmiljö][ASE], som körs i ditt VNet.
> 
> 

## <a name="pricing-details"></a>Prisinformation
Det finns några olika delarna som du bör vara medveten om när du använder hello VNet integrationsfunktionen priser. Det finns 3 relaterade kostnader toohello användningen av funktionen:

* ASP priser nivå krav
* Kostnader för överföring av data
* VPN-Gateway kostnader.

För att dina appar toobe kan toouse funktionen, behöver de toobe i en Standard eller Premium App Service-Plan. Du kan visa mer information om de här kostnaderna: [priser för Apptjänst][ASPricing]. 

På grund av toohello sätt punkt tooSite VPN hanteras kan du alltid har en avgift för utgående data via VNet integrering anslutningen även om hello VNet i hello samma datacenter. toosee vilka dessa tillägg är, ta en titt här: [Data Transfer prisinformation][DataPricing]. 

hello sista objektet är hello kostnaden för hello VNet gateways. Om du inte behöver hello gateways för något annat, till exempel plats tooSite VPN, sedan betalar du för integrationsfunktionen för gateways toosupport hello virtuella nätverk. Det finns information om de här kostnaderna: [priser för VPN-Gateway][VNETPricing]. 

## <a name="troubleshooting"></a>Felsökning
Medan hello-funktionen är enkel tooset upp det betyder inte din upplevelse att problem som är ledigt. Bör du stöter på är problem med åtkomst till det önskade slutpunkten vissa verktyg som du kan använda tootest anslutning från hello app-konsolen. Det finns två konsolen upplevelser som du kan använda. En från hello Kudu-konsolen och hello andra är hello-konsol som du kan nå i hello Azure-portalen. tooget toohello Kudu-konsolen från din app går tooTools Kudu ->. Detta är hello samma som för [sitename]. scm.azurewebsites.net. När öppnas bara gå toohello Debug konsolen fliken tooget toohello Azure portal värdbaserade konsolen sedan från din app går > tooTools-konsol. 

#### <a name="tools"></a>Verktyg
hello verktyg ping, nslookup och tracert fungerar inte via hello-konsolen på grund av toosecurity begränsningar. Det toofill hello void har två separata verktyg som lagts till. Vi lagt till ett verktyg som heter nameresolver.exe i ordning tootest DNS-funktioner. hello-syntaxen är:

    nameresolver.exe hostname [optional: DNS Server]

Du kan använda nameresolver toocheck hello värdnamn som din app är beroende av. Det här sättet kan du testa om du har något felaktigt konfigurerad med din DNS eller kanske inte har åtkomst till tooyour DNS-servern.

hello nästa verktyget kan du tootest för TCP anslutningen tooa värd och port kombination. Det här verktyget kallas tcpping.exe och hello syntaxen är:

    tcpping.exe hostname [optional: port]

Hej tcpping verktyget får du om du till en specifik värd och port. Det kan bara visa lyckas om: det finns ett program som lyssnar på hello-värd och port-kombination och det finns nätverksåtkomst från din app toohello angivna värd och port.

#### <a name="debugging-access-toovnet-hosted-resources"></a>Felsökning åtkomst tooVNet värdbaserade resurser
Det finns ett antal saker som kan förhindra att din app når en viss värd och port. För hello mesta är det en av tre saker:

* **Det finns en brandvägg i hello sätt** om du har en brandvägg på hello sätt, träffar du hello TCP-timeout. Som är i det här fallet 21 sekunder. Använd hello tcpping verktyget tootest anslutning. TCP-tidsgränser kan bero på att toomany saker utanför brandväggar men det startas. 
* **DNS är inte tillgänglig** hello DNS-timeout är tre sekunder per DNS-server. Om du har två DNS-servrar är hello timeout 6 sekunder. Använd nameresolver toosee att DNS fungerar. Kom ihåg att du inte kan använda nslookup som som inte använder hello ditt virtuella nätverk är konfigurerat med DNS.
* **Ogiltig P2S IP-adressintervall** hello punkt toosite IP-intervallet måste toobe i hello RFC 1918 privata IP-adressintervall (10.0.0.0-10.255.255.255 / 172.16.0.0-172.31.255.255 / 192.168.0.0-192.168.255.255). Om hello intervall använder IP-adresser utanför som, sedan fungerar saker inte. 

Om ditt problem inte svara på dessa objekt, leta efter hello enkla t.ex.: 

* Visar hello Gateway som upp hello portal?
* Visa certifikat som synkroniserade?
* Ändrades vem som helst hello nätverkskonfigurationen utan att göra en ”Sync nätverk” i hello påverkas ASP? 

Om din gateway är igång kan sedan ta den säkerhetskopiera. Om ditt certifikat är inte synkroniserade går toohello ASP vy över din VNet-integrering och träffar ”Sync nätverk”. Om du misstänker att det har en ändring som gjorts tooyour konfiguration av virtuellt nätverk och det inte att synkronisera skulle med din ASP går toohello ASP vy över din VNet-integrering och tryck på ”Sync nätverk” precis som en påminnelse, detta orsakar ett kort avbrott med din VNet-anslutning och dina appar . 

Om alla som är bra, behöver du toodig i en djupare bitars:

* Finns det några andra appar som använder VNet integrering tooreach resurser i hello samma virtuella nätverk? 
* Kan du gå toohello app-konsolen och använda tcpping tooreach andra resurser i ditt VNet? 

Om någon av hello ovan är uppfyllda, sedan VNet-integrering är bra och hello problemet är någon annanstans. Detta är där hämtar toobe av en utmaning eftersom det finns inget enkelt sätt toosee varför du inte kan nå värdnamn: port. Några av hello orsaker är:

* du har en brandvägg på värddatorn hindrar åtkomstport toohello program från ditt punkt toosite IP-adressintervall. Ofta passerande undernät kräver offentlig åtkomst.
* mål-värden är inte tillgänglig
* programmet är inte tillgänglig
* du hade hello fel IP-Adressen eller värdnamnet
* ditt program lyssnar på en annan port än vad du förväntade dig. Du kan kontrollera detta genom att gå till värden och använder ”netstat – aon” från hello-kommandotolk. Detta visar vilken process som ID lyssnar på vad port. 
* din nätverkssäkerhetsgrupper har konfigurerats på ett sådant sätt att åtkomst tooyour programvärd och port från din plats toosite IP-adressintervall

Kom ihåg att du inte vet vilken IP i ditt punkt tooSite IP-adressintervall som din app ska använda så du behöver tooallow åtkomst från hela hello-intervall. 

Ytterligare stegen innefattar:

* logga in på en annan virtuell dator i ditt virtuella nätverk och försök tooreach din resurs värdnamn: port därifrån. Det finns vissa TCP ping verktyg du kan använda för detta ändamål eller kan även använda telnet om behöver vara. hello är syftet här bara toodetermine om anslutningen är det från den här andra VM. 
* Öppna ett program på en annan virtuell dator och testa åtkomst toothat värd och port från hello-konsolen från din app

#### <a name="on-premises-resources"></a>Lokala resurser ####
Om din app inte kan nå resurser på lokalt, sedan hello första bör du kontrollera är om du kan nå en resurs i ditt VNet. Om det fungerar försök tooreach hello lokalt program från en virtuell dator i hello virtuella nätverk. Du kan använda telnet eller en TCP ping-verktyget. Om den virtuella datorn inte kan nå din lokal resurs, kontrollera din plats tooSite VPN-anslutning fungerar. Om den fungerar kontrollerar du hello samma saker antecknade tidigare samt hello lokala gateway-konfiguration och status. 

Nu ditt VNet finns VM kan nå datorn lokalt men appen kan sedan hello orsak är förmodligen en av hello följande:

* din vägar har inte konfigurerats med din punkt toosite IP-adressintervall i din lokala gateway
* din nätverkssäkerhetsgrupper blockerar åtkomst för ditt punkt tooSite IP-adressintervall
* din lokala brandväggar blockerar trafik från din plats tooSite IP-adressintervall
* du har definierat Route(UDR) en användare i ditt VNet som förhindrar punkt tooSite baserat trafiken från att nå lokala nätverk

## <a name="hybrid-connections-and-app-service-environments"></a>Hybridanslutningar och Apptjänstmiljöer
Det finns tre funktioner som gör att komma åt tooVNet värdbaserade resurser. De är:

* VNet-integrering
* Hybridanslutningar
* Apptjänstmiljöer

Hybridanslutningar kräver tooinstall en relay agent kallas hello Hybrid anslutning Manager(HCM) i nätverket. hello HCM måste toobe kan tooconnect tooAzure och tooyour program. Den här lösningen är särskilt stor från nätverket, till exempel ditt lokala nätverk eller även ett annat moln värdbaserad nätverk eftersom det inte kräver en tillgänglig internet-slutpunkt. hello HCM körs bara på Windows och du kan ha upp toofive instanser tooprovide hög tillgänglighet. Hybridanslutningar stöder endast TCP men och varje HC slutpunkt har toomatch tooa specifika värdnamn: port-kombination. 

Hej kan Apptjänst-miljö du toorun en instans av hello Azure App Service i ditt VNet. Detta gör att dina appar komma åt resurser i ditt virtuella nätverk utan extra steg. Vissa hello andra fördelar med en Apptjänst-miljö är att du kan använda Dv2 baserat anställda med upp too14 GB RAM. En annan fördel är att du kan skala hello system toomeet dina behov. Till skillnad från hello flera innehavare miljöer där ASP är begränsad too20 instanser, i en ASE kan du skala upp too100 ASP instanser. Hello saker som du får med en ASE som du inte med VNet-integrering är att en Apptjänst-miljö kan arbeta med en ExpressRoute-VPN. 

När det inte finns några använder case överlappar varandra kan kan ingen av de här funktionerna ersätta någon hello andra. Att veta vilka funktionen toouse är bundet tooyour behov. Exempel:

* Om du utvecklar och bara vill toorun en plats i Azure och har åtkomst hello databasen hello arbetsstation under skrivbordet, sedan hello enklaste sak toouse Hybridanslutningar. 
* Om du har en stor organisation som vill tooput ett stort antal webbegenskaper i hello offentligt moln och hantera dem i ditt eget nätverk vill du toogo med hello Apptjänst-miljö. 
* Om du har ett antal Apptjänst finns appar och bara vill tooaccess resurser i ditt VNet, sedan VNet-integrering är hello sätt toogo. 

Det finns vissa enkelhet utöver hello användningsområden relaterade aspekter. Om ditt VNet är redan ansluten tooyour lokalt nätverk genom integrering av virtuellt nätverk eller en Apptjänst-miljö är ett enkelt sätt tooconsume lokala resurser. På hello ansluten däremot om ditt VNet inte är tooyour lokalt nätverk är det mycket mer övergripande tooset upp en plats toosite VPN med ditt VNet jämfört med att installera hello HCM. 

Därutöver Hej funktionella skillnader, också priser skillnader. Hej Apptjänstmiljö funktionen är en Premium tjänsterbjudande men erbjudanden hello möjligheter för de flesta nätverk configuration dessutom tooother bra funktioner. VNet-integrering kan användas med Standard eller Premium ASP och är perfekt för förbrukning av resurser i ditt virtuella nätverk från hello flera innehavare Apptjänst på ett säkert sätt. Hybridanslutningar för närvarande beror på ett konto som har prissättning nivåer som startar ledigt och sedan hämta progressivt dyrare baserat på hello beloppet måste BizTalk. När det gäller tooworking över flera nätverk men finns det ingen annan funktion som Hybridanslutningar som gör att du tooaccess resurser i väl över 100 separata nätverk. 

<!--Image references-->
[1]: ./media/web-sites-integrate-with-vnet/vnetint-upgradeplan.png
[2]: ./media/web-sites-integrate-with-vnet/vnetint-existingvnet.png
[3]: ./media/web-sites-integrate-with-vnet/vnetint-createvnet.png
[4]: ./media/web-sites-integrate-with-vnet/vnetint-howitworks.png
[5]: ./media/web-sites-integrate-with-vnet/vnetint-appmanage.png
[6]: ./media/web-sites-integrate-with-vnet/vnetint-aspmanage.png
[7]: ./media/web-sites-integrate-with-vnet/vnetint-aspmanagedetail.png
[8]: ./media/web-sites-integrate-with-vnet/vnetint-vnetp2s.png

<!--Links-->
[VNETOverview]: http://azure.microsoft.com/documentation/articles/virtual-networks-overview/ 
[AzurePortal]: http://portal.azure.com/
[ASPricing]: http://azure.microsoft.com/pricing/details/app-service/
[VNETPricing]: http://azure.microsoft.com/pricing/details/vpn-gateway/
[DataPricing]: http://azure.microsoft.com/pricing/details/data-transfers/
[V2VNETP2S]: http://azure.microsoft.com/documentation/articles/vpn-gateway-howto-point-to-site-rm-ps/
[IntPowershell]: http://azure.microsoft.com/documentation/articles/app-service-vnet-integration-powershell/
[ASEintro]: http://docs.microsoft.com/azure/app-service/app-service-environment/intro
[ILBASE]: http://docs.microsoft.com/azure/app-service/app-service-environment/create-ilb-ase
[V2VNETPortal]: https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal
[VPNERCoex]: http://docs.microsoft.com/en-us/azure/expressroute/expressroute-howto-coexist-resource-manager
[ASE]: http://docs.microsoft.com/azure/app-service/app-service-environment/intro
