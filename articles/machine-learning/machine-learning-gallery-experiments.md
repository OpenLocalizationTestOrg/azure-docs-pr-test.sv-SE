---
title: aaaCortana Intelligence Gallery experimenten | Microsoft Docs
description: Identifiera och dela experiment i Cortana Intelligence Gallery.
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: f4248922-c961-4d3a-9e1b-aec743210166
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: roopalik;garye
ms.openlocfilehash: 55994846e277061bbb3febb759e0c68ad6cee7e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="discover-experiments-in-cortana-intelligence-gallery"></a>Identifiera experiment i Cortana Intelligence Gallery
[!INCLUDE [machine-learning-gallery-item-selector](../../includes/machine-learning-gallery-item-selector.md)]

## <a name="experiments-for-machine-learning-studio"></a>Experiment för Machine Learning Studio
hello Gallery innehåller en mängd olika [experiment](https://gallery.cortanaintelligence.com/experiments) som har utvecklats i [Azure Machine Learning Studio](https://studio.azureml.net). Experiment mellan snabb proof of concept experiment som visar specifik machine learning-tekniken, toofully utvecklat lösningar för komplexa machine learning problem.

> [!NOTE]
> En ***experimentera*** är en arbetsyta i Machine Learning Studio som du kan använda tooconstruct en prediktiv analysmodell. Du kan skapa hello modellen genom att ansluta data med olika analytiska moduler. Du kan prova olika idéer gör utvärderingsversion körs och slutligen distribuera din modell som en webbtjänst i Azure. Ett exempel på hur toocreate ett grundläggande experiment finns [självstudie om maskininlärning: skapa ditt första experiment i Azure Machine Learning Studio](machine-learning-create-experiment.md). För en mer omfattande genomgång av hur toocreate en förutsägelseanalys finns [genomgång: utveckla en förutsägelseanalys för kreditriskbedömning i Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md).
>
>

## <a name="discover"></a>Utforska
toobrowse experiment [i hello galleriet](http://gallery.cortanaintelligence.com), hello överst i hello galleriet startsidan, Välj **experiment**.

Hej  **[experiment](https://gallery.cortanaintelligence.com/experiments)**  sidan visas en lista över nyligen tillagda och populära experiment. Välj om toosee alla experiment hello **se alla** knappen. toosearch för en specifik experiment, Välj **se alla**, och välj sedan filtreringsvillkor. Du kan också ange sökvillkor i hello **Sök** rutan hello överst på sidan för hello galleri.

Du kan få mer information om ett experiment på informationssidan för hello experiment. tooopen ett experiment på sidan Välj hello experiment. På ett experiment sidan information i hello **kommentarer** avsnitt, du kan kommentera, ge feedback eller ställa frågor om hello experiment. Du kan också dela hello experiment med vänner och kollegor på Twitter och LinkedIn. Du kan också skicka en länk toohello experiment på sidan tooinvite andra användare tooview hello-sidan.

![Dela objektet med vänner](media/machine-learning-gallery-how-to-use-contribute-publish/share-links.png)

![Lägga till egna kommentarer](media/machine-learning-gallery-how-to-use-contribute-publish/comments.png)

## <a name="download"></a>Ladda ned
Du kan hämta en kopia av alla experiment hello galleriet tooyour Machine Learning Studio-arbetsytan. Sedan kan du ändra din kopia toocreate egna lösningar.

Cortana Intelligence Gallery erbjuder två sätt tooimport en kopia av ett experiment:

* **Från hello galleriet**. Om du hittar ett experiment som du vill använda i hello galleriet kan du hämta en kopia och sedan öppna den i Machine Learning Studio-arbetsytan.
* **Inifrån Maskininlärning Studio**. I Machine Learning Studio kan du använda alla experiment i hello galleriet som mall-toocreate ett nytt experiment.

### <a name="from-hello-gallery"></a>Från hello galleri

1. Öppna sidan hello experiment i hello galleriet.
2. Välj **öppna i Studio**.

    ![Öppna experiment från hello galleri](media/machine-learning-gallery-experiments/open-experiment-from-gallery.png)

När du väljer **öppna i Studio**, hello experiment öppnas i Machine Learning Studio-arbetsytan. (Om du inte redan är inloggad tooMachine Learning Studio uppmanas du toofirst logga in med ditt Microsoft-konto.)

### <a name="from-within-machine-learning-studio"></a>Inifrån Maskininlärning Studio

1. Välj i Machine Learning Studio **ny**.
2. Välj **Experiment**. Du kan välja från en lista över galleriet experiment eller söka efter en specifik experiment med hello **Sök** rutan.
3. Peka med musen på ett experiment och välj sedan **öppna i Studio**. (Välj toosee information om hello experiment **vyn i galleriet**. Då kommer du toohello experiment informationssidan i hello Gallery.)

    ![Öppna galleriet experiment från i Machine Learning Studio](media/machine-learning-gallery-experiments/open-experiment-from-studio.png)

Du kan anpassa, iterera och distribuera ett hämtade experiment som andra experiment som du skapar i Machine Learning Studio.

![Experiment som öppnas i Studio](media/machine-learning-gallery-experiments/experiment-open-in-studio.png)

## <a name="contribute"></a>Bidra
När du loggar in toohello galleriet blir medlem i hello galleri. Du kan bidra egna experiment, så att andra användare kan dra nytta av hello lösningar som du har upptäckt som medlem i hello community.

### <a name="publish-your-experiment-toohello-gallery"></a>Publicera experiment toohello galleriet

1. Logga in tooMachine Learning Studio med ditt Microsoft-konto.
2. Skapa experimentet och sedan köra den.
3. När du är klar toopublish experimentet i hello galleriet i hello lista med åtgärder under hello experimentet väljer **publicera tooGallery**.

    ![Välj ”publicera tooGallery”](media/machine-learning-gallery-experiments/publish-experiment-to-gallery.png)
4. På hello **Experiment beskrivning** anger du en rubrik och taggar. Se hello rubrik och taggar beskrivande. Markera hello-metoder som du använde eller hello verkliga problemet du lösa. Ett exempel på en beskrivande experiment rubrik är ”binär klassificering: Twitter-Sentiment Analysis”.

    ![Ange rubrik och taggar för publicering](media/machine-learning-gallery-experiments/experiment-description.png)
5. I hello **sammanfattning** anger du en sammanfattning av experimentet. Beskriv hello problemet hello experiment löser och hur du närmat sig.
6. I hello **detaljerad beskrivning** rutan, beskriver hello steg som du har gjort i varje del av experimentet. Vissa användbara avsnitt tooinclude är:
   * Skärmbild av experimentet diagram
   * Förklaring och datakällor
   * Databearbetning
   * Funktionstekniker
   * Beskrivning
   * Resultat och utvärdering av modellen prestanda

   Du kan använda markdown tooformat beskrivningen. toosee hur posterna på hello experimentet beskrivningssida ser ut när hello experiment publiceras, Välj **Preview**.

   ![Välj ”förhandsversion” toosee texten utseende](media/machine-learning-gallery-experiments/preview-markdown-text.png)

   > [!TIP]
   > Hej textrutor som föreskrivs markdown redigering och förhandsgranskning små. Vi rekommenderar att du skriva dokumentationen experiment i en markdown-redigerare, kopiera den och sedan klistra in hello slutförts dokumentationen i hello textruta i hello Gallery. När du har publicerat experimentet du eventuella ändringar med hjälp av standard webbaserade verktyg att använda markdown för redigering och förhandsgranskning.

7. På hello **valet** väljer du miniatyrbilden för experimentet. hello miniatyrbilden visas överst hello hello experiment informationssidan och i hello experiment sida vid sida. Andra användare ser hello miniatyrbilden när de söker hello-galleriet. Du kan ladda upp en bild från datorn eller välj en bild från hello Gallery.
    </br>
    ![Ladda upp eller välj en bild för hello galleri](media/machine-learning-gallery-experiments/select-gallery-image.png)
8. På hello **inställningar** sidan under **synlighet**, Välj om toopublish ditt innehåll offentligt (**offentliga**) eller toohave den tillgänglig endast toopeople som har en länk toohello sida (**nya**).

    ![Välj om toopublish offentligt eller som inte finns i listan](media/machine-learning-gallery-experiments/choose-public-or-unlisted.png)

    <!-- -->

   > [!TIP]
   > Om du vill att dokumentationen verkar vara korrekta innan du släpper offentligt toomake måste du först publicera hello experiment som **nya**. Senare kan du ändra hello synlighet inställningen för**offentliga** på informationssidan för hello experiment.
   >
   >
9. toopublish hello experiment toohello galleriet väljer hello **OK** är markerat.

    ![Välj hello OK markerat toopublish hello experiment](media/machine-learning-gallery-experiments/ok-checkmark.png)

Tips om hur toopublish hög kvalitet galleriet experimentera finns [Tips för att dokumentera och publicering av experimentet](#tips-for-documenting-and-publishing-your-experiment).

Det är den - alla övningen.

Du kan nu visa experimentet i hello galleriet och dela hello länken med andra. Om du har publicerat experimentet genom att använda hello **offentliga** synlighet inställningen hello experiment visar upp Bläddra och Sök resulterar i hello Gallery. Du kan redigera experiment dokumentationen på informationssidan för hello experiment när du är inloggad toohello Gallery.

toosee hello lista över dina bidrag Markera avbildningen i hello övre högra hörnet på en gallerisida. Markera namnet-tooopen din kontosida.

![Välj namnet på ditt konto](media/machine-learning-gallery-experiments/click-account-name.png)

### <a name="update-your-experiment"></a>Uppdatera experimentet
Om du behöver göra du ändringar toohello arbetsflöde (moduler, parametrar och så vidare) i ett experiment som du publicerade toohello Gallery. Gör alla ändringar som du vill toomake toohello experiment och sedan publicera igen i Machine Learning Studio. Experimentet publicerade uppdateras med dina ändringar.

Du kan ändra följande information för experimentet direkt i hello galleriet hello:

* Namn på experimentet
* Sammanfattning eller beskrivning
* Taggar
* Bild
* Inställningar (**offentliga** eller **nya**)

Du kan också ta bort hello experiment från hello Gallery.

Du kan göra dessa ändringar eller ta bort hello experiment från hello experiment sidan eller från din profilsida i hello Gallery.


#### <a name="from-hello-experiment-details-page"></a>Från hello experimentera informationssidan
Hello experiment information på sidan Välj toochange hello information för ditt experiment **redigera**.

![Välj Redigera tooedit experimentet](media/machine-learning-gallery-experiments/edit-button.png)

hello informationssidan försätts i redigeringsläge. Välj toomake ändringar **redigera** nästa toohello experimentera namn, sammanfattning eller taggar. När du är klar väljer **klar**.

![Välj ”Redigera” tooedit information och välj ”klar” när du är klar](media/machine-learning-gallery-experiments/edit-details-page.png)

toochange hello synlighetsinställningar för hello experiment (**offentliga** eller **nya**), eller toodelete hello experiment från hello galleriet väljer hello **inställningar** ikon.

![Välj ”inställningar” toochange synlighet eller toodelete hello experiment](media/machine-learning-gallery-experiments/settings-button.png)

#### <a name="from-your-profile-page"></a>Från profilsidan
Välj hello NEDPIL för hello experiment på din profilsida och välj sedan **redigera**. Då kommer du toohello informationssidan för ditt experiment i redigeringsläge. När du är klar med ändringarna, Välj **klar**.

toodelete hello experiment från hello galleriet, Välj **ta bort**.

![Välj ”Redigera” eller ”ta bort”](media/machine-learning-gallery-experiments/edit-delete-buttons.png)

### <a name="tips-for-documenting-and-publishing-your-experiment"></a>Tips för att dokumentera och publicering av experimentet
* Du kan anta att hello läsaren har tidigare datavetenskap upplevelse, men det kan vara användbara toouse enkelt språk. Innehåller en förklaring i detalj när det är möjligt.
* Cortana Intelligence Suite är relativt nytt. Inte alla läsare känner till hur toouse den. Ange tillräckligt med information och detaljerade förklaringar toohelp läsare navigera experimentet.
* Visuell information kan vara användbar för läsare toointerpret och använda dokumentationen experiment på rätt sätt. Visuell information inkluderar experimentdiagram och skärmdumpar av data. Mer information om hur tooinclude bilder i experimentet-dokumentationen finns hello [publiceringsriktlinjer och exempel samling](https://gallery.cortanaintelligence.com/Collection/Publishing-Guidelines-and-Examples-1).
* Om du inkluderar en datamängd i experimentet (det vill säga du inte importerar hello datauppsättningen genom hello Import Data module), hello datauppsättning är en del av experimentet och publiceras i hello Gallery. Kontrollera att hello datauppsättning som du publicerar har licensvillkoren som tillåter delning och hämtning av vem som helst. Galleriet bidrag omfattas hello Azure [användningsvillkoren](https://azure.microsoft.com/support/legal/website-terms-of-use/).

## <a name="frequently-asked-questions"></a>Vanliga frågor och svar
**Vad är hello kraven för att skicka eller redigerar en bild för mitt experiment?**

Bilder som du skickar med experimentet har använt toocreate ett experiment panelen för ditt bidrag. Vi rekommenderar att bilder är mindre än 500 KB, med en aspect ratio av 3:2 och en upplösning på 960 &#215; 640.

**Vad händer toohello datauppsättning jag använde i hello experiment? Hello datauppsättning också publiceras i hello galleriet?**

Om din datauppsättning är en del av experimentet och inte importeras via hello importera Data modul, har hello datauppsättning publicerats i hello galleriet som en del av experimentet. Kontrollera att hello datauppsättning som du publicerar med experimentet har hello lämpliga licensvillkoren. hello licensvillkoren ska tillåta alla tooshare och hämta hello-data.

**Jag har ett experiment som använder en Import Data modulen toopull data från Azure HDInsight eller SQL Server. Mina autentiseringsuppgifter tooretrieve hello data används. Kan jag publicera den här typen av experimentet? Hur kan jag vara säker på att mina autentiseringsuppgifter inte delas?**

Du kan för närvarande inte publicera ett experiment som använder autentiseringsuppgifter i hello Gallery.

**Hur anger flera taggar?**

När du har angett en tagg tooenter en annan tagg, trycker du på tabbtangenten hello.

**[Gå toohello-galleriet](http://gallery.cortanaintelligence.com)**

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]
