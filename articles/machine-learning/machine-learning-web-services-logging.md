---
title: "aaaLogging för Machine Learning-webbtjänster | Microsoft Docs"
description: "Lär dig hur tooenable loggning för Machine Learning-webbtjänster. Loggning ger ytterligare information toohelp felsöka hello API: er."
services: machine-learning
documentationcenter: 
author: raymondlaghaeian
manager: jhubbard
editor: cgronlun
ms.assetid: c54d41e1-0300-46ef-bbfc-d6f7dca85086
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/15/2017
ms.author: raymondl;garye
ms.openlocfilehash: ed23933d52d2151af658af2307d7df8743071f65
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-logging-for-machine-learning-web-services"></a>Aktivera loggning för Machine Learning-webbtjänster
Det här dokumentet innehåller information om hello loggning möjligheterna för Machine Learning-webbtjänster. Loggning ger ytterligare information, förutom bara ett felnummer och ett meddelande som kan hjälpa dig att felsöka din anrop toohello Machine Learning API: er.  

## <a name="how-tooenable-logging-for-a-web-service"></a>Hur tooenable loggning för en webbtjänst

Du aktiverar loggning från hello [Azure Machine Learning-webbtjänster](https://services.azureml.net) portal. 

1. Logga in toohello Azure Machine Learning-webbtjänster portalen på [https://services.azureml.net](https://services.azureml.net). För en klassisk webbtjänst, du kan också få toohello portal genom att klicka på **nya Web Services upplevelsen** på hello Machine Learning-webbtjänster sida i Machine Learning Studio.

   ![Ny Services webbupplevelse länk](media/machine-learning-web-services-logging/new-web-services-experience-link.png)

2. Hello översta menyraden klickar du på **Web Services** för en ny webbtjänst eller klicka på **klassiska webbtjänster** ett klassiskt webbtjänsten.

   ![Välj ny eller klassisk webbtjänster](media/machine-learning-web-services-logging/select-web-service.png)

3. Klicka på hello webbtjänstnamn för en ny webbtjänst. Klicka på hello webbtjänstnamn och på hello nästa sida klickar du på lämplig hello-slutpunkten för en klassisk webbtjänst.

4. Klicka på hello huvudmenyn **konfigurera**.

5. Ange hello **aktivera loggning** alternativet för*fel* (toolog endast fel) eller *alla* (för fullständig loggning).

   ![Välj loggningsnivån](media/machine-learning-web-services-logging/enable-logging.png)

6. Klicka på **Spara**.

7. För klassisk webbtjänster, skapa hello **ml-diagnostik** behållare.

   Alla web service loggar sparas i en blobbbehållare med namnet **ml-diagnostik** i hello storage-konto som är associerade med hello-webbtjänsten. Nya webbtjänster, har den här behållaren skapats hello första gången du öppnar hello-webbtjänsten. För klassisk webbtjänster behöver du toocreate hello behållaren om den inte redan finns. 

   1. I hello [Azure-portalen](https://portal.azure.com)går toohello storage-konto som är associerade med hello-webbtjänsten.

   2. Under **Blob-tjänsten**, klickar du på **behållare**.

   3. Om hello behållaren **ml-diagnostik** inte finns, klickar du på **+ behållare**, ge hello behållaren hello namnet ”ml-diagnostik” och välj hello **komma åt typen** som ”Blob”. Klicka på **OK**.

      ![Välj loggningsnivån](media/machine-learning-web-services-logging/create-ml-diagnostics-container.png)

> [!TIP]
>
> Hello Web Services instrumentpanelen i Machine Learning Studio har också en växel tooenable loggning för en klassisk webbtjänst. Eftersom loggning hanteras nu via hello Web Services-portalen, behöver du dock tooenable via hello portal som beskrivs i den här artikeln. Om du redan loggar in Studio, inaktivera loggning i hello Web Services-portalen och aktivera det igen.


## <a name="hello-effects-of-enabling-logging"></a>hello effekterna av att aktivera loggning
När loggning är aktiverat hello diagnostik- och fel från hello webbtjänstslutpunkt loggas i hello **ml-diagnostik** blob-behållaren i hello Azure Storage-konto som är kopplad till hello användarens arbetsytan. Den här behållaren innehåller alla hello diagnostikinformationen för alla hello slutpunkter för webbtjänster för alla hello arbetsytor som är associerade med det här lagringskontot.

hello loggar kan visas med hjälp av hello flera verktyg tillgängliga tooexplore ett Azure Storage-konto. hello enklaste får toonavigate toohello storage-konto i hello Azure-portalen, klicka på **behållare**, och klicka sedan på hello behållaren **ml-diagnostik**.  

## <a name="log-blob-detail-information"></a>Information om blob logg
Varje blobb i behållaren hello innehåller hello diagnostikinformationen för exakt ett av hello följande åtgärder:

* En körning av hello batchkörning metoden  
* En körning av metoden hello begäran och svar  
* Initieringen av en begäran och svar-behållare

hello namnet på varje blob har prefixet hello följande format: 


`{Workspace Id}-{Web service Id}-{Endpoint Id}/{Log type}`


Där _logga typen_ är en av hello följande värden:  

* Batch  
* score-begäranden  
* poäng/init  

