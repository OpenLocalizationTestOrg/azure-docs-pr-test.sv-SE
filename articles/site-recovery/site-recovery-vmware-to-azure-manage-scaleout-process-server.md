---
title: " Hantera en skalbar Processerver i Azure Site Recovery | Microsoft Docs"
description: "Den här artikeln beskriver hur du konfigurerar och hanterar en skalbar Processerver i Azure Site Recovery."
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
ms.openlocfilehash: e5c01de19917235c34c035415df86291b9152bf0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-a-scale-out-process-server"></a><span data-ttu-id="9222f-103">Hantera en skalbar Processerver</span><span class="sxs-lookup"><span data-stu-id="9222f-103">Manage a Scale-out Process Server</span></span>

<span data-ttu-id="9222f-104">Skalbar Processerver fungerar som en koordinator för att överföra data mellan Site Recovery services och lokal infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="9222f-104">Scale-out Process Server acts as a coordinator for data transfer between the Site Recovery services and your on-premises infrastructure.</span></span> <span data-ttu-id="9222f-105">Den här artikeln beskriver hur du kan skapa, konfigurera och hantera en skalbar Processerver.</span><span class="sxs-lookup"><span data-stu-id="9222f-105">This article describes how you can set up, configure, and manage a Scale-out Process Server.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9222f-106">Krav</span><span class="sxs-lookup"><span data-stu-id="9222f-106">Prerequisites</span></span>
<span data-ttu-id="9222f-107">Följande är den rekommenderade maskinvara, programvara och nätverkskonfiguration som krävs för att konfigurera en skalbar Processerver.</span><span class="sxs-lookup"><span data-stu-id="9222f-107">The following are the recommended hardware, software, and network configuration required to set up a Scale-out Process Server.</span></span>

> [!NOTE]
> <span data-ttu-id="9222f-108">[Kapacitetsplanering](site-recovery-capacity-planner.md) är ett viktigt steg så att du distribuerar skalbar Processerver med en konfiguration som paket belastningen-krav.</span><span class="sxs-lookup"><span data-stu-id="9222f-108">[Capacity planning](site-recovery-capacity-planner.md) is an important step to ensure that you deploy the Scale-out Process Server with a configuration that suites your load requirements.</span></span> <span data-ttu-id="9222f-109">Läs mer om [skalning egenskaper för en skalbar Processerver](#sizing-requirements-for-a-configuration-server).</span><span class="sxs-lookup"><span data-stu-id="9222f-109">Read more about [Scaling characteristics for a Scale-out Process Server](#sizing-requirements-for-a-configuration-server).</span></span>

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

## <a name="downloading-the-scale-out-process-server-software"></a><span data-ttu-id="9222f-110">Hämta programvaran skalbar Processerver</span><span class="sxs-lookup"><span data-stu-id="9222f-110">Downloading the Scale-out Process Server software</span></span>
1. <span data-ttu-id="9222f-111">Logga in på Azure-portalen och bläddra till Recovery Services-ventilen.</span><span class="sxs-lookup"><span data-stu-id="9222f-111">Log on to the Azure portal and browse to your Recovery Services Vault.</span></span>
2. <span data-ttu-id="9222f-112">Bläddra till **Site Recovery-infrastruktur** > **Konfigurationsservrar** (under för VMware och fysiska datorer).</span><span class="sxs-lookup"><span data-stu-id="9222f-112">Browse to **Site Recovery Infrastructure** > **Configuration Servers** (under For VMware & Physical Machines).</span></span>
3. <span data-ttu-id="9222f-113">Välj din konfigurationsservern och öka detaljnivån till server configuration informationssidan.</span><span class="sxs-lookup"><span data-stu-id="9222f-113">Select your configuration server to drill down into the configuration server's details page.</span></span>
4. <span data-ttu-id="9222f-114">Klicka på den **+ Processervern** knappen.</span><span class="sxs-lookup"><span data-stu-id="9222f-114">Click the **+ Process Server** button.</span></span>
5. <span data-ttu-id="9222f-115">I den **lägga till processervern** väljer **distribuera en skalbar Processerver lokala** alternativet från den **Välj där du vill distribuera din processerver för** listrutan.</span><span class="sxs-lookup"><span data-stu-id="9222f-115">In the **Add Process server** page, select **Deploy a Scale-out Process Server on-premises** option from the **Choose where you want to deploy your process server** drop-down.</span></span>

  ![Lägg till servrar på sidan](./media/site-recovery-vmware-to-azure-manage-scaleout-process-server/add-process-server.png)
6. <span data-ttu-id="9222f-117">Klicka på den **ladda ned Microsoft Azure Site Recovery Unified installationsprogram** länk för att hämta den senaste versionen av skalbara processen Server-installationen.</span><span class="sxs-lookup"><span data-stu-id="9222f-117">Click the **Download the Microsoft Azure Site Recovery Unified Setup** link to download the latest version of the Scale-out Process Server installation.</span></span>

  > [!WARNING]
  <span data-ttu-id="9222f-118">Versionen för din skalbar Processerver måste vara lika med eller lägre version än konfigurationsservern körs i din miljö.</span><span class="sxs-lookup"><span data-stu-id="9222f-118">The version of your Scale-out Process Server should be equal to or lesser than the Configuration Server version running in your environment.</span></span> <span data-ttu-id="9222f-119">Ett enkelt sätt att kontrollera versionskompatibiliteten är att använda samma installer bitarna som du nyligen använt för att installera/uppdatera din konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="9222f-119">A simple way to ensure version compatibility is to use the same installer bits that you recently used to install/update your Configuration Server.</span></span>

## <a name="installing-and-registering-a-scale-out-process-server-from-gui"></a><span data-ttu-id="9222f-120">Installera och registrera en skalbar Processerver från GUI</span><span class="sxs-lookup"><span data-stu-id="9222f-120">Installing and registering a Scale-out Process Server from GUI</span></span>
<span data-ttu-id="9222f-121">Om du har skala upp distributionen utöver 200 källdatorer eller en daglig totala omsättningen på mer än 2 TB, om du behöver ytterligare servrar att hantera trafikvolymen.</span><span class="sxs-lookup"><span data-stu-id="9222f-121">If you have to scale out your deployment beyond 200 source machines, or a total daily churn rate of more than 2 TB, you need additional process servers to handle the traffic volume.</span></span>

<span data-ttu-id="9222f-122">Kontrollera den [storlek rekommendationer för servrar](#size-recommendations-for-the-process-server), och följ sedan anvisningarna för att ställa in processervern.</span><span class="sxs-lookup"><span data-stu-id="9222f-122">Check the [size recommendations for process servers](#size-recommendations-for-the-process-server), and then follow these instructions to set up the process server.</span></span> <span data-ttu-id="9222f-123">När du har installerat på servern, kan du migrera källdatorer för att använda den.</span><span class="sxs-lookup"><span data-stu-id="9222f-123">After setting up the server, you migrate source machines to use it.</span></span>

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-add-process-server.md)]


## <a name="installing-and-registering-a-scale-out-process-server-using-command-line"></a><span data-ttu-id="9222f-124">Installera och registrera en skalbar Processerver med hjälp av kommandoraden</span><span class="sxs-lookup"><span data-stu-id="9222f-124">Installing and registering a Scale-out Process Server using command-line</span></span>

```
UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]
```

### <a name="sample-usage"></a><span data-ttu-id="9222f-125">Exempel på användning</span><span class="sxs-lookup"><span data-stu-id="9222f-125">Sample usage</span></span>
```
MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /xC:\Temp\Extracted
cd C:\Temp\Extracted
UNIFIEDSETUP.EXE /AcceptThirdpartyEULA /servermode "PS" /InstallLocation "D:\" /EnvType "VMWare" /CSIP "10.150.24.119" /PassphraseFilePath "C:\Users\Administrator\Desktop\Passphrase.txt" /DataTransferSecurePort 443
```

### <a name="scale-out-process-server-installer-command-line-arguments"></a><span data-ttu-id="9222f-126">Skalbar Processerver installer kommandoradsargument.</span><span class="sxs-lookup"><span data-stu-id="9222f-126">Scale-out Process Server installer command-line arguments.</span></span>
[!INCLUDE [site-recovery-unified-setup-parameters](../../includes/site-recovery-unified-installer-command-parameters.md)]


### <a name="create-a-proxy-settings-configuration-file"></a><span data-ttu-id="9222f-127">Skapa en konfigurationsfil för proxy-inställningar</span><span class="sxs-lookup"><span data-stu-id="9222f-127">Create a proxy settings configuration file</span></span>
<span data-ttu-id="9222f-128">Parametern ProxySettingsFilePath använder en fil som indata.</span><span class="sxs-lookup"><span data-stu-id="9222f-128">ProxySettingsFilePath parameter takes a file as input.</span></span> <span data-ttu-id="9222f-129">Skapa fil med följande format och skickar den som indataparameter till ProxySettingsFilePath.</span><span class="sxs-lookup"><span data-stu-id="9222f-129">Create file using the following format and pass it as input ProxySettingsFilePath parameter.</span></span>
```
* [ProxySettings]
* ProxyAuthentication = "Yes/No"
* Proxy IP = "IP Address"
* ProxyPort = "Port"
* ProxyUserName="UserName"
* ProxyPassword="Password"
```
## <a name="modifying-proxy-settings-for-scale-out-process-server"></a><span data-ttu-id="9222f-130">Ändra proxyinställningar för skalbar Processerver</span><span class="sxs-lookup"><span data-stu-id="9222f-130">Modifying proxy settings for Scale-out Process Server</span></span>
1. <span data-ttu-id="9222f-131">Logga in på servern för skalbar processen.</span><span class="sxs-lookup"><span data-stu-id="9222f-131">Login  into your Scale-out Process Server.</span></span>
2. <span data-ttu-id="9222f-132">Öppna ett kommandofönster Admin PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9222f-132">Open an Admin PowerShell command window.</span></span>
3. <span data-ttu-id="9222f-133">Kör följande kommando</span><span class="sxs-lookup"><span data-stu-id="9222f-133">Run the following command</span></span>
  ```
  $pwd = ConvertTo-SecureString -String MyProxyUserPassword
  Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber –ProxyUserName domain\username -ProxyPassword $pwd
  net stop obengine
  net start obengine
  ```
4. <span data-ttu-id="9222f-134">Bläddra sedan till katalogen **%PROGRAMDATA%\ASR\Agent** och kör följande kommando</span><span class="sxs-lookup"><span data-stu-id="9222f-134">Next browse to the directory **%PROGRAMDATA%\ASR\Agent** and run the following command</span></span>
  ```
  cmd
  cdpcli.exe --registermt

  net stop obengine

  net start obengine

  exit
  ```

## <a name="re-registering-a-scale-out-process-server"></a><span data-ttu-id="9222f-135">Registrera en skalbar Processerver</span><span class="sxs-lookup"><span data-stu-id="9222f-135">Re-registering a Scale-out Process Server</span></span>
[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

* <span data-ttu-id="9222f-136">Nästa gång du öppna en kommandotolk för administratören.</span><span class="sxs-lookup"><span data-stu-id="9222f-136">Next open an Admin command prompt.</span></span>
* <span data-ttu-id="9222f-137">Bläddra till mappen **%PROGRAMDATA%\ASR\Agent** och kör kommandot</span><span class="sxs-lookup"><span data-stu-id="9222f-137">Browse to the directory **%PROGRAMDATA%\ASR\Agent** and run the command</span></span>

```
cdpcli.exe --registermt

net stop obengine

net start obengine
```

## <a name="upgrading-a-scale-out-process-server"></a><span data-ttu-id="9222f-138">Uppgradera en skalbar Processerver</span><span class="sxs-lookup"><span data-stu-id="9222f-138">Upgrading a Scale-out Process Server</span></span>
[!INCLUDE [site-recovery-vmware-upgrade -process-server](../../includes/site-recovery-vmware-upgrade-process-server-internal.md)]

## <a name="decommissioning-a-scale-out-process-server"></a><span data-ttu-id="9222f-139">Inaktivering av en skalbar Processerver</span><span class="sxs-lookup"><span data-stu-id="9222f-139">Decommissioning a Scale-out Process Server</span></span>
1. <span data-ttu-id="9222f-140">Se till att:</span><span class="sxs-lookup"><span data-stu-id="9222f-140">Ensure that:</span></span>
  - <span data-ttu-id="9222f-141">Status för Configuration Server-anslutningen visas som **ansluten** i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="9222f-141">Configuration Server's connection state shows as **Connected** in the Azure portal</span></span>
  - <span data-ttu-id="9222f-142">Processen för servern är fortfarande kunna kommunicera med konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="9222f-142">Process Server's is still able to communicate with the Configuration server.</span></span>
2. <span data-ttu-id="9222f-143">Logga in till processervern som administratör</span><span class="sxs-lookup"><span data-stu-id="9222f-143">Log in to the process server as an administrator</span></span>
3. <span data-ttu-id="9222f-144">Öppna Kontrollpanelen > Program > avinstallera program</span><span class="sxs-lookup"><span data-stu-id="9222f-144">Open up Control Panel > Program > Uninstall Programs</span></span>
4. <span data-ttu-id="9222f-145">Avinstallera program i den ordning som anges i följande:</span><span class="sxs-lookup"><span data-stu-id="9222f-145">Uninstall the programs in the sequence given following:</span></span>
  * <span data-ttu-id="9222f-146">Server-processen för Microsoft Azure Site Recovery konfigurationsservern</span><span class="sxs-lookup"><span data-stu-id="9222f-146">Microsoft Azure Site Recovery Configuration Server/Process Server</span></span>
  * <span data-ttu-id="9222f-147">Microsoft Azure Site Recovery Configuration Server beroenden</span><span class="sxs-lookup"><span data-stu-id="9222f-147">Microsoft Azure Site Recovery Configuration Server Dependencies</span></span>
  * <span data-ttu-id="9222f-148">Microsoft Azure Recovery Services-agent</span><span class="sxs-lookup"><span data-stu-id="9222f-148">Microsoft Azure Recovery Services Agent</span></span>

<span data-ttu-id="9222f-149">Det kan ta upp till 15 minuter för att Processervern återspeglar i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9222f-149">It can take up-to 15 minutes for the Process Server deletion to reflect in the Azure portal.</span></span>

  > [!NOTE]
  <span data-ttu-id="9222f-150">Om processervern kan inte kommunicera med konfigurationsservern (anslutningens status i portalen är frånkopplad), måste du följa anvisningarna nedan om du vill rensa från konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="9222f-150">If the Process server is unable to communicate with the Configuration Server (Connection State in portal is Disconnected), then you need to follow the following steps to purge it from the Configuration Server.</span></span>

## <a name="unregistering-a-disconnected-scale-out-process-server-from-a-configuration-server"></a><span data-ttu-id="9222f-151">Avregistrerar en frånkopplad skalbar processerver från en Server Configuration</span><span class="sxs-lookup"><span data-stu-id="9222f-151">Unregistering a disconnected Scale-out Process server from a Configuration Server</span></span>

[!INCLUDE [site-recovery-vmware-upgrade-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]

## <a name="sizing-requirements-for-a-scale-out-process-server"></a><span data-ttu-id="9222f-152">Ändra storlek på kraven för en skalbar Processerver</span><span class="sxs-lookup"><span data-stu-id="9222f-152">Sizing requirements for a Scale-out Process Server</span></span>

| <span data-ttu-id="9222f-153">**Ytterligare processervern**</span><span class="sxs-lookup"><span data-stu-id="9222f-153">**Additional process server**</span></span> | <span data-ttu-id="9222f-154">**Cachestorleken för disk**</span><span class="sxs-lookup"><span data-stu-id="9222f-154">**Cache disk size**</span></span> | <span data-ttu-id="9222f-155">**Dataändringshastighet**</span><span class="sxs-lookup"><span data-stu-id="9222f-155">**Data change rate**</span></span> | <span data-ttu-id="9222f-156">**Skyddade datorer**</span><span class="sxs-lookup"><span data-stu-id="9222f-156">**Protected machines**</span></span> |
| --- | --- | --- | --- |
|<span data-ttu-id="9222f-157">4 vCPUs (2 sockets * 2 kärnor @ 2,5 GHz), 8 GB minne</span><span class="sxs-lookup"><span data-stu-id="9222f-157">4 vCPUs (2 sockets * 2 cores @ 2.5 GHz), 8-GB memory</span></span> |<span data-ttu-id="9222f-158">300 GB</span><span class="sxs-lookup"><span data-stu-id="9222f-158">300 GB</span></span> |<span data-ttu-id="9222f-159">250 GB eller mindre</span><span class="sxs-lookup"><span data-stu-id="9222f-159">250 GB or less</span></span> |<span data-ttu-id="9222f-160">Replikera 85 eller mindre datorer.</span><span class="sxs-lookup"><span data-stu-id="9222f-160">Replicate 85 or less machines.</span></span> |
|<span data-ttu-id="9222f-161">8 vCPUs (2 sockets * 4 kärnor @ 2,5 GHz), 12 GB minne</span><span class="sxs-lookup"><span data-stu-id="9222f-161">8 vCPUs (2 sockets * 4 cores @ 2.5 GHz), 12-GB memory</span></span> |<span data-ttu-id="9222f-162">600 GB</span><span class="sxs-lookup"><span data-stu-id="9222f-162">600 GB</span></span> |<span data-ttu-id="9222f-163">250 GB till 1 TB</span><span class="sxs-lookup"><span data-stu-id="9222f-163">250 GB to 1 TB</span></span> |<span data-ttu-id="9222f-164">Replikera mellan 85 150 datorer.</span><span class="sxs-lookup"><span data-stu-id="9222f-164">Replicate between 85-150 machines.</span></span> |
|<span data-ttu-id="9222f-165">12 vCPUs (2 sockets * 6 kärnor @ 2,5 GHz) 24 GB minne</span><span class="sxs-lookup"><span data-stu-id="9222f-165">12 vCPUs (2 sockets * 6 cores @ 2.5 GHz) 24-GB memory</span></span> |<span data-ttu-id="9222f-166">1 TB</span><span class="sxs-lookup"><span data-stu-id="9222f-166">1 TB</span></span> |<span data-ttu-id="9222f-167">1 TB till 2 TB</span><span class="sxs-lookup"><span data-stu-id="9222f-167">1 TB to 2 TB</span></span> |<span data-ttu-id="9222f-168">Replikera mellan 150 225 datorer.</span><span class="sxs-lookup"><span data-stu-id="9222f-168">Replicate between 150-225 machines.</span></span> |
