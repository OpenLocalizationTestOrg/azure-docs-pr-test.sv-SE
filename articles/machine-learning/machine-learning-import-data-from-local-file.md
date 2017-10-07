---
title: "aaaImport data från en fil i Azure Machine Learning Studio | Microsoft Docs"
description: "Lär dig hur tooupload en träningsdata fil från din hårddisk tooAzure Machine Learning Studio. Detta skapar en dataset-modul i hello arbetsyta."
keywords: "Importera data, dataformatet, datatyper, datakällor, träningsdata"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: c0dd9e90-23c4-4f64-8b8f-489ad79f047b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye;bradsev
ms.openlocfilehash: 636facd9042145382c953a1c75969149ede6f6fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="import-training-data-from-a-file-on-your-hard-drive-into-machine-learning-studio"></a>Importera träningsdata från en fil på hårddisken till Machine Learning Studio
[!INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]

Lär dig hur tooupload en fil från din hårddisk toouse som utbildningsdata i Azure Machine Learning Studio. Genom att importera hello datafilen, har du en dataset-modul som är redo för användning i arbetsytan.

## <a name="steps-tooimport-data-from-a-local-file"></a>Steg tooimport data från en lokal fil
tooimport data från en lokal hårddisk hello följande:

1. Klicka på **+ ny** längst hello hello Machine Learning Studio-fönstret.
2. Välj **DATASET** och **från en lokal fil**.
3. I hello **ladda upp en ny datamängd** dialogrutan Bläddra toohello-fil som du vill tooupload
4. Ange ett namn, identifiera hello datatyp och även ange en beskrivning. En beskrivning rekommenderas – kan du toorecord alla egenskaper om hello data som du vill tooremember när du använder hello data i framtiden hello.
5. Hej kryssrutan **detta är hello ny version av en befintlig datauppsättning** kan du tooupdate en befintlig datamängd med nya data. Klicka på den här kryssrutan och ange hello namnet på en befintlig datauppsättning.

![Ladda upp en ny datamängd](media/machine-learning-import-data-from-local-file/upload-dataset.png)

Under överföring visas ett meddelande om att filen överförs. Överför tiden beror på hello storleken på dina data och hello hastigheten på anslutningstjänst toohello. Om du vet hello filen tar lång tid kan göra du andra saker i Machine Learning Studio medan du väntar. Dock gör stänger hello webbläsare hello data överför toofail.

## <a name="dataset-module-is-ready-for-use"></a>DataSet-modulen är redo för användning
När data överförs lagras i en dataset-modul och är tillgängliga tooany experiment på arbetsytan.

När du redigerar ett experiment kan du hitta hello datauppsättningar som du har skapat i hello **Mina datauppsättningar** listan under hello **sparade datauppsättningar** hello modulpaletten-listan. Du kan dra och släpp hello dataset till hello experimentet när du vill toouse hello dataset för ytterligare analys och maskininlärning.
