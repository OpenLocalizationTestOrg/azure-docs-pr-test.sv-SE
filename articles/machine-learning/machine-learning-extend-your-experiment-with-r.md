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
# <a name="extend-your-experiment-with-r"></a><span data-ttu-id="7a5d5-103">Utöka experimentet med R</span><span class="sxs-lookup"><span data-stu-id="7a5d5-103">Extend your experiment with R</span></span>
<span data-ttu-id="7a5d5-104">Du kan utöka hello funktioner i Azure Machine Learning Studio via hello R språk med hjälp av hello [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="7a5d5-104">You can extend hello functionality of Azure Machine Learning Studio through hello R language by using hello [Execute R Script][execute-r-script] module.</span></span>

<span data-ttu-id="7a5d5-105">Den här modulen accepterar flera indatauppsättningar och ger en enda dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="7a5d5-105">This module accepts multiple input datasets and yields a single dataset as output.</span></span> <span data-ttu-id="7a5d5-106">Du kan skriva ett R-skript i hello **R-skriptet** parametern för hello [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="7a5d5-106">You can type an R script into hello **R Script** parameter of hello [Execute R Script][execute-r-script] module.</span></span>

<span data-ttu-id="7a5d5-107">Du har åtkomst till varje indataport för hello modulen med kod liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="7a5d5-107">You access each input port of hello module by using code similar toohello following:</span></span>

    dataset1 <- maml.mapInputPort(1)

## <a name="listing-all-currently-installed-packages"></a><span data-ttu-id="7a5d5-108">Visar en lista över alla installerade paket</span><span class="sxs-lookup"><span data-stu-id="7a5d5-108">Listing all currently-installed packages</span></span>
<span data-ttu-id="7a5d5-109">hello listan över installerade paket kan ändra.</span><span class="sxs-lookup"><span data-stu-id="7a5d5-109">hello list of installed packages can change.</span></span> <span data-ttu-id="7a5d5-110">En lista över installerade paket finns i [R-paket som stöds av Azure Machine Learning](https://msdn.microsoft.com/library/azure/mt741980.aspx).</span><span class="sxs-lookup"><span data-stu-id="7a5d5-110">A list of currently installed packages can be found in [R Packages Supported by Azure Machine Learning](https://msdn.microsoft.com/library/azure/mt741980.aspx).</span></span>

<span data-ttu-id="7a5d5-111">Du kan också få hello fullständig, aktuell listan över installerade paket genom att ange följande kod i hello hello [köra R-skriptet] [ execute-r-script] modulen:</span><span class="sxs-lookup"><span data-stu-id="7a5d5-111">You also can get hello complete, current list of installed packages by entering hello following code into hello [Execute R Script][execute-r-script] module:</span></span>

    out <- data.frame(installed.packages(,,,fields="Description"))
    maml.mapOutputPort("out")

<span data-ttu-id="7a5d5-112">Detta skickar hello listan över paket toohello utdataporten för hello [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="7a5d5-112">This sends hello list of packages toohello output port of hello [Execute R Script][execute-r-script] module.</span></span>
<span data-ttu-id="7a5d5-113">tooview hello paketet och ansluta en konvertering modul som [konvertera tooCSV] [ convert-to-csv] toohello kvar utdata från hello [köra R-skriptet] [ execute-r-script]modul, kör hello experiment, och klicka sedan på hello utdata från hello konverteraren och välj **hämta**.</span><span class="sxs-lookup"><span data-stu-id="7a5d5-113">tooview hello package list, connect a conversion module such as [Convert tooCSV][convert-to-csv] toohello left output of hello [Execute R Script][execute-r-script] module, run hello experiment, then click hello output of hello conversion module and select **Download**.</span></span> 

![Hämta utdata från modulen ”konvertera tooCSV”](./media/machine-learning-extend-your-experiment-with-r/download-package-list.png)


<!--
For convenience, here is hello [current full list with version numbers in Excel format](http://az754797.vo.msecnd.net/docs/RPackages.xlsx).
-->

## <a name="importing-packages"></a><span data-ttu-id="7a5d5-115">Importera paket</span><span class="sxs-lookup"><span data-stu-id="7a5d5-115">Importing packages</span></span>
<span data-ttu-id="7a5d5-116">Du kan importera paket som inte redan är installerade med hjälp av följande kommandon i hello hello [köra R-skriptet] [ execute-r-script] modulen:</span><span class="sxs-lookup"><span data-stu-id="7a5d5-116">You can import packages that are not already installed by using hello following commands in hello [Execute R Script][execute-r-script] module:</span></span>

    install.packages("src/my_favorite_package.zip", lib = ".", repos = NULL, verbose = TRUE)
    success <- library("my_favorite_package", lib.loc = ".", logical.return = TRUE, verbose = TRUE)

<span data-ttu-id="7a5d5-117">Hej där `my_favorite_package.zip` filen innehåller paketet.</span><span class="sxs-lookup"><span data-stu-id="7a5d5-117">where hello `my_favorite_package.zip` file contains your package.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[convert-to-csv]: https://msdn.microsoft.com/library/azure/faa6ba63-383c-4086-ba58-7abf26b85814/
