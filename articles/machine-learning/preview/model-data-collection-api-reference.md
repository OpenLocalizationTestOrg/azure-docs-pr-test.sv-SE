---
title: Azure Machine Learning modellen Data Collection API-referens | Microsoft Docs
description: Azure Machine Learning modellen Data Collection API-referens.
services: machine-learning
author: aashishb
ms.author: aashishb
manager: neerajkh
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 09/12/2017
ms.openlocfilehash: 7a0fda8a44d13bcaba84b4124d9b693c05874154
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="azure-machine-learning-model-data-collection-api-reference"></a>Azure Machine Learning modellen Data Collection API-referens

Insamling av modellen låter dig Arkivera modellen indata och förutsägelser från machine learning-webbtjänst. Finns det [modellen data collection instruktioner](how-to-use-model-data-collection.md) att förstå hur du installerar `azureml.datacollector` på din Windows- och Linux-dator.

I den här API-referenshandboken använda vi stegvis om hur du samlar in modellen indata och förutsägelser från machine learning-webbtjänst.

## <a name="enable-model-data-collection-in-azure-ml-workbench-environment"></a>Aktivera datainsamling för modellen i Azure ML arbetsstationsmiljö

 Leta efter conda\_dependencies.yml filen i projektet under mappen aml_config och har din conda\_dependencies.yml filen innehåller modulen azureml.datacollector under avsnittet pip enligt nedan. Observera att detta är endast delar av en fullständig conda\_dependencies.yml fil:

    dependencies:
      - python=3.5.2
      - pip:
        - azureml.datacollector==0.1.0a13

>[!NOTE] 
>För närvarande kan också använda modulen insamlaren data i Azure ML-arbetsstationen genom att köra i docker-läge. Lokalt läge kanske inte fungerar för alla miljöer.




## <a name="enable-model-data-collection-in-the-scoring-file"></a>Aktivera datainsamling för modellen i filen bedömningsprofil

Importera data collector modulen och ModelDataCollector klass i bedömningsprofil filen som används för operationalization:

    from azureml.datacollector import ModelDataCollector


## <a name="model-data-collector-instantiation"></a>Modellen data collector instansiering
Skapa en instans av en ny instans av en ModelDataCollector:

DC = ModelDataCollector (modellnamn, identifierare = 'default' feature_names = ingen model_management_account_id = 'okänt', webservice_name = 'okänt', model_id = 'okänt', model_version = 'okänt')

Se klassen och parametern information:

### <a name="class"></a>Klass
| Namn | Beskrivning |
|--------------------|--------------------|
| ModelDataCollector | En klass i azureml.datacollector namnområde. En instans av den här klassen används för att samla in modelldata. En enskild bedömningsprofil fil kan innehålla flera ModelDataCollectors. Varje instans som ska användas för att samla in data i en separat plats i filen bedömningsprofil så att schemat för insamlade data förblir konsekventa (det vill säga indata och förutsägelse)|


### <a name="parameters"></a>Parametrar

| Namn | Typ | Beskrivning |
|-------------|------------|-------------------------|
| modellnamn | Sträng | namnet på den modell som vilka data som samlas in |
| Identifierare | Sträng | platsen i kod som identifierar dessa data, dvs. 'RawInput' eller 'Förutsägelse' |
| feature_names | lista med strängar | en lista med funktionsnamn som blivit csv-huvudet när anges |
| model_management_account_id | Sträng | identifierare för hanteringskontot modell där den här modellen lagras. Det fylls i automatiskt när modeller operationalized via AML |
| webservice_name | Sträng | namnet på den webbtjänst som den här modellen för närvarande har distribuerats. Det fylls i automatiskt när modeller operationalized via AML |
| model_id | Sträng | Den unika identifieraren för den här modellen i samband med ett konto för hantering av modellen. Det fylls i automatiskt när modeller operationalized via AML |
| model_version | Sträng | versionsnummer för den här modellen i samband med ett konto för hantering av modellen. Det fylls i automatiskt när modeller operationalized via AML |



 

## <a name="collecting-the-model-data"></a>Samla in modelldata

Du kan samla in modelldata med en instans av ModelDataCollector skapade ovan.

    dc.collect(input_data, user_correlation_id="")

Se metoden och parametern information:

### <a name="method"></a>Metod
| Namn | Beskrivning |
|--------------------|--------------------|
| samla in | Används för att samla in data för en modell indata eller förutsägelse|


### <a name="parameters"></a>Parametrar

| Namn | Typ | Beskrivning |
|-------------|------------|-------------------------|
| input_data | flera typer | data samlas in (för närvarande accepterar listan över filtyper, numpy.array, pandas. DataFrame, pyspark.sql.DataFrame). För dataframe typer om en rubrik med funktionsnamn, information om detta finns i data-målet (utan att behöva skicka uttryckligen funktionsnamn i konstruktorn ModelDataCollector) |
| user_correlation_id | Sträng | ett valfritt Korrelations-id som kan anges av användaren för att korrelera denna förutsägelse |

