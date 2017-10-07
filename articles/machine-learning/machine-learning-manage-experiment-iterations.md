---
title: aaaManage experimentera iterationer i Machine Learning Studio | Microsoft Docs
description: Hur toomanage experimentera iterationer i Azure Machine Learning Studio
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 6a53530f-20d5-40ae-9b49-7b499ccb44b7
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye
ms.openlocfilehash: bd30c048ce063811b1b2de8ce6d71e99ba975713
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-experiment-iterations-in-azure-machine-learning-studio"></a>Hantera iterationer av experiment i Azure Machine Learning Studio
Utveckla en prediktiv analysmodell är en återkommande process - när du ändrar hello olika funktioner och parametrar av experimentet resultaten att Konvergera tills du är nöjd du har en tränad, effektiv modell. Nyckeln toothis processen är spårning hello olika iterationer av dina experiment parametrar och konfigurationer.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Du kan granska tidigare körs från dina experiment när som helst i ordning toochallenge besöker, och slutligen bekräfta eller förfina tidigare antaganden. När du kör ett experiment, sparar en historik över hello kör, inklusive dataset, modul, och kommunikationsportar och parametrar i Machine Learning Studio. Denna historik avbildar också resultat, runtime information, till exempel start- och stopptider, loggmeddelanden och Körstatus. Du kan gå tillbaka till någon av dessa körs vid varje gång tooreview hello kronologisk ordning experiment och mellanliggande resultat. Du kan även använda en tidigare körning av experimentet-toolaunch till en ny fas i förfrågan och identifiering på din sökväg toocreating enkla, komplexa eller även ensemble modeling lösningar.

> [!NOTE]
> När du visar en tidigare körning av ett experiment som versionen av hello experiment är låst och kan inte redigeras. Du kan dock spara en kopia av den genom att klicka på **Spara som** och ange ett nytt namn för hello kopia. Machine Learning Studio öppnas hello ny kopia som du kan redigera och köra. Den här kopian av experimentet är tillgängliga i hello **EXPERIMENT** listan tillsammans med alla andra försök.
> 
> 

## <a name="viewing-hello-prior-run"></a>Visa hello tidigare kör
När du har en öppen i experiment som du måste köra minst en gång, kan du visa hello före körning av hello experiment genom att klicka på **tidigare kör** hello egenskaper i fönstret.

Anta att du skapar ett experiment och kör versioner av det på 11:23 11:42 och 11:55. Om du öppnar hello senaste körningen av experimentet hello (11:55) och klicka på **tidigare kör**, hello-version som du körde på 11:42 öppnas.

## <a name="viewing-hello-run-history"></a>Visa hello Körningshistorik
Du kan visa alla hello tidigare körningar av ett experiment genom att klicka på **visa Körningshistorik** i en öppen experiment.

Anta att du skapar ett experiment med hello [linjär Regression] [ linear-regression] modul och du vill att tooobserve hello effekten av att ändra hello värdet för **inlärningsfrekvensen** på experiment resultaten. Du har kört hello experiment flera gånger med olika värden för den här parametern på följande sätt:

| Learning värde | Starttid för körning |
| --- | --- |
| 0.1 |2014-9-11 4:18:58 pm. |
| 0.2 |2014-9-11 4:24:33 pm |
| 0.4 |2014-9-11 4:28:36 pm |
| 0.5 |2014-9-11 4:33:31 pm. |

Om du klickar på **visa KÖRNINGSHISTORIK**, visas en lista över alla körs:

![Exempel kör historik][runhistory]

Klicka på någon av dessa körs tooview en ögonblicksbild av hello experimentera tidpunkt hello du körde den. hello konfiguration, parametervärden, kommentarer och resultat är alla bevarad toogive du en fullständig post för att körningen av experimentet.

> [!TIP]
> toodocument din iterationer av experiment hello, kan du ändra hello rubrik varje gång den körs, kan du uppdatera hello **sammanfattning** hello experiment i hello egenskaper, och du kan lägga till eller uppdatera kommentarer om enskilda moduler toorecord dina ändringar. hello rubrik, Sammanfattning och modulen kommentarer sparas med varje körning av hello experiment.
> 
> 

hello lista över experiment i hello **EXPERIMENT** i Machine Learning Studio alltid visar hello senaste versionen av ett experiment. Om du öppnar en tidigare körning av hello experiment (med hjälp av **tidigare kör** eller **visa KÖRNINGSHISTORIK**), kan du returnera toohello utkast genom att klicka på **visa KÖRNINGSHISTORIK** och välja Hej iteration som har en **tillstånd** av **redigeras**.

## <a name="iterating-on-a-previous-run"></a>Iteration av en tidigare körning
När du klickar på **tidigare kör** eller **visa KÖRNINGSHISTORIK** och öppna en tidigare körning kan du visa en klar experiment i skrivskyddat läge.

Om du vill toobegin en iterationer av experimentet från och med hello sätt som du har konfigurerat för en tidigare körning kan du göra detta genom att öppna hello köra och klicka **Spara som**. Detta skapar ett nytt experiment med ett nytt namn, en tom körningshistorik och alla hello komponenter och parametervärdena för hello tidigare kör. Nya experimentet finns med i hello **EXPERIMENTEN** fliken i hello Machine Learning Studio-startsidan, och du kan ändra och köra den initierar en ny kör historik för den här iterationer av experimentet. 

Anta att du har hello experiment kör historik som visas i hello föregående avsnitt. Du vill tooobserve vad som händer när du ställer in hello **inlärningsfrekvensen** too0.4 parametern och prova olika värden för hello **antalet utbildning epoker** parameter.

1. Klicka på **visa KÖRNINGSHISTORIK** och öppna hello iteration av hello experiment som du körde klockan 4:28:36 (där du anger hello parametern värdet too0.4).
2. Klicka på **Spara som**.
3. Ange ett nytt namn och klicka på hello **OK** markering. En ny kopia av hello experiment skapas.
4. Ändra hello **antalet utbildning epoker** parameter.
5. Klicka på **kör**.

Du kan nu fortsätta toomodify och kör den här versionen av experimentet, skapa en ny körningshistorik toorecord ditt arbete.

<!-- Images -->
[runhistory]:./media/machine-learning-manage-experiment-iterations/viewrunhistory.jpg


<!-- Module References -->
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
