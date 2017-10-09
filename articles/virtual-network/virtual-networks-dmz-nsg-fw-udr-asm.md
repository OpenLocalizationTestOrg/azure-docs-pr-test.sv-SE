---
title: "aaaDMZ exempel – skapa en DMZ tooProtect nätverk med en brandvägg, UDR och NSG | Microsoft Docs"
description: "Skapa en DMZ med en brandvägg, användardefinierade routning (UDR) och Nätverkssäkerhetsgrupper (NSG)"
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: 
ms.assetid: dc01ccfb-27b0-4887-8f0b-2792f770ffff
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/01/2016
ms.author: jonor;sivae
ms.openlocfilehash: cc121f9cd5fe3c3e9ac2c70fbb7d982a80bb345d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="example-3--build-a-dmz-tooprotect-networks-with-a-firewall-udr-and-nsg"></a>Exempel 3 – skapa en DMZ tooProtect nätverk med en brandvägg, UDR och NSG
[Returnera toohello gräns bästa praxis sidan][HOME]

Det här exemplet skapar en DMZ med en brandvägg, fyra windows-servrar, användaren definierat routning, IP-vidarebefordring och Nätverkssäkerhetsgrupper. Den kommer också att gå igenom varje hello relevanta kommandon tooprovide en bättre förståelse för varje steg. Det finns också en trafik scenariot avsnittet tooprovide en djupgående steg för steg hur trafik fortsätter via hello lager i skyddsstrategierna hello DMZ. Slutligen är i referensavsnittet hello hello fullständiga koden och instruktion toobuild den här miljön tootest och experimentera med olika scenarier. 

![Dubbelriktad DMZ med NVA, NSG och UDR][1]

## <a name="environment-setup"></a>Miljökonfiguration
I det här exemplet finns det en prenumeration som innehåller hello följande:

* Tre molntjänster: ”SecSvc001”, ”FrontEnd001” och ”BackEnd001”
* Ett virtuellt nätverk ”CorpNetwork” med tre undernät: ”SecNet”, ”FrontEnd” och ”BackEnd”
* En virtuell nätverksenhet, i det här exemplet en brandvägg, anslutna toohello SecNet undernät
* En Windows-Server som representerar en program-webbserver (”IIS01”)
* Två windows-servrar som representerar tillbaka programmet avslutas servrar (”AppVM01”, ”AppVM02”)
* En Windows-server som representerar en DNS-server (”DNS01”)

I hello referensavsnittet nedan finns ett PowerShell-skript som skapar de flesta av hello miljön som beskrivs ovan. Skapa hello virtuella datorer och virtuella nätverk, som även om utförs av hello exempelskriptet inte beskrivs i detalj i detta dokument.

toobuild hello miljö:

1. Spara hello nätverk XML-konfigurationsfilen finns i hello referenser avsnitt (uppdaterade med namn, plats och IP-adresser toomatch hello angivna scenario)
2. Uppdatera hello Användarvariabler i hello skriptet toomatch hello miljö hello skript är toobe körs mot (prenumerationer, tjänstnamn osv.)
3. Kör hello-skriptet i PowerShell

**Obs**: hello region som visas i hello PowerShell-skriptet måste matcha hello region som visas i hello nätverket xml-konfigurationsfilen.

När hello skriptet har körts vidtas hello efter skriptet följande:

1. Konfigurera brandväggsregler för hello detta beskrivs i hello avsnittet nedan: beskrivning av brandväggen.
2. Du kan också är hello referensavsnittet två skript tooset hello webbservern och app-servern med en enkel web application tooallow testning med den här konfigurationen DMZ.

När hello skriptet har körts hello brandväggsregler måste toobe slutförts, detta beskrivs i avsnittet hello: brandväggsregler.

## <a name="user-defined-routing-udr"></a>Användardefinierad routning (UDR)
Som standard definieras hello följande systemvägar som:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.0.0/16}     VNETLocal                            Active   Default    
         {0.0.0.0/0}       Internet                             Active   Default    
         {10.0.0.0/8}      Null                                 Active   Default    
         {100.64.0.0/10}   Null                                 Active   Default    
         {172.16.0.0/12}   Null                                 Active   Default    
         {192.168.0.0/16}  Null                                 Active   Default

Hej VNETLocal är alltid hello definierats adress prefix för hello virtuella nätverk för det specifika nätverket (ie ändras från VNet tooVNet beroende på hur varje specifik VNet har definierats). hello återstående systemvägar är statiska och standard som ovan.

För prioritet, skulle vägar bearbetas via hello längsta Prefix-matchning (LPM) metoden gäller därför hello mest specifika väg i hello tabell tooa angivna måladressen.

Därför ska trafik (till exempel toohello DNS01 server, 10.0.2.4), avsedda för hello lokala nätverk (10.0.0.0/16) skickas över hello VNet tooits mål på grund av toohello 10.0.0.0/16 vägen. Med andra ord för 10.0.2.4 är hello 10.0.0.0/16 väg hello mest specifika flödet, även om hello 10.0.0.0/8 och 0.0.0.0/0 kunde gäller även, men eftersom de är mindre specifikt de påverkar inte den här trafiken. Därmed skulle hello trafik too10.0.2.4 ha en nästa hopp hello virtuella lokala nätverk och bara vidarebefordra toohello mål.

Om trafiken är avsedd för 10.1.1.1 till exempel, hello 10.0.0.0/16 väg skulle tillämpas, men hello 10.0.0.0/8 skulle vara hello mest specifika och hello trafiken skulle vara bort (”svart holed”) eftersom hello nästa hopp är Null. 

Om hello mål kunde inte tillämpas tooany hello Null-prefix eller hello VNETLocal prefix och det skulle följer hello minst specifika vidarebefordra 0.0.0.0/0 och skickas ut toohello Internet som hello nästa hopp och därför ut Azures internet kant.

Om det finns två identiska prefix i hello routningstabellen, följer hello hello prioritetsordning baserat på hello vägar ”” källattribut:

1. ”VirtualAppliance” = en användardefinierad manuellt tillagda toohello routningstabell
2. ”VPNGateway” = en dynamisk väg BGP (när det används med hybrid nätverk), lagts till av en dynamisk nätverksprotokoll, dessa vägar kan förändras över tid när hello dynamiska protokollet automatiskt återger ändringar i peerkoppla nätverk
3. ”Standard” = hello Systemvägar hello lokala VNet och hello statiska poster som visas i hello routningstabellen ovan.

> [!NOTE]
> Du kan nu använda användaren definierat routning (UDR) med ExpressRoute- och VPN-gatewayer tooforce utgående och inkommande anslutningar mellan lokala trafiken toobe routade tooa virtuell nätverksenhet (NVA).
> 
> 

#### <a name="creating-hello-local-routes"></a>Skapa hello lokala vägar
I det här exemplet krävs två routningstabeller, en för hello Frontend och Backend-undernät. Varje tabell har lästs in med statiska vägar som är lämpliga för hello angivna undernätet. För hello syftet med det här exemplet har varje tabell tre vägar:

1. Lokal undernätstrafik med ingen nästa hopp definierats tooallow undernätet trafik toobypass hello brandvägg
2. Trafik i virtuella nätverk med en nästa hopp har definierats som brandväggen, åsidosätts hello Standardregeln som tillåter lokala tooroute för VNet-trafik direkt
3. Alla återstående trafik (0/0) med en nästa hopp definierade som hello brandväggen

När hello routningstabeller skapas är de bundna tootheir undernät. För hello klientdel undernät routningstabell, när skapats och bundna toohello undernät bör se ut så här:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.1.0/24}     VNETLocal                            Active 
         {10.0.0.0/16}     VirtualAppliance 10.0.0.4            Active    
         {0.0.0.0/0}       VirtualAppliance 10.0.0.4            Active


I det här exemplet hello följande kommandon är används toobuild hello routningstabellen, lägga till en användardefinierad väg och binda hello flödet tabell tooa undernät (Obs!; eventuella objekt nedan som börjar med ett dollartecken (t.ex.: $BESubnet) är användardefinierade variabler från hello skript i hello referensavsnitt i det här dokumentet):

1. Första hello grundläggande routningstabell måste skapas. Kodutdrag hello skapandet av hello tabellen för hello Backend-undernät. I hello skript skapas även en motsvarande tabell för hello Klientdelens undernät.
   
     Nya AzureRouteTable-Name $BERouteTableName '
   
         -Location $DeploymentLocation `
         -Label "Route table for $BESubnet subnet"
2. När du har skapat hello routningstabellen läggas specifika användardefinierade vägar. I det här liten, kommer all trafik (0.0.0.0/0) att dirigeras via hello virtuell installation (en variabel, $VMIP [0], är används toopass i hello IP-adress när hello virtuell installation som skapades tidigare i hello skript). I hello skript skapas även en motsvarande regel i hello klientdel tabell.
   
     Get-AzureRouteTable $BERouteTableName | `
   
         Set-AzureRoute -RouteName "All traffic tooFW" -AddressPrefix 0.0.0.0/0 `
         -NextHopType VirtualAppliance `
         -NextHopIpAddress $VMIP[0]
3. hello ovan väg åsidosätter hello standardvägen ”0.0.0.0/0”, men hello 10.0.0.0/16 standardregel fortfarande befintliga som gör att trafiken inom hello VNet tooroute direkt toohello mål och inte toohello virtuell nätverksenhet. toocorrect regeln beteende hello Följ måste läggas till.
   
        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "Internal traffic tooFW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
4. Det finns nu en toobe för val som görs. Med hello ovan två vägar dirigerar all trafik toohello brandväggen för utvärdering, även trafik i ett enda undernät. Detta kan det vara önskvärt, men tooallow trafik i ett undernät tooroute lokalt utan inblandning från hello brandvägg kan läggas till en tredje, mycket specifik regel. Den här vägen tillstånd för alla adresser destine för hello undernätet enkelt kan vidarebefordra det direkt (NextHopType = VNETLocal).
   
        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
            -NextHopType VNETLocal
5. Slutligen med hello routningstabell skapas och konfigureras med en användardefinierade vägar, måste hello tabell nu vara bundna tooa undernät. Hello klientdelen routningstabellen är också bundna toohello undernätet Frontend i hello skript. Här är hello bindning skript för hello backend-undernät.
   
     Set-AzureSubnetRouteTable - VirtualNetworkName $VNetName '
   
        -SubnetName $BESubnet `
        -RouteTableName $BERouteTableName

## <a name="ip-forwarding"></a>IP-vidarebefordran
En funktion tooUDR tillhörande är IP-vidarebefordring. Detta är en inställning i en virtuell installation som tillåter den tooreceive trafik inte uttryckligen åtgärdas toohello installation och vidarebefordra trafik tooits ultimate destinationen.

Exempelvis om trafik från AppVM01 gör en begäran toohello DNS01 server skulle UDR vidarebefordra toohello-brandväggen. Med IP-vidarebefordran aktiverat, kommer hello trafik för hello DNS01 mål (10.0.2.4) accepteras av hello-enhet (10.0.0.4) och sedan vidarebefordras tooits slutdestinationen (10.0.2.4). Utan IP-vidarebefordran aktiverad på hello brandväggen, skulle trafik inte godkännas av hello installation även om hello routningstabellen har hello brandväggen som hello nästa hopp. 

> [!IMPORTANT]
> Det är kritiska tooremember tooenable IP-vidarebefordran tillsammans med användaren definierat routning.
> 
> 

Konfigurera IP-vidarebefordran är ett enda kommando och kan göras vid tidpunkten för skapandet av virtuell dator. För hello flödet av det här exemplet hello kodstycket hello utgången av hello skript och grupperas med hello UDR kommandon:

1. Anropa hello VM-instans som är din virtuella installation, hello brandväggen i det här fallet och aktivera IP-vidarebefordring (Obs!; ett objekt i rött som börjar med ett dollartecken (t.ex.: $VMName[0]) är en användardefinierad variabel från hello skript under hello referens i det här dokumentet. Hej noll i hakparenteserna, [0], representerar hello första virtuella datorn i hello matris av virtuella datorer, för hello exempel skriptet toowork utan ändringar, hello första virtuella dator (VM 0) måste vara hello brandvägg):
   
     Get-AzureVM-Name $VMName [0] - ServiceName $ServiceName [0] | `
   
        Set-AzureIPForwarding -Enable

## <a name="network-security-groups-nsg"></a>Nätverkssäkerhetsgrupper (NSG)
I det här exemplet bygger en NSG-grupp och sedan in med en enskild regel. Den här gruppen är sedan bunden endast toohello Frontend och Backend-undernät (inte hello SecNet). Deklarativt skapas hello följande regel:

1. All trafik (alla portar) från hello Internet toohello hela VNet (alla undernät) nekas

Även om NSG: er som används i det här exemplet är huvudsakliga syftet som en sekundär försvarslinje mot manuell felaktig konfiguration. Vi vill tooblock alla inkommande trafik från hello internet tooeither hello klientdel eller Backend-undernät, trafik bör endast passera hello SecNet undernät toohello brandvägg (och om lämpliga på toohello klientdelen eller serverdelen undernät). Dessutom med hello UDR regler och skulle all trafik som gjorde det till hello klientdelen eller serverdelen undernät dirigeras ut toohello brandvägg (tack tooUDR). hello brandväggen visas detta som en asymmetrisk dataflöde och skulle sjunka hello utgående trafik. Därför finns det tre lager av säkerhet skyddar hello Frontend och Backend-undernät; 1) utan öppna slutpunkter på hello FrontEnd001 och BackEnd001 molntjänster, 2) NSG: er nekar trafik från hello Internet, 3) hello brandvägg släpper asymmetriska trafik.

En intressant avseende hello säkerhetsgrupp för nätverk i det här exemplet är att den innehåller endast en regel som visas nedan, toodeny internet-trafik toohello hela virtuella nätverk som omfattar hello säkerhet undernät. 

    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule -Name "Isolate hello $VNetName VNet `
        from hello Internet" `
        -Type Inbound -Priority 100 -Action Deny `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK `
        -DestinationPortRange '*' `
        -Protocol *

Men eftersom hello NSG är endast bunden toohello Frontend och Backend-undernät, hello regel inte bearbetas på trafik inkommande toohello säkerhet undernät. Därför även om hello NSG regeln står ingen Internet-trafik tooany adress på hello VNet, eftersom hello NSG har aldrig bundet toohello säkerhet undernät kommer trafiken toohello säkerhet undernät.

    Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
        -SubnetName $FESubnet -VirtualNetworkName $VNetName

    Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
        -SubnetName $BESubnet -VirtualNetworkName $VNetName

## <a name="firewall-rules"></a>Brandväggsregler
Hello Brandvägg för måste vidarebefordran regler toobe skapas. Eftersom hello-brandväggen blockerar eller vidarebefordran alla inkommande, utgående och intra-VNet-trafik krävs många brandväggsregler. Dessutom kommer all inkommande trafik till hello Security Service offentlig IP-adress (på olika portar) toobe bearbetas av hello brandväggen. Ett bra tips är toodiagram hello logiska flöden innan du konfigurerar hello-undernät och brandväggen regler tooavoid dubbelarbete senare. hello är följande bild en logisk vy för hello brandväggsregler för det här exemplet:

![Logisk vy för hello brandväggsregler][2]

> [!NOTE]
> Hello hanteringsportar varierar utifrån hello virtuell nätverksenhet används. I det här exemplet refererar till en brandvägg för nästa generation av Barracuda som använder port 22, 801 och 807. Kontakta hello installation leverantörens dokumentation toofind hello exakt portarna som används för hantering av hello enhet som används.
> 
> 

### <a name="logical-rule-description"></a>Beskrivning av logiska regel
I hello logiskt diagram ovan visas hello säkerhet undernät inte eftersom hello brandväggen är hello endast resurs i undernätet och det här diagrammet visar hello brandväggsregler och hur de logiskt Tillåt eller neka trafikflöde och inte hello faktiska routade sökväg. Dessutom hello externa portar som valts för hello RDP-trafik är högre låg portar (8014 – 8026) och har markerat toosomewhat överensstämmer med hello senast två oktetterna i hello lokal IP-adress för att lättare att läsa (t.ex. lokala serveradress 10.0.1.4 är associerad med externa i port 8014), men alla högre icke motstridig portar kan användas.

Vi behöver 7 typer av regler, dessa regeltyper är som följer beskrivs i det här exemplet:

* Externa regler (för inkommande trafik):
  1. Hantering av brandväggsregel: Regeln App omdirigering tillåter trafik toopass toohello hanteringsportar för hello virtuell nätverksenhet.
  2. RDP-regler (för varje windows server): dessa fyra regler (en för varje server) kan hanteringen av hello enskilda servrar via RDP. Detta kan också ihop till en regel beroende på hello funktionerna i hello virtuell nätverksenhet som används.
  3. Regler för nätverkstrafik för programmet: Finns två regler för nätverkstrafik för programmet och hello först för hello klientdelen webbtrafik hello andra för hello backend-trafik (t ex web server toodata nivån). hello konfigurationen av reglerna beror på hello nätverksarkitekturen (där servrarna placeras) och trafikflöden (vilken riktning hello trafiken flödar och vilka portar som används).
     * hello första regeln tillåter hello faktiska trafik tooreach hello programmet programserver. Hello andra regler som tillåter för säkerhet, hantering, etc., är program-regler vad tillåta externa användare eller tjänster tooaccess hello program. Det finns en enda webbserver på port 80, vilket en enda brandväggsregel för programmet att omdirigera inkommande trafik toohello extern IP, toohello servrar interna IP-adressen i det här exemplet. hello omdirigerad trafik session skulle NAT tas toohello interna servern.
     * hello regel för nätverkstrafik för andra program är hello serverdel regeln tooallow hello webbservern tootalk toohello AppVM01 server (men inte AppVM02) via alla portar.
* Interna regler (för intra-VNet-trafik)
  1. Utgående tooInternet regel: den här regeln tillåter trafik från alla nätverk toopass toohello markerat nätverk. Den här regeln är vanligtvis en standardregel som redan finns på hello brandväggen, men i ett inaktiverat tillstånd. Den här regeln ska aktiveras för det här exemplet.
  2. DNS-regel: Den här regeln kan bara DNS (port 53) trafik toopass toohello DNS-server. För den här miljön som de flesta trafik från hello klientdel toohello Backend blockeras tillåter den här regeln specifikt DNS från alla lokala undernätet.
  3. Undernät tooSubnet regel: den här regeln är tooallow alla servrar på hello tillbaka avslutas undernät tooconnect tooany server i hello klientdelen undernät (men inte hello omvänd).
* Felsäker regel (för trafik som inte uppfyller något av ovanstående hello):
  1. Neka alla Trafikregel: Detta ska alltid vara hello sista regeln (vad gäller prioritet) och som sådan om en trafiken flödar misslyckas toomatch någon hello före regler som den kommer att tas bort av den här regeln. Det här är en standardregel och vanligtvis aktiverad behövs vanligtvis inga ändringar.

> [!TIP]
> Alla portar tillåts för enkelt i detta exempel på hello andra program regel för nätverkstrafik, i ett verkligt scenario hello mest specifika porten och adressen adressintervall ska använda tooreduce hello risken för angrepp på den här regeln.
> 
> 

<br />

> [!IMPORTANT]
> När alla hello ovan regler skapas, är det viktigt tooreview hello prioritet för varje regel tooensure trafik ska tillåtas eller nekas efter behov. I det här exemplet är hello i prioritetsordning. Det är enkelt toobe utelåst från hello-brandväggen på grund av toomis sorterade regler. Kontrollera hello hantering för själva hello brandväggen är alltid hello absolut högsta prioritet regeln minimum.
> 
> 

### <a name="rule-prerequisites"></a>Regel för krav
En nödvändig för hello virtuella datorn körs hello brandväggen är offentliga slutpunkter. Hello offentliga slutpunkter måste vara öppna för hello brandväggen tooprocess trafik. Det finns tre typer av trafik i det här exemplet; 1) management trafik toocontrol hello-brandväggen och brandväggsregler, 2) RDP-trafik toocontrol hello windows-servrar och 3) programmets trafik. Dessa är hello tre kolumner med typer av trafik i hello övre hälften av logisk vy för hello brandväggsregler ovan.

> [!IMPORTANT]
> En nyckel takeway är tooremember som **alla** trafik kommer hello-brandväggen. Så tooremote skrivbord toohello IIS01 server, även om den i hello Front End-Molntjänsten och på hello klientdelen undernät, tooaccess den här servern måste vi tooRDP toohello brandväggen på port 8014 och låt hello brandväggen tooroute hello RDP begäran internt toohello IIS01 RDP-porten. hello Azure portal ”Anslut” knappen inte fungerar eftersom det inte finns några direkt RDP sökvägen tooIIS01 (som hello portal kan se). Det innebär att alla anslutningar från hello internet kommer att vara toohello Security Service och en Port, t.ex. secscv001.cloudapp.net:xxxx (referens hello ovan diagram för hello mappning för extern Port och intern IP-adress och Port).
> 
> 

En slutpunkt kan öppnas antingen när hello dags att skapa en virtuell dator eller efter version som är klar i hello exempelskriptet och nedan i det här kodstycket (Obs!; alla objekt som börjar med ett dollartecken (t.ex.: $VMName[$i]) är en användardefinierad variabel från hello skriptet i hello referen CE avsnitt i det här dokumentet. Hej ”$i” i hakparenteserna, [$i] representerar hello matris antalet för en specifik virtuell dator i en matris av virtuella datorer):

    Add-AzureEndpoint -Name "HTTP" -Protocol tcp -PublicPort 80 -LocalPort 80 `
        -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | `
        Update-AzureVM

Även om inte klart visas här på grund av toohello användning av variabler, men slutpunkter är **endast** öppnas på hello tjänst i molnet. Detta är att all inkommande trafik hanteras tooensure (dirigeras, NAT hade, släppa) av hello brandvägg.

Management-klienten ska måste toobe installerad på en dator toomanage hello brandvägg och skapa hello-konfigurationer som krävs. Se hello dokumentationen från brandväggen (eller andra NVA) leverantör på hur toomanage hello enhet. hello resten av det här avsnittet och hello nästa avsnitt, brandvägg regler skapas, beskriver hello konfigurationen av hello brandvägg, via hello leverantörer management-klienten (d.v.s. inte hello Azure-portalen eller PowerShell).

Instruktioner för att klienten ska ladda ned och anslutande toohello Barracuda som används i det här exemplet finns här: [Barracuda NG Admin](https://techlib.barracuda.com/NG61/NGAdmin)

När inloggad på hello brandväggen men innan du skapar brandväggsregler, finns det två nödvändiga objektklasser som kan underlätta skapar hello regler; Nätverks- och objekt.

I det här exemplet ska tre namngivna nätverksobjekt definierade (ett för hello Frontend-undernät och hello Backend-undernät, även ett nätverksobjekt för hello hello DNS-serverns IP-adress). toocreate ett namngivet nätverk. från hello Barracuda NG Admin klienten instrumentpanelen navigera toohello konfigurationsfliken, i hello operativa konfigurationsavsnittet RuleSet-metod, sedan klickar du på ”nätverk” Hej Brandväggsobjekt menyn Klicka på ny hello nätverk för Redigera-menyn. hello nätverksobjekt kan nu skapas genom att lägga till hello namn och hello prefix:

![Skapa en FrontEnd-objektet][3]

Detta skapar ett namngivet nätverk för hello klientdel undernät, en liknande objekt ska skapas för samt hello BackEnd-undernät. Nu kan hello undernät refereras enklare namn i hello brandväggsregler.

För hello DNS-Server-objekt:

![Skapa en DNS-Server-objekt][4]

Den här enda IP-adress referensen används i en DNS-regel senare i hello dokumentet.

hello andra nödvändiga objekt är Services-objekt. Dessa representerar hello RDP portar för varje server. Eftersom hello befintliga RDP-tjänstobjekt bundna toohello RDP standardporten kan 3389, nya tjänster skapas tooallow trafik från externa hello-portar (8014 8026). hello nya portar kan också läggas toohello befintliga RDP-tjänsten, men för att underlätta demonstration, kan du skapa en regel för varje server. toocreate en ny RDP-regel för en server. Starta från hello Barracuda NG Admin klienten instrumentpanelen navigerar toohello konfigurationsfliken, hello användningsinställningar avsnittet Klicka i RuleSet-metod, och sedan klickar du på ”tjänster” under hello Brandväggsobjekt menyn Bläddra nedåt hello listan med tjänster och välj hello ”RDP”-tjänsten. Högerklicka och välj Kopiera, högerklicka och välj Klistra in. Det finns nu en RDP-Copy1 Service-objekt som kan redigeras. Högerklicka på RDP-Copy1 och väljer Redigera, hello redigera webbtjänstobjektet fönster visas som visas här:

![Kopia av RDP standardregel][5]

hello-värden kan sedan vara redigerade toorepresent hello RDP-tjänst för en specifik server. För AppVM01 hello ovan standard RDP regeln ska vara ändrade tooreflect tjänstnamn, beskrivning, och externa RDP-porten som används i hello figur 8 diagram (Obs: hello portar ändras från hello RDP standardvärdet 3389 toohello externa-porten som används för den här specifik server hello gäller AppVM01 hello externa porten är 8025) hello ändrade tjänsten visas nedan:

![AppVM01 regel][6]

Den här processen måste vara upprepade toocreate RDP-tjänster för hello återstående servrar. AppVM02 DNS01 och IIS01. hello skapande av dessa tjänster gör skapande av hello enklare och mer påtagligt i hello nästa avsnitt.

> [!NOTE]
> En RDP-tjänst för hello brandväggen behövs inte av två skäl. 1) första hello brandväggen VM är en avbildning av Linux-baserade så SSH används på port 22 för hantering av virtuell dator i stället för RDP och 2) port 22 och två andra hanteringsportar tillåts i hello första management regeln som beskrivs nedan tooallow för anslutning.
> 
> 

### <a name="firewall-rules-creation"></a>Skapa regler för brandvägg
Det finns tre typer av brandväggsregler som används i det här exemplet, alla har olika ikoner:

hello programmet omdirigera regel: ![omdirigera programikon][7]

hello mål NAT-regel: ![mål NAT-ikon][8]

hello Pass regel: ![skicka ikon][9]

Mer information om dessa regler finns på webbplatsen för hello Barracuda.

toocreate hello följande regler (eller verifiera befintliga standardregler) från hello Barracuda NG Admin klienten instrumentpanelen navigera toohello konfigurationsfliken klickar du på i hello användningsinställningar avsnittet RuleSet-metod. Ett rutnät som kallas visar ”Main regler” hello befintliga aktiva och inaktiva regler på den här brandväggen. Hello övre högra hörnet i det här rutnätet är en liten, grön ”+”, klicka på den här toocreate en ny regel (Obs: brandväggen kan vara i ”låst” för ändringar, om du ser en knapp markerad ”låsa” och du är toocreate eller redigera regler, klicka på knappen för ”låsa upp” Hej regeln se t och tillåta redigering). Om du inte vill tooedit en befintlig regel, Välj regeln, högerklicka och välj Redigera regel.

När reglerna skapas eller ändras, måste de vara pushas toohello brandväggen och sedan aktiveras om det inte görs hello regeln ändringarna inte börjar gälla. hello push och aktivering process beskrivs nedan hello information regeln beskrivningar.

hello detaljerna i varje regel krävs toocomplete som beskrivs i det här exemplet på följande sätt:

* **Brandväggen Management regeln**: den här appen omdirigera regel som tillåter trafik toopass toohello hanteringsportar för hello virtuell nätverksenhet, i det här exemplet Barracuda nästa generation brandvägg. hello hanteringsportar är 801, 807 och eventuellt 22. hello externa och interna portar är hello samma (dvs. inga port translation). Detta regeln för installationsprogrammet MGMT åtkomst, är en standardregel och aktiverat som standard (i brandväggen för nästa generation av Barracuda version 6.1).
  
    ![Hantering av brandväggsregel][10]

> [!TIP]
> hello käll-adressutrymme i den här regeln finns, om hello hantering av IP-adressintervall är kända, vilket minskar det här området skulle också minska hello attack ytan toohello hanteringsportar.
> 
> 

* **RDP-regler**: dessa mål NAT-regler kan hanteringen av hello enskilda servrar via RDP.
  Det finns fyra viktiga fälten toocreate regeln:
  
  1. Källan – tooallow RDP från var som helst, hello referens ”någon” används i hello fält i datakällan.
  2. Service – Använd lämpligt serviceobjektet skapat tidigare i det här fallet ”AppVM01 RDP” hello, hello externa portar omdirigera toohello servrar lokal IP-adress och tooport 3386 (hello RDP standardporten). Den här specifika regeln är för tooAppVM01 för RDP-åtkomst.
  3. Målservern – ska vara hello *lokal port i brandväggen hello*, ”DCHP 1 lokala-IP- eller eth0 om du använder statiska IP-adresser. hello ordningstal (eth0, eth1 osv.) kan skilja sig om en nätverksenhet har flera lokala gränssnitt. Hello porten hello brandväggen skickar från (kan vara hello samma som hello tar emot port), hello mållistan fältet är hello faktiska routade mål.
  4. Omdirigering av – det här avsnittet visar hello virtuell utrustning där tooultimately dirigera trafiken. hello enklaste omdirigering sker tooplace hello IP och Port (valfritt) i hello mållistan fältet. Om ingen port används hello målport på hello inkommande begäran kommer att användas (d.v.s. Ingen översättning) om en port används hello port tas också skulle NAT tillsammans med hello IP adress.
     
     ![RDP-brandväggsregel][11]
     
     Summan av fyra RDP-regler måste toobe skapas: 
     
     | Regelnamn | Server | Tjänst | Mållistan |
     | --- | --- | --- | --- |
     | RDP-IIS01 |IIS01 |IIS01 RDP |10.0.1.4:3389 |
     | RDP-DNS01 |DNS01 |DNS01 RDP |10.0.2.4:3389 |
     | RDP-AppVM01 |AppVM01 |AppVM01 RDP |10.0.2.5:3389 |
     | RDP-AppVM02 |AppVM02 |AppVm02 RDP |10.0.2.6:3389 |

> [!TIP]
> Begränsa hello omfattning hello källa och Service-fält kommer att minska hello risken för angrepp. hello mest begränsat scope som gör att funktionerna ska användas.
> 
> 

* **Regler för nätverkstrafik i programmet**: det finns två regler för nätverkstrafik för programmet, hello först för hello klientdelen webbtrafik och hello andra för hello backend-trafik (t ex web server toodata nivån). De här reglerna beror på hello nätverksarkitekturen (där servrarna placeras) och trafikflöden (vilken riktning hello trafiken flödar och vilka portar som används).
  
    Först beskrivs är hello klientdelen regel för webbtrafik:
  
    ![Web brandväggsregel][12]
  
    Det här målet NAT-regeln låter hello faktiska trafik tooreach hello programmet programservern. Hello andra regler som tillåter för säkerhet, hantering, etc., är program-regler vad tillåta externa användare eller tjänster tooaccess hello program. Det finns en enda webbserver på port 80, därför hello enda program brandväggsregel omdirigerar inkommande trafik toohello extern IP, toohello servrar interna IP-adressen för det här exemplet.
  
    **Obs**: att ingen port har tilldelats i hello mållistan fältet, därför hello inkommande port 80 (eller 443 för hello tjänst valde) ska användas i hello omdirigering av hello webbservern. Om webbservern hello lyssnar på en annan port, kunde till exempel port 8080, hello mållistan fältet uppdaterade too10.0.1.4:8080 tooallow för hello Port samt omdirigering.
  
    hello nästa regel för nätverkstrafik för programmet är hello serverdel regeln tooallow hello webbservern tootalk toohello AppVM01 server (men inte AppVM02) via någon tjänst:
  
    ![AppVM01 brandväggsregel][13]
  
    Den här Pass regeln kan alla IIS-servern på hello klientdel undernät tooreach hello AppVM01 (IP-adress 10.0.2.5) på alla portar via protokollet tooaccess data krävs av hello webbprogram.
  
    I den här skärmbilden en ”\<explicit dest\>” används i hello mål fältet toosignify 10.0.2.5 som hello mål. Detta kan vara något explicit enligt eller en med namnet nätverksobjekt (som har utförts i hello förutsättningar för hello DNS-server). Detta är in toohello administratör för hello brandväggen toowhich metoden används. tooadd 10.0.2.5 som en Explict Desitnation dubbelklickar du på hello första tomma rader under \<explicit dest\> och ange hello-adress i hello-fönster som visas.
  
    Med regeln skicka krävs ingen NAT eftersom det är intern trafik, så hello anslutningsmetod kan anges för ”Nej SNAT”.
  
    **Obs**: hello Källnätverk i den här regeln är en resurs på hello klientdel undernät om det ska bara finnas ett, eller skapa ett känt specifika antal webbservrar, ett nätverksobjekt resurs kan vara toobe mer specifika toothose exakt IP-adresser i stället för hello hela undernätet Frontend.

> [!TIP]
> Den här regeln använder hello service ”alla” toomake hello exempel program enklare toosetup och använder, det gör också att ICMPv4 (ping) i en enda regel. Men rekommenderas detta inte. hello portar och protokoll (”tjänsten”) ska vara smalnar toohello minsta möjliga som gör programmet åtgärden tooreduce hello risken för angrepp på den här begränsningen.
> 
> 

<br />

> [!TIP]
> Även om den här regeln visar en referens till en explicit dest används, bör du använda en konsekvent metod i hela hello brandväggskonfigurationen. Du rekommenderas att hello med namnet nätverksobjekt användas i hela för enklare läsbarhet och support. explicit hello-målet är en alternativ metod för används här endast tooshow och rekommenderas vanligtvis inte (särskilt för komplexa konfigurationer).
> 
> 

* **Utgående tooInternet regeln**: skicka den här regeln tillåter trafik från alla nätverk toopass toohello valt mål källnätverken. Den här regeln är en standardregel vanligtvis redan hello Barracuda nästa generation brandväggen, men är i ett inaktiverat tillstånd. Högerklicka på den här regeln kan komma åt hello aktivera regeln kommandot. hello-regel som visas här har ändrat tooadd hello två lokala undernät som har skapats som referens i hello nödvändiga avsnittet i det här dokumentet toohello källattributet för den här regeln.
  
    ![Utgående brandväggsregel][14]
* **DNS-regel**: skicka den här regeln kan bara DNS (port 53) trafik toopass toohello DNS-server. Den här regeln kan särskilt DNS för den här miljön som de flesta trafik från hello klientdel toohello BackEnd blockeras.
  
    ![DNS-brandväggsregel][15]
  
    **Obs**: ingår som hello anslutningsmetod i den här skärmen. Eftersom den här regeln är intern IP-toointernal IP-adress trafik kan inga NATing krävs, detta hello anslutningsmetod anges för ”Nej SNAT” för den här Pass-regeln.
* **Undernät tooSubnet regeln**: skicka den här regeln är en standardregel som har aktiverats och ändrade tooallow alla servrar på hello tillbaka upphör undernät tooconnect tooany server hello klientdelens undernät. Den här regeln är alla intern trafik hello anslutningsmetod kan ange tooNo SNAT.
  
    ![Inom VNet brandväggsregel][16]
  
    **Obs**: hello dubbelriktad kryssrutan kontrolleras inte (är inte heller incheckad regler för de flesta), detta är viktig för den här regeln eftersom den gör detta regel ”envägs”, kan du initiera en anslutning från hello serverdel toohello klientdelen undernätverk, men inte hello omvänd. Om kryssrutan är markerad skulle med den här regeln göra dubbelriktad trafik som från våra logiskt diagram inte används.
* **Neka alla Trafikregel**: du bör alltid hello sista regeln (vad gäller prioritet) och som sådan om en trafiken flödar misslyckas toomatch något av föregående regler hello den kommer att tas bort av den här regeln. Det här är en standardregel och vanligtvis aktiverad behövs vanligtvis inga ändringar. 
  
    ![Regel för att neka brandväggen][17]

> [!IMPORTANT]
> När alla hello ovan regler skapas, är det viktigt tooreview hello prioritet för varje regel tooensure trafik ska tillåtas eller nekas efter behov. I det här exemplet är hello i hello ordning som de ska visas i hello Main rutnätet vidarebefordra regler i hello Barracuda Management-klienten.
> 
> 

## <a name="rule-activation"></a>Regeln aktivering
Med hello ruleset ändrade toohello specifikation av hello logiska diagram, hello RuleSet-metod måste vara överförs toohello brandväggen och sedan aktiveras.

![Brandväggen regeln aktivering][18]

Är ett kluster på knapparna i hello övre högra hörnet av hello management-klienten. Hello ”skicka ändringar” knappen toosend hello ändrade regler toohello brandväggen Klicka på knappen för hello ”aktivera”.

Det här exemplet miljö bygget är klar med hello aktivering hello brandväggen RuleSet-metod.

## <a name="traffic-scenarios"></a>Trafik scenarier
> [!IMPORTANT]
> En nyckel takeway är tooremember som **alla** trafik kommer hello-brandväggen. Så tooremote skrivbord toohello IIS01 server, även om den i hello Front End-Molntjänsten och på hello klientdelen undernät, tooaccess den här servern måste vi tooRDP toohello brandväggen på port 8014 och låt hello brandväggen tooroute hello RDP begäran internt toohello IIS01 RDP-porten. hello Azure portal ”Anslut” knappen inte fungerar eftersom det inte finns några direkt RDP sökvägen tooIIS01 (som hello portal kan se). Det innebär att alla anslutningar från hello internet kommer att vara toohello Security Service och en Port, t.ex. secscv001.cloudapp.net:xxxx.
> 
> 

För dessa scenarier ska hello följande brandväggsregler finnas:

1. Brandväggshantering
2. RDP-tooIIS01
3. RDP-tooDNS01
4. RDP-tooAppVM01
5. RDP-tooAppVM02
6. Appen trafik toohello Web
7. Appen trafik tooAppVM01
8. Utgående toohello Internet
9. Klientdel tooDNS01
10. Intra-undernätstrafik (endast serverdel toofront end)
11. Neka alla

hello faktiska brandväggen ruleset har antagligen många andra regler i tillägg toothese, hello reglerna på alla angivna brandväggen har även olika prioritet siffror än hello de som visas här. Den här listan och tillhörande nummer är tooprovide relevans mellan bara dessa elva regler och hello relativ prioritet bland dem. Med andra ord; hello faktiska brandväggen kan hello ”RDP tooIIS01” vara regeln tal 5, men så länge det är under hello ”Brandväggshantering” regeln och över hello ”RDP tooDNS01” regeln skulle uppfyller hello syftet med den här listan. hello-lista också underlätta vid hello nedan scenarier för att tillåta planeringsaspekter; t.ex. ”FW regeln 9 (DNS)”. Planeringsaspekter, hello fyra RDP-regler kommer att gemensamt kallas också, ”hello RDP regler” när hello trafik scenario är inte relaterat tooRDP.

Återkalla också att Nätverkssäkerhetsgrupper är på plats för inkommande trafik för internet på hello Frontend och Backend-undernät.

#### <a name="allowed-internet-tooweb-server"></a>(Tillåts) Internet tooWeb Server
1. Internet användaren begär http-sida från SecSvc001.CloudApp.Net (Internet Facing Cloud Service)
2. Cloud service överför trafik via öppna slutpunkt på port 80 toofirewall gränssnittet på 10.0.0.4:80
3. Inga NSG tilldelade tooSecurity undernät så att systemet NSG-reglerna tillåter trafik toofirewall
4. Trafik träffar intern IP-adress för brandvägg hello (10.0.1.4)
5. Brandväggen börjar regeln bearbetning:
   1. FW regel 1 (FW Mgmt) inte gäller flytta toonext regel
   2. FW regler 2-5 (RDP regler) inte tillämpas, flytta toonext regel
   3. FW regel 6 (App: Web) gäller, tillåts trafik, brandvägg NAT den too10.0.1.4 (IIS01)
6. hello klientdel undernät börjar bearbetning av inkommande regel:
   1. NSG regel 1 (blockera Internet) kan inte användas (den här trafiken har NAT skulle av hello brandvägg, vilket hello källadress är nu hello brandvägg som hello säkerhet undernätet och ses av hello klientdel undernät NSG toobe ”lokal” trafik och tillåts därför), flytta toonext regel
   2. Standard NSG-reglerna tillåter toosubnet undernätstrafik, tillåts trafik, stoppa NSG-Regelbearbetning
7. IIS01 lyssnar för webbtrafik, tar emot denna begäran och påbörjar bearbetningen av hello begäran
8. IIS01 försöker tooinitiates tooAppVM01 en FTP-sessionen på Backend-undernät
9. Hej UDR väg för undernätet Frontend gör hello brandväggen hello nästa hopp
10. Inga regler för utgående trafik på undernätet Frontend, tillåts trafik
11. Brandväggen börjar regeln bearbetning:
    1. FW regel 1 (FW Mgmt) inte gäller flytta toonext regel
    2. Inte Använd FW regel 2-5 (RDP regler) flytta toonext regel
    3. FW regel 6 (App: Web) inte tillämpas, flytta toonext regel
    4. FW regel 7 (App: Backend) gäller, tillåts trafik, brandvägg vidarebefordrar trafik too10.0.2.5 (AppVM01)
12. hello Backend-undernät börjar bearbetning av inkommande regel:
    1. NSG regel 1 (blockera Internet) kan inte användas, flytta toonext regel
    2. Standard NSG-reglerna tillåter toosubnet undernätstrafik, tillåts trafik, stoppa NSG-Regelbearbetning
13. AppVM01 tar emot hello begäran och initierar hello-session och svarar
14. Hej UDR väg på Backend-undernät gör hello brandväggen hello nästa hopp
15. Eftersom det finns inga utgående NSG-regler på hello Backend undernät hello svar är tillåtet
16. Eftersom detta returnerar trafik på skickar en upprättad session hello brandvägg hello svar tillbaka toohello webbserver (IIS01)
17. Undernätet frontend börjar bearbetning av inkommande regel:
    1. NSG regel 1 (blockera Internet) kan inte användas, flytta toonext regel
    2. Standard NSG-reglerna tillåter toosubnet undernätstrafik, tillåts trafik, stoppa NSG-Regelbearbetning
18. hello IIS-servern tar emot hello svar är klar hello transaktionen med AppVM01 och slutför sedan skapa hello HTTP-svar, HTTP-svar skickas toohello begärande
19. Eftersom det finns inga utgående NSG-regler på hello klientdel undernät hello svar är tillåtet
20. hello HTTP-svar träffar hello brandvägg och eftersom det är hello svar tooan upprätta NAT session accepteras av hello brandvägg
21. hello brandväggen omdirigerar sedan hello svar tillbaka toohello Internet-användare
22. Eftersom det inte finns några utgående NSG-regler eller UDR hopp hello klientdel undernät hello svaret tillåts och hello Internet-användare får hello webbsida som begärdes.

#### <a name="allowed-internet-rdp-toobackend"></a>(Tillåts) Internet RDP tooBackend
1. Serveradministratören på internet begär RDP-session tooAppVM01 via SecSvc001.CloudApp.Net:8025, där 8025 är hello användartilldelade portnummer för hello ”RDP tooAppVM01” brandväggsregel
2. Hej Molntjänsten skickar trafik via hello öppna slutpunkter på port 8025 toofirewall gränssnittet på 10.0.0.4:8025
3. Inga NSG tilldelade tooSecurity undernät så att systemet NSG-reglerna tillåter trafik toofirewall
4. Brandväggen börjar regeln bearbetning:
   1. FW regel 1 (FW Mgmt) inte gäller flytta toonext regel
   2. FW regel 2 (RDP IIS) inte gäller flytta toonext regel
   3. FW regel 3 (RDP DNS01) inte gäller flytta toonext regel
   4. FW regel 4 (RDP AppVM01) gäller, tillåts trafik, brandvägg NAT den too10.0.2.5:3386 (RDP-porten på AppVM01)
5. hello Backend-undernät börjar bearbetning av inkommande regel:
   1. NSG regel 1 (blockera Internet) kan inte användas (den här trafiken har NAT skulle av hello brandvägg, vilket hello källadress är nu hello brandvägg som hello säkerhet undernätet och ses av hello Backend-undernät NSG toobe ”lokal” trafik och tillåts därför), flytta toonext regel
   2. Standard NSG-reglerna tillåter toosubnet undernätstrafik, tillåts trafik, stoppa NSG-Regelbearbetning
6. AppVM01 lyssnar efter RDP-trafik och svarar
7. Med inga utgående NSG-regler gäller standardregler och returnera trafik tillåts
8. UDR vägar utgående trafik toohello brandväggen som hello nästa hopp
9. Eftersom detta returnerar trafik på skickar en upprättad session hello brandvägg hello svar tillbaka toohello internet-användare
10. RDP-session är aktiverad
11. AppVM01 efterfrågar användarnamn lösenord

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(Tillåts) Web Server DNS-sökning på DNS-server
1. Web Server, IIS01, måste en datafeed på www.data.gov, men måste tooresolve hello adress.
2. hello nätverkskonfiguration för hello VNet listor DNS01 (10.0.2.4 på hello Backend-undernät) som hello primära DNS-server, IIS01 skickar hello DNS-begäran tooDNS01
3. UDR vägar utgående trafik toohello brandväggen som hello nästa hopp
4. Inga utgående NSG-reglerna är bundna toohello klientdel undernät, tillåts trafik
5. Brandväggen börjar regeln bearbetning:
   1. FW regel 1 (FW Mgmt) inte gäller flytta toonext regel
   2. Inte Använd FW regel 2-5 (RDP regler) flytta toonext regel
   3. FW reglerna 6 och 7 (Appregler) inte tillämpas, flytta toonext regel
   4. FW regeln 8 (tooInternet) inte gäller flytta toonext regel
   5. FW regel 9 (DNS) gäller, tillåts trafik, brandvägg vidarebefordrar trafik too10.0.2.4 (DNS01)
6. hello Backend-undernät börjar bearbetning av inkommande regel:
   1. NSG regel 1 (blockera Internet) kan inte användas, flytta toonext regel
   2. Standard NSG-reglerna tillåter toosubnet undernätstrafik, tillåts trafik, stoppa NSG-Regelbearbetning
7. DNS-servern tar emot hello begäran
8. DNS-servern inte har cachelagrade hello-adress och begär en rot-DNS-servern på hello internet
9. UDR vägar utgående trafik toohello brandväggen som hello nästa hopp
10. Inga utgående NSG-regler på Backend-undernät, tillåts trafik
11. Brandväggen börjar regeln bearbetning:
    1. FW regel 1 (FW Mgmt) inte gäller flytta toonext regel
    2. Inte Använd FW regel 2-5 (RDP regler) flytta toonext regel
    3. FW reglerna 6 och 7 (Appregler) inte tillämpas, flytta toonext regel
    4. FW regeln 8 (tooInternet) gäller, tillåts trafik, sessionen är SNAT ut tooroot DNS-server i hello Internet
12. Internet-DNS-servern svarar, eftersom denna session har startats från hello brandvägg hello svar accepteras av hello-brandväggen
13. Eftersom det är en upprättad session vidarebefordrar hello brandväggen hello svar toohello initierar DNS01-servern
14. hello Backend-undernät börjar bearbetning av inkommande regel:
    1. NSG regel 1 (blockera Internet) kan inte användas, flytta toonext regel
    2. Standard NSG-reglerna tillåter toosubnet undernätstrafik, tillåts trafik, stoppa NSG-Regelbearbetning
15. hello DNS-servern tar emot och cachelagrar hello svaret och svarar toohello första begäran tillbaka tooIIS01
16. Hej UDR väg på backend-undernät gör hello brandväggen hello nästa hopp
17. Det finns några utgående NSG-regler på hello Backend-undernät, tillåts trafik
18. Det här är en upprättad session hello brandväggen, hello svaret vidarebefordras av hello brandväggen tillbaka toohello IIS-servern
19. Undernätet frontend börjar bearbetning av inkommande regel:
    1. Det finns ingen NSG-regel som gäller tooInbound trafik från hello Backend undernät toohello undernätet Frontend, så ingen hälsningspaket NSG-regler gäller
    2. hello system Standardregeln för att tillåta trafik mellan undernät skulle göra att den här trafiken så hello trafik tillåts
20. IIS01 får hello svar från DNS01

#### <a name="allowed-backend-server-toofrontend-server"></a>(Tillåts) Serverdelen server tooFrontend
1. En administratör som har loggat in tooAppVM02 via RDP begär en fil direkt från hello IIS01 server via Utforskaren i windows
2. Hej UDR väg på Backend-undernät gör hello brandväggen hello nästa hopp
3. Eftersom det finns inga utgående NSG-regler på hello Backend undernät hello svar är tillåtet
4. Brandväggen börjar regeln bearbetning:
   1. FW regel 1 (FW Mgmt) inte gäller flytta toonext regel
   2. Inte Använd FW regel 2-5 (RDP regler) flytta toonext regel
   3. FW reglerna 6 och 7 (Appregler) inte tillämpas, flytta toonext regel
   4. FW regeln 8 (tooInternet) inte gäller flytta toonext regel
   5. FW regel 9 (DNS) inte gäller flytta toonext regel
   6. FW regeln 10 (Intra-undernät) gäller, tillåts trafik, brandvägg skickar trafik too10.0.1.4 (IIS01)
5. Undernätet frontend börjar bearbetning av inkommande regel:
   1. NSG regel 1 (blockera Internet) kan inte användas, flytta toonext regel
   2. Standard NSG-reglerna tillåter toosubnet undernätstrafik, tillåts trafik, stoppa NSG-Regelbearbetning
6. Under förutsättning att korrekt autentisering och auktorisering, IIS01 accepterar hello begäran och svarar
7. Hej UDR väg för undernätet Frontend gör hello brandväggen hello nästa hopp
8. Eftersom det finns inga utgående NSG-regler på hello klientdel undernät hello svar är tillåtet
9. Eftersom det är en befintlig session hello Brandvägg för det här svaret tillåts och hello brandväggen returnerar hello svar tooAppVM02
10. Backend-undernät börjar bearbetning av inkommande regel:
    1. NSG regel 1 (blockera Internet) kan inte användas, flytta toonext regel
    2. Standard NSG-reglerna tillåter toosubnet undernätstrafik, tillåts trafik, stoppa NSG-Regelbearbetning
11. AppVM02 tar emot hello svar

#### <a name="denied-internet-direct-tooweb-server"></a>(Nekad) Internet direkt tooWeb Server
1. Internet-användare försöker tooaccess hello webbservern, IIS01, via hello FrontEnd001.CloudApp.Net service
2. Eftersom det inte finns några slutpunkter som är öppen för HTTP-trafik, detta skulle inte passerar hello Molntjänsten och skulle nå hello-server
3. Om hello slutpunkter öppna av någon anledning skulle hello NSG (blockera Internet) på undernätet för hello Frontend blockera den här trafiken
4. Slutligen hello klientdel undernät UDR väg skulle skicka all utgående trafik från IIS01 toohello brandvägg som hello nästa hopp och hello brandväggen skulle se detta som en asymmetrisk trafik och släpp hello utgående svar därför nås det finns minst tre oberoende lager i försvar mellan hello internet och IIS01 via dess Molntjänsten hindrar obehöriga/olämplig åtkomst.

#### <a name="denied-internet-toobackend-server"></a>(Nekad) Internet tooBackend Server
1. Internet-användare försöker tooaccess en fil på AppVM01 via hello BackEnd001.CloudApp.Net service
2. Eftersom det inte finns några slutpunkter som är öppna för filresursen, detta skulle inte klarar hello Molntjänsten och skulle nå hello-server
3. Om hello slutpunkter öppna av någon anledning skulle hello NSG (blockera Internet) blockera den här trafiken
4. Slutligen hello UDR väg skulle skicka all utgående trafik från AppVM01 toohello brandvägg som hello nästa hopp och hello brandväggen skulle se detta som en asymmetrisk trafik och släpp hello utgående svar därför nås det finns minst tre oberoende lager i skyddsstrategierna mellan Hej internet och AppVM01 via dess Molntjänsten hindrar obehöriga/olämplig åtkomst.

#### <a name="denied-frontend-server-toobackend-server"></a>(Nekad) Frontend-server tooBackend Server
1. Anta IIS01 har drabbats och kör skadlig kod som försöker tooscan hello-backendservrar undernät.
2. hello klientdel undernät UDR väg skulle skicka all utgående trafik från IIS01 toohello brandvägg som hello nästa hopp. Detta är inte något som kan ändras genom hello komprometteras VM.
3. hello brandväggen skulle behandla hello trafik om hello begäran var tooAppVM01 eller toohello DNS-server för DNS-sökning trafik potentiellt tillåts av hello brandvägg (förfaller tooFW regler 7 och 9). All annan trafik skulle blockeras av FW regel 11 (neka alla).
4. Om avancerade hotidentifiering har aktiverats på hello-brandväggen (som inte omfattas i det här dokumentet, se hello leverantörens dokumentation för din specifika nätverksenhet advanced threat funktioner), även trafik som skulle vara tillåten av hello grundläggande vidarebefordran regler beskrivs i detta dokument kan förhindras om hello trafik finns kända signaturer eller mönster som flaggan en regel för avancerade hot.

#### <a name="denied-internet-dns-lookup-on-dns-server"></a>(Nekad) Internet-DNS-sökning på DNS-server
1. Internet-användare försöker toolookup en intern DNS-post på DNS01 via BackEnd001.CloudApp.Net-tjänsten 
2. Eftersom det inte finns några slutpunkter som är öppen för DNS-trafik, detta skulle inte passerar hello Molntjänsten och skulle nå hello-server
3. Om hello slutpunkter öppna av någon anledning skulle hello NSG regel (blockera Internet) på undernätet för hello Frontend blockera den här trafiken
4. Slutligen hello serverdelsvägar undernät UDR skickar all utgående trafik från DNS01 toohello brandvägg som hello nästa hopp och hello brandväggen skulle se detta som en asymmetrisk trafik och släpp hello utgående svar därför nås det finns minst tre oberoende lager i försvar mellan hello internet och DNS01 via dess Molntjänsten hindrar obehöriga/olämplig åtkomst.

#### <a name="denied-internet-toosql-access-through-firewall"></a>(Nekad) TooSQL Internetåtkomst via brandvägg
1. Internet-användare begär SQL-data från SecSvc001.CloudApp.Net (Internet Facing Cloud Service)
2. Eftersom inga slutpunkter är öppen för SQL är detta skulle inte klarar hello Molntjänsten och skulle nå hello-brandväggen
3. Om SQL-slutpunkter öppna av någon anledning skulle hello brandväggen börja regeln bearbetning:
   1. FW regel 1 (FW Mgmt) inte gäller flytta toonext regel
   2. FW regler 2-5 (RDP regler) inte tillämpas, flytta toonext regel
   3. FW regel 6 och 7 (programmet regler) inte tillämpas, flytta toonext regel
   4. FW regeln 8 (tooInternet) inte gäller flytta toonext regel
   5. FW regel 9 (DNS) inte gäller flytta toonext regel
   6. FW regeln 10 (Intra-undernät) inte gäller flytta toonext regel
   7. FW regel 11 (neka alla) gäller, trafik är blockerad, stoppa regel bearbetning

## <a name="references"></a>Referenser
### <a name="main-script-and-network-config"></a>Huvudskriptet och nätverkskonfiguration
Spara hello fullständig skript i ett PowerShell-skriptfil. Spara hello nätverkskonfiguration till en fil med namnet ”NetworkConf2.xml”.
Ändra hello användardefinierade variabler. Kör hello skript och följer hello brandväggen regeln installationsprogrammet anvisningarna ovan.

#### <a name="full-script"></a>Fullständig skript
Det här skriptet kommer utifrån hello användardefinierade variabler:

1. Ansluta tooan Azure-prenumeration
2. Skapa ett nytt lagringskonto
3. Skapa ett nytt virtuellt nätverk och tre undernät som har definierats i konfigurationsfilen för hello nätverk
4. Skapa fem virtuella maskiner; Brandvägg för 1 och 4 windows server virtuella datorer
5. Konfigurera UDR inklusive:
   1. Skapa två nya vägtabeller
   2. Lägga till vägar toohello tabeller
   3. Binda tabeller tooappropriate undernät
6. Aktivera IP-vidarebefordring på hello NVA
7. Konfigurera NSG inklusive:
   1. Skapa en NSG
   2. Lägger till en regel
   3. Bindningen hello NSG toohello lämpliga undernät

Detta PowerShell-skript ska köras lokalt på en internet-ansluten dator eller server.

> [!IMPORTANT]
> När du kör det här skriptet kanske varningar eller andra informationsmeddelanden som visas i PowerShell. Endast felmeddelanden i rött är orsaken till problem.
> 
> 

    <# 
     .SYNOPSIS
      Example of DMZ and User Defined Routing in an isolated network (Azure only, no hybrid connections)

     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Three new cloud services
       - Three Subnets (SecNet, FrontEnd, and BackEnd subnets)
       - A Network Virtual Appliance (NVA), in this case a Barracuda NextGen Firewall
       - One server on hello FrontEnd Subnet
       - Three Servers on hello BackEnd Subnet
       - IP Forwading from hello FireWall out toohello internet
       - User Defined Routing FrontEnd and BackEnd Subnets toohello NVA

      Before running script, ensure hello network configuration file is created in
      hello directory referenced by $NetworkConfigFile variable (or update the
      variable tooreflect hello path and file name of hello config file being used).

     .Notes
      Everyone's security requirements are different and can be addressed in a myriad of ways.
      Please be sure that any sensitive data or applications are behind hello appropriate
      layer(s) of protection. This script serves as an example of some of hello techniques
      that can be used, but should not be used for all scenarios. You are responsible to
      assess your security needs and hello appropriate protections needed, and then effectively
      implement those protections.

      Security Service (SecNet subnet 10.0.0.0/24)
       myFirewall - 10.0.0.4

      FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
       IIS01      - 10.0.1.4

      BackEnd Service (BackEnd subnet 10.0.2.0/24)
       DNS01      - 10.0.2.4
       AppVM01    - 10.0.2.5
       AppVM02    - 10.0.2.6

    #>

    # Fixed Variables
        $LocalAdminPwd = Read-Host -Prompt "Enter Local Admin Password toobe used for all VMs"
        $VMName = @()
        $ServiceName = @()
        $VMFamily = @()
        $img = @()
        $size = @()
        $SubnetName = @()
        $VMIP = @()

    # User Defined Global Variables
      # These should be changes tooreflect your subscription and services
      # Invalid options will fail in hello validation section

      # Subscription Access Details
        $subID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"

      # VM Account, Location, and Storage Details
        $LocalAdmin = "theAdmin"
        $DeploymentLocation = "Central US"
        $StorageAccountName = "vmstore02"

      # Service Details
        $SecureService = "SecSvc001"
        $FrontEndService = "FrontEnd001"
        $BackEndService = "BackEnd001"

      # Network Details
        $VNetName = "CorpNetwork"
        $VNetPrefix = "10.0.0.0/16"
        $SecNet = "SecNet"
        $FESubnet = "FrontEnd"
        $FEPrefix = "10.0.1.0/24"
        $BESubnet = "BackEnd"
        $BEPrefix = "10.0.2.0/24"
        $NetworkConfigFile = "C:\Scripts\NetworkConf3.xml"

      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
        $FWImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Barracuda NextGen Firewall'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}

      # UDR Details
        $FERouteTableName = "FrontEndSubnetRouteTable"
        $BERouteTableName = "BackEndSubnetRouteTable"

      # NSG Details
        $NSGName = "MyVNetSG"

    # User Defined VM Specific Config
        # Note: tooensure UDR and IP forwarding is setup
        # properly this script requires VM 0 be hello NVA.

        # VM 0 - hello Network Virtual Appliance (NVA)
          $VMName += "myFirewall"
          $ServiceName += $SecureService
          $VMFamily += "Firewall"
          $img += $FWImg
          $size += "Small"
          $SubnetName += $SecNet
          $VMIP += "10.0.0.4"

        # VM 1 - hello Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.4"

        # VM 2 - hello First Appliaction Server
          $VMName += "AppVM01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.5"

        # VM 3 - hello Second Appliaction Server
          $VMName += "AppVM02"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.6"

        # VM 4 - hello DNS Server
          $VMName += "DNS01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.4"

    # ----------------------------- #
    # No User Defined Varibles or   #
    # Configuration past this point #
    # ----------------------------- #

      # Get your Azure accounts
        Add-AzureAccount
        Set-AzureSubscription –SubscriptionId $subID -ErrorAction Stop
        Select-AzureSubscription -SubscriptionId $subID -Current -ErrorAction Stop

      # Create Storage Account
        If (Test-AzureName -Storage -Name $StorageAccountName) { 
            Write-Host "Fatal Error: This storage account name is already in use, please pick a diffrent name." -ForegroundColor Red
            Return}
        Else {Write-Host "Creating Storage Account" -ForegroundColor Cyan 
              New-AzureStorageAccount -Location $DeploymentLocation -StorageAccountName $StorageAccountName}

      # Update Subscription Pointer tooNew Storage Account
        Write-Host "Updating Subscription Pointer tooNew Storage Account" -ForegroundColor Cyan 
        Set-AzureSubscription –SubscriptionId $subID -CurrentStorageAccountName $StorageAccountName -ErrorAction Stop

    # Validation
    $FatalError = $false

    If (-Not (Get-AzureLocation | Where {$_.DisplayName -eq $DeploymentLocation})) {
         Write-Host "This Azure Location was not found or available for use" -ForegroundColor Yellow
         $FatalError = $true}

    If (Test-AzureName -Service -Name $SecureService) { 
        Write-Host "hello SecureService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "hello FrontEndService service name is valid for use." -ForegroundColor Green}

    If (Test-AzureName -Service -Name $FrontEndService) { 
        Write-Host "hello FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "hello FrontEndService service name is valid for use" -ForegroundColor Green}

    If (Test-AzureName -Service -Name $BackEndService) { 
        Write-Host "hello BackEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "hello BackEndService service name is valid for use." -ForegroundColor Green}

    If (-Not (Test-Path $NetworkConfigFile)) { 
        Write-Host 'hello network config file was not found, please update hello $NetworkConfigFile variable toopoint toohello network config xml file.' -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "hello network config file was found" -ForegroundColor Green
            If (-Not (Select-String -Pattern $DeploymentLocation -Path $NetworkConfigFile)) {
                Write-Host 'hello deployment location was not found in hello network config file, please check hello network config file tooensure hello $DeploymentLocation varible is correct and hello netowrk config file matches.' -ForegroundColor Yellow
                $FatalError = $true}
            Else { Write-Host "hello deployment location was found in hello network config file." -ForegroundColor Green}}

    If ($FatalError) {
        Write-Host "A fatal error has occured, please see hello above messages for more information." -ForegroundColor Red
        Return}
    Else { Write-Host "Validation passed, now building hello environment." -ForegroundColor Green}

    # Create VNET
        Write-Host "Creating VNET" -ForegroundColor Cyan 
        Set-AzureVNetConfig -ConfigurationPath $NetworkConfigFile -ErrorAction Stop

    # Create Services
        Write-Host "Creating Services" -ForegroundColor Cyan
        New-AzureService -Location $DeploymentLocation -ServiceName $SecureService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $FrontEndService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $BackEndService -ErrorAction Stop

    # Build VMs
        $i=0
        $VMName | Foreach {
            Write-Host "Building $($VMName[$i])" -ForegroundColor Cyan
            If ($VMFamily[$i] -eq "Firewall") 
                { 
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Linux -LinuxUser $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                # Set up all hello EndPoints we'll need once we're up and running
                # Note: All traffic goes through hello firewall, so we'll need tooset up all ports here.
                #       Also, hello firewall will be redirecting traffic tooa new IP and Port in a forwarding
                #       rule, so all of these endpoint have hello same public and local port and hello firewall
                #       will do hello mapping, NATing, and/or redirection as declared in hello firewall rules.
                Add-AzureEndpoint -Name "MgmtPort1" -Protocol tcp -PublicPort 801  -LocalPort 801  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "MgmtPort2" -Protocol tcp -PublicPort 807  -LocalPort 807  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "HTTP"      -Protocol tcp -PublicPort 80   -LocalPort 80   -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPWeb"    -Protocol tcp -PublicPort 8014 -LocalPort 8014 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp1"   -Protocol tcp -PublicPort 8025 -LocalPort 8025 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp2"   -Protocol tcp -PublicPort 8026 -LocalPort 8026 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPDNS01"  -Protocol tcp -PublicPort 8024 -LocalPort 8024 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                # Note: A SSH endpoint is automatically created on port 22 when hello appliance is created.
                }
            Else
                {
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                    Remove-AzureEndpoint -Name "RemoteDesktop" | `
                    Remove-AzureEndpoint -Name "PowerShell" | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                }
            $i++
        }

    # Configure UDR and IP Forwarding
        Write-Host "Configuring UDR" -ForegroundColor Cyan

      # Create hello Route Tables
        Write-Host "Creating hello Route Tables" -ForegroundColor Cyan 
        New-AzureRouteTable -Name $BERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $BESubnet subnet"
        New-AzureRouteTable -Name $FERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $FESubnet subnet"

      # Add Routes tooRoute Tables
        Write-Host "Adding Routes toohello Route Tables" -ForegroundColor Cyan 
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "All traffic tooFW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic tooFW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
            -NextHopType VNETLocal
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "All traffic tooFW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic tooFW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $FEPrefix `
            -NextHopType VNETLocal

      # Assoicate hello Route Tables with hello Subnets
        Write-Host "Binding Route Tables toohello Subnets" -ForegroundColor Cyan 
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $BESubnet `
            -RouteTableName $BERouteTableName
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $FESubnet `
            -RouteTableName $FERouteTableName

     # Enable IP Forwarding on hello Virtual Appliance
        Get-AzureVM -Name $VMName[0] -ServiceName $ServiceName[0] `
            |Set-AzureIPForwarding -Enable

    # Configure NSG
        Write-Host "Configuring hello Network Security Group (NSG)" -ForegroundColor Cyan

      # Build hello NSG
        Write-Host "Building hello NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"

      # Add NSG Rule
        Write-Host "Writing rules into hello NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate hello $VNetName VNet from hello Internet" -Type Inbound -Priority 100 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
            -Protocol *

      # Assign hello NSG tootwo Subnets
        # hello NSG is *not* bound toohello Security Subnet. hello result
        # is that internet traffic flows only toohello Security subnet
        # since hello NSG bound toohello Frontend and Backback subnets
        # will Deny internet traffic toothose subnets.
        Write-Host "Binding hello NSG tootwo subnets" -ForegroundColor Cyan
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $FESubnet -VirtualNetworkName $VNetName
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $BESubnet -VirtualNetworkName $VNetName

    # Optional Post-script Manual Configuration
      # Configure Firewall
      # Install Test Web App (Run Post-Build Script on hello IIS Server)
      # Install Backend resource (Run Post-Build Script on hello AppVM01)
      Write-Host
      Write-Host "Build Complete!" -ForegroundColor Green
      Write-Host
      Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
      Write-Host " - Configure Firewall" -ForegroundColor Gray
      Write-Host " - Install Test Web App (Run Post-Build Script on hello IIS Server)" -ForegroundColor Gray
      Write-Host " - Install Backend resource (Run Post-Build Script on hello AppVM01)" -ForegroundColor Gray
      Write-Host


#### <a name="network-config-file"></a>Konfigurationsfilen för nätverk
Spara XML-filen med uppdaterad plats och Lägg till hello länken toothis filen toohello $NetworkConfigFile variabeln i hello skriptet ovan.

    <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
      <VirtualNetworkConfiguration>
        <Dns>
          <DnsServers>
            <DnsServer name="DNS01" IPAddress="10.0.2.4" />
            <DnsServer name="Level3" IPAddress="209.244.0.3" />
          </DnsServers>
        </Dns>
        <VirtualNetworkSites>
          <VirtualNetworkSite name="CorpNetwork" Location="Central US">
            <AddressSpace>
              <AddressPrefix>10.0.0.0/16</AddressPrefix>
            </AddressSpace>
            <Subnets>
              <Subnet name="SecNet">
                <AddressPrefix>10.0.0.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="FrontEnd">
                <AddressPrefix>10.0.1.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="BackEnd">
                <AddressPrefix>10.0.2.0/24</AddressPrefix>
              </Subnet>
            </Subnets>
            <DnsServersRef>
              <DnsServerRef name="DNS01" />
              <DnsServerRef name="Level3" />
            </DnsServersRef>
          </VirtualNetworkSite>
        </VirtualNetworkSites>
      </VirtualNetworkConfiguration>
    </NetworkConfiguration>

#### <a name="sample-application-scripts"></a>Exempelskript för programmet
Om du vill tooinstall ett exempelprogram för det här och andra DMZ exempel kan en har angetts på hello följande länk: [exempelskript för programmet][SampleApp]

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3design.png "Dubbelriktad DMZ med NVA, NSG och UDR"
[2]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3firewalllogical.png "Logisk vy för hello brandväggsregler"
[3]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectfrontend.png "Skapa en FrontEnd-objektet"
[4]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectdns.png "Skapa en DNS-Server-objekt"
[5]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpa.png "Kopia av RDP standardregel"
[6]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpb.png "AppVM01 regel"
[7]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconapplicationredirect.png "Omdirigerings-ikon"
[8]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/icondestinationnat.png "Mål NAT-ikon"
[9]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconpass.png "Skicka ikon"
[10]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulefirewall.png "Hantering av brandväggsregel"
[11]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulerdp.png "RDP-brandväggsregel"
[12]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleweb.png "Web brandväggsregel"
[13]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleappvm01.png "AppVM01 brandväggsregel"
[14]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleoutbound.png "Utgående brandväggsregel"
[15]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledns.png "DNS-brandväggsregel"
[16]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleintravnet.png "Inom VNet brandväggsregel"
[17]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledeny.png "Regel för att neka brandväggen"
[18]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/firewallruleactivate.png "Brandväggen regeln aktivering"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
