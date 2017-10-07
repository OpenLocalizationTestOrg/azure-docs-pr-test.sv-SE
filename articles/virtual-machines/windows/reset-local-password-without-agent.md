---
title: "aaaReset ett lokalt Windows-lösenord utan Azure-agenten | Microsoft Docs"
description: "Hur tooreset hello lösenord för lokala Windows-användarkonto när hello Azure gästagenten inte har installerats eller fungerar på en virtuell dator"
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: cf353dd3-89c9-47f6-a449-f874f0957013
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/07/2017
ms.author: iainfou
ms.openlocfilehash: c559c31ea142f9cf50d2c5b6182c5355fec9bac5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooreset-local-windows-password-for-azure-vm"></a>Hur tooreset lokala Windows-lösenord för Azure VM
Du kan återställa hello lokala Windows-lösenord för en virtuell dator i Azure med hjälp av hello [Azure-portalen eller Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) angivna hello Azure gäst-agenten är installerad. Den här metoden är hello primära sätt tooreset ett lösenord för en Azure VM. Om du får problem med hello Azure gästagent inte svarar eller misslyckas tooinstall efter överföring av en anpassad avbildning måste återställa du manuellt en Windows-lösenord. Den här artikeln beskrivs hur tooreset ett lokalt kontolösenord genom att koppla hello källa OS virtuell disk tooanother VM. 

> [!WARNING]
> Endast använda den här processen som en sista utväg. Försök tooreset alltid ett lösenord med hello [Azure-portalen eller Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) första.
> 
> 

## <a name="overview-of-hello-process"></a>Översikt över hello-processen
hello core stegen för att utföra en lokal återställning av lösenord för en virtuell Windows-dator i Azure när det finns ingen åtkomst toohello Azure gästagent är följande:

* Ta bort Virtuella hello källdatorn. hello virtuella diskar bevaras.
* Koppla hello källa VM OS disk tooanother VM på hello samma plats i din Azure-prenumeration. Den här virtuella datorn är refererad tooas hello felsökning VM.
* Använd hello felsökning VM för att skapa vissa config-filer på hello källa VM OS-disken.
* Koppla bort hello VM OS-disken från hello felsökning VM.
* Använd en virtuell dator med hello ursprungliga virtuella disken för toocreate en Resource Manager-mall.
* När hello nya VM Starter hello konfigurationsfiler skapar du uppdateringen hello lösenordet för användaren som hello krävs.

## <a name="detailed-steps"></a>Detaljerade steg
Försök tooreset alltid ett lösenord med hello [Azure-portalen eller Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) innan du försöker hello följande steg. Kontrollera att du har en säkerhetskopia av den virtuella datorn innan du börjar. 

1. Ta bort hello påverkas VM i Azure-portalen. Ta bort hello VM tar bara bort hello metadata, hello referens för hello VM i Azure. hello virtuella diskar bevaras när hello VM tas bort:
   
   * Välj hello VM i hello Azure-portalen klickar du på *ta bort*:
     
     ![Ta bort en befintlig virtuell dator](./media/reset-local-password-without-agent/delete_vm.png)
2. Koppla hello källa VM OS disk toohello felsökning VM. hello felsökning VM måste vara i hello samma region som hello källa VM OS-disken (som `West US`):
   
   * Välj hello felsökning VM i hello Azure-portalen. Klicka på *diskar* | *bifoga befintliga*:
     
     ![Bifoga den befintliga disken](./media/reset-local-password-without-agent/disks_attach_existing.png)
     
     Välj *VHD-filen* och välj sedan hello storage-konto som innehåller ditt Virtuella källdatorn:
     
     ![Välj lagringskonto](./media/reset-local-password-without-agent/disks_select_storageaccount.PNG)
     
     Välj hello käll-behållare. hello källa behållare är vanligtvis *virtuella hårddiskar*:
     
     ![Välj lagringsbehållare](./media/reset-local-password-without-agent/disks_select_container.png)
     
     Välj hello OS vhd tooattach. Klicka på *Välj* toocomplete hello processen:
     
     ![Välj källa för virtuell disk](./media/reset-local-password-without-agent/disks_select_source_vhd.png)
3. Ansluta toohello felsökning av virtuell dator med hjälp av fjärrskrivbord och säkerställa hello källa VM OS-disken visas:
   
   * Välj hello felsökning VM i hello Azure-portalen och klicka på *Anslut*.
   * Öppna hello RDP-filen som hämtas. Ange hello användarnamnet och lösenordet för hello felsökning VM.
   * I Utforskaren, leta efter hello datadisk bifogade. Om hello källa Virtuella datorns virtuella Hårddisken är hello endast data disk ansluten toohello felsökning VM, bör det vara hello F: enhet:
     
     ![Visa bifogade datadisk](./media/reset-local-password-without-agent/troubleshooting_vm_fileexplorer.png)
4. Skapa `gpt.ini` i `\Windows\System32\GroupPolicy` på hello källa VM-enhet (om det finns gpt.ini, byta namn på toogpt.ini.bak):
   
   > [!WARNING]
   > Se till att du inte oavsiktligt skapa hello följande filer i C:\Windows, hello OS-enhet för hello felsökning VM. Skapa följande filer i hello OS-enhet för källan virtuell dator som är anslutna som en datadisk hello.
   > 
   > 
   
   * Lägg till följande rader i hello hello `gpt.ini` fil som du skapade:
     
     ```
     [General]
     gPCFunctionalityVersion=2
     gPCMachineExtensionNames=[{42B5FAAE-6536-11D2-AE5A-0000F87571E3}{40B6664F-4972-11D1-A7CA-0000F87571E3}]
     Version=1
     ```
     
     ![Skapa gpt.ini](./media/reset-local-password-without-agent/create_gpt_ini.png)
5. Skapa `scripts.ini` i `\Windows\System32\GroupPolicy\Machine\Scripts`. Kontrollera att dolda mappar som visas. Om det behövs, skapa hello `Machine` eller `Scripts` mappar.
   
   * Lägg till följande rader hello hello `scripts.ini` fil som du skapade:
     
     ```
     [Startup]
     0CmdLine=C:\Windows\System32\FixAzureVM.cmd
     0Parameters=
     ```
     
     ![Skapa scripts.ini](./media/reset-local-password-without-agent/create_scripts_ini.png)
6. Skapa `FixAzureVM.cmd` i `\Windows\System32` med hello efter innehållet, ersätter `<username>` och `<newpassword>` med egna värden:
   
    ```
    net user <username> <newpassword> /add
    net localgroup administrators <username> /add
    net localgroup "remote desktop users" <username> /add

    ```

    ![Skapa FixAzureVM.cmd](./media/reset-local-password-without-agent/create_fixazure_cmd.png)
   
    När du definierar hello nytt lösenord måste du uppfylla kraven på lösenordskomplexitet hello som konfigurerats för den virtuella datorn.
7. Koppla bort hello disk från hello felsökning VM i Azure-portalen:
   
   * Välj hello felsökning VM i hello Azure-portalen, klicka på *diskar*.
   * Välj hello datadisk kopplade i steg 2, klickar du på *Detach*:
     
     ![Koppla bort disk](./media/reset-local-password-without-agent/detach_disk.png)
8. Innan du skapar en virtuell dator måste du hämta hello URI tooyour källa OS-disken:
   
   * Välj hello storage-konto i hello Azure-portalen klickar du på *Blobbar*.
   * Välj hello-behållare. hello källa behållare är vanligtvis *virtuella hårddiskar*:
     
     ![Välj kontot lagringsblob](./media/reset-local-password-without-agent/select_storage_details.png)
     
     Välj källa VM OS VHD och klicka på hello *kopiera* knappen Nästa toohello *URL* namn:
     
     ![Kopiera disk URI](./media/reset-local-password-without-agent/copy_source_vhd_uri.png)
9. Skapa en virtuell dator från hello källa VM OS-disken:
   
   * Använd [Azure Resource Manager-mallen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd) toocreate en virtuell dator från en särskild virtuell Hårddisk. Klicka på hello `Deploy tooAzure` knappen tooopen hello Azure-portalen med hello mallbaserat information fylls i automatiskt.
   * Om du vill tooretain alla tidigare hello-inställningarna för hello VM, Välj *redigera mallen* tooprovide din befintliga VNet, undernät, nätverkskort eller offentlig IP-adress.
   * I hello `OSDISKVHDURI` klistra in hello käll-VHD-URI hämta i hello föregående steg i textrutan parameter:
     
     ![Skapa en virtuell dator från mall](./media/reset-local-password-without-agent/create_new_vm_from_template.png)
10. Efter hello nya virtuella datorn körs, ansluta toohello virtuella datorn med Fjärrskrivbord med hello nya lösenordet du angav i hello `FixAzureVM.cmd` skript.
11. Från din fjärrsession toohello filer nya virtuella datorn, ta bort hello följande tooclean in hello miljö:
    
    * Från %windir%\System32
      * ta bort FixAzureVM.cmd
    * Från %windir%\System32\GroupPolicy\Machine\
      * ta bort scripts.ini
    * Från %windir%\System32\GroupPolicy
      * ta bort gpt.ini (om gpt.ini fanns före och du byta namn på den toogpt.ini.bak, Byt namn på hello .bak-filen tillbaka toogpt.ini)

## <a name="next-steps"></a>Nästa steg
Om du fortfarande inte kan ansluta med hjälp av fjärrskrivbord, se hello [RDP felsökningsguiden](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Hej [detaljerad RDP felsökningsguide för](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) tittar på felsökning metoder i stället för särskilda åtgärder. Du kan också [öppnar du ett Azure supportbegäran](https://azure.microsoft.com/support/options/) för praktiska hjälp.

