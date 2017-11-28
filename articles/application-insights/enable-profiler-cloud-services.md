---
title: "Aktivera Azure Application Insights Profiler på en resurs för molntjänster | Microsoft Docs"
description: "Lär dig hur du ställer in profileraren på ett ASP.NET-program som en resurs i Azure Cloud Services värd."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: bwren
ms.openlocfilehash: 5ff062ac81dca9d8b205cec966d2a9c11a4005b6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="enable-application-insights-profiler-on-an-azure-cloud-services-resource"></a><span data-ttu-id="fe96d-103">Aktivera Application Insights Profiler på en resurs i Azure Cloud Services</span><span class="sxs-lookup"><span data-stu-id="fe96d-103">Enable Application Insights Profiler on an Azure Cloud Services resource</span></span>

<span data-ttu-id="fe96d-104">Den här genomgången visar hur du aktiverar Azure Application Insights Profiler på ett ASP.NET-program som en resurs i Azure Cloud Services värd.</span><span class="sxs-lookup"><span data-stu-id="fe96d-104">This walkthrough demonstrates how to enable Azure Application Insights Profiler on an ASP.NET application hosted by an Azure Cloud Services resource.</span></span> <span data-ttu-id="fe96d-105">Exemplen är stöd för Azure virtuella datorer, virtuella datorer och Azure Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="fe96d-105">The examples include support for Azure Virtual Machines, virtual machine scale sets, and Azure Service Fabric.</span></span> <span data-ttu-id="fe96d-106">Alla exemplen är beroende av mallar som har stöd för Azure Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="fe96d-106">The examples all rely on templates that support the Azure Resource Manager deployment model.</span></span> <span data-ttu-id="fe96d-107">Mer information om distributionsmodell [Azure Resource Manager och klassisk distribution: Förstå distributionsmodeller och tillståndet för dina resurser](/azure-resource-manager/resource-manager-deployment-model).</span><span class="sxs-lookup"><span data-stu-id="fe96d-107">For more information about the deployment model, review [Azure Resource Manager vs. classic deployment: Understand deployment models and the state of your resources](/azure-resource-manager/resource-manager-deployment-model).</span></span>

## <a name="overview"></a><span data-ttu-id="fe96d-108">Översikt</span><span class="sxs-lookup"><span data-stu-id="fe96d-108">Overview</span></span>

<span data-ttu-id="fe96d-109">Följande diagram illustrerar hur profileraren fungerar för Azure Cloud Services resurser.</span><span class="sxs-lookup"><span data-stu-id="fe96d-109">The following diagram illustrates how the profiler works for Azure Cloud Services resources.</span></span> <span data-ttu-id="fe96d-110">Den använder en virtuell Azure-dator som exempel.</span><span class="sxs-lookup"><span data-stu-id="fe96d-110">It uses an Azure virtual machine as an example.</span></span>

<span data-ttu-id="fe96d-111">![Översikt över](./media/enable-profiler-compute/overview.png) för att samla in information för bearbetning och visas på Azure-portalen måste du installera komponenten diagnostik Agent för Azure Cloud Services-resurser.</span><span class="sxs-lookup"><span data-stu-id="fe96d-111">![Overview](./media/enable-profiler-compute/overview.png) To collect information for processing and display on the Azure portal, you must install the Diagnostics Agent component for the Azure Cloud Services resources.</span></span> <span data-ttu-id="fe96d-112">Resten av den här genomgången innehåller råd om hur du installerar och konfigurerar diagnostik-Agent om du vill aktivera Application Insights Profiler.</span><span class="sxs-lookup"><span data-stu-id="fe96d-112">The rest of the walkthrough provides guidance on how to install and configure the Diagnostics Agent to enable Application Insights Profiler.</span></span>

## <a name="prerequisites-for-the-walkthrough"></a><span data-ttu-id="fe96d-113">Krav för den här genomgången</span><span class="sxs-lookup"><span data-stu-id="fe96d-113">Prerequisites for the walkthrough</span></span>

* <span data-ttu-id="fe96d-114">En Distributionsmall för Resource Manager som installerar profiler-agenterna på de virtuella datorerna ([WindowsVirtualMachine.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json)) eller skala uppsättningar ([WindowsVirtualMachineScaleSet.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json)).</span><span class="sxs-lookup"><span data-stu-id="fe96d-114">A deployment Resource Manager template that installs the profiler agents on the VMs ([WindowsVirtualMachine.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json)) or scale sets ([WindowsVirtualMachineScaleSet.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json)).</span></span>

* <span data-ttu-id="fe96d-115">En Application Insights-instans som har aktiverats för profilering.</span><span class="sxs-lookup"><span data-stu-id="fe96d-115">An Application Insights instance enabled for profiling.</span></span> <span data-ttu-id="fe96d-116">Instruktioner finns i [aktivera profilen](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-profiler#enable-the-profiler).</span><span class="sxs-lookup"><span data-stu-id="fe96d-116">For instructions, see [Enable the profile](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-profiler#enable-the-profiler).</span></span>

* <span data-ttu-id="fe96d-117">.NET framework 4.6.1 eller senare installerat på Azure Cloud Services målresursen.</span><span class="sxs-lookup"><span data-stu-id="fe96d-117">.NET Framework 4.6.1 or later installed on the target Azure Cloud Services resource.</span></span>

## <a name="create-a-resource-group-in-your-azure-subscription"></a><span data-ttu-id="fe96d-118">Skapa en resursgrupp i Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="fe96d-118">Create a resource group in your Azure subscription</span></span>
<span data-ttu-id="fe96d-119">I följande exempel visar hur du skapar en resursgrupp med hjälp av ett PowerShell-skript:</span><span class="sxs-lookup"><span data-stu-id="fe96d-119">The following example demonstrates how to create a resource group by using a PowerShell script:</span></span>

```
New-AzureRmResourceGroup -Name "Replace_With_Resource_Group_Name" -Location "Replace_With_Resource_Group_Location"
```

## <a name="create-an-application-insights-resource-in-the-resource-group"></a><span data-ttu-id="fe96d-120">Skapa en Application Insights-resurs i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="fe96d-120">Create an Application Insights resource in the resource group</span></span>
<span data-ttu-id="fe96d-121">På den **Programinsikter** bladet, ange information för din resurs som visas i det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="fe96d-121">On the **Application Insights** blade, enter the information for your resource, as shown in this example:</span></span> 

![Application Insights-bladet](./media/enable-profiler-compute/createai.png)

## <a name="apply-an-application-insights-instrumentation-key-in-the-azure-resource-manager-template"></a><span data-ttu-id="fe96d-123">Tillämpa en Application Insights instrumentation nyckel i Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="fe96d-123">Apply an Application Insights instrumentation key in the Azure Resource Manager template</span></span>

1. <span data-ttu-id="fe96d-124">Om du inte har hämtat mallen ännu, hämta det från [GitHub](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json).</span><span class="sxs-lookup"><span data-stu-id="fe96d-124">If you haven't downloaded the template yet, download it from [GitHub](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json).</span></span>

2. <span data-ttu-id="fe96d-125">Hitta nyckeln för Application Insights.</span><span class="sxs-lookup"><span data-stu-id="fe96d-125">Find the Application Insights key.</span></span>
   
   ![Platsen för nyckeln](./media/enable-profiler-compute/copyaikey.png)

3. <span data-ttu-id="fe96d-127">Ersätt värdet för mallen.</span><span class="sxs-lookup"><span data-stu-id="fe96d-127">Replace the template value.</span></span>
   
   ![Värdet i mallen](./media/enable-profiler-compute/copyaikeytotemplate.png)

## <a name="create-an-azure-vm-to-host-the-web-application"></a><span data-ttu-id="fe96d-129">Skapa en virtuell Azure-dator som värd för webbprogrammet</span><span class="sxs-lookup"><span data-stu-id="fe96d-129">Create an Azure VM to host the web application</span></span>
1. <span data-ttu-id="fe96d-130">Skapa en säker sträng om du vill spara lösenordet.</span><span class="sxs-lookup"><span data-stu-id="fe96d-130">Create a secure string to save the password.</span></span>

   ```
   $password = ConvertTo-SecureString -String "Replace_With_Your_Password" -AsPlainText -Force
   ```

2. <span data-ttu-id="fe96d-131">Distribuera Azure Resource Manager-mallen.</span><span class="sxs-lookup"><span data-stu-id="fe96d-131">Deploy the Azure Resource Manager template.</span></span>

   <span data-ttu-id="fe96d-132">Ändra katalogen i PowerShell-konsol till den mapp som innehåller Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="fe96d-132">Change the directory in the PowerShell console to the folder that contains your Resource Manager template.</span></span> <span data-ttu-id="fe96d-133">Distribuera mallen genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="fe96d-133">To deploy the template, run the following command:</span></span>

   ```
   New-AzureRmResourceGroupDeployment -ResourceGroupName "Replace_With_Resource_Group_Name" -TemplateFile .\WindowsVirtualMachine.json -adminUsername "Replace_With_your_user_name" -adminPassword $password -dnsNameForPublicIP "Replace_WIth_your_DNS_Name" -Verbose
   ```

<span data-ttu-id="fe96d-134">När skriptet har körts kan du hitta en virtuell dator med namnet **MyWindowsVM** i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="fe96d-134">After the script runs successfully, you should find a VM named **MyWindowsVM** in your resource group.</span></span>

## <a name="configure-web-deploy-on-the-vm"></a><span data-ttu-id="fe96d-135">Konfigurera webb distribuera på den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="fe96d-135">Configure Web Deploy on the VM</span></span>
<span data-ttu-id="fe96d-136">Kontrollera att webbdistribution är aktiverat på den virtuella datorn så att du kan publicera webbprogrammet från Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fe96d-136">Make sure that Web Deploy is enabled on your VM so you can publish your web application from Visual Studio.</span></span>

<span data-ttu-id="fe96d-137">Om du vill installera webbdistribution på en virtuell dator manuellt via WebPI [installera och konfigurera webbdistribution på IIS 8.0 eller senare](https://docs.microsoft.com/en-us/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later).</span><span class="sxs-lookup"><span data-stu-id="fe96d-137">To install Web Deploy on a VM manually via WebPI, see [Installing and Configuring Web Deploy on IIS 8.0 or Later](https://docs.microsoft.com/en-us/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later).</span></span> <span data-ttu-id="fe96d-138">Ett exempel på hur du automatiserar installera webbdistribution genom att använda en Azure Resource Manager-mallen finns [skapa, konfigurera och distribuera ett program till en Azure VM](https://azure.microsoft.com/en-us/resources/templates/201-web-app-vm-dsc/).</span><span class="sxs-lookup"><span data-stu-id="fe96d-138">For an example of how to automate installing Web Deploy by using an Azure Resource Manager template, see [Create, configure, and deploy a web application to an Azure VM](https://azure.microsoft.com/en-us/resources/templates/201-web-app-vm-dsc/).</span></span>

<span data-ttu-id="fe96d-139">Om du distribuerar ett ASP.NET MVC-program, gå till Server Manager, Välj **Lägg till roller och funktioner** > **webbserver (IIS)** > **webbservern** > **programutveckling**, och aktivera ASP.NET 4.5 på servern.</span><span class="sxs-lookup"><span data-stu-id="fe96d-139">If you are deploying an ASP.NET MVC application, go to Server Manager, select **Add Roles and Features** > **Web Server (IIS)** > **Web Server** > **Application Development**, and enable ASP.NET 4.5 on your server.</span></span>

![Lägg till ASP.NET](./media/enable-profiler-compute/addaspnet45.png)

## <a name="install-the-azure-application-insights-sdk-for-your-project"></a><span data-ttu-id="fe96d-141">Installera Azure Application Insights SDK för ditt projekt</span><span class="sxs-lookup"><span data-stu-id="fe96d-141">Install the Azure Application Insights SDK for your project</span></span>
1. <span data-ttu-id="fe96d-142">Öppna din ASP.NET-webbprogram i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fe96d-142">Open your ASP.NET web application in Visual Studio.</span></span>

2. <span data-ttu-id="fe96d-143">Högerklicka på projektet och välj **Lägg till** > **Connected Services**.</span><span class="sxs-lookup"><span data-stu-id="fe96d-143">Right-click the project and select **Add** > **Connected Services**.</span></span>

3. <span data-ttu-id="fe96d-144">Välj **Programinsikter**.</span><span class="sxs-lookup"><span data-stu-id="fe96d-144">Select **Application Insights**.</span></span>

4. <span data-ttu-id="fe96d-145">Följ instruktionerna på sidan.</span><span class="sxs-lookup"><span data-stu-id="fe96d-145">Follow the instructions on the page.</span></span> <span data-ttu-id="fe96d-146">Välj Application Insights-resurs som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="fe96d-146">Select the Application Insights resource that you created earlier.</span></span>

5. <span data-ttu-id="fe96d-147">Välj den **registrera** knappen.</span><span class="sxs-lookup"><span data-stu-id="fe96d-147">Select the **Register** button.</span></span>


## <a name="publish-the-project-to-an-azure-vm"></a><span data-ttu-id="fe96d-148">Publicera projektet till en Azure VM</span><span class="sxs-lookup"><span data-stu-id="fe96d-148">Publish the project to an Azure VM</span></span>
<span data-ttu-id="fe96d-149">Det finns flera sätt att publicera ett program till en Azure VM.</span><span class="sxs-lookup"><span data-stu-id="fe96d-149">There are several ways to publish an application to an Azure VM.</span></span> <span data-ttu-id="fe96d-150">Ett sätt är att använda Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="fe96d-150">One way is to use Visual Studio 2017.</span></span>

1. <span data-ttu-id="fe96d-151">Högerklicka på projektet och välj **publicera**.</span><span class="sxs-lookup"><span data-stu-id="fe96d-151">Right-click the project and select **Publish**.</span></span>

2. <span data-ttu-id="fe96d-152">Välj **Microsoft Azure Virtual Machines** som publicera mål och följer sedan anvisningarna.</span><span class="sxs-lookup"><span data-stu-id="fe96d-152">Select **Microsoft Azure Virtual Machines** as the publish target and follow the steps.</span></span>

   ![Publicera FromVS](./media/enable-profiler-compute/publishtoVM.png)

3. <span data-ttu-id="fe96d-154">Kör ett belastningstest mot ditt program.</span><span class="sxs-lookup"><span data-stu-id="fe96d-154">Run a load test against your application.</span></span> <span data-ttu-id="fe96d-155">Du bör se resultaten på portalen webbsidan för Application Insights-instans.</span><span class="sxs-lookup"><span data-stu-id="fe96d-155">You should see results on the Application Insights instance portal webpage.</span></span>


## <a name="enable-the-profiler"></a><span data-ttu-id="fe96d-156">Aktivera profileraren</span><span class="sxs-lookup"><span data-stu-id="fe96d-156">Enable the profiler</span></span>
1. <span data-ttu-id="fe96d-157">Gå till Application Insights **prestanda** och välj **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="fe96d-157">Go to your Application Insights **Performance** blade and select **Configure**.</span></span>
   
   ![Ikonen konfigurera](./media/enable-profiler-compute/enableprofiler1.png)
 
2. <span data-ttu-id="fe96d-159">Välj **aktivera profileraren**.</span><span class="sxs-lookup"><span data-stu-id="fe96d-159">Select **Enable Profiler**.</span></span>
   
   ![Aktivera profileraren ikon](./media/enable-profiler-compute/enableprofiler2.png)

## <a name="add-a-performance-test-to-your-application"></a><span data-ttu-id="fe96d-161">Lägg till en prestandatest i ditt program</span><span class="sxs-lookup"><span data-stu-id="fe96d-161">Add a performance test to your application</span></span>
<span data-ttu-id="fe96d-162">Följ dessa steg så att vi kan samla in exempeldata som ska visas i Application Insights Profiler:</span><span class="sxs-lookup"><span data-stu-id="fe96d-162">Follow these steps so we can collect some sample data to be displayed in Application Insights Profiler:</span></span>

1. <span data-ttu-id="fe96d-163">Bläddra till Application Insights-resursen som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="fe96d-163">Browse to the Application Insights resource that you created earlier.</span></span> 

2. <span data-ttu-id="fe96d-164">Gå till den **tillgänglighet** bladet och Lägg till en prestandatest som skickar begäranden till programmets URL.</span><span class="sxs-lookup"><span data-stu-id="fe96d-164">Go to the **Availability** blade and add a performance test that sends web requests to your application URL.</span></span> 

   ![Lägg till prestandatester](./media/enable-profiler-compute/AvailabilityTest.png)

## <a name="view-your-performance-data"></a><span data-ttu-id="fe96d-166">Visa prestandadata</span><span class="sxs-lookup"><span data-stu-id="fe96d-166">View your performance data</span></span>

1. <span data-ttu-id="fe96d-167">Vänta 10 – 15 minuter tills profileraren att samla in och analysera data.</span><span class="sxs-lookup"><span data-stu-id="fe96d-167">Wait 10-15 minutes for the profiler to collect and analyze the data.</span></span> 

2. <span data-ttu-id="fe96d-168">Gå till den **prestanda** bladet i Application Insights-resursen och visa hur programmet fungerar när det är under belastning.</span><span class="sxs-lookup"><span data-stu-id="fe96d-168">Go to the **Performance** blade in your Application Insights resource and view how your application is performing when it's under load.</span></span>

   ![Visa prestanda](./media/enable-profiler-compute/aiperformance.png)

3. <span data-ttu-id="fe96d-170">Välj ikonen under **exempel** att öppna den **Trace visa** bladet.</span><span class="sxs-lookup"><span data-stu-id="fe96d-170">Select the icon under **Examples** to open the **Trace View** blade.</span></span>

   ![Öppna bladet Trace vy](./media/enable-profiler-compute/traceview.png)


## <a name="work-with-an-existing-template"></a><span data-ttu-id="fe96d-172">Arbeta med en befintlig mall</span><span class="sxs-lookup"><span data-stu-id="fe96d-172">Work with an existing template</span></span>

1. <span data-ttu-id="fe96d-173">Leta upp resursdeklarationen Azure-diagnostik i mallen för distribution.</span><span class="sxs-lookup"><span data-stu-id="fe96d-173">Locate the Azure Diagnostics resource declaration in your deployment template.</span></span>
   
   <span data-ttu-id="fe96d-174">Om du inte har en deklaration, kan du skapa något som liknar deklarationen i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="fe96d-174">If you don't have a declaration, you can create one that resembles the declaration in the following example.</span></span> <span data-ttu-id="fe96d-175">Du kan uppdatera mallen från den [resursutforskaren Azure-webbplatsen](https://resources.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fe96d-175">You can update the template from the [Azure Resource Explorer website](https://resources.azure.com).</span></span>

2. <span data-ttu-id="fe96d-176">Ändra utgivaren från `Microsoft.Azure.Diagnostics` till `AIP.Diagnostics.Test`.</span><span class="sxs-lookup"><span data-stu-id="fe96d-176">Change the publisher from `Microsoft.Azure.Diagnostics` to `AIP.Diagnostics.Test`.</span></span>

3. <span data-ttu-id="fe96d-177">För `typeHandlerVersion`, Använd `0.0`.</span><span class="sxs-lookup"><span data-stu-id="fe96d-177">For `typeHandlerVersion`, use `0.0`.</span></span>

4. <span data-ttu-id="fe96d-178">Se till att `autoUpgradeMinorVersion` är inställd på `true`.</span><span class="sxs-lookup"><span data-stu-id="fe96d-178">Make sure that `autoUpgradeMinorVersion` is set to `true`.</span></span>

5. <span data-ttu-id="fe96d-179">Lägg till de nya `ApplicationInsightsProfiler` sink-instans i den `WadCfg` settings-objekt som visas i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="fe96d-179">Add the new `ApplicationInsightsProfiler` sink instance in the `WadCfg` settings object, as shown in the following example:</span></span>

```
"resources": [
        {
          "type": "extensions",
          "name": "Microsoft.Insights.VMDiagnosticsSettings",
          "apiVersion": "2016-03-30",
          "properties": {
            "publisher": "AIP.Diagnostics.Test",
            "type": "IaaSDiagnostics",
            "typeHandlerVersion": "0.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
              "WadCfg": {
                "SinksConfig": {
                  "Sink": [
                    {
                      "name": "Give a descriptive short name. E.g.: MyApplicationInsightsProfilerSink",
                      "ApplicationInsightsProfiler": "Enter the Application Insights instance instrumentation key guid here"
                    }
                  ]
                },
                "DiagnosticMonitorConfiguration": {
                    ...
                }
                ...
              }
              ...
            }
            ...
          }
          ...
]
```

## <a name="enable-the-profiler-on-virtual-machine-scale-sets"></a><span data-ttu-id="fe96d-180">Aktivera profileraren på virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="fe96d-180">Enable the profiler on virtual machine scale sets</span></span>
<span data-ttu-id="fe96d-181">Om du vill se hur du aktiverar profileraren hämta den [WindowsVirtualMachineScaleSet.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json) mall.</span><span class="sxs-lookup"><span data-stu-id="fe96d-181">To see how to enable the profiler, download the [WindowsVirtualMachineScaleSet.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json) template.</span></span> <span data-ttu-id="fe96d-182">Gäller samma ändringar i en mall för resursen diagnostik tillägget för virtuella datorns skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="fe96d-182">Apply the same changes in a VM template to the diagnostics extension resource for the virtual machine scale set.</span></span>

<span data-ttu-id="fe96d-183">Se till att varje instans i skaluppsättning har åtkomst till internet.</span><span class="sxs-lookup"><span data-stu-id="fe96d-183">Make sure that each instance in the scale set has access to the internet.</span></span> <span data-ttu-id="fe96d-184">Agenten Profiler kan skicka de insamlade exemplen till Application Insights för visning och analys.</span><span class="sxs-lookup"><span data-stu-id="fe96d-184">The Profiler Agent can then send the collected samples to Application Insights for display and analysis.</span></span>

## <a name="enable-the-profiler-on-service-fabric-applications"></a><span data-ttu-id="fe96d-185">Aktivera profileraren i Service Fabric-program</span><span class="sxs-lookup"><span data-stu-id="fe96d-185">Enable the profiler on Service Fabric applications</span></span>
1. <span data-ttu-id="fe96d-186">Etablera Service Fabric-kluster till Azure-diagnostik-tillägg som installerar Agent Profiler.</span><span class="sxs-lookup"><span data-stu-id="fe96d-186">Provision the Service Fabric cluster to have the Azure Diagnostics extension that installs the Profiler Agent.</span></span>

2. <span data-ttu-id="fe96d-187">Installera Application Insights SDK i projektet och konfigurera Application Insights-nyckel.</span><span class="sxs-lookup"><span data-stu-id="fe96d-187">Install the Application Insights SDK in the project and configure the Application Insights key.</span></span>

3. <span data-ttu-id="fe96d-188">Lägg till programkod betalningsinstrument telemetri.</span><span class="sxs-lookup"><span data-stu-id="fe96d-188">Add application code to instrument telemetry.</span></span>

### <a name="provision-the-service-fabric-cluster-to-have-the-azure-diagnostics-extension-that-installs-the-profiler-agent"></a><span data-ttu-id="fe96d-189">Etablera Service Fabric-kluster till Azure-diagnostik-tillägg som installerar Agent Profiler</span><span class="sxs-lookup"><span data-stu-id="fe96d-189">Provision the Service Fabric cluster to have the Azure Diagnostics extension that installs the Profiler Agent</span></span>
<span data-ttu-id="fe96d-190">Service Fabric-klustret kan vara säker eller inte är säker.</span><span class="sxs-lookup"><span data-stu-id="fe96d-190">A Service Fabric cluster can be secure or non-secure.</span></span> <span data-ttu-id="fe96d-191">Du kan ange en gateway-kluster ska vara icke-säker så inte behöver ett certifikat för åtkomst.</span><span class="sxs-lookup"><span data-stu-id="fe96d-191">You can set one gateway cluster to be non-secure so it doesn't require a certificate for access.</span></span> <span data-ttu-id="fe96d-192">Kluster som värd för affärslogik och data ska vara säker.</span><span class="sxs-lookup"><span data-stu-id="fe96d-192">Clusters that host business logic and data should be secure.</span></span> <span data-ttu-id="fe96d-193">Du kan aktivera profileraren på både säkra och icke-säker Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="fe96d-193">You can enable the profiler on both secure and non-secure Service Fabric clusters.</span></span> <span data-ttu-id="fe96d-194">Den här genomgången använder ett kluster som inte är säkra som ett exempel som förklarar vilka ändringar som krävs för att aktivera profileraren.</span><span class="sxs-lookup"><span data-stu-id="fe96d-194">This walkthrough uses a non-secure cluster as an example to explain what changes are required to enable the profiler.</span></span> <span data-ttu-id="fe96d-195">Du kan etablera en säker klustret på samma sätt.</span><span class="sxs-lookup"><span data-stu-id="fe96d-195">You can provision a secure cluster in the same way.</span></span>

1. <span data-ttu-id="fe96d-196">Hämta [ServiceFabricCluster.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/ServiceFabricCluster.json).</span><span class="sxs-lookup"><span data-stu-id="fe96d-196">Download [ServiceFabricCluster.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/ServiceFabricCluster.json).</span></span> <span data-ttu-id="fe96d-197">Som du gjorde för virtuella datorer och virtuella datorer, Ersätt `Application_Insights_Key` med Application Insights-nyckel:</span><span class="sxs-lookup"><span data-stu-id="fe96d-197">As you did for VMs and virtual machine scale sets, replace `Application_Insights_Key` with your Application Insights key:</span></span>

   ```
   "publisher": "AIP.Diagnostics.Test",
                 "settings": {
                   "WadCfg": {
                     "SinksConfig": {
                       "Sink": [
                         {
                           "name": "MyApplicationInsightsProfilerSinkVMSS",
                           "ApplicationInsightsProfiler": "[Application_Insights_Key]"
                         }
                       ]
                     },
   ```

2. <span data-ttu-id="fe96d-198">Distribuera mallen med hjälp av ett PowerShell-skript:</span><span class="sxs-lookup"><span data-stu-id="fe96d-198">Deploy the template by using a PowerShell script:</span></span>

   ```
   Login-AzureRmAccount
   New-AzureRmResourceGroup -Name [Your_Resource_Group_Name] -Location [Your_Resource_Group_Location] -Verbose -Force
   New-AzureRmResourceGroupDeployment -Name [Choose_An_Arbitrary_Name] -ResourceGroupName [Your_Resource_Group_Name] -TemplateFile [Path_To_Your_Template]

   ```

### <a name="install-the-application-insights-sdk-in-the-project-and-configure-the-application-insights-key"></a><span data-ttu-id="fe96d-199">Installera Application Insights SDK i projektet och konfigurera Application Insights-nyckel</span><span class="sxs-lookup"><span data-stu-id="fe96d-199">Install the Application Insights SDK in the project and configure the Application Insights key</span></span>
<span data-ttu-id="fe96d-200">Installera Application Insights SDK från den [NuGet-paketet](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/).</span><span class="sxs-lookup"><span data-stu-id="fe96d-200">Install the Application Insights SDK from the [NuGet package](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/).</span></span> <span data-ttu-id="fe96d-201">Kontrollera att du installerar en stabil version 2.3 eller senare.</span><span class="sxs-lookup"><span data-stu-id="fe96d-201">Make sure that you install a stable version, 2.3 or later.</span></span> 

<span data-ttu-id="fe96d-202">Information om hur du konfigurerar Application Insights i projekt finns [med hjälp av Service Fabric med Application Insights](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/blob/dev/appinsights/ApplicationInsights.md).</span><span class="sxs-lookup"><span data-stu-id="fe96d-202">For information about configuring Application Insights in your projects, see [Using Service Fabric with Application Insights](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/blob/dev/appinsights/ApplicationInsights.md).</span></span>

### <a name="add-application-code-to-instrument-telemetry"></a><span data-ttu-id="fe96d-203">Lägg till programkod betalningsinstrument telemetri</span><span class="sxs-lookup"><span data-stu-id="fe96d-203">Add application code to instrument telemetry</span></span>
1. <span data-ttu-id="fe96d-204">För varje typ av kod som du vill instrumentera, lägga till en med hjälp av instruktionen runt den.</span><span class="sxs-lookup"><span data-stu-id="fe96d-204">For any piece of code that you want to instrument, add a using statement around it.</span></span> 

   <span data-ttu-id="fe96d-205">I följande exempel visas den `RunAsync` metoden gör vissa arbete och `telemetryClient` klassen samlar in telemetrin när den har startat.</span><span class="sxs-lookup"><span data-stu-id="fe96d-205">In the following  example, the `RunAsync` method is doing some work, and the `telemetryClient` class captures the telemetry after it starts.</span></span> <span data-ttu-id="fe96d-206">Händelsen behöver ett unikt namn för programmet.</span><span class="sxs-lookup"><span data-stu-id="fe96d-206">The event needs a unique name across the application.</span></span>

   ```
   protected override async Task RunAsync(CancellationToken cancellationToken)
       {
           // TODO: Replace the following sample code with your own logic
           //       or remove this RunAsync override if it's not needed in your service.

           while (true)
           {
               using( var operation = telemetryClient.StartOperation<RequestTelemetry>("[Insert_Event_Unique_Name]"))
               {
                   cancellationToken.ThrowIfCancellationRequested();

                   ++this.iterations;

                   ServiceEventSource.Current.ServiceMessage(this.Context, "Working-{0}", this.iterations);

                   await Task.Delay(TimeSpan.FromSeconds(1), cancellationToken);
               }

           }
       }
   ```

2. <span data-ttu-id="fe96d-207">Distribuera programmet till Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="fe96d-207">Deploy your application to the Service Fabric cluster.</span></span> <span data-ttu-id="fe96d-208">Vänta tills app att köras i 10 minuter.</span><span class="sxs-lookup"><span data-stu-id="fe96d-208">Wait for the app to run for 10 minutes.</span></span> <span data-ttu-id="fe96d-209">Du kan köra ett belastningstest för bättre effekt på appen.</span><span class="sxs-lookup"><span data-stu-id="fe96d-209">For better effect, you can run a load test on the app.</span></span> <span data-ttu-id="fe96d-210">Gå till Application Insights-portalen **prestanda** bladet och du bör se exempel på profilering spårningar visas.</span><span class="sxs-lookup"><span data-stu-id="fe96d-210">Go to the Application Insights portal's **Performance** blade, and you should see examples of profiling traces appear.</span></span>

<!---
Commenting out these sections for now
## Enable the Profiler on Cloud Services applications
[TODO]
## Enable the Profiler on classic Azure Virtual Machines
[TODO]
## Enable the Profiler on on-premise servers
[TODO]
--->

## <a name="next-steps"></a><span data-ttu-id="fe96d-211">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fe96d-211">Next steps</span></span>

- <span data-ttu-id="fe96d-212">Få hjälp med felsökning av problem med profiler i [profileraren felsökning](app-insights-profiler.md#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="fe96d-212">Find help with troubleshooting profiler issues in [Profiler troubleshooting](app-insights-profiler.md#troubleshooting).</span></span>

- <span data-ttu-id="fe96d-213">Läs mer om profileraren i [Application Insights Profiler](app-insights-profiler.md).</span><span class="sxs-lookup"><span data-stu-id="fe96d-213">Read more about the profiler in [Application Insights Profiler](app-insights-profiler.md).</span></span>
