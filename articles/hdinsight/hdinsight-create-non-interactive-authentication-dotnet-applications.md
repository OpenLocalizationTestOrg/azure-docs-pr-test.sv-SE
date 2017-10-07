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
# <a name="create-non-interactive-authentication-net-hdinsight-applications"></a>Skapa icke-interaktiv autentisering .NET HDInsight-program
Du kan köra din .NET Azure HDInsight-program under programmets egen identitet (icke-interaktiv) eller under hello identitet hello inloggade användaren för hello program (Interaktiv). Ett exempel på interaktivt hello-program finns i [ansluta tooAzure HDInsight](hdinsight-administer-use-dotnet-sdk.md#connect-to-azure-hdinsight). Den här artikeln beskrivs hur du toocreate icke-interaktiv autentisering .NET application tooconnect tooAzure och hantera HDInsight.

Från ditt icke-interaktiv .NET-program behöver du:

* Din Azure-prenumeration klient-ID (även kallade katalog-ID). Se [hämta klient-ID](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-tenant-id).
* hello Azure Active Directory application klient-ID. Se [skapa ett Azure Active Directory-program](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application), och [hämta ett program-ID](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key)
* hello Azure Active Directory application hemlig nyckel. Se [autentiseringsnyckel för Get-program](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key)

## <a name="prerequisites"></a>Krav
* HDInsight-kluster. Se [komma igång-självstudiekurs](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster).



## <a name="assign-azure-ad-application-toorole"></a>Tilldela toorole för Azure AD-program
Du måste tilldela hello programmet tooa [rollen](../active-directory/role-based-access-built-in-roles.md) toogrant den behörighet för att utföra åtgärder. Du kan ange hello scope på hello nivå hello prenumerationen, resursgruppen eller resursen. hello behörigheter är ärvda toolower nivåer av scope (till exempel att lägga till ett program toohello Reader roll för en resursgrupp innebär att den kan läsa hello resursgruppen och alla resurser som den innehåller). I den här självstudiekursen kommer du ange hello scope på hello resursgruppsnivå. Mer information finns i [använda rollen tilldelningar toomanage åtkomst tooyour Azure-prenumerationsresurser](../active-directory/role-based-access-control-configure.md)

**tooadd hello ägare rollen toohello Azure AD-program**

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Klicka på **resursgruppen** från hello till vänster.
3. Klicka på hello resursgruppen som innehåller hello HDInsight-kluster där du kör Hive-fråga senare i den här kursen. Om det finns för många resursgrupper kan använda du hello filter.
4. Klicka på **åtkomstkontroll (IAM)** hello resource group-menyn.
5. Klicka på **Lägg till** från hello **användare** bladet.
6. Följ hello instruktion tooadd hello **ägare** rollen toohello Azure AD-program som du skapade i föregående procedur för hello. När du slutför det har visas hello programmet visas i bladet för hello användare med hello ägarrollen.

## <a name="develop-hdinsight-client-application"></a>Utveckla klientprogram för HDInsight

1. Skapa ett C#-konsolprogram
2. Lägg till följande Nuget-paket hello:

        Install-Package Microsoft.Azure.Common.Authentication -Pre
        Install-Package Microsoft.Azure.Management.HDInsight -Pre
        Install-Package Microsoft.Azure.Management.Resources -Pre

3. Använd följande kodexempel hello:

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

## <a name="next-steps"></a>Nästa steg
* [Skapa Azure Active Directory-program och tjänstens huvudnamn med hjälp av portalen](../azure-resource-manager/resource-group-create-service-principal-portal.md)
* [Autentisera tjänstens huvudnamn med Azure Resource Manager](../azure-resource-manager/resource-group-authenticate-service-principal.md)
* [Rollbaserad åtkomstkontroll i Azure](../active-directory/role-based-access-control-configure.md)
