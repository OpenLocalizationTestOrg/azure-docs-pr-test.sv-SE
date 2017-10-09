---
title: "aaaEnable Azure Application Insights Profiler på en resurs för molntjänster | Microsoft Docs"
description: "Lär dig hur tooset in hello profiler på ett ASP.NET-program som är värd för en Azure Cloud Services-resurs."
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
ms.openlocfilehash: b9ac3bca513bf4518f44780389a9f2945f6ccc98
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-application-insights-profiler-on-an-azure-cloud-services-resource"></a><span data-ttu-id="2f0b8-103">Aktivera Application Insights Profiler på en resurs i Azure Cloud Services</span><span class="sxs-lookup"><span data-stu-id="2f0b8-103">Enable Application Insights Profiler on an Azure Cloud Services resource</span></span>

<span data-ttu-id="2f0b8-104">Den här genomgången visar hur tooenable Azure Application Insights Profiler på ett ASP.NET-program som är värd för en Azure Cloud Services-resurs.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-104">This walkthrough demonstrates how tooenable Azure Application Insights Profiler on an ASP.NET application hosted by an Azure Cloud Services resource.</span></span> <span data-ttu-id="2f0b8-105">hello exempel stöd för Azure virtuella datorer, virtuella datorer och Azure Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-105">hello examples include support for Azure Virtual Machines, virtual machine scale sets, and Azure Service Fabric.</span></span> <span data-ttu-id="2f0b8-106">hello-exempel som alla är beroende av mallar som stöder hello Azure Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-106">hello examples all rely on templates that support hello Azure Resource Manager deployment model.</span></span> <span data-ttu-id="2f0b8-107">Mer information om distributionsmodell hello, granska [Azure Resource Manager och klassisk distribution: Förstå distributionsmodeller och hello för dina resurser](/azure-resource-manager/resource-manager-deployment-model).</span><span class="sxs-lookup"><span data-stu-id="2f0b8-107">For more information about hello deployment model, review [Azure Resource Manager vs. classic deployment: Understand deployment models and hello state of your resources](/azure-resource-manager/resource-manager-deployment-model).</span></span>

## <a name="overview"></a><span data-ttu-id="2f0b8-108">Översikt</span><span class="sxs-lookup"><span data-stu-id="2f0b8-108">Overview</span></span>

<span data-ttu-id="2f0b8-109">hello följande diagram illustrerar hur hello profiler fungerar för Azure Cloud Services resurser.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-109">hello following diagram illustrates how hello profiler works for Azure Cloud Services resources.</span></span> <span data-ttu-id="2f0b8-110">Den använder en virtuell Azure-dator som exempel.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-110">It uses an Azure virtual machine as an example.</span></span>

<span data-ttu-id="2f0b8-111">![Översikt över](./media/enable-profiler-compute/overview.png) toocollect information för bearbetning och visas på hello Azure-portalen måste du installera hello diagnostik Agent-komponenten för hello Azure Cloud Services resurser.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-111">![Overview](./media/enable-profiler-compute/overview.png) toocollect information for processing and display on hello Azure portal, you must install hello Diagnostics Agent component for hello Azure Cloud Services resources.</span></span> <span data-ttu-id="2f0b8-112">hello resten av hello genomgång innehåller råd om hur tooinstall och konfigurera hello diagnostik Agent tooenable Application Insights Profiler.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-112">hello rest of hello walkthrough provides guidance on how tooinstall and configure hello Diagnostics Agent tooenable Application Insights Profiler.</span></span>

## <a name="prerequisites-for-hello-walkthrough"></a><span data-ttu-id="2f0b8-113">Krav för hello genomgång</span><span class="sxs-lookup"><span data-stu-id="2f0b8-113">Prerequisites for hello walkthrough</span></span>

* <span data-ttu-id="2f0b8-114">En Distributionsmall för Resource Manager som installerar hello profileraren agenter på hello virtuella datorer ([WindowsVirtualMachine.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json)) eller skala uppsättningar ([WindowsVirtualMachineScaleSet.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json)).</span><span class="sxs-lookup"><span data-stu-id="2f0b8-114">A deployment Resource Manager template that installs hello profiler agents on hello VMs ([WindowsVirtualMachine.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json)) or scale sets ([WindowsVirtualMachineScaleSet.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json)).</span></span>

* <span data-ttu-id="2f0b8-115">En Application Insights-instans som har aktiverats för profilering.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-115">An Application Insights instance enabled for profiling.</span></span> <span data-ttu-id="2f0b8-116">Instruktioner finns i [aktivera hello profilen](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-profiler#enable-the-profiler).</span><span class="sxs-lookup"><span data-stu-id="2f0b8-116">For instructions, see [Enable hello profile](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-profiler#enable-the-profiler).</span></span>

* <span data-ttu-id="2f0b8-117">.NET framework 4.6.1 eller senare installeras på hello mål Azure Cloud Services resurs.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-117">.NET Framework 4.6.1 or later installed on hello target Azure Cloud Services resource.</span></span>

## <a name="create-a-resource-group-in-your-azure-subscription"></a><span data-ttu-id="2f0b8-118">Skapa en resursgrupp i Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="2f0b8-118">Create a resource group in your Azure subscription</span></span>
<span data-ttu-id="2f0b8-119">hello som följande exempel visar hur toocreate en resurs gruppen genom att använda ett PowerShell-skript:</span><span class="sxs-lookup"><span data-stu-id="2f0b8-119">hello following example demonstrates how toocreate a resource group by using a PowerShell script:</span></span>

```
New-AzureRmResourceGroup -Name "Replace_With_Resource_Group_Name" -Location "Replace_With_Resource_Group_Location"
```

## <a name="create-an-application-insights-resource-in-hello-resource-group"></a><span data-ttu-id="2f0b8-120">Skapa en Application Insights-resurs i hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-120">Create an Application Insights resource in hello resource group</span></span>
<span data-ttu-id="2f0b8-121">På hello **Programinsikter** bladet ange hello information för din resurs som visas i det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="2f0b8-121">On hello **Application Insights** blade, enter hello information for your resource, as shown in this example:</span></span> 

![Application Insights-bladet](./media/enable-profiler-compute/createai.png)

## <a name="apply-an-application-insights-instrumentation-key-in-hello-azure-resource-manager-template"></a><span data-ttu-id="2f0b8-123">Tillämpa en Application Insights instrumentation nyckel i hello Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="2f0b8-123">Apply an Application Insights instrumentation key in hello Azure Resource Manager template</span></span>

1. <span data-ttu-id="2f0b8-124">Om du inte har hämtat hello mallen ännu, hämta det från [GitHub](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json).</span><span class="sxs-lookup"><span data-stu-id="2f0b8-124">If you haven't downloaded hello template yet, download it from [GitHub](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json).</span></span>

2. <span data-ttu-id="2f0b8-125">Hitta hello Application Insights nyckel.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-125">Find hello Application Insights key.</span></span>
   
   ![Platsen för hello nyckel](./media/enable-profiler-compute/copyaikey.png)

3. <span data-ttu-id="2f0b8-127">Ersätt hello mallen värde.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-127">Replace hello template value.</span></span>
   
   ![Värdet i hello mall](./media/enable-profiler-compute/copyaikeytotemplate.png)

## <a name="create-an-azure-vm-toohost-hello-web-application"></a><span data-ttu-id="2f0b8-129">Skapa en Azure VM toohost hello-webbprogram</span><span class="sxs-lookup"><span data-stu-id="2f0b8-129">Create an Azure VM toohost hello web application</span></span>
1. <span data-ttu-id="2f0b8-130">Skapa en säker sträng toosave hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-130">Create a secure string toosave hello password.</span></span>

   ```
   $password = ConvertTo-SecureString -String "Replace_With_Your_Password" -AsPlainText -Force
   ```

2. <span data-ttu-id="2f0b8-131">Distribuera hello Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-131">Deploy hello Azure Resource Manager template.</span></span>

   <span data-ttu-id="2f0b8-132">Ändra hello katalog i hello PowerShell-konsolen toohello mapp som innehåller Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-132">Change hello directory in hello PowerShell console toohello folder that contains your Resource Manager template.</span></span> <span data-ttu-id="2f0b8-133">toodeploy hello mall kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="2f0b8-133">toodeploy hello template, run hello following command:</span></span>

   ```
   New-AzureRmResourceGroupDeployment -ResourceGroupName "Replace_With_Resource_Group_Name" -TemplateFile .\WindowsVirtualMachine.json -adminUsername "Replace_With_your_user_name" -adminPassword $password -dnsNameForPublicIP "Replace_WIth_your_DNS_Name" -Verbose
   ```

<span data-ttu-id="2f0b8-134">När hello skriptet har körts kan du hitta en virtuell dator med namnet **MyWindowsVM** i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-134">After hello script runs successfully, you should find a VM named **MyWindowsVM** in your resource group.</span></span>

## <a name="configure-web-deploy-on-hello-vm"></a><span data-ttu-id="2f0b8-135">Konfigurera webbdistribution på hello VM</span><span class="sxs-lookup"><span data-stu-id="2f0b8-135">Configure Web Deploy on hello VM</span></span>
<span data-ttu-id="2f0b8-136">Kontrollera att webbdistribution är aktiverat på den virtuella datorn så att du kan publicera webbprogrammet från Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-136">Make sure that Web Deploy is enabled on your VM so you can publish your web application from Visual Studio.</span></span>

<span data-ttu-id="2f0b8-137">tooinstall webbdistribution på en virtuell dator manuellt via WebPI, se [installera och konfigurera webbdistribution på IIS 8.0 eller senare](https://docs.microsoft.com/en-us/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later).</span><span class="sxs-lookup"><span data-stu-id="2f0b8-137">tooinstall Web Deploy on a VM manually via WebPI, see [Installing and Configuring Web Deploy on IIS 8.0 or Later](https://docs.microsoft.com/en-us/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later).</span></span> <span data-ttu-id="2f0b8-138">Ett exempel på hur tooautomate installera webbdistribution genom att använda en Azure Resource Manager-mallen finns [skapa, konfigurera och distribuera en web application tooan Azure VM](https://azure.microsoft.com/en-us/resources/templates/201-web-app-vm-dsc/).</span><span class="sxs-lookup"><span data-stu-id="2f0b8-138">For an example of how tooautomate installing Web Deploy by using an Azure Resource Manager template, see [Create, configure, and deploy a web application tooan Azure VM](https://azure.microsoft.com/en-us/resources/templates/201-web-app-vm-dsc/).</span></span>

<span data-ttu-id="2f0b8-139">Om du distribuerar ett ASP.NET MVC-program går tooServer Manager, Välj **Lägg till roller och funktioner** > **webbserver (IIS)** > **webbservern**  >  **Programutveckling**, och aktivera ASP.NET 4.5 på servern.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-139">If you are deploying an ASP.NET MVC application, go tooServer Manager, select **Add Roles and Features** > **Web Server (IIS)** > **Web Server** > **Application Development**, and enable ASP.NET 4.5 on your server.</span></span>

![Lägg till ASP.NET](./media/enable-profiler-compute/addaspnet45.png)

## <a name="install-hello-azure-application-insights-sdk-for-your-project"></a><span data-ttu-id="2f0b8-141">Installera hello Azure Application Insights SDK för ditt projekt</span><span class="sxs-lookup"><span data-stu-id="2f0b8-141">Install hello Azure Application Insights SDK for your project</span></span>
1. <span data-ttu-id="2f0b8-142">Öppna din ASP.NET-webbprogram i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-142">Open your ASP.NET web application in Visual Studio.</span></span>

2. <span data-ttu-id="2f0b8-143">Högerklicka på hello-projektet och välj **Lägg till** > **Connected Services**.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-143">Right-click hello project and select **Add** > **Connected Services**.</span></span>

3. <span data-ttu-id="2f0b8-144">Välj **Programinsikter**.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-144">Select **Application Insights**.</span></span>

4. <span data-ttu-id="2f0b8-145">Följ instruktionerna för hello på hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-145">Follow hello instructions on hello page.</span></span> <span data-ttu-id="2f0b8-146">Välj hello Application Insights-resurs som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-146">Select hello Application Insights resource that you created earlier.</span></span>

5. <span data-ttu-id="2f0b8-147">Välj hello **registrera** knappen.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-147">Select hello **Register** button.</span></span>


## <a name="publish-hello-project-tooan-azure-vm"></a><span data-ttu-id="2f0b8-148">Publicera hello projektet tooan Azure VM</span><span class="sxs-lookup"><span data-stu-id="2f0b8-148">Publish hello project tooan Azure VM</span></span>
<span data-ttu-id="2f0b8-149">Det finns flera sätt toopublish ett program tooan Azure VM.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-149">There are several ways toopublish an application tooan Azure VM.</span></span> <span data-ttu-id="2f0b8-150">Ett sätt är toouse Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-150">One way is toouse Visual Studio 2017.</span></span>

1. <span data-ttu-id="2f0b8-151">Högerklicka på hello-projektet och välj **publicera**.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-151">Right-click hello project and select **Publish**.</span></span>

2. <span data-ttu-id="2f0b8-152">Välj **Microsoft Azure Virtual Machines** som hello publicera mål och hello gör.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-152">Select **Microsoft Azure Virtual Machines** as hello publish target and follow hello steps.</span></span>

   ![Publicera FromVS](./media/enable-profiler-compute/publishtoVM.png)

3. <span data-ttu-id="2f0b8-154">Kör ett belastningstest mot ditt program.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-154">Run a load test against your application.</span></span> <span data-ttu-id="2f0b8-155">Du bör se resultaten på portalen webbsidan för hello Application Insights-instans.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-155">You should see results on hello Application Insights instance portal webpage.</span></span>


## <a name="enable-hello-profiler"></a><span data-ttu-id="2f0b8-156">Aktivera hello profiler</span><span class="sxs-lookup"><span data-stu-id="2f0b8-156">Enable hello profiler</span></span>
1. <span data-ttu-id="2f0b8-157">Gå tooyour Application Insights **prestanda** och välj **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-157">Go tooyour Application Insights **Performance** blade and select **Configure**.</span></span>
   
   ![Ikonen konfigurera](./media/enable-profiler-compute/enableprofiler1.png)
 
2. <span data-ttu-id="2f0b8-159">Välj **aktivera profileraren**.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-159">Select **Enable Profiler**.</span></span>
   
   ![Aktivera profileraren ikon](./media/enable-profiler-compute/enableprofiler2.png)

## <a name="add-a-performance-test-tooyour-application"></a><span data-ttu-id="2f0b8-161">Lägg till en tooyour testprogrammet prestanda</span><span class="sxs-lookup"><span data-stu-id="2f0b8-161">Add a performance test tooyour application</span></span>
<span data-ttu-id="2f0b8-162">Följ dessa steg så att vi kan samla in några exempel data toobe som visas i Application Insights Profiler:</span><span class="sxs-lookup"><span data-stu-id="2f0b8-162">Follow these steps so we can collect some sample data toobe displayed in Application Insights Profiler:</span></span>

1. <span data-ttu-id="2f0b8-163">Bläddra toohello Application Insights-resurs som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-163">Browse toohello Application Insights resource that you created earlier.</span></span> 

2. <span data-ttu-id="2f0b8-164">Gå toohello **tillgänglighet** bladet och Lägg till en prestandatest som skickar webbprogrammets begäranden tooyour URL-adress.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-164">Go toohello **Availability** blade and add a performance test that sends web requests tooyour application URL.</span></span> 

   ![Lägg till prestandatester](./media/enable-profiler-compute/AvailabilityTest.png)

## <a name="view-your-performance-data"></a><span data-ttu-id="2f0b8-166">Visa prestandadata</span><span class="sxs-lookup"><span data-stu-id="2f0b8-166">View your performance data</span></span>

1. <span data-ttu-id="2f0b8-167">Vänta 10 – 15 minuter tills hello profileraren toocollect och analysera hello data.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-167">Wait 10-15 minutes for hello profiler toocollect and analyze hello data.</span></span> 

2. <span data-ttu-id="2f0b8-168">Gå toohello **prestanda** bladet i Application Insights-resursen och visa hur programmet fungerar när det är under belastning.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-168">Go toohello **Performance** blade in your Application Insights resource and view how your application is performing when it's under load.</span></span>

   ![Visa prestanda](./media/enable-profiler-compute/aiperformance.png)

3. <span data-ttu-id="2f0b8-170">Välj hello ikon under **exempel** tooopen hello **Trace visa** bladet.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-170">Select hello icon under **Examples** tooopen hello **Trace View** blade.</span></span>

   ![Öppna hello Trace vybladet](./media/enable-profiler-compute/traceview.png)


## <a name="work-with-an-existing-template"></a><span data-ttu-id="2f0b8-172">Arbeta med en befintlig mall</span><span class="sxs-lookup"><span data-stu-id="2f0b8-172">Work with an existing template</span></span>

1. <span data-ttu-id="2f0b8-173">Leta upp hello Azure Diagnostics resursdeklarationen i mallen för distribution.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-173">Locate hello Azure Diagnostics resource declaration in your deployment template.</span></span>
   
   <span data-ttu-id="2f0b8-174">Om du inte har en deklaration, kan du skapa något som liknar hello deklarationen i hello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-174">If you don't have a declaration, you can create one that resembles hello declaration in hello following example.</span></span> <span data-ttu-id="2f0b8-175">Du kan uppdatera hello-mall från hello [resursutforskaren Azure-webbplatsen](https://resources.azure.com).</span><span class="sxs-lookup"><span data-stu-id="2f0b8-175">You can update hello template from hello [Azure Resource Explorer website](https://resources.azure.com).</span></span>

2. <span data-ttu-id="2f0b8-176">Utgivare för entitetstillståndsändringar hello från `Microsoft.Azure.Diagnostics` för`AIP.Diagnostics.Test`.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-176">Change hello publisher from `Microsoft.Azure.Diagnostics` too`AIP.Diagnostics.Test`.</span></span>

3. <span data-ttu-id="2f0b8-177">För `typeHandlerVersion`, Använd `0.0`.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-177">For `typeHandlerVersion`, use `0.0`.</span></span>

4. <span data-ttu-id="2f0b8-178">Se till att `autoUpgradeMinorVersion` har angetts för`true`.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-178">Make sure that `autoUpgradeMinorVersion` is set too`true`.</span></span>

5. <span data-ttu-id="2f0b8-179">Lägga till nya hello `ApplicationInsightsProfiler` sink-instans i hello `WadCfg` settings-objekt som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="2f0b8-179">Add hello new `ApplicationInsightsProfiler` sink instance in hello `WadCfg` settings object, as shown in hello following example:</span></span>

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
                      "ApplicationInsightsProfiler": "Enter hello Application Insights instance instrumentation key guid here"
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

## <a name="enable-hello-profiler-on-virtual-machine-scale-sets"></a><span data-ttu-id="2f0b8-180">Aktivera hello profiler på virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="2f0b8-180">Enable hello profiler on virtual machine scale sets</span></span>
<span data-ttu-id="2f0b8-181">toosee hur tooenable hello profiler kan hämta hello [WindowsVirtualMachineScaleSet.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json) mall.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-181">toosee how tooenable hello profiler, download hello [WindowsVirtualMachineScaleSet.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json) template.</span></span> <span data-ttu-id="2f0b8-182">Tillämpa hello samma ändringar i en VM toohello diagnostik tillägget mallresurs för hello virtuella datorns skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-182">Apply hello same changes in a VM template toohello diagnostics extension resource for hello virtual machine scale set.</span></span>

<span data-ttu-id="2f0b8-183">Se till att varje instans i hello skaluppsättning har åtkomst toohello internet.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-183">Make sure that each instance in hello scale set has access toohello internet.</span></span> <span data-ttu-id="2f0b8-184">hello profileraren Agent kan sedan skicka hello samlas in prover tooApplication insikter visa och analysera.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-184">hello Profiler Agent can then send hello collected samples tooApplication Insights for display and analysis.</span></span>

## <a name="enable-hello-profiler-on-service-fabric-applications"></a><span data-ttu-id="2f0b8-185">Aktivera hello profiler för Service Fabric-program</span><span class="sxs-lookup"><span data-stu-id="2f0b8-185">Enable hello profiler on Service Fabric applications</span></span>
1. <span data-ttu-id="2f0b8-186">Etablera hello Service Fabric-kluster toohave hello Azure-diagnostik tillägg som installerar hello profileraren Agent.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-186">Provision hello Service Fabric cluster toohave hello Azure Diagnostics extension that installs hello Profiler Agent.</span></span>

2. <span data-ttu-id="2f0b8-187">Installera hello Application Insights SDK i hello projekt och konfigurera hello Application Insights nyckel.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-187">Install hello Application Insights SDK in hello project and configure hello Application Insights key.</span></span>

3. <span data-ttu-id="2f0b8-188">Lägg till program kod tooinstrument telemetri.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-188">Add application code tooinstrument telemetry.</span></span>

### <a name="provision-hello-service-fabric-cluster-toohave-hello-azure-diagnostics-extension-that-installs-hello-profiler-agent"></a><span data-ttu-id="2f0b8-189">Etablera hello Service Fabric-kluster toohave hello Azure Diagnostics-tillägg som installerar hello profileraren Agent</span><span class="sxs-lookup"><span data-stu-id="2f0b8-189">Provision hello Service Fabric cluster toohave hello Azure Diagnostics extension that installs hello Profiler Agent</span></span>
<span data-ttu-id="2f0b8-190">Service Fabric-klustret kan vara säker eller inte är säker.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-190">A Service Fabric cluster can be secure or non-secure.</span></span> <span data-ttu-id="2f0b8-191">Du kan ange en gateway-kluster toobe icke-säker så inte behöver ett certifikat för åtkomst.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-191">You can set one gateway cluster toobe non-secure so it doesn't require a certificate for access.</span></span> <span data-ttu-id="2f0b8-192">Kluster som värd för affärslogik och data ska vara säker.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-192">Clusters that host business logic and data should be secure.</span></span> <span data-ttu-id="2f0b8-193">Du kan aktivera hello profiler på både säkra och icke-säker Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-193">You can enable hello profiler on both secure and non-secure Service Fabric clusters.</span></span> <span data-ttu-id="2f0b8-194">Den här genomgången använder ett kluster som inte är säkra som ett exempel tooexplain vilka ändringar som är nödvändiga tooenable hello profiler.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-194">This walkthrough uses a non-secure cluster as an example tooexplain what changes are required tooenable hello profiler.</span></span> <span data-ttu-id="2f0b8-195">Du kan etablera en säker klustret i hello samma sätt.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-195">You can provision a secure cluster in hello same way.</span></span>

1. <span data-ttu-id="2f0b8-196">Hämta [ServiceFabricCluster.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/ServiceFabricCluster.json).</span><span class="sxs-lookup"><span data-stu-id="2f0b8-196">Download [ServiceFabricCluster.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/ServiceFabricCluster.json).</span></span> <span data-ttu-id="2f0b8-197">Som du gjorde för virtuella datorer och virtuella datorer, Ersätt `Application_Insights_Key` med Application Insights-nyckel:</span><span class="sxs-lookup"><span data-stu-id="2f0b8-197">As you did for VMs and virtual machine scale sets, replace `Application_Insights_Key` with your Application Insights key:</span></span>

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

2. <span data-ttu-id="2f0b8-198">Distribuera hello-mallen med hjälp av ett PowerShell-skript:</span><span class="sxs-lookup"><span data-stu-id="2f0b8-198">Deploy hello template by using a PowerShell script:</span></span>

   ```
   Login-AzureRmAccount
   New-AzureRmResourceGroup -Name [Your_Resource_Group_Name] -Location [Your_Resource_Group_Location] -Verbose -Force
   New-AzureRmResourceGroupDeployment -Name [Choose_An_Arbitrary_Name] -ResourceGroupName [Your_Resource_Group_Name] -TemplateFile [Path_To_Your_Template]

   ```

### <a name="install-hello-application-insights-sdk-in-hello-project-and-configure-hello-application-insights-key"></a><span data-ttu-id="2f0b8-199">Installera hello Application Insights SDK i hello projekt och konfigurera hello Application Insights nyckel</span><span class="sxs-lookup"><span data-stu-id="2f0b8-199">Install hello Application Insights SDK in hello project and configure hello Application Insights key</span></span>
<span data-ttu-id="2f0b8-200">Installera hello Application Insights SDK från hello [NuGet-paketet](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/).</span><span class="sxs-lookup"><span data-stu-id="2f0b8-200">Install hello Application Insights SDK from hello [NuGet package](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/).</span></span> <span data-ttu-id="2f0b8-201">Kontrollera att du installerar en stabil version 2.3 eller senare.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-201">Make sure that you install a stable version, 2.3 or later.</span></span> 

<span data-ttu-id="2f0b8-202">Information om hur du konfigurerar Application Insights i projekt finns [med hjälp av Service Fabric med Application Insights](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/blob/dev/appinsights/ApplicationInsights.md).</span><span class="sxs-lookup"><span data-stu-id="2f0b8-202">For information about configuring Application Insights in your projects, see [Using Service Fabric with Application Insights](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/blob/dev/appinsights/ApplicationInsights.md).</span></span>

### <a name="add-application-code-tooinstrument-telemetry"></a><span data-ttu-id="2f0b8-203">Lägg till programtelemetri kod tooinstrument</span><span class="sxs-lookup"><span data-stu-id="2f0b8-203">Add application code tooinstrument telemetry</span></span>
1. <span data-ttu-id="2f0b8-204">För varje typ av kod som du vill tooinstrument lägger du till en med hjälp av instruktionen runt den.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-204">For any piece of code that you want tooinstrument, add a using statement around it.</span></span> 

   <span data-ttu-id="2f0b8-205">I följande exempel hello, hello `RunAsync` metoden vissa projekt och hello `telemetryClient` klassen fångar hello telemetri när den har startat.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-205">In hello following  example, hello `RunAsync` method is doing some work, and hello `telemetryClient` class captures hello telemetry after it starts.</span></span> <span data-ttu-id="2f0b8-206">hello händelsen behöver ett unikt namn för programmet hello.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-206">hello event needs a unique name across hello application.</span></span>

   ```
   protected override async Task RunAsync(CancellationToken cancellationToken)
       {
           // TODO: Replace hello following sample code with your own logic
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

2. <span data-ttu-id="2f0b8-207">Distribuera programmet toohello Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-207">Deploy your application toohello Service Fabric cluster.</span></span> <span data-ttu-id="2f0b8-208">Vänta tills hello app toorun i 10 minuter.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-208">Wait for hello app toorun for 10 minutes.</span></span> <span data-ttu-id="2f0b8-209">Du kan köra ett belastningstest för bättre effekt på hello app.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-209">For better effect, you can run a load test on hello app.</span></span> <span data-ttu-id="2f0b8-210">Gå toohello Application Insights portal **prestanda** bladet och du bör se exempel på profilering spårningar visas.</span><span class="sxs-lookup"><span data-stu-id="2f0b8-210">Go toohello Application Insights portal's **Performance** blade, and you should see examples of profiling traces appear.</span></span>

<!---
Commenting out these sections for now
## Enable hello Profiler on Cloud Services applications
[TODO]
## Enable hello Profiler on classic Azure Virtual Machines
[TODO]
## Enable hello Profiler on on-premise servers
[TODO]
--->

## <a name="next-steps"></a><span data-ttu-id="2f0b8-211">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2f0b8-211">Next steps</span></span>

- <span data-ttu-id="2f0b8-212">Få hjälp med felsökning av problem med profiler i [profileraren felsökning](app-insights-profiler.md#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="2f0b8-212">Find help with troubleshooting profiler issues in [Profiler troubleshooting](app-insights-profiler.md#troubleshooting).</span></span>

- <span data-ttu-id="2f0b8-213">Läs mer om hello-profiler i [Application Insights Profiler](app-insights-profiler.md).</span><span class="sxs-lookup"><span data-stu-id="2f0b8-213">Read more about hello profiler in [Application Insights Profiler](app-insights-profiler.md).</span></span>
