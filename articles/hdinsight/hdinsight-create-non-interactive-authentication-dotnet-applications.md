---
title: "aaaCreate icke-interaktiv autentisering .NET HDInsight skall det finnas bestämmelser - Azure | Microsoft Docs"
description: "Lär dig hur toocreate icke-interaktiv autentisering .NET HDInsight-program."
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
ms.assetid: 8e32430f-6404-498a-9fcd-f20338d964af
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: 5367c160b0146e6b855486b95f363e8fe7f1c98f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-non-interactive-authentication-net-hdinsight-applications"></a><span data-ttu-id="8423d-103">Skapa icke-interaktiv autentisering .NET HDInsight-program</span><span class="sxs-lookup"><span data-stu-id="8423d-103">Create non-interactive authentication .NET HDInsight applications</span></span>
<span data-ttu-id="8423d-104">Du kan köra din .NET Azure HDInsight-program under programmets egen identitet (icke-interaktiv) eller under hello identitet hello inloggade användaren för hello program (Interaktiv).</span><span class="sxs-lookup"><span data-stu-id="8423d-104">You can run your .NET Azure HDInsight application either under application's own identity (non-interactive) or under hello identity of hello signed-in user of hello application (interactive).</span></span> <span data-ttu-id="8423d-105">Ett exempel på interaktivt hello-program finns i [ansluta tooAzure HDInsight](hdinsight-administer-use-dotnet-sdk.md#connect-to-azure-hdinsight).</span><span class="sxs-lookup"><span data-stu-id="8423d-105">For a sample of hello interactive application, see [Connect tooAzure HDInsight](hdinsight-administer-use-dotnet-sdk.md#connect-to-azure-hdinsight).</span></span> <span data-ttu-id="8423d-106">Den här artikeln beskrivs hur du toocreate icke-interaktiv autentisering .NET application tooconnect tooAzure och hantera HDInsight.</span><span class="sxs-lookup"><span data-stu-id="8423d-106">This article shows you how toocreate non-interactive authentication .NET application tooconnect tooAzure and manage HDInsight.</span></span>

<span data-ttu-id="8423d-107">Från ditt icke-interaktiv .NET-program behöver du:</span><span class="sxs-lookup"><span data-stu-id="8423d-107">From your non-interactive .NET application, you need:</span></span>

* <span data-ttu-id="8423d-108">Din Azure-prenumeration klient-ID (även kallade katalog-ID).</span><span class="sxs-lookup"><span data-stu-id="8423d-108">Your Azure subscription tenant ID (A.K.A directory ID).</span></span> <span data-ttu-id="8423d-109">Se [hämta klient-ID](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-tenant-id).</span><span class="sxs-lookup"><span data-stu-id="8423d-109">See [Get tenant ID](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-tenant-id).</span></span>
* <span data-ttu-id="8423d-110">hello Azure Active Directory application klient-ID.</span><span class="sxs-lookup"><span data-stu-id="8423d-110">hello Azure Active Directory application client ID.</span></span> <span data-ttu-id="8423d-111">Se [skapa ett Azure Active Directory-program](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application), och [hämta ett program-ID](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key)</span><span class="sxs-lookup"><span data-stu-id="8423d-111">See [Create an Azure Active Directory application](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application), and [Get an application ID](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key)</span></span>
* <span data-ttu-id="8423d-112">hello Azure Active Directory application hemlig nyckel.</span><span class="sxs-lookup"><span data-stu-id="8423d-112">hello Azure Active Directory application secret key.</span></span> <span data-ttu-id="8423d-113">Se [autentiseringsnyckel för Get-program](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key)</span><span class="sxs-lookup"><span data-stu-id="8423d-113">See [Get application authentication key](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8423d-114">Krav</span><span class="sxs-lookup"><span data-stu-id="8423d-114">Prerequisites</span></span>
* <span data-ttu-id="8423d-115">HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="8423d-115">HDInsight cluster.</span></span> <span data-ttu-id="8423d-116">Se [komma igång-självstudiekurs](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster).</span><span class="sxs-lookup"><span data-stu-id="8423d-116">See [getting started tutorial](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster).</span></span>



## <a name="assign-azure-ad-application-toorole"></a><span data-ttu-id="8423d-117">Tilldela toorole för Azure AD-program</span><span class="sxs-lookup"><span data-stu-id="8423d-117">Assign Azure AD application toorole</span></span>
<span data-ttu-id="8423d-118">Du måste tilldela hello programmet tooa [rollen](../active-directory/role-based-access-built-in-roles.md) toogrant den behörighet för att utföra åtgärder.</span><span class="sxs-lookup"><span data-stu-id="8423d-118">You must assign hello application tooa [role](../active-directory/role-based-access-built-in-roles.md) toogrant it permissions for performing actions.</span></span> <span data-ttu-id="8423d-119">Du kan ange hello scope på hello nivå hello prenumerationen, resursgruppen eller resursen.</span><span class="sxs-lookup"><span data-stu-id="8423d-119">You can set hello scope at hello level of hello subscription, resource group, or resource.</span></span> <span data-ttu-id="8423d-120">hello behörigheter är ärvda toolower nivåer av scope (till exempel att lägga till ett program toohello Reader roll för en resursgrupp innebär att den kan läsa hello resursgruppen och alla resurser som den innehåller).</span><span class="sxs-lookup"><span data-stu-id="8423d-120">hello permissions are inherited toolower levels of scope (for example, adding an application toohello Reader role for a resource group means it can read hello resource group and any resources it contains).</span></span> <span data-ttu-id="8423d-121">I den här självstudiekursen kommer du ange hello scope på hello resursgruppsnivå.</span><span class="sxs-lookup"><span data-stu-id="8423d-121">In this tutorial, you will set hello scope at hello resource group level.</span></span> <span data-ttu-id="8423d-122">Mer information finns i [använda rollen tilldelningar toomanage åtkomst tooyour Azure-prenumerationsresurser](../active-directory/role-based-access-control-configure.md)</span><span class="sxs-lookup"><span data-stu-id="8423d-122">For more information, see [Use role assignments toomanage access tooyour Azure subscription resources](../active-directory/role-based-access-control-configure.md)</span></span>

<span data-ttu-id="8423d-123">**tooadd hello ägare rollen toohello Azure AD-program**</span><span class="sxs-lookup"><span data-stu-id="8423d-123">**tooadd hello Owner role toohello Azure AD application**</span></span>

1. <span data-ttu-id="8423d-124">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8423d-124">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="8423d-125">Klicka på **resursgruppen** från hello till vänster.</span><span class="sxs-lookup"><span data-stu-id="8423d-125">Click **Resource Group** from hello left pane.</span></span>
3. <span data-ttu-id="8423d-126">Klicka på hello resursgruppen som innehåller hello HDInsight-kluster där du kör Hive-fråga senare i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="8423d-126">Click hello resource group that contains hello HDInsight cluster where you will run your Hive query later in this tutorial.</span></span> <span data-ttu-id="8423d-127">Om det finns för många resursgrupper kan använda du hello filter.</span><span class="sxs-lookup"><span data-stu-id="8423d-127">If there are too many resource groups, you can use hello filter.</span></span>
4. <span data-ttu-id="8423d-128">Klicka på **åtkomstkontroll (IAM)** hello resource group-menyn.</span><span class="sxs-lookup"><span data-stu-id="8423d-128">Click **Access control (IAM)** from hello resource group menu.</span></span>
5. <span data-ttu-id="8423d-129">Klicka på **Lägg till** från hello **användare** bladet.</span><span class="sxs-lookup"><span data-stu-id="8423d-129">Click **Add** from hello **Users** blade.</span></span>
6. <span data-ttu-id="8423d-130">Följ hello instruktion tooadd hello **ägare** rollen toohello Azure AD-program som du skapade i föregående procedur för hello.</span><span class="sxs-lookup"><span data-stu-id="8423d-130">Follow hello instruction tooadd hello **Owner** role toohello Azure AD application you created in hello last procedure.</span></span> <span data-ttu-id="8423d-131">När du slutför det har visas hello programmet visas i bladet för hello användare med hello ägarrollen.</span><span class="sxs-lookup"><span data-stu-id="8423d-131">When you complete it successfully, you shall see hello application listed in hello Users blade with hello Owner role.</span></span>

## <a name="develop-hdinsight-client-application"></a><span data-ttu-id="8423d-132">Utveckla klientprogram för HDInsight</span><span class="sxs-lookup"><span data-stu-id="8423d-132">Develop HDInsight client application</span></span>

1. <span data-ttu-id="8423d-133">Skapa ett C#-konsolprogram</span><span class="sxs-lookup"><span data-stu-id="8423d-133">Create a C# console application.</span></span>
2. <span data-ttu-id="8423d-134">Lägg till följande Nuget-paket hello:</span><span class="sxs-lookup"><span data-stu-id="8423d-134">Add hello following Nuget packages:</span></span>

        Install-Package Microsoft.Azure.Common.Authentication -Pre
        Install-Package Microsoft.Azure.Management.HDInsight -Pre
        Install-Package Microsoft.Azure.Management.Resources -Pre

3. <span data-ttu-id="8423d-135">Använd följande kodexempel hello:</span><span class="sxs-lookup"><span data-stu-id="8423d-135">Use hello following code sample:</span></span>

        using System;
        using System.Security;
        using Microsoft.Azure;
        using Microsoft.Azure.Common.Authentication;
        using Microsoft.Azure.Common.Authentication.Factories;
        using Microsoft.Azure.Common.Authentication.Models;
        using Microsoft.Azure.Management.Resources;
        using Microsoft.Azure.Management.HDInsight;
        
        namespace CreateHDICluster
        {
            internal class Program
            {
                private static HDInsightManagementClient _hdiManagementClient;
        
                private static Guid SubscriptionId = new Guid("<Enter Your Azure Subscription ID>");
                private static string tenantID = "<Enter Your Tenant ID (A.K.A. Directory ID)>";
                private static string applicationID = "<Enter Your Application ID>";
                private static string secretKey = "<Enter hello Application Secret Key>";
        
                private static void Main(string[] args)
                {
                    var key = new SecureString();
                    foreach (char c in secretKey) { key.AppendChar(c); }

                    var tokenCreds = GetTokenCloudCredentials(tenantID, applicationID, key);
                    var subCloudCredentials = GetSubscriptionCloudCredentials(tokenCreds, SubscriptionId);
        
                    var resourceManagementClient = new ResourceManagementClient(subCloudCredentials);
                    resourceManagementClient.Providers.Register("Microsoft.HDInsight");
        
                    _hdiManagementClient = new HDInsightManagementClient(subCloudCredentials);
        
                    var results = _hdiManagementClient.Clusters.List();
                    foreach (var name in results.Clusters)
                    {
                        Console.WriteLine("Cluster Name: " + name.Name);
                        Console.WriteLine("\t Cluster type: " + name.Properties.ClusterDefinition.ClusterType);
                        Console.WriteLine("\t Cluster location: " + name.Location);
                        Console.WriteLine("\t Cluster version: " + name.Properties.ClusterVersion);
                    }
                    Console.WriteLine("Press Enter toocontinue");
                    Console.ReadLine();
                }

                /// Get hello access token for a service principal and provided key                
                public static TokenCloudCredentials GetTokenCloudCredentials(string tenantId, string clientId, SecureString secretKey)
                {
                    var authFactory = new AuthenticationFactory();
                    var account = new AzureAccount { Type = AzureAccount.AccountType.ServicePrincipal, Id = clientId };
                    var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
                    var accessToken =
                        authFactory.Authenticate(account, env, tenantId, secretKey, ShowDialog.Never).AccessToken;
        
                    return new TokenCloudCredentials(accessToken);
                }
        
                public static SubscriptionCloudCredentials GetSubscriptionCloudCredentials(SubscriptionCloudCredentials creds, Guid subId)
                {
                    return new TokenCloudCredentials(subId.ToString(), ((TokenCloudCredentials)creds).Token);
                }
            }
        }

## <a name="next-steps"></a><span data-ttu-id="8423d-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8423d-136">Next steps</span></span>
* [<span data-ttu-id="8423d-137">Skapa Azure Active Directory-program och tjänstens huvudnamn med hjälp av portalen</span><span class="sxs-lookup"><span data-stu-id="8423d-137">Create Azure Active Directory application and service principal using portal</span></span>](../azure-resource-manager/resource-group-create-service-principal-portal.md)
* [<span data-ttu-id="8423d-138">Autentisera tjänstens huvudnamn med Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="8423d-138">Authenticate service principal with Azure Resource Manager</span></span>](../azure-resource-manager/resource-group-authenticate-service-principal.md)
* [<span data-ttu-id="8423d-139">Rollbaserad åtkomstkontroll i Azure</span><span class="sxs-lookup"><span data-stu-id="8423d-139">Azure Role-Based Access Control</span></span>](../active-directory/role-based-access-control-configure.md)
