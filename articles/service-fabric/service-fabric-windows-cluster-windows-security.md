---
title: "aaaSecure ett kluster som körs på Windows med hjälp av Windows-säkerhet | Microsoft Docs"
description: "Lär dig hur säkerhet för tooconfigure nod till nod och klient-till-nod på en fristående kluster som körs på Windows med hjälp av Windows-säkerhet."
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
ms.openlocfilehash: 44f3011eb630357f342052a48d6c852b17dccec4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="secure-a-standalone-cluster-on-windows-by-using-windows-security"></a><span data-ttu-id="85d4c-103">Skydda ett fristående kluster på Windows med hjälp av Windows-säkerhet</span><span class="sxs-lookup"><span data-stu-id="85d4c-103">Secure a standalone cluster on Windows by using Windows security</span></span>
<span data-ttu-id="85d4c-104">tooprevent obehörig åtkomst tooa Service Fabric-kluster, måste du skydda hello klustret.</span><span class="sxs-lookup"><span data-stu-id="85d4c-104">tooprevent unauthorized access tooa Service Fabric cluster, you must secure hello cluster.</span></span> <span data-ttu-id="85d4c-105">Säkerhet är särskilt viktigt när hello klustret körs produktionsarbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="85d4c-105">Security is especially important when hello cluster runs production workloads.</span></span> <span data-ttu-id="85d4c-106">Den här artikeln beskriver hur säkerhet för tooconfigure nod till nod och klient-till-nod med hjälp av Windows-säkerhet i hello *ClusterConfig.JSON* fil.</span><span class="sxs-lookup"><span data-stu-id="85d4c-106">This article describes how tooconfigure node-to-node and client-to-node security by using Windows security in hello *ClusterConfig.JSON* file.</span></span>  <span data-ttu-id="85d4c-107">hello process motsvarar toohello konfigurera säkerhetssteg i [skapa ett fristående kluster som körs på Windows](service-fabric-cluster-creation-for-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="85d4c-107">hello process corresponds toohello configure security step of [Create a standalone cluster running on Windows](service-fabric-cluster-creation-for-windows-server.md).</span></span> <span data-ttu-id="85d4c-108">Mer information om hur Service Fabric använder Windows-säkerhet finns [kluster säkerhetsscenarier](service-fabric-cluster-security.md).</span><span class="sxs-lookup"><span data-stu-id="85d4c-108">For more information about how Service Fabric uses Windows security, see [Cluster security scenarios](service-fabric-cluster-security.md).</span></span>

> [!NOTE]
> <span data-ttu-id="85d4c-109">Bör du hello val av nod till nod säkerhet noggrant eftersom det finns inget klusteruppgradering från en säkerhet val tooanother.</span><span class="sxs-lookup"><span data-stu-id="85d4c-109">You should consider hello selection of node-to-node security carefully because there is no cluster upgrade from one security choice tooanother.</span></span> <span data-ttu-id="85d4c-110">toochange hello säkerhet val du har toorebuild hello fullständig kluster.</span><span class="sxs-lookup"><span data-stu-id="85d4c-110">toochange hello security selection, you have toorebuild hello full cluster.</span></span>
>
>

## <a name="configure-windows-security-using-gmsa"></a><span data-ttu-id="85d4c-111">Konfigurera Windows-säkerhet som använder gMSA</span><span class="sxs-lookup"><span data-stu-id="85d4c-111">Configure Windows security using gMSA</span></span>  
<span data-ttu-id="85d4c-112">hello exempel *ClusterConfig.gMSA.Windows.MultiMachine.JSON* konfigurationsfilen hämtas med hello [Microsoft.Azure.ServiceFabric.WindowsServer.<version>. ZIP](http://go.microsoft.com/fwlink/?LinkId=730690) fristående klusterpaket innehåller en mall för att konfigurera Windows-säkerhet med [Grupphanterat tjänstkonto (gMSA)](https://technet.microsoft.com/library/hh831782.aspx):</span><span class="sxs-lookup"><span data-stu-id="85d4c-112">hello sample *ClusterConfig.gMSA.Windows.MultiMachine.JSON* configuration file downloaded with hello [Microsoft.Azure.ServiceFabric.WindowsServer.<version>.zip](http://go.microsoft.com/fwlink/?LinkId=730690) standalone cluster package contains a template for configuring Windows security using [Group Managed Service Account (gMSA)](https://technet.microsoft.com/library/hh831782.aspx):</span></span>  

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
  
| <span data-ttu-id="85d4c-113">**Konfigurationsinställningen**</span><span class="sxs-lookup"><span data-stu-id="85d4c-113">**Configuration Setting**</span></span> | <span data-ttu-id="85d4c-114">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="85d4c-114">**Description**</span></span> |  
| --- | --- |  
| <span data-ttu-id="85d4c-115">WindowsIdentities</span><span class="sxs-lookup"><span data-stu-id="85d4c-115">WindowsIdentities</span></span> |<span data-ttu-id="85d4c-116">Innehåller hello klustret och klienten identiteter.</span><span class="sxs-lookup"><span data-stu-id="85d4c-116">Contains hello cluster and client identities.</span></span> |  
| <span data-ttu-id="85d4c-117">ClustergMSAIdentity</span><span class="sxs-lookup"><span data-stu-id="85d4c-117">ClustergMSAIdentity</span></span> |<span data-ttu-id="85d4c-118">Konfigurerar nod till noden säkerhet.</span><span class="sxs-lookup"><span data-stu-id="85d4c-118">Configures node-to-node security.</span></span> <span data-ttu-id="85d4c-119">En grupp Hanterat tjänstkonto.</span><span class="sxs-lookup"><span data-stu-id="85d4c-119">A group managed service account.</span></span> |  
| <span data-ttu-id="85d4c-120">ClusterSPN</span><span class="sxs-lookup"><span data-stu-id="85d4c-120">ClusterSPN</span></span> |<span data-ttu-id="85d4c-121">Fullständigt kvalificerat domännamn SPN för gMSA-kontot</span><span class="sxs-lookup"><span data-stu-id="85d4c-121">Fully qualified domain SPN for gMSA account</span></span>|  
| <span data-ttu-id="85d4c-122">ClientIdentities</span><span class="sxs-lookup"><span data-stu-id="85d4c-122">ClientIdentities</span></span> |<span data-ttu-id="85d4c-123">Konfigurerar säkerhet för klient-till-nod.</span><span class="sxs-lookup"><span data-stu-id="85d4c-123">Configures client-to-node security.</span></span> <span data-ttu-id="85d4c-124">En matris med användarkonton för klienten.</span><span class="sxs-lookup"><span data-stu-id="85d4c-124">An array of client user accounts.</span></span> |  
| <span data-ttu-id="85d4c-125">Identitet</span><span class="sxs-lookup"><span data-stu-id="85d4c-125">Identity</span></span> |<span data-ttu-id="85d4c-126">hello klientens identitet, en domänanvändare.</span><span class="sxs-lookup"><span data-stu-id="85d4c-126">hello client identity, a domain user.</span></span> |  
| <span data-ttu-id="85d4c-127">IsAdmin</span><span class="sxs-lookup"><span data-stu-id="85d4c-127">IsAdmin</span></span> |<span data-ttu-id="85d4c-128">TRUE anger hello domänanvändaren har administratörsbehörighet för klienten, false för klientåtkomst för användaren.</span><span class="sxs-lookup"><span data-stu-id="85d4c-128">True specifies that hello domain user has administrator client access, false for user client access.</span></span> |  
  
<span data-ttu-id="85d4c-129">[Noden toonode säkerhet](service-fabric-cluster-security.md#node-to-node-security) konfigureras genom att ange **ClustergMSAIdentity** när service fabric måste toorun under gMSA.</span><span class="sxs-lookup"><span data-stu-id="85d4c-129">[Node toonode security](service-fabric-cluster-security.md#node-to-node-security) is configured by setting **ClustergMSAIdentity** when service fabric needs toorun under gMSA.</span></span> <span data-ttu-id="85d4c-130">I ordning toobuild förtroenderelationer mellan noder, måste de göras medveten om varandra.</span><span class="sxs-lookup"><span data-stu-id="85d4c-130">In order toobuild trust relationships between nodes, they must be made aware of each other.</span></span> <span data-ttu-id="85d4c-131">Detta kan åstadkommas på två olika sätt: Ange hello Grupphanterat tjänstkonto som innehåller alla noder i klustret hello eller ange hello datorn domängrupp som innehåller alla noder i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="85d4c-131">This can be accomplished in two different ways: Specify hello Group Managed Service Account that includes all nodes in hello cluster or Specify hello domain machine group that includes all nodes in hello cluster.</span></span> <span data-ttu-id="85d4c-132">Vi rekommenderar starkt med hello [Grupphanterat tjänstkonto (gMSA)](https://technet.microsoft.com/library/hh831782.aspx) metod, särskilt för större kluster (fler än 10 noder) eller för kluster som är sannolikt toogrow eller minska.</span><span class="sxs-lookup"><span data-stu-id="85d4c-132">We strongly recommend using hello [Group Managed Service Account (gMSA)](https://technet.microsoft.com/library/hh831782.aspx) approach, particularly for larger clusters (more than 10 nodes) or for clusters that are likely toogrow or shrink.</span></span>  
<span data-ttu-id="85d4c-133">Den här metoden kräver inte hello skapandet av en domängrupp som klusteradministratörer har beviljats åtkomst rättigheter tooadd och ta bort medlemmar.</span><span class="sxs-lookup"><span data-stu-id="85d4c-133">This approach does not require hello creation of a domain group for which cluster administrators have been granted access rights tooadd and remove members.</span></span> <span data-ttu-id="85d4c-134">Dessa konton är också användbara för automatisk lösenordshantering.</span><span class="sxs-lookup"><span data-stu-id="85d4c-134">These accounts are also useful for automatic password management.</span></span> <span data-ttu-id="85d4c-135">Mer information finns i [komma igång med Grupphanterade tjänstkonton](http://technet.microsoft.com/library/jj128431.aspx).</span><span class="sxs-lookup"><span data-stu-id="85d4c-135">For more information, see [Getting Started with Group Managed Service Accounts](http://technet.microsoft.com/library/jj128431.aspx).</span></span>  
 
<span data-ttu-id="85d4c-136">[Toonode klientsäkerhet](service-fabric-cluster-security.md#client-to-node-security) konfigureras med hjälp av **ClientIdentities**.</span><span class="sxs-lookup"><span data-stu-id="85d4c-136">[Client toonode security](service-fabric-cluster-security.md#client-to-node-security) is configured using **ClientIdentities**.</span></span> <span data-ttu-id="85d4c-137">I ordning tooestablish förtroende mellan klienten och hello-kluster, måste du konfigurera hello klustret tooknow som klienten identiteter som den kan lita på.</span><span class="sxs-lookup"><span data-stu-id="85d4c-137">In order tooestablish trust between a client and hello cluster, you must configure hello cluster tooknow which client identities that it can trust.</span></span> <span data-ttu-id="85d4c-138">Detta kan göras på två olika sätt: Ange hello gruppen Domänanvändare som kan ansluta eller ange hello nod domänanvändare som kan ansluta.</span><span class="sxs-lookup"><span data-stu-id="85d4c-138">This can be done in two different ways: Specify hello domain group users that can connect or specify hello domain node users that can connect.</span></span> <span data-ttu-id="85d4c-139">Service Fabric stöder två typer av olika åtkomstkontroll för klienter som är anslutna tooa Service Fabric-kluster: administratörs- och.</span><span class="sxs-lookup"><span data-stu-id="85d4c-139">Service Fabric supports two different access control types for clients that are connected tooa Service Fabric cluster: administrator and user.</span></span> <span data-ttu-id="85d4c-140">Åtkomstkontroll ger hello möjligheten för hello administratören toolimit åtkomst toocertain klustertyper för klusteråtgärder för olika grupper av användare, göra hello klustret säkrare.</span><span class="sxs-lookup"><span data-stu-id="85d4c-140">Access control provides hello ability for hello cluster administrator toolimit access toocertain types of cluster operations for different groups of users, making hello cluster more secure.</span></span>  <span data-ttu-id="85d4c-141">Administratörer har fullständig åtkomst toomanagement (inklusive funktioner för läsning och skrivning).</span><span class="sxs-lookup"><span data-stu-id="85d4c-141">Administrators have full access toomanagement capabilities (including read/write capabilities).</span></span> <span data-ttu-id="85d4c-142">Användare, har som standard bara läsbehörighet toomanagement funktioner (till exempel frågefunktioner) och hello möjlighet tooresolve program och tjänster.</span><span class="sxs-lookup"><span data-stu-id="85d4c-142">Users, by default, have only read access toomanagement capabilities (for example, query capabilities), and hello ability tooresolve applications and services.</span></span> <span data-ttu-id="85d4c-143">Mer information om åtkomstkontroller finns [rollbaserad åtkomstkontroll för Service Fabric-klienter](service-fabric-cluster-security-roles.md).</span><span class="sxs-lookup"><span data-stu-id="85d4c-143">For more information on access controls, see [Role based access control for Service Fabric clients](service-fabric-cluster-security-roles.md).</span></span>  
 
<span data-ttu-id="85d4c-144">Hej följande exempel **säkerhet** avsnittet konfigurerar Windows-säkerhet som använder gMSA och anger att hello datorer i *ServiceFabric.clusterA.contoso.com* gMSA tillhör hello klustret och att *CONTOSO\usera* har åtkomst till admin-klienten:</span><span class="sxs-lookup"><span data-stu-id="85d4c-144">hello following example **security** section configures Windows security using gMSA and specifies that hello machines in *ServiceFabric.clusterA.contoso.com* gMSA are part of hello cluster and that *CONTOSO\usera* has admin client access:</span></span>  
  
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
  
## <a name="configure-windows-security-using-a-machine-group"></a><span data-ttu-id="85d4c-145">Konfigurera Windows-säkerhet med hjälp av en grupp datorer</span><span class="sxs-lookup"><span data-stu-id="85d4c-145">Configure Windows security using a machine group</span></span>  
<span data-ttu-id="85d4c-146">hello exempel *ClusterConfig.Windows.MultiMachine.JSON* konfigurationsfilen hämtas med hello [Microsoft.Azure.ServiceFabric.WindowsServer.<version>. ZIP](http://go.microsoft.com/fwlink/?LinkId=730690) fristående klusterpaket innehåller en mall för att konfigurera Windows-säkerhet.</span><span class="sxs-lookup"><span data-stu-id="85d4c-146">hello sample *ClusterConfig.Windows.MultiMachine.JSON* configuration file downloaded with hello [Microsoft.Azure.ServiceFabric.WindowsServer.<version>.zip](http://go.microsoft.com/fwlink/?LinkId=730690) standalone cluster package contains a template for configuring Windows security.</span></span>  <span data-ttu-id="85d4c-147">Windows-säkerhet har konfigurerats i hello **egenskaper** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="85d4c-147">Windows security is configured in hello **Properties** section:</span></span> 

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

| <span data-ttu-id="85d4c-148">**Konfigurationsinställningen**</span><span class="sxs-lookup"><span data-stu-id="85d4c-148">**Configuration setting**</span></span> | <span data-ttu-id="85d4c-149">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="85d4c-149">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="85d4c-150">ClusterCredentialType</span><span class="sxs-lookup"><span data-stu-id="85d4c-150">ClusterCredentialType</span></span> |<span data-ttu-id="85d4c-151">**ClusterCredentialType** har angetts för*Windows* om ClusterIdentity anger en Active Directory datorn gruppnamn.</span><span class="sxs-lookup"><span data-stu-id="85d4c-151">**ClusterCredentialType** is set too*Windows* if ClusterIdentity specifies an Active Directory Machine Group Name.</span></span> |  
| <span data-ttu-id="85d4c-152">ServerCredentialType</span><span class="sxs-lookup"><span data-stu-id="85d4c-152">ServerCredentialType</span></span> |<span data-ttu-id="85d4c-153">Ställa in också*Windows* tooenable Windows-säkerhet för klienter.</span><span class="sxs-lookup"><span data-stu-id="85d4c-153">Set too*Windows* tooenable Windows security for clients.</span></span><br /><br /><span data-ttu-id="85d4c-154">Detta anger att hello klienter på hello och hello klustret själva körs inom en Active Directory-domän.</span><span class="sxs-lookup"><span data-stu-id="85d4c-154">This indicates that hello clients of hello cluster and hello cluster itself are running within an Active Directory domain.</span></span> |  
| <span data-ttu-id="85d4c-155">WindowsIdentities</span><span class="sxs-lookup"><span data-stu-id="85d4c-155">WindowsIdentities</span></span> |<span data-ttu-id="85d4c-156">Innehåller hello klustret och klienten identiteter.</span><span class="sxs-lookup"><span data-stu-id="85d4c-156">Contains hello cluster and client identities.</span></span> |  
| <span data-ttu-id="85d4c-157">ClusterIdentity</span><span class="sxs-lookup"><span data-stu-id="85d4c-157">ClusterIdentity</span></span> |<span data-ttu-id="85d4c-158">Använd en dator gruppnamn, domain\machinegroup, tooconfigure nod till noden säkerhet.</span><span class="sxs-lookup"><span data-stu-id="85d4c-158">Use a machine group name, domain\machinegroup, tooconfigure node-to-node security.</span></span> |  
| <span data-ttu-id="85d4c-159">ClientIdentities</span><span class="sxs-lookup"><span data-stu-id="85d4c-159">ClientIdentities</span></span> |<span data-ttu-id="85d4c-160">Konfigurerar säkerhet för klient-till-nod.</span><span class="sxs-lookup"><span data-stu-id="85d4c-160">Configures client-to-node security.</span></span> <span data-ttu-id="85d4c-161">En matris med användarkonton för klienten.</span><span class="sxs-lookup"><span data-stu-id="85d4c-161">An array of client user accounts.</span></span> |  
| <span data-ttu-id="85d4c-162">Identitet</span><span class="sxs-lookup"><span data-stu-id="85d4c-162">Identity</span></span> |<span data-ttu-id="85d4c-163">Lägg till hello domänanvändare domän\användarnamn för hello klientens identitet.</span><span class="sxs-lookup"><span data-stu-id="85d4c-163">Add hello domain user, domain\username, for hello client identity.</span></span> |  
| <span data-ttu-id="85d4c-164">IsAdmin</span><span class="sxs-lookup"><span data-stu-id="85d4c-164">IsAdmin</span></span> |<span data-ttu-id="85d4c-165">Ange tootrue toospecify som hello domänanvändare har klienten administratörsåtkomst eller false för klientåtkomst för användaren.</span><span class="sxs-lookup"><span data-stu-id="85d4c-165">Set tootrue toospecify that hello domain user has administrator client access or false for user client access.</span></span> |  

<span data-ttu-id="85d4c-166">[Noden toonode säkerhet](service-fabric-cluster-security.md#node-to-node-security) konfigureras med hjälp av inställningen **ClusterIdentity** om du vill toouse en grupp datorer inom en Active Directory-domän.</span><span class="sxs-lookup"><span data-stu-id="85d4c-166">[Node toonode security](service-fabric-cluster-security.md#node-to-node-security) is configured by setting using **ClusterIdentity** if you want toouse a machine group within an Active Directory Domain.</span></span> <span data-ttu-id="85d4c-167">Mer information finns i [skapar en datorgruppen i Active Directory](https://msdn.microsoft.com/library/aa545347(v=cs.70).aspx).</span><span class="sxs-lookup"><span data-stu-id="85d4c-167">For more information, see [Create a Machine Group in Active Directory](https://msdn.microsoft.com/library/aa545347(v=cs.70).aspx).</span></span>

<span data-ttu-id="85d4c-168">[Klient-till-nod säkerhet](service-fabric-cluster-security.md#client-to-node-security) konfigureras med hjälp av **ClientIdentities**.</span><span class="sxs-lookup"><span data-stu-id="85d4c-168">[Client-to-node security](service-fabric-cluster-security.md#client-to-node-security) is configured by using **ClientIdentities**.</span></span> <span data-ttu-id="85d4c-169">tooestablish förtroende mellan klienten och hello kluster, måste du konfigurera hello klustret tooknow hello klient kan lita på identiteter som hello klustret.</span><span class="sxs-lookup"><span data-stu-id="85d4c-169">tooestablish trust between a client and hello cluster, you must configure hello cluster tooknow hello client identities that hello cluster can trust.</span></span> <span data-ttu-id="85d4c-170">Du kan upprätta förtroende på två olika sätt:</span><span class="sxs-lookup"><span data-stu-id="85d4c-170">You can establish trust in two different ways:</span></span>

- <span data-ttu-id="85d4c-171">Ange hello gruppen Domänanvändare som kan ansluta.</span><span class="sxs-lookup"><span data-stu-id="85d4c-171">Specify hello domain group users that can connect.</span></span>
- <span data-ttu-id="85d4c-172">Ange hello nod domänanvändare som kan ansluta.</span><span class="sxs-lookup"><span data-stu-id="85d4c-172">Specify hello domain node users that can connect.</span></span>

<span data-ttu-id="85d4c-173">Service Fabric stöder två typer av olika åtkomstkontroll för klienter som är anslutna tooa Service Fabric-kluster: administratörs- och.</span><span class="sxs-lookup"><span data-stu-id="85d4c-173">Service Fabric supports two different access control types for clients that are connected tooa Service Fabric cluster: administrator and user.</span></span> <span data-ttu-id="85d4c-174">Åtkomstkontroll kan Hej administratör toolimit åtkomst toocertain klustertyper för klusteråtgärder för olika grupper av användare, vilket gör att hello klustret är säkrare.</span><span class="sxs-lookup"><span data-stu-id="85d4c-174">Access control enables hello cluster administrator toolimit access toocertain types of cluster operations for different groups of users, which makes hello cluster more secure.</span></span>  <span data-ttu-id="85d4c-175">Administratörer har fullständig åtkomst toomanagement (inklusive funktioner för läsning och skrivning).</span><span class="sxs-lookup"><span data-stu-id="85d4c-175">Administrators have full access toomanagement capabilities (including read/write capabilities).</span></span> <span data-ttu-id="85d4c-176">Användare, har som standard bara läsbehörighet toomanagement funktioner (till exempel frågefunktioner) och hello möjlighet tooresolve program och tjänster.</span><span class="sxs-lookup"><span data-stu-id="85d4c-176">Users, by default, have only read access toomanagement capabilities (for example, query capabilities), and hello ability tooresolve applications and services.</span></span>  

<span data-ttu-id="85d4c-177">Hej följande exempel **säkerhet** avsnittet konfigurerar Windows-säkerhet, anger den hello datorer i *ServiceFabric/clusterA.contoso.com* tillhör hello kluster och anger att  *CONTOSO\usera* har åtkomst till admin-klienten:</span><span class="sxs-lookup"><span data-stu-id="85d4c-177">hello following example **security** section configures Windows security, specifies that hello machines in *ServiceFabric/clusterA.contoso.com* are part of hello cluster, and specifies that *CONTOSO\usera* has admin client access:</span></span>

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
> <span data-ttu-id="85d4c-178">Service Fabric bör inte distribueras på en domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="85d4c-178">Service Fabric should not be deployed on a domain controller.</span></span> <span data-ttu-id="85d4c-179">Kontrollera att ClusterConfig.json inte omfatta hello IP-adress för hello domänkontrollant när du använder en dator grupp eller grupphanterat tjänstkonto (gMSA).</span><span class="sxs-lookup"><span data-stu-id="85d4c-179">Make sure that ClusterConfig.json does not include hello IP address of hello domain controller when using a machine group or group Managed Service Account (gMSA).</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="85d4c-180">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="85d4c-180">Next steps</span></span>
<span data-ttu-id="85d4c-181">När du har konfigurerat Windows-säkerhet i hello *ClusterConfig.JSON* fil, återuppta hello skapar kluster i [skapa ett fristående kluster som körs på Windows](service-fabric-cluster-creation-for-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="85d4c-181">After configuring Windows security in hello *ClusterConfig.JSON* file, resume hello cluster creation process in [Create a standalone cluster running on Windows](service-fabric-cluster-creation-for-windows-server.md).</span></span>

<span data-ttu-id="85d4c-182">Mer information om hur nod till noden säkerhet, klient-till-nod säkerhet och rollbaserad åtkomstkontroll finns [kluster säkerhetsscenarier](service-fabric-cluster-security.md).</span><span class="sxs-lookup"><span data-stu-id="85d4c-182">For more information about how node-to-node security, client-to-node security, and role-based access control, see [Cluster security scenarios](service-fabric-cluster-security.md).</span></span>

<span data-ttu-id="85d4c-183">Se [Anslut tooa säker klustret](service-fabric-connect-to-secure-cluster.md) exempel på ansluter med hjälp av PowerShell eller FabricClient.</span><span class="sxs-lookup"><span data-stu-id="85d4c-183">See [Connect tooa secure cluster](service-fabric-connect-to-secure-cluster.md) for examples of connecting by using PowerShell or FabricClient.</span></span>
