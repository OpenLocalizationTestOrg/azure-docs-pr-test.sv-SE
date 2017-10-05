---
title: "Skydda ett kluster som kör Windows med hjälp av Windows-säkerhet | Microsoft Docs"
description: "Lär dig hur du konfigurerar säkerhet nod till nod och klient-till-nod på en fristående kluster som körs på Windows med hjälp av Windows-säkerhet."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: ce3bf686-ffc4-452f-b15a-3c812aa9e672
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/24/2017
ms.author: dekapur
ms.openlocfilehash: e093a631b0cf81195981a8e3d345504ebce02723
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="secure-a-standalone-cluster-on-windows-by-using-windows-security"></a><span data-ttu-id="b5294-103">Skydda ett fristående kluster på Windows med hjälp av Windows-säkerhet</span><span class="sxs-lookup"><span data-stu-id="b5294-103">Secure a standalone cluster on Windows by using Windows security</span></span>
<span data-ttu-id="b5294-104">För att förhindra obehörig åtkomst till ett Service Fabric-kluster, måste du skydda klustret.</span><span class="sxs-lookup"><span data-stu-id="b5294-104">To prevent unauthorized access to a Service Fabric cluster, you must secure the cluster.</span></span> <span data-ttu-id="b5294-105">Säkerhet är särskilt viktigt när klustret körs produktionsarbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="b5294-105">Security is especially important when the cluster runs production workloads.</span></span> <span data-ttu-id="b5294-106">Den här artikeln beskriver hur du konfigurerar säkerhet nod till nod och klient-till-nod med hjälp av Windows-säkerhet i den *ClusterConfig.JSON* fil.</span><span class="sxs-lookup"><span data-stu-id="b5294-106">This article describes how to configure node-to-node and client-to-node security by using Windows security in the *ClusterConfig.JSON* file.</span></span>  <span data-ttu-id="b5294-107">Processen motsvarar konfigurera säkerhetssteg i [skapa ett fristående kluster som körs på Windows](service-fabric-cluster-creation-for-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="b5294-107">The process corresponds to the configure security step of [Create a standalone cluster running on Windows](service-fabric-cluster-creation-for-windows-server.md).</span></span> <span data-ttu-id="b5294-108">Mer information om hur Service Fabric använder Windows-säkerhet finns [kluster säkerhetsscenarier](service-fabric-cluster-security.md).</span><span class="sxs-lookup"><span data-stu-id="b5294-108">For more information about how Service Fabric uses Windows security, see [Cluster security scenarios](service-fabric-cluster-security.md).</span></span>

> [!NOTE]
> <span data-ttu-id="b5294-109">Du bör valet av nod till nod säkerhet noggrant eftersom det inte finns några klusteruppgradering från en alternativ till en annan.</span><span class="sxs-lookup"><span data-stu-id="b5294-109">You should consider the selection of node-to-node security carefully because there is no cluster upgrade from one security choice to another.</span></span> <span data-ttu-id="b5294-110">Om du vill ändra valet av säkerhet, måste du återskapa hela klustret.</span><span class="sxs-lookup"><span data-stu-id="b5294-110">To change the security selection, you have to rebuild the full cluster.</span></span>
>
>

## <a name="configure-windows-security-using-gmsa"></a><span data-ttu-id="b5294-111">Konfigurera Windows-säkerhet som använder gMSA</span><span class="sxs-lookup"><span data-stu-id="b5294-111">Configure Windows security using gMSA</span></span>  
<span data-ttu-id="b5294-112">Exemplet *ClusterConfig.gMSA.Windows.MultiMachine.JSON* konfigurationsfilen hämtas med den [Microsoft.Azure.ServiceFabric.WindowsServer.<version>. ZIP](http://go.microsoft.com/fwlink/?LinkId=730690) fristående klusterpaket innehåller en mall för att konfigurera Windows-säkerhet med [Grupphanterat tjänstkonto (gMSA)](https://technet.microsoft.com/library/hh831782.aspx):</span><span class="sxs-lookup"><span data-stu-id="b5294-112">The sample *ClusterConfig.gMSA.Windows.MultiMachine.JSON* configuration file downloaded with the [Microsoft.Azure.ServiceFabric.WindowsServer.<version>.zip](http://go.microsoft.com/fwlink/?LinkId=730690) standalone cluster package contains a template for configuring Windows security using [Group Managed Service Account (gMSA)](https://technet.microsoft.com/library/hh831782.aspx):</span></span>  

```  
"security": {  
            "WindowsIdentities": {  
                "ClustergMSAIdentity": "accountname@fqdn"  
                "ClusterSPN": "fqdn"  
                "ClientIdentities": [  
                    {  
                        "Identity": "domain\\username",  
                        "IsAdmin": true  
                    }  
                ]  
            }  
        }  
```  
  
| <span data-ttu-id="b5294-113">**Konfigurationsinställningen**</span><span class="sxs-lookup"><span data-stu-id="b5294-113">**Configuration Setting**</span></span> | <span data-ttu-id="b5294-114">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="b5294-114">**Description**</span></span> |  
| --- | --- |  
| <span data-ttu-id="b5294-115">WindowsIdentities</span><span class="sxs-lookup"><span data-stu-id="b5294-115">WindowsIdentities</span></span> |<span data-ttu-id="b5294-116">Innehåller klustret och klienten identiteter.</span><span class="sxs-lookup"><span data-stu-id="b5294-116">Contains the cluster and client identities.</span></span> |  
| <span data-ttu-id="b5294-117">ClustergMSAIdentity</span><span class="sxs-lookup"><span data-stu-id="b5294-117">ClustergMSAIdentity</span></span> |<span data-ttu-id="b5294-118">Konfigurerar nod till noden säkerhet.</span><span class="sxs-lookup"><span data-stu-id="b5294-118">Configures node-to-node security.</span></span> <span data-ttu-id="b5294-119">En grupp Hanterat tjänstkonto.</span><span class="sxs-lookup"><span data-stu-id="b5294-119">A group managed service account.</span></span> |  
| <span data-ttu-id="b5294-120">ClusterSPN</span><span class="sxs-lookup"><span data-stu-id="b5294-120">ClusterSPN</span></span> |<span data-ttu-id="b5294-121">Fullständigt kvalificerat domännamn SPN för gMSA-kontot</span><span class="sxs-lookup"><span data-stu-id="b5294-121">Fully qualified domain SPN for gMSA account</span></span>|  
| <span data-ttu-id="b5294-122">ClientIdentities</span><span class="sxs-lookup"><span data-stu-id="b5294-122">ClientIdentities</span></span> |<span data-ttu-id="b5294-123">Konfigurerar säkerhet för klient-till-nod.</span><span class="sxs-lookup"><span data-stu-id="b5294-123">Configures client-to-node security.</span></span> <span data-ttu-id="b5294-124">En matris med användarkonton för klienten.</span><span class="sxs-lookup"><span data-stu-id="b5294-124">An array of client user accounts.</span></span> |  
| <span data-ttu-id="b5294-125">Identitet</span><span class="sxs-lookup"><span data-stu-id="b5294-125">Identity</span></span> |<span data-ttu-id="b5294-126">Klientidentiteten domänanvändare.</span><span class="sxs-lookup"><span data-stu-id="b5294-126">The client identity, a domain user.</span></span> |  
| <span data-ttu-id="b5294-127">IsAdmin</span><span class="sxs-lookup"><span data-stu-id="b5294-127">IsAdmin</span></span> |<span data-ttu-id="b5294-128">SANT anger att domänanvändaren har administratören klientåtkomst false för klientåtkomst för användaren.</span><span class="sxs-lookup"><span data-stu-id="b5294-128">True specifies that the domain user has administrator client access, false for user client access.</span></span> |  
  
<span data-ttu-id="b5294-129">[Nod till noden säkerhet](service-fabric-cluster-security.md#node-to-node-security) konfigureras genom att ange **ClustergMSAIdentity** när service fabric måste köras under gMSA.</span><span class="sxs-lookup"><span data-stu-id="b5294-129">[Node to node security](service-fabric-cluster-security.md#node-to-node-security) is configured by setting **ClustergMSAIdentity** when service fabric needs to run under gMSA.</span></span> <span data-ttu-id="b5294-130">För att skapa betrodda relationer mellan noder, måste de göras medveten om varandra.</span><span class="sxs-lookup"><span data-stu-id="b5294-130">In order to build trust relationships between nodes, they must be made aware of each other.</span></span> <span data-ttu-id="b5294-131">Detta kan åstadkommas på två olika sätt: Ange den Grupphanterat tjänstkonto som innehåller alla noder i klustret eller datorn domängrupp som innehåller alla noder i klustret.</span><span class="sxs-lookup"><span data-stu-id="b5294-131">This can be accomplished in two different ways: Specify the Group Managed Service Account that includes all nodes in the cluster or Specify the domain machine group that includes all nodes in the cluster.</span></span> <span data-ttu-id="b5294-132">Vi rekommenderar starkt med hjälp av den [Grupphanterat tjänstkonto (gMSA)](https://technet.microsoft.com/library/hh831782.aspx) metod, särskilt för större kluster (fler än 10 noder) eller för kluster som sannolikt kommer att öka eller minska.</span><span class="sxs-lookup"><span data-stu-id="b5294-132">We strongly recommend using the [Group Managed Service Account (gMSA)](https://technet.microsoft.com/library/hh831782.aspx) approach, particularly for larger clusters (more than 10 nodes) or for clusters that are likely to grow or shrink.</span></span>  
<span data-ttu-id="b5294-133">Den här metoden kräver inte att skapa en domängrupp som klusteradministratörer har gett behörigheter att lägga till och ta bort medlemmar.</span><span class="sxs-lookup"><span data-stu-id="b5294-133">This approach does not require the creation of a domain group for which cluster administrators have been granted access rights to add and remove members.</span></span> <span data-ttu-id="b5294-134">Dessa konton är också användbara för automatisk lösenordshantering.</span><span class="sxs-lookup"><span data-stu-id="b5294-134">These accounts are also useful for automatic password management.</span></span> <span data-ttu-id="b5294-135">Mer information finns i [komma igång med Grupphanterade tjänstkonton](http://technet.microsoft.com/library/jj128431.aspx).</span><span class="sxs-lookup"><span data-stu-id="b5294-135">For more information, see [Getting Started with Group Managed Service Accounts](http://technet.microsoft.com/library/jj128431.aspx).</span></span>  
 
<span data-ttu-id="b5294-136">[Klient till noden säkerhet](service-fabric-cluster-security.md#client-to-node-security) konfigureras med hjälp av **ClientIdentities**.</span><span class="sxs-lookup"><span data-stu-id="b5294-136">[Client to node security](service-fabric-cluster-security.md#client-to-node-security) is configured using **ClientIdentities**.</span></span> <span data-ttu-id="b5294-137">För att upprätta förtroende mellan en klient och klustret måste du konfigurera klustret så att du vet vilken klient identiteter som den kan lita på.</span><span class="sxs-lookup"><span data-stu-id="b5294-137">In order to establish trust between a client and the cluster, you must configure the cluster to know which client identities that it can trust.</span></span> <span data-ttu-id="b5294-138">Detta kan göras på två olika sätt: Ange de gruppanvändare som kan ansluta eller ange nod domänanvändare som kan ansluta.</span><span class="sxs-lookup"><span data-stu-id="b5294-138">This can be done in two different ways: Specify the domain group users that can connect or specify the domain node users that can connect.</span></span> <span data-ttu-id="b5294-139">Service Fabric stöder två typer av olika åtkomstkontroll för klienter som är anslutna till ett Service Fabric-kluster: administratörs- och.</span><span class="sxs-lookup"><span data-stu-id="b5294-139">Service Fabric supports two different access control types for clients that are connected to a Service Fabric cluster: administrator and user.</span></span> <span data-ttu-id="b5294-140">Åtkomstkontroll gör möjligheten att begränsa åtkomsten till vissa typer av klusteråtgärder för olika grupper av användare, vilket gör att klustret säkrare Klusteradministratören.</span><span class="sxs-lookup"><span data-stu-id="b5294-140">Access control provides the ability for the cluster administrator to limit access to certain types of cluster operations for different groups of users, making the cluster more secure.</span></span>  <span data-ttu-id="b5294-141">Administratörer har fullständig åtkomst till funktioner för hantering (inklusive funktioner för läsning och skrivning).</span><span class="sxs-lookup"><span data-stu-id="b5294-141">Administrators have full access to management capabilities (including read/write capabilities).</span></span> <span data-ttu-id="b5294-142">Användare, har som standard bara läsbehörighet till funktioner för hantering (till exempel frågefunktioner) och möjligheten att lösa program och tjänster.</span><span class="sxs-lookup"><span data-stu-id="b5294-142">Users, by default, have only read access to management capabilities (for example, query capabilities), and the ability to resolve applications and services.</span></span> <span data-ttu-id="b5294-143">Mer information om åtkomstkontroller finns [rollbaserad åtkomstkontroll för Service Fabric-klienter](service-fabric-cluster-security-roles.md).</span><span class="sxs-lookup"><span data-stu-id="b5294-143">For more information on access controls, see [Role based access control for Service Fabric clients](service-fabric-cluster-security-roles.md).</span></span>  
 
<span data-ttu-id="b5294-144">I följande exempel **säkerhet** avsnittet konfigurerar Windows-säkerhet som använder gMSA och anger att datorer i *ServiceFabric.clusterA.contoso.com* gMSA tillhör klustret och att  *CONTOSO\usera* har åtkomst till admin-klienten:</span><span class="sxs-lookup"><span data-stu-id="b5294-144">The following example **security** section configures Windows security using gMSA and specifies that the machines in *ServiceFabric.clusterA.contoso.com* gMSA are part of the cluster and that *CONTOSO\usera* has admin client access:</span></span>  
  
```  
"security": {  
    "WindowsIdentities": {  
        "ClustergMSAIdentity" : "ServiceFabric.clusterA.contoso.com",  
        "ClusterSPN" : "clusterA.contoso.com",  
        "ClientIdentities": [{  
            "Identity": "CONTOSO\\usera",  
            "IsAdmin": true  
        }]  
    }  
}  
```  
  
## <a name="configure-windows-security-using-a-machine-group"></a><span data-ttu-id="b5294-145">Konfigurera Windows-säkerhet med hjälp av en grupp datorer</span><span class="sxs-lookup"><span data-stu-id="b5294-145">Configure Windows security using a machine group</span></span>  
<span data-ttu-id="b5294-146">Exemplet *ClusterConfig.Windows.MultiMachine.JSON* konfigurationsfilen hämtas med den [Microsoft.Azure.ServiceFabric.WindowsServer.<version>. ZIP](http://go.microsoft.com/fwlink/?LinkId=730690) fristående klusterpaket innehåller en mall för att konfigurera Windows-säkerhet.</span><span class="sxs-lookup"><span data-stu-id="b5294-146">The sample *ClusterConfig.Windows.MultiMachine.JSON* configuration file downloaded with the [Microsoft.Azure.ServiceFabric.WindowsServer.<version>.zip](http://go.microsoft.com/fwlink/?LinkId=730690) standalone cluster package contains a template for configuring Windows security.</span></span>  <span data-ttu-id="b5294-147">Windows-säkerhet har konfigurerats i den **egenskaper** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="b5294-147">Windows security is configured in the **Properties** section:</span></span> 

```
"security": {
            "ClusterCredentialType": "Windows",
            "ServerCredentialType": "Windows",
            "WindowsIdentities": {
                "ClusterIdentity" : "[domain\machinegroup]",
                "ClientIdentities": [{
                    "Identity": "[domain\username]",
                    "IsAdmin": true
                }]
            }
        }
```

| <span data-ttu-id="b5294-148">**Konfigurationsinställningen**</span><span class="sxs-lookup"><span data-stu-id="b5294-148">**Configuration setting**</span></span> | <span data-ttu-id="b5294-149">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="b5294-149">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="b5294-150">ClusterCredentialType</span><span class="sxs-lookup"><span data-stu-id="b5294-150">ClusterCredentialType</span></span> |<span data-ttu-id="b5294-151">**ClusterCredentialType** är inställd på *Windows* om ClusterIdentity anger en Active Directory datorn gruppnamn.</span><span class="sxs-lookup"><span data-stu-id="b5294-151">**ClusterCredentialType** is set to *Windows* if ClusterIdentity specifies an Active Directory Machine Group Name.</span></span> |  
| <span data-ttu-id="b5294-152">ServerCredentialType</span><span class="sxs-lookup"><span data-stu-id="b5294-152">ServerCredentialType</span></span> |<span data-ttu-id="b5294-153">Ange till *Windows* att aktivera Windows-säkerhet för klienter.</span><span class="sxs-lookup"><span data-stu-id="b5294-153">Set to *Windows* to enable Windows security for clients.</span></span><br /><br /><span data-ttu-id="b5294-154">Detta anger att klienter i klustret och själva klustret körs inom en Active Directory-domän.</span><span class="sxs-lookup"><span data-stu-id="b5294-154">This indicates that the clients of the cluster and the cluster itself are running within an Active Directory domain.</span></span> |  
| <span data-ttu-id="b5294-155">WindowsIdentities</span><span class="sxs-lookup"><span data-stu-id="b5294-155">WindowsIdentities</span></span> |<span data-ttu-id="b5294-156">Innehåller klustret och klienten identiteter.</span><span class="sxs-lookup"><span data-stu-id="b5294-156">Contains the cluster and client identities.</span></span> |  
| <span data-ttu-id="b5294-157">ClusterIdentity</span><span class="sxs-lookup"><span data-stu-id="b5294-157">ClusterIdentity</span></span> |<span data-ttu-id="b5294-158">Använd en dator gruppnamn domain\machinegroup, om du vill konfigurera nod till noden säkerhet.</span><span class="sxs-lookup"><span data-stu-id="b5294-158">Use a machine group name, domain\machinegroup, to configure node-to-node security.</span></span> |  
| <span data-ttu-id="b5294-159">ClientIdentities</span><span class="sxs-lookup"><span data-stu-id="b5294-159">ClientIdentities</span></span> |<span data-ttu-id="b5294-160">Konfigurerar säkerhet för klient-till-nod.</span><span class="sxs-lookup"><span data-stu-id="b5294-160">Configures client-to-node security.</span></span> <span data-ttu-id="b5294-161">En matris med användarkonton för klienten.</span><span class="sxs-lookup"><span data-stu-id="b5294-161">An array of client user accounts.</span></span> |  
| <span data-ttu-id="b5294-162">Identitet</span><span class="sxs-lookup"><span data-stu-id="b5294-162">Identity</span></span> |<span data-ttu-id="b5294-163">Lägg till domänanvändaren domän\användarnamn för klientens identitet.</span><span class="sxs-lookup"><span data-stu-id="b5294-163">Add the domain user, domain\username, for the client identity.</span></span> |  
| <span data-ttu-id="b5294-164">IsAdmin</span><span class="sxs-lookup"><span data-stu-id="b5294-164">IsAdmin</span></span> |<span data-ttu-id="b5294-165">Anges till true för att ange att domänanvändaren har klienten administratörsåtkomst eller false för klientåtkomst för användaren.</span><span class="sxs-lookup"><span data-stu-id="b5294-165">Set to true to specify that the domain user has administrator client access or false for user client access.</span></span> |  

<span data-ttu-id="b5294-166">[Nod till noden säkerhet](service-fabric-cluster-security.md#node-to-node-security) konfigureras med hjälp av inställningen **ClusterIdentity** om du vill använda en grupp datorer inom en Active Directory-domän.</span><span class="sxs-lookup"><span data-stu-id="b5294-166">[Node to node security](service-fabric-cluster-security.md#node-to-node-security) is configured by setting using **ClusterIdentity** if you want to use a machine group within an Active Directory Domain.</span></span> <span data-ttu-id="b5294-167">Mer information finns i [skapar en datorgruppen i Active Directory](https://msdn.microsoft.com/library/aa545347(v=cs.70).aspx).</span><span class="sxs-lookup"><span data-stu-id="b5294-167">For more information, see [Create a Machine Group in Active Directory](https://msdn.microsoft.com/library/aa545347(v=cs.70).aspx).</span></span>

<span data-ttu-id="b5294-168">[Klient-till-nod säkerhet](service-fabric-cluster-security.md#client-to-node-security) konfigureras med hjälp av **ClientIdentities**.</span><span class="sxs-lookup"><span data-stu-id="b5294-168">[Client-to-node security](service-fabric-cluster-security.md#client-to-node-security) is configured by using **ClientIdentities**.</span></span> <span data-ttu-id="b5294-169">Om du vill upprätta förtroende mellan en klient och klustret måste du konfigurera klustret för att kunna veta klienten identiteter kan lita på klustret.</span><span class="sxs-lookup"><span data-stu-id="b5294-169">To establish trust between a client and the cluster, you must configure the cluster to know the client identities that the cluster can trust.</span></span> <span data-ttu-id="b5294-170">Du kan upprätta förtroende på två olika sätt:</span><span class="sxs-lookup"><span data-stu-id="b5294-170">You can establish trust in two different ways:</span></span>

- <span data-ttu-id="b5294-171">Ange de gruppanvändare som kan ansluta.</span><span class="sxs-lookup"><span data-stu-id="b5294-171">Specify the domain group users that can connect.</span></span>
- <span data-ttu-id="b5294-172">Ange nod domänanvändare som kan ansluta.</span><span class="sxs-lookup"><span data-stu-id="b5294-172">Specify the domain node users that can connect.</span></span>

<span data-ttu-id="b5294-173">Service Fabric stöder två typer av olika åtkomstkontroll för klienter som är anslutna till ett Service Fabric-kluster: administratörs- och.</span><span class="sxs-lookup"><span data-stu-id="b5294-173">Service Fabric supports two different access control types for clients that are connected to a Service Fabric cluster: administrator and user.</span></span> <span data-ttu-id="b5294-174">Åtkomstkontroll kan Klusteradministratören att begränsa åtkomsten till vissa typer av klusteråtgärder för olika grupper av användare, vilket gör klustringen säkrare.</span><span class="sxs-lookup"><span data-stu-id="b5294-174">Access control enables the cluster administrator to limit access to certain types of cluster operations for different groups of users, which makes the cluster more secure.</span></span>  <span data-ttu-id="b5294-175">Administratörer har fullständig åtkomst till funktioner för hantering (inklusive funktioner för läsning och skrivning).</span><span class="sxs-lookup"><span data-stu-id="b5294-175">Administrators have full access to management capabilities (including read/write capabilities).</span></span> <span data-ttu-id="b5294-176">Användare, har som standard bara läsbehörighet till funktioner för hantering (till exempel frågefunktioner) och möjligheten att lösa program och tjänster.</span><span class="sxs-lookup"><span data-stu-id="b5294-176">Users, by default, have only read access to management capabilities (for example, query capabilities), and the ability to resolve applications and services.</span></span>  

<span data-ttu-id="b5294-177">I följande exempel **säkerhet** avsnittet konfigurerar Windows-säkerhet, anger att datorer i *ServiceFabric/clusterA.contoso.com* är en del av klustret, och anger att *CONTOSO\usera* har åtkomst till admin-klienten:</span><span class="sxs-lookup"><span data-stu-id="b5294-177">The following example **security** section configures Windows security, specifies that the machines in *ServiceFabric/clusterA.contoso.com* are part of the cluster, and specifies that *CONTOSO\usera* has admin client access:</span></span>

```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {
        "ClusterIdentity" : "ServiceFabric/clusterA.contoso.com",
        "ClientIdentities": [{
            "Identity": "CONTOSO\\usera",
            "IsAdmin": true
        }]
    }
},
```

> [!NOTE]
> <span data-ttu-id="b5294-178">Service Fabric bör inte distribueras på en domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="b5294-178">Service Fabric should not be deployed on a domain controller.</span></span> <span data-ttu-id="b5294-179">Kontrollera att ClusterConfig.json inte inkludera IP-adressen för domänkontrollanten när du använder en dator grupp eller grupphanterat tjänstkonto (gMSA).</span><span class="sxs-lookup"><span data-stu-id="b5294-179">Make sure that ClusterConfig.json does not include the IP address of the domain controller when using a machine group or group Managed Service Account (gMSA).</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="b5294-180">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b5294-180">Next steps</span></span>
<span data-ttu-id="b5294-181">När du har konfigurerat Windows-säkerhet i den *ClusterConfig.JSON* fil, återuppta klusterskapandeprocessen i [skapa ett fristående kluster som körs på Windows](service-fabric-cluster-creation-for-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="b5294-181">After configuring Windows security in the *ClusterConfig.JSON* file, resume the cluster creation process in [Create a standalone cluster running on Windows](service-fabric-cluster-creation-for-windows-server.md).</span></span>

<span data-ttu-id="b5294-182">Mer information om hur nod till noden säkerhet, klient-till-nod säkerhet och rollbaserad åtkomstkontroll finns [kluster säkerhetsscenarier](service-fabric-cluster-security.md).</span><span class="sxs-lookup"><span data-stu-id="b5294-182">For more information about how node-to-node security, client-to-node security, and role-based access control, see [Cluster security scenarios](service-fabric-cluster-security.md).</span></span>

<span data-ttu-id="b5294-183">Se [Anslut till en säker kluster](service-fabric-connect-to-secure-cluster.md) exempel på ansluter med hjälp av PowerShell eller FabricClient.</span><span class="sxs-lookup"><span data-stu-id="b5294-183">See [Connect to a secure cluster](service-fabric-connect-to-secure-cluster.md) for examples of connecting by using PowerShell or FabricClient.</span></span>
