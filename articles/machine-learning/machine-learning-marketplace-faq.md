---
title: "aaa(deprecated) vanliga frågor och svar - publicera och använda Machine Learning-appar i Azure Marketplace | Microsoft Docs"
description: "(föråldrad) Vanliga frågor om publishing Machine Learning-appar i hello Azure Marketplace"
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
redirect_document_id: True
ms.openlocfilehash: b3ae45dfff211fe9baccaf54faaf360a8309c780
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-publishing-and-using-machine-learning-apps-in-hello-azure-marketplace-faq"></a>(föråldrad) Publicera och använda Machine Learning-appar i hello Azure Marketplace: vanliga frågor och svar

> [!NOTE]
> Tjänsterna och DataMarket är som har återkallats och befintliga prenumerationer ska tas bort och avbrutits startar 3/31/2017. Därför kan är den här artikeln inaktuell. 
> 
> Alternativt kan du publicera din Machine Learning-experiment toohello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/) hello förmån för hello datavetenskap community. Mer information finns i [resursen och identifiera resurser i hello Cortana Intelligence Gallery](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-gallery-how-to-use-contribute-publish).


## <a name="questions-about-consuming-from-marketplace"></a>Frågor om förbrukar från Marketplace
**1. Varför visas följande felmeddelande när du har angett indata för hello webbtjänsten hello:**

**hello-förfrågan resulterade i ett backend-timeout eller backend-fel. hello-teamet undersöker hello problem. Vi kan tyvärr för hello besvär. (500)**

Din indataparametrar överensstämmer inte toohello format som krävs för specifika hello-webbtjänsten. Se toohello motsvarande dokumentationen länken toofind hello rätt format för indataparametrar och hello begränsningar för den här webbtjänsten.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

**2. Om jag kopiera hello API-länk för hello-webbtjänst som visas på sidan ”Utforska den här datauppsättningen” hello och klistra in den i ett nytt webbläsarfönster, vilka autentiseringsuppgifter ska jag använda tooaccess hello resultat, och hur kan jag se dem?**

Du bör använda Marketplace-konto som hello användarnamn och hello primära nyckel som hello lösenord. hello primära konto nyckeln finns på hello **Utforska den här datauppsättningen** sidan under hello beskrivning av hello web service (klicka på hello **visa** knappen). hello resultat visas i webbläsaren hello eller vara tillgängliga för hämtning, beroende på vilken webbläsare du använder.

**3. Varför visas följande felmeddelande när jag ange hello indata för hello webbtjänsten på hello ”Utforska den här datauppsättningen” sidan hello:** 

**Ett oväntat fel uppstod när din begäran behandlades. Försök igen.**

En eller flera indataparametrarna för webbtjänsten kanske har överskridit maxlängden hello när förbrukar hello webbtjänst på hello marketplace **Utforska den här datauppsättningen** sidan. hello-tjänster kan anropas med ett längre indatalängden med hjälp av HTTP POST-metoderna. Exempel finns i [exempel lösningar med hjälp av R på Machine Learning och publicerade tooMarketplace](machine-learning-r-csharp-web-service-examples.md).

**4. Varför inte visas i hello ”API EXPLORER” fliken int hello Store i hello Azure klassiska Portal?** 

Detta är ett känt problem med hello Azure klassiska Portal Marketplace. hello-teamet arbetar tooresolve problemet. 

## <a name="questions-about-publishing-from-azure-machine-learning-on-marketplace"></a>Frågor om hur du publicerar från Azure Machine Learning på Marketplace
**1. Varför är min transaktioner av logotyper eller bilder inte uppdaterar för min webbtjänst?** 

Logotyper och bilder cachelagras i hello publishing portal och det kan ta upp too10 dagar för hello nya logotypen eller avbildning tooupdate på hello-portalen.

**2. Varför är hello ””-informationsfliken för min webbtjänst på Marketplace visar ett felmeddelande?**

Det är ett känt problem i Marketplace vid anslutning tooAzure Machine Learning för information om tjänsten. hello-teamet arbetar tooresolve problemet.

**3. Varför fungerar inte hello R exempelkoden i hello Azure Machine Learning-webbtjänster för konsumerande hello web services Marketplace?**

hello autentiseringssystem är olika jämfört ansluta tooAzure Machine Learning-webbtjänster direkt tooconnecting toothese webbtjänster via hello Marketplace. hello tjänster i Marketplace är OData-tjänster och de kan anropas med metoderna GET eller POST. 

**4. Varför är hello stöd länkar för min webbtjänst erbjuder inte uppdateras korrekt för vissa min erbjudanden?**

hello stöd länkar är globala per utgivare, inte per erbjudandet. 

**5. Hur publicera en webbtjänst med indata batchläge i Marketplace?**

inkommande batchläge hello stöds för närvarande inte i Marketplace-webbtjänster.

**6. Vem ska jag kontakta tooget hjälp om jag har frågor om att bli en utgivare, eller om jag har problem vid publicering?**

Kontakta hello Azure Marketplace-teamet på < mailto:datamarketbd@microsoft.com > för mer information.

