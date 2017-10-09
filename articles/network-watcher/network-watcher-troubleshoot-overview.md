---
title: "aaaIntroduction tooresource felsökning i Azure-Nätverksbevakaren | Microsoft Docs"
description: "Den här sidan innehåller en översikt över funktioner för hello Nätverksbevakaren resurs felsökning"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: c1145cd6-d1cf-4770-b1cc-eaf0464cc315
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: gwallace
ms.openlocfilehash: ccbe4c1c2364473aba06e709460d67c773cf25ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooresource-troubleshooting-in-azure-network-watcher"></a>Introduktion tooresource felsökning i Azure Nätverksbevakaren

Virtuella Nätverksgatewayer anslutningsbarhet mellan lokala resurser och andra virtuella nätverk i Azure. Övervaka dessa gateways och deras anslutningar är kritiska tooensuring kommunikation är inte skadad. Nätverksbevakaren ger hello kapaciteten tootroubleshoot virtuella Nätverksgatewayer och anslutningar. Det kan anropas via hello portal, PowerShell, CLI eller REST API. När den anropas, diagnostiserar Nätverksbevakaren hello hälsotillståndet för hello virtuell nätverksgateway eller anslutningen och returnerar hello rätt resultat. Denna begäran är en tidskrävande transaktion, returneras hello resultat när hello diagnos är klar.

![portal][2]

## <a name="results"></a>Resultat

hello preliminära resultat returneras ger en övergripande bild av hello hälsotillstånd hello resurs. Mer detaljerad information kan anges för resurser som visas i följande avsnitt hello:

hello följande lista finns hello-värden som returneras med hello felsöka API:

* **startTime** -värdet är hello tid hello felsöka API-anrop som är igång.
* **endTime** -värdet är hello tidpunkt då hello felsökning avslutades.
* **koden** -värdet är ohälsosamma, om det finns ett enda diagnos-fel.
* **resultaten** -resultat är en samling av resultaten på hello anslutning eller hello virtuell nätverksgateway.
    * **ID** -värdet är hello feltypen.
    * **Översikt över** -värdet är en sammanfattning av hello-fel.
    * **detaljerad** -värdet ger en detaljerad beskrivning av hello-fel.
    * **recommendedActions** -den här egenskapen är en uppsättning rekommenderade åtgärder tootake.
      * **actionText** -värdet innehåller hello text som beskriver vilka åtgärden tootake.
      * **actionUri** -värdet innehåller hello URI toodocumentation tooact.
      * **actionUriText** -det här värdet är en kort beskrivning av hello åtgärd text.

hello följande tabeller visar hello olika fel typer (id under resultat från hello föregående lista) som är tillgängliga och om hello fel skapar loggar.

### <a name="gateway"></a>Gateway

| Feltypen | Orsak | Logga|
|---|---|---|
| NoFault | Om inga fel har identifierats. |Ja|
| GatewayNotFound | Det går inte att hitta Gateway eller Gateway inte har etablerats. |Nej|
| PlannedMaintenance |  Gateway-instans är under underhåll.  |Nej|
| UserDrivenUpdate | När en användare uppdatering pågår. Detta kan vara en åtgärd för storleksändring. | Nej |
| VipUnResponsive | Det går inte att nå hello primära instansen av hello Gateway. Detta händer när hello hälsoavsökningen misslyckas. | Nej |
| PlatformInActive | Det finns ett problem med hello-plattformen. | Nej|
| ServiceNotRunning | hello underliggande tjänst körs inte. | Nej|
| NoConnectionsFoundForGateway | Det finns inga anslutningar på hello gateway. Detta är endast en varning.| Nej|
| ConnectionsNotConnected | Anslutningar är inte ansluten. Detta är endast en varning.| Ja|
| GatewayCPUUsageExceeded | hello Gateway processorkraft är > 95%. | Ja |

### <a name="connection"></a>Anslutning

| Feltypen | Orsak | Logga|
|---|---|---|
| NoFault | Om inga fel har identifierats. |Ja|
| GatewayNotFound | Det går inte att hitta Gateway eller Gateway inte har etablerats. |Nej|
| PlannedMaintenance | Gateway-instans är under underhåll.  |Nej|
| UserDrivenUpdate | När en användare uppdatering pågår. Detta kan vara en åtgärd för storleksändring.  | Nej |
| VipUnResponsive | Det går inte att nå hello primära instansen av hello Gateway. Det händer när hello hälsoavsökningen misslyckas. | Nej |
| ConnectionEntityNotFound | Konfigurationen för anslutningen saknas. | Nej |
| ConnectionIsMarkedDisconnected | hello anslutning markeras ”frånkopplad”. |Nej|
| ConnectionNotConfiguredOnGateway | hello underliggande tjänsten har inte hello anslutning konfigureras. | Ja |
| ConnectionMarkedStandy | hello underliggande tjänsten har markerats som vänteläge.| Ja|
| Autentisering | I förväg delad nyckel matchar inte. | Ja|
| PeerReachability | hello peer-gateway kan inte nås. | Ja|
| IkePolicyMismatch | hello peer-gateway har IKE-principer som inte stöds av Azure. | Ja|
| WfpParse fel | Ett fel uppstod tolkning hello WFP-loggen. |Ja|

## <a name="supported-gateway-types"></a>Gateway-typer som stöds

hello följande lista visar hello stöd visar vilka gateways och anslutningar stöds med Nätverksbevakaren felsökning.
|  |  |
|---------|---------|
|**Gateway-typer**   |         |
|VPN      | Stöds        |
|ExpressRoute | Stöds inte |
|Hypernet | Stöds inte|
|**VPN-typer** | |
|Vägen baserat | Stöds|
|Principbaserad | Stöds inte|
|**Anslutningstyper**||
|IPSec| Stöds|
|VNet2Vnet| Stöds|
|ExpressRoute| Stöds inte|
|Hypernet| Stöds inte|
|VPNClient| Stöds inte|

## <a name="log-files"></a>Loggfiler

hello resurs felsökning loggfilerna lagras i ett lagringskonto efter resurs felsökning har slutförts. hello visar följande bild hello exempel innehållet i ett anrop som resulterade i ett fel.

![ZIP-filen][1]

> [!NOTE]
> I vissa fall kan skrivs endast en delmängd av hello loggfiler toostorage.

Anvisningar för hämtning av filer från azure storage-konton finns för[komma igång med Azure Blob storage med hjälp av .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Ett annat verktyg som kan användas är Lagringsutforskaren. Mer information om Lagringsutforskaren hittar du här på hello följande länk: [Lagringsutforskaren](http://storageexplorer.com/)

### <a name="connectionstatstxt"></a>ConnectionStats.txt

Hej **ConnectionStats.txt** filen innehåller övergripande statistik för hello anslutningen, inklusive ingående och utgående byte och anslutningsstatus hello tid hello anslutning har upprättats.

> [!NOTE]
> Om hello anropet toohello felsökning API returnerar felfri hello enda returneras i hello zip-filen är en **ConnectionStats.txt** fil.

hello innehållet i den här filen är liknande toohello följande exempel:

```
Connectivity State : Connected
Remote Tunnel Endpoint :
Ingress Bytes (since last connected) : 288 B
Egress Bytes (Since last connected) : 288 B
Connected Since : 2/1/2017 8:22:06 PM
```

### <a name="cpustatstxt"></a>CPUStats.txt

Hej **CPUStats.txt** filen innehåller CPU-användning och minne för närvarande hello tester.  hello innehållet i den här filen är liknande toohello följande exempel:

```
Current CPU Usage : 0 % Current Memory Available : 641 MBs
```

### <a name="ikeerrorstxt"></a>IKEErrors.txt

Hej **IKEErrors.txt** filen innehåller några IKE-fel som identifierades under övervakning.

hello visar följande exempel hello innehållet i en IKEErrors.txt-fil. Felen kan vara olika beroende på hello problemet.

```
Error: Authentication failed. Check shared key. Check crypto. Check lifetimes. 
     based on log : Peer failed with Windows error 13801(ERROR_IPSEC_IKE_AUTH_FAIL)
Error: On-prem device sent invalid payload. 
     based on log : IkeFindPayloadInPacket failed with Windows error 13843(ERROR_IPSEC_IKE_INVALID_PAYLOAD)
```

### <a name="scrubbed-wfpdiagtxt"></a>Befordras wfpdiag.txt

Hej **Scrubbed wfpdiag.txt** loggfilen innehåller hello wfp-loggen. Den här loggfilen innehåller loggning av paketet släpp och IKE/AuthIP fel.

hello visar följande exempel hello innehållet i hello Scrubbed wfpdiag.txt fil. I det här exemplet hello delade nyckeln för en anslutning inte på rätt sätt kan ses från hello 3: e raden från hello längst ned. hello följande exempel är bara ett fragment för hello hela loggen, eftersom hello loggen kan vara tidskrävande beroende på hello problemet.

```
...
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|Deleted ICookie from hello high priority thread pool list
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|IKE diagnostic event:
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|Event Header:
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Timestamp: 1601-01-01T00:00:00.000Z
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Flags: 0x00000106
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|    Local address field set
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|    Remote address field set
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|    IP version field set
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  IP version: IPv4
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  IP protocol: 0
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Local address: 13.78.238.92
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Remote address: 52.161.24.36
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Local Port: 0
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Remote Port: 0
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Application ID:
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  User SID: <invalid>
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|Failure type: IKE/Authip Main Mode Failure
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|Type specific info:
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Failure error code:0x000035e9
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|    IKE authentication credentials are unacceptable
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Failure point: Remote
...
```

### <a name="wfpdiagtxtsum"></a>wfpdiag.txt.SUM

Hej **wfpdiag.txt.sum** filen är en logg som visar hello buffertar och händelser som har bearbetats.

hello är följande exempel hello innehållet i en wfpdiag.txt.sum-fil.
```
Files Processed:
    C:\Resources\directory\924336c47dd045d5a246c349b8ae57f2.GatewayTenantWorker.DiagnosticsStorage\2017-02-02T17-34-23\wfpdiag.etl
Total Buffers Processed 8
Total Events  Processed 2169
Total Events  Lost      0
Total Format  Errors    0
Total Formats Unknown   486
Elapsed Time            330 sec
+-----------------------------------------------------------------------------------+
|EventCount    EventName            EventType   TMF                                 |
+-----------------------------------------------------------------------------------+
|        36    ikeext               ike_addr_utils_c844  a0c064ca-d954-350a-8b2f-1a7464eef8b6|
|        12    ikeext               ike_addr_utils_c857  a0c064ca-d954-350a-8b2f-1a7464eef8b6|
|        96    ikeext               ike_addr_utils_c832  a0c064ca-d954-350a-8b2f-1a7464eef8b6|
|         6    ikeext               ike_bfe_callbacks_c133  1dc2d67f-8381-6303-e314-6c1452eeb529|
|         6    ikeext               ike_bfe_callbacks_c61  1dc2d67f-8381-6303-e314-6c1452eeb529|
|        12    ikeext               ike_sa_management_c5698  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|         6    ikeext               ike_sa_management_c8447  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|        12    ikeext               ike_sa_management_c494  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|        12    ikeext               ike_sa_management_c642  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|         6    ikeext               ike_sa_management_c3162  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|        12    ikeext               ike_sa_management_c3307  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
```

## <a name="next-steps"></a>Nästa steg

Lär dig hur toodiagnose VPN-gatewayer och anslutningar via hello portal genom att besöka [Gateway felsökning – Azure-portalen](network-watcher-troubleshoot-manage-portal.md).
<!--Image references-->

[1]: ./media/network-watcher-troubleshoot-overview/GatewayTenantWorkerLogs.png
[2]: ./media/network-watcher-troubleshoot-overview/portal.png
