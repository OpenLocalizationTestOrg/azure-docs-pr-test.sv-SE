---
title: aaaCreate Service Fabric-klustret i hello Azure-portalen | Microsoft Docs
description: "Den här artikeln beskriver hur tooset upp en säker Service Fabric-kluster i Azure med hjälp av hello Azure-portalen och Azure Key Vault."
services: service-fabric
documentationcenter: .net
author: chackdan
manager: timlt
editor: vturecek
ms.assetid: 426c3d13-127a-49eb-a54c-6bde7c87a83b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/21/2017
ms.author: chackdan
ms.openlocfilehash: 045f71b491260e741ce7a54a75c440e1b33059a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-service-fabric-cluster-in-azure-using-hello-azure-portal"></a><span data-ttu-id="6c9b5-103">Skapa ett Service Fabric-kluster i Azure med hjälp av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="6c9b5-103">Create a Service Fabric cluster in Azure using hello Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6c9b5-104">Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="6c9b5-104">Azure Resource Manager</span></span>](service-fabric-cluster-creation-via-arm.md)
> * [<span data-ttu-id="6c9b5-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6c9b5-105">Azure portal</span></span>](service-fabric-cluster-creation-via-portal.md)
> 
> 

<span data-ttu-id="6c9b5-106">Det här är en stegvis guide som vägleder dig genom hello stegen för att skapa en säker Service Fabric-kluster i Azure med hjälp av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-106">This is a step-by-step guide that walks you through hello steps of setting up a secure Service Fabric cluster in Azure using hello Azure portal.</span></span> <span data-ttu-id="6c9b5-107">Den här guiden vägleder dig genom hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="6c9b5-107">This guide walks you through hello following steps:</span></span>

* <span data-ttu-id="6c9b5-108">Ställ in Key Vault toomanage nycklar för säkerhet för klustret.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-108">Set up Key Vault toomanage keys for cluster security.</span></span>
* <span data-ttu-id="6c9b5-109">Skapa ett skyddat kluster i Azure via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-109">Create a secured cluster in Azure through hello Azure portal.</span></span>
* <span data-ttu-id="6c9b5-110">Autentisera administratörer som använder certifikat.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-110">Authenticate administrators using certificates.</span></span>

> [!NOTE]
> <span data-ttu-id="6c9b5-111">För mer avancerade säkerhetsalternativ, till exempel autentisering med Azure Active Directory och hur du konfigurerar certifikat för programmet säkerhet [skapar ett kluster med Azure Resource Manager][create-cluster-arm].</span><span class="sxs-lookup"><span data-stu-id="6c9b5-111">For more advanced security options, such as user authentication with Azure Active Directory and setting up certificates for application security, [create your cluster using Azure Resource Manager][create-cluster-arm].</span></span>
> 
> 

<span data-ttu-id="6c9b5-112">En säker klustret är ett kluster som förhindrar obehörig åtkomst toomanagement åtgärder, vilket innefattar distribution, uppgradera och ta bort program, tjänster och hello data de innehåller.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-112">A secure cluster is a cluster that prevents unauthorized access toomanagement operations, which includes deploying, upgrading, and deleting applications, services, and hello data they contain.</span></span> <span data-ttu-id="6c9b5-113">Ett oskyddat klustret är ett kluster alla kan ansluta tooat helst och utföra hanteringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-113">An unsecure cluster is a cluster that anyone can connect tooat any time and perform management operations.</span></span> <span data-ttu-id="6c9b5-114">Men det är möjligt toocreate ett oskyddade kluster, det är **rekommenderar toocreate säker klustret**.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-114">Although it is possible toocreate an unsecure cluster, it is **highly recommended toocreate a secure cluster**.</span></span> <span data-ttu-id="6c9b5-115">Ett oskyddade kluster **går inte att senare säkra** -måste du skapa ett nytt kluster.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-115">An unsecure cluster **cannot be secured later** - a new cluster must be created.</span></span>

<span data-ttu-id="6c9b5-116">hello begrepp är hello samma för att skapa skyddade kluster, oavsett om hello kluster är Linux-kluster eller Windows-kluster.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-116">hello concepts are hello same for creating secure clusters, whether hello clusters are Linux clusters or Windows clusters.</span></span> <span data-ttu-id="6c9b5-117">Mer information och hjälp skript för att skapa säkra Linux-kluster, se [att skapa skyddade kluster på Linux](service-fabric-cluster-creation-via-arm.md#secure-linux-clusters).</span><span class="sxs-lookup"><span data-stu-id="6c9b5-117">For more information and helper scripts for creating secure Linux clusters, please see [Creating secure clusters on Linux](service-fabric-cluster-creation-via-arm.md#secure-linux-clusters).</span></span> <span data-ttu-id="6c9b5-118">hello parametrar som erhålls genom hello helper skriptet kan vara in direkt hello portal enligt beskrivningen i avsnittet hello [skapar ett kluster i hello Azure-portalen](#create-cluster-portal).</span><span class="sxs-lookup"><span data-stu-id="6c9b5-118">hello parameters obtained by hello helper script provided can be input directly into hello portal as described in hello section [Create a cluster in hello Azure portal](#create-cluster-portal).</span></span>

## <a name="log-in-tooazure"></a><span data-ttu-id="6c9b5-119">Logga in tooAzure</span><span class="sxs-lookup"><span data-stu-id="6c9b5-119">Log in tooAzure</span></span>
<span data-ttu-id="6c9b5-120">Den här guiden använder [Azure PowerShell][azure-powershell].</span><span class="sxs-lookup"><span data-stu-id="6c9b5-120">This guide uses [Azure PowerShell][azure-powershell].</span></span> <span data-ttu-id="6c9b5-121">När du startar en ny PowerShell-session, logga in tooyour Azure-konto och välja din prenumeration innan du kör kommandon för Azure.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-121">When starting a new PowerShell session, log in tooyour Azure account and select your subscription before executing Azure commands.</span></span>

<span data-ttu-id="6c9b5-122">Logga in tooyour azure-konto:</span><span class="sxs-lookup"><span data-stu-id="6c9b5-122">Log in tooyour azure account:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="6c9b5-123">Välj din prenumeration:</span><span class="sxs-lookup"><span data-stu-id="6c9b5-123">Select your subscription:</span></span>

```powershell
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

## <a name="set-up-key-vault"></a><span data-ttu-id="6c9b5-124">Konfigurera Key Vault</span><span class="sxs-lookup"><span data-stu-id="6c9b5-124">Set up Key Vault</span></span>
<span data-ttu-id="6c9b5-125">Den här delen av hello guiden vägleder dig genom att skapa ett Nyckelvalv för Service Fabric-kluster i Azure och för Service Fabric-program.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-125">This part of hello guide walks you through creating a Key Vault for a Service Fabric cluster in Azure and for Service Fabric applications.</span></span> <span data-ttu-id="6c9b5-126">En fullständig guide på Key Vault finns hello [Key Vault Kom igång med][key-vault-get-started].</span><span class="sxs-lookup"><span data-stu-id="6c9b5-126">For a complete guide on Key Vault, see hello [Key Vault getting started guide][key-vault-get-started].</span></span>

<span data-ttu-id="6c9b5-127">Service Fabric använder X.509-certifikat toosecure ett kluster.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-127">Service Fabric uses X.509 certificates toosecure a cluster.</span></span> <span data-ttu-id="6c9b5-128">Azure Key Vault är används toomanage certifikat för Service Fabric-kluster i Azure.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-128">Azure Key Vault is used toomanage certificates for Service Fabric clusters in Azure.</span></span> <span data-ttu-id="6c9b5-129">När ett kluster distribueras i Azure hämtar certifikat från Nyckelvalvet hello Azure-resursprovider ansvarar för att skapa Service Fabric-kluster och installerar dem på hello klustrets virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-129">When a cluster is deployed in Azure, hello Azure resource provider responsible for creating Service Fabric clusters pulls certificates from Key Vault and installs them on hello cluster VMs.</span></span>

<span data-ttu-id="6c9b5-130">hello illustrerar följande diagram hello förhållandet mellan Nyckelvalvet, Service Fabric-kluster och hello Azure-resursprovider som använder certifikat som lagras i Nyckelvalvet när den skapar ett kluster:</span><span class="sxs-lookup"><span data-stu-id="6c9b5-130">hello following diagram illustrates hello relationship between Key Vault, a Service Fabric cluster, and hello Azure resource provider that uses certificates stored in Key Vault when it creates a cluster:</span></span>

![Certifikatinstallation][cluster-security-cert-installation]

### <a name="create-a-resource-group"></a><span data-ttu-id="6c9b5-132">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="6c9b5-132">Create a Resource Group</span></span>
<span data-ttu-id="6c9b5-133">hello första steget är toocreate en ny resursgrupp specifikt för Nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-133">hello first step is toocreate a new resource group specifically for Key Vault.</span></span> <span data-ttu-id="6c9b5-134">Placera Nyckelvalvet i sin egen resursgruppen rekommenderas så att du kan ta bort beräkning och lagring resursgrupper – exempelvis hello resursgrupp som har Service Fabric-kluster – utan att förlora dina nycklar och hemligheter.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-134">Putting Key Vault into its own resource group is recommended so that you can remove compute and storage resource groups - such as hello resource group that has your Service Fabric cluster - without losing your keys and secrets.</span></span> <span data-ttu-id="6c9b5-135">hello resursgruppen som innehåller ditt Nyckelvalv måste vara i hello samma region som hello-kluster som använder den.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-135">hello resource group that has your Key Vault must be in hello same region as hello cluster that is using it.</span></span>

```powershell

    PS C:\Users\vturecek> New-AzureRmResourceGroup -Name mycluster-keyvault -Location 'West US'
    WARNING: hello output object type of this cmdlet will be modified in a future release.

    ResourceGroupName : mycluster-keyvault
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/<guid>/resourceGroups/mycluster-keyvault

```

### <a name="create-key-vault"></a><span data-ttu-id="6c9b5-136">Skapa Nyckelvalvet</span><span class="sxs-lookup"><span data-stu-id="6c9b5-136">Create Key Vault</span></span>
<span data-ttu-id="6c9b5-137">Skapa ett Nyckelvalv i hello ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-137">Create a Key Vault in hello new resource group.</span></span> <span data-ttu-id="6c9b5-138">hello Key Vault **måste vara aktiverat för distribution** tooallow hello Service Fabric resource provider tooget certifikat från den och installera på klusternoder:</span><span class="sxs-lookup"><span data-stu-id="6c9b5-138">hello Key Vault **must be enabled for deployment** tooallow hello Service Fabric resource provider tooget certificates from it and install on cluster nodes:</span></span>

```powershell

    PS C:\Users\vturecek> New-AzureRmKeyVault -VaultName 'myvault' -ResourceGroupName 'mycluster-keyvault' -Location 'West US' -EnabledForDeployment


    Vault Name                       : myvault
    Resource Group Name              : mycluster-keyvault
    Location                         : West US
    Resource ID                      : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault
    Vault URI                        : https://myvault.vault.azure.net
    Tenant ID                        : <guid>
    SKU                              : Standard
    Enabled For Deployment?          : False
    Enabled For Template Deployment? : False
    Enabled For Disk Encryption?     : False
    Access Policies                  :
                                       Tenant ID                :    <guid>
                                       Object ID                :    <guid>
                                       Application ID           :
                                       Display Name             :    
                                       Permissions tooKeys      :    get, create, delete, list, update, import, backup, restore
                                       Permissions tooSecrets   :    all


    Tags                             :
```

<span data-ttu-id="6c9b5-139">Om du har en befintlig Key Vault kan aktivera du den för distribution med Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="6c9b5-139">If you have an existing Key Vault, you can enable it for deployment using Azure CLI:</span></span>

```cli
> azure login
> azure account set "your account"
> azure config mode arm 
> azure keyvault list
> azure keyvault set-policy --vault-name "your vault name" --enabled-for-deployment true
```


## <a name="add-certificates-tookey-vault"></a><span data-ttu-id="6c9b5-140">Lägg till certifikat tooKey valvet</span><span class="sxs-lookup"><span data-stu-id="6c9b5-140">Add certificates tooKey Vault</span></span>
<span data-ttu-id="6c9b5-141">Certifikat används i Service Fabric tooprovide autentisering och kryptering toosecure olika aspekter av ett kluster och dess program.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-141">Certificates are used in Service Fabric tooprovide authentication and encryption toosecure various aspects of a cluster and its applications.</span></span> <span data-ttu-id="6c9b5-142">Mer information om hur du använder certifikat i Service Fabric finns [säkerhetsscenarier för Service Fabric-klustret][service-fabric-cluster-security].</span><span class="sxs-lookup"><span data-stu-id="6c9b5-142">For more information on how certificates are used in Service Fabric, see [Service Fabric cluster security scenarios][service-fabric-cluster-security].</span></span>

### <a name="cluster-and-server-certificate-required"></a><span data-ttu-id="6c9b5-143">Kluster- och certifikat (krävs)</span><span class="sxs-lookup"><span data-stu-id="6c9b5-143">Cluster and server certificate (required)</span></span>
<span data-ttu-id="6c9b5-144">Det här certifikatet är obligatoriska toosecure ett kluster och förhindra obehörig åtkomst tooit.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-144">This certificate is required toosecure a cluster and prevent unauthorized access tooit.</span></span> <span data-ttu-id="6c9b5-145">Det ger säkerhet i klustret på ett par olika sätt:</span><span class="sxs-lookup"><span data-stu-id="6c9b5-145">It provides cluster security in a couple ways:</span></span>

* <span data-ttu-id="6c9b5-146">**Autentisering för klustret:** autentiserar nod till nod kommunikation för klustret.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-146">**Cluster authentication:** Authenticates node-to-node communication for cluster federation.</span></span> <span data-ttu-id="6c9b5-147">Endast noder som kan bevisa sin identitet med det här certifikatet kan gå hello klustret.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-147">Only nodes that can prove their identity with this certificate can join hello cluster.</span></span>
* <span data-ttu-id="6c9b5-148">**Serverautentisering:** autentiserar hello cluster management slutpunkter tooa management-klienten, så att hello Hanteringsklient vet pratar toohello verkliga klustret.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-148">**Server authentication:** Authenticates hello cluster management endpoints tooa management client, so that hello management client knows it is talking toohello real cluster.</span></span> <span data-ttu-id="6c9b5-149">Det här certifikatet ger också SSL för hello HTTPS hanterings-API och för Service Fabric Explorer via HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-149">This certificate also provides SSL for hello HTTPS management API and for Service Fabric Explorer over HTTPS.</span></span>

<span data-ttu-id="6c9b5-150">tooserve dessa ändamål hello certifikat måste uppfylla följande krav hello:</span><span class="sxs-lookup"><span data-stu-id="6c9b5-150">tooserve these purposes, hello certificate must meet hello following requirements:</span></span>

* <span data-ttu-id="6c9b5-151">hello certifikatet måste innehålla en privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-151">hello certificate must contain a private key.</span></span>
* <span data-ttu-id="6c9b5-152">hello certifikat måste skapas för nyckelutbyte, exportera tooa Personal Information Exchange (.pfx)-fil.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-152">hello certificate must be created for key exchange, exportable tooa Personal Information Exchange (.pfx) file.</span></span>
* <span data-ttu-id="6c9b5-153">hello måste certifikatets ämnesnamn matcha hello domän används tooaccess hello Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-153">hello certificate's subject name must match hello domain used tooaccess hello Service Fabric cluster.</span></span> <span data-ttu-id="6c9b5-154">Detta är obligatorisk tooprovide SSL för hello klustrets HTTPS management slutpunkter och Service Fabric Explorer.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-154">This is required tooprovide SSL for hello cluster's HTTPS management endpoints and Service Fabric Explorer.</span></span> <span data-ttu-id="6c9b5-155">Du kan skaffa ett SSL-certifikat från en certifikatutfärdare (CA) för hello `.cloudapp.azure.com` domän.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-155">You cannot obtain an SSL certificate from a certificate authority (CA) for hello `.cloudapp.azure.com` domain.</span></span> <span data-ttu-id="6c9b5-156">Hämta ett anpassat domännamn för klustret.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-156">Acquire a custom domain name for your cluster.</span></span> <span data-ttu-id="6c9b5-157">När du begär måste ett certifikat från en Certifikatutfärdare hello certifikatets ämnesnamn matcha hello domännamn som används för klustret.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-157">When you request a certificate from a CA hello certificate's subject name must match hello custom domain name used for your cluster.</span></span>

### <a name="client-authentication-certificates"></a><span data-ttu-id="6c9b5-158">Certifikat för klientautentisering</span><span class="sxs-lookup"><span data-stu-id="6c9b5-158">Client authentication certificates</span></span>
<span data-ttu-id="6c9b5-159">Ytterligare klientcertifikat autentisera administratörer för hanteringsuppgifter för klustret.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-159">Additional client certificates authenticate administrators for cluster management tasks.</span></span> <span data-ttu-id="6c9b5-160">Service Fabric har två åtkomstnivåer: **admin** och **skrivskyddade användaren**.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-160">Service Fabric has two access levels: **admin** and **read-only user**.</span></span> <span data-ttu-id="6c9b5-161">Minst ska ett enda certifikat för administrativ åtkomst användas.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-161">At minimum, a single certificate for administrative access should be used.</span></span> <span data-ttu-id="6c9b5-162">För ytterligare användarrättigheter, måste ett separat certifikat tillhandahållas.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-162">For additional user-level access, a separate certificate must be provided.</span></span> <span data-ttu-id="6c9b5-163">Mer information om åtkomst roller finns [rollbaserad åtkomstkontroll för Service Fabric-klienter][service-fabric-cluster-security-roles].</span><span class="sxs-lookup"><span data-stu-id="6c9b5-163">For more information on access roles, see [role-based access control for Service Fabric clients][service-fabric-cluster-security-roles].</span></span>

<span data-ttu-id="6c9b5-164">Du behöver inte tooupload klienten autentisering certifikat tooKey valvet toowork med Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-164">You do not need tooupload Client authentication certificates tooKey Vault toowork with Service Fabric.</span></span> <span data-ttu-id="6c9b5-165">Dessa certifikat behöver bara toobe angetts toousers som har behörighet för hantering av kluster.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-165">These certificates only need toobe provided toousers who are authorized for cluster management.</span></span> 

> [!NOTE]
> <span data-ttu-id="6c9b5-166">Azure Active Directory är hello rekommenderade sätt tooauthenticate klienter för klustret hanteringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-166">Azure Active Directory is hello recommended way tooauthenticate clients for cluster management operations.</span></span> <span data-ttu-id="6c9b5-167">toouse Azure Active Directory, måste du [skapa ett kluster med Azure Resource Manager][create-cluster-arm].</span><span class="sxs-lookup"><span data-stu-id="6c9b5-167">toouse Azure Active Directory, you must [create a cluster using Azure Resource Manager][create-cluster-arm].</span></span>
> 
> 

### <a name="application-certificates-optional"></a><span data-ttu-id="6c9b5-168">Certifikat för programmet (valfritt)</span><span class="sxs-lookup"><span data-stu-id="6c9b5-168">Application certificates (optional)</span></span>
<span data-ttu-id="6c9b5-169">Valfritt antal ytterligare certifikat kan installeras på ett kluster av säkerhetsskäl för programmet.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-169">Any number of additional certificates can be installed on a cluster for application security purposes.</span></span> <span data-ttu-id="6c9b5-170">Tänk hello applikationssäkerhets-scenarier som kräver ett certifikat toobe som har installerats på hello noder innan du skapar klustret:</span><span class="sxs-lookup"><span data-stu-id="6c9b5-170">Before creating your cluster, consider hello application security scenarios that require a certificate toobe installed on hello nodes, such as:</span></span>

* <span data-ttu-id="6c9b5-171">Kryptering och dekryptering av konfigurationsvärden för programmet</span><span class="sxs-lookup"><span data-stu-id="6c9b5-171">Encryption and decryption of application configuration values</span></span>
* <span data-ttu-id="6c9b5-172">Kryptering av data mellan noderna vid replikering</span><span class="sxs-lookup"><span data-stu-id="6c9b5-172">Encryption of data across nodes during replication</span></span> 

<span data-ttu-id="6c9b5-173">Certifikat för programmet kan inte konfigureras när du skapar ett kluster via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-173">Application certificates cannot be configured when creating a cluster through hello Azure portal.</span></span> <span data-ttu-id="6c9b5-174">tooconfigure programcertifikat vid klustret konfigurationen, måste du [skapa ett kluster med Azure Resource Manager][create-cluster-arm].</span><span class="sxs-lookup"><span data-stu-id="6c9b5-174">tooconfigure application certificates at cluster setup time, you must [create a cluster using Azure Resource Manager][create-cluster-arm].</span></span> <span data-ttu-id="6c9b5-175">Du kan också lägga till programmet certifikat toohello klustret när den har skapats.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-175">You can also add application certificates toohello cluster after it has been created.</span></span>

### <a name="formatting-certificates-for-azure-resource-provider-use"></a><span data-ttu-id="6c9b5-176">Formatering certifikat för användning av Azure resource provider</span><span class="sxs-lookup"><span data-stu-id="6c9b5-176">Formatting certificates for Azure resource provider use</span></span>
<span data-ttu-id="6c9b5-177">Privata nyckelfiler (.pfx) kan läggas till och användas direkt via Nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-177">Private key files (.pfx) can be added and used directly through Key Vault.</span></span> <span data-ttu-id="6c9b5-178">Hello Azure-resursprovider kräver dock nycklar toobe lagras i en särskild JSON-format som innehåller hello .pfx som en Base64-kodad sträng och hello lösenordet privata nyckeln.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-178">However, hello Azure resource provider requires keys toobe stored in a special JSON format that includes hello .pfx as a base-64 encoded string and hello private key password.</span></span> <span data-ttu-id="6c9b5-179">tooaccommodate kraven nycklar måste placeras i en JSON-sträng och sedan lagras som *hemligheter* i Nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-179">tooaccommodate these requirements, keys must be placed in a JSON string and then stored as *secrets* in Key Vault.</span></span>

<span data-ttu-id="6c9b5-180">toomake detta bearbeta lättare, en PowerShell-modul är [finns på GitHub][service-fabric-rp-helpers].</span><span class="sxs-lookup"><span data-stu-id="6c9b5-180">toomake this process easier, a PowerShell module is [available on GitHub][service-fabric-rp-helpers].</span></span> <span data-ttu-id="6c9b5-181">Följ dessa steg toouse hello-modulen:</span><span class="sxs-lookup"><span data-stu-id="6c9b5-181">Follow these steps toouse hello module:</span></span>

1. <span data-ttu-id="6c9b5-182">Hämta hello hela innehållet i hello lagringsplatsen till en lokal katalog.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-182">Download hello entire contents of hello repo into a local directory.</span></span> 
2. <span data-ttu-id="6c9b5-183">Importera hello-modul i PowerShell-fönstret:</span><span class="sxs-lookup"><span data-stu-id="6c9b5-183">Import hello module in your PowerShell window:</span></span>

```powershell
  PS C:\Users\vturecek> Import-Module "C:\users\vturecek\Documents\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"
```

<span data-ttu-id="6c9b5-184">Hej `Invoke-AddCertToKeyVault` kommandot i PowerShell-modulen automatiskt formaterar en certifikatets privata nyckel till en JSON-sträng och överförs tooKey valvet.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-184">hello `Invoke-AddCertToKeyVault` command in this PowerShell module automatically formats a certificate private key into a JSON string and uploads it tooKey Vault.</span></span> <span data-ttu-id="6c9b5-185">Använda den tooadd hello klustret certifikat och några ytterligare certifikat tooKey valvet.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-185">Use it tooadd hello cluster certificate and any additional application certificates tooKey Vault.</span></span> <span data-ttu-id="6c9b5-186">Upprepa det här steget för några ytterligare certifikat som du vill använda tooinstall i klustret.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-186">Repeat this step for any additional certificates you want tooinstall in your cluster.</span></span>

```powershell
PS C:\Users\vturecek> Invoke-AddCertToKeyVault -SubscriptionId <guid> -ResourceGroupName mycluster-keyvault -Location "West US" -VaultName myvault -CertificateName mycert -Password "<password>" -UseExistingCertificate -ExistingPfxFilePath "C:\path\to\mycertkey.pfx"

    Switching context tooSubscriptionId <guid>
    Ensuring ResourceGroup mycluster-keyvault in West US
    WARNING: hello output object type of this cmdlet will be modified in a future release.
    Using existing valut myvault in West US
    Reading pfx file from C:\path\to\key.pfx
    Writing secret toomyvault in vault myvault


Name  : CertificateThumbprint
Value : <value>

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault

Name  : CertificateURL
Value : https://myvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47

```

<span data-ttu-id="6c9b5-187">Dessa är alla hello Key Vault-krav för att konfigurera ett Service Fabric-kluster Resource Manager-mall som installerar certifikat för noden autentisering, hantering av slutpunktssäkerhet och autentisering och eventuella ytterligare programsäkerhet funktioner som använder X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-187">These are all hello Key Vault prerequisites for configuring a Service Fabric cluster Resource Manager template that installs certificates for node authentication, management endpoint security and authentication, and any additional application security features that use X.509 certificates.</span></span> <span data-ttu-id="6c9b5-188">Nu är bör du nu ha hello efter installationen i Azure:</span><span class="sxs-lookup"><span data-stu-id="6c9b5-188">At this point, you should now have hello following setup in Azure:</span></span>

* <span data-ttu-id="6c9b5-189">Key Vault resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-189">Key Vault resource group</span></span>
  * <span data-ttu-id="6c9b5-190">Key Vault</span><span class="sxs-lookup"><span data-stu-id="6c9b5-190">Key Vault</span></span>
    * <span data-ttu-id="6c9b5-191">Klustret certifikat för serverautentisering</span><span class="sxs-lookup"><span data-stu-id="6c9b5-191">Cluster server authentication certificate</span></span>

<span data-ttu-id="6c9b5-192">< /a ”Skapa kluster-portal” ></a></span><span class="sxs-lookup"><span data-stu-id="6c9b5-192"></a "create-cluster-portal" ></a></span></span>

## <a name="create-cluster-in-hello-azure-portal"></a><span data-ttu-id="6c9b5-193">Skapa kluster i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="6c9b5-193">Create cluster in hello Azure portal</span></span>
### <a name="search-for-hello-service-fabric-cluster-resource"></a><span data-ttu-id="6c9b5-194">Sök efter hello klusterresurs för Service Fabric</span><span class="sxs-lookup"><span data-stu-id="6c9b5-194">Search for hello Service Fabric cluster resource</span></span>
![Sök efter mallen för Service Fabric-kluster på hello Azure-portalen.][SearchforServiceFabricClusterTemplate]

1. <span data-ttu-id="6c9b5-196">Logga in toohello [Azure-portalen][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="6c9b5-196">Sign in toohello [Azure portal][azure-portal].</span></span>
2. <span data-ttu-id="6c9b5-197">Klicka på **ny** tooadd en ny resursmall för.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-197">Click **New** tooadd a new resource template.</span></span> <span data-ttu-id="6c9b5-198">Sök efter hello Service Fabric-kluster mallen i hello **Marketplace** under **allt**.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-198">Search for hello Service Fabric Cluster template in hello **Marketplace** under **Everything**.</span></span>
3. <span data-ttu-id="6c9b5-199">Välj **Service Fabric-kluster** hello-listan.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-199">Select **Service Fabric Cluster** from hello list.</span></span>
4. <span data-ttu-id="6c9b5-200">Navigera toohello **Service Fabric-kluster** bladet, klickar du på **skapa**,</span><span class="sxs-lookup"><span data-stu-id="6c9b5-200">Navigate toohello **Service Fabric Cluster** blade, click **Create**,</span></span>
5. <span data-ttu-id="6c9b5-201">Hej **skapar Service Fabric-kluster** bladet har hello följande fyra steg.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-201">hello **Create Service Fabric cluster** blade has hello following four steps.</span></span>

#### <a name="1-basics"></a><span data-ttu-id="6c9b5-202">1. Grundläggande inställningar</span><span class="sxs-lookup"><span data-stu-id="6c9b5-202">1. Basics</span></span>
![Skärmbild som visar hur du skapar en ny resursgrupp.][CreateRG]

<span data-ttu-id="6c9b5-204">I bladet för grundläggande hello måste tooprovide hello grundläggande information för klustret.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-204">In hello Basics blade you need tooprovide hello basic details for your cluster.</span></span>

1. <span data-ttu-id="6c9b5-205">Ange hello namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-205">Enter hello name of your cluster.</span></span>
2. <span data-ttu-id="6c9b5-206">Ange en **användarnamn** och **lösenord** Fjärrskrivbord för hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-206">Enter a **user name** and **password** for Remote Desktop for hello VMs.</span></span>
3. <span data-ttu-id="6c9b5-207">Se till att tooselect hello **prenumeration** som du vill ditt kluster toobe som distribuerats till, särskilt om du har flera prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-207">Make sure tooselect hello **Subscription** that you want your cluster toobe deployed to, especially if you have multiple subscriptions.</span></span>
4. <span data-ttu-id="6c9b5-208">Skapa en **ny resursgrupp**.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-208">Create a **new resource group**.</span></span> <span data-ttu-id="6c9b5-209">Det är bästa toogive den hello samma namn som hello klustret, eftersom det hjälper dig att hitta dem senare, särskilt när du försöker toomake ändringar tooyour distribution eller ta bort klustret.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-209">It is best toogive it hello same name as hello cluster, since it helps in finding them later, especially when you are trying toomake changes tooyour deployment or delete your cluster.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="6c9b5-210">Även om du kan bestämma toouse en befintlig resursgrupp är en bra idé toocreate en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-210">Although you can decide toouse an existing resource group, it is a good practice toocreate a new resource group.</span></span> <span data-ttu-id="6c9b5-211">Detta gör det enkelt toodelete kluster som du inte behöver.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-211">This makes it easy toodelete clusters that you do not need.</span></span>
   > 
   > 
5. <span data-ttu-id="6c9b5-212">Välj hello **region** som du vill toocreate hello klustret.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-212">Select hello **region** in which you want toocreate hello cluster.</span></span> <span data-ttu-id="6c9b5-213">Du måste använda hello tillhör samma region som din nyckel valvet.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-213">You must use hello same region that your Key Vault is in.</span></span>

#### <a name="2-cluster-configuration"></a><span data-ttu-id="6c9b5-214">2. Klusterkonfiguration</span><span class="sxs-lookup"><span data-stu-id="6c9b5-214">2. Cluster configuration</span></span>
![Skapa en nodtyp][CreateNodeType]

<span data-ttu-id="6c9b5-216">Konfigurera klusternoderna.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-216">Configure your cluster nodes.</span></span> <span data-ttu-id="6c9b5-217">Nodtyper definiera hello VM-storlekar, hello antal virtuella datorer och deras egenskaper.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-217">Node types define hello VM sizes, hello number of VMs, and their properties.</span></span> <span data-ttu-id="6c9b5-218">Klustret kan ha fler än en nodtyp, men hello primära nodtypen (hello förstnämnda som du definierar i hello portal) måste ha minst fem virtuella datorer, eftersom det är hello nodtypen där Service Fabric systemtjänster placeras.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-218">Your cluster can have more than one node type, but hello primary node type (hello first one that you define on hello portal) must have at least five VMs, as this is hello node type where Service Fabric system services are placed.</span></span> <span data-ttu-id="6c9b5-219">Konfigurera inte **placeringsegenskaper** eftersom en standardegenskap placering av ”NodeTypeName” läggs till automatiskt.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-219">Do not configure **Placement Properties** because a default placement property of "NodeTypeName" is added automatically.</span></span>

> [!NOTE]
> <span data-ttu-id="6c9b5-220">Ett vanligt scenario för flera nodtyper är ett program som innehåller en frontend-tjänst och en backend tjänst.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-220">A common scenario for multiple node types is an application that contains a front-end service and a back-end service.</span></span> <span data-ttu-id="6c9b5-221">Du vill tooput hello frontend-tjänst på mindre virtuella datorer (VM-storlekar som D2) med portar öppna toohello Internet men du vill tooput hello backend-tjänst på större virtuella datorer (med VM-storlekar som D4, D6, D15 och så vidare) utan Internet-riktade öppna portar.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-221">You want tooput hello front-end service on smaller VMs (VM sizes like D2) with ports open toohello Internet, but you want tooput hello back-end service on larger VMs (with VM sizes like D4, D6, D15, and so on) with no Internet-facing ports open.</span></span>
> 
> 

1. <span data-ttu-id="6c9b5-222">Välj ett namn för din nodtypen (1 too12 tecken som innehåller endast bokstäver och siffror).</span><span class="sxs-lookup"><span data-stu-id="6c9b5-222">Choose a name for your node type (1 too12 characters containing only letters and numbers).</span></span>
2. <span data-ttu-id="6c9b5-223">hello minsta **storlek** för virtuella datorer för hello primära noden typen drivs av hello **hållbarhet** nivå som du väljer för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-223">hello minimum **size** of VMs for hello primary node type is driven by hello **durability** tier you choose for hello cluster.</span></span> <span data-ttu-id="6c9b5-224">hello standardvärdet för hello hållbarhetsnivån är Brons.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-224">hello default for hello durability tier is bronze.</span></span> <span data-ttu-id="6c9b5-225">Mer information om hållbarhet finns [hur toochoose hello Service Fabric-kluster tillförlitlighet och hållbarhet][service-fabric-cluster-capacity].</span><span class="sxs-lookup"><span data-stu-id="6c9b5-225">For more information on durability, see [how toochoose hello Service Fabric cluster reliability and durability][service-fabric-cluster-capacity].</span></span>
3. <span data-ttu-id="6c9b5-226">Välj hello VM-storlek och prisnivå.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-226">Select hello VM size and pricing tier.</span></span> <span data-ttu-id="6c9b5-227">D-serien virtuella datorer ha SSD-enheterna och rekommenderas för tillståndskänsliga program.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-227">D-series VMs have SSD drives and are highly recommended for stateful applications.</span></span> <span data-ttu-id="6c9b5-228">Använd inte VM-SKU som har delvis kärnor eller vara mindre än 7 GB tillgängligt diskutrymme.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-228">Do not use any VM SKU that has partial cores or have less than 7 GB of available disk capacity.</span></span> 
4. <span data-ttu-id="6c9b5-229">hello minsta **nummer** för virtuella datorer för hello primära noden typen drivs av hello **tillförlitlighet** nivå som du väljer.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-229">hello minimum **number** of VMs for hello primary node type is driven by hello **reliability** tier you choose.</span></span> <span data-ttu-id="6c9b5-230">hello standardvärdet för hello tillförlitlighetsnivån är Silver.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-230">hello default for hello reliability tier is Silver.</span></span> <span data-ttu-id="6c9b5-231">Mer information om tillförlitlighet finns [hur toochoose hello Service Fabric-kluster tillförlitlighet och hållbarhet][service-fabric-cluster-capacity].</span><span class="sxs-lookup"><span data-stu-id="6c9b5-231">For more information on reliability, see [how toochoose hello Service Fabric cluster reliability and durability][service-fabric-cluster-capacity].</span></span>
5. <span data-ttu-id="6c9b5-232">Välj hello antal virtuella datorer för hello nodtypen.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-232">Choose hello number of VMs for hello node type.</span></span> <span data-ttu-id="6c9b5-233">Du kan skala upp eller ned hello antal virtuella datorer i en nodtyp vid ett senare tillfälle, men på hello primära nodtypen hello minsta drivs av hello tillförlitlighet nivå som du har valt.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-233">You can scale up or down hello number of VMs in a node type later on, but on hello primary node type, hello minimum is driven by hello reliability level that you have chosen.</span></span> <span data-ttu-id="6c9b5-234">Andra nodtyper kan ha minst 1 VM.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-234">Other node types can have a minimum of 1 VM.</span></span>
6. <span data-ttu-id="6c9b5-235">Konfigurera anpassade slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-235">Configure custom endpoints.</span></span> <span data-ttu-id="6c9b5-236">Det här fältet kan tooenter en kommaavgränsad lista över portar som du vill tooexpose via hello Azure belastningsutjämnare toohello offentliga Internet för dina program.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-236">This field allows you tooenter a comma-separated list of ports that you want tooexpose through hello Azure Load Balancer toohello public Internet for your applications.</span></span> <span data-ttu-id="6c9b5-237">Om du planerar toodeploy ett web application tooyour kluster, t.ex ”80” här tooallow trafik på port 80 till klustret.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-237">For example, if you plan toodeploy a web application tooyour cluster, enter "80" here tooallow traffic on port 80 into your cluster.</span></span> <span data-ttu-id="6c9b5-238">Läs mer på slutpunkter [kommunicera med program][service-fabric-connect-and-communicate-with-services]</span><span class="sxs-lookup"><span data-stu-id="6c9b5-238">For more information on endpoints, see [communicating with applications][service-fabric-connect-and-communicate-with-services]</span></span>
7. <span data-ttu-id="6c9b5-239">Konfigurera klustret **diagnostik**.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-239">Configure cluster **diagnostics**.</span></span> <span data-ttu-id="6c9b5-240">Som standard aktiveras diagnostik på ditt kluster tooassist med felsökning av problem.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-240">By default, diagnostics are enabled on your cluster tooassist with troubleshooting issues.</span></span> <span data-ttu-id="6c9b5-241">Om du vill toodisable diagnostik ändra hello **Status** växla för**av**.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-241">If you want toodisable diagnostics change hello **Status** toggle too**Off**.</span></span> <span data-ttu-id="6c9b5-242">Om du inaktiverar diagnostik är **inte** rekommenderas.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-242">Turning off diagnostics is **not** recommended.</span></span>
8. <span data-ttu-id="6c9b5-243">Välj hello Fabric Uppgraderingsläge som du vill ange ditt kluster till.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-243">Select hello Fabric upgrade mode you want set your cluster to.</span></span> <span data-ttu-id="6c9b5-244">Välj **automatisk**, om du vill hello system tooautomatically Välj in hello senaste tillgängliga versionen och försök tooupgrade kluster-tooit.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-244">Select **Automatic**, if you want hello system tooautomatically pick up hello latest available version and try tooupgrade your cluster tooit.</span></span> <span data-ttu-id="6c9b5-245">Ange hello-läge för**manuell**om du vill toochoose en version som stöds.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-245">Set hello mode too**Manual**, if you want toochoose a supported version.</span></span>

> [!NOTE]
> <span data-ttu-id="6c9b5-246">Vi stöder endast kluster som kör versioner av service Fabric stöds.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-246">We support only clusters that are running supported versions of service Fabric.</span></span> <span data-ttu-id="6c9b5-247">Genom att välja hello **manuell** läge du tar på hello ansvar tooupgrade ditt kluster tooa som stöds av version.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-247">By selecting hello **Manual** mode, you are taking on hello responsibility tooupgrade your cluster tooa supported version.</span></span> <span data-ttu-id="6c9b5-248">Mer information om hello Fabric Uppgraderingsläge finns hello [service fabric-kluster-uppgradering dokumentet.][service-fabric-cluster-upgrade]</span><span class="sxs-lookup"><span data-stu-id="6c9b5-248">For more details on hello Fabric upgrade mode see hello [service-fabric-cluster-upgrade document.][service-fabric-cluster-upgrade]</span></span>
> 
> 

#### <a name="3-security"></a><span data-ttu-id="6c9b5-249">3. Säkerhet</span><span class="sxs-lookup"><span data-stu-id="6c9b5-249">3. Security</span></span>
![Skärmbild som visar säkerhetskonfigurationer på Azure-portalen.][SecurityConfigs]

<span data-ttu-id="6c9b5-251">hello sista steget är tooprovide certifikat information toosecure hello kluster med hjälp av hello Key Vault och certifikat information skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-251">hello final step is tooprovide certificate information toosecure hello cluster using hello Key Vault and certificate information created earlier.</span></span>

* <span data-ttu-id="6c9b5-252">Fylla i hello certifikatet för primär fält med hello utdata från överför hello **klustret certifikat** tooKey valvet med hello `Invoke-AddCertToKeyVault` PowerShell-kommando.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-252">Populate hello primary certificate fields with hello output obtained from uploading hello **cluster certificate** tooKey Vault using hello `Invoke-AddCertToKeyVault` PowerShell command.</span></span>

```powershell
Name  : CertificateThumbprint
Value : <value>

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault

Name  : CertificateURL
Value : https://myvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47
```

* <span data-ttu-id="6c9b5-253">Kontrollera hello **konfigurera avancerade inställningar** rutan tooenter klientcertifikat för **administratörsklient** och **skrivskyddad klient**.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-253">Check hello **Configure advanced settings** box tooenter client certificates for **admin client** and **read-only client**.</span></span> <span data-ttu-id="6c9b5-254">I de här fälten att ange hello tumavtryck klientcertifikatet admin och hello tumavtryck klientcertifikatet skrivskyddade användaren, om tillämpligt.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-254">In these fields, enter hello thumbprint of your admin client certificate and hello thumbprint of your read-only user client certificate, if applicable.</span></span> <span data-ttu-id="6c9b5-255">När administratörer tooconnect toohello klustret kan beviljas åtkomst endast om de har ett certifikat med ett tumavtryck som matchar hello tumavtrycket värden anges här.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-255">When administrators attempt tooconnect toohello cluster, they are granted access only if they have a certificate with a thumbprint that matches hello thumbprint values entered here.</span></span>  

#### <a name="4-summary"></a><span data-ttu-id="6c9b5-256">4. Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="6c9b5-256">4. Summary</span></span>
![<span data-ttu-id="6c9b5-257">Skärmbild som visar hello start board visa ”distribuera Service Fabric-kluster”.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-257">Screen shot of hello start board displaying "Deploying Service Fabric Cluster."</span></span> ][Notifications]

<span data-ttu-id="6c9b5-258">toocomplete hello klustret har skapats, klickar du på **sammanfattning** toosee hello konfigurationer som du har angett eller hämta hello Azure Resource Manager-mall som används toodeploy klustret.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-258">toocomplete hello cluster creation, click **Summary** toosee hello configurations that you have provided, or download hello Azure Resource Manager template that that used toodeploy your cluster.</span></span> <span data-ttu-id="6c9b5-259">När du har angett hello obligatoriska inställningar hello **OK** knappen blir grön och du kan starta hello klusterskapandeprocessen genom att klicka på den.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-259">After you have provided hello mandatory settings, hello **OK** button becomes green and you can start hello cluster creation process by clicking it.</span></span>

<span data-ttu-id="6c9b5-260">Du kan se hello förlopp i hello meddelanden.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-260">You can see hello creation progress in hello notifications.</span></span> <span data-ttu-id="6c9b5-261">(Klicka på hello ”” klockikonen nära hello hello övre högra hörnet på skärmen i statusfältet.) Om du klickade på **PIN-kod tooStartboard** när du skapar klustret hello visas **distribuerar Service Fabric-kluster** Fäst toohello **starta** kort.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-261">(Click hello "Bell" icon near hello status bar at hello upper right of your screen.) If you clicked **Pin tooStartboard** while creating hello cluster, you will see **Deploying Service Fabric Cluster** pinned toohello **Start** board.</span></span>

### <a name="view-your-cluster-status"></a><span data-ttu-id="6c9b5-262">Visa klusterstatus för</span><span class="sxs-lookup"><span data-stu-id="6c9b5-262">View your cluster status</span></span>
![Skärmbild som visar information om kluster hello instrumentpanelen.][ClusterDashboard]

<span data-ttu-id="6c9b5-264">När klustret har skapats kan du granska klustret i hello portal:</span><span class="sxs-lookup"><span data-stu-id="6c9b5-264">Once your cluster is created, you can inspect your cluster in hello portal:</span></span>

1. <span data-ttu-id="6c9b5-265">Gå för**Bläddra** och på **Service Fabric-kluster**.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-265">Go too**Browse** and click **Service Fabric Clusters**.</span></span>
2. <span data-ttu-id="6c9b5-266">Leta upp ditt kluster och klicka på den.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-266">Locate your cluster and click it.</span></span>
3. <span data-ttu-id="6c9b5-267">Du kan nu se hello information på klustret i hello instrumentpanel, inklusive hello klustrets offentlig slutpunkt och en länk tooService Fabric Explorer.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-267">You can now see hello details of your cluster in hello dashboard, including hello cluster's public endpoint and a link tooService Fabric Explorer.</span></span>

<span data-ttu-id="6c9b5-268">Hej **nod övervakaren** avsnitt på hello klustrets instrumentpanel blad anger hello antal virtuella datorer som är felfria och inte felfri.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-268">hello **Node Monitor** section on hello cluster's dashboard blade indicates hello number of VMs that are healthy and not healthy.</span></span> <span data-ttu-id="6c9b5-269">Du hittar mer information om hello klustrets hälsa i [Service Fabric hälsa modellen introduktion][service-fabric-health-introduction].</span><span class="sxs-lookup"><span data-stu-id="6c9b5-269">You can find more details about hello cluster's health at [Service Fabric health model introduction][service-fabric-health-introduction].</span></span>

> [!NOTE]
> <span data-ttu-id="6c9b5-270">Service Fabric-kluster kräver ett visst antal noder toobe alltid toomaintain tillgänglighet och bevara tillstånd - refererad tooas ”underhålla kvorum”.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-270">Service Fabric clusters require a certain number of nodes toobe up always toomaintain availability and preserve state - referred tooas "maintaining quorum".</span></span> <span data-ttu-id="6c9b5-271">Therfore, det är vanligtvis inte säker tooshut ned alla datorer i hello kluster om du först har utfört en [fullständig säkerhetskopiering av din tillstånd][service-fabric-reliable-services-backup-restore].</span><span class="sxs-lookup"><span data-stu-id="6c9b5-271">Therfore, it is typically not safe tooshut down all machines in hello cluster unless you have first performed a [full backup of your state][service-fabric-reliable-services-backup-restore].</span></span>
> 
> 

## <a name="remote-connect-tooa-virtual-machine-scale-set-instance-or-a-cluster-node"></a><span data-ttu-id="6c9b5-272">Fjärranslutning tooa Virtual Machine Scale Set-instans eller en klusternod</span><span class="sxs-lookup"><span data-stu-id="6c9b5-272">Remote connect tooa Virtual Machine Scale Set instance or a cluster node</span></span>
<span data-ttu-id="6c9b5-273">Var och en av hello nodetypes får du ange i klustret resultaten i en virtuell dator Skaluppsättning komma installation.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-273">Each of hello NodeTypes you specify in your cluster results in a Virtual Machine Scale Set getting set-up.</span></span> <span data-ttu-id="6c9b5-274">Se [fjärranslutning tooa Skaluppsättning för virtuell dator instans] [ remote-connect-to-a-vm-scale-set] mer information.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-274">See [Remote connect tooa Virtual Machine Scale Set instance][remote-connect-to-a-vm-scale-set] for details.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6c9b5-275">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6c9b5-275">Next steps</span></span>
<span data-ttu-id="6c9b5-276">Du har nu ett säker kluster som använder certifikat för autentisering för hantering.</span><span class="sxs-lookup"><span data-stu-id="6c9b5-276">At this point, you have a secure cluster using certificates for management authentication.</span></span> <span data-ttu-id="6c9b5-277">Nästa [ansluta tooyour klustret](service-fabric-connect-to-secure-cluster.md) och lära dig hur för[hantera program hemligheter](service-fabric-application-secret-management.md).</span><span class="sxs-lookup"><span data-stu-id="6c9b5-277">Next, [connect tooyour cluster](service-fabric-connect-to-secure-cluster.md) and learn how too[manage application secrets](service-fabric-application-secret-management.md).</span></span>  <span data-ttu-id="6c9b5-278">Lär dig dessutom [Service Fabric supportalternativ](service-fabric-support.md).</span><span class="sxs-lookup"><span data-stu-id="6c9b5-278">Also, learn about [Service Fabric support options](service-fabric-support.md).</span></span>

<!-- Links -->
[azure-powershell]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[service-fabric-rp-helpers]: https://github.com/ChackDan/Service-Fabric/tree/master/Scripts/ServiceFabricRPHelpers
[azure-portal]: https://portal.azure.com/
[key-vault-get-started]: ../key-vault/key-vault-get-started.md
[create-cluster-arm]: service-fabric-cluster-creation-via-arm.md
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[service-fabric-cluster-security-roles]: service-fabric-cluster-security-roles.md
[service-fabric-cluster-capacity]: service-fabric-cluster-capacity.md
[service-fabric-connect-and-communicate-with-services]: service-fabric-connect-and-communicate-with-services.md
[service-fabric-health-introduction]: service-fabric-health-introduction.md
[service-fabric-reliable-services-backup-restore]: service-fabric-reliable-services-backup-restore.md
[remote-connect-to-a-vm-scale-set]: service-fabric-cluster-nodetypes.md#remote-connect-to-a-vm-scale-set-instance-or-a-cluster-node
[service-fabric-cluster-upgrade]: service-fabric-cluster-upgrade.md

<!--Image references-->
[SearchforServiceFabricClusterTemplate]: ./media/service-fabric-cluster-creation-via-portal/SearchforServiceFabricClusterTemplate.png
[CreateRG]: ./media/service-fabric-cluster-creation-via-portal/CreateRG.png
[CreateNodeType]: ./media/service-fabric-cluster-creation-via-portal/NodeType.png
[SecurityConfigs]: ./media/service-fabric-cluster-creation-via-portal/SecurityConfigs.png
[Notifications]: ./media/service-fabric-cluster-creation-via-portal/notifications.png
[ClusterDashboard]: ./media/service-fabric-cluster-creation-via-portal/ClusterDashboard.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png
