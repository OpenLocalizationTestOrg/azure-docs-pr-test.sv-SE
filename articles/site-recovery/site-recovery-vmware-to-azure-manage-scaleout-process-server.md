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
# <a name="manage-a-scale-out-process-server"></a><span data-ttu-id="a4e1c-103">Hantera en skalbar Processerver</span><span class="sxs-lookup"><span data-stu-id="a4e1c-103">Manage a Scale-out Process Server</span></span>

<span data-ttu-id="a4e1c-104">Skalbar Processerver fungerar som en koordinator för att överföra data mellan hello Site Recovery services och lokal infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="a4e1c-104">Scale-out Process Server acts as a coordinator for data transfer between hello Site Recovery services and your on-premises infrastructure.</span></span> <span data-ttu-id="a4e1c-105">Den här artikeln beskriver hur du kan skapa, konfigurera och hantera en skalbar Processerver.</span><span class="sxs-lookup"><span data-stu-id="a4e1c-105">This article describes how you can set up, configure, and manage a Scale-out Process Server.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a4e1c-106">Krav</span><span class="sxs-lookup"><span data-stu-id="a4e1c-106">Prerequisites</span></span>
<span data-ttu-id="a4e1c-107">hello följande är hello rekommenderad maskinvara, programvara och nätverk konfiguration krävs tooset upp en skalbar Processerver.</span><span class="sxs-lookup"><span data-stu-id="a4e1c-107">hello following are hello recommended hardware, software, and network configuration required tooset up a Scale-out Process Server.</span></span>

> [!NOTE]
> <span data-ttu-id="a4e1c-108">[Kapacitetsplanering](site-recovery-capacity-planner.md) är ett viktigt steg tooensure som du distribuerar hello skalbar Processerver med en konfiguration som paket belastningen-krav.</span><span class="sxs-lookup"><span data-stu-id="a4e1c-108">[Capacity planning](site-recovery-capacity-planner.md) is an important step tooensure that you deploy hello Scale-out Process Server with a configuration that suites your load requirements.</span></span> <span data-ttu-id="a4e1c-109">Läs mer om [skalning egenskaper för en skalbar Processerver](#sizing-requirements-for-a-configuration-server).</span><span class="sxs-lookup"><span data-stu-id="a4e1c-109">Read more about [Scaling characteristics for a Scale-out Process Server](#sizing-requirements-for-a-configuration-server).</span></span>

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

## <a name="downloading-hello-scale-out-process-server-software"></a><span data-ttu-id="a4e1c-110">Hämtar hello skalbar Processerver programvara</span><span class="sxs-lookup"><span data-stu-id="a4e1c-110">Downloading hello Scale-out Process Server software</span></span>
1. <span data-ttu-id="a4e1c-111">Logga in toohello Azure-portalen och bläddra tooyour Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="a4e1c-111">Log on toohello Azure portal and browse tooyour Recovery Services Vault.</span></span>
2. <span data-ttu-id="a4e1c-112">Bläddra för**Site Recovery-infrastruktur** > **Konfigurationsservrar** (under för VMware och fysiska datorer).</span><span class="sxs-lookup"><span data-stu-id="a4e1c-112">Browse too**Site Recovery Infrastructure** > **Configuration Servers** (under For VMware & Physical Machines).</span></span>
3. <span data-ttu-id="a4e1c-113">Välj din konfiguration server toodrill i hello configuration server informationssidan.</span><span class="sxs-lookup"><span data-stu-id="a4e1c-113">Select your configuration server toodrill down into hello configuration server's details page.</span></span>
4. <span data-ttu-id="a4e1c-114">Klicka på hello **+ Processervern** knappen.</span><span class="sxs-lookup"><span data-stu-id="a4e1c-114">Click hello **+ Process Server** button.</span></span>
5. <span data-ttu-id="a4e1c-115">I hello **lägga till processervern** väljer **distribuera en skalbar Processerver lokala** alternativet från hello **Välj önskad toodeploy processervern** listrutan.</span><span class="sxs-lookup"><span data-stu-id="a4e1c-115">In hello **Add Process server** page, select **Deploy a Scale-out Process Server on-premises** option from hello **Choose where you want toodeploy your process server** drop-down.</span></span>

  ![Lägg till servrar på sidan](./media/site-recovery-vmware-to-azure-manage-scaleout-process-server/add-process-server.png)
6. <span data-ttu-id="a4e1c-117">Klicka på hello **Download hello installationsprogram för Microsoft Azure Site Recovery enhetlig** länk toodownload hello senaste versionen av hello skalbar Processerver installation.</span><span class="sxs-lookup"><span data-stu-id="a4e1c-117">Click hello **Download hello Microsoft Azure Site Recovery Unified Setup** link toodownload hello latest version of hello Scale-out Process Server installation.</span></span>

  > [!WARNING]
  <span data-ttu-id="a4e1c-118">hello versionen av din skalbar Processerver måste vara lika tooor som är mindre än hello Configuration Server-version som körs i din miljö.</span><span class="sxs-lookup"><span data-stu-id="a4e1c-118">hello version of your Scale-out Process Server should be equal tooor lesser than hello Configuration Server version running in your environment.</span></span> <span data-ttu-id="a4e1c-119">Ett enkelt sätt tooensure versionskompatibilitet är toouse hello samma installer bitar som du nyligen använt tooinstall/uppdatera din konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="a4e1c-119">A simple way tooensure version compatibility is toouse hello same installer bits that you recently used tooinstall/update your Configuration Server.</span></span>

## <a name="installing-and-registering-a-scale-out-process-server-from-gui"></a><span data-ttu-id="a4e1c-120">Installera och registrera en skalbar Processerver från GUI</span><span class="sxs-lookup"><span data-stu-id="a4e1c-120">Installing and registering a Scale-out Process Server from GUI</span></span>
<span data-ttu-id="a4e1c-121">Om du har tooscale upp distributionen utöver 200 källdatorer eller ett totalt dagligt omsättningsuppdateringar av mer än 2 TB, måste ytterligare processer servrar toohandle hello trafikvolym.</span><span class="sxs-lookup"><span data-stu-id="a4e1c-121">If you have tooscale out your deployment beyond 200 source machines, or a total daily churn rate of more than 2 TB, you need additional process servers toohandle hello traffic volume.</span></span>

<span data-ttu-id="a4e1c-122">Kontrollera hello [storlek rekommendationer för servrar](#size-recommendations-for-the-process-server), och Följ dessa instruktioner tooset in hello processervern.</span><span class="sxs-lookup"><span data-stu-id="a4e1c-122">Check hello [size recommendations for process servers](#size-recommendations-for-the-process-server), and then follow these instructions tooset up hello process server.</span></span> <span data-ttu-id="a4e1c-123">När du skapat hello-server kan du migrera källa datorer toouse den.</span><span class="sxs-lookup"><span data-stu-id="a4e1c-123">After setting up hello server, you migrate source machines toouse it.</span></span>

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-add-process-server.md)]


## <a name="installing-and-registering-a-scale-out-process-server-using-command-line"></a><span data-ttu-id="a4e1c-124">Installera och registrera en skalbar Processerver med hjälp av kommandoraden</span><span class="sxs-lookup"><span data-stu-id="a4e1c-124">Installing and registering a Scale-out Process Server using command-line</span></span>

```
UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address toobe used for data transfer] [/CSIP <IP address of CS toobe registered with>] [/PassphraseFilePath <Passphrase file path>]
```

### <a name="sample-usage"></a><span data-ttu-id="a4e1c-125">Exempel på användning</span><span class="sxs-lookup"><span data-stu-id="a4e1c-125">Sample usage</span></span>
```
MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /xC:\Temp\Extracted
cd C:\Temp\Extracted
UNIFIEDSETUP.EXE /AcceptThirdpartyEULA /servermode "PS" /InstallLocation "D:\" /EnvType "VMWare" /CSIP "10.150.24.119" /PassphraseFilePath "C:\Users\Administrator\Desktop\Passphrase.txt" /DataTransferSecurePort 443
```

### <a name="scale-out-process-server-installer-command-line-arguments"></a><span data-ttu-id="a4e1c-126">Skalbar Processerver installer kommandoradsargument.</span><span class="sxs-lookup"><span data-stu-id="a4e1c-126">Scale-out Process Server installer command-line arguments.</span></span>
[!INCLUDE [site-recovery-unified-setup-parameters](../../includes/site-recovery-unified-installer-command-parameters.md)]


### <a name="create-a-proxy-settings-configuration-file"></a><span data-ttu-id="a4e1c-127">Skapa en konfigurationsfil för proxy-inställningar</span><span class="sxs-lookup"><span data-stu-id="a4e1c-127">Create a proxy settings configuration file</span></span>
<span data-ttu-id="a4e1c-128">Parametern ProxySettingsFilePath använder en fil som indata.</span><span class="sxs-lookup"><span data-stu-id="a4e1c-128">ProxySettingsFilePath parameter takes a file as input.</span></span> <span data-ttu-id="a4e1c-129">Skapa fil med hello följande format och skickar den som indataparameter till ProxySettingsFilePath.</span><span class="sxs-lookup"><span data-stu-id="a4e1c-129">Create file using hello following format and pass it as input ProxySettingsFilePath parameter.</span></span>
```
* [ProxySettings]
* ProxyAuthentication = "Yes/No"
* Proxy IP = "IP Address"
* ProxyPort = "Port"
* ProxyUserName="UserName"
* ProxyPassword="Password"
```
## <a name="modifying-proxy-settings-for-scale-out-process-server"></a><span data-ttu-id="a4e1c-130">Ändra proxyinställningar för skalbar Processerver</span><span class="sxs-lookup"><span data-stu-id="a4e1c-130">Modifying proxy settings for Scale-out Process Server</span></span>
1. <span data-ttu-id="a4e1c-131">Logga in på servern för skalbar processen.</span><span class="sxs-lookup"><span data-stu-id="a4e1c-131">Login  into your Scale-out Process Server.</span></span>
2. <span data-ttu-id="a4e1c-132">Öppna ett kommandofönster Admin PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a4e1c-132">Open an Admin PowerShell command window.</span></span>
3. <span data-ttu-id="a4e1c-133">Kör följande kommando hello</span><span class="sxs-lookup"><span data-stu-id="a4e1c-133">Run hello following command</span></span>
  ```
  $pwd = ConvertTo-SecureString -String MyProxyUserPassword
  Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber –ProxyUserName domain\username -ProxyPassword $pwd
  net stop obengine
  net start obengine
  ```
4. <span data-ttu-id="a4e1c-134">Nästa Bläddra i katalog för toohello **%PROGRAMDATA%\ASR\Agent** och hello kör följande kommando</span><span class="sxs-lookup"><span data-stu-id="a4e1c-134">Next browse toohello directory **%PROGRAMDATA%\ASR\Agent** and run hello following command</span></span>
  ```
  cmd
  cdpcli.exe --registermt

  net stop obengine

  net start obengine

  exit
  ```

## <a name="re-registering-a-scale-out-process-server"></a><span data-ttu-id="a4e1c-135">Registrera en skalbar Processerver</span><span class="sxs-lookup"><span data-stu-id="a4e1c-135">Re-registering a Scale-out Process Server</span></span>
[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

* <span data-ttu-id="a4e1c-136">Nästa gång du öppna en kommandotolk för administratören.</span><span class="sxs-lookup"><span data-stu-id="a4e1c-136">Next open an Admin command prompt.</span></span>
* <span data-ttu-id="a4e1c-137">Bläddra i katalog toohello **%PROGRAMDATA%\ASR\Agent** och kör hello kommando</span><span class="sxs-lookup"><span data-stu-id="a4e1c-137">Browse toohello directory **%PROGRAMDATA%\ASR\Agent** and run hello command</span></span>

```
cdpcli.exe --registermt

net stop obengine

net start obengine
```

## <a name="upgrading-a-scale-out-process-server"></a><span data-ttu-id="a4e1c-138">Uppgradera en skalbar Processerver</span><span class="sxs-lookup"><span data-stu-id="a4e1c-138">Upgrading a Scale-out Process Server</span></span>
[!INCLUDE [site-recovery-vmware-upgrade -process-server](../../includes/site-recovery-vmware-upgrade-process-server-internal.md)]

## <a name="decommissioning-a-scale-out-process-server"></a><span data-ttu-id="a4e1c-139">Inaktivering av en skalbar Processerver</span><span class="sxs-lookup"><span data-stu-id="a4e1c-139">Decommissioning a Scale-out Process Server</span></span>
1. <span data-ttu-id="a4e1c-140">Se till att:</span><span class="sxs-lookup"><span data-stu-id="a4e1c-140">Ensure that:</span></span>
  - <span data-ttu-id="a4e1c-141">Status för Configuration Server-anslutningen visas som **ansluten** i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="a4e1c-141">Configuration Server's connection state shows as **Connected** in hello Azure portal</span></span>
  - <span data-ttu-id="a4e1c-142">Processervern är fortfarande toocommunicate med hello konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="a4e1c-142">Process Server's is still able toocommunicate with hello Configuration server.</span></span>
2. <span data-ttu-id="a4e1c-143">Logga in toohello processervern som administratör</span><span class="sxs-lookup"><span data-stu-id="a4e1c-143">Log in toohello process server as an administrator</span></span>
3. <span data-ttu-id="a4e1c-144">Öppna Kontrollpanelen > Program > avinstallera program</span><span class="sxs-lookup"><span data-stu-id="a4e1c-144">Open up Control Panel > Program > Uninstall Programs</span></span>
4. <span data-ttu-id="a4e1c-145">För att avinstallera hello i hello ordning som anges efter:</span><span class="sxs-lookup"><span data-stu-id="a4e1c-145">Uninstall hello programs in hello sequence given following:</span></span>
  * <span data-ttu-id="a4e1c-146">Server-processen för Microsoft Azure Site Recovery konfigurationsservern</span><span class="sxs-lookup"><span data-stu-id="a4e1c-146">Microsoft Azure Site Recovery Configuration Server/Process Server</span></span>
  * <span data-ttu-id="a4e1c-147">Microsoft Azure Site Recovery Configuration Server beroenden</span><span class="sxs-lookup"><span data-stu-id="a4e1c-147">Microsoft Azure Site Recovery Configuration Server Dependencies</span></span>
  * <span data-ttu-id="a4e1c-148">Microsoft Azure Recovery Services-agent</span><span class="sxs-lookup"><span data-stu-id="a4e1c-148">Microsoft Azure Recovery Services Agent</span></span>

<span data-ttu-id="a4e1c-149">Det kan ta upp too15 minuter för hello Processervern borttagning tooreflect i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a4e1c-149">It can take up-too15 minutes for hello Process Server deletion tooreflect in hello Azure portal.</span></span>

  > [!NOTE]
  <span data-ttu-id="a4e1c-150">Om hello processervern är toocommunicate med hello konfigurationsservern (anslutningens status i portalen är frånkopplad), måste du toofollow hello följande steg toopurge från hello konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="a4e1c-150">If hello Process server is unable toocommunicate with hello Configuration Server (Connection State in portal is Disconnected), then you need toofollow hello following steps toopurge it from hello Configuration Server.</span></span>

## <a name="unregistering-a-disconnected-scale-out-process-server-from-a-configuration-server"></a><span data-ttu-id="a4e1c-151">Avregistrerar en frånkopplad skalbar processerver från en Server Configuration</span><span class="sxs-lookup"><span data-stu-id="a4e1c-151">Unregistering a disconnected Scale-out Process server from a Configuration Server</span></span>

[!INCLUDE [site-recovery-vmware-upgrade-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]

## <a name="sizing-requirements-for-a-scale-out-process-server"></a><span data-ttu-id="a4e1c-152">Ändra storlek på kraven för en skalbar Processerver</span><span class="sxs-lookup"><span data-stu-id="a4e1c-152">Sizing requirements for a Scale-out Process Server</span></span>

| <span data-ttu-id="a4e1c-153">**Ytterligare processervern**</span><span class="sxs-lookup"><span data-stu-id="a4e1c-153">**Additional process server**</span></span> | <span data-ttu-id="a4e1c-154">**Cachestorleken för disk**</span><span class="sxs-lookup"><span data-stu-id="a4e1c-154">**Cache disk size**</span></span> | <span data-ttu-id="a4e1c-155">**Dataändringshastighet**</span><span class="sxs-lookup"><span data-stu-id="a4e1c-155">**Data change rate**</span></span> | <span data-ttu-id="a4e1c-156">**Skyddade datorer**</span><span class="sxs-lookup"><span data-stu-id="a4e1c-156">**Protected machines**</span></span> |
| --- | --- | --- | --- |
|<span data-ttu-id="a4e1c-157">4 vCPUs (2 sockets * 2 kärnor @ 2,5 GHz), 8 GB minne</span><span class="sxs-lookup"><span data-stu-id="a4e1c-157">4 vCPUs (2 sockets * 2 cores @ 2.5 GHz), 8-GB memory</span></span> |<span data-ttu-id="a4e1c-158">300 GB</span><span class="sxs-lookup"><span data-stu-id="a4e1c-158">300 GB</span></span> |<span data-ttu-id="a4e1c-159">250 GB eller mindre</span><span class="sxs-lookup"><span data-stu-id="a4e1c-159">250 GB or less</span></span> |<span data-ttu-id="a4e1c-160">Replikera 85 eller mindre datorer.</span><span class="sxs-lookup"><span data-stu-id="a4e1c-160">Replicate 85 or less machines.</span></span> |
|<span data-ttu-id="a4e1c-161">8 vCPUs (2 sockets * 4 kärnor @ 2,5 GHz), 12 GB minne</span><span class="sxs-lookup"><span data-stu-id="a4e1c-161">8 vCPUs (2 sockets * 4 cores @ 2.5 GHz), 12-GB memory</span></span> |<span data-ttu-id="a4e1c-162">600 GB</span><span class="sxs-lookup"><span data-stu-id="a4e1c-162">600 GB</span></span> |<span data-ttu-id="a4e1c-163">250 GB too1 TB</span><span class="sxs-lookup"><span data-stu-id="a4e1c-163">250 GB too1 TB</span></span> |<span data-ttu-id="a4e1c-164">Replikera mellan 85 150 datorer.</span><span class="sxs-lookup"><span data-stu-id="a4e1c-164">Replicate between 85-150 machines.</span></span> |
|<span data-ttu-id="a4e1c-165">12 vCPUs (2 sockets * 6 kärnor @ 2,5 GHz) 24 GB minne</span><span class="sxs-lookup"><span data-stu-id="a4e1c-165">12 vCPUs (2 sockets * 6 cores @ 2.5 GHz) 24-GB memory</span></span> |<span data-ttu-id="a4e1c-166">1 TB</span><span class="sxs-lookup"><span data-stu-id="a4e1c-166">1 TB</span></span> |<span data-ttu-id="a4e1c-167">1 TB too2 TB</span><span class="sxs-lookup"><span data-stu-id="a4e1c-167">1 TB too2 TB</span></span> |<span data-ttu-id="a4e1c-168">Replikera mellan 150 225 datorer.</span><span class="sxs-lookup"><span data-stu-id="a4e1c-168">Replicate between 150-225 machines.</span></span> |
