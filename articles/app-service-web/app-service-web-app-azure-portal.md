---
title: "aaaReference för navigering hello Azure-portalen"
description: "Läs hello olika användarupplevelser för App Service Web mellan hello-hanteringsportalen och hello Azure-portalen"
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
ms.openlocfilehash: dcf7c1fc17f9a0c31005ad0f2fd53723d2966058
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="reference-for-navigating-hello-azure-portal"></a>Referens för navigering hello Azure-portalen
Azure Websites kallas [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714). Vi uppdaterar alla vår dokumentation tooreflect det här namnet för ändrings- och tooprovide instruktioner för hello Azure-portalen. Förrän den här processen är klar kan du använda det här dokumentet som en vägledning för att arbeta med Webbappar i hello Azure-portalen.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="hello-future-of-hello-azure-classic-portal"></a>hello framtida hello klassiska Azure-portalen
När du ser hello anpassningsändringar på hello klassiska Azure-portalen är den portalen i hello processen att ersättas med hello Azure-portalen. Eftersom hello klassiska portalen kommer att överges, hello fokus för nyutveckling rörliga toohello Azure-portalen. Alla kommande nya funktioner för Web Apps kommer i hello Azure-portalen. Börja använda hello Azure Portal tootake nytta av hello senaste och bästa att Web Apps har toooffer.

## <a name="layout-differences-between-hello-azure-classic-portal-and-azure-portal"></a>Layout skillnader mellan hello klassiska Azure-portalen och Azure Portal
I klassiska portalen hello alla hello Azure tjänsterna hello vänster. Navigering i hello klassiska portalen följer trädstrukturer, där du startar från hello-tjänsten och navigera till varje element. Den här strukturen fungerar bra när du hanterar oberoende komponenter. Men program som bygger på Azure är en samling sammankopplade tjänster och den här trädstrukturen är inte idealiskt för att arbeta med samlingar av tjänster. 

hello Azure-portalen gör det enkelt toobuild program slutpunkt till slutpunkt med komponenter från flera tjänster. hello-portalen har ordnats som *resor*. En *resa* är en serie *blad*, som är behållare för hello olika komponenter. Exempelvis ställa in automatisk skalning för ett webbprogram är en *resa* som tar du flera blad som visas i följande exempel hello: hello **webbplats** bladet (att rubrik ännu inte har uppdaterats toouse hello nya terminologi), hello **inställningar** bladet och hello **skala ut** bladet. I exemplet hello Autoskala ställs in toodepend på CPU-användning, så det finns också en **CPU-procent** bladet. Hej komponenter i hello *blad* kallas *delar*, som ser ut som paneler. 

![](./media/app-service-web-app-azure-portal/AutoScaling.png)

## <a name="navigation-example-create-a-web-app"></a>Navigering exempel: skapa en webbapp
Skapa nya webbappar är fortfarande lika enkelt som 1-2-3. hello följande bild visar hello klassiska portalen och hello portal sida-vid-sida toodemonstrate som inte mycket har ändrats i hello antalet steg som behövs tooget en webbapp in och körs. 

![](./media/app-service-web-app-azure-portal/CreateWebApp.png)

Du kan välja från hello de vanligaste typerna av webbprogram, inklusive Galleri för populära program som WordPress hello-portalen. En fullständig lista över tillgängliga program finns hello [Azure Marketplace].

När du skapar ett webbprogram kan du ange URL: en App Service-plan och plats på samma sätt som i hello klassiska portalen hello-portalen. 

![](./media/app-service-web-app-azure-portal/CreateWebAppSettings.png)

Dessutom kan hello-portalen du definiera andra vanliga inställningar. Till exempel [resursgrupper](../azure-resource-manager/resource-group-overview.md) gör det enkelt toosee och hantera relaterade Azure-resurser. 

## <a name="navigation-example-settings-and-features"></a>Navigering exempel: inställningar och funktioner
Alla hello inställningar och funktioner nu är logiskt grupperade i ett enda blad som du kan navigera.

![](./media/app-service-web-app-azure-portal/WebAppSettings.png)

Du kan till exempel skapa anpassade domäner genom att klicka på **anpassade domäner och SSL** i hello **inställningar** bladet.

![](./media/app-service-web-app-azure-portal/ConfigureWebApp.png)

tooset in en övervakningsavisering klickar du på **begäranden och fel** och sedan **Lägg till avisering**.

![](./media/app-service-web-app-azure-portal/Monitoring.png)

tooenable diagnostics, klickar du på **diagnostik loggar** i hello **inställningar** bladet.

![](./media/app-service-web-app-azure-portal/Diagnostics.png)

tooconfigure programinställningar, klickar du på **programinställningar** i hello **inställningar** bladet. 

![](./media/app-service-web-app-azure-portal/AppSettingsPreview.png)

Än hello varumärke, några saker i hello portal ha ändrats eller grupperade annorlunda toomake den enklare toofind dem. Till exempel nedan visas en skärmbild av sidan hello motsvarande app-inställningar (**konfigurera**) i hello klassiska portalen.

![](./media/app-service-web-app-azure-portal/AppSettings.png)

## <a name="more-resources"></a>Fler resurser
[Azure Portal]: https://portal.azure.com
[Azure Marketplace]: /marketplace/

> [!NOTE]
> Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service. Inget kreditkort krävs, och du gör inga åtaganden.
> 
> 

## <a name="whats-changed"></a>Nyheter
* En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)

