---
title: "aaaImport data till Machine Learning Studio från en annan experiment | Microsoft Docs"
description: "Hur toosave utbildningsdata i Azure Machine Learning Studio och använda den i ett annat experiment."
keywords: "Importera data, data, datakällor, träningsdata"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 7da9dcec-5693-4bb6-8166-15904e7f75c3
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye;bradsev
ms.openlocfilehash: ca085149ed23c219a791ce09ac48dafeb807cb88
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="import-your-data-into-azure-machine-learning-studio-from-another-experiment"></a>Importera data till Azure Machine Learning Studio från ett annat experiment
[!INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]

Det ska finnas tillfällen när du vill tootake ett mellanliggande resultat från en experiment och använda den som en del av ett annat experiment. toodo kan du spara hello modulen som en datamängd:

1. Klicka på hello utdata från hello-modul som du vill toosave som en datamängd.
2. Klicka på **Spara som Dataset**.
3. När du uppmanas, anger du ett namn och en beskrivning som gör att du tooidentify hello dataset enkelt.
4. Klicka på hello **OK** markering.

När hello sparar har avslutats, blir hello dataset tillgängliga för användning i alla experiment på arbetsytan. Du hittar i hello **sparade datauppsättningar** hello modulpaletten-listan.

