---
title: "aaaWindows virtuella datorer – översikt | Microsoft Docs"
description: "Läs mer om hur du skapar och hanterar virtuella Windows-datorer i Azure."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: fbae9c8e-2341-4ed0-bb20-fd4debb2f9ca
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: davidmu
ms.custom: mvc
ms.openlocfilehash: 8015b1aa4bcdaac2e721f25d85a5bc995a22f0f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-windows-virtual-machines-in-azure"></a>Översikt över virtuella Windows-datorer i Azure

Virtuella Azure-datorer (Virtual Machines, VM) är en av flera typer av [behovsbaserade och skalbara datorresurser](../../app-service-web/choose-web-site-cloud-service-vm.md) som Azure erbjuder. Vanligtvis väljer du en virtuell dator när du behöver mer kontroll över hello datormiljö än hello andra alternativen erbjuder. Den här artikeln innehåller information om vad du bör tänka på innan du skapar en virtuell dator, hur du skapar den och hur du hanterar den.

En virtuell dator i Azure ger du hello virtualiseringsflexibilitet utan toobuy och underhålla hello fysisk maskinvara som kör den. Behöver du dock fortfarande toomaintain hello VM genom att utföra åtgärder, till exempel konfigurera, korrigera och installera hello-programvara som körs på den.

Virtuella datorer i Azure kan användas på olika sätt. Några exempel är:

* **Utveckling och testning** – Azure Virtual Machines erbjuder ett snabbt och enkelt sätt toocreate en dator med specifika konfigurationer som krävs för toocode och testa ett program.
* **Program i molnet hello** – eftersom begäran för ditt program kan variera, kan det göra ekonomiskt meningsfullt toorun den på en virtuell dator i Azure. Du betalar extra för virtuella datorer när du behöver dem och stänger av dem när du inte behöver dem.
* **Utökad datacenter** – virtuella datorer i en Azure-nätverket enkelt kan anslutna tooyour organisationens nätverk.

hello antalet virtuella datorer som används i ditt program kan skala upp och ut toowhatever är obligatoriska toomeet dina behov.

## <a name="what-do-i-need-toothink-about-before-creating-a-vm"></a>Vad behöver jag toothink om innan du skapar en virtuell dator?
Det finns alltid en rad [överväganden vid utformning](/architecture/reference-architectures/virtual-machines-linux?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) när du utökar en programinfrastruktur i Azure. Dessa aspekter av en virtuell dator är viktiga toothink om innan du börjar:

* hello namnen på dina programresurser
* hello plats där hello resurser lagras
* hello storleken på hello VM
* hello maximalt antal virtuella datorer som kan skapas
* hello-operativsystemet som hello VM körs
* hello konfigurationen av hello VM när den har startat
* hello relaterade resurser som hello måste VM

### <a name="naming"></a>Namngivning
En virtuell dator har en [namn](/architecture/best-practices/naming-conventions#naming-rules-and-restrictions?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) tilldelade tooit och den har ett datornamn som konfigurerats som en del av hello-operativsystemet. hello namnet på en virtuell dator kan vara upp too15 tecken.

Om du använder Azure toocreate hello operativsystemdisk, hello datornamn och hello virtuella datornamn är hello samma. Om du [överföra och använda en egen avbildning](upload-generalized-managed.md) som innehåller ett tidigare konfigurerade operativsystem och använda den toocreate en virtuell dator, hello namn kan vara olika. Vi rekommenderar att när du överför dina egna avbildningsfil du gör hello datornamnet i hello operativsystemet och hello virtuellt datornamn hello samma.

### <a name="locations"></a>Platser
Alla resurser som skapats i Azure är fördelade på flera [geografiska regioner](https://azure.microsoft.com/regions/) hello världen. Vanligtvis hello region kallas **plats** när du skapar en virtuell dator. För en virtuell anger hello plats där hello virtuella hårddiskarna lagras.

Den här tabellen visar några av hello sätt som du kan hämta en lista över tillgängliga platser.

| Metod | Beskrivning |
| --- | --- |
| Azure Portal |Välj en plats hello listan när du skapar en virtuell dator. |
| Azure PowerShell |Använd hello [Get-AzureRmLocation](/powershell/module/azurerm.resources/get-azurermlocation) kommando. |
| REST API |Använd hello [listan platser](https://docs.microsoft.com/rest/api/resources/subscriptions#Subscriptions_ListLocations) igen. |

### <a name="vm-size"></a>Storlek på virtuell dator
Hej [storlek](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) av hello VM som du använder bestäms av hello arbetsbelastning som du vill toorun. hello storlek som du väljer bestämmer faktorer, till exempel power, minne och lagringsutrymme processorkapaciteten. Azure erbjuder en mängd olika storlekar toosupport många typer av använder.

Azure avgifter en [pris per timme](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) baserat på hello VM storleken och operativsystemet. För delar av timmar tar Azure debiterar endast för hello minuter. Lagringsutrymme prissätts och debiteras separat.

### <a name="vm-limits"></a>Begränsningar för den virtuella datorn
Din prenumeration har standard [kvotgränserna](../../azure-subscription-service-limits.md) som kan påverka hello distribution av många virtuella datorer för projektet. hello aktuella gränsen på grundval av per prenumeration är 20 virtuella datorer per region. Begränsningen kan ökas om du anmäler ett supportärende och begär en ökning.

### <a name="operating-system-disks-and-images"></a>Operativsystemsdiskar och avbildningar
Virtuella datorer använda [virtuella hårddiskar (VHD)](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toostore deras data och operativsystem (OS). Virtuella hårddiskar används också för hello bilder som du kan välja mellan tooinstall ett operativsystem. 

Azure tillhandahåller många [marketplace-bilder](https://azure.microsoft.com/marketplace/virtual-machines/) toouse med olika versioner och typer av Windows Server-operativsystem. Marketplace-avbildningar identifieras av avbildningens utgivare, erbjudande, sku och version (vanligtvis anges versionen som den senaste). 

Den här tabellen visar några metoder som du kan hitta hello information för en bild.

| Metod | Beskrivning |
| --- | --- |
| Azure Portal |hello värdena anges automatiskt åt dig när du väljer en bild toouse. |
| Azure PowerShell |[Get-AzureRMVMImagePublisher](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.5.0/get-azurermvmimagepublisher) -Location "location"<BR>[Get-AzureRMVMImageOffer](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.5.0/get-azurermvmimageoffer) -Location "location" -Publisher "publisherName"<BR>[Get-AzureRMVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) -Location "location" -Publisher "publisherName" -Offer "offerName" |
| REST API:er |[Lista över avbildningsutgivare](https://docs.microsoft.com/rest/api/compute/platformimages/platformimages-list-publishers)<BR>[Lista över avbildningserbjudanden](https://docs.microsoft.com/rest/api/compute/platformimages/platformimages-list-publisher-offers)<BR>[Lista över avbildnings-SKU:er](https://docs.microsoft.com/rest/api/compute/platformimages/platformimages-list-publisher-offer-skus) |

Du kan välja för[överföra och använda en egen avbildning](upload-generalized-managed.md#upload-the-vhd-to-your-storage-account) och när du gör hello Utgivarnamn, erbjudande och sku inte används.

### <a name="extensions"></a>Tillägg
[Tillägg](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) för virtuella datorer ger din virtuella dator fler funktioner genom konfiguration efter distribution och automatiserade uppgifter.

Dessa vanliga uppgifter kan utföras med hjälp av tillägg:

* **Köra anpassade skript** – hello [tillägget för anpassat skript](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) hjälper dig att konfigurera arbetsbelastningar på hello VM genom att köra skriptet när hello VM etableras.
* **Distribuera och hantera konfigurationer** – hello [PowerShell önskad tillstånd Configuration DSC ()-tillägget](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) kan du konfigurera DSC på en VM-toomanage konfigurationer och miljöer.
* **Samla in diagnostikdata** – hello [Azure Diagnostics tillägget](extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) kan du konfigurera hello VM toocollect diagnostikdata som kan använda toomonitor hello hälsotillståndet för programmet.

### <a name="related-resources"></a>Relaterade resurser
hello resurser i den här tabellen används av hello VM och behöver tooexist eller skapas när hello VM skapas.

| Resurs | Krävs | Beskrivning |
| --- | --- | --- |
| [Resursgrupp](../../azure-resource-manager/resource-group-overview.md) |Ja |hello VM måste finnas i en resursgrupp. |
| [Lagringskonto](../../storage/common/storage-create-storage-account.md) |Ja |hello VM måste hello storage-konto toostore datorns virtuella hårddiskar. |
| [Virtuellt nätverk](../../virtual-network/virtual-networks-overview.md) |Ja |hello VM måste vara medlem i ett virtuellt nätverk. |
| [Offentlig IP-adress](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) |Nej |hello VM kan ha en offentlig IP-adress som tilldelats tooit tooremotely åtkomst till den. |
| [Nätverksgränssnitt](../../virtual-network/virtual-network-network-interface.md) |Ja |hello VM måste hello network interface toocommunicate i hello nätverk. |
| [Datadiskar](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |Nej |hello VM kan innehålla data diskar tooexpand lagringskapacitet. |

## <a name="how-do-i-create-my-first-vm"></a>Hur skapar jag min första virtuella dator?
Du har flera valmöjligheter när du skapar en virtuell dator. hello val som du gör beror på hello-miljö i. 

Den här tabellen innehåller information tooget du igång med att skapa den virtuella datorn.

| Metod | Artikel |
| --- | --- |
| Azure Portal |[Skapa en virtuell dator som kör Windows med hjälp av hello portal](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| Mallar |[Skapa en virtuell Windows-dator med en Resource Manager-mall](ps-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| Azure PowerShell |[Skapa en virtuell Windows-dator med PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| Klient-SDK: er |[Distribuera Azure-resurser med C#](csharp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| REST API:er |[Skapa eller uppdatera en virtuell dator](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-create-or-update) |

Man hoppas att det aldrig händer, men ibland går något fel. Om detta händer tooyou, titta på hello i [felsöka Resource Manager distributionsproblem med att skapa en virtuell Windows-dator i Azure](troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="how-do-i-manage-hello-vm-that-i-created"></a>Hur hanterar hello VM som jag har skapat?
Virtuella datorer kan hanteras från en webbläsarbaserad portal, kommandoradsverktyg med skriptstöd eller direkt från API:erna. Några vanliga hanteringsuppgifter som du kan utföra får information om en virtuell dator, logga in tooa VM, hantera tillgänglighet och säkerhetskopieringar.

### <a name="get-information-about-a-vm"></a>Hämta information om en virtuell dator
Den här tabellen visar några av hello sätt som du kan få information om en virtuell dator.

| Metod | Beskrivning |
| --- | --- |
| Azure Portal |Hej hubbmenyn, klicka på **virtuella datorer** och välj sedan hello VM hello-listan. På hello bladet för hello VM har du åtkomst toooverview information, inställningsvärden och övervakning mått. |
| Azure PowerShell |Information om hur du använder PowerShell toomanage virtuella datorer finns i [skapa och hantera virtuella Windows-datorer med hello Azure PowerShell-modulen](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). |
| REST API |Använd hello [hämta VM information](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-get) åtgärden tooget information om en virtuell dator. |
| Klient-SDK: er |Information om hur du använder C# toomanage virtuella datorer finns i [hantera virtuella datorer i Azure med Azure Resource Manager och C#](csharp-manage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). |

### <a name="log-on-toohello-vm"></a>Logga in toohello VM
Du använder hello Connect-knappen i hello Azure-portalen för[starta en session med fjärrskrivbord (RDP)](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Något kan ibland går fel vid försök toouse en fjärranslutning. Om detta händer tooyou, kolla hello hjälpinformation i [Felsöka fjärrskrivbord anslutningar tooan Azure virtuell dator som kör Windows](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="manage-availability"></a>Hantera tillgänglighet
Det är viktigt att du toounderstand hur för[garantera hög tillgänglighet](manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) för ditt program. Den här konfigurationen innebär att du skapar flera virtuella datorer tooensure som minst körs.

För din distribution tooqualify för våra 99.95 serviceavtal för VM måste toodeploy två eller flera virtuella datorer som körs din arbetsbelastning i ett [tillgänglighetsuppsättning](tutorial-availability-sets.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Den här konfigurationen säkerställer att dina virtuella datorer distribueras via flera feldomäner och på värdar med olika underhållsfönster. hello fullständig [Azure-serviceavtalet](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/) förklarar hello garanteras tillgänglighet för Azure som helhet.

### <a name="back-up-hello-vm"></a>Säkerhetskopiera hello VM
En [Recovery Services-valvet](../../backup/backup-introduction-to-azure-backup.md) är används tooprotect data och tillgångar i både Azure Backup och Azure Site Recovery services. Du kan använda ett Recovery Services-valv för[distribuera och hantera säkerhetskopior för Resource Manager distribuerade virtuella datorer med hjälp av PowerShell](../../backup/backup-azure-vms-automation.md). 

## <a name="next-steps"></a>Nästa steg
* Om din avsikt är toowork med Linux virtuella datorer, titta på [Azure- och Linux](../linux/overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Mer information om hello riktlinjer kring konfigurera infrastrukturen i hello [exempel Azure infrastructure genomgången](infrastructure-example.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
