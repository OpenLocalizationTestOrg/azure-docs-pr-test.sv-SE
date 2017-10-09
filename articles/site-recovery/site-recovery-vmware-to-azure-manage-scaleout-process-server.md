---
title: " Hantera en skalbar Processerver i Azure Site Recovery | Microsoft Docs"
description: "Den här artikeln beskriver hur tooset och hantera en skalbar Processerver i Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: backup-recovery
ms.date: 06/29/2017
ms.author: anoopkv
ms.openlocfilehash: 3d72f9c2c7014a4ff2fa2af168aa55ad1452eae5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-a-scale-out-process-server"></a>Hantera en skalbar Processerver

Skalbar Processerver fungerar som en koordinator för att överföra data mellan hello Site Recovery services och lokal infrastruktur. Den här artikeln beskriver hur du kan skapa, konfigurera och hantera en skalbar Processerver.

## <a name="prerequisites"></a>Krav
hello följande är hello rekommenderad maskinvara, programvara och nätverk konfiguration krävs tooset upp en skalbar Processerver.

> [!NOTE]
> [Kapacitetsplanering](site-recovery-capacity-planner.md) är ett viktigt steg tooensure som du distribuerar hello skalbar Processerver med en konfiguration som paket belastningen-krav. Läs mer om [skalning egenskaper för en skalbar Processerver](#sizing-requirements-for-a-configuration-server).

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

## <a name="downloading-hello-scale-out-process-server-software"></a>Hämtar hello skalbar Processerver programvara
1. Logga in toohello Azure-portalen och bläddra tooyour Recovery Services-valvet.
2. Bläddra för**Site Recovery-infrastruktur** > **Konfigurationsservrar** (under för VMware och fysiska datorer).
3. Välj din konfiguration server toodrill i hello configuration server informationssidan.
4. Klicka på hello **+ Processervern** knappen.
5. I hello **lägga till processervern** väljer **distribuera en skalbar Processerver lokala** alternativet från hello **Välj önskad toodeploy processervern** listrutan.

  ![Lägg till servrar på sidan](./media/site-recovery-vmware-to-azure-manage-scaleout-process-server/add-process-server.png)
6. Klicka på hello **Download hello installationsprogram för Microsoft Azure Site Recovery enhetlig** länk toodownload hello senaste versionen av hello skalbar Processerver installation.

  > [!WARNING]
  hello versionen av din skalbar Processerver måste vara lika tooor som är mindre än hello Configuration Server-version som körs i din miljö. Ett enkelt sätt tooensure versionskompatibilitet är toouse hello samma installer bitar som du nyligen använt tooinstall/uppdatera din konfigurationsservern.

## <a name="installing-and-registering-a-scale-out-process-server-from-gui"></a>Installera och registrera en skalbar Processerver från GUI
Om du har tooscale upp distributionen utöver 200 källdatorer eller ett totalt dagligt omsättningsuppdateringar av mer än 2 TB, måste ytterligare processer servrar toohandle hello trafikvolym.

Kontrollera hello [storlek rekommendationer för servrar](#size-recommendations-for-the-process-server), och Följ dessa instruktioner tooset in hello processervern. När du skapat hello-server kan du migrera källa datorer toouse den.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-add-process-server.md)]


## <a name="installing-and-registering-a-scale-out-process-server-using-command-line"></a>Installera och registrera en skalbar Processerver med hjälp av kommandoraden

```
UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address toobe used for data transfer] [/CSIP <IP address of CS toobe registered with>] [/PassphraseFilePath <Passphrase file path>]
```

### <a name="sample-usage"></a>Exempel på användning
```
MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /xC:\Temp\Extracted
cd C:\Temp\Extracted
UNIFIEDSETUP.EXE /AcceptThirdpartyEULA /servermode "PS" /InstallLocation "D:\" /EnvType "VMWare" /CSIP "10.150.24.119" /PassphraseFilePath "C:\Users\Administrator\Desktop\Passphrase.txt" /DataTransferSecurePort 443
```

### <a name="scale-out-process-server-installer-command-line-arguments"></a>Skalbar Processerver installer kommandoradsargument.
[!INCLUDE [site-recovery-unified-setup-parameters](../../includes/site-recovery-unified-installer-command-parameters.md)]


### <a name="create-a-proxy-settings-configuration-file"></a>Skapa en konfigurationsfil för proxy-inställningar
Parametern ProxySettingsFilePath använder en fil som indata. Skapa fil med hello följande format och skickar den som indataparameter till ProxySettingsFilePath.
```
* [ProxySettings]
* ProxyAuthentication = "Yes/No"
* Proxy IP = "IP Address"
* ProxyPort = "Port"
* ProxyUserName="UserName"
* ProxyPassword="Password"
```
## <a name="modifying-proxy-settings-for-scale-out-process-server"></a>Ändra proxyinställningar för skalbar Processerver
1. Logga in på servern för skalbar processen.
2. Öppna ett kommandofönster Admin PowerShell.
3. Kör följande kommando hello
  ```
  $pwd = ConvertTo-SecureString -String MyProxyUserPassword
  Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber –ProxyUserName domain\username -ProxyPassword $pwd
  net stop obengine
  net start obengine
  ```
4. Nästa Bläddra i katalog för toohello **%PROGRAMDATA%\ASR\Agent** och hello kör följande kommando
  ```
  cmd
  cdpcli.exe --registermt

  net stop obengine

  net start obengine

  exit
  ```

## <a name="re-registering-a-scale-out-process-server"></a>Registrera en skalbar Processerver
[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

* Nästa gång du öppna en kommandotolk för administratören.
* Bläddra i katalog toohello **%PROGRAMDATA%\ASR\Agent** och kör hello kommando

```
cdpcli.exe --registermt

net stop obengine

net start obengine
```

## <a name="upgrading-a-scale-out-process-server"></a>Uppgradera en skalbar Processerver
[!INCLUDE [site-recovery-vmware-upgrade -process-server](../../includes/site-recovery-vmware-upgrade-process-server-internal.md)]

## <a name="decommissioning-a-scale-out-process-server"></a>Inaktivering av en skalbar Processerver
1. Se till att:
  - Status för Configuration Server-anslutningen visas som **ansluten** i hello Azure-portalen
  - Processervern är fortfarande toocommunicate med hello konfigurationsservern.
2. Logga in toohello processervern som administratör
3. Öppna Kontrollpanelen > Program > avinstallera program
4. För att avinstallera hello i hello ordning som anges efter:
  * Server-processen för Microsoft Azure Site Recovery konfigurationsservern
  * Microsoft Azure Site Recovery Configuration Server beroenden
  * Microsoft Azure Recovery Services-agent

Det kan ta upp too15 minuter för hello Processervern borttagning tooreflect i hello Azure-portalen.

  > [!NOTE]
  Om hello processervern är toocommunicate med hello konfigurationsservern (anslutningens status i portalen är frånkopplad), måste du toofollow hello följande steg toopurge från hello konfigurationsservern.

## <a name="unregistering-a-disconnected-scale-out-process-server-from-a-configuration-server"></a>Avregistrerar en frånkopplad skalbar processerver från en Server Configuration

[!INCLUDE [site-recovery-vmware-upgrade-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]

## <a name="sizing-requirements-for-a-scale-out-process-server"></a>Ändra storlek på kraven för en skalbar Processerver

| **Ytterligare processervern** | **Cachestorleken för disk** | **Dataändringshastighet** | **Skyddade datorer** |
| --- | --- | --- | --- |
|4 vCPUs (2 sockets * 2 kärnor @ 2,5 GHz), 8 GB minne |300 GB |250 GB eller mindre |Replikera 85 eller mindre datorer. |
|8 vCPUs (2 sockets * 4 kärnor @ 2,5 GHz), 12 GB minne |600 GB |250 GB too1 TB |Replikera mellan 85 150 datorer. |
|12 vCPUs (2 sockets * 6 kärnor @ 2,5 GHz) 24 GB minne |1 TB |1 TB too2 TB |Replikera mellan 150 225 datorer. |
