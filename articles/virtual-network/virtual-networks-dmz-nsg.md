---
title: "aaaAzure DMZ exempel – skapa en enkel DMZ med NSG: er | Microsoft Docs"
description: "Skapa en DMZ med Nätverkssäkerhetsgrupper (NSG)"
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/03/2017
ms.author: jonor
ms.openlocfilehash: 11c5c6026da30fbc9c5e585f5c16e2d411d6fd80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="example-1--build-a-simple-dmz-using-nsgs-with-an-azure-resource-manager-template"></a>Exempel 1 – skapa en enkel DMZ med NSG: er med en Azure Resource Manager-mall
[Returnera toohello gräns bästa praxis sidan][HOME]

> [!div class="op_single_selector"]
> * [Resource Manager-mall](virtual-networks-dmz-nsg.md)
> * [Klassisk - PowerShell](virtual-networks-dmz-nsg-asm.md)
> 
>

Det här exemplet skapar en primitiv DMZ med fyra Nätverkssäkerhetsgrupper och Windows-servrar. Det här exemplet visar hello relevant mall avsnitt tooprovide en bättre förståelse för varje steg. Det finns också en trafik scenariot avsnittet tooprovide stegvisa inblick i hur trafik fortsätter via hello lager i skyddsstrategierna hello DMZ. Slutligen är i referensavsnittet hello hello fullständig mall-koden och instruktioner toobuild den här miljön tootest och experimentera med olika scenarier. 

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)] 

![Inkommande DMZ med NSG][1]

## <a name="environment-description"></a>Beskrivning av miljö
I det här exemplet innehåller en prenumeration hello följande resurser:

* En enskild resursgrupp
* Ett virtuellt nätverk med två undernät; ”FrontEnd” och ”BackEnd”
* En Nätverkssäkerhetsgrupp som är kopplade tooboth undernät
* En Windows-Server som representerar en program-webbserver (”IIS01”)
* Två windows-servrar som representerar backend-programservrar (”AppVM01”, ”AppVM02”)
* En Windows-server som representerar en DNS-server (”DNS01”)
* En offentlig IP-adress som är kopplade till webbservern för hello program

I avsnittet för hello referenser finns en länk tooan Azure Resource Manager-mall som skapar hello miljön som beskrivs i det här exemplet. Skapa hello virtuella datorer och virtuella nätverk, som även om utfärdad av hello exempel mallen inte beskrivs i detalj i detta dokument. 

**toobuild den här miljön** (detaljerade anvisningar finns i avsnittet för hello-referenser i det här dokumentet);

1. Distribuera hello Azure Resource Manager-mall på: [Azure Quickstart-mallar][Template]
2. Installera hello exempelprogrammet på: [exempelskript för programmet][SampleApp]

>[!NOTE]
>tooRDP tooany backend-servrar i den här instansen hello IIS-servern används som en ”hopp”. Första RDP toohello IIS-servern och sedan hello IIS Server RDP toohello backend-servern. Alternativt kan en offentlig IP-adress associeras med varje servernätverkskort för enklare RDP.
> 
>

hello innehåller följande avsnitt en detaljerad beskrivning av hello Nätverkssäkerhetsgruppen och hur den fungerar i det här exemplet genom att gå igenom viktiga rader av hello Azure Resource Manager-mall.

## <a name="network-security-groups-nsg"></a>Nätverkssäkerhetsgrupper (NSG)
I det här exemplet bygger en NSG-grupp och sedan in med sex regler. 

>[!TIP]
>Generellt sett bör du först skapa specifika ”Tillåt” regler och hello generisk ”Deny” regler senast. hello prioritet avgör vilka regler som utvärderas först. När trafiken hittas tooapply tooa specifik regel, utvärderas inga fler regler. NSG-regler kan använda antingen i hello i inkommande eller utgående riktning (ur hello hello undernät).
>
>

Deklarativt, byggs hello följande regler för inkommande trafik:

1. Intern DNS-trafik (port 53) tillåts
2. RDP-trafik (port 3389) från hello Internet tooany VM tillåts
3. HTTP-trafik (port 80) från hello tooweb Internetserver (IIS01) tillåts
4. All trafik (alla portar) från IIS01 tooAppVM1 tillåts
5. All trafik (alla portar) från hello Internet toohello hela VNet (båda undernäten) nekas
6. All trafik (alla portar) från hello klientdel undernät toohello Backend-undernät nekas

Med dessa regler bundna tooeach undernät, om en HTTP-begäran var inkommande trafik från hello webbserver för Internet toohello, både regler 3 (Tillåt) och 5 (neka) är skulle ha använts, men eftersom regel 3 har högre prioritet bara den skulle tillämpas och regel 5 inte skulle komma till användning. Hello HTTP-begäran skulle därför tillåtna toohello webbservern. Om samma trafiken har tooreach hello DNS01 server, skulle regel 5 (neka) vara hello första tooapply och hello trafik skulle inte toopass toohello server. Regel 6 (neka) blockerar hello klientdel undernät från pratar toohello Backend-undernät (förutom tillåten trafik i regler 1 och 4), den här regeluppsättningen skyddar hello Backend-nätverket om en angripare kompromisser hello webbprogram på hello klientdel, hello angripare skulle begränsad åtkomst toohello Backend ”skyddad” nätverket (endast tooresources som visas på hello AppVM01 server).

Det finns en utgående Standardregeln som tillåter trafik ut toohello internet. I det här exemplet vi att tillåta utgående trafik och inte ändra de utgående reglerna. tooapply säkerhet princip tootraffic i båda riktningarna, användaren definierade routning krävs och är utforskade ”exempel 3” på hello [gräns bästa praxis sidan][HOME].

Varje regel diskuteras i detalj på följande sätt:

1. En säkerhetsgrupp för nätverk-resursen måste vara instansierad toohold hello regler:

    ```JSON
    "resources": [
      {
        "apiVersion": "2015-05-01-preview",
        "type": "Microsoft.Network/networkSecurityGroups",
        "name": "[variables('NSGName')]",
        "location": "[resourceGroup().location]",
        "properties": { }
      }
    ]
    ``` 

2. hello första regeln i det här exemplet kan DNS-trafik mellan alla interna nätverk toohello DNS-server i hello backend-undernät. hello regel har vissa viktiga parametrar:
  * ”destinationAddressPrefix” - regler kan använda en särskild typ av adressprefix kallas en ”standard tagg”, dessa taggar är systemdefinierade identifierare som tillåter ett enkelt sätt tooaddress en större kategori av adressprefix. Den här regeln använder hello standard taggen ”Internet” toosignify någon adress utanför hello virtuella nätverk. Andra prefix etiketter är VirtualNetwork och AzureLoadBalancer.
  * ”Riktning” betyder i vilken riktning för trafikflöde regeln träder i kraft. hello-riktningen är från hello perspektiv hello undernät eller virtuella datorn (beroende på om den här NSG binds). Därför om riktningen är ”inkommande” trafik kommer in hello undernät, hello regeln ska användas och trafik som lämnar hello undernät inte påverkas av den här regeln.
  * ”Prioritet” anger hello ordningen på ett trafikflöde är. hello lägre hello nummer hello högre hello prioritet. När en regel använder tooa specifika trafikflöde kan bearbetas inga ytterligare regler. Därför om en regel med prioritet 1 tillåter trafik, och en regel med prioritet 2 nekar trafik, och båda regler gäller tootraffic sedan hello trafik ska tillåtas tooflow (eftersom regel 1 har en högre prioritet det tog att gälla och inga ytterligare regler har tillämpats).
  * ”Åtkomst” anger om trafik som påverkas av regeln är blockerade (”Deny”) eller tillåtna (”Tillåt”).

    ```JSON
    "properties": {
    "securityRules": [
      {
        "name": "enable_dns_rule",
        "properties": {
          "description": "Enable Internal DNS",
          "protocol": "*",
          "sourcePortRange": "*",
          "destinationPortRange": "53",
          "sourceAddressPrefix": "VirtualNetwork",
          "destinationAddressPrefix": "10.0.2.4",
          "access": "Allow",
          "priority": 100,
          "direction": "Inbound"
        }
      },
    ```

3. Den här regeln kan tooflow för RDP-trafik från hello internet toohello RDP-porten på varje server i hello bunden undernät. 

    ```JSON
    {
      "name": "enable_rdp_rule",
      "properties": {
        "description": "Allow RDP",
        "protocol": "Tcp",
        "sourcePortRange": "*",
        "destinationPortRange": "3389",
        "sourceAddressPrefix": "*",
        "destinationAddressPrefix": "*",
        "access": "Allow",
        "priority": 110,
        "direction": "Inbound"
      }
    },
    ```

4. Den här regeln kan inkommande internet-trafik toohit hello webbservern. Hej dirigeringsbeteendet ändras inte den här regeln. hello regeln kan endast trafik till IIS01 toopass. Därför om trafik från hello Internet hade hello webbserver som leder den här regeln skulle göra det möjligt och stoppa bearbetningen ytterligare regler. (I hello regeln med prioritet 140 alla andra inkommande internet-trafiken blockeras). Om du bara bearbetning av HTTP-trafik, den här regeln kan vara mer begränsade tooonly Tillåt mål Port 80.

    ```JSON
    {
      "name": "enable_web_rule",
      "properties": {
        "description": "Enable Internet too[variables('VM01Name')]",
        "protocol": "Tcp",
        "sourcePortRange": "*",
        "destinationPortRange": "80",
        "sourceAddressPrefix": "Internet",
        "destinationAddressPrefix": "10.0.1.5",
        "access": "Allow",
        "priority": 120,
        "direction": "Inbound"
        }
      },
    ```

5. Den här regeln kan trafik toopass från hello IIS01 server toohello AppVM01 server, en senare regel blockerar alla andra klientdel tooBackend trafik. tooimprove som den här regeln om hello port är känd som ska läggas till. Till exempel om hello IIS-servern träffa endast SQL Server på AppVM01, hello Målportintervall ändras från ”*” (alla) too1433 (hello SQL-port) så att en mindre inkommande risken för angrepp på AppVM01 bör hello webbprogrammet någonsin äventyras.

    ```JSON
    {
      "name": "enable_app_rule",
      "properties": {
        "description": "Enable [variables('VM01Name')] too[variables('VM02Name')]",
        "protocol": "*",
        "sourcePortRange": "*",
        "destinationPortRange": "*",
        "sourceAddressPrefix": "10.0.1.5",
        "destinationAddressPrefix": "10.0.2.5",
        "access": "Allow",
        "priority": 130,
        "direction": "Inbound"
      }
    },
     ```

6. Den här regeln nekar trafik från hello internet tooany servrar på hello nätverk. Hello effekt är tooallow bara inkommande trafik toohello brandvägg och RDP-portar på servrar och blockerar allt annat hello-regler med prioritet 110 och 120. Den här regeln är ”felsäkert” regel tooblock alla oväntat flöden.

    ```JSON
    {
      "name": "deny_internet_rule",
      "properties": {
        "description": "Isolate hello [variables('VNetName')] VNet from hello Internet",
        "protocol": "*",
        "sourcePortRange": "*",
        "destinationPortRange": "*",
        "sourceAddressPrefix": "Internet",
        "destinationAddressPrefix": "VirtualNetwork",
        "access": "Deny",
        "priority": 140,
        "direction": "Inbound"
      }
    },
     ```

7. hello slutliga regel nekar trafik från hello klientdel undernät toohello Backend-undernät. Eftersom den här regeln är en regel för inkommande endast, tillåts omvänd trafik (från hello Backend toohello klientdel).

    ```JSON
    {
      "name": "deny_frontend_rule",
      "properties": {
        "description": "Isolate hello [variables('Subnet1Name')] subnet from hello [variables('Subnet2Name')] subnet",
        "protocol": "*",
        "sourcePortRange": "*",
        "destinationPortRange": "*",
        "sourceAddressPrefix": "[variables('Subnet1Prefix')]",
        "destinationAddressPrefix": "[variables('Subnet2Prefix')]",
        "access": "Deny",
        "priority": 150,
        "direction": "Inbound"
      }
    }
    ```

## <a name="traffic-scenarios"></a>Trafik scenarier
#### <a name="allowed-internet-tooweb-server"></a>(*Tillåtna*) tooweb Internetserver
1. En internet-användare begär en HTTP-sida från hello offentliga IP-adress hello NIC som är associerade med hello IIS01 NIC
2. hello offentlig IP-adress skickar trafik toohello VNet mot IIS01 (hello webbserver)
3. Undernätet frontend börjar bearbetning av inkommande regel:
  1. NSG regel 1 (DNS) inte gäller flytta toonext regel
  2. NSG regel 2 (RDP) inte gäller flytta toonext regel
  3. NSG regel 3 (Internet tooIIS01) gäller, trafik är tillåtna, stoppa regel bearbetning
4. Trafik träffar interna IP-adress hello webbservern IIS01 (10.0.1.5)
5. IIS01 lyssnar för webbtrafik, tar emot denna begäran och påbörjar bearbetningen av hello begäran
6. IIS01 begär hello SQL Server på AppVM01 information
7. Inga regler för utgående trafik på undernätet Frontend, tillåts trafik
8. hello Backend-undernät börjar bearbetning av inkommande regel:
  1. NSG regel 1 (DNS) inte gäller flytta toonext regel
  2. NSG regel 2 (RDP) inte gäller flytta toonext regel
  3. NSG regel 3 (Internet tooFirewall) inte gäller flytta toonext regel
  4. NSG regel 4 (IIS01 tooAppVM01) gäller, trafik är tillåtna, stoppa regel bearbetning
9. AppVM01 får hello SQL-fråga och svarar
10. Eftersom det finns inga regler för utgående trafik på hello Backend-undernät, är hello svar tillåtet
11. Undernätet frontend börjar bearbetning av inkommande regel:
  1. Det finns ingen NSG-regel som gäller tooInbound trafik från hello Backend undernät toohello undernätet Frontend, så ingen hälsningspaket NSG-regler gäller
  2. hello system Standardregeln för att tillåta trafik mellan undernät att den här trafiken så hello trafik tillåts.
12. hello IIS-servern tar emot hello SQL svar och slutför hello HTTP-svar och skickar beställaren toohello
13. Eftersom det finns inga regler för utgående trafik på hello klientdel undernät, hello svaret tillåts och hello Internet-användare får hello webbsida som begärdes.

#### <a name="allowed-rdp-tooiis-server"></a>(*Tillåtna*) RDP tooIIS server
1. Serveradministratör på internet begär en RDP-session tooIIS01 på hello offentliga IP-adress hello NIC som är associerade med hello IIS01 NIC (den här offentliga IP-adressen finns via hello-portalen eller PowerShell)
2. hello offentlig IP-adress skickar trafik toohello VNet mot IIS01 (hello webbserver)
3. Undernätet frontend börjar bearbetning av inkommande regel:
  1. NSG regel 1 (DNS) inte gäller flytta toonext regel
  2. NSG regel 2 (RDP) gäller, trafik är tillåtna, stoppa regel bearbetning
4. Med några regler för utgående trafik standardregler gäller och returnera trafik tillåts
5. RDP-session är aktiverad
6. IIS01 efterfrågar hello användarnamn och lösenord

>[!NOTE]
>tooRDP tooany backend-servrar i den här instansen hello IIS-servern används som en ”hopp”. Första RDP toohello IIS-servern och sedan hello IIS Server RDP toohello backend-servern.
>
>

#### <a name="allowed-web-server-dns-look-up-on-dns-server"></a>(*Tillåtna*) Web server DNS-sökningen på DNS-server
1. Web Server, IIS01, måste en datafeed på www.data.gov, men måste tooresolve hello adress.
2. hello nätverkskonfiguration för hello VNet listor DNS01 (10.0.2.4 på hello Backend-undernät) som hello primära DNS-server, IIS01 skickar hello DNS-begäran tooDNS01
3. Inga regler för utgående trafik på undernätet Frontend, tillåts trafik
4. Backend-undernät börjar bearbetning av inkommande regel:
  * NSG regel 1 (DNS) gäller, trafik är tillåtna, stoppa regel bearbetning
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

#### <a name="allowed-web-server-access-file-on-appvm01"></a>(*Tillåtna*) Web server access-fil på AppVM01
1. IIS01 begär en fil på AppVM01
2. Inga regler för utgående trafik på undernätet Frontend, tillåts trafik
3. hello Backend-undernät börjar bearbetning av inkommande regel:
  1. NSG regel 1 (DNS) inte gäller flytta toonext regel
  2. NSG regel 2 (RDP) inte gäller flytta toonext regel
  3. NSG regel 3 (Internet tooIIS01) inte gäller flytta toonext regel
  4. NSG regel 4 (IIS01 tooAppVM01) gäller, trafik är tillåtna, stoppa regel bearbetning
4. AppVM01 tar emot hello begäran och svarar med (förutsatt att du har behörighet)
5. Eftersom det finns inga regler för utgående trafik på hello Backend-undernät, är hello svar tillåtet
6. Undernätet frontend börjar bearbetning av inkommande regel:
  1. Det finns ingen NSG-regel som gäller tooInbound trafik från hello Backend undernät toohello undernätet Frontend, så ingen hälsningspaket NSG-regler gäller
  2. hello system Standardregeln för att tillåta trafik mellan undernät att den här trafiken så hello trafik tillåts.
7. hello IIS-servern tar emot hello-fil

#### <a name="denied-rdp-toobackend"></a>(*Nekas*) RDP toobackend
1. En internet-användare försöker tooRDP tooserver AppVM01
2. Eftersom det inte finns några offentliga IP-adresser som är associerade med den här NIC-servrar, den här trafiken anger aldrig hello VNet och skulle nå hello-servern
3. Men om en offentlig IP-adress har aktiverats av någon anledning NSG regel 2 (RDP) skulle göra att den här trafiken

>[!NOTE]
>tooRDP tooany backend-servrar i den här instansen hello IIS-servern används som en ”hopp”. Första RDP toohello IIS-servern och sedan hello IIS Server RDP toohello backend-servern.
>
>

#### <a name="denied-web-toobackend-server"></a>(*Nekas*) toobackend webbserver
1. En internet-användare försöker tooaccess en fil på AppVM01
2. Eftersom det inte finns några offentliga IP-adresser som är associerade med den här NIC-servrar, den här trafiken anger aldrig hello VNet och skulle nå hello-servern
3. Om en offentlig IP-adress har aktiverats av någon anledning skulle NSG regel 5 (Internet tooVNet) blockera den här trafiken

#### <a name="denied-web-dns-look-up-on-dns-server"></a>(*Nekas*) Web DNS-sökningen på DNS-server
1. En internet-användare försöker toolook upp en intern DNS-post på DNS01
2. Eftersom det inte finns några offentliga IP-adresser som är associerade med den här NIC-servrar, den här trafiken anger aldrig hello VNet och skulle nå hello-servern
3. Om en offentlig IP-adress har aktiverats av någon anledning NSG regel 5 (Internet tooVNet) skulle blockera den här trafiken (Obs: att regel 1 (DNS) inte tillämpas eftersom hello begär källadress är hello internet och regel 1 gäller endast toohello virtuella lokala nätverk som hello källa)

#### <a name="denied-sql-access-on-hello-web-server"></a>(*Nekas*) SQL-åtkomst på hello webbserver
1. En internet-användare begär SQL-data från IIS01
2. Eftersom det inte finns några offentliga IP-adresser som är associerade med den här NIC-servrar, den här trafiken anger aldrig hello VNet och skulle nå hello-servern
3. Om en offentlig IP-adress har aktiverats av någon anledning, börjar hello klientdel undernät bearbetning av inkommande regel:
  1. NSG regel 1 (DNS) inte gäller flytta toonext regel
  2. NSG regel 2 (RDP) inte gäller flytta toonext regel
  3. NSG regel 3 (Internet tooIIS01) gäller, trafik är tillåtna, stoppa regel bearbetning
4. Trafik träffar interna IP-adress hello IIS01 (10.0.1.5)
5. IIS01 lyssnar inte på port 1433, så ingen begäran om svar toohello

## <a name="conclusion"></a>Slutsats
Det här exemplet är ett relativt enkla och rakt framåt sätt att isolera hello backend-undernät från inkommande trafik.

Fler exempel och en översikt över nätverket säkerhetsgränser finns [här][HOME].

## <a name="references"></a>Referenser
### <a name="azure-resource-manager-template"></a>Azure Resource Manager-mall
Det här exemplet använder en fördefinierad Azure Resource Manager-mall i en GitHub-databas som hanteras av Microsoft och öppna toohello community. Den här mallen kan distribueras direkt från GitHub, eller hämtade och ändrade toofit dina behov. 

hello Huvudmall är i hello-fil med namnet ”azuredeploy.json”. Den här mallen kan skickas via PowerShell eller CLI (med hello associerade ”azuredeploy.parameters.json”-fil) toodeploy den här mallen. Jag hitta hello enklaste vägen toouse hello ”distribuera tooAzure”-knappen på hello README.md sidan på GitHub.

toodeploy hello mall som skapar det här exemplet från GitHub och hello Azure-portalen så här:

1. Från en webbläsare, navigerar du toohello [mall][Template]
2. Klicka hello ”distribuera tooAzure” (eller hello ”visualisera” knappen toosee en grafisk representation av den här mallen)
3. Ange hello Storage-konto, användarnamn och lösenord i hello parametrar bladet och klicka sedan på **OK**
5. Skapa en resursgrupp för distributionen (du kan använda en befintlig, men jag rekommenderar en ny för bästa resultat)
6. Om det behövs ändrar du inställningar för hello prenumerationen och platsen för din VNet.
7. Klicka på **Granska juridiska villkor**, Läs hello villkoren och klicka på **inköp** tooagree.
8. Klicka på **skapa** toobegin hello distribution av den här mallen.
9. Navigera toohello resursgruppen som skapats för den här distributionen toosee hello-resurser som konfigurerats i när hello distribution slutförs.

>[!NOTE]
>Den här mallen kan RDP toohello IIS01 endast server (Sök hello offentlig IP för IIS01 på hello Portal). tooRDP tooany backend-servrar i den här instansen hello IIS-servern används som en ”hopp”. Första RDP toohello IIS-servern och sedan hello IIS Server RDP toohello backend-servern.
>
>

tooremove den här distributionen, ta bort hello resursgruppen och alla underordnade resurser tas också bort.

#### <a name="sample-application-scripts"></a>Exempelskript för programmet
När hello mallen har körts kan ställa du in hello webbservern och app-servern med en enkel web application tooallow testning med den här konfigurationen DMZ. tooinstall ett exempelprogram för det här och andra DMZ exempel något har angetts på hello följande länk: [exempelskript för programmet][SampleApp]

## <a name="next-steps"></a>Nästa steg

* Distribuera det här exemplet
* Skapa hello exempelprogrammet
* Testa olika trafikflöden via den här DMZ

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-arm/example1design.png "Inkommande DMZ med NSG"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[Template]: https://github.com/Azure/azure-quickstart-templates/tree/master/301-dmz-nsg
[SampleApp]: ./virtual-networks-sample-app.md