---
title: "aaaCreate en VM-tillgänglighet som i Azure | Microsoft Docs"
description: "Lär dig hur toocreate en hanterad tillgänglighet ange eller ohanterad tillgänglighet för dina virtuella datorer med Azure PowerShell eller hello portalen i hello Resource Manager-distributionsmodellen."
keywords: "Tillgänglighetsuppsättning"
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: a3db8659-ace8-4e78-8b8c-1e75c04c042c
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: eadcdfcd28bb2fa21a4647f207b390c33e022ef1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="increase-vm-availability-by-creating-an-azure-availability-set"></a>Öka VM tillgänglighet genom att skapa en Azure tillgänglighetsuppsättning 
Tillgänglighetsuppsättningar ger redundans tooyour program. Vi rekommenderar att du grupperar två eller flera virtuella datorer i en tillgänglighetsuppsättning. Den här konfigurationen garanterar att under antingen en planerad eller oplanerad underhållshändelse, minst en virtuell dator blir tillgänglig och uppfyller hello 99,95% SLA för Azure. Mer information finns i hello [SLA för virtuella datorer](https://azure.microsoft.com/support/legal/sla/virtual-machines/).

> [!IMPORTANT]
> Virtuella datorer måste skapas i hello samma resursgrupp som hello tillgänglighetsuppsättning.
> 

Om du vill VM toobe-tillhör en tillgänglighetsuppsättning, du behöver toocreate hello tillgänglighet Ange första eller när du skapar din första virtuella datorn i hello uppsättningen. Om den virtuella datorn kommer att använda hanterade diskar, skapas hello tillgänglighetsuppsättning som en hanterad tillgänglighetsuppsättning.

Läs mer om hur du skapar och använder tillgänglighetsuppsättningar [hantera hello tillgängligheten för virtuella datorer](manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="use-hello-portal-toocreate-an-availability-set-before-creating-your-vms"></a>Använd hello portal toocreate en tillgänglighetsuppsättning innan du skapar dina virtuella datorer
1. Hej hubbmenyn, klicka på **Bläddra** och välj **tillgänglighetsuppsättningar**.
2. På hello **tillgänglighetsuppsättningar bladet**, klickar du på **Lägg till**.
   
    ![Skärmbild som visar hello lägga till knappen för att skapa en ny tillgänglighetsuppsättning.](./media/create-availability-set/add-availability-set.png)
3. På hello **skapa tillgänglighetsuppsättning** bladet, fullständig hello information för din uppsättning.
   
    ![Skärmbild som visar hello information du behöver tooenter toocreate hello tillgänglighet anges.](./media/create-availability-set/create-availability-set.png)
   
   * **Namnet** -hello namnet ska vara 1 – 80 tecken som består av siffror, bokstäver, punkter, understreck och bindestreck. hello första tecknet måste vara en bokstav eller siffra. hello sista tecknet måste vara en bokstav, siffra eller understreck.
   * **Fault domäner** -feldomäner definiera hello grupp virtuella datorer som delar en gemensam käll- och strömbrytare. Som standard hello VMs separeras över in toothree feldomäner och kan vara ändrade toobetween 1 och 3.
   * **Uppdatera domäner** – fem update domäner tilldelas som standard och du kan ange toobetween 1 och 20. Uppdatera domäner ange grupper av virtuella datorer och underliggande fysiska maskinvara som kan startas på hello samtidigt. Till exempel om vi anger fem update domäner, när fler än fem virtuella datorer är konfigurerade i en enda Tillgänglighetsuppsättning, hello sjätte virtuella datorn placeras i hello samma uppdateringsdomän som hello första virtuella datorn hello sjunde i hello samma UD som hello andra virtuella datorer och så vidare. hello ordning hello omstarter kanske inte sekventiell, men kommer att startas om endast en uppdateringsdomän i taget.
   * **Prenumerationen** -Välj hello prenumeration toouse om du har mer än ett.
   * **Resursgruppen** -Välj en befintlig resursgrupp genom att klicka på pilen hello och välja en resursgrupp hello listrutan. Du kan också skapa en ny resursgrupp genom att skriva in ett namn. hello kan innehålla något av följande tecken hello: bokstäver, siffror, punkter, bindestreck, understreck och inledande eller avslutande parentes. hello namn får inte sluta med en punkt. Alla hello virtuella datorer i hello tillgänglighetsgrupp behöver toobe som skapats i hello samma resursgrupp som hello tillgänglighetsuppsättning.
   * **Plats** -Välj en plats hello i listrutan.
   * **Hanterad** – Välj *Ja* toocreate en hanterad tillgänglighet ange toouse med virtuella datorer som använder hanterade diskar för lagring. Välj **nr** om hello virtuella datorer som ska finnas i uppsättning hello använda ohanterade diskar i ett lagringskonto.
   
4. När du är klar att mata in hello information klickar du på **skapa**. 

## <a name="use-hello-portal-toocreate-a-virtual-machine-and-an-availability-set-at-hello-same-time"></a>Använd hello portal toocreate en virtuell dator och en tillgänglighet ange på hello samma tid
Om du skapar en ny virtuell dator med hjälp av hello portal, du kan också skapa en ny tillgänglighetsuppsättning för hello VM, när du skapar hello första virtuella datorn i hello uppsättningen. Om du väljer toouse hanteras diskarna för den virtuella datorn skapas en hanterad tillgänglighetsuppsättning.

![Skärmbild som visar hello processen för att skapa en ny tillgänglighetsuppsättning när du skapar hello VM.](./media/create-availability-set/new-vm-avail-set.png)

## <a name="add-a-new-vm-tooan-existing-availability-set-in-hello-portal"></a>Lägg till en ny VM tooan befintliga tillgänglighetsuppsättning i hello-portalen
För varje ytterligare VM som du skapar som bör tillhör hello set, bör du skapa den i hello samma **resursgruppen** och sedan väljer hello befintliga tillgänglighetsuppsättning i steg3. 

![Skärmbild som visar hur tooselect en befintlig tillgänglighetsuppsättning ange toouse för den virtuella datorn.](./media/create-availability-set/add-vm-to-set.png)

## <a name="use-powershell-toocreate-an-availability-set"></a>Använd PowerShell toocreate tillgänglighet ange
Det här exemplet skapar en tillgänglighetsuppsättning namngivna **myAvailabilitySet** i hello **myResourceGroup** resursgrupp i hello **västra USA** plats. Detta måste toobe klar innan du skapar hello första virtuella dator som ska ingå i hello uppsättningen.

Innan du börjar bör du kontrollera att du har hello senaste versionen av hello AzureRM.Compute PowerShell-modulen. Kör följande kommando tooinstall hello den.

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
Mer information finns i [Azure PowerShell versionshantering](/powershell/azure/overview).


Om du använder hanterade diskar för dina virtuella datorer, skriver du:

```powershell
    New-AzureRmAvailabilitySet -ResourceGroupName "myResourceGroup" '
    -Name "myAvailabilitySet" -Location "West US" -managed
```

Om du använder egna storage-konton för dina virtuella datorer, skriver du:

```powershell
    New-AzureRmAvailabilitySet -ResourceGroupName "myResourceGroup" '
    -Name "myAvailabilitySet" -Location "West US" 
```

Mer information finns i [ny AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).

## <a name="troubleshooting"></a>Felsökning
* När du skapar kanske en virtuell dator, om hello tillgänglighetsuppsättningen som du vill använda inte finns i hello listrutan i hello portal du har skapat den i en annan resursgrupp. Om du inte vet hello resursgrupp för ditt tillgänglighet ange, gå toohello hubbmenyn och klicka på Bläddra > tillgänglighetsuppsättningar toosee anger en lista över din tillgänglighet och vilka resursgrupper som de tillhör.

## <a name="next-steps"></a>Nästa steg
Lägga till ytterligare lagringsutrymme tooyour VM genom att lägga till ytterligare [datadisk](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

