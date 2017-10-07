---
title: aaaDiagnostics verktyget tootroubleshoot StorSimple 8000 enhet | Microsoft Docs
description: "Beskriver hello StorSimple enheten lägen och förklarar hur toouse Windows PowerShell för StorSimple toochange hello enhet-läge."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2017
ms.author: alkohli
ms.openlocfilehash: e8b7fdbc44d2533973b63da841335ba73ba0014b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-diagnostics-tool-tootroubleshoot-8000-series-device-issues"></a>Använd hello StorSimple diagnostikverktyget tootroubleshoot 8000-serien problem med enheter

## <a name="overview"></a>Översikt

Hej StorSimple diagnostikverktyget diagnostiserar problem relaterade toosystem, prestanda, nätverk och maskinvara komponentens hälsostatus för en StorSimple-enhet. hello diagnostikverktyget kan användas i olika scenarier. Dessa scenarier som inkluderar arbetsbelastning planerar, distribuerar en StorSimple-enhet, bedöma hello nätverksmiljö och bestämmer hello resultatet av en operativa enhet. Den här artikeln innehåller en översikt över hello-diagnostik och beskriver hur hello verktyget kan användas med en StorSimple-enhet.

hello-diagnostik är främst avsett för StorSimple 8000-serien lokala enheter (8100 och 8600).

## <a name="run-diagnostics-tool"></a>Kör diagnostik

Det här verktyget kan köras via hello Windows PowerShell-gränssnittet för din StorSimple-enhet. Det finns två sätt tooaccess hello lokala gränssnittet på din enhet:

* [Använd PuTTY tooconnect toohello enhetens seriekonsol](storsimple-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).
* [Fjärransluta till hello verktyget via hello Windows PowerShell för StorSimple](storsimple-remote-connect.md).

I den här artikeln förutsätter vi att du har anslutit toohello enhetens seriekonsol via PuTTY.

#### <a name="toorun-hello-diagnostics-tool"></a>toorun hello-diagnostik

När du har anslutit toohello Windows PowerShell-gränssnittet för hello enhet, utföra hello följande steg toorun hello cmdlet.
1. Logga in toohello enhetens seriekonsol genom att följa stegen hello i [använda PuTTY tooconnect toohello enhetens seriekonsol](storsimple-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).

2. Skriv följande kommando hello:

    `Invoke-HcsDiagnostics`

    Om hello omfattningsparametern inte anges körs alla hello diagnostiktest hello cmdlet. Dessa tester inkluderar system, komponenthälsa för maskinvara, nätverk och prestanda. 
    
    toorun endast specifika test, ange hello omfattningsparameter. Till exempel toorun endast hello nätverkstest, typ

    `Invoke-HcsDiagnostics -Scope Network`

3. Markera och kopiera hello utdata från hello PuTTY fönstret till en textfil för ytterligare analys.

## <a name="scenarios-toouse-hello-diagnostics-tool"></a>Diagnostik för scenarier toouse hello

Använd hello diagnostik verktyget tootroubleshoot hello nätverk, prestanda, system och maskinvara hälsotillstånd hello system. Här följer några möjliga scenarier:

* **Enheten offline** -din StorSimple 8000-serien enheten är offline. Dock från hello Windows PowerShell-gränssnittet verkar att båda hello domänkontrollanter är igång.
    * Du kan använda det här verktyget toothen bestämma hello nätverket tillstånd.
         
         > [!NOTE]
         > Använd inte det här verktyget tooassess prestanda- och nätverksinställningar på en enhet innan hello-registrering (eller konfigurera via installationsguiden). En giltig IP-adress tilldelas toohello enhet under installationsguiden och registrering. Du kan köra denna cmdlet på en enhet som inte är registrerad, för maskinvara hälso- och system. Använd hello omfattningsparametern, till exempel:
         >
         > `Invoke-HcsDiagnostics -Scope Hardware`
         >
         > `Invoke-HcsDiagnostics -Scope System`

* **Beständiga enhetsproblem** -du har enhetsproblem med som verkar vara toopersist. Till exempel misslyckas registreringen. Du kan också ha problem med enhetsproblem när hello enheten är korrekt registrerade och fungerar på ett tag.
    * I detta fall kan använda verktyget för preliminär felsökning innan du loggar en tjänstbegäran med Microsoft-supporten. Vi rekommenderar att du kör det här verktyget och avbilda hello utdata från det här verktyget. Sedan kan du ge den här utdata tooSupport tooexpedite felsökning.
    * Om det finns några maskinvarufel för komponent eller ett kluster, bör du logga in en supportbegäran.

* **Lite Enhetsprestanda** -din StorSimple-enhet är långsam.
    * I det här fallet köra denna cmdlet med scope parametern set tooperformance. Analysera hello utdata. Du får hello molnet skrivskyddad svarstider. Använd hello rapporterade svarstiderna som maximalt hastigheterna mål, ta hänsyn till vissa kostnader för hello interna databearbetning och sedan distribuera hello arbetsbelastningar på hello system. Mer information finns för[använda hello test tootroubleshoot enheten nätverksprestanda](#network-test).


## <a name="diagnostics-test-and-sample-outputs"></a>Diagnostik för testning och exempel utdata

### <a name="hardware-test"></a>Maskinvarutest

Det här testet hello status för maskinvarukomponenter hello, hello över inbyggd programvara och hello disk inbyggd programvara körs på datorn.

* hello maskinvarukomponenter rapporterade är de komponenterna som misslyckats hello-test eller finns inte i hello system.
* hello över inbyggd programvara och disk versioner av inbyggd rapporteras för hello styrenhet 0, 1 domänkontrollant, och delade komponenter i systemet. En fullständig lista över maskinvarukomponenter finns:

    * [Komponenter i primära hölje](storsimple-monitor-hardware-status.md#component-list-for-primary-enclosure-of-storsimple-device)
    * [Komponenter i EBOD hölje](storsimple-monitor-hardware-status.md#component-list-for-ebod-enclosure-of-storsimple-device)

> [!NOTE]
> Om hello maskinvarutest rapporterar misslyckade komponenter, [logga in en tjänstbegäran med Microsoft-supporten](storsimple-contact-microsoft-support.md).

#### <a name="sample-output-of-hardware-test-run-on-an-8100-device"></a>Exempel på utdata av maskinvarutest som körs på en 8100-enhet

Här är ett exempel på utdata från en StorSimple 8100-enhet. Hello 8100 modellen enhet finns hello EBOD hölje inte. Därför rapporteras hello EBOD domänkontrollant komponenter inte.

```
Controller0>Invoke-HcsDiagnostics -Scope Hardware
Running hardware diagnostics ...
--------------------------------------------------
Hardware components failed or not present
----------------------

           Type           State      Controller           Index     EnclosureId
           ----           -----      ----------           -----     -----------
...rVipResource      NotPresent            None               1            None
...rVipResource      NotPresent            None               2            None
...rVipResource      NotPresent            None               3            None
...rVipResource      NotPresent            None               4            None
...rVipResource      NotPresent            None               5            None
...rVipResource      NotPresent            None               6            None
...rVipResource      NotPresent            None               7            None
...rVipResource      NotPresent            None               8            None
...rVipResource      NotPresent            None               9            None
...rVipResource      NotPresent            None              10            None
...rVipResource      NotPresent            None              11            None

Firmware information
----------------------
TalladegaController : ActiveBIOS:0.45.0010
                      BackupBIOS:0.45.0006
                      MainCPLD:17.0.000b
                      ActiveBMCRoot:2.0.001F
                      BackupBMCRoot:2.0.001F
                      BMCBoot:2.0.0002
                      LsiFirmware:20.00.04.00
                      LsiBios:07.37.00.00
                      Battery1Firmware:06.2C
                      Battery2Firmware:06.2C
                      DomFirmware:X231600
                      CanisterFirmware:3.5.0.56
                      CanisterBootloader:5.03
                      CanisterConfigCRC:0x9134777A
                      CanisterVPDStructure:0x06
                      CanisterGEMCPLD:0x19
                      CanisterVPDCRC:0x142F7DC2
                      MidplaneVPDStructure:0x0C
                      MidplaneVPDCRC:0xA6BD4F64
                      MidplaneCPLD:0x10
                      PCM1Firmware:1.00|1.05
                      PCM1VPDStructure:0x05
                      PCM1VPDCRC:0x41BEF99C
                      PCM2Firmware:1.00|1.05
                      PCM2VPDStructure:0x05
                      PCM2VPDCRC:0x41BEF99C

EbodController      :
DisksFirmware       : SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08

TalladegaController : ActiveBIOS:0.45.0010
                      BackupBIOS:0.45.0006
                      MainCPLD:17.0.000b
                      ActiveBMCRoot:2.0.001F
                      BackupBMCRoot:2.0.001F
                      BMCBoot:2.0.0002
                      LsiFirmware:20.00.04.00
                      LsiBios:07.37.00.00
                      Battery1Firmware:06.2C
                      Battery2Firmware:06.2C
                      DomFirmware:X231600
                      CanisterFirmware:3.5.0.56
                      CanisterBootloader:5.03
                      CanisterConfigCRC:0x9134777A
                      CanisterVPDStructure:0x06
                      CanisterGEMCPLD:0x19
                      CanisterVPDCRC:0x142F7DC2
                      MidplaneVPDStructure:0x0C
                      MidplaneVPDCRC:0xA6BD4F64
                      MidplaneCPLD:0x10
                      PCM1Firmware:1.00|1.05
                      PCM1VPDStructure:0x05
                      PCM1VPDCRC:0x41BEF99C
                      PCM2Firmware:1.00|1.05
                      PCM2VPDStructure:0x05
                      PCM2VPDCRC:0x41BEF99C

EbodController      :
DisksFirmware       : SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08

--------------------------------------------------
```

### <a name="system-test"></a>System-test

Det här testet rapporterar hello Systeminformation, hello tillgängliga uppdateringar, hello klusterinformation och hello tjänstinformation för din enhet.

* hello Systeminformation innehåller hello modellen, enhetens serienummer, tidszon, status för domänkontrollanten och detaljerad hello programvaruversion som körs på hello system. toounderstand hello olika systemparametrar rapporteras som hello utdata gå för[tolka Systeminformation](#appendix-interpreting-system-information).

* hello update tillgänglighet rapporter om hello regular och underhåll lägen är tillgängliga och deras associerade paketnamn. Om `RegularUpdates` och `MaintenanceModeUpdates` är `false`, detta indikerar att hello uppdateringar inte är tillgängliga. Enheten är uppdaterad.
* hello klusterinformation innehåller hello information om olika logiska komponenter till alla hello HCS klustergrupper och deras respektive status. Om du ser ett offline klustergrupp i det här avsnittet av hello rapport [kontaktar Microsoft Support](storsimple-contact-microsoft-support.md).
* hello tjänstinformation innehåller hello namn och status för alla hello HCS och TIS-tjänster som körs på enheten. Den här informationen är användbart för hello Microsoft-supporten felsöka hello enheten problemet.

#### <a name="sample-output-of-system-test-run-on-an-8100-device"></a>Exempel på utdata från system-test som körs på en 8100-enhet

Här är ett exempel på utdata från hello system test som körs på en 8100-enhet.

```
Controller0>Invoke-HcsDiagnostics -Scope System
Running system diagnostics ...
--------------------------------------------------

System information
----------------------
Controller0:

InstanceId              : 7382407f-a56b-4622-8f3f-756fe04cfd38
Name                    : 8100-SHX0991003G467K
Model                   : 8100
SerialNumber            : SHX0991003G467K
TimeZone                : (UTC-08:00) Pacific Time (US & Canada)
CurrentController       : Controller0
ActiveController        : Controller0
Controller0Status       : Normal
Controller1Status       : Normal
SystemMode              : Normal
FriendlySoftwareVersion : StorSimple 8000 Series Update 4.0
HcsSoftwareVersion      : 6.3.9600.17820
ApiVersion              : 9.0.0.0
VhdVersion              : 6.3.9600.17759
OSVersion               : 6.3.9600.0
CisAgentVersion         : 1.0.9441.0
MdsAgentVersion         : 35.2.2.0
Lsisas2Version          : 2.0.78.0
Capacity                : 219902325555200
RemoteManagementMode    : Disabled
FipsMode                : Enabled

Controller1:
InstanceId              : 7382407f-a56b-4622-8f3f-756fe04cfd38
Name                    : 8100-SHX0991003G467K
Model                   : 8100
SerialNumber            : SHX0991003G467K
TimeZone                :
CurrentController       : Controller0
ActiveController        : Controller0
Controller0Status       : Normal
Controller1Status       : Normal
SystemMode              : Normal
FriendlySoftwareVersion : StorSimple 8000 Series Update 4.0
HcsSoftwareVersion      : 6.3.9600.17820
ApiVersion              : 9.0.0.0
VhdVersion              : 6.3.9600.17759
OSVersion               : 6.3.9600.0
CisAgentVersion         : 1.0.9441.0
MdsAgentVersion         : 35.2.2.0
Lsisas2Version          : 2.0.78.0
Capacity                : 219902325555200
RemoteManagementMode    : HttpsAndHttpEnabled
FipsMode                : Enabled

Update availability
----------------------
RegularUpdates              : False
MaintenanceModeUpdates      : False
RegularUpdatesTitle         : {}
MaintenanceModeUpdatesTitle : {}

Cluster information
----------------------
Name                          State OwnerGroup
----                          ----- ----------
ApplicationHostRLUA           Online HCS Cluster Group
Data0v4                       Online HCS Cluster Group
HCS Vnic Resource             Online HCS Cluster Group
hcs_cloud_connectivity_...    Online HCS Cluster Group
hcs_controller_replacement    Online HCS Cluster Group
hcs_datapath_service          Online HCS Cluster Group
hcs_management_servic         Online HCS Cluster Group
hcs_nvram_service             Online HCS Cluster Group
hcs_passive_datapath          Online HCS Passive Cluster Group
hcs_platform_service          Online HCS Cluster Group
hcs_saas_agent_service        Online HCS Cluster Group
HddDataClusterDisk            Online HCS Cluster Group
HddMgmtClusterDisk            Online HCS Cluster Group
HddReplClusterDisk            Online HCS Cluster Group
iSCSI Target Server           Online HCS Cluster Group
NvramClusterDisk              Online HCS Cluster Group
SSAdminRLUA                   Online HCS Cluster Group
SsdDataClusterDisk            Online HCS Cluster Group
SsdNvramClusterDisk           Online HCS Cluster Group

Service information
----------------------
Name                                          Status DisplayName
----                                          ------ -----------
CiSAgentSvc                                   Stopped CiS Service Agent
hcs_cloud_connectivity_...                    Running hcs_cloud_connectivity...
hcs_controller_replacement                    Running HCS Controller Replace...
hcs_datapath_service                          Running HCS Datapath Service
hcs_management_service                        Running HCS Management Service
hcs_minishell                                 Running hcs_minishell
HCS_NVRAM_Service                             Running HCS NVRAM Service
hcs_passive_datapath                          Stopped HCS Passive Datapath S...
hcs_platform_service                          Running HCS Platform Monitor S...
hcs_saas_agent_service                        Running hcs_saas_agent_service
hcs_startup                                   Stopped hcs_startup
--------------------------------------------------
```

### <a name="network-test"></a>Nätverkstest

Det här testet kontrollerar hello status för hello nätverksgränssnitt, portar, DNS och NTP serveranslutning, SSL certifikat, lagringskontouppgifter, anslutning toohello Update-servrar och proxy webbanslutningen på StorSimple-enheten.

#### <a name="sample-output-of-network-test-when-only-data0-is-enabled"></a>Exempel på utdata för nätverket testa när endast DATA0 är aktiverad

Här är ett exempel på utdata för hello 8100-enhet. Du kan se i hello utdata som:
* Endast DATA 0 och DATA 1 nätverksgränssnitt är aktiverad och konfigurerad.
* DATA 2-5 är inte aktiverade i hello-portalen.
* hello DNS-serverkonfigurationen är giltig och hello enhet kan ansluta via hello DNS-server.
* hello NTP-serveranslutning är också bra.
* Portarna 80 och 443 är öppna. Dock är port 9354 blockerad. Baserat på hello [system nätverkskrav](storsimple-system-requirements.md), behöver du tooopen den här porten för hello service bus-kommunikation.
* hello SSL-certifikat är giltigt.
* hello-enhet kan ansluta toohello lagringskonto: _myss8000storageacct_.
* hello anslutningen tooUpdate servrar är giltig.
* hello webbproxy har inte konfigurerats på den här enheten.

#### <a name="sample-output-of-network-test-when-data0-and-data1-are-enabled"></a>Utdata från nätverkstest när DATA0 och fil1 har aktiverats

```
Controller0>Invoke-HcsDiagnostics -Scope Network
Running network diagnostics ....
--------------------------------------------------
Validating networks .....
Name                Entity              Result              Details
----                ------              ------              -------
Network interface   Data0               Valid
Network interface   Data1               Valid
Network interface   Data2               Not enabled
Network interface   Data3               Not enabled
Network interface   Data4               Not enabled
Network interface   Data5               Not enabled
DNS                 10.222.118.154      Valid
NTP                 time.windows.com    Valid
Port                80                  Open
Port                443                 Open
Port                9354                Blocked
SSL certificate     https://myss8000... Valid
Storage account ... myss8000storageacct Valid
URL                 http://download.... Valid
URL                 http://download.... Valid
Web proxy                               Not enabled         Web proxy is not...
--------------------------------------------------
```

### <a name="performance-test"></a>Prestandatest

Det här testet rapporterar hello molnet prestanda via hello molnet skrivskyddad svarstiderna för din enhet. Det här verktyget kan vara används tooestablish en baslinje för hello molnet prestanda som du kan uppnå med StorSimple. hello verktyget rapporter hello maximala prestanda (bästa möjliga scenario för skrivskyddad svarstiderna) som du kan hämta för anslutningen.

Som hello rapporteras hello högsta prestanda, kan vi använda hello rapporterade svarstiderna skrivskyddad som mål när du distribuerar hello arbetsbelastningar.

hello test simulerar hello blob-storlekar som är associerade med hello annan volymtyper på hello enhet. Vanliga nivåer och säkerhetskopior av lokalt fästa volymer använder en 64 KB blob-storlek. Nivåindelade volymer med Arkiv alternativet markerat använda 512 KB storleken för blob-data. Om enheten har konfigurerade nivåindelade och lokalt fästa volymer endast hello test motsvarande too64 KB blob datastorleken körs.

toouse det här verktyget utför hello följande steg:

1.  Skapa först en blandning av nivåindelade volymer och nivåindelade volymer med arkiverade alternativet är markerat. Den här åtgärden säkerställer att hello-verktyget körs hello tester för både 64 KB och 512 KB blob-storlekar.

2. Kör hello cmdlet när du har skapat och konfigurerat hello volymer. Ange:

    `Invoke-HcsDiagnostics -Scope Performance`

3. Anteckna hello skrivskyddad fördröjningar som rapporterats av hello-verktyget. Det här testet kan ta flera minuter toorun innan den rapporterar hello resultat.

4. Om hello anslutning svarstiderna är alla under hello förväntade intervall och hello fördröjningar som rapporterats av hello verktyget kan användas som maximalt hastigheterna mål när du distribuerar hello arbetsbelastningar. Ta hänsyn till vissa kostnader för interna databearbetning.

    Hello-diagnostik är hög om hello skrivskyddad fördröjningar som rapporterats av:

    1. Konfigurera Storage Analytics för blob-tjänster och analysera hello utdata toounderstand hello svarstiderna för hello Azure storage-konto. Detaljerade anvisningar finns för[aktivera och konfigurera Storage Analytics](../storage/common/storage-enable-and-view-metrics.md). Om dessa svarstider är hög och jämförbara toohello siffror som du har fått från hello StorSimple-diagnostik, måste toolog en tjänstbegäran med Azure storage.

    2. Om hello storage-konto svarstiderna låg kontaktar du din nätverket administratören tooinvestigate svarstid för eventuella problem i nätverket.

#### <a name="sample-output-of-performance-test-run-on-an-8100-device"></a>Exempel på utdata av prestandatest som körs på en 8100-enhet

```
Controller0>Invoke-HcsDiagnostics -Scope Performance
Running performance diagnostics...
--------------------------------------------------
Cloud performance: writing blobs
Cloud write latency: 194 ms using credential 'myss8000storageacct', blob size '64KB'
Cloud performance: reading blobs..
Cloud read latency: 544 ms using credential 'myss8000storageacct', blob size '64KB'
Cloud performance: writing blobs.
Cloud write latency: 369 ms using credential 'myss8000storageacct', blob size '512KB'
Cloud performance: reading blobs...
Cloud read latency: 4924 ms using credential 'myss8000storageacct', blob size '512KB'
--------------------------------------------------
Controller0>
```

## <a name="appendix-interpreting-system-information"></a>Bilaga: tolka Systeminformation

Här är en tabell som beskriver vilka hello olika Windows PowerShell-parametrar i hello Systeminformation mappa till. 

| PowerShell-Parameter    | Beskrivning  |
|-------------------------|------------------|
| Instans-ID             | Varje domänkontrollant har en unik identifierare eller ett GUID som är kopplade till den.|
| Namn                    | hello eget namn på hello enheten som konfigurerats via hello Azure-portalen under distributionen av enheten. eget namn för hello standard är hello enhetens serienummer. |
| Modellen                   | hello modell för enheten StorSimple 8000-serien. hello modell kan vara 8100 eller 8600.|
| Serienummer            | hello enhetens serienummer som tilldelas i hello fabriken och 15 tecken. Exempelvis anger 8600-SHX0991003G44HT:<br> 8600 – är hello enhetsmodell.<br>SHX – är hello tillverkar plats.<br> 0991003 - är en specifik produkt. <br> G44HT hello 5 sista siffrorna är stegvis toocreate unika serienummer. Det får inte vara en sekventiell uppsättning.|
| Tidszon                | hello enheten tidszon som konfigurerats i hello Azure-portalen under distributionen av enheten.|
| CurrentController       | hello-domänkontrollant som du är ansluten toothrough hello Windows PowerShell-gränssnittet för din StorSimple-enhet.|
| ActiveController        | hello-domänkontrollant som är aktiv på enheten och kontrollerar alla hello nätverks- och åtgärder. Detta kan vara styrenhet 0 eller 1 för domänkontrollanten.  |
| Controller0Status       | hello status för styrenhet 0 på enheten. status för hello domänkontrollanten kan vara normal, i återställningsläge eller kan inte nås.|
| Controller1Status       | hello status för styrenheten 1 på enheten.  status för hello domänkontrollanten kan vara normal, i återställningsläge eller kan inte nås.|
| SystemMode              | Hej övergripande status för din StorSimple-enhet. hello Enhetsstatus kan vara normal, underhåll, eller inaktiverade (motsvarar toodeactivated i hello Azure-portalen).|
| FriendlySoftwareVersion | hello eget sträng som motsvarar toohello programvaruversionen för enheten. För ett system som kör uppdatering 4 blir hello eget programvaruversionen StorSimple 8000 Series uppdatering 4.0.|
| HcsSoftwareVersion      | Hej HCS programvaruversionen körs på enheten. Till exempel hello HCS programvara version motsvarande tooStorSimple 8000-serien uppdatering 4.0 är 6.3.9600.17820. |
| ApiVersion              | hello programvaruversion hello Windows PowerShell-API för hello HCS enhet.|
| VhdVersion              | hello programvaruversion hello fabriksavbildning som hello enheten levererades med. Om du återställer din enhet toofactory standardvärden kör den här programvaruversionen.|
| OSVersion               | hello programvaruversion hello Windows Server-operativsystem körs på hello enhet. Hej StorSimple-enhet baseras hello Windows Server 2012 R2 som motsvarar too6.3.9600.|
| CisAgentVersion         | hello-version för din TIS-agenten som körs på din StorSimple-enhet. Den här agenten kan kommunicera med hello StorSimple Manager-tjänsten körs i Azure.|
| MdsAgentVersion         | hello version motsvarande toohello Mds-agenten som körs på din StorSimple-enhet. Den här agenten flyttar data toohello övervakning och diagnostik Service (MDS).|
| Lsisas2Version          | Hej version motsvarande toohello LSI drivrutiner på din StorSimple-enhet.|
| Kapacitet                | hello total kapacitet för hello-enhet i byte.|
| RemoteManagementMode    | Anger om hello enheten kan fjärrhanteras via dess Windows PowerShell-gränssnittet. |
| FipsMode                | Anger om hello USA FIPS Federal Information Processing Standard ()-läge är aktiverat på enheten. hello FIPS 140-standarden definierar kryptografiska algoritmer som godkänts för användning av oss federala myndigheter datorsystem för hello skyddet av känsliga data. FIPS-läge är aktiverat som standard för enheter som kör uppdatering 4 eller senare. |

## <a name="next-steps"></a>Nästa steg

* Läs hello [syntaxen för hello Invoke-HcsDiagnostics cmdlet](https://technet.microsoft.com/library/mt795371.aspx).

* Läs mer om hur för[felsöka distributionsproblem](storsimple-troubleshoot-deployment.md) på StorSimple-enheten.
