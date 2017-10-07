---
title: "aaaTroubleshoot Autoskala med Virtual Machine-Skalningsuppsättningar | Microsoft Docs"
description: "Felsöka Autoskala med virtuella datorer. Förstå vanliga problem och hur tooresolve dem."
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c7d87b72-ee24-4e52-9377-a42f337f76fa
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: windows
ms.devlang: na
ms.topic: article
ms.date: 10/28/2016
ms.author: guybo
ms.openlocfilehash: 4c9a70992348d87fb43646421a90a027bf400a17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-autoscale-with-virtual-machine-scale-sets"></a>Felsöka Autoskala med virtuella datorer
**Problemet** – du har skapat en autoskalning infrastruktur i Azure Resource Manager med Skalningsuppsättningar – till exempel genom att distribuera en mall så här: https://github.com/Azure/azure-quickstart-templates/tree/master/201- vmss bottle Autoskala – du har din definierade regler för skala och passar utmärkt, förutom att oavsett hur mycket belastning du spärra hello virtuella datorer är det inte Autoskala.

## <a name="troubleshooting-steps"></a>Felsökningssteg
Vissa saker tooconsider omfattar:

* Hur många kärnor finns varje virtuell dator, och du läser in varje core? hello Azure Quickstart exempelmall ovan har ett do_work.php skript som läser in en enda kärna. Om du använder en virtuell dator som är större än en enda kärna VM-storlek som Standard_A1 eller D1 sedan du skulle behöva toorun den här inläsningen flera gånger. Kontrollera hur många kärnor dina virtuella datorer genom att granska [storlekar för Windows-datorer i Azure](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* Hur många virtuella datorer i VM Scale Set hello allt arbete på var och en?
  
    En skalbar händelse bara sker när hello Genomsnittlig CPU över **alla** hello virtuella datorer i en skaluppsättning överskrider hello tröskelvärde med tiden för hello interna definieras i hello Autoskala regler.
* Missade du skala händelser?
  
    Kontrollera hello granskningsloggar i hello Azure-portalen för skala händelser. Kanske det fanns en skala upp och en skala ned som har missats. Du kan filtrera efter ”skala”...
  
    ![Granskningsloggar][audit]
* Är din skala i och skalbar tröskelvärden tillräckligt?
  
    Anta att du anger en regel tooscale ut när Genomsnittlig CPU är större än 50% under 5 minuter och tooscale i när Genomsnittlig CPU är mindre än 50%. Detta innebär att ”flapping” problem när processoranvändningen är Stäng toothis tröskelvärde med skalningsåtgärder hello ständigt öka och minska storleken på hello uppsättning. Därför försöker hello Autoskala tjänsten tooprevent ”växlar”, som kan visas som inte skalning. Därför kontrollera din skalbar och tröskelvärden för skalan är tillräckligt olika tooallow Frigör utrymme mellan skalning.
* Du skriva en egen JSON-mall?
  
    Det är enkelt toomake misstag, så börja med en mall som hello ovanpå som bevisas toowork och göra små inkrementella ändringar. 
* Kan du manuellt skala in eller ut?
  
    Försök att omdistribuera hello VM Scale Set resurs med en annan ”kapacitet” inställningen toochange hello antal virtuella datorer manuellt. Ett exempel mallen toodo som det här är: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing – du kan behöva tooedit hello mallen toomake att den har hello samma datorn storlek som Scale Set använder. Om du kan ändra hello antal virtuella datorer manuellt, vet hello problemet är isolerad tooautoscale.
* Kontrollera din Microsoft.Compute/virtualMachineScaleSet och Microsoft.Insights resurser i hello [resursutforskaren i Azure](https://resources.azure.com/)
  
    Detta är ett nödvändigt felsökningsverktyg som visar hello för dina Azure Resource Manager-resurser. Klicka på din prenumeration och titta på hello resursgrupp som du felsöker. Titta på hello VM Scale Set du skapade under hello Compute-resursprovidern och kontrollera hello instansvyn som visar hello tillståndet för en distribution. Kontrollera också hello instansvyn för virtuella datorer i hello Skaluppsättning. Sedan gå till hello Microsoft.Insights resursprovidern och kontrollera hello Autoskala regler ser bra ut.
* Hello diagnostiska tillägg fungerar och är avger prestandadata?
  
    **Uppdatering:** Azure autoskalningsfunktionen har förbättrat toouse värdbaserad mått pipelinen som kräver inte längre en diagnostik tillägget toobe installerad. Det innebär att hello nästa några stycken inte längre användas om du skapar ett autoskalning program med hjälp av hello ny pipeline. Ett exempel på Azure-mallar som har varit konverterade toouse hello värden pipeline är: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale. 
  
    Med hjälp av värdbaserad mätvärden för Autoskala är bättre för hello följande orsaker:
  
  * Färre rörliga delar som inga diagnostik tillägg måste toobe installerad.
  * Enklare mallar. Lägg bara till insikter Autoskala regler tooan befintlig skaluppsättning för mallen.
  * Mer tillförlitlig rapportering och snabbare start av nya virtuella datorer.
    
    hello skulle endast orsaker som du använder ett tillägg för diagnostiska tookeep vara om du behöver minne diagnostik reporting/skalning. Värdbaserad mått rapporterar inte minne.
    
    Med detta i åtanke bara följa hello resten av den här artikeln om du fortfarande använder diagnostiska tillägg för din autoskalning.
    
    Autoskala i Azure Resource Manager kan arbeta (men har inte längre att) med hjälp av en VM-tillägget som kallas hello diagnostik tillägg. Den avger prestanda data tooa storage-konto du definierar i hello mallen. Informationen sammanställs sedan av hello Azure-Monitor-tjänsten.
    
    Om hello Insights-tjänsten inte kan läsa data från hello VMs, ska toosend du ett e-postmeddelande – till exempel om hello VMs ned, så kontrollera din e-post (hello något du angav när du skapar hello Azure-konto).
    
    Du kan också gå och titta på hello data själv. Titta på hello Azure storage-konto med en cloud explorer. Till exempel använder hello [Visual Studio Cloud Explorer](https://visualstudiogallery.msdn.microsoft.com/aaef6e67-4d99-40bc-aacf-662237db85a2), logga in och välj hello du använder Azure-prenumeration och hello diagnostiklagringskonto namn som refereras i hello diagnostik resurstilläggsdefinitionen i distributionen mall...
    
    ![Cloud Explorer][explorer]
    
    Här visas flera tabeller där hello data från varje virtuell dator lagras. Tar Linux- och hello CPU måttet som ett exempel kan titta på hello senaste rader. hello Visual Studio cloud explorer stöder ett frågespråk så att du kan köra en fråga som ”tidsstämpel gt datetime'2016-02-02T21:20:00Z'” toomake till att du får hello senaste händelser (antar tid är i UTC). Skalas hello data som du ser i det motsvarar toohello regler som du ställer in? I hello exemplet nedan igång hello CPU för datorn 20 öka too100% över hello sista 5 minuter...
    
    ![Storage-tabeller][tables]
    
    Om hello data inte är det, sedan innebär den hello problemet är med hello diagnostiska tillägg som körs i hello virtuella datorer. Om hello data är det, innebär det att det finns problem med din skalningsregler eller hello Insights-tjänsten. Kontrollera [Azure Status](https://azure.microsoft.com/status/).
    
    När du har genom stegen, om du fortfarande har problem med Autoskala du försöka hello-forum [MSDN](https://social.msdn.microsoft.com/forums/azure/home?category=windowsazureplatform%2Cazuremarketplace%2Cwindowsazureplatformctp), eller [stackspill](http://stackoverflow.com/questions/tagged/azure), eller logga kundsupport. Vara förberedda tooshare hello mall och en vy över hello prestandadata.

[audit]: ./media/virtual-machine-scale-sets-troubleshoot/image3.png
[explorer]: ./media/virtual-machine-scale-sets-troubleshoot/image1.png
[tables]: ./media/virtual-machine-scale-sets-troubleshoot/image4.png
