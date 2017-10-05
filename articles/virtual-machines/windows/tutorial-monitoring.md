---
title: "Azure övervaknings- och Windows-datorer | Microsoft Docs"
description: "Kursen - Övervakare för en virtuell dator i Windows med Azure PowerShell"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/04/2017
ms.author: davidmu
ms.custom: mvc
ms.openlocfilehash: 42a475092bdd615c23154e5f813861b0acefcf29
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-a-windows-virtual-machine-with-azure-powershell"></a>Övervaka en Windows-dator med Azure PowerShell

Azure övervakning använder agenter för att samla in start-och prestandadata från Azure virtuella datorer, lagra dessa data i Azure-lagring och göra det tillgängligt via portalen, Azure PowerShell-modulen och Azure CLI. I den här guiden får du lära dig hur man:

> [!div class="checklist"]
> * Aktivera startdiagnostikinställningar på en virtuell dator
> * Visa startdiagnostik
> * Visa VM värden mått
> * Installera tillägget diagnostik
> * Visa VM mått
> * Skapa en avisering
> * Konfigurera avancerad övervakning

Den här självstudien kräver Azure PowerShell-modul version 3.6 eller senare. Kör ` Get-Module -ListAvailable AzureRM` för att hitta versionen. Om du behöver uppgradera [installera Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps).

Du måste ha en befintlig virtuell dator för att slutföra exemplet i den här självstudiekursen. Om det behövs, detta [skriptexempel](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) kan skapa en åt dig. När du arbetar med kursen, Ersätt resursgrupp, namn och plats där det behövs.

## <a name="view-boot-diagnostics"></a>Visa startdiagnostik

Eftersom Windows-datorer starta avbildar Start diagnostikagenten utdata som kan användas för felsökning. Den här funktionen är aktiverad som standard. Fångade skärmdumpar lagras i Azure storage-konto som skapas också som standard. 

Du kan hämta Start diagnostikdata med den [Get-AzureRmVMBootDiagnosticsData](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmbootdiagnosticsdata) kommando. I följande exempel hämtas startdiagnostikinställningar till roten av den * c:\* enhet. 

```powershell
Get-AzureRmVMBootDiagnosticsData -ResourceGroupName myResourceGroup -Name myVM -Windows -LocalPath "c:\"
```

## <a name="view-host-metrics"></a>Visa värden mått

En virtuell Windows-dator har en dedikerad värd eller virtuell dator i Azure som den interagerar med. Mått samlas in automatiskt för värden och kan visas i Azure-portalen.

1. I Azure-portalen klickar du på **resursgrupper**väljer **myResourceGroup**, och välj sedan **myVM** i resurslistan.
2. Klicka på **mått** på VM-bladet och välj sedan någon av mätvärdena som är värd under **tillgängliga mått** att se hur den Virtuella värden utförs.

    ![Visa värden mått](./media/tutorial-monitoring/tutorial-monitor-host-metrics.png)

## <a name="install-diagnostics-extension"></a>Installera tillägg för diagnostik

De grundläggande värden är tillgänglig, men att se mer detaljerad och VM-specifika statistik, och du behöver installera Azure-diagnostik-tillägget på den virtuella datorn. Azure diagnostics-tillägget kan ytterligare övervakning och diagnostikdata som ska hämtas från den virtuella datorn. Du kan visa dessa prestandamått och skapa varningar baserat på hur den virtuella datorn utför. Diagnostiska tillägget installeras via Azure portal på följande sätt:

1. I Azure-portalen klickar du på **resursgrupper**väljer **myResourceGroup**, och välj sedan **myVM** i resurslistan.
2. Klicka på **diagnos inställningar**. I listan visas som *starta diagnostik* redan aktiverat från föregående avsnitt. Klicka på kryssrutan för *grundläggande mått*.
3. Klicka på den **aktivera övervakning av gästnivå** knappen.

    ![Visa diagnostik mått](./media/tutorial-monitoring/enable-diagnostics-extension.png)

## <a name="view-vm-metrics"></a>Visa VM mått

Du kan visa VM mått på samma sätt som du granskat värden VM mått:

1. I Azure-portalen klickar du på **resursgrupper**väljer **myResourceGroup**, och välj sedan **myVM** i resurslistan.
2. Klicka för att se hur den virtuella datorn utför **mått** på VM-bladet och välj sedan någon av diagnostik mått under **tillgängliga mått**.

    ![Visa VM mått](./media/tutorial-monitoring/monitor-vm-metrics.png)

## <a name="create-alerts"></a>Skapa aviseringar

Du kan skapa varningar baserat på specifika prestandamått. Aviseringar kan användas för att meddela dig när Genomsnittlig CPU-användning överskrider ett visst tröskelvärde eller tillgängligt diskutrymme sjunker under ett visst belopp, till exempel. Aviseringar visas i Azure-portalen eller kan skickas via e-post. Du kan också utlösa Azure Automation-runbooks eller Logikappar i Azure som svar på varningar genereras.

I följande exempel skapas en avisering för Genomsnittlig CPU-användning.

1. I Azure-portalen klickar du på **resursgrupper**väljer **myResourceGroup**, och välj sedan **myVM** i resurslistan.
2. Klicka på **Varna regler** på VM-bladet Klicka **Lägg till mått avisering** överst på bladet aviseringar.
4. Ange en **namn** om varningen som *myAlertRule*
5. För att utlösa en avisering när CPU-procent överskrider 1.0 5 minuter, lämnar du alla andra standardinställningar som valts.
6. Du kan också markera kryssrutan för *e-ägare, deltagare och läsare* att skicka e-postmeddelande. Standardåtgärden är att visa ett meddelande i portalen.
7. Klicka på **OK**.

## <a name="advanced-monitoring"></a>Avancerad övervakning 

Du kan göra mer avancerad övervakning av den virtuella datorn med hjälp av [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview). Om du inte redan gjort det. Du kan registrera dig för en [kostnadsfri utvärderingsversion](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial) av Operations Management Suite.

När du har åtkomst till OMS-portalen hittar du den arbetsyta och arbetsytan identifierare på bladet inställningar. Använd den [Set AzureRmVMExtension](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmextension) cmmand för att lägga till OMS-tillägget på den virtuella datorn. Uppdatera variabelvärden i den nedan exempel för att återspegla du OMS arbetsytenyckel och arbetsytan Id.  

```powershell
$omsId = "<Replace with your OMS Id>"
$omsKey = "<Replace with your OMS key>"

Set-AzureRmVMExtension -ResourceGroupName myResourceGroup `
  -ExtensionName "Microsoft.EnterpriseCloud.Monitoring" `
  -VMName myVM `
  -Publisher "Microsoft.EnterpriseCloud.Monitoring" `
  -ExtensionType "MicrosoftMonitoringAgent" `
  -TypeHandlerVersion 1.0 `
  -Settings @{"workspaceId" = $omsId} `
  -ProtectedSettings @{"workspaceKey" = $omsKey} `
  -Location eastus
```

Du bör se den nya virtuella datorn i OMS-arbetsytan efter några minuter. 

![OMS-bladet](./media/tutorial-monitoring/tutorial-monitor-oms.png)

## <a name="next-steps"></a>Nästa steg
I kursen får du konfigurerade och granskas virtuella datorer med Azure Security Center. Du har lärt dig hur till:

> [!div class="checklist"]
> * Skapa ett virtuellt nätverk
> * Skapa en resursgrupp och en virtuell dator 
> * Aktivera startdiagnostikinställningar på den virtuella datorn
> * Visa startdiagnostik
> * Visa värden mått
> * Installera tillägget diagnostik
> * Visa VM mått
> * Skapa en avisering
> * Konfigurera avancerad övervakning

Gå vidare till nästa kurs vill veta mer om Azure security center.

> [!div class="nextstepaction"]
> [Hantera säkerheten för virtuella datorer](./tutorial-azure-security.md)