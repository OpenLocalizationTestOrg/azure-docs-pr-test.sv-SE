---
title: "aaaDeploying en ny webbtjänst i Azure Machine Learning | Microsoft Docs"
description: "hello Arbetsflödesbaserade för att distribuera en ARM webbtjänst"
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
redirect_document_id: True
ms.openlocfilehash: 2cbfda44b97a6b992fbdfdfb0c761e6c9e169035
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-new-web-service"></a>Distribuera en ny webbtjänst
Microsoft Azure Machine learning nu innehåller webbtjänster som baseras på [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) tillåter nya fakturering alternativ och distribuera din web service toomultiple regioner.

hello Allmänt arbetsflöde toodeploy en webbtjänst med hjälp av Microsoft Azure Machine Learning-webbtjänster är:

* Skapa en prediktiv experiment
* distribuera den
* konfigurera dess namn
* faktureringsavtal
* testa den
* använda den.

hello följande bild illustrerar arbetsflödet för hello.

![Arbetsflöde för distribution av Web service][1]

## <a name="deploy-web-service-from-studio"></a>Distribuera webbtjänsten från Studio
toodeploy ett experiment som en ny webbtjänst. Logga in på hello Machine Learning Studio och skapa en ny förutsägande webbtjänst. 

**Obs**: Om du redan har distribuerat ett experiment som en klassiska webbtjänst kan du distribuera den som en ny webbtjänst.

Klicka på **kör** längst hello hello experimentera arbetsytan och klicka sedan på **distribuera webbtjänsten** och **distribuera webbtjänsten [ny]**. hello öppnas distribution av hello Machine Learning-webbtjänst manager.

## <a name="machine-learning-web-service-manager-deploy-experiment-page"></a>Machine Learning Web Service Manager distribuera Experiment sida
Ange ett namn för webbtjänsten hello hello distribuera Experiment på sidan.
Välj en prisnivå plan. Om du har en befintlig prisavtal kan du välja det annars skapa du en ny prisplan för hello-tjänsten. 

1. I hello **prisplan** listrutan väljer du ett befintligt schema eller hello **Välj ny plan** alternativet.
2. I **schemanamn**, Skriv ett namn som identifierar hello plan på fakturan.
3. Välj en av hello **månatliga planera nivåer**. Observera att hello plan nivåerna standard toohello planer för standardregion och webbtjänsten är distribuerade toothat region.

Klicka på **distribuera** och hello Snabbstart för webbtjänsten öppnas.

## <a name="quickstart-page"></a>Sidan Snabbstart
hello-webbtjänstsida Quickstart ger dig åtkomst och vägledning på hello vanligaste uppgifter som du utför när du har skapat en ny webbtjänst. Härifrån kan du enkelt komma åt båda hello **Test** sidan och **förbruka** sidan.

## <a name="testing-your-web-service"></a>Testa din webbtjänst
Klicka på Testa webbtjänsten under vanliga uppgifter från hello Snabbstart.   

tootest hello webbtjänsten som ett svar på begäranden tjänsten (RR):

* Klicka på **Test** hello menyraden.
* Klicka på **begäran och svar**.
* Ange lämpliga värden för hello indatakolumnerna av experimentet.
* Klicka på Testa **begäran och svar**.

Du resultat visas hello höger på sidan hello.

tootest en Batch Execution Service BES-webbtjänsten, ska du använda en CSV-fil:

* Klicka på **Test** hello menyraden.
* Klicka på **Batch**.
* Klicka på Bläddra och navigera tooyour exempeldatafil under dina indata.
* Klicka på **Test**.

hello status för testet visas under **testa batchjobb**.

## <a name="consuming-your-web-service"></a>Använda webbtjänsten
När de distribueras som en webbtjänst ger Azure Machine Learning-experiment en REST-API som kan användas av en mängd olika plattformar och enheter. Det beror på att hello enkla REST-API accepterar och svarar med JSON-formaterade meddelanden. hello Azure Machine Learning-portalen innehåller koden som du kan använda toocall hello-webbtjänsten i R, C# och Python.

På sidan för användning av hello du:

* hello API-nyckeln och URI: er för förbrukning av webbtjänsten i appar.
* Excel och web app mallar tookick starta förbrukning-processen.
* Exempelkoden i C#, python och R tooget du startade.

Mer information om att konsumera webbtjänster finns [hur tooconsume en Azure Machine Learning-webbtjänst](machine-learning-consume-web-services.md).

## <a name="next-steps"></a>Nästa steg
Mer information om att konsumera webbtjänster finns:

[Hur tooconsume en Azure Machine Learning-webbtjänst](machine-learning-consume-web-services.md)

[Azure Machine Learning-webbtjänster: Distribution och användning](machine-learning-deploy-consume-web-service-guide.md)

<!--Image references-->
[1]: ./media/machine-learning-webservice-deploy-a-web-service/armdeploymentworkflow.png


<!--links-->
