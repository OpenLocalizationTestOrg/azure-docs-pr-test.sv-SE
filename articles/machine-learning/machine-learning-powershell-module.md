---
title: "aaaPowerShell-modul för Machine Learning | Microsoft Docs"
description: "hello PowerShell-modulen för Azure Machine Learning är tillgänglig i förhandsversion läge. Använd PowerShell toocreate och hantera arbetsytor, experiment, webbtjänster och mycket mer."
keywords: "experiment, linjär regression,machine learning algoritmer, machine learning självstudier, teknik för förutsägbar modellering, dataexperiment"
services: machine-learning
documentationcenter: 
author: hning86
manager: jhubbard
editor: cgronlun
ms.assetid: a9001cc2-3aa0-47e1-b175-1f76408ba1d1
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/15/2017
ms.author: garye;haining
ms.openlocfilehash: 59362027356b86bf286b7c07380db677ae1d71c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="powershell-module-for-microsoft-azure-machine-learning"></a>PowerShell-modul för Microsoft Azure Machine Learning
hello PowerShell-modulen för Azure Machine Learning är ett kraftfullt verktyg som gör att du toouse Windows PowerShell toomanage arbetsytor, experiment, datauppsättningar, klassisk webbtjänster och mycket mer.

Du kan visa hello dokumentation och hämta hello modulen, tillsammans med hello fullständig källkoden på [https://aka.ms/amlps](https://aka.ms/amlps). 

> [!NOTE]
> hello Azure Machine Learning PowerShell-modulen är för närvarande i förhandsgranskningsläge. hello modulen fortsätter toobe förbättrade och expanderas under den här förhandsversionen. Hålla ett öga på hello [Cortana Intelligence och Machine Learning blogg](https://blogs.technet.microsoft.com/machinelearning/) för nyheter och information.

## <a name="what-is-hello-machine-learning-powershell-module"></a>Vad är Machine Learning PowerShell-modulen för hello?
hello Machine Learning PowerShell-modulen är en. NET-baserade dll-modulen som du kan använda toofully hantera Azure Machine Learning arbetsytor, experiment, datauppsättningar, klassisk webbtjänster och klassisk slutpunkter för webbtjänster från Windows PowerShell. 

Tillsammans med hello modulen kan du hämta hello fullständig källkoden som innehåller ett korrekt åtskilda [C#-API-nivå](https://github.com/hning86/azuremlps/blob/master/code/AzureMLSDK.cs). Du kan referera till DLL-filen från .NET projektet och hantera Azure Machine Learning via .NET-kod. Dessutom beroende hello DLL av underliggande REST API: er som kan användas direkt från din favorit-klienten.

## <a name="what-can-i-do-with-hello-powershell-module"></a>Vad kan jag göra med hello PowerShell-modulen?
Här är några av hello uppgifter du kan utföra med PowerShell-modulen. Kolla in hello [fullständig dokumentation](https://aka.ms/amlps) för dessa och många fler funktioner.

* Etablera en ny arbetsyta med ett hanteringscertifikat ([New AmlWorkspace](https://github.com/hning86/azuremlps#new-amlworkspace))
* Exportera och importera en JSON-fil som representerar ett experimentdiagram ([Export AmlExperimentGraph](https://github.com/hning86/azuremlps#export-amlexperimentgraph) och [Importera AmlExperimentGraph](https://github.com/hning86/azuremlps#import-amlexperimentgraph))
* Kör ett experiment ([Start-AmlExperiment](https://github.com/hning86/azuremlps#start-amlexperiment))
* Skapa en webbtjänst utanför en förutsägbart experiment ([New AmlWebService](https://github.com/hning86/azuremlps#new-amlwebservice))
* Skapa en slutpunkt på en publicerad webbtjänst ([Lägg till AmlWebServiceEndpoint](https://github.com/hning86/azuremlps#add-amlwebserviceendpoint))
* Anropa en RRS- och/eller en BES-webbtjänstslutpunkt ([Invoke-AmlWebServiceRRSEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicerrsendpoint) och [Invoke-AmlWebServicBESEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicebesendpoint))

Här är ett enkelt exempel på hur du använder PowerShell toorun ett befintligt experiment:

        #Find hello first Experiment named “xyz”
        $exp = (Get-AmlExperiment | where Description -eq ‘xyz’)[0]
        #Run hello Experiment
        Start-AmlExperiment -ExperimentId $exp.ExperimentId 

En mer detaljerad användningsfall finns i denna artikel om hur du använder hello PowerShell-modulen tooautomate en aktivitet som begärs ofta: [skapar många Machine Learning-modeller och web service slutpunkter från ett experiment med hjälp av PowerShell](machine-learning-create-models-and-endpoints-with-powershell.md).

## <a name="how-do-i-get-started"></a>Hur kommer jag igång?
tooget igång med Machine Learning PowerShell kan du ladda ner hello [versionspaket](https://github.com/hning86/azuremlps/releases) från GitHub och följ hello [anvisningar för installation](https://github.com/hning86/azuremlps/blob/master/README.md). hello anvisningar förklarar hur toounblock hello hämtas/uppackade DLL-filen och sedan importera den till din PowerShell-miljö. De flesta hello cmdlets kräva att du anger hello arbetsyte-ID, hello arbetsytan autentiseringstoken, och hello Azure-region som hello arbetsytan är i. hello enklaste sättet tooprovide hello värden är via en standardfil config.json. hello anvisningar förklaras också hur tooconfigure den här filen. 

Och om du vill kan du klona hello git-trädet, ändra hello koden och kompilera den lokalt med hjälp av Visual Studio.

## <a name="next-steps"></a>Nästa steg
Du kan hitta hello fullständig dokumentation för hello PowerShell-modulen på [https://aka.ms/amlps](https://aka.ms/amlps). 

Ett utökat exempel på hur toouse hello-modul i ett verkligt scenario checka ut hello djupgående användningsfall, [skapar många Machine Learning-modeller och web service slutpunkter från ett experiment med hjälp av PowerShell](machine-learning-create-models-and-endpoints-with-powershell.md).
