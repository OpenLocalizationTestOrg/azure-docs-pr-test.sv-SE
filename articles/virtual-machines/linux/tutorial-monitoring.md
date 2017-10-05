---
title: "Övervaka Linux-datorer i Azure | Microsoft Docs"
description: "Lär dig hur du övervakar Start diagnostik- och prestandamått i en virtuell Linux-dator i Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/08/2017
ms.author: davidmu
ms.custom: mvc
ms.openlocfilehash: 3fe8390e88e609b57a462e066f972346f8e8730e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-monitor-a-linux-virtual-machine-in-azure"></a>Så här övervakar du en virtuell Linux-dator i Azure

Du kan granska Start diagnostik- och prestandamått för att se till att dina virtuella datorer (VM) i Azure körs på rätt sätt. I den här guiden får du lära dig hur man:

> [!div class="checklist"]
> * Aktivera startdiagnostikinställningar på den virtuella datorn
> * Visa startdiagnostik
> * Visa värden mått
> * Aktivera diagnostik tillägget på den virtuella datorn
> * Visa VM mått
> * Skapa aviseringar baserat på diagnostiska mått
> * Konfigurera avancerad övervakning


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Om du väljer att installera och använda CLI lokalt kursen krävs att du använder Azure CLI version 2.0.4 eller senare. Kör `az --version` för att hitta versionen. Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="create-vm"></a>Skapa en virtuell dator

Om du vill visa diagnostik- och mått i praktiken, behöver du en virtuell dator. Börja med att skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create). I följande exempel skapas en resursgrupp med namnet *myResourceGroupMonitor* i den *eastus* plats.

```azurecli-interactive 
az group create --name myResourceGroupMonitor --location eastus
```

Nu skapa en virtuell dator med [az vm skapa](https://docs.microsoft.com/cli/azure/vm#create). I följande exempel skapas en virtuell dator med namnet *myVM*:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

## <a name="enable-boot-diagnostics"></a>Aktivera startdiagnostikinställningar

Eftersom virtuella Linux-datorer startar Start diagnostiska tillägget avbildas Start utdata och lagrar den i Azure-lagring. Dessa data kan användas för att felsöka problem med VM-start. Startdiagnostikinställningar aktiveras inte automatiskt när du skapar en Linux VM som använder Azure CLI.

Innan du aktiverar startdiagnostikinställningar behöver ett lagringskonto skapas för att lagra start loggar. Storage-konton måste ha ett globalt unikt namn vara mellan 3 och 24 tecken och får enbart innehålla siffror och gemener. Skapa ett lagringskonto med den [az storage-konto skapar](/cli/azure/storage/account#create) kommando. I det här exemplet används en slumpmässig sträng för att skapa en unik lagringskontonamnet. 

```azurecli-interactive 
storageacct=mydiagdata$RANDOM

az storage account create \
  --resource-group myResourceGroupMonitor \
  --name $storageacct \
  --sku Standard_LRS \
  --location eastus
```

När du aktiverar startdiagnostikinställningar krävs URI till blob storage-behållare. Följande kommando frågar storage-konto för att returnera URI: N. URI-värdet lagras i ett variabelnamn *bloburi*, som används i nästa steg.

```azurecli-interactive 
bloburi=$(az storage account show --resource-group myResourceGroupMonitor --name $storageacct --query 'primaryEndpoints.blob' -o tsv)
```

Nu aktivera startdiagnostikinställningar med [az vm-startdiagnostikinställningar aktivera](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#enable). Den `--storage` värdet är blob-URI som samlas in i föregående steg.

```azurecli-interactive 
az vm boot-diagnostics enable \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --storage $bloburi
```


## <a name="view-boot-diagnostics"></a>Visa startdiagnostik

När startdiagnostikinställningar aktiveras skrivs varje gång du stoppa och starta den virtuella datorn, information om startprocessen till en loggfil. I det här exemplet först frigöra den virtuella datorn med den [az vm frigöra](/cli/azure/vm#deallocate) kommandot på följande sätt:

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupMonitor --name myVM
```

Nu starta den virtuella datorn med den [az vm start]( /cli/azure/vm#stop) kommandot på följande sätt:

```azurecli-interactive 
az vm start --resource-group myResourceGroupMonitor --name myVM
```

Du kan hämta Start diagnostikdata för *myVM* med den [az vm-startdiagnostikinställningar get-Start-loggen](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#get-boot-log) kommandot på följande sätt:

```azurecli-interactive 
az vm boot-diagnostics get-boot-log --resource-group myResourceGroupMonitor --name myVM
```


## <a name="view-host-metrics"></a>Visa värden mått

En Linux-VM med en dedikerad värd i Azure som den interagerar med. Mått är automatiskt samlas in för värden och kan visas i Azure-portalen på följande sätt:

1. I Azure-portalen klickar du på **resursgrupper**väljer **myResourceGroupMonitor**, och välj sedan **myVM** i resurslistan.
1. Klicka för att se hur den Virtuella värddatorn utförs **mått** på VM-bladet välj sedan någon av de *[värd]* mått under **tillgängliga mått**.

    ![Visa värden mått](./media/tutorial-monitoring/monitor-host-metrics.png)


## <a name="install-diagnostics-extension"></a>Installera tillägg för diagnostik

> [!IMPORTANT]
> Det här dokumentet beskriver version 2.3 av Linux diagnostiska tillägget, som har tagits bort. Version 2.3 kommer att stödjas tills 30 juni 2018.
>
> Version 3.0 av Linux diagnostiska tillägget kan aktiveras i stället. Mer information finns i [dokumentationen](./diagnostic-extension.md).

De grundläggande värden är tillgänglig, men att se mer detaljerad och VM-specifika statistik, och du behöver installera Azure-diagnostik-tillägget på den virtuella datorn. Azure diagnostics-tillägget kan ytterligare övervakning och diagnostikdata som ska hämtas från den virtuella datorn. Du kan visa dessa prestandamått och skapa varningar baserat på hur den virtuella datorn utför. Diagnostiska tillägget installeras via Azure portal på följande sätt:

1. I Azure-portalen klickar du på **resursgrupper**väljer **myResourceGroup**, och välj sedan **myVM** i resurslistan.
1. Klicka på **diagnos inställningar**. I listan visas som *starta diagnostik* redan aktiverat från föregående avsnitt. Klicka på kryssrutan för *grundläggande mått*.
1. I den *lagringskonto* , bläddra till och välj den *mydiagdata [1234]* konto som har skapats i föregående avsnitt.
1. Klicka på knappen **Spara**.

    ![Visa diagnostik mått](./media/tutorial-monitoring/enable-diagnostics-extension.png)


## <a name="view-vm-metrics"></a>Visa VM mått

Du kan visa VM mått på samma sätt som du granskat värden VM mått:

1. I Azure-portalen klickar du på **resursgrupper**väljer **myResourceGroup**, och välj sedan **myVM** i resurslistan.
1. Klicka för att se hur den virtuella datorn utför **mått** på VM-bladet och välj sedan någon av diagnostik mått under **tillgängliga mått**.

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

När du har åtkomst till OMS-portalen hittar du den arbetsyta och arbetsytan identifierare på bladet inställningar. Ersätt < arbetsytenyckel > och < arbetsytan-id > med värden från din OMS för arbetsytan och du sedan kan använda **az vm-tillägget set** lägga till OMS-tillägget till den virtuella datorn:

```azurecli-interactive 
az vm extension set \
  --resource-group myResourceGroupMonitor \
  --vm-name myVM \
  --name OmsAgentForLinux \
  --publisher Microsoft.EnterpriseCloud.Monitoring \
  --version 1.3 \
  --protected-settings '{"workspaceKey": "<workspace-key>"}' \
  --settings '{"workspaceId": "<workspace-id>"}'
```

I bladet loggen Sök i OMS-portalen kan du se *myVM* , till exempel vad som ska visas i följande bild:

![OMS-bladet](./media/tutorial-monitoring/tutorial-monitor-oms.png)

## <a name="next-steps"></a>Nästa steg

I kursen får du konfigurerade och granskas virtuella datorer med Azure Security Center. Du har lärt dig hur till:

> [!div class="checklist"]
> * Aktivera startdiagnostikinställningar på den virtuella datorn
> * Visa startdiagnostik
> * Visa värden mått
> * Aktivera diagnostik tillägget på den virtuella datorn
> * Visa VM mått
> * Skapa aviseringar baserat på diagnostiska mått
> * Konfigurera avancerad övervakning

Gå vidare till nästa kurs vill veta mer om Azure security center.

> [!div class="nextstepaction"]
> [Hantera säkerheten för virtuella datorer](./tutorial-azure-security.md)