---
title: "aaaConnect tooa Machine Learning-webbtjänst | Microsoft Docs"
description: "Med C# eller Python, ansluta tooan Azure Machine Learning webbtjänst med hjälp av auktoriseringsnyckel."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 59b07bab-b60f-48c4-a385-a162e50ec7c2
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/02/2017
ms.author: garye
ROBOTS: NOINDEX
redirect_url: machine-learning-consume-web-services
redirect_document_id: True
ms.openlocfilehash: 0108e71e30a05539a8c0ee93d5aadb07e3d1efa9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooan-azure-machine-learning-web-service"></a>Ansluta tooan Azure Machine Learning-webbtjänst
hello Azure Machine Learning developer upplevelse är en Web service API toomake förutsägelser från indata i realtid eller i batch-läge. Du använder Azure Machine Learning Studio toocreate förutsägelser och distribuera en Azure Machine Learning-webbtjänst.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

toolearn om hur toocreate och distribuera en Machine Learning-webbtjänst med hjälp av Machine Learning Studio:

* En självstudiekurs om hur toocreate ett experiment i Machine Learning Studio finns [skapa ditt första experiment](machine-learning-create-experiment.md).
* Mer information om hur toodeploy en webbtjänst finns [distribuera en Machine Learning-webbtjänst](machine-learning-publish-a-machine-learning-web-service.md).
* Mer information om Machine Learning finns på hello [Machine Learning Documentation Center](https://azure.microsoft.com/documentation/services/machine-learning/).

## <a name="azure-machine-learning-web-service"></a>Azure Machine Learning-webbtjänst
Ett externt program kommunicerar med en Machine Learning arbetsflöde bedömningsprofil modell i realtid med hello Azure Machine Learning-webbtjänst. Ett Machine Learning webbtjänstanrop returnerar resultat för förutsägelse tooan externt program. toomake ett Machine Learning webbtjänstanrop du skickar en API-nyckel som skapas när du distribuerar en förutsägelse. hello Machine Learning-webbtjänst baseras på REST, en populär arkitekturval för webb-programmering projekt.

Azure Machine Learning har två typer av tjänster:

* Svar på begäranden tjänsten (RR) – en låg latens, mycket skalbar tjänst som tillhandahåller ett gränssnitt toohello tillståndslösa modeller skapas och distribueras från hello Machine Learning Studio.
* Batch Execution Service (BES) – en asynkron tjänsten som resultat en batch för dataposter.

Mer information om Machine Learning-webbtjänster finns [distribuera en Machine Learning-webbtjänst](machine-learning-publish-a-machine-learning-web-service.md).

## <a name="get-an-azure-machine-learning-authorization-key"></a>Hämta auktoriseringsnyckel Azure Machine Learning
När du distribuerar experimentet genereras API-nycklar för hello webbtjänsten. Du kan hämta hello nycklar från flera platser.

### <a name="from-hello-microsoft-azure-machine-learning-web-services-portal"></a>Från hello-portalen för Microsoft Azure Machine Learning-webbtjänster
Logga in toohello [Microsoft Azure Machine Learning-webbtjänster](https://services.azureml.net) portal.

tooretrieve hello API-nyckel för en ny Machine Learning-webbtjänst:

1. I hello Azure Machine Learning-webbtjänster portalen klickar du på **Web Services** hello översta menyn.
2. Klicka på hello-webbtjänst som du vill tooretrieve hello nyckel.
3. På hello översta menyn **förbruka**.
4. Kopiera och spara hello **primärnyckel**.

tooretrieve hello API-nyckel för en klassiska Machine Learning-webbtjänst:

1. I hello Azure Machine Learning-webbtjänster portalen klickar du på **klassiska webbtjänster** hello översta menyn.
2. Klicka på hello-webbtjänst som du arbetar.
3. Klicka på hello-slutpunkt som du vill tooretrieve hello nyckel.
4. På hello översta menyn **förbruka**.
5. Kopiera och spara hello **primärnyckel**.

### <a name="classic-web-service"></a>Klassiska webbtjänst
 Du kan också hämta en nyckel för det klassiska från Machine Learning Studio eller hello klassiska Azure-portalen.

#### <a name="machine-learning-studio"></a>Machine Learning Studio
1. I Machine Learning Studio klickar du på **WEB SERVICES** hello vänster.
2. Klicka på en webbtjänst. Hej **API-nyckel** på hello **INSTRUMENTPANELEN** fliken.

#### <a name="azure-classic-portal"></a>Klassisk Azure-portal
1. Klicka på **MASKININLÄRNING** hello vänster.
2. Klicka på hello arbetsyta där webbtjänsten finns.
3. Klicka på **WEBBTJÄNSTER**.
4. Klicka på en webbtjänst.
5. Klicka på en slutpunkt. Hej ”API-NYCKELN” är inte tillgänglig på hello längst ned till höger.

## <a id="connect"></a>Ansluta tooa på Machine Learning-webbtjänst
Du kan ansluta tooa Machine Learning webbtjänst med hjälp av programmeringsspråk som stöder HTTP-begäran och svar. Du kan visa exempel i C#, Python och R från en Machine Learning-webbtjänstens hjälpsida.

**Datorn Learning API hjälp** hjälp med Machine Learning API skapas när du distribuerar en webbtjänst. Se [Azure Machine Learning genomgången distribuerar webbtjänsten](machine-learning-walkthrough-5-publish-web-service.md).
hello Machine Learning API-hjälpen innehåller information om en förutsägelse webbtjänsten.

1. Klicka på hello-webbtjänst som du arbetar.
2. Klicka på hello-slutpunkt som du vill tooview hello API-hjälpsidan.
3. På hello översta menyn **förbruka**.
4. Klicka på **API hjälpsidan** under hello svar på begäranden eller Batch Execution slutpunkter.

**tooview Machine Learning API hjälp för en ny webbtjänst**

I hello Azure Machine Learning Web Services-portalen:

1. Klicka på **WEB SERVICES** på hello översta menyn.
2. Klicka på hello-webbtjänst som du vill tooretrieve hello nyckel.

Klicka på **förbruka** tooget hello URI: er för hello begäran Reposonse och Batch Execution tjänster och exempel kod i C#, R och Python.

Klicka på **Swagger API** tooget Swagger baserat dokumentationen för hello API: er anropas från hello angivna URI: er.

### <a name="c-sample"></a>C#-exempel
tooconnect tooa Machine Learning-webbtjänsten, använda en **HttpClient** skicka ScoreData. ScoreData innehåller en FeatureVector en endimensionell n vector av numeriska funktioner som representerar hello ScoreData. Du kan autentisera toohello Machine Learning-tjänsten med en API-nyckel.

tooconnect tooa Machine Learning-webbtjänst, hello **Microsoft.AspNet.WebApi.Client** NuGet-paketet måste vara installerad.

**Installera Microsoft.AspNet.WebApi.Client NuGet i Visual Studio**

1. Publicera hello Download datauppsättning från UCI: vuxna 2 klass dataset-webbtjänst.
2. Klicka på **Verktyg** > **NuGet Package Manager** > **Package Manager Console**.
3. Välj **Install-Package Microsoft.AspNet.WebApi.Client**.

**toorun hello-kodexempel**

1. Publicera ”exempel 1: hämta dataset från UCI: vuxen 2 klass dataset” experiment, en del av hello Machine Learning provtagning.
2. Tilldela apiKey med hello nyckel från en webbtjänst. Se **hämta auktoriseringsnyckel Azure Machine Learning** ovan.
3. Tilldela serviceUri med hello Begärd URI.

### <a name="python-sample"></a>Python-exempel
tooconnect tooa Machine Learning-webbtjänsten, använda hello **urllib2** biblioteket skicka ScoreData. ScoreData innehåller en FeatureVector en endimensionell n vector av numeriska funktioner som representerar hello ScoreData. Du kan autentisera toohello Machine Learning-tjänsten med en API-nyckel.

**toorun hello-kodexempel**

1. Distribuera ”exempel 1: hämta dataset från UCI: vuxen 2 klass dataset” experiment, en del av hello Machine Learning provtagning.
2. Tilldela apiKey med hello nyckel från en webbtjänst. Se hello **hämta auktoriseringsnyckel Azure Machine Learning** avsnittet hello början av den här artikeln.
3. Tilldela serviceUri med hello Begärd URI.

