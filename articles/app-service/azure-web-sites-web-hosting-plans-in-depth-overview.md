---
title: "aaaAzure App Service-planer djupgående översikt | Microsoft Docs"
description: "Lär dig hur App Service-planer för Azure App Service och hur de får din hanteringsupplevelse."
keywords: app service, azure app service, scale, scalable, app service plan, app service cost
services: app-service
documentationcenter: 
author: btardif
manager: erikre
editor: 
ms.assetid: dea3f41e-cf35-481b-a6bc-33d7fc9d01b1
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: byvinyal
ms.openlocfilehash: b384790d9e69b234ca69ac591164c48a4b6ed210
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-plans-in-depth-overview"></a>Detaljerad översikt över Azure App Service-avtal

Programtjänstplaner representerar hello mängd fysiska resurser som används toohost dina appar.

App Service-planer definierar följande:

- Region (västra USA, östra USA osv.)
- Skalningsantalet (en, två, tre instanser, etc.)
- Storlek (Small, Medium, Large)
- SKU (Kostnadsfri, Delad, Basic, Standard, Premium)

Web Apps, Mobilappar, API Apps, funktionen appar (eller funktioner) i [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) alla kör i en apptjänstplan.  Appar i hello samma prenumeration och region kan dela en App Service-plan. 

Alla program som tilldelats tooan **programtjänstplanen** dela hello resurser som definieras av den. Den här delning sparar pengar när värd för flera appar i en enda App Service-plan.

Din **programtjänstplanen** kan skala från **lediga** och **delade** SKU: er för**grundläggande**, **Standard**, och **Premium** SKU: er som ger dig åtkomst till toomore resurser och funktioner längs hello sätt.

Om din App Service-plan anges för**grundläggande** SKU eller senare, och du kan styra hello **storlek** och skala antalet hello virtuella datorer.

Om din plan är konfigurerade toouse två ”liten” instanser i hello standard-tjänstnivå, till exempel köra alla appar som är kopplade med planen på båda instanser. Appar har också åtkomst toohello standard service tier funktioner. Instanser av programtjänstplanen som kör appar är helt hanterad och hög tillgänglighet.

> [!IMPORTANT]
> Hej **SKU** och **skala** av hello App Service plan avgör hello kostnad och inte hello många appar i den.

Den här artikeln innehåller hello viktiga egenskaper som till exempel nivå och skalan för en apptjänstplan och hur de komma till användning vid hantering av dina appar.

## <a name="apps-and-app-service-plans"></a>Appar och App Service-planer

En app i App Service kan associeras med endast en App Service-plan vid en given tidpunkt.

Både appar och planer finns i en **resursgruppen**. En resursgrupp fungerar som hello livscykel gräns för varje resurs som ligger inom det. Du kan använda resursen grupper toomanage alla hello delar i ett program tillsammans.

Eftersom en enskild resursgrupp kan ha flera programtjänstplaner, kan du allokera olika appar toodifferent fysiska resurser.

Du kan till exempel separata resurser mellan dev, test- och miljöer. Med separata miljöer för produktion och utveckling och testning kan du isolera resurser. På så sätt kan läsa in testar mot en ny version av dina appar inte konkurrerar för hello samma resurser som dina appar för produktion som hanterar verkliga kunder.

När du har flera planer i en enskild resursgrupp kan du också definiera ett program som omfattar geografiska regioner.

En hög tillgänglighet app som körs i två områden innehåller till exempel minst två planer, en för varje region och en app som är associerade med varje plan. I en sådan situation finns sedan alla hello kopior av hello app i en enskild resursgrupp. En resursgrupp med flera planer och flera appar blir det enkelt toomanage-, kontroll- och visa hello hälsotillståndet för programmet hello.

## <a name="create-an-app-service-plan-or-use-existing-one"></a>Skapa en apptjänstplan eller använda befintlig

När du skapar en app, bör du överväga att skapa en resursgrupp. Hej däremot på om den här appen är en komponent för ett större program, skapa inom hello resursgrupp som har allokerats för större programmet.

Om hello appen är ett helt nytt program eller en del av en större, kan du välja toouse en befintlig plan toohost den eller skapa en ny. Det här beslutet är mer en fråga av kapacitet och belastning.

Vi rekommenderar att isolera din app till en ny Apptjänst planerar:

- Appen är resurskrävande.
- Programmet har olika skalning faktorer från hello andra appar som finns i ett befintligt schema.
- Appen måste resurs i en annan geografisk region.

Det här sättet du tilldela en ny uppsättning resurser för din app och få större kontroll över dina appar.

## <a name="create-an-app-service-plan"></a>Skapa en App Service-plan

> [!TIP]
> Om du har en Apptjänst-miljö kan du granska hello dokumentationen specifika tooApp miljöer här: [skapa en apptjänstplan i en Apptjänst-miljö](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md#createplan)

Du kan skapa en tom apptjänstplan från hello navigeringsmiljö för App Service-plan eller som en del av skapa en app.

I hello [Azure-portalen](https://portal.azure.com), klickar du på **ny** > **webb + mobilt**, och välj sedan **Web App** eller en annan typ för Apptjänst-app.

![Skapa en app i hello Azure-portalen.][createWebApp]

Du kan sedan välja eller skapa hello App Service-plan för hello nya app.

 ![Skapa en apptjänstplan.][createASP]

toocreate en apptjänstplan klickar du på **[+] Skapa nya**, typen hello **programtjänstplanen** namn och välj sedan ett lämpligt **plats**. Klicka på **prisnivå**, och välj sedan ett lämpligt prisnivån för hello-tjänsten. Välj **visa alla** tooview priser fler alternativ, exempelvis **lediga** och **delade**. När du har valt hello prisnivå, klickar du på hello **Välj** knappen.

## <a name="move-an-app-tooa-different-app-service-plan"></a>Flytta en app tooa annan programtjänstplan

Du kan flytta en app tooa olika apptjänstplan i hello [Azure-portalen](https://portal.azure.com). Du kan flytta appar mellan planer så länge hello-planer finns i hello samma resursgrupp och geografiska region.

toomove en app tooanother plan:

- Navigera toohello app som du vill toomove.
- I hello **menyn**, leta efter hello **Apptjänstplan** avsnitt.
- Välj **ändra programtjänstplanen** toostart hello processen.

**Ändra programtjänstplanen** öppnas hello **programtjänstplanen** väljare. Nu kan du välja en befintlig plan toomove den här appen till.

> [!IMPORTANT]
> hello väljer programtjänstplanen UI filtreras efter hello följande kriterier:
> - Finns i hello samma resursgrupp
> - Det finns i hello samma geografiska Region
> - Finns i hello samma Webbutrymmet
>
> En Webbutrymmet är en logisk konstruktion i App Service som definierar en gruppering av serverresurser. En geografisk region (till exempel USA, västra) innehåller många Webspaces i ordning tooallocate kunder använder App Service. Apptjänst resurser är för närvarande inte kan toobe flyttas mellan Webspaces.
>

![App Service-plan väljare.][change]

Varje plan har sin egen prisnivån. Flytta en plats från en kostnadsfri nivå tooa standardnivån kan till exempel alla appar som har tilldelats tooit toouse hello funktioner och resurser av hello standardnivån.

## <a name="clone-an-app-tooa-different-app-service-plan"></a>Klona en app tooa annan programtjänstplan

Om du vill toomove hello app tooa annan region är ett alternativ app kloning. Kloningen gör en kopia av din app i en ny eller befintlig programtjänstplan i en region.

Du kan hitta **klona App** i hello **utvecklingsverktyg** hello-menyn.

> [!IMPORTANT]
> Kloningen har vissa begränsningar som du kan läsa om vid [Azure Apptjänst-App kloning med hjälp av Azure portal](../app-service-web/app-service-web-app-cloning-portal.md).

## <a name="scale-an-app-service-plan"></a>Skala en programtjänstplan

Det finns tre sätt tooscale en plan:

- **Ändra hello plan prisnivån**. En plan i hello grundläggande nivån kan vara konverterade tooStandard och alla appar tilldelad tooit toouse hello funktioner i hello standardnivån.
- **Ändra storlek på hello plan**. Exempelvis kan en plan i hello grundläggande nivån som använder små instanser vara ändrade toouse stora instanser. Alla appar som är kopplade med planen kan använda hello ytterligare minne och processorresurser som hello större instans storlek erbjudanden.
- **Ändra hello plan instansantalet**. En Standard-plan som utskalad toothree instanser kan till exempel vara skalade too10 instanser. En premiumplan kan skaländras ut too20 instanser (ämne tooavailability). Alla appar som är kopplade med planen kan använda hello ytterligare minne och processorresurser som hello större instans antal erbjudanden.

Du kan ändra hello priser nivå och instans storlek genom att klicka på hello **skala upp** objekt under inställningar för hello app eller hello App Service-plan. Ändringarna tillämpas toohello App Service-plan och påverkar alla appar som den är värd för.

 ![Ange värden tooscale upp en app.][pricingtier]

## <a name="app-service-plan-cleanup"></a>Rensning av App Service-plan

> [!IMPORTANT]
> **Apptjänstplaner** som har inga appar som är associerade toothem fortfarande avgifter eftersom de fortsätta tooreserve hello beräkningskapacitet.

tooavoid oväntat kostnader, när du tar bort hello senaste app finns i en apptjänstplan hello resulterande tom App Service-plan tas också bort.

## <a name="summary"></a>Sammanfattning

Programtjänstplaner representerar en uppsättning funktioner och kapaciteter som du kan dela över dina appar. Apptjänstplaner ger du hello flexibilitet tooallocate specifika appar tooa uppsättning resurser och ytterligare optimera din Azure resursutnyttjande. På så sätt kan om du vill toosave pengar i din testmiljö kan du dela en plan över flera appar. Du kan också maximera dataflödet för produktionsmiljön genom att skala över flera regioner och planer.

## <a name="whats-changed"></a>Nyheter

- En guide toohello ändring från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)

[pricingtier]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/appserviceplan-pricingtier.png
[assign]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/assing-appserviceplan.png
[change]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/change-appserviceplan.png
[createASP]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-appserviceplan.png
[createWebApp]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-web-app.png
