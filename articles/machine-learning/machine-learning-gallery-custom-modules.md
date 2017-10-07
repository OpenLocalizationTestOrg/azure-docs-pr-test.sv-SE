---
title: aaaCortana Intelligence Gallery anpassade moduler | Microsoft Docs
description: Identifiera anpassade machine learning-moduler i Cortana Intelligence Gallery.
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 16037a84-dad0-4a8c-9874-a1d3bd551cf0
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/14/2017
ms.author: roopalik;garye
ms.openlocfilehash: e2a2d39935e6d367eb192de723fb82318d04e2be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="discover-custom-machine-learning-modules-in-cortana-intelligence-gallery"></a>Identifiera anpassade machine learning-moduler i Cortana Intelligence Gallery
[!INCLUDE [machine-learning-gallery-item-selector](../../includes/machine-learning-gallery-item-selector.md)]

## <a name="custom-modules-for-machine-learning-studio"></a>Anpassade moduler för Machine Learning Studio
Cortana Intelligence Gallery erbjuder flera [anpassade moduler](https://gallery.cortanaintelligence.com/customModules) som expanderar hello funktionerna i Azure Machine Learning Studio. Du kan importera hello moduler toouse i dina experiment, så kan du utveckla ännu mer avancerade förutsägelseanalyslösningar.

För närvarande hello galleriet erbjuder moduler på *time series analytics*, *association regler*, *clustering algoritmer* (utöver k-means) och  *visualiseringar*, och andra bestämmer hög grad verktyget moduler.


## <a name="discover"></a>Utforska
toobrowse anpassade moduler [i hello galleriet](http://gallery.cortanaintelligence.com)under **mer**väljer **anpassade moduler**.

![Välj Anpassad moduler på startsidan för hello-galleriet](media/machine-learning-gallery-custom-modules/select-custom-modules-in-gallery.png)

Hej  **[anpassade moduler](https://gallery.cortanaintelligence.com/customModules)**  sidan visar en lista över nyligen tillagda och populära moduler. tooview alla anpassade moduler, Välj hello **se alla** knappen. toosearch för en anpassad modul, Välj **se alla**, och välj sedan filtreringsvillkor. Du kan också ange sökvillkor i hello **Sök** rutan hello överst på sidan för hello galleri.

![Välj ”Visa alla” toobrowse alla anpassade moduler](media/machine-learning-gallery-custom-modules/click-see-all-for-all-custom-modules.png)

### <a name="understand"></a>Förstå

hello anpassad modul tooopen hello modulen informationssidan Välj toounderstand hur en anpassad modul publicerade fungerar. hello informationssidan ger en konsekvent och informativa learning-upplevelse. Till exempel hello informationssidan visar hello syftet hello modulen och visar förväntade indata, utdata och parametrar. hello informationssidan har även en länk toohello underliggande källkoden, vilket du kan granska och anpassa.

### <a name="comment-and-share"></a>Kommentar och dela
En anpassad modul information i sidan hello **kommentarer** avsnitt, du kan kommentera, ge feedback eller ställa frågor om hello-modulen. Du kan också dela hello modulen med vänner och kollegor på Twitter och LinkedIn. Du också kan e-en länk toohello modulen informationssidan, tooinvite andra användare tooview hello-sidan.

![Dela objektet med vänner](media/machine-learning-gallery-how-to-use-contribute-publish/share-links.png)

![Lägga till egna kommentarer](media/machine-learning-gallery-how-to-use-contribute-publish/comments.png)

## <a name="import"></a>Importera
Du kan importera en anpassad modul från hello galleriet tooyour egna experiment.

Cortana Intelligence Gallery erbjuder två sätt tooimport en kopia av hello modulen:

* **Från hello galleriet**. När du importerar en anpassad modul från hello galleriet måste få du också en exempelexperimentet som ger ett exempel på hur toouse hello modulen.
* **Inifrån Maskininlärning Studio**. Du kan importera en anpassad modul när du arbetar i Machine Learning Studio (i detta fall kan du inte får hello exempelexperimentet).

### <a name="from-hello-gallery"></a>Från hello galleri

1. Öppna hello modulen informationssidan i hello Gallery. 
2. Välj **öppna i Studio**.
   
    ![Öppna anpassad modul från hello galleri](media/machine-learning-gallery-custom-modules/open-custom-module-from-gallery.png)
   
Varje anpassad modul innehåller en exempelexperimentet som visar hur toouse hello modulen. När du väljer **öppna i Studio**, hello exempelexperimentet öppnas i Machine Learning Studio-arbetsytan. (Om du inte redan är inloggad tooStudio uppmanas du toofirst logga in med ditt Microsoft-konto.)

Dessutom toohello exempel experiment, hello anpassad modul är kopierade tooyour arbetsytan. Även placeras den i din modulpaletten med alla inbyggda eller anpassade Machine Learning Studio modulerna. Du kan nu använda den i dina egna experiment, t.ex. en annan modul på arbetsytan.

### <a name="from-within-machine-learning-studio"></a>Inifrån Maskininlärning Studio

1. Välj i Machine Learning Studio **ny**.
2. Välj **modulen**. Du kan välja från en lista med moduler i galleriet eller söka efter en viss modul med hello **Sök** rutan.
3. Peka med musen på en modul och välj sedan **Import-Module**. (tooget information om hello modulen, Välj **vyn i galleriet**. Då kommer du toohello modulen informationssidan i hello Gallery.)
   
    ![Importera anpassad modul till Machine Learning Studio](media/machine-learning-gallery-custom-modules/add-custom-module-in-studio.png)

anpassad hello-modul är kopierade tooyour arbetsytan och placeras i din modulpaletten med din inbyggda eller anpassade Machine Learning Studio-moduler. Du kan nu använda den i dina egna experiment, t.ex. en annan modul på arbetsytan.

## <a name="use"></a>Användning

Oavsett vilken metod du tooimport en anpassad modul när du importerar hello modulen hello modulen är placerad i modulpaletten i Machine Learning Studio. Från din modulpaletten kan du använda hello anpassad modul i alla experiment på arbetsytan, precis som en annan modul.

toouse en importerade modulen:

1. Skapa ett experiment eller öppna ett befintligt experiment.
2. tooexpand hello listan över anpassade moduler i din arbetsyta i hello modulpaletten Välj **anpassade**. Hej modulpaletten är toohello till vänster i hello experimentet.
   
    ![Anpassade Modullista Studio paletten](media/machine-learning-gallery-custom-modules/custom-module-in-studio-palette.png)
3. Välj hello-modul som du har importerat och dra den tooyour experiment.


**[Gå toohello-galleriet](http://gallery.cortanaintelligence.com)**

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

