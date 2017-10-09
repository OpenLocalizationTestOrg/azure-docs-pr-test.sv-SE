---
title: "aaaRestrict Azure CDN innehåll efter land | Microsoft Docs"
description: "Lär dig hur toorestrict tooyour Azure CDN innehåll med hjälp av hello filtrering av Geo-funktionen."
services: cdn
documentationcenter: 
author: lichard
manager: akucer
editor: 
ms.assetid: 12c17cc5-28ee-4b0b-ba22-2266be2e786a
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: rli
ms.openlocfilehash: ffdd994612b6c9cfbf1a6e29d260709b4afa86e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="restrict-azure-cdn-content-by-country"></a>Begränsa Azure CDN innehåll efter land

## <a name="overview"></a>Översikt
När en användare begär innehåll, som standard, hanteras hello innehållet oavsett om hello användare gjort denna begäran från. I vissa fall kan kanske du vill toorestrict åtkomst till tooyour innehåll efter land. Det här avsnittet beskrivs hur toouse hello **Geo-filtrering** funktion i ordning tooconfigure hello service tooallow eller blockera åtkomst av land.

> [!IMPORTANT]
> hello Verizon och Akamai produkter har hello samma funktioner för filtrering av geo men har en liten skillnad i te landskoder som de stöder. Se steg3 för en länk toohello skillnader.


Den här typen av begränsning finns information om saker som gäller tooconfiguring hello [överväganden](cdn-restrict-access-by-country.md#considerations) avsnittet hello slutet av hello-avsnittet.  

![Landsfiltrering](./media/cdn-filtering/cdn-country-filtering-akamai.png)

## <a name="step-1-define-hello-directory-path"></a>Steg 1: Definiera hello sökväg
Välj din slutpunkt i hello-portalen och hitta hello Geo-filtrering fliken på hello vänstra navigeringsfönstret toofind den här funktionen.

När du konfigurerar land-filtret, måste du ange hello relativ sökväg toohello plats toowhich användare ska tillåtas eller nekas åtkomst. Du kan använda geo-filtrering för alla filer med ”/” eller valda mappar genom att ange katalogsökvägar ”/ bilder /”. Du kan också använda filtrering av geo tooa en fil genom att ange hello-fil och lämnar hello avslutande snedstreck ”/ pictures/city.png”.

Exempel directory sökvägen filter:

    /                                 
    /Photos/
    /Photos/Strasbourg/
      /Photos/Strasbourg/city.png

## <a name="step-2-define-hello-action-block-or-allow"></a>Steg 2: Definiera hello åtgärd: blockera eller tillåta
**Blockering:** användare från hello angivna länder kommer att nekas åtkomst tooassets begärs från den rekursiva sökvägen. Om inga andra land filtreringsalternativ har konfigurerats för den platsen, sedan får alla andra användare åtkomst.

**Tillåt:** endast användare från hello angivna länder tillåts åtkomst tooassets begärs från den rekursiva sökvägen.

## <a name="step-3-define-hello-countries"></a>Steg 3: Definiera hello länder
Välj hello länder du vill tooblock eller tillåta hello sökväg. 

Till exempel filtrerar hello regeln för blockeringen /Photos/Strasbourg/filer, inklusive:

    http://<endpoint>.azureedge.net/Photos/Strasbourg/1000.jpg
    http://<endpoint>.azureedge.net/Photos/Strasbourg/Cathedral/1000.jpg


### <a name="country-codes"></a>Landskoder
Hej **Geo-filtrering** funktionen använder land koder toodefine hello länder där en begäran ska tillåtas eller blockeras för en skyddad katalog. Du hittar hello landskoder i [Azure CDN landskoder](https://msdn.microsoft.com/library/mt761717.aspx). 

## <a id="considerations"></a>Överväganden
* Det kan ta upp too90 minuter för Verizon, eller ett par minuter med Akamai, för ändringar tooyour land filtrering configuration tootake effekt.
* Den här funktionen har inte stöd för jokertecken (till exempel ' *').
* hello geo-filtrering konfiguration som är associerad med hello relativa sökvägen kommer att tillämpas rekursivt toothat sökväg.
* Endast en regel kan vara tillämpade toohello samma relativa sökväg (du kan inte skapa flera land filter som punkt toohello samma relativa sökväg. En mapp kan dock ha flera land filter. Detta är på grund av toohello rekursiv uppbyggnad land filter. Med andra ord en undermapp till en tidigare konfigurerade mapp som kan tilldelas ett annat land filter.

