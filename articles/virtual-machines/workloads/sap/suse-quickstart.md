---
title: "aaaTesting SAP NetWeaver på Microsoft Azure SUSE Linux Virtual Machines | Microsoft Docs"
description: "Testa SAP NetWeaver på Microsoft Azure virtuella SUSE Linux-datorer"
services: virtual-machines-linux
documentationcenter: 
author: hermanndms
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 645e358b-3ca1-4d3d-bf70-b0f287498d7a
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/14/2017
ms.author: hermannd
ms.openlocfilehash: 0e3faab05417a1a15541e2b79aa7eddacda44611
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="running-sap-netweaver-on-microsoft-azure-suse-linux-vms"></a>Köra SAP NetWeaver på virtuella SUSE Linux-datorer i Microsoft Azure
Den här artikeln beskrivs olika saker tooconsider när du kör SAP NetWeaver på Microsoft Azure SUSE Linux virtuella datorer (VM). Kan 19 2016 stöds officiellt SAP NetWeaver på SUSE Linux virtuella datorer i Azure. All information om Linux-versionerna, SAP kernel-versioner och så vidare kan hittas i SAP Obs 1928533 ”SAP-program i Azure: produkter som stöds och Azure VM typer”.
Ytterligare dokumentation om SAP på virtuella Linux-datorer finns här: [med SAP på Linux virtuella datorer (VM)](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

hello följande information hjälper dig att undvika vissa fallgropar.

## <a name="suse-images-on-azure-for-running-sap"></a>SUSE avbildningar i Azure för att köra SAP
För att köra SAP NetWeaver på Azure, Använd endast SUSE Linux Enterprise Server SLES 12 (SPx) – Se även SAP-kommentar 1928533. En särskild SUSE avbildning finns i hello Azure Marketplace (”SLES 11 SP3 för SAP CAL”), men detta är inte avsedd för användning. Använd inte den här avbildningen eftersom den är reserverad för hello [SAP Molnbibliotek installation](https://cal.sap.com/) lösning.  

Du bör använda Azure Resource Manager för alla nya kontroller och installationer i Azure. toolook för SUSE SLES avbildningar och versioner genom att använda Azure PowerShell eller hello Azure-kommandoradsgränssnittet (CLI), Använd hello följande kommandon. Du kan sedan använda hello utdata, till exempel toodefine hello OS-avbildningen i en JSON-mall för att distribuera en ny SUSE Linux virtuell dator.
Dessa PowerShell-kommandon är giltig för Azure PowerShell version 1.0.1 och senare.

Även om det är fortfarande möjligt toouse hello standard SLES avbildningar för SAP installationer rekommenderas toomake användning av hello nya SLES för SAP-avbildningar som är tillgängliga nu på hello Azure bild-galleriet. Mer information om dessa avbildningar kan hittas i hello motsvarande [Azure Marketplace sidan]( https://azuremarketplace.microsoft.com/en-us/marketplace/apps/SUSE.SLES-SAP ) eller hello [webbsida för SUSE vanliga frågor och svar om SLES SAP]( https://www.suse.com/products/sles-for-sap/frequently-asked-questions/ ).


* Sök efter befintliga utgivare, inklusive SUSE:
  
   ```
   PS  : Get-AzureRmVMImagePublisher -Location "West Europe"  | where-object { $_.publishername -like "*US*"  }
   CLI : azure vm image list-publishers westeurope | grep "US"
   ```
* Sök efter befintliga erbjudanden från SUSE:
  
   ```
   PS  : Get-AzureRmVMImageOffer -Location "West Europe" -Publisher "SUSE"
   CLI : azure vm image list-offers westeurope SUSE
   ```
* Leta efter SUSE SLES erbjudanden:
  
   ```
   PS  : Get-AzureRmVMImageSku -Location "West Europe" -Publisher "SUSE" -Offer "SLES"
   PS  : Get-AzureRmVMImageSku -Location "West Europe" -Publisher "SUSE" -Offer "SLES-SAP"
   CLI : azure vm image list-skus westeurope SUSE SLES
   CLI : azure vm image list-skus westeurope SUSE SLES-SAP
   ```
* Leta efter en viss version av en SLES SKU:
  
   ```
   PS  : Get-AzureRmVMImage -Location "West Europe" -Publisher "SUSE" -Offer "SLES" -skus "12-SP2"
   PS  : Get-AzureRmVMImage -Location "West Europe" -Publisher "SUSE" -Offer "SLES-SAP" -skus "12-SP2"
   CLI : azure vm image list westeurope SUSE SLES 12-SP2
   CLI : azure vm image list westeurope SUSE SLES-SAP 12-SP2
   ```

## <a name="installing-walinuxagent-in-a-suse-vm"></a>Installera WALinuxAgent på en virtuell dator SUSE
hello agent kallas WALinuxAgent är en del av hello SLES bilder i hello Azure Marketplace. Information om hur du installerar den manuellt (till exempel när du överför en SLES OS virtuell hårddisk (VHD) från lokala) finns i:

* [OpenSUSE](http://software.opensuse.org/package/WALinuxAgent)
* [Azure](../../linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [SUSE](https://www.suse.com/communities/blog/suse-linux-enterprise-server-configuration-for-windows-azure/)

## <a name="sap-enhanced-monitoring"></a>SAP ”förbättrad övervakning”
SAP ”förbättrad övervakning” är en obligatorisk nödvändiga toorun SAP på Azure. Kontrollera informationen i SAP Observera 2191498 ”SAP på Linux med Azure: förbättrad övervakning”.

## <a name="attaching-azure-data-disks-tooan-azure-linux-vm"></a>Koppla Azure data diskar tooan virtuella Azure Linux-datorn
Du bör aldrig montera Azure data diskar tooan virtuella Azure Linux-datorn med hjälp av hello enhets-ID. Använd i stället hello universellt Unik identifierare (UUID). Var försiktig när du använder grafiska verktyg toomount Azure datadiskar, t.ex. Kontrollera hello poster i /etc/fstab.

hello problemet med hello enhets-ID är att den kan ändras och sedan hello Azure VM kan hänga i hello startprocessen. toomitigate hello problem du kan lägga till hello nofail parameter i /etc/fstab. Men var försiktig med nofail eftersom program kan använda hello monteringspunkt som innan och kan skriva till hello rot-filsystem om en extern Azure-datadisk inte monterade under hello start.

hello endast undantag toomounting via UUID kopplar en OS-disk i felsökningssyfte, enligt beskrivningen i följande avsnitt hello.

## <a name="troubleshooting-a-suse-vm-that-isnt-accessible-anymore"></a>Felsöka en SUSE VM som inte är tillgänglig längre
Det kan finnas situationer där en SUSE VM på Azure låser sig i hello startprocessen (till exempel med ett fel som rör montera diskar). Du kan kontrollera det här problemet genom att använda hello Start-diagnostik för virtuella datorer i Azure v2 i hello Azure-portalen. Mer information finns i [starta diagnostik](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/).

Enkelriktade toosolve hello problemet är tooattach hello OS-disken från hello skadad VM tooanother SUSE VM på Azure. Sedan göra ändringar som redigera /etc/fstab eller ta bort nätverket udev regler som beskrivs i nästa avsnitt om hello.

Det finns en viktiga tooconsider. Distribuera flera SUSE virtuella datorer från samma Azure Marketplace-avbildning (exempelvis SLES 11 SP4) medför hello hello OS disk tooalways monteras av hello samma UUID. Därför använder hello UUID tooattach en OS-disk från en annan virtuell dator som har distribuerats med hjälp av hello samma Azure Marketplace-avbildning leder till att två identiska UUID: er. Detta orsakar problem och kan innebära att hello VM avsett för felsökning i själva verket startar från hello ansluten och skadade OS-disken i stället för hello ursprungliga.

Det finns två sätt tooavoid detta:

* Använd en annan Azure Marketplace-avbildning för hello felsökning VM (till exempel SLES 11 SPx i stället för SLES 12).
* Inte koppla hello skadad OS-disk från en annan virtuell dator med hjälp av UUID--Använd något annat.

## <a name="uploading-a-suse-vm-from-on-premises-tooazure"></a>Ladda upp en SUSE VM från lokala tooAzure
En beskrivning av hello steg tooupload en SUSE VM från lokala tooAzure finns [förbereda en virtuell dator SLES eller openSUSE för Azure](../../linux/suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Om du vill att en virtuell dator utan hello avetablering steg slutet hello (till exempel tookeep en befintlig installation av SAP, samt hello värdnamn) tooupload Kontrollera hello följande objekt:

* Kontrollera att hello OS-disken är monterad med hjälp av UUID och inte hello enhets-ID. Ändra tooUUID just-in-/etc/fstab räcker inte för hello OS-disken. Dessutom inte glömma tooadapt hello-startprogram via YaST eller genom att redigera /boot/grub/menu.lst.
* Om du använder hello VHDX-format för hello SUSE OS-disken och konvertera det tooVHD för uppladdning av tooAzure, är det mycket troligt att hello nätverksenhet kommer att ändras från eth0 tooeth1. tooavoid problem när du startar på Azure senare ändra tillbaka tooeth0 enligt beskrivningen i [korrigerat eth0 i klonas SLES 11 VMware](https://dartron.wordpress.com/2013/09/27/fixing-eth1-in-cloned-sles-11-vmware/).

Dessutom toowhat's beskrivs i artikel hello rekommenderar vi att du tar bort det här:

   /lib/udev/rules.d/75-persistent-NET-Generator.rules

Du kan också installera hello Azure Linux-agenten (waagent) toohelp du undvika problem, så länge det inte finns flera nätverkskort.

## <a name="deploying-a-suse-vm-on-azure"></a>Distribuera en SUSE virtuell dator på Azure
Du bör skapa nya SUSE virtuella datorer med hjälp av JSON-mallfilerna i hello nya Azure Resource Manager-modellen. När mallen hello JSON-fil har skapats kan distribuera du hello VM med hjälp av följande CLI-kommando som ett alternativt tooPowerShell hello:

   ```
   azure group deployment create "<deployment name>" -g "<resource group name>" --template-file "<../../filename.json>"

   ```
Mer information om JSON-mallfilerna finns [redigera Azure Resource Manager-mallar](../../../resource-group-authoring-templates.md) och [Azure snabbstartsmallar](https://azure.microsoft.com/documentation/templates/).

Mer information om CLI och Azure Resource Manager finns [Använd hello Azure CLI för Mac, Linux och Windows med Azure Resource Manager](../../../xplat-cli-azure-resource-manager.md).

## <a name="sap-license-and-hardware-key"></a>Nyckeln för SAP licens och maskinvara
För hello officiell SAP-Azure-certifiering var en ny mekanism introducerades toocalculate hello SAP maskinvarunyckel som används för hello SAP-licens. hello SAP kernel hade toobe anpassas toomake användning av detta. Tidigare SAP kernel-versioner för Linux innehöll inte den här koden ändras. Därför kan i vissa situationer (till exempel Azure VM storleksändring), hello SAP maskinvarunyckel ändras och hello SAP-licens har inte längre är giltig. Detta kan lösas i hello senaste SAP Linux kärnor. Mer information finns i SAP-kommentar 1928533.

## <a name="suse-sapconf-package--tuned-adm"></a>SUSE sapconf paket / justerade adm
SUSE innehåller ett paket som kallas ”sapconf” som hanterar en uppsättning SAP-specifika inställningar. För mer information om vad det här paketet matchar och hur tooinstall och använda den, se [med sapconf tooprepare SUSE Linux Enterprise Server toorun SAP-system](https://www.suse.com/communities/blog/using-sapconf-to-prepare-suse-linux-enterprise-server-to-run-sap-systems/) och [vad är sapconf eller hur tooprepare en SUSE Linux Enterprise Server för att köra SAP-system? ](http://scn.sap.com/community/linux/blog/2014/03/31/what-is-sapconf-or-how-to-prepare-a-suse-linux-enterprise-server-for-running-sap-systems).

Hello tiden finns det ett nytt verktyg som ersätter sapconf - justerade adm. En kan hitta mer information om det här verktyget efter hello två länkarna nedan.

SLES dokumentation om justerade adm profil sap-hana finns [här](https://www.suse.com/documentation/sles-for-sap-12/book_s4s/data/sec_s4s_configure_sapconf.html) 

Prestandajustering system för SAP-arbetsbelastningar med justerade adm - hittar [här](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/book_s4s/book_s4s.pdf) i kapitlet 6.2

## <a name="nfs-share-in-distributed-sap-installations"></a>NFS-resursens i distribuerade SAP-installationer
Om du har en distribuerad installation – kan där du vill tooinstall hello databasen och hello SAP programservrar i olika virtuella datorer – du dela hello /sapmnt directory via Network File System (NFS). Om du har problem med hello installationssteg när du har skapat hello NFS-resursens för /sapmnt Kontrollera toosee om ”no_root_squash” anges för hello resurs.

## <a name="logical-volumes"></a>Logiska volymer
I tidigare hello om en krävs en stor logisk volym över flera diskar i Azure data (till exempel för hello SAP-databasen), rekommenderades toouse mdadm som lvm inte helt har verifierats ännu i Azure. hur tooset in Linux RAID på Azure med hjälp av mdadm, se toolearn [konfigurera programvarubaserad RAID på Linux](../../linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). I hello tiden från och med början av maj 2016 även lvm stöds helt i Azure och kan användas som ett alternativt toomdadm. Mer information om lvm i Azure finns [konfigurera LVM på en Linux-VM i Azure](../../linux/configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="azure-suse-repository"></a>Azure SUSE databasen
Om du har ett problem med åtkomst toohello standard Azure SUSE databasen kan du använda ett enkelt kommando tooreset den. Detta kan inträffa om du skapar ett privat OS-avbildningen i en Azure-region och sedan kopiera hello avbildningen tooa annan region där du vill att toodeploy nya virtuella datorer som är baserade på det här privata OS-avbildningen. Kör du följande kommando i hello VM hello bara:

   ```
   service guestregister restart
   ```

## <a name="gnome-desktop"></a>Gör väldigt lätt desktop
Om du vill toouse hello gör väldigt lätt skrivbord tooinstall ett komplett SAP demo-system i en enda virtuell dator – inklusive GUI SAP, webbläsare och SAP-hanteringskonsolen--använder den här tipset tooinstall på hello Azure SLES bilder:

   För SLES 11:

   ```
   zypper in -t pattern gnome
   ```

   För SLES 12:

   ```
   zypper in -t pattern gnome-basic
   ```

## <a name="sap-support-for-oracle-on-linux-in-hello-cloud"></a>SAP-stöd för Oracle på Linux i hello moln
Det finns en begränsning med stöd från Oracle på Linux i virtualiserade miljöer. Även om detta inte är en Azure-specifika avsnittet, är viktiga toounderstand. Oracle stöder inte på SUSE eller Red Hat i ett offentligt moln som Azure SAP. toodiscuss det här avsnittet, kontakta Oracle direkt.

