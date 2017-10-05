---
title: "Skapa en tabellmodell med hjälp av Azure Analysis Services Web-designer | Microsoft Docs"
description: "Lär dig hur du skapar en tabellmodell Azure Analysis Services med hjälp av Webbdesigner i Azure-portalen."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/21/2017
ms.author: owend
ms.openlocfilehash: bd58f1845dabf6afb47ce27236d14479677a8808
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-model-in-azure-portal"></a><span data-ttu-id="5e374-103">Skapa en modell i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="5e374-103">Create a model in Azure portal</span></span>

<span data-ttu-id="5e374-104">Funktionen Azure Analysis Services web designer (förhandsversion) i Azure-portalen innehåller ett snabbt och enkelt sätt att skapa och redigera tabellmodeller och fråga modellen data direkt i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="5e374-104">The Azure Analysis Services web designer (preview) feature in Azure portal provides a quick and easy way to create and edit tabular models and query model data right in your browser.</span></span> 

<span data-ttu-id="5e374-105">Kom ihåg, Webbdesigner är **preview**.</span><span class="sxs-lookup"><span data-stu-id="5e374-105">Keep in mind, the web designer is **preview**.</span></span> <span data-ttu-id="5e374-106">När nya funktioner har lagts till hela tiden, i preview är funktionerna begränsade.</span><span class="sxs-lookup"><span data-stu-id="5e374-106">While new functionality is being added all the time, in preview, functionality is limited.</span></span> <span data-ttu-id="5e374-107">För mer avancerade modellen utveckling och testning är det bäst att använda Visual Studio (SSDT) och SQL Server Management Studio (SSMS).</span><span class="sxs-lookup"><span data-stu-id="5e374-107">For more advanced model development and testing, it's best to use Visual Studio (SSDT) and SQL Server Management Studio (SSMS).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5e374-108">Krav</span><span class="sxs-lookup"><span data-stu-id="5e374-108">Prerequisites</span></span>

- <span data-ttu-id="5e374-109">En Azure Analysis Services-server till Standard eller utvecklare nivån.</span><span class="sxs-lookup"><span data-stu-id="5e374-109">An Azure Analysis Services server at the Standard or Developer tier.</span></span> <span data-ttu-id="5e374-110">Nya modeller som skapats med hjälp av Webbdesigner är DirectQuery, stöds endast av dessa nivåer.</span><span class="sxs-lookup"><span data-stu-id="5e374-110">New models created by using the Web designer are DirectQuery, supported only by these tiers.</span></span>
- <span data-ttu-id="5e374-111">Azure SQL Database, Azure SQL Data Warehouse eller Power BI Desktop (.pbix)-filen som en datakälla.</span><span class="sxs-lookup"><span data-stu-id="5e374-111">An Azure SQL Database, Azure SQL Data Warehouse, or Power BI Desktop (.pbix) file as a datasource.</span></span> <span data-ttu-id="5e374-112">Nya modeller som skapats från Power BI Desktop filer stöd Azure SQL Database, Azure SQL Data Warehouse, Oracle och Teradata-datakällor.</span><span class="sxs-lookup"><span data-stu-id="5e374-112">New models created from Power BI Desktop files support Azure SQL Database, Azure SQL Data Warehouse, Oracle, and Teradata data sources.</span></span>
- <span data-ttu-id="5e374-113">Ett SQL Server-konto och lösenord för att ansluta till Azure SQL Database eller Azure SQL Data Warehouse-datakällor.</span><span class="sxs-lookup"><span data-stu-id="5e374-113">A SQL Server account and password for connecting to Azure SQL Database or Azure SQL Data Warehouse data sources.</span></span>

## <a name="to-create-a-new-tabular-model"></a><span data-ttu-id="5e374-114">Skapa en ny tabellmodell</span><span class="sxs-lookup"><span data-stu-id="5e374-114">To create a new tabular model</span></span>

1. <span data-ttu-id="5e374-115">I serverns **översikt** bladet > **Webbdesigner**, klickar du på **öppna**.</span><span class="sxs-lookup"><span data-stu-id="5e374-115">In your server's **Overview** blade > **Web designer**, click **Open**.</span></span>

    ![Skapa en modell i Azure-portalen](./media/analysis-services-create-model-portal/aas-create-portal-overview-wd.png)

2. <span data-ttu-id="5e374-117">I **Webbdesigner** > **modeller**, klickar du på **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="5e374-117">In **Web designer** > **Models**, click **+ Add**.</span></span>

    ![Skapa en modell i Azure-portalen](./media/analysis-services-create-model-portal/aas-create-portal-models.png)

3. <span data-ttu-id="5e374-119">I **ny modell**, Skriv ett namn och välj sedan en datakälla.</span><span class="sxs-lookup"><span data-stu-id="5e374-119">In **New model**, type a model name, and then select a data source.</span></span>

    ![Dialogrutan ny modell i Azure-portalen](./media/analysis-services-create-model-portal/aas-create-portal-new-model.png)

4. <span data-ttu-id="5e374-121">I **Anslut**, ange anslutningsegenskaper.</span><span class="sxs-lookup"><span data-stu-id="5e374-121">In **Connect**, enter the connection properties.</span></span> <span data-ttu-id="5e374-122">Användarnamn och lösenord måste vara en SQL Server-konto.</span><span class="sxs-lookup"><span data-stu-id="5e374-122">Username and password must be a SQL Server account.</span></span>

     ![Ansluta dialogrutan i Azure-portalen](./media/analysis-services-create-model-portal/aas-create-portal-connect.png)

5. <span data-ttu-id="5e374-124">I **tabeller och vyer**, Välj tabellerna som du vill inkludera i din modell och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="5e374-124">In **Tables and views**, select the tables to include in your model, and then click **Create**.</span></span> <span data-ttu-id="5e374-125">Är att skapa relationer automatiskt mellan tabeller med ett nyckelpar.</span><span class="sxs-lookup"><span data-stu-id="5e374-125">Relationships are created automatically between tables with a key pair.</span></span>

     ![Välj tabeller och vyer](./media/analysis-services-create-model-portal/aas-create-portal-tables.png)

<span data-ttu-id="5e374-127">Din nya modell visas i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="5e374-127">Your new model appears in your browser.</span></span> <span data-ttu-id="5e374-128">Här kan göra du följande:</span><span class="sxs-lookup"><span data-stu-id="5e374-128">From here, you can:</span></span>   

- <span data-ttu-id="5e374-129">Fråga modelldata genom att dra fält till frågedesignern och lägga till filter.</span><span class="sxs-lookup"><span data-stu-id="5e374-129">Query model data by dragging fields to the query designer and adding filters.</span></span>
- <span data-ttu-id="5e374-130">Skapa nya åtgärder i tabeller.</span><span class="sxs-lookup"><span data-stu-id="5e374-130">Create new measures in tables.</span></span>
- <span data-ttu-id="5e374-131">Redigera modellmetadata med hjälp av json-redigerare.</span><span class="sxs-lookup"><span data-stu-id="5e374-131">Edit model metadata by using the json editor.</span></span>
- <span data-ttu-id="5e374-132">Öppna modellen i Visual Studio (SSDT), Power BI Desktop eller Excel.</span><span class="sxs-lookup"><span data-stu-id="5e374-132">Open the model in Visual Studio (SSDT), Power BI Desktop, or Excel.</span></span>

![Välj tabeller och vyer](./media/analysis-services-create-model-portal/aas-create-portal-query.png)

> [!NOTE]
> <span data-ttu-id="5e374-134">När du redigerar modellmetadata eller skapa nya åtgärder i webbläsaren, sparar du ändringarna i modellen i Azure.</span><span class="sxs-lookup"><span data-stu-id="5e374-134">When you edit model metadata or create new measures in your browser, you're saving those changes to your model in Azure.</span></span> <span data-ttu-id="5e374-135">Om du arbetar också på din modell i SSDT, Power BI Desktop eller Excel, kan du få synkroniserad din modell.</span><span class="sxs-lookup"><span data-stu-id="5e374-135">If you're also working on your model in SSDT, Power BI Desktop, or Excel, your model can get out of sync.</span></span>


## <a name="next-steps"></a><span data-ttu-id="5e374-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5e374-136">Next steps</span></span> 
[<span data-ttu-id="5e374-137">Hantera databasroller och användare</span><span class="sxs-lookup"><span data-stu-id="5e374-137">Manage database roles and users</span></span>](analysis-services-database-users.md)  
[<span data-ttu-id="5e374-138">Anslut till Excel</span><span class="sxs-lookup"><span data-stu-id="5e374-138">Connect with Excel</span></span>](analysis-services-connect-excel.md)  


