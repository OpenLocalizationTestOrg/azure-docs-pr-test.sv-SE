---
title: aaaUse Azure Stream Analytics med SQL Data Warehouse | Microsoft Docs
description: "Tips för att använda Azure Stream Analytics med Azure SQL Data Warehouse för utveckling av lösningar."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: 
ms.assetid: 8aeb2247-20c5-4a29-b327-30a8ce09dfdc
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: 1278197a6764864124fd92fc672de00b83ec343f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-stream-analytics-with-sql-data-warehouse"></a><span data-ttu-id="894fd-103">Använda Azure Stream Analytics med SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="894fd-103">Use Azure Stream Analytics with SQL Data Warehouse</span></span>
<span data-ttu-id="894fd-104">Azure Stream Analytics är en helt hanterad tjänst som tillhandahåller låg latens, hög tillgänglighet och skalbara komplexa händelsebearbetning över strömmande data i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="894fd-104">Azure Stream Analytics is a fully managed service providing low-latency, highly available, scalable complex event processing over streaming data in hello cloud.</span></span> <span data-ttu-id="894fd-105">Du kan lära dig grunderna i hello genom att läsa [introduktion tooAzure Stream Analytics][Introduction tooAzure Stream Analytics].</span><span class="sxs-lookup"><span data-stu-id="894fd-105">You can learn hello basics by reading [Introduction tooAzure Stream Analytics][Introduction tooAzure Stream Analytics].</span></span> <span data-ttu-id="894fd-106">Sedan kan du lära dig hur toocreate en slutpunkt till slutpunkt-lösning med Stream Analytics genom att följa hello [komma igång med Azure Stream Analytics] [ Get started using Azure Stream Analytics] kursen.</span><span class="sxs-lookup"><span data-stu-id="894fd-106">You can then learn how toocreate an end-to-end solution with Stream Analytics by following hello [Get started using Azure Stream Analytics][Get started using Azure Stream Analytics] tutorial.</span></span>

<span data-ttu-id="894fd-107">I den här artikeln får du lära dig hur toouse din Azure SQL Data Warehouse-databas som utdatamottagare för ånga Analytics-jobb.</span><span class="sxs-lookup"><span data-stu-id="894fd-107">In this article, you will learn how toouse your Azure SQL Data Warehouse database as an output sink for your Steam Analytics jobs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="894fd-108">Krav</span><span class="sxs-lookup"><span data-stu-id="894fd-108">Prerequisites</span></span>
<span data-ttu-id="894fd-109">Kör först via hello följa stegen i hello [komma igång med Azure Stream Analytics] [ Get started using Azure Stream Analytics] kursen.</span><span class="sxs-lookup"><span data-stu-id="894fd-109">First, run through hello following steps in hello [Get started using Azure Stream Analytics][Get started using Azure Stream Analytics] tutorial.</span></span>  

1. <span data-ttu-id="894fd-110">Skapa en Event Hub-indata</span><span class="sxs-lookup"><span data-stu-id="894fd-110">Create an Event Hub input</span></span>
2. <span data-ttu-id="894fd-111">Konfigurera och starta program eller generator-händelse</span><span class="sxs-lookup"><span data-stu-id="894fd-111">Configure and start event generator application</span></span>
3. <span data-ttu-id="894fd-112">Etablera ett Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="894fd-112">Provision a Stream Analytics job</span></span>
4. <span data-ttu-id="894fd-113">Ange indata för jobbet och fråga</span><span class="sxs-lookup"><span data-stu-id="894fd-113">Specify job input and query</span></span>

<span data-ttu-id="894fd-114">Skapa sedan en Azure SQL Data Warehouse-databas</span><span class="sxs-lookup"><span data-stu-id="894fd-114">Then, create an Azure SQL Data Warehouse database</span></span>

## <a name="specify-job-output-azure-sql-data-warehouse-database"></a><span data-ttu-id="894fd-115">Ange jobbutdata: Azure SQL Data Warehouse-databas</span><span class="sxs-lookup"><span data-stu-id="894fd-115">Specify job output: Azure SQL Data Warehouse database</span></span>
### <a name="step-1"></a><span data-ttu-id="894fd-116">Steg 1</span><span class="sxs-lookup"><span data-stu-id="894fd-116">Step 1</span></span>
<span data-ttu-id="894fd-117">Klicka på i Stream Analytics-jobbet **utdata** från hello överkant hello sidan och klicka sedan på **lägga till utdata**.</span><span class="sxs-lookup"><span data-stu-id="894fd-117">In your Stream Analytics job click **OUTPUT** from hello top of hello page, and then click **ADD OUTPUT**.</span></span>

### <a name="step-2"></a><span data-ttu-id="894fd-118">Steg 2</span><span class="sxs-lookup"><span data-stu-id="894fd-118">Step 2</span></span>
<span data-ttu-id="894fd-119">Välj SQL-databasen och klicka på Nästa.</span><span class="sxs-lookup"><span data-stu-id="894fd-119">Select SQL Database and click next.</span></span>

![][add-output]

### <a name="step-3"></a><span data-ttu-id="894fd-120">Steg 3</span><span class="sxs-lookup"><span data-stu-id="894fd-120">Step 3</span></span>
<span data-ttu-id="894fd-121">Ange följande värden på nästa sida i hello hello:</span><span class="sxs-lookup"><span data-stu-id="894fd-121">Enter hello following values on hello next page:</span></span>

* <span data-ttu-id="894fd-122">*Utdata Alias*: Ange ett eget namn för det här jobbutdata.</span><span class="sxs-lookup"><span data-stu-id="894fd-122">*Output Alias*: Enter a friendly name for this job output.</span></span>
* <span data-ttu-id="894fd-123">*Prenumerationen*:</span><span class="sxs-lookup"><span data-stu-id="894fd-123">*Subscription*:</span></span>
  * <span data-ttu-id="894fd-124">Om din SQL Data Warehouse-databas i hello samma prenumeration som hello Stream Analytics-jobbet väljer använda SQL-databas från aktuell prenumeration.</span><span class="sxs-lookup"><span data-stu-id="894fd-124">If your SQL Data Warehouse database is in hello same subscription as hello Stream Analytics job, select Use SQL Database from Current Subscription.</span></span>
  * <span data-ttu-id="894fd-125">Om databasen är i en annan prenumeration väljer du Använd SQL-databas från en annan prenumeration.</span><span class="sxs-lookup"><span data-stu-id="894fd-125">If your database is in a different subscription, select Use SQL Database from Another Subscription.</span></span>
* <span data-ttu-id="894fd-126">*Databasen*: Ange en måldatabas hello namn.</span><span class="sxs-lookup"><span data-stu-id="894fd-126">*Database*: Specify hello name of a destination database.</span></span>
* <span data-ttu-id="894fd-127">*Servernamnet*: Ange hello servernamnet hello-databasen som du precis angav.</span><span class="sxs-lookup"><span data-stu-id="894fd-127">*Server Name*: Specify hello server name for hello database you just specified.</span></span> <span data-ttu-id="894fd-128">Du kan använda hello Azure klassiska Portal toofind den.</span><span class="sxs-lookup"><span data-stu-id="894fd-128">You can use hello Azure Classic Portal toofind this.</span></span>

![][server-name]

* <span data-ttu-id="894fd-129">*Användarnamnet*: Ange hello användarnamnet för ett konto som har skrivbehörighet för hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="894fd-129">*User Name*: Specify hello user name of an account that has write permissions for hello database.</span></span>
* <span data-ttu-id="894fd-130">*Lösenordet*: Ange hello lösenordet för hello angivna användarkontot.</span><span class="sxs-lookup"><span data-stu-id="894fd-130">*Password*: Provide hello password for hello specified user account.</span></span>
* <span data-ttu-id="894fd-131">*Tabellen*: Ange hello namnet på hello måltabellen i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="894fd-131">*Table*: Specify hello name of hello target table in hello database.</span></span>

![][add-database]

### <a name="step-4"></a><span data-ttu-id="894fd-132">Steg 4</span><span class="sxs-lookup"><span data-stu-id="894fd-132">Step 4</span></span>
<span data-ttu-id="894fd-133">Klicka på hello Kontrollera knappen tooadd denna jobbutdata och tooverify att Stream Analytics kan ansluta toohello databas.</span><span class="sxs-lookup"><span data-stu-id="894fd-133">Click hello check button tooadd this job output and tooverify that Stream Analytics can successfully connect toohello database.</span></span>

![][test-connection]

<span data-ttu-id="894fd-134">När hello toohello databas lyckas visas ett meddelande längst ned hello hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="894fd-134">When hello connection toohello database succeeds, you will see a notification at hello bottom of hello portal.</span></span> <span data-ttu-id="894fd-135">Du kan klicka på Testa anslutning på hello nedre tootest hello toohello databas.</span><span class="sxs-lookup"><span data-stu-id="894fd-135">You can click Test Connection at hello bottom tootest hello connection toohello database.</span></span>

## <a name="next-steps"></a><span data-ttu-id="894fd-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="894fd-136">Next steps</span></span>
<span data-ttu-id="894fd-137">En översikt över integrering finns [översikt över SQL Data Warehouse-integration][SQL Data Warehouse integration overview].</span><span class="sxs-lookup"><span data-stu-id="894fd-137">For an overview of integration, see [SQL Data Warehouse integration overview][SQL Data Warehouse integration overview].</span></span>

<span data-ttu-id="894fd-138">För fler utvecklingstips, se [Översikt över SQL Data Warehouse-utveckling][SQL Data Warehouse development overview].</span><span class="sxs-lookup"><span data-stu-id="894fd-138">For more development tips, see [SQL Data Warehouse development overview][SQL Data Warehouse development overview].</span></span>

<!--Image references-->

[add-output]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-output.png
[server-name]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/dw-server-name.png
[add-database]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-database.png
[test-connection]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/test-connection.png

<!--Article references-->

[Introduction tooAzure Stream Analytics]: ../stream-analytics/stream-analytics-introduction.md
[Get started using Azure Stream Analytics]: ../stream-analytics/stream-analytics-real-time-fraud-detection.md
[SQL Data Warehouse development overview]:  ./sql-data-warehouse-overview-develop.md
[SQL Data Warehouse integration overview]:  ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Stream Analytics documentation]: http://azure.microsoft.com/documentation/services/stream-analytics/
