---
title: "Kundens godkännande fas i livscykeln Team datavetenskap Process - Azure | Microsoft Docs"
description: "Mål, uppgifter och leveranser för kundens godkännande steg i datavetenskap projekt"
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/04/2017
ms.author: bradsev;
ms.openlocfilehash: e150b3e7c7e98443264dd5b8aaeda1bfe6047b2c
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/09/2017
---
# <a name="customer-acceptance"></a>Kundgodkännande

Den här artikeln beskrivs mål, uppgifter och leveranser som är associerade med kundens godkännande fas av Team Data vetenskap processen (TDSP). Denna process tillhandahåller en rekommenderad livscykel som du kan använda för att strukturera datavetenskap projekt. Livscykeln beskrivs viktiga steg som projekt vanligtvis köra, ofta upprepade gånger:

   1. **Så här fungerar för företag**
   2. **Datainsamling och förstå**
   3. **Modeling**
   4. **Distribution**
   5. **Kundens godkännande**

Här är en bild av TDSP livscykeln: 

![TDSP livscykel](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goal"></a>Målet
**Slutför projektet leveranser**: Bekräfta att pipeline, modellen och deras distribution i en produktionsmiljö uppfyller kundens mål.

## <a name="how-to-do-it"></a>Hur du gör det.
Det finns två huvudsakliga uppgifter som beskrivs i det här steget:

   * **Systemvalidering**: Bekräfta att den distribuerade modellen och pipeline uppfyller kundens behov.
   * **Projektet hand av**: lämnar projektet till den enhet som ska köra den i produktion.

Kunden bör verifiera att systemet uppfyller sina affärsbehov och den svarar på frågor med acceptabel Precision att distribuera systemet till produktion för användning med sina klientprogram. All dokumentation är slutförd och granskas. Projektet är lämnas av till entitet som ansvarar för åtgärder. Den här entiteten kanske till exempel en IT- eller kunden datavetenskap team eller en agent på kundens som ansvarar för att köra systemet i produktion. 

## <a name="artifacts"></a>Artefakter
Den huvudsakliga artefakt som skapas i det här sista steget är det **avsluta rapporten projektets för kunden**. Den tekniska rapporten innehåller information om projektet som är användbara för att lära dig mer om hur du använder systemet. TDSP ger en [avsluta rapporten](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Project/Exit%20Report.md) mall. Du kan använda mallen som den är eller anpassa den för specifika kundens behov. 


## <a name="next-steps"></a>Nästa steg

Här är länkar till varje steg i livscykeln för TDSP:

   1. [Så här fungerar för företag](lifecycle-business-understanding.md)
   2. [Datainsamling och förstå](lifecycle-data.md)
   3. [Modeling](lifecycle-modeling.md)
   4. [Distribution](lifecycle-deployment.md)
   5. [Kundens godkännande](lifecycle-acceptance.md)

Vi ger fullständig slutpunkt till slutpunkt genomgång som visar alla steg i processen för specifika scenarier. Den [exempel genomgång](walkthroughs.md) artikeln innehåller en lista över scenarier med länkar och miniatyr beskrivningar. Genomgångar illustrerar hur du kombinerar moln, lokala verktyg och tjänster i ett arbetsflöde eller en rörledning för att skapa ett intelligent program. 

Exempel på hur du utför stegen i TDSPs som använder Azure Machine Learning Studio finns [använder TDSP med Azure Machine Learning](http://aka.ms/datascienceprocess).
