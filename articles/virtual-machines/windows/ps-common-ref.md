---
title: "aaaCommon PowerShell-kommandon för Azure Virtual Machines | Microsoft Docs"
description: Vanliga PowerShell-kommandon tooget du startade skapa och hantera dina virtuella Windows-datorer i Azure.
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: ba3839a2-f3d5-4e19-a5de-95bfb1c0e61e
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/17/2017
ms.author: davidmu
ms.openlocfilehash: 3de9bd4d20259ced2c0aa8ef7a3f7d9520a071d0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="common-powershell-commands-for-creating-and-managing-azure-virtual-machines"></a>Vanliga PowerShell-kommandon för att skapa och hantera virtuella datorer i Azure

Den här artikeln beskrivs några av hello Azure PowerShell-kommandon som du kan använda toocreate och hantera virtuella datorer i din Azure-prenumeration.  Mer detaljerad hjälp med specifika kommandoradsväxlar och alternativ, kan du använda **Get-Help** *kommandot*.

Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) information om installation hello senaste versionen av Azure PowerShell, välja din prenumeration och loggar in tooyour konto.

Dessa variabler kan vara praktiskt om körs mer än en av hello kommandon i den här artikeln:

- $location - hello platsen för hello virtuella datorn. Du kan använda [Get-AzureRmLocation](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermlocation) toofind en [geografisk region](https://azure.microsoft.com/regions/) som passar dig.
- $myResourceGroup - hello namnet på hello resursgruppen som innehåller hello virtuell dator.
- $myVM - hello hello virtuella datorns namn.

## <a name="create-a-vm"></a>Skapa en virtuell dator

| Aktivitet | Kommando |
| ---- | ------- |
| Skapa en VM-konfiguration |$vm = [ny AzureRmVMConfig](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvmconfig) - VMName $myVM - VMSize ”Standard_D1_v1”<BR></BR><BR></BR>hello VM-konfiguration är använda toodefine eller uppdatera inställningarna för hello VM. hello-konfigurationen har initierats med hello namnet hello VM och dess [storlek](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). |
| Lägg till konfigurationsinställningar |$vm = [set AzureRmVMOperatingSystem](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) - VM $vm-Windows - ComputerName $myVM-autentiseringsuppgifter $cred - ProvisionVMAgent - EnableAutoUpdate<BR></BR><BR></BR>Inställningar för operativsystem inklusive [autentiseringsuppgifter](https://technet.microsoft.com/library/hh849815.aspx) läggs toohello konfigurationsobjekt som du skapat med hjälp av ny AzureRmVMConfig. |
| Lägg till ett nätverksgränssnitt |$vm = [Lägg till AzureRmVMNetworkInterface](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.5.0/Add-AzureRmVMNetworkInterface) - VM $vm-Id $nic. ID<BR></BR><BR></BR>En virtuell dator måste ha en [nätverksgränssnittet](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toocommunicate i ett virtuellt nätverk. Du kan också använda [Get-AzureRmNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) tooretrieve ett befintligt nätverk gränssnittet objekt. |
| Ange en plattformsavbildning |$vm = [set AzureRmVMSourceImage](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmsourceimage) - VM $vm - PublisherName ”publisher_name”-erbjudande ”publisher_offer” - SKU: er ”product_sku”-”senaste” Version<BR></BR><BR></BR>[Bild information](cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) läggs toohello konfigurationsobjekt som du skapat med hjälp av ny AzureRmVMConfig. hello-objektet som returneras från det här kommandot används bara när du ställer in hello OS disk toouse en plattformsavbildning. |
| Ange en plattformsavbildning för toouse för OS-disk |$vm = [set AzureRmVMOSDisk](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmosdisk) - VM $vm-http://mystore1.blob.core.windows.net/vhds/myOSDisk.vhd ”namn” myOSDisk ”- VhdUri” - CreateOption FromImage<BR></BR><BR></BR>hello namn hello operativsystemdisken och dess plats i [lagring](../../storage/common/storage-powershell-guide-full.md) läggs toohello konfigurationsobjekt som du skapade tidigare. |
| Ange en generaliserad avbildning för toouse för OS-disk |$vm = AzureRmVMOSDisk set - VM $vm-https://mystore1.blob.core.windows.net/system/Microsoft.Compute/Images/myimages/myprefix-osDisk ”namn” myOSDisk ”- SourceImageUri. {guid} VHD ”- VhdUri” https://mystore1.blob.core.windows.net/vhds/disk_name.vhd ”- CreateOption FromImage-Windows<BR></BR><BR></BR>hello namn hello operativsystemdisk, hello platsen för hello Källavbildningen och hello disk plats i [lagring](../../storage/common/storage-powershell-guide-full.md) läggs toohello konfigurationsobjektet. |
| Ange en specialiserad avbildning för toouse för OS-disk |$vm = AzureRmVMOSDisk set - VM $vm-http://mystore1.blob.core.windows.net/vhds/ ”namn” myOSDisk ”- VhdUri” - CreateOption bifoga - Windows |
| Skapa en virtuell dator |[Nya AzureRmVM]() - ResourceGroupName $myResourceGroup-plats $location - VM $vm<BR></BR><BR></BR>Alla resurser skapas i en [resursgruppen](../../azure-resource-manager/powershell-azure-resource-manager.md). Innan du kör det här kommandot Kör New-AzureRmVMConfig, Set-AzureRmVMOperatingSystem, Set-AzureRmVMSourceImage, Lägg till AzureRmVMNetworkInterface och Set-AzureRmVMOSDisk. |

## <a name="get-information-about-vms"></a>Hämta information om virtuella datorer

| Aktivitet | Kommando |
| ---- | ------- |
| Lista över virtuella datorer i en prenumeration |[Get-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvm) |
| Lista över virtuella datorer i en resursgrupp |Get-AzureRmVM - ResourceGroupName $myResourceGroup<BR></BR><BR></BR>tooget en lista över resurs grupper i din prenumeration, använder [Get-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermresourcegroup). |
| Hämta information om en virtuell dator |Get-AzureRmVM - ResourceGroupName $myResourceGroup-Name $myVM |

## <a name="manage-vms"></a>Hantera virtuella datorer
| Aktivitet | Kommando |
| --- | --- |
| Starta en virtuell dator |[Start-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/start-azurermvm) - ResourceGroupName $myResourceGroup-Name $myVM |
| Stoppa en virtuell dator |[Stoppa AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/stop-azurermvm) - ResourceGroupName $myResourceGroup-Name $myVM |
| Starta om en aktiv virtuell dator |[Starta om AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/restart-azurermvm) - ResourceGroupName $myResourceGroup-Name $myVM |
| Ta bort en virtuell dator |[Ta bort AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/remove-azurermvm) - ResourceGroupName $myResourceGroup-Name $myVM |
| Generalisera en virtuell dator |[Ange AzureRmVm](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvm) - ResourceGroupName $myResourceGroup-Name $myVM-generaliserad<BR></BR><BR></BR>Kör kommandot innan du kör spara AzureRmVMImage. |
| Avbilda en virtuell dator |[Spara AzureRmVMImage](https://docs.microsoft.com/powershell/module/azurerm.compute/save-azurermvmimage) - ResourceGroupName $myResourceGroup - VMName $myVM - DestinationContainerName ”myImageContainer” - VHDNamePrefix ”myImagePrefix”-sökvägen ”C:\filepath\filename.json”<BR></BR><BR></BR>En virtuell dator måste vara [förberedd, Stäng och generaliserad](prepare-for-upload-vhd-image.md) toobe används toocreate en bild. Innan du kör det här kommandot Kör Set-AzureRmVm. |
| Uppdatera en virtuell dator |[Uppdatera AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvm) - ResourceGroupName $myResourceGroup - VM $vm<BR></BR><BR></BR>Hämta hello aktuella VM-konfigurationen med hjälp av Get-AzureRmVM, ändra konfigurationsinställningarna på hello VM-objekt och sedan köra kommandot. |
| Lägg till en data disk tooa VM |[Lägg till AzureRmVMDataDisk](https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvmdatadisk) - VM $vm-https://mystore1.blob.core.windows.net/vhds/myDataDisk.vhd ”namn” myDataDisk ”- VhdUri” - LUN #-cachelagring ReadWrite - DiskSizeinGB # - CreateOption är tom<BR></BR><BR></BR>Använd Get-AzureRmVM tooget hello VM-objekt. Ange hello LUN-nummer och hello hello diskens storlek. Kör Update-AzureRmVM tooapply hello configuration ändringar toohello VM. hello-disk som du lägger till har inte initierats. |
| Ta bort en datadisk från en virtuell dator |[Ta bort AzureRmVMDataDisk](https://docs.microsoft.com/powershell/module/azurerm.compute/remove-azurermvmdatadisk) - VM $vm-Name ”myDataDisk”<BR></BR><BR></BR>Använd Get-AzureRmVM tooget hello VM-objekt. Kör Update-AzureRmVM tooapply hello configuration ändringar toohello VM. |
| Lägg till ett tillägg tooa VM |[Ange AzureRmVMExtension](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmextension) - ResourceGroupName $myResourceGroup-plats $location - VMName $myVM-Name ”Tilläggsnamn”-Publisher ”publisherName”-typ ”extensionType” - TypeHandlerVersion ”#. #”-inställningar $Settings - ProtectedSettings $ProtectedSettings<BR></BR><BR></BR>Kör kommandot med lämpliga hello [konfigurationsinformation](template-description.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#extensions) för hello-tillägg som du vill tooinstall. |
| Ta bort ett virtuellt datortillägg |[Ta bort AzureRmVMExtension](https://docs.microsoft.com/powershell/module/azurerm.compute/remove-azurermvmextension) - ResourceGroupName $myResourceGroup-Name ”Tilläggsnamn” - VMName $myVM |

## <a name="next-steps"></a>Nästa steg
* Se hello grundläggande steg för att skapa en virtuell dator i [skapa en virtuell Windows-dator med Resource Manager och PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

