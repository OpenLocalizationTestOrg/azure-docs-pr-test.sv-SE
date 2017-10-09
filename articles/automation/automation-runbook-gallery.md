---
title: "modul för aaaRunbook och stänga Azure Automation | Microsoft Docs"
description: "Runbooks och moduler från Microsoft och hello community är tillgängliga för du tooinstall och använda i Azure Automation-miljön.  Den här artikeln beskriver hur du kan komma åt dessa resurser och toocontribute runbooks toohello galleriet."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: d3fee7b4-630a-4c10-8425-9bf51d7c9e58
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/14/2016
ms.author: magoedte;bwren
ms.openlocfilehash: 10b634460edf66dd7548017e3a2f7111b7125f30
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="runbook-and-module-galleries-for-azure-automation"></a>Azure Automation Runbook- och stänga
Du kan komma åt en mängd olika scenarier som redan har skapats av Microsoft och hello community istället för att skapa egna runbooks och moduler i Azure Automation.  Du kan antingen använda de här scenarierna utan ändringar eller använda dem som en startpunkt och redigera dem för dina specifika krav.

Du kan hämta runbooks från hello [Runbook-galleriet](#runbooks-in-runbook-gallery) och moduler från hello [PowerShell-galleriet](#modules-in-powerShell-gallery).  Du kan även bidra toohello community genom att dela scenarier som du utvecklar.

## <a name="runbooks-in-runbook-gallery"></a>Runbooks i Runbook-galleriet
Hej [Runbook-galleriet](http://gallery.technet.microsoft.com/scriptcenter/site/search?f\[0\].Type=RootCategory&f\[0\].Value=WindowsAzure&f\[1\].Type=SubCategory&f\[1\].Value=WindowsAzure_automation&f\[1\].Text=Automation) innehåller en mängd olika runbooks från Microsoft och hello-community som du kan importera till Azure Automation. Du kan antingen ladda ned en runbook från hello-galleriet som är värd för hello [TechNet Script Center](https://gallery.technet.microsoft.com/scriptcenter/site/upload), eller så kan du importera runbooks direkt från hello gallery från hello klassiska Azure-portalen eller Azure-portalen.

Du kan bara importera direkt från hello Runbook-galleriet med hello klassiska Azure-portalen eller Azure-portalen. Du kan inte utföra den här funktionen med Windows PowerShell.

> [!NOTE]
> Du bör verifiera hello innehållet i alla runbooks du hämta från hello Runbook-galleriet och vara mycket försiktig installera och köra dem i en produktionsmiljö. |
> 
> 

### <a name="tooimport-a-runbook-from-hello-runbook-gallery-with-hello-azure-classic-portal"></a>tooimport en runbook från hello Runbook-galleriet med hello klassiska Azure-portalen
1. I hello Azure-portalen klickar du på, **ny**, **Apptjänster**, **Automation**, **Runbook**, **från galleriet**.
2. Välj en kategori tooview relaterade runbooks och välj en runbook tooview information. När du väljer hello runbook du vill klicka hello högerpilen.
   
    ![Runbook-galleri](media/automation-runbook-gallery/runbook-gallery.png)
3. Granska hello innehållet i hello runbook och notera eventuella krav i hello beskrivning. Klicka på högerpilen för hello när du är klar.
4. Ange information om hello runbook och klicka hello markering. Hej runbooknamnet kommer redan fyllas i.
5. Hej runbook visas på hello **Runbooks** för hello Automation-konto.

### <a name="tooimport-a-runbook-from-hello-runbook-gallery-with-hello-azure-portal"></a>tooimport en runbook från hello Runbook-galleriet med hello Azure-portalen
1. Hello Azure-portalen, öppna ditt Automation-konto.
2. Klicka på hello **Runbooks** panelen tooopen hello lista över runbooks.
3. Klicka på **Bläddra galleriet** knappen.
   
    ![Galleriet knappen Bläddra](media/automation-runbook-gallery/browse-gallery-button.png)
4. Leta upp hello galleriobjektet och markera den tooview information.
   
    ![Bläddra galleri](media/automation-runbook-gallery/browse-gallery.png)
5. Klicka på **Visa projekt** tooview hello objekt i hello [TechNet Script Center](http://gallery.technet.microsoft.com/).
6. tooimport ett objekt, klicka på den tooview information och klickar sedan på hello **importera** knappen.
   
    ![Knappen Importera](media/automation-runbook-gallery/gallery-item-detail.png)
7. Du kan också ändra hello namnet på hello runbook och klicka sedan på **OK** tooimport hello runbook.
8. Hej runbook visas på hello **Runbooks** för hello Automation-konto.

### <a name="adding-a-runbook-toohello-runbook-gallery"></a>Lägga till en runbook toohello runbook-galleriet
Microsoft rekommenderar att du tooadd runbooks toohello Runbook-galleriet som du tror kan vara användbar tooother kunder.  Du kan lägga till en runbook av [överföra den toohello Script Center](http://gallery.technet.microsoft.com/site/upload) med hänsyn till kontot hello följande information.

* Du måste ange *Windows Azure* för hello **kategori** och *Automation* för hello **underkategori** för hello runbook toobe visas i guiden hello.  
* hello överför måste vara en enskild .ps1 eller .graphrunbook-fil.  Om hello runbook kräver att alla moduler, underordnade runbooks och tillgångar, ska du ange dem i hello beskrivning av hello överföring och i hello kommentarer i hello runbook.  Om du har ett scenario som kräver flera runbooks kan överföra varje separat och listan hello namnen på hello relaterade runbooks i var och en av deras beskrivningar. Kontrollera att du använder samma taggar så att de visas i hello hello samma kategori. En användare behöver tooread hello beskrivning tooknow att andra runbooks är obligatoriska hello scenariot toowork.
* Lägg till hello taggen ”GraphicalPS” om du publicerar en **grafisk runbook** (inte ett grafiskt arbetsflöde). 
* Infoga en PowerShell eller PowerShell-arbetsflöde kodstycke i hello beskrivning med **Infoga kod avsnitt** ikon.
* hello visas Sammanfattning för hello överför i hello Runbook-galleriet resulterar så bör du ge detaljerad information som hjälper användaren identifiera hello funktionerna i hello runbook.
* Du bör tilldela en toothree av hello följande taggar toohello överföringen.  Hej runbook visas i guiden hello under hello kategorier som matchar dess taggar.  Alla taggar inte på den här listan kommer att ignoreras av hello guiden. Om du inte anger någon matchande taggar hello runbook kommer att visas under hello kategori.
  
  * Säkerhetskopiering
  * Kapacitetshantering
  * Ändringshantering
  * Efterlevnad
  * Dev / Test miljöer
  * Katastrofåterställning
  * Övervakning
  * Korrigering
  * Etablering
  * Reparation
  * Livscykelhantering för VM
* Automation uppdaterar hello galleriet en gång i timmen, så att du inte se dina bidrag omedelbart.

## <a name="modules-in-powershell-gallery"></a>Moduler i PowerShell-galleriet
PowerShell-moduler innehåller cmdletar som du kan använda i dina runbooks och befintliga moduler som du kan installera i Azure Automation är tillgängliga i hello [PowerShell-galleriet](http://www.powershellgallery.com).  Du kan öppna den här gallery från hello Azure-portalen och installera dem direkt i Azure Automation eller hämta dem och installera dem manuellt.  Du kan inte installera hello moduler direkt från hello klassiska Azure-portalen, men du kan hämta dem installera dem som en annan modul.

### <a name="tooimport-a-module-from-hello-automation-module-gallery-with-hello-azure-portal"></a>tooimport en modul från hello Automation Modulgalleriet med hello Azure-portalen
1. Hello Azure-portalen, öppna ditt Automation-konto.
2. Klicka på hello **tillgångar** panelen tooopen hello lista över tillgångar.
3. Klicka på hello **moduler** panelen tooopen hello listan över moduler.
4. Klicka på hello **Bläddra galleriet** bladet i galleriet Bläddra-knappen och hello startas.
   
    ![Modulgalleriet](media/automation-runbook-gallery/modules-blade.png) <br>
5. När du har öppnat hello Bläddra gallery-bladet, kan du söka efter hello följande fält:
   
   * Modulnamn
   * Taggar
   * Skapa
   * Cmdlet/DSC-resurs
6. Hitta en modul som du är intresserad av och markera den tooview information.  
   När du detaljerat en modul kan du visa mer information om hello modulen, inklusive en länk tillbaka toohello PowerShell-galleriet alla obligatoriska beroenden, och alla hello-cmdlets och/eller DSC-resurser som hello modulen innehåller.
   
    ![PowerShell-modulinformation](media/automation-runbook-gallery/gallery-item-details-blade.png) <br>
7. tooinstall hello modulen direkt i Azure Automation klickar du på hello **importera** knappen.
   
    ![Importera modulen knappen](media/automation-runbook-gallery/module-import-button.png)
8. När du klickar på knappen för hello importera visas hello Modulnamn som du tooimport. Om alla hello beroenden är installerade hello **OK** knappen kommer att vara aktiv. Om du saknar beroenden måste tooimport dem innan du kan importera den här modulen.
9. Klicka på **OK** tooimport hello modulen och hello modulen bladet startas. När Azure Automation importerar en modul tooyour konto, extraherar metadata om hello modulen och hello-cmdlets.
   
    ![Importera modulen bladet](media/automation-runbook-gallery/module-import-blade.png)
   
    Det kan ta ett par minuter eftersom varje aktivitet måste toobe extraheras.
10. Du får ett meddelande att hello modulen distribueras och ett meddelande när det har slutförts.
11. När hello-modul har importerats hello tillgängliga aktiviteter visas och du kan använda sina resurser i runbooks och Desired State Configuration.

## <a name="requesting-a-runbook-or-module"></a>Begär en runbook eller en modul
Du kan skicka begäranden för[User Voice](https://feedback.azure.com/forums/246290-azure-automation/).  Om du behöver hjälp skriver du en runbook eller har en fråga om PowerShell, efter en fråga tooour [forum](http://social.msdn.microsoft.com/Forums/windowsazure/en-US/home?forum=azureautomation&filter=alltypes&sort=lastpostdesc).

## <a name="next-steps"></a>Nästa steg
* tooget igång med runbooks, se [skapa eller importera en runbook i Azure Automation](automation-creating-importing-runbook.md)
* toounderstand hello skillnaderna mellan PowerShell och PowerShell-arbetsflöde med runbooks, se [Learning PowerShell-arbetsflöde](automation-powershell-workflow.md)

