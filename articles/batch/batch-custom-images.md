---
title: "aaaProvision Azure Batch pooler från anpassade avbildningar | Microsoft Docs"
description: "Du kan skapa en Batch adresspoolen från en anpassad avbildning tooprovision compute-noder som innehåller hello programvara och data som du behöver för ditt program. Anpassade avbildningar är ett effektivt sätt tooconfigure compute-noder toorun Batch-arbetsbelastningar."
services: batch
author: tamram
manager: timlt
ms.service: batch
ms.topic: article
ms.date: 08/07/2017
ms.author: tamram
ms.openlocfilehash: 5cb698ee90f7d3ec9ffe69fa4dc602132c3f7569
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-a-custom-image-toocreate-a-pool-of-virtual-machines"></a>Använda en anpassad avbildning toocreate en pool av virtuella datorer

När du skapar en pool med virtuella datorer i Azure Batch, anger du en avbildning av virtuell dator (VM) som tillhandahåller hello operativsystemet för varje beräkningsnod i poolen hello. Du kan skapa en pool med virtuella datorer med hjälp av en Azure Marketplace-avbildning, eller genom att tillhandahålla en anpassad VHD-avbildning som du har förberett. När du anger en anpassad avbildning har kontroll över hur hello operativsystemet konfigureras hello samtidigt som varje compute-nod har etablerats. Den anpassade avbildningen kan även inkludera program och referensdata som är tillgängliga på hello beräkningsnod så snart det har etablerats.

Med hjälp av en anpassad avbildning kan spara tid vid hämtning av din pool datornoderna redo toorun Batch-arbetsbelastning. När du kan alltid använda en Azure Marketplace-avbildning och installera programvara på varje beräkningsnod när den har etablerats kan kan den här metoden vara mindre effektivt än att använda en anpassad avbildning. 

Vissa orsaker toouse en anpassad avbildning som har konfigurerats för ditt scenario är behöva:

- **Konfigurera hello operativsystem (OS)** någon specialkonfiguration av hello operativsystem kan utföras på hello anpassad avbildning. 
- **Installera stora program.** Installera program på en anpassad avbildning är effektivare än att installera dem på varje beräkningsnod när den har etablerats.
- **Kopiera stora mängder data.** Om hello data är kopierade toohello anpassad avbildning, behöver bara toobe kopieras en gång, i stället för tooeach beräkningsnod, sparar tid och bandbredd.
- **Starta om hello VM under hello installationsprocessen.** Om hello VM kan vara en tidskrävande process, särskilt om du har ett antal compute-noder.

## <a name="prerequisites"></a>Krav

- **Ett Batch-konto som har skapats med hello användarens prenumeration poolen allokering läge.** toouse en anpassad avbildning tooprovision virtuella pooler, skapa Batch-kontot med hello användarens prenumeration [pool allokering läge](batch-api-basics.md#pool-allocation-mode). Med det här läget allokeras Batch pooler till hello prenumeration där hello kontot finns. Se hello [konto](batch-api-basics.md#account) i avsnittet [utveckla storskaliga parallell compute lösningar med Batch](batch-api-basics.md) information om inställningen hello poolen allokering läge när du skapar ett Batch-konto.

- **Ett Azure Storage-konto.** toocreate en pool av virtuella datorer med en anpassad avbildning behöver du en standard, generell Azure Storage-konto i hello samma prenumeration och region. Om du skapar en anpassad avbildning från en Azure VM, ska du kopiera hello avbildningens toohello lagringskonto där hello VM OS-disken finns, och du behöver inte toocreate ett separat lagringskonto. 
    
## <a name="prepare-a-custom-image"></a>Förbereda en anpassad avbildning

Du måste generalisera en befintlig installation av Linux eller Windows tooprepare en anpassad avbildning för användning med Batch. Att generalisera en installation av operativsystemet tar bort datorspecifik information. hello resultatet är en avbildning som kan installeras på andra datorer eller virtuella datorer.  

> [!IMPORTANT]
> Batch för närvarande stöder inte användning av Azure hanteras bilder tooprovision poolen. hello anpassad avbildning använda tooprovision poolen måste lagras i Azure Storage. 
>
> När du förbereder den anpassade avbildningen, Kom ihåg hello följande punkter:
> - Se till att hello OS basavbildning du använder tooprovision Batch-pooler har inte har någon förinstallerat Azure tillägg, till exempel hello tillägget för anpassat skript. Om hello bilden innehåller ett tillägg för förinstallerade, stöta Azure på problem som distribuerar hello VM.
> - Se till att hello OS basavbildning du tillhandahålla använder hello temp standardenheten som hello Batch nod agent för närvarande förväntar temp hello-standardenheten.
>
>

Du kan använda någon befintlig förberedda Windows eller Linux-avbildning som en anpassad avbildning. Till exempel om du vill toouse en lokal image och sedan överföra hello avbildningen tooan Azure Storage-konto som tillhör hello samma prenumeration och region som din Batch-kontot med [AzCopy](../storage/storage-use-azcopy.md) eller något annat överföringsverktyg.

Dessutom kan du förbereda en anpassad avbildning från en ny eller befintlig Azure-VM. Om du skapar en ny virtuell dator kan du använda en Azure Marketplace-avbildning som hello basavbildning för den anpassade avbildningen och anpassa den. toocustomize hello källinstallation, skapa en virtuell dator i Azure och lägga till dina program och data tooit. Sedan generalisera hello VM tooserve som den anpassade avbildningen och spara den tooAzure lagring. 

tooprepare en anpassad avbildning från en Azure VM, Följ dessa steg:

1. Skapa en **ohanterade** virtuella Azure-datorn från Azure Marketplace-avbildning. Azure Marketplace innehåller avbildningar för både [Windows](../virtual-machines/windows/quick-create-portal.md) och [Linux](../virtual-machines/linux/quick-create-portal.md).
    
    Kontrollera att du väljer i steg 3 i hello VM skapandeprocessen **nr** för **lagring: använda hanterade diskar** alternativet. Även anteckna hello lagringskontonamnet för hello VM OS-disken, som det här lagringskontot är också där Azure sparar den anpassade avbildningen:

    ![Skapa en ohanterad VM och Observera hello lagringskontonamn](media/batch-custom-images/vm-create-storage.png)
 
2. Slutför hello processen att skapa den virtuella datorn och vänta tills den toobe allokerats av Azure. Här är en bild som visar en virtuell dator i hello Azure-portalen i hello körs:

    ![Skapa en virtuell dator från en marketplace-avbildning](media/batch-custom-images/vm-status-running.png)

3. När hello VM körs kan ansluta tooit via RDP (för Windows) eller SSH (för Linux). Installera programvara som krävs eller kopiera önskade data och sedan generalisera hello VM. Följ hello stegen som beskrivs i [Generalize hello VM](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sa-copy-generalized.md#generalize-the-vm). 
   
4. Gör hello för[logga in tooAzure PowerShell](../virtual-machines/windows/sa-copy-generalized.md#log-in-to-azure-powershell). tooinstall Azure PowerShell finns [översikt av Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.2.0). 

5. Därefter gör hello för[Deallocate hello VM och ange hello tillstånd toogeneralized](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sa-copy-generalized#deallocate-the-vm-and-set-the-state-to-generalized). 

    Observera att hello har frigjorts i hello Azure-portalen:

    ![Se till att hello har frigjorts](media/batch-custom-images/vm-status-deallocated.png)

6.  Skapa och spara hello VM avbildningen tooAzure lagring med hjälp av hello [spara AzureRmVMImage](https://docs.microsoft.com/powershell/module/azurerm.compute/save-azurermvmimage) PowerShell-cmdlet. Följ instruktionerna i hello [skapa hello avbildningen](../virtual-machines/windows/sa-copy-generalized.md#create-the-image).
    
    hello VM-avbildning sparas toohello Azure Storage-konto som skapas när hello VM skapades, som visas i steg 1 i den här proceduren. hello spara AzureRmVMImage cmdlet sparar hello avbildningen toohello **system** behållare i detta lagringskonto. Hej `-DestinationContainername` parametern trots att en virtuell katalog på hello **system** behållare. Hej `-VHDNamePrefix` parametern anger ett prefix för hello blob-namnet. Det här prefixet är inledd toohello blob-namn med ett bindestreck. 

    Anta att du anropa Save AzureRmVMImage med hello följande parametrar:  

        Save-AzureRmVMImage -ResourceGroupName sample-resource-group -Name vm-custom-image -DestinationContainerName batchimages -VHDNamePrefix custom -Path C:\Temp\Images\vm-custom-image.json

    hello bilden sparas toohello plats och blob-namnet som visas här:

    ![Platsen för sparade virtuella Hårddisken i system-behållaren](media/batch-custom-images/vhd-in-vm-storage-account.png)

    > [!NOTE]
    > En ohanterad Azure VM skapar flera lagringskonton för olika ändamål. Om du inte antecknade hello namnet på lagringsbehållaren hello för hello OS-disk när hello VM skapades, så Sök hello associerade storage-konto som innehåller hello **system** behållare. Gå igenom hello **system** behållare toofind hello anpassad avbildning med hello-värden som du angav för hello **spara AzureRmVMImage** kommando.

## <a name="create-a-pool-from-a-custom-image-in-hello-portal"></a>Skapa en programpool från en anpassad avbildning i hello-portalen

När du har sparat den anpassade avbildningen och du vet att platsen, skapar du en Batch-pool från den avbildningen. Följ dessa steg toocreate poolen från hello Azure-portalen:

1. Navigera tooyour Batch-kontot i hello Azure-portalen. Det här kontot måste vara i hello samma prenumeration och region som hello storage-konto som innehåller hello anpassad avbildning. 
2. I hello **inställningar** fönstret till vänster, hello väljer hello **pooler** menyalternativet.
3. I hello **pooler** fönster, Välj hello **Lägg till** kommando.
4. På hello **Lägg till Pool för** väljer **anpassad bild (Linux/Windows)** från hello **bildtypen** listrutan. hello portalen visar hello **anpassad bild** väljare. Navigera toohello storage-konto där den anpassade avbildningen finns och välj hello önskade VHD från hello behållare. 
5. Välj rätt hello **Publisher-erbjudande-Sku** för dina anpassade virtuella Hårddisken.
6. Ange hello återstående nödvändiga inställningar, inklusive hello **nodstorlek**, **mål dedikerade noder**, och **med låg prioritet noder**, liksom alla önskade valfria inställningar.

    Till exempel för en anpassad avbildning i Microsoft Windows Server Datacenter 2016 hello **Lägg till Pool för** visas i fönstret som visas här:

    ![Lägg till pool från anpassade Windows-avbildning](media/batch-custom-images/add-pool-custom-image.png)
  
toocheck om en befintlig adresspool baseras på en anpassad avbildning i hello **operativsystemet** egenskap i sammanfattningen av hello resurs i hello **Pool** fönster. Om hello poolen har skapats från en anpassad avbildning genom att den har angetts för**anpassade VM-avbildning**.

Alla anpassade virtuella hårddiskar som är associerade med en pool visas på hello poolen **egenskaper** fönster.
 
## <a name="next-steps"></a>Nästa steg

- En detaljerad översikt över Batch finns [utveckla storskaliga parallell compute lösningar med Batch](batch-api-basics.md).
- Information om hur du skapar ett Batch-konto finns i [skapa ett batchkonto med hello Azure-portalen](batch-account-create-portal.md).
