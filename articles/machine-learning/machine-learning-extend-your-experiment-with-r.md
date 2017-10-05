---
title: "Utöka ditt experiment med R | Microsoft Docs"
description: "Så här utökar funktionerna i Azure Machine Learning Studio via R-språk med hjälp av modulen köra R-skriptet."
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
ms.openlocfilehash: fe207ef917980be8b554ad9c08176d108b19fb71
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="extend-your-experiment-with-r"></a><span data-ttu-id="46ce9-103">Utöka experimentet med R</span><span class="sxs-lookup"><span data-stu-id="46ce9-103">Extend your experiment with R</span></span>
<span data-ttu-id="46ce9-104">Du kan utöka funktionerna i Azure Machine Learning Studio via R-språk med hjälp av den [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="46ce9-104">You can extend the functionality of Azure Machine Learning Studio through the R language by using the [Execute R Script][execute-r-script] module.</span></span>

<span data-ttu-id="46ce9-105">Den här modulen accepterar flera indatauppsättningar och ger en enda dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="46ce9-105">This module accepts multiple input datasets and yields a single dataset as output.</span></span> <span data-ttu-id="46ce9-106">Du kan skriva ett R-skript i den **R-skriptet** parameter för den [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="46ce9-106">You can type an R script into the **R Script** parameter of the [Execute R Script][execute-r-script] module.</span></span>

<span data-ttu-id="46ce9-107">Du har åtkomst till varje inkommande port på modulen som med hjälp av kod som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="46ce9-107">You access each input port of the module by using code similar to the following:</span></span>

    dataset1 <- maml.mapInputPort(1)

## <a name="listing-all-currently-installed-packages"></a><span data-ttu-id="46ce9-108">Visar en lista över alla installerade paket</span><span class="sxs-lookup"><span data-stu-id="46ce9-108">Listing all currently-installed packages</span></span>
<span data-ttu-id="46ce9-109">Ändra listan över installerade paket.</span><span class="sxs-lookup"><span data-stu-id="46ce9-109">The list of installed packages can change.</span></span> <span data-ttu-id="46ce9-110">En lista över installerade paket finns i [R-paket som stöds av Azure Machine Learning](https://msdn.microsoft.com/library/azure/mt741980.aspx).</span><span class="sxs-lookup"><span data-stu-id="46ce9-110">A list of currently installed packages can be found in [R Packages Supported by Azure Machine Learning](https://msdn.microsoft.com/library/azure/mt741980.aspx).</span></span>

<span data-ttu-id="46ce9-111">Du kan också få fullständig, aktuell listan över installerade paket genom att skriva in följande kod i den [köra R-skriptet] [ execute-r-script] modulen:</span><span class="sxs-lookup"><span data-stu-id="46ce9-111">You also can get the complete, current list of installed packages by entering the following code into the [Execute R Script][execute-r-script] module:</span></span>

    out <- data.frame(installed.packages(,,,fields="Description"))
    maml.mapOutputPort("out")

<span data-ttu-id="46ce9-112">Listan över paket skickas till utdataporten för den [köra R-skriptet] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="46ce9-112">This sends the list of packages to the output port of the [Execute R Script][execute-r-script] module.</span></span>
<span data-ttu-id="46ce9-113">Om du vill visa paketlistan ansluta en konvertering modul som [konvertera till CSV] [ convert-to-csv] till vänstra utdataporten för den [köra R-skriptet] [ execute-r-script] modulen Kör experimentet, och sedan klicka på utdata från modulen konvertering och välj **hämta**.</span><span class="sxs-lookup"><span data-stu-id="46ce9-113">To view the package list, connect a conversion module such as [Convert to CSV][convert-to-csv] to the left output of the [Execute R Script][execute-r-script] module, run the experiment, then click the output of the conversion module and select **Download**.</span></span> 

![Hämta utdata från modulen ”konvertera till CSV”](./media/machine-learning-extend-your-experiment-with-r/download-package-list.png)


<!--
For convenience, here is the [current full list with version numbers in Excel format](http://az754797.vo.msecnd.net/docs/RPackages.xlsx).
-->

## <a name="importing-packages"></a><span data-ttu-id="46ce9-115">Importera paket</span><span class="sxs-lookup"><span data-stu-id="46ce9-115">Importing packages</span></span>
<span data-ttu-id="46ce9-116">Du kan importera paket som inte redan är installerade med hjälp av följande kommandon i den [köra R-skriptet] [ execute-r-script] modulen:</span><span class="sxs-lookup"><span data-stu-id="46ce9-116">You can import packages that are not already installed by using the following commands in the [Execute R Script][execute-r-script] module:</span></span>

    install.packages("src/my_favorite_package.zip", lib = ".", repos = NULL, verbose = TRUE)
    success <- library("my_favorite_package", lib.loc = ".", logical.return = TRUE, verbose = TRUE)

<span data-ttu-id="46ce9-117">där den `my_favorite_package.zip` filen innehåller paketet.</span><span class="sxs-lookup"><span data-stu-id="46ce9-117">where the `my_favorite_package.zip` file contains your package.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[convert-to-csv]: https://msdn.microsoft.com/library/azure/faa6ba63-383c-4086-ba58-7abf26b85814/
