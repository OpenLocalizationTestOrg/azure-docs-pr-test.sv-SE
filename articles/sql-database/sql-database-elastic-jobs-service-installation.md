---
title: aaaInstalling elastisk databas jobb | Microsoft Docs
description: "Gå igenom installationen av hello elastiska jobb."
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
ms.openlocfilehash: 0349f66a4428f81d00d43681d7f2177f273ec032
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="installing-elastic-database-jobs-overview"></a><span data-ttu-id="48038-103">Installera översikt över elastisk databas</span><span class="sxs-lookup"><span data-stu-id="48038-103">Installing Elastic Database jobs overview</span></span>
<span data-ttu-id="48038-104">[**Den elastiska databasen jobb** ](sql-database-elastic-jobs-overview.md) kan installeras via PowerShell eller via hello Azure klassiska Portal.You kan få åtkomst till toocreate och hantera jobb med hjälp av hello PowerShell API endast om du installerar hello PowerShell.</span><span class="sxs-lookup"><span data-stu-id="48038-104">[**Elastic Database jobs**](sql-database-elastic-jobs-overview.md) can be installed via PowerShell or through hello Azure Classic Portal.You can gain access toocreate and manage jobs using hello PowerShell API only if you install hello PowerShell package.</span></span> <span data-ttu-id="48038-105">Dessutom hello PowerShell APIs tillhandahålla betydligt fler funktioner än hello portal vid denna tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="48038-105">Additionally, hello PowerShell APIs provide significantly more functionality than hello portal at this point in time.</span></span>

<span data-ttu-id="48038-106">Om du redan har installerat **elastisk databas jobb** via hello portalen från en befintlig **elastisk pool**, innehåller hello senaste Powershell förhandsversionen skript tooupgrade din befintliga installation.</span><span class="sxs-lookup"><span data-stu-id="48038-106">If you have already installed **Elastic Database jobs** through hello Portal from an existing **elastic pool**, hello latest Powershell preview includes scripts tooupgrade your existing installation.</span></span> <span data-ttu-id="48038-107">Det är mycket rekommenderas tooupgrade din installation toohello senaste **elastisk databas jobb** komponenter i ordning tootake utnyttja nya funktioner som exponeras via hello PowerShell APIs.</span><span class="sxs-lookup"><span data-stu-id="48038-107">It is highly recommended tooupgrade your installation toohello latest **Elastic Database jobs** components in order tootake advantage of new functionality exposed via hello PowerShell APIs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="48038-108">Krav</span><span class="sxs-lookup"><span data-stu-id="48038-108">Prerequisites</span></span>
* <span data-ttu-id="48038-109">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="48038-109">An Azure subscription.</span></span> <span data-ttu-id="48038-110">För en kostnadsfri utvärderingsversion finns [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="48038-110">For a free trial, see [Free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="48038-111">Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="48038-111">Azure PowerShell.</span></span> <span data-ttu-id="48038-112">Installera hello senaste versionen med hello [installationsprogram för webbplattform](http://go.microsoft.com/fwlink/p/?linkid=320376).</span><span class="sxs-lookup"><span data-stu-id="48038-112">Install hello latest version using hello [Web Platform Installer](http://go.microsoft.com/fwlink/p/?linkid=320376).</span></span> <span data-ttu-id="48038-113">Detaljerad information finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="48038-113">For detailed information, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="48038-114">[NuGet Command-line Utility](https://nuget.org/nuget.exe) är används tooinstall hello elastisk databas jobb paket.</span><span class="sxs-lookup"><span data-stu-id="48038-114">[NuGet Command-line Utility](https://nuget.org/nuget.exe) is used tooinstall hello Elastic Database jobs package.</span></span> <span data-ttu-id="48038-115">Mer information finns i http://docs.nuget.org/docs/start-here/installing-nuget.</span><span class="sxs-lookup"><span data-stu-id="48038-115">For more information, see http://docs.nuget.org/docs/start-here/installing-nuget.</span></span>

## <a name="download-and-import-hello-elastic-database-jobs-powershell-package"></a><span data-ttu-id="48038-116">Hämta och importera hello elastisk databas jobb PowerShell paket</span><span class="sxs-lookup"><span data-stu-id="48038-116">Download and import hello Elastic Database jobs PowerShell package</span></span>
1. <span data-ttu-id="48038-117">Starta Microsoft Azure PowerShell-Kommandotolken och navigera toohello katalogen där du hämtade NuGet Command-line Utility (nuget.exe).</span><span class="sxs-lookup"><span data-stu-id="48038-117">Launch Microsoft Azure PowerShell command window and navigate toohello directory where you downloaded NuGet Command-line Utility (nuget.exe).</span></span>
2. <span data-ttu-id="48038-118">Hämta och importera **elastisk databas jobb** paketet till hello katalogen med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="48038-118">Download and import **Elastic Database jobs** package into hello current directory with hello following command:</span></span>
   
        PS C:\>.\nuget install Microsoft.Azure.SqlDatabase.Jobs -prerelease
   
    <span data-ttu-id="48038-119">Hej **elastisk databas jobb** filerna placeras på hello lokal katalog i en mapp med namnet **Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x** där *x.x.xxxx.x* Visar hello versionsnumret.</span><span class="sxs-lookup"><span data-stu-id="48038-119">hello **Elastic Database jobs** files are placed in hello local directory in a folder named **Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x** where *x.x.xxxx.x* reflects hello version number.</span></span> <span data-ttu-id="48038-120">hello PowerShell-cmdlet (inklusive nödvändiga klient DLL-filer) finns i hello **tools\ElasticDatabaseJobs** underkatalog hello PowerShell-skript tooinstall, uppgradera och avinstallera även finnas i hello  **Verktyg** underkatalog.</span><span class="sxs-lookup"><span data-stu-id="48038-120">hello PowerShell cmdlets (including required client .dlls) are located in hello **tools\ElasticDatabaseJobs** sub-directory and hello PowerShell scripts tooinstall, upgrade and uninstall also reside in hello **tools** sub-directory.</span></span>
3. <span data-ttu-id="48038-121">Navigera toohello verktyg underkatalog under hello Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x mappen genom att skriva CD-verktyg, till exempel:</span><span class="sxs-lookup"><span data-stu-id="48038-121">Navigate toohello tools sub-directory under hello Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x folder by typing cd tools, for example:</span></span>
   
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools

4. <span data-ttu-id="48038-122">Köra hello.\InstallElasticDatabaseJobsCmdlets.ps1 skriptet toocopy hello ElasticDatabaseJobs directory i $home\Documents\WindowsPowerShell\Modules.</span><span class="sxs-lookup"><span data-stu-id="48038-122">Execute hello .\InstallElasticDatabaseJobsCmdlets.ps1 script toocopy hello ElasticDatabaseJobs directory into $home\Documents\WindowsPowerShell\Modules.</span></span> <span data-ttu-id="48038-123">Detta också importeras automatiskt hello-modulen för användning, till exempel:</span><span class="sxs-lookup"><span data-stu-id="48038-123">This will also automatically import hello module for use, for example:</span></span>
   
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobsCmdlets.ps1
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\InstallElasticDatabaseJobsCmdlets.ps1

## <a name="install-hello-elastic-database-jobs-components-using-powershell"></a><span data-ttu-id="48038-124">Installera hello elastiska jobb med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="48038-124">Install hello Elastic Database jobs components using PowerShell</span></span>
1. <span data-ttu-id="48038-125">Starta Microsoft Azure PowerShell-Kommandotolken och navigera toohello \tools underkatalog under hello Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x mapp: Skriv cd \tools</span><span class="sxs-lookup"><span data-stu-id="48038-125">Launch a Microsoft Azure PowerShell command window and navigate toohello \tools sub-directory under hello Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x folder: Type cd \tools</span></span>
   
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools

2. <span data-ttu-id="48038-126">Köra hello.\InstallElasticDatabaseJobs.ps1 PowerShell-skript och ange värden för de begärda variablerna.</span><span class="sxs-lookup"><span data-stu-id="48038-126">Execute hello .\InstallElasticDatabaseJobs.ps1 PowerShell script and supply values for its requested variables.</span></span> <span data-ttu-id="48038-127">Det här skriptet skapar hello-komponenter som beskrivs i [elastisk databas jobb komponenter och prissättning](sql-database-elastic-jobs-overview.md#components-and-pricing) tillsammans med Konfigurera hello Azure Cloud Service tooappropriately använder hello beroende komponenter.</span><span class="sxs-lookup"><span data-stu-id="48038-127">This script will create hello components described in [Elastic Database jobs components and pricing](sql-database-elastic-jobs-overview.md#components-and-pricing) along with configuring hello Azure Cloud Service tooappropriately use hello dependent components.</span></span>

        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobs.ps1
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\InstallElasticDatabaseJobs.ps1

<span data-ttu-id="48038-128">När du kör det här kommandot ett fönster upp som ber om en **användarnamn** och **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="48038-128">When you run this command a window opens asking for a **user name** and **password**.</span></span> <span data-ttu-id="48038-129">Det är inte dina Azure-autentiseringsuppgifter, ange hello användarnamn och lösenord som hello administratörsautentiseringsuppgifter som du vill använda toocreate för hello ny server.</span><span class="sxs-lookup"><span data-stu-id="48038-129">This is not your Azure credentials, enter hello user name and password that will be hello administrator credentials you want toocreate for hello new server.</span></span>

<span data-ttu-id="48038-130">hello-parametrar som ges i det här exemplet anrop kan ändras för inställningarna.</span><span class="sxs-lookup"><span data-stu-id="48038-130">hello parameters provided on this sample invocation can be modified for your desired settings.</span></span> <span data-ttu-id="48038-131">hello följande finns mer information om hello beteendet för varje parameter:</span><span class="sxs-lookup"><span data-stu-id="48038-131">hello following provides more information on hello behavior of each parameter:</span></span>

<table style="width:100%">
  <tr>
    <th><span data-ttu-id="48038-132">Parameter</span><span class="sxs-lookup"><span data-stu-id="48038-132">Parameter</span></span></th>
    <th><span data-ttu-id="48038-133">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="48038-133">Description</span></span></th>
  </tr>

<tr>
    <td><span data-ttu-id="48038-134">resourceGroupName</span><span class="sxs-lookup"><span data-stu-id="48038-134">ResourceGroupName</span></span></td>
    <td><span data-ttu-id="48038-135">Ger hello Azure resursgruppens namn skapas toocontain hello nyskapad Azure komponenter.</span><span class="sxs-lookup"><span data-stu-id="48038-135">Provides hello Azure resource group name created toocontain hello newly created Azure components.</span></span> <span data-ttu-id="48038-136">Den här parametern standardvärden för ”__ElasticDatabaseJob”.</span><span class="sxs-lookup"><span data-stu-id="48038-136">This parameter defaults too“__ElasticDatabaseJob”.</span></span> <span data-ttu-id="48038-137">Det är inte rekommenderat toochange detta värde.</span><span class="sxs-lookup"><span data-stu-id="48038-137">It is not recommended toochange this value.</span></span></td>
    </tr>

</tr>

    <tr>
    <td><span data-ttu-id="48038-138">ResourceGroupLocation</span><span class="sxs-lookup"><span data-stu-id="48038-138">ResourceGroupLocation</span></span></td>
    <td><span data-ttu-id="48038-139">Ger hello Azure-plats toobe används för hello nyskapad Azure komponenter.</span><span class="sxs-lookup"><span data-stu-id="48038-139">Provides hello Azure location toobe used for hello newly created Azure components.</span></span> <span data-ttu-id="48038-140">Den här parametern används standard toohello centrala USA plats.</span><span class="sxs-lookup"><span data-stu-id="48038-140">This parameter defaults toohello Central US location.</span></span></td>
</tr>

<tr>
    <td><span data-ttu-id="48038-141">ServiceWorkerCount</span><span class="sxs-lookup"><span data-stu-id="48038-141">ServiceWorkerCount</span></span></td>
    <td><span data-ttu-id="48038-142">Innehåller hello antal service arbetare tooinstall.</span><span class="sxs-lookup"><span data-stu-id="48038-142">Provides hello number of service workers tooinstall.</span></span> <span data-ttu-id="48038-143">Den här parametern används standard too1.</span><span class="sxs-lookup"><span data-stu-id="48038-143">This parameter defaults too1.</span></span> <span data-ttu-id="48038-144">Ett högre antal anställda kan vara används tooscale ut hello-tjänsten och tooprovide hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="48038-144">A higher number of workers can be used tooscale out hello service and tooprovide high availability.</span></span> <span data-ttu-id="48038-145">Det rekommenderas toouse ”2” för distributioner som kräver hög tillgänglighet för hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="48038-145">It is recommended toouse “2” for deployments that require high availability of hello service.</span></span></td>
    </tr>

</tr>
    <tr>
    <td><span data-ttu-id="48038-146">ServiceVmSize</span><span class="sxs-lookup"><span data-stu-id="48038-146">ServiceVmSize</span></span></td>
    <td><span data-ttu-id="48038-147">Ger hello VM-storlek för användning i hello tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="48038-147">Provides hello VM size for usage within hello Cloud Service.</span></span> <span data-ttu-id="48038-148">Den här parametern används standard tooA0.</span><span class="sxs-lookup"><span data-stu-id="48038-148">This parameter defaults tooA0.</span></span> <span data-ttu-id="48038-149">Parametervärdena A0/A1/A2/a3 accepteras som orsakar hello worker rollen toouse en ExtraSmall/Small/Medium/Large storlek.</span><span class="sxs-lookup"><span data-stu-id="48038-149">Parameters values of A0/A1/A2/A3 are accepted which cause hello worker role toouse an ExtraSmall/Small/Medium/Large size, respectively.</span></span> <span data-ttu-id="48038-150">För mer information om storlekar för worker-rollen och se [elastisk databas jobb komponenter och prissättning](sql-database-elastic-jobs-overview.md#components-and-pricing).</span><span class="sxs-lookup"><span data-stu-id="48038-150">Fo more information on worker role sizes, see [Elastic Database jobs components and pricing](sql-database-elastic-jobs-overview.md#components-and-pricing).</span></span></td>
</tr>

</tr>
    <tr>
    <td><span data-ttu-id="48038-151">SqlServerDatabaseSlo</span><span class="sxs-lookup"><span data-stu-id="48038-151">SqlServerDatabaseSlo</span></span></td>
    <td><span data-ttu-id="48038-152">Tillhandahåller hello servicenivåmål för en Standard-utgåva.</span><span class="sxs-lookup"><span data-stu-id="48038-152">Provides hello service level objective for a Standard edition.</span></span> <span data-ttu-id="48038-153">Den här parametern används standard tooS0.</span><span class="sxs-lookup"><span data-stu-id="48038-153">This parameter defaults tooS0.</span></span> <span data-ttu-id="48038-154">Parametervärdena för S0/S1/S2/S3 accepteras som orsakar hello Azure SQL Database toouse hello respektive Servicenivåmål.</span><span class="sxs-lookup"><span data-stu-id="48038-154">Parameter values of S0/S1/S2/S3 are accepted which cause hello Azure SQL Database toouse hello respective SLO.</span></span> <span data-ttu-id="48038-155">Mer information om servicenivåmål för SQL-databasen finns [elastisk databas jobb komponenter och prissättning](sql-database-elastic-jobs-overview.md#components-and-pricing).</span><span class="sxs-lookup"><span data-stu-id="48038-155">For more information on SQL Database SLOs, see [Elastic Database jobs components and pricing](sql-database-elastic-jobs-overview.md#components-and-pricing).</span></span></td>
</tr>

</tr>
    <tr>
    <td><span data-ttu-id="48038-156">SqlServerAdministratorUserName</span><span class="sxs-lookup"><span data-stu-id="48038-156">SqlServerAdministratorUserName</span></span></td>
    <td><span data-ttu-id="48038-157">Ger hello administratörsanvändarnamnet för hello nyskapad Azure SQL Database-server.</span><span class="sxs-lookup"><span data-stu-id="48038-157">Provides hello admin user name for hello newly created Azure SQL Database server.</span></span> <span data-ttu-id="48038-158">Om inget värde anges öppnas ett fönster för PowerShell-autentiseringsuppgifter tooprompt hello autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="48038-158">When not specified, a PowerShell credentials window will open tooprompt for hello credentials.</span></span></td>
</tr>

</tr>
    <tr>
    <td><span data-ttu-id="48038-159">SqlServerAdministratorPassword</span><span class="sxs-lookup"><span data-stu-id="48038-159">SqlServerAdministratorPassword</span></span></td>
    <td><span data-ttu-id="48038-160">Tillhandahåller hello administratörslösenord för hello nyskapad Azure SQL Database-server.</span><span class="sxs-lookup"><span data-stu-id="48038-160">Provides hello admin password for hello newly created Azure SQL Database server.</span></span> <span data-ttu-id="48038-161">När inte tillhandahålls, öppnas ett fönster för PowerShell-autentiseringsuppgifter tooprompt hello autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="48038-161">When not provided, a PowerShell credentials window will open tooprompt for hello credentials.</span></span></td>
</tr>
</table>

<span data-ttu-id="48038-162">För datorer som är avsedda att ha stort antal jobb som körs parallellt mot ett stort antal databaser, bör toospecify parametrar som: - ServiceWorkerCount 2 - ServiceVmSize A2 - SqlServerDatabaseSlo S2.</span><span class="sxs-lookup"><span data-stu-id="48038-162">For systems that target having large numbers of jobs running in parallel against a large number of databases, it is recommended toospecify parameters such as: -ServiceWorkerCount 2 -ServiceVmSize A2 -SqlServerDatabaseSlo S2.</span></span>

    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobs.ps1
    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>.\InstallElasticDatabaseJobs.ps1 -ServiceWorkerCount 2 -ServiceVmSize A2 -SqlServerDatabaseSlo S2

## <a name="update-an-existing-elastic-database-jobs-components-installation-using-powershell"></a><span data-ttu-id="48038-163">Uppdatera en befintlig elastisk databas jobb komponenter installation med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="48038-163">Update an existing Elastic Database jobs components installation using PowerShell</span></span>
<span data-ttu-id="48038-164">**Den elastiska databasen jobb** kan uppdateras i en befintlig installation för skalning och hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="48038-164">**Elastic Database jobs** can be updated within an existing installation for scale and high-availability.</span></span> <span data-ttu-id="48038-165">Den här processen kan för framtida uppgraderingar av Tjänstkod utan toodrop och återskapa hello databasen.</span><span class="sxs-lookup"><span data-stu-id="48038-165">This process allows for future upgrades of service code without having toodrop and recreate hello control database.</span></span> <span data-ttu-id="48038-166">Den här processen kan också användas inom hello samma version toomodify hello service VM-storlek eller hello worker antalet servrar.</span><span class="sxs-lookup"><span data-stu-id="48038-166">This process can also be used within hello same version toomodify hello service VM size or hello server worker count.</span></span>

<span data-ttu-id="48038-167">tooupdate hello VM-storlek för en installation, kör hello följande skript med parametrar uppdaterade toohello värden du väljer.</span><span class="sxs-lookup"><span data-stu-id="48038-167">tooupdate hello VM size of an installation, run hello following script with parameters updated toohello values of your choice.</span></span>

    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>Unblock-File .\UpdateElasticDatabaseJobs.ps1
    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>.\UpdateElasticDatabaseJobs.ps1 -ServiceVmSize A1 -ServiceWorkerCount 2

<table style="width:100%">
  <tr>
  <th><span data-ttu-id="48038-168">Parameter</span><span class="sxs-lookup"><span data-stu-id="48038-168">Parameter</span></span></th>
  <th><span data-ttu-id="48038-169">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="48038-169">Description</span></span></th>
</tr>

  <tr>
    <td><span data-ttu-id="48038-170">resourceGroupName</span><span class="sxs-lookup"><span data-stu-id="48038-170">ResourceGroupName</span></span></td>
    <td><span data-ttu-id="48038-171">Identifierar hello Azure resursgruppens namn används när hello elastisk databas jobbet komponenter installerades från början.</span><span class="sxs-lookup"><span data-stu-id="48038-171">Identifies hello Azure resource group name used when hello Elastic Database job components were initially installed.</span></span> <span data-ttu-id="48038-172">Den här parametern standardvärden för ”__ElasticDatabaseJob”.</span><span class="sxs-lookup"><span data-stu-id="48038-172">This parameter defaults too“__ElasticDatabaseJob”.</span></span> <span data-ttu-id="48038-173">Eftersom det inte rekommenderas toochange det här värdet du ska inte toospecify den här parametern.</span><span class="sxs-lookup"><span data-stu-id="48038-173">Since it is not recommended toochange this value, you shouldn't have toospecify this parameter.</span></span></td>
    </tr>
</tr>

</tr>

  <tr>
    <td><span data-ttu-id="48038-174">ServiceWorkerCount</span><span class="sxs-lookup"><span data-stu-id="48038-174">ServiceWorkerCount</span></span></td>
    <td><span data-ttu-id="48038-175">Innehåller hello antal service arbetare tooinstall.</span><span class="sxs-lookup"><span data-stu-id="48038-175">Provides hello number of service workers tooinstall.</span></span>  <span data-ttu-id="48038-176">Den här parametern används standard too1.</span><span class="sxs-lookup"><span data-stu-id="48038-176">This parameter defaults too1.</span></span>  <span data-ttu-id="48038-177">Ett högre antal anställda kan vara används tooscale ut hello-tjänsten och tooprovide hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="48038-177">A higher number of workers can be used tooscale out hello service and tooprovide high availability.</span></span>  <span data-ttu-id="48038-178">Det rekommenderas toouse ”2” för distributioner som kräver hög tillgänglighet för hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="48038-178">It is recommended toouse “2” for deployments that require high availability of hello service.</span></span></td>
</tr>

</tr>

    <tr>
    <td><span data-ttu-id="48038-179">ServiceVmSize</span><span class="sxs-lookup"><span data-stu-id="48038-179">ServiceVmSize</span></span></td>
    <td><span data-ttu-id="48038-180">Ger hello VM-storlek för användning i hello tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="48038-180">Provides hello VM size for usage within hello Cloud Service.</span></span> <span data-ttu-id="48038-181">Den här parametern används standard tooA0.</span><span class="sxs-lookup"><span data-stu-id="48038-181">This parameter defaults tooA0.</span></span> <span data-ttu-id="48038-182">Parametervärdena A0/A1/A2/a3 accepteras som orsakar hello worker rollen toouse en ExtraSmall/Small/Medium/Large storlek.</span><span class="sxs-lookup"><span data-stu-id="48038-182">Parameters values of A0/A1/A2/A3 are accepted which cause hello worker role toouse an ExtraSmall/Small/Medium/Large size, respectively.</span></span> <span data-ttu-id="48038-183">För mer information om storlekar för worker-rollen och se [elastisk databas jobb komponenter och prissättning](sql-database-elastic-jobs-overview.md#components-and-pricing).</span><span class="sxs-lookup"><span data-stu-id="48038-183">Fo more information on worker role sizes, see [Elastic Database jobs components and pricing](sql-database-elastic-jobs-overview.md#components-and-pricing).</span></span></td>
</tr>

</table>

## <a name="install-hello-elastic-database-jobs-components-using-hello-portal"></a><span data-ttu-id="48038-184">Installera hello elastiska jobb med hjälp av hello Portal</span><span class="sxs-lookup"><span data-stu-id="48038-184">Install hello Elastic Database jobs components using hello Portal</span></span>
<span data-ttu-id="48038-185">När du har [skapas en elastisk pool](sql-database-elastic-pool-manage-portal.md), kan du installera **elastisk databas jobb** komponenter tooenable körning av administrativa uppgifter mot varje databas i hello elastisk pool.</span><span class="sxs-lookup"><span data-stu-id="48038-185">Once you have [created an elastic pool](sql-database-elastic-pool-manage-portal.md), you can install **Elastic Database jobs** components tooenable execution of administrative tasks against each database in hello elastic pool.</span></span> <span data-ttu-id="48038-186">Till skillnad från när du använder hello **elastisk databas jobb** PowerShell APIs hello portal gränssnittet är för närvarande begränsad tooonly körs mot en befintlig adresspool.</span><span class="sxs-lookup"><span data-stu-id="48038-186">Unlike when using hello **Elastic Database jobs** PowerShell APIs, hello portal interface is currently restricted tooonly executing against an existing pool.</span></span>

<span data-ttu-id="48038-187">**Uppskattad tid toocomplete:** 10 minuter.</span><span class="sxs-lookup"><span data-stu-id="48038-187">**Estimated time toocomplete:** 10 minutes.</span></span>

1. <span data-ttu-id="48038-188">Från hello instrumentpanelsvy för hello elastisk pool via hello [Azure Portal](https://portal.azure.com/#) , klickar du på **skapa jobbet**.</span><span class="sxs-lookup"><span data-stu-id="48038-188">From hello dashboard view of hello elastic pool via hello [Azure Portal](https://portal.azure.com/#) , click **Create job**.</span></span>
2. <span data-ttu-id="48038-189">Om du skapar ett jobb för hello första gången, måste du installera **elastisk databas jobb** genom att klicka på **FÖRHANDSGRANSKNINGSVILLKOREN**.</span><span class="sxs-lookup"><span data-stu-id="48038-189">If you are creating a job for hello first time, you must install **Elastic Database jobs** by clicking **PREVIEW TERMS**.</span></span>
3. <span data-ttu-id="48038-190">Acceptera hello villkoren genom att klicka på hello kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="48038-190">Accept hello terms by clicking hello checkbox.</span></span>
4. <span data-ttu-id="48038-191">Klicka på vyn hello ”installera tjänster” **jobbet AUTENTISERINGSUPPGIFTER**.</span><span class="sxs-lookup"><span data-stu-id="48038-191">In hello "Install services" view, click **JOB CREDENTIALS**.</span></span>
   
    ![Hello-tjänster installerades][1]
5. <span data-ttu-id="48038-193">Ange ett användarnamn och lösenord för databas-administratör. Som en del av hello installation skapas en ny Azure SQL Database-server.</span><span class="sxs-lookup"><span data-stu-id="48038-193">Type a user name and password for a database admin. As part of hello installation, a new Azure SQL Database server is created.</span></span> <span data-ttu-id="48038-194">I den här nya servern, en ny databas kallas hello kontrollen databasen skapas och toocontain hello metadata för elastisk databas jobb.</span><span class="sxs-lookup"><span data-stu-id="48038-194">Within this new server, a new database, known as hello control database, is created and used toocontain hello meta data for Elastic Database jobs.</span></span> <span data-ttu-id="48038-195">hello-användarnamn och lösenord som skapas här används för hello syftet loggning i databasen för toohello.</span><span class="sxs-lookup"><span data-stu-id="48038-195">hello user name and password created here are used for hello purpose of logging in toohello control database.</span></span> <span data-ttu-id="48038-196">En separat autentiseringsuppgifter används för skriptkörning hello databaser inom hello poolen.</span><span class="sxs-lookup"><span data-stu-id="48038-196">A separate credential is used for script execution against hello databases within hello pool.</span></span>
   
    ![Skapa användarnamn och lösenord][2]
6. <span data-ttu-id="48038-198">Klicka på OK hello-knappen.</span><span class="sxs-lookup"><span data-stu-id="48038-198">Click hello OK button.</span></span> <span data-ttu-id="48038-199">hello komponenter skapas automatiskt ett par minuter i en ny [resursgruppen](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="48038-199">hello components are created for you in a few minutes in a new [Resource group](../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="48038-200">hello ny resursgrupp fästs toohello starta board, enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="48038-200">hello new resource group is pinned toohello start board, as shown below.</span></span> <span data-ttu-id="48038-201">När du skapade, elastisk databas jobb (Cloud Service, SQL Database, Service Bus och Storage) skapas i hello-gruppen.</span><span class="sxs-lookup"><span data-stu-id="48038-201">Once created, elastic database jobs (Cloud Service, SQL Database, Service Bus, and Storage) are all created in hello group.</span></span>
   
    ![resursgrupp i start-kort][3]
7. <span data-ttu-id="48038-203">Om du försöker toocreate eller hantera ett jobb när elastisk databas jobb installeras när de tillhandahåller **autentiseringsuppgifter** hello följande meddelande visas.</span><span class="sxs-lookup"><span data-stu-id="48038-203">If you attempt toocreate or manage a job while elastic database jobs is installing, when providing **Credentials** you will see hello following message.</span></span>
   
    ![Distributionen fortfarande pågår][4]

<span data-ttu-id="48038-205">Om avinstallationen krävs, tar du bort hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="48038-205">If uninstallation is required, delete hello resource group.</span></span> <span data-ttu-id="48038-206">Se [hur toouninstall hello elastisk databas jobbet komponenter](sql-database-elastic-jobs-uninstall.md).</span><span class="sxs-lookup"><span data-stu-id="48038-206">See [How toouninstall hello Elastic Database job components](sql-database-elastic-jobs-uninstall.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="48038-207">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="48038-207">Next steps</span></span>
<span data-ttu-id="48038-208">Se till att en autentiseringsuppgift med hello behörighet för körning av skript skapas på varje databas i hello-grupp, mer information finns i [skydda SQL-databasen](sql-database-manage-logins.md).</span><span class="sxs-lookup"><span data-stu-id="48038-208">Ensure a credential with hello appropriate rights for script execution is created on each database in hello group, for more information see [Securing your SQL Database](sql-database-manage-logins.md).</span></span>
<span data-ttu-id="48038-209">Se [skapa och hantera en elastisk databas jobb](sql-database-elastic-jobs-create-and-manage.md) tooget igång.</span><span class="sxs-lookup"><span data-stu-id="48038-209">See [Creating and managing an Elastic Database jobs](sql-database-elastic-jobs-create-and-manage.md) tooget started.</span></span>

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-service-installation/screen-1.png
[2]: ./media/sql-database-elastic-jobs-service-installation/credentials.png
[3]: ./media/sql-database-elastic-jobs-service-installation/start-board.png
[4]: ./media/sql-database-elastic-jobs-service-installation/not-done.png
