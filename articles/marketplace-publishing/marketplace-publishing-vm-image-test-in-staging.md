---
title: "den virtuella datorn erbjuder för hello Marketplace aaaTest | Microsoft Docs"
description: "Förstå hur tootest din virtuella dator bild för hello Azure Marketplace."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 7a41c3c6-625c-4478-b804-e124dee89040
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2016
ms.author: hascipio
ms.openlocfilehash: ab166d2c3c536810a3a8f48330f0482b9b4e58d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="test-your-vm-offer-for-hello-azure-marketplace-in-staging"></a>Testa din VM-erbjudande för hello Azure Marketplace under mellanlagring
Mellanlagring betyder att distribuera din SKU i ett privat ”sandbox” där du kan testa och validera dess funktioner innan du distribuerar den toohello Marketplace. hello SKU visas i Förproduktion precis som tooa kund som har distribuerat den. VM-avbildning måste vara certifierad toobe pushas toostaging.

## <a name="step-1-push-your-offer-toostaging"></a>Steg 1: Push erbjudande-toostaging
1. På hello **publicera** klickar du på **Push tooStaging**.
   
    ![Rita](media/marketplace-publishing-vm-image-test-in-staging/vm-image-push-to-staging.png)
2. Om hello Publishing Portal meddelar dig om eventuella fel, rätta dem.
3. I hello **vem som kan komma åt erbjudandet mellanlagrade?** dialogrutan Ange hello lista över Azure-prenumerationer som du ska använda toopreview erbjudandet i hello [Azure preview portal](https://portal.azure.com).
   
   > [!NOTE]
   > Om virtuella datorer och lösningsmallar du **inte** godkända prenumerationer av typen CSP DreamSpark eller Azure i Open.
   > 
   > 

    > Om virtuella datorer, när du klickar på knappen hello **PUSH tooSTAGING**, hello följande steg utförs bakom hello scen. Du kommer att kunna tooview hello förloppet för varje steg i hello hello publicera fliken publicering av portalen. Du måste kontrollera den här sidan med regelbundna intervall (tills hello status visar MELLANLAGRAD) alla felinformation som behöver korrigering från din slutpunkt.

    > - Din fristående begäran går toohello certifikatutfärdare team som Validera hello vhd först. Men om din begäran har gjort endast marknadsföring ändring, hoppas hello certifikatutfärdare steget över.
    > - När hello certifikatutfärdare är klar hello replikering av hello erbjudande start för alla Azure-datacenter. Normalt tar 24 48hours för hello replikering toocomplete men kan ta upp tooa vecka beroende på hello storleken på hello vhd. Om din begäran har stött på marknadsföring bara ändra, sedan är hello replikering dock snabbare.
    > - När hello replikeringen är klar, sedan hello erbjudandet ska finnas i hello [Azure-portalen](http:/portal.azure.com). Vid den tiden hello status blir MELLANLAGRAD i hello publicering av portalen. En mellanlagrad erbjudandet är synliga i hello [Azure-portalen](http:/portal.azure.com) med enbart hello e-ID som associeras med hello med vilka hello erbjudande mellanlagras.

1. Logga in toohello [Azure preview portal](https://portal.azure.com) genom att använda en hello Azure-prenumerationer som anges i hello föregående steg.
2. Hitta erbjudandet och verifiera din VM avbildningen punkter:
   
   * Kontrollera att marknadsföring innehåll visas korrekt i hello Marketplace.
   * Slutpunkt till slutpunkt för distributionen av hello VM-avbildning.
     
      ![img kartan portal](media/marketplace-publishing-push-to-staging/pubportal-mapping-azure-portal.jpg)

> [!IMPORTANT]
> Erbjudandet finns kvar i Förproduktion tills du meddela Microsoft via hello Publishing Portal [**publicera** fliken > Klicka på hello **”begär godkännande tooPush tooProduction”**] att du är klar toopush tooproduction. Det här är en perfekt tid toohave alla medlemmar i gruppen kontrollera över allt inför erbjudandet ska visas.
> 
> hello mellanlagring plattform är avsedd för testning hello erbjudandet i ett förhandsgranskningsläge för av hello utgivare. Vi avråder med hjälp av den här platofrm för mobilkommunikationssystemet ändamål.
> 
> 

## <a name="next-steps"></a>Nästa steg
Nu när erbjudandet ”mellanlagras” och du har testat dess funktioner och marknadsföring innehåll, kan du fortsätta toohello slutliga publicering fasen **steg 4**: [distribuera ditt erbjudande toohello Marketplace](marketplace-publishing-push-to-production.md).

## <a name="see-also"></a>Se även
* [Komma igång: hur toopublish ett erbjudande toohello Azure Marketplace](marketplace-publishing-getting-started.md)

