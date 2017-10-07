---
title: "aaaAzure övervaknings- och Windows-datorer | Microsoft Docs"
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
ms.openlocfilehash: 191dc5a30d41c25a9e38f8ec2a32efdc05e03015
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-a-windows-virtual-machine-with-azure-powershell"></a>Övervaka en Windows-dator med Azure PowerShell

Azure-övervakning används agenter toocollect start och prestandadata från Azure virtuella datorer kan lagra dessa data i Azure-lagring och gör den tillgänglig via portalen, hello Azure PowerShell-modulen och hello Azure CLI. I den här guiden får du lära dig hur man:

> [!div class="checklist"]
> * Aktivera startdiagnostikinställningar på en virtuell dator
> * Visa startdiagnostik
> * Visa VM värden mått
> * Installera tillägg för hello diagnostik
> * Visa VM mått
> * Skapa en avisering
> * Konfigurera avancerad övervakning

Den här kursen kräver hello Azure PowerShell module 3,6 eller senare. Kör ` Get-Module -ListAvailable AzureRM` toofind hello version. Om du behöver tooupgrade finns [installera Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps).

toocomplete hello exempel i den här självstudiekursen, måste du ha en befintlig virtuell dator. Om det behövs, detta [skriptexempel](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) kan skapa en åt dig. När du arbetar via hello kursen, Ersätt hello resursgrupp, namn och plats där det behövs.

## <a name="view-boot-diagnostics"></a>Visa startdiagnostik

Eftersom Windows-datorer starta avbildar hello Start diagnostikagenten utdata som kan användas för felsökning. Den här funktionen är aktiverad som standard. hello avbildas skärmen bilderna lagras i Azure storage-konto som skapas också som standard. 

Du kan hämta hello Start diagnostikdata med hello [Get-AzureRmVMBootDiagnosticsData](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmbootdiagnosticsdata) kommando. I följande exempel hello, startdiagnostikinställningar är hämtade toohello rot hello * c:\* enhet. 

```powershell
Get-AzureRmVMBootDiagnosticsData -ResourceGroupName myResourceGroup -Name myVM -Windows -LocalPath "c:\"
```

## <a name="view-host-metrics"></a>Visa värden mått

En virtuell Windows-dator har en dedikerad värd eller virtuell dator i Azure som den interagerar med. Mått är automatiskt samlas in för hello värd och kan visas i hello Azure-portalen.

1. I hello Azure-portalen klickar du på **resursgrupper**väljer **myResourceGroup**, och välj sedan **myVM** i hello resurslistan.
2. Klicka på **mått** på hello VM-bladet och välj sedan någon av hello värden mått under **tillgängliga mått** toosee hur hello värden VM utförs.

    ![Visa värden mått](./media/tutorial-monitoring/tutorial-monitor-host-metrics.png)

## <a name="install-diagnostics-extension"></a>Installera tillägg för diagnostik

hello grundläggande värden är tillgänglig, men toosee mer detaljerade och VM-specifika statistik, och du tooneed tooinstall hello Azure diagnostics tillägg på hello VM. hello Azure diagnostics tillägget kan ytterligare övervakning och diagnostik data toobe hämtas från hello VM. Du kan visa dessa prestandamått och skapa varningar baserat på hur hello VM utför. hello diagnostiska tillägg har installerats via hello Azure-portalen på följande sätt:

1. I hello Azure-portalen klickar du på **resursgrupper**väljer **myResourceGroup**, och välj sedan **myVM** i hello resurslistan.
2. Klicka på **diagnos inställningar**. hello lista visar att *starta diagnostik* redan aktiverat från hello föregående avsnitt. Klicka på hello kryssrutan för *grundläggande mått*.
3. Klicka på hello **aktivera övervakning av gästnivå** knappen.

    ![Visa diagnostik mått](./media/tutorial-monitoring/enable-diagnostics-extension.png)

## <a name="view-vm-metrics"></a>Visa VM mått

Du kan visa hello VM mått i hello samma sätt som du granskat hello värden VM mått:

1. I hello Azure-portalen klickar du på **resursgrupper**väljer **myResourceGroup**, och välj sedan **myVM** i hello resurslistan.
2. toosee hur hello VM utförs klickar du på **mått** på hello VM-bladet och välj sedan någon av hello diagnostik mått under **tillgängliga mått**.

    ![Visa VM mått](./media/tutorial-monitoring/monitor-vm-metrics.png)

## <a name="create-alerts"></a>Skapa aviseringar

Du kan skapa varningar baserat på specifika prestandamått. Aviseringar kan till exempel vara används toonotify när Genomsnittlig CPU-användning överskrider ett visst tröskelvärde eller tillgängligt diskutrymme sjunker under en viss mängd. Aviseringar visas i hello Azure-portalen eller kan skickas via e-post. Du kan även utlösa Azure Automation-runbooks eller Azure Logikappar i svaret tooalerts som genereras.

hello skapas följande exempel en avisering för Genomsnittlig CPU-användning.

1. I hello Azure-portalen klickar du på **resursgrupper**väljer **myResourceGroup**, och välj sedan **myVM** i hello resurslistan.
2. Klicka på **Varna regler** på hello VM-bladet, klickar sedan på **Lägg till mått avisering** hello överst på bladet med säkerhetsaviseringar hello.
4. Ange en **namn** om varningen som *myAlertRule*
5. tootrigger en avisering när CPU-procent överskrider 1.0 i fem minuter lämnar du alla hello standardvärden som valts.
6. Du kan också markera kryssrutan hello för *e-ägare, deltagare och läsare* toosend e-postmeddelande. hello standardåtgärden är toopresent ett meddelande i hello-portalen.
7. Klicka på hello **OK** knappen.

## <a name="advanced-monitoring"></a>Avancerad övervakning 

Du kan göra mer avancerad övervakning av den virtuella datorn med hjälp av [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview). Om du inte redan gjort det. Du kan registrera dig för en [kostnadsfri utvärderingsversion](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial) av Operations Management Suite.

När du har åtkomst toohello OMS-portalen hittar hello arbetsytan arbetsytan och nyckeln identifierare på inställningsbladet för hello. Använd hello [Set AzureRmVMExtension](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmextension) cmmand tootooadd hello OMS tillägget toohello VM. Uppdatera hello variabeln värden i hello nedan exempel tooreflect du arbetsytan ID och arbetsytenyckel OMS  

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

Efter några minuter, bör du se hello ny virtuell dator i hello OMS-arbetsyta. 

![OMS-bladet](./media/tutorial-monitoring/tutorial-monitor-oms.png)

## <a name="next-steps"></a>Nästa steg
I kursen får du konfigurerade och granskas virtuella datorer med Azure Security Center. Du har lärt dig att:

> [!div class="checklist"]
> * Skapa ett virtuellt nätverk
> * Skapa en resursgrupp och en virtuell dator 
> * Aktivera startdiagnostikinställningar på hello VM
> * Visa startdiagnostik
> * Visa värden mått
> * Installera tillägg för hello diagnostik
> * Visa VM mått
> * Skapa en avisering
> * Konfigurera avancerad övervakning

Avancera toohello nästa självstudiekurs toolearn om Azure security center.

> [!div class="nextstepaction"]
> [Hantera säkerheten för virtuella datorer](./tutorial-azure-security.md)