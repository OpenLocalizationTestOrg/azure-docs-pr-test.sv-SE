---
title: "aaaDMZ exempel – skapar en DMZ tooprotect program med en brandvägg och NSG: er | Microsoft Docs"
description: "Skapa en DMZ med en brandvägg och Nätverkssäkerhetsgrupper (NSG)"
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: 
ms.assetid: c78491c7-54ac-4469-851c-b35bfed0f528
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/01/2016
ms.author: jonor;sivae
ms.openlocfilehash: 18f116dc3897567bff14a509ae8c13f449182bfb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="example-2--build-a-dmz-tooprotect-applications-with-a-firewall-and-nsgs"></a>Exempel 2 – Skapa en DMZ tooprotect program med en brandvägg och NSG: er
[Returnera toohello gräns bästa praxis sidan][HOME]

Det här exemplet skapar en DMZ med en brandvägg, fyra windows-servrar och Nätverkssäkerhetsgrupper. Den kommer också att gå igenom varje hello relevanta kommandon tooprovide en bättre förståelse för varje steg. Det finns också en trafik scenariot avsnittet tooprovide en djupgående steg för steg hur trafik fortsätter via hello lager i skyddsstrategierna hello DMZ. Slutligen är i referensavsnittet hello hello fullständiga koden och instruktion toobuild den här miljön tootest och experimentera med olika scenarier. 

![Inkommande DMZ med NVA och NSG][1]

## <a name="environment-description"></a>Beskrivning av miljö
I det här exemplet finns det en prenumeration som innehåller hello följande:

* Två molntjänster: ”FrontEnd001” och ”BackEnd001”
* Ett virtuellt nätverk ”CorpNetwork” med två undernät: ”FrontEnd” och ”BackEnd”
* En enda Nätverkssäkerhetsgrupp som är kopplade tooboth undernät
* En virtuell nätverksenhet, i det här exemplet Barracuda nästa generation brandvägg, anslutna toohello klientdel undernät
* En Windows-Server som representerar en program-webbserver (”IIS01”)
* Två windows-servrar som representerar tillbaka programmet avslutas servrar (”AppVM01”, ”AppVM02”)
* En Windows-server som representerar en DNS-server (”DNS01”)

> [!NOTE]
> Även om det här exemplet används en Barracuda nästa generation brandvägg många hello olika virtuella nätverksenheter kan användas för det här exemplet.
> 
> 

I hello referensavsnittet nedan finns ett PowerShell-skript som skapar de flesta av hello miljön som beskrivs ovan. Skapa hello virtuella datorer och virtuella nätverk, som även om utförs av hello exempelskriptet inte beskrivs i detalj i detta dokument.

toobuild hello miljö:

1. Spara hello nätverk XML-konfigurationsfilen finns i hello referenser avsnitt (uppdaterade med namn, plats och IP-adresser toomatch hello angivna scenario)
2. Uppdatera hello Användarvariabler i hello skriptet toomatch hello miljö hello skript är toobe körs mot (prenumerationer, tjänstnamn osv.)
3. Kör hello-skriptet i PowerShell

**Obs**: hello region som visas i hello PowerShell-skriptet måste matcha hello region som visas i hello nätverket xml-konfigurationsfilen.

När hello skriptet har körts vidtas hello efter skriptet följande:

1. Konfigurera brandväggsregler för hello detta beskrivs i hello avsnittet nedan: brandväggsregler.
2. Du kan också är hello referensavsnittet två skript tooset hello webbservern och app-servern med en enkel web application tooallow testning med den här konfigurationen DMZ.

hello nästa avsnitt beskrivs de flesta av hello skript instruktioner relativa tooNetwork säkerhetsgrupper.

## <a name="network-security-groups-nsg"></a>Nätverkssäkerhetsgrupper (NSG)
I det här exemplet bygger en NSG-grupp och sedan in med sex regler. 

> [!TIP]
> Generellt sett bör du först skapa specifika ”Tillåt” regler och hello generisk ”Deny” regler senast. hello prioritet avgör vilka regler som utvärderas först. När trafiken hittas tooapply tooa specifik regel, utvärderas inga fler regler. NSG-regler kan använda antingen i hello i inkommande eller utgående riktning (ur hello hello undernät).
> 
> 

Deklarativt, byggs hello följande regler för inkommande trafik:

1. Intern DNS-trafik (port 53) tillåts
2. RDP-trafik (port 3389) från hello Internet tooany VM tillåts
3. HTTP-trafik (port 80) från hello Internet toohello NVA (brandvägg) tillåts
4. All trafik (alla portar) från IIS01 tooAppVM1 tillåts
5. All trafik (alla portar) från hello Internet toohello hela VNet (båda undernäten) nekas
6. All trafik (alla portar) från hello klientdel undernät toohello Backend-undernät nekas

Med dessa regler bundna tooeach undernät, om en HTTP-begäran var inkommande trafik från hello webbserver för Internet toohello, både regler 3 (Tillåt) och 5 (neka) är skulle ha använts, men eftersom regel 3 har högre prioritet bara den skulle tillämpas och regel 5 inte skulle komma till användning. Hello HTTP-begäran skulle därför tillåtna toohello brandväggen. Om samma trafiken har tooreach hello DNS01 server, skulle regel 5 (neka) vara hello första tooapply och hello trafik skulle inte toopass toohello server. Regel 6 (neka) block hello undernätet Frontend från pratar toohello Backend-undernät (förutom tillåten trafik i regler 1 och 4), detta skyddar hello Backend-nätverket om en angripare kompromisser hello webbprogram på hello klientdel, hello angriparen begränsad åtkomst toohello Backend ”skyddad” nätverket (endast tooresources som visas på hello AppVM01 server).

Det finns en utgående Standardregeln som tillåter trafik ut toohello internet. I det här exemplet vi att tillåta utgående trafik och inte ändra de utgående reglerna. toolock ned i båda riktningarna användaren definierade routning krävs, detta är utforskade i ett annat exempel finns i hello [huvudsakliga säkerhet gräns dokumentet][HOME].

hello ovan beskrivs NSG-regler är mycket lik toohello NSG-regler i [exempel 1 - skapa en enkel DMZ med NSG: er][Example1]. Granska hello NSG beskrivning i detta dokument för en närmare titt på varje NSG regel och dess attribut.

## <a name="firewall-rules"></a>Brandväggsregler
Management-klienten ska måste toobe installerad på en dator toomanage hello brandvägg och skapa hello-konfigurationer som krävs. Se hello dokumentationen från brandväggen (eller andra NVA) leverantör på hur toomanage hello enhet. hello resten av det här avsnittet beskriver hello konfigurationen av hello brandvägg, via hello leverantörer management-klienten (d.v.s. inte hello Azure-portalen eller PowerShell).

Instruktioner för att klienten ska ladda ned och anslutande toohello Barracuda som används i det här exemplet finns här: [Barracuda NG Admin](https://techlib.barracuda.com/NG61/NGAdmin)

Hello Brandvägg för måste vidarebefordran regler toobe skapas. Eftersom det här exemplet skickar endast internet-trafik inkommande toohello brandväggen och sedan toohello webbservern, behövs bara en vidarebefordran NAT-regeln. På hello Barracuda nästa generation brandväggen som används i det här exemplet hello är regel en mål-NAT-regel (”Dst NAT”) toopass den här trafiken.

toocreate hello följande regel (eller verifiera befintliga standardregler) från hello Barracuda NG Admin klienten instrumentpanelen navigera toohello konfigurationsfliken klickar du på i hello användningsinställningar avsnittet RuleSet-metod. Ett rutnät som kallas visar ”Main regler” hello befintliga aktiva och inaktiva regler hello-brandväggen. Hello övre högra hörnet i det här rutnätet är en liten, grön ”+”, klicka på den här toocreate en ny regel (Obs: brandväggen kan vara i ”låst” för ändringar, om du ser en knapp markerad ”låsa” och du är toocreate eller redigera regler, klicka på den här knappen för ”låsa upp” Hej regeluppsättning och tillåta redigering). Om du inte vill tooedit en befintlig regel, Välj regeln, högerklicka och välj Redigera regel.

Skapa en ny regel och ange ett namn, till exempel ”WebTraffic”. 

ikonen för hello mål NAT-regel som ser ut så här: ![mål NAT-ikon][2]

själva hello regeln skulle se ut ungefär så här:

![Brandväggsregel][3]

Här några inkommande adress att träffar hello brandväggen försök tooreach HTTP (port 80 eller 443 för HTTPS) kommer att skickas hello brandväggens ”DHCP1 lokala-IP-gränssnittet och omdirigerade toohello webbserver med hello 10.0.1.5 IP-adress. Eftersom hello trafik kommer in på port 80 och pågående toohello webbserver på port 80 krävs ingen portändring av. Dock hello mållistan ha varit 10.0.1.5:8080 om våra webbservern lyssnar på port 8080 därför översattes hello inkommande port 80 på hello brandväggen tooinbound port 8080 på hello webbserver.

Anslutningsmetod bör också vara visas, för hello mål regeln från hello Internet, ”dynamisk SNAT” som passar bäst. 

Även om bara en regel har skapats är det viktigt att dess prioritet är korrekt. Om i hello rutnät för alla regler hello Brandvägg för den nya regeln finns på hello nedre (under hello ”BLOCKALL” regel) kommer den aldrig till play. Kontrollera hello nyligen skapade regeln för webbtrafik ovan hello BLOCKALL regeln.

När hello regeln har skapats måste det vara pushas toohello brandväggen och sedan aktiveras om det inte görs hello regeln ändringen ska börja gälla. hello push och aktivering processen beskrivs i nästa avsnitt om hello.

## <a name="rule-activation"></a>Regeln aktivering
Med hello ruleset ändras tooadd regeln kan hello RuleSet-metod måste vara överförs toohello brandvägg och aktiverat.

![Brandväggen regeln aktivering][4]

Är ett kluster på knapparna i hello övre högra hörnet av hello management-klienten. Hello ”skicka ändringar” knappen toosend hello ändrade regler toohello brandväggen Klicka på knappen för hello ”aktivera”.

Det här exemplet miljö bygget är klar med hello aktivering hello brandväggen RuleSet-metod. Du kan också hello post build skript i hello referenser som kan vara kör tooadd ett program toothis miljö tootest hello nedan trafik scenarier.

> [!IMPORTANT]
> Det är kritiska toorealize att du inte kommer träffa hello webbservern direkt. När en webbläsare begär en HTTP-sida från FrontEnd001.CloudApp.Net, webbserver hello http-slutpunkt (port 80) överför den här trafiken toohello brandväggen inte hello. Hej brandväggen sedan enligt regel toohello skapade ovan NAT som begär toohello webbservern.
> 
> 

## <a name="traffic-scenarios"></a>Trafik scenarier
#### <a name="allowed-web-tooweb-server-through-firewall"></a>(Tillåts) Web tooWeb servern via brandväggen
1. Internet användaren begär http-sida från FrontEnd001.CloudApp.Net (Internet Facing Cloud Service)
2. Cloud service överför trafik via öppna slutpunkter på port 80 toofirewall lokala gränssnittet på 10.0.1.4:80
3. Undernätet frontend börjar bearbetning av inkommande regel:
   1. NSG regel 1 (DNS) inte gäller flytta toonext regel
   2. NSG regel 2 (RDP) inte gäller flytta toonext regel
   3. NSG regel 3 (Internet tooFirewall) gäller, trafik är tillåtna, stoppa regel bearbetning
4. Trafik träffar intern IP-adress för brandvägg hello (10.0.1.4)
5. Vidarebefordringsregel för av brandväggen finns detta trafik på port 80, omdirigerar det toohello webbservern IIS01
6. IIS01 lyssnar för webbtrafik, tar emot denna begäran och påbörjar bearbetningen av hello begäran
7. IIS01 begär hello SQL Server på AppVM01 information
8. Inga regler för utgående trafik på undernätet Frontend, tillåts trafik
9. hello Backend-undernät börjar bearbetning av inkommande regel:
   1. NSG regel 1 (DNS) inte gäller flytta toonext regel
   2. NSG regel 2 (RDP) inte gäller flytta toonext regel
   3. NSG regel 3 (Internet tooFirewall) inte gäller flytta toonext regel
   4. NSG regel 4 (IIS01 tooAppVM01) gäller, trafik är tillåtna, stoppa regel bearbetning
10. AppVM01 får hello SQL-fråga och svarar
11. Eftersom det finns inga regler för utgående trafik på hello Backend undernät hello svar är tillåtet
12. Undernätet frontend börjar bearbetning av inkommande regel:
    1. Det finns ingen NSG-regel som gäller tooInbound trafik från hello Backend undernät toohello undernätet Frontend, så ingen hälsningspaket NSG-regler gäller
    2. hello system Standardregeln för att tillåta trafik mellan undernät att den här trafiken så hello trafik tillåts.
13. hello IIS-servern tar emot hello SQL svar och slutför hello HTTP-svar och skickar toohello begärande
14. Eftersom det är en NAT-session från hello brandvägg är hello svar målet (ursprungligen) för hello brandväggen
15. hello brandväggen får hello svar från hello webbservern och vidarebefordrar tillbaka toohello Internet-användare
16. Eftersom det finns inga regler för utgående trafik på hello klientdel undernät hello svaret tillåts och hello Internet-användare får hello webbsida som begärdes.

#### <a name="allowed-rdp-toobackend"></a>(Tillåts) RDP-tooBackend
1. Serveradministratören på internet begär RDP-session tooAppVM01 på BackEnd001.CloudApp.Net:xxxxx där xxxxx är hello slumpmässigt tilldelad portnummer för RDP-tooAppVM01 (hello tilldelad port finns på hello Azure-portalen eller via PowerShell)
2. Eftersom hello brandväggen lyssnar bara på hello FrontEnd001.CloudApp.Net adress, ingår det inte med den här trafikflöde
3. Backend-undernät börjar bearbetning av inkommande regel:
   1. NSG regel 1 (DNS) inte gäller flytta toonext regel
   2. NSG regel 2 (RDP) gäller, trafik är tillåtna, stoppa regel bearbetning
4. Med några regler för utgående trafik standardregler gäller och returnera trafik tillåts
5. RDP-session är aktiverad
6. AppVM01 efterfrågar användarnamn lösenord

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(Tillåts) Web Server DNS-sökning på DNS-server
1. Web Server, IIS01, måste en datafeed på www.data.gov, men måste tooresolve hello adress.
2. hello nätverkskonfiguration för hello VNet listor DNS01 (10.0.2.4 på hello Backend-undernät) som hello primära DNS-server, IIS01 skickar hello DNS-begäran tooDNS01
3. Inga regler för utgående trafik på undernätet Frontend, tillåts trafik
4. Backend-undernät börjar bearbetning av inkommande regel:
   1. NSG regel 1 (DNS) gäller, trafik är tillåtna, stoppa regel bearbetning
5. DNS-servern tar emot hello begäran
6. DNS-servern inte har cachelagrade hello-adress och begär en rot-DNS-servern på hello internet
7. Inga regler för utgående trafik på Backend-undernät, tillåts trafik
8. Internet-DNS-servern svarar, eftersom denna session initierades internt, är hello svar tillåtet
9. DNS-servern cachelagrar hello svaret och svarar toohello första begäran tillbaka tooIIS01
10. Inga regler för utgående trafik på Backend-undernät, tillåts trafik
11. Undernätet frontend börjar bearbetning av inkommande regel:
    1. Det finns ingen NSG-regel som gäller tooInbound trafik från hello Backend undernät toohello undernätet Frontend, så ingen hälsningspaket NSG-regler gäller
    2. hello system Standardregeln för att tillåta trafik mellan undernät skulle göra att den här trafiken så hello trafik tillåts
12. IIS01 får hello svar från DNS01

#### <a name="allowed-web-server-access-file-on-appvm01"></a>(Tillåts) Web Server access-fil på AppVM01
1. IIS01 begär en fil på AppVM01
2. Inga regler för utgående trafik på undernätet Frontend, tillåts trafik
3. hello Backend-undernät börjar bearbetning av inkommande regel:
   1. NSG regel 1 (DNS) inte gäller flytta toonext regel
   2. NSG regel 2 (RDP) inte gäller flytta toonext regel
   3. NSG regel 3 (Internet tooFirewall) inte gäller flytta toonext regel
   4. NSG regel 4 (IIS01 tooAppVM01) gäller, trafik är tillåtna, stoppa regel bearbetning
4. AppVM01 tar emot hello begäran och svarar med (förutsatt att du har behörighet)
5. Eftersom det finns inga regler för utgående trafik på hello Backend undernät hello svar är tillåtet
6. Undernätet frontend börjar bearbetning av inkommande regel:
   1. Det finns ingen NSG-regel som gäller tooInbound trafik från hello Backend undernät toohello undernätet Frontend, så ingen hälsningspaket NSG-regler gäller
   2. hello system Standardregeln för att tillåta trafik mellan undernät att den här trafiken så hello trafik tillåts.
7. hello IIS-servern tar emot hello-fil

#### <a name="denied-web-direct-tooweb-server"></a>(Nekad) Web direkt tooWeb Server
Eftersom hello webbservern och IIS01 hello brandväggen är i hello samma molntjänst de delar hello samma offentliga Internetriktade IP-adress. Därför skulle HTTP-trafik dirigeras toohello brandväggen. Medan hello begäran skulle vara hanteras, det går inte att gå direkt toohello webbservern skickas, fungerar, via hello brandväggen först. Se hello första scenariot i det här avsnittet för hello trafikflöde.

#### <a name="denied-web-toobackend-server"></a>(Nekad) Web tooBackend Server
1. Internet-användare försöker tooaccess en fil på AppVM01 via hello BackEnd001.CloudApp.Net service
2. Eftersom det inte finns några slutpunkter som är öppna för filresursen, detta skulle inte klarar hello Molntjänsten och skulle nå hello-server
3. Om hello slutpunkter öppna av någon anledning skulle NSG regel 5 (Internet tooVNet) blockera den här trafiken

#### <a name="denied-web-dns-lookup-on-dns-server"></a>(Nekad) Web DNS-sökning på DNS-server
1. Internet-användare försöker toolookup en intern DNS-post på DNS01 via hello BackEnd001.CloudApp.Net service
2. Eftersom inga slutpunkter är öppen för DNS är detta skulle inte klarar hello Molntjänsten och skulle nå hello-server
3. Om hello slutpunkter öppna av någon anledning NSG regel 5 (Internet tooVNet) skulle blockera den här trafiken (Obs: regel 1 (DNS) inte tillämpas av två skäl, första hello källadress är hello internet, den här regeln gäller endast toohello också virtuella lokala nätverk som hello datakälla Det här är en Tillåt-regel, så det skulle aldrig neka trafik)

#### <a name="denied-web-toosql-access-through-firewall"></a>(Nekad) Webbåtkomst tooSQL genom brandväggen
1. Internet-användare begär SQL-data från FrontEnd001.CloudApp.Net (Internet Facing Cloud Service)
2. Eftersom inga slutpunkter är öppen för SQL är detta skulle inte klarar hello Molntjänsten och skulle nå hello-brandväggen
3. Om slutpunkter öppna av någon anledning börjar hello klientdel undernät bearbetning av inkommande regel:
   1. NSG regel 1 (DNS) inte gäller flytta toonext regel
   2. NSG regel 2 (RDP) inte gäller flytta toonext regel
   3. NSG regel 2 (Internet tooFirewall) gäller, trafik är tillåtna, stoppa regel bearbetning
4. Trafik träffar intern IP-adress för brandvägg hello (10.0.1.4)
5. Brandväggen har inga vidarebefordringsregler för SQL och således hello trafik

## <a name="conclusion"></a>Slutsats
Detta är ett relativt direkt vidarebefordra sätt att skydda ditt program med en brandvägg och isolera hello backend-undernät från inkommande trafik.

Fler exempel och en översikt över nätverket säkerhetsgränser finns [här][HOME].

## <a name="references"></a>Referenser
### <a name="main-script-and-network-config"></a>Huvudskriptet och nätverkskonfiguration
Spara hello fullständig skript i ett PowerShell-skriptfil. Spara hello nätverkskonfiguration till en fil med namnet ”NetworkConf2.xml”.
Ändra hello användardefinierade variabler. Kör hello skript och följer hello brandväggen regeln installationsprogrammet anvisningarna ovan.

#### <a name="full-script"></a>Fullständig skript
Det här skriptet kommer utifrån hello användardefinierade variabler:

1. Ansluta tooan Azure-prenumeration
2. Skapa ett nytt lagringskonto
3. Skapa ett nytt virtuellt nätverk och två undernät som definierats i konfigurationsfilen för hello nätverk
4. Skapa 4 windows server-datorer
5. Konfigurera NSG inklusive:
   * Skapa en NSG
   * Fylla det med regler
   * Bindningen hello NSG toohello lämpliga undernät

Detta PowerShell-skript ska köras lokalt på en internet-ansluten dator eller server.

> [!IMPORTANT]
> När du kör det här skriptet kanske varningar eller andra informationsmeddelanden som visas i PowerShell. Endast felmeddelanden i rött är orsaken till problem.
> 
> 

    <# 
     .SYNOPSIS
      Example of DMZ and Network Security Groups in an isolated network (Azure only, no hybrid connections)

     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Two new cloud services
       - Two Subnets (FrontEnd and BackEnd subnets)
       - A Network Virtual Appliance (NVA), in this case a Barracuda NextGen Firewall
       - One server on hello FrontEnd Subnet (plus hello NVA on hello FrontEnd subnet)
       - Three Servers on hello BackEnd Subnet
       - Network Security Groups tooallow/deny traffic patterns as declared

      Before running script, ensure hello network configuration file is created in
      hello directory referenced by $NetworkConfigFile variable (or update the
      variable tooreflect hello path and file name of hello config file being used).

     .Notes
      Security requirements are different for each use case and can be addressed in a
      myriad of ways. Please be sure that any sensitive data or applications are behind
      hello appropriate layer(s) of protection. This script serves as an example of some
      of hello techniques that can be used, but should not be used for all scenarios. You
      are responsible tooassess your security needs and hello appropriate protections
      needed, and then effectively implement those protections.

      FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
       myFirewall - 10.0.1.4
       IIS01      - 10.0.1.5

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
        $FrontEndService = "FrontEnd001"
        $BackEndService = "BackEnd001"

      # Network Details
        $VNetName = "CorpNetwork"
        $FESubnet = "FrontEnd"
        $FEPrefix = "10.0.1.0/24"
        $BESubnet = "BackEnd"
        $BEPrefix = "10.0.2.0/24"
        $NetworkConfigFile = "C:\Scripts\NetworkConf2.xml"

      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
        $FWImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Barracuda NextGen Firewall'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}

      # NSG Details
        $NSGName = "MyVNetSG"

    # User Defined VM Specific Config
        # Note: tooensure proper NSG Rule creation later in this script:
        #       - hello Web Server must be VM 1
        #       - hello AppVM1 Server must be VM 2
        #       - hello DNS server must be VM 4
        #
        #       Otherwise hello NSG rules in hello last section of this
        #       script will need toobe changed toomatch hello modified
        #       VM array numbers ($i) so hello NSG Rule IP addresses
        #       are aligned toohello associated VM IP addresses.

        # VM 0 - hello Network Virtual Appliance (NVA)
          $VMName += "myFirewall"
          $ServiceName += $FrontEndService
          $VMFamily += "Firewall"
          $img += $FWImg
          $size += "Small"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.4"

        # VM 1 - hello Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.5"

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

    If (Test-AzureName -Service -Name $FrontEndService) { 
        Write-Host "hello FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "hello FrontEndService service name is valid for use." -ForegroundColor Green}

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
                # Note: Web traffic goes through hello firewall, so we'll need tooset up a HTTP endpoint.
                #       Also, hello firewall will be redirecting web traffic tooa new IP and Port in a
                #       forwarding rule, so hello HTTP endpoint here will have hello same public and local
                #       port and hello firewall will do hello NATing and redirection as declared in the
                #       firewall rule.
                Add-AzureEndpoint -Name "MgmtPort1" -Protocol tcp -PublicPort 801  -LocalPort 801  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "MgmtPort2" -Protocol tcp -PublicPort 807  -LocalPort 807  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "HTTP"      -Protocol tcp -PublicPort 80   -LocalPort 80   -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                # Note: A SSH endpoint is automatically created on port 22 when hello appliance is created.
                }
            Else
                {
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                    Remove-AzureEndpoint -Name "PowerShell" | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                    # Note: A Remote Desktop endpoint is automatically created when each VM is created.
                }
            $i++
        }

    # Configure NSG
        Write-Host "Configuring hello Network Security Group (NSG)" -ForegroundColor Cyan

      # Build hello NSG
        Write-Host "Building hello NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"

      # Add NSG Rules
        Write-Host "Writing rules into hello NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" -Type Inbound -Priority 100 -Action Allow `
            -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[4] -DestinationPortRange '53' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable RDP too$VNetName VNet" -Type Inbound -Priority 110 -Action Allow `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '3389' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internet too$($VMName[0])" -Type Inbound -Priority 120 -Action Allow `
            -SourceAddressPrefix Internet -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[0] -DestinationPortRange '*' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable $($VMName[1]) too$($VMName[2])" -Type Inbound -Priority 130 -Action Allow `
            -SourceAddressPrefix $VMIP[1] -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[2] -DestinationPortRange '*' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate hello $VNetName VNet from hello Internet" -Type Inbound -Priority 140 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate hello $FESubnet subnet from hello $BESubnet subnet" -Type Inbound -Priority 150 -Action Deny `
            -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
            -DestinationAddressPrefix $BEPrefix -DestinationPortRange '*' `
            -Protocol *

        # Assign hello NSG toohello Subnets
            Write-Host "Binding hello NSG tooboth subnets" -ForegroundColor Cyan
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
[1]: ./media/virtual-networks-dmz-nsg-fw-asm/example2design.png "Inkommande DMZ med NSG"
[2]: ./media/virtual-networks-dmz-nsg-fw-asm/dstnaticon.png "Mål NAT-ikon"
[3]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallrule.png "Brandväggsregel"
[4]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallruleactivate.png "Brandväggen regeln aktivering"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
[Example1]: ./virtual-networks-dmz-nsg-asm.md
