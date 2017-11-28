---
title: "aaaTroubleshoot Azure Data Lake Analytics-jobb med hjälp av Azure Portal | Microsoft Docs"
description: "Lär dig hur toouse hello Azure Portal tootroubleshoot Data Lake Analytics-jobb. "
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: b7066d81-3142-474f-8a34-32b0b39656dc
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: edmaca
ms.openlocfilehash: e810d56bab8f1a8254721ec9906bb6a4508dc22a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-data-lake-analytics-jobs-using-azure-portal"></a><span data-ttu-id="d2f56-103">Felsöka Azure Data Lake Analytics-jobb med hjälp av Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d2f56-103">Troubleshoot Azure Data Lake Analytics jobs using Azure Portal</span></span>
<span data-ttu-id="d2f56-104">Lär dig hur toouse hello Azure Portal tootroubleshoot Data Lake Analytics-jobb.</span><span class="sxs-lookup"><span data-stu-id="d2f56-104">Learn how toouse hello Azure Portal tootroubleshoot Data Lake Analytics jobs.</span></span>

<span data-ttu-id="d2f56-105">I den här kursen ska du konfigurera saknas källa filen problem och använder hello Azure Portal tootroubleshoot hello problem.</span><span class="sxs-lookup"><span data-stu-id="d2f56-105">In this tutorial, you will setup a missing source file problem, and use hello Azure Portal tootroubleshoot hello problem.</span></span>

## <a name="submit-a-data-lake-analytics-job"></a><span data-ttu-id="d2f56-106">Skicka ett Data Lake Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="d2f56-106">Submit a Data Lake Analytics job</span></span>

<span data-ttu-id="d2f56-107">Skicka hello följande U-SQL-jobb:</span><span class="sxs-lookup"><span data-stu-id="d2f56-107">Submit hello following U-SQL job:</span></span>

```
@searchlog =
   EXTRACT UserId          int,
           Start           DateTime,
           Region          string,
           Query           string,
           Duration        int?,
           Urls            string,
           ClickedUrls     string
   FROM "/Samples/Data/SearchLog.tsv1"
   USING Extractors.Tsv();

OUTPUT @searchlog   
   too"/output/SearchLog-from-adls.csv"
   USING Outputters.Csv();
```
    
<span data-ttu-id="d2f56-108">hello käll-filen som definierats i hello skript är **/Samples/Data/SearchLog.tsv1**, där det ska vara **/Samples/Data/SearchLog.tsv**.</span><span class="sxs-lookup"><span data-stu-id="d2f56-108">hello source file defined in hello script is **/Samples/Data/SearchLog.tsv1**, where it should be **/Samples/Data/SearchLog.tsv**.</span></span>


## <a name="troubleshoot-hello-job"></a><span data-ttu-id="d2f56-109">Felsöka hello jobb</span><span class="sxs-lookup"><span data-stu-id="d2f56-109">Troubleshoot hello job</span></span>

<span data-ttu-id="d2f56-110">**toosee alla hello jobb**</span><span class="sxs-lookup"><span data-stu-id="d2f56-110">**toosee all hello jobs**</span></span>

1. <span data-ttu-id="d2f56-111">Hello Azure-portalen, klicka på **Microsoft Azure** i hello övre vänstra hörnet.</span><span class="sxs-lookup"><span data-stu-id="d2f56-111">From hello Azure portal, click **Microsoft Azure** in hello upper left corner.</span></span>
2. <span data-ttu-id="d2f56-112">Klicka på panelen hello med ditt Data Lake Analytics-kontonamn.</span><span class="sxs-lookup"><span data-stu-id="d2f56-112">Click hello tile with your Data Lake Analytics account name.</span></span>  <span data-ttu-id="d2f56-113">hello jobb sammanfattning visas på hello **jobbhantering** panelen.</span><span class="sxs-lookup"><span data-stu-id="d2f56-113">hello job summary is shown on hello **Job Management** tile.</span></span>

    ![Hantering av Azure Data Lake Analytics-jobbet](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-job-management.png)

    <span data-ttu-id="d2f56-115">hello jobbet Management ger dig en överblick över av hello jobbstatus.</span><span class="sxs-lookup"><span data-stu-id="d2f56-115">hello job Management gives you a glance of hello job status.</span></span> <span data-ttu-id="d2f56-116">Observera att det är ett jobb som misslyckades.</span><span class="sxs-lookup"><span data-stu-id="d2f56-116">Notice there is a failed job.</span></span>
3. <span data-ttu-id="d2f56-117">Klicka på hello **jobbhantering** panelen toosee hello jobb.</span><span class="sxs-lookup"><span data-stu-id="d2f56-117">Click hello **Job Management** tile toosee hello jobs.</span></span> <span data-ttu-id="d2f56-118">hello jobb kategoriseras i **kör**, **i kö**, och **avslutat**.</span><span class="sxs-lookup"><span data-stu-id="d2f56-118">hello jobs are categorized in **Running**, **Queued**, and **Ended**.</span></span> <span data-ttu-id="d2f56-119">Du bör se din misslyckade jobb i hello **avslutat** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="d2f56-119">You shall see your failed job in hello **Ended** section.</span></span> <span data-ttu-id="d2f56-120">Den är förstnämnda hello-listan.</span><span class="sxs-lookup"><span data-stu-id="d2f56-120">It shall be first one in hello list.</span></span> <span data-ttu-id="d2f56-121">När du har många jobb kan du klicka på **Filter** toohelp du toolocate jobb.</span><span class="sxs-lookup"><span data-stu-id="d2f56-121">When you have a lot of jobs, you can click **Filter** toohelp you toolocate jobs.</span></span>

    ![Filtrera Azure Data Lake Analytics-jobb](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-filter-jobs.png)
4. <span data-ttu-id="d2f56-123">Klicka på hello misslyckade jobbet från jobbinformation för hello listan tooopen hello i ett nytt blad:</span><span class="sxs-lookup"><span data-stu-id="d2f56-123">Click hello failed job from hello list tooopen hello job details in a new blade:</span></span>

    ![Azure Data Lake Analytics misslyckades jobb](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-failed-job.png)

    <span data-ttu-id="d2f56-125">Meddelande hello **skicka** knappen.</span><span class="sxs-lookup"><span data-stu-id="d2f56-125">Notice hello **Resubmit** button.</span></span> <span data-ttu-id="d2f56-126">När du har åtgärdat hello problem kan du skicka hello jobb.</span><span class="sxs-lookup"><span data-stu-id="d2f56-126">After you fix hello problem, you can resubmit hello job.</span></span>
5. <span data-ttu-id="d2f56-127">Klicka på markerade del från hello föregående skärmbild tooopen hello felinformation.</span><span class="sxs-lookup"><span data-stu-id="d2f56-127">Click highlighted part from hello previous screenshot tooopen hello error details.</span></span>  <span data-ttu-id="d2f56-128">Du bör se något som liknar:</span><span class="sxs-lookup"><span data-stu-id="d2f56-128">You shall see something like:</span></span>

    ![Azure Data Lake Analytics misslyckades jobbinformation](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-failed-job-details.png)

    <span data-ttu-id="d2f56-130">Den visar hello källmapp inte hittas.</span><span class="sxs-lookup"><span data-stu-id="d2f56-130">It tells you hello source folder is not found.</span></span>
6. <span data-ttu-id="d2f56-131">Klicka på **duplicera skriptet**.</span><span class="sxs-lookup"><span data-stu-id="d2f56-131">Click **Duplicate Script**.</span></span>
7. <span data-ttu-id="d2f56-132">Uppdatera hello **FROM** sökväg toohello följande:</span><span class="sxs-lookup"><span data-stu-id="d2f56-132">Update hello **FROM** path toohello following:</span></span>

    <span data-ttu-id="d2f56-133">”/ Samples/Data/SearchLog.tsv”</span><span class="sxs-lookup"><span data-stu-id="d2f56-133">"/Samples/Data/SearchLog.tsv"</span></span>
8. <span data-ttu-id="d2f56-134">Klicka på **Skicka jobb**.</span><span class="sxs-lookup"><span data-stu-id="d2f56-134">Click **Submit Job**.</span></span>

## <a name="see-also"></a><span data-ttu-id="d2f56-135">Se även</span><span class="sxs-lookup"><span data-stu-id="d2f56-135">See also</span></span>
* [<span data-ttu-id="d2f56-136">Översikt över Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="d2f56-136">Azure Data Lake Analytics overview</span></span>](data-lake-analytics-overview.md)
* [<span data-ttu-id="d2f56-137">Kom igång med Azure Data Lake Analytics med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="d2f56-137">Get started with Azure Data Lake Analytics using Azure PowerShell</span></span>](data-lake-analytics-get-started-powershell.md)
* [<span data-ttu-id="d2f56-138">Kom igång med Azure Data Lake Analytics och U-SQL med Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d2f56-138">Get started with Azure Data Lake Analytics and U-SQL using Visual Studio</span></span>](data-lake-analytics-u-sql-get-started.md)
* [<span data-ttu-id="d2f56-139">Hantera Azure Data Lake Analytics med hjälp av Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d2f56-139">Manage Azure Data Lake Analytics using Azure Portal</span></span>](data-lake-analytics-manage-use-portal.md)
