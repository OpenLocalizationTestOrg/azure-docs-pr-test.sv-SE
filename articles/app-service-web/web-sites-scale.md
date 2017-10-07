---
title: aaaScale in en app i Azure | Microsoft Docs
description: "Lär dig hur tooscale upp en app i Azure App Service tooadd kapacitet och funktioner."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: mollybos
ms.assetid: f7091b25-b2b6-48da-8d4a-dcf9b7baccab
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2016
ms.author: cephalin
ms.openlocfilehash: 97f46f77aeef95aec90d38e8396a023820e3caeb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scale-up-an-app-in-azure"></a>Skala upp en app i Azure
Den här artikeln beskrivs hur du tooscale din app i Azure App Service. Det finns två arbetsflöden för skalning, skala upp och skala ut och den här artikeln förklarar hello skala in arbetsflöde.

* [Skala upp](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): hämta mer CPU, minne, diskutrymme och extra funktioner såsom dedikerade virtuella datorer (VM), anpassade domäner och certifikat, mellanlagringsplatser kortplatser, autoskalning med mera. Du skalar upp genom att ändra hello prisnivån för App Service-plan som tillhör din app.
* [Skala ut](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): öka hello antalet VM-instanser som appen körs.
  Du kan skala ut tooas många som 20 instanser, beroende på din prisnivå. [Apptjänstmiljöer](../app-service/app-service-app-service-environments-readme.md) i **Premium** öka nivån ytterligare dina skalbar antal too50-instanser. Mer information om att skala ut finns [skala instansantalet manuellt eller automatiskt](../monitoring-and-diagnostics/insights-how-to-scale.md). Det finns out hur toouse autoskalning, vilket är tooscale instansantalet automatiskt baserat på fördefinierade regler och scheman.

Hej skala inställningar ta endast sekunder tooapply och påverkar alla program i din [programtjänstplanen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
De behöver du toochange koden eller inte omdistribuera ditt program.

Information om hello priser och funktioner i enskilda App Service-planer finns [App Service-prisinformation](https://azure.microsoft.com/pricing/details/web-sites/).  

> [!NOTE]
> Innan du växlar en apptjänstplan från hello **lediga** nivån, måste du först ta bort hello [utgiftsgränser](https://azure.microsoft.com/pricing/spending-limits/) för din Azure-prenumeration. tooview eller ändra alternativen för prenumerationen Microsoft Azure App Service finns [Microsoft Azure-prenumerationer][azuresubscriptions].
> 
> 

<a name="scalingsharedorbasic"></a>
<a name="scalingstandard"></a>

## <a name="scale-up-your-pricing-tier"></a>Skala upp din prisnivå
1. Öppna i din webbläsare hello [Azure-portalen][portal].
2. I bladet för din app, klickar du på **alla inställningar**, och klicka sedan på **skala upp**.
   
    ![Navigera tooscale in din Azure-app.][ChooseWHP]
3. Välj din nivå och klicka sedan på **Välj**.
   
    Hej **meddelanden** fliken blinkar en grön **lyckade** när hello åtgärd har slutförts.

<a name="ScalingSQLServer"></a>

## <a name="scale-related-resources"></a>Skala relaterade resurser
Om din app är beroende av andra tjänster, till exempel Azure SQL Database eller Azure Storage, kan du även skala upp dessa resurser utifrån dina behov. Dessa resurser skalas inte med hello App Service-plan och måste skalas separat.

1. I **Essentials**, klicka på hello **resursgruppen** länk.
   
    ![Skala upp din Azure-app relaterade resurser](./media/web-sites-scale/RGEssentialsLink.png)
2. I hello **sammanfattning** tillhör hello **resursgruppen** blad, klicka på en resurs som du vill tooscale. hello följande skärmbild visar en SQL-databas resurs och ett Azure Storage-resurs.
   
    ![Navigera tooresource grupp bladet tooscale in din Azure-app](./media/web-sites-scale/ResourceGroup.png)
3. För en resurs för SQL-databasen, klickar du på **inställningar** > **prisnivå** tooscale hello prisnivån.
   
    ![Skala upp hello SQL Database-serverdelen för din Azure-app](./media/web-sites-scale/ScaleDatabase.png)
   
    Du kan också aktivera [georeplikering](../sql-database/sql-database-geo-replication-overview.md) för din instans av SQL-databas.
   
    För ett Azure Storage-resurs, klickar du på **inställningar** > **Configuration** tooscale konfigurerar din lagringsalternativ.
   
    ![Skala upp hello Azure Storage-konto som används av din Azure-app](./media/web-sites-scale/ScaleStorage.png)

<a name="devfeatures"></a>

## <a name="learn-about-developer-features"></a>Läs mer om funktionerna för utvecklare
Beroende på hello prisnivån, hello följande utvecklarorienterade funktioner är tillgängliga:

### <a name="bitness"></a>Bitness
* Hej **grundläggande**, **Standard**, och **Premium** nivåer stöd för 64-bitars och 32-bitars program.
* Hej **lediga** och **delade** plan nivåer stöder endast 32-bitarsprogram.

### <a name="debugger-support"></a>Stöd för felsökning
* Felsökare support är tillgänglig för hello **lediga**, **delade**, och **grundläggande** lägen på en anslutning per App Service-plan.
* Felsökare support är tillgänglig för hello **Standard** och **Premium** lägen på fem anslutningar per App Service-plan.

<a name="OtherFeatures"></a>

## <a name="learn-about-other-features"></a>Lär dig mer om andra funktioner
* Detaljerad information om alla återstående funktioner i hello Apptjänst hello planer, inklusive priser och funktioner intresse tooall användare (inklusive utvecklare), finns [App Service-prisinformation](https://azure.microsoft.com/pricing/details/web-sites/).

> [!NOTE]
> Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/) där du direkt kan skapa en tillfällig startwebbapp i App Service. Inget kreditkort krävs och gör inga åtaganden.
> 
> 

<a name="Next Steps"></a>

## <a name="next-steps"></a>Nästa steg
* tooget igång med Azure, se [kostnadsfri utvärderingsversion av Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/).
* Besök hello följande länkar för information om priser, support och SERVICENIVÅAVTAL.
  
    [Prisinformation för dataöverföringar](https://azure.microsoft.com/pricing/details/data-transfers/)
  
    [Microsoft Azure-supportplaner](https://azure.microsoft.com/support/plans/)
  
    [serviceavtal](https://azure.microsoft.com/support/legal/sla/)
  
    [Prisinformation för SQL-databas](https://azure.microsoft.com/pricing/details/sql-database/)
  
    [Virtuell dator och Molntjänststorlekar för Microsoft Azure][vmsizes]
  
    [Prisinformation för Apptjänst](https://azure.microsoft.com/pricing/details/app-service/)
  
    [Apptjänst prisinformationen - SSL-anslutningar](https://azure.microsoft.com/pricing/details/web-sites/#ssl-connections)
* Information om Azure App Service finns bästa praxis, inklusive att skapa en skalbar och flexibel arkitektur [metodtips: Azure App Service Web Apps](http://blogs.msdn.com/b/windowsazure/archive/2014/02/10/best-practices-windows-azure-websites-waws.aspx).
* Videor om att skala Apptjänst-appar finns i hello följande resurser:
  
  * [När tooScale Azure Websites – med Stefan Schackow](https://azure.microsoft.com/resources/videos/azure-web-sites-free-vs-standard-scaling/)
  * [Automatisk skalning Azure Websites, CPU eller schemalagt - med Stefan Schackow](https://azure.microsoft.com/resources/videos/auto-scaling-azure-web-sites/)
  * [Hur Azure webbplatser skala - med Stefan Schackow](https://azure.microsoft.com/resources/videos/how-azure-web-sites-scale/)

<!-- LINKS -->
[vmsizes]:/pricing/details/app-service/
[SQLaccountsbilling]:http://go.microsoft.com/fwlink/?LinkId=234930
[azuresubscriptions]:http://go.microsoft.com/fwlink/?LinkID=235288
[portal]: https://portal.azure.com/

<!-- IMAGES -->
[ChooseWHP]: ./media/web-sites-scale/scale1ChooseWHP.png
[ChooseBasicInstances]: ./media/web-sites-scale/scale2InstancesBasic.png
[SaveButton]: ./media/web-sites-scale/05SaveButton.png
[BasicComplete]: ./media/web-sites-scale/06BasicComplete.png
[ScaleStandard]: ./media/web-sites-scale/scale3InstancesStandard.png
[Autoscale]: ./media/web-sites-scale/scale4AutoScale.png
[SetTargetMetrics]: ./media/web-sites-scale/scale5AutoScaleTargetMetrics.png
[SetFirstRule]: ./media/web-sites-scale/scale6AutoScaleFirstRule.png
[SetSecondRule]: ./media/web-sites-scale/scale7AutoScaleSecondRule.png
[SetThirdRule]: ./media/web-sites-scale/scale8AutoScaleThirdRule.png
[SetRulesFinal]: ./media/web-sites-scale/scale9AutoScaleFinal.png
[ResourceGroup]: ./media/web-sites-scale/scale10ResourceGroup.png
[ScaleDatabase]: ./media/web-sites-scale/scale11SQLScale.png
[GeoReplication]: ./media/web-sites-scale/scale12SQLGeoReplication.png
