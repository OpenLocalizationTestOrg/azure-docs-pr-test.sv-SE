---
title: aaaDeploy ditt erbjudande toohello Azure Marketplace | Microsoft Docs
description: "Lär dig mer om och går igenom hello instruktioner toodeploy erbjudandet--avbildning av virtuell dator, developer service, datatjänst, etc.--toohello Azure Marketplace."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 8f79b891-84e2-4f41-ba0d-66420e2c6b2e
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/02/2016
ms.author: hascipio
ms.openlocfilehash: ab0bb7c78020187505c2d5f09c4de246987ecd97
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-your-offer-toohello-azure-marketplace"></a>Distribuera din erbjudande toohello Azure Marketplace
När du är nöjd med erbjudandet (det vill säga du har testat kundscenarier marknadsföring innehåll, etc.) och du är redo toolaunch begäran **Push tooproduction** på hello **publicera** fliken.  

1. hello fyra steg under hello GENOMGÅNGEN sida hello publicering portal ska vara slutfördes och en grön. Se till att följa riktlinjerna hello följs för virtuella erbjudanden.
   
    ![Rita][img-pubportal-walkthru-checked]
2. Välj hello **publicera** fliken hello listan på vänster sida hello.
   
    ![Rita][img-pubportal-menu-publish]
3. Klicka på knappen hello **begära godkännande toopush tooproduction**. När hello begäran görs hello godkännande team körs en slutgiltig granskning och sedan erbjudandet blir tillgängliga i hello Azure Marketplace.
   
    ![Rita][img-pubportal-publish-pushproduction]

> [!IMPORTANT]
> Om virtuella datorer, när du klickar på hello knappen begäran om godkännande toopush tooproduction utförs hello följande steg bakom hello scen. Du kommer att kunna tooview hello förloppet för varje steg i hello hello publicera fliken publicering av portalen. Du måste markera den här sidan med regelbundna intervall (tills hello status visar ”listan”) fel information som behöver korrigering från din slutpunkt.
> 
> * Produktion-begäran går toohello certifikatutfärdare team som Validera hello vhd först. Men om du uppdaterar redan listade erbjudandet och hello begäran har fick endast marknadsföring ändring, hoppas hello certifikatutfärdare steget över.
> * Vid hello nästa steg ska komma hello begäran toohello innehållsvalidering team som Kontrollera hello marknadsföring innehållet i hello erbjudandet.
> * Om hello senare steg lyckas, godkänns hello erbjudandet i produktion. För tillfället hello status blir ”visas” i hello publishing portal. Dock innebär inte statusen ”listan” att hello processen har slutförts. hello följande steg måste toobe slutförd innan hello erbjudandet är tillgängligt i hello Azure Marketplace.
> * När hello erbjudande godkänns i produktion i hello steget ovan hello replikering av hello erbjudande start för alla Azure-datacenter. Normalt tar 24 48hours för hello replikering toocomplete men kan ta upp tooa vecka beroende på hello storleken på hello vhd. Om du uppdaterar redan listade erbjudandet och får endast marknadsföring ändring sedan är hello replikering dock snabbare.
> * När hello replikeringen är klar, kommer hello erbjudandet är tillgänglig i hello Azure Marketplace.
> 
> Du kan alltid ta bort hello erbjudande när den är i ett **utkast** status (d.v.s. aldrig **Push toostaging** eller **Push tooproduction**). På hello **historik** klickar du på hello **Kasta utkast** knappen längst ned hello hello sidan toodelete ett utkast.
> 
> 

## <a name="production-checklist-for-all-virtual-machine-offers"></a>Checklista för produktion för alla virtuella erbjudanden
* Kontrollera att du är en Microsoft Azure Certified partner
* Hello SKU: er fliken måste hello alternativet ”dölja det här SKU från hello Marketplace eftersom alltid ska köpas via en lösningsmall” markeras som Ja endast om hello SKU är en del av en Lösningsmall. Hej i alla andra fall, det här alternativet bör alltid markeras som Nej.
* Kom ihåg: Ska du inte ändra hello SKU inställningar när hello SKU visas. Vi stöder inte den här funktionen.
* Se till att hello logotyper följer toohello Azure Marketplace-Logotypriktlinjerna nedan.
* Erbjudandet och SKU beskrivningen får inte vara samma.
* Rubrik på SKU: er och erbjuda lång sammanfattning får inte vara samma.
* SKU-rubrik och erbjuda sammanfattning får inte vara samma.
* SKU-titlar får inte vara identiska för ett erbjudande med flera SKU: er.

**Riktlinjer för Azure Marketplace-logotyp**

* hello Azure design har en enkel färgpalett. Håll nere hello antalet primära och sekundära färgerna på din logotyp.
* hello färger med hello Azure-portalen är vit och svart. Undvik därför att använda färgerna som hello bakgrundsfärgen för din logotyper. Använd en färg som gör dina logotyper framträdande i hello Azure-portalen. Vi rekommenderar enkla primära färger. Om du använder genomskinlig bakgrund och sedan kontrollera att hello logotypen/texten är inte vitt eller svart.
* Använd inte en toning bakgrund på hello-logotypen.
* Undvik att text, även ditt företag eller varumärke, på hello-logotypen.
* hello utseende och känslan av din logotyp måste vara 'flat' och undvika toningar.
* hello logotypen bör inte anpassas.

**Riktlinjer för hello hjälte logotyp:**

* hello hjälte logotypen är valfritt. hello publisher kan välja inte tooupload en hjälte logotyp. **Men en gång överförda hello hjälte ikon kan inte tas bort från hello publicering av portalen. Då hello partner måste följa hello Azure Marketplace riktlinjer för hjälte ikoner annan hello erbjudandet är inte godkänd tooproduction.**
* hello Publisher visningsnamn, SKU rubrik och hello erbjuder lång sammanfattning visas i vit färg. Därför bör du undvika att hålla alla ljusare i hello bakgrund hello hjälte ikon. Svart, vit och genomskinlig bakgrund är inte tillåtet för hjälte ikoner.
* hello utgivarens namn, SKU-rubrik, hello erbjudande lång sammanfattning och hello knapp för att skapa bäddas programmässigt inuti hello hjälte logotypen när hello erbjudandet går listan. Därför bör du inte ange text när du utformar hello hjälte logotyp. Lämna tomt utrymme hello höger eftersom hello text (d.v.s. utgivarens namn, SKU rubrik, hello erbjudande lång sammanfattning) ska tas med via programmering genom oss där. hello tomt utrymme för hello text ska vara 415 × 100 på rätt hello (och 370px från vänster hello kompenseras).

## <a name="additional-production-checklist-for-already-listed-virtual-machine-offers"></a>Ytterligare produktion checklista för virtuell dator är redan listade erbjuder
* Kontrollera om det finns redan ett erbjudande med hello samma erbjuder namn från ditt företag. Om Ja, bör du lägga till en ny version av hello SKU i hello befintliga erbjudande istället för att skapa en ny dubbla erbjudandet.
* Datadisk bör inte ändra mellan två versioner av hello samma SKU: N.
* hello Azure Marketplace stöder inte priser för ändring av hello visas SKU: er som den påverkar hello faktureringen av hello befintliga kunder. Se till att du inte ändrar hello prissättningen av hello visas SKU: er i hello regioner där hello SKU är tillgängligt. Du kan lägga till nya SKU: er eller lägga till nya regioner tooan befintliga SKU.

## <a name="next-steps"></a>Nästa steg
När hello erbjudande lanseras testa hello kunden scenarier toovalidate som alla hello kontrakt och funktioner fungerar korrekt i produktionsmiljö hello som testade och validerade i hello mellanlagringsmiljön.

## <a name="see-also"></a>Se även
* [Komma igång: hur toopublish ett erbjudande toohello Azure Marketplace](marketplace-publishing-getting-started.md)

[img-pubportal-walkthru-checked]:media/marketplace-publishing-push-to-production/pubportal-walkthru-checked.png
[img-pubportal-menu-publish]:media/marketplace-publishing-push-to-production/pubportal-menu-publish.png
[img-pubportal-publish-pushproduction]:media/marketplace-publishing-push-to-production/pubportal-publish-pushproduction.png
