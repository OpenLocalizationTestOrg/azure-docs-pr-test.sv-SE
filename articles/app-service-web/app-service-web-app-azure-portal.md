---
title: "Referens för navigering Azure-portalen"
description: "Läs olika användarupplevelser för App Service Web mellan management-portalen och Azure Portal"
services: app-service
documentationcenter: 
author: jaime-espinosa
manager: erikre
editor: jimbe
ms.assetid: 0cc6a3cc-bd89-4a96-9177-d25f6fb737bb
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2016
ms.author: jaime-espinosa
ms.openlocfilehash: d1ef6e87d82df0840e49412154df40cc937b320c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="reference-for-navigating-the-azure-portal"></a>Referens för navigering Azure-portalen
Azure Websites kallas [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714). Vi uppdaterar alla av vår dokumentation till namnet ändringen och att ge instruktioner åt Azure-portalen. Förrän den här processen är klar, kan du använda det här dokumentet som en vägledning för för att arbeta med Web Apps i Azure-portalen.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="the-future-of-the-azure-classic-portal"></a>Framtiden för den klassiska Azure-portalen
När du ser företagsanpassning ändringarna på den klassiska Azure-portalen håller som portalanvändare på att ersättas av Azure-portalen. Eftersom den klassiska portalen kommer att överges, rörliga fokus för nyutveckling till Azure-portalen. Alla kommande nya funktioner för Web Apps kommer i Azure Portal. Börja använda Azure Portal för att dra nytta av de senaste och mest att Web Apps har att erbjuda.

## <a name="layout-differences-between-the-azure-classic-portal-and-azure-portal"></a>Layout skillnader mellan den klassiska Azure-portalen och Azure Portal
I den klassiska portalen visas alla Azure-tjänster på den vänstra sidan. Navigering i den klassiska portalen följer trädstrukturer, där du startar från tjänsten och navigera till varje element. Den här strukturen fungerar bra när du hanterar oberoende komponenter. Men program som bygger på Azure är en samling sammankopplade tjänster och den här trädstrukturen är inte idealiskt för att arbeta med samlingar av tjänster. 

Azure portal gör det enkelt att skapa program slutpunkt till slutpunkt med komponenter från flera tjänster. Portalen har ordnats som *resor*. En *resa* är en serie *blad*, som är behållare för de olika komponenterna. Exempelvis ställa in automatisk skalning för ett webbprogram är en *resa* som tar du flera blad som visas i följande exempel: den **webbplats** bladet (att rubrik inte har ännu har uppdaterats för att använda den nya den här artikeln) den **inställningar** bladet och **skala ut** bladet. I det här exemplet Autoskala ställs in beroende CPU-användning, så det finns också en **CPU-procent** bladet. Komponenterna i den *blad* kallas *delar*, som ser ut som paneler. 

![](./media/app-service-web-app-azure-portal/AutoScaling.png)

## <a name="navigation-example-create-a-web-app"></a>Navigering exempel: skapa en webbapp
Skapa nya webbappar är fortfarande lika enkelt som 1-2-3. Följande bild visar den klassiska portalen och portal sida vid sida att demonstrera att inte mycket har ändrats i antalet steg som behövs för att komma ett webbprogram och köra. 

![](./media/app-service-web-app-azure-portal/CreateWebApp.png)

Du kan välja från de vanligaste typerna av webbprogram, inklusive Galleri för populära program som WordPress i portalen. En fullständig lista över tillgängliga program finns i [Azure Marketplace].

När du skapar ett webbprogram kan du ange URL: en App Service-plan och plats på samma sätt som i den klassiska portalen-portalen. 

![](./media/app-service-web-app-azure-portal/CreateWebAppSettings.png)

Dessutom kan portalen du definiera andra vanliga inställningar. Till exempel [resursgrupper](../azure-resource-manager/resource-group-overview.md) gör det enkelt att se och hantera relaterade Azure-resurser. 

## <a name="navigation-example-settings-and-features"></a>Navigering exempel: inställningar och funktioner
Alla inställningar och funktioner är nu logiskt grupperade i ett enda blad som du kan navigera.

![](./media/app-service-web-app-azure-portal/WebAppSettings.png)

Du kan till exempel skapa anpassade domäner genom att klicka på **anpassade domäner och SSL** i den **inställningar** bladet.

![](./media/app-service-web-app-azure-portal/ConfigureWebApp.png)

Klicka på Konfigurera en övervakningsavisering **begäranden och fel** och sedan **Lägg till avisering**.

![](./media/app-service-web-app-azure-portal/Monitoring.png)

Aktivera diagnostik, klicka på **diagnostik loggar** i den **inställningar** bladet.

![](./media/app-service-web-app-azure-portal/Diagnostics.png)

Om du vill konfigurera programinställningar, klickar du på **programinställningar** i den **inställningar** bladet. 

![](./media/app-service-web-app-azure-portal/AppSettingsPreview.png)

Några saker i portalen har bytt namn eller grupperas på olika sätt för att göra det enklare att hitta dem än varumärke. Till exempel nedan visas en skärmbild av sidan motsvarande för app-inställningar (**konfigurera**) i den klassiska portalen.

![](./media/app-service-web-app-azure-portal/AppSettings.png)

## <a name="more-resources"></a>Fler resurser
[Azure Portal]: https://portal.azure.com
[Azure Marketplace]: /marketplace/

> [!NOTE]
> Om du vill komma igång med Azure Apptjänst innan du registrerar dig för ett Azure-konto kan du gå till [Prova Apptjänst](https://azure.microsoft.com/try/app-service/). Där kan du direkt skapa en tillfällig startwebbapp i Apptjänst. Inget kreditkort krävs, och du gör inga åtaganden.
> 
> 

## <a name="whats-changed"></a>Nyheter
* En guide till övergången från Webbplatser till App Service finns i: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)

