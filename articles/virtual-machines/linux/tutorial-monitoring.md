---
title: aaaMonitor Linux virtuella datorer i Azure | Microsoft Docs
description: "Lär dig hur toomonitor starta diagnostik- och prestandamått i en virtuell Linux-dator i Azure"
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
ms.openlocfilehash: 282da0f03ab0bf37bd44750c22813ca8d1c89560
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomonitor-a-linux-virtual-machine-in-azure"></a>Hur toomonitor en Linux-dator i Azure

tooensure dina virtuella datorer (VM) i Azure körs på rätt sätt, kan du granska Start diagnostik- och prestandamått. I den här guiden får du lära dig hur man:

> [!div class="checklist"]
> * Aktivera startdiagnostikinställningar på hello VM
> * Visa startdiagnostik
> * Visa värden mått
> * Aktivera diagnostik hello VM-tillägget
> * Visa VM mått
> * Skapa aviseringar baserat på diagnostiska mått
> * Konfigurera avancerad övervakning


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt kursen krävs att du kör hello Azure CLI version 2.0.4 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="create-vm"></a>Skapa en virtuell dator

toosee diagnostik- och mått i åtgärden måste en virtuell dator. Börja med att skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create). hello följande exempel skapar en resursgrupp med namnet *myResourceGroupMonitor* i hello *eastus* plats.

```azurecli-interactive 
az group create --name myResourceGroupMonitor --location eastus
```

Nu skapa en virtuell dator med [az vm skapa](https://docs.microsoft.com/cli/azure/vm#create). hello följande exempel skapas en virtuell dator med namnet *myVM*:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

## <a name="enable-boot-diagnostics"></a>Aktivera startdiagnostikinställningar

Som virtuella Linux-datorer startar hello Start diagnostiska tillägget avbildas Start utdata och lagrar den i Azure-lagring. Dessa data kan vara används tootroubleshoot VM startproblem. Startdiagnostikinställningar aktiveras inte automatiskt när du skapar en Linux VM som använder hello Azure CLI.

Innan du aktiverar startdiagnostikinställningar måste ett lagringskonto toobe som skapats för att lagra start loggar. Storage-konton måste ha ett globalt unikt namn vara mellan 3 och 24 tecken och får enbart innehålla siffror och gemener. Skapa ett lagringskonto med hello [az storage-konto skapar](/cli/azure/storage/account#create) kommando. I det här exemplet är en slumpmässig sträng används toocreate en unik lagringskontonamnet. 

```azurecli-interactive 
storageacct=mydiagdata$RANDOM

az storage account create \
  --resource-group myResourceGroupMonitor \
  --name $storageacct \
  --sku Standard_LRS \
  --location eastus
```

När du aktiverar startdiagnostikinställningar krävs hello URI toohello blob storage-behållare. hello hello följande kommandofrågor storage-konto tooreturn URI: N. hello URI-värdet lagras i ett variabelnamn *bloburi*, som används i hello nästa steg.

```azurecli-interactive 
bloburi=$(az storage account show --resource-group myResourceGroupMonitor --name $storageacct --query 'primaryEndpoints.blob' -o tsv)
```

Nu aktivera startdiagnostikinställningar med [az vm-startdiagnostikinställningar aktivera](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#enable). Hej `--storage` värdet är hello blob-URI som samlas in i hello föregående steg.

```azurecli-interactive 
az vm boot-diagnostics enable \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --storage $bloburi
```


## <a name="view-boot-diagnostics"></a>Visa startdiagnostik

När startdiagnostikinställningar aktiveras skrivs varje gång du stoppa och starta hello VM, information om hello startprocessen tooa loggfilen. I det här exemplet först frigöra hello VM med hello [az vm frigöra](/cli/azure/vm#deallocate) kommandot på följande sätt:

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupMonitor --name myVM
```

Nu starta hello VM med hello [az vm start]( /cli/azure/vm#stop) kommandot på följande sätt:

```azurecli-interactive 
az vm start --resource-group myResourceGroupMonitor --name myVM
```

Du kan hämta hello Start diagnostikdata för *myVM* med hello [az vm-startdiagnostikinställningar get-Start-loggen](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#get-boot-log) kommandot på följande sätt:

```azurecli-interactive 
az vm boot-diagnostics get-boot-log --resource-group myResourceGroupMonitor --name myVM
```


## <a name="view-host-metrics"></a>Visa värden mått

En Linux-VM med en dedikerad värd i Azure som den interagerar med. Mått är automatiskt samlas in för hello värd och kan visas i hello Azure-portalen på följande sätt:

1. I hello Azure-portalen klickar du på **resursgrupper**väljer **myResourceGroupMonitor**, och välj sedan **myVM** i hello resurslistan.
1. toosee hur hello värden VM utförs klickar du på **mått** hello VM-bladet, välj sedan någon av hello *[värd]* mått under **tillgängliga mått**.

    ![Visa värden mått](./media/tutorial-monitoring/monitor-host-metrics.png)


## <a name="install-diagnostics-extension"></a>Installera tillägg för diagnostik

> [!IMPORTANT]
> Det här dokumentet beskriver version 2.3 av hello diagnostiska Linux-tillägg, som har tagits bort. Version 2.3 kommer att stödjas tills 30 juni 2018.
>
> Version 3.0 av hello Linux diagnostiska tillägget kan aktiveras i stället. Mer information finns i [hello dokumentationen](./diagnostic-extension.md).

hello grundläggande värden är tillgänglig, men toosee mer detaljerade och VM-specifika statistik, och du tooneed tooinstall hello Azure diagnostics tillägg på hello VM. hello Azure diagnostics tillägget kan ytterligare övervakning och diagnostik data toobe hämtas från hello VM. Du kan visa dessa prestandamått och skapa varningar baserat på hur hello VM utför. hello diagnostiska tillägg har installerats via hello Azure-portalen på följande sätt:

1. I hello Azure-portalen klickar du på **resursgrupper**väljer **myResourceGroup**, och välj sedan **myVM** i hello resurslistan.
1. Klicka på **diagnos inställningar**. hello lista visar att *starta diagnostik* redan aktiverat från hello föregående avsnitt. Klicka på hello kryssrutan för *grundläggande mått*.
1. I hello *lagringskonto* bläddrar tooand väljer hello *mydiagdata [1234]* konto hello föregående avsnitt.
1. Klicka på hello **spara** knappen.

    ![Visa diagnostik mått](./media/tutorial-monitoring/enable-diagnostics-extension.png)


## <a name="view-vm-metrics"></a>Visa VM mått

Du kan visa hello VM mått i hello samma sätt som du granskat hello värden VM mått:

1. I hello Azure-portalen klickar du på **resursgrupper**väljer **myResourceGroup**, och välj sedan **myVM** i hello resurslistan.
1. toosee hur hello VM utförs klickar du på **mått** på hello VM-bladet och välj sedan någon av hello diagnostik mått under **tillgängliga mått**.

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

När du har åtkomst toohello OMS-portalen hittar hello arbetsytan arbetsytan och nyckeln identifierare på inställningsbladet för hello. Ersätt < arbetsytenyckel > och < arbetsytan-id > med hello värden för från din OMS arbetsyta och du sedan kan använda **az vm-tillägget set** tooadd hello OMS tillägget toohello VM:

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

På hello loggen Sök bladet för hello OMS-portalen, bör du se *myVM* , till exempel vad som visas i följande bild hello:

![OMS-bladet](./media/tutorial-monitoring/tutorial-monitor-oms.png)

## <a name="next-steps"></a>Nästa steg

I kursen får du konfigurerade och granskas virtuella datorer med Azure Security Center. Du har lärt dig att:

> [!div class="checklist"]
> * Aktivera startdiagnostikinställningar på hello VM
> * Visa startdiagnostik
> * Visa värden mått
> * Aktivera diagnostik hello VM-tillägget
> * Visa VM mått
> * Skapa aviseringar baserat på diagnostiska mått
> * Konfigurera avancerad övervakning

Avancera toohello nästa självstudiekurs toolearn om Azure security center.

> [!div class="nextstepaction"]
> [Hantera säkerheten för virtuella datorer](./tutorial-azure-security.md)