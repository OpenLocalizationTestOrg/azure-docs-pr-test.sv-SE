---
title: "aaaCreate en tabellmodell genom att använda hello Azure Analysis Services Web designer | Microsoft Docs"
description: "Lär dig hur toocreate en tabellmodell med hjälp av Azure Analysis Services hello Webbdesigner i Azure-portalen."
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
ms.openlocfilehash: a37b326b76c84fc3a4300827bc1c8706b0584701
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-model-in-azure-portal"></a><span data-ttu-id="5b97d-103">Skapa en modell i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="5b97d-103">Create a model in Azure portal</span></span>

<span data-ttu-id="5b97d-104">hello Azure Analysis Services web designer (förhandsversion)-funktionen i Azure-portalen innehåller ett snabbt och enkelt sätt toocreate och redigera tabellmodeller och fråga modellen data direkt i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="5b97d-104">hello Azure Analysis Services web designer (preview) feature in Azure portal provides a quick and easy way toocreate and edit tabular models and query model data right in your browser.</span></span> 

<span data-ttu-id="5b97d-105">Kom ihåg, hello Webbdesigner är **preview**.</span><span class="sxs-lookup"><span data-stu-id="5b97d-105">Keep in mind, hello web designer is **preview**.</span></span> <span data-ttu-id="5b97d-106">När nya funktioner läggs alla hello tiden, i förhandsvisningen är funktionerna begränsade.</span><span class="sxs-lookup"><span data-stu-id="5b97d-106">While new functionality is being added all hello time, in preview, functionality is limited.</span></span> <span data-ttu-id="5b97d-107">För mer avancerade modellen utveckling och testning är det bästa toouse Visual Studio (SSDT) och SQL Server Management Studio (SSMS).</span><span class="sxs-lookup"><span data-stu-id="5b97d-107">For more advanced model development and testing, it's best toouse Visual Studio (SSDT) and SQL Server Management Studio (SSMS).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5b97d-108">Krav</span><span class="sxs-lookup"><span data-stu-id="5b97d-108">Prerequisites</span></span>

- <span data-ttu-id="5b97d-109">En Azure Analysis Services-server på hello Standard eller utvecklare nivå.</span><span class="sxs-lookup"><span data-stu-id="5b97d-109">An Azure Analysis Services server at hello Standard or Developer tier.</span></span> <span data-ttu-id="5b97d-110">Nya modeller som skapats med hjälp av hello Webbdesigner är DirectQuery, stöds endast av dessa nivåer.</span><span class="sxs-lookup"><span data-stu-id="5b97d-110">New models created by using hello Web designer are DirectQuery, supported only by these tiers.</span></span>
- <span data-ttu-id="5b97d-111">Azure SQL Database, Azure SQL Data Warehouse eller Power BI Desktop (.pbix)-filen som en datakälla.</span><span class="sxs-lookup"><span data-stu-id="5b97d-111">An Azure SQL Database, Azure SQL Data Warehouse, or Power BI Desktop (.pbix) file as a datasource.</span></span> <span data-ttu-id="5b97d-112">Nya modeller som skapats från Power BI Desktop filer stöd Azure SQL Database, Azure SQL Data Warehouse, Oracle och Teradata-datakällor.</span><span class="sxs-lookup"><span data-stu-id="5b97d-112">New models created from Power BI Desktop files support Azure SQL Database, Azure SQL Data Warehouse, Oracle, and Teradata data sources.</span></span>
- <span data-ttu-id="5b97d-113">Ett SQL Server-konto och lösenord för att ansluta tooAzure SQL-databas eller Azure SQL Data Warehouse-datakällor.</span><span class="sxs-lookup"><span data-stu-id="5b97d-113">A SQL Server account and password for connecting tooAzure SQL Database or Azure SQL Data Warehouse data sources.</span></span>

## <a name="toocreate-a-new-tabular-model"></a><span data-ttu-id="5b97d-114">toocreate en ny tabellmodell</span><span class="sxs-lookup"><span data-stu-id="5b97d-114">toocreate a new tabular model</span></span>

1. <span data-ttu-id="5b97d-115">I serverns **översikt** bladet > **Webbdesigner**, klickar du på **öppna**.</span><span class="sxs-lookup"><span data-stu-id="5b97d-115">In your server's **Overview** blade > **Web designer**, click **Open**.</span></span>

    ![Skapa en modell i Azure-portalen](./media/analysis-services-create-model-portal/aas-create-portal-overview-wd.png)

2. <span data-ttu-id="5b97d-117">I **Webbdesigner** > **modeller**, klickar du på **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="5b97d-117">In **Web designer** > **Models**, click **+ Add**.</span></span>

    ![Skapa en modell i Azure-portalen](./media/analysis-services-create-model-portal/aas-create-portal-models.png)

3. <span data-ttu-id="5b97d-119">I **ny modell**, Skriv ett namn och välj sedan en datakälla.</span><span class="sxs-lookup"><span data-stu-id="5b97d-119">In **New model**, type a model name, and then select a data source.</span></span>

    ![Dialogrutan ny modell i Azure-portalen](./media/analysis-services-create-model-portal/aas-create-portal-new-model.png)

4. <span data-ttu-id="5b97d-121">I **Anslut**, ange hello anslutningsegenskaper.</span><span class="sxs-lookup"><span data-stu-id="5b97d-121">In **Connect**, enter hello connection properties.</span></span> <span data-ttu-id="5b97d-122">Användarnamn och lösenord måste vara en SQL Server-konto.</span><span class="sxs-lookup"><span data-stu-id="5b97d-122">Username and password must be a SQL Server account.</span></span>

     ![Ansluta dialogrutan i Azure-portalen](./media/analysis-services-create-model-portal/aas-create-portal-connect.png)

5. <span data-ttu-id="5b97d-124">I **tabeller och vyer**, Välj hello tabeller tooinclude i din modell och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="5b97d-124">In **Tables and views**, select hello tables tooinclude in your model, and then click **Create**.</span></span> <span data-ttu-id="5b97d-125">Är att skapa relationer automatiskt mellan tabeller med ett nyckelpar.</span><span class="sxs-lookup"><span data-stu-id="5b97d-125">Relationships are created automatically between tables with a key pair.</span></span>

     ![Välj tabeller och vyer](./media/analysis-services-create-model-portal/aas-create-portal-tables.png)

<span data-ttu-id="5b97d-127">Din nya modell visas i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="5b97d-127">Your new model appears in your browser.</span></span> <span data-ttu-id="5b97d-128">Här kan göra du följande:</span><span class="sxs-lookup"><span data-stu-id="5b97d-128">From here, you can:</span></span>   

- <span data-ttu-id="5b97d-129">Fråga modelldata genom att dra fält toohello Frågedesigner och lägga till filter.</span><span class="sxs-lookup"><span data-stu-id="5b97d-129">Query model data by dragging fields toohello query designer and adding filters.</span></span>
- <span data-ttu-id="5b97d-130">Skapa nya åtgärder i tabeller.</span><span class="sxs-lookup"><span data-stu-id="5b97d-130">Create new measures in tables.</span></span>
- <span data-ttu-id="5b97d-131">Redigera modellmetadata med hello json-redigerare.</span><span class="sxs-lookup"><span data-stu-id="5b97d-131">Edit model metadata by using hello json editor.</span></span>
- <span data-ttu-id="5b97d-132">Öppna hello modellen i Visual Studio (SSDT), Power BI Desktop eller Excel.</span><span class="sxs-lookup"><span data-stu-id="5b97d-132">Open hello model in Visual Studio (SSDT), Power BI Desktop, or Excel.</span></span>

![Välj tabeller och vyer](./media/analysis-services-create-model-portal/aas-create-portal-query.png)

> [!NOTE]
> <span data-ttu-id="5b97d-134">När du redigerar modellmetadata eller skapa nya åtgärder i webbläsaren, sparar du dessa ändringar tooyour modellen i Azure.</span><span class="sxs-lookup"><span data-stu-id="5b97d-134">When you edit model metadata or create new measures in your browser, you're saving those changes tooyour model in Azure.</span></span> <span data-ttu-id="5b97d-135">Om du arbetar också på din modell i SSDT, Power BI Desktop eller Excel, kan du få synkroniserad din modell.</span><span class="sxs-lookup"><span data-stu-id="5b97d-135">If you're also working on your model in SSDT, Power BI Desktop, or Excel, your model can get out of sync.</span></span>


## <a name="next-steps"></a><span data-ttu-id="5b97d-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5b97d-136">Next steps</span></span> 
[<span data-ttu-id="5b97d-137">Hantera databasroller och användare</span><span class="sxs-lookup"><span data-stu-id="5b97d-137">Manage database roles and users</span></span>](analysis-services-database-users.md)  
[<span data-ttu-id="5b97d-138">Anslut till Excel</span><span class="sxs-lookup"><span data-stu-id="5b97d-138">Connect with Excel</span></span>](analysis-services-connect-excel.md)  


