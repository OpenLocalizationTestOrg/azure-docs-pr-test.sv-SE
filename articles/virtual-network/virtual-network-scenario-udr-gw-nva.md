---
title: "aaaHybrid anslutning med 2-nivåprogram | Microsoft Docs"
description: "Lär dig hur toodeploy virtuella installationer och UDR toocreate en flernivåapp miljö i Azure"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: 1f509bec-bdd1-470d-8aa4-3cf2bb7f6134
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/05/2016
ms.author: jdial
ms.openlocfilehash: 53599862289dbf9c6ab3289b0cb8dda6430f20f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="virtual-appliance-scenario"></a>Virtuella utrustningsscenario
Ett vanligt scenario bland större Azure kunden är hello måste tooprovide två skikt program som exponeras-toohello Internet, samtidigt som toohello tillbaka åtkomstnivå från ett lokalt datacenter. Det här dokumentet vägleder dig genom ett scenario med användaren användardefinierade vägar (UDR), en VPN-Gateway och nätverket virtuella installationer toodeploy en tvålagers-miljö som uppfyller hello följande krav:

* Webbprogrammet måste vara tillgänglig från hello offentliga Internet.
* Server värd hello webbprogrammet måste vara kan tooaccess en backend-programserver.
* All trafik från hello Internet toohello webbprogram måste gå via en virtuell brandväggsinstallation. Den här virtuella installations ska användas för Internet-trafiken.
* All trafik toohello programservern måste gå via en virtuell brandväggsinstallation. Den här virtuella installations används för åtkomst till backend-servern toohello slut och åtkomst kommer från hello lokalt nätverk via en VPN-Gateway.
* Administratörer måste vara kan toomanage hello brandväggen virtuella installationer från sina lokala datorer, med hjälp av en tredje brandväggen virtuell installation som uteslutande används för hanteringsändamål.

Det här är ett standard DMZ-scenario med en DMZ och ett skyddat nätverk. Detta scenario kan konstrueras i Azure med hjälp av NSG: er, virtuella installationer av brandväggen eller en kombination av båda. hello tabellen nedan visas några av hello- och nackdelar mellan NSG: er och virtuella installationer i brandväggen.

|  | Tekniker | Nackdelar |
| --- | --- | --- |
| NSG |Utan kostnad. <br/>Integrerat i Azure RBAC. <br/>Regler kan skapas i ARM-mallar. |Komplexitet kan variera i större miljöer. |
| Brandvägg |Fullständig kontroll över dataplan. <br/>Central hantering via brandväggen-konsolen. |Kostnaden för brandväggsinstallation. <br/>Inte integrerat med Azure RBAC. |

hello lösningen nedan använder brandväggen virtuella installationer tooimplement en DMZ/skyddad scenariot.

## <a name="considerations"></a>Överväganden
Du kan distribuera hello miljön som beskrivs ovan i Azure med hjälp av olika funktioner som är tillgängliga i dag, enligt följande.

* **Virtuella nätverk (VNet)**. Ett virtuellt Azure-nätverk fungerar på liknande sätt tooan lokala nätverk och kan vara upp i en eller flera undernät tooprovide trafikisolering och avgränsning av säkerhetsskäl.
* **Virtuell installation**. Flera partners erbjuda virtuella installationer i hello Azure Marketplace som kan användas för hello tre brandväggar som beskrivs ovan. 
* **Användardefinierade vägar (UDR)**. Routningstabeller kan innehålla udr: er som används av Azure nätverk toocontrol hello flödet av paket i ett virtuellt nätverk. Dessa routningstabeller kan vara tillämpade toosubnets. En av hello nyaste funktionerna i Azure är hello möjlighet tooapply en väg tabell toohello GatewaySubnet, tillhandahåller hello möjlighet tooforward all trafik som kommer till hello Azure VNet från hybrid anslutning tooa virtuell utrustning.
* **IP-vidarebefordring**. Som standard hello Azure nätverk motorn vidarebefordra paket toovirtual nätverkskort (NIC) endast om hello paket IP-adressen matchar hello NIC IP-adress. Om en UDR definierar att ett paket måste skickas tooa angivna virtuell installation, skulle därför hello Azure nätverk motorn släppa paketets. tooensure hello paketet levereras tooa VM (i det här fallet en virtuell installation) som inte är hello faktiska mål för hello-paket måste du tooenable IP-vidarebefordring för hello virtuell installation.
* **Nätverkssäkerhetsgrupper (NSG: er)**. hello exemplet nedan gör inte användning av NSG: er, men du kan använda NSG: er används toohello undernät och/eller nätverkskort i den här lösningen toofurther filter hello trafiken till och från dessa undernät och nätverkskort.

![IPv6-anslutning](./media/virtual-network-scenario-udr-gw-nva/figure01.png)

I det här exemplet finns det en prenumeration som innehåller hello följande:

* 2 resursgrupper, visas inte i hello diagram. 
  * **ONPREMRG**. Innehåller alla resurser nödvändiga toosimulate ett lokalt nätverk.
  * **AZURERG**. Innehåller alla resurser som behövs för hello virtuella Azure-nätverksmiljö. 
* Ett VNet med namnet **onpremvnet** används toomimic som ett lokalt datacenter segmenterat enligt nedan.
  * **onpremsn1**. Undernät som innehåller en virtuell dator (VM) som kör Ubuntu toomimic en lokal server.
  * **onpremsn2**. Undernät som innehåller en virtuell dator som kör Ubuntu toomimic en lokal dator som används av en administratör.
* Det finns en virtuell brandväggsinstallation med namnet **OPFW** på **onpremvnet** används toomaintain en tunnel för**azurevnet**.
* Ett VNet med namnet **azurevnet** segmenterat enligt nedan.
  * **azsn1**. Extern brandvägg undernät som används endast för hello extern brandvägg. Alla Internet-trafiken kommer via det här undernätet. Det här undernätet innehåller endast en extern brandvägg NIC länkade toohello.
  * **azsn2**. Klientdelens undernät som värd för en virtuell dator som körs som en server som ska kunna nås från hello Internet.
  * **azsn3**. Backend-undernät som värd för en virtuell dator som kör en backend-programserver som ska användas av hello frontend-webbserver.
  * **azsn4**. Hantering av undernät användas uteslutande tooprovide management åtkomst tooall brandväggen virtuella installationer. Det här undernätet innehåller bara ett nätverkskort för varje virtuell brandväggsinstallation som används i hello lösningar.
  * **GatewaySubnet**. Hybridrapportering i Azure-anslutningen undernät som krävs för ExpressRoute- och VPN-Gateway tooprovide-anslutning mellan Azure Vnet och andra nätverk. 
* Det finns 3 brandväggen virtuella installationer i hello **azurevnet** nätverk. 
  * **AZF1**. Extern brandvägg exponeras toohello offentliga Internet med hjälp av en offentlig IP-adressresurs i Azure. Du måste tooensure som du har en mall från hello Marketplace eller direkt från leverantören av enheten, som tillhandahåller en 3-NIC virtuell installation.
  * **AZF2**. Intern brandvägg används toocontrol trafik mellan **azsn2** och **azsn3**. Det här är en virtuell 3-NIC-installation.
  * **AZF3**. Hantering av brandvägg tillgänglig tooadministrators från hello lokalt datacenter och anslutna tooa management undernät används toomanage alla brandväggsenheter. Du kan hitta 2-NIC virtuell installation mallar i hello Marketplace eller be en direkt från leverantören av enheten.

## <a name="user-defined-routing-udr"></a>Användardefinierad routning (UDR)
Varje undernät i Azure kan vara länkad tooa UDR tabell som används för toodefine hur omdirigeras trafik som initieras i det undernätet. Om inga udr: er har definierats använder Azure standard vägar tooallow trafik tooflow från ett undernät tooanother. toobetter förstå udr: er, besök [vad är användardefinierade vägar och IP-vidarebefordran](virtual-networks-udr-overview.md#ip-forwarding).

tooensure kommunikation görs via hello rätt brandväggsinstallation utifrån hello senaste kravet ovan måste du toocreate hello följande som innehåller udr: er i routningstabellen **azurevnet**.

### <a name="azgwudr"></a>azgwudr
I det här scenariot hello endast trafik som flödar från lokala tooAzure kommer att använda toomanage hello brandväggar genom att ansluta för**AZF3**, och att trafik måste gå igenom hello intern brandvägg, **AZF2**. Därför bara en väg är nödvändigt i hello **GatewaySubnet** enligt nedan.

| Mål | Nästa hopp | Förklaring |
| --- | --- | --- |
| 10.0.4.0/24 |10.0.3.11 |Tillåter lokal trafik tooreach management brandväggen **AZF3** |

### <a name="azsn2udr"></a>azsn2udr
| Mål | Nästa hopp | Förklaring |
| --- | --- | --- |
| 10.0.3.0/24 |10.0.2.11 |Tillåter trafik toohello backend-undernät på värdservern hello program via **AZF2** |
| 0.0.0.0/0 |10.0.2.10 |Gör alla andra trafik toobe dirigeras via **AZF1** |

### <a name="azsn3udr"></a>azsn3udr
| Mål | Nästa hopp | Förklaring |
| --- | --- | --- |
| 10.0.2.0/24 |10.0.3.10 |Tillåter trafik för**azsn2** tooflow från appen server toohello webbserver via **AZF2** |

Du måste också toocreate routningstabeller för hello undernäten i **onpremvnet** toomimic hello lokalt datacenter.

### <a name="onpremsn1udr"></a>onpremsn1udr
| Mål | Nästa hopp | Förklaring |
| --- | --- | --- |
| 192.168.2.0/24 |192.168.1.4 |Tillåter trafik för**onpremsn2** via **OPFW** |

### <a name="onpremsn2udr"></a>onpremsn2udr
| Mål | Nästa hopp | Förklaring |
| --- | --- | --- |
| 10.0.3.0/24 |192.168.2.4 |Tillåter trafik toohello säkerhetskopieras undernät i Azure via **OPFW** |
| 192.168.1.0/24 |192.168.2.4 |Tillåter trafik för**onpremsn1** via **OPFW** |

## <a name="ip-forwarding"></a>IP-vidarebefordran
UDR och IP-vidarebefordran är funktioner som du kan använda i kombination tooallow virtuella installationer toobe används toocontrol trafikflöde i ett Azure VNet.  En virtuell installation finns inget mer än en virtuell dator som kör ett program som används för toohandle nätverkstrafik på något sätt, till exempel en brandvägg eller NAT-enhet.

Den här virtuella installations VM måste vara kan tooreceive inkommande trafik som är adresserad inte tooitself. tooallow en tooreceive VM-trafik adresserad tooother mål, måste du aktivera IP-vidarebefordring för hello VM. Det här är en Azure-inställning, inte en inställning i hello gästoperativsystemet. Din virtuella enhet fortfarande behov toorun vissa typ av program toohandle hello inkommande trafik och vidarebefordra korrekt.

toolearn mer om IP-vidarebefordran finns [vad är användardefinierade vägar och IP-vidarebefordran](virtual-networks-udr-overview.md#ip-forwarding).

Exempel anta att du har hello efter installationen i ett Azure-vnet:

* Undernät **onpremsn1** innehåller en virtuell dator med namnet **onpremvm1**.
* Undernät **onpremsn2** innehåller en virtuell dator med namnet **onpremvm2**.
* En virtuell installation med namnet **OPFW** är ansluten för**onpremsn1** och **onpremsn2**.
* En användardefinierad väg för länkad**onpremsn1** anger att all trafik för**onpremsn2** måste skickas för**OPFW**.

AT som detta pekar, om **onpremvm1** försöker tooestablish en anslutning med **onpremvm2**hello UDR används och trafik skickas för**OPFW** som hello nästa hopp. Tänk på att hello faktiska paketets mål inte ändras, det är fortfarande **onpremvm2** är hello mål. 

Utan IP-vidarebefordran aktiverad för **OPFW**hello Azure virtuella nätverk logik släpper hälsningspaket, eftersom det bara tillåter att paket toobe skickas tooa VM om hello Virtuella datorns IP-adressen är hello mål för hello-paket.

Med IP-vidarebefordran vidarebefordrar hello virtuella Azure-nätverket logik hello paket tooOPFW utan att ändra dess ursprungliga måladressen. **OPFW** måste hantera hälsningspaket och avgöra vilka toodo med dem..

Hello scenariot ovan toowork måste du aktivera IP-vidarebefordring på hello nätverkskort för **OPFW**, **AZF1**, **AZF2**, och **AZF3** som används för routning (alla NIC utom hello de länkade toohello management undernät). 

## <a name="firewall-rules"></a>Brandväggsregler
Enligt beskrivningen ovan, garanterar IP-vidarebefordran endast paket skickas toohello virtuella installationer. Din enhet måste fortfarande toodecide vilka toodo med dessa paket. Du behöver toocreate hello följande regler i dina installationer i hello scenariot ovan:

### <a name="opfw"></a>OPFW
OPFW representerar en lokal enhet som innehåller hello följande regler:

* **Väg**: All trafik too10.0.0.0/16 (**azurevnet**) måste skickas via tunnel **ONPREMAZURE**.
* **Principen**: alla dubbelriktad trafik mellan **port2** och **ONPREMAZURE**.

### <a name="azf1"></a>AZF1
AZF1 representerar en Azure virtuell installation som innehåller hello följande regler:

* **Principen**: alla dubbelriktad trafik mellan **port1** och **port2**.

### <a name="azf2"></a>AZF2
AZF2 representerar en Azure virtuell installation som innehåller hello följande regler:

* **Väg**: All trafik too10.0.0.0/16 (**onpremvnet**) måste användas för att skicka toohello Azure gateway IP-adress (d.v.s. 10.0.0.1) **port1**.
* **Principen**: alla dubbelriktad trafik mellan **port1** och **port2**.

## <a name="network-security-groups-nsgs"></a>Nätverkssäkerhetsgrupper (NSG: er)
I det här scenariot används inte NSG: er. Du kan dock använda NSG: er tooeach toorestrict inkommande och utgående undernätstrafik. Exempelvis kan du använda hello följande NSG-regler toohello externa FW undernät.

**Inkommande**

* Tillåt alla TCP-trafik från hello Internet tooport 80 på någon virtuell dator i hello undernät.
* Neka all annan trafik från hello Internet.

**Utgående**

* Neka alla trafik toohello Internet.

## <a name="high-level-steps"></a>Hög nivå steg
toodeploy det här scenariot, följ hello hög nivå stegen nedan.

1. Inloggningen tooyour Azure-prenumeration.
2. Om du vill toodeploy ett VNet toomimic hello lokalt nätverk, etablera hello-resurser som ingår i **ONPREMRG**.
3. Etablera hello-resurser som ingår i **AZURERG**.
4. Etablera hello tunnel från **onpremvnet** för**azurevnet**.
5. När alla resurser etableras, logga in för**onpremvm2** och ping 10.0.3.101 tootest anslutningen mellan **onpremsn2** och **azsn3**.

