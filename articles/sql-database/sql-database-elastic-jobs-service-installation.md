---
title: Installera elastisk databas jobb | Microsoft Docs
description: "Gå igenom installationen av elastiska jobb."
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: cbe0aa2b-17e3-4b6f-a16f-6ebc1f5a66af
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: e7a2d6dbcefbb31d76257eaf96ccc235d7a29416
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="installing-elastic-database-jobs-overview"></a><span data-ttu-id="caf4e-103">Installera översikt över elastisk databas</span><span class="sxs-lookup"><span data-stu-id="caf4e-103">Installing Elastic Database jobs overview</span></span>
<span data-ttu-id="caf4e-104">[**Den elastiska databasen jobb** ](sql-database-elastic-jobs-overview.md) kan installeras via PowerShell eller via den klassiska Azure-Portal.You kan komma åt för att skapa och hantera jobb med hjälp av PowerShell API endast om du har installerat PowerShell.</span><span class="sxs-lookup"><span data-stu-id="caf4e-104">[**Elastic Database jobs**](sql-database-elastic-jobs-overview.md) can be installed via PowerShell or through the Azure Classic Portal.You can gain access to create and manage jobs using the PowerShell API only if you install the PowerShell package.</span></span> <span data-ttu-id="caf4e-105">Dessutom tillhandahålla PowerShell APIs betydligt fler funktioner än portalen vid denna tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="caf4e-105">Additionally, the PowerShell APIs provide significantly more functionality than the portal at this point in time.</span></span>

<span data-ttu-id="caf4e-106">Om du redan har installerat **elastisk databas jobb** via portalen från en befintlig **elastisk pool**, innehåller den senaste Powershell-förhandsversionen skript för att uppgradera den befintliga installationen.</span><span class="sxs-lookup"><span data-stu-id="caf4e-106">If you have already installed **Elastic Database jobs** through the Portal from an existing **elastic pool**, the latest Powershell preview includes scripts to upgrade your existing installation.</span></span> <span data-ttu-id="caf4e-107">Vi rekommenderar starkt att uppgradera installationen till senast **elastisk databas jobb** komponenter för att kunna dra nytta av nya funktioner som exponeras via PowerShell APIs.</span><span class="sxs-lookup"><span data-stu-id="caf4e-107">It is highly recommended to upgrade your installation to the latest **Elastic Database jobs** components in order to take advantage of new functionality exposed via the PowerShell APIs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="caf4e-108">Krav</span><span class="sxs-lookup"><span data-stu-id="caf4e-108">Prerequisites</span></span>
* <span data-ttu-id="caf4e-109">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="caf4e-109">An Azure subscription.</span></span> <span data-ttu-id="caf4e-110">För en kostnadsfri utvärderingsversion finns [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="caf4e-110">For a free trial, see [Free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="caf4e-111">Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="caf4e-111">Azure PowerShell.</span></span> <span data-ttu-id="caf4e-112">Installera den senaste versionen med hjälp av den [installationsprogram för webbplattform](http://go.microsoft.com/fwlink/p/?linkid=320376).</span><span class="sxs-lookup"><span data-stu-id="caf4e-112">Install the latest version using the [Web Platform Installer](http://go.microsoft.com/fwlink/p/?linkid=320376).</span></span> <span data-ttu-id="caf4e-113">Mer information finns i [Så här installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="caf4e-113">For detailed information, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="caf4e-114">[NuGet Command-line Utility](https://nuget.org/nuget.exe) används för att installera paketet för elastisk databas jobb.</span><span class="sxs-lookup"><span data-stu-id="caf4e-114">[NuGet Command-line Utility](https://nuget.org/nuget.exe) is used to install the Elastic Database jobs package.</span></span> <span data-ttu-id="caf4e-115">Mer information finns i http://docs.nuget.org/docs/start-here/installing-nuget.</span><span class="sxs-lookup"><span data-stu-id="caf4e-115">For more information, see http://docs.nuget.org/docs/start-here/installing-nuget.</span></span>

## <a name="download-and-import-the-elastic-database-jobs-powershell-package"></a><span data-ttu-id="caf4e-116">Hämta och importera jobb PowerShell-paketet för elastisk databas</span><span class="sxs-lookup"><span data-stu-id="caf4e-116">Download and import the Elastic Database jobs PowerShell package</span></span>
1. <span data-ttu-id="caf4e-117">Starta Microsoft Azure PowerShell-kommandofönster och gå till den katalog där du hämtade NuGet Command-line Utility (nuget.exe).</span><span class="sxs-lookup"><span data-stu-id="caf4e-117">Launch Microsoft Azure PowerShell command window and navigate to the directory where you downloaded NuGet Command-line Utility (nuget.exe).</span></span>
2. <span data-ttu-id="caf4e-118">Hämta och importera **elastisk databas jobb** paketet till den aktuella katalogen med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="caf4e-118">Download and import **Elastic Database jobs** package into the current directory with the following command:</span></span>
   
        PS C:\>.\nuget install Microsoft.Azure.SqlDatabase.Jobs -prerelease
   
    <span data-ttu-id="caf4e-119">Den **elastisk databas jobb** filerna placeras på den lokala katalogen i en mapp med namnet **Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x** där *x.x.xxxx.x* återspeglar den versionsnummer.</span><span class="sxs-lookup"><span data-stu-id="caf4e-119">The **Elastic Database jobs** files are placed in the local directory in a folder named **Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x** where *x.x.xxxx.x* reflects the version number.</span></span> <span data-ttu-id="caf4e-120">PowerShell-cmdlet (inklusive nödvändiga klient DLL-filer) finns i den **tools\ElasticDatabaseJobs** underkatalog och PowerShell-skript för att installera, uppgradera och avinstallera finns också i den **verktyg** underkatalog.</span><span class="sxs-lookup"><span data-stu-id="caf4e-120">The PowerShell cmdlets (including required client .dlls) are located in the **tools\ElasticDatabaseJobs** sub-directory and the PowerShell scripts to install, upgrade and uninstall also reside in the **tools** sub-directory.</span></span>
3. <span data-ttu-id="caf4e-121">Gå till underkatalogen verktyg under mappen Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x genom att skriva CD-verktyg, till exempel:</span><span class="sxs-lookup"><span data-stu-id="caf4e-121">Navigate to the tools sub-directory under the Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x folder by typing cd tools, for example:</span></span>
   
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools

4. <span data-ttu-id="caf4e-122">Kör skriptet.\InstallElasticDatabaseJobsCmdlets.ps1 om du vill kopiera katalogen ElasticDatabaseJobs till $home\Documents\WindowsPowerShell\Modules.</span><span class="sxs-lookup"><span data-stu-id="caf4e-122">Execute the .\InstallElasticDatabaseJobsCmdlets.ps1 script to copy the ElasticDatabaseJobs directory into $home\Documents\WindowsPowerShell\Modules.</span></span> <span data-ttu-id="caf4e-123">Detta kommer också automatiskt importera modulen för användning, till exempel:</span><span class="sxs-lookup"><span data-stu-id="caf4e-123">This will also automatically import the module for use, for example:</span></span>
   
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobsCmdlets.ps1
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\InstallElasticDatabaseJobsCmdlets.ps1

## <a name="install-the-elastic-database-jobs-components-using-powershell"></a><span data-ttu-id="caf4e-124">Installera elastiska jobb med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="caf4e-124">Install the Elastic Database jobs components using PowerShell</span></span>
1. <span data-ttu-id="caf4e-125">Starta Microsoft Azure PowerShell-Kommandotolken och navigera till \tools underkatalog under mappen Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x: Skriv cd \tools</span><span class="sxs-lookup"><span data-stu-id="caf4e-125">Launch a Microsoft Azure PowerShell command window and navigate to the \tools sub-directory under the Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x folder: Type cd \tools</span></span>
   
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools

2. <span data-ttu-id="caf4e-126">Köra.\InstallElasticDatabaseJobs.ps1 PowerShell-skript och ange värden för de begärda variablerna.</span><span class="sxs-lookup"><span data-stu-id="caf4e-126">Execute the .\InstallElasticDatabaseJobs.ps1 PowerShell script and supply values for its requested variables.</span></span> <span data-ttu-id="caf4e-127">Det här skriptet skapar de komponenter som beskrivs i [elastisk databas jobb komponenter och prissättning](sql-database-elastic-jobs-overview.md#components-and-pricing) tillsammans med hur du konfigurerar Azure Cloud Service om du vill använda på lämpligt sätt beroende komponenter.</span><span class="sxs-lookup"><span data-stu-id="caf4e-127">This script will create the components described in [Elastic Database jobs components and pricing](sql-database-elastic-jobs-overview.md#components-and-pricing) along with configuring the Azure Cloud Service to appropriately use the dependent components.</span></span>

        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobs.ps1
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\InstallElasticDatabaseJobs.ps1

<span data-ttu-id="caf4e-128">När du kör det här kommandot ett fönster upp som ber om en **användarnamn** och **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="caf4e-128">When you run this command a window opens asking for a **user name** and **password**.</span></span> <span data-ttu-id="caf4e-129">Detta är inte dina Azure-autentiseringsuppgifter, ange användarnamn och lösenord som administratören som du vill skapa för den nya servern.</span><span class="sxs-lookup"><span data-stu-id="caf4e-129">This is not your Azure credentials, enter the user name and password that will be the administrator credentials you want to create for the new server.</span></span>

<span data-ttu-id="caf4e-130">De parametrar som ges i det här exemplet anrop kan ändras för inställningarna.</span><span class="sxs-lookup"><span data-stu-id="caf4e-130">The parameters provided on this sample invocation can be modified for your desired settings.</span></span> <span data-ttu-id="caf4e-131">Nedan finns mer information om beteendet för varje parameter:</span><span class="sxs-lookup"><span data-stu-id="caf4e-131">The following provides more information on the behavior of each parameter:</span></span>

<table style="width:100%">
  <tr>
    <th><span data-ttu-id="caf4e-132">Parameter</span><span class="sxs-lookup"><span data-stu-id="caf4e-132">Parameter</span></span></th>
    <th><span data-ttu-id="caf4e-133">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="caf4e-133">Description</span></span></th>
  </tr>

<tr>
    <td><span data-ttu-id="caf4e-134">resourceGroupName</span><span class="sxs-lookup"><span data-stu-id="caf4e-134">ResourceGroupName</span></span></td>
    <td><span data-ttu-id="caf4e-135">Tillhandahåller Azure resursgruppens namn innehåller de nyligen skapade Azure komponenterna.</span><span class="sxs-lookup"><span data-stu-id="caf4e-135">Provides the Azure resource group name created to contain the newly created Azure components.</span></span> <span data-ttu-id="caf4e-136">Den här parametern standard ”__ElasticDatabaseJob”.</span><span class="sxs-lookup"><span data-stu-id="caf4e-136">This parameter defaults to “__ElasticDatabaseJob”.</span></span> <span data-ttu-id="caf4e-137">Du bör inte ändra det här värdet.</span><span class="sxs-lookup"><span data-stu-id="caf4e-137">It is not recommended to change this value.</span></span></td>
    </tr>

</tr>

    <tr>
    <td><span data-ttu-id="caf4e-138">ResourceGroupLocation</span><span class="sxs-lookup"><span data-stu-id="caf4e-138">ResourceGroupLocation</span></span></td>
    <td><span data-ttu-id="caf4e-139">Innehåller den Azure-platsen som ska användas för de nyligen skapade Azure-komponenterna.</span><span class="sxs-lookup"><span data-stu-id="caf4e-139">Provides the Azure location to be used for the newly created Azure components.</span></span> <span data-ttu-id="caf4e-140">Den här parametern standard centrala USA-plats.</span><span class="sxs-lookup"><span data-stu-id="caf4e-140">This parameter defaults to the Central US location.</span></span></td>
</tr>

<tr>
    <td><span data-ttu-id="caf4e-141">ServiceWorkerCount</span><span class="sxs-lookup"><span data-stu-id="caf4e-141">ServiceWorkerCount</span></span></td>
    <td><span data-ttu-id="caf4e-142">Visar antalet service anställda att installera.</span><span class="sxs-lookup"><span data-stu-id="caf4e-142">Provides the number of service workers to install.</span></span> <span data-ttu-id="caf4e-143">Den här parametern standardvärdet 1.</span><span class="sxs-lookup"><span data-stu-id="caf4e-143">This parameter defaults to 1.</span></span> <span data-ttu-id="caf4e-144">Ett högre antal anställda kan användas för att skala ut tjänsten och för att tillhandahålla hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="caf4e-144">A higher number of workers can be used to scale out the service and to provide high availability.</span></span> <span data-ttu-id="caf4e-145">Det rekommenderas att använda ”2” för distributioner som kräver hög tillgänglighet för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="caf4e-145">It is recommended to use “2” for deployments that require high availability of the service.</span></span></td>
    </tr>

</tr>
    <tr>
    <td><span data-ttu-id="caf4e-146">ServiceVmSize</span><span class="sxs-lookup"><span data-stu-id="caf4e-146">ServiceVmSize</span></span></td>
    <td><span data-ttu-id="caf4e-147">Innehåller VM-storlek för användning i Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="caf4e-147">Provides the VM size for usage within the Cloud Service.</span></span> <span data-ttu-id="caf4e-148">Den här parametern standard A0.</span><span class="sxs-lookup"><span data-stu-id="caf4e-148">This parameter defaults to A0.</span></span> <span data-ttu-id="caf4e-149">Parametervärdena A0/A1/A2/a3 accepteras som orsakar arbetsrollen att använda en ExtraSmall/Small/Medium/Large storlek respektive.</span><span class="sxs-lookup"><span data-stu-id="caf4e-149">Parameters values of A0/A1/A2/A3 are accepted which cause the worker role to use an ExtraSmall/Small/Medium/Large size, respectively.</span></span> <span data-ttu-id="caf4e-150">För mer information om storlekar för worker-rollen och se [elastisk databas jobb komponenter och prissättning](sql-database-elastic-jobs-overview.md#components-and-pricing).</span><span class="sxs-lookup"><span data-stu-id="caf4e-150">Fo more information on worker role sizes, see [Elastic Database jobs components and pricing](sql-database-elastic-jobs-overview.md#components-and-pricing).</span></span></td>
</tr>

</tr>
    <tr>
    <td><span data-ttu-id="caf4e-151">SqlServerDatabaseSlo</span><span class="sxs-lookup"><span data-stu-id="caf4e-151">SqlServerDatabaseSlo</span></span></td>
    <td><span data-ttu-id="caf4e-152">Tillhandahåller servicenivåmålet för en Standard-utgåva.</span><span class="sxs-lookup"><span data-stu-id="caf4e-152">Provides the service level objective for a Standard edition.</span></span> <span data-ttu-id="caf4e-153">Den här parametern standardvärdet S0.</span><span class="sxs-lookup"><span data-stu-id="caf4e-153">This parameter defaults to S0.</span></span> <span data-ttu-id="caf4e-154">Parametervärdena för S0/S1/S2/S3 accepteras vilket leder till Azure SQL-databasen ska användas på respektive SLO.</span><span class="sxs-lookup"><span data-stu-id="caf4e-154">Parameter values of S0/S1/S2/S3 are accepted which cause the Azure SQL Database to use the respective SLO.</span></span> <span data-ttu-id="caf4e-155">Mer information om servicenivåmål för SQL-databasen finns [elastisk databas jobb komponenter och prissättning](sql-database-elastic-jobs-overview.md#components-and-pricing).</span><span class="sxs-lookup"><span data-stu-id="caf4e-155">For more information on SQL Database SLOs, see [Elastic Database jobs components and pricing](sql-database-elastic-jobs-overview.md#components-and-pricing).</span></span></td>
</tr>

</tr>
    <tr>
    <td><span data-ttu-id="caf4e-156">SqlServerAdministratorUserName</span><span class="sxs-lookup"><span data-stu-id="caf4e-156">SqlServerAdministratorUserName</span></span></td>
    <td><span data-ttu-id="caf4e-157">Ger administratören användarnamnet för den nyligen skapade Azure SQL Database-servern.</span><span class="sxs-lookup"><span data-stu-id="caf4e-157">Provides the admin user name for the newly created Azure SQL Database server.</span></span> <span data-ttu-id="caf4e-158">Om inget värde anges öppnas ett fönster för PowerShell-autentiseringsuppgifter för att efterfråga autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="caf4e-158">When not specified, a PowerShell credentials window will open to prompt for the credentials.</span></span></td>
</tr>

</tr>
    <tr>
    <td><span data-ttu-id="caf4e-159">SqlServerAdministratorPassword</span><span class="sxs-lookup"><span data-stu-id="caf4e-159">SqlServerAdministratorPassword</span></span></td>
    <td><span data-ttu-id="caf4e-160">Tillhandahåller administratörslösenordet för den nyligen skapade Azure SQL Database-servern.</span><span class="sxs-lookup"><span data-stu-id="caf4e-160">Provides the admin password for the newly created Azure SQL Database server.</span></span> <span data-ttu-id="caf4e-161">När inte tillhandahålls, öppnas ett fönster för PowerShell-autentiseringsuppgifter för att efterfråga autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="caf4e-161">When not provided, a PowerShell credentials window will open to prompt for the credentials.</span></span></td>
</tr>
</table>

<span data-ttu-id="caf4e-162">För datorer som är avsedda att ha stort antal jobb som körs parallellt mot ett stort antal databaser, rekommenderas att ange parametrar, till exempel: - ServiceWorkerCount 2 - ServiceVmSize A2 - SqlServerDatabaseSlo S2.</span><span class="sxs-lookup"><span data-stu-id="caf4e-162">For systems that target having large numbers of jobs running in parallel against a large number of databases, it is recommended to specify parameters such as: -ServiceWorkerCount 2 -ServiceVmSize A2 -SqlServerDatabaseSlo S2.</span></span>

    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobs.ps1
    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>.\InstallElasticDatabaseJobs.ps1 -ServiceWorkerCount 2 -ServiceVmSize A2 -SqlServerDatabaseSlo S2

## <a name="update-an-existing-elastic-database-jobs-components-installation-using-powershell"></a><span data-ttu-id="caf4e-163">Uppdatera en befintlig elastisk databas jobb komponenter installation med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="caf4e-163">Update an existing Elastic Database jobs components installation using PowerShell</span></span>
<span data-ttu-id="caf4e-164">**Den elastiska databasen jobb** kan uppdateras i en befintlig installation för skalning och hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="caf4e-164">**Elastic Database jobs** can be updated within an existing installation for scale and high-availability.</span></span> <span data-ttu-id="caf4e-165">Den här processen kan för framtida uppgraderingar av Tjänstkod utan att behöva ta bort och återskapa kontroll-databasen.</span><span class="sxs-lookup"><span data-stu-id="caf4e-165">This process allows for future upgrades of service code without having to drop and recreate the control database.</span></span> <span data-ttu-id="caf4e-166">Den här processen kan också användas inom samma version för att ändra tjänsten VM-storlek eller antalet server worker.</span><span class="sxs-lookup"><span data-stu-id="caf4e-166">This process can also be used within the same version to modify the service VM size or the server worker count.</span></span>

<span data-ttu-id="caf4e-167">För att uppdatera VM-storlek för en installation, kör du följande skript med parametrarna uppdateras till värdena du väljer.</span><span class="sxs-lookup"><span data-stu-id="caf4e-167">To update the VM size of an installation, run the following script with parameters updated to the values of your choice.</span></span>

    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>Unblock-File .\UpdateElasticDatabaseJobs.ps1
    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>.\UpdateElasticDatabaseJobs.ps1 -ServiceVmSize A1 -ServiceWorkerCount 2

<table style="width:100%">
  <tr>
  <th><span data-ttu-id="caf4e-168">Parameter</span><span class="sxs-lookup"><span data-stu-id="caf4e-168">Parameter</span></span></th>
  <th><span data-ttu-id="caf4e-169">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="caf4e-169">Description</span></span></th>
</tr>

  <tr>
    <td><span data-ttu-id="caf4e-170">resourceGroupName</span><span class="sxs-lookup"><span data-stu-id="caf4e-170">ResourceGroupName</span></span></td>
    <td><span data-ttu-id="caf4e-171">Identifierar Azure resursgruppens namn används när elastisk databas jobbet-komponenter installerades från början.</span><span class="sxs-lookup"><span data-stu-id="caf4e-171">Identifies the Azure resource group name used when the Elastic Database job components were initially installed.</span></span> <span data-ttu-id="caf4e-172">Den här parametern standard ”__ElasticDatabaseJob”.</span><span class="sxs-lookup"><span data-stu-id="caf4e-172">This parameter defaults to “__ElasticDatabaseJob”.</span></span> <span data-ttu-id="caf4e-173">Eftersom det inte rekommenderas att ändra det här värdet, inte bör du måste ange den här parametern.</span><span class="sxs-lookup"><span data-stu-id="caf4e-173">Since it is not recommended to change this value, you shouldn't have to specify this parameter.</span></span></td>
    </tr>
</tr>

</tr>

  <tr>
    <td><span data-ttu-id="caf4e-174">ServiceWorkerCount</span><span class="sxs-lookup"><span data-stu-id="caf4e-174">ServiceWorkerCount</span></span></td>
    <td><span data-ttu-id="caf4e-175">Visar antalet service anställda att installera.</span><span class="sxs-lookup"><span data-stu-id="caf4e-175">Provides the number of service workers to install.</span></span>  <span data-ttu-id="caf4e-176">Den här parametern standardvärdet 1.</span><span class="sxs-lookup"><span data-stu-id="caf4e-176">This parameter defaults to 1.</span></span>  <span data-ttu-id="caf4e-177">Ett högre antal anställda kan användas för att skala ut tjänsten och för att tillhandahålla hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="caf4e-177">A higher number of workers can be used to scale out the service and to provide high availability.</span></span>  <span data-ttu-id="caf4e-178">Det rekommenderas att använda ”2” för distributioner som kräver hög tillgänglighet för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="caf4e-178">It is recommended to use “2” for deployments that require high availability of the service.</span></span></td>
</tr>

</tr>

    <tr>
    <td><span data-ttu-id="caf4e-179">ServiceVmSize</span><span class="sxs-lookup"><span data-stu-id="caf4e-179">ServiceVmSize</span></span></td>
    <td><span data-ttu-id="caf4e-180">Innehåller VM-storlek för användning i Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="caf4e-180">Provides the VM size for usage within the Cloud Service.</span></span> <span data-ttu-id="caf4e-181">Den här parametern standard A0.</span><span class="sxs-lookup"><span data-stu-id="caf4e-181">This parameter defaults to A0.</span></span> <span data-ttu-id="caf4e-182">Parametervärdena A0/A1/A2/a3 accepteras som orsakar arbetsrollen att använda en ExtraSmall/Small/Medium/Large storlek respektive.</span><span class="sxs-lookup"><span data-stu-id="caf4e-182">Parameters values of A0/A1/A2/A3 are accepted which cause the worker role to use an ExtraSmall/Small/Medium/Large size, respectively.</span></span> <span data-ttu-id="caf4e-183">För mer information om storlekar för worker-rollen och se [elastisk databas jobb komponenter och prissättning](sql-database-elastic-jobs-overview.md#components-and-pricing).</span><span class="sxs-lookup"><span data-stu-id="caf4e-183">Fo more information on worker role sizes, see [Elastic Database jobs components and pricing](sql-database-elastic-jobs-overview.md#components-and-pricing).</span></span></td>
</tr>

</table>

## <a name="install-the-elastic-database-jobs-components-using-the-portal"></a><span data-ttu-id="caf4e-184">Installera elastiska jobb med hjälp av portalen</span><span class="sxs-lookup"><span data-stu-id="caf4e-184">Install the Elastic Database jobs components using the Portal</span></span>
<span data-ttu-id="caf4e-185">När du har [skapas en elastisk pool](sql-database-elastic-pool-manage-portal.md), kan du installera **elastisk databas jobb** komponenter om du vill aktivera körningen av administrativa uppgifter mot varje databas i den elastiska poolen.</span><span class="sxs-lookup"><span data-stu-id="caf4e-185">Once you have [created an elastic pool](sql-database-elastic-pool-manage-portal.md), you can install **Elastic Database jobs** components to enable execution of administrative tasks against each database in the elastic pool.</span></span> <span data-ttu-id="caf4e-186">Till skillnad från när du använder den **jobb för elastisk databas** PowerShell APIs gränssnittet Företagsportalen är för närvarande begränsad till endast körs mot en befintlig adresspool.</span><span class="sxs-lookup"><span data-stu-id="caf4e-186">Unlike when using the **Elastic Database jobs** PowerShell APIs, the portal interface is currently restricted to only executing against an existing pool.</span></span>

<span data-ttu-id="caf4e-187">**Uppskattad tidsåtgång:** 10 minuter.</span><span class="sxs-lookup"><span data-stu-id="caf4e-187">**Estimated time to complete:** 10 minutes.</span></span>

1. <span data-ttu-id="caf4e-188">Från instrumentpanelsvyn för den elastiska poolen via den [Azure Portal](https://portal.azure.com/#) , klickar du på **skapa jobbet**.</span><span class="sxs-lookup"><span data-stu-id="caf4e-188">From the dashboard view of the elastic pool via the [Azure Portal](https://portal.azure.com/#) , click **Create job**.</span></span>
2. <span data-ttu-id="caf4e-189">Om du skapar ett jobb för första gången, måste du installera **elastisk databas jobb** genom att klicka på **FÖRHANDSGRANSKNINGSVILLKOREN**.</span><span class="sxs-lookup"><span data-stu-id="caf4e-189">If you are creating a job for the first time, you must install **Elastic Database jobs** by clicking **PREVIEW TERMS**.</span></span>
3. <span data-ttu-id="caf4e-190">Acceptera villkoren genom att klicka på kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="caf4e-190">Accept the terms by clicking the checkbox.</span></span>
4. <span data-ttu-id="caf4e-191">I ”installera services” visas klickar du på **jobbet AUTENTISERINGSUPPGIFTER**.</span><span class="sxs-lookup"><span data-stu-id="caf4e-191">In the "Install services" view, click **JOB CREDENTIALS**.</span></span>
   
    ![Installera tjänsterna][1]
5. <span data-ttu-id="caf4e-193">Ange ett användarnamn och lösenord för databas-administratör.</span><span class="sxs-lookup"><span data-stu-id="caf4e-193">Type a user name and password for a database admin.</span></span> <span data-ttu-id="caf4e-194">Som en del av installationen skapas en ny Azure SQL Database-server.</span><span class="sxs-lookup"><span data-stu-id="caf4e-194">As part of the installation, a new Azure SQL Database server is created.</span></span> <span data-ttu-id="caf4e-195">I den här nya servern, en ny databas kallas kontroll-databasen skapas och används för att innehålla metadata för elastisk databas jobb.</span><span class="sxs-lookup"><span data-stu-id="caf4e-195">Within this new server, a new database, known as the control database, is created and used to contain the meta data for Elastic Database jobs.</span></span> <span data-ttu-id="caf4e-196">Användarnamn och lösenord som skapas här används för att logga in på databasen för kontrollen.</span><span class="sxs-lookup"><span data-stu-id="caf4e-196">The user name and password created here are used for the purpose of logging in to the control database.</span></span> <span data-ttu-id="caf4e-197">En separat autentiseringsuppgifter används för körning av skript mot databaser i poolen.</span><span class="sxs-lookup"><span data-stu-id="caf4e-197">A separate credential is used for script execution against the databases within the pool.</span></span>
   
    ![Skapa användarnamn och lösenord][2]
6. <span data-ttu-id="caf4e-199">Klicka på OK.</span><span class="sxs-lookup"><span data-stu-id="caf4e-199">Click the OK button.</span></span> <span data-ttu-id="caf4e-200">Komponenterna som skapas automatiskt om några minuter i en ny [resursgruppen](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="caf4e-200">The components are created for you in a few minutes in a new [Resource group](../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="caf4e-201">Den nya resursgruppen är fäst på start-planen som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="caf4e-201">The new resource group is pinned to the start board, as shown below.</span></span> <span data-ttu-id="caf4e-202">När du skapade, elastisk databas jobb (Cloud Service, SQL Database, Service Bus och Storage) skapas i gruppen.</span><span class="sxs-lookup"><span data-stu-id="caf4e-202">Once created, elastic database jobs (Cloud Service, SQL Database, Service Bus, and Storage) are all created in the group.</span></span>
   
    ![resursgrupp i start-kort][3]
7. <span data-ttu-id="caf4e-204">Om du försöker skapa eller hantera ett jobb när elastisk databas jobb installeras när de tillhandahåller **autentiseringsuppgifter** visas följande meddelande.</span><span class="sxs-lookup"><span data-stu-id="caf4e-204">If you attempt to create or manage a job while elastic database jobs is installing, when providing **Credentials** you will see the following message.</span></span>
   
    ![Distributionen fortfarande pågår][4]

<span data-ttu-id="caf4e-206">Ta bort resursgruppen om avinstallation krävs.</span><span class="sxs-lookup"><span data-stu-id="caf4e-206">If uninstallation is required, delete the resource group.</span></span> <span data-ttu-id="caf4e-207">Se [avinstallera elastiska jobb databaskomponenterna](sql-database-elastic-jobs-uninstall.md).</span><span class="sxs-lookup"><span data-stu-id="caf4e-207">See [How to uninstall the Elastic Database job components](sql-database-elastic-jobs-uninstall.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="caf4e-208">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="caf4e-208">Next steps</span></span>
<span data-ttu-id="caf4e-209">Se till att en autentiseringsuppgift med rätt behörighet för körning av skript skapas på varje databas i gruppen, mer information finns i [skydda SQL-databasen](sql-database-manage-logins.md).</span><span class="sxs-lookup"><span data-stu-id="caf4e-209">Ensure a credential with the appropriate rights for script execution is created on each database in the group, for more information see [Securing your SQL Database](sql-database-manage-logins.md).</span></span>
<span data-ttu-id="caf4e-210">Se [skapa och hantera en elastisk databas jobb](sql-database-elastic-jobs-create-and-manage.md) att komma igång.</span><span class="sxs-lookup"><span data-stu-id="caf4e-210">See [Creating and managing an Elastic Database jobs](sql-database-elastic-jobs-create-and-manage.md) to get started.</span></span>

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-service-installation/screen-1.png
[2]: ./media/sql-database-elastic-jobs-service-installation/credentials.png
[3]: ./media/sql-database-elastic-jobs-service-installation/start-board.png
[4]: ./media/sql-database-elastic-jobs-service-installation/not-done.png
