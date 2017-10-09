---
title: "aaaGet värden för app - autentisering i Azure SQL Database | Microsoft Docs"
description: "Skapa ett huvudnamn för tjänsten för att komma åt SQL-databas från kod."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
tags: 
ms.assetid: b43e43bb-6660-49e6-b069-abde97eb5770
ms.service: sql-database
ms.custom: develop apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 09/30/2016
ms.author: sstein
ms.openlocfilehash: b57dc075ec9e679da9f2f5fa53e02312539cdf07
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-required-values-for-authenticating-an-application-tooaccess-sql-database-from-code"></a><span data-ttu-id="b59a7-103">Hämta hello krävs värden för att autentisera en programmet tooaccess SQL-databas från kod</span><span class="sxs-lookup"><span data-stu-id="b59a7-103">Get hello required values for authenticating an application tooaccess SQL Database from code</span></span>
<span data-ttu-id="b59a7-104">toocreate och hantera SQL-databas från kod måste du registrera din app i hello Azure Active Directory (AAD)-domän i hello prenumeration där resurserna i Azure har skapats.</span><span class="sxs-lookup"><span data-stu-id="b59a7-104">toocreate and manage SQL Database from code you must register your app in hello Azure Active Directory (AAD) domain  in hello subscription where your Azure resources have been created.</span></span>

## <a name="create-a-service-principal-tooaccess-resources-from-an-application"></a><span data-ttu-id="b59a7-105">Skapa en service principal tooaccess resurser från ett program</span><span class="sxs-lookup"><span data-stu-id="b59a7-105">Create a service principal tooaccess resources from an application</span></span>
<span data-ttu-id="b59a7-106">Du behöver toohave hello senaste [Azure PowerShell](https://msdn.microsoft.com/library/mt619274.aspx) installerad och körs.</span><span class="sxs-lookup"><span data-stu-id="b59a7-106">You need toohave hello latest [Azure PowerShell](https://msdn.microsoft.com/library/mt619274.aspx) installed and running.</span></span> <span data-ttu-id="b59a7-107">Detaljerad information finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="b59a7-107">For detailed information, see [How tooinstall and configure Azure PowerShell](/powershell/azureps-cmdlets-docs).</span></span>

<span data-ttu-id="b59a7-108">hello skapar följande PowerShell-skript hello Active Directory (AD) och en hello tjänst huvudnamn vi behöver tooauthenticate våra C#-appen.</span><span class="sxs-lookup"><span data-stu-id="b59a7-108">hello following PowerShell script creates hello Active Directory (AD) application and hello service principal that we need tooauthenticate our C# app.</span></span> <span data-ttu-id="b59a7-109">hello skript matar ut värden som vi behöver för hello föregående C# exempel.</span><span class="sxs-lookup"><span data-stu-id="b59a7-109">hello script outputs values we need for hello preceding C# sample.</span></span> <span data-ttu-id="b59a7-110">Detaljerad information finns i [Använd Azure PowerShell toocreate ett huvudnamn för tjänsten tooaccess resurser](../azure-resource-manager/resource-group-authenticate-service-principal.md).</span><span class="sxs-lookup"><span data-stu-id="b59a7-110">For detailed information, see [Use Azure PowerShell toocreate a service principal tooaccess resources](../azure-resource-manager/resource-group-authenticate-service-principal.md).</span></span>

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




## <a name="see-also"></a><span data-ttu-id="b59a7-111">Se även</span><span class="sxs-lookup"><span data-stu-id="b59a7-111">See also</span></span>
* [<span data-ttu-id="b59a7-112">Skapa en SQL-databas med C#</span><span class="sxs-lookup"><span data-stu-id="b59a7-112">Create a SQL database with C#</span></span>](sql-database-get-started-csharp.md)
* [<span data-ttu-id="b59a7-113">Ansluta tooSQL databasen med hjälp av Azure Active Directory-autentisering</span><span class="sxs-lookup"><span data-stu-id="b59a7-113">Connecting tooSQL Database By Using Azure Active Directory Authentication</span></span>](sql-database-aad-authentication.md)

