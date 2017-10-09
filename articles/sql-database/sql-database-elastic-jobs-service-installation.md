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
# <a name="installing-elastic-database-jobs-overview"></a>Installera översikt över elastisk databas
[**Den elastiska databasen jobb** ](sql-database-elastic-jobs-overview.md) kan installeras via PowerShell eller via hello Azure klassiska Portal.You kan få åtkomst till toocreate och hantera jobb med hjälp av hello PowerShell API endast om du installerar hello PowerShell. Dessutom hello PowerShell APIs tillhandahålla betydligt fler funktioner än hello portal vid denna tidpunkt.

Om du redan har installerat **elastisk databas jobb** via hello portalen från en befintlig **elastisk pool**, innehåller hello senaste Powershell förhandsversionen skript tooupgrade din befintliga installation. Det är mycket rekommenderas tooupgrade din installation toohello senaste **elastisk databas jobb** komponenter i ordning tootake utnyttja nya funktioner som exponeras via hello PowerShell APIs.

## <a name="prerequisites"></a>Krav
* En Azure-prenumeration. För en kostnadsfri utvärderingsversion finns [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).
* Azure PowerShell. Installera hello senaste versionen med hello [installationsprogram för webbplattform](http://go.microsoft.com/fwlink/p/?linkid=320376). Detaljerad information finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).
* [NuGet Command-line Utility](https://nuget.org/nuget.exe) är används tooinstall hello elastisk databas jobb paket. Mer information finns i http://docs.nuget.org/docs/start-here/installing-nuget.

## <a name="download-and-import-hello-elastic-database-jobs-powershell-package"></a>Hämta och importera hello elastisk databas jobb PowerShell paket
1. Starta Microsoft Azure PowerShell-Kommandotolken och navigera toohello katalogen där du hämtade NuGet Command-line Utility (nuget.exe).
2. Hämta och importera **elastisk databas jobb** paketet till hello katalogen med hello följande kommando:
   
        PS C:\>.\nuget install Microsoft.Azure.SqlDatabase.Jobs -prerelease
   
    Hej **elastisk databas jobb** filerna placeras på hello lokal katalog i en mapp med namnet **Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x** där *x.x.xxxx.x* Visar hello versionsnumret. hello PowerShell-cmdlet (inklusive nödvändiga klient DLL-filer) finns i hello **tools\ElasticDatabaseJobs** underkatalog hello PowerShell-skript tooinstall, uppgradera och avinstallera även finnas i hello  **Verktyg** underkatalog.
3. Navigera toohello verktyg underkatalog under hello Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x mappen genom att skriva CD-verktyg, till exempel:
   
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools

4. Köra hello.\InstallElasticDatabaseJobsCmdlets.ps1 skriptet toocopy hello ElasticDatabaseJobs directory i $home\Documents\WindowsPowerShell\Modules. Detta också importeras automatiskt hello-modulen för användning, till exempel:
   
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobsCmdlets.ps1
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\InstallElasticDatabaseJobsCmdlets.ps1

## <a name="install-hello-elastic-database-jobs-components-using-powershell"></a>Installera hello elastiska jobb med hjälp av PowerShell
1. Starta Microsoft Azure PowerShell-Kommandotolken och navigera toohello \tools underkatalog under hello Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x mapp: Skriv cd \tools
   
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools

2. Köra hello.\InstallElasticDatabaseJobs.ps1 PowerShell-skript och ange värden för de begärda variablerna. Det här skriptet skapar hello-komponenter som beskrivs i [elastisk databas jobb komponenter och prissättning](sql-database-elastic-jobs-overview.md#components-and-pricing) tillsammans med Konfigurera hello Azure Cloud Service tooappropriately använder hello beroende komponenter.

        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobs.ps1
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\InstallElasticDatabaseJobs.ps1

När du kör det här kommandot ett fönster upp som ber om en **användarnamn** och **lösenord**. Det är inte dina Azure-autentiseringsuppgifter, ange hello användarnamn och lösenord som hello administratörsautentiseringsuppgifter som du vill använda toocreate för hello ny server.

hello-parametrar som ges i det här exemplet anrop kan ändras för inställningarna. hello följande finns mer information om hello beteendet för varje parameter:

<table style="width:100%">
  <tr>
    <th>Parameter</th>
    <th>Beskrivning</th>
  </tr>

<tr>
    <td>resourceGroupName</td>
    <td>Ger hello Azure resursgruppens namn skapas toocontain hello nyskapad Azure komponenter. Den här parametern standardvärden för ”__ElasticDatabaseJob”. Det är inte rekommenderat toochange detta värde.</td>
    </tr>

</tr>

    <tr>
    <td>ResourceGroupLocation</td>
    <td>Ger hello Azure-plats toobe används för hello nyskapad Azure komponenter. Den här parametern används standard toohello centrala USA plats.</td>
</tr>

<tr>
    <td>ServiceWorkerCount</td>
    <td>Innehåller hello antal service arbetare tooinstall. Den här parametern används standard too1. Ett högre antal anställda kan vara används tooscale ut hello-tjänsten och tooprovide hög tillgänglighet. Det rekommenderas toouse ”2” för distributioner som kräver hög tillgänglighet för hello-tjänsten.</td>
    </tr>

</tr>
    <tr>
    <td>ServiceVmSize</td>
    <td>Ger hello VM-storlek för användning i hello tjänst i molnet. Den här parametern används standard tooA0. Parametervärdena A0/A1/A2/a3 accepteras som orsakar hello worker rollen toouse en ExtraSmall/Small/Medium/Large storlek. För mer information om storlekar för worker-rollen och se [elastisk databas jobb komponenter och prissättning](sql-database-elastic-jobs-overview.md#components-and-pricing).</td>
</tr>

</tr>
    <tr>
    <td>SqlServerDatabaseSlo</td>
    <td>Tillhandahåller hello servicenivåmål för en Standard-utgåva. Den här parametern används standard tooS0. Parametervärdena för S0/S1/S2/S3 accepteras som orsakar hello Azure SQL Database toouse hello respektive Servicenivåmål. Mer information om servicenivåmål för SQL-databasen finns [elastisk databas jobb komponenter och prissättning](sql-database-elastic-jobs-overview.md#components-and-pricing).</td>
</tr>

</tr>
    <tr>
    <td>SqlServerAdministratorUserName</td>
    <td>Ger hello administratörsanvändarnamnet för hello nyskapad Azure SQL Database-server. Om inget värde anges öppnas ett fönster för PowerShell-autentiseringsuppgifter tooprompt hello autentiseringsuppgifter.</td>
</tr>

</tr>
    <tr>
    <td>SqlServerAdministratorPassword</td>
    <td>Tillhandahåller hello administratörslösenord för hello nyskapad Azure SQL Database-server. När inte tillhandahålls, öppnas ett fönster för PowerShell-autentiseringsuppgifter tooprompt hello autentiseringsuppgifter.</td>
</tr>
</table>

För datorer som är avsedda att ha stort antal jobb som körs parallellt mot ett stort antal databaser, bör toospecify parametrar som: - ServiceWorkerCount 2 - ServiceVmSize A2 - SqlServerDatabaseSlo S2.

    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobs.ps1
    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>.\InstallElasticDatabaseJobs.ps1 -ServiceWorkerCount 2 -ServiceVmSize A2 -SqlServerDatabaseSlo S2

## <a name="update-an-existing-elastic-database-jobs-components-installation-using-powershell"></a>Uppdatera en befintlig elastisk databas jobb komponenter installation med hjälp av PowerShell
**Den elastiska databasen jobb** kan uppdateras i en befintlig installation för skalning och hög tillgänglighet. Den här processen kan för framtida uppgraderingar av Tjänstkod utan toodrop och återskapa hello databasen. Den här processen kan också användas inom hello samma version toomodify hello service VM-storlek eller hello worker antalet servrar.

tooupdate hello VM-storlek för en installation, kör hello följande skript med parametrar uppdaterade toohello värden du väljer.

    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>Unblock-File .\UpdateElasticDatabaseJobs.ps1
    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>.\UpdateElasticDatabaseJobs.ps1 -ServiceVmSize A1 -ServiceWorkerCount 2

<table style="width:100%">
  <tr>
  <th>Parameter</th>
  <th>Beskrivning</th>
</tr>

  <tr>
    <td>resourceGroupName</td>
    <td>Identifierar hello Azure resursgruppens namn används när hello elastisk databas jobbet komponenter installerades från början. Den här parametern standardvärden för ”__ElasticDatabaseJob”. Eftersom det inte rekommenderas toochange det här värdet du ska inte toospecify den här parametern.</td>
    </tr>
</tr>

</tr>

  <tr>
    <td>ServiceWorkerCount</td>
    <td>Innehåller hello antal service arbetare tooinstall.  Den här parametern används standard too1.  Ett högre antal anställda kan vara används tooscale ut hello-tjänsten och tooprovide hög tillgänglighet.  Det rekommenderas toouse ”2” för distributioner som kräver hög tillgänglighet för hello-tjänsten.</td>
</tr>

</tr>

    <tr>
    <td>ServiceVmSize</td>
    <td>Ger hello VM-storlek för användning i hello tjänst i molnet. Den här parametern används standard tooA0. Parametervärdena A0/A1/A2/a3 accepteras som orsakar hello worker rollen toouse en ExtraSmall/Small/Medium/Large storlek. För mer information om storlekar för worker-rollen och se [elastisk databas jobb komponenter och prissättning](sql-database-elastic-jobs-overview.md#components-and-pricing).</td>
</tr>

</table>

## <a name="install-hello-elastic-database-jobs-components-using-hello-portal"></a>Installera hello elastiska jobb med hjälp av hello Portal
När du har [skapas en elastisk pool](sql-database-elastic-pool-manage-portal.md), kan du installera **elastisk databas jobb** komponenter tooenable körning av administrativa uppgifter mot varje databas i hello elastisk pool. Till skillnad från när du använder hello **elastisk databas jobb** PowerShell APIs hello portal gränssnittet är för närvarande begränsad tooonly körs mot en befintlig adresspool.

**Uppskattad tid toocomplete:** 10 minuter.

1. Från hello instrumentpanelsvy för hello elastisk pool via hello [Azure Portal](https://portal.azure.com/#) , klickar du på **skapa jobbet**.
2. Om du skapar ett jobb för hello första gången, måste du installera **elastisk databas jobb** genom att klicka på **FÖRHANDSGRANSKNINGSVILLKOREN**.
3. Acceptera hello villkoren genom att klicka på hello kryssrutan.
4. Klicka på vyn hello ”installera tjänster” **jobbet AUTENTISERINGSUPPGIFTER**.
   
    ![Hello-tjänster installerades][1]
5. Ange ett användarnamn och lösenord för databas-administratör. Som en del av hello installation skapas en ny Azure SQL Database-server. I den här nya servern, en ny databas kallas hello kontrollen databasen skapas och toocontain hello metadata för elastisk databas jobb. hello-användarnamn och lösenord som skapas här används för hello syftet loggning i databasen för toohello. En separat autentiseringsuppgifter används för skriptkörning hello databaser inom hello poolen.
   
    ![Skapa användarnamn och lösenord][2]
6. Klicka på OK hello-knappen. hello komponenter skapas automatiskt ett par minuter i en ny [resursgruppen](../azure-resource-manager/resource-group-overview.md). hello ny resursgrupp fästs toohello starta board, enligt nedan. När du skapade, elastisk databas jobb (Cloud Service, SQL Database, Service Bus och Storage) skapas i hello-gruppen.
   
    ![resursgrupp i start-kort][3]
7. Om du försöker toocreate eller hantera ett jobb när elastisk databas jobb installeras när de tillhandahåller **autentiseringsuppgifter** hello följande meddelande visas.
   
    ![Distributionen fortfarande pågår][4]

Om avinstallationen krävs, tar du bort hello resursgruppen. Se [hur toouninstall hello elastisk databas jobbet komponenter](sql-database-elastic-jobs-uninstall.md).

## <a name="next-steps"></a>Nästa steg
Se till att en autentiseringsuppgift med hello behörighet för körning av skript skapas på varje databas i hello-grupp, mer information finns i [skydda SQL-databasen](sql-database-manage-logins.md).
Se [skapa och hantera en elastisk databas jobb](sql-database-elastic-jobs-create-and-manage.md) tooget igång.

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-service-installation/screen-1.png
[2]: ./media/sql-database-elastic-jobs-service-installation/credentials.png
[3]: ./media/sql-database-elastic-jobs-service-installation/start-board.png
[4]: ./media/sql-database-elastic-jobs-service-installation/not-done.png
