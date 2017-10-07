---
title: "aaaTesting Data Service-erbjudande för hello Marketplace | Microsoft Docs"
description: "Förstå hur tootest Data Service-erbjudande för hello Azure Marketplace."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: e861bd11-f74d-4d77-b4b5-23fb463644ad
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: 1b9c7027d8e0818b9bdee5cfca971bab25dd1959
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="testing-your-data-service-offer-in-staging"></a>Testa din Data tjänsterbjudande i Förproduktion
> [!IMPORTANT]
> **Just nu vi inte längre onboarding några nya Data Service-utgivare. Nya dataservices kommer inte godkännas för lista.** Om du har ett SaaS-affärsprogram som toopublish på AppSource hittar du mer information [här](https://appsource.microsoft.com/partners). Om du har en IaaS-program eller utvecklare tjänst du skulle t.ex toopublish på Azure Marketplace hittar du mer information [här](https://azure.microsoft.com/marketplace/programs/certified/).
> 
> 

När du har slutfört hello två första stegen av [skapar Microsoft Developer-konto](marketplace-publishing-accounts-creation-registration.md) och [skapar ditt Data Service erbjuder i Publishing Portal](marketplace-publishing-data-service-creation.md) är du redo för att göra erbjudandet tillgängliga i hello Azure Marketplace. Det här avsnittet kommer vägleder dig genom hello först, mellanliggande, steg kallas ”mellanlagring”

Mellanlagring sätt distribuera erbjudandet i ett privat ”sandbox” där du kan testa och verifiera dess funktioner innan du skickar den tooproduction. hello erbjudandet visas i Förproduktion precis som tooa kund som har distribuerat den.

## <a name="step-1-pushing-your-offer-toostaging"></a>Steg 1. Push-överföring erbjudande-toostaging
Push-överföring erbjudande-toostaging kan du tootest hello erbjudande innan den blir tillgänglig toofuture prenumeranter.  Du kan se hur erbjudandet visas och fungerar för de prenumerera tooyour data.  

  ![Rita](media/marketplace-publishing-data-service-test-in-staging/step-1.1.png)

1. Logga in på hello [Publishing Portal](https://publish.windowsazure.com)
2. Välj **datatjänster** i hello vänstra navigeringsfönstret fönster
3. Välj önskade toopush toostaging erbjudandet. Hej ovanför skärmen visas.
4. Klicka på **Push tooStaging** knappen.  
5. Om det finns problem med hello erbjudande som behövs toobe slutfört tidigare toopushing toostaging, visas en lista som visas.  Korrigera objekten genom att klicka på varje objekt i listan hello. När alla ändringar som gjorts, klickar på **Push tooStaging** igen.

Om det inte finns några problem med erbjudandet visas hello popup-fönstret nedan.  

Om du inte planerar ej godkända toosurface erbjudandet i Azure-portalen (för närvarande har begränsad kapacitet), och sedan bara Stäng hello popup-fönster.

tootest dina Data i Azure Portal-tjänsten (i tillägg toohello DataMarket portal), behöver du ett Azure-prenumerationens ID tootest med.  Prenumerations-ID identifierar hello-konto som ska vara tillåtna tootest erbjudandet.  

Klipp ut och klistra in ditt prenumerations-ID och klicka på hello markering toocontinue.

  ![Rita](media/marketplace-publishing-data-service-test-in-staging/step-1.2.png)

> [!NOTE]
> Dessa ID: N för Azure-prenumerationer är bara krävs för att testa och mellanlagring i hello [Azure-hanteringsportalen](https://manage.windowsazure.com). De är inte obligatoriska tootest i Azure Marketplace.
> 
> 

hello nästa skärm visas visar att publicering sker genom att visa hello ”pågående” ikonen markerat gul nedan. Push-överföring toostaging tar mellan 10 too15 minuter.  Om det tar längre tid först uppdatera webbläsaren (tryck på F5 i IE).  Klicka på hello länken Kontakta oss toolet oss om att det finns ett problem i hello sällsynta fall där erbjudandet är fortfarande pushar toostaging efter en timme.

  ![Rita](media/marketplace-publishing-data-service-test-in-staging/step-1.3.png)

När hello Push tooStaging slutförs hello ”pågående” ikonen slutar att flytta och hello status uppdateras för ”mellanlagrad”.  Du är nu redo tootest erbjudandet.  

## <a name="step-2-test-your-staged-offer-in-datamarket"></a>Steg 2. Testa mellanlagrade erbjudandet i DataMarket
Klicka på länken för hello efter hello texten **”se din tjänst som erbjuder på...”** toodisplay hello-skärmen som hello prenumeranten kommer att se när erbjudandet går tooproduction och visas i DataMarket.

  ![Rita](media/marketplace-publishing-data-service-test-in-staging/step-2.2.png)

Testa eller kontrollera var och en av hello 12 objekt markeras ovan tooensure alla logotyper, priser-transaktioner, text, bilder, dokumentation och länkar är korrekta och fungerar korrekt.  Det här är ett bra tillfälle tooensure alla testvärden som du angav när du skapar din prenumeration har ersatts med faktiska värden.

1. Logotyp för erbjudande
2. Erbjudandenamn
3. Utgivarens namn/länk tooyour företags webbplats
4. Sök efter kategorier för ditt erbjudande
5. Ditt erbjudande stöd länken tooassist prenumeranter
6. Kontextuella beskrivning för ditt erbjudande
7. Erbjudande plan illustrerar faktureringsinformation
8. Tooimplementation kod
9. Exempel bilder som illustrerar användning av erbjudandet data
10. I/o-metadata för varje tjänst inom hello erbjudande
11. Erbjudandet, villkor för användning
12. Förhandsgranskning av hello erbjudande data

Kontrollera slutligen hello-tjänsten fungerar via hello Datamarket genom att klicka på hello länken ”Utforska den här DATAUPPSÄTTNINGEN”.  Öppnar ett nytt fönster i hello verktyget vi kallar ”Service Explorer” så att du kan förhandsgranska hello resultatet av en fråga mot din tjänst.  I det här fönstret kan ange du hello parametrar som behövs och se hello resultat från en fråga till tjänsten visas.   Dessutom är visas hello URL för din fråga.  

> [!NOTE]
> Vara säker på att tooreview hello textbeskrivning av hello tjänsten visas överst hello.  Och om erbjudandet består av fler än en tjänst anropa på hello flikar på hello nedre tooswitch toohello nästa service tooreview och testa.
> 
> 

## <a name="next-step"></a>Nästa steg
Om du har problem och behöver hjälp med att lösa dem. Kontakta [Azure utgivarens Support](http://go.microsoft.com/fwlink/?LinkId=272975).

Om du är nöjd och redo toopublish erbjudandet Läs hello [begäran om godkännande av tooPush tooProduction](marketplace-publishing-push-to-production.md) dokumentation.

## <a name="see-also"></a>Se även
* [Komma igång: Hur toopublish ett erbjudande toohello Azure Marketplace](marketplace-publishing-getting-started.md)

