---
title: aaaExtend ditt experiment med R | Microsoft Docs
description: "Hur tooextend hello Azure Machine Learning Studio-funktion via hello R språk med hjälp av hello köra R-skriptet modulen."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 2c038a45-ba4d-42ea-9a88-e67391ef8c0a
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye
ms.openlocfilehash: 396489f26f367a744922af65e04f056c7afa1399
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="extend-your-experiment-with-r"></a>Utöka experimentet med R
Du kan utöka hello funktioner i Azure Machine Learning Studio via hello R språk med hjälp av hello [köra R-skriptet] [ execute-r-script] modul.

Den här modulen accepterar flera indatauppsättningar och ger en enda dataset som utdata. Du kan skriva ett R-skript i hello **R-skriptet** parametern för hello [köra R-skriptet] [ execute-r-script] modul.

Du har åtkomst till varje indataport för hello modulen med kod liknande toohello följande:

    dataset1 <- maml.mapInputPort(1)

## <a name="listing-all-currently-installed-packages"></a>Visar en lista över alla installerade paket
hello listan över installerade paket kan ändra. En lista över installerade paket finns i [R-paket som stöds av Azure Machine Learning](https://msdn.microsoft.com/library/azure/mt741980.aspx).

Du kan också få hello fullständig, aktuell listan över installerade paket genom att ange följande kod i hello hello [köra R-skriptet] [ execute-r-script] modulen:

    out <- data.frame(installed.packages(,,,fields="Description"))
    maml.mapOutputPort("out")

Detta skickar hello listan över paket toohello utdataporten för hello [köra R-skriptet] [ execute-r-script] modul.
tooview hello paketet och ansluta en konvertering modul som [konvertera tooCSV] [ convert-to-csv] toohello kvar utdata från hello [köra R-skriptet] [ execute-r-script]modul, kör hello experiment, och klicka sedan på hello utdata från hello konverteraren och välj **hämta**. 

![Hämta utdata från modulen ”konvertera tooCSV”](./media/machine-learning-extend-your-experiment-with-r/download-package-list.png)


<!--
For convenience, here is hello [current full list with version numbers in Excel format](http://az754797.vo.msecnd.net/docs/RPackages.xlsx).
-->

## <a name="importing-packages"></a>Importera paket
Du kan importera paket som inte redan är installerade med hjälp av följande kommandon i hello hello [köra R-skriptet] [ execute-r-script] modulen:

    install.packages("src/my_favorite_package.zip", lib = ".", repos = NULL, verbose = TRUE)
    success <- library("my_favorite_package", lib.loc = ".", logical.return = TRUE, verbose = TRUE)

Hej där `my_favorite_package.zip` filen innehåller paketet.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[convert-to-csv]: https://msdn.microsoft.com/library/azure/faa6ba63-383c-4086-ba58-7abf26b85814/
