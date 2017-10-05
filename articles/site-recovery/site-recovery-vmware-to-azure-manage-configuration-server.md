---
title: " Hantera en konfigurationsserver i Azure Site Recovery | Microsoft Docs"
description: "Den här artikeln beskriver hur du ställa in och hantera en Server Configuration."
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
ms.openlocfilehash: 36da8c7d0f3ace194522e5288f26069cf46d470e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-a-configuration-server"></a><span data-ttu-id="504fa-103">Hantera en konfigurationsserver</span><span class="sxs-lookup"><span data-stu-id="504fa-103">Manage a Configuration Server</span></span>

<span data-ttu-id="504fa-104">Konfigurationsservern fungerar som en koordinator mellan Site Recovery services och lokal infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="504fa-104">Configuration Server acts as a coordinator between the Site Recovery services and your on-premises infrastructure.</span></span> <span data-ttu-id="504fa-105">Den här artikeln beskriver hur du kan skapa, konfigurera och hantera konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="504fa-105">This article describes how you can set up, configure, and manage the Configuration Server.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="504fa-106">Krav</span><span class="sxs-lookup"><span data-stu-id="504fa-106">Prerequisites</span></span>
<span data-ttu-id="504fa-107">Följande är minimikraven på maskinvara, programvara och nätverkskonfiguration som krävs för att ställa in en konfigurationsserver.</span><span class="sxs-lookup"><span data-stu-id="504fa-107">The following are the minimum hardware, software, and network configuration required to set up a Configuration Server.</span></span>

> [!NOTE]
> <span data-ttu-id="504fa-108">[Kapacitetsplanering](site-recovery-capacity-planner.md) är ett viktigt steg så att du distribuerar konfigurationsservern med en konfiguration som paket belastningen-krav.</span><span class="sxs-lookup"><span data-stu-id="504fa-108">[Capacity planning](site-recovery-capacity-planner.md) is an important step to ensure that you deploy the Configuration Server with a configuration that suites your load requirements.</span></span> <span data-ttu-id="504fa-109">Läs mer om [storleksanpassa kraven för en konfigurationsserver](#sizing-requirements-for-a-configuration-server).</span><span class="sxs-lookup"><span data-stu-id="504fa-109">Read more about [Sizing requirements for a Configuration Server](#sizing-requirements-for-a-configuration-server).</span></span>

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

## <a name="downloading-the-configuration-server-software"></a><span data-ttu-id="504fa-110">Hämta programvaran konfigurationsservern</span><span class="sxs-lookup"><span data-stu-id="504fa-110">Downloading the Configuration Server software</span></span>
1. <span data-ttu-id="504fa-111">Logga in på Azure-portalen och bläddra till Recovery Services-ventilen.</span><span class="sxs-lookup"><span data-stu-id="504fa-111">Log on to the Azure portal and browse to your Recovery Services Vault.</span></span>
2. <span data-ttu-id="504fa-112">Bläddra till **Site Recovery-infrastruktur** > **Konfigurationsservrar** (under för VMware och fysiska datorer).</span><span class="sxs-lookup"><span data-stu-id="504fa-112">Browse to **Site Recovery Infrastructure** > **Configuration Servers** (under For VMware & Physical Machines).</span></span>

  ![Lägg till servrar på sidan](./media/site-recovery-vmware-to-azure-manage-configuration-server/AddServers.png)
3. <span data-ttu-id="504fa-114">Klicka på den **+ servrar** knappen.</span><span class="sxs-lookup"><span data-stu-id="504fa-114">Click the **+Servers** button.</span></span>
4. <span data-ttu-id="504fa-115">På den **Lägg till Server** klickar du på knappen ladda ned för att ladda ned Registreringsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="504fa-115">On the **Add Server** page, click the Download button to download the Registration key.</span></span> <span data-ttu-id="504fa-116">Du behöver den här nyckeln under installationen av Configuration Server för att registrera den med Azure Site Recovery-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="504fa-116">You need this key during the Configuration Server installation to register it with Azure Site Recovery service.</span></span>
5. <span data-ttu-id="504fa-117">Klicka på den **ladda ned Microsoft Azure Site Recovery Unified installationsprogram** länk för att hämta den senaste versionen av konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="504fa-117">Click the **Download the Microsoft Azure Site Recovery Unified Setup** link to download the latest version of the Configuration Server.</span></span>

  ![Hämtningssidan](./media/site-recovery-vmware-to-azure-manage-configuration-server/downloadcs.png)

  > [!TIP]
  <span data-ttu-id="504fa-119">Senaste versionen av konfigurationsservern kan hämtas direkt från [hämtningssidan för Microsoft Download Center](http://aka.ms/unifiedsetup)</span><span class="sxs-lookup"><span data-stu-id="504fa-119">Latest version of the Configuration Server can be downloaded directly from [Microsoft Download Center download page](http://aka.ms/unifiedsetup)</span></span>

## <a name="installing-and-registering-a-configuration-server-from-gui"></a><span data-ttu-id="504fa-120">Installera och registrera en Server Configuration från GUI</span><span class="sxs-lookup"><span data-stu-id="504fa-120">Installing and Registering a Configuration Server from GUI</span></span>
[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

## <a name="installing-and-registering-a-configuration-server-using-command-line"></a><span data-ttu-id="504fa-121">Installera och registrera en konfigurationsservern med hjälp av kommandoraden</span><span class="sxs-lookup"><span data-stu-id="504fa-121">Installing and registering a Configuration Server using Command line</span></span>

  ```
  UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]
  ```

### <a name="sample-usage"></a><span data-ttu-id="504fa-122">Exempel på användning</span><span class="sxs-lookup"><span data-stu-id="504fa-122">Sample usage</span></span>
  ```
  MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /xC:\Temp\Extracted
  cd C:\Temp\Extracted
  UNIFIEDSETUP.EXE /AcceptThirdpartyEULA /servermode "CS" /InstallLocation "D:\" /MySQLCredsFilePath "C:\Temp\MySQLCredentialsfile.txt" /VaultCredsFilePath "C:\Temp\MyVault.vaultcredentials" /EnvType "VMWare"
  ```


### <a name="configuration-server-installer-command-line-arguments"></a><span data-ttu-id="504fa-123">Konfiguration av servern installer kommandoradsargument.</span><span class="sxs-lookup"><span data-stu-id="504fa-123">Configuration Server installer command-line arguments.</span></span>
[!INCLUDE [site-recovery-unified-setup-parameters](../../includes/site-recovery-unified-installer-command-parameters.md)]


### <a name="create-a-mysql-credentials-file"></a><span data-ttu-id="504fa-124">Skapa en MySql-fil för autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="504fa-124">Create a MySql credentials file</span></span>
<span data-ttu-id="504fa-125">Parametern MySQLCredsFilePath använder en fil som indata.</span><span class="sxs-lookup"><span data-stu-id="504fa-125">MySQLCredsFilePath parameter takes a file as input.</span></span> <span data-ttu-id="504fa-126">Skapa filen med följande format och skickar den som indataparameter till MySQLCredsFilePath.</span><span class="sxs-lookup"><span data-stu-id="504fa-126">Create the file using the following format and pass it as input MySQLCredsFilePath parameter.</span></span>
```
[MySQLCredentials]
MySQLRootPassword = "Password>"
MySQLUserPassword = "Password"
```
### <a name="create-a-proxy-settings-configuration-file"></a><span data-ttu-id="504fa-127">Skapa en konfigurationsfil för proxy-inställningar</span><span class="sxs-lookup"><span data-stu-id="504fa-127">Create a proxy settings configuration file</span></span>
<span data-ttu-id="504fa-128">Parametern ProxySettingsFilePath använder en fil som indata.</span><span class="sxs-lookup"><span data-stu-id="504fa-128">ProxySettingsFilePath parameter takes a file as input.</span></span> <span data-ttu-id="504fa-129">Skapa filen med följande format och skickar den som indataparameter till ProxySettingsFilePath.</span><span class="sxs-lookup"><span data-stu-id="504fa-129">Create the file using the following format and pass it as input ProxySettingsFilePath parameter.</span></span>

```
[ProxySettings]
ProxyAuthentication = "Yes/No"
Proxy IP = "IP Address"
ProxyPort = "Port"
ProxyUserName="UserName"
ProxyPassword="Password"
```
## <a name="modifying-proxy-settings-for-configuration-server"></a><span data-ttu-id="504fa-130">Ändra proxyinställningar för konfigurationsservern</span><span class="sxs-lookup"><span data-stu-id="504fa-130">Modifying proxy settings for Configuration Server</span></span>
1. <span data-ttu-id="504fa-131">Logga in på ditt konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="504fa-131">Login to your Configuration Server.</span></span>
2. <span data-ttu-id="504fa-132">Starta cspsconfigtool.exe med genvägen på din.</span><span class="sxs-lookup"><span data-stu-id="504fa-132">Launch the cspsconfigtool.exe using the shortcut on your.</span></span>
3. <span data-ttu-id="504fa-133">Klicka på den **valvet registrering** fliken.</span><span class="sxs-lookup"><span data-stu-id="504fa-133">Click the **Vault Registration** tab.</span></span>
4. <span data-ttu-id="504fa-134">Hämtar en ny fil i valvet registrering från portalen och ange den som indata till verktyget.</span><span class="sxs-lookup"><span data-stu-id="504fa-134">Download a new Vault Registration file from the portal and provide it as input to the tool.</span></span>

  ![registrera konfigurationsservern](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)
5. <span data-ttu-id="504fa-136">Ange ny proxyserver information och klicka på den **registrera** knappen.</span><span class="sxs-lookup"><span data-stu-id="504fa-136">Provide the new Proxy Server details and click the **Register** button.</span></span>
6. <span data-ttu-id="504fa-137">Öppna ett kommandofönster Admin PowerShell.</span><span class="sxs-lookup"><span data-stu-id="504fa-137">Open an Admin PowerShell command window.</span></span>
7. <span data-ttu-id="504fa-138">Kör följande kommando</span><span class="sxs-lookup"><span data-stu-id="504fa-138">Run the following command</span></span>
  ```
  $pwd = ConvertTo-SecureString -String MyProxyUserPassword
  Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
  net stop obengine
  net start obengine
  ```

  >[!WARNING]
  <span data-ttu-id="504fa-139">Om du har skalbar processen servrar som är kopplade till den här konfigurationen-servern, behöver du [åtgärda proxyinställningarna på alla servrar som skalbara processen](site-recovery-vmware-to-azure-manage-scaleout-process-server.md#modifying-proxy-settings-for-scale-out-process-server) i distributionen.</span><span class="sxs-lookup"><span data-stu-id="504fa-139">If you have Scale-out Process servers attached to this Configuration Server, you need to [fix the proxy settings on all the scale-out process servers](site-recovery-vmware-to-azure-manage-scaleout-process-server.md#modifying-proxy-settings-for-scale-out-process-server) in your deployment.</span></span>

## <a name="re-register-a-configuration-server-with-the-same-recovery-services-vault"></a><span data-ttu-id="504fa-140">Registrera en konfigurationsservern med samma Recovery Services-valvet</span><span class="sxs-lookup"><span data-stu-id="504fa-140">Re-register a Configuration Server with the same Recovery Services Vault</span></span>
  1. <span data-ttu-id="504fa-141">Logga in på ditt konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="504fa-141">Login to your Configuration Server.</span></span>
  2. <span data-ttu-id="504fa-142">Starta cspsconfigtool.exe använder genvägen på skrivbordet.</span><span class="sxs-lookup"><span data-stu-id="504fa-142">Launch the cspsconfigtool.exe using the shortcut on your desktop.</span></span>
  3. <span data-ttu-id="504fa-143">Klicka på den **valvet registrering** fliken.</span><span class="sxs-lookup"><span data-stu-id="504fa-143">Click the **Vault Registration** tab.</span></span>
  4. <span data-ttu-id="504fa-144">Hämtar en ny registreringsfil från portalen och ange den som indata till verktyget.</span><span class="sxs-lookup"><span data-stu-id="504fa-144">Download a new Registration file from the portal and provide it as input to the tool.</span></span>
        <span data-ttu-id="504fa-145">![registrera konfigurationsservern](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)</span><span class="sxs-lookup"><span data-stu-id="504fa-145">![register-configuration-server](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)</span></span>
  5. <span data-ttu-id="504fa-146">Ange en proxyserver information och klicka på den **registrera** knappen.</span><span class="sxs-lookup"><span data-stu-id="504fa-146">Provide the Proxy Server details and click the **Register** button.</span></span>  
  6. <span data-ttu-id="504fa-147">Öppna ett kommandofönster Admin PowerShell.</span><span class="sxs-lookup"><span data-stu-id="504fa-147">Open an Admin PowerShell command window.</span></span>
  7. <span data-ttu-id="504fa-148">Kör följande kommando</span><span class="sxs-lookup"><span data-stu-id="504fa-148">Run the following command</span></span>

      ```
      $pwd = ConvertTo-SecureString -String MyProxyUserPassword
      Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
      net stop obengine
      net start obengine
      ```

  >[!WARNING]
  <span data-ttu-id="504fa-149">Om du har skalbar processen servrar som är kopplade till den här konfigurationen-servern, behöver du [registrera alla servrar som skalbara processen](site-recovery-vmware-to-azure-manage-scaleout-process-server.md#re-registering-a-scale-out-process-server) i distributionen.</span><span class="sxs-lookup"><span data-stu-id="504fa-149">If you have Scale-out Process servers attached to this Configuration Server, you need to [re-register all the scale-out process servers](site-recovery-vmware-to-azure-manage-scaleout-process-server.md#re-registering-a-scale-out-process-server) in your deployment.</span></span>

## <a name="registering-a-configuration-server-with-a-different-recovery-services-vault"></a><span data-ttu-id="504fa-150">Registrerar en konfigurationsserver med en annan Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="504fa-150">Registering a Configuration Server with a different Recovery Services Vault.</span></span>
1. <span data-ttu-id="504fa-151">Logga in på ditt konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="504fa-151">Login to your Configuration Server.</span></span>
2. <span data-ttu-id="504fa-152">Kör kommando från en kommandotolk admin</span><span class="sxs-lookup"><span data-stu-id="504fa-152">from an admin command prompt, run the command</span></span>

```
reg delete HKLM\Software\Microsoft\Azure Site Recovery\Registration
net stop dra
```
3. <span data-ttu-id="504fa-153">Starta cspsconfigtool.exe med genvägen på din.</span><span class="sxs-lookup"><span data-stu-id="504fa-153">Launch the cspsconfigtool.exe using the shortcut on your.</span></span>
4. <span data-ttu-id="504fa-154">Klicka på den **valvet registrering** fliken.</span><span class="sxs-lookup"><span data-stu-id="504fa-154">Click the **Vault Registration** tab.</span></span>
5. <span data-ttu-id="504fa-155">Hämtar en ny registreringsfil från portalen och ange den som indata till verktyget.</span><span class="sxs-lookup"><span data-stu-id="504fa-155">Download a new Registration file from the portal and provide it as input to the tool.</span></span>

    ![registrera konfigurationsservern](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)
6. <span data-ttu-id="504fa-157">Ange en proxyserver information och klicka på den **registrera** knappen.</span><span class="sxs-lookup"><span data-stu-id="504fa-157">Provide the Proxy Server details and click the **Register** button.</span></span>  
7. <span data-ttu-id="504fa-158">Öppna ett kommandofönster Admin PowerShell.</span><span class="sxs-lookup"><span data-stu-id="504fa-158">Open an Admin PowerShell command window.</span></span>
8. <span data-ttu-id="504fa-159">Kör följande kommando</span><span class="sxs-lookup"><span data-stu-id="504fa-159">Run the following command</span></span>
    ```
    $pwd = ConvertTo-SecureString -String MyProxyUserPassword
    Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
    net stop obengine
    net start obengine
    ```

## <a name="decommissioning-a-configuration-server"></a><span data-ttu-id="504fa-160">Inaktiverar en konfigurationsserver</span><span class="sxs-lookup"><span data-stu-id="504fa-160">Decommissioning a Configuration Server</span></span>
<span data-ttu-id="504fa-161">Kontrollera följande innan du börjar inaktivering av serverns konfiguration.</span><span class="sxs-lookup"><span data-stu-id="504fa-161">Ensure the following before you start decommissioning your Configuration Server.</span></span>
1. <span data-ttu-id="504fa-162">Inaktivera skyddet för alla virtuella datorer under den här konfigurationen-servern.</span><span class="sxs-lookup"><span data-stu-id="504fa-162">Disable protection for all virtual machines under this Configuration Server.</span></span>
2. <span data-ttu-id="504fa-163">Kopplingen mellan alla replikeringsprinciper från konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="504fa-163">Disassociate all Replication policies from the Configuration Server.</span></span>
3. <span data-ttu-id="504fa-164">Ta bort alla Vcenter-servrar/vSphere-värdar som är kopplade till konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="504fa-164">Delete all vCenters servers/vSphere hosts that are associated to the Configuration Server.</span></span>

### <a name="delete-the-configuration-server-from-azure-portal"></a><span data-ttu-id="504fa-165">Ta bort konfigurationsservern från Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="504fa-165">Delete the Configuration Server from Azure portal</span></span>
1. <span data-ttu-id="504fa-166">Bläddra till i Azure-portalen **Site Recovery-infrastruktur** > **Konfigurationsservrar** på menyn valvet.</span><span class="sxs-lookup"><span data-stu-id="504fa-166">In Azure portal, browse to **Site Recovery Infrastructure** > **Configuration Servers** from the Vault menu.</span></span>
2. <span data-ttu-id="504fa-167">Klicka på konfigurationsservern som du vill inaktivera.</span><span class="sxs-lookup"><span data-stu-id="504fa-167">Click the Configuration Server that you want to decommission.</span></span>
3. <span data-ttu-id="504fa-168">På den konfigurationsservern information klickar du på knappen Ta bort.</span><span class="sxs-lookup"><span data-stu-id="504fa-168">On the Configuration Server's details page, click the Delete button.</span></span>

  ![ta bort konfigurationsservern](./media/site-recovery-vmware-to-azure-manage-configuration-server/delete-configuration-server.PNG)
4. <span data-ttu-id="504fa-170">Klicka på **Ja** att bekräfta borttagningen av servern.</span><span class="sxs-lookup"><span data-stu-id="504fa-170">Click **Yes** to confirm the deletion of the server.</span></span>

  >[!WARNING]
  <span data-ttu-id="504fa-171">Om du har virtuella datorer, replikeringsprinciper eller vCenter-servrar/vSphere-värdar som är associerade med den här konfigurationsservern kan du ta bort servern.</span><span class="sxs-lookup"><span data-stu-id="504fa-171">If you have any virtual machines, Replication policies or vCenter servers/vSphere hosts associated with this Configuration Server, you cannot delete the server.</span></span> <span data-ttu-id="504fa-172">Ta bort dessa enheter innan du försöker ta bort valvet.</span><span class="sxs-lookup"><span data-stu-id="504fa-172">Delete these entities before you try to delete the vault.</span></span>

### <a name="uninstall-the-configuration-server-software-and-its-dependencies"></a><span data-ttu-id="504fa-173">Avinstallera Configuration Server-programvaran och dess beroenden</span><span class="sxs-lookup"><span data-stu-id="504fa-173">Uninstall the Configuration Server software and its dependencies</span></span>
  > [!TIP]
  <span data-ttu-id="504fa-174">Om du tänker återanvända konfigurationsservern med Azure Site Recovery igen kan sedan du hoppa till steg 4 direkt</span><span class="sxs-lookup"><span data-stu-id="504fa-174">If you plan to reuse the Configuration Server with Azure Site Recovery again, then you can skip to step 4 directly</span></span>

1. <span data-ttu-id="504fa-175">Logga in på av konfigurationsservern som administratör.</span><span class="sxs-lookup"><span data-stu-id="504fa-175">Log on to the Configuration Server as an Administrator.</span></span>
2. <span data-ttu-id="504fa-176">Öppna Kontrollpanelen > Program > avinstallera program</span><span class="sxs-lookup"><span data-stu-id="504fa-176">Open up Control Panel > Program > Uninstall Programs</span></span>
3. <span data-ttu-id="504fa-177">Avinstallera program i följande ordning:</span><span class="sxs-lookup"><span data-stu-id="504fa-177">Uninstall the programs in the following sequence:</span></span>
  * <span data-ttu-id="504fa-178">Microsoft Azure Recovery Services-agent</span><span class="sxs-lookup"><span data-stu-id="504fa-178">Microsoft Azure Recovery Services Agent</span></span>
  * <span data-ttu-id="504fa-179">Microsoft Azure Site Recovery Mobility tjänsten eller Huvudtjänsten målservern</span><span class="sxs-lookup"><span data-stu-id="504fa-179">Microsoft Azure Site Recovery Mobility Service/Master Target server</span></span>
  * <span data-ttu-id="504fa-180">Microsoft Azure Site Recovery-providern</span><span class="sxs-lookup"><span data-stu-id="504fa-180">Microsoft Azure Site Recovery Provider</span></span>
  * <span data-ttu-id="504fa-181">Server-processen för Microsoft Azure Site Recovery konfigurationsservern</span><span class="sxs-lookup"><span data-stu-id="504fa-181">Microsoft Azure Site Recovery Configuration Server/Process Server</span></span>
  * <span data-ttu-id="504fa-182">Microsoft Azure Site Recovery Configuration Server beroenden</span><span class="sxs-lookup"><span data-stu-id="504fa-182">Microsoft Azure Site Recovery Configuration Server Dependencies</span></span>
  * <span data-ttu-id="504fa-183">MySQL-Server 5.5</span><span class="sxs-lookup"><span data-stu-id="504fa-183">MySQL Server 5.5</span></span>
4. <span data-ttu-id="504fa-184">Kör följande kommando från och admin-kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="504fa-184">Run the following command from and admin command prompt.</span></span>
  ```
  reg delete HKLM\Software\Microsoft\Azure Site Recovery\Registration
  ```

## <a name="renew-configuration-server-secure-socket-layerssl-certificates"></a><span data-ttu-id="504fa-185">Förnya Configuration servercertifikat Secure Socket Layer(SSL)</span><span class="sxs-lookup"><span data-stu-id="504fa-185">Renew Configuration Server Secure Socket Layer(SSL) Certificates</span></span>
<span data-ttu-id="504fa-186">Konfigurationsservern har ett inbyggda webbserver som samordnar aktiviteter Mobilitetstjänsten, servrar och Huvudmålservern servrar som är anslutna till konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="504fa-186">The Configuration Server has an inbuilt webserver, which orchestrates the activities of the Mobility Service, Process Servers, and Master Target servers connected to the Configuration Server.</span></span> <span data-ttu-id="504fa-187">Den konfigurationsservern webbserver använder ett SSL-certifikat för att autentisera klienter.</span><span class="sxs-lookup"><span data-stu-id="504fa-187">The Configuration Server's webserver uses an SSL certificate to authenticate its clients.</span></span> <span data-ttu-id="504fa-188">Det här certifikatet har en utgången av tre år och kan förnyas när som helst på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="504fa-188">This certificate has an expiry of three years and can be renewed at any time using the following method:</span></span>

> [!WARNING]
<span data-ttu-id="504fa-189">Certifikatet upphör att gälla kan bara utföras på version 9.4.XXXX. X eller senare.</span><span class="sxs-lookup"><span data-stu-id="504fa-189">Certificate expiry can be performed only on version 9.4.XXXX.X or higher.</span></span> <span data-ttu-id="504fa-190">Uppgradera alla Azure Site Recovery-komponenter (konfigurationsservern, Processervern, Huvudmålserver, Mobilitetstjänsten) innan du startar arbetsflödet förnya certifikat.</span><span class="sxs-lookup"><span data-stu-id="504fa-190">Upgrade all the Azure Site Recovery components (Configuration Server, Process Server, Master Target Server, Mobility Service) before you start the Renew Certificates workflow.</span></span>

1. <span data-ttu-id="504fa-191">På Azure-portalen går du till ditt valv > Site Recovery-infrastruktur > konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="504fa-191">On the Azure portal, browse to your Vault > Site Recovery Infrastructure > Configuration Server.</span></span>
2. <span data-ttu-id="504fa-192">Klicka på konfigurationsservern för vilka du behöver att förnya certifikatet för SSL.</span><span class="sxs-lookup"><span data-stu-id="504fa-192">Click the Configuration Server for which you need to renew the SSL Certificate for.</span></span>
3. <span data-ttu-id="504fa-193">Du kan se utgångsdatum för SSL-certifikatet under Configuration Server-hälsa.</span><span class="sxs-lookup"><span data-stu-id="504fa-193">Under the Configuration Server health, you can see the expiry date for the SSL Certificate.</span></span>
4. <span data-ttu-id="504fa-194">Förnya certifikatet genom att klicka på den **förnya certifikat** åtgärd som visas i följande bild:</span><span class="sxs-lookup"><span data-stu-id="504fa-194">Renew the certificate by clicking the **Renew Certificates** action as shown in the following image:</span></span>

  ![ta bort konfigurationsservern](./media/site-recovery-vmware-to-azure-manage-configuration-server/renew-cert-page.png)

### <a name="secure-socket-layer-certificate-expiry-warning"></a><span data-ttu-id="504fa-196">Secure Socket Layer certifikatupphörandevarning</span><span class="sxs-lookup"><span data-stu-id="504fa-196">Secure Socket Layer certificate expiry warning</span></span>

> [!NOTE]
<span data-ttu-id="504fa-197">SSL-certifikatets giltighetsperiod för alla installationer som inträffade före maj 2016 har angetts till ett år.</span><span class="sxs-lookup"><span data-stu-id="504fa-197">The SSL Certificate's validity for all installations that happened before May 2016 was set to one year.</span></span> <span data-ttu-id="504fa-198">du har startat ser certifikatet upphör att gälla meddelanden visas i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="504fa-198">you have started seeing certificate expiry notifications showing up in the Azure portal.</span></span>

1. <span data-ttu-id="504fa-199">Om Konfigurationsserverns SSL-certifikatet upphör att gälla under de kommande två månaderna, startas tjänsten meddela användare via Azure-portalen & e-post (du måste prenumerera på Azure Site Recovery-meddelanden).</span><span class="sxs-lookup"><span data-stu-id="504fa-199">If the Configuration Server's SSL certificate is going to expire in the next two months, the service starts notifying users via the Azure portal & email (you need to be subscribed to Azure Site Recovery notifications).</span></span> <span data-ttu-id="504fa-200">Du ser en meddelandebanderoll på resurssidan på valvet.</span><span class="sxs-lookup"><span data-stu-id="504fa-200">You start seeing a notification banner on the Vault's resource page.</span></span>

  ![certifikat-meddelande](./media/site-recovery-vmware-to-azure-manage-configuration-server/ssl-cert-renew-notification.png)
2. <span data-ttu-id="504fa-202">Klicka på listen för att få mer information om certifikatet upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="504fa-202">Click the banner to get additional details on the Certificate expiry.</span></span>

  ![information för certifikat](./media/site-recovery-vmware-to-azure-manage-configuration-server/ssl-cert-expiry-details.png)

  >[!TIP]
  <span data-ttu-id="504fa-204">Om i stället för en **förnya nu** knapp som du ser en **uppgradera nu** knappen.</span><span class="sxs-lookup"><span data-stu-id="504fa-204">If instead of a **Renew Now** button you see an **Upgrade Now** button.</span></span> <span data-ttu-id="504fa-205">Det innebär att det finns vissa komponenter i din miljö som ännu inte har uppgraderats till 9.4.xxxx.x eller senare versioner.</span><span class="sxs-lookup"><span data-stu-id="504fa-205">This means that there are some components in your environment that have not yet been upgraded to 9.4.xxxx.x or higher versions.</span></span>

## <a name="sizing-requirements-for-a-configuration-server"></a><span data-ttu-id="504fa-206">Ändra storlek på kraven för en konfigurationsserver</span><span class="sxs-lookup"><span data-stu-id="504fa-206">Sizing requirements for a Configuration Server</span></span>

| <span data-ttu-id="504fa-207">**CPU**</span><span class="sxs-lookup"><span data-stu-id="504fa-207">**CPU**</span></span> | <span data-ttu-id="504fa-208">**Minne**</span><span class="sxs-lookup"><span data-stu-id="504fa-208">**Memory**</span></span> | <span data-ttu-id="504fa-209">**Cachestorleken för disk**</span><span class="sxs-lookup"><span data-stu-id="504fa-209">**Cache disk size**</span></span> | <span data-ttu-id="504fa-210">**Dataändringshastighet**</span><span class="sxs-lookup"><span data-stu-id="504fa-210">**Data change rate**</span></span> | <span data-ttu-id="504fa-211">**Skyddade datorer**</span><span class="sxs-lookup"><span data-stu-id="504fa-211">**Protected machines**</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="504fa-212">8 vCPUs (2 sockets * 4 kärnor @ 2,5 GHz)</span><span class="sxs-lookup"><span data-stu-id="504fa-212">8 vCPUs (2 sockets * 4 cores @ 2.5 GHz)</span></span> |<span data-ttu-id="504fa-213">16 GB</span><span class="sxs-lookup"><span data-stu-id="504fa-213">16 GB</span></span> |<span data-ttu-id="504fa-214">300 GB</span><span class="sxs-lookup"><span data-stu-id="504fa-214">300 GB</span></span> |<span data-ttu-id="504fa-215">500 GB eller mindre</span><span class="sxs-lookup"><span data-stu-id="504fa-215">500 GB or less</span></span> |<span data-ttu-id="504fa-216">Replikera färre än 100 datorer.</span><span class="sxs-lookup"><span data-stu-id="504fa-216">Replicate fewer than 100 machines.</span></span> |
| <span data-ttu-id="504fa-217">12 vCPUs (2 sockets * @ 2,5 GHz-6 kärnor)</span><span class="sxs-lookup"><span data-stu-id="504fa-217">12 vCPUs (2 sockets * 6 cores @ 2.5 GHz)</span></span> |<span data-ttu-id="504fa-218">18 GB</span><span class="sxs-lookup"><span data-stu-id="504fa-218">18 GB</span></span> |<span data-ttu-id="504fa-219">600 GB</span><span class="sxs-lookup"><span data-stu-id="504fa-219">600 GB</span></span> |<span data-ttu-id="504fa-220">500 GB till 1 TB</span><span class="sxs-lookup"><span data-stu-id="504fa-220">500 GB to 1 TB</span></span> |<span data-ttu-id="504fa-221">Replikera mellan 100 150 datorer.</span><span class="sxs-lookup"><span data-stu-id="504fa-221">Replicate between 100-150 machines.</span></span> |
| <span data-ttu-id="504fa-222">16 vCPUs (2 sockets * 8 kärnor @ 2,5 GHz)</span><span class="sxs-lookup"><span data-stu-id="504fa-222">16 vCPUs (2 sockets * 8 cores @ 2.5 GHz)</span></span> |<span data-ttu-id="504fa-223">32 GB</span><span class="sxs-lookup"><span data-stu-id="504fa-223">32 GB</span></span> |<span data-ttu-id="504fa-224">1 TB</span><span class="sxs-lookup"><span data-stu-id="504fa-224">1 TB</span></span> |<span data-ttu-id="504fa-225">1 TB till 2 TB</span><span class="sxs-lookup"><span data-stu-id="504fa-225">1 TB to 2 TB</span></span> |<span data-ttu-id="504fa-226">Replikera mellan 150 200 datorer.</span><span class="sxs-lookup"><span data-stu-id="504fa-226">Replicate between 150-200 machines.</span></span> |

  >[!TIP]
  <span data-ttu-id="504fa-227">Om din dagliga dataomsättningen överskrider 2 TB, eller om du planerar att replikera mer än 200 virtuella datorer, rekommenderas att distribuera ytterligare servrar för att belastningsutjämna replikeringstrafiken.</span><span class="sxs-lookup"><span data-stu-id="504fa-227">If your daily data churn exceeds 2 TB, or you plan to replicate more than 200 virtual machines, it is recommended to deploy additional process servers to load balance the replication traffic.</span></span> <span data-ttu-id="504fa-228">Lär dig mer om hur du distribuerar skalbar Process-servrarna.</span><span class="sxs-lookup"><span data-stu-id="504fa-228">Learn more about How to deploy Scale-out Process severs.</span></span>


## <a name="common-issues"></a><span data-ttu-id="504fa-229">Vanliga problem</span><span class="sxs-lookup"><span data-stu-id="504fa-229">Common issues</span></span>
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]
