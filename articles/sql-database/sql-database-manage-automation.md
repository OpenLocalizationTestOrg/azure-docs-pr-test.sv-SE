---
title: "Hantera Azure SQL-databaser med hjälp av Azure Automation | Microsoft Docs"
description: "Läs mer om hur Azure Automation-tjänsten kan användas för att hantera Azure SQL-databaser i större skala."
services: sql-database, automation
documentationcenter: 
author: jodoglevy
manager: jhubbard
editor: monicar
ms.assetid: 77c262a1-9b93-456d-b3c7-b2f23bdfcd61
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2017
ms.author: jhubbard
ms.openlocfilehash: 7f45b8b654691063823c13bee61e9bb874a6a13a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="managing-azure-sql-databases-using-azure-automation"></a><span data-ttu-id="ceb2c-103">Hantera Azure SQL-databaser med hjälp av Azure Automation</span><span class="sxs-lookup"><span data-stu-id="ceb2c-103">Managing Azure SQL Databases using Azure Automation</span></span>
<span data-ttu-id="ceb2c-104">Den här guiden innehåller en introduktion till Azure Automation-tjänsten och hur det kan användas för att förenkla hanteringen av dina Azure SQL-databaser.</span><span class="sxs-lookup"><span data-stu-id="ceb2c-104">This guide will introduce you to the Azure Automation service, and how it can be used to simplify management of your Azure SQL databases.</span></span>

## <a name="what-is-azure-automation"></a><span data-ttu-id="ceb2c-105">Vad är Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="ceb2c-105">What is Azure Automation?</span></span>
<span data-ttu-id="ceb2c-106">[Azure Automation](https://azure.microsoft.com/services/automation/) är en Azure-tjänst som förenklar molnhantering via automatisering.</span><span class="sxs-lookup"><span data-stu-id="ceb2c-106">[Azure Automation](https://azure.microsoft.com/services/automation/) is an Azure service for simplifying cloud management through process automation.</span></span> <span data-ttu-id="ceb2c-107">Med Azure Automation kan tidskrävande, manuell, felbenägna och ofta återkommande uppgifter automatiseras för att öka tillförlitligheten, effektivitet och tid för värde för din organisation.</span><span class="sxs-lookup"><span data-stu-id="ceb2c-107">Using Azure Automation, long-running, manual, error-prone, and frequently repeated tasks can be automated to increase reliability, efficiency, and time to value for your organization.</span></span>

<span data-ttu-id="ceb2c-108">Azure Automation ger en Körningsmotor för arbetsflöden för hög tillförlitliga och hög tillgänglighet som kan skalas för att uppfylla dina behov när organisationen växer.</span><span class="sxs-lookup"><span data-stu-id="ceb2c-108">Azure Automation provides a highly-reliable and highly-available workflow execution engine that scales to meet your needs as your organization grows.</span></span> <span data-ttu-id="ceb2c-109">I Azure Automation kan processer vara inletts manuellt av 3 part eller med schemalagda intervall så att aktiviteter inträffa exakt vid behov.</span><span class="sxs-lookup"><span data-stu-id="ceb2c-109">In Azure Automation, processes can be kicked off manually, by 3rd-party systems, or at scheduled intervals so that tasks happen exactly when needed.</span></span>

<span data-ttu-id="ceb2c-110">Lägre operativ tillsyn och frigöra IT / DevOps-personal att fokusera på arbete som lägger till företag värde genom att flytta dina uppgifter för hantering av molnet att köras automatiskt av Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="ceb2c-110">Lower operational overhead and free up IT / DevOps staff to focus on work that adds business value by moving your cloud management tasks to be run automatically by Azure Automation.</span></span>

## <a name="how-can-azure-automation-help-manage-azure-sql-databases"></a><span data-ttu-id="ceb2c-111">Hur Azure Automation kan hantera Azure SQL-databaser?</span><span class="sxs-lookup"><span data-stu-id="ceb2c-111">How can Azure Automation help manage Azure SQL databases?</span></span>
<span data-ttu-id="ceb2c-112">Azure SQL Database kan hanteras i Azure Automation med hjälp av den [Azure SQL Database PowerShell-cmdlets](https://docs.microsoft.com/powershell/servicemanagement/azure.sqldatabase/v1.6.1/azure.sqldatabase/) som är tillgängliga i den [Azure PowerShell verktygen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ceb2c-112">Azure SQL Database can be managed in Azure Automation by using the [Azure SQL Database PowerShell cmdlets](https://docs.microsoft.com/powershell/servicemanagement/azure.sqldatabase/v1.6.1/azure.sqldatabase/) that are available in the [Azure PowerShell tools](/powershell/azure/overview).</span></span> <span data-ttu-id="ceb2c-113">Azure Automation har dessa Azure SQL Database PowerShell-cmdlets som är tillgängliga efter installationen, så att du kan utföra alla aktiviteter i tjänsten SQL DB management.</span><span class="sxs-lookup"><span data-stu-id="ceb2c-113">Azure Automation has these Azure SQL Database PowerShell cmdlets available out of the box, so that you can perform all of your SQL DB management tasks within the service.</span></span> <span data-ttu-id="ceb2c-114">Du kan också koppla dessa cmdlets i Azure Automation med cmdlets för andra Azure-tjänster, att automatisera avancerade uppgifter över Azure-tjänster och 3 part.</span><span class="sxs-lookup"><span data-stu-id="ceb2c-114">You can also pair these cmdlets in Azure Automation with the cmdlets for other Azure services, to automate complex tasks across Azure services and 3rd party systems.</span></span>

<span data-ttu-id="ceb2c-115">Azure Automation har också möjlighet att kommunicera med SQL-servrar direkt, genom att utfärda SQL-kommandon med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ceb2c-115">Azure Automation also has the ability to communicate with SQL servers directly, by issuing SQL commands using PowerShell.</span></span>

<span data-ttu-id="ceb2c-116">Den [Azure Automation runbook-galleriet](https://azure.microsoft.com/blog/2014/10/07/introducing-the-azure-automation-runbook-gallery/) innehåller en mängd produkten team och community runbooks för att komma igång med att automatisera hanteringen av Azure SQL-databaser, andra Azure-tjänster och 3 part.</span><span class="sxs-lookup"><span data-stu-id="ceb2c-116">The [Azure Automation runbook gallery](https://azure.microsoft.com/blog/2014/10/07/introducing-the-azure-automation-runbook-gallery/) contains a variety of product team and community runbooks to get started automating management of Azure SQL Databases, other Azure services, and 3rd party systems.</span></span> <span data-ttu-id="ceb2c-117">Galleriet runbooks är:</span><span class="sxs-lookup"><span data-stu-id="ceb2c-117">Gallery runbooks include:</span></span>

* [<span data-ttu-id="ceb2c-118">Kör SQL-frågor mot en SQL Server-databas</span><span class="sxs-lookup"><span data-stu-id="ceb2c-118">Run SQL queries against a SQL Server database</span></span>](https://gallery.technet.microsoft.com/scriptcenter/How-to-use-a-SQL-Command-be77f9d2)
* [<span data-ttu-id="ceb2c-119">Lodrätt skala (upp eller ned) en Azure SQL Database enligt ett schema</span><span class="sxs-lookup"><span data-stu-id="ceb2c-119">Vertically scale (up or down) an Azure SQL Database on a schedule</span></span>](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-e957354f)
* [<span data-ttu-id="ceb2c-120">Trunkera en SQLtabell om databasen närmar sig maximal storlek</span><span class="sxs-lookup"><span data-stu-id="ceb2c-120">Truncate a SQL table if its database approaches its maximum size</span></span>](https://gallery.technet.microsoft.com/scriptcenter/Azure-Automation-Your-SQL-30f8736b)
* [<span data-ttu-id="ceb2c-121">Indexera tabeller i en Azure SQL Database om de är mycket fragmenterat</span><span class="sxs-lookup"><span data-stu-id="ceb2c-121">Index tables in an Azure SQL Database if they are highly fragmented</span></span>](https://gallery.technet.microsoft.com/scriptcenter/Indexes-tables-in-an-Azure-73a2a8ea)

## <a name="next-steps"></a><span data-ttu-id="ceb2c-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ceb2c-122">Next steps</span></span>
<span data-ttu-id="ceb2c-123">Nu när du har lärt dig grunderna i Azure Automation och hur det kan användas för att hantera Azure SQL-databaser, följa dessa länkar för att lära dig mer om Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="ceb2c-123">Now that you've learned the basics of Azure Automation and how it can be used to manage Azure SQL databases, follow these links to learn more about Azure Automation.</span></span>

* [<span data-ttu-id="ceb2c-124">Azure Automation-översikt</span><span class="sxs-lookup"><span data-stu-id="ceb2c-124">Azure Automation Overview</span></span>](../automation/automation-intro.md)
* [<span data-ttu-id="ceb2c-125">Min första Runbook</span><span class="sxs-lookup"><span data-stu-id="ceb2c-125">My first runbook</span></span>](../automation/automation-first-runbook-graphical.md)
* [<span data-ttu-id="ceb2c-126">Azure Automation-inlärningsguide</span><span class="sxs-lookup"><span data-stu-id="ceb2c-126">Azure Automation learning map</span></span>](https://azure.microsoft.com/documentation/learning-paths/automation/)
* [<span data-ttu-id="ceb2c-127">Azure Automation: Din SQL Agent i molnet</span><span class="sxs-lookup"><span data-stu-id="ceb2c-127">Azure Automation: Your SQL Agent in the Cloud</span></span>](https://azure.microsoft.com/blog/2014/06/26/azure-automation-your-sql-agent-in-the-cloud/) 

