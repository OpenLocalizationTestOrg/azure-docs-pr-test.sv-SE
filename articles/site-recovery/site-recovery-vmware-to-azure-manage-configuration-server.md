---
title: " Hantera en konfigurationsserver i Azure Site Recovery | Microsoft Docs"
description: "Den här artikeln beskriver hur tooset och hantera en Server Configuration."
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
ms.openlocfilehash: 2852bcd25409121be46a1ebf135ebfcdce6e5de5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-a-configuration-server"></a>Hantera en konfigurationsserver

Konfigurationsservern fungerar som en koordinator mellan hello Site Recovery services och lokal infrastruktur. Den här artikeln beskriver hur du kan skapa, konfigurera och hantera hello konfigurationsservern.

## <a name="prerequisites"></a>Krav
hello följande är hello minimikraven för maskinvara, programvara och nätverk konfiguration krävs tooset upp en konfigurationsserver.

> [!NOTE]
> [Kapacitetsplanering](site-recovery-capacity-planner.md) är ett viktigt steg tooensure som du distribuerar hello konfigurationsservern med en konfiguration som paket belastningen-krav. Läs mer om [storleksanpassa kraven för en konfigurationsserver](#sizing-requirements-for-a-configuration-server).

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

## <a name="downloading-hello-configuration-server-software"></a>Hämtar hello Configuration Server-programvara
1. Logga in toohello Azure-portalen och bläddra tooyour Recovery Services-valvet.
2. Bläddra för**Site Recovery-infrastruktur** > **Konfigurationsservrar** (under för VMware och fysiska datorer).

  ![Lägg till servrar på sidan](./media/site-recovery-vmware-to-azure-manage-configuration-server/AddServers.png)
3. Klicka på hello **+ servrar** knappen.
4. På hello **Lägg till Server** klickar du på hello Download knappen toodownload hello registreringsnyckel. Du behöver den här nyckeln under hello konfigurationsservern installation tooregister den med Azure Site Recovery-tjänsten.
5. Klicka på hello **Download hello installationsprogram för Microsoft Azure Site Recovery enhetlig** länk toodownload hello senaste versionen av hello konfigurationsservern.

  ![Hämtningssidan](./media/site-recovery-vmware-to-azure-manage-configuration-server/downloadcs.png)

  > [!TIP]
  Senaste versionen av hello konfigurationsservern kan hämtas direkt från [hämtningssidan för Microsoft Download Center](http://aka.ms/unifiedsetup)

## <a name="installing-and-registering-a-configuration-server-from-gui"></a>Installera och registrera en Server Configuration från GUI
[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

## <a name="installing-and-registering-a-configuration-server-using-command-line"></a>Installera och registrera en konfigurationsservern med hjälp av kommandoraden

  ```
  UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address toobe used for data transfer] [/CSIP <IP address of CS toobe registered with>] [/PassphraseFilePath <Passphrase file path>]
  ```

### <a name="sample-usage"></a>Exempel på användning
  ```
  MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /xC:\Temp\Extracted
  cd C:\Temp\Extracted
  UNIFIEDSETUP.EXE /AcceptThirdpartyEULA /servermode "CS" /InstallLocation "D:\" /MySQLCredsFilePath "C:\Temp\MySQLCredentialsfile.txt" /VaultCredsFilePath "C:\Temp\MyVault.vaultcredentials" /EnvType "VMWare"
  ```


### <a name="configuration-server-installer-command-line-arguments"></a>Konfiguration av servern installer kommandoradsargument.
[!INCLUDE [site-recovery-unified-setup-parameters](../../includes/site-recovery-unified-installer-command-parameters.md)]


### <a name="create-a-mysql-credentials-file"></a>Skapa en MySql-fil för autentiseringsuppgifter
Parametern MySQLCredsFilePath använder en fil som indata. Skapa hello-fil med hello följande format och skickar den som indataparameter till MySQLCredsFilePath.
```
[MySQLCredentials]
MySQLRootPassword = "Password>"
MySQLUserPassword = "Password"
```
### <a name="create-a-proxy-settings-configuration-file"></a>Skapa en konfigurationsfil för proxy-inställningar
Parametern ProxySettingsFilePath använder en fil som indata. Skapa hello-fil med hello följande format och skickar den som indataparameter till ProxySettingsFilePath.

```
[ProxySettings]
ProxyAuthentication = "Yes/No"
Proxy IP = "IP Address"
ProxyPort = "Port"
ProxyUserName="UserName"
ProxyPassword="Password"
```
## <a name="modifying-proxy-settings-for-configuration-server"></a>Ändra proxyinställningar för konfigurationsservern
1. Inloggningen tooyour konfigurationsservern.
2. Starta hello cspsconfigtool.exe med hello genvägen på din.
3. Klicka på hello **valvet registrering** fliken.
4. Hämta en ny fil i valvet registrering från hello-portalen och ange som inkommande toohello verktyg.

  ![registrera konfigurationsservern](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)
5. Anger hello ny proxyserver information och klickar på hello **registrera** knappen.
6. Öppna ett kommandofönster Admin PowerShell.
7. Kör följande kommando hello
  ```
  $pwd = ConvertTo-SecureString -String MyProxyUserPassword
  Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
  net stop obengine
  net start obengine
  ```

  >[!WARNING]
  Om du har servrar skalbar kopplade toothis konfigurationsservern måste du för[åtgärda hello proxyinställningarna på alla servrar som hello skalbar processen](site-recovery-vmware-to-azure-manage-scaleout-process-server.md#modifying-proxy-settings-for-scale-out-process-server) i distributionen.

## <a name="re-register-a-configuration-server-with-hello-same-recovery-services-vault"></a>Registrera en konfigurationsserver med hello samma Recovery Services-valvet
  1. Inloggningen tooyour konfigurationsservern.
  2. Starta hello cspsconfigtool.exe använder hello genvägen på skrivbordet.
  3. Klicka på hello **valvet registrering** fliken.
  4. Hämta en ny registreringsfil från hello-portalen och ange som inkommande toohello verktyg.
        ![registrera konfigurationsservern](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)
  5. Ange hello proxyserver information och klicka på hello **registrera** knappen.  
  6. Öppna ett kommandofönster Admin PowerShell.
  7. Kör följande kommando hello

      ```
      $pwd = ConvertTo-SecureString -String MyProxyUserPassword
      Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
      net stop obengine
      net start obengine
      ```

  >[!WARNING]
  Om du har servrar skalbar kopplade toothis konfigurationsservern måste du för[Omregistrera alla hello skalbara servrar](site-recovery-vmware-to-azure-manage-scaleout-process-server.md#re-registering-a-scale-out-process-server) i distributionen.

## <a name="registering-a-configuration-server-with-a-different-recovery-services-vault"></a>Registrerar en konfigurationsserver med en annan Recovery Services-valvet.
1. Inloggningen tooyour konfigurationsservern.
2. Kör hello-kommando från en kommandotolk admin

```
reg delete HKLM\Software\Microsoft\Azure Site Recovery\Registration
net stop dra
```
3. Starta hello cspsconfigtool.exe med hello genvägen på din.
4. Klicka på hello **valvet registrering** fliken.
5. Hämta en ny registreringsfil från hello-portalen och ange som inkommande toohello verktyg.

    ![registrera konfigurationsservern](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)
6. Ange hello proxyserver information och klicka på hello **registrera** knappen.  
7. Öppna ett kommandofönster Admin PowerShell.
8. Kör följande kommando hello
    ```
    $pwd = ConvertTo-SecureString -String MyProxyUserPassword
    Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
    net stop obengine
    net start obengine
    ```

## <a name="decommissioning-a-configuration-server"></a>Inaktiverar en konfigurationsserver
Kontrollera hello följande innan du börjar inaktivering av serverns konfiguration.
1. Inaktivera skyddet för alla virtuella datorer under den här konfigurationen-servern.
2. Kopplingen mellan alla replikeringsprinciper från hello konfigurationsservern.
3. Ta bort alla Vcenter-servrar/vSphere-värdar som är associerade toohello konfigurationsservern.

### <a name="delete-hello-configuration-server-from-azure-portal"></a>Ta bort hello konfigurationsservern från Azure-portalen
1. I Azure-portalen, bläddra för**Site Recovery-infrastruktur** > **Konfigurationsservrar** hello valvet-menyn.
2. Klicka på hello konfigurationsservern som du vill toodecommission.
3. På hello Configuration Server information klickar du på knappen för hello ta bort.

  ![ta bort konfigurationsservern](./media/site-recovery-vmware-to-azure-manage-configuration-server/delete-configuration-server.PNG)
4. Klicka på **Ja** tooconfirm hello borttagning av hello-server.

  >[!WARNING]
  Om du har virtuella datorer, replikeringsprinciper eller vCenter-servrar/vSphere-värdar som är associerade med den här konfigurationsservern kan du ta bort hello-server. Ta bort dessa enheter innan du försöker toodelete hello valvet.

### <a name="uninstall-hello-configuration-server-software-and-its-dependencies"></a>Avinstallera hello konfigurationsservern programvaran och dess beroenden
  > [!TIP]
  Om du planerar tooreuse hello konfigurationsservern med Azure Site Recovery kan sedan du kan hoppa över toostep 4 direkt

1. Logga in toohello konfigurationsservern som administratör.
2. Öppna Kontrollpanelen > Program > avinstallera program
3. För att avinstallera hello i hello följande ordning:
  * Microsoft Azure Recovery Services-agent
  * Microsoft Azure Site Recovery Mobility tjänsten eller Huvudtjänsten målservern
  * Microsoft Azure Site Recovery-providern
  * Server-processen för Microsoft Azure Site Recovery konfigurationsservern
  * Microsoft Azure Site Recovery Configuration Server beroenden
  * MySQL-Server 5.5
4. Kör följande kommando från hello och admin-kommandotolk.
  ```
  reg delete HKLM\Software\Microsoft\Azure Site Recovery\Registration
  ```

## <a name="renew-configuration-server-secure-socket-layerssl-certificates"></a>Förnya Configuration servercertifikat Secure Socket Layer(SSL)
hello konfigurationsservern har ett inbyggda webbserver som samordnar hello hello Mobilitetstjänsten, servrar, aktiviteter och Huvudmålet servrar anslutna toohello konfigurationsservern. hello Configuration Server webbserver använder ett SSL-certifikat tooauthenticate sina klienter. Det här certifikatet har en utgången av tre år och kan förnyas när som helst med följande metod hello:

> [!WARNING]
Certifikatet upphör att gälla kan bara utföras på version 9.4.XXXX. X eller senare. Uppgradera alla hello Azure Site Recovery-komponenter (konfigurationsservern, Processervern, Huvudmålserver, Mobilitetstjänsten) innan du startar hello förnya certifikat arbetsflöde.

1. På hello Azure-portalen, bläddra tooyour valv > Site Recovery-infrastruktur > konfigurationsservern.
2. Klicka på hello konfigurationsservern som du behöver toorenew hello SSL-certifikat.
3. Under hello konfigurationsservern hälsa, kan du se hello utgångsdatum för hello SSL-certifikat.
4. Förnya hello certifikat genom att klicka på hello **förnya certifikat** åtgärd som visas i följande bild hello:

  ![ta bort konfigurationsservern](./media/site-recovery-vmware-to-azure-manage-configuration-server/renew-cert-page.png)

### <a name="secure-socket-layer-certificate-expiry-warning"></a>Secure Socket Layer certifikatupphörandevarning

> [!NOTE]
hello SSL-certifikatets giltighetsperiod för alla installationer som inträffade före maj 2016 angavs tooone år. du har startat ser certifikatet upphör att gälla meddelanden visas i hello Azure-portalen.

1. Om hello Configuration-serverns SSL-certifikat ska tooexpire i hello två månader, startar hello tjänsten meddela användare via hello Azure-portalen och e-post (du måste toobe prenumererar tooAzure Site Recovery-meddelanden). Du ser en meddelandebanderoll på resurssidan hello-valvet.

  ![certifikat-meddelande](./media/site-recovery-vmware-to-azure-manage-configuration-server/ssl-cert-renew-notification.png)
2. Klicka på hello banderoll tooget ytterligare information om hello certifikatet upphör att gälla.

  ![information för certifikat](./media/site-recovery-vmware-to-azure-manage-configuration-server/ssl-cert-expiry-details.png)

  >[!TIP]
  Om i stället för en **förnya nu** knapp som du ser en **uppgradera nu** knappen. Det innebär att det finns vissa komponenter i din miljö som ännu inte har uppgraderat too9.4.xxxx.x eller senare versioner.

## <a name="sizing-requirements-for-a-configuration-server"></a>Ändra storlek på kraven för en konfigurationsserver

| **CPU** | **Minne** | **Cachestorleken för disk** | **Dataändringshastighet** | **Skyddade datorer** |
| --- | --- | --- | --- | --- |
| 8 vCPUs (2 sockets * 4 kärnor @ 2,5 GHz) |16 GB |300 GB |500 GB eller mindre |Replikera färre än 100 datorer. |
| 12 vCPUs (2 sockets * @ 2,5 GHz-6 kärnor) |18 GB |600 GB |500 GB too1 TB |Replikera mellan 100 150 datorer. |
| 16 vCPUs (2 sockets * 8 kärnor @ 2,5 GHz) |32 GB |1 TB |1 TB too2 TB |Replikera mellan 150 200 datorer. |

  >[!TIP]
  Om din dagliga dataomsättningen överskrider 2 TB, eller om du planerar tooreplicate mer än 200 virtuella datorer, bör toodeploy ytterligare processer servrar tooload saldo hello replikeringstrafik. Läs mer om hur toodeploy skalbar processen servrarna.


## <a name="common-issues"></a>Vanliga problem
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]
