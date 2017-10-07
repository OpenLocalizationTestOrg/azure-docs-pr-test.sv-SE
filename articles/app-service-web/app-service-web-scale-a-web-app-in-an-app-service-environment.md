---
title: "aaaHow tooScale en App i en Apptjänst-miljö"
description: "Skala en app i en Apptjänst-miljö"
services: app-service
documentationcenter: 
author: ccompy
manager: stefsch
editor: jimbe
ms.assetid: 78eb1e49-4fcd-49e7-b3c7-f1906f0f22e3
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2016
ms.author: ccompy
ms.openlocfilehash: 08916eac056c46bf8cb6edffbf96285317b32062
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scaling-apps-in-an-app-service-environment"></a>Skala appar i en App Service Environment
Det finns vanligtvis tre saker du kan skala i hello Azure App Service:

* priser för planen
* Worker storlek 
* antal instanser.

Det finns inga måste tooselect eller ändra hello som priser plan i en ASE.  Vad gäller funktioner har den redan en Premium prissättning kapaciteten.  

Med avseende tooworker storlekar tilldela Hej ASE administratör hello storleken på hello beräkning resurs toobe används för varje worker-pool.  Det innebär du kan ha Worker Pool 1 med P4 beräkningsresurser och Worker Pool 2 med P1 beräkningsresurser om så önskas.  De har inte toobe i storlek ordning.  Mer information kring hello storlekar och deras prisnivå finns hello dokumentet här [priser för Azure Apptjänst][AppServicePricing].  Detta lämnar hello skalningsalternativ för webbprogram och Apptjänstplaner i en Apptjänst-miljö toobe:

* val av Worker-pool
* antal instanser

Ändra antingen objektet görs via hello lämpliga gränssnitt som visas för dina ASE finns App Service-planer.  

![][1]

Du kan skala upp ASP utöver hello antalet tillgängliga beräkningsresurser i hello arbetspool som ASP finns i.  Om du behöver beräkningsresurser i den poolen, worker måste tooget din ASE administratör tooadd dem.  För information om hur du konfigurerar din ASE läsa hello information här: [hur tooConfigure en Apptjänst-miljö][HowtoConfigureASE].  Du kan också tootake nytta av hello ASE Autoskala funktioner tooadd kapacitet baserat på schema eller mått.  tooget finns mer information om hur du konfigurerar Autoskala för hello ASE miljö själva [hur tooconfigure Autoskala för en Apptjänstmiljö][ASEAutoscale].

Du kan skapa flera app service-planer med beräkningsresurser från olika arbetarpooler eller du kan använda hello samma arbetspool.  För exempel om du har (10) tillgängliga beräkningsresurser i arbetsprocessen Pool 1, kan du toocreate en apptjänstplan med (6) beräkningsresurser och en andra app service-plan som används (4) beräkningsresurser.

### <a name="scaling-hello-number-of-instances"></a>Skalning hello antal instanser
När du först skapar ditt webbprogram i en Apptjänst-miljö startar med 1-instans.  Därefter kan du skala ut tooadditional instanser tooprovide ytterligare beräkningsresurser för din app.   

Om din ASE har tillräckligt med kapacitet är detta ganska enkelt.  Du kan gå tooyour App Service-Plan som innehåller hello platser tooscale upp och markera skala.  Då öppnas hello användargränssnitt där du kan manuellt ange hello skala för din ASP eller konfigurera automatiska regler för ASP.  toomanually skala ditt program bara ange ***skala genom*** för***instansantal som anges manuellt***.  Härifrån dra hello skjutreglaget toohello önskad kvantitet eller ange i hello rutan nästa toohello skjutreglaget.  

![][2] 

hello automatiska regler för ett ASP i en ASE arbete hello samma som de normalt gör.  Du kan välja ***Prosessorprocent*** under ***skala genom*** och skapar automatiska regler för ASP baserat på CPU-procent eller du kan skapa mer komplexa regler med hjälp av ***schema-och prestanda***.  toosee slutföra mer information om hur du konfigurerar Autoskala Använd hello guiden här [skala en app i Azure App Service][AppScale]. 

### <a name="worker-pool-selection"></a>Val av Worker-Pool
Som tidigare nämnts åt hello worker poolen markeringen från hello ASP-Användargränssnittet.  Öppna hello bladet för hello ASP som du vill använda tooscale och välj arbetspool.  Du kommer se alla hello arbetarpooler som du har konfigurerat i din Apptjänst-miljö.  Om du har bara en arbetspool kommer du bara se hello en pool i listan.  toochange vilken worker poolen ASP, du bara välja hello arbetspool som du vill att din App Service-Plan toomove till.  

![][3]

Innan du flyttar ASP från en arbetare poolen tooanother är det viktigt att du har tillräcklig kapacitet för ASP toomake.  I hello lista över arbetarpooler, inte bara hello worker namn visas men du kan också se hur många personer som är tillgängliga i den poolen, worker.  Kontrollera att det inte finns tillräckligt med tillgängliga instanser toocontain din Programtjänstplan.  Om du behöver fler beräkningsresurser i hello arbetspool gärna toomove att hämta dina ASE administratör tooadd dem.  

> [!NOTE]
> Flytta en ASP från en arbetspool gör att kalla startar hello appar i att ASP.  Detta kan orsaka begäranden toorun långsamt som din app är kall startats på hello nya beräkningsresurser.  hello kallstart kan undvikas genom att använda hello [programmet varmt in kapaciteten] [ AppWarmup] i Azure App Service.  hello programinitiering modulen beskrivs i artikeln hello fungerar även för kallstart eftersom hello initieringsprocessen anropas också när appar är kall startats på nya beräkningsresurser. 
> 
> 

## <a name="getting-started"></a>Komma igång
tooget igång med Apptjänstmiljöer, se [hur tooCreate en Apptjänst-miljö][HowtoCreateASE]

Mer information om hello Azure Apptjänst-plattformen finns [Azure App Service][AzureAppService].

<!--Image references-->
[1]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-aspblade.png
[2]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-manualscale.png
[3]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-sizescale.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ScaleWebapp]: http://azure.microsoft.com/documentation/articles/web-sites-scale/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoConfigureASE]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[CreateWebappinASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-a-web-app-in-an-ase/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[AppScale]: http://azure.microsoft.com/documentation/articles/web-sites-scale/
[AppWarmup]: http://ruslany.net/2015/09/how-to-warm-up-azure-web-app-during-deployment-slots-swap/
