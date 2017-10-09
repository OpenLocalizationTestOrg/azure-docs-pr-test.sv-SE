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
# <a name="enable-application-insights-profiler-on-an-azure-cloud-services-resource"></a>Aktivera Application Insights Profiler på en resurs i Azure Cloud Services

Den här genomgången visar hur tooenable Azure Application Insights Profiler på ett ASP.NET-program som är värd för en Azure Cloud Services-resurs. hello exempel stöd för Azure virtuella datorer, virtuella datorer och Azure Service Fabric. hello-exempel som alla är beroende av mallar som stöder hello Azure Resource Manager-distributionsmodellen. Mer information om distributionsmodell hello, granska [Azure Resource Manager och klassisk distribution: Förstå distributionsmodeller och hello för dina resurser](/azure-resource-manager/resource-manager-deployment-model).

## <a name="overview"></a>Översikt

hello följande diagram illustrerar hur hello profiler fungerar för Azure Cloud Services resurser. Den använder en virtuell Azure-dator som exempel.

![Översikt över](./media/enable-profiler-compute/overview.png) toocollect information för bearbetning och visas på hello Azure-portalen måste du installera hello diagnostik Agent-komponenten för hello Azure Cloud Services resurser. hello resten av hello genomgång innehåller råd om hur tooinstall och konfigurera hello diagnostik Agent tooenable Application Insights Profiler.

## <a name="prerequisites-for-hello-walkthrough"></a>Krav för hello genomgång

* En Distributionsmall för Resource Manager som installerar hello profileraren agenter på hello virtuella datorer ([WindowsVirtualMachine.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json)) eller skala uppsättningar ([WindowsVirtualMachineScaleSet.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json)).

* En Application Insights-instans som har aktiverats för profilering. Instruktioner finns i [aktivera hello profilen](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-profiler#enable-the-profiler).

* .NET framework 4.6.1 eller senare installeras på hello mål Azure Cloud Services resurs.

## <a name="create-a-resource-group-in-your-azure-subscription"></a>Skapa en resursgrupp i Azure-prenumeration
hello som följande exempel visar hur toocreate en resurs gruppen genom att använda ett PowerShell-skript:

```
New-AzureRmResourceGroup -Name "Replace_With_Resource_Group_Name" -Location "Replace_With_Resource_Group_Location"
```

## <a name="create-an-application-insights-resource-in-hello-resource-group"></a>Skapa en Application Insights-resurs i hello resursgruppen.
På hello **Programinsikter** bladet ange hello information för din resurs som visas i det här exemplet: 

![Application Insights-bladet](./media/enable-profiler-compute/createai.png)

## <a name="apply-an-application-insights-instrumentation-key-in-hello-azure-resource-manager-template"></a>Tillämpa en Application Insights instrumentation nyckel i hello Azure Resource Manager-mall

1. Om du inte har hämtat hello mallen ännu, hämta det från [GitHub](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json).

2. Hitta hello Application Insights nyckel.
   
   ![Platsen för hello nyckel](./media/enable-profiler-compute/copyaikey.png)

3. Ersätt hello mallen värde.
   
   ![Värdet i hello mall](./media/enable-profiler-compute/copyaikeytotemplate.png)

## <a name="create-an-azure-vm-toohost-hello-web-application"></a>Skapa en Azure VM toohost hello-webbprogram
1. Skapa en säker sträng toosave hello lösenord.

   ```
   $password = ConvertTo-SecureString -String "Replace_With_Your_Password" -AsPlainText -Force
   ```

2. Distribuera hello Azure Resource Manager-mall.

   Ändra hello katalog i hello PowerShell-konsolen toohello mapp som innehåller Resource Manager-mall. toodeploy hello mall kör hello följande kommando:

   ```
   New-AzureRmResourceGroupDeployment -ResourceGroupName "Replace_With_Resource_Group_Name" -TemplateFile .\WindowsVirtualMachine.json -adminUsername "Replace_With_your_user_name" -adminPassword $password -dnsNameForPublicIP "Replace_WIth_your_DNS_Name" -Verbose
   ```

När hello skriptet har körts kan du hitta en virtuell dator med namnet **MyWindowsVM** i resursgruppen.

## <a name="configure-web-deploy-on-hello-vm"></a>Konfigurera webbdistribution på hello VM
Kontrollera att webbdistribution är aktiverat på den virtuella datorn så att du kan publicera webbprogrammet från Visual Studio.

tooinstall webbdistribution på en virtuell dator manuellt via WebPI, se [installera och konfigurera webbdistribution på IIS 8.0 eller senare](https://docs.microsoft.com/en-us/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later). Ett exempel på hur tooautomate installera webbdistribution genom att använda en Azure Resource Manager-mallen finns [skapa, konfigurera och distribuera en web application tooan Azure VM](https://azure.microsoft.com/en-us/resources/templates/201-web-app-vm-dsc/).

Om du distribuerar ett ASP.NET MVC-program går tooServer Manager, Välj **Lägg till roller och funktioner** > **webbserver (IIS)** > **webbservern**  >  **Programutveckling**, och aktivera ASP.NET 4.5 på servern.

![Lägg till ASP.NET](./media/enable-profiler-compute/addaspnet45.png)

## <a name="install-hello-azure-application-insights-sdk-for-your-project"></a>Installera hello Azure Application Insights SDK för ditt projekt
1. Öppna din ASP.NET-webbprogram i Visual Studio.

2. Högerklicka på hello-projektet och välj **Lägg till** > **Connected Services**.

3. Välj **Programinsikter**.

4. Följ instruktionerna för hello på hello-sidan. Välj hello Application Insights-resurs som du skapade tidigare.

5. Välj hello **registrera** knappen.


## <a name="publish-hello-project-tooan-azure-vm"></a>Publicera hello projektet tooan Azure VM
Det finns flera sätt toopublish ett program tooan Azure VM. Ett sätt är toouse Visual Studio 2017.

1. Högerklicka på hello-projektet och välj **publicera**.

2. Välj **Microsoft Azure Virtual Machines** som hello publicera mål och hello gör.

   ![Publicera FromVS](./media/enable-profiler-compute/publishtoVM.png)

3. Kör ett belastningstest mot ditt program. Du bör se resultaten på portalen webbsidan för hello Application Insights-instans.


## <a name="enable-hello-profiler"></a>Aktivera hello profiler
1. Gå tooyour Application Insights **prestanda** och välj **konfigurera**.
   
   ![Ikonen konfigurera](./media/enable-profiler-compute/enableprofiler1.png)
 
2. Välj **aktivera profileraren**.
   
   ![Aktivera profileraren ikon](./media/enable-profiler-compute/enableprofiler2.png)

## <a name="add-a-performance-test-tooyour-application"></a>Lägg till en tooyour testprogrammet prestanda
Följ dessa steg så att vi kan samla in några exempel data toobe som visas i Application Insights Profiler:

1. Bläddra toohello Application Insights-resurs som du skapade tidigare. 

2. Gå toohello **tillgänglighet** bladet och Lägg till en prestandatest som skickar webbprogrammets begäranden tooyour URL-adress. 

   ![Lägg till prestandatester](./media/enable-profiler-compute/AvailabilityTest.png)

## <a name="view-your-performance-data"></a>Visa prestandadata

1. Vänta 10 – 15 minuter tills hello profileraren toocollect och analysera hello data. 

2. Gå toohello **prestanda** bladet i Application Insights-resursen och visa hur programmet fungerar när det är under belastning.

   ![Visa prestanda](./media/enable-profiler-compute/aiperformance.png)

3. Välj hello ikon under **exempel** tooopen hello **Trace visa** bladet.

   ![Öppna hello Trace vybladet](./media/enable-profiler-compute/traceview.png)


## <a name="work-with-an-existing-template"></a>Arbeta med en befintlig mall

1. Leta upp hello Azure Diagnostics resursdeklarationen i mallen för distribution.
   
   Om du inte har en deklaration, kan du skapa något som liknar hello deklarationen i hello följande exempel. Du kan uppdatera hello-mall från hello [resursutforskaren Azure-webbplatsen](https://resources.azure.com).

2. Utgivare för entitetstillståndsändringar hello från `Microsoft.Azure.Diagnostics` för`AIP.Diagnostics.Test`.

3. För `typeHandlerVersion`, Använd `0.0`.

4. Se till att `autoUpgradeMinorVersion` har angetts för`true`.

5. Lägga till nya hello `ApplicationInsightsProfiler` sink-instans i hello `WadCfg` settings-objekt som visas i följande exempel hello:

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

## <a name="enable-hello-profiler-on-virtual-machine-scale-sets"></a>Aktivera hello profiler på virtuella datorer
toosee hur tooenable hello profiler kan hämta hello [WindowsVirtualMachineScaleSet.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json) mall. Tillämpa hello samma ändringar i en VM toohello diagnostik tillägget mallresurs för hello virtuella datorns skaluppsättning.

Se till att varje instans i hello skaluppsättning har åtkomst toohello internet. hello profileraren Agent kan sedan skicka hello samlas in prover tooApplication insikter visa och analysera.

## <a name="enable-hello-profiler-on-service-fabric-applications"></a>Aktivera hello profiler för Service Fabric-program
1. Etablera hello Service Fabric-kluster toohave hello Azure-diagnostik tillägg som installerar hello profileraren Agent.

2. Installera hello Application Insights SDK i hello projekt och konfigurera hello Application Insights nyckel.

3. Lägg till program kod tooinstrument telemetri.

### <a name="provision-hello-service-fabric-cluster-toohave-hello-azure-diagnostics-extension-that-installs-hello-profiler-agent"></a>Etablera hello Service Fabric-kluster toohave hello Azure Diagnostics-tillägg som installerar hello profileraren Agent
Service Fabric-klustret kan vara säker eller inte är säker. Du kan ange en gateway-kluster toobe icke-säker så inte behöver ett certifikat för åtkomst. Kluster som värd för affärslogik och data ska vara säker. Du kan aktivera hello profiler på både säkra och icke-säker Service Fabric-kluster. Den här genomgången använder ett kluster som inte är säkra som ett exempel tooexplain vilka ändringar som är nödvändiga tooenable hello profiler. Du kan etablera en säker klustret i hello samma sätt.

1. Hämta [ServiceFabricCluster.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/ServiceFabricCluster.json). Som du gjorde för virtuella datorer och virtuella datorer, Ersätt `Application_Insights_Key` med Application Insights-nyckel:

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

2. Distribuera hello-mallen med hjälp av ett PowerShell-skript:

   ```
   Login-AzureRmAccount
   New-AzureRmResourceGroup -Name [Your_Resource_Group_Name] -Location [Your_Resource_Group_Location] -Verbose -Force
   New-AzureRmResourceGroupDeployment -Name [Choose_An_Arbitrary_Name] -ResourceGroupName [Your_Resource_Group_Name] -TemplateFile [Path_To_Your_Template]

   ```

### <a name="install-hello-application-insights-sdk-in-hello-project-and-configure-hello-application-insights-key"></a>Installera hello Application Insights SDK i hello projekt och konfigurera hello Application Insights nyckel
Installera hello Application Insights SDK från hello [NuGet-paketet](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/). Kontrollera att du installerar en stabil version 2.3 eller senare. 

Information om hur du konfigurerar Application Insights i projekt finns [med hjälp av Service Fabric med Application Insights](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/blob/dev/appinsights/ApplicationInsights.md).

### <a name="add-application-code-tooinstrument-telemetry"></a>Lägg till programtelemetri kod tooinstrument
1. För varje typ av kod som du vill tooinstrument lägger du till en med hjälp av instruktionen runt den. 

   I följande exempel hello, hello `RunAsync` metoden vissa projekt och hello `telemetryClient` klassen fångar hello telemetri när den har startat. hello händelsen behöver ett unikt namn för programmet hello.

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

2. Distribuera programmet toohello Service Fabric-klustret. Vänta tills hello app toorun i 10 minuter. Du kan köra ett belastningstest för bättre effekt på hello app. Gå toohello Application Insights portal **prestanda** bladet och du bör se exempel på profilering spårningar visas.

<!---
Commenting out these sections for now
## Enable hello Profiler on Cloud Services applications
[TODO]
## Enable hello Profiler on classic Azure Virtual Machines
[TODO]
## Enable hello Profiler on on-premise servers
[TODO]
--->

## <a name="next-steps"></a>Nästa steg

- Få hjälp med felsökning av problem med profiler i [profileraren felsökning](app-insights-profiler.md#troubleshooting).

- Läs mer om hello-profiler i [Application Insights Profiler](app-insights-profiler.md).
