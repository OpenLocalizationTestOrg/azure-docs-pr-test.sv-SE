---
title: "C#: Kom igång med Azure SQL Database | Microsoft Docs"
description: "Testa SQL Database för att utveckla appar SQL och C# och skapa en Azure SQL Database med C# med hello SQL Database-biblioteket för .NET."
keywords: Testa sql, sql c#
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: cgronlun
ms.assetid: cfff2299-a474-4054-8d99-759af1ae5188
ms.service: sql-database
ms.custom: develop apps
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: csharp
ms.workload: data-management
ms.date: 10/04/2016
ms.author: sstein
ms.openlocfilehash: e880ebabd53546bea37a13186b0f1a13db35b684
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-c-toocreate-a-sql-database-with-hello-sql-database-library-for-net"></a>Använda C# toocreate en SQL-databas med hello SQL Database-biblioteket för .NET

Lär dig hur toouse C# toocreate en Azure SQL-databas med hello [Microsoft Azure SQL Management-biblioteket för .NET](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql). Den här artikeln beskriver hur toocreate en enskild databas med SQL och C#. toocreate elastiska pooler, se [skapa en elastisk pool](sql-database-elastic-pool-manage-csharp.md).

hello Azure SQL Database Management-biblioteket för .NET ger en [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md)-baserad API som omsluter hello [Resource Manager-baserade SQL Database REST API](https://docs.microsoft.com/rest/api/sql/).

> [!NOTE]
> Många nya funktioner i SQL-databas stöds endast när du använder hello [Azure Resource Manager-distributionsmodellen](../azure-resource-manager/resource-group-overview.md), så du bör alltid använda hello senaste **Azure SQL Database Management-biblioteket för .NET ([dokument](https://docs.microsoft.com/dotnet/api/overview/azure/sql?view=azure-dotnet) | [NuGet-paketet](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql))**. hello äldre [klassiska distributionsmodellen baserat bibliotek](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Sql) stöds för bakåtkompatibilitet, så vi rekommenderar att du använder hello senare Resource Manager-baserat bibliotek.
> 
> 

toocomplete hello stegen i den här artikeln, behöver du hello följande:

* En Azure-prenumeration. Om du behöver en Azure-prenumeration helt enkelt klicka **kostnadsfritt konto** hello överst i den här sidan och kommer tillbaka toofinish den här artikeln.
* Visual Studio. För en kostnadsfri version av Visual Studio finns hello [Visual Studio-hämtningar](https://www.visualstudio.com/downloads/download-visual-studio-vs) sidan.

> [!NOTE]
> Den här artikeln skapar en ny, tom SQL-databas. Ändra hello *CreateOrUpdateDatabase(...)*  metod i följande exempel toocopy databaser hello, skala databaser måste du skapa en databas i en pool osv.  
> 

## <a name="create-a-console-app-and-install-hello-required-libraries"></a>Skapa en konsolapp och installera hello krävs bibliotek
1. Starta Visual Studio.
2. Klicka på **Arkiv** > **Nytt** > **Projekt**.
3. Skapa ett C#-baserat **konsolprogram** och ge det namnet *SqlDbConsoleApp*

toocreate en SQL-databas med C# belastningen hello krävs hantering av bibliotek (med hjälp av hello [pakethanterarkonsolen](http://docs.nuget.org/Consume/Package-Manager-Console)):

1. Klicka på **Verktyg** > **NuGet Package Manager** > **Package Manager Console**.
2. Typen `Install-Package Microsoft.Azure.Management.Sql -Pre` tooinstall hello senaste [bibliotek för Microsoft Azure SQL](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql).
3. Typen `Install-Package Microsoft.Azure.Management.ResourceManager -Pre` tooinstall hello [Microsoft Azure Resource Manager Library](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager).
4. Typen `Install-Package Microsoft.Azure.Common.Authentication -Pre` tooinstall hello [vanliga Autentiseringsbibliotek för Microsoft Azure](https://www.nuget.org/packages/Microsoft.Azure.Common.Authentication). 

> [!NOTE]
> hello använder exemplen i den här artikeln en synkron form av varje API-begäran och blockera tills hello REST-anrop på hello underliggande tjänsten har slutförts. Det finns asynkrona metoder tillgängliga.
> 
> 

## <a name="create-a-sql-database-server-firewall-rule-and-sql-database---c-example"></a>Skapa en SQL Database-server, brandväggsregel och SQL-databas – C#-baserat exempel
hello följande exempel skapar en resursgrupp, server, brandväggsregel och en SQL-databas. Se, [och skapa en service principal tooaccess resurser](#create-a-service-principal-to-access-resources) tooget hello `_subscriptionId, _tenantId, _applicationId, and _applicationSecret` variabler.

Ersätt hello innehållet i **Program.cs** med följande hello och uppdatera hello `{variables}` med app-värden (inkluderar inte hello `{}`).

    using Microsoft.Azure;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.Azure.Management.Sql;
    using Microsoft.Azure.Management.Sql.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System;

    namespace SqlDbConsoleApp
    {
    class Program
       {
        // For details about these four (4) values, see
        // https://azure.microsoft.com/documentation/articles/resource-group-authenticate-service-principal/
        static string _subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _tenantId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _applicationId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _applicationSecret = "{your-password}";

        // Create management clients for hello Azure resources your app needs toowork with.
        // This app works with Resource Groups, and Azure SQL Database.
        static ResourceManagementClient _resourceMgmtClient;
        static SqlManagementClient _sqlMgmtClient;

        // Authentication token
        static AuthenticationResult _token;

        // Azure resource variables
        static string _resourceGroupName = "{resource-group-name}";
        static string _resourceGrouplocation = "{Azure-region}";

        static string _serverlocation = _resourceGrouplocation;
        static string _serverName = "{server-name}";
        static string _serverAdmin = "{server-admin-login}";
        static string _serverAdminPassword = "{server-admin-password}";

        static string _firewallRuleName = "{firewall-rule-name}";
        static string _startIpAddress = "{0.0.0.0}";
        static string _endIpAddress = "{255.255.255.255}";

        static string _databaseName = "{dbfromcsarticle}";
        static string _databaseEdition = DatabaseEditions.Basic;
        static string _databasePerfLevel = ""; // "S0", "S1", and so on here for other tiers


        static void Main(string[] args)
        {
            // Authenticate:
            _token = GetToken(_tenantId, _applicationId, _applicationSecret);
            Console.WriteLine("Token acquired. Expires on:" + _token.ExpiresOn);

            // Instantiate management clients:
            _resourceMgmtClient = new ResourceManagementClient(new Microsoft.Rest.TokenCredentials(_token.AccessToken));
            _sqlMgmtClient = new SqlManagementClient(new Microsoft.Rest.TokenCredentials(_token.AccessToken)) { SubscriptionId = _subscriptionId };

            Console.WriteLine("Resource group...");
            ResourceGroup rg = CreateOrUpdateResourceGroup(_resourceMgmtClient, _subscriptionId, _resourceGroupName, _resourceGrouplocation);
            Console.WriteLine("Resource group: " + rg.Id);


            Console.WriteLine("Server...");
            Server sgr = CreateOrUpdateServer(_sqlMgmtClient, _resourceGroupName, _serverlocation, _serverName, _serverAdmin, _serverAdminPassword);
            Console.WriteLine("Server: " + sgr.Id);

            Console.WriteLine("Server firewall...");
            FirewallRule fwr = CreateOrUpdateFirewallRule(_sqlMgmtClient, _resourceGroupName, _serverName, _firewallRuleName, _startIpAddress, _endIpAddress);
            Console.WriteLine("Server firewall: " + fwr.Id);

            Console.WriteLine("Database...");
            Database dbr = CreateOrUpdateDatabase(_sqlMgmtClient, _resourceGroupName, _serverName, _databaseName, _databaseEdition, _databasePerfLevel);
            Console.WriteLine("Database: " + dbr.Id);


            Console.WriteLine("Press any key toocontinue...");
            Console.ReadKey();
        }

        static ResourceGroup CreateOrUpdateResourceGroup(ResourceManagementClient resourceMgmtClient, string subscriptionId, string resourceGroupName, string resourceGroupLocation)
        {
            ResourceGroup resourceGroupParameters = new ResourceGroup()
            {
                Location = resourceGroupLocation,
            };
            resourceMgmtClient.SubscriptionId = subscriptionId;
            ResourceGroup resourceGroupResult = resourceMgmtClient.ResourceGroups.CreateOrUpdate(resourceGroupName, resourceGroupParameters);
            return resourceGroupResult;
        }

        static Server CreateOrUpdateServer(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverLocation, string serverName, string serverAdmin, string serverAdminPassword)
        {
            Server serverParameters = new Server()
            {
                Location = serverLocation,
                AdministratorLogin = serverAdmin,
                AdministratorLoginPassword = serverAdminPassword,
                Version = "12.0"
            };
            Server serverResult = sqlMgmtClient.Servers.CreateOrUpdate(resourceGroupName, serverName, serverParameters);
            return serverResult;
        }

        static FirewallRule CreateOrUpdateFirewallRule(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string firewallRuleName, string startIpAddress, string endIpAddress)
        {
            FirewallRule firewallParameters = new FirewallRule()
            {
                StartIpAddress = startIpAddress,
                EndIpAddress = endIpAddress
            };
            FirewallRule firewallResult = sqlMgmtClient.FirewallRules.CreateOrUpdate(resourceGroupName, serverName, firewallRuleName, firewallParameters);
            return firewallResult;
        }

        static Database CreateOrUpdateDatabase(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string databaseName, string databaseEdition, string databasePerfLevel)
        {
            // Retrieve hello server that will host this database
            Server currentServer = sqlMgmtClient.Servers.Get(resourceGroupName, serverName);

            // Create a database: configure create or update parameters and properties explicitly
            Database newDatabaseParameters = new Database()
            {
                Location = currentServer.Location,
                CreateMode = CreateMode.Default,
                Edition = databaseEdition,
                RequestedServiceObjectiveName = databasePerfLevel

            };
            Database dbResponse = sqlMgmtClient.Databases.CreateOrUpdate(resourceGroupName, serverName, databaseName, newDatabaseParameters);
            return dbResponse;
        }



        private static AuthenticationResult GetToken(string tenantId, string applicationId, string applicationSecret)
        {
            AuthenticationContext authContext = new AuthenticationContext("https://login.windows.net/" + tenantId);
            _token = authContext.AcquireToken("https://management.core.windows.net/", new ClientCredential(applicationId, applicationSecret));
            return _token;
        }
      }
    }





## <a name="create-a-service-principal-tooaccess-resources"></a>Skapa en service principal tooaccess resurser
hello skapar följande PowerShell-skript hello Active Directory (AD) och en hello tjänst huvudnamn vi behöver tooauthenticate våra C#-appen. hello skript matar ut värden som vi behöver för hello föregående C# exempel. Detaljerad information finns i [Använd Azure PowerShell toocreate ett huvudnamn för tjänsten tooaccess resurser](../azure-resource-manager/resource-group-authenticate-service-principal.md).

    # Sign in tooAzure.
    Add-AzureRmAccount

    # If you have multiple subscriptions, uncomment and set toohello subscription you want toowork with.
    #$subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}"
    #Set-AzureRmContext -SubscriptionId $subscriptionId

    # Provide these values for your new AAD app.
    # $appName is hello display name for your app, must be unique in your directory.
    # $uri does not need toobe a real uri.
    # $secret is a password you create.

    $appName = "{app-name}"
    $uri = "http://{app-name}"
    $secret = "{app-password}"

    # Create a AAD app
    $azureAdApplication = New-AzureRmADApplication -DisplayName $appName -HomePage $Uri -IdentifierUris $Uri -Password $secret

    # Create a Service Principal for hello app
    $svcprincipal = New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

    # tooavoid a PrincipalNotFound error, I pause here for 15 seconds.
    Start-Sleep -s 15

    # If you still get a PrincipalNotFound error, then rerun hello following until successful. 
    $roleassignment = New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $azureAdApplication.ApplicationId.Guid


    # Output hello values we need for our C# application toosuccessfully authenticate

    Write-Output "Copy these values into hello C# sample app"

    Write-Output "_subscriptionId:" (Get-AzureRmContext).Subscription.SubscriptionId
    Write-Output "_tenantId:" (Get-AzureRmContext).Tenant.TenantId
    Write-Output "_applicationId:" $azureAdApplication.ApplicationId.Guid
    Write-Output "_applicationSecret:" $secret



## <a name="next-steps"></a>Nästa steg
Nu när du har testat SQL Database och ställt in en databas med C#, är du redo för hello följande artiklar:

* [Ansluta tooSQL databasen med SQL Server Management Studio och utföra en exempelfråga i T-SQL](sql-database-connect-query-ssms.md)

## <a name="additional-resources"></a>Ytterligare resurser
* [SQL Database](https://azure.microsoft.com/documentation/services/sql-database/)
* [Databasklass](https://msdn.microsoft.com/library/azure/microsoft.azure.management.sql.models.database.aspx)

<!--Image references-->
[1]: ./media/sql-database-get-started-csharp/aad.png
[2]: ./media/sql-database-get-started-csharp/permissions.png
[3]: ./media/sql-database-get-started-csharp/getdomain.png
[4]: ./media/sql-database-get-started-csharp/aad2.png
[5]: ./media/sql-database-get-started-csharp/aad-applications.png
[6]: ./media/sql-database-get-started-csharp/add.png
[7]: ./media/sql-database-get-started-csharp/add-application.png
[8]: ./media/sql-database-get-started-csharp/add-application2.png
[9]: ./media/sql-database-get-started-csharp/clientid.png
