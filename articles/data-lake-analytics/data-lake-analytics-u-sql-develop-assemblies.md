---
title: "Utveckla U-SQL-sammansättningar för Azure Data Lake Analytics-jobb | Microsoft Docs"
description: "Lär dig hur du utvecklar sammansättningar som ska användas och återanvändas i Data Lake Analytics-jobb. "
services: data-lake-analytics
documentationcenter: 
author: jejiang
manager: jhubbard
editor: cgronlun
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/30/2016
ms.author: jejiang
ms.openlocfilehash: c49f80f8dcd330d7f46726241e7178351b9cc28f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="develop-u-sql-assemblies-for-azure-data-lake-analytics-jobs"></a><span data-ttu-id="34c6f-103">Utveckla U-SQL-sammansättningar för Azure Data Lake Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="34c6f-103">Develop U-SQL assemblies for Azure Data Lake Analytics jobs</span></span>
<span data-ttu-id="34c6f-104">Lär dig hur du aktiverar bakomliggande kod i sammansättningar som används och återanvändas i Data Lake Analytics-jobb.</span><span class="sxs-lookup"><span data-stu-id="34c6f-104">Learn how to turn code-behind into assemblies to be used and reused in Data Lake Analytics jobs.</span></span> 

<span data-ttu-id="34c6f-105">U-SQL gör det lätt att lägga till din egen kod i .net-språk, till exempel C#, VB.Net eller F #.</span><span class="sxs-lookup"><span data-stu-id="34c6f-105">U-SQL makes it easy to add your own custom code in .Net languages, such as C#, VB.Net or F#.</span></span> <span data-ttu-id="34c6f-106">Du kan även distribuera din egen runtime för att stödja andra språk.</span><span class="sxs-lookup"><span data-stu-id="34c6f-106">You can even deploy your own runtime to support other languages.</span></span>

<span data-ttu-id="34c6f-107">Det enklaste sättet att använda anpassade kod är att använda Data Lake-verktyg för Visual Studio bakomliggande kod funktioner.</span><span class="sxs-lookup"><span data-stu-id="34c6f-107">The easiest way to use custom code is to use the Data Lake Tools for Visual Studio’s code-behind capabilities.</span></span> <span data-ttu-id="34c6f-108">Mer information finns i [Självstudier: utveckla U-SQL-skript med hjälp av Data Lake-verktyg för Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="34c6f-108">For more information, see [Tutorial: develop U-SQL scripts using Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span> <span data-ttu-id="34c6f-109">Det finns några nackdelar med bakomliggande kod:</span><span class="sxs-lookup"><span data-stu-id="34c6f-109">There are a few drawbacks of using code-behind:</span></span>

- <span data-ttu-id="34c6f-110">Källkoden hämtar upp för varje skript.</span><span class="sxs-lookup"><span data-stu-id="34c6f-110">The source code gets uploaded for every script submission.</span></span>
- <span data-ttu-id="34c6f-111">bakomliggande kod kan inte delas med andra jobb.</span><span class="sxs-lookup"><span data-stu-id="34c6f-111">code-behind cannot be shared with other jobs.</span></span>

<span data-ttu-id="34c6f-112">Du kan aktivera bakomliggande kod till sammansättningar och registrera sammansättningarna i Data Lake Analytics-katalogen för att åtgärda dessa nackdelar.</span><span class="sxs-lookup"><span data-stu-id="34c6f-112">To address these drawbacks, you can turn code-behind into assemblies, and register the assemblies to the Data Lake Analytics catalog.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="34c6f-113">Krav</span><span class="sxs-lookup"><span data-stu-id="34c6f-113">Prerequisites</span></span>
* <span data-ttu-id="34c6f-114">Visual Studio 2017, Visual Studio 2015, Visual Studio 2013 update 4 eller Visual Studio 2012 med Visual C++ installerat</span><span class="sxs-lookup"><span data-stu-id="34c6f-114">Visual Studio 2017, Visual Studio 2015, Visual Studio 2013 update 4, or Visual Studio 2012 with Visual C++ Installed</span></span>
* <span data-ttu-id="34c6f-115">Microsoft Azure SDK för .NET version 2.5 eller högre.</span><span class="sxs-lookup"><span data-stu-id="34c6f-115">Microsoft Azure SDK for .NET version 2.5 or above.</span></span>  <span data-ttu-id="34c6f-116">Installera den med hjälp av installationsprogram för webbplattform eller Visual Studio-installationsprogrammet</span><span class="sxs-lookup"><span data-stu-id="34c6f-116">Install it using the Web platform installer or Visual Studio Installer</span></span>
* <span data-ttu-id="34c6f-117">Ett Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="34c6f-117">A Data Lake Analytics account.</span></span>  <span data-ttu-id="34c6f-118">Se [Kom igång med Azure Data Lake Analytics med hjälp av Azure Portal](data-lake-analytics-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="34c6f-118">See [Get Started with Azure Data Lake Analytics using Azure portal](data-lake-analytics-get-started-portal.md).</span></span>
* <span data-ttu-id="34c6f-119">Gå igenom den [Kom igång med Azure Data Lake Analytics U-SQL Studio](data-lake-analytics-u-sql-get-started.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="34c6f-119">Go through the [Get started with Azure Data Lake Analytics U-SQL Studio](data-lake-analytics-u-sql-get-started.md) tutorial.</span></span>
* <span data-ttu-id="34c6f-120">Ansluta till Azure.</span><span class="sxs-lookup"><span data-stu-id="34c6f-120">Connect to Azure.</span></span>
* <span data-ttu-id="34c6f-121">Överför källdata, se [Kom igång med Azure Data Lake Analytics U-SQL Studio](data-lake-analytics-u-sql-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="34c6f-121">Upload the source data, see [Get started with Azure Data Lake Analytics U-SQL Studio](data-lake-analytics-u-sql-get-started.md).</span></span> 

## <a name="develop-assemblies-for-u-sql"></a><span data-ttu-id="34c6f-122">Utveckla sammansättningar för U-SQL</span><span class="sxs-lookup"><span data-stu-id="34c6f-122">Develop assemblies for U-SQL</span></span>

<span data-ttu-id="34c6f-123">**Skapa och skicka ett U-SQL-jobb**</span><span class="sxs-lookup"><span data-stu-id="34c6f-123">**To create and submit a U-SQL job**</span></span>

1. <span data-ttu-id="34c6f-124">Klicka på **Nytt** i **Arkiv**-menyn och klicka sedan på **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="34c6f-124">From the **File** menu, click **New**, and then click **Project**.</span></span>
2. <span data-ttu-id="34c6f-125">Expandera **installerad**, **mallar**, **Azure Data Lake**, **U-SQL(ADLA)**, Välj den **Class Library (för U-SQL Program)** mallen och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="34c6f-125">Expand **Installed**, **Templates**, **Azure Data Lake**, **U-SQL(ADLA)**, select the **Class Library (For U-SQL Application)** template, and then click **OK**.</span></span>
3. <span data-ttu-id="34c6f-126">Skriv koden i Class1.cs.</span><span class="sxs-lookup"><span data-stu-id="34c6f-126">Write your code in Class1.cs.</span></span>  <span data-ttu-id="34c6f-127">Följande är ett kodexempel.</span><span class="sxs-lookup"><span data-stu-id="34c6f-127">The following is a code sample.</span></span>

        using Microsoft.Analytics.Interfaces;

        namespace USQLApplication_codebehind
        {
            [SqlUserDefinedProcessor]
            public class MyProcessor : IProcessor
            {
                public override IRow Process(IRow input, IUpdatableRow output)
                {
                    output.Set(0, input.Get<string>(0));
                    output.Set(0, input.Get<string>(0));
                    return output.AsReadOnly();
                }
            }
        }
4. <span data-ttu-id="34c6f-128">Klicka på den **skapa** -menyn och klicka sedan på **skapa lösning** att skapa DLL-filen.</span><span class="sxs-lookup"><span data-stu-id="34c6f-128">Click the **Build** menu, and then click **Build Solution** to create the dll.</span></span>

## <a name="register-assemblies"></a><span data-ttu-id="34c6f-129">Registrera sammansättningar</span><span class="sxs-lookup"><span data-stu-id="34c6f-129">Register assemblies</span></span>

<span data-ttu-id="34c6f-130">Se [Använd Data Lake Analytics(U-SQL) katalogen](data-lake-analytics-use-u-sql-catalog.md).</span><span class="sxs-lookup"><span data-stu-id="34c6f-130">See [Use Data Lake Analytics(U-SQL) catalog](data-lake-analytics-use-u-sql-catalog.md).</span></span>


## <a name="use-the-assemblies"></a><span data-ttu-id="34c6f-131">Använda sammansättningarna</span><span class="sxs-lookup"><span data-stu-id="34c6f-131">Use the assemblies</span></span>

<span data-ttu-id="34c6f-132">Se [Använd Azure Data Lake-verktyg för Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).</span><span class="sxs-lookup"><span data-stu-id="34c6f-132">See [Use the Azure Data Lake Tools for Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="34c6f-133">Se även</span><span class="sxs-lookup"><span data-stu-id="34c6f-133">See also</span></span>
* [<span data-ttu-id="34c6f-134">Kom igång med Data Lake Analytics med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="34c6f-134">Get started with Data Lake Analytics using PowerShell</span></span>](data-lake-analytics-get-started-powershell.md)
* [<span data-ttu-id="34c6f-135">Kom igång med Data Lake Analytics med hjälp av Azure portal</span><span class="sxs-lookup"><span data-stu-id="34c6f-135">Get started with Data Lake Analytics using the Azure portal</span></span>](data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="34c6f-136">Använd Data Lake-verktyg för Visual Studio för att utveckla U-SQL-program</span><span class="sxs-lookup"><span data-stu-id="34c6f-136">Use Data Lake Tools for Visual Studio for developing U-SQL applications</span></span>](data-lake-analytics-data-lake-tools-get-started.md)
* [<span data-ttu-id="34c6f-137">Använd Data Lake Analytics(U-SQL) katalog</span><span class="sxs-lookup"><span data-stu-id="34c6f-137">Use Data Lake Analytics(U-SQL) catalog</span></span>](data-lake-analytics-use-u-sql-catalog.md)
* [<span data-ttu-id="34c6f-138">Använda Azure Data Lake Tools för Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="34c6f-138">Use the Azure Data Lake Tools for Visual Studio Code</span></span>](data-lake-analytics-data-lake-tools-for-vscode.md)