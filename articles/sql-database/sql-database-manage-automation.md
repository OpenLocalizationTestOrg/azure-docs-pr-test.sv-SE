---
title: "aaaManage Azure SQL-databaser med hjälp av Azure Automation | Microsoft Docs"
description: "Läs mer om hur hello Azure Automation-tjänsten kan vara används toomanage Azure SQL-databaser i större skala."
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
ms.openlocfilehash: d613cca32ba86e27b9c1b952c4e723ea7f07beb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="managing-azure-sql-databases-using-azure-automation"></a><span data-ttu-id="cc1a3-103">Hantera Azure SQL-databaser med hjälp av Azure Automation</span><span class="sxs-lookup"><span data-stu-id="cc1a3-103">Managing Azure SQL Databases using Azure Automation</span></span>
<span data-ttu-id="cc1a3-104">Den här guiden beskrivs toohello Azure Automation-tjänsten och hur det kan vara används toosimplify hanteringen av dina Azure SQL-databaser.</span><span class="sxs-lookup"><span data-stu-id="cc1a3-104">This guide will introduce you toohello Azure Automation service, and how it can be used toosimplify management of your Azure SQL databases.</span></span>

## <a name="what-is-azure-automation"></a><span data-ttu-id="cc1a3-105">Vad är Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="cc1a3-105">What is Azure Automation?</span></span>
<span data-ttu-id="cc1a3-106">[Azure Automation](https://azure.microsoft.com/services/automation/) är en Azure-tjänst som förenklar molnhantering via automatisering.</span><span class="sxs-lookup"><span data-stu-id="cc1a3-106">[Azure Automation](https://azure.microsoft.com/services/automation/) is an Azure service for simplifying cloud management through process automation.</span></span> <span data-ttu-id="cc1a3-107">Med Azure Automation kan tidskrävande, manuell, felbenägna och ofta återkommande uppgifter vara automatisk tooincrease tillförlitlighet, effektivitet och tid toovalue för din organisation.</span><span class="sxs-lookup"><span data-stu-id="cc1a3-107">Using Azure Automation, long-running, manual, error-prone, and frequently repeated tasks can be automated tooincrease reliability, efficiency, and time toovalue for your organization.</span></span>

<span data-ttu-id="cc1a3-108">Azure Automation ger en Körningsmotor för arbetsflöden för hög tillförlitliga och hög tillgänglighet som kan skalas toomeet dina behov när organisationen växer.</span><span class="sxs-lookup"><span data-stu-id="cc1a3-108">Azure Automation provides a highly-reliable and highly-available workflow execution engine that scales toomeet your needs as your organization grows.</span></span> <span data-ttu-id="cc1a3-109">I Azure Automation kan processer vara inletts manuellt av 3 part eller med schemalagda intervall så att aktiviteter inträffa exakt vid behov.</span><span class="sxs-lookup"><span data-stu-id="cc1a3-109">In Azure Automation, processes can be kicked off manually, by 3rd-party systems, or at scheduled intervals so that tasks happen exactly when needed.</span></span>

<span data-ttu-id="cc1a3-110">Lägre operativ tillsyn och frigöra IT / DevOps personal toofocus på arbete som lägger till affärsvärde genom att flytta dina molntjänster management uppgifter toobe körs automatiskt av Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="cc1a3-110">Lower operational overhead and free up IT / DevOps staff toofocus on work that adds business value by moving your cloud management tasks toobe run automatically by Azure Automation.</span></span>

## <a name="how-can-azure-automation-help-manage-azure-sql-databases"></a><span data-ttu-id="cc1a3-111">Hur Azure Automation kan hantera Azure SQL-databaser?</span><span class="sxs-lookup"><span data-stu-id="cc1a3-111">How can Azure Automation help manage Azure SQL databases?</span></span>
<span data-ttu-id="cc1a3-112">Azure SQL Database kan hanteras i Azure Automation med hjälp av hello [Azure SQL Database PowerShell-cmdlets](https://docs.microsoft.com/powershell/servicemanagement/azure.sqldatabase/v1.6.1/azure.sqldatabase/) som är tillgängliga i hello [Azure PowerShell verktygen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="cc1a3-112">Azure SQL Database can be managed in Azure Automation by using hello [Azure SQL Database PowerShell cmdlets](https://docs.microsoft.com/powershell/servicemanagement/azure.sqldatabase/v1.6.1/azure.sqldatabase/) that are available in hello [Azure PowerShell tools](/powershell/azure/overview).</span></span> <span data-ttu-id="cc1a3-113">Azure Automation har dessa Azure SQL Database PowerShell-cmdlets som är tillgängligt out of box hello, så att du kan utföra alla aktiviteter i hello tjänsten SQL DB management.</span><span class="sxs-lookup"><span data-stu-id="cc1a3-113">Azure Automation has these Azure SQL Database PowerShell cmdlets available out of hello box, so that you can perform all of your SQL DB management tasks within hello service.</span></span> <span data-ttu-id="cc1a3-114">Du kan också koppla dessa cmdlets i Azure Automation med hello-cmdlets för andra Azure-tjänster, tooautomate komplicerade uppgifter över Azure-tjänster och 3 part.</span><span class="sxs-lookup"><span data-stu-id="cc1a3-114">You can also pair these cmdlets in Azure Automation with hello cmdlets for other Azure services, tooautomate complex tasks across Azure services and 3rd party systems.</span></span>

<span data-ttu-id="cc1a3-115">Azure Automation har också hello möjlighet toocommunicate med SQL-servrar direkt, genom att utfärda SQL-kommandon med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="cc1a3-115">Azure Automation also has hello ability toocommunicate with SQL servers directly, by issuing SQL commands using PowerShell.</span></span>

<span data-ttu-id="cc1a3-116">Hej [Azure Automation runbook-galleriet](https://azure.microsoft.com/blog/2014/10/07/introducing-the-azure-automation-runbook-gallery/) innehåller en mängd produkten team och community runbooks tooget igång automatisering av hantering av Azure SQL-databaser, andra Azure-tjänster och 3 part.</span><span class="sxs-lookup"><span data-stu-id="cc1a3-116">hello [Azure Automation runbook gallery](https://azure.microsoft.com/blog/2014/10/07/introducing-the-azure-automation-runbook-gallery/) contains a variety of product team and community runbooks tooget started automating management of Azure SQL Databases, other Azure services, and 3rd party systems.</span></span> <span data-ttu-id="cc1a3-117">Galleriet runbooks är:</span><span class="sxs-lookup"><span data-stu-id="cc1a3-117">Gallery runbooks include:</span></span>

* [<span data-ttu-id="cc1a3-118">Kör SQL-frågor mot en SQL Server-databas</span><span class="sxs-lookup"><span data-stu-id="cc1a3-118">Run SQL queries against a SQL Server database</span></span>](https://gallery.technet.microsoft.com/scriptcenter/How-to-use-a-SQL-Command-be77f9d2)
* [<span data-ttu-id="cc1a3-119">Lodrätt skala (upp eller ned) en Azure SQL Database enligt ett schema</span><span class="sxs-lookup"><span data-stu-id="cc1a3-119">Vertically scale (up or down) an Azure SQL Database on a schedule</span></span>](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-e957354f)
* [<span data-ttu-id="cc1a3-120">Trunkera en SQLtabell om databasen närmar sig maximal storlek</span><span class="sxs-lookup"><span data-stu-id="cc1a3-120">Truncate a SQL table if its database approaches its maximum size</span></span>](https://gallery.technet.microsoft.com/scriptcenter/Azure-Automation-Your-SQL-30f8736b)
* [<span data-ttu-id="cc1a3-121">Indexera tabeller i en Azure SQL Database om de är mycket fragmenterat</span><span class="sxs-lookup"><span data-stu-id="cc1a3-121">Index tables in an Azure SQL Database if they are highly fragmented</span></span>](https://gallery.technet.microsoft.com/scriptcenter/Indexes-tables-in-an-Azure-73a2a8ea)

## <a name="next-steps"></a><span data-ttu-id="cc1a3-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cc1a3-122">Next steps</span></span>
<span data-ttu-id="cc1a3-123">Nu när du har lärt dig grunderna hello i Azure Automation och hur det kan vara används toomanage Azure SQL-databaser kan du följa dessa länkar toolearn mer om Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="cc1a3-123">Now that you've learned hello basics of Azure Automation and how it can be used toomanage Azure SQL databases, follow these links toolearn more about Azure Automation.</span></span>

* [<span data-ttu-id="cc1a3-124">Azure Automation-översikt</span><span class="sxs-lookup"><span data-stu-id="cc1a3-124">Azure Automation Overview</span></span>](../automation/automation-intro.md)
* [<span data-ttu-id="cc1a3-125">Min första Runbook</span><span class="sxs-lookup"><span data-stu-id="cc1a3-125">My first runbook</span></span>](../automation/automation-first-runbook-graphical.md)
* [<span data-ttu-id="cc1a3-126">Azure Automation-inlärningsguide</span><span class="sxs-lookup"><span data-stu-id="cc1a3-126">Azure Automation learning map</span></span>](https://azure.microsoft.com/documentation/learning-paths/automation/)
* [<span data-ttu-id="cc1a3-127">Azure Automation: Din SQL Agent hello moln</span><span class="sxs-lookup"><span data-stu-id="cc1a3-127">Azure Automation: Your SQL Agent in hello Cloud</span></span>](https://azure.microsoft.com/blog/2014/06/26/azure-automation-your-sql-agent-in-the-cloud/) 

