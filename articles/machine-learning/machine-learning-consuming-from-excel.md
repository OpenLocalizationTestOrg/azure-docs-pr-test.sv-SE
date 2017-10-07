---
title: "aaaConsume en Machine Learning-webbtjänst från Excel | Microsoft Docs"
description: "Använda en Azure Machine Learning-webbtjänst från Excel"
services: machine-learning
documentationcenter: 
author: tedway
manager: jhubbard
editor: cgronlun
ms.assetid: 3f3cdd2f-1816-487e-ab78-530e01e9788f
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 2/13/2017
ms.author: tedway
ms.openlocfilehash: e2e8bbf7ba75b6618a0285539555ce175ec03c1a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="consuming-an-azure-machine-learning-web-service-from-excel"></a>Använda en Azure Machine Learning-webbtjänst från Excel
 Azure Machine Learning Studio gör det enkelt toocall webbtjänster direkt från Excel utan hello måste toowrite någon kod.

Om du använder Excel 2013 (eller senare) eller Excel Online, så vi rekommenderar att du använder hello Excel [Excel-tillägg](machine-learning-excel-add-in-for-web-services.md).

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="steps"></a>Steg
Publicera en webbtjänst. [Den här sidan](machine-learning-walkthrough-5-publish-web-service.md) förklarar hur toodo den. För närvarande stöds endast hello Excel-arbetsboken funktion för frågor och svar tjänsterna som har ett enda utflöde (det vill säga en enda bedömningsprofil etikett). 

När du har en webbtjänst kan klicka på hello **WEB SERVICES** avsnittet hello vänster hello Studio och väljer sedan hello web service tooconsume från Excel.

**Klassiska webbtjänst**

1. På hello **INSTRUMENTPANELEN** för hello-webbtjänsten är en rad för hello **frågor och svar** service. Om den här tjänsten har ett enda utflöde, bör du se hello **hämta Excel-arbetsbok** länk på den raden.
   
    ![][1]
2. Klicka på **hämta Excel-arbetsbok**.

**Ny webbtjänst**

1. Markera i hello Azure Machine Learning-webbtjänst portal **förbruka**.
2. Hello förbruka i sidan hello **Web-förbrukning alternativ** klickar hello Excel-ikonen.

**Med hjälp av hello arbetsboken**

1. Öppna hello arbetsboken.
2. En säkerhetsvarning visas. Klicka på hello **Aktivera redigering av** knappen.
   
    ![][2]
3. En säkerhetsvarning visas. Klicka på hello **Aktivera innehåll** knappen toorun makron i kalkylbladet.
   
    ![][3]
4. När makron är aktiverade, skapas en tabell. Kolumner i blå är krävs som indata till hello Resursposter webbtjänst eller **parametrar**. Observera hello utdata från hello RR-tjänsten, **FÖRUTSADE värden** i grönt. När alla kolumner för en viss rad fylls hello arbetsboken automatiskt hello bedömningen API-anrop och visar hello bedömas resultat.
   
    ![][4]
5. tooscore mer än en rad, förutsade fill hello andra raden med data och hello värden genereras. Du kan även klistra in flera rader på samma gång.

Du kan använda någon av hello Excel-funktioner (diagram, power karta, villkorlig formatering, etc.) med hello förutsade toohelp värdena visualisera hello data.    

## <a name="sharing-your-workbook"></a>Dela din arbetsbok
API-nyckel måste vara en del av hello kalkylblad för hello makron toowork. Det innebär att du ska dela hello arbetsboken endast med entiteter/personer som du litar på.

## <a name="automatic-updates"></a>Automatiska uppdateringar
En RR-anrop i dessa två situationer:

1. Hej första gången en rad har innehåll i alla dess **parametrar**
2. Varje gång någon av hello **parametrar** ändringar i en rad som har alla dess **parametrar** anges.

[1]: ./media/machine-learning-consuming-from-excel/excellink.png
[2]: ./media/machine-learning-consuming-from-excel/enableeditting.png
[3]: ./media/machine-learning-consuming-from-excel/enablecontent.png
[4]: ./media/machine-learning-consuming-from-excel/sampletable.png
