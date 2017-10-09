---
title: "aaaDevelop U-SQL-användardefinierade operatorer (UDO) | Microsoft Docs"
description: "Lär dig hur toodevelop användardefinierade operatorer toobe används och återanvändas i Data Lake Analytics-jobb. "
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: e5189e4e-9438-46d1-8686-ed4836bf3356
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: edmaca
ms.openlocfilehash: 6b86618efd3751cd9a5e91875879d7dd6d6a7b02
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="develop-u-sql-user-defined-operators-udos"></a><span data-ttu-id="8c4bf-103">Utveckla U-SQL-användardefinierade operatorer (UDO)</span><span class="sxs-lookup"><span data-stu-id="8c4bf-103">Develop U-SQL user-defined operators (UDOs)</span></span>
<span data-ttu-id="8c4bf-104">Lär dig hur toodevelop användardefinierade operatorer tooprocess data i ett U-SQL-jobb.</span><span class="sxs-lookup"><span data-stu-id="8c4bf-104">Learn how toodevelop user-defined operators tooprocess data in a U-SQL job.</span></span>

<span data-ttu-id="8c4bf-105">Instruktioner om hur du utvecklar allmänna sammansättningar för U-SQL finns i [utveckla U-SQL-sammansättningar för Azure Data Lake Analytics-jobb](data-lake-analytics-u-sql-develop-assemblies.md)</span><span class="sxs-lookup"><span data-stu-id="8c4bf-105">For instructions on developing general-purpose assemblies for U-SQL, see [Develop U-SQL assemblies for Azure Data Lake Analytics jobs](data-lake-analytics-u-sql-develop-assemblies.md)</span></span>

## <a name="define-and-use-a-user-defined-operator-in-u-sql"></a><span data-ttu-id="8c4bf-106">Definiera och använder en användardefinierad operator i U-SQL</span><span class="sxs-lookup"><span data-stu-id="8c4bf-106">Define and use a user-defined operator in U-SQL</span></span>
<span data-ttu-id="8c4bf-107">**toocreate och skicka ett U-SQL-jobb**</span><span class="sxs-lookup"><span data-stu-id="8c4bf-107">**toocreate and submit a U-SQL job**</span></span>

1. <span data-ttu-id="8c4bf-108">Hello Visual Studio väljer **Arkiv > Nytt > Projekt > U-SQL-projekt**.</span><span class="sxs-lookup"><span data-stu-id="8c4bf-108">From hello Visual Studio select **File > New > Project > U-SQL Project**.</span></span>
2. <span data-ttu-id="8c4bf-109">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="8c4bf-109">Click **OK**.</span></span> <span data-ttu-id="8c4bf-110">Visual Studio skapar en lösning med en Script.usql-fil.</span><span class="sxs-lookup"><span data-stu-id="8c4bf-110">Visual Studio creates a solution with a Script.usql file.</span></span>
3. <span data-ttu-id="8c4bf-111">Från **Solution Explorer**, expandera Script.usql och dubbelklicka sedan på **Script.usql.cs**.</span><span class="sxs-lookup"><span data-stu-id="8c4bf-111">From **Solution Explorer**, expand Script.usql, and then double-click **Script.usql.cs**.</span></span>
4. <span data-ttu-id="8c4bf-112">Klistra in följande kod i filen hello hello:</span><span class="sxs-lookup"><span data-stu-id="8c4bf-112">Paste hello following code into hello file:</span></span>

        using Microsoft.Analytics.Interfaces;
        using System.Collections.Generic;

        namespace USQL_UDO
        {
            public class CountryName : IProcessor
            {
                private static IDictionary<string, string> CountryTranslation = new Dictionary<string, string>
                {
                    {
                        "Deutschland", "Germany"
                    },
                    {
                        "Suisse", "Switzerland"
                    },
                    {
                        "UK", "United Kingdom"
                    },
                    {
                        "USA", "United States of America"
                    },
                    {
                        "中国", "PR China"
                    }
                };

                public override IRow Process(IRow input, IUpdatableRow output)
                {

                    string UserID = input.Get<string>("UserID");
                    string Name = input.Get<string>("Name");
                    string Address = input.Get<string>("Address");
                    string City = input.Get<string>("City");
                    string State = input.Get<string>("State");
                    string PostalCode = input.Get<string>("PostalCode");
                    string Country = input.Get<string>("Country");
                    string Phone = input.Get<string>("Phone");

                    if (CountryTranslation.Keys.Contains(Country))
                    {
                        Country = CountryTranslation[Country];
                    }
                    output.Set<string>(0, UserID);
                    output.Set<string>(1, Name);
                    output.Set<string>(2, Address);
                    output.Set<string>(3, City);
                    output.Set<string>(4, State);
                    output.Set<string>(5, PostalCode);
                    output.Set<string>(6, Country);
                    output.Set<string>(7, Phone);

                    return output.AsReadOnly();
                }
            }
        }
6. <span data-ttu-id="8c4bf-113">Öppna **Script.usql**, och klistra in hello följande U-SQL-skript:</span><span class="sxs-lookup"><span data-stu-id="8c4bf-113">Open **Script.usql**, and paste hello following U-SQL script:</span></span>

        @drivers =
            EXTRACT UserID      string,
                    Name        string,
                    Address     string,
                    City        string,
                    State       string,
                    PostalCode  string,
                    Country     string,
                    Phone       string
            FROM "/Samples/Data/AmbulanceData/Drivers.txt"
            USING Extractors.Tsv(Encoding.Unicode);

        @drivers_CountryName =
            PROCESS @drivers
            PRODUCE UserID string,
                    Name string,
                    Address string,
                    City string,
                    State string,
                    PostalCode string,
                    Country string,
                    Phone string
            USING new USQL_UDO.CountryName();    

        OUTPUT @drivers_CountryName
            too"/Samples/Outputs/Drivers.csv"
            USING Outputters.Csv(Encoding.Unicode);
7. <span data-ttu-id="8c4bf-114">Ange hello Data Lake Analytics-kontot, databas och Schema.</span><span class="sxs-lookup"><span data-stu-id="8c4bf-114">Specify hello Data Lake Analytics account, Database, and Schema.</span></span>
8. <span data-ttu-id="8c4bf-115">Från **Solution Explorer**, högerklicka på **Script.usql** och klicka sedan på **Skapa skript**.</span><span class="sxs-lookup"><span data-stu-id="8c4bf-115">From **Solution Explorer**, right-click **Script.usql**, and then click **Build Script**.</span></span>
9. <span data-ttu-id="8c4bf-116">Från **Solution Explorer**, högerklicka på **Script.usql** och klicka sedan på **Skicka skript**.</span><span class="sxs-lookup"><span data-stu-id="8c4bf-116">From **Solution Explorer**, right-click **Script.usql**, and then click **Submit Script**.</span></span>
10. <span data-ttu-id="8c4bf-117">Om du inte har anslutit tooyour Azure-prenumeration, kommer du att ange tooenter dina Azure-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="8c4bf-117">If you haven't connected tooyour Azure subscription, you will be prompted tooenter your Azure account credentials.</span></span>
11. <span data-ttu-id="8c4bf-118">Klicka på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="8c4bf-118">Click **Submit**.</span></span> <span data-ttu-id="8c4bf-119">Resultat för skicka och jobblänk är tillgängliga i resultatfönstret hello när hello överföringen är klar.</span><span class="sxs-lookup"><span data-stu-id="8c4bf-119">Submission results and job link are available in hello Results window when hello submission is completed.</span></span>
12. <span data-ttu-id="8c4bf-120">Klicka på hello **uppdatera** knappen toosee hello senaste jobb status och uppdatera hello-skärmen.</span><span class="sxs-lookup"><span data-stu-id="8c4bf-120">Click hello **Refresh** button toosee hello latest job status and refresh hello screen.</span></span>

<span data-ttu-id="8c4bf-121">**toosee hello utdata**</span><span class="sxs-lookup"><span data-stu-id="8c4bf-121">**toosee hello output**</span></span>

1. <span data-ttu-id="8c4bf-122">Från **Server Explorer**, expandera **Azure**, expandera **Datasjöanalys**, expandera Data Lake Analytics-kontot, expandera **Lagringskonton**högerklickar du på hello standard lagring och klicka sedan på **Explorer**.</span><span class="sxs-lookup"><span data-stu-id="8c4bf-122">From **Server Explorer**, expand **Azure**, expand **Data Lake Analytics**, expand your Data Lake Analytics account, expand **Storage Accounts**, right-click hello Default Storage, and then click **Explorer**.</span></span>
2. <span data-ttu-id="8c4bf-123">Expandera exempel utdata, och dubbelklicka sedan på **Drivers.csv**.</span><span class="sxs-lookup"><span data-stu-id="8c4bf-123">Expand Samples, expand Outputs, and then double-click **Drivers.csv**.</span></span>

## <a name="see-also"></a><span data-ttu-id="8c4bf-124">Se även</span><span class="sxs-lookup"><span data-stu-id="8c4bf-124">See also</span></span>
* [<span data-ttu-id="8c4bf-125">Kom igång med Data Lake Analytics med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="8c4bf-125">Get started with Data Lake Analytics using PowerShell</span></span>](data-lake-analytics-get-started-powershell.md)
* [<span data-ttu-id="8c4bf-126">Kom igång med Data Lake Analytics med hjälp av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="8c4bf-126">Get started with Data Lake Analytics using hello Azure portal</span></span>](data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="8c4bf-127">Använd Data Lake-verktyg för Visual Studio för att utveckla U-SQL-program</span><span class="sxs-lookup"><span data-stu-id="8c4bf-127">Use Data Lake Tools for Visual Studio for developing U-SQL applications</span></span>](data-lake-analytics-data-lake-tools-get-started.md)
