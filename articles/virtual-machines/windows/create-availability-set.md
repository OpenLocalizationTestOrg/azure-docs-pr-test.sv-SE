---
title: "Skapa en VM-tillgänglighetsuppsättning i Azure | Microsoft Docs"
description: "Lär dig hur du skapar en tillgänglighetsuppsättning för hanterade eller ohanterade tillgänglighetsuppsättning för dina virtuella datorer med Azure PowerShell eller portalen i Resource Manager-distributionsmodellen."
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
ms.openlocfilehash: e813ade8e105169f21138ed8a8eafda1ff39f42e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="increase-vm-availability-by-creating-an-azure-availability-set"></a>Öka VM tillgänglighet genom att skapa en Azure tillgänglighetsuppsättning 
Tillgänglighetsuppsättningar ger redundans till ditt program. Vi rekommenderar att du grupperar två eller flera virtuella datorer i en tillgänglighetsuppsättning. Den här konfigurationen garanterar att under antingen en planerad eller oplanerad underhållshändelse, minst en virtuell dator ska vara tillgänglig och uppfyller 99,95% SLA för Azure. Mer information finns i [Serviceavtal för Virtual Machines](https://azure.microsoft.com/support/legal/sla/virtual-machines/).

> [!IMPORTANT]
> Virtuella datorer måste skapas i samma resursgrupp som tillgänglighetsuppsättningen.
> 

Om du vill att den virtuella datorn ska ingå i en tillgänglighetsuppsättning, måste du skapa tillgänglighetsuppsättning först eller när du skapar din första virtuella datorn i uppsättningen. Om den virtuella datorn kommer att använda hanterade diskar, skapas tillgänglighetsuppsättningen som en hanterad tillgänglighetsuppsättning.

Läs mer om hur du skapar och använder tillgänglighetsuppsättningar [hantera tillgängligheten för virtuella datorer](manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="use-the-portal-to-create-an-availability-set-before-creating-your-vms"></a>Använda portalen för att skapa en tillgänglighetsuppsättning innan du skapar dina virtuella datorer
1. Klicka på hubbmenyn, **Bläddra** och välj **tillgänglighetsuppsättningar**.
2. På den **tillgänglighetsuppsättningar bladet**, klickar du på **Lägg till**.
   
    ![Skärmbild som visar knappen Lägg till för att skapa en ny tillgänglighet anges.](./media/create-availability-set/add-availability-set.png)
3. På den **skapa tillgänglighetsuppsättning** bladet slutföra information för din uppsättning.
   
    ![Skärmbild som visar informationen du behöver ange för att skapa tillgänglighetsuppsättningen.](./media/create-availability-set/create-availability-set.png)
   
   * **Namnet** -namnet ska vara 1 – 80 tecken som består av siffror, bokstäver, punkter, understreck och bindestreck. Det första tecknet måste vara en bokstav eller siffra. Det sista tecknet måste vara en bokstav, siffra eller understreck.
   * **Fault domäner** -feldomäner definiera grupp med virtuella datorer som delar en gemensam käll- och strömbrytare. Som standard de virtuella datorerna separeras över upp till tre feldomäner och kan ändras till mellan 1 och 3.
   * **Uppdatera domäner** – fem update domäner tilldelas som standard och det kan anges mellan 1 och 20. Uppdatera domäner ange grupper av virtuella datorer och underliggande fysiska maskinvara som kan startas på samma gång. Till exempel om vi anger fem uppdatera domäner, när fler än fem virtuella datorer är konfigurerade i en enda Tillgänglighetsuppsättning sjätte virtuella datorn placeras i samma uppdateringsdomän som den första virtuella datorn, sjunde i samma UD som den andra virtuella och så vidare. Ordningen på omstarter kanske inte sekventiell, men kommer att startas om endast en uppdateringsdomän i taget.
   * **Prenumerationen** – Välj prenumerationen som ska användas om du har fler än en.
   * **Resursgruppen** -Välj en befintlig resursgrupp genom att klicka på pilen och välja en resursgrupp i nedrullningsbara ned. Du kan också skapa en ny resursgrupp genom att skriva in ett namn. Namnet kan innehålla något av följande tecken: bokstäver, siffror, punkter, bindestreck, understreck och inledande eller avslutande parentes. Namnet får inte sluta med en punkt. Alla virtuella datorer i tillgänglighetsgruppen måste skapas i samma resursgrupp som tillgänglighetsuppsättningen.
   * **Plats** -Välj en plats från listrutan.
   * **Hanterad** – Välj *Ja* att skapa en hanterad tillgänglighetsuppsättning för att använda med virtuella datorer som använder hanterade diskar för lagring. Välj **nr** om de virtuella datorerna som ska ingå i uppsättningen använder ohanterade diskar i ett lagringskonto.
   
4. När du är klar att lägga till information, klickar du på **skapa**. 

## <a name="use-the-portal-to-create-a-virtual-machine-and-an-availability-set-at-the-same-time"></a>Använda portalen för att skapa en virtuell dator och en tillgänglighetsuppsättning på samma gång
Du kan också skapa en ny tillgänglighetsuppsättning för den virtuella datorn när du skapar den första virtuella datorn i uppsättningen om du skapar en ny virtuell dator med hjälp av portalen. Om du väljer att använda hanterade diskar för den virtuella datorn skapas en hanterad tillgänglighetsuppsättning.

![Skärmbild som visar processen för att skapa en ny tillgänglighetsuppsättning när du skapar den virtuella datorn.](./media/create-availability-set/new-vm-avail-set.png)

## <a name="add-a-new-vm-to-an-existing-availability-set-in-the-portal"></a>Lägg till en ny virtuell dator i en befintlig tillgänglighetsuppsättning i portalen
Se till att du skapar den i samma för varje ytterligare VM som du skapar som ska tillhöra i uppsättningen **resursgruppen** och välj sedan den befintliga tillgänglighetsuppsättning i steg3. 

![Skärmbild som visar hur du väljer en befintlig tillgänglighetsuppsättning för den virtuella datorn.](./media/create-availability-set/add-vm-to-set.png)

## <a name="use-powershell-to-create-an-availability-set"></a>Använd PowerShell för att skapa en tillgänglighetsuppsättning
Det här exemplet skapar en tillgänglighetsuppsättning namngivna **myAvailabilitySet** i den **myResourceGroup** resursgrupp i den **västra USA** plats. Detta måste göras innan du skapar den första virtuella dator som ska ingå i uppsättningen.

Innan du börjar bör du kontrollera att du har den senaste versionen av AzureRM.Compute PowerShell-modulen. Kör följande kommando för att installera den.

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
* När du skapar en virtuell dator, om det inte finns i den nedrullningsbara listan i portalen tillgänglighetsuppsättningen som du vill kan du skapade den i en annan resursgrupp. Om du inte vet resursgruppen för din tillgänglighet ange, gå till hubbmenyn och klicka på Bläddra > tillgänglighetsuppsättningar för att se en lista över dina tillgänglighetsuppsättningar, och resursgrupper som de tillhör.

## <a name="next-steps"></a>Nästa steg
Lägga till ytterligare lagringsutrymme till den virtuella datorn genom att lägga till ytterligare [datadisk](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

