---
title: "Komma igång med Azure Automation DSC | Microsoft Docs"
description: "Förklaring och exempel på de vanligaste uppgifterna i Azure Automation önskad tillstånd Configuration (DSC)"
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
ms.openlocfilehash: 8a10d961ad7c107c68b57c64ee6c88544ff8832b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="getting-started-with-azure-automation-dsc"></a><span data-ttu-id="01da1-103">Komma igång med Azure Automation DSC</span><span class="sxs-lookup"><span data-stu-id="01da1-103">Getting started with Azure Automation DSC</span></span>
<span data-ttu-id="01da1-104">Det här avsnittet beskriver hur du utför de vanligaste uppgifterna med Azure Automation önskad tillstånd Configuration (DSC), till exempel att skapa, importera, och kompilering konfigurationer, onboarding datorer för att hantera, och visa rapporter.</span><span class="sxs-lookup"><span data-stu-id="01da1-104">This topic explains how to do the most common tasks with Azure Automation Desired State Configuration (DSC), such as creating, importing, and compiling configurations, onboarding machines to manage, and viewing reports.</span></span> <span data-ttu-id="01da1-105">En översikt över vilka Azure Automation DSC är finns [översikt över Azure Automation DSC](automation-dsc-overview.md).</span><span class="sxs-lookup"><span data-stu-id="01da1-105">For an overview of what Azure Automation DSC is, see [Azure Automation DSC Overview](automation-dsc-overview.md).</span></span> <span data-ttu-id="01da1-106">DSC-dokumentation finns [Windows PowerShell Desired Configuration översikt över](https://msdn.microsoft.com/PowerShell/dsc/overview).</span><span class="sxs-lookup"><span data-stu-id="01da1-106">For DSC documentation, see [Windows PowerShell Desired State Configuration Overview](https://msdn.microsoft.com/PowerShell/dsc/overview).</span></span>

<span data-ttu-id="01da1-107">Det här avsnittet innehåller stegvisa instruktioner till med hjälp av Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="01da1-107">This topic provides a step-by-step guide to using Azure Automation DSC.</span></span> <span data-ttu-id="01da1-108">Om du vill att en exempel-miljö som redan har konfigurerat utan att följa stegen som beskrivs i det här avsnittet, kan du använda [följande ARM-mallen](https://github.com/azureautomation/automation-packs/tree/master/102-sample-automation-setup).</span><span class="sxs-lookup"><span data-stu-id="01da1-108">If you want a sample environment that is already set up without following the steps described in this topic, you can use [the following ARM template](https://github.com/azureautomation/automation-packs/tree/master/102-sample-automation-setup).</span></span> <span data-ttu-id="01da1-109">Den här mallen ställer in en slutförd Azure Automation DSC-miljö, inklusive en Azure-dator som hanteras av Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="01da1-109">This template sets up a completed Azure Automation DSC environment, including an Azure VM that is managed by Azure Automation DSC.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="01da1-110">Krav</span><span class="sxs-lookup"><span data-stu-id="01da1-110">Prerequisites</span></span>
<span data-ttu-id="01da1-111">Om du vill utföra exemplen i det här avsnittet, krävs följande:</span><span class="sxs-lookup"><span data-stu-id="01da1-111">To complete the examples in this topic, the following are required:</span></span>

* <span data-ttu-id="01da1-112">Ett Azure Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="01da1-112">An Azure Automation account.</span></span> <span data-ttu-id="01da1-113">Instruktioner om hur du skapar ett Azure Automation kör som-konto finns i [Azure kör som-konto](automation-sec-configure-azure-runas-account.md).</span><span class="sxs-lookup"><span data-stu-id="01da1-113">For instructions on creating an Azure Automation Run As account, see [Azure Run As Account](automation-sec-configure-azure-runas-account.md).</span></span>
* <span data-ttu-id="01da1-114">En Azure Resource Manager VM (inte klassiskt) kör Windows Server 2008 R2 eller senare.</span><span class="sxs-lookup"><span data-stu-id="01da1-114">An Azure Resource Manager VM (not Classic) running Windows Server 2008 R2 or later.</span></span> <span data-ttu-id="01da1-115">Anvisningar om hur du skapar en virtuell dator finns [skapa din första Windows-dator i Azure-portalen](../virtual-machines/virtual-machines-windows-hero-tutorial.md)</span><span class="sxs-lookup"><span data-stu-id="01da1-115">For instructions on creating a VM, see [Create your first Windows virtual machine in the Azure portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md)</span></span>

## <a name="creating-a-dsc-configuration"></a><span data-ttu-id="01da1-116">Skapa en DSC-konfiguration</span><span class="sxs-lookup"><span data-stu-id="01da1-116">Creating a DSC configuration</span></span>
<span data-ttu-id="01da1-117">Vi skapar en enkel [DSC-konfigurationen](https://msdn.microsoft.com/powershell/dsc/configurations) som säkerställer förekomsten eller frånvaron av den **Web Server** Windows funktionen (IIS), beroende på hur du tilldelar noder.</span><span class="sxs-lookup"><span data-stu-id="01da1-117">We will create a simple [DSC configuration](https://msdn.microsoft.com/powershell/dsc/configurations) that ensures either the presence or absence of the **Web-Server** Windows Feature (IIS), depending on how you assign nodes.</span></span>

1. <span data-ttu-id="01da1-118">Starta Windows PowerShell ISE (eller valfri textredigerare).</span><span class="sxs-lookup"><span data-stu-id="01da1-118">Start the Windows PowerShell ISE (or any text editor).</span></span>
2. <span data-ttu-id="01da1-119">Skriv följande text:</span><span class="sxs-lookup"><span data-stu-id="01da1-119">Type the following text:</span></span>
   
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
3. <span data-ttu-id="01da1-120">Spara filen som `TestConfig.ps1`.</span><span class="sxs-lookup"><span data-stu-id="01da1-120">Save the file as `TestConfig.ps1`.</span></span>

<span data-ttu-id="01da1-121">Den här konfigurationen anropar en resurs i varje nod i block i [WindowsFeature resurs](https://msdn.microsoft.com/powershell/dsc/windowsfeatureresource), som säkerställer förekomsten eller frånvaron av den **Web Server** funktion.</span><span class="sxs-lookup"><span data-stu-id="01da1-121">This configuration calls one resource in each node block, the [WindowsFeature resource](https://msdn.microsoft.com/powershell/dsc/windowsfeatureresource), that ensures either the presence or absence of the **Web-Server** feature.</span></span>

## <a name="importing-a-configuration-into-azure-automation"></a><span data-ttu-id="01da1-122">Importera en konfiguration i Azure Automation</span><span class="sxs-lookup"><span data-stu-id="01da1-122">Importing a configuration into Azure Automation</span></span>
<span data-ttu-id="01da1-123">Vi kan sedan importera konfigurationen till Automation-kontot.</span><span class="sxs-lookup"><span data-stu-id="01da1-123">Next, we'll import the configuration into the Automation account.</span></span>

1. <span data-ttu-id="01da1-124">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="01da1-124">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="01da1-125">På navmenyn klickar du på **alla resurser** och sedan namnet på ditt Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="01da1-125">On the Hub menu, click **All resources** and then the name of your Automation account.</span></span>
3. <span data-ttu-id="01da1-126">På den **automatiseringskontot** bladet, klickar du på **DSC-konfigurationer**.</span><span class="sxs-lookup"><span data-stu-id="01da1-126">On the **Automation account** blade, click **DSC Configurations**.</span></span>
4. <span data-ttu-id="01da1-127">På den **DSC-konfigurationer** bladet, klickar du på **lägga till en konfiguration**.</span><span class="sxs-lookup"><span data-stu-id="01da1-127">On the **DSC Configurations** blade, click **Add a configuration**.</span></span>
5. <span data-ttu-id="01da1-128">På den **importkonfigurationen** bladet, bläddra till den `TestConfig.ps1` filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="01da1-128">On the **Import Configuration** blade, browse to the `TestConfig.ps1` file on your computer.</span></span>
   
    ![Skärmbild av den ** importera konfigurationen ** bladet](./media/automation-dsc-getting-started/AddConfig.png)
6. <span data-ttu-id="01da1-130">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="01da1-130">Click **OK**.</span></span>

## <a name="viewing-a-configuration-in-azure-automation"></a><span data-ttu-id="01da1-131">Visa en konfiguration i Azure Automation</span><span class="sxs-lookup"><span data-stu-id="01da1-131">Viewing a configuration in Azure Automation</span></span>
<span data-ttu-id="01da1-132">När du har importerat en konfiguration, kan du visa den i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="01da1-132">After you have imported a configuration, you can view it in the Azure portal.</span></span>

1. <span data-ttu-id="01da1-133">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="01da1-133">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="01da1-134">På navmenyn klickar du på **alla resurser** och sedan namnet på ditt Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="01da1-134">On the Hub menu, click **All resources** and then the name of your Automation account.</span></span>
3. <span data-ttu-id="01da1-135">På den **automatiseringskontot** bladet, klickar du på **DSC-konfigurationer**</span><span class="sxs-lookup"><span data-stu-id="01da1-135">On the **Automation account** blade, click **DSC Configurations**</span></span>
4. <span data-ttu-id="01da1-136">På den **DSC-konfigurationer** bladet, klickar du på **TestConfig** (detta är namnet på den konfiguration som du har importerat i föregående procedur).</span><span class="sxs-lookup"><span data-stu-id="01da1-136">On the **DSC Configurations** blade, click **TestConfig** (this is the name of the configuration you imported in the previous procedure).</span></span>
5. <span data-ttu-id="01da1-137">På den **TestConfig Configuration** bladet, klickar du på **visa konfigurationskälla**.</span><span class="sxs-lookup"><span data-stu-id="01da1-137">On the **TestConfig Configuration** blade, click **View configuration source**.</span></span>
   
    ![Skärmbild av bladet TestConfig konfiguration](./media/automation-dsc-getting-started/ViewConfigSource.png)
   
    <span data-ttu-id="01da1-139">En **TestConfig konfigurationskälla** öppnas bladet med PowerShell-koden för konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="01da1-139">A **TestConfig Configuration source** blade opens, displaying the PowerShell code for the configuration.</span></span>

## <a name="compiling-a-configuration-in-azure-automation"></a><span data-ttu-id="01da1-140">Kompilera en konfiguration i Azure Automation</span><span class="sxs-lookup"><span data-stu-id="01da1-140">Compiling a configuration in Azure Automation</span></span>
<span data-ttu-id="01da1-141">Innan du kan tillämpa en tillstånd till en nod, måste en DSC-konfiguration som definierar det aktuella tillståndet vara kompileras till ett eller flera nodkonfigurationer (MOF dokument) och placeras i Automation DSC Pull-Server.</span><span class="sxs-lookup"><span data-stu-id="01da1-141">Before you can apply a desired state to a node, a DSC configuration defining that state must be compiled into one or more node configurations (MOF document), and placed on the Automation DSC Pull Server.</span></span> <span data-ttu-id="01da1-142">En mer detaljerad beskrivning av kompilering konfigurationer i Azure Automation DSC, se [kompilering konfigurationer i Azure Automation DSC](automation-dsc-compile.md).</span><span class="sxs-lookup"><span data-stu-id="01da1-142">For a more detailed description of compiling configurations in Azure Automation DSC, see [Compiling configurations in Azure Automation DSC](automation-dsc-compile.md).</span></span> <span data-ttu-id="01da1-143">Mer information om kompilering konfigurationer finns [DSC-konfigurationer](https://msdn.microsoft.com/PowerShell/DSC/configurations).</span><span class="sxs-lookup"><span data-stu-id="01da1-143">For more information about compiling configurations, see [DSC Configurations](https://msdn.microsoft.com/PowerShell/DSC/configurations).</span></span>

1. <span data-ttu-id="01da1-144">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="01da1-144">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="01da1-145">På navmenyn klickar du på **alla resurser** och sedan namnet på ditt Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="01da1-145">On the Hub menu, click **All resources** and then the name of your Automation account.</span></span>
3. <span data-ttu-id="01da1-146">På den **automatiseringskontot** bladet, klickar du på **DSC-konfigurationer**</span><span class="sxs-lookup"><span data-stu-id="01da1-146">On the **Automation account** blade, click **DSC Configurations**</span></span>
4. <span data-ttu-id="01da1-147">På den **DSC-konfigurationer** bladet, klickar du på **TestConfig** (namnet på den tidigare importerat konfigurationen).</span><span class="sxs-lookup"><span data-stu-id="01da1-147">On the **DSC Configurations** blade, click **TestConfig** (the name of the previously imported configuration).</span></span>
5. <span data-ttu-id="01da1-148">På den **TestConfig Configuration** bladet, klickar du på **Kompilera**, och klicka sedan på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="01da1-148">On the **TestConfig Configuration** blade, click **Compile**, and then click **Yes**.</span></span> <span data-ttu-id="01da1-149">Detta startar ett jobb för kompilering.</span><span class="sxs-lookup"><span data-stu-id="01da1-149">This starts a compilation job.</span></span>
   
    ![Skärmbild av bladet TestConfig configuration syntaxmarkering kompilera knappen](./media/automation-dsc-getting-started/CompileConfig.png)

> [!NOTE]
> <span data-ttu-id="01da1-151">När du kompilerar en konfiguration i Azure Automation distribuerar automatiskt alla skapade nodkonfiguration MOF-filer till den pull-servern.</span><span class="sxs-lookup"><span data-stu-id="01da1-151">When you compile a configuration in Azure Automation, it automatically deploys any created node configuration MOFs to the pull server.</span></span>
> 
> 

## <a name="viewing-a-compilation-job"></a><span data-ttu-id="01da1-152">Visa en kompileringsjobbet</span><span class="sxs-lookup"><span data-stu-id="01da1-152">Viewing a compilation job</span></span>
<span data-ttu-id="01da1-153">När du startar en sammanställning, du kan visa den i den **Kompileringsjobb** panelen i den **Configuration** bladet.</span><span class="sxs-lookup"><span data-stu-id="01da1-153">After you start a compilation, you can view it in the **Compilation jobs** tile in the **Configuration** blade.</span></span> <span data-ttu-id="01da1-154">Den **Kompileringsjobb** panelen visar för närvarande körs slutfört och misslyckade jobb.</span><span class="sxs-lookup"><span data-stu-id="01da1-154">The **Compilation jobs** tile shows currently running, completed, and failed jobs.</span></span> <span data-ttu-id="01da1-155">När du öppnar en sammanställning jobb-bladet visas information om det jobbet inklusive fel eller varningar uppstod, indataparametrar som används i konfigurationen och kompilering loggar.</span><span class="sxs-lookup"><span data-stu-id="01da1-155">When you open a compilation job blade, it shows information about that job including any errors or warnings encountered, input parameters used in the configuration, and compilation logs.</span></span>

1. <span data-ttu-id="01da1-156">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="01da1-156">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="01da1-157">På navmenyn klickar du på **alla resurser** och sedan namnet på ditt Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="01da1-157">On the Hub menu, click **All resources** and then the name of your Automation account.</span></span>
3. <span data-ttu-id="01da1-158">På den **automatiseringskontot** bladet, klickar du på **DSC-konfigurationer**.</span><span class="sxs-lookup"><span data-stu-id="01da1-158">On the **Automation account** blade, click **DSC Configurations**.</span></span>
4. <span data-ttu-id="01da1-159">På den **DSC-konfigurationer** bladet, klickar du på **TestConfig** (namnet på den tidigare importerat konfigurationen).</span><span class="sxs-lookup"><span data-stu-id="01da1-159">On the **DSC Configurations** blade, click **TestConfig** (the name of the previously imported configuration).</span></span>
5. <span data-ttu-id="01da1-160">På den **Kompileringsjobb** panelen av den **TestConfig Configuration** bladet, klickar du på någon av de jobb som visas.</span><span class="sxs-lookup"><span data-stu-id="01da1-160">On the **Compilation jobs** tile of the **TestConfig Configuration** blade, click on any of the jobs listed.</span></span> <span data-ttu-id="01da1-161">En **Kompileringsjobbet** blad öppnas med det datum då kompileringsjobbet startades.</span><span class="sxs-lookup"><span data-stu-id="01da1-161">A **Compilation Job** blade opens, labeled with the date that the compilation job was started.</span></span>
   
    ![Skärmbild av bladet Kompileringsjobbet](./media/automation-dsc-getting-started/CompilationJob.png)
6. <span data-ttu-id="01da1-163">Klicka på panelen i den **Kompileringsjobbet** bladet för att se ytterligare information om jobbet.</span><span class="sxs-lookup"><span data-stu-id="01da1-163">Click on any tile in the **Compilation Job** blade to see further details about the job.</span></span>

## <a name="viewing-node-configurations"></a><span data-ttu-id="01da1-164">Visa nodkonfigurationer</span><span class="sxs-lookup"><span data-stu-id="01da1-164">Viewing node configurations</span></span>
<span data-ttu-id="01da1-165">Slutförande av en kompileringsjobbet skapar en eller flera nya nodkonfigurationer.</span><span class="sxs-lookup"><span data-stu-id="01da1-165">Successful completion of a compilation job creates one or more new node configurations.</span></span> <span data-ttu-id="01da1-166">Konfiguration av noden är en MOF-dokument som har distribuerats till den pull-servern och redo att dras in och används av en eller flera noder.</span><span class="sxs-lookup"><span data-stu-id="01da1-166">A node configuration is a MOF document that is deployed to the pull server and ready to be pulled and applied by one or more nodes.</span></span> <span data-ttu-id="01da1-167">Du kan visa nodkonfigurationer i ditt Automation-konto i den **DSC-Nodkonfigurationer** bladet.</span><span class="sxs-lookup"><span data-stu-id="01da1-167">You can view the node configurations in your Automation account in the **DSC Node Configurations** blade.</span></span> <span data-ttu-id="01da1-168">Konfiguration av noden har ett namn med formatet *ConfigurationName*. *Nodnamn*.</span><span class="sxs-lookup"><span data-stu-id="01da1-168">A node configuration has a name with the form *ConfigurationName*.*NodeName*.</span></span>

1. <span data-ttu-id="01da1-169">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="01da1-169">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="01da1-170">På navmenyn klickar du på **alla resurser** och sedan namnet på ditt Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="01da1-170">On the Hub menu, click **All resources** and then the name of your Automation account.</span></span>
3. <span data-ttu-id="01da1-171">På den **automatiseringskontot** bladet, klickar du på **DSC-Nodkonfigurationer**.</span><span class="sxs-lookup"><span data-stu-id="01da1-171">On the **Automation account** blade, click **DSC Node Configurations**.</span></span>
   
    ![Skärmbild av bladet DSC-Nodkonfigurationer](./media/automation-dsc-getting-started/NodeConfigs.png)

## <a name="onboarding-an-azure-vm-for-management-with-azure-automation-dsc"></a><span data-ttu-id="01da1-173">Ett Azure virtuella datorn för hantering med Azure Automation DSC</span><span class="sxs-lookup"><span data-stu-id="01da1-173">Onboarding an Azure VM for management with Azure Automation DSC</span></span>
<span data-ttu-id="01da1-174">Du kan använda Azure Automation DSC för att hantera virtuella Azure-datorer (både klassiska och hanteraren för filserverresurser), lokala virtuella datorer, Linux-datorer, AWS virtuella datorer och lokala fysiska datorer.</span><span class="sxs-lookup"><span data-stu-id="01da1-174">You can use Azure Automation DSC to manage Azure VMs (both Classic and Resource Manager), on-premises VMs, Linux machines, AWS VMs, and on-premises physical machines.</span></span> <span data-ttu-id="01da1-175">I det här avsnittet beskriver vi hur du publicera endast Azure Resource Manager virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="01da1-175">In this topic, we cover how to onboard only Azure Resource Manager VMs.</span></span> <span data-ttu-id="01da1-176">Information om onboarding finns andra typer av datorer, [Onboarding datorer för hantering av Azure Automation DSC](automation-dsc-onboarding.md).</span><span class="sxs-lookup"><span data-stu-id="01da1-176">For information about onboarding other types of machines, see [Onboarding machines for management by Azure Automation DSC](automation-dsc-onboarding.md).</span></span>

### <a name="to-onboard-an-azure-resource-manager-vm-for-management-by-azure-automation-dsc"></a><span data-ttu-id="01da1-177">Publicera en Azure Resource Manager-VM för hantering av Azure Automation DSC</span><span class="sxs-lookup"><span data-stu-id="01da1-177">To onboard an Azure Resource Manager VM for management by Azure Automation DSC</span></span>
1. <span data-ttu-id="01da1-178">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="01da1-178">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="01da1-179">På navmenyn klickar du på **alla resurser** och sedan namnet på ditt Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="01da1-179">On the Hub menu, click **All resources** and then the name of your Automation account.</span></span>
3. <span data-ttu-id="01da1-180">På den **automatiseringskontot** bladet, klickar du på **DSC-noder**.</span><span class="sxs-lookup"><span data-stu-id="01da1-180">On the **Automation account** blade, click **DSC Nodes**.</span></span>
4. <span data-ttu-id="01da1-181">I den **DSC-noder** bladet, klickar du på **lägga till Azure VM**.</span><span class="sxs-lookup"><span data-stu-id="01da1-181">In the **DSC Nodes** blade, click **Add Azure VM**.</span></span>
   
    ![Skärmbild av bladet DSC-noder Markera knappen Lägg till Azure VM](./media/automation-dsc-getting-started/OnboardVM.png)
5. <span data-ttu-id="01da1-183">I den **lägga till virtuella Azure-datorer** bladet, klickar du på **Välj virtuella datorer som ska publiceras**.</span><span class="sxs-lookup"><span data-stu-id="01da1-183">In the **Add Azure VMs** blade, click **Select virtual machines to onboard**.</span></span>
6. <span data-ttu-id="01da1-184">I den **Välj virtuella datorer** bladet, Välj den virtuella datorn som du vill publicera och klickar på **OK**.</span><span class="sxs-lookup"><span data-stu-id="01da1-184">In the **Select VMs** blade, select the VM you want to onboard, and click **OK**.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="01da1-185">Det här måste vara en Azure Resource Manager VM kör Windows Server 2008 R2 eller senare.</span><span class="sxs-lookup"><span data-stu-id="01da1-185">This must be an Azure Resource Manager VM running Windows Server 2008 R2 or later.</span></span>
   > 
   > 
7. <span data-ttu-id="01da1-186">I den **lägga till virtuella Azure-datorer** bladet, klickar du på **konfigurera registreringsdata**.</span><span class="sxs-lookup"><span data-stu-id="01da1-186">In the **Add Azure VMs** blade, click **Configure registration data**.</span></span>
8. <span data-ttu-id="01da1-187">I den **registrering** bladet anger du namnet på nodkonfigurationen som du vill koppla till den virtuella datorn i den **Nodkonfigurationsnamnet** rutan.</span><span class="sxs-lookup"><span data-stu-id="01da1-187">In the **Registration** blade, enter the name of the node configuration you want to apply to the VM in the **Node Configuration Name** box.</span></span> <span data-ttu-id="01da1-188">Detta måste exakt matcha namnet på en nodkonfiguration i Automation-kontot.</span><span class="sxs-lookup"><span data-stu-id="01da1-188">This must exactly match the name of a node configuration in the Automation account.</span></span> <span data-ttu-id="01da1-189">Att tillhandahålla ett namn i det här läget är valfritt.</span><span class="sxs-lookup"><span data-stu-id="01da1-189">Providing a name at this point is optional.</span></span> <span data-ttu-id="01da1-190">Du kan ändra den tilldelade nodkonfigurationen efter onboarding noden.</span><span class="sxs-lookup"><span data-stu-id="01da1-190">You can change the assigned node configuration after onboarding the node.</span></span>
   <span data-ttu-id="01da1-191">Kontrollera **starta om noden om det behövs**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="01da1-191">Check **Reboot Node if Needed**, and then click **OK**.</span></span>
   
    ![Skärmbild av bladet registrering](./media/automation-dsc-getting-started/RegisterVM.png)
   
    <span data-ttu-id="01da1-193">Nodkonfiguration som du angett tillämpas på den virtuella datorn med intervall som anges av den **Konfigurationslägesfrekvens**, och den virtuella datorn kommer att söka efter uppdateringar till nodkonfiguration med intervall som anges av den **uppdateringsfrekvens**.</span><span class="sxs-lookup"><span data-stu-id="01da1-193">The node configuration you specified will be applied to the VM at intervals specified by the **Configuration Mode Frequency**, and the VM will check for updates to the node configuration at intervals specified by the **Refresh Frequency**.</span></span> <span data-ttu-id="01da1-194">Läs mer om hur dessa värden används [konfigurera den lokala Configuration Manager](https://msdn.microsoft.com/PowerShell/DSC/metaConfig).</span><span class="sxs-lookup"><span data-stu-id="01da1-194">For more information about how these values are used, see [Configuring the Local Configuration Manager](https://msdn.microsoft.com/PowerShell/DSC/metaConfig).</span></span>
9. <span data-ttu-id="01da1-195">I den **lägga till virtuella Azure-datorer** bladet, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="01da1-195">In the **Add Azure VMs** blade, click **Create**.</span></span>

<span data-ttu-id="01da1-196">Azure kommer att starta processen för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="01da1-196">Azure will start the process of onboarding the VM.</span></span> <span data-ttu-id="01da1-197">När den är klar, den virtuella datorn kommer att visas i den **DSC-noder** bladet i Automation-kontot.</span><span class="sxs-lookup"><span data-stu-id="01da1-197">When it is complete, the VM will show up in the **DSC Nodes** blade in the Automation account.</span></span>

## <a name="viewing-the-list-of-dsc-nodes"></a><span data-ttu-id="01da1-198">Visa listan över DSC-noder</span><span class="sxs-lookup"><span data-stu-id="01da1-198">Viewing the list of DSC nodes</span></span>
<span data-ttu-id="01da1-199">Du kan visa listan över alla datorer som har publicerats så för hantering i ditt Automation-konto i den **DSC-noder** bladet.</span><span class="sxs-lookup"><span data-stu-id="01da1-199">You can view the list of all machines that have been onboarded for management in your Automation account in the **DSC Nodes** blade.</span></span>

1. <span data-ttu-id="01da1-200">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="01da1-200">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="01da1-201">På navmenyn klickar du på **alla resurser** och sedan namnet på ditt Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="01da1-201">On the Hub menu, click **All resources** and then the name of your Automation account.</span></span>
3. <span data-ttu-id="01da1-202">På den **automatiseringskontot** bladet, klickar du på **DSC-noder**.</span><span class="sxs-lookup"><span data-stu-id="01da1-202">On the **Automation account** blade, click **DSC Nodes**.</span></span>

## <a name="viewing-reports-for-dsc-nodes"></a><span data-ttu-id="01da1-203">Visa rapporter för DSC-noder</span><span class="sxs-lookup"><span data-stu-id="01da1-203">Viewing reports for DSC nodes</span></span>
<span data-ttu-id="01da1-204">Varje gång som Azure Automation DSC utför en konsekvenskontroll på en hanterad nod skickar noden en statusrapport tillbaka till den pull-servern.</span><span class="sxs-lookup"><span data-stu-id="01da1-204">Each time Azure Automation DSC performs a consistency check on a managed node, the node sends a status report back to the pull server.</span></span> <span data-ttu-id="01da1-205">Du kan visa rapporterna på bladet för noden.</span><span class="sxs-lookup"><span data-stu-id="01da1-205">You can view these reports on the blade for that node.</span></span>

1. <span data-ttu-id="01da1-206">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="01da1-206">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="01da1-207">På navmenyn klickar du på **alla resurser** och sedan namnet på ditt Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="01da1-207">On the Hub menu, click **All resources** and then the name of your Automation account.</span></span>
3. <span data-ttu-id="01da1-208">På den **automatiseringskontot** bladet, klickar du på **DSC-noder**.</span><span class="sxs-lookup"><span data-stu-id="01da1-208">On the **Automation account** blade, click **DSC Nodes**.</span></span>
4. <span data-ttu-id="01da1-209">På den **rapporter** panelen, klickar du på någon av rapporterna i listan.</span><span class="sxs-lookup"><span data-stu-id="01da1-209">On the **Reports** tile, click on any of the reports in the list.</span></span>
   
    ![Skärmbild av bladet rapport](./media/automation-dsc-getting-started/NodeReport.png)

<span data-ttu-id="01da1-211">I bladet för en enskild rapport kan se du följande statusinformation för motsvarande konsekvenskontrollen:</span><span class="sxs-lookup"><span data-stu-id="01da1-211">On the blade for an individual report, you can see the following status information for the corresponding consistency check:</span></span>

* <span data-ttu-id="01da1-212">Rapportstatus – om noden är ”kompatibla”, ”misslyckades” konfigurationen eller noden ”inkompatibla” (när noden är i **applyandmonitor** läge och datorn inte är i önskad tillstånd).</span><span class="sxs-lookup"><span data-stu-id="01da1-212">The report status — whether the node is "Compliant", the configuration "Failed", or the node is "Not Compliant" (when the node is in **applyandmonitor** mode and the machine is not in the desired state).</span></span>
* <span data-ttu-id="01da1-213">Starttid för konsekvenskontrollen.</span><span class="sxs-lookup"><span data-stu-id="01da1-213">The start time for the consistency check.</span></span>
* <span data-ttu-id="01da1-214">Den totala körtiden för konsekvenskontrollen.</span><span class="sxs-lookup"><span data-stu-id="01da1-214">The total runtime for the consistency check.</span></span>
* <span data-ttu-id="01da1-215">Typ av konsekvenskontroll.</span><span class="sxs-lookup"><span data-stu-id="01da1-215">The type of consistency check.</span></span>
* <span data-ttu-id="01da1-216">Fel, inklusive felkoden och felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="01da1-216">Any errors, including the error code and error message.</span></span> 
* <span data-ttu-id="01da1-217">Alla DSC-resurser som används i konfigurationen och statusen för varje resurs (om noden är i tillståndet önskade för den här resursen) – du kan klicka på varje resurs för att få mer detaljerad information för den här resursen.</span><span class="sxs-lookup"><span data-stu-id="01da1-217">Any DSC resources used in the configuration, and the state of each resource (whether the node is in the desired state for that resource) — you can click on each resource to get more detailed information for that resource.</span></span>
* <span data-ttu-id="01da1-218">Namn, IP-adress och konfigurationsläge för noden.</span><span class="sxs-lookup"><span data-stu-id="01da1-218">The name, IP address, and configuration mode of the node.</span></span>

<span data-ttu-id="01da1-219">Du kan också klicka på **Visa rapport om oformaterat** att se de faktiska data som noden skickar till servern.</span><span class="sxs-lookup"><span data-stu-id="01da1-219">You can also click **View raw report** to see the actual data that the node sends to the server.</span></span> <span data-ttu-id="01da1-220">Mer information om hur du använder dessa data finns [med hjälp av en rapportserver för DSC](https://msdn.microsoft.com/powershell/dsc/reportserver).</span><span class="sxs-lookup"><span data-stu-id="01da1-220">For more information about using that data, see [Using a DSC report server](https://msdn.microsoft.com/powershell/dsc/reportserver).</span></span>

<span data-ttu-id="01da1-221">Det kan ta lite tid när en nod har publicerats så innan den första rapporten är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="01da1-221">It can take some time after a node is onboarded before the first report is available.</span></span> <span data-ttu-id="01da1-222">Du kan behöva vänta upp till 30 minuter för den första rapporten när du har registrerat en nod.</span><span class="sxs-lookup"><span data-stu-id="01da1-222">You might need to wait up to 30 minutes for the first report after you onboard a node.</span></span>

## <a name="reassigning-a-node-to-a-different-node-configuration"></a><span data-ttu-id="01da1-223">Tilldela om en nod till en annan nod-konfiguration</span><span class="sxs-lookup"><span data-stu-id="01da1-223">Reassigning a node to a different node configuration</span></span>
<span data-ttu-id="01da1-224">Du kan tilldela en nod ska använda en annan nodkonfiguration än den som ursprungligen tilldelades.</span><span class="sxs-lookup"><span data-stu-id="01da1-224">You can assign a node to use a different node configuration than the one you initially assigned.</span></span>

1. <span data-ttu-id="01da1-225">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="01da1-225">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="01da1-226">På navmenyn klickar du på **alla resurser** och sedan namnet på ditt Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="01da1-226">On the Hub menu, click **All resources** and then the name of your Automation account.</span></span>
3. <span data-ttu-id="01da1-227">På den **automatiseringskontot** bladet, klickar du på **DSC-noder**.</span><span class="sxs-lookup"><span data-stu-id="01da1-227">On the **Automation account** blade, click **DSC Nodes**.</span></span>
4. <span data-ttu-id="01da1-228">På den **DSC-noder** bladet, klickar du på namnet på den nod som du vill tilldela.</span><span class="sxs-lookup"><span data-stu-id="01da1-228">On the **DSC Nodes** blade, click on the name of the node you want to reassign.</span></span>
5. <span data-ttu-id="01da1-229">Klicka på bladet för noden **tilldela noden**.</span><span class="sxs-lookup"><span data-stu-id="01da1-229">On the blade for that node, click **Assign node**.</span></span>
   
    ![Skärmbild av bladet nod Markera knappen Tilldela noden](./media/automation-dsc-getting-started/AssignNode.png)
6. <span data-ttu-id="01da1-231">På den **tilldela nodkonfiguration** bladet välj nodkonfiguration som du vill tilldela noden och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="01da1-231">On the **Assign Node Configuration** blade, select the node configuration to which you want to assign the node, and then click **OK**.</span></span>
   
    ![Skärmbild av bladet tilldela nodkonfiguration](./media/automation-dsc-getting-started/AssignNodeConfig.png)

## <a name="unregistering-a-node"></a><span data-ttu-id="01da1-233">Avregistrerar en nod</span><span class="sxs-lookup"><span data-stu-id="01da1-233">Unregistering a node</span></span>
<span data-ttu-id="01da1-234">Om du inte längre vill att en nod som ska hanteras av Azure Automation DSC kan du avregistrera det.</span><span class="sxs-lookup"><span data-stu-id="01da1-234">If you no longer want a node to be managed by Azure Automation DSC, you can unregister it.</span></span>

1. <span data-ttu-id="01da1-235">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="01da1-235">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="01da1-236">På navmenyn klickar du på **alla resurser** och sedan namnet på ditt Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="01da1-236">On the Hub menu, click **All resources** and then the name of your Automation account.</span></span>
3. <span data-ttu-id="01da1-237">På den **automatiseringskontot** bladet, klickar du på **DSC-noder**.</span><span class="sxs-lookup"><span data-stu-id="01da1-237">On the **Automation account** blade, click **DSC Nodes**.</span></span>
4. <span data-ttu-id="01da1-238">På den **DSC-noder** bladet, klickar du på namnet på den nod som du vill avregistrera.</span><span class="sxs-lookup"><span data-stu-id="01da1-238">On the **DSC Nodes** blade, click on the name of the node you want to unregister.</span></span>
5. <span data-ttu-id="01da1-239">Klicka på bladet för noden **avregistrera**.</span><span class="sxs-lookup"><span data-stu-id="01da1-239">On the blade for that node, click **Unregister**.</span></span>
   
    ![Skärmbild av bladet nod syntaxmarkering Unregister-knappen](./media/automation-dsc-getting-started/UnregisterNode.png)

## <a name="related-articles"></a><span data-ttu-id="01da1-241">Relaterade artiklar</span><span class="sxs-lookup"><span data-stu-id="01da1-241">Related Articles</span></span>
* [<span data-ttu-id="01da1-242">Översikt över Azure Automation DSC</span><span class="sxs-lookup"><span data-stu-id="01da1-242">Azure Automation DSC overview</span></span>](automation-dsc-overview.md)
* [<span data-ttu-id="01da1-243">Onboarding-datorer för hantering av Azure Automation DSC</span><span class="sxs-lookup"><span data-stu-id="01da1-243">Onboarding machines for management by Azure Automation DSC</span></span>](automation-dsc-onboarding.md)
* [<span data-ttu-id="01da1-244">Windows PowerShell Desired State Configuration-översikt</span><span class="sxs-lookup"><span data-stu-id="01da1-244">Windows PowerShell Desired State Configuration Overview</span></span>](https://msdn.microsoft.com/powershell/dsc/overview)
* [<span data-ttu-id="01da1-245">Azure Automation DSC-cmdlets</span><span class="sxs-lookup"><span data-stu-id="01da1-245">Azure Automation DSC cmdlets</span></span>](/powershell/module/azurerm.automation/#automation)
* [<span data-ttu-id="01da1-246">Priser för Azure Automation DSC</span><span class="sxs-lookup"><span data-stu-id="01da1-246">Azure Automation DSC pricing</span></span>](https://azure.microsoft.com/pricing/details/automation/)

