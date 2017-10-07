---
title: aaaDeploy Azure API Management services toomultiple Azure regioner | Microsoft Docs
description: "Lär dig hur toodeploy ett Azure API Management-tjänsten instans toomultiple Azure regioner."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 47389ad6-f865-4706-833f-846115e22e4d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 04a3e762261237d73a769320a21363f99f1d20cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodeploy-an-azure-api-management-service-instance-toomultiple-azure-regions"></a>Hur toodeploy ett Azure API Management-tjänsten instans toomultiple Azure regioner
API-hantering stöder distribution av flera regioner där API utgivare toodistribute ett enda API-tjänsten till alla önskade Azure-regioner. Detta minskar begäran latens uppfattas av geografiskt distribuerat API-konsumenter och förbättrar även tjänsttillgängligheten om en region tas offline. 

När en API Management-tjänst skapas från början, innehåller endast en [enhet] [ unit] och finns i en enda Azure-region, som är tilldelad som hello primär Region. Ytterligare regioner kan enkelt lagts till via hello Azure-portalen. En API Management gateway-servern är distribuerad tooeach region och trafik call blir routade toohello närmaste gateway. Om en region frånkopplas är hello trafik automatiskt igen dirigerad toohello nästa närmaste gateway. 

> [!IMPORTANT]
> Distribution av flera regioner är endast tillgängligt i hello  **[Premium] [ Premium]**  nivå.
> 
> 

## <a name="add-region"></a>Distribuera en API Management service-instans tooa nya region
> [!NOTE]
> Om du inte har skapat en instans för API Management-tjänsten finns [skapa en instans för API Management-tjänsten] [ Create an API Management service instance] i hello [Kom igång med Azure API Management] [ Get started with Azure API Management] kursen.
> 
> 

Navigera i hello Azure Portal toohello **skala och prissättning** för din API Management service-instans. 

![Skala][api-management-scale-service]

toodeploy tooa ny region, klicka på **+ Lägg till region** hello-verktygsfältet.

![Lägg till region][api-management-add-region]

Välj hello plats från hello listrutan och ange hello antal enheter för med hello skjutreglaget.

![Ange enheter][api-management-select-location-units]

Klicka på **Lägg till** tooplace ditt val i tabellen för hello-platser. 

Upprepa den här processen tills du har alla platser som har konfigurerats och klicka på **spara** från hello verktygsfältet toostart hello distributionsprocessen.

## <a name="remove-region"></a>Ta bort en instans för API Management-tjänsten från en plats
Navigera i hello Azure Portal toohello **skala och prissättning** för din API Management service-instans. 

![Skala][api-management-scale-service]

Hello-plats som öppnar tooremove hello snabbmenyn med hello **...**  längst hello högra ände hello tabell. Välj hello **ta bort** alternativet.

![Ta bort region][api-management-remove-region]

Bekräfta borttagning av hello och klicka på **spara** tooapply hello ändringar.

[api-management-management-console]: ./media/api-management-howto-deploy-multi-region/api-management-management-console.png

[api-management-scale-service]: ./media/api-management-howto-deploy-multi-region/api-management-scale-service.png
[api-management-add-region]: ./media/api-management-howto-deploy-multi-region/api-management-add-region.png
[api-management-select-location-units]: ./media/api-management-howto-deploy-multi-region/api-management-select-location-units.png
[api-management-remove-region]: ./media/api-management-howto-deploy-multi-region/api-management-remove-region.png

[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Get started with Azure API Management]: api-management-get-started.md

[Deploy an API Management service instance tooa new region]: #add-region
[Delete an API Management service instance from a region]: #remove-region

[unit]: http://azure.microsoft.com/pricing/details/api-management/
[Premium]: http://azure.microsoft.com/pricing/details/api-management/

