---
title: "Dataflödet i aaaTesting Virtuella Azure-nätverket | Microsoft Docs"
description: "Lär dig hur tootest Azure virtuella nätverk genomflöde."
services: virtual-network
documentationcenter: na
author: steveesp
manager: Gerald DeGrace
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/21/2017
ms.author: steveesp
ms.openlocfilehash: 2da85c27bc8d16a443b215891f4cd0460f41926f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="bandwidththroughput-testing-ntttcp"></a>Bandbredd/dataflöde testning (NTTTCP)

När du testar nätverksprestanda genomflöde i Azure, är det bästa toouse ett verktyg som riktar sig till hello nätverket för testning och minimerar hello användning av andra resurser som kan påverka prestanda. NTTTCP rekommenderas.

Kopiera hello verktyget tootwo Azure virtuella datorer i hello samma storlek. En virtuell dator fungerar som avsändare och hello andra som mottagare.

#### <a name="deploying-vms-for-testing"></a>Distribuera virtuella datorer för testning
Hello enligt det här testet hello två virtuella datorer måste vara i hello samma molntjänst eller hello samma Tillgänglighetsuppsättning så att vi kan använda sina interna IP-adresser och undanta hello belastningsutjämnare från hello test. Det är möjligt tootest med hello VIP men den här typen av testning är utanför hello omfånget för det här dokumentet.
 
Anteckna hello MOTTAGARENS IP-adress. Vi ringer som IP ”a.b.c.r”

Anteckna hello antal kärnor på hello VM. Vi ska anropa detta ”\#num\_kärnor”
 
Kör hello NTTTCP testa för 300 sekunder (eller 5 minuter) på hello avsändaren VM och mottagaren VM.

Tips: När du ställer in det här testet för hello första gången, kan du en kortare test period tooget feedback snabbare. När verktyget hello fungerar som förväntat, utöka hello test period too300 sekunder för hello mest korrekta resultat.

> [!NOTE]
> hello avsändaren **och** mottagare måste ange **hello samma** testa varaktighet parameter (-t).

tootest en TCP-ström för 10 sekunder:

Mottagaren parametrar: ntttcp - r -t 10 - P 1

Avsändaren parametrar: ntttcp-s10.27.33.7 -t 10 - n 1 P - 1

> [!NOTE]
> föregående exempel hello bara ska använda tooconfirm din konfiguration. Giltiga exempel tester beskrivs senare i det här dokumentet.

## <a name="testing-vms-running-windows"></a>Testa virtuella datorer som kör WINDOWS:

#### <a name="get-ntttcp-onto-hello-vms"></a>Hämta NTTTCP till hello virtuella datorer.

Hämta senaste versionen av hello: <https://gallery.technet.microsoft.com/NTttcp-Version-528-Now-f8b12769>

Eller söka efter den om flyttas: <https://www.bing.com/search?q=ntttcp+download> \< --bör först träffar

Du kan lägga NTTTCP i separat mapp som c:\\verktyg

#### <a name="allow-ntttcp-through-hello-windows-firewall"></a>Tillåt NTTTCP hello Windows-brandväggen
Skapa en Tillåt-regel på hello Windows-brandväggen tooallow NTTTCP trafik tooarrive på hello mottagare. I stället för att inkommande tooallow specifika TCP-portar är det enklaste tooallow hello hela NTTTCP programmet efter namn.

Tillåt ntttcp via hello Windows-brandväggen så här:

netsh advfirewall firewall Lägg till regel för programmet =\<sökväg\>\\ntttcp.exe namn = ”ntttcp”-protokollet = alla dir = i praktiken = Tillåt aktivera = yes profil = alla

Till exempel om du har kopierat ntttcp.exe toohello ”c:\\verktyg” mapp, skulle detta hello-kommando: 

netsh advfirewall firewall Lägg till regel för programmet = c:\\verktyg\\ntttcp.exe namn = ”ntttcp”-protokollet = alla dir = i praktiken = Tillåt aktivera = yes profil = alla

#### <a name="running-ntttcp-tests"></a>Kör NTTTCP

Starta NTTTCP på hello mottagare (**kör från CMD**, inte från PowerShell):

ntttcp - r-m [2\*\#num\_kärnor],\*, a.b.c.r -t 300

Om hello VM har fyra kärnor och IP-adressen 10.0.0.4, skulle det se ut så här:

ntttcp - r-m 8,\*, 10.0.0.4 -t 300


Starta NTTTCP på hello AVSÄNDAREN (**kör från CMD**, inte från PowerShell):

ntttcp -s-m 8,\*, 10.0.0.4 -t 300 

Vänta tills hello resultat.


## <a name="testing-vms-running-linux"></a>Testa virtuella datorer som kör LINUX:

Använd nttcp för linux. Den är tillgänglig från <https://github.com/Microsoft/ntttcp-for-linux>

Kör följande kommandon för att förbereda ntttcp för linux på dina virtuella datorer på hello virtuella Linux-datorer (både AVSÄNDAREN och mottagaren):

CentOS - Install Git:
``` bash
  yum install gcc -y  
  yum install git -y
```
Ubuntu - Install Git:
``` bash
 apt-get -y install build-essential  
 apt-get -y install git
```
Kontrollera och installera både:
``` bash
 git clone <https://github.com/Microsoft/ntttcp-for-linux>
 cd ntttcp-for-linux/src
 make && make install
```

Som hello Windows exempel förutsätter vi att hello Linux MOTTAGARENS IP är 10.0.0.4

Starta NTTTCP för Linux hello mottagare:

``` bash
ntttcp -r -t 300
```

Och på hello AVSÄNDAREN, kör:

``` bash
ntttcp -s10.0.0.4 -t 300
```
 
Testa längd standardvärden too60 sekunder om ingen tid parameter anges

## <a name="testing-between-vms-running-windows-and-linux"></a>Testa mellan virtuella datorer som kör Windows och LINUX:

På den här scenarier bör vi aktivera hello Nej-Synkroniseringsläge hello test kan köras. Detta görs med hjälp av hello **-N flaggan** för Linux och **-ns flaggan** för Windows.

#### <a name="from-linux-toowindows"></a>Från Linux tooWindows:

Mottagaren <Windows>:

``` bash
ntttcp -r -m <2 x nr cores>,*,<Windows server IP>
```

Avsändaren <Linux> :

``` bash
ntttcp -s -m <2 x nr cores>,*,<Windows server IP> -N -t 300
```

#### <a name="from-windows-toolinux"></a>Från Windows tooLinux:

Mottagaren <Linux>:

``` bash
ntttcp -r -m <2 x nr cores>,*,<Linux server IP>
```

Avsändaren <Windows>:

``` bash
ntttcp -s -m <2 x nr cores>,*,<Linux  server IP> -ns -t 300
```

## <a name="next-steps"></a>Nästa steg
* Beroende på resultatet, det kan finnas utrymme för[optimera genomflödet datorer](virtual-network-optimize-network-bandwidth.md) för ditt scenario.
* Lär dig mer i [Azure Virtual Network vanliga frågor (FAQ)](virtual-networks-faq.md)
