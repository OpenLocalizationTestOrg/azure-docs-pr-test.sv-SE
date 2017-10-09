---
title: "aaaAvailability anger självstudier för Linux virtuella datorer i Azure | Microsoft Docs"
description: "Läs mer om hello Tillgänglighetsuppsättningar för Linux virtuella datorer i Azure."
documentationcenter: 
services: virtual-machines-linux
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 2a91e4a6057180035ec51410d9fffccaca343758
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-availability-sets"></a>Hur toouse tillgänglighetsuppsättningar


I kursen får du lära dig hur tooincrease hello tillgänglighet och tillförlitlighet för dina lösningar för virtuell dator på Azure med hjälp av en funktion kallad Tillgänglighetsuppsättningar. Tillgänglighetsuppsättningar se till att hello virtuella datorer som du distribuerar i Azure är fördelade på flera isolerade maskinvara kluster. Detta säkerställer att om ett maskinvaru- eller fel i Azure inträffar, underordnade uppsättning dina virtuella datorer som påverkas och som din lösning ska vara tillgängliga och fungerar ur hello av dina kunder som använder den.

I den här guiden får du lära dig hur man:

> [!div class="checklist"]
> * Skapa en tillgänglighetsuppsättning
> * Skapa en virtuell dator i en tillgänglighetsuppsättning
> * Kontrollera tillgängliga storlekar på VM


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt kursen krävs att du kör hello Azure CLI version 2.0.4 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="availability-set-overview"></a>Översikt över tillgänglighetsuppsättning

En Tillgänglighetsuppsättning är en logisk gruppering funktion som du kan använda i Azure tooensure att hello VM resurser som du placerar i den isoleras från varandra när de distribueras i ett Azure-datacenter. Azure garanterar att hello virtuella datorer du placera inom en Tillgänglighetsuppsättning körs över flera fysiska servrar, beräkna rack, lagringsenheter och nätverksväxlar. Detta säkerställer att hello för händelsen maskinvaru- eller programvarufel Azure, påverkas endast en delmängd av dina virtuella datorer, och tillämpningsprogrammet övergripande förblir upp och fortsätta toobe tillgängliga tooyour kunder. Med hjälp av Tillgänglighetsuppsättningar är en viktig funktion tooleverage om du vill toobuild tillförlitliga molnlösningar.

Nu ska vi titta en typisk VM-baserad lösning där du kan ha 4 frontend-webbservrar och använda 2 backend-VMs som värd för en databas. Med Azure, du kan toodefine två tillgänglighetsuppsättningar innan du distribuerar dina virtuella datorer: en tillgänglighetsuppsättning för hello ”web”-nivå och en tillgänglighetsuppsättning för hello ”databas” nivå. När du skapar en ny virtuell dator kan du ange hello tillgänglighet som en parameter toohello az virtuell dator skapar kommando och Azure automatiskt garanterar att hello virtuella datorer som du skapar inom hello tillgänglig separat uppsättning över flera fysiska maskinvaruresurser. Det innebär att du vet att hello om hello fysiska maskinvara som din webbserver eller databasen Server virtuella datorer körs på ett problem har uppstått, andra instanser av webbservern och databasen virtuella datorer fortsätter att köras bra eftersom de finns på olika typer av maskinvara.

Du bör alltid använda Tillgänglighetsuppsättningar när du vill toodeploy tillförlitliga VM-baserade lösningar i Azure.


## <a name="create-an-availability-set"></a>Skapa en tillgänglighetsuppsättning

Du kan skapa en tillgänglighetsuppsättning med [az vm tillgänglighetsuppsättning skapa](/cli/azure/vm/availability-set#create). I det här exemplet vi ange båda hello antalet domäner för uppdatering och fel på *2* för hello tillgänglighetsuppsättning namngivna *myAvailabilitySet* i hello *myResourceGroupAvailability*resursgruppen.

Skapa en resursgrupp.

```azurecli-interactive 
az group create --name myResourceGroupAvailability --location eastus
```


```azurecli-interactive 
az vm availability-set create \
    --resource-group myResourceGroupAvailability \
    --name myAvailabilitySet \
    --platform-fault-domain-count 2 \
    --platform-update-domain-count 2
```

Tillgänglighetsuppsättningar tillåter tooisolate resurser över ”feldomäner” och ”uppdatera domäner”. En **feldomän** representerar isolerade mängd server + nätverk + lagring resurser. I föregående exempel hello, ange som vi vill vår tillgänglighetsuppsättning toobe fördelade på minst två feldomäner när våra virtuella datorer distribueras. Vi också ange att vi vårt tillgänglighetsuppsättning distribuerade mellan två **uppdatera domäner**.  Två update domäner för att se till att våra VM-resurser är isolerade hindrar alla hello-program som körs under våra VM uppdateras vid hello när Azure utför uppdateringar samtidigt.

## <a name="configure-virtual-network"></a>Konfigurera ett virtuellt nätverk
Innan du distribuerar vissa virtuella datorer och testa din belastningsutjämnare, skapa hello stöder nätverksresurser på virtuella datorer. Mer information om virtuella nätverk finns hello [hantera virtuella Azure-nätverk](tutorial-virtual-network.md) kursen.

### <a name="create-network-resources"></a>Skapa nätverksresurser
Skapa ett virtuellt nätverk med [az network vnet skapa](/cli/azure/network/vnet#create). hello följande exempel skapas ett virtuellt nätverk med namnet *myVnet* med ett undernät med namnet *mySubnet*:

```azurecli-interactive 
az network vnet create \
    --resource-group myResourceGroupAvailability \
    --name myVnet \
    --subnet-name mySubnet
```
Virtuella nätverkskort skapas med [az nätverket nic skapa](/cli/azure/network/nic#create). hello skapas följande exempel tre virtuella nätverkskort. (Ett virtuellt nätverkskort för varje virtuell dator som du skapar för din app i hello följande steg). Du kan skapa ytterligare virtuella nätverkskort och virtuella datorer när som helst och lägga till dem toohello belastningsutjämnare:

```bash
for i in `seq 1 3`; do
    az network nic create \
        --resource-group myResourceGroupAvailability \
        --name myNic$i \
        --vnet-name myVnet \
        --subnet mySubnet \
        --lb-name myLoadBalancer \
        --lb-address-pools myBackEndPool
done
```

## <a name="create-vms-inside-an-availability-set"></a>Skapa virtuella datorer i en tillgänglighetsuppsättning

Virtuella datorer måste skapas inom hello tillgänglighet set toomake till korrekt distribueras de över hello maskinvara. Du kan inte lägga till en befintlig virtuell dator tooan tillgänglighetsuppsättning när den har skapats. 

När du skapar en virtuell dator med hjälp av [az vm skapa](/cli/azure/vm#create) du ange hello tillgänglighet anges med hello `--availability-set` toospecify hello på parameternamn hello tillgänglighetsuppsättning.

```azurecli-interactive 
for i in `seq 1 2`; do
   az vm create \
     --resource-group myResourceGroupAvailability \
     --name myVM$i \
     --availability-set myAvailabilitySet \
     --nics myNic$i \
     --size Standard_DS1_v2  \
     --image Canonical:UbuntuServer:14.04.4-LTS:latest \
     --admin-username azureuser \
     --generate-ssh-keys \
     --no-wait
done 
```

Nu har vi två virtuella datorer i vår nya tillgänglighetsuppsättningen. Eftersom de hello samma tillgänglighetsuppsättning Azure säkerställer att hello virtuella datorer och alla sina resurser (inklusive datadiskar) är fördelade på isolerade fysisk maskinvara. Den här distributionen garanterar mycket högre tillgänglighet av vår övergripande VM-lösningen.

En sak som du kan stöta på när du lägger till virtuella datorer är att en viss VM-storlek är inte längre tillgängliga toouse inom din tillgänglighetsuppsättning. Det här problemet kan inträffa om det finns inte längre tillräckligt med kapacitet tooadd det samtidigt som hello isolering regler hello tillgänglighetsuppsättning tvingar. Du kan kontrollera toosee vilka VM-storlekar är tillgängliga toouse i en befintlig tillgänglighetsuppsättning med hello `--availability-set list-sizes` parameter.

## <a name="check-for-available-vm-sizes"></a>Sök efter tillgängliga storlekar på VM 

Du kan lägga till flera virtuella datorer toohello tillgänglighetsuppsättning senare, men du måste tooknow vilka VM-storlekar är tillgängliga på hello maskinvara. Använd [az vm-tillgänglighetsuppsättning lista över-storlekar](/cli/azure/availability-set#list-sizes) toolist alla hello tillgängliga storlekar på hello maskinvara kluster för hello tillgänglighetsuppsättning.

```azurecli-interactive 
az vm availability-set list-sizes \
     --resource-group myResourceGroupAvailability \
     --name myAvailabilitySet \
     --output table  
```

## <a name="next-steps"></a>Nästa steg

I den här självstudiekursen lärde du dig att:

> [!div class="checklist"]
> * Skapa en tillgänglighetsuppsättning
> * Skapa en virtuell dator i en tillgänglighetsuppsättning
> * Kontrollera tillgängliga storlekar på VM

Avancera toohello nästa självstudiekurs toolearn om skalningsuppsättningar i virtuella datorer.

> [!div class="nextstepaction"]
> [Skapa en VM-skalningsuppsättning](tutorial-create-vmss.md)

