---
title: "aaaDesign första Azure SQL database - C# | Microsoft Docs"
description: "Lär dig toodesign din första Azure SQL-databas och ansluta tooit med ett C#-program med hjälp av ADO.NET."
services: sql-database
documentationcenter: 
author: MightyPen
manager: craigg-msft
editor: CarlRabeler
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: develop databases
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 07/31/2017
ms.author: genemi;carlrab
ms.openlocfilehash: 8161de24bff1ec2fa307efa93adab2bd1b761fd9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="design-an-azure-sql-database-and-connect-with-cx23-and-adonet"></a><span data-ttu-id="28096-103">Utforma en Azure SQL database och ansluta med C & #x23; och ADO.NET</span><span class="sxs-lookup"><span data-stu-id="28096-103">Design an Azure SQL database and connect with C&#x23; and ADO.NET</span></span>

<span data-ttu-id="28096-104">Azure SQL Database är en relationell databas-som-en tjänst (DBaaS) i hello Microsoft Cloud (”Azure”).</span><span class="sxs-lookup"><span data-stu-id="28096-104">Azure SQL Database is a relational database-as-a service (DBaaS) in hello Microsoft Cloud ("Azure").</span></span> <span data-ttu-id="28096-105">I kursen får du lära dig hur toouse hello Azure-portalen och ADO.NET med Visual Studio för att:</span><span class="sxs-lookup"><span data-stu-id="28096-105">In this tutorial, you learn how toouse hello Azure portal and ADO.NET with Visual Studio to:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="28096-106">Skapa en databas i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="28096-106">Create a database in hello Azure portal</span></span>
> * <span data-ttu-id="28096-107">Konfigurera en brandväggsregel på servernivå i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="28096-107">Set up a server-level firewall rule in hello Azure portal</span></span>
> * <span data-ttu-id="28096-108">Ansluta toohello databasen med ADO.NET och Visual Studio</span><span class="sxs-lookup"><span data-stu-id="28096-108">Connect toohello database with ADO.NET and Visual Studio</span></span>
> * <span data-ttu-id="28096-109">Skapa tabeller med ADO.NET</span><span class="sxs-lookup"><span data-stu-id="28096-109">Create tables with ADO.NET</span></span>
> * <span data-ttu-id="28096-110">Infoga, uppdatera och ta bort data med ADO.NET</span><span class="sxs-lookup"><span data-stu-id="28096-110">Insert, update, and delete data with ADO.NET</span></span> 
> * <span data-ttu-id="28096-111">Fråga efter data ADO.NET</span><span class="sxs-lookup"><span data-stu-id="28096-111">Query data ADO.NET</span></span>

<span data-ttu-id="28096-112">Om du inte har en Azure-prenumeration [skapa ett kostnadsfritt konto](https://azure.microsoft.com/free/) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="28096-112">If you don't have an Azure subscription, [create a free account](https://azure.microsoft.com/free/) before you begin.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="28096-113">Krav</span><span class="sxs-lookup"><span data-stu-id="28096-113">Prerequisites</span></span>

<span data-ttu-id="28096-114">En installation av [Visual Studio Community 2017, Visual Studio Professional 2017 eller Visual Studio Enterprise 2017](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="28096-114">An installation of [Visual Studio Community 2017, Visual Studio Professional 2017, or Visual Studio Enterprise 2017](https://www.visualstudio.com/downloads/).</span></span>

<!-- hello following included .md, sql-database-tutorial-portal-create-firewall-connection-1.md, is long.
And it starts with a ## H2.
-->

[!INCLUDE [sql-database-tutorial-portal-create-firewall-connection-1](../../includes/sql-database-tutorial-portal-create-firewall-connection-1.md)]


<!-- hello following included .md, sql-database-csharp-adonet-create-query-2.md, is long.
And it starts with a ## H2.
-->

[!INCLUDE [sql-database-csharp-adonet-create-query-2](../../includes/sql-database-csharp-adonet-create-query-2.md)]


## <a name="next-steps"></a><span data-ttu-id="28096-115">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="28096-115">Next steps</span></span>

<span data-ttu-id="28096-116">I kursen får du lärt dig grundläggande uppgifter som skapar en databas och tabeller, läsa in fråga efter data och återställningspunkt hello databasen tooa tidigare tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="28096-116">In this tutorial, you learned basic database tasks such as create a database and tables, load and query data, and restore hello database tooa previous point in time.</span></span> <span data-ttu-id="28096-117">Du har lärt dig att:</span><span class="sxs-lookup"><span data-stu-id="28096-117">You learned how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="28096-118">Skapa en databas</span><span class="sxs-lookup"><span data-stu-id="28096-118">Create a database</span></span>
> * <span data-ttu-id="28096-119">Konfigurera en brandväggsregel</span><span class="sxs-lookup"><span data-stu-id="28096-119">Set up a firewall rule</span></span>
> * <span data-ttu-id="28096-120">Ansluta toohello databasen med [Visual Studio och C#](sql-database-connect-query-dotnet-visual-studio.md)</span><span class="sxs-lookup"><span data-stu-id="28096-120">Connect toohello database with [Visual Studio and C#](sql-database-connect-query-dotnet-visual-studio.md)</span></span>
> * <span data-ttu-id="28096-121">Skapa tabeller</span><span class="sxs-lookup"><span data-stu-id="28096-121">Create tables</span></span>
> * <span data-ttu-id="28096-122">Infoga, uppdatera och ta bort data</span><span class="sxs-lookup"><span data-stu-id="28096-122">Insert, update, and delete data</span></span>
> * <span data-ttu-id="28096-123">Frågedata</span><span class="sxs-lookup"><span data-stu-id="28096-123">Query data</span></span>

<span data-ttu-id="28096-124">Nästa självstudiekurs toolearn toohello om att migrera dina data i förväg.</span><span class="sxs-lookup"><span data-stu-id="28096-124">Advance toohello next tutorial toolearn about migrating your data.</span></span>

> [!div class="nextstepaction"]
>[<span data-ttu-id="28096-125">Migrera din SQL Server-databasen tooAzure SQL-databas</span><span class="sxs-lookup"><span data-stu-id="28096-125">Migrate your SQL Server database tooAzure SQL Database</span></span>](sql-database-migrate-your-sql-server-database.md)

