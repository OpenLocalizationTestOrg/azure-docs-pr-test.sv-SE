---
title: "aaaGetting igång med Azure Automation DSC | Microsoft Docs"
description: "Förklaring och exempel på hello vanligaste uppgifterna i Azure Automation önskad tillstånd Configuration (DSC)"
services: automation
documentationcenter: na
author: eslesar
manager: carmonm
editor: tysonn
ms.assetid: a3816593-70a3-403b-9a43-d5555fd2cee2
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: na
ms.date: 11/21/2016
ms.author: magoedte;eslesar
ms.openlocfilehash: 82910c96e928b9264c2e1b52a5c98dc47273dcc5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-automation-dsc"></a><span data-ttu-id="2e7eb-103">Komma igång med Azure Automation DSC</span><span class="sxs-lookup"><span data-stu-id="2e7eb-103">Getting started with Azure Automation DSC</span></span>
<span data-ttu-id="2e7eb-104">Det här avsnittet beskrivs hur toodo hello vanligaste uppgifterna med Azure Automation önskad tillstånd Configuration (DSC), till exempel att skapa, importera, och kompilering konfigurationer, onboarding för att hantera datorer, och visa rapporter.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-104">This topic explains how toodo hello most common tasks with Azure Automation Desired State Configuration (DSC), such as creating, importing, and compiling configurations, onboarding machines too manage, and viewing reports.</span></span> <span data-ttu-id="2e7eb-105">En översikt över vilka Azure Automation DSC är finns [översikt över Azure Automation DSC](automation-dsc-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2e7eb-105">For an overview of what Azure Automation DSC is, see [Azure Automation DSC Overview](automation-dsc-overview.md).</span></span> <span data-ttu-id="2e7eb-106">DSC-dokumentation finns [Windows PowerShell Desired Configuration översikt över](https://msdn.microsoft.com/PowerShell/dsc/overview).</span><span class="sxs-lookup"><span data-stu-id="2e7eb-106">For DSC documentation, see [Windows PowerShell Desired State Configuration Overview](https://msdn.microsoft.com/PowerShell/dsc/overview).</span></span>

<span data-ttu-id="2e7eb-107">Det här avsnittet innehåller en stegvis guide toousing Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-107">This topic provides a step-by-step guide toousing Azure Automation DSC.</span></span> <span data-ttu-id="2e7eb-108">Om du vill att en exempel-miljö som redan har konfigurerat utan hello stegen som beskrivs i det här avsnittet, kan du använda [hello följande ARM-mallen](https://github.com/azureautomation/automation-packs/tree/master/102-sample-automation-setup).</span><span class="sxs-lookup"><span data-stu-id="2e7eb-108">If you want a sample environment that is already set up without following hello steps described in this topic, you can use [hello following ARM template](https://github.com/azureautomation/automation-packs/tree/master/102-sample-automation-setup).</span></span> <span data-ttu-id="2e7eb-109">Den här mallen ställer in en slutförd Azure Automation DSC-miljö, inklusive en Azure-dator som hanteras av Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-109">This template sets up a completed Azure Automation DSC environment, including an Azure VM that is managed by Azure Automation DSC.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2e7eb-110">Krav</span><span class="sxs-lookup"><span data-stu-id="2e7eb-110">Prerequisites</span></span>
<span data-ttu-id="2e7eb-111">toocomplete hello exemplen i det här avsnittet, hello följande krävs:</span><span class="sxs-lookup"><span data-stu-id="2e7eb-111">toocomplete hello examples in this topic, hello following are required:</span></span>

* <span data-ttu-id="2e7eb-112">Ett Azure Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-112">An Azure Automation account.</span></span> <span data-ttu-id="2e7eb-113">Instruktioner om hur du skapar ett Kör som-konto för Azure Automation finns i [Azure Kör som-konto](automation-sec-configure-azure-runas-account.md).</span><span class="sxs-lookup"><span data-stu-id="2e7eb-113">For instructions on creating an Azure Automation Run As account, see [Azure Run As Account](automation-sec-configure-azure-runas-account.md).</span></span>
* <span data-ttu-id="2e7eb-114">En Azure Resource Manager VM (inte klassiskt) kör Windows Server 2008 R2 eller senare.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-114">An Azure Resource Manager VM (not Classic) running Windows Server 2008 R2 or later.</span></span> <span data-ttu-id="2e7eb-115">Anvisningar om hur du skapar en virtuell dator finns [skapa din första Windows virtuell dator i hello Azure-portalen](../virtual-machines/virtual-machines-windows-hero-tutorial.md)</span><span class="sxs-lookup"><span data-stu-id="2e7eb-115">For instructions on creating a VM, see [Create your first Windows virtual machine in hello Azure portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md)</span></span>

## <a name="creating-a-dsc-configuration"></a><span data-ttu-id="2e7eb-116">Skapa en DSC-konfiguration</span><span class="sxs-lookup"><span data-stu-id="2e7eb-116">Creating a DSC configuration</span></span>
<span data-ttu-id="2e7eb-117">Vi skapar en enkel [DSC-konfigurationen](https://msdn.microsoft.com/powershell/dsc/configurations) som säkerställer hello förekomsten eller frånvaron av hello **Web Server** Windows funktionen (IIS), beroende på hur du tilldelar noder.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-117">We will create a simple [DSC configuration](https://msdn.microsoft.com/powershell/dsc/configurations) that ensures either hello presence or absence of hello **Web-Server** Windows Feature (IIS), depending on how you assign nodes.</span></span>

1. <span data-ttu-id="2e7eb-118">Starta hello Windows PowerShell ISE (eller valfri textredigerare).</span><span class="sxs-lookup"><span data-stu-id="2e7eb-118">Start hello Windows PowerShell ISE (or any text editor).</span></span>
2. <span data-ttu-id="2e7eb-119">Skriv hello följande text:</span><span class="sxs-lookup"><span data-stu-id="2e7eb-119">Type hello following text:</span></span>
   
    ```powershell
    configuration TestConfig
    {
        Node WebServer
        {
            WindowsFeature IIS
            {
                Ensure               = 'Present'
                Name                 = 'Web-Server'
                IncludeAllSubFeature = $true
   
            }
        }
   
        Node NotWebServer
        {
            WindowsFeature IIS
            {
                Ensure               = 'Absent'
                Name                 = 'Web-Server'
   
            }
        }
        }
    ```
3. <span data-ttu-id="2e7eb-120">Spara hello som `TestConfig.ps1`.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-120">Save hello file as `TestConfig.ps1`.</span></span>

<span data-ttu-id="2e7eb-121">Den här konfigurationen anropar en resurs i varje nod i block hello [WindowsFeature resurs](https://msdn.microsoft.com/powershell/dsc/windowsfeatureresource), som säkerställer hello förekomsten eller frånvaron av hello **Web Server** funktion.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-121">This configuration calls one resource in each node block, hello [WindowsFeature resource](https://msdn.microsoft.com/powershell/dsc/windowsfeatureresource), that ensures either hello presence or absence of hello **Web-Server** feature.</span></span>

## <a name="importing-a-configuration-into-azure-automation"></a><span data-ttu-id="2e7eb-122">Importera en konfiguration i Azure Automation</span><span class="sxs-lookup"><span data-stu-id="2e7eb-122">Importing a configuration into Azure Automation</span></span>
<span data-ttu-id="2e7eb-123">Vi kommer därefter importera hello konfiguration till hello Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-123">Next, we'll import hello configuration into hello Automation account.</span></span>

1. <span data-ttu-id="2e7eb-124">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="2e7eb-124">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="2e7eb-125">Hej hubbmenyn, klicka på **alla resurser** och sedan hello namnet på ditt Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-125">On hello Hub menu, click **All resources** and then hello name of your Automation account.</span></span>
3. <span data-ttu-id="2e7eb-126">På hello **automatiseringskontot** bladet, klickar du på **DSC-konfigurationer**.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-126">On hello **Automation account** blade, click **DSC Configurations**.</span></span>
4. <span data-ttu-id="2e7eb-127">På hello **DSC-konfigurationer** bladet, klickar du på **lägga till en konfiguration**.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-127">On hello **DSC Configurations** blade, click **Add a configuration**.</span></span>
5. <span data-ttu-id="2e7eb-128">På hello **importkonfigurationen** bladet, bläddra toohello `TestConfig.ps1` fil på din dator.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-128">On hello **Import Configuration** blade, browse toohello `TestConfig.ps1` file on your computer.</span></span>
   
    ![Skärmbild av hello ** importera konfigurationen ** bladet](./media/automation-dsc-getting-started/AddConfig.png)
6. <span data-ttu-id="2e7eb-130">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-130">Click **OK**.</span></span>

## <a name="viewing-a-configuration-in-azure-automation"></a><span data-ttu-id="2e7eb-131">Visa en konfiguration i Azure Automation</span><span class="sxs-lookup"><span data-stu-id="2e7eb-131">Viewing a configuration in Azure Automation</span></span>
<span data-ttu-id="2e7eb-132">När du har importerat en konfiguration, kan du visa den i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-132">After you have imported a configuration, you can view it in hello Azure portal.</span></span>

1. <span data-ttu-id="2e7eb-133">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="2e7eb-133">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="2e7eb-134">Hej hubbmenyn, klicka på **alla resurser** och sedan hello namnet på ditt Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-134">On hello Hub menu, click **All resources** and then hello name of your Automation account.</span></span>
3. <span data-ttu-id="2e7eb-135">På hello **automatiseringskontot** bladet, klickar du på **DSC-konfigurationer**</span><span class="sxs-lookup"><span data-stu-id="2e7eb-135">On hello **Automation account** blade, click **DSC Configurations**</span></span>
4. <span data-ttu-id="2e7eb-136">På hello **DSC-konfigurationer** bladet, klickar du på **TestConfig** (detta är hello namn på hello-konfiguration som du har importerat i hello ovan).</span><span class="sxs-lookup"><span data-stu-id="2e7eb-136">On hello **DSC Configurations** blade, click **TestConfig** (this is hello name of hello configuration you imported in hello previous procedure).</span></span>
5. <span data-ttu-id="2e7eb-137">På hello **TestConfig Configuration** bladet, klickar du på **visa konfigurationskälla**.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-137">On hello **TestConfig Configuration** blade, click **View configuration source**.</span></span>
   
    ![Skärmbild av bladet för hello TestConfig konfiguration](./media/automation-dsc-getting-started/ViewConfigSource.png)
   
    <span data-ttu-id="2e7eb-139">En **TestConfig konfigurationskälla** öppnas bladet visar hello PowerShell-koden för hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-139">A **TestConfig Configuration source** blade opens, displaying hello PowerShell code for hello configuration.</span></span>

## <a name="compiling-a-configuration-in-azure-automation"></a><span data-ttu-id="2e7eb-140">Kompilera en konfiguration i Azure Automation</span><span class="sxs-lookup"><span data-stu-id="2e7eb-140">Compiling a configuration in Azure Automation</span></span>
<span data-ttu-id="2e7eb-141">Innan du kan tillämpa en tillstånd tooa nod, måste en DSC-konfiguration som definierar det aktuella tillståndet vara kompileras till ett eller flera nodkonfigurationer (MOF dokument) och placeras på hello Automation DSC Pull-Server.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-141">Before you can apply a desired state tooa node, a DSC configuration defining that state must be compiled into one or more node configurations (MOF document), and placed on hello Automation DSC Pull Server.</span></span> <span data-ttu-id="2e7eb-142">En mer detaljerad beskrivning av kompilering konfigurationer i Azure Automation DSC, se [kompilering konfigurationer i Azure Automation DSC](automation-dsc-compile.md).</span><span class="sxs-lookup"><span data-stu-id="2e7eb-142">For a more detailed description of compiling configurations in Azure Automation DSC, see [Compiling configurations in Azure Automation DSC](automation-dsc-compile.md).</span></span> <span data-ttu-id="2e7eb-143">Mer information om kompilering konfigurationer finns [DSC-konfigurationer](https://msdn.microsoft.com/PowerShell/DSC/configurations).</span><span class="sxs-lookup"><span data-stu-id="2e7eb-143">For more information about compiling configurations, see [DSC Configurations](https://msdn.microsoft.com/PowerShell/DSC/configurations).</span></span>

1. <span data-ttu-id="2e7eb-144">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="2e7eb-144">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="2e7eb-145">Hej hubbmenyn, klicka på **alla resurser** och sedan hello namnet på ditt Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-145">On hello Hub menu, click **All resources** and then hello name of your Automation account.</span></span>
3. <span data-ttu-id="2e7eb-146">På hello **automatiseringskontot** bladet, klickar du på **DSC-konfigurationer**</span><span class="sxs-lookup"><span data-stu-id="2e7eb-146">On hello **Automation account** blade, click **DSC Configurations**</span></span>
4. <span data-ttu-id="2e7eb-147">På hello **DSC-konfigurationer** bladet, klickar du på **TestConfig** (hello namnet på hello tidigare importerat konfigurationen).</span><span class="sxs-lookup"><span data-stu-id="2e7eb-147">On hello **DSC Configurations** blade, click **TestConfig** (hello name of hello previously imported configuration).</span></span>
5. <span data-ttu-id="2e7eb-148">På hello **TestConfig Configuration** bladet, klickar du på **Kompilera**, och klicka sedan på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-148">On hello **TestConfig Configuration** blade, click **Compile**, and then click **Yes**.</span></span> <span data-ttu-id="2e7eb-149">Detta startar ett jobb för kompilering.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-149">This starts a compilation job.</span></span>
   
    ![Skärmbild av hello TestConfig configuration bladet syntaxmarkering kompilera knappen](./media/automation-dsc-getting-started/CompileConfig.png)

> [!NOTE]
> <span data-ttu-id="2e7eb-151">När du kompilerar en konfiguration i Azure Automation distribuerar automatiskt alla skapade nod MOF-filer toohello pull konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-151">When you compile a configuration in Azure Automation, it automatically deploys any created node configuration MOFs toohello pull server.</span></span>
> 
> 

## <a name="viewing-a-compilation-job"></a><span data-ttu-id="2e7eb-152">Visa en kompileringsjobbet</span><span class="sxs-lookup"><span data-stu-id="2e7eb-152">Viewing a compilation job</span></span>
<span data-ttu-id="2e7eb-153">När du startar en sammanställning kan du visa den i hello **Kompileringsjobb** panelen i hello **Configuration** bladet.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-153">After you start a compilation, you can view it in hello **Compilation jobs** tile in hello **Configuration** blade.</span></span> <span data-ttu-id="2e7eb-154">Hej **Kompileringsjobb** panelen visar för närvarande körs slutfört och misslyckade jobb.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-154">hello **Compilation jobs** tile shows currently running, completed, and failed jobs.</span></span> <span data-ttu-id="2e7eb-155">När du öppnar en sammanställning jobb-bladet visas information om det jobbet inklusive fel eller varningar uppstod, indataparametrar som används i hello konfiguration och kompilering loggar.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-155">When you open a compilation job blade, it shows information about that job including any errors or warnings encountered, input parameters used in hello configuration, and compilation logs.</span></span>

1. <span data-ttu-id="2e7eb-156">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="2e7eb-156">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="2e7eb-157">Hej hubbmenyn, klicka på **alla resurser** och sedan hello namnet på ditt Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-157">On hello Hub menu, click **All resources** and then hello name of your Automation account.</span></span>
3. <span data-ttu-id="2e7eb-158">På hello **automatiseringskontot** bladet, klickar du på **DSC-konfigurationer**.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-158">On hello **Automation account** blade, click **DSC Configurations**.</span></span>
4. <span data-ttu-id="2e7eb-159">På hello **DSC-konfigurationer** bladet, klickar du på **TestConfig** (hello namnet på hello tidigare importerat konfigurationen).</span><span class="sxs-lookup"><span data-stu-id="2e7eb-159">On hello **DSC Configurations** blade, click **TestConfig** (hello name of hello previously imported configuration).</span></span>
5. <span data-ttu-id="2e7eb-160">På hello **Kompileringsjobb** för hello **TestConfig Configuration** bladet, klickar du på någon av hello jobb visas.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-160">On hello **Compilation jobs** tile of hello **TestConfig Configuration** blade, click on any of hello jobs listed.</span></span> <span data-ttu-id="2e7eb-161">En **Kompileringsjobbet** blad öppnas med hello datum då hello kompileringsjobbet startades.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-161">A **Compilation Job** blade opens, labeled with hello date that hello compilation job was started.</span></span>
   
    ![Skärmbild av bladet för hello Kompileringsjobbet](./media/automation-dsc-getting-started/CompilationJob.png)
6. <span data-ttu-id="2e7eb-163">Klicka på panelen i hello **Kompileringsjobbet** bladet toosee ytterligare information om hello jobb.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-163">Click on any tile in hello **Compilation Job** blade toosee further details about hello job.</span></span>

## <a name="viewing-node-configurations"></a><span data-ttu-id="2e7eb-164">Visa nodkonfigurationer</span><span class="sxs-lookup"><span data-stu-id="2e7eb-164">Viewing node configurations</span></span>
<span data-ttu-id="2e7eb-165">Slutförande av en kompileringsjobbet skapar en eller flera nya nodkonfigurationer.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-165">Successful completion of a compilation job creates one or more new node configurations.</span></span> <span data-ttu-id="2e7eb-166">Konfiguration av noden är en MOF-dokument som är distribuerade toohello hämtningsservern och redo toobe hämtas och används av en eller flera noder.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-166">A node configuration is a MOF document that is deployed toohello pull server and ready toobe pulled and applied by one or more nodes.</span></span> <span data-ttu-id="2e7eb-167">Du kan visa hello nodkonfigurationer i ditt Automation-konto i hello **DSC-Nodkonfigurationer** bladet.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-167">You can view hello node configurations in your Automation account in hello **DSC Node Configurations** blade.</span></span> <span data-ttu-id="2e7eb-168">Konfiguration av noden har ett namn med formatet hello *ConfigurationName*. *Nodnamn*.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-168">A node configuration has a name with hello form *ConfigurationName*.*NodeName*.</span></span>

1. <span data-ttu-id="2e7eb-169">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="2e7eb-169">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="2e7eb-170">Hej hubbmenyn, klicka på **alla resurser** och sedan hello namnet på ditt Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-170">On hello Hub menu, click **All resources** and then hello name of your Automation account.</span></span>
3. <span data-ttu-id="2e7eb-171">På hello **automatiseringskontot** bladet, klickar du på **DSC-Nodkonfigurationer**.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-171">On hello **Automation account** blade, click **DSC Node Configurations**.</span></span>
   
    ![Skärmbild av bladet för hello DSC-Nodkonfigurationer](./media/automation-dsc-getting-started/NodeConfigs.png)

## <a name="onboarding-an-azure-vm-for-management-with-azure-automation-dsc"></a><span data-ttu-id="2e7eb-173">Ett Azure virtuella datorn för hantering med Azure Automation DSC</span><span class="sxs-lookup"><span data-stu-id="2e7eb-173">Onboarding an Azure VM for management with Azure Automation DSC</span></span>
<span data-ttu-id="2e7eb-174">Du kan använda Azure Automation DSC toomanage virtuella Azure-datorer (både klassiska och hanteraren för filserverresurser), lokala virtuella datorer, Linux-datorer, AWS virtuella datorer och lokala fysiska datorer.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-174">You can use Azure Automation DSC toomanage Azure VMs (both Classic and Resource Manager), on-premises VMs, Linux machines, AWS VMs, and on-premises physical machines.</span></span> <span data-ttu-id="2e7eb-175">I det här avsnittet beskriver vi hur tooonboard endast Azure Resource Manager virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-175">In this topic, we cover how tooonboard only Azure Resource Manager VMs.</span></span> <span data-ttu-id="2e7eb-176">Information om onboarding finns andra typer av datorer, [Onboarding datorer för hantering av Azure Automation DSC](automation-dsc-onboarding.md).</span><span class="sxs-lookup"><span data-stu-id="2e7eb-176">For information about onboarding other types of machines, see [Onboarding machines for management by Azure Automation DSC](automation-dsc-onboarding.md).</span></span>

### <a name="tooonboard-an-azure-resource-manager-vm-for-management-by-azure-automation-dsc"></a><span data-ttu-id="2e7eb-177">tooonboard en Azure Resource Manager-VM för hantering av Azure Automation DSC</span><span class="sxs-lookup"><span data-stu-id="2e7eb-177">tooonboard an Azure Resource Manager VM for management by Azure Automation DSC</span></span>
1. <span data-ttu-id="2e7eb-178">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="2e7eb-178">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="2e7eb-179">Hej hubbmenyn, klicka på **alla resurser** och sedan hello namnet på ditt Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-179">On hello Hub menu, click **All resources** and then hello name of your Automation account.</span></span>
3. <span data-ttu-id="2e7eb-180">På hello **automatiseringskontot** bladet, klickar du på **DSC-noder**.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-180">On hello **Automation account** blade, click **DSC Nodes**.</span></span>
4. <span data-ttu-id="2e7eb-181">I hello **DSC-noder** bladet, klickar du på **lägga till Azure VM**.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-181">In hello **DSC Nodes** blade, click **Add Azure VM**.</span></span>
   
    ![Skärmbild av hello DSC-noder bladet syntaxmarkering hello lägga till Virtuella Azure-knappen](./media/automation-dsc-getting-started/OnboardVM.png)
5. <span data-ttu-id="2e7eb-183">I hello **lägga till virtuella Azure-datorer** bladet, klickar du på **Välj virtuella datorer tooonboard**.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-183">In hello **Add Azure VMs** blade, click **Select virtual machines tooonboard**.</span></span>
6. <span data-ttu-id="2e7eb-184">I hello **Välj virtuella datorer** bladet välj hello VM som du vill tooonboard och på **OK**.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-184">In hello **Select VMs** blade, select hello VM you want tooonboard, and click **OK**.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="2e7eb-185">Det här måste vara en Azure Resource Manager VM kör Windows Server 2008 R2 eller senare.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-185">This must be an Azure Resource Manager VM running Windows Server 2008 R2 or later.</span></span>
   > 
   > 
7. <span data-ttu-id="2e7eb-186">I hello **lägga till virtuella Azure-datorer** bladet, klickar du på **konfigurera registreringsdata**.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-186">In hello **Add Azure VMs** blade, click **Configure registration data**.</span></span>
8. <span data-ttu-id="2e7eb-187">I hello **registrering** bladet ange hello namn hello nodkonfiguration som du vill tooapply toohello VM i hello **Nodkonfigurationsnamnet** rutan.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-187">In hello **Registration** blade, enter hello name of hello node configuration you want tooapply toohello VM in hello **Node Configuration Name** box.</span></span> <span data-ttu-id="2e7eb-188">Detta måste exakt matcha hello namnet på en nodkonfiguration i hello Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-188">This must exactly match hello name of a node configuration in hello Automation account.</span></span> <span data-ttu-id="2e7eb-189">Att tillhandahålla ett namn i det här läget är valfritt.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-189">Providing a name at this point is optional.</span></span> <span data-ttu-id="2e7eb-190">Du kan ändra hello tilldelade nodkonfiguration efter onboarding hello nod.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-190">You can change hello assigned node configuration after onboarding hello node.</span></span>
   <span data-ttu-id="2e7eb-191">Kontrollera **starta om noden om det behövs**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-191">Check **Reboot Node if Needed**, and then click **OK**.</span></span>
   
    ![Skärmbild av bladet för hello-registrering](./media/automation-dsc-getting-started/RegisterVM.png)
   
    <span data-ttu-id="2e7eb-193">hello nodkonfiguration som du har angett kommer att tillämpas toohello VM med intervall som anges av hello **Konfigurationslägesfrekvens**, och hello VM ska söka efter uppdateringar toohello nodkonfiguration med intervall som anges av hello  **Uppdateringsfrekvens**.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-193">hello node configuration you specified will be applied toohello VM at intervals specified by hello **Configuration Mode Frequency**, and hello VM will check for updates toohello node configuration at intervals specified by hello **Refresh Frequency**.</span></span> <span data-ttu-id="2e7eb-194">Läs mer om hur dessa värden används [konfigurera hello Local Configuration Manager](https://msdn.microsoft.com/PowerShell/DSC/metaConfig).</span><span class="sxs-lookup"><span data-stu-id="2e7eb-194">For more information about how these values are used, see [Configuring hello Local Configuration Manager](https://msdn.microsoft.com/PowerShell/DSC/metaConfig).</span></span>
9. <span data-ttu-id="2e7eb-195">I hello **lägga till virtuella Azure-datorer** bladet, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-195">In hello **Add Azure VMs** blade, click **Create**.</span></span>

<span data-ttu-id="2e7eb-196">Azure startar hello processen för onboarding hello VM.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-196">Azure will start hello process of onboarding hello VM.</span></span> <span data-ttu-id="2e7eb-197">När den är klar hello VM visas i hello **DSC-noder** bladet i hello Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-197">When it is complete, hello VM will show up in hello **DSC Nodes** blade in hello Automation account.</span></span>

## <a name="viewing-hello-list-of-dsc-nodes"></a><span data-ttu-id="2e7eb-198">Visa hello lista över DSC-noder</span><span class="sxs-lookup"><span data-stu-id="2e7eb-198">Viewing hello list of DSC nodes</span></span>
<span data-ttu-id="2e7eb-199">Du kan visa hello lista över alla datorer som har publicerats så för hantering i ditt Automation-konto i hello **DSC-noder** bladet.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-199">You can view hello list of all machines that have been onboarded for management in your Automation account in hello **DSC Nodes** blade.</span></span>

1. <span data-ttu-id="2e7eb-200">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="2e7eb-200">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="2e7eb-201">Hej hubbmenyn, klicka på **alla resurser** och sedan hello namnet på ditt Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-201">On hello Hub menu, click **All resources** and then hello name of your Automation account.</span></span>
3. <span data-ttu-id="2e7eb-202">På hello **automatiseringskontot** bladet, klickar du på **DSC-noder**.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-202">On hello **Automation account** blade, click **DSC Nodes**.</span></span>

## <a name="viewing-reports-for-dsc-nodes"></a><span data-ttu-id="2e7eb-203">Visa rapporter för DSC-noder</span><span class="sxs-lookup"><span data-stu-id="2e7eb-203">Viewing reports for DSC nodes</span></span>
<span data-ttu-id="2e7eb-204">Varje gång som Azure Automation DSC utför en konsekvenskontroll på en hanterad nod skickar hello nod en status tillbaka toohello pull-rapportserver.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-204">Each time Azure Automation DSC performs a consistency check on a managed node, hello node sends a status report back toohello pull server.</span></span> <span data-ttu-id="2e7eb-205">Du kan visa rapporterna på hello bladet för noden.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-205">You can view these reports on hello blade for that node.</span></span>

1. <span data-ttu-id="2e7eb-206">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="2e7eb-206">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="2e7eb-207">Hej hubbmenyn, klicka på **alla resurser** och sedan hello namnet på ditt Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-207">On hello Hub menu, click **All resources** and then hello name of your Automation account.</span></span>
3. <span data-ttu-id="2e7eb-208">På hello **automatiseringskontot** bladet, klickar du på **DSC-noder**.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-208">On hello **Automation account** blade, click **DSC Nodes**.</span></span>
4. <span data-ttu-id="2e7eb-209">På hello **rapporter** panelen, klicka på någon av hello rapporter i hello-listan.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-209">On hello **Reports** tile, click on any of hello reports in hello list.</span></span>
   
    ![Skärmbild av bladet för hello-rapport](./media/automation-dsc-getting-started/NodeReport.png)

<span data-ttu-id="2e7eb-211">På hello bladet för en enskild rapport visas hello efter information om status för hello motsvarande konsekvenskontroll kontrollera:</span><span class="sxs-lookup"><span data-stu-id="2e7eb-211">On hello blade for an individual report, you can see hello following status information for hello corresponding consistency check:</span></span>

* <span data-ttu-id="2e7eb-212">Hej rapportera status – om hello nod är ”kompatibla”, hello configuration ”misslyckades” eller hello noden är ”inkompatibla” (när hello nod är i **applyandmonitor** läge och hello datorn inte är i hello önskad tillstånd).</span><span class="sxs-lookup"><span data-stu-id="2e7eb-212">hello report status — whether hello node is "Compliant", hello configuration "Failed", or hello node is "Not Compliant" (when hello node is in **applyandmonitor** mode and hello machine is not in hello desired state).</span></span>
* <span data-ttu-id="2e7eb-213">hello starttid för hello konsekvenskontroll.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-213">hello start time for hello consistency check.</span></span>
* <span data-ttu-id="2e7eb-214">Kontrollera hello totala körtiden hello konsekvent.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-214">hello total runtime for hello consistency check.</span></span>
* <span data-ttu-id="2e7eb-215">Kontrollera hello typ av konsekvenskontroll.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-215">hello type of consistency check.</span></span>
* <span data-ttu-id="2e7eb-216">Fel, inklusive hello kod och fel visas.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-216">Any errors, including hello error code and error message.</span></span> 
* <span data-ttu-id="2e7eb-217">Alla DSC-resurser som används i hello konfiguration och hello tillståndet för varje resurs (om hello noden är hello önskad status för den här resursen) – du kan klicka på varje resurs tooget mer detaljerad information för den här resursen.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-217">Any DSC resources used in hello configuration, and hello state of each resource (whether hello node is in hello desired state for that resource) — you can click on each resource tooget more detailed information for that resource.</span></span>
* <span data-ttu-id="2e7eb-218">hello namn, IP-adress och konfigurationsläge för hello-nod.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-218">hello name, IP address, and configuration mode of hello node.</span></span>

<span data-ttu-id="2e7eb-219">Du kan också klicka på **Visa rapport om oformaterat** toosee hello faktiska data som hello nod skickar toohello server.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-219">You can also click **View raw report** toosee hello actual data that hello node sends toohello server.</span></span> <span data-ttu-id="2e7eb-220">Mer information om hur du använder dessa data finns [med hjälp av en rapportserver för DSC](https://msdn.microsoft.com/powershell/dsc/reportserver).</span><span class="sxs-lookup"><span data-stu-id="2e7eb-220">For more information about using that data, see [Using a DSC report server](https://msdn.microsoft.com/powershell/dsc/reportserver).</span></span>

<span data-ttu-id="2e7eb-221">Det kan ta lite tid när en nod har publicerats så innan hello första rapporten är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-221">It can take some time after a node is onboarded before hello first report is available.</span></span> <span data-ttu-id="2e7eb-222">Du kan behöva toowait in too30 minuter för hello första rapporten efter att du publicera en nod.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-222">You might need toowait up too30 minutes for hello first report after you onboard a node.</span></span>

## <a name="reassigning-a-node-tooa-different-node-configuration"></a><span data-ttu-id="2e7eb-223">Tilldela en annan nodkonfiguration av noden tooa</span><span class="sxs-lookup"><span data-stu-id="2e7eb-223">Reassigning a node tooa different node configuration</span></span>
<span data-ttu-id="2e7eb-224">Du kan tilldela en nod toouse en annan nodkonfiguration än hello en som du ursprungligen har tilldelats.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-224">You can assign a node toouse a different node configuration than hello one you initially assigned.</span></span>

1. <span data-ttu-id="2e7eb-225">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="2e7eb-225">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="2e7eb-226">Hej hubbmenyn, klicka på **alla resurser** och sedan hello namnet på ditt Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-226">On hello Hub menu, click **All resources** and then hello name of your Automation account.</span></span>
3. <span data-ttu-id="2e7eb-227">På hello **automatiseringskontot** bladet, klickar du på **DSC-noder**.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-227">On hello **Automation account** blade, click **DSC Nodes**.</span></span>
4. <span data-ttu-id="2e7eb-228">På hello **DSC-noder** bladet, klickar du på namnet för hello hello nod som du vill tooreassign.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-228">On hello **DSC Nodes** blade, click on hello name of hello node you want tooreassign.</span></span>
5. <span data-ttu-id="2e7eb-229">Klicka på hello bladet för noden **tilldela noden**.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-229">On hello blade for that node, click **Assign node**.</span></span>
   
    ![Skärmbild av hello nod bladet syntaxmarkering hello tilldela nod knappen](./media/automation-dsc-getting-started/AssignNode.png)
6. <span data-ttu-id="2e7eb-231">På hello **tilldela nodkonfiguration** bladet, Välj hello nod configuration toowhich du vill tooassign hello nod och klickar sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-231">On hello **Assign Node Configuration** blade, select hello node configuration toowhich you want tooassign hello node, and then click **OK**.</span></span>
   
    ![Skärmbild av bladet för hello tilldela nodkonfiguration](./media/automation-dsc-getting-started/AssignNodeConfig.png)

## <a name="unregistering-a-node"></a><span data-ttu-id="2e7eb-233">Avregistrerar en nod</span><span class="sxs-lookup"><span data-stu-id="2e7eb-233">Unregistering a node</span></span>
<span data-ttu-id="2e7eb-234">Om du inte längre vill att en nod toobe som hanteras av Azure Automation DSC kan du avregistrera det.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-234">If you no longer want a node toobe managed by Azure Automation DSC, you can unregister it.</span></span>

1. <span data-ttu-id="2e7eb-235">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="2e7eb-235">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="2e7eb-236">Hej hubbmenyn, klicka på **alla resurser** och sedan hello namnet på ditt Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-236">On hello Hub menu, click **All resources** and then hello name of your Automation account.</span></span>
3. <span data-ttu-id="2e7eb-237">På hello **automatiseringskontot** bladet, klickar du på **DSC-noder**.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-237">On hello **Automation account** blade, click **DSC Nodes**.</span></span>
4. <span data-ttu-id="2e7eb-238">På hello **DSC-noder** bladet, klickar du på namnet för hello hello nod som du vill toounregister.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-238">On hello **DSC Nodes** blade, click on hello name of hello node you want toounregister.</span></span>
5. <span data-ttu-id="2e7eb-239">Klicka på hello bladet för noden **avregistrera**.</span><span class="sxs-lookup"><span data-stu-id="2e7eb-239">On hello blade for that node, click **Unregister**.</span></span>
   
    ![Skärmbild av hello nod bladet syntaxmarkering hello Unregister-knappen](./media/automation-dsc-getting-started/UnregisterNode.png)

## <a name="related-articles"></a><span data-ttu-id="2e7eb-241">Relaterade artiklar</span><span class="sxs-lookup"><span data-stu-id="2e7eb-241">Related Articles</span></span>
* [<span data-ttu-id="2e7eb-242">Översikt över Azure Automation DSC</span><span class="sxs-lookup"><span data-stu-id="2e7eb-242">Azure Automation DSC overview</span></span>](automation-dsc-overview.md)
* [<span data-ttu-id="2e7eb-243">Onboarding-datorer för hantering av Azure Automation DSC</span><span class="sxs-lookup"><span data-stu-id="2e7eb-243">Onboarding machines for management by Azure Automation DSC</span></span>](automation-dsc-onboarding.md)
* [<span data-ttu-id="2e7eb-244">Windows PowerShell Desired State Configuration-översikt</span><span class="sxs-lookup"><span data-stu-id="2e7eb-244">Windows PowerShell Desired State Configuration Overview</span></span>](https://msdn.microsoft.com/powershell/dsc/overview)
* [<span data-ttu-id="2e7eb-245">Azure Automation DSC-cmdlets</span><span class="sxs-lookup"><span data-stu-id="2e7eb-245">Azure Automation DSC cmdlets</span></span>](/powershell/module/azurerm.automation/#automation)
* [<span data-ttu-id="2e7eb-246">Priser för Azure Automation DSC</span><span class="sxs-lookup"><span data-stu-id="2e7eb-246">Azure Automation DSC pricing</span></span>](https://azure.microsoft.com/pricing/details/automation/)

