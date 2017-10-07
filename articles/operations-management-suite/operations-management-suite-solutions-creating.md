---
title: "aaaBuild en lösning i OMS | Microsoft Docs"
description: "Hanteringslösningar utöka hello funktioner i Operations Management Suite (OMS) genom att tillhandahålla paketerade hanteringsscenarier som kunder kan lägga till tootheir OMS-arbetsyta.  Den här artikeln innehåller information om hur du kan skapa management lösningar toobe används i din egen miljö eller göras tillgänglig tooyour kunder."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 1915e204-ba7e-431b-9718-9eb6b4213ad8
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/20/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: dea4c0d9e608d9fe4aa41088705958c9fe999372
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="design-and-build-a-management-solution-in-operations-management-suite-oms-preview"></a>Skapa en lösning i Operations Management Suite (OMS) (förhandsgranskning)
> [!NOTE]
> Den här är dokumentationen preliminär för att skapa lösningar för hantering i OMS som för närvarande finns i förhandsgranskningen. Ett schema som beskrivs nedan är ämne toochange.

[Hanteringslösningar](operations-management-suite-solutions.md) utöka hello funktioner i Operations Management Suite (OMS) genom att tillhandahålla paketerade hanteringsscenarier som kunder kan lägga till tootheir OMS-arbetsyta.  Den här artikeln innehåller en grundläggande process toodesign och skapa en lösning som passar för de vanligaste krav.  Om du är ny toobuilding hanteringslösningar kan du använda den här processen som en startpunkt och sedan utnyttja hello begrepp för mer komplexa lösningar som dina behov utvecklas.

## <a name="what-is-a-management-solution"></a>Vad är en lösning?

Lösningar för hantering innehåller OMS och Azure-resurser som arbetar tillsammans för tooachieve ett visst scenario för övervakning.  De är implementerade som [resurshantering mallar](../azure-resource-manager/resource-manager-template-walkthrough.md) som innehåller information om hur tooinstall och konfigurera sina befintliga resurser när hello lösningen är installerad.

Hej grundläggande strategi är toostart din lösning genom att skapa hello enskilda komponenter i Azure-miljön.  När du har hello funktioner fungerar, du kan starta paketera dem till en [management lösningsfilen](operations-management-suite-solutions-solution-file.md). 


## <a name="design-your-solution"></a>Utforma din lösning
hello vanligaste mönster som en lösning för visas i följande diagram hello.  hello olika komponenter i det här mönstret diskuteras hello nedan.

![OMS-lösning: översikt](media/operations-management-suite-solutions/solution-overview.png)


### <a name="data-sources"></a>Datakällor
hello första steget i utforma en lösning är att bestämma hello data som du behöver från hello logganalys-databasen.  Dessa data kan samlas in av en [datakällan](../log-analytics/log-analytics-data-sources.md) eller [en annan lösning](operations-management-suite-solutions.md), eller din lösning måste tooprovide hello processen toocollect den.

Det finns ett antal sätt datakällor som kan samlas i hello logganalys databasen enligt beskrivningen i [datakällor i logganalys](../log-analytics/log-analytics-data-sources.md).  Detta innefattar händelser i hello händelseloggen eller genereras av Syslog dessutom tooperformance räknarna för både Windows- och Linux-klienter.  Du kan också samla in data från Azure-resurser som samlas in av Azure-Monitor.  

Om du behöver data som inte är tillgängligt via hello tillgängliga datakällor, så du kan använda hello [HTTP Data Collector API: et](../log-analytics/log-analytics-data-collector-api.md) vilket gör att du toowrite toohello logganalys lagringsplats för data från alla klienter som kan anropa en REST API.  hello vanligaste för anpassad insamling i en lösning för hantering av är toocreate en [runbook i Azure Automation](../automation/automation-runbook-types.md) som samlar in hello krävs data från Azure eller externa resurser och använder hello Data Collector API toowrite toohello databas.  

### <a name="log-searches"></a>Log-sökningar
[Logga sökningar](../log-analytics/log-analytics-log-searches.md) har använt tooextract och analysera data i hello logganalys-databasen.  De används i vyer och aviseringar i tillägg tooallowing hello användaren tooperform ad hoc-analys av data i hello-databasen.  

Du bör definiera frågor som du tror kommer att vara användbara toohello användaren även om de inte används av alla vyer eller aviseringar.  Dessa kommer att finnas tillgängliga toothem som sparade sökningar i hello portal och du kan även inkludera dem i en [listan frågor visualiseringen del](../log-analytics/log-analytics-view-designer-parts.md#list-of-queries-part) i den anpassade vyn.

### <a name="alerts"></a>Aviseringar
[Aviseringar i logganalys](../log-analytics/log-analytics-alerts.md) identifiera problem med hjälp av [logga sökningar](#log-searches) mot hello data i hello-databas.  De Avisera hello användaren eller köra automatiskt en åtgärd som svar. Du bör identifiera olika avisering villkor för ditt program och inkludera motsvarande Varningsregler i din lösningsfilen.

Om hello problemet kan eventuellt åtgärdas med en automatiserad process, ska sedan du vanligtvis skapa en runbook i Azure Automation tooperform denna reparation.  De flesta Azure-tjänster kan hanteras med [cmdlets](/powershell/azure/overview) vilka hello runbook skulle utnyttja tooperform sådana funktioner.

Om din lösning kräver externa funktioner i svaret tooan avisering, så du kan använda en [webhook svar](../log-analytics/log-analytics-alerts-actions.md).  Detta ger dig toocall en extern webbtjänst som skickar information från hello avisering.

### <a name="views"></a>Vyer
Vyer i logganalys ska använda toovisualize data från hello logganalys-databasen.  Varje lösning innehåller vanligtvis en enskild vy med en [panelen](../log-analytics/log-analytics-view-designer-tiles.md) som visas på hello användarens huvudinstrumentpanelen.  hello vyn kan innehålla valfritt antal [visualiseringen delar](../log-analytics/log-analytics-view-designer-parts.md) tooprovide olika datavisualiseringar hello insamlade data toohello användare.

Du [skapa anpassade vyer med hjälp av hello Vydesigner](../log-analytics/log-analytics-view-designer.md) som du kan exportera som ska ingå i din lösningsfilen senare.  


## <a name="create-solution-file"></a>Skapa lösningsfilen
När du har konfigurerat och testat hello-komponenter som ska ingå i din lösning, kan du [skapa din lösningsfilen](operations-management-suite-solutions-solution-file.md).  Du kommer att implementera hello komponenter i en [Resource Manager-mall](../azure-resource-manager/resource-group-authoring-templates.md) som innehåller en [resurs](operations-management-suite-solutions-solution-file.md#solution-resource) med relationer toohello hello andra resurser i filen.  


## <a name="test-your-solution"></a>Testa din lösning
När du utvecklar din lösning du behöver tooinstall och testa den i din arbetsyta.  Du kan göra detta med hjälp av hello tillgängliga metoder för[testa och installera Resource Manager-mallar](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="publish-your-solution"></a>Publicera din lösning
När du har slutfört och testat din lösning kan göra du det tillgängligt toocustomers via antingen hello följande källor.

- **Azure-snabbstartsmallar**.  [Azure-snabbstartsmallar](https://azure.microsoft.com/resources/templates/) är en uppsättning Resource Manager-mallar som har bidragit med hello community via GitHub.  Du kan göra din lösning tillgängliga med följande information i hello [bidrag guiden](https://github.com/Azure/azure-quickstart-templates/tree/master/1-CONTRIBUTION-GUIDE).
- **Azure Marketplace**.  Hej [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/) kan du toodistribute och sälja din lösning tooother utvecklare, ISV: er, och IT-proffs.  Du kan lära dig hur toopublish din lösning tooAzure Marketplace på [hur toopublish och hantera ett erbjudande i hello Azure Marketplace](../marketplace-publishing/marketplace-publishing-getting-started.md).



## <a name="next-steps"></a>Nästa steg
* Lär dig hur för[skapa en lösningsfil](operations-management-suite-solutions-solution-file.md) för din lösning för hantering.
* Läs hello information om [redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md).
* Sök [Azure Quickstart mallar](https://azure.microsoft.com/documentation/templates) exempel på olika Resource Manager-mallar.