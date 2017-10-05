---
title: "Azure Automation Runbook- och stänga | Microsoft Docs"
description: "Runbooks och moduler från Microsoft och communityn är tillgängliga för dig att installera och använda i Azure Automation-miljön.  Den här artikeln beskriver hur du kan använda dessa resurser och bidra runbooks i galleriet."
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
ms.openlocfilehash: 04cfafc9e7a037d915f63723fd0b67a07954460b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="runbook-and-module-galleries-for-azure-automation"></a>Azure Automation Runbook- och stänga
Du kan komma åt en mängd olika scenarier som redan har skapats av Microsoft och communityn istället för att skapa egna runbooks och moduler i Azure Automation.  Du kan antingen använda de här scenarierna utan ändringar eller använda dem som en startpunkt och redigera dem för dina specifika krav.

Du kan hämta runbooks från den [Runbook-galleriet](#runbooks-in-runbook-gallery) och moduler från den [PowerShell-galleriet](#modules-in-powerShell-gallery).  Du kan även bidra till gruppen genom att dela scenarier som du utvecklar.

## <a name="runbooks-in-runbook-gallery"></a>Runbooks i Runbook-galleriet
Den [Runbook-galleriet](http://gallery.technet.microsoft.com/scriptcenter/site/search?f\[0\].Type=RootCategory&f\[0\].Value=WindowsAzure&f\[1\].Type=SubCategory&f\[1\].Value=WindowsAzure_automation&f\[1\].Text=Automation) innehåller en mängd olika runbooks från Microsoft och som du kan importera till Azure Automation. Du kan antingen ladda ned en runbook från galleriet som är värd för den [TechNet Script Center](https://gallery.technet.microsoft.com/scriptcenter/site/upload), eller så kan du importera runbooks direkt från galleriet från den klassiska Azure-portalen eller Azure-portalen.

Du kan bara importera direkt från galleriet Runbook med den klassiska Azure-portalen eller Azure-portalen. Du kan inte utföra den här funktionen med Windows PowerShell.

> [!NOTE]
> Du bör verifiera innehållet i alla runbooks du hämta från Runbook-galleriet och vara mycket försiktig installera och köra dem i en produktionsmiljö. |
> 
> 

### <a name="to-import-a-runbook-from-the-runbook-gallery-with-the-azure-classic-portal"></a>Så här importerar du en runbook från galleriet Runbook med den klassiska Azure-portalen
1. I Azure-portalen klickar du på, **ny**, **Apptjänster**, **Automation**, **Runbook**, **från galleriet**.
2. Välj en kategori för att visa relaterade runbooks och välj en runbook för att visa dess egenskaper. När du väljer den runbook du vill, klicka på högerpilen.
   
    ![Runbook-galleri](media/automation-runbook-gallery/runbook-gallery.png)
3. Granska innehållet i runbook och notera eventuella krav i beskrivningen. Klicka på högerpilen när du är klar.
4. Ange information om runbook och klicka sedan på knappen markering. Runbook-namnet kommer redan fyllas i.
5. Runbook visas på den **Runbooks** för Automation-kontot.

### <a name="to-import-a-runbook-from-the-runbook-gallery-with-the-azure-portal"></a>Importera en runbook från Runbook-galleriet med Azure-portalen
1. Öppna ditt Automation-konto på Azure-portalen.
2. Öppna listan med runbooks genom att klicka på panelen **Runbooks**.
3. Klicka på **Bläddra galleriet** knappen.
   
    ![Galleriet knappen Bläddra](media/automation-runbook-gallery/browse-gallery-button.png)
4. Leta upp det Galleriobjekt som du vill och väljer att visa dess egenskaper.
   
    ![Bläddra galleri](media/automation-runbook-gallery/browse-gallery.png)
5. Klicka på **Visa projekt** att visa objektet i den [TechNet Script Center](http://gallery.technet.microsoft.com/).
6. Om du vill importera ett objekt, klickar du på den för att visa information och klickar sedan på den **importera** knappen.
   
    ![Knappen Importera](media/automation-runbook-gallery/gallery-item-detail.png)
7. Du kan också byta namn på runbook och klicka sedan på **OK** att importera runbook.
8. Runbook visas på den **Runbooks** för Automation-kontot.

### <a name="adding-a-runbook-to-the-runbook-gallery"></a>Lägga till en runbook i runbook-galleriet
Microsoft rekommenderar att du vill lägga till runbooks i Runbook-galleriet som du tror kan vara användbart för andra kunder.  Du kan lägga till en runbook av [överföra den till Script Center](http://gallery.technet.microsoft.com/site/upload) med hänsyn till följande information.

* Du måste ange *Windows Azure* för den **kategori** och *Automation* för den **underkategori** för runbook som ska visas i den guiden.  
* Överföringen måste vara en enskild .ps1 eller .graphrunbook-fil.  Om runbook kräver att alla moduler, underordnade runbooks och tillgångar, ska du ange dem i beskrivningen av överföring och i kommentaravsnittet i runbook.  Om du har ett scenario som kräver flera runbooks, överföra varje separat och visa en lista med namnen på de relaterade runbooks i var och en av deras beskrivningar. Kontrollera att du använder samma taggar så att de visas i samma kategori. En användare behöver läsa beskrivning för att kunna veta att andra runbooks är nödvändiga scenariot ska fungera.
* Lägg till taggen ”GraphicalPS” om du publicerar en **grafisk runbook** (inte ett grafiskt arbetsflöde). 
* Infoga en PowerShell eller PowerShell-arbetsflöde kodfragment i en beskrivning med hjälp av **Infoga kod avsnitt** ikon.
* Sammanfattning för överföringen visas i resultaten för Runbook-galleriet så bör du ge detaljerad information som hjälper användaren identifiera funktionen för runbook.
* Du bör tilldela en till tre av följande taggar till överföringen.  Runbook visas i guiden under kategorier som matchar dess taggar.  Alla taggar inte på den här listan kommer att ignoreras av guiden. Om du inte anger någon matchande taggar visas runbook under kategorin andra.
  
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
* Automation uppdaterar galleriet en gång i timmen, så att du inte se dina bidrag omedelbart.

## <a name="modules-in-powershell-gallery"></a>Moduler i PowerShell-galleriet
PowerShell-moduler innehåller cmdletar som du kan använda i dina runbooks och befintliga moduler som du kan installera i Azure Automation är tillgängliga i den [PowerShell-galleriet](http://www.powershellgallery.com).  Du kan starta den här gallery från Azure-portalen och installera dem direkt i Azure Automation eller hämta dem och installera dem manuellt.  Du kan inte installera modulerna direkt från den klassiska Azure-portalen, men du kan hämta dem installera dem som en annan modul.

### <a name="to-import-a-module-from-the-automation-module-gallery-with-the-azure-portal"></a>Importera en modul från galleriet Automation-modul med Azure-portalen
1. Öppna ditt Automation-konto på Azure-portalen.
2. Klicka på den **tillgångar** rutan för att öppna en lista över tillgångar.
3. Klicka på den **moduler** öppna listan över moduler.
4. Klicka på den **Bläddra galleriet** knappen och bläddra galleriet bladet har startats.
   
    ![Modulgalleriet](media/automation-runbook-gallery/modules-blade.png) <br>
5. När du har öppnat bladet Bläddra galleriet som du kan söka efter följande fält:
   
   * Modulnamn
   * Taggar
   * Skapa
   * Cmdlet/DSC-resurs
6. Hitta en modul som du är intresserad av och markera den att visa information.  
   När du detaljerat en modul kan du visa mer information om modulen, inklusive en länk till PowerShell-galleriet alla obligatoriska beroenden och alla cmdlets och/eller DSC-resurser som innehåller modulen.
   
    ![PowerShell-modulinformation](media/automation-runbook-gallery/gallery-item-details-blade.png) <br>
7. Installera modulen direkt i Azure Automation, klicka på den **importera** knappen.
   
    ![Importera modulen knappen](media/automation-runbook-gallery/module-import-button.png)
8. När du klickar på knappen Import visas modulens namn som du ska importera. Om alla beroenden är installerade på **OK** knappen kommer att vara aktiv. Om du saknar beroenden, måste du importera dem innan du kan importera den här modulen.
9. Klicka på **OK** att importera modulen och bladet modulen startas. När Azure Automation importerar en modul till ditt konto, extraherar metadata om modulen och cmdletar.
   
    ![Importera modulen bladet](media/automation-runbook-gallery/module-import-blade.png)
   
    Det kan ta ett par minuter eftersom varje aktivitet måste ska extraheras.
10. Du får ett meddelande som modulen distribueras och ett meddelande när den har slutförts.
11. När modulen importeras, visas tillgängliga aktiviteter och du kan använda sina resurser i runbooks och Desired State Configuration.

## <a name="requesting-a-runbook-or-module"></a>Begär en runbook eller en modul
Du kan skicka förfrågningar till [User Voice](https://feedback.azure.com/forums/246290-azure-automation/).  Om du behöver hjälp skriver du en runbook eller har en fråga om PowerShell kan du skicka en fråga till vår [forum](http://social.msdn.microsoft.com/Forums/windowsazure/en-US/home?forum=azureautomation&filter=alltypes&sort=lastpostdesc).

## <a name="next-steps"></a>Nästa steg
* Kom igång med runbooks finns [skapa eller importera en runbook i Azure Automation](automation-creating-importing-runbook.md)
* Om du vill förstå skillnaderna mellan PowerShell och PowerShell-arbetsflöde med runbooks finns [Learning PowerShell-arbetsflöde](automation-powershell-workflow.md)

