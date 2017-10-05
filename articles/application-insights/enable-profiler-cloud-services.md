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
# <a name="enable-application-insights-profiler-on-an-azure-cloud-services-resource"></a>Aktivera Application Insights Profiler på en resurs i Azure Cloud Services

Den här genomgången visar hur du aktiverar Azure Application Insights Profiler på ett ASP.NET-program som en resurs i Azure Cloud Services värd. Exemplen är stöd för Azure virtuella datorer, virtuella datorer och Azure Service Fabric. Alla exemplen är beroende av mallar som har stöd för Azure Resource Manager-distributionsmodellen. Mer information om distributionsmodell [Azure Resource Manager och klassisk distribution: Förstå distributionsmodeller och tillståndet för dina resurser](/azure-resource-manager/resource-manager-deployment-model).

## <a name="overview"></a>Översikt

Följande diagram illustrerar hur profileraren fungerar för Azure Cloud Services resurser. Den använder en virtuell Azure-dator som exempel.

![Översikt över](./media/enable-profiler-compute/overview.png) för att samla in information för bearbetning och visas på Azure-portalen måste du installera komponenten diagnostik Agent för Azure Cloud Services-resurser. Resten av den här genomgången innehåller råd om hur du installerar och konfigurerar diagnostik-Agent om du vill aktivera Application Insights Profiler.

## <a name="prerequisites-for-the-walkthrough"></a>Krav för den här genomgången

* En Distributionsmall för Resource Manager som installerar profiler-agenterna på de virtuella datorerna ([WindowsVirtualMachine.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json)) eller skala uppsättningar ([WindowsVirtualMachineScaleSet.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json)).

* En Application Insights-instans som har aktiverats för profilering. Instruktioner finns i [aktivera profilen](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-profiler#enable-the-profiler).

* .NET framework 4.6.1 eller senare installerat på Azure Cloud Services målresursen.

## <a name="create-a-resource-group-in-your-azure-subscription"></a>Skapa en resursgrupp i Azure-prenumeration
I följande exempel visar hur du skapar en resursgrupp med hjälp av ett PowerShell-skript:

```
New-AzureRmResourceGroup -Name "Replace_With_Resource_Group_Name" -Location "Replace_With_Resource_Group_Location"
```

## <a name="create-an-application-insights-resource-in-the-resource-group"></a>Skapa en Application Insights-resurs i resursgruppen.
På den **Programinsikter** bladet, ange information för din resurs som visas i det här exemplet: 

![Application Insights-bladet](./media/enable-profiler-compute/createai.png)

## <a name="apply-an-application-insights-instrumentation-key-in-the-azure-resource-manager-template"></a>Tillämpa en Application Insights instrumentation nyckel i Azure Resource Manager-mall

1. Om du inte har hämtat mallen ännu, hämta det från [GitHub](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json).

2. Hitta nyckeln för Application Insights.
   
   ![Platsen för nyckeln](./media/enable-profiler-compute/copyaikey.png)

3. Ersätt värdet för mallen.
   
   ![Värdet i mallen](./media/enable-profiler-compute/copyaikeytotemplate.png)

## <a name="create-an-azure-vm-to-host-the-web-application"></a>Skapa en virtuell Azure-dator som värd för webbprogrammet
1. Skapa en säker sträng om du vill spara lösenordet.

   ```
   $password = ConvertTo-SecureString -String "Replace_With_Your_Password" -AsPlainText -Force
   ```

2. Distribuera Azure Resource Manager-mallen.

   Ändra katalogen i PowerShell-konsol till den mapp som innehåller Resource Manager-mall. Distribuera mallen genom att köra följande kommando:

   ```
   New-AzureRmResourceGroupDeployment -ResourceGroupName "Replace_With_Resource_Group_Name" -TemplateFile .\WindowsVirtualMachine.json -adminUsername "Replace_With_your_user_name" -adminPassword $password -dnsNameForPublicIP "Replace_WIth_your_DNS_Name" -Verbose
   ```

När skriptet har körts kan du hitta en virtuell dator med namnet **MyWindowsVM** i resursgruppen.

## <a name="configure-web-deploy-on-the-vm"></a>Konfigurera webb distribuera på den virtuella datorn
Kontrollera att webbdistribution är aktiverat på den virtuella datorn så att du kan publicera webbprogrammet från Visual Studio.

Om du vill installera webbdistribution på en virtuell dator manuellt via WebPI [installera och konfigurera webbdistribution på IIS 8.0 eller senare](https://docs.microsoft.com/en-us/iis/install/installing-publishing-technologies/installing-and-configuring-web-deploy-on-iis-80-or-later). Ett exempel på hur du automatiserar installera webbdistribution genom att använda en Azure Resource Manager-mallen finns [skapa, konfigurera och distribuera ett program till en Azure VM](https://azure.microsoft.com/en-us/resources/templates/201-web-app-vm-dsc/).

Om du distribuerar ett ASP.NET MVC-program, gå till Server Manager, Välj **Lägg till roller och funktioner** > **webbserver (IIS)** > **webbservern** > **programutveckling**, och aktivera ASP.NET 4.5 på servern.

![Lägg till ASP.NET](./media/enable-profiler-compute/addaspnet45.png)

## <a name="install-the-azure-application-insights-sdk-for-your-project"></a>Installera Azure Application Insights SDK för ditt projekt
1. Öppna din ASP.NET-webbprogram i Visual Studio.

2. Högerklicka på projektet och välj **Lägg till** > **Connected Services**.

3. Välj **Programinsikter**.

4. Följ instruktionerna på sidan. Välj Application Insights-resurs som du skapade tidigare.

5. Välj den **registrera** knappen.


## <a name="publish-the-project-to-an-azure-vm"></a>Publicera projektet till en Azure VM
Det finns flera sätt att publicera ett program till en Azure VM. Ett sätt är att använda Visual Studio 2017.

1. Högerklicka på projektet och välj **publicera**.

2. Välj **Microsoft Azure Virtual Machines** som publicera mål och följer sedan anvisningarna.

   ![Publicera FromVS](./media/enable-profiler-compute/publishtoVM.png)

3. Kör ett belastningstest mot ditt program. Du bör se resultaten på portalen webbsidan för Application Insights-instans.


## <a name="enable-the-profiler"></a>Aktivera profileraren
1. Gå till Application Insights **prestanda** och välj **konfigurera**.
   
   ![Ikonen konfigurera](./media/enable-profiler-compute/enableprofiler1.png)
 
2. Välj **aktivera profileraren**.
   
   ![Aktivera profileraren ikon](./media/enable-profiler-compute/enableprofiler2.png)

## <a name="add-a-performance-test-to-your-application"></a>Lägg till en prestandatest i ditt program
Följ dessa steg så att vi kan samla in exempeldata som ska visas i Application Insights Profiler:

1. Bläddra till Application Insights-resursen som du skapade tidigare. 

2. Gå till den **tillgänglighet** bladet och Lägg till en prestandatest som skickar begäranden till programmets URL. 

   ![Lägg till prestandatester](./media/enable-profiler-compute/AvailabilityTest.png)

## <a name="view-your-performance-data"></a>Visa prestandadata

1. Vänta 10 – 15 minuter tills profileraren att samla in och analysera data. 

2. Gå till den **prestanda** bladet i Application Insights-resursen och visa hur programmet fungerar när det är under belastning.

   ![Visa prestanda](./media/enable-profiler-compute/aiperformance.png)

3. Välj ikonen under **exempel** att öppna den **Trace visa** bladet.

   ![Öppna bladet Trace vy](./media/enable-profiler-compute/traceview.png)


## <a name="work-with-an-existing-template"></a>Arbeta med en befintlig mall

1. Leta upp resursdeklarationen Azure-diagnostik i mallen för distribution.
   
   Om du inte har en deklaration, kan du skapa något som liknar deklarationen i följande exempel. Du kan uppdatera mallen från den [resursutforskaren Azure-webbplatsen](https://resources.azure.com).

2. Ändra utgivaren från `Microsoft.Azure.Diagnostics` till `AIP.Diagnostics.Test`.

3. För `typeHandlerVersion`, Använd `0.0`.

4. Se till att `autoUpgradeMinorVersion` är inställd på `true`.

5. Lägg till de nya `ApplicationInsightsProfiler` sink-instans i den `WadCfg` settings-objekt som visas i följande exempel:

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

## <a name="enable-the-profiler-on-virtual-machine-scale-sets"></a>Aktivera profileraren på virtuella datorer
Om du vill se hur du aktiverar profileraren hämta den [WindowsVirtualMachineScaleSet.json](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json) mall. Gäller samma ändringar i en mall för resursen diagnostik tillägget för virtuella datorns skaluppsättning.

Se till att varje instans i skaluppsättning har åtkomst till internet. Agenten Profiler kan skicka de insamlade exemplen till Application Insights för visning och analys.

## <a name="enable-the-profiler-on-service-fabric-applications"></a>Aktivera profileraren i Service Fabric-program
1. Etablera Service Fabric-kluster till Azure-diagnostik-tillägg som installerar Agent Profiler.

2. Installera Application Insights SDK i projektet och konfigurera Application Insights-nyckel.

3. Lägg till programkod betalningsinstrument telemetri.

### <a name="provision-the-service-fabric-cluster-to-have-the-azure-diagnostics-extension-that-installs-the-profiler-agent"></a>Etablera Service Fabric-kluster till Azure-diagnostik-tillägg som installerar Agent Profiler
Service Fabric-klustret kan vara säker eller inte är säker. Du kan ange en gateway-kluster ska vara icke-säker så inte behöver ett certifikat för åtkomst. Kluster som värd för affärslogik och data ska vara säker. Du kan aktivera profileraren på både säkra och icke-säker Service Fabric-kluster. Den här genomgången använder ett kluster som inte är säkra som ett exempel som förklarar vilka ändringar som krävs för att aktivera profileraren. Du kan etablera en säker klustret på samma sätt.

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

2. Distribuera mallen med hjälp av ett PowerShell-skript:

   ```
   Login-AzureRmAccount
   New-AzureRmResourceGroup -Name [Your_Resource_Group_Name] -Location [Your_Resource_Group_Location] -Verbose -Force
   New-AzureRmResourceGroupDeployment -Name [Choose_An_Arbitrary_Name] -ResourceGroupName [Your_Resource_Group_Name] -TemplateFile [Path_To_Your_Template]

   ```

### <a name="install-the-application-insights-sdk-in-the-project-and-configure-the-application-insights-key"></a>Installera Application Insights SDK i projektet och konfigurera Application Insights-nyckel
Installera Application Insights SDK från den [NuGet-paketet](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/). Kontrollera att du installerar en stabil version 2.3 eller senare. 

Information om hur du konfigurerar Application Insights i projekt finns [med hjälp av Service Fabric med Application Insights](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/blob/dev/appinsights/ApplicationInsights.md).

### <a name="add-application-code-to-instrument-telemetry"></a>Lägg till programkod betalningsinstrument telemetri
1. För varje typ av kod som du vill instrumentera, lägga till en med hjälp av instruktionen runt den. 

   I följande exempel visas den `RunAsync` metoden gör vissa arbete och `telemetryClient` klassen samlar in telemetrin när den har startat. Händelsen behöver ett unikt namn för programmet.

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

2. Distribuera programmet till Service Fabric-klustret. Vänta tills app att köras i 10 minuter. Du kan köra ett belastningstest för bättre effekt på appen. Gå till Application Insights-portalen **prestanda** bladet och du bör se exempel på profilering spårningar visas.

<!---
Commenting out these sections for now
## Enable the Profiler on Cloud Services applications
[TODO]
## Enable the Profiler on classic Azure Virtual Machines
[TODO]
## Enable the Profiler on on-premise servers
[TODO]
--->

## <a name="next-steps"></a>Nästa steg

- Få hjälp med felsökning av problem med profiler i [profileraren felsökning](app-insights-profiler.md#troubleshooting).

- Läs mer om profileraren i [Application Insights Profiler](app-insights-profiler.md).
