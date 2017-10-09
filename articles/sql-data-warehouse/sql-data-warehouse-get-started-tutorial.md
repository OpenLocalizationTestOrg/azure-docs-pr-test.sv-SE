---
title: "Kom igång aaaAzure SQL Data Warehouse - kursen | Microsoft Docs"
description: "Den här kursen lär du dig hur tooprovision och Läs in data till Azure SQL Data Warehouse. Du får också lära dig hello grunderna om skalning, pausa och justera."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: johnmac
editor: barbkess
ms.assetid: 52DFC191-E094-4B04-893F-B64D5828A900
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: quickstart
ms.date: 01/26/2017
ms.author: elbutter;barbkess
ms.openlocfilehash: edd2a21b0fe49ca8e9792c7c512310339a822c55
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-sql-data-warehouse"></a><span data-ttu-id="7d70f-104">Komma igång med SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="7d70f-104">Get started with SQL Data Warehouse</span></span>

<span data-ttu-id="7d70f-105">Den här kursen visar hur tooprovision och Läs in data till Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="7d70f-105">This tutorial shows how tooprovision and load data into Azure SQL Data Warehouse.</span></span> <span data-ttu-id="7d70f-106">Du får också lära dig hello grunderna om skalning, pausa och justera.</span><span class="sxs-lookup"><span data-stu-id="7d70f-106">You’ll also learn hello basics about scaling, pausing, and tuning.</span></span> <span data-ttu-id="7d70f-107">När du är klar kan du vara redo tooquery och utforska dina data warehouse.</span><span class="sxs-lookup"><span data-stu-id="7d70f-107">When you’re finished, you’ll be ready tooquery and explore your data warehouse.</span></span>

<span data-ttu-id="7d70f-108">**Uppskattad tid toocomplete:** det här är en vägledning för slutpunkt till slutpunkt med exempelkod som tar cirka 30 minuter toocomplete när hello förutsättningar är uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="7d70f-108">**Estimated time toocomplete:** This is an end-to-end tutorial with example code that takes about 30 minutes toocomplete once you have met hello prerequisites.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="7d70f-109">Krav</span><span class="sxs-lookup"><span data-stu-id="7d70f-109">Prerequisites</span></span>

<span data-ttu-id="7d70f-110">hello kursen förutsätter att du är bekant med grundläggande koncept för SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="7d70f-110">hello tutorial assumes you are familiar with SQL Data Warehouse basic concepts.</span></span> <span data-ttu-id="7d70f-111">En introduktion finns i [Vad är SQL Data Warehouse?](sql-data-warehouse-overview-what-is.md)</span><span class="sxs-lookup"><span data-stu-id="7d70f-111">If you need an introduction, see [What is SQL Data Warehouse?](sql-data-warehouse-overview-what-is.md)</span></span> 

### <a name="sign-up-for-microsoft-azure"></a><span data-ttu-id="7d70f-112">Registrera dig för Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="7d70f-112">Sign up for Microsoft Azure</span></span>
<span data-ttu-id="7d70f-113">Om du inte redan har ett Microsoft Azure-konto, måste toosign för en toouse den här tjänsten.</span><span class="sxs-lookup"><span data-stu-id="7d70f-113">If you don't already have a Microsoft Azure account, you need toosign up for one toouse this service.</span></span> <span data-ttu-id="7d70f-114">Om du redan har ett konto kan du hoppa över det här steget.</span><span class="sxs-lookup"><span data-stu-id="7d70f-114">If you already have an account, you may skip this step.</span></span> 

1. <span data-ttu-id="7d70f-115">Navigera toohello konto sidor [https://azure.microsoft.com/account/](https://azure.microsoft.com/account/)</span><span class="sxs-lookup"><span data-stu-id="7d70f-115">Navigate toohello account pages [https://azure.microsoft.com/account/](https://azure.microsoft.com/account/)</span></span>
2. <span data-ttu-id="7d70f-116">Skapa ett kostnadsfritt Azure-konto eller köp ett konto.</span><span class="sxs-lookup"><span data-stu-id="7d70f-116">Create a free Azure account, or purchase an account.</span></span>
3. <span data-ttu-id="7d70f-117">Följ instruktionerna för hello</span><span class="sxs-lookup"><span data-stu-id="7d70f-117">Follow hello instructions</span></span>

### <a name="install-appropriate-sql-client-drivers-and-tools"></a><span data-ttu-id="7d70f-118">Installera rätt verktyg och drivrutin för SQL-klienten</span><span class="sxs-lookup"><span data-stu-id="7d70f-118">Install appropriate SQL client drivers and tools</span></span>

<span data-ttu-id="7d70f-119">De flesta SQL-klientverktygen kan ansluta tooSQL Data Warehouse med hjälp av ADO.NET, JDBC eller ODBC.</span><span class="sxs-lookup"><span data-stu-id="7d70f-119">Most SQL client tools can connect tooSQL Data Warehouse by using JDBC, ODBC, or ADO.NET.</span></span> <span data-ttu-id="7d70f-120">På grund av toohello stort antal T-SQL-funktioner som har stöd för SQL Data Warehouse, är vissa klientprogram inte helt kompatibel med SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="7d70f-120">Due toohello large number of T-SQL features that SQL Data Warehouse supports, some client applications are not fully compatible with SQL Data Warehouse.</span></span>

<span data-ttu-id="7d70f-121">Om du kör ett Windows-operativsystem rekommenderar vi att du använder antingen [Visual Studio] eller [SQL Server Management Studio].</span><span class="sxs-lookup"><span data-stu-id="7d70f-121">If you are running a Windows operating system, we recommend using either [Visual Studio] or [SQL Server Management Studio].</span></span>

[!INCLUDE [Create a new logical server](../../includes/sql-data-warehouse-create-logical-server.md)] 

[!INCLUDE [SQL Database create server](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="create-a-sql-data-warehouse"></a><span data-ttu-id="7d70f-122">Skapa ett SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="7d70f-122">Create a SQL Data Warehouse</span></span>

<span data-ttu-id="7d70f-123">En SQL Data Warehouse är en särskild typ av databas som är avsedd för MPP (massively parallel processing).</span><span class="sxs-lookup"><span data-stu-id="7d70f-123">A SQL Data Warehouse is a special type of database that is designed for massively parallel processing.</span></span> <span data-ttu-id="7d70f-124">hello databasen distribueras över flera noder och bearbetar frågor parallellt.</span><span class="sxs-lookup"><span data-stu-id="7d70f-124">hello database is distributed across multiple nodes and processes queries in parallel.</span></span> <span data-ttu-id="7d70f-125">SQL Data Warehouse har en control-noden som samordnar hello aktiviteter för alla hello-noder.</span><span class="sxs-lookup"><span data-stu-id="7d70f-125">SQL Data Warehouse has a control node that orchestrates hello activities of all hello nodes.</span></span> <span data-ttu-id="7d70f-126">själva hello noderna använda SQL-databas toomanage dina data.</span><span class="sxs-lookup"><span data-stu-id="7d70f-126">hello nodes themselves use SQL Database toomanage your data.</span></span>  

> [!NOTE]
> <span data-ttu-id="7d70f-127">Att skapa ett SQL Data Warehouse kan resultera i en ny fakturerbar tjänst.</span><span class="sxs-lookup"><span data-stu-id="7d70f-127">Creating a SQL Data Warehouse might result in a new billable service.</span></span>  <span data-ttu-id="7d70f-128">Mer information finns på [prissidan för SQL Data Warehouse](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).</span><span class="sxs-lookup"><span data-stu-id="7d70f-128">For more information, see [SQL Data Warehouse pricing](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).</span></span>
>

### <a name="create-a-data-warehouse"></a><span data-ttu-id="7d70f-129">Skapa ett datalager</span><span class="sxs-lookup"><span data-stu-id="7d70f-129">Create a data warehouse</span></span>

1. <span data-ttu-id="7d70f-130">Logga in på hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7d70f-130">Sign into hello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="7d70f-131">Klicka på **Nytt** > **Databaser** > **SQL Data Warehouse**.</span><span class="sxs-lookup"><span data-stu-id="7d70f-131">Click **New** > **Databases** > **SQL Data Warehouse**.</span></span>

    <span data-ttu-id="7d70f-132">![NewBlade](../../includes/media/sql-data-warehouse-create-dw/blade-click-new.png) ![SelectDW](../../includes/media/sql-data-warehouse-create-dw/blade-select-dw.png)</span><span class="sxs-lookup"><span data-stu-id="7d70f-132">![NewBlade](../../includes/media/sql-data-warehouse-create-dw/blade-click-new.png) ![SelectDW](../../includes/media/sql-data-warehouse-create-dw/blade-select-dw.png)</span></span>

3. <span data-ttu-id="7d70f-133">Fyll i distributionsinformationen</span><span class="sxs-lookup"><span data-stu-id="7d70f-133">Fill out deployment details</span></span>

    <span data-ttu-id="7d70f-134">**Databasnamn**: Välj önskat namn.</span><span class="sxs-lookup"><span data-stu-id="7d70f-134">**Database Name**: Pick anything you'd like.</span></span> <span data-ttu-id="7d70f-135">Om du har flera datalager, rekommenderar vi ditt namn som innehåller information om till exempel hello region, miljö, till exempel *mydw-westus-1-test*.</span><span class="sxs-lookup"><span data-stu-id="7d70f-135">If you have multiple data warehouses, we recommend your names include details such as hello region, environment, for example *mydw-westus-1-test*.</span></span>

    <span data-ttu-id="7d70f-136">**Prenumeration**: Din Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="7d70f-136">**Subscription**: Your Azure subscription</span></span>

    <span data-ttu-id="7d70f-137">**Resursgrupp**: Skapa en resursgrupp eller använd en befintlig resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="7d70f-137">**Resource Group**: Create a resource group or use an existing resource group.</span></span>
    > [!NOTE]
    > <span data-ttu-id="7d70f-138">Resursgrupper är användbara för resursadministration, till exempel för att styra åtkomstkontroll och mallbaserad distribution.</span><span class="sxs-lookup"><span data-stu-id="7d70f-138">Resource groups are useful for resource administration such as scoping access control and templated deployment.</span></span> <span data-ttu-id="7d70f-139">Läs mer om Azure-resursgrupper och bästa praxis [här](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups)</span><span class="sxs-lookup"><span data-stu-id="7d70f-139">Read more about Azure resource groups and best practices [here](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups)</span></span>

    <span data-ttu-id="7d70f-140">**Källa**: Tom databas</span><span class="sxs-lookup"><span data-stu-id="7d70f-140">**Source**: Blank Database</span></span>

    <span data-ttu-id="7d70f-141">**Server**: Välj hello-server som du skapade i [krav].</span><span class="sxs-lookup"><span data-stu-id="7d70f-141">**Server**: Select hello server you created in [Prerequisites].</span></span>

    <span data-ttu-id="7d70f-142">**Sorteringen**: lämna hello Standardsortering SQL_Latin1_General_CP1_CI_AS.</span><span class="sxs-lookup"><span data-stu-id="7d70f-142">**Collation**: Leave hello default collation SQL_Latin1_General_CP1_CI_AS.</span></span>

    <span data-ttu-id="7d70f-143">**Välj prestanda**: Vi rekommenderar att börja med hello standard 400DWU.</span><span class="sxs-lookup"><span data-stu-id="7d70f-143">**Select performance**: We recommend starting with hello standard 400DWU.</span></span>

4. <span data-ttu-id="7d70f-144">Välj **PIN-kod toodashboard** ![tooDashboard PIN-kod](./media/sql-data-warehouse-get-started-tutorial/pin-to-dashboard.png)</span><span class="sxs-lookup"><span data-stu-id="7d70f-144">Choose **Pin toodashboard** ![Pin tooDashboard](./media/sql-data-warehouse-get-started-tutorial/pin-to-dashboard.png)</span></span>

5. <span data-ttu-id="7d70f-145">Sitta tillbaka och vänta på din data warehouse toodeploy!</span><span class="sxs-lookup"><span data-stu-id="7d70f-145">Sit back and wait for your data warehouse toodeploy!</span></span> <span data-ttu-id="7d70f-146">Det är normalt för den här processen tootake flera minuter.</span><span class="sxs-lookup"><span data-stu-id="7d70f-146">It's normal for this process tootake several minutes.</span></span> <span data-ttu-id="7d70f-147">hello-portalen meddelar dig när dina data warehouse är klar toouse.</span><span class="sxs-lookup"><span data-stu-id="7d70f-147">hello portal notifies you when your data warehouse is ready toouse.</span></span> 

## <a name="connect-toosql-data-warehouse"></a><span data-ttu-id="7d70f-148">Ansluta tooSQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="7d70f-148">Connect tooSQL Data Warehouse</span></span>

<span data-ttu-id="7d70f-149">Den här kursen använder SQL Server Management Studio (SSMS) tooconnect toohello-datalagret.</span><span class="sxs-lookup"><span data-stu-id="7d70f-149">This tutorial uses SQL Server Management Studio (SSMS) tooconnect toohello data warehouse.</span></span> <span data-ttu-id="7d70f-150">Du kan ansluta tooSQL datalagret via anslutningarna stöds: ADO.NET, JDBC, ODBC- och PHP.</span><span class="sxs-lookup"><span data-stu-id="7d70f-150">You can connect tooSQL Data Warehouse through these supported connectors: ADO.NET, JDBC, ODBC, and PHP.</span></span> <span data-ttu-id="7d70f-151">Tänk på att funktionaliteten kan vara begränsad med verktyg som inte stöds av Microsoft.</span><span class="sxs-lookup"><span data-stu-id="7d70f-151">Remember, functionality might be limited for tools that are not supported by Microsoft.</span></span>


### <a name="get-connection-information"></a><span data-ttu-id="7d70f-152">Hämta anslutningsinformation</span><span class="sxs-lookup"><span data-stu-id="7d70f-152">Get connection information</span></span>

<span data-ttu-id="7d70f-153">tooconnect tooyour datalagret, behöver du tooconnect via hello logiska SQLServer som du skapade i [krav].</span><span class="sxs-lookup"><span data-stu-id="7d70f-153">tooconnect tooyour data warehouse, you need tooconnect through hello logical SQL server you created in [Prerequisites].</span></span>

1. <span data-ttu-id="7d70f-154">Välj ditt data warehouse hello instrumentpanel eller söker efter det i dina resurser.</span><span class="sxs-lookup"><span data-stu-id="7d70f-154">Select your data warehouse from hello dashboard or search for it in your resources.</span></span>

    ![Instrumentpanel för SQL Data Warehouse](./media/sql-data-warehouse-get-started-tutorial/sql-dw-dashboard.png)

2. <span data-ttu-id="7d70f-156">Hitta hello fullständigt namn för hello logiska SQLServer.</span><span class="sxs-lookup"><span data-stu-id="7d70f-156">Find hello full name for hello logical SQL server.</span></span>

    ![Välj servernamn](./media/sql-data-warehouse-get-started-tutorial/select-server.png)

3. <span data-ttu-id="7d70f-158">Öppna SSMS och använder object explorer tooconnect toothis server med administratörsautentiseringsuppgifter för hello server du skapade i [krav]</span><span class="sxs-lookup"><span data-stu-id="7d70f-158">Open SSMS and use object explorer tooconnect toothis server using hello server admin credentials you created in [Prerequisites]</span></span>

    ![Anslut med SSMS](./media/sql-data-warehouse-get-started-tutorial/ssms-connect.png)

<span data-ttu-id="7d70f-160">Om allt går korrekt, bör du nu anslutet tooyour logisk SQL-server.</span><span class="sxs-lookup"><span data-stu-id="7d70f-160">If all goes correctly, you should now be connected tooyour logical SQL server.</span></span> <span data-ttu-id="7d70f-161">Eftersom du har loggat in som hello-administratören kan ansluta du tooany databasen hos hello-servern, inklusive hello master-databasen.</span><span class="sxs-lookup"><span data-stu-id="7d70f-161">Since you logged in as hello server admin, you can connect tooany database hosted by hello server, including hello master database.</span></span> 

<span data-ttu-id="7d70f-162">Det finns bara en server-administratörskontot och hello har de flesta behörigheter för alla användare.</span><span class="sxs-lookup"><span data-stu-id="7d70f-162">There is only one server admin account and it has hello most privileges of any user.</span></span> <span data-ttu-id="7d70f-163">Var noga med att inte tooallow för många personer i din organisation tooknow hello administratörslösenord.</span><span class="sxs-lookup"><span data-stu-id="7d70f-163">Be careful not tooallow too many people in your organization tooknow hello admin password.</span></span> 

<span data-ttu-id="7d70f-164">Du kan också ha ett administratörskonto för Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="7d70f-164">You can also have an Azure active directory admin account.</span></span> <span data-ttu-id="7d70f-165">Vi tillhandahålla inte hello informationen här.</span><span class="sxs-lookup"><span data-stu-id="7d70f-165">We don't provide hello details here.</span></span> <span data-ttu-id="7d70f-166">Om du vill toolearn mer om hur du använder Azure Active Directory-autentisering, se [Azure AD authentication](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication).</span><span class="sxs-lookup"><span data-stu-id="7d70f-166">If you want toolearn more about using Azure Active Directory authentication, see [Azure AD authentication](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication).</span></span>

<span data-ttu-id="7d70f-167">Därefter skapar vi ytterligare inloggningar och användare.</span><span class="sxs-lookup"><span data-stu-id="7d70f-167">Next, we explore creating additional logins and users.</span></span>


## <a name="create-a-database-user"></a><span data-ttu-id="7d70f-168">Skapa en databasanvändare</span><span class="sxs-lookup"><span data-stu-id="7d70f-168">Create a database user</span></span>

<span data-ttu-id="7d70f-169">I det här steget skapar du en användare konto tooaccess ditt data warehouse.</span><span class="sxs-lookup"><span data-stu-id="7d70f-169">In this step, you create a user account tooaccess your data warehouse.</span></span> <span data-ttu-id="7d70f-170">Vi också visar hur toogive som användaren hello möjlighet toorun frågor med stora mängder minne och processorresurser.</span><span class="sxs-lookup"><span data-stu-id="7d70f-170">We also show you how toogive that user hello ability toorun queries with a large amount of memory and CPU resources.</span></span>

### <a name="notes-about-resource-classes-for-allocating-resources-tooqueries"></a><span data-ttu-id="7d70f-171">Information om resursklasser för att fördela resurser tooqueries</span><span class="sxs-lookup"><span data-stu-id="7d70f-171">Notes about resource classes for allocating resources tooqueries</span></span>

- <span data-ttu-id="7d70f-172">tookeep dina data är säkra, Använd inte hello server admin toorun frågor på produktionsdatabaserna.</span><span class="sxs-lookup"><span data-stu-id="7d70f-172">tookeep your data safe, don't use hello server admin toorun queries on your production databases.</span></span> <span data-ttu-id="7d70f-173">Det har hello de flesta behörigheter för alla användare och använder den tooperform åtgärder på användardata placerar dina data i fara.</span><span class="sxs-lookup"><span data-stu-id="7d70f-173">It has hello most privileges of any user and using it tooperform operations on user data puts your data at risk.</span></span> <span data-ttu-id="7d70f-174">Dessutom eftersom hello serveradministratören är avsedd tooperform hanteringsåtgärder, körs operations med bara en liten fördelningen av minne och processorresurser.</span><span class="sxs-lookup"><span data-stu-id="7d70f-174">Also, since hello server admin is meant tooperform management operations, it runs operations with only a small allocation of memory and CPU resources.</span></span> 

- <span data-ttu-id="7d70f-175">SQL Data Warehouse använder fördefinierade databasroller som kallas resursklasser, tooallocate olika mängder minne, CPU-resurser och samtidighet fack toousers.</span><span class="sxs-lookup"><span data-stu-id="7d70f-175">SQL Data Warehouse uses pre-defined database roles, called resource classes, tooallocate different amounts of memory, CPU resources, and concurrency slots toousers.</span></span> <span data-ttu-id="7d70f-176">Varje användare kan tillhöra tooa small, medium, stora eller extra stor resursklassen.</span><span class="sxs-lookup"><span data-stu-id="7d70f-176">Each user can belong tooa small, medium, large, or extra-large resource class.</span></span> <span data-ttu-id="7d70f-177">hello användarens resursklassen bestämmer hello resurser hello användaren har toorun frågor och läsa in åtgärder.</span><span class="sxs-lookup"><span data-stu-id="7d70f-177">hello user's resource class determines hello resources hello user has toorun queries and load operations.</span></span>

- <span data-ttu-id="7d70f-178">För optimala datakomprimering måste hello användaren tooload med stor eller extra stor resursallokeringar.</span><span class="sxs-lookup"><span data-stu-id="7d70f-178">For optimal data compression, hello user may need tooload with large or extra large resource allocations.</span></span> <span data-ttu-id="7d70f-179">Läs mer om resursklasser [här](./sql-data-warehouse-develop-concurrency.md#resource-classes):</span><span class="sxs-lookup"><span data-stu-id="7d70f-179">Read more about resource classes [here](./sql-data-warehouse-develop-concurrency.md#resource-classes):</span></span>

### <a name="create-an-account-that-can-control-a-database"></a><span data-ttu-id="7d70f-180">Skapa ett konto som kan styra en databas</span><span class="sxs-lookup"><span data-stu-id="7d70f-180">Create an account that can control a database</span></span>

<span data-ttu-id="7d70f-181">Eftersom du är inloggad i hello-administratören ha behörigheter toocreate inloggningar och användare.</span><span class="sxs-lookup"><span data-stu-id="7d70f-181">Since you are currently logged in as hello server admin you have permissions toocreate logins and users.</span></span>

1. <span data-ttu-id="7d70f-182">Med SSMS eller någon annan frågeklient öppnar du en ny fråga för **huvuddatabasen**.</span><span class="sxs-lookup"><span data-stu-id="7d70f-182">Using SSMS or another query client, open a new query for **master**.</span></span>

    ![Ny fråga mot huvuddatabas](./media/sql-data-warehouse-get-started-tutorial/query-on-server.png)

    ![Ny fråga mot huvuddatabas1](./media/sql-data-warehouse-get-started-tutorial/query-on-master.png)

2. <span data-ttu-id="7d70f-185">Kör det här kommandot T-SQL-toocreate i hello frågefönstret en inloggning med namnet MedRCLogin och användare med namnet LoadingUser.</span><span class="sxs-lookup"><span data-stu-id="7d70f-185">In hello query window, run this T-SQL command toocreate a login named MedRCLogin and user named LoadingUser.</span></span> <span data-ttu-id="7d70f-186">Den här inloggningen kan ansluta toohello logisk SQL-server.</span><span class="sxs-lookup"><span data-stu-id="7d70f-186">This login can connect toohello logical SQL server.</span></span>

    ```sql
    CREATE LOGIN MedRCLogin WITH PASSWORD = 'a123reallySTRONGpassword!';
    CREATE USER LoadingUser FOR LOGIN MedRCLogin;
    ```

3. <span data-ttu-id="7d70f-187">Nu frågar hello *SQL Data Warehouse-databas*, skapa en databasanvändare baserat på hello inloggning som du skapade tooaccess och utföra åtgärder på hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="7d70f-187">Now querying hello *SQL Data Warehouse database*, create a database user based on hello login you created tooaccess and perform operations on hello database.</span></span>

    ```sql
    CREATE USER LoadingUser FOR LOGIN MedRCLogin;
    ```

4. <span data-ttu-id="7d70f-188">Ge hello användare kontroll behörigheter toohello databasen kallas NYT.</span><span class="sxs-lookup"><span data-stu-id="7d70f-188">Give hello database user control permissions toohello database called NYT.</span></span> 

    ```sql
    GRANT CONTROL ON DATABASE::[NYT] tooLoadingUser;
    ```
    > [!NOTE]
    > <span data-ttu-id="7d70f-189">Om databasnamnet är bindestreck vara säker på att toowrap den inom parentes!</span><span class="sxs-lookup"><span data-stu-id="7d70f-189">If your database name has hyphens in it, be sure toowrap it in brackets!</span></span> 
    >

### <a name="give-hello-user-medium-resource-allocations"></a><span data-ttu-id="7d70f-190">Ge hello användaren medelhög resursallokeringar</span><span class="sxs-lookup"><span data-stu-id="7d70f-190">Give hello user medium resource allocations</span></span>

1. <span data-ttu-id="7d70f-191">Kör den här T-SQL-kommandot toomake detta till en medlem av hello medelhög resursklassen, som kallas mediumrc.</span><span class="sxs-lookup"><span data-stu-id="7d70f-191">Run this T-SQL command toomake it a member of hello medium resource class, which is called mediumrc.</span></span> 

    ```sql
    EXEC sp_addrolemember 'mediumrc', 'LoadingUser';
    ```
    > [!NOTE]
    > <span data-ttu-id="7d70f-192">Klicka på [här](sql-data-warehouse-develop-concurrency.md#resource-classes) toolearn mer om samtidighet och resursen!</span><span class="sxs-lookup"><span data-stu-id="7d70f-192">Click [here](sql-data-warehouse-develop-concurrency.md#resource-classes) toolearn more about concurrency and resource classes!</span></span> 
    >

2. <span data-ttu-id="7d70f-193">Ansluta toohello logisk server med hello nya autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="7d70f-193">Connect toohello logical server with hello new credentials</span></span>

    ![Logga in med ny inloggning](./media/sql-data-warehouse-get-started-tutorial/new-login.png)


## <a name="load-data-from-azure-blob-storage"></a><span data-ttu-id="7d70f-195">Läsa in data från Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="7d70f-195">Load data from Azure blob storage</span></span>

<span data-ttu-id="7d70f-196">Du är nu redo tooload data till data warehouse.</span><span class="sxs-lookup"><span data-stu-id="7d70f-196">You are now ready tooload data into your data warehouse.</span></span> <span data-ttu-id="7d70f-197">Det här steget visar hur tooload New York City taxi cab data från en offentlig Azure storage blob.</span><span class="sxs-lookup"><span data-stu-id="7d70f-197">This step shows you how tooload New York City taxi cab data from a public Azure storage blob.</span></span> 

- <span data-ttu-id="7d70f-198">Ett vanligt sätt tooload data till SQL Data Warehouse är toofirst flytta hello data tooAzure blob-lagring och läsa in den i ditt data warehouse.</span><span class="sxs-lookup"><span data-stu-id="7d70f-198">A common way tooload data into SQL Data Warehouse is toofirst move hello data tooAzure blob storage, and then load it into your data warehouse.</span></span> <span data-ttu-id="7d70f-199">toomake den enklare toounderstand hur tooload, har vi New York taxi cab-data som redan finns i en offentlig Azure storage blob.</span><span class="sxs-lookup"><span data-stu-id="7d70f-199">toomake it easier toounderstand how tooload, we have New York taxi cab data already hosted in a public Azure storage blob.</span></span> 

- <span data-ttu-id="7d70f-200">För framtida bruk, toolearn hur tooget data-tooAzure blob-lagring eller tooload den direkt från din datakälla till SQL Data Warehouse finns hello [översikt över inläsning](sql-data-warehouse-overview-load.md).</span><span class="sxs-lookup"><span data-stu-id="7d70f-200">For future reference, toolearn how tooget your data tooAzure blob storage or tooload it directly from your source into SQL Data Warehouse, see hello [loading overview](sql-data-warehouse-overview-load.md).</span></span>


### <a name="define-external-data"></a><span data-ttu-id="7d70f-201">Definiera externa data</span><span class="sxs-lookup"><span data-stu-id="7d70f-201">Define external data</span></span>

1. <span data-ttu-id="7d70f-202">Skapa en huvudnyckel.</span><span class="sxs-lookup"><span data-stu-id="7d70f-202">Create a master key.</span></span> <span data-ttu-id="7d70f-203">Du behöver bara toocreate en huvudnyckel en gång per databas.</span><span class="sxs-lookup"><span data-stu-id="7d70f-203">You only need toocreate a master key once per database.</span></span> 

    ```sql
    CREATE MASTER KEY;
    ```

2. <span data-ttu-id="7d70f-204">Definiera hello placering av hello Azure blob som innehåller data som hello taxi CAB-filen.</span><span class="sxs-lookup"><span data-stu-id="7d70f-204">Define hello location of hello Azure blob that contains hello taxi cab data.</span></span>  

    ```sql
    CREATE EXTERNAL DATA SOURCE NYTPublic
    WITH
    (
        TYPE = Hadoop,
        LOCATION = 'wasbs://2013@nytpublic.blob.core.windows.net/'
    );
    ```

3. <span data-ttu-id="7d70f-205">Ange hello externt filformat</span><span class="sxs-lookup"><span data-stu-id="7d70f-205">Define hello external file formats</span></span>

    <span data-ttu-id="7d70f-206">Hej ```CREATE EXTERNAL FILE FORMAT``` kommandot är används toospecify formatet för filer som innehåller hello externa data.</span><span class="sxs-lookup"><span data-stu-id="7d70f-206">hello ```CREATE EXTERNAL FILE FORMAT``` command is used toospecify the format of files that contain hello external data.</span></span> <span data-ttu-id="7d70f-207">De innehåller text som avgränsas med ett eller flera tecken som kallas avgränsare.</span><span class="sxs-lookup"><span data-stu-id="7d70f-207">They contain text separated by one or more characters called delimiters.</span></span> <span data-ttu-id="7d70f-208">I demonstrationssyfte lagras hello taxi cab data som komprimerade data såväl som gzip komprimerade data.</span><span class="sxs-lookup"><span data-stu-id="7d70f-208">For demonstration purposes, hello taxi cab data is stored both as uncompressed data and as gzip compressed data.</span></span>

    <span data-ttu-id="7d70f-209">Kör följande T-SQL-kommandon toodefine två olika format: okomprimerade och komprimerade.</span><span class="sxs-lookup"><span data-stu-id="7d70f-209">Run these T-SQL commands toodefine two different formats: uncompressed and compressed.</span></span>

    ```sql
    CREATE EXTERNAL FILE FORMAT uncompressedcsv
    WITH (
        FORMAT_TYPE = DELIMITEDTEXT,
        FORMAT_OPTIONS ( 
            FIELD_TERMINATOR = ',',
            STRING_DELIMITER = '',
            DATE_FORMAT = '',
            USE_TYPE_DEFAULT = False
        )
    );

    CREATE EXTERNAL FILE FORMAT compressedcsv
    WITH ( 
        FORMAT_TYPE = DELIMITEDTEXT,
        FORMAT_OPTIONS ( FIELD_TERMINATOR = '|',
            STRING_DELIMITER = '',
        DATE_FORMAT = '',
            USE_TYPE_DEFAULT = False
        ),
        DATA_COMPRESSION = 'org.apache.hadoop.io.compress.GzipCodec'
    );
    ```

4.  <span data-ttu-id="7d70f-210">Skapa ett schema för det externa filformatet.</span><span class="sxs-lookup"><span data-stu-id="7d70f-210">Create a schema for your external file format.</span></span> 

    ```sql
    CREATE SCHEMA ext;
    ```
5. <span data-ttu-id="7d70f-211">Skapa hello externa tabeller.</span><span class="sxs-lookup"><span data-stu-id="7d70f-211">Create hello external tables.</span></span> <span data-ttu-id="7d70f-212">Dessa tabeller refererar till data som lagras i Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="7d70f-212">These tables reference data stored in Azure blob storage.</span></span> <span data-ttu-id="7d70f-213">Kör följande T-SQL-kommandon toocreate hello flera externa tabeller som alla punkt toohello Azure blob vi definierats tidigare i vår extern datakälla.</span><span class="sxs-lookup"><span data-stu-id="7d70f-213">Run hello following T-SQL commands toocreate several external tables that all point toohello Azure blob we defined previously in our external data source.</span></span>

```sql
    CREATE EXTERNAL TABLE [ext].[Date] 
    (
        [DateID] int NOT NULL,
        [Date] datetime NULL,
        [DateBKey] char(10) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfMonth] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DaySuffix] varchar(4) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayName] varchar(9) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfWeek] char(1) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfWeekInMonth] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfWeekInYear] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfQuarter] varchar(3) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfYear] varchar(3) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [WeekOfMonth] varchar(1) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [WeekOfQuarter] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [WeekOfYear] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Month] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MonthName] varchar(9) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MonthOfQuarter] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Quarter] char(1) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [QuarterName] varchar(9) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Year] char(4) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [YearName] char(7) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MonthYear] char(10) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MMYYYY] char(6) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [FirstDayOfMonth] date NULL,
        [LastDayOfMonth] date NULL,
        [FirstDayOfQuarter] date NULL,
        [LastDayOfQuarter] date NULL,
        [FirstDayOfYear] date NULL,
        [LastDayOfYear] date NULL,
        [IsHolidayUSA] bit NULL,
        [IsWeekday] bit NULL,
        [HolidayUSA] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        LOCATION = 'Date',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    );
    
    CREATE EXTERNAL TABLE [ext].[Geography]
    (
        [GeographyID] int NOT NULL,
        [ZipCodeBKey] varchar(10) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [County] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [City] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [State] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Country] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [ZipCode] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        LOCATION = 'Geography',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0 
    );
        
    
    CREATE EXTERNAL TABLE [ext].[HackneyLicense]
    (
        [HackneyLicenseID] int NOT NULL,
        [HackneyLicenseBKey] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [HackneyLicenseCode] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        LOCATION = 'HackneyLicense',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
        
    
    CREATE EXTERNAL TABLE [ext].[Medallion]
    (
        [MedallionID] int NOT NULL,
        [MedallionBKey] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [MedallionCode] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        LOCATION = 'Medallion',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
        
    CREATE EXTERNAL TABLE [ext].[Time]
    (
        [TimeID] int NOT NULL,
        [TimeBKey] varchar(8) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [HourNumber] tinyint NOT NULL,
        [MinuteNumber] tinyint NOT NULL,
        [SecondNumber] tinyint NOT NULL,
        [TimeInSecond] int NOT NULL,
        [HourlyBucket] varchar(15) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [DayTimeBucketGroupKey] int NOT NULL,
        [DayTimeBucket] varchar(100) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL
    )
    WITH
    (
        LOCATION = 'Time',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
    
    
    CREATE EXTERNAL TABLE [ext].[Trip]
    (
        [DateID] int NOT NULL,
        [MedallionID] int NOT NULL,
        [HackneyLicenseID] int NOT NULL,
        [PickupTimeID] int NOT NULL,
        [DropoffTimeID] int NOT NULL,
        [PickupGeographyID] int NULL,
        [DropoffGeographyID] int NULL,
        [PickupLatitude] float NULL,
        [PickupLongitude] float NULL,
        [PickupLatLong] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DropoffLatitude] float NULL,
        [DropoffLongitude] float NULL,
        [DropoffLatLong] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [PassengerCount] int NULL,
        [TripDurationSeconds] int NULL,
        [TripDistanceMiles] float NULL,
        [PaymentType] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [FareAmount] money NULL,
        [SurchargeAmount] money NULL,
        [TaxAmount] money NULL,
        [TipAmount] money NULL,
        [TollsAmount] money NULL,
        [TotalAmount] money NULL
    )
    WITH
    (
        LOCATION = 'Trip2013',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = compressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
    
    CREATE EXTERNAL TABLE [ext].[Weather]
    (
        [DateID] int NOT NULL,
        [GeographyID] int NOT NULL,
        [PrecipitationInches] float NOT NULL,
        [AvgTemperatureFahrenheit] float NOT NULL
    )
    WITH
    (
        LOCATION = 'Weather2013',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
```

### <a name="import-hello-data-from-azure-blob-storage"></a><span data-ttu-id="7d70f-214">Importera hello data från Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="7d70f-214">Import hello data from Azure blob storage.</span></span>

<span data-ttu-id="7d70f-215">SQL Data Warehouse har stöd för en nyckelinstruktion som kallas CREATE TABLE AS SELECT (CTAS).</span><span class="sxs-lookup"><span data-stu-id="7d70f-215">SQL Data Warehouse supports a key statement called CREATE TABLE AS SELECT (CTAS).</span></span> <span data-ttu-id="7d70f-216">Den här instruktionen skapas en ny tabell baserat på hello resultaten av en select-instruktion.</span><span class="sxs-lookup"><span data-stu-id="7d70f-216">This statement creates a new table based on hello results of a select statement.</span></span> <span data-ttu-id="7d70f-217">hello ny tabell har hello samma kolumner och datatyper som hello resultaten av hello select-uttryck.</span><span class="sxs-lookup"><span data-stu-id="7d70f-217">hello new table has hello same columns and data types as hello results of hello select statement.</span></span>  <span data-ttu-id="7d70f-218">Här är en elegant sätt tooimport data från Azure blob storage till SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="7d70f-218">This is an elegant way tooimport data from Azure blob storage into SQL Data Warehouse.</span></span>

1. <span data-ttu-id="7d70f-219">Kör det här skriptet tooimport dina data.</span><span class="sxs-lookup"><span data-stu-id="7d70f-219">Run this script tooimport your data.</span></span>

    ```sql
    CREATE TABLE [dbo].[Date]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Date]
    OPTION (LABEL = 'CTAS : Load [dbo].[Date]')
    ;
    
    CREATE TABLE [dbo].[Geography]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS
    SELECT * FROM [ext].[Geography]
    OPTION (LABEL = 'CTAS : Load [dbo].[Geography]')
    ;
    
    CREATE TABLE [dbo].[HackneyLicense]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[HackneyLicense]
    OPTION (LABEL = 'CTAS : Load [dbo].[HackneyLicense]')
    ;
    
    CREATE TABLE [dbo].[Medallion]
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Medallion]
    OPTION (LABEL = 'CTAS : Load [dbo].[Medallion]')
    ;
    
    CREATE TABLE [dbo].[Time]
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Time]
    OPTION (LABEL = 'CTAS : Load [dbo].[Time]')
    ;
    
    CREATE TABLE [dbo].[Weather]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Weather]
    OPTION (LABEL = 'CTAS : Load [dbo].[Weather]')
    ;
    
    CREATE TABLE [dbo].[Trip]
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Trip]
    OPTION (LABEL = 'CTAS : Load [dbo].[Trip]')
    ;
    ```

2. <span data-ttu-id="7d70f-220">Visa data som laddas.</span><span class="sxs-lookup"><span data-stu-id="7d70f-220">View your data as it loads.</span></span>

   <span data-ttu-id="7d70f-221">Du läser in flera GB data och komprimerar dem till högpresterande klustrade Columnstore-index.</span><span class="sxs-lookup"><span data-stu-id="7d70f-221">You’re loading several GBs of data and compressing it into highly performant clustered columnstore indexes.</span></span> <span data-ttu-id="7d70f-222">Kör följande fråga som använder en dynamisk vyer (av DMV: er) tooshow hello Hanteringsstatus hello belastningen hello.</span><span class="sxs-lookup"><span data-stu-id="7d70f-222">Run hello following query that uses a dynamic management views (DMVs) tooshow hello status of hello load.</span></span> <span data-ttu-id="7d70f-223">När du har startat hello frågan hämtar du en kaffe och en tilltugg medan SQL Data Warehouse stöder vissa frekventa lyft.</span><span class="sxs-lookup"><span data-stu-id="7d70f-223">After starting hello query, grab a coffee and a snack while SQL Data Warehouse does some heavy lifting.</span></span>
    
    ```sql
    SELECT
        r.command,
        s.request_id,
        r.status,
        count(distinct input_name) as nbr_files,
        sum(s.bytes_processed)/1024/1024/1024 as gb_processed
    FROM 
        sys.dm_pdw_exec_requests r
        INNER JOIN sys.dm_pdw_dms_external_work s
        ON r.request_id = s.request_id
    WHERE
        r.[label] = 'CTAS : Load [dbo].[Date]' OR
        r.[label] = 'CTAS : Load [dbo].[Geography]' OR
        r.[label] = 'CTAS : Load [dbo].[HackneyLicense]' OR
        r.[label] = 'CTAS : Load [dbo].[Medallion]' OR
        r.[label] = 'CTAS : Load [dbo].[Time]' OR
        r.[label] = 'CTAS : Load [dbo].[Weather]' OR
        r.[label] = 'CTAS : Load [dbo].[Trip]'
    GROUP BY
        r.command,
        s.request_id,
        r.status
    ORDER BY
        nbr_files desc, 
        gb_processed desc;
    ```

3. <span data-ttu-id="7d70f-224">Visa alla systemfrågor.</span><span class="sxs-lookup"><span data-stu-id="7d70f-224">View all system queries.</span></span>

    ```sql
    SELECT * FROM sys.dm_pdw_exec_requests;
    ```

4. <span data-ttu-id="7d70f-225">Se hur dina data läses in i Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="7d70f-225">Enjoy seeing your data nicely loaded into your Azure SQL Data Warehouse.</span></span>

    ![Visa inlästa data](./media/sql-data-warehouse-get-started-tutorial/see-data-loaded.png)


## <a name="improve-query-performance"></a><span data-ttu-id="7d70f-227">Förbättra prestanda för frågor</span><span class="sxs-lookup"><span data-stu-id="7d70f-227">Improve query performance</span></span>

<span data-ttu-id="7d70f-228">Det finns flera sätt tooimprove frågeprestanda och tooachieve hello snabba prestanda som SQL Data Warehouse är utformad tooprovide.</span><span class="sxs-lookup"><span data-stu-id="7d70f-228">There are several ways tooimprove query performance and tooachieve hello high-speed performance that SQL Data Warehouse is designed tooprovide.</span></span>  

### <a name="see-hello-effect-of-scaling-on-query-performance"></a><span data-ttu-id="7d70f-229">Se hello av skalning på frågeprestanda</span><span class="sxs-lookup"><span data-stu-id="7d70f-229">See hello effect of scaling on query performance</span></span> 

<span data-ttu-id="7d70f-230">Enkelriktade tooimprove frågeprestanda är tooscale resurser genom att ändra hello DWU servicenivå för ditt informationslager.</span><span class="sxs-lookup"><span data-stu-id="7d70f-230">One way tooimprove query performance is tooscale resources by changing hello DWU service level for your data warehouse.</span></span> <span data-ttu-id="7d70f-231">Varje tjänstenivå kostar mer, men du kan minska eller pausa resurser när som helst.</span><span class="sxs-lookup"><span data-stu-id="7d70f-231">Each service level costs more, but you can scale back or pause resources at any time.</span></span> 

<span data-ttu-id="7d70f-232">I det här steget jämför du prestanda på två olika DWU-inställningar.</span><span class="sxs-lookup"><span data-stu-id="7d70f-232">In this step, you compare performance at two different DWU settings.</span></span>

<span data-ttu-id="7d70f-233">Först ska vi skala hello sizing ned too100 DWU så att vi kan få en uppfattning om hur en beräkningsnod kan utföra på en egen.</span><span class="sxs-lookup"><span data-stu-id="7d70f-233">First, let's scale hello sizing down too100 DWU so we can get an idea of how one compute node might perform on its own.</span></span>

1. <span data-ttu-id="7d70f-234">Gå toohello portal och välj ditt SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="7d70f-234">Go toohello portal and select your SQL Data Warehouse.</span></span>

2. <span data-ttu-id="7d70f-235">Välj skala hello SQL Data Warehouse-bladet.</span><span class="sxs-lookup"><span data-stu-id="7d70f-235">Select scale in hello SQL Data Warehouse blade.</span></span> 

    ![Skala DW från portalen](./media/sql-data-warehouse-get-started-tutorial/scale-dw.png)

3. <span data-ttu-id="7d70f-237">Skala hello prestanda liggande too100 DWU och klicka på Spara.</span><span class="sxs-lookup"><span data-stu-id="7d70f-237">Scale down hello performance bar too100 DWU and hit save.</span></span>

    ![Skala och spara](./media/sql-data-warehouse-get-started-tutorial/scale-and-save.png)

4. <span data-ttu-id="7d70f-239">Vänta medan din skala åtgärden toofinish.</span><span class="sxs-lookup"><span data-stu-id="7d70f-239">Wait for your scale operation toofinish.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7d70f-240">Frågor kan inte köras medan du ändrar hello skala.</span><span class="sxs-lookup"><span data-stu-id="7d70f-240">Queries cannot run while changing hello scale.</span></span> <span data-ttu-id="7d70f-241">Skalning **stoppar** dina frågor som körs för närvarande.</span><span class="sxs-lookup"><span data-stu-id="7d70f-241">Scaling **kills** your currently running queries.</span></span> <span data-ttu-id="7d70f-242">Du kan starta om dem när hello-åtgärden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="7d70f-242">You can restart them when hello operation is finished.</span></span>
    >
    
5. <span data-ttu-id="7d70f-243">Göra en genomsökning åtgärd på hello resa data, välja hello översta miljoner poster för alla hello-kolumner.</span><span class="sxs-lookup"><span data-stu-id="7d70f-243">Do a scan operation on hello trip data, selecting hello top million entries for all hello columns.</span></span> <span data-ttu-id="7d70f-244">Om du snabbt är gärna toomove på du ledigt tooselect färre rader.</span><span class="sxs-lookup"><span data-stu-id="7d70f-244">If you're eager toomove on quickly, feel free tooselect fewer rows.</span></span> <span data-ttu-id="7d70f-245">Anteckna hello tid det tar toorun för den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="7d70f-245">Take note of hello time it takes toorun this operation.</span></span>

    ```sql
    SELECT TOP(1000000) * FROM dbo.[Trip]
    ```
6. <span data-ttu-id="7d70f-246">Skala ditt informationslager tillbaka too400 DWU.</span><span class="sxs-lookup"><span data-stu-id="7d70f-246">Scale your data warehouse back too400 DWU.</span></span> <span data-ttu-id="7d70f-247">Kom ihåg att varje 100 DWU är att lägga till en annan compute-nod tooyour Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="7d70f-247">Remember, each 100 DWU is adding another compute node tooyour Azure SQL Data Warehouse.</span></span>

7. <span data-ttu-id="7d70f-248">Kör hello frågan igen!</span><span class="sxs-lookup"><span data-stu-id="7d70f-248">Run hello query again!</span></span> <span data-ttu-id="7d70f-249">Du bör märka en stor skillnad.</span><span class="sxs-lookup"><span data-stu-id="7d70f-249">You should notice a significant difference.</span></span> 

    > [!NOTE]
    > <span data-ttu-id="7d70f-250">Eftersom hello frågan returnerar stora mängder data, kanske hello bandbreddstillgänglighet av hello-dator som kör SSMS flaskhalsar.</span><span class="sxs-lookup"><span data-stu-id="7d70f-250">Because hello query returns a lot of data, hello bandwidth availability of hello machine running SSMS may be a performance bottleneck.</span></span> <span data-ttu-id="7d70f-251">Det kan leda till att du inte ser några prestandaförbättringar!</span><span class="sxs-lookup"><span data-stu-id="7d70f-251">This can result in you not seeing any performance improvements!</span></span>

> [!NOTE]
> <span data-ttu-id="7d70f-252">Eftersom SQL Data Warehouse använder massiv parallell bearbetning (MPP).</span><span class="sxs-lookup"><span data-stu-id="7d70f-252">Since SQL Data Warehouse uses massively parallel processing.</span></span> <span data-ttu-id="7d70f-253">Frågor som skanna eller utföra analysfunktioner på miljoner rader upplevelse hello true kraften i Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="7d70f-253">Queries that scan or perform analytic functions on millions of rows experience hello true power of Azure SQL Data Warehouse.</span></span>
>

### <a name="see-hello-effect-of-statistics-on-query-performance"></a><span data-ttu-id="7d70f-254">Se hello av statistik på frågeprestanda</span><span class="sxs-lookup"><span data-stu-id="7d70f-254">See hello effect of statistics on query performance</span></span>

1. <span data-ttu-id="7d70f-255">Köra en fråga som kopplar hello datumtabell med hello resa tabell</span><span class="sxs-lookup"><span data-stu-id="7d70f-255">Run a query that joins hello Date table with hello Trip table</span></span>

    ```sql
    SELECT TOP (1000000) 
        dt.[DayOfWeek],
        tr.[MedallionID],
        tr.[HackneyLicenseID],
        tr.[PickupTimeID],
        tr.[DropoffTimeID],
        tr.[PickupGeographyID],
        tr.[DropoffGeographyID],
        tr.[PickupLatitude],
        tr.[PickupLongitude],
        tr.[PickupLatLong],
        tr.[DropoffLatitude],
        tr.[DropoffLongitude],
        tr.[DropoffLatLong],
        tr.[PassengerCount],
        tr.[TripDurationSeconds],
        tr.[TripDistanceMiles],
        tr.[PaymentType],
        tr.[FareAmount],
        tr.[SurchargeAmount],
        tr.[TaxAmount],
        tr.[TipAmount],
        tr.[TollsAmount],
        tr.[TotalAmount]
    FROM [dbo].[Trip] as tr
        JOIN dbo.[Date] as dt
        ON  tr.DateID = dt.DateID
    ```

    <span data-ttu-id="7d70f-256">Den här frågan tar ett tag eftersom SQL Data Warehouse har tooshuffle data innan den kan utföra hello koppling.</span><span class="sxs-lookup"><span data-stu-id="7d70f-256">This query takes a while because SQL Data Warehouse has tooshuffle data before it can perform hello join.</span></span> <span data-ttu-id="7d70f-257">Kopplingar har inte tooshuffle data om de är utformad toojoin data i hello samma sätt som den distribueras.</span><span class="sxs-lookup"><span data-stu-id="7d70f-257">Joins do not have tooshuffle data if they are designed toojoin data in hello same way it is distributed.</span></span> <span data-ttu-id="7d70f-258">Det är ett djupare ämne.</span><span class="sxs-lookup"><span data-stu-id="7d70f-258">That's a deeper subject.</span></span> 

2. <span data-ttu-id="7d70f-259">Statistik gör skillnad.</span><span class="sxs-lookup"><span data-stu-id="7d70f-259">Statistics make a difference.</span></span> 
3. <span data-ttu-id="7d70f-260">Kör den här instruktionen toocreate statistik på hello kopplingskolumner.</span><span class="sxs-lookup"><span data-stu-id="7d70f-260">Run this statement toocreate statistics on hello join columns.</span></span>

    ```sql
    CREATE STATISTICS [dbo.Date DateID stats] ON dbo.Date (DateID);
    CREATE STATISTICS [dbo.Trip DateID stats] ON dbo.Trip (DateID);
    ```

    > [!NOTE]
    > <span data-ttu-id="7d70f-261">SQL DW hanterar inte statistik automatiskt åt dig.</span><span class="sxs-lookup"><span data-stu-id="7d70f-261">SQL DW does not automatically manage statistics for you.</span></span> <span data-ttu-id="7d70f-262">Statistik är viktigt för frågeprestanda och vi rekommenderar att du skapar och uppdaterar statistik.</span><span class="sxs-lookup"><span data-stu-id="7d70f-262">Statistics are important for query performance and it is highly recommended you create and update statistics.</span></span>
    > 
    > <span data-ttu-id="7d70f-263">**Du får hello utnyttja genom att låta statistik för kolumner som ingår i kopplingar, kolumner som används i hello där satsen och kolumner finns i GROUP BY.**</span><span class="sxs-lookup"><span data-stu-id="7d70f-263">**You gain hello most benefit by having statistics on columns involved in joins, columns used in hello WHERE clause and columns found in GROUP BY.**</span></span>
    >

3. <span data-ttu-id="7d70f-264">Kör hello frågan från förutsättningarna igen och notera eventuella skillnader i prestanda.</span><span class="sxs-lookup"><span data-stu-id="7d70f-264">Run hello query from Prerequisites again and observe any performance differences.</span></span> <span data-ttu-id="7d70f-265">Hello skillnader i prestanda för frågor kommer inte att som drastiskt som skala upp, bör du se en påskynda.</span><span class="sxs-lookup"><span data-stu-id="7d70f-265">While hello differences in query performance will not be as drastic as scaling up, you should notice a  speed-up.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="7d70f-266">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7d70f-266">Next steps</span></span>

<span data-ttu-id="7d70f-267">Du är nu redo tooquery och utforska.</span><span class="sxs-lookup"><span data-stu-id="7d70f-267">You're now ready tooquery and explore.</span></span> <span data-ttu-id="7d70f-268">Se våra bästa praxis och tips.</span><span class="sxs-lookup"><span data-stu-id="7d70f-268">Check out our best practices or tips.</span></span>

<span data-ttu-id="7d70f-269">Om du är klar utforska hello dag, se till att toopause din instans!</span><span class="sxs-lookup"><span data-stu-id="7d70f-269">If you're done exploring for hello day, make sure toopause your instance!</span></span> <span data-ttu-id="7d70f-270">I produktion, du får mycket stora besparingar av du pausar och skalning toomeet dina affärsbehov.</span><span class="sxs-lookup"><span data-stu-id="7d70f-270">In production, you can experience enormous savings by pausing and scaling toomeet your business needs.</span></span>

![Pausa](./media/sql-data-warehouse-get-started-tutorial/pause.png)

## <a name="useful-readings"></a><span data-ttu-id="7d70f-272">Bra information</span><span class="sxs-lookup"><span data-stu-id="7d70f-272">Useful readings</span></span>

<span data-ttu-id="7d70f-273">[Concurrency and workload management][] (Hantering av samtidiga körningar och arbetsbelastningar)</span><span class="sxs-lookup"><span data-stu-id="7d70f-273">[Concurrency and Workload Management][]</span></span>

<span data-ttu-id="7d70f-274">[Metodtips för Azure SQL Data Warehouse][]</span><span class="sxs-lookup"><span data-stu-id="7d70f-274">[Best practices for Azure SQL Data Warehouse][]</span></span>

<span data-ttu-id="7d70f-275">[Query Monitoring][] (Frågeövervakning)</span><span class="sxs-lookup"><span data-stu-id="7d70f-275">[Query Monitoring][]</span></span>

<span data-ttu-id="7d70f-276">[Top 10 Best Practices for Building a Large Scale Relational Data Warehouse][] (Tio rekommendationer för att skapa ett storskaligt relationsinformationslager)</span><span class="sxs-lookup"><span data-stu-id="7d70f-276">[Top 10 Best Practices for Building a Large Scale Relational Data Warehouse][]</span></span>

<span data-ttu-id="7d70f-277">[Migrera Data tooAzure SQL Data Warehouse][]</span><span class="sxs-lookup"><span data-stu-id="7d70f-277">[Migrating Data tooAzure SQL Data Warehouse][]</span></span>

[Concurrency and workload management]: sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example (Hantering av samtidiga körningar och arbetsbelastningar)
[Metodtips för Azure SQL Data Warehouse]: sql-data-warehouse-best-practices.md#hash-distribute-large-tables
[Query Monitoring]: sql-data-warehouse-manage-monitor.md (Frågeövervakning)
[Top 10 Best Practices for Building a Large Scale Relational Data Warehouse]: https://blogs.msdn.microsoft.com/sqlcat/2013/09/16/top-10-best-practices-for-building-a-large-scale-relational-data-warehouse/ (Tio rekommendationer för att skapa ett storskaligt relationsinformationslager)
[Migrera Data tooAzure SQL Data Warehouse]: https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/



[!INCLUDE [Additional Resources](../../includes/sql-data-warehouse-article-footer.md)]

<!-- Internal Links -->
[krav]: sql-data-warehouse-get-started-tutorial.md#prerequisites

<!--Other Web references-->
[Visual Studio]: https://www.visualstudio.com/
[SQL Server Management Studio]: https://msdn.microsoft.com/en-us/library/mt238290.aspx
