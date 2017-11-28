---
title: "aaaAutomate installation av Mobilitetstjänsten för Azure Site Recovery med hjälp av programdistribueringsverktyg | Microsoft Docs"
description: "Den här artikeln hjälper dig att automatisera installation av Mobilitetstjänsten med hjälp av verktyg för programvarudistribution som System Center Configuration Manager."
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: anoopkv
ms.openlocfilehash: 6c883c6d5308dcec6e0628b0c2196b3a12e08ebe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="automate-mobility-service-installation-by-using-software-deployment-tools"></a><span data-ttu-id="cee4a-103">Automatisera installation av Mobilitetstjänsten med hjälp av verktyg för programvarudistribution</span><span class="sxs-lookup"><span data-stu-id="cee4a-103">Automate Mobility Service installation by using software deployment tools</span></span>

>[!IMPORTANT]
<span data-ttu-id="cee4a-104">Det här dokumentet förutsätts att du använder version **9.9.4510.1** eller högre.</span><span class="sxs-lookup"><span data-stu-id="cee4a-104">This document assumes you are using version **9.9.4510.1** or higher.</span></span>

<span data-ttu-id="cee4a-105">Den här artikeln innehåller ett exempel på hur du kan använda System Center Configuration Manager toodeploy hello Azure Site Recovery Mobility-tjänsten i ditt datacenter.</span><span class="sxs-lookup"><span data-stu-id="cee4a-105">This article provides you an example of how you can use System Center Configuration Manager toodeploy hello Azure Site Recovery Mobility Service in your datacenter.</span></span> <span data-ttu-id="cee4a-106">Använda ett distributionsverktyg för programvara som Configuration Manager har hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="cee4a-106">Using a software deployment tool like Configuration Manager has hello following advantages:</span></span>
* <span data-ttu-id="cee4a-107">Schemalägga distributionen av nya installationer och uppgraderingar under planerat underhållsfönstret för programuppdateringar</span><span class="sxs-lookup"><span data-stu-id="cee4a-107">Scheduling deployment of fresh installations and upgrades, during your planned maintenance window for software updates</span></span>
* <span data-ttu-id="cee4a-108">Skalning toohundreds för distribution av servrar samtidigt</span><span class="sxs-lookup"><span data-stu-id="cee4a-108">Scaling deployment toohundreds of servers simultaneously</span></span>


> [!NOTE]
> <span data-ttu-id="cee4a-109">Den här artikeln använder System Center Configuration Manager 2012 R2 toodemonstrate hello aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="cee4a-109">This article uses System Center Configuration Manager 2012 R2 toodemonstrate hello deployment activity.</span></span> <span data-ttu-id="cee4a-110">Du kan också automatisera installation av Mobilitetstjänsten med hjälp av [Azure Automation och Desired State Configuration](site-recovery-automate-mobility-service-install.md).</span><span class="sxs-lookup"><span data-stu-id="cee4a-110">You could also automate Mobility Service installation by using [Azure Automation and Desired State Configuration](site-recovery-automate-mobility-service-install.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cee4a-111">Krav</span><span class="sxs-lookup"><span data-stu-id="cee4a-111">Prerequisites</span></span>
1. <span data-ttu-id="cee4a-112">Ett program distributionsverktyg, som Configuration Manager som redan har distribuerats i din miljö.</span><span class="sxs-lookup"><span data-stu-id="cee4a-112">A software deployment tool, like Configuration Manager, that is already deployed in your environment.</span></span>
  <span data-ttu-id="cee4a-113">Skapa två [enhetssamlingar](https://technet.microsoft.com/library/gg682169.aspx), en för alla **Windows-servrar**, och en annan för alla **Linux-servrar**, som du vill tooprotect genom att använda Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="cee4a-113">Create two [device collections](https://technet.microsoft.com/library/gg682169.aspx), one for all **Windows servers**, and another for all **Linux servers**, that you want tooprotect by using Site Recovery.</span></span>
3. <span data-ttu-id="cee4a-114">Konfigurationsservern som redan har registrerats med Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="cee4a-114">A configuration server that is already registered with Site Recovery.</span></span>
4. <span data-ttu-id="cee4a-115">En säker nätverksfilresurs (Server Message Block-resurs) som kan användas av hello Configuration Manager-servern.</span><span class="sxs-lookup"><span data-stu-id="cee4a-115">A secure network file share (Server Message Block share) that can be accessed by hello Configuration Manager server.</span></span>

## <a name="deploy-mobility-service-on-computers-running-windows"></a><span data-ttu-id="cee4a-116">Distribuera Mobilitetstjänsten på datorer som kör Windows</span><span class="sxs-lookup"><span data-stu-id="cee4a-116">Deploy Mobility Service on computers running Windows</span></span>
> [!NOTE]
> <span data-ttu-id="cee4a-117">Den här artikeln förutsätter att hello hello configuration serverns IP-adress är 192.168.3.121 och att hello säkert nätverk filresursen \\\ContosoSecureFS\MobilityServiceInstallers.</span><span class="sxs-lookup"><span data-stu-id="cee4a-117">This article assumes that hello IP address of hello configuration server is 192.168.3.121, and that hello secure network file share is \\\ContosoSecureFS\MobilityServiceInstallers.</span></span>

### <a name="step-1-prepare-for-deployment"></a><span data-ttu-id="cee4a-118">Steg 1: Förbereda för distribution</span><span class="sxs-lookup"><span data-stu-id="cee4a-118">Step 1: Prepare for deployment</span></span>
1. <span data-ttu-id="cee4a-119">Skapa en mapp på hello nätverksresurs och ge den namnet **MobSvcWindows**.</span><span class="sxs-lookup"><span data-stu-id="cee4a-119">Create a folder on hello network share, and name it **MobSvcWindows**.</span></span>
2. <span data-ttu-id="cee4a-120">Logga in tooyour konfigurationsservern och öppna en administrativ kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="cee4a-120">Sign in tooyour configuration server, and open an administrative command prompt.</span></span>
3. <span data-ttu-id="cee4a-121">Kör följande kommandon toogenerate en lösenfras fil hello:</span><span class="sxs-lookup"><span data-stu-id="cee4a-121">Run hello following commands toogenerate a passphrase file:</span></span>

    `cd %ProgramData%\ASR\home\svsystems\bin`

    `genpassphrase.exe -v > MobSvc.passphrase`
4. <span data-ttu-id="cee4a-122">Kopiera hello **MobSvc.passphrase** filen till hello **MobSvcWindows** mapp på nätverksresursen.</span><span class="sxs-lookup"><span data-stu-id="cee4a-122">Copy hello **MobSvc.passphrase** file into hello **MobSvcWindows** folder on your network share.</span></span>
5. <span data-ttu-id="cee4a-123">Bläddra toohello installer databasen på hello konfigurationsservern genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="cee4a-123">Browse toohello installer repository on hello configuration server by running hello following command:</span></span>

   `cd %ProgramData%\ASR\home\svsystems\puhsinstallsvc\repository`

6. <span data-ttu-id="cee4a-124">Kopiera hello  **Microsoft ASR\_UA\_*version*\_Windows\_GA\_*datum* \_ Release.exe** toohello **MobSvcWindows** mapp på nätverksresursen.</span><span class="sxs-lookup"><span data-stu-id="cee4a-124">Copy hello **Microsoft-ASR\_UA\_*version*\_Windows\_GA\_*date*\_Release.exe** toohello **MobSvcWindows** folder on your network share.</span></span>
7. <span data-ttu-id="cee4a-125">Kopiera följande kod hello och spara den som **install.bat** till hello **MobSvcWindows** mapp.</span><span class="sxs-lookup"><span data-stu-id="cee4a-125">Copy hello following code, and save it as **install.bat** into hello **MobSvcWindows** folder.</span></span>

   > [!NOTE]
   > <span data-ttu-id="cee4a-126">Ersätt hello [CSIP] platshållare i det här skriptet med hello faktiska värden för hello configuration-serverns IP-adress.</span><span class="sxs-lookup"><span data-stu-id="cee4a-126">Replace hello [CSIP] placeholders in this script with hello actual values of hello IP address of your configuration server.</span></span>

```DOS
Time /t >> C:\Temp\logfile.log
REM ==================================================
REM ==== Clean up hello folders ========================
RMDIR /S /q %temp%\MobSvc
MKDIR %Temp%\MobSvc
MKDIR C:\Temp
REM ==================================================

REM ==== Copy new files ==============================
COPY M*.* %Temp%\MobSvc
CD %Temp%\MobSvc
REN Micro*.exe MobSvcInstaller.exe
REM ==================================================

REM ==== Extract hello installer =======================
MobSvcInstaller.exe /q /x:%Temp%\MobSvc\Extracted
REM ==== Wait 10s for extraction toocomplete =========
TIMEOUT /t 10
REM =================================================

REM ==== Perform installation =======================
REM =================================================

CD %Temp%\MobSvc\Extracted
whoami >> C:\Temp\logfile.log
SET PRODKEY=HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall
REG QUERY %PRODKEY%\{275197FC-14FD-4560-A5EB-38217F80CBD1}
IF NOT %ERRORLEVEL% EQU 0 (
    echo "Product is not installed. Goto INSTALL." >> C:\Temp\logfile.log
    GOTO :INSTALL
) ELSE (
    echo "Product is installed." >> C:\Temp\logfile.log

    echo "Checking for Post-install action status." >> C:\Temp\logfile.log
    GOTO :POSTINSTALLCHECK
)

:POSTINSTALLCHECK
    REG QUERY "HKLM\SOFTWARE\Wow6432Node\InMage Systems\Installed Products\5" /v "PostInstallActions" | Find "Succeeded"
    If %ERRORLEVEL% EQU 0 (
        echo "Post-install actions succeeded. Checking for Configuration status." >> C:\Temp\logfile.log
        GOTO :CONFIGURATIONCHECK
    ) ELSE (
        echo "Post-install actions didn't succeed. Goto INSTALL." >> C:\Temp\logfile.log
        GOTO :INSTALL
    )

:CONFIGURATIONCHECK
    REG QUERY "HKLM\SOFTWARE\Wow6432Node\InMage Systems\Installed Products\5" /v "AgentConfigurationStatus" | Find "Succeeded"
    If %ERRORLEVEL% EQU 0 (
        echo "Configuration has succeeded. Goto UPGRADE." >> C:\Temp\logfile.log
        GOTO :UPGRADE
    ) ELSE (
        echo "Configuration didn't succeed. Goto CONFIGURE." >> C:\Temp\logfile.log
        GOTO :CONFIGURE
    )


:INSTALL
    echo "Perform installation." >> C:\Temp\logfile.log
    UnifiedAgent.exe /Role MS /InstallLocation "C:\Program Files (x86)\Microsoft Azure Site Recovery" /Platform "VmWare" /Silent
    IF %ERRORLEVEL% EQU 0 (
        echo "Installation has succeeded." >> C:\Temp\logfile.log
        (GOTO :CONFIGURE)
    ) ELSE (
        echo "Installation has failed." >> C:\Temp\logfile.log
        GOTO :ENDSCRIPT
    )

:CONFIGURE
    echo "Perform configuration." >> C:\Temp\logfile.log
    cd "C:\Program Files (x86)\Microsoft Azure Site Recovery\agent"
    UnifiedAgentConfigurator.exe  /CSEndPoint "[CSIP]" /PassphraseFilePath %Temp%\MobSvc\MobSvc.passphrase
    IF %ERRORLEVEL% EQU 0 (
        echo "Configuration has succeeded." >> C:\Temp\logfile.log
    ) ELSE (
        echo "Configuration has failed." >> C:\Temp\logfile.log
    )
    GOTO :ENDSCRIPT

:UPGRADE
    echo "Perform upgrade." >> C:\Temp\logfile.log
    UnifiedAgent.exe /Platform "VmWare" /Silent
    IF %ERRORLEVEL% EQU 0 (
        echo "Upgrade has succeeded." >> C:\Temp\logfile.log
    ) ELSE (
        echo "Upgrade has failed." >> C:\Temp\logfile.log
    )
    GOTO :ENDSCRIPT

:ENDSCRIPT
    echo "End of script." >> C:\Temp\logfile.log


```

### <a name="step-2-create-a-package"></a><span data-ttu-id="cee4a-127">Steg 2: Skapa ett paket</span><span class="sxs-lookup"><span data-stu-id="cee4a-127">Step 2: Create a package</span></span>

1. <span data-ttu-id="cee4a-128">Logga in tooyour Configuration Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="cee4a-128">Sign in tooyour Configuration Manager console.</span></span>
2. <span data-ttu-id="cee4a-129">Bläddra för**programbibliotek** > **programhantering** > **paket**.</span><span class="sxs-lookup"><span data-stu-id="cee4a-129">Browse too**Software Library** > **Application Management** > **Packages**.</span></span>
3. <span data-ttu-id="cee4a-130">Högerklicka på **paket**, och välj **skapa paket**.</span><span class="sxs-lookup"><span data-stu-id="cee4a-130">Right-click **Packages**, and select **Create Package**.</span></span>
4. <span data-ttu-id="cee4a-131">Ange värden för hello namn, beskrivning, tillverkare, språk och version.</span><span class="sxs-lookup"><span data-stu-id="cee4a-131">Provide values for hello name, description, manufacturer, language, and version.</span></span>
5. <span data-ttu-id="cee4a-132">Välj hello **det här paketet innehåller källfiler** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="cee4a-132">Select hello **This package contains source files** check box.</span></span>
6. <span data-ttu-id="cee4a-133">Klicka på **Bläddra**, och välj hello nätverksresurs där hello installer lagras (\\\ContosoSecureFS\MobilityServiceInstaller\MobSvcWindows).</span><span class="sxs-lookup"><span data-stu-id="cee4a-133">Click **Browse**, and select hello network share where hello installer is stored (\\\ContosoSecureFS\MobilityServiceInstaller\MobSvcWindows).</span></span>

  ![Skärmbild av skapa paket och Program Guiden](./media/site-recovery-install-mobility-service-using-sccm/create_sccm_package.png)

7. <span data-ttu-id="cee4a-135">På hello **Välj hello typ av program som du vill toocreate** väljer **standardprogram**, och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="cee4a-135">On hello **Choose hello program type that you want toocreate** page, select **Standard Program**, and click **Next**.</span></span>

  ![Skärmbild av skapa paket och Program Guiden](./media/site-recovery-install-mobility-service-using-sccm/sccm-standard-program.png)

8. <span data-ttu-id="cee4a-137">På hello **ange information om det här standardprogrammet** anger hello följande indata, och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="cee4a-137">On hello **Specify information about this standard program** page, provide hello following inputs, and click **Next**.</span></span> <span data-ttu-id="cee4a-138">(hello andra indata kan använda sina standardvärden.)</span><span class="sxs-lookup"><span data-stu-id="cee4a-138">(hello other inputs can use their default values.)</span></span>

  | <span data-ttu-id="cee4a-139">**Parameternamn**</span><span class="sxs-lookup"><span data-stu-id="cee4a-139">**Parameter name**</span></span> | <span data-ttu-id="cee4a-140">**Värde**</span><span class="sxs-lookup"><span data-stu-id="cee4a-140">**Value**</span></span> |
  |--|--|
  | <span data-ttu-id="cee4a-141">Namn</span><span class="sxs-lookup"><span data-stu-id="cee4a-141">Name</span></span> | <span data-ttu-id="cee4a-142">Installera Microsoft Azure-Mobilitetstjänsten (Windows)</span><span class="sxs-lookup"><span data-stu-id="cee4a-142">Install Microsoft Azure Mobility Service (Windows)</span></span> |
  | <span data-ttu-id="cee4a-143">Kommandorad</span><span class="sxs-lookup"><span data-stu-id="cee4a-143">Command line</span></span> | <span data-ttu-id="cee4a-144">Install.bat</span><span class="sxs-lookup"><span data-stu-id="cee4a-144">install.bat</span></span> |
  | <span data-ttu-id="cee4a-145">Programmet kan köras</span><span class="sxs-lookup"><span data-stu-id="cee4a-145">Program can run</span></span> | <span data-ttu-id="cee4a-146">Om en användare är inloggad</span><span class="sxs-lookup"><span data-stu-id="cee4a-146">Whether or not a user is logged on</span></span> |

  ![Skärmbild av skapa paket och Program Guiden](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties.png)

9. <span data-ttu-id="cee4a-148">Välj hello operativsystem på hello nästa sida.</span><span class="sxs-lookup"><span data-stu-id="cee4a-148">On hello next page, select hello target operating systems.</span></span> <span data-ttu-id="cee4a-149">Mobilitetstjänsten kan installeras endast på Windows Server 2012 R2, Windows Server 2012 och Windows Server 2008 R2.</span><span class="sxs-lookup"><span data-stu-id="cee4a-149">Mobility Service can be installed only on Windows Server 2012 R2, Windows Server 2012, and Windows Server 2008 R2.</span></span>

  ![Skärmbild av skapa paket och Program Guiden](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties-page2.png)

10. <span data-ttu-id="cee4a-151">toocomplete hello guiden klickar du på **nästa** två gånger.</span><span class="sxs-lookup"><span data-stu-id="cee4a-151">toocomplete hello wizard, click **Next** twice.</span></span>


> [!NOTE]
> <span data-ttu-id="cee4a-152">hello skript stöder både nya installationer av Mobilitetstjänsten agenter och uppdaterar tooagents som redan är installerade.</span><span class="sxs-lookup"><span data-stu-id="cee4a-152">hello script supports both new installations of Mobility Service agents and updates tooagents that are already installed.</span></span>

### <a name="step-3-deploy-hello-package"></a><span data-ttu-id="cee4a-153">Steg 3: Distribuera hello-paket</span><span class="sxs-lookup"><span data-stu-id="cee4a-153">Step 3: Deploy hello package</span></span>
1. <span data-ttu-id="cee4a-154">Högerklicka på paketet i hello Configuration Manager-konsolen och välj **distribuera innehåll**.</span><span class="sxs-lookup"><span data-stu-id="cee4a-154">In hello Configuration Manager console, right-click your package, and select **Distribute Content**.</span></span>
  <span data-ttu-id="cee4a-155">![Skärmbild av Configuration Manager-konsolen](./media/site-recovery-install-mobility-service-using-sccm/sccm_distribute.png)</span><span class="sxs-lookup"><span data-stu-id="cee4a-155">![Screenshot of Configuration Manager console](./media/site-recovery-install-mobility-service-using-sccm/sccm_distribute.png)</span></span>
2. <span data-ttu-id="cee4a-156">Välj hello  **[distributionsplatser](https://technet.microsoft.com/library/gg712321.aspx#BKMK_PlanForDistributionPoints)**  på toowhich hello paket ska kopieras.</span><span class="sxs-lookup"><span data-stu-id="cee4a-156">Select hello **[distribution points](https://technet.microsoft.com/library/gg712321.aspx#BKMK_PlanForDistributionPoints)** on toowhich hello packages should be copied.</span></span>
3. <span data-ttu-id="cee4a-157">Fullständig hello guiden.</span><span class="sxs-lookup"><span data-stu-id="cee4a-157">Complete hello wizard.</span></span> <span data-ttu-id="cee4a-158">hello-paketet och sedan startar replikerar toohello angivna distributionsplatser.</span><span class="sxs-lookup"><span data-stu-id="cee4a-158">hello package then starts replicating toohello specified distribution points.</span></span>
4. <span data-ttu-id="cee4a-159">När hello paketdistribution görs, högerklicka på hello paketet och väljer **distribuera**.</span><span class="sxs-lookup"><span data-stu-id="cee4a-159">After hello package distribution is done, right-click hello package, and select **Deploy**.</span></span>
  <span data-ttu-id="cee4a-160">![Skärmbild av Configuration Manager-konsolen](./media/site-recovery-install-mobility-service-using-sccm/sccm_deploy.png)</span><span class="sxs-lookup"><span data-stu-id="cee4a-160">![Screenshot of Configuration Manager console](./media/site-recovery-install-mobility-service-using-sccm/sccm_deploy.png)</span></span>
5. <span data-ttu-id="cee4a-161">Välj hello Windows Server enhetssamling som du skapade i avsnittet förutsättningar för hello som hello målsamlingen för distributionen.</span><span class="sxs-lookup"><span data-stu-id="cee4a-161">Select hello Windows Server device collection you created in hello prerequisites section as hello target collection for deployment.</span></span>

  ![Guiden Distribuera programvara skärmbild](./media/site-recovery-install-mobility-service-using-sccm/sccm-select-target-collection.png)

6. <span data-ttu-id="cee4a-163">På hello **ange hello innehållsmål** väljer din **distributionsplatser**.</span><span class="sxs-lookup"><span data-stu-id="cee4a-163">On hello **Specify hello content destination** page, select your **Distribution Points**.</span></span>
7. <span data-ttu-id="cee4a-164">På hello **ange inställningar toocontrol hur programvaran distribueras** , se till att hello syfte är **krävs**.</span><span class="sxs-lookup"><span data-stu-id="cee4a-164">On hello **Specify settings toocontrol how this software is deployed** page, ensure that hello purpose is **Required**.</span></span>

  ![Guiden Distribuera programvara skärmbild](./media/site-recovery-install-mobility-service-using-sccm/sccm-deploy-select-purpose.png)

8. <span data-ttu-id="cee4a-166">På hello **ange hello schemat för distributionen** anger du ett schema.</span><span class="sxs-lookup"><span data-stu-id="cee4a-166">On hello **Specify hello schedule for this deployment** page, specify a schedule.</span></span> <span data-ttu-id="cee4a-167">Mer information finns i [schemaläggning paket](https://technet.microsoft.com/library/gg682178.aspx).</span><span class="sxs-lookup"><span data-stu-id="cee4a-167">For more information, see [scheduling packages](https://technet.microsoft.com/library/gg682178.aspx).</span></span>
9. <span data-ttu-id="cee4a-168">På hello **distributionsplatser** konfigurerar hello egenskaper enligt toohello behoven i ditt datacenter.</span><span class="sxs-lookup"><span data-stu-id="cee4a-168">On hello **Distribution Points** page, configure hello properties according toohello needs of your datacenter.</span></span> <span data-ttu-id="cee4a-169">Slutför guiden hello.</span><span class="sxs-lookup"><span data-stu-id="cee4a-169">Then complete hello wizard.</span></span>

> [!TIP]
> <span data-ttu-id="cee4a-170">tooavoid onödiga startas om, schema hello installationen under din månatligt underhåll eller programvara uppdateringar fönster.</span><span class="sxs-lookup"><span data-stu-id="cee4a-170">tooavoid unnecessary reboots, schedule hello package installation during your monthly maintenance window or software updates window.</span></span>

<span data-ttu-id="cee4a-171">Du kan övervaka hello förlopp med hjälp av hello Configuration Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="cee4a-171">You can monitor hello deployment progress by using hello Configuration Manager console.</span></span> <span data-ttu-id="cee4a-172">Gå för**övervakning** > **distributioner** > *[package name]*.</span><span class="sxs-lookup"><span data-stu-id="cee4a-172">Go too**Monitoring** > **Deployments** > *[your package name]*.</span></span>

  ![Skärmbild av Configuration Manager alternativet toomonitor distributioner](./media/site-recovery-install-mobility-service-using-sccm/report.PNG)

## <a name="deploy-mobility-service-on-computers-running-linux"></a><span data-ttu-id="cee4a-174">Distribuera Mobilitetstjänsten på datorer som kör Linux</span><span class="sxs-lookup"><span data-stu-id="cee4a-174">Deploy Mobility Service on computers running Linux</span></span>
> [!NOTE]
> <span data-ttu-id="cee4a-175">Den här artikeln förutsätter att hello hello configuration serverns IP-adress är 192.168.3.121 och att hello säkert nätverk filresursen \\\ContosoSecureFS\MobilityServiceInstallers.</span><span class="sxs-lookup"><span data-stu-id="cee4a-175">This article assumes that hello IP address of hello configuration server is 192.168.3.121, and that hello secure network file share is \\\ContosoSecureFS\MobilityServiceInstallers.</span></span>

### <a name="step-1-prepare-for-deployment"></a><span data-ttu-id="cee4a-176">Steg 1: Förbereda för distribution</span><span class="sxs-lookup"><span data-stu-id="cee4a-176">Step 1: Prepare for deployment</span></span>
1. <span data-ttu-id="cee4a-177">Skapa en mapp på hello nätverksresurs, och ger den namnet som **MobSvcLinux**.</span><span class="sxs-lookup"><span data-stu-id="cee4a-177">Create a folder on hello network share, and name it as **MobSvcLinux**.</span></span>
2. <span data-ttu-id="cee4a-178">Logga in tooyour konfigurationsservern och öppna en administrativ kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="cee4a-178">Sign in tooyour configuration server, and open an administrative command prompt.</span></span>
3. <span data-ttu-id="cee4a-179">Kör följande kommandon toogenerate en lösenfras fil hello:</span><span class="sxs-lookup"><span data-stu-id="cee4a-179">Run hello following commands toogenerate a passphrase file:</span></span>

    `cd %ProgramData%\ASR\home\svsystems\bin`

    `genpassphrase.exe -v > MobSvc.passphrase`
4. <span data-ttu-id="cee4a-180">Kopiera hello **MobSvc.passphrase** filen till hello **MobSvcLinux** mapp på nätverksresursen.</span><span class="sxs-lookup"><span data-stu-id="cee4a-180">Copy hello **MobSvc.passphrase** file into hello **MobSvcLinux** folder on your network share.</span></span>
5. <span data-ttu-id="cee4a-181">Bläddra toohello installer databasen på hello konfigurationsservern genom att köra hello-kommando:</span><span class="sxs-lookup"><span data-stu-id="cee4a-181">Browse toohello installer repository on hello configuration server by running hello command:</span></span>

   `cd %ProgramData%\ASR\home\svsystems\puhsinstallsvc\repository`

6. <span data-ttu-id="cee4a-182">Kopiera hello följande filer toohello **MobSvcLinux** mapp på nätverksresursen:</span><span class="sxs-lookup"><span data-stu-id="cee4a-182">Copy hello following files toohello **MobSvcLinux** folder on your network share:</span></span>
   * <span data-ttu-id="cee4a-183">Microsoft ASR\_UA\*RHEL6 64*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="cee4a-183">Microsoft-ASR\_UA\*RHEL6-64*release.tar.gz</span></span>
   * <span data-ttu-id="cee4a-184">Microsoft ASR\_UA\*RHEL7 64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="cee4a-184">Microsoft-ASR\_UA\*RHEL7-64\*release.tar.gz</span></span>
   * <span data-ttu-id="cee4a-185">Microsoft ASR\_UA\*SLES11-SP3-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="cee4a-185">Microsoft-ASR\_UA\*SLES11-SP3-64\*release.tar.gz</span></span>
   * <span data-ttu-id="cee4a-186">Microsoft ASR\_UA\*SLES11-SP4-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="cee4a-186">Microsoft-ASR\_UA\*SLES11-SP4-64\*release.tar.gz</span></span>
   * <span data-ttu-id="cee4a-187">Microsoft ASR\_UA\*OL6 64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="cee4a-187">Microsoft-ASR\_UA\*OL6-64\*release.tar.gz</span></span>
   * <span data-ttu-id="cee4a-188">Microsoft ASR\_UA\*UBUNTU-14.04-64\*release.tar.gz</span><span class="sxs-lookup"><span data-stu-id="cee4a-188">Microsoft-ASR\_UA\*UBUNTU-14.04-64\*release.tar.gz</span></span>


7. <span data-ttu-id="cee4a-189">Kopiera följande kod hello och spara den som **install_linux.sh** till hello **MobSvcLinux** mapp.</span><span class="sxs-lookup"><span data-stu-id="cee4a-189">Copy hello following code, and save it as **install_linux.sh** into hello **MobSvcLinux** folder.</span></span>
   > [!NOTE]
   > <span data-ttu-id="cee4a-190">Ersätt hello [CSIP] platshållare i det här skriptet med hello faktiska värden för hello configuration-serverns IP-adress.</span><span class="sxs-lookup"><span data-stu-id="cee4a-190">Replace hello [CSIP] placeholders in this script with hello actual values of hello IP address of your configuration server.</span></span>

```Bash
#!/usr/bin/env bash

rm -rf /tmp/MobSvc
mkdir -p /tmp/MobSvc
INSTALL_DIR='/usr/local/ASR'
VX_VERSION_FILE='/usr/local/.vx_version'

echo "=============================" >> /tmp/MobSvc/sccm.log
echo `date` >> /tmp/MobSvc/sccm.log
echo "=============================" >> /tmp/MobSvc/sccm.log

if [ -f /etc/oracle-release ] && [ -f /etc/redhat-release ]; then
    if grep -q 'Oracle Linux Server release 6.*' /etc/oracle-release; then
        if uname -a | grep -q x86_64; then
            OS="OL6-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *OL6*.tar.gz /tmp/MobSvc
        fi
    fi
elif [ -f /etc/redhat-release ]; then
    if grep -q 'Red Hat Enterprise Linux Server release 6.* (Santiago)' /etc/redhat-release || \
        grep -q 'CentOS Linux release 6.* (Final)' /etc/redhat-release || \
        grep -q 'CentOS release 6.* (Final)' /etc/redhat-release; then
        if uname -a | grep -q x86_64; then
            OS="RHEL6-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *RHEL6*.tar.gz /tmp/MobSvc
        fi
    elif grep -q 'Red Hat Enterprise Linux Server release 7.* (Maipo)' /etc/redhat-release || \
        grep -q 'CentOS Linux release 7.* (Core)' /etc/redhat-release; then
        if uname -a | grep -q x86_64; then
            OS="RHEL7-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *RHEL7*.tar.gz /tmp/MobSvc
                fi
    fi
elif [ -f /etc/SuSE-release ] && grep -q 'VERSION = 11' /etc/SuSE-release; then
    if grep -q "SUSE Linux Enterprise Server 11" /etc/SuSE-release && grep -q 'PATCHLEVEL = 3' /etc/SuSE-release; then
        if uname -a | grep -q x86_64; then
            OS="SLES11-SP3-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *SLES11-SP3*.tar.gz /tmp/MobSvc
        fi
    elif grep -q "SUSE Linux Enterprise Server 11" /etc/SuSE-release && grep -q 'PATCHLEVEL = 4' /etc/SuSE-release; then
        if uname -a | grep -q x86_64; then
            OS="SLES11-SP4-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *SLES11-SP4*.tar.gz /tmp/MobSvc
        fi
    fi
elif [ -f /etc/lsb-release ] ; then
    if grep -q 'DISTRIB_RELEASE=14.04' /etc/lsb-release ; then
       if uname -a | grep -q x86_64; then
           OS="UBUNTU-14.04-64"
           echo $OS >> /tmp/MobSvc/sccm.log
           cp *UBUNTU-14*.tar.gz /tmp/MobSvc
       fi
    fi
else
    exit 1
fi

if [ -z "$OS" ]; then
    exit 1
fi

Install()
{
    echo "Perform Installation." >> /tmp/MobSvc/sccm.log
    ./install -q -d ${INSTALL_DIR} -r MS -v VmWare
    RET_VAL=$?
    echo "Installation Returncode: $RET_VAL" >> /tmp/MobSvc/sccm.log
    if [ $RET_VAL -eq 0 ]; then
        echo "Installation has succeeded. Proceed tooconfiguration." >> /tmp/MobSvc/sccm.log
        Configure
    else
        echo "Installation has failed." >> /tmp/MobSvc/sccm.log
        exit $RET_VAL
    fi
}

Configure()
{
    echo "Perform configuration." >> /tmp/MobSvc/sccm.log
    ${INSTALL_DIR}/Vx/bin/UnifiedAgentConfigurator.sh -i [CSIP] -P MobSvc.passphrase
    RET_VAL=$?
    echo "Configuration Returncode: $RET_VAL" >> /tmp/MobSvc/sccm.log
    if [ $RET_VAL -eq 0 ]; then
        echo "Configuration has succeeded." >> /tmp/MobSvc/sccm.log
    else
        echo "Configuration has failed." >> /tmp/MobSvc/sccm.log
        exit $RET_VAL
    fi
}

Upgrade()
{
    echo "Perform Upgrade." >> /tmp/MobSvc/sccm.log
    ./install -q -v VmWare
    RET_VAL=$?
    echo "Upgrade Returncode: $RET_VAL" >> /tmp/MobSvc/sccm.log
    if [ $RET_VAL -eq 0 ]; then
        echo "Upgrade has succeeded." >> /tmp/MobSvc/sccm.log
    else
        echo "Upgrade has failed." >> /tmp/MobSvc/sccm.log
        exit $RET_VAL
    fi
}

cp MobSvc.passphrase /tmp/MobSvc
cd /tmp/MobSvc

tar -zxvf *.tar.gz

if [ -e ${VX_VERSION_FILE} ]; then
    echo "${VX_VERSION_FILE} exists. Checking for configuration status." >> /tmp/MobSvc/sccm.log
    agent_configuration=$(grep ^AGENT_CONFIGURATION_STATUS "${VX_VERSION_FILE}" | cut -d"=" -f2 | tr -d " ")
    echo "agent_configuration=$agent_configuration" >> /tmp/MobSvc/sccm.log
     if [ "$agent_configuration" == "Succeeded" ]; then
        echo "Agent is already configured. Proceed tooUpgrade." >> /tmp/MobSvc/sccm.log
        Upgrade
    else
        echo "Agent is not configured. Proceed tooConfigure." >> /tmp/MobSvc/sccm.log
        Configure
    fi
else
    Install
fi

cd /tmp

```

### <a name="step-2-create-a-package"></a><span data-ttu-id="cee4a-191">Steg 2: Skapa ett paket</span><span class="sxs-lookup"><span data-stu-id="cee4a-191">Step 2: Create a package</span></span>

1. <span data-ttu-id="cee4a-192">Logga in tooyour Configuration Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="cee4a-192">Sign in  tooyour Configuration Manager console.</span></span>
2. <span data-ttu-id="cee4a-193">Bläddra för**programbibliotek** > **programhantering** > **paket**.</span><span class="sxs-lookup"><span data-stu-id="cee4a-193">Browse too**Software Library** > **Application Management** > **Packages**.</span></span>
3. <span data-ttu-id="cee4a-194">Högerklicka på **paket**, och välj **skapa paket**.</span><span class="sxs-lookup"><span data-stu-id="cee4a-194">Right-click **Packages**, and select **Create Package**.</span></span>
4. <span data-ttu-id="cee4a-195">Ange värden för hello namn, beskrivning, tillverkare, språk och version.</span><span class="sxs-lookup"><span data-stu-id="cee4a-195">Provide values for hello name, description, manufacturer, language, and version.</span></span>
5. <span data-ttu-id="cee4a-196">Välj hello **det här paketet innehåller källfiler** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="cee4a-196">Select hello **This package contains source files** check box.</span></span>
6. <span data-ttu-id="cee4a-197">Klicka på **Bläddra**, och välj hello nätverksresurs där hello installer lagras (\\\ContosoSecureFS\MobilityServiceInstaller\MobSvcLinux).</span><span class="sxs-lookup"><span data-stu-id="cee4a-197">Click **Browse**, and select hello network share where hello installer is stored (\\\ContosoSecureFS\MobilityServiceInstaller\MobSvcLinux).</span></span>

  ![Skärmbild av skapa paket och Program Guiden](./media/site-recovery-install-mobility-service-using-sccm/create_sccm_package-linux.png)

7. <span data-ttu-id="cee4a-199">På hello **Välj hello typ av program som du vill toocreate** väljer **standardprogram**, och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="cee4a-199">On hello **Choose hello program type that you want toocreate** page, select **Standard Program**, and click **Next**.</span></span>

  ![Skärmbild av skapa paket och Program Guiden](./media/site-recovery-install-mobility-service-using-sccm/sccm-standard-program.png)

8. <span data-ttu-id="cee4a-201">På hello **ange information om det här standardprogrammet** anger hello följande indata, och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="cee4a-201">On hello **Specify information about this standard program** page, provide hello following inputs, and click **Next**.</span></span> <span data-ttu-id="cee4a-202">(hello andra indata kan använda sina standardvärden.)</span><span class="sxs-lookup"><span data-stu-id="cee4a-202">(hello other inputs can use their default values.)</span></span>

    | <span data-ttu-id="cee4a-203">**Parameternamn**</span><span class="sxs-lookup"><span data-stu-id="cee4a-203">**Parameter name**</span></span> | <span data-ttu-id="cee4a-204">**Värde**</span><span class="sxs-lookup"><span data-stu-id="cee4a-204">**Value**</span></span> |
  |--|--|
  | <span data-ttu-id="cee4a-205">Namn</span><span class="sxs-lookup"><span data-stu-id="cee4a-205">Name</span></span> | <span data-ttu-id="cee4a-206">Installera Microsoft Azure-Mobilitetstjänsten (Linux)</span><span class="sxs-lookup"><span data-stu-id="cee4a-206">Install Microsoft Azure Mobility Service (Linux)</span></span> |
  | <span data-ttu-id="cee4a-207">Kommandorad</span><span class="sxs-lookup"><span data-stu-id="cee4a-207">Command line</span></span> | <span data-ttu-id="cee4a-208">./install_linux.SH</span><span class="sxs-lookup"><span data-stu-id="cee4a-208">./install_linux.sh</span></span> |
  | <span data-ttu-id="cee4a-209">Programmet kan köras</span><span class="sxs-lookup"><span data-stu-id="cee4a-209">Program can run</span></span> | <span data-ttu-id="cee4a-210">Om en användare är inloggad</span><span class="sxs-lookup"><span data-stu-id="cee4a-210">Whether or not a user is logged on</span></span> |

  ![Skärmbild av skapa paket och Program Guiden](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties-linux.png)

9. <span data-ttu-id="cee4a-212">På nästa sida i hello väljer **det här programmet kan köras på alla plattformar**.</span><span class="sxs-lookup"><span data-stu-id="cee4a-212">On hello next page, select **This program can run on any platform**.</span></span>
  <span data-ttu-id="cee4a-213">![Skärmbild av skapa paket och Program Guiden](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties-page2-linux.png)</span><span class="sxs-lookup"><span data-stu-id="cee4a-213">![Screenshot of Create Package and Program wizard](./media/site-recovery-install-mobility-service-using-sccm/sccm-program-properties-page2-linux.png)</span></span>

10. <span data-ttu-id="cee4a-214">toocomplete hello guiden klickar du på **nästa** två gånger.</span><span class="sxs-lookup"><span data-stu-id="cee4a-214">toocomplete hello wizard, click **Next** twice.</span></span>

> [!NOTE]
> <span data-ttu-id="cee4a-215">hello skript stöder både nya installationer av Mobilitetstjänsten agenter och uppdaterar tooagents som redan är installerade.</span><span class="sxs-lookup"><span data-stu-id="cee4a-215">hello script supports both new installations of Mobility Service agents and updates tooagents that are already installed.</span></span>

### <a name="step-3-deploy-hello-package"></a><span data-ttu-id="cee4a-216">Steg 3: Distribuera hello-paket</span><span class="sxs-lookup"><span data-stu-id="cee4a-216">Step 3: Deploy hello package</span></span>
1. <span data-ttu-id="cee4a-217">Högerklicka på paketet i hello Configuration Manager-konsolen och välj **distribuera innehåll**.</span><span class="sxs-lookup"><span data-stu-id="cee4a-217">In hello Configuration Manager console, right-click your package, and select **Distribute Content**.</span></span>
  <span data-ttu-id="cee4a-218">![Skärmbild av Configuration Manager-konsolen](./media/site-recovery-install-mobility-service-using-sccm/sccm_distribute.png)</span><span class="sxs-lookup"><span data-stu-id="cee4a-218">![Screenshot of Configuration Manager console](./media/site-recovery-install-mobility-service-using-sccm/sccm_distribute.png)</span></span>
2. <span data-ttu-id="cee4a-219">Välj hello  **[distributionsplatser](https://technet.microsoft.com/library/gg712321.aspx#BKMK_PlanForDistributionPoints)**  på toowhich hello paket ska kopieras.</span><span class="sxs-lookup"><span data-stu-id="cee4a-219">Select hello **[distribution points](https://technet.microsoft.com/library/gg712321.aspx#BKMK_PlanForDistributionPoints)** on toowhich hello packages should be copied.</span></span>
3. <span data-ttu-id="cee4a-220">Fullständig hello guiden.</span><span class="sxs-lookup"><span data-stu-id="cee4a-220">Complete hello wizard.</span></span> <span data-ttu-id="cee4a-221">hello-paketet och sedan startar replikerar toohello angivna distributionsplatser.</span><span class="sxs-lookup"><span data-stu-id="cee4a-221">hello package then starts replicating toohello specified distribution points.</span></span>
4. <span data-ttu-id="cee4a-222">När hello paketdistribution görs, högerklicka på hello paketet och väljer **distribuera**.</span><span class="sxs-lookup"><span data-stu-id="cee4a-222">After hello package distribution is done, right-click hello package, and select **Deploy**.</span></span>
  <span data-ttu-id="cee4a-223">![Skärmbild av Configuration Manager-konsolen](./media/site-recovery-install-mobility-service-using-sccm/sccm_deploy.png)</span><span class="sxs-lookup"><span data-stu-id="cee4a-223">![Screenshot of Configuration Manager console](./media/site-recovery-install-mobility-service-using-sccm/sccm_deploy.png)</span></span>
5. <span data-ttu-id="cee4a-224">Välj hello Linux Server enhetssamling som du skapade i avsnittet förutsättningar för hello som hello målsamlingen för distributionen.</span><span class="sxs-lookup"><span data-stu-id="cee4a-224">Select hello Linux Server device collection you created in hello prerequisites section as hello target collection for deployment.</span></span>

  ![Guiden Distribuera programvara skärmbild](./media/site-recovery-install-mobility-service-using-sccm/sccm-select-target-collection-linux.png)

6. <span data-ttu-id="cee4a-226">På hello **ange hello innehållsmål** väljer din **distributionsplatser**.</span><span class="sxs-lookup"><span data-stu-id="cee4a-226">On hello **Specify hello content destination** page, select your **Distribution Points**.</span></span>
7. <span data-ttu-id="cee4a-227">På hello **ange inställningar toocontrol hur programvaran distribueras** , se till att hello syfte är **krävs**.</span><span class="sxs-lookup"><span data-stu-id="cee4a-227">On hello **Specify settings toocontrol how this software is deployed** page, ensure that hello purpose is **Required**.</span></span>

  ![Guiden Distribuera programvara skärmbild](./media/site-recovery-install-mobility-service-using-sccm/sccm-deploy-select-purpose.png)

8. <span data-ttu-id="cee4a-229">På hello **ange hello schemat för distributionen** anger du ett schema.</span><span class="sxs-lookup"><span data-stu-id="cee4a-229">On hello **Specify hello schedule for this deployment** page, specify a schedule.</span></span> <span data-ttu-id="cee4a-230">Mer information finns i [schemaläggning paket](https://technet.microsoft.com/library/gg682178.aspx).</span><span class="sxs-lookup"><span data-stu-id="cee4a-230">For more information, see [scheduling packages](https://technet.microsoft.com/library/gg682178.aspx).</span></span>
9. <span data-ttu-id="cee4a-231">På hello **distributionsplatser** konfigurerar hello egenskaper enligt toohello behoven i ditt datacenter.</span><span class="sxs-lookup"><span data-stu-id="cee4a-231">On hello **Distribution Points** page, configure hello properties according toohello needs of your datacenter.</span></span> <span data-ttu-id="cee4a-232">Slutför guiden hello.</span><span class="sxs-lookup"><span data-stu-id="cee4a-232">Then complete hello wizard.</span></span>

<span data-ttu-id="cee4a-233">Mobilitetstjänsten installeras på hello Enhetssamling för Linux-servern, enligt toohello schema som du har konfigurerat.</span><span class="sxs-lookup"><span data-stu-id="cee4a-233">Mobility Service gets installed on hello Linux Server Device Collection, according toohello schedule you configured.</span></span>

## <a name="other-methods-tooinstall-mobility-service"></a><span data-ttu-id="cee4a-234">Andra metoder tooinstall Mobilitetstjänsten</span><span class="sxs-lookup"><span data-stu-id="cee4a-234">Other methods tooinstall Mobility Service</span></span>
<span data-ttu-id="cee4a-235">Här följer några andra alternativ för att installera Mobilitetstjänsten:</span><span class="sxs-lookup"><span data-stu-id="cee4a-235">Here are some other options for installing Mobility Service:</span></span>
* [<span data-ttu-id="cee4a-236">Manuell Installation med hjälp av Användargränssnittet</span><span class="sxs-lookup"><span data-stu-id="cee4a-236">Manual Installation using GUI</span></span>](http://aka.ms/mobsvcmanualinstall)
* [<span data-ttu-id="cee4a-237">Manuell Installation med hjälp av kommandoraden</span><span class="sxs-lookup"><span data-stu-id="cee4a-237">Manual Installation using command-line</span></span>](http://aka.ms/mobsvcmanualinstallcli)
* [<span data-ttu-id="cee4a-238">Push-Installation med konfigurationsservern</span><span class="sxs-lookup"><span data-stu-id="cee4a-238">Push Installation using configuration server </span></span>](http://aka.ms/pushinstall)
* [<span data-ttu-id="cee4a-239">Automatisk Installation med hjälp av Azure Automation & Desired State Configuration</span><span class="sxs-lookup"><span data-stu-id="cee4a-239">Automated Installation using Azure Automation & Desired State Configuration </span></span>](http://aka.ms/mobsvcdscinstall)

## <a name="uninstall-mobility-service"></a><span data-ttu-id="cee4a-240">Avinstallera Mobilitetstjänsten</span><span class="sxs-lookup"><span data-stu-id="cee4a-240">Uninstall Mobility Service</span></span>
<span data-ttu-id="cee4a-241">Du kan skapa Configuration Manager paket toouninstall Mobilitetstjänsten.</span><span class="sxs-lookup"><span data-stu-id="cee4a-241">You can create Configuration Manager packages toouninstall Mobility Service.</span></span> <span data-ttu-id="cee4a-242">Använd följande skript toodo så hello:</span><span class="sxs-lookup"><span data-stu-id="cee4a-242">Use hello following script toodo so:</span></span>

```
Time /t >> C:\logfile.log
REM ==================================================
REM ==== Check if Mob Svc is already installed =======
REM ==== If not installed no operation required ========
REM ==== Else run uninstall command =====================
REM ==== {275197FC-14FD-4560-A5EB-38217F80CBD1} is ====
REM ==== guid for Mob Svc Installer ====================
whoami >> C:\logfile.log
NET START | FIND "InMage Scout Application Service"
IF  %ERRORLEVEL% EQU 1 (GOTO :INSTALL) ELSE GOTO :UNINSTALL
:NOOPERATION
                echo "No Operation Required." >> c:\logfile.log
                GOTO :ENDSCRIPT
:UNINSTALL
                echo "Uninstall" >> C:\logfile.log
                MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
:ENDSCRIPT

```

## <a name="next-steps"></a><span data-ttu-id="cee4a-243">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cee4a-243">Next steps</span></span>
<span data-ttu-id="cee4a-244">Du är nu redo för[Aktivera skydd](https://docs.microsoft.com/en-us/azure/site-recovery/site-recovery-vmware-to-azure#step-6-replicate-applications) för dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="cee4a-244">You are now ready too[enable protection](https://docs.microsoft.com/en-us/azure/site-recovery/site-recovery-vmware-to-azure#step-6-replicate-applications) for your virtual machines.</span></span>
