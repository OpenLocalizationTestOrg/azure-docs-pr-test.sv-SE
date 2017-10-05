---
title: "Testa SAP NetWeaver på Microsoft Azure SUSE Linux Virtual Machines | Microsoft Docs"
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
ms.openlocfilehash: 118b56376eace80788a20625497849181ad2e253
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="running-sap-netweaver-on-microsoft-azure-suse-linux-vms"></a>Köra SAP NetWeaver på virtuella SUSE Linux-datorer i Microsoft Azure
Den här artikeln beskrivs olika saker att tänka på när du kör SAP NetWeaver på Microsoft Azure SUSE Linux virtuella datorer (VM). Kan 19 2016 stöds officiellt SAP NetWeaver på SUSE Linux virtuella datorer i Azure. All information om Linux-versionerna, SAP kernel-versioner och så vidare kan hittas i SAP Obs 1928533 ”SAP-program i Azure: produkter som stöds och Azure VM typer”.
Ytterligare dokumentation om SAP på virtuella Linux-datorer finns här: [med SAP på Linux virtuella datorer (VM)](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Följande information hjälper dig att undvika vissa fallgropar.

## <a name="suse-images-on-azure-for-running-sap"></a>SUSE avbildningar i Azure för att köra SAP
För att köra SAP NetWeaver på Azure, Använd endast SUSE Linux Enterprise Server SLES 12 (SPx) – Se även SAP-kommentar 1928533. En särskild SUSE avbildning finns i Azure Marketplace (”SLES 11 SP3 för SAP CAL”), men detta är inte avsedd för användning. Använd inte den här avbildningen eftersom den är reserverad för den [SAP Molnbibliotek installation](https://cal.sap.com/) lösning.  

Du bör använda Azure Resource Manager för alla nya kontroller och installationer i Azure. Om du vill söka efter SUSE SLES avbildningar och versioner genom att använda Azure PowerShell eller Azure-kommandoradsgränssnittet (CLI), använder du följande kommandon. Du kan sedan använda utdata, till exempel att definiera den OS-avbildningen i en JSON-mall för att distribuera en ny SUSE Linux virtuell dator.
Dessa PowerShell-kommandon är giltig för Azure PowerShell version 1.0.1 och senare.

Även om det är fortfarande möjligt att använda standard SLES avbildningar för SAP-installationer har vi rekommenderar för att använda den nya SLES för SAP-avbildningar som finns nu på Azure-avbildning galleriet. Mer information om dessa avbildningar kan hittas i den motsvarande [Azure Marketplace sidan]( https://azuremarketplace.microsoft.com/en-us/marketplace/apps/SUSE.SLES-SAP ) eller [webbsida för SUSE vanliga frågor och svar om SLES SAP]( https://www.suse.com/products/sles-for-sap/frequently-asked-questions/ ).


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
Agenten kallas WALinuxAgent är en del av SLES avbildningar i Azure Marketplace. Information om hur du installerar den manuellt (till exempel när du överför en SLES OS virtuell hårddisk (VHD) från lokala) finns i:

* [OpenSUSE](http://software.opensuse.org/package/WALinuxAgent)
* [Azure](../../linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [SUSE](https://www.suse.com/communities/blog/suse-linux-enterprise-server-configuration-for-windows-azure/)

## <a name="sap-enhanced-monitoring"></a>SAP ”förbättrad övervakning”
SAP ”förbättrad övervakning” är en obligatorisk förutsättning för att köra SAP på Azure. Kontrollera informationen i SAP Observera 2191498 ”SAP på Linux med Azure: förbättrad övervakning”.

## <a name="attaching-azure-data-disks-to-an-azure-linux-vm"></a>Kopplar Azure datadiskar till en Azure Linux-dator
Bör du aldrig montera Azure datadiskar på en Azure Linux-dator med hjälp av enhets-ID. Använd i stället universellt Unik identifierare (UUID). Var försiktig när du använder grafiska verktyg för att montera Azure datadiskar, t.ex. Kontrollera posterna i /etc/fstab.

Problemet med enhets-ID är att den kan ändras och sedan den virtuella Azure-datorn kan hänga i startprocessen. För att åtgärda problemet, kan du lägga till parametern nofail i /etc/fstab. Men var försiktig med nofail eftersom program kan använda monteringspunkten som innan du kan skriva till filsystemet rot om en extern Azure-datadisk inte monterade under starten av.

Det enda undantaget montering via UUID är kopplar en OS-disk i felsökningssyfte, enligt beskrivningen i följande avsnitt.

## <a name="troubleshooting-a-suse-vm-that-isnt-accessible-anymore"></a>Felsöka en SUSE VM som inte är tillgänglig längre
Det kan finnas situationer där en SUSE VM på Azure låser sig i startprocessen (till exempel med ett fel som rör montera diskar). Du kan kontrollera det här problemet med hjälp av funktionen Start diagnostik för virtuella datorer i Azure v2 i Azure-portalen. Mer information finns i [starta diagnostik](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/).

Ett sätt att lösa problemet är att koppla OS-disken från den skadade virtuella datorn till en annan SUSE VM på Azure. Sedan göra ändringar som redigera /etc/fstab eller ta bort nätverket udev regler som beskrivs i nästa avsnitt.

Det finns en viktig sak att tänka på. Distribuera flera SUSE virtuella datorer från samma Azure Marketplace-avbildning (exempelvis SLES 11 SP4) medför OS-disk som alltid ska monteras av samma UUID. Bifoga en OS-disk från en annan virtuell dator som har distribuerats med hjälp av samma Azure Marketplace-avbildning med UUID kommer därför resultera i två identiska UUID: er. Detta orsakar problem och kan innebära att den virtuella datorn avsett för felsökning startas i själva verket från anslutna och skadat OS-disken i stället för det ursprungliga.

Det finns två sätt att undvika detta:

* Använd en annan Azure Marketplace-avbildning för felsökning VM (till exempel SLES 11 SPx i stället för SLES 12).
* Inte bifoga skadade OS-disk från en annan virtuell dator med hjälp av UUID--Använd något annat.

## <a name="uploading-a-suse-vm-from-on-premises-to-azure"></a>Ladda upp en SUSE virtuell dator från lokal till Azure
En beskrivning av hur du överför en SUSE VM från lokal till Azure, se [förbereda en virtuell dator SLES eller openSUSE för Azure](../../linux/suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Om du vill överföra en virtuell dator utan avetablering steg i slutet (till exempel för att behålla en befintlig installation av SAP, samt värdnamnet) kontrollerar du följande:

* Kontrollera att OS-disken är monterad med hjälp av UUID och inte enhets-ID. Ändra till UUID bara i /etc/fstab räcker inte för OS-disk. Dessutom Glöm inte att anpassa startprogrammet via YaST eller genom att redigera /boot/grub/menu.lst.
* Om du använder VHDX-format för SUSE OS-disken och konvertera det till VHD för överföring till Azure, är det mycket troligt att nätverksenheten som kommer att ändras från eth0 till eth1. Om du vill undvika problem när du senare starta på Azure, ändra tillbaka till eth0 enligt beskrivningen i [korrigerat eth0 i klonas SLES 11 VMware](https://dartron.wordpress.com/2013/09/27/fixing-eth1-in-cloned-sles-11-vmware/).

Utöver det som beskrivs i artikeln, rekommenderar vi att du tar bort det här:

   /lib/udev/rules.d/75-persistent-NET-Generator.rules

Du kan också installera Azure Linux-agenten (waagent) för att hjälpa dig att undvika problem, så länge det inte finns flera nätverkskort.

## <a name="deploying-a-suse-vm-on-azure"></a>Distribuera en SUSE virtuell dator på Azure
Du bör skapa nya SUSE virtuella datorer med hjälp av JSON-mallfilerna i den nya Azure Resource Manager-modellen. När mallen JSON-filen har skapats kan distribuera du den virtuella datorn med hjälp av följande CLI-kommando som ett alternativ till PowerShell:

   ```
   azure group deployment create "<deployment name>" -g "<resource group name>" --template-file "<../../filename.json>"

   ```
Mer information om JSON-mallfilerna finns [redigera Azure Resource Manager-mallar](../../../resource-group-authoring-templates.md) och [Azure snabbstartsmallar](https://azure.microsoft.com/documentation/templates/).

Mer information om CLI och Azure Resource Manager finns [använder Azure CLI för Mac, Linux och Windows med Azure Resource Manager](../../../xplat-cli-azure-resource-manager.md).

## <a name="sap-license-and-hardware-key"></a>Nyckeln för SAP licens och maskinvara
För den officiella SAP-Azure-certifieringen introducerades en ny mekanism för att beräkna SAP maskinvarunyckeln som används för SAP-licens. SAP-kernel var tvungen att anpassas för att göra denna. Tidigare SAP kernel-versioner för Linux innehöll inte den här koden ändras. Därför kan i vissa situationer (till exempel Azure VM storleksändring), SAP maskinvarunyckel ändras och SAP-licensen har inte längre är giltig. Detta är lösta i de senaste SAP Linux-kärnor. Mer information finns i SAP-kommentar 1928533.

## <a name="suse-sapconf-package--tuned-adm"></a>SUSE sapconf paket / justerade adm
SUSE innehåller ett paket som kallas ”sapconf” som hanterar en uppsättning SAP-specifika inställningar. Mer information om det här paketet har och hur du installerar och använder den finns [använder sapconf för att förbereda en SUSE Linux Enterprise Server att köra SAP-system](https://www.suse.com/communities/blog/using-sapconf-to-prepare-suse-linux-enterprise-server-to-run-sap-systems/) och [vad är sapconf eller hur du förbereder en SUSE Linux Enterprise Server för att köra SAP-system? ](http://scn.sap.com/community/linux/blog/2014/03/31/what-is-sapconf-or-how-to-prepare-a-suse-linux-enterprise-server-for-running-sap-systems).

Under tiden är det ett nytt verktyg som ersätter sapconf - justerade adm. En kan hitta mer information om det här verktyget följande två länkarna nedan.

SLES dokumentation om justerade adm profil sap-hana finns [här](https://www.suse.com/documentation/sles-for-sap-12/book_s4s/data/sec_s4s_configure_sapconf.html) 

Prestandajustering system för SAP-arbetsbelastningar med justerade adm - hittar [här](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/book_s4s/book_s4s.pdf) i kapitlet 6.2

## <a name="nfs-share-in-distributed-sap-installations"></a>NFS-resursens i distribuerade SAP-installationer
Om du har en distribuerad installation – till exempel där du vill installera databasen och SAP-programservrar i olika virtuella datorer – kan du dela katalogen /sapmnt via Network File System (NFS). Om du har problem med följande steg när du har skapat NFS-resurs för /sapmnt, kontrollera om ”no_root_squash” har angetts för resursen.

## <a name="logical-volumes"></a>Logiska volymer
I förflutna om en krävs en stor logisk volym över flera diskar i Azure data (till exempel för SAP-databasen) rekommenderades använda mdadm eftersom lvm inte helt har verifierats ännu i Azure. Information om hur du ställer in Linux RAID på Azure med hjälp av mdadm finns [konfigurera programvarubaserad RAID på Linux](../../linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Under tiden från och med början av maj 2016 också lvm stöds helt i Azure och kan användas som ett alternativ till mdadm. Mer information om lvm i Azure finns [konfigurera LVM på en Linux-VM i Azure](../../linux/configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="azure-suse-repository"></a>Azure SUSE databasen
Du kan använda ett enkelt kommando för att återställa den om du har problem med åtkomst till standard SUSE Azure-databasen. Detta kan inträffa om du skapar en privat operativsystemsavbildning i en Azure-region och sedan kopiera avbildningen till en annan region där du vill distribuera nya virtuella datorer som är baserade på det här privata OS-avbildningen. Bara att köra följande kommando inuti den virtuella datorn:

   ```
   service guestregister restart
   ```

## <a name="gnome-desktop"></a>Gör väldigt lätt desktop
Om du vill använda skrivbordet gör väldigt lätt för att installera ett komplett SAP demo-system i en enda virtuell dator – inklusive SAP-GUI använder webbläsaren och SAP-hanteringskonsolen--denna ledtråd för att installera den på Azure SLES bilder:

   För SLES 11:

   ```
   zypper in -t pattern gnome
   ```

   För SLES 12:

   ```
   zypper in -t pattern gnome-basic
   ```

## <a name="sap-support-for-oracle-on-linux-in-the-cloud"></a>SAP-stöd för Oracle på Linux i molnet
Det finns en begränsning med stöd från Oracle på Linux i virtualiserade miljöer. Även om detta inte är en Azure-specifika avsnittet, är det viktigt att förstå. Oracle stöder inte på SUSE eller Red Hat i ett offentligt moln som Azure SAP. Kontakta Oracle direkt om du vill diskutera det här avsnittet.

