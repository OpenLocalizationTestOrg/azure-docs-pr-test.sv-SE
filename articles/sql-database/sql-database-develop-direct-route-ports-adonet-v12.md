---
title: "aaaPorts utöver 1433 för SQL-databas | Microsoft Docs"
description: "Klientanslutningar från ADO.NET tooAzure SQL-databas ibland kringgå hello proxy och interagera direkt med hello-databasen. Andra portar än 1433 blir viktiga."
services: sql-database
documentationcenter: 
author: MightyPen
manager: jhubbard
editor: 
ms.assetid: 3f17106a-92fd-4aa4-b6a9-1daa29421f64
ms.service: sql-database
ms.custom: develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2016
ms.author: sstein
ms.openlocfilehash: a35ff2d827ae3fa29b3ea855dbb7ed78583c82eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="ports-beyond-1433-for-adonet-45"></a><span data-ttu-id="db9bb-104">Portar utöver 1433 för ADO.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="db9bb-104">Ports beyond 1433 for ADO.NET 4.5</span></span>
<span data-ttu-id="db9bb-105">Det här avsnittet beskriver hello Azure SQL Database anslutningsbeteendet för klienter som använder ADO.NET 4.5 eller senare.</span><span class="sxs-lookup"><span data-stu-id="db9bb-105">This topic describes hello Azure SQL Database connection behavior for clients that use ADO.NET 4.5 or a later version.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="db9bb-106">Information om anslutningen arkitektur finns [Azure SQL Database connectivity arkitektur](sql-database-connectivity-architecture.md).</span><span class="sxs-lookup"><span data-stu-id="db9bb-106">For information about connectivity architecture, see [Azure SQL Database connectivity architecture](sql-database-connectivity-architecture.md).</span></span>
>

## <a name="outside-vs-inside"></a><span data-ttu-id="db9bb-107">Utanför eller innanför</span><span class="sxs-lookup"><span data-stu-id="db9bb-107">Outside vs inside</span></span>
<span data-ttu-id="db9bb-108">För anslutningar tooAzure SQL-databas måste först ber vi om klientprogrammet körs *utanför* eller *inuti* hello Azure-molnet gräns.</span><span class="sxs-lookup"><span data-stu-id="db9bb-108">For connections tooAzure SQL Database, we must first ask whether your client program runs *outside* or *inside* hello Azure cloud boundary.</span></span> <span data-ttu-id="db9bb-109">hello underavsnitt beskrivs två vanliga scenarier.</span><span class="sxs-lookup"><span data-stu-id="db9bb-109">hello subsections discuss two common scenarios.</span></span>

#### <a name="outside-client-runs-on-your-desktop-computer"></a><span data-ttu-id="db9bb-110">*Utanför:* klienten körs på datorn</span><span class="sxs-lookup"><span data-stu-id="db9bb-110">*Outside:* Client runs on your desktop computer</span></span>
<span data-ttu-id="db9bb-111">Port 1433 är endast hello-port som måste vara öppna på den stationära datorn som är värd för ditt SQL Database-klientprogram.</span><span class="sxs-lookup"><span data-stu-id="db9bb-111">Port 1433 is hello only port that must be open on your desktop computer that hosts your SQL Database client application.</span></span>

#### <a name="inside-client-runs-on-azure"></a><span data-ttu-id="db9bb-112">*Inre:* klienten körs på Azure</span><span class="sxs-lookup"><span data-stu-id="db9bb-112">*Inside:* Client runs on Azure</span></span>
<span data-ttu-id="db9bb-113">När klienten körs hello Azure-molnet gräns, använder vi kan anropa en *vägen* toointeract med hello SQL Database-server.</span><span class="sxs-lookup"><span data-stu-id="db9bb-113">When your client runs inside hello Azure cloud boundary, it uses what we can call a *direct route* toointeract with hello SQL Database server.</span></span> <span data-ttu-id="db9bb-114">När en anslutning har upprättats kan innebära ytterligare interaktion mellan hello-klienten och databasen ingen mellanprogram proxy.</span><span class="sxs-lookup"><span data-stu-id="db9bb-114">After a connection is established, further interactions between hello client and database involve no middleware proxy.</span></span>

<span data-ttu-id="db9bb-115">hello-sekvensen är följande:</span><span class="sxs-lookup"><span data-stu-id="db9bb-115">hello sequence is as follows:</span></span>

1. <span data-ttu-id="db9bb-116">ADO.NET 4.5 (eller senare) initierar en kort interaktion med hello Azure-molnet och tar emot ett dynamiskt identifierade portnummer.</span><span class="sxs-lookup"><span data-stu-id="db9bb-116">ADO.NET 4.5 (or later) initiates a brief interaction with hello Azure cloud, and receives a dynamically identified port number.</span></span>
   
   * <span data-ttu-id="db9bb-117">hello dynamiskt identifieras portnumret är i intervallet hello 11000 11999 eller 14000 14999.</span><span class="sxs-lookup"><span data-stu-id="db9bb-117">hello dynamically identified port number is in hello range of 11000-11999 or 14000-14999.</span></span>
2. <span data-ttu-id="db9bb-118">ADO.NET sedan ansluter toohello SQL Database-server direkt, utan mellanprogram mellan.</span><span class="sxs-lookup"><span data-stu-id="db9bb-118">ADO.NET then connects toohello SQL Database server directly, with no middleware in between.</span></span>
3. <span data-ttu-id="db9bb-119">Frågor skickas direkt toohello databasen och resultat returneras direkt toohello klienten.</span><span class="sxs-lookup"><span data-stu-id="db9bb-119">Queries are sent directly toohello database, and results are returned directly toohello client.</span></span>

<span data-ttu-id="db9bb-120">Se till att hello-port intervall med 11000 11999 och 14000 14999 på Azure klientdatorn lämnas tillgänglig för ADO.NET 4.5 klienten interaktioner med SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="db9bb-120">Ensure that hello port ranges of 11000-11999 and 14000-14999 on your Azure client machine are left available for ADO.NET 4.5 client interactions with SQL Database.</span></span>

* <span data-ttu-id="db9bb-121">I synnerhet måste portar i hello intervallet vara fria från andra utgående blockeringar.</span><span class="sxs-lookup"><span data-stu-id="db9bb-121">In particular, ports in hello range must be free of any other outbound blockers.</span></span>
* <span data-ttu-id="db9bb-122">På den virtuella datorn i Azure, hello **Windows-brandväggen med avancerad säkerhet** kontroller hello portinställningar.</span><span class="sxs-lookup"><span data-stu-id="db9bb-122">On your Azure VM, hello **Windows Firewall with Advanced Security** controls hello port settings.</span></span>
  
  * <span data-ttu-id="db9bb-123">Du kan använda hello [brandväggens användargränssnittet](http://msdn.microsoft.com/library/cc646023.aspx) tooadd en regel som du anger hello **TCP** protokollet tillsammans med ett portintervall med hello syntax som **11000 11999**.</span><span class="sxs-lookup"><span data-stu-id="db9bb-123">You can use hello [firewall's user interface](http://msdn.microsoft.com/library/cc646023.aspx) tooadd a rule for which you specify hello **TCP** protocol along with a port range with hello syntax like **11000-11999**.</span></span>

## <a name="version-clarifications"></a><span data-ttu-id="db9bb-124">Version förtydliganden</span><span class="sxs-lookup"><span data-stu-id="db9bb-124">Version clarifications</span></span>
<span data-ttu-id="db9bb-125">Det här avsnittet förklarar hello monikrar som refererar tooproduct versioner.</span><span class="sxs-lookup"><span data-stu-id="db9bb-125">This section clarifies hello monikers that refer tooproduct versions.</span></span> <span data-ttu-id="db9bb-126">Dessutom visas vissa pairings versioner mellan produkter.</span><span class="sxs-lookup"><span data-stu-id="db9bb-126">It also lists some pairings of versions between products.</span></span>

#### <a name="adonet"></a><span data-ttu-id="db9bb-127">ADO.NET</span><span class="sxs-lookup"><span data-stu-id="db9bb-127">ADO.NET</span></span>
* <span data-ttu-id="db9bb-128">ADO.NET 4.0 stöder hello 7.3 TDS-protokollet, men inte 7.4.</span><span class="sxs-lookup"><span data-stu-id="db9bb-128">ADO.NET 4.0 supports hello TDS 7.3 protocol, but not 7.4.</span></span>
* <span data-ttu-id="db9bb-129">ADO.NET 4.5 och senare stöder hello 7.4 TDS-protokollet.</span><span class="sxs-lookup"><span data-stu-id="db9bb-129">ADO.NET 4.5 and later supports hello TDS 7.4 protocol.</span></span>

## <a name="related-links"></a><span data-ttu-id="db9bb-130">Relaterade länkar</span><span class="sxs-lookup"><span data-stu-id="db9bb-130">Related links</span></span>
* <span data-ttu-id="db9bb-131">ADO.NET 4.6 gavs ut 20 juli 2015.</span><span class="sxs-lookup"><span data-stu-id="db9bb-131">ADO.NET 4.6 was released on July 20, 2015.</span></span> <span data-ttu-id="db9bb-132">En blogg meddelande från hello .NET-teamet finns [här](http://blogs.msdn.com/b/dotnet/archive/2015/07/20/announcing-net-framework-4-6.aspx).</span><span class="sxs-lookup"><span data-stu-id="db9bb-132">A blog announcement from hello .NET team is available [here](http://blogs.msdn.com/b/dotnet/archive/2015/07/20/announcing-net-framework-4-6.aspx).</span></span>
* <span data-ttu-id="db9bb-133">ADO.NET 4.5 gavs ut 15 augusti 2012.</span><span class="sxs-lookup"><span data-stu-id="db9bb-133">ADO.NET 4.5 was released on August 15, 2012.</span></span> <span data-ttu-id="db9bb-134">En blogg meddelande från hello .NET-teamet finns [här](http://blogs.msdn.com/b/dotnet/archive/2012/08/15/announcing-the-release-of-net-framework-4-5-rtm-product-and-source-code.aspx).</span><span class="sxs-lookup"><span data-stu-id="db9bb-134">A blog announcement from hello .NET team is available [here](http://blogs.msdn.com/b/dotnet/archive/2012/08/15/announcing-the-release-of-net-framework-4-5-rtm-product-and-source-code.aspx).</span></span>
  
  * <span data-ttu-id="db9bb-135">Det finns ett blogginlägg om ADO.NET 4.5.1 [här](http://blogs.msdn.com/b/dotnet/archive/2013/06/26/announcing-the-net-framework-4-5-1-preview.aspx).</span><span class="sxs-lookup"><span data-stu-id="db9bb-135">A blog post about ADO.NET 4.5.1 is available [here](http://blogs.msdn.com/b/dotnet/archive/2013/06/26/announcing-the-net-framework-4-5-1-preview.aspx).</span></span>
* [<span data-ttu-id="db9bb-136">TDS-protokollet versionslista</span><span class="sxs-lookup"><span data-stu-id="db9bb-136">TDS protocol version list</span></span>](http://www.freetds.org/userguide/tdshistory.htm)
* [<span data-ttu-id="db9bb-137">Översikt över SQL Database-utveckling</span><span class="sxs-lookup"><span data-stu-id="db9bb-137">SQL Database Development Overview</span></span>](sql-database-develop-overview.md)
* [<span data-ttu-id="db9bb-138">Azure SQL Database-brandvägg</span><span class="sxs-lookup"><span data-stu-id="db9bb-138">Azure SQL Database firewall</span></span>](sql-database-firewall-configure.md)
* [<span data-ttu-id="db9bb-139">Så här: konfigurera brandväggsinställningar på SQL-databas</span><span class="sxs-lookup"><span data-stu-id="db9bb-139">How to: Configure firewall settings on SQL Database</span></span>](sql-database-configure-firewall-settings.md)

