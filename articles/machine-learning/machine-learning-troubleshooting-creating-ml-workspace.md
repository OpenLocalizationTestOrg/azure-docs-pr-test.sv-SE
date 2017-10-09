---
title: "Felsöka: Skapa och ansluta tooa Machine Learning-arbetsytan | Microsoft Docs"
description: "Lösningar på vanliga problem med att skapa och ansluta tooan Azure Machine Learning-arbetsytan"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 1a8aec4b-35f9-44e8-9570-2575b8979ab1
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye
ms.openlocfilehash: 965a0025e85ba4e22c2b037edfa923e7f7599069
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-guide-create-and-connect-tooan-machine-learning-workspace"></a>Felsökningsguiden: skapa och ansluta tooan Machine Learning-arbetsytan
Den här guiden ger lösningar för några vanliga påträffade utmaningar när du ställer in Azure Machine Learning arbetsytor.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="workspace-owner"></a>Arbetsytan ägare
tooopen en arbetsyta i Machine Learning Studio, måste du vara inloggad toohello Account du använde toocreate hello arbetsytan eller måste tooreceive en inbjudan från hello ägare toojoin hello arbetsytan. Du kan hantera hello arbetsytan som innehåller hello möjlighet tooconfigure åtkomst från hello Azure-portalen.

Mer information om hur du hanterar en arbetsyta finns [hantera en Azure Machine Learning-arbetsytan].

[hantera en Azure Machine Learning-arbetsytan]: machine-learning-manage-workspace.md

## <a name="allowed-regions"></a>Tillåtna regioner
Machine Learning är tillgänglig i ett begränsat antal regioner. Om din prenumeration inte innehåller något av dessa regioner kan du se hello felmeddelande, ”du har inga prenumerationer i hello tillåtna regioner”.

toorequest som en region läggs tooyour prenumerationen, skapa en ny Microsoft supportförfrågan från hello Azure portal, väljer **fakturering** som hello problemtyp och följ hello uppmanar toosubmit din begäran.

## <a name="storage-account"></a>Lagringskonto
hello tjänsten Machine Learning måste en toostore kontodata lagring. Du kan använda ett befintligt lagringskonto eller skapa ett nytt lagringskonto när du skapar hello nya Machine Learning-arbetsytan (om du har kvoten toocreate ett nytt lagringskonto).

När hello nya Machine Learning-arbetsytan har skapats kan logga du in tooMachine Learning Studio med hjälp av hello Microsoft-konto du använde toocreate hello arbetsytan. Om det uppstår fel hälsningsmeddelande Använd ”arbetsytan kunde inte hittas” (liknande toohello följande skärmbild), följande steg toodelete hello dina cookies.

![Det gick inte att hitta arbetsytan][screen3]

**toodelete cookies i webbläsaren**

1. Om du använder Internet Explorer klickar du på hello **verktyg** i hello övre högra hörnet och välj **Internetalternativ**.  

![Internet-alternativ][screen4]

2. Under hello **allmänna** klickar du på **ta bort...**

![Fliken Allmänt][screen5]

3. I hello **ta bort webbhistorik** dialogrutan Kontrollera **Cookies och webbplatsen data** är markerad och klicka på **ta bort**.

![Ta bort cookies][screen6]

Starta om hello webbläsaren efter hello cookies bort, och sedan gå toohello [Microsoft Azure Machine Learning](https://studio.azureml.net) sidan. När du uppmanas ange ett användarnamn och lösenord, hello samma Microsoft-konto som du använde toocreate hello arbetsytan.

## <a name="comments"></a>Kommentarer

Vårt mål är toomake hello Machine Learning-upplevelse som sömlös som möjligt. Kontrollera efter eventuella kommentarer och problem vid hello [Azure Machine Learning-forum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) toohelp oss fungerar du bättre.

[screen1]:media/machine-learning-troubleshooting-creating-ml-workspace/screen1.png
[screen2]:media/machine-learning-troubleshooting-creating-ml-workspace/screen2.png
[screen3]:media/machine-learning-troubleshooting-creating-ml-workspace/screen3.png
[screen4]:media/machine-learning-troubleshooting-creating-ml-workspace/screen4.png
[screen5]:media/machine-learning-troubleshooting-creating-ml-workspace/screen5.png
[screen6]:media/machine-learning-troubleshooting-creating-ml-workspace/screen6.png
