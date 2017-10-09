---
title: "aaaDevelop U-SQL-sammansättningar för Azure Data Lake Analytics-jobb | Microsoft Docs"
description: "Lär dig hur toodevelop sammansättningar toobe används och återanvändas i Data Lake Analytics-jobb. "
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
ms.openlocfilehash: 86dd17b25e0967306ed36bb5b7f3178d9409d53d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="develop-u-sql-assemblies-for-azure-data-lake-analytics-jobs"></a><span data-ttu-id="4ada2-103">Utveckla U-SQL-sammansättningar för Azure Data Lake Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="4ada2-103">Develop U-SQL assemblies for Azure Data Lake Analytics jobs</span></span>
<span data-ttu-id="4ada2-104">Lär dig hur tooturn bakomliggande kod till sammansättningar toobe används och återanvändas i Data Lake Analytics-jobb.</span><span class="sxs-lookup"><span data-stu-id="4ada2-104">Learn how tooturn code-behind into assemblies toobe used and reused in Data Lake Analytics jobs.</span></span> 

<span data-ttu-id="4ada2-105">U-SQL gör det enkelt tooadd dina egna anpassade kod i .net-språk, till exempel C#, VB.Net eller F #.</span><span class="sxs-lookup"><span data-stu-id="4ada2-105">U-SQL makes it easy tooadd your own custom code in .Net languages, such as C#, VB.Net or F#.</span></span> <span data-ttu-id="4ada2-106">Du kan även distribuera din egen runtime toosupport andra språk.</span><span class="sxs-lookup"><span data-stu-id="4ada2-106">You can even deploy your own runtime toosupport other languages.</span></span>

<span data-ttu-id="4ada2-107">hello enklaste sättet toouse anpassad kod är toouse hello Data Lake-verktyg för Visual Studio bakomliggande kod funktioner.</span><span class="sxs-lookup"><span data-stu-id="4ada2-107">hello easiest way toouse custom code is toouse hello Data Lake Tools for Visual Studio’s code-behind capabilities.</span></span> <span data-ttu-id="4ada2-108">Mer information finns i [Självstudier: utveckla U-SQL-skript med hjälp av Data Lake-verktyg för Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="4ada2-108">For more information, see [Tutorial: develop U-SQL scripts using Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span> <span data-ttu-id="4ada2-109">Det finns några nackdelar med bakomliggande kod:</span><span class="sxs-lookup"><span data-stu-id="4ada2-109">There are a few drawbacks of using code-behind:</span></span>

- <span data-ttu-id="4ada2-110">hello källkoden hämtar upp för varje skript.</span><span class="sxs-lookup"><span data-stu-id="4ada2-110">hello source code gets uploaded for every script submission.</span></span>
- <span data-ttu-id="4ada2-111">bakomliggande kod kan inte delas med andra jobb.</span><span class="sxs-lookup"><span data-stu-id="4ada2-111">code-behind cannot be shared with other jobs.</span></span>

<span data-ttu-id="4ada2-112">tooaddress dessa nackdelar du kan aktivera bakomliggande kod till sammansättningar och registrera hello sammansättningar toohello Data Lake Analytics-katalogen.</span><span class="sxs-lookup"><span data-stu-id="4ada2-112">tooaddress these drawbacks, you can turn code-behind into assemblies, and register hello assemblies toohello Data Lake Analytics catalog.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4ada2-113">Krav</span><span class="sxs-lookup"><span data-stu-id="4ada2-113">Prerequisites</span></span>
* <span data-ttu-id="4ada2-114">Visual Studio 2017, Visual Studio 2015, Visual Studio 2013 update 4 eller Visual Studio 2012 med Visual C++ installerat</span><span class="sxs-lookup"><span data-stu-id="4ada2-114">Visual Studio 2017, Visual Studio 2015, Visual Studio 2013 update 4, or Visual Studio 2012 with Visual C++ Installed</span></span>
* <span data-ttu-id="4ada2-115">Microsoft Azure SDK för .NET version 2.5 eller högre.</span><span class="sxs-lookup"><span data-stu-id="4ada2-115">Microsoft Azure SDK for .NET version 2.5 or above.</span></span>  <span data-ttu-id="4ada2-116">Installera den med hjälp av hello installationsprogram för webbplattform eller Visual Studio-installationsprogrammet</span><span class="sxs-lookup"><span data-stu-id="4ada2-116">Install it using hello Web platform installer or Visual Studio Installer</span></span>
* <span data-ttu-id="4ada2-117">Ett Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="4ada2-117">A Data Lake Analytics account.</span></span>  <span data-ttu-id="4ada2-118">Se [Kom igång med Azure Data Lake Analytics med hjälp av Azure Portal](data-lake-analytics-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="4ada2-118">See [Get Started with Azure Data Lake Analytics using Azure portal](data-lake-analytics-get-started-portal.md).</span></span>
* <span data-ttu-id="4ada2-119">Gå igenom hello [Kom igång med Azure Data Lake Analytics U-SQL Studio](data-lake-analytics-u-sql-get-started.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="4ada2-119">Go through hello [Get started with Azure Data Lake Analytics U-SQL Studio](data-lake-analytics-u-sql-get-started.md) tutorial.</span></span>
* <span data-ttu-id="4ada2-120">Ansluta tooAzure.</span><span class="sxs-lookup"><span data-stu-id="4ada2-120">Connect tooAzure.</span></span>
* <span data-ttu-id="4ada2-121">Överför hello källdata, se [Kom igång med Azure Data Lake Analytics U-SQL Studio](data-lake-analytics-u-sql-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="4ada2-121">Upload hello source data, see [Get started with Azure Data Lake Analytics U-SQL Studio](data-lake-analytics-u-sql-get-started.md).</span></span> 

## <a name="develop-assemblies-for-u-sql"></a><span data-ttu-id="4ada2-122">Utveckla sammansättningar för U-SQL</span><span class="sxs-lookup"><span data-stu-id="4ada2-122">Develop assemblies for U-SQL</span></span>

<span data-ttu-id="4ada2-123">**toocreate och skicka ett U-SQL-jobb**</span><span class="sxs-lookup"><span data-stu-id="4ada2-123">**toocreate and submit a U-SQL job**</span></span>

1. <span data-ttu-id="4ada2-124">Från hello **filen** -menyn klickar du på **ny**, och klicka sedan på **projekt**.</span><span class="sxs-lookup"><span data-stu-id="4ada2-124">From hello **File** menu, click **New**, and then click **Project**.</span></span>
2. <span data-ttu-id="4ada2-125">Expandera **installerad**, **mallar**, **Azure Data Lake**, **U-SQL(ADLA)**väljer hello **Class Library (för U-SQL Program)** mallen och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="4ada2-125">Expand **Installed**, **Templates**, **Azure Data Lake**, **U-SQL(ADLA)**, select hello **Class Library (For U-SQL Application)** template, and then click **OK**.</span></span>
3. <span data-ttu-id="4ada2-126">Skriv koden i Class1.cs.</span><span class="sxs-lookup"><span data-stu-id="4ada2-126">Write your code in Class1.cs.</span></span>  <span data-ttu-id="4ada2-127">hello följande är ett kodexempel.</span><span class="sxs-lookup"><span data-stu-id="4ada2-127">hello following is a code sample.</span></span>

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
4. <span data-ttu-id="4ada2-128">Klicka på hello **skapa** -menyn och klicka sedan på **skapa lösning** toocreate hello dll.</span><span class="sxs-lookup"><span data-stu-id="4ada2-128">Click hello **Build** menu, and then click **Build Solution** toocreate hello dll.</span></span>

## <a name="register-assemblies"></a><span data-ttu-id="4ada2-129">Registrera sammansättningar</span><span class="sxs-lookup"><span data-stu-id="4ada2-129">Register assemblies</span></span>

<span data-ttu-id="4ada2-130">Se [Använd Data Lake Analytics(U-SQL) katalogen](data-lake-analytics-use-u-sql-catalog.md).</span><span class="sxs-lookup"><span data-stu-id="4ada2-130">See [Use Data Lake Analytics(U-SQL) catalog](data-lake-analytics-use-u-sql-catalog.md).</span></span>


## <a name="use-hello-assemblies"></a><span data-ttu-id="4ada2-131">Använd hello sammansättningar</span><span class="sxs-lookup"><span data-stu-id="4ada2-131">Use hello assemblies</span></span>

<span data-ttu-id="4ada2-132">Se [använder hello Azure Data Lake-verktyg för Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).</span><span class="sxs-lookup"><span data-stu-id="4ada2-132">See [Use hello Azure Data Lake Tools for Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="4ada2-133">Se även</span><span class="sxs-lookup"><span data-stu-id="4ada2-133">See also</span></span>
* [<span data-ttu-id="4ada2-134">Kom igång med Data Lake Analytics med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="4ada2-134">Get started with Data Lake Analytics using PowerShell</span></span>](data-lake-analytics-get-started-powershell.md)
* [<span data-ttu-id="4ada2-135">Kom igång med Data Lake Analytics med hjälp av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="4ada2-135">Get started with Data Lake Analytics using hello Azure portal</span></span>](data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="4ada2-136">Använd Data Lake-verktyg för Visual Studio för att utveckla U-SQL-program</span><span class="sxs-lookup"><span data-stu-id="4ada2-136">Use Data Lake Tools for Visual Studio for developing U-SQL applications</span></span>](data-lake-analytics-data-lake-tools-get-started.md)
* [<span data-ttu-id="4ada2-137">Använd Data Lake Analytics(U-SQL) katalog</span><span class="sxs-lookup"><span data-stu-id="4ada2-137">Use Data Lake Analytics(U-SQL) catalog</span></span>](data-lake-analytics-use-u-sql-catalog.md)
* [<span data-ttu-id="4ada2-138">Använd hello Azure Data Lake-verktyg för Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4ada2-138">Use hello Azure Data Lake Tools for Visual Studio Code</span></span>](data-lake-analytics-data-lake-tools-for-vscode.md)
