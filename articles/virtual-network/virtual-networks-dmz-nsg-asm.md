---
title: "aaaAzure DMZ exempel – skapa en enkel DMZ med NSG: er | Microsoft Docs"
description: "Skapa en DMZ med Nätverkssäkerhetsgrupper (NSG)"
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: 
ms.assetid: f8622b1d-c07d-4ea6-b41c-4ae98d998fff
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/03/2017
ms.author: jonor
ms.openlocfilehash: 32a40a8dc7539c4c7293988e6c36e5e32ef11045
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="example-1--build-a-simple-dmz-using-nsgs-with-classic-powershell"></a>Exempel 1 – skapa en enkel DMZ NSG: er med klassiska PowerShell
[Returnera toohello gräns bästa praxis sidan][HOME]

> [!div class="op_single_selector"]
> * [Resource Manager-mall](virtual-networks-dmz-nsg.md)
> * [Klassisk - PowerShell](virtual-networks-dmz-nsg-asm.md)
> 
>

Det här exemplet skapar en primitiv DMZ med fyra Nätverkssäkerhetsgrupper och Windows-servrar. Det här exemplet visar hello relevanta PowerShell-kommandon tooprovide en bättre förståelse för varje steg. Det finns också en trafik scenariot avsnittet tooprovide en djupgående steg för steg hur trafik fortsätter via hello lager i skyddsstrategierna hello DMZ. Slutligen är i referensavsnittet hello hello fullständiga koden och instruktion toobuild den här miljön tootest och experimentera med olika scenarier. 

![Inkommande DMZ med NSG][1]

## <a name="environment-description"></a>Beskrivning av miljö
I det här exemplet innehåller en prenumeration hello följande resurser:

* Två molntjänster: ”FrontEnd001” och ”BackEnd001”
* Ett virtuellt nätverk ”CorpNetwork” med två undernät; ”FrontEnd” och ”BackEnd”
* En Nätverkssäkerhetsgrupp som är kopplade tooboth undernät
* En Windows-Server som representerar en program-webbserver (”IIS01”)
* Två windows-servrar som representerar backend-programservrar (”AppVM01”, ”AppVM02”)
* En Windows-server som representerar en DNS-server (”DNS01”)

I avsnittet för hello referenser finns ett PowerShell-skript som bygger mest hello miljön som beskrivs i det här exemplet. Skapa hello virtuella datorer och virtuella nätverk, som även om utförs av hello exempelskriptet inte beskrivs i detalj i detta dokument. 

toobuild hello-miljön.

1. Spara hello nätverk XML-konfigurationsfilen finns i hello referenser avsnitt (uppdaterade med namn, plats och IP-adresser toomatch hello angivna scenario)
2. Uppdatera hello Användarvariabler i hello skriptet toomatch hello miljö hello skript är toobe körs mot (prenumerationer, tjänstnamn osv.)
3. Kör hello-skriptet i PowerShell

>[!Note]
>hello-region som visas i hello PowerShell-skriptet måste matcha hello region som visas i hello nätverket xml-konfigurationsfilen.
>
>

När hello skriptet körs har ytterligare valfria steg vidtas finns i avsnittet för hello referenser två skript tooset hello webbservern och app-servern med en enkel web application tooallow testning med den här konfigurationen DMZ.

hello innehåller följande avsnitt en detaljerad beskrivning av Nätverkssäkerhetsgrupper och hur de fungerar i det här exemplet genom att gå igenom viktiga rader på hello PowerShell-skript.

## <a name="network-security-groups-nsg"></a>Nätverkssäkerhetsgrupper (NSG)
I det här exemplet bygger en NSG-grupp och sedan in med sex regler. 

> [!TIP]
> Generellt sett bör du först skapa specifika ”Tillåt” regler och hello generisk ”Deny” regler senast. hello prioritet avgör vilka regler som utvärderas först. När trafiken hittas tooapply tooa specifik regel, utvärderas inga fler regler. NSG-regler kan använda antingen i hello i inkommande eller utgående riktning (ur hello hello undernät).
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

Det finns en utgående Standardregeln som tillåter trafik ut toohello internet. I det här exemplet vi att tillåta utgående trafik och inte ändra de utgående reglerna. toolock ned i båda riktningarna användaren definierade routning krävs och är utforskade ”exempel 3” på hello [gräns bästa praxis sidan][HOME].

Varje regel är beskrivs i detalj (**Observera**: ett objekt i följande lista som börjar med ett dollartecken hello (till exempel: $NSGName) är en användardefinierad variabel från hello skript under hello referens i det här dokumentet):

1. Först måste en Nätverkssäkerhetsgrupp byggas toohold hello regler:

    ```PowerShell
    New-AzureNetworkSecurityGroup -Name $NSGName `
        -Location $DeploymentLocation `
        -Label "Security group for $VNetName subnets in $DeploymentLocation"
    ```

2. hello första regeln i det här exemplet kan DNS-trafik mellan alla interna nätverk toohello DNS-server i hello backend-undernät. hello regel har vissa viktiga parametrar:
   
   * ”Typ” betyder i vilken riktning för trafikflöde regeln träder i kraft. hello-riktningen är från hello perspektiv hello undernät eller virtuella datorn (beroende på om den här NSG binds). Därför om typen är ”inkommande” trafik kommer in hello undernät, hello regeln ska användas och trafik som lämnar hello undernät inte påverkas av den här regeln.
   * ”Prioritet” anger hello ordningen på ett trafikflöde är. hello lägre hello nummer hello högre hello prioritet. När en regel använder tooa specifika trafikflöde kan bearbetas inga ytterligare regler. Därför om en regel med prioritet 1 tillåter trafik, och en regel med prioritet 2 nekar trafik, och båda regler gäller tootraffic sedan hello trafik ska tillåtas tooflow (eftersom regel 1 har en högre prioritet det tog att gälla och inga ytterligare regler har tillämpats).
   * ”Åtgärden” innebär det att om trafik som påverkas av regeln blockeras eller tillåts.

    ```PowerShell    
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" `
        -Type Inbound -Priority 100 -Action Allow `
        -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[4] `
        -DestinationPortRange '53' `
        -Protocol *
    ```

3. Den här regeln kan tooflow för RDP-trafik från hello internet toohello RDP-porten på varje server i hello bunden undernät. Den här regeln använder två särskilda typer av adressprefix; ”VIRTUAL_NETWORK” och ”INTERNET”. Dessa taggar är ett enkelt sätt tooaddress en större kategori av adressprefix.

    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
         Set-AzureNetworkSecurityRule -Name "Enable RDP too$VNetName VNet" `
         -Type Inbound -Priority 110 -Action Allow `
         -SourceAddressPrefix INTERNET -SourcePortRange '*' `
         -DestinationAddressPrefix VIRTUAL_NETWORK `
         -DestinationPortRange '3389' `
         -Protocol *
    ```

4. Den här regeln kan inkommande internet-trafik toohit hello webbservern. Hej dirigeringsbeteendet ändras inte den här regeln. hello regeln kan endast trafik till IIS01 toopass. Därför om trafik från hello Internet hade hello webbserver som leder den här regeln skulle göra det möjligt och stoppa bearbetningen ytterligare regler. (I hello regeln med prioritet 140 alla andra inkommande internet-trafiken blockeras). Om du bara bearbetning av HTTP-trafik, den här regeln kan vara mer begränsade tooonly Tillåt mål Port 80.

    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
         Set-AzureNetworkSecurityRule -Name "Enable Internet too$VMName[0]" `
         -Type Inbound -Priority 120 -Action Allow `
         -SourceAddressPrefix Internet -SourcePortRange '*' `
         -DestinationAddressPrefix $VMIP[0] `
         -DestinationPortRange '*' `
         -Protocol *
    ```

5. Den här regeln kan trafik toopass från hello IIS01 server toohello AppVM01 server, en senare regel blockerar alla andra klientdel tooBackend trafik. tooimprove som den här regeln om hello port är känd som ska läggas till. Till exempel om hello IIS-servern träffa endast SQL Server på AppVM01, hello Målportintervall ändras från ”*” (alla) too1433 (hello SQL-port) så att en mindre inkommande risken för angrepp på AppVM01 bör hello webbprogrammet någonsin äventyras.

    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule -Name "Enable $VMName[1] too$VMName[2]" `
        -Type Inbound -Priority 130 -Action Allow `
        -SourceAddressPrefix $VMIP[1] -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[2] `
        -DestinationPortRange '*' `
        -Protocol *
    ```

6. Den här regeln nekar trafik från hello internet tooany servrar på hello nätverk. Hello effekt är tooallow bara inkommande trafik toohello brandvägg och RDP-portar på servrar och blockerar allt annat hello-regler med prioritet 110 och 120. Den här regeln är ”felsäkert” regel tooblock alla oväntat flöden.
    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule `
        -Name "Isolate hello $VNetName VNet from hello Internet" `
        -Type Inbound -Priority 140 -Action Deny `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK `
        -DestinationPortRange '*' `
        -Protocol *
    ```
7. hello slutliga regel nekar trafik från hello klientdel undernät toohello Backend-undernät. Eftersom den här regeln är en regel för inkommande endast, tillåts omvänd trafik (från hello Backend toohello klientdel).

    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule `
        -Name "Isolate hello $FESubnet subnet from hello $BESubnet subnet" `
        -Type Inbound -Priority 150 -Action Deny `
        -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
        -DestinationAddressPrefix $BEPrefix `
        -DestinationPortRange '*' `
        -Protocol * 
    ```

## <a name="traffic-scenarios"></a>Trafik scenarier
#### <a name="allowed-internet-tooweb-server"></a>(*Tillåtna*) tooweb Internetserver
1. En internet-användare begär en HTTP-sida från FrontEnd001.CloudApp.Net (Internet Facing Cloud Service)
2. Cloud service överför trafik via öppna slutpunkter på port 80 mot IIS01 (hello webbserver)
3. Undernätet frontend börjar bearbetning av inkommande regel:
   1. NSG regel 1 (DNS) inte gäller flytta toonext regel
   2. NSG regel 2 (RDP) inte gäller flytta toonext regel
   3. NSG regel 3 (Internet tooIIS01) gäller, trafik är tillåtna, stoppa regel bearbetning
4. Trafik träffar interna IP-adress hello webbservern IIS01 (10.0.1.5)
5. IIS01 lyssnar för webbtrafik, tar emot denna begäran och påbörjar bearbetningen av hello begäran
6. IIS01 begär hello SQL Server på AppVM01 information
7. Eftersom det finns inga regler för utgående trafik på undernätet Frontend, tillåts trafik
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
12. hello IIS-servern tar emot hello SQL svar och slutför hello HTTP-svar och skickar toohello begärande
13. Eftersom det inte finns några regler för utgående trafik på undernätet för hello Frontend hello svaret tillåts och hello internet användare tar emot hello webbsida som begärdes.

#### <a name="allowed-rdp-toobackend"></a>(*Tillåtna*) RDP toobackend
1. Serveradministratören på internet begär RDP-session tooAppVM01 på BackEnd001.CloudApp.Net:xxxxx där xxxxx är hello slumpmässigt tilldelad portnummer för RDP-tooAppVM01 (hello tilldelad port finns på hello Azure-portalen eller via PowerShell)
2. Backend-undernät börjar bearbetning av inkommande regel:
   1. NSG regel 1 (DNS) inte gäller flytta toonext regel
   2. NSG regel 2 (RDP) gäller, trafik är tillåtna, stoppa regel bearbetning
3. Med några regler för utgående trafik standardregler gäller och returnera trafik tillåts
4. RDP-session är aktiverad
5. AppVM01 efterfrågar hello användarnamn och lösenord

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

#### <a name="denied-web-toobackend-server"></a>(*Nekas*) toobackend webbserver
1. En internet-användare försöker tooaccess en fil på AppVM01 via hello BackEnd001.CloudApp.Net service
2. Eftersom inga slutpunkter är öppen för filresursen är den här trafiken skulle inte klarar hello Molntjänsten och skulle nå hello-server
3. Om hello slutpunkter öppna av någon anledning skulle NSG regel 5 (Internet tooVNet) blockera den här trafiken

#### <a name="denied-web-dns-look-up-on-dns-server"></a>(*Nekas*) Web DNS-sökningen på DNS-server
1. En internet-användare försöker toolook upp en intern DNS-post på DNS01 via hello BackEnd001.CloudApp.Net service
2. Eftersom inga slutpunkter är öppen för DNS är den här trafiken skulle inte klarar hello Molntjänsten och skulle nå hello-server
3. Om hello slutpunkter öppna av någon anledning NSG regel 5 (Internet tooVNet) skulle blockera den här trafiken (Obs: regel 1 (DNS) inte tillämpas av två skäl, första hello källadress är hello internet, den här regeln gäller endast toohello också virtuella lokala nätverk som hello datakälla den här regeln är en Tillåt-regel, så det skulle aldrig neka trafik)

#### <a name="denied-web-toosql-access-through-firewall"></a>(*Nekas*) webbåtkomst tooSQL genom brandväggen
1. En internet-användare begär SQL-data från FrontEnd001.CloudApp.Net (Internet Facing Cloud Service)
2. Eftersom inga slutpunkter är öppen för SQL är den här trafiken skulle inte klarar hello Molntjänsten och skulle nå hello-brandväggen
3. Om slutpunkter öppna av någon anledning börjar hello klientdel undernät bearbetning av inkommande regel:
   1. NSG regel 1 (DNS) inte gäller flytta toonext regel
   2. NSG regel 2 (RDP) inte gäller flytta toonext regel
   3. NSG regel 3 (Internet tooIIS01) gäller, trafik är tillåtna, stoppa regel bearbetning
4. Trafik träffar interna IP-adress hello IIS01 (10.0.1.5)
5. IIS01 lyssnar inte på port 1433, så ingen begäran om svar toohello

## <a name="conclusion"></a>Slutsats
Det här exemplet är ett relativt enkla och rakt framåt sätt att isolera hello backend-undernät från inkommande trafik.

Fler exempel och en översikt över nätverket säkerhetsgränser finns [här][HOME].

## <a name="references"></a>Referenser
### <a name="main-script-and-network-config"></a>Huvudsakliga skript- och konfiguration
Spara hello fullständig skript i ett PowerShell-skriptfil. Spara hello nätverkskonfiguration till en fil med namnet ”NetworkConf1.xml”.
Ändra hello användardefinierade variabler som behövs och kör hello skript.

#### <a name="full-script"></a>Fullständigt skript
Det här skriptet kommer att baseras på hello användardefinierade variabler.

1. Ansluta tooan Azure-prenumeration
2. skapar ett lagringskonto
3. Skapa ett VNet och två undernät som definierats i konfigurationsfilen för hello nätverk
4. Skapa fyra windows server-datorer
5. Konfigurera NSG inklusive:
   * Skapa en NSG
   * Fylla det med regler
   * Bindningen hello NSG toohello lämpliga undernät

Detta PowerShell-skript ska köras lokalt på en internet-ansluten dator eller server.

> [!IMPORTANT]
> När du kör det här skriptet kanske varningar eller andra informationsmeddelanden som visas i PowerShell. Endast felmeddelanden i rött är orsaken till problem.
> 
>

```PowerShell
<# 
 .SYNOPSIS
  Example of Network Security Groups in an isolated network (Azure only, no hybrid connections)

 .DESCRIPTION
  This script will build out a sample DMZ setup containing:
   - A default storage account for VM disks
   - Two new cloud services
   - Two Subnets (FrontEnd and BackEnd subnets)
   - One server on hello FrontEnd Subnet
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

# User-Defined Global Variables
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
    $NetworkConfigFile = "C:\Scripts\NetworkConf1.xml"

  # VM Base Disk Image Details
    $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}

  # NSG Details
    $NSGName = "MyVNetSG"

# User-Defined VM Specific Configuration
    # Note: tooensure proper NSG Rule creation later in this script:
    #       - hello Web Server must be VM 0
    #       - hello AppVM1 Server must be VM 1
    #       - hello DNS server must be VM 3
    #
    #       Otherwise hello NSG rules in hello last section of this
    #       script will need toobe changed toomatch hello modified
    #       VM array numbers ($i) so hello NSG Rule IP addresses
    #       are aligned toohello associated VM IP addresses.

    # VM 0 - hello Web Server
      $VMName += "IIS01"
      $ServiceName += $FrontEndService
      $VMFamily += "Windows"
      $img += $SrvImg
      $size += "Standard_D3"
      $SubnetName += $FESubnet
      $VMIP += "10.0.1.5"

    # VM 1 - hello First Application Server
      $VMName += "AppVM01"
      $ServiceName += $BackEndService
      $VMFamily += "Windows"
      $img += $SrvImg
      $size += "Standard_D3"
      $SubnetName += $BESubnet
      $VMIP += "10.0.2.5"

    # VM 2 - hello Second Application Server
      $VMName += "AppVM02"
      $ServiceName += $BackEndService
      $VMFamily += "Windows"
      $img += $SrvImg
      $size += "Standard_D3"
      $SubnetName += $BESubnet
      $VMIP += "10.0.2.6"

    # VM 3 - hello DNS Server
      $VMName += "DNS01"
      $ServiceName += $BackEndService
      $VMFamily += "Windows"
      $img += $SrvImg
      $size += "Standard_D3"
      $SubnetName += $BESubnet
      $VMIP += "10.0.2.4"

# ----------------------------- #
# No User-Defined Variables or  #
# Configuration past this point #
# ----------------------------- #    

  # Get your Azure accounts
    Add-AzureAccount
    Set-AzureSubscription –SubscriptionId $subID -ErrorAction Stop
    Select-AzureSubscription -SubscriptionId $subID -Current -ErrorAction Stop

  # Create Storage Account
    If (Test-AzureName -Storage -Name $StorageAccountName) { 
        Write-Host "Fatal Error: This storage account name is already in use, please pick a different name." -ForegroundColor Red
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
Else { Write-Host "hello network configuration file was found" -ForegroundColor Green
        If (-Not (Select-String -Pattern $DeploymentLocation -Path $NetworkConfigFile)) {
            Write-Host 'hello deployment location was not found in hello network config file, please check hello network config file tooensure hello $DeploymentLocation variable is correct and hello network config file matches.' -ForegroundColor Yellow
            $FatalError = $true}
        Else { Write-Host "hello deployment location was found in hello network config file." -ForegroundColor Green}}

If ($FatalError) {
    Write-Host "A fatal error has occurred, please see hello above messages for more information." -ForegroundColor Red
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
        New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
            Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
            Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
            Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
            Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
            Remove-AzureEndpoint -Name "PowerShell" | `
            New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
            # Note: A Remote Desktop endpoint is automatically created when each VM is created.
        $i++
    }
    # Add HTTP Endpoint for IIS01
    Get-AzureVM -ServiceName $ServiceName[0] -Name $VMName[0] | Add-AzureEndpoint -Name HTTP -Protocol tcp -LocalPort 80 -PublicPort 80 | Update-AzureVM

# Configure NSG
    Write-Host "Configuring hello Network Security Group (NSG)" -ForegroundColor Cyan

  # Build hello NSG
    Write-Host "Building hello NSG" -ForegroundColor Cyan
    New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"

  # Add NSG Rules
    Write-Host "Writing rules into hello NSG" -ForegroundColor Cyan
    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" -Type Inbound -Priority 100 -Action Allow `
        -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[3] -DestinationPortRange '53' `
        -Protocol *

    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable RDP too$VNetName VNet" -Type Inbound -Priority 110 -Action Allow `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '3389' `
        -Protocol *

    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internet too$($VMName[0])" -Type Inbound -Priority 120 -Action Allow `
        -SourceAddressPrefix Internet -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[0] -DestinationPortRange '*' `
        -Protocol *

    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable $($VMName[0]) too$($VMName[1])" -Type Inbound -Priority 130 -Action Allow `
        -SourceAddressPrefix $VMIP[0] -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[1] -DestinationPortRange '*' `
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
  # Install Test Web App (Run Post-Build Script on hello IIS Server)
  # Install Backend resource (Run Post-Build Script on hello AppVM01)
  Write-Host
  Write-Host "Build Complete!" -ForegroundColor Green
  Write-Host
  Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
  Write-Host " - Install Test Web App (Run Post-Build Script on hello IIS Server)" -ForegroundColor Gray
  Write-Host " - Install Backend resource (Run Post-Build Script on hello AppVM01)" -ForegroundColor Gray
  Write-Host
```

#### <a name="network-config-file"></a>Konfigurationsfilen för nätverk
Spara XML-filen med uppdaterad plats och Lägg till hello länken toothis filen toohello $NetworkConfigFile variabeln i hello föregående skript.

```XML
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
```

#### <a name="sample-application-scripts"></a>Exempelskript för programmet
Om du vill tooinstall ett exempelprogram för det här och andra DMZ exempel kan en har angetts på hello följande länk: [exempelskript för programmet][SampleApp]

## <a name="next-steps"></a>Nästa steg
* Uppdatera och spara XML-fil
* Kör hello PowerShell-skriptet toobuild hello-miljö
* Installera hello exempelprogrammet
* Testa olika trafikflöden via den här DMZ

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-asm/example1design.png "Inkommande DMZ med NSG"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md

