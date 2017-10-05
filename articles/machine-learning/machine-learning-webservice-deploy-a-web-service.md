---
title: "Distribuera en ny webbtjänst i Azure Machine Learning | Microsoft Docs"
description: "Arbetsflöde för att distribuera en ARM-baserad webbtjänst"
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondl
editor: 
ms.assetid: a358b04f-0d08-4d50-820e-eeac971854cf
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2017
ms.author: v-donglo
ROBOTS: NOINDEX
redirect_url: machine-learning-publish-a-machine-learning-web-service
redirect_document_id: TRUE
ms.openlocfilehash: 1415709f9da2bb2cce859af9feb0ec15c1fa5801
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-a-new-web-service"></a>Distribuera en ny webbtjänst
Microsoft Azure Machine learning nu innehåller webbtjänster som baseras på [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) tillåter nya fakturering alternativ och distribuera webbtjänsten till flera regioner.

Det allmänna arbetsflödet för att distribuera en webbtjänst med hjälp av Microsoft Azure Machine Learning-webbtjänster är:

* Skapa en prediktiv experiment
* distribuera den
* konfigurera dess namn
* faktureringsavtal
* testa den
* använda den.

Följande bild illustrerar arbetsflödet.

![Arbetsflöde för distribution av Web service][1]

## <a name="deploy-web-service-from-studio"></a>Distribuera webbtjänsten från Studio
Att distribuera ett experiment som en ny webbtjänst. Logga in på Machine Learning Studio och skapa en ny förutsägande webbtjänst. 

**Obs**: Om du redan har distribuerat ett experiment som en klassiska webbtjänst kan du distribuera den som en ny webbtjänst.

Klicka på **kör** längst ned i experimentet arbetsytan och klicka sedan på **distribuera webbtjänsten** och **distribuera webbtjänsten [ny]**. Sidan distribution av Machine Learning-webbtjänst manager öppnas.

## <a name="machine-learning-web-service-manager-deploy-experiment-page"></a>Machine Learning Web Service Manager distribuera Experiment sida
Ange ett namn för webbtjänsten på sidan distribuera Experiment.
Välj en prisnivå plan. Om du har en befintlig prisavtal kan du välja det måste annars du skapa en ny prisplan för tjänsten. 

1. I den **prisplan** listrutan väljer du ett befintligt schema eller den **Välj ny plan** alternativet.
2. I **schemanamn**, Skriv ett namn som identifierar planen på fakturan.
3. Välj en av de **månatliga planera nivåer**. Observera att planen nivåerna standard att planer för standardregion och webbtjänsten har distribuerats till den regionen.

Klicka på **distribuera** och öppnas sidan Snabbstart för webbtjänsten.

## <a name="quickstart-page"></a>Sidan Snabbstart
Sidan Snabbstart service ger dig åtkomst och riktlinjer för de vanligaste uppgifterna som du utför när du har skapat en ny webbtjänst. Härifrån du enkelt kan komma åt både den **Test** sidan och **förbruka** sidan.

## <a name="testing-your-web-service"></a>Testa din webbtjänst
Sidan Snabbstart klickar du på Test webbtjänsten under vanliga uppgifter.   

Så här testar webbtjänsten som ett svar på begäranden tjänsten (RR):

* Klicka på **Test** på menyraden.
* Klicka på **begäran och svar**.
* Ange lämpliga värden för indatakolumnerna av experimentet.
* Klicka på Testa **begäran och svar**.

Du resultat visas till höger på sidan.

Om du vill testa en Batch Execution Service BES-webbtjänsten använder du en CSV-fil:

* Klicka på **Test** på menyraden.
* Klicka på **Batch**.
* Klicka på Bläddra och navigera till din exempeldatafil under dina indata.
* Klicka på **Test**.

Status för testet visas under **testa batchjobb**.

## <a name="consuming-your-web-service"></a>Använda webbtjänsten
När de distribueras som en webbtjänst ger Azure Machine Learning-experiment en REST-API som kan användas av en mängd olika plattformar och enheter. Detta beror på att enkelt REST API accepterar och svarar med JSON-formaterade meddelanden. Azure Machine Learning-portalen innehåller kod som kan användas för att anropa webbtjänsten i R, C# och Python.

På sidan förbrukning hittar du:

* API-nyckel och URI: er för förbrukning av webbtjänsten i appar.
* Mallar för Excel och web app kan förbättra starta förbrukning-processen.
* Exempelkoden i C#, python och R att komma igång.

Mer information om att konsumera webbtjänster finns [använda en Azure Machine Learning-webbtjänst](machine-learning-consume-web-services.md).

## <a name="next-steps"></a>Nästa steg
Mer information om att konsumera webbtjänster finns:

[Använda en Azure Machine Learning-webbtjänst](machine-learning-consume-web-services.md)

[Azure Machine Learning-webbtjänster: Distribution och användning](machine-learning-deploy-consume-web-service-guide.md)

<!--Image references-->
[1]: ./media/machine-learning-webservice-deploy-a-web-service/armdeploymentworkflow.png


<!--links-->
