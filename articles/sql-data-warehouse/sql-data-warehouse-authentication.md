---
title: aaaAuthentication tooAzure SQL Data Warehouse | Microsoft Docs
description: Azure Active Directory (AAD) och SQL Server-autentisering tooAzure SQL Data Warehouse.
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: jhubbard
editor: 
tags: 
ms.assetid: fefaaa75-2d0c-4e5d-aadb-410342d1ad73
ms.service: sql-data-warehouse
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.custom: security
ms.date: 03/21/2017
ms.author: rortloff;barbkess
ms.openlocfilehash: 66673ce1d4761243755254c8f64de1078a0ea82e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="authentication-tooazure-sql-data-warehouse"></a><span data-ttu-id="0d2c7-103">Autentisering tooAzure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="0d2c7-103">Authentication tooAzure SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="0d2c7-104">Översikt över säkerheten</span><span class="sxs-lookup"><span data-stu-id="0d2c7-104">Security Overview</span></span>](sql-data-warehouse-overview-manage-security.md)
> * [<span data-ttu-id="0d2c7-105">Autentisering</span><span class="sxs-lookup"><span data-stu-id="0d2c7-105">Authentication</span></span>](sql-data-warehouse-authentication.md)
> * [<span data-ttu-id="0d2c7-106">Kryptering (Portal)</span><span class="sxs-lookup"><span data-stu-id="0d2c7-106">Encryption (Portal)</span></span>](sql-data-warehouse-encryption-tde.md)
> * [<span data-ttu-id="0d2c7-107">Kryptering (T-SQL)</span><span class="sxs-lookup"><span data-stu-id="0d2c7-107">Encryption (T-SQL)</span></span>](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

<span data-ttu-id="0d2c7-108">tooconnect tooSQL Data Warehouse, du måste ange säkerhetsreferenser för autentisering.</span><span class="sxs-lookup"><span data-stu-id="0d2c7-108">tooconnect tooSQL Data Warehouse, you must pass in security credentials for authentication purposes.</span></span> <span data-ttu-id="0d2c7-109">Vissa inställningar är konfigurerade som en del av upprätta sessionen frågan när en anslutning.</span><span class="sxs-lookup"><span data-stu-id="0d2c7-109">Upon establishing a connection, certain connection settings are configured as part of establishing your query session.</span></span>  

<span data-ttu-id="0d2c7-110">Mer information om säkerhet och hur tooenable anslutningar tooyour datalagret finns [skydda en databas i SQL Data Warehouse][Secure a database in SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="0d2c7-110">For more information on security and how tooenable connections tooyour data warehouse, see [Secure a database in SQL Data Warehouse][Secure a database in SQL Data Warehouse].</span></span>

## <a name="sql-authentication"></a><span data-ttu-id="0d2c7-111">SQL-autentisering</span><span class="sxs-lookup"><span data-stu-id="0d2c7-111">SQL authentication</span></span>
<span data-ttu-id="0d2c7-112">tooconnect tooSQL datalagret, måste du ange hello följande information:</span><span class="sxs-lookup"><span data-stu-id="0d2c7-112">tooconnect tooSQL Data Warehouse, you must provide hello following information:</span></span>

* <span data-ttu-id="0d2c7-113">Fullständigt kvalificerade servernamn</span><span class="sxs-lookup"><span data-stu-id="0d2c7-113">Fully qualified servername</span></span>
* <span data-ttu-id="0d2c7-114">Ange SQL-autentisering</span><span class="sxs-lookup"><span data-stu-id="0d2c7-114">Specify SQL authentication</span></span>
* <span data-ttu-id="0d2c7-115">Användarnamn</span><span class="sxs-lookup"><span data-stu-id="0d2c7-115">Username</span></span>
* <span data-ttu-id="0d2c7-116">Lösenord</span><span class="sxs-lookup"><span data-stu-id="0d2c7-116">Password</span></span>
* <span data-ttu-id="0d2c7-117">Standarddatabasen (valfritt)</span><span class="sxs-lookup"><span data-stu-id="0d2c7-117">Default database (optional)</span></span>

<span data-ttu-id="0d2c7-118">Din anslutning ansluter som standard toohello *master* databasen och inte databasen.</span><span class="sxs-lookup"><span data-stu-id="0d2c7-118">By default your connection connects toohello *master* database and not your user database.</span></span> <span data-ttu-id="0d2c7-119">tooconnect tooyour användardatabas, du kan välja toodo något av följande:</span><span class="sxs-lookup"><span data-stu-id="0d2c7-119">tooconnect tooyour user database, you can choose toodo one of two things:</span></span>

* <span data-ttu-id="0d2c7-120">Ange hello standarddatabasen när du registrerar din server med hello SQL Server Object Explorer i SSDT SSMS, eller i anslutningssträngen program.</span><span class="sxs-lookup"><span data-stu-id="0d2c7-120">Specify hello default database when registering your server with hello SQL Server Object Explorer in SSDT, SSMS, or in your application connection string.</span></span> <span data-ttu-id="0d2c7-121">Till exempel omfattar hello InitialCatalog parameter för en ODBC-anslutning.</span><span class="sxs-lookup"><span data-stu-id="0d2c7-121">For example, include hello InitialCatalog parameter for an ODBC connection.</span></span>
* <span data-ttu-id="0d2c7-122">Markera hello-databasen innan du skapar en session i SSDT.</span><span class="sxs-lookup"><span data-stu-id="0d2c7-122">Highlight hello user database before creating a session in SSDT.</span></span>

> [!NOTE]
> <span data-ttu-id="0d2c7-123">hello Transact-SQL-instruktionen **Använd mindatabas;** stöds inte för att ändra hello databasen för en anslutning.</span><span class="sxs-lookup"><span data-stu-id="0d2c7-123">hello Transact-SQL statement **USE MyDatabase;** is not supported for changing hello database for a connection.</span></span> <span data-ttu-id="0d2c7-124">Riktlinjer som ansluter tooSQL Data Warehouse med SSDT finns toohello [fråga med Visual Studio] [ Query with Visual Studio] artikel.</span><span class="sxs-lookup"><span data-stu-id="0d2c7-124">For guidance connecting tooSQL Data Warehouse with SSDT, refer toohello [Query with Visual Studio][Query with Visual Studio] article.</span></span>
> 
> 

## <a name="azure-active-directory-aad-authentication"></a><span data-ttu-id="0d2c7-125">Azure Active Directory (AAD)-autentisering</span><span class="sxs-lookup"><span data-stu-id="0d2c7-125">Azure Active Directory (AAD) authentication</span></span>
<span data-ttu-id="0d2c7-126">[Azure Active Directory] [ What is Azure Active Directory] autentisering är en mekanism för anslutning tooMicrosoft Azure SQL Data Warehouse med hjälp av identiteter i Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="0d2c7-126">[Azure Active Directory][What is Azure Active Directory] authentication is a mechanism of connecting tooMicrosoft Azure SQL Data Warehouse by using identities in Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="0d2c7-127">Med Azure Active Directory-autentisering, kan du centralt hantera hello identiteter för databasanvändare och andra Microsoft-tjänster på en central plats.</span><span class="sxs-lookup"><span data-stu-id="0d2c7-127">With Azure Active Directory authentication, you can centrally manage hello identities of database users and other Microsoft services in one central location.</span></span> <span data-ttu-id="0d2c7-128">Central ID-hantering och erbjuder en enda plats toomanage SQL Data Warehouse användare och förenklar hantering av behörighet.</span><span class="sxs-lookup"><span data-stu-id="0d2c7-128">Central ID management provides a single place toomanage SQL Data Warehouse users and simplifies permission management.</span></span> 

### <a name="benefits"></a><span data-ttu-id="0d2c7-129">Fördelar</span><span class="sxs-lookup"><span data-stu-id="0d2c7-129">Benefits</span></span>
<span data-ttu-id="0d2c7-130">Azure fördelar Active Directory:</span><span class="sxs-lookup"><span data-stu-id="0d2c7-130">Azure Active Directory benefits include:</span></span>

* <span data-ttu-id="0d2c7-131">Ger en alternativ tooSQL Server-autentisering.</span><span class="sxs-lookup"><span data-stu-id="0d2c7-131">Provides an alternative tooSQL Server authentication.</span></span>
* <span data-ttu-id="0d2c7-132">Hello ökande mängden av användaridentiteter stoppar över databasservrar.</span><span class="sxs-lookup"><span data-stu-id="0d2c7-132">Helps stop hello proliferation of user identities across database servers.</span></span>
* <span data-ttu-id="0d2c7-133">Tillåter lösenord rotation i en enda plats</span><span class="sxs-lookup"><span data-stu-id="0d2c7-133">Allows password rotation in a single place</span></span>
* <span data-ttu-id="0d2c7-134">Hantera databasbehörigheter använder externa (AAD)-grupper.</span><span class="sxs-lookup"><span data-stu-id="0d2c7-134">Manage database permissions using external (AAD) groups.</span></span>
* <span data-ttu-id="0d2c7-135">Eliminerar lagra lösenord genom att aktivera integrerad Windows-autentisering och andra former av autentisering som stöds av Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0d2c7-135">Eliminates storing passwords by enabling integrated Windows authentication and other forms of authentication supported by Azure Active Directory.</span></span>
* <span data-ttu-id="0d2c7-136">Använder finns databasen användare tooauthenticate identiteter på hello databasnivå.</span><span class="sxs-lookup"><span data-stu-id="0d2c7-136">Uses contained database users tooauthenticate identities at hello database level.</span></span>
* <span data-ttu-id="0d2c7-137">Stöder tokenbaserad autentisering för program som ansluter tooSQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="0d2c7-137">Supports token-based authentication for applications connecting tooSQL Data Warehouse.</span></span>
* <span data-ttu-id="0d2c7-138">Stöder multifaktorautentisering via Active Directory Universal autentisering för SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="0d2c7-138">Supports Multi-Factor authentication through Active Directory Universal Authentication for SQL Server Management Studio.</span></span> <span data-ttu-id="0d2c7-139">En beskrivning av Multi-Factor Authentication finns [SSMS stöd för Azure AD MFA med SQL Database och SQL Data Warehouse](../sql-database/sql-database-ssms-mfa-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="0d2c7-139">For a description of Multi-Factor Authentication, see [SSMS support for Azure AD MFA with SQL Database and SQL Data Warehouse](../sql-database/sql-database-ssms-mfa-authentication.md).</span></span>

> [!NOTE]
> <span data-ttu-id="0d2c7-140">Azure Active Directory är fortfarande relativt nytt och har vissa begränsningar.</span><span class="sxs-lookup"><span data-stu-id="0d2c7-140">Azure Active Directory is still relatively new and has some limitations.</span></span> <span data-ttu-id="0d2c7-141">tooensure att Azure Active Directory passar bra för din miljö finns [Azure AD-funktioner och begränsningar][Azure AD features and limitations], särskilt hello ytterligare överväganden.</span><span class="sxs-lookup"><span data-stu-id="0d2c7-141">tooensure that Azure Active Directory is a good fit for your environment, see [Azure AD features and limitations][Azure AD features and limitations], specifically hello Additional considerations.</span></span>
> 
> 

### <a name="configuration-steps"></a><span data-ttu-id="0d2c7-142">Konfigurationssteg</span><span class="sxs-lookup"><span data-stu-id="0d2c7-142">Configuration steps</span></span>
<span data-ttu-id="0d2c7-143">Följ dessa steg tooconfigure Azure Active Directory-autentisering.</span><span class="sxs-lookup"><span data-stu-id="0d2c7-143">Follow these steps tooconfigure Azure Active Directory authentication.</span></span>

1. <span data-ttu-id="0d2c7-144">Skapa och fylla i ett Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0d2c7-144">Create and populate an Azure Active Directory</span></span>
2. <span data-ttu-id="0d2c7-145">Valfritt: Koppla eller ändra hello active directory som associeras med din Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="0d2c7-145">Optional: Associate or change hello active directory that is currently associated with your Azure Subscription</span></span>
3. <span data-ttu-id="0d2c7-146">Skapa en Azure Active Directory-administratör för Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="0d2c7-146">Create an Azure Active Directory administrator for Azure SQL Data Warehouse.</span></span>
4. <span data-ttu-id="0d2c7-147">Konfigurera klientdatorer</span><span class="sxs-lookup"><span data-stu-id="0d2c7-147">Configure your client computers</span></span>
5. <span data-ttu-id="0d2c7-148">Skapa oberoende databasanvändare i din databas mappas tooAzure AD identiteter</span><span class="sxs-lookup"><span data-stu-id="0d2c7-148">Create contained database users in your database mapped tooAzure AD identities</span></span>
6. <span data-ttu-id="0d2c7-149">Ansluta tooyour data warehouse med hjälp av Azure AD identiteter</span><span class="sxs-lookup"><span data-stu-id="0d2c7-149">Connect tooyour data warehouse by using Azure AD identities</span></span>

<span data-ttu-id="0d2c7-150">Azure Active Directory-användare visas för närvarande inte i SSDT Object Explorer.</span><span class="sxs-lookup"><span data-stu-id="0d2c7-150">Currently Azure Active Directory users are not shown in SSDT Object Explorer.</span></span> <span data-ttu-id="0d2c7-151">Som en tillfällig lösning kan visa hello användare i [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).</span><span class="sxs-lookup"><span data-stu-id="0d2c7-151">As a workaround, view hello users in [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).</span></span>

### <a name="find-hello-details"></a><span data-ttu-id="0d2c7-152">Hello-information</span><span class="sxs-lookup"><span data-stu-id="0d2c7-152">Find hello details</span></span>
* <span data-ttu-id="0d2c7-153">hello steg tooconfigure och använda Azure Active Directory-autentisering är nästan identisk för Azure SQL Database och Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="0d2c7-153">hello steps tooconfigure and use Azure Active Directory authentication are nearly identical for Azure SQL Database and Azure SQL Data Warehouse.</span></span> <span data-ttu-id="0d2c7-154">Följ hello detaljerade anvisningarna i avsnittet om hello [ansluta tooSQL Database eller SQL Data Warehouse med hjälp av Azure Active Directory Authentication](../sql-database/sql-database-aad-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="0d2c7-154">Follow hello detailed steps in hello topic [Connecting tooSQL Database or SQL Data Warehouse By Using Azure Active Directory Authentication](../sql-database/sql-database-aad-authentication.md).</span></span>
* <span data-ttu-id="0d2c7-155">Skapa anpassade databasroller och Lägg till användare toohello roller.</span><span class="sxs-lookup"><span data-stu-id="0d2c7-155">Create custom database roles and add users toohello roles.</span></span> <span data-ttu-id="0d2c7-156">Ge detaljerade behörigheter toohello roller.</span><span class="sxs-lookup"><span data-stu-id="0d2c7-156">Then grant granular permissions toohello roles.</span></span> <span data-ttu-id="0d2c7-157">Mer information finns i [komma igång med motorn databasbehörighet](https://msdn.microsoft.com/library/mt667986.aspx).</span><span class="sxs-lookup"><span data-stu-id="0d2c7-157">For more information, see [Getting Started with Database Engine Permissions](https://msdn.microsoft.com/library/mt667986.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="0d2c7-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0d2c7-158">Next steps</span></span>
<span data-ttu-id="0d2c7-159">toostart fråga ditt data warehouse med Visual Studio och andra program, se [fråga med Visual Studio][Query with Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="0d2c7-159">toostart querying your data warehouse with Visual Studio and other applications, see [Query with Visual Studio][Query with Visual Studio].</span></span>

<!-- Article references -->
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md
[Query with Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
[What is Azure Active Directory]: ../active-directory/active-directory-whatis.md
[Azure AD features and limitations]: ../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations
