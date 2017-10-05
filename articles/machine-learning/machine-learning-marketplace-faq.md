---
title: "(föråldrad) Vanliga frågor och svar - publicera och använda Machine Learning-appar i Azure Marketplace | Microsoft Docs"
description: "(föråldrad) Vanliga frågor och svar om publishing Machine Learning-appar i Azure Marketplace"
services: machine-learning
documentationcenter: 
author: bharaths
manager: jhubbard
editor: cgronlun
ms.assetid: 26b3a1e7-8b9a-4004-98bc-17456d4c25e8
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: bharaths
ROBOTS: NOINDEX
redirect_url: https://gallery.cortanaintelligence.com/
redirect_document_id: TRUE
ms.openlocfilehash: a4631dfeb2f817b3a3c612a8f6af48398e4f2ab9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deprecated-publishing-and-using-machine-learning-apps-in-the-azure-marketplace-faq"></a>(föråldrad) Publicera och använda Machine Learning-appar i Azure Marketplace: vanliga frågor och svar

> [!NOTE]
> Tjänsterna och DataMarket är som har återkallats och befintliga prenumerationer ska tas bort och avbrutits startar 3/31/2017. Därför kan är den här artikeln inaktuell. 
> 
> Alternativt kan du publicera Machine Learning-experiment till den [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/) för datavetenskap community. Mer information finns i [resursen och identifiera resurser i Cortana Intelligence Gallery](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-gallery-how-to-use-contribute-publish).


## <a name="questions-about-consuming-from-marketplace"></a>Frågor om förbrukar från Marketplace
**1. Varför visas följande felmeddelande när du har angett indata för webbtjänsten:**

**Förfrågan resulterade i ett backend-timeout eller backend-fel. Teamet undersöker problemet. Vi kan Vi beklagar besväret. (500)**

Din indataparametrar överensstämmer inte med formatet som krävs för specifika webbtjänsten. Hänvisa till länken motsvarande att hitta rätt format för indataparametrar och begränsningar för den här webbtjänsten.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

**2. Om jag kopiera API-länk för den webbtjänst som visas på sidan ”Utforska den här datauppsättningen” och klistra in det i ett nytt webbläsarfönster, vilka autentiseringsuppgifter ska jag använda för att komma åt resultaten och hur kan jag se dem?**

Du bör använda Marketplace-konto som användarnamnet och den primära konto nyckeln som lösenord. Den primära konto nyckeln finns på den **Utforska den här datauppsättningen** sidan under beskrivningen av webbtjänsten (klickar du på den **visa** knappen). Resultatet visas i webbläsaren eller det kan vara tillgänglig för nedladdning, beroende på vilken webbläsare du använder.

**3. Varför visas följande felmeddelande när jag ange indata för webbtjänsten på sidan ”Utforska den här datauppsättningen”:** 

**Ett oväntat fel uppstod när din begäran behandlades. Försök igen.**

En eller flera indataparametrarna för webbtjänsten kanske har överskridit maxlängden när förbrukar webbtjänsten på marketplace **Utforska den här datauppsättningen** sidan. Tjänsterna kan anropas med ett längre indatalängden med hjälp av HTTP POST-metoderna. Exempel finns i [exempel lösningar med hjälp av R på Machine Learning och publiceras på Marketplace](machine-learning-r-csharp-web-service-examples.md).

**4. Varför kan jag inte se någonting i ”API EXPLORER” fliken int arkivet på den klassiska Azure-portalen?** 

Detta är ett känt problem med Azure klassiska Portal Marketplace. Teamet arbetar för att lösa problemet. 

## <a name="questions-about-publishing-from-azure-machine-learning-on-marketplace"></a>Frågor om hur du publicerar från Azure Machine Learning på Marketplace
**1. Varför är min transaktioner av logotyper eller bilder inte uppdaterar för min webbtjänst?** 

Logotyper och bilder cachelagras i publishing portal och det kan ta upp till 10 dagar för nya logotypen eller bilden att uppdatera på portalen.

**2. Varför är min webbtjänst på Marketplace visar ett felmeddelande ”information” fliken?**

Det finns ett känt problem i Marketplace vid anslutning till Azure Machine Learning för information om tjänsten. Teamet arbetar för att lösa problemet.

**3. Varför fungerar inte R exempelkoden i Azure Machine Learning-webbtjänster för förbrukning av webbtjänster på Marketplace?**

Autentiseringssystem är olika vid anslutning till Azure Machine Learning-webbtjänster direkt jämfört med att ansluta till dessa webbtjänster på marknadsplatsen. Tjänsterna i Marketplace är OData och de kan anropas med metoderna GET eller POST. 

**4. Varför är support länkar för min webbtjänst erbjuder inte uppdateras korrekt för vissa min erbjudanden?**

Stöd för länkar är globala per utgivare, inte per erbjudandet. 

**5. Hur publicera en webbtjänst med indata batchläge i Marketplace?**

Inkommande batchläge stöds för närvarande inte i Marketplace-webbtjänster.

**6. Vem ska jag kontakta för att få hjälp om jag har frågor om att bli en utgivare, eller om jag har problem vid publicering?**

Kontakta Azure Marketplace-teamet på < mailto:datamarketbd@microsoft.com > för mer information.

