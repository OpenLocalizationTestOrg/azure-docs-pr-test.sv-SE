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
# <a name="manage-a-configuration-server"></a><span data-ttu-id="33f0d-103">Hantera en konfigurationsserver</span><span class="sxs-lookup"><span data-stu-id="33f0d-103">Manage a Configuration Server</span></span>

<span data-ttu-id="33f0d-104">Konfigurationsservern fungerar som en koordinator mellan hello Site Recovery services och lokal infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="33f0d-104">Configuration Server acts as a coordinator between hello Site Recovery services and your on-premises infrastructure.</span></span> <span data-ttu-id="33f0d-105">Den här artikeln beskriver hur du kan skapa, konfigurera och hantera hello konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="33f0d-105">This article describes how you can set up, configure, and manage hello Configuration Server.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="33f0d-106">Krav</span><span class="sxs-lookup"><span data-stu-id="33f0d-106">Prerequisites</span></span>
<span data-ttu-id="33f0d-107">hello följande är hello minimikraven för maskinvara, programvara och nätverk konfiguration krävs tooset upp en konfigurationsserver.</span><span class="sxs-lookup"><span data-stu-id="33f0d-107">hello following are hello minimum hardware, software, and network configuration required tooset up a Configuration Server.</span></span>

> [!NOTE]
> <span data-ttu-id="33f0d-108">[Kapacitetsplanering](site-recovery-capacity-planner.md) är ett viktigt steg tooensure som du distribuerar hello konfigurationsservern med en konfiguration som paket belastningen-krav.</span><span class="sxs-lookup"><span data-stu-id="33f0d-108">[Capacity planning](site-recovery-capacity-planner.md) is an important step tooensure that you deploy hello Configuration Server with a configuration that suites your load requirements.</span></span> <span data-ttu-id="33f0d-109">Läs mer om [storleksanpassa kraven för en konfigurationsserver](#sizing-requirements-for-a-configuration-server).</span><span class="sxs-lookup"><span data-stu-id="33f0d-109">Read more about [Sizing requirements for a Configuration Server](#sizing-requirements-for-a-configuration-server).</span></span>

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

## <a name="downloading-hello-configuration-server-software"></a><span data-ttu-id="33f0d-110">Hämtar hello Configuration Server-programvara</span><span class="sxs-lookup"><span data-stu-id="33f0d-110">Downloading hello Configuration Server software</span></span>
1. <span data-ttu-id="33f0d-111">Logga in toohello Azure-portalen och bläddra tooyour Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="33f0d-111">Log on toohello Azure portal and browse tooyour Recovery Services Vault.</span></span>
2. <span data-ttu-id="33f0d-112">Bläddra för**Site Recovery-infrastruktur** > **Konfigurationsservrar** (under för VMware och fysiska datorer).</span><span class="sxs-lookup"><span data-stu-id="33f0d-112">Browse too**Site Recovery Infrastructure** > **Configuration Servers** (under For VMware & Physical Machines).</span></span>

  ![Lägg till servrar på sidan](./media/site-recovery-vmware-to-azure-manage-configuration-server/AddServers.png)
3. <span data-ttu-id="33f0d-114">Klicka på hello **+ servrar** knappen.</span><span class="sxs-lookup"><span data-stu-id="33f0d-114">Click hello **+Servers** button.</span></span>
4. <span data-ttu-id="33f0d-115">På hello **Lägg till Server** klickar du på hello Download knappen toodownload hello registreringsnyckel.</span><span class="sxs-lookup"><span data-stu-id="33f0d-115">On hello **Add Server** page, click hello Download button toodownload hello Registration key.</span></span> <span data-ttu-id="33f0d-116">Du behöver den här nyckeln under hello konfigurationsservern installation tooregister den med Azure Site Recovery-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="33f0d-116">You need this key during hello Configuration Server installation tooregister it with Azure Site Recovery service.</span></span>
5. <span data-ttu-id="33f0d-117">Klicka på hello **Download hello installationsprogram för Microsoft Azure Site Recovery enhetlig** länk toodownload hello senaste versionen av hello konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="33f0d-117">Click hello **Download hello Microsoft Azure Site Recovery Unified Setup** link toodownload hello latest version of hello Configuration Server.</span></span>

  ![Hämtningssidan](./media/site-recovery-vmware-to-azure-manage-configuration-server/downloadcs.png)

  > [!TIP]
  <span data-ttu-id="33f0d-119">Senaste versionen av hello konfigurationsservern kan hämtas direkt från [hämtningssidan för Microsoft Download Center](http://aka.ms/unifiedsetup)</span><span class="sxs-lookup"><span data-stu-id="33f0d-119">Latest version of hello Configuration Server can be downloaded directly from [Microsoft Download Center download page](http://aka.ms/unifiedsetup)</span></span>

## <a name="installing-and-registering-a-configuration-server-from-gui"></a><span data-ttu-id="33f0d-120">Installera och registrera en Server Configuration från GUI</span><span class="sxs-lookup"><span data-stu-id="33f0d-120">Installing and Registering a Configuration Server from GUI</span></span>
[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

## <a name="installing-and-registering-a-configuration-server-using-command-line"></a><span data-ttu-id="33f0d-121">Installera och registrera en konfigurationsservern med hjälp av kommandoraden</span><span class="sxs-lookup"><span data-stu-id="33f0d-121">Installing and registering a Configuration Server using Command line</span></span>

  ```
  UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address toobe used for data transfer] [/CSIP <IP address of CS toobe registered with>] [/PassphraseFilePath <Passphrase file path>]
  ```

### <a name="sample-usage"></a><span data-ttu-id="33f0d-122">Exempel på användning</span><span class="sxs-lookup"><span data-stu-id="33f0d-122">Sample usage</span></span>
  ```
  MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /xC:\Temp\Extracted
  cd C:\Temp\Extracted
  UNIFIEDSETUP.EXE /AcceptThirdpartyEULA /servermode "CS" /InstallLocation "D:\" /MySQLCredsFilePath "C:\Temp\MySQLCredentialsfile.txt" /VaultCredsFilePath "C:\Temp\MyVault.vaultcredentials" /EnvType "VMWare"
  ```


### <a name="configuration-server-installer-command-line-arguments"></a><span data-ttu-id="33f0d-123">Konfiguration av servern installer kommandoradsargument.</span><span class="sxs-lookup"><span data-stu-id="33f0d-123">Configuration Server installer command-line arguments.</span></span>
[!INCLUDE [site-recovery-unified-setup-parameters](../../includes/site-recovery-unified-installer-command-parameters.md)]


### <a name="create-a-mysql-credentials-file"></a><span data-ttu-id="33f0d-124">Skapa en MySql-fil för autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="33f0d-124">Create a MySql credentials file</span></span>
<span data-ttu-id="33f0d-125">Parametern MySQLCredsFilePath använder en fil som indata.</span><span class="sxs-lookup"><span data-stu-id="33f0d-125">MySQLCredsFilePath parameter takes a file as input.</span></span> <span data-ttu-id="33f0d-126">Skapa hello-fil med hello följande format och skickar den som indataparameter till MySQLCredsFilePath.</span><span class="sxs-lookup"><span data-stu-id="33f0d-126">Create hello file using hello following format and pass it as input MySQLCredsFilePath parameter.</span></span>
```
[MySQLCredentials]
MySQLRootPassword = "Password>"
MySQLUserPassword = "Password"
```
### <a name="create-a-proxy-settings-configuration-file"></a><span data-ttu-id="33f0d-127">Skapa en konfigurationsfil för proxy-inställningar</span><span class="sxs-lookup"><span data-stu-id="33f0d-127">Create a proxy settings configuration file</span></span>
<span data-ttu-id="33f0d-128">Parametern ProxySettingsFilePath använder en fil som indata.</span><span class="sxs-lookup"><span data-stu-id="33f0d-128">ProxySettingsFilePath parameter takes a file as input.</span></span> <span data-ttu-id="33f0d-129">Skapa hello-fil med hello följande format och skickar den som indataparameter till ProxySettingsFilePath.</span><span class="sxs-lookup"><span data-stu-id="33f0d-129">Create hello file using hello following format and pass it as input ProxySettingsFilePath parameter.</span></span>

```
[ProxySettings]
ProxyAuthentication = "Yes/No"
Proxy IP = "IP Address"
ProxyPort = "Port"
ProxyUserName="UserName"
ProxyPassword="Password"
```
## <a name="modifying-proxy-settings-for-configuration-server"></a><span data-ttu-id="33f0d-130">Ändra proxyinställningar för konfigurationsservern</span><span class="sxs-lookup"><span data-stu-id="33f0d-130">Modifying proxy settings for Configuration Server</span></span>
1. <span data-ttu-id="33f0d-131">Inloggningen tooyour konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="33f0d-131">Login tooyour Configuration Server.</span></span>
2. <span data-ttu-id="33f0d-132">Starta hello cspsconfigtool.exe med hello genvägen på din.</span><span class="sxs-lookup"><span data-stu-id="33f0d-132">Launch hello cspsconfigtool.exe using hello shortcut on your.</span></span>
3. <span data-ttu-id="33f0d-133">Klicka på hello **valvet registrering** fliken.</span><span class="sxs-lookup"><span data-stu-id="33f0d-133">Click hello **Vault Registration** tab.</span></span>
4. <span data-ttu-id="33f0d-134">Hämta en ny fil i valvet registrering från hello-portalen och ange som inkommande toohello verktyg.</span><span class="sxs-lookup"><span data-stu-id="33f0d-134">Download a new Vault Registration file from hello portal and provide it as input toohello tool.</span></span>

  ![registrera konfigurationsservern](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)
5. <span data-ttu-id="33f0d-136">Anger hello ny proxyserver information och klickar på hello **registrera** knappen.</span><span class="sxs-lookup"><span data-stu-id="33f0d-136">Provide hello new Proxy Server details and click hello **Register** button.</span></span>
6. <span data-ttu-id="33f0d-137">Öppna ett kommandofönster Admin PowerShell.</span><span class="sxs-lookup"><span data-stu-id="33f0d-137">Open an Admin PowerShell command window.</span></span>
7. <span data-ttu-id="33f0d-138">Kör följande kommando hello</span><span class="sxs-lookup"><span data-stu-id="33f0d-138">Run hello following command</span></span>
  ```
  $pwd = ConvertTo-SecureString -String MyProxyUserPassword
  Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
  net stop obengine
  net start obengine
  ```

  >[!WARNING]
  <span data-ttu-id="33f0d-139">Om du har servrar skalbar kopplade toothis konfigurationsservern måste du för[åtgärda hello proxyinställningarna på alla servrar som hello skalbar processen](site-recovery-vmware-to-azure-manage-scaleout-process-server.md#modifying-proxy-settings-for-scale-out-process-server) i distributionen.</span><span class="sxs-lookup"><span data-stu-id="33f0d-139">If you have Scale-out Process servers attached toothis Configuration Server, you need too[fix hello proxy settings on all hello scale-out process servers](site-recovery-vmware-to-azure-manage-scaleout-process-server.md#modifying-proxy-settings-for-scale-out-process-server) in your deployment.</span></span>

## <a name="re-register-a-configuration-server-with-hello-same-recovery-services-vault"></a><span data-ttu-id="33f0d-140">Registrera en konfigurationsserver med hello samma Recovery Services-valvet</span><span class="sxs-lookup"><span data-stu-id="33f0d-140">Re-register a Configuration Server with hello same Recovery Services Vault</span></span>
  1. <span data-ttu-id="33f0d-141">Inloggningen tooyour konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="33f0d-141">Login tooyour Configuration Server.</span></span>
  2. <span data-ttu-id="33f0d-142">Starta hello cspsconfigtool.exe använder hello genvägen på skrivbordet.</span><span class="sxs-lookup"><span data-stu-id="33f0d-142">Launch hello cspsconfigtool.exe using hello shortcut on your desktop.</span></span>
  3. <span data-ttu-id="33f0d-143">Klicka på hello **valvet registrering** fliken.</span><span class="sxs-lookup"><span data-stu-id="33f0d-143">Click hello **Vault Registration** tab.</span></span>
  4. <span data-ttu-id="33f0d-144">Hämta en ny registreringsfil från hello-portalen och ange som inkommande toohello verktyg.</span><span class="sxs-lookup"><span data-stu-id="33f0d-144">Download a new Registration file from hello portal and provide it as input toohello tool.</span></span>
        <span data-ttu-id="33f0d-145">![registrera konfigurationsservern](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)</span><span class="sxs-lookup"><span data-stu-id="33f0d-145">![register-configuration-server](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)</span></span>
  5. <span data-ttu-id="33f0d-146">Ange hello proxyserver information och klicka på hello **registrera** knappen.</span><span class="sxs-lookup"><span data-stu-id="33f0d-146">Provide hello Proxy Server details and click hello **Register** button.</span></span>  
  6. <span data-ttu-id="33f0d-147">Öppna ett kommandofönster Admin PowerShell.</span><span class="sxs-lookup"><span data-stu-id="33f0d-147">Open an Admin PowerShell command window.</span></span>
  7. <span data-ttu-id="33f0d-148">Kör följande kommando hello</span><span class="sxs-lookup"><span data-stu-id="33f0d-148">Run hello following command</span></span>

      ```
      $pwd = ConvertTo-SecureString -String MyProxyUserPassword
      Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
      net stop obengine
      net start obengine
      ```

  >[!WARNING]
  <span data-ttu-id="33f0d-149">Om du har servrar skalbar kopplade toothis konfigurationsservern måste du för[Omregistrera alla hello skalbara servrar](site-recovery-vmware-to-azure-manage-scaleout-process-server.md#re-registering-a-scale-out-process-server) i distributionen.</span><span class="sxs-lookup"><span data-stu-id="33f0d-149">If you have Scale-out Process servers attached toothis Configuration Server, you need too[re-register all hello scale-out process servers](site-recovery-vmware-to-azure-manage-scaleout-process-server.md#re-registering-a-scale-out-process-server) in your deployment.</span></span>

## <a name="registering-a-configuration-server-with-a-different-recovery-services-vault"></a><span data-ttu-id="33f0d-150">Registrerar en konfigurationsserver med en annan Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="33f0d-150">Registering a Configuration Server with a different Recovery Services Vault.</span></span>
1. <span data-ttu-id="33f0d-151">Inloggningen tooyour konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="33f0d-151">Login tooyour Configuration Server.</span></span>
2. <span data-ttu-id="33f0d-152">Kör hello-kommando från en kommandotolk admin</span><span class="sxs-lookup"><span data-stu-id="33f0d-152">from an admin command prompt, run hello command</span></span>

```
reg delete HKLM\Software\Microsoft\Azure Site Recovery\Registration
net stop dra
```
3. <span data-ttu-id="33f0d-153">Starta hello cspsconfigtool.exe med hello genvägen på din.</span><span class="sxs-lookup"><span data-stu-id="33f0d-153">Launch hello cspsconfigtool.exe using hello shortcut on your.</span></span>
4. <span data-ttu-id="33f0d-154">Klicka på hello **valvet registrering** fliken.</span><span class="sxs-lookup"><span data-stu-id="33f0d-154">Click hello **Vault Registration** tab.</span></span>
5. <span data-ttu-id="33f0d-155">Hämta en ny registreringsfil från hello-portalen och ange som inkommande toohello verktyg.</span><span class="sxs-lookup"><span data-stu-id="33f0d-155">Download a new Registration file from hello portal and provide it as input toohello tool.</span></span>

    ![registrera konfigurationsservern](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)
6. <span data-ttu-id="33f0d-157">Ange hello proxyserver information och klicka på hello **registrera** knappen.</span><span class="sxs-lookup"><span data-stu-id="33f0d-157">Provide hello Proxy Server details and click hello **Register** button.</span></span>  
7. <span data-ttu-id="33f0d-158">Öppna ett kommandofönster Admin PowerShell.</span><span class="sxs-lookup"><span data-stu-id="33f0d-158">Open an Admin PowerShell command window.</span></span>
8. <span data-ttu-id="33f0d-159">Kör följande kommando hello</span><span class="sxs-lookup"><span data-stu-id="33f0d-159">Run hello following command</span></span>
    ```
    $pwd = ConvertTo-SecureString -String MyProxyUserPassword
    Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
    net stop obengine
    net start obengine
    ```

## <a name="decommissioning-a-configuration-server"></a><span data-ttu-id="33f0d-160">Inaktiverar en konfigurationsserver</span><span class="sxs-lookup"><span data-stu-id="33f0d-160">Decommissioning a Configuration Server</span></span>
<span data-ttu-id="33f0d-161">Kontrollera hello följande innan du börjar inaktivering av serverns konfiguration.</span><span class="sxs-lookup"><span data-stu-id="33f0d-161">Ensure hello following before you start decommissioning your Configuration Server.</span></span>
1. <span data-ttu-id="33f0d-162">Inaktivera skyddet för alla virtuella datorer under den här konfigurationen-servern.</span><span class="sxs-lookup"><span data-stu-id="33f0d-162">Disable protection for all virtual machines under this Configuration Server.</span></span>
2. <span data-ttu-id="33f0d-163">Kopplingen mellan alla replikeringsprinciper från hello konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="33f0d-163">Disassociate all Replication policies from hello Configuration Server.</span></span>
3. <span data-ttu-id="33f0d-164">Ta bort alla Vcenter-servrar/vSphere-värdar som är associerade toohello konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="33f0d-164">Delete all vCenters servers/vSphere hosts that are associated toohello Configuration Server.</span></span>

### <a name="delete-hello-configuration-server-from-azure-portal"></a><span data-ttu-id="33f0d-165">Ta bort hello konfigurationsservern från Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="33f0d-165">Delete hello Configuration Server from Azure portal</span></span>
1. <span data-ttu-id="33f0d-166">I Azure-portalen, bläddra för**Site Recovery-infrastruktur** > **Konfigurationsservrar** hello valvet-menyn.</span><span class="sxs-lookup"><span data-stu-id="33f0d-166">In Azure portal, browse too**Site Recovery Infrastructure** > **Configuration Servers** from hello Vault menu.</span></span>
2. <span data-ttu-id="33f0d-167">Klicka på hello konfigurationsservern som du vill toodecommission.</span><span class="sxs-lookup"><span data-stu-id="33f0d-167">Click hello Configuration Server that you want toodecommission.</span></span>
3. <span data-ttu-id="33f0d-168">På hello Configuration Server information klickar du på knappen för hello ta bort.</span><span class="sxs-lookup"><span data-stu-id="33f0d-168">On hello Configuration Server's details page, click hello Delete button.</span></span>

  ![ta bort konfigurationsservern](./media/site-recovery-vmware-to-azure-manage-configuration-server/delete-configuration-server.PNG)
4. <span data-ttu-id="33f0d-170">Klicka på **Ja** tooconfirm hello borttagning av hello-server.</span><span class="sxs-lookup"><span data-stu-id="33f0d-170">Click **Yes** tooconfirm hello deletion of hello server.</span></span>

  >[!WARNING]
  <span data-ttu-id="33f0d-171">Om du har virtuella datorer, replikeringsprinciper eller vCenter-servrar/vSphere-värdar som är associerade med den här konfigurationsservern kan du ta bort hello-server.</span><span class="sxs-lookup"><span data-stu-id="33f0d-171">If you have any virtual machines, Replication policies or vCenter servers/vSphere hosts associated with this Configuration Server, you cannot delete hello server.</span></span> <span data-ttu-id="33f0d-172">Ta bort dessa enheter innan du försöker toodelete hello valvet.</span><span class="sxs-lookup"><span data-stu-id="33f0d-172">Delete these entities before you try toodelete hello vault.</span></span>

### <a name="uninstall-hello-configuration-server-software-and-its-dependencies"></a><span data-ttu-id="33f0d-173">Avinstallera hello konfigurationsservern programvaran och dess beroenden</span><span class="sxs-lookup"><span data-stu-id="33f0d-173">Uninstall hello Configuration Server software and its dependencies</span></span>
  > [!TIP]
  <span data-ttu-id="33f0d-174">Om du planerar tooreuse hello konfigurationsservern med Azure Site Recovery kan sedan du kan hoppa över toostep 4 direkt</span><span class="sxs-lookup"><span data-stu-id="33f0d-174">If you plan tooreuse hello Configuration Server with Azure Site Recovery again, then you can skip toostep 4 directly</span></span>

1. <span data-ttu-id="33f0d-175">Logga in toohello konfigurationsservern som administratör.</span><span class="sxs-lookup"><span data-stu-id="33f0d-175">Log on toohello Configuration Server as an Administrator.</span></span>
2. <span data-ttu-id="33f0d-176">Öppna Kontrollpanelen > Program > avinstallera program</span><span class="sxs-lookup"><span data-stu-id="33f0d-176">Open up Control Panel > Program > Uninstall Programs</span></span>
3. <span data-ttu-id="33f0d-177">För att avinstallera hello i hello följande ordning:</span><span class="sxs-lookup"><span data-stu-id="33f0d-177">Uninstall hello programs in hello following sequence:</span></span>
  * <span data-ttu-id="33f0d-178">Microsoft Azure Recovery Services-agent</span><span class="sxs-lookup"><span data-stu-id="33f0d-178">Microsoft Azure Recovery Services Agent</span></span>
  * <span data-ttu-id="33f0d-179">Microsoft Azure Site Recovery Mobility tjänsten eller Huvudtjänsten målservern</span><span class="sxs-lookup"><span data-stu-id="33f0d-179">Microsoft Azure Site Recovery Mobility Service/Master Target server</span></span>
  * <span data-ttu-id="33f0d-180">Microsoft Azure Site Recovery-providern</span><span class="sxs-lookup"><span data-stu-id="33f0d-180">Microsoft Azure Site Recovery Provider</span></span>
  * <span data-ttu-id="33f0d-181">Server-processen för Microsoft Azure Site Recovery konfigurationsservern</span><span class="sxs-lookup"><span data-stu-id="33f0d-181">Microsoft Azure Site Recovery Configuration Server/Process Server</span></span>
  * <span data-ttu-id="33f0d-182">Microsoft Azure Site Recovery Configuration Server beroenden</span><span class="sxs-lookup"><span data-stu-id="33f0d-182">Microsoft Azure Site Recovery Configuration Server Dependencies</span></span>
  * <span data-ttu-id="33f0d-183">MySQL-Server 5.5</span><span class="sxs-lookup"><span data-stu-id="33f0d-183">MySQL Server 5.5</span></span>
4. <span data-ttu-id="33f0d-184">Kör följande kommando från hello och admin-kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="33f0d-184">Run hello following command from and admin command prompt.</span></span>
  ```
  reg delete HKLM\Software\Microsoft\Azure Site Recovery\Registration
  ```

## <a name="renew-configuration-server-secure-socket-layerssl-certificates"></a><span data-ttu-id="33f0d-185">Förnya Configuration servercertifikat Secure Socket Layer(SSL)</span><span class="sxs-lookup"><span data-stu-id="33f0d-185">Renew Configuration Server Secure Socket Layer(SSL) Certificates</span></span>
<span data-ttu-id="33f0d-186">hello konfigurationsservern har ett inbyggda webbserver som samordnar hello hello Mobilitetstjänsten, servrar, aktiviteter och Huvudmålet servrar anslutna toohello konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="33f0d-186">hello Configuration Server has an inbuilt webserver, which orchestrates hello activities of hello Mobility Service, Process Servers, and Master Target servers connected toohello Configuration Server.</span></span> <span data-ttu-id="33f0d-187">hello Configuration Server webbserver använder ett SSL-certifikat tooauthenticate sina klienter.</span><span class="sxs-lookup"><span data-stu-id="33f0d-187">hello Configuration Server's webserver uses an SSL certificate tooauthenticate its clients.</span></span> <span data-ttu-id="33f0d-188">Det här certifikatet har en utgången av tre år och kan förnyas när som helst med följande metod hello:</span><span class="sxs-lookup"><span data-stu-id="33f0d-188">This certificate has an expiry of three years and can be renewed at any time using hello following method:</span></span>

> [!WARNING]
<span data-ttu-id="33f0d-189">Certifikatet upphör att gälla kan bara utföras på version 9.4.XXXX. X eller senare.</span><span class="sxs-lookup"><span data-stu-id="33f0d-189">Certificate expiry can be performed only on version 9.4.XXXX.X or higher.</span></span> <span data-ttu-id="33f0d-190">Uppgradera alla hello Azure Site Recovery-komponenter (konfigurationsservern, Processervern, Huvudmålserver, Mobilitetstjänsten) innan du startar hello förnya certifikat arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="33f0d-190">Upgrade all hello Azure Site Recovery components (Configuration Server, Process Server, Master Target Server, Mobility Service) before you start hello Renew Certificates workflow.</span></span>

1. <span data-ttu-id="33f0d-191">På hello Azure-portalen, bläddra tooyour valv > Site Recovery-infrastruktur > konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="33f0d-191">On hello Azure portal, browse tooyour Vault > Site Recovery Infrastructure > Configuration Server.</span></span>
2. <span data-ttu-id="33f0d-192">Klicka på hello konfigurationsservern som du behöver toorenew hello SSL-certifikat.</span><span class="sxs-lookup"><span data-stu-id="33f0d-192">Click hello Configuration Server for which you need toorenew hello SSL Certificate for.</span></span>
3. <span data-ttu-id="33f0d-193">Under hello konfigurationsservern hälsa, kan du se hello utgångsdatum för hello SSL-certifikat.</span><span class="sxs-lookup"><span data-stu-id="33f0d-193">Under hello Configuration Server health, you can see hello expiry date for hello SSL Certificate.</span></span>
4. <span data-ttu-id="33f0d-194">Förnya hello certifikat genom att klicka på hello **förnya certifikat** åtgärd som visas i följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="33f0d-194">Renew hello certificate by clicking hello **Renew Certificates** action as shown in hello following image:</span></span>

  ![ta bort konfigurationsservern](./media/site-recovery-vmware-to-azure-manage-configuration-server/renew-cert-page.png)

### <a name="secure-socket-layer-certificate-expiry-warning"></a><span data-ttu-id="33f0d-196">Secure Socket Layer certifikatupphörandevarning</span><span class="sxs-lookup"><span data-stu-id="33f0d-196">Secure Socket Layer certificate expiry warning</span></span>

> [!NOTE]
<span data-ttu-id="33f0d-197">hello SSL-certifikatets giltighetsperiod för alla installationer som inträffade före maj 2016 angavs tooone år.</span><span class="sxs-lookup"><span data-stu-id="33f0d-197">hello SSL Certificate's validity for all installations that happened before May 2016 was set tooone year.</span></span> <span data-ttu-id="33f0d-198">du har startat ser certifikatet upphör att gälla meddelanden visas i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="33f0d-198">you have started seeing certificate expiry notifications showing up in hello Azure portal.</span></span>

1. <span data-ttu-id="33f0d-199">Om hello Configuration-serverns SSL-certifikat ska tooexpire i hello två månader, startar hello tjänsten meddela användare via hello Azure-portalen och e-post (du måste toobe prenumererar tooAzure Site Recovery-meddelanden).</span><span class="sxs-lookup"><span data-stu-id="33f0d-199">If hello Configuration Server's SSL certificate is going tooexpire in hello next two months, hello service starts notifying users via hello Azure portal & email (you need toobe subscribed tooAzure Site Recovery notifications).</span></span> <span data-ttu-id="33f0d-200">Du ser en meddelandebanderoll på resurssidan hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="33f0d-200">You start seeing a notification banner on hello Vault's resource page.</span></span>

  ![certifikat-meddelande](./media/site-recovery-vmware-to-azure-manage-configuration-server/ssl-cert-renew-notification.png)
2. <span data-ttu-id="33f0d-202">Klicka på hello banderoll tooget ytterligare information om hello certifikatet upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="33f0d-202">Click hello banner tooget additional details on hello Certificate expiry.</span></span>

  ![information för certifikat](./media/site-recovery-vmware-to-azure-manage-configuration-server/ssl-cert-expiry-details.png)

  >[!TIP]
  <span data-ttu-id="33f0d-204">Om i stället för en **förnya nu** knapp som du ser en **uppgradera nu** knappen.</span><span class="sxs-lookup"><span data-stu-id="33f0d-204">If instead of a **Renew Now** button you see an **Upgrade Now** button.</span></span> <span data-ttu-id="33f0d-205">Det innebär att det finns vissa komponenter i din miljö som ännu inte har uppgraderat too9.4.xxxx.x eller senare versioner.</span><span class="sxs-lookup"><span data-stu-id="33f0d-205">This means that there are some components in your environment that have not yet been upgraded too9.4.xxxx.x or higher versions.</span></span>

## <a name="sizing-requirements-for-a-configuration-server"></a><span data-ttu-id="33f0d-206">Ändra storlek på kraven för en konfigurationsserver</span><span class="sxs-lookup"><span data-stu-id="33f0d-206">Sizing requirements for a Configuration Server</span></span>

| <span data-ttu-id="33f0d-207">**CPU**</span><span class="sxs-lookup"><span data-stu-id="33f0d-207">**CPU**</span></span> | <span data-ttu-id="33f0d-208">**Minne**</span><span class="sxs-lookup"><span data-stu-id="33f0d-208">**Memory**</span></span> | <span data-ttu-id="33f0d-209">**Cachestorleken för disk**</span><span class="sxs-lookup"><span data-stu-id="33f0d-209">**Cache disk size**</span></span> | <span data-ttu-id="33f0d-210">**Dataändringshastighet**</span><span class="sxs-lookup"><span data-stu-id="33f0d-210">**Data change rate**</span></span> | <span data-ttu-id="33f0d-211">**Skyddade datorer**</span><span class="sxs-lookup"><span data-stu-id="33f0d-211">**Protected machines**</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="33f0d-212">8 vCPUs (2 sockets * 4 kärnor @ 2,5 GHz)</span><span class="sxs-lookup"><span data-stu-id="33f0d-212">8 vCPUs (2 sockets * 4 cores @ 2.5 GHz)</span></span> |<span data-ttu-id="33f0d-213">16 GB</span><span class="sxs-lookup"><span data-stu-id="33f0d-213">16 GB</span></span> |<span data-ttu-id="33f0d-214">300 GB</span><span class="sxs-lookup"><span data-stu-id="33f0d-214">300 GB</span></span> |<span data-ttu-id="33f0d-215">500 GB eller mindre</span><span class="sxs-lookup"><span data-stu-id="33f0d-215">500 GB or less</span></span> |<span data-ttu-id="33f0d-216">Replikera färre än 100 datorer.</span><span class="sxs-lookup"><span data-stu-id="33f0d-216">Replicate fewer than 100 machines.</span></span> |
| <span data-ttu-id="33f0d-217">12 vCPUs (2 sockets * @ 2,5 GHz-6 kärnor)</span><span class="sxs-lookup"><span data-stu-id="33f0d-217">12 vCPUs (2 sockets * 6 cores @ 2.5 GHz)</span></span> |<span data-ttu-id="33f0d-218">18 GB</span><span class="sxs-lookup"><span data-stu-id="33f0d-218">18 GB</span></span> |<span data-ttu-id="33f0d-219">600 GB</span><span class="sxs-lookup"><span data-stu-id="33f0d-219">600 GB</span></span> |<span data-ttu-id="33f0d-220">500 GB too1 TB</span><span class="sxs-lookup"><span data-stu-id="33f0d-220">500 GB too1 TB</span></span> |<span data-ttu-id="33f0d-221">Replikera mellan 100 150 datorer.</span><span class="sxs-lookup"><span data-stu-id="33f0d-221">Replicate between 100-150 machines.</span></span> |
| <span data-ttu-id="33f0d-222">16 vCPUs (2 sockets * 8 kärnor @ 2,5 GHz)</span><span class="sxs-lookup"><span data-stu-id="33f0d-222">16 vCPUs (2 sockets * 8 cores @ 2.5 GHz)</span></span> |<span data-ttu-id="33f0d-223">32 GB</span><span class="sxs-lookup"><span data-stu-id="33f0d-223">32 GB</span></span> |<span data-ttu-id="33f0d-224">1 TB</span><span class="sxs-lookup"><span data-stu-id="33f0d-224">1 TB</span></span> |<span data-ttu-id="33f0d-225">1 TB too2 TB</span><span class="sxs-lookup"><span data-stu-id="33f0d-225">1 TB too2 TB</span></span> |<span data-ttu-id="33f0d-226">Replikera mellan 150 200 datorer.</span><span class="sxs-lookup"><span data-stu-id="33f0d-226">Replicate between 150-200 machines.</span></span> |

  >[!TIP]
  <span data-ttu-id="33f0d-227">Om din dagliga dataomsättningen överskrider 2 TB, eller om du planerar tooreplicate mer än 200 virtuella datorer, bör toodeploy ytterligare processer servrar tooload saldo hello replikeringstrafik.</span><span class="sxs-lookup"><span data-stu-id="33f0d-227">If your daily data churn exceeds 2 TB, or you plan tooreplicate more than 200 virtual machines, it is recommended toodeploy additional process servers tooload balance hello replication traffic.</span></span> <span data-ttu-id="33f0d-228">Läs mer om hur toodeploy skalbar processen servrarna.</span><span class="sxs-lookup"><span data-stu-id="33f0d-228">Learn more about How toodeploy Scale-out Process severs.</span></span>


## <a name="common-issues"></a><span data-ttu-id="33f0d-229">Vanliga problem</span><span class="sxs-lookup"><span data-stu-id="33f0d-229">Common issues</span></span>
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]
