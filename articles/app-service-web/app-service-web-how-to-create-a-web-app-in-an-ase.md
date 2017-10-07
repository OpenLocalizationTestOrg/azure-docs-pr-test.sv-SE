---
title: "aaaCreate en webbapp i en Apptjänst-miljö v1"
description: "Lär dig hur toocreate web apps och apptjänstplaner i en Apptjänst-miljö v1"
services: app-service
documentationcenter: 
author: ccompy
manager: stefsch
editor: 
ms.assetid: 983ba055-e9e4-495a-9342-fd3708dcc9ac
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/11/2017
ms.author: ccompy
ms.openlocfilehash: 322ef344517c54247b102fb4920e35645986ef98
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-in-an-app-service-environment-v1"></a>Skapa en webbapp i en Apptjänst-miljö v1

> [!NOTE]
> Den här artikeln handlar om hello Apptjänstmiljö v1.  Det finns en nyare version av hello Apptjänst-miljö som är enklare toouse och körs på mer kraftfulla infrastruktur. Mer om hello nya versionen börjar med hello toolearn [introduktion toohello Apptjänstmiljö](../app-service/app-service-environment/intro.md).
> 

## <a name="overview"></a>Översikt
Den här kursen visar hur toocreate webbappar och apptjänstplaner i en [Apptjänstmiljö v1](app-service-app-service-environment-intro.md) (ASE). 

> [!NOTE]
> Om du vill toolearn hur toocreate en webbapp, men behöver inte toodo den i en Apptjänst-miljö, se [skapa ett .NET-webbprogram](app-service-web-get-started-dotnet.md) eller något av hello relaterade självstudier för andra språk och ramverk.
> 
> 

## <a name="prerequisites"></a>Krav
Den här kursen förutsätter att du har skapat en Apptjänst-miljö. Om du inte har gjort det ännu kan se [skapa en Apptjänst-miljö](app-service-web-how-to-create-an-app-service-environment.md). 

## <a name="create-a-web-app"></a>Skapa en webbapp
1. I hello [Azure Portal](https://portal.azure.com/), klickar du på **Ny > webb + mobilt > Webbapp**. 
   
    ![][1]
2. Välj din prenumeration.  
   
    Om du har flera prenumerationer vara medveten om att toocreate en app i din Apptjänst-miljö måste toouse hello samma prenumeration som du använde när du skapar hello-miljö. 
3. Välj eller skapa en Resursgrupp.
   
    *Resursgrupper* aktivera du toomanage relaterade Azure-resurser som en enhet och är användbara när *rollbaserad åtkomstkontroll* (RBAC) regler för dina appar. Mer information finns i [översikt över Azure Resource Manager][ResourceGroups]. 
4. Välj eller skapa en apptjänstplan.
   
    *Apptjänstplaner* är hanterade uppsättningar av webbprogram.  När du väljer prisnivå är normalt hello priset tillämpade toohello App Service-plan i stället för enskilda toohello-appar. I en ASE du betalar för hello beräkning instanser som allokerats toohello ASE i stället för att du har angett med ASP.  tooscale hello antal instanser av ett webbprogram som du skalar upp hello instanser av din programtjänstplan och det påverkar alla hello web apps i planen.  Vissa funktioner, till exempel plats platser eller VNET-integrering har också antal begränsningar i hello plan.  Mer information finns i [översikt över Azure App Service-planer](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)
   
    Du kan identifiera hello apptjänstplaner i din ASE genom att titta på hello-plats som anges under hello namn.  
   
    ![][5]
   
    Välj den planen om du vill toouse App Service-plan som redan finns i din Apptjänst-miljö. Om du vill toocreate en ny programtjänstplan finns i följande avsnitt i den här självstudien hello [skapa en apptjänstplan i en Apptjänst-miljö](#createplan).
5. Ange hello namn för ditt webbprogram och klicka sedan på **skapa**. 
   
    Om din ASE använder en extern VIP hello-URL för en app i en ASE: [*sitename*]. [ *namn på din Apptjänstmiljö*]. p.azurewebsites.net i stället för [*sitename*]. azurewebsites.net
   
    Om din ASE använder en intern VIP sedan hello URL för en app i den ASE är: [*sitename*]. [ *underdomän som angetts under skapande av ASE*]   
    När du har valt ASP under skapande av ASE hello underdomän uppdatera nedan visas **namn**

## <a name="createplan"></a>Skapa en apptjänstplan
När du skapar en apptjänstplan i en Apptjänst-miljö skiljer worker-alternativ som det finns ingen delad arbetare i en ASE.  hello arbetare som du har toouse är hello de som har tilldelats toohello ASE av hello-administratör.  Det innebär att toocreate en ny plan måste toohave mer arbetare allokerade tooyour ASE-arbetspool än hello Totalt antal instanser för alla dina planer redan i den poolen, worker.  Om du inte har tillräckligt med personer i din ASE worker poolen toocreate din plan, måste toowork med din ASE admin tooget dem lagts till.

En annan skillnad programtjänstplaner hos en Apptjänst-miljö är hello saknas priser val.  När du har en Apptjänst-miljö betalar för beräkningsresurser som används av hello system och har inte lagts till avgifter för hello planer i miljö.  När du skapar en apptjänstplan välja normalt en prisavtal som avgör din fakturering.  En Apptjänst-miljö är i grunden en privat plats där du kan skapa innehåll.  Du betalar för hello-miljön och inte toohost ditt innehåll.

hello visar följande anvisningar hur toocreate en Apptjänst planerar när du skapar en webbapp enligt beskrivningen i föregående hello hello kursen.

1. Klicka på **Skapa nytt** i hello plan användargränssnitt och ange ett namn för din plan precis som vanligt utanför en ASE.
2. Välj hello ASE som du vill toouse från väljaren för platsen.
   
    Eftersom en Apptjänst-miljö är i grunden en privat distributionsplats bör visas under plats. 
   
    ![][2]
   
    Efter valet av en ASE i hello plats väljare uppdaterar hello App Service-plan skapa UI.  hello plats visas nu hello namnet på hello ASE system och hello region i och hello priser plan Väljaren ersätts med en worker poolen väljare.  
   
    ![][3]

### <a name="selecting-a-worker-pool"></a>Att välja en arbetspool
Normalt finns i Azure App Service och utanför en Apptjänst-miljö 3 beräknings-storlekar som är tillgängliga med hello valet av en dedikerad prisplan.  På ett liknande sätt för en ASE kan du definiera too3 grupp av anställda och ange hello beräkning storlek som används för att worker-poolen.  Vad det innebär när innehavare av hello ASE är att i stället för att välja prisavtal med beräkning storlek för din App Service-plan kan du ange något som kallas en *arbetspool*.  

hello worker poolen Användargränssnittet visar hello beräkning storlek för poolen worker under hello namn.  hello tillgängligt antal refererar toohow många compute-instanser som är tillgängliga för användning i poolen.  hello totala pool kan faktiskt har fler förekomster än det antalet men refererar toosimply hur många används inte.  Om du behöver tooadjust beräkna din Apptjänstmiljö tooadd mer resurser finns [konfigurera din Apptjänst-miljö](app-service-web-configure-an-app-service-environment.md).

![][4]

I det här exemplet ser du bara två arbetarpooler. Det beror på att hello ASE administratör tilldelas endast värdar i de två arbetarpooler.  hello skulle tredje visas när det finns virtuella datorer som har allokerats till den.  

## <a name="after-web-app-creation"></a>När webbappen
Det finns några överväganden för att köra webbappar och hantera programtjänstplaner i en ASE som behöver toobe beaktas.  

Som tidigare nämnts hello ägare hello ASE ansvarar för hello storleken på hello system och därför är också ansvarig för att säkerställa att det finns tillräcklig kapacitet toohost hello önskad App Service-planer. Om det finns inga tillgängliga Worker-arbeten kan du inte kommer att kunna toocreate din programtjänstplan.  Detta är SANT tooscaling upp ditt webbprogram.  Om du behöver fler förekomster så har du tooget din Apptjänstmiljö admin tooadd flera arbetare.

När du har skapat din webbapp och programtjänstplanen är det en bra idé tooscale den.  I en ASE måste alltid toohave minst 2 instanser av din App Service-plan tooprovide feltolerans för dina appar.  Skala en Apptjänst är plan i en ASE hello samma som vanligt via hello programtjänstplanen Användargränssnittet.  Mer information om skalning, [hur tooscale en webbapp i en Apptjänst-miljö](app-service-web-scale-a-web-app-in-an-app-service-environment.md)

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspnewwebapp.png
[2]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createasplocation.png
[3]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspselected.png
[4]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspworkerpool.png
[5]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/selectaspinase.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoScale]: http://azure.microsoft.com/documentation/articles/app-service-web-scale-a-web-app-in-an-app-service-environment
[HowtoConfigureASE]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment
[ResourceGroups]: ../azure-resource-manager/resource-group-overview.md
[AzurePowershell]: http://azure.microsoft.com/documentation/articles/powershell-install-configure/
