---
title: "aaaCreate ett Azure Service Fabric-kluster från en mall | Microsoft Docs"
description: "Den här artikeln beskriver hur tooset upp en säker Service Fabric-kluster i Azure med Azure Resource Manager, Azure Key Vault och Azure Active Directory (Azure AD) för klientautentisering."
services: service-fabric
documentationcenter: .net
author: chackdan
manager: timlt
editor: chackdan
ms.assetid: 15d0ab67-fc66-4108-8038-3584eeebabaa
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/22/2017
ms.author: chackdan
ms.openlocfilehash: a4563c58a68127720a8290c3be0df9d026833eb4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-service-fabric-cluster-by-using-azure-resource-manager"></a><span data-ttu-id="21ca4-103">Skapa ett Service Fabric-kluster med hjälp av Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="21ca4-103">Create a Service Fabric cluster by using Azure Resource Manager</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="21ca4-104">Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="21ca4-104">Azure Resource Manager</span></span>](service-fabric-cluster-creation-via-arm.md)
> * [<span data-ttu-id="21ca4-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="21ca4-105">Azure portal</span></span>](service-fabric-cluster-creation-via-portal.md)
>
>

<span data-ttu-id="21ca4-106">Den här stegvisa guiden vägleder dig genom att skapa en säker Azure Service Fabric-kluster i Azure med hjälp av Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="21ca4-106">This step-by-step guide walks you through setting up a secure Azure Service Fabric cluster in Azure by using Azure Resource Manager.</span></span> <span data-ttu-id="21ca4-107">Vi bekräftar den hello artikeln är långt.</span><span class="sxs-lookup"><span data-stu-id="21ca4-107">We acknowledge that hello article is long.</span></span> <span data-ttu-id="21ca4-108">Dock om du redan är noggrant bekant med hello innehåll vara säker på att toofollow varje steg noggrant.</span><span class="sxs-lookup"><span data-stu-id="21ca4-108">Nevertheless, unless you are already thoroughly familiar with hello content, be sure toofollow each step carefully.</span></span>

<span data-ttu-id="21ca4-109">hello handboken innehåller hello följande procedurer:</span><span class="sxs-lookup"><span data-stu-id="21ca4-109">hello guide covers hello following procedures:</span></span>

* <span data-ttu-id="21ca4-110">Skapa ett Azure key vault tooupload certifikat för kluster- och säkerhet</span><span class="sxs-lookup"><span data-stu-id="21ca4-110">Setting up an Azure key vault tooupload certificates for cluster and application security</span></span>
* <span data-ttu-id="21ca4-111">Skapar en skyddad kluster i Azure med hjälp av Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="21ca4-111">Creating a secured cluster in Azure by using Azure Resource Manager</span></span>
* <span data-ttu-id="21ca4-112">Autentisering av användare med hjälp av Azure Active Directory (Azure AD) för hantering av kluster</span><span class="sxs-lookup"><span data-stu-id="21ca4-112">Authenticating users by using Azure Active Directory (Azure AD) for cluster management</span></span>

<span data-ttu-id="21ca4-113">En säker klustret är ett kluster som förhindrar obehörig åtkomst toomanagement åtgärder.</span><span class="sxs-lookup"><span data-stu-id="21ca4-113">A secure cluster is a cluster that prevents unauthorized access toomanagement operations.</span></span> <span data-ttu-id="21ca4-114">Detta inkluderar att distribuera, uppgradering, och ta bort program, tjänster och hello data de innehåller.</span><span class="sxs-lookup"><span data-stu-id="21ca4-114">This includes deploying, upgrading, and deleting applications, services, and hello data they contain.</span></span> <span data-ttu-id="21ca4-115">Ett oskyddat klustret är ett kluster alla kan ansluta tooat helst och utföra hanteringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="21ca4-115">An unsecure cluster is a cluster that anyone can connect tooat any time and perform management operations.</span></span> <span data-ttu-id="21ca4-116">Men det är möjligt toocreate ett oskyddade kluster, rekommenderar vi starkt att du skapar en säker kluster från hello början.</span><span class="sxs-lookup"><span data-stu-id="21ca4-116">Although it is possible toocreate an unsecure cluster, we highly recommend that you create a secure cluster from hello outset.</span></span> <span data-ttu-id="21ca4-117">Eftersom ett oskyddade kluster inte kan säkras senare, måste du skapa ett nytt kluster.</span><span class="sxs-lookup"><span data-stu-id="21ca4-117">Because an unsecure cluster cannot be secured later, a new cluster must be created.</span></span>

<span data-ttu-id="21ca4-118">hello begreppet skapa skyddade kluster är hello samma, oavsett om de är Linux eller Windows-kluster.</span><span class="sxs-lookup"><span data-stu-id="21ca4-118">hello concept of creating secure clusters is hello same, whether they are Linux or Windows clusters.</span></span> <span data-ttu-id="21ca4-119">Mer information och hjälp skript för att skapa säkra Linux-kluster, se [att skapa skyddade kluster på Linux](#secure-linux-clusters).</span><span class="sxs-lookup"><span data-stu-id="21ca4-119">For more information and helper scripts for creating secure Linux clusters, see [Creating secure clusters on Linux](#secure-linux-clusters).</span></span>

## <a name="sign-in-tooyour-azure-account"></a><span data-ttu-id="21ca4-120">Logga in tooyour Azure-konto</span><span class="sxs-lookup"><span data-stu-id="21ca4-120">Sign in tooyour Azure account</span></span>
<span data-ttu-id="21ca4-121">Den här guiden använder [Azure PowerShell][azure-powershell].</span><span class="sxs-lookup"><span data-stu-id="21ca4-121">This guide uses [Azure PowerShell][azure-powershell].</span></span> <span data-ttu-id="21ca4-122">När du startar en ny PowerShell-session, logga in tooyour Azure-konto och välja din prenumeration innan du kan köra kommandon för Azure.</span><span class="sxs-lookup"><span data-stu-id="21ca4-122">When you start a new PowerShell session, sign in tooyour Azure account and select your subscription before you execute Azure commands.</span></span>

<span data-ttu-id="21ca4-123">Logga in tooyour Azure-konto:</span><span class="sxs-lookup"><span data-stu-id="21ca4-123">Sign in tooyour Azure account:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="21ca4-124">Välj din prenumeration:</span><span class="sxs-lookup"><span data-stu-id="21ca4-124">Select your subscription:</span></span>

```powershell
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

## <a name="set-up-a-key-vault"></a><span data-ttu-id="21ca4-125">Ställ in ett nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="21ca4-125">Set up a key vault</span></span>
<span data-ttu-id="21ca4-126">Det här avsnittet beskrivs skapa nyckelvalvet för Service Fabric-kluster i Azure och för Service Fabric-program.</span><span class="sxs-lookup"><span data-stu-id="21ca4-126">This section discusses creating a key vault for a Service Fabric cluster in Azure and for Service Fabric applications.</span></span> <span data-ttu-id="21ca4-127">Fullständig guide-tooAzure Key Vault finns toohello [Key Vault Kom igång med][key-vault-get-started].</span><span class="sxs-lookup"><span data-stu-id="21ca4-127">For a complete guide tooAzure Key Vault, refer toohello [Key Vault getting started guide][key-vault-get-started].</span></span>

<span data-ttu-id="21ca4-128">Service Fabric använder X.509-certifikat toosecure ett kluster och säkerhetsfunktioner för programmet.</span><span class="sxs-lookup"><span data-stu-id="21ca4-128">Service Fabric uses X.509 certificates toosecure a cluster and provide application security features.</span></span> <span data-ttu-id="21ca4-129">Du använder Key Vault toomanage certifikat för Service Fabric-kluster i Azure.</span><span class="sxs-lookup"><span data-stu-id="21ca4-129">You use Key Vault toomanage certificates for Service Fabric clusters in Azure.</span></span> <span data-ttu-id="21ca4-130">När ett kluster distribueras i Azure hämtar certifikat från Nyckelvalvet hello Azure-resursprovider som ansvarar för att skapa Service Fabric-kluster och installerar dem på hello klustrets virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="21ca4-130">When a cluster is deployed in Azure, hello Azure resource provider that's responsible for creating Service Fabric clusters pulls certificates from Key Vault and installs them on hello cluster VMs.</span></span>

<span data-ttu-id="21ca4-131">hello illustrerar följande diagram hello förhållandet mellan Azure Key Vault, Service Fabric-kluster och hello Azure-resursprovider som använder certifikat som lagras i en nyckelvalvet när den skapar ett kluster:</span><span class="sxs-lookup"><span data-stu-id="21ca4-131">hello following diagram illustrates hello relationship between Azure Key Vault, a Service Fabric cluster, and hello Azure resource provider that uses certificates stored in a key vault when it creates a cluster:</span></span>

![Diagram över certifikatinstallation][cluster-security-cert-installation]

### <a name="create-a-resource-group"></a><span data-ttu-id="21ca4-133">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="21ca4-133">Create a resource group</span></span>
<span data-ttu-id="21ca4-134">hello första steget är toocreate en resursgrupp för nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="21ca4-134">hello first step is toocreate a resource group specifically for your key vault.</span></span> <span data-ttu-id="21ca4-135">Vi rekommenderar att du placerar hello nyckelvalv i sin egen resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="21ca4-135">We recommend that you put hello key vault into its own resource group.</span></span> <span data-ttu-id="21ca4-136">Den här åtgärden kan du ta bort hello beräkning och lagring resursgrupper, inklusive hello resursgruppen som innehåller Service Fabric-klustret utan att förlora dina nycklar och hemligheter.</span><span class="sxs-lookup"><span data-stu-id="21ca4-136">This action lets you remove hello compute and storage resource groups, including hello resource group that contains your Service Fabric cluster, without losing your keys and secrets.</span></span> <span data-ttu-id="21ca4-137">hello resursgruppen som innehåller nyckelvalvet _hello måste ha samma region_ som hello-kluster som använder den.</span><span class="sxs-lookup"><span data-stu-id="21ca4-137">hello resource group that contains your key vault _must be in hello same region_ as hello cluster that is using it.</span></span>

<span data-ttu-id="21ca4-138">Om du planerar toodeploy kluster i flera områden, föreslår vi att du namnger hello resursgrupp och hello nyckelvalv på ett sätt som anger vilken region som den tillhör.</span><span class="sxs-lookup"><span data-stu-id="21ca4-138">If you plan toodeploy clusters in multiple regions, we suggest that you name hello resource group and hello key vault in a way that indicates which region it belongs to.</span></span>  

```powershell

    New-AzureRmResourceGroup -Name westus-mykeyvault -Location 'West US'
```
<span data-ttu-id="21ca4-139">hello utdata ska se ut så här:</span><span class="sxs-lookup"><span data-stu-id="21ca4-139">hello output should look like this:</span></span>

```powershell

    WARNING: hello output object type of this cmdlet is going toobe modified in a future release.

    ResourceGroupName : westus-mykeyvault
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/<guid>/resourceGroups/westus-mykeyvault

```
<a id="new-key-vault"></a>

### <a name="create-a-key-vault-in-hello-new-resource-group"></a><span data-ttu-id="21ca4-140">Skapa en nyckelvalvet i hello ny resursgrupp</span><span class="sxs-lookup"><span data-stu-id="21ca4-140">Create a key vault in hello new resource group</span></span>
<span data-ttu-id="21ca4-141">Hej nyckelvalv _måste vara aktiverat för distribution_ tooallow hello beräkning resource provider tooget certifikat från den och installera det på instanser för virtuella datorer:</span><span class="sxs-lookup"><span data-stu-id="21ca4-141">hello key vault _must be enabled for deployment_ tooallow hello compute resource provider tooget certificates from it and install it on virtual machine instances:</span></span>

```powershell

    New-AzureRmKeyVault -VaultName 'mywestusvault' -ResourceGroupName 'westus-mykeyvault' -Location 'West US' -EnabledForDeployment

```

<span data-ttu-id="21ca4-142">hello utdata ska se ut så här:</span><span class="sxs-lookup"><span data-stu-id="21ca4-142">hello output should look like this:</span></span>

```powershell

    Vault Name                       : mywestusvault
    Resource Group Name              : westus-mykeyvault
    Location                         : West US
    Resource ID                      : /subscriptions/<guid>/resourceGroups/westus-mykeyvault/providers/Microsoft.KeyVault/vaults/mywestusvault
    Vault URI                        : https://mywestusvault.vault.azure.net
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
<a id="existing-key-vault"></a>

## <a name="use-an-existing-key-vault"></a><span data-ttu-id="21ca4-143">Använd en befintlig nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="21ca4-143">Use an existing key vault</span></span>

<span data-ttu-id="21ca4-144">toouse en befintlig nyckelvalv du _måste aktivera den för distribution_ tooallow hello beräkning resource provider tooget certifikat från den och installera den på klusternoder:</span><span class="sxs-lookup"><span data-stu-id="21ca4-144">toouse an existing key vault, you _must enable it for deployment_ tooallow hello compute resource provider tooget certificates from it and install it on cluster nodes:</span></span>

```powershell

Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

```

<a id="add-certificate-to-key-vault"></a>

## <a name="add-certificates-tooyour-key-vault"></a><span data-ttu-id="21ca4-145">Lägg till certifikat tooyour nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="21ca4-145">Add certificates tooyour key vault</span></span>

<span data-ttu-id="21ca4-146">Certifikat används i Service Fabric tooprovide autentisering och kryptering toosecure olika aspekter av ett kluster och dess program.</span><span class="sxs-lookup"><span data-stu-id="21ca4-146">Certificates are used in Service Fabric tooprovide authentication and encryption toosecure various aspects of a cluster and its applications.</span></span> <span data-ttu-id="21ca4-147">Mer information om hur du använder certifikat i Service Fabric finns [säkerhetsscenarier för Service Fabric-klustret][service-fabric-cluster-security].</span><span class="sxs-lookup"><span data-stu-id="21ca4-147">For more information on how certificates are used in Service Fabric, see [Service Fabric cluster security scenarios][service-fabric-cluster-security].</span></span>

### <a name="cluster-and-server-certificate-required"></a><span data-ttu-id="21ca4-148">Kluster- och certifikat (krävs)</span><span class="sxs-lookup"><span data-stu-id="21ca4-148">Cluster and server certificate (required)</span></span>
<span data-ttu-id="21ca4-149">Det här certifikatet är obligatoriska toosecure ett kluster och förhindra obehörig åtkomst tooit.</span><span class="sxs-lookup"><span data-stu-id="21ca4-149">This certificate is required toosecure a cluster and prevent unauthorized access tooit.</span></span> <span data-ttu-id="21ca4-150">Det ger säkerhet i klustret på två sätt:</span><span class="sxs-lookup"><span data-stu-id="21ca4-150">It provides cluster security in two ways:</span></span>

* <span data-ttu-id="21ca4-151">Autentisering för klustret: autentiserar nod till nod kommunikation för klustret.</span><span class="sxs-lookup"><span data-stu-id="21ca4-151">Cluster authentication: Authenticates node-to-node communication for cluster federation.</span></span> <span data-ttu-id="21ca4-152">Endast noder som kan bevisa sin identitet med det här certifikatet kan gå hello klustret.</span><span class="sxs-lookup"><span data-stu-id="21ca4-152">Only nodes that can prove their identity with this certificate can join hello cluster.</span></span>
* <span data-ttu-id="21ca4-153">Serverautentisering: autentiserar hello cluster management slutpunkter tooa management-klienten, så att hello Hanteringsklient vet pratar toohello verkliga klustret.</span><span class="sxs-lookup"><span data-stu-id="21ca4-153">Server authentication: Authenticates hello cluster management endpoints tooa management client, so that hello management client knows it is talking toohello real cluster.</span></span> <span data-ttu-id="21ca4-154">Det här certifikatet ger också en SSL för hello HTTPS hanterings-API och för Service Fabric Explorer via HTTPS.</span><span class="sxs-lookup"><span data-stu-id="21ca4-154">This certificate also provides an SSL for hello HTTPS management API and for Service Fabric Explorer over HTTPS.</span></span>

<span data-ttu-id="21ca4-155">tooserve dessa ändamål hello certifikat måste uppfylla följande krav hello:</span><span class="sxs-lookup"><span data-stu-id="21ca4-155">tooserve these purposes, hello certificate must meet hello following requirements:</span></span>

* <span data-ttu-id="21ca4-156">hello certifikatet måste innehålla en privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="21ca4-156">hello certificate must contain a private key.</span></span>
* <span data-ttu-id="21ca4-157">hello certifikat måste skapas för nyckelutbyte, som kan exporteras tooa Personal Information Exchange (.pfx)-fil.</span><span class="sxs-lookup"><span data-stu-id="21ca4-157">hello certificate must be created for key exchange, which is exportable tooa Personal Information Exchange (.pfx) file.</span></span>
* <span data-ttu-id="21ca4-158">hello certifikatets ämnesnamn måste matcha hello-domän som du använder tooaccess hello Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="21ca4-158">hello certificate's subject name must match hello domain that you use tooaccess hello Service Fabric cluster.</span></span> <span data-ttu-id="21ca4-159">Den här matchning är obligatoriska tooprovide en SSL för hello klustrets HTTPS management slutpunkter och Service Fabric Explorer.</span><span class="sxs-lookup"><span data-stu-id="21ca4-159">This matching is required tooprovide an SSL for hello cluster's HTTPS management endpoints and Service Fabric Explorer.</span></span> <span data-ttu-id="21ca4-160">Du kan skaffa ett SSL-certifikat från en certifikatutfärdare (CA) för hello. cloudapp.azure.com domän.</span><span class="sxs-lookup"><span data-stu-id="21ca4-160">You cannot obtain an SSL certificate from a certificate authority (CA) for hello .cloudapp.azure.com domain.</span></span> <span data-ttu-id="21ca4-161">Du måste skaffa ett anpassat domännamn för klustret.</span><span class="sxs-lookup"><span data-stu-id="21ca4-161">You must obtain a custom domain name for your cluster.</span></span> <span data-ttu-id="21ca4-162">När du begär ett certifikat från en Certifikatutfärdare måste hello certifikatets ämnesnamn matcha hello domännamn som du använder för klustret.</span><span class="sxs-lookup"><span data-stu-id="21ca4-162">When you request a certificate from a CA, hello certificate's subject name must match hello custom domain name that you use for your cluster.</span></span>

### <a name="application-certificates-optional"></a><span data-ttu-id="21ca4-163">Certifikat för programmet (valfritt)</span><span class="sxs-lookup"><span data-stu-id="21ca4-163">Application certificates (optional)</span></span>
<span data-ttu-id="21ca4-164">Valfritt antal ytterligare certifikat kan installeras på ett kluster av säkerhetsskäl för programmet.</span><span class="sxs-lookup"><span data-stu-id="21ca4-164">Any number of additional certificates can be installed on a cluster for application security purposes.</span></span> <span data-ttu-id="21ca4-165">Tänk hello applikationssäkerhets-scenarier som kräver ett certifikat toobe som har installerats på hello noder innan du skapar klustret:</span><span class="sxs-lookup"><span data-stu-id="21ca4-165">Before creating your cluster, consider hello application security scenarios that require a certificate toobe installed on hello nodes, such as:</span></span>

* <span data-ttu-id="21ca4-166">Kryptering och dekryptering av konfigurationsvärden för programmet.</span><span class="sxs-lookup"><span data-stu-id="21ca4-166">Encryption and decryption of application configuration values.</span></span>
* <span data-ttu-id="21ca4-167">Kryptering av data mellan noderna under replikeringen.</span><span class="sxs-lookup"><span data-stu-id="21ca4-167">Encryption of data across nodes during replication.</span></span>

### <a name="formatting-certificates-for-azure-resource-provider-use"></a><span data-ttu-id="21ca4-168">Formatering certifikat för användning av Azure resource provider</span><span class="sxs-lookup"><span data-stu-id="21ca4-168">Formatting certificates for Azure resource provider use</span></span>
<span data-ttu-id="21ca4-169">Du kan lägga till och använda privata nyckelfiler (.pfx) direkt via nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="21ca4-169">You can add and use private key files (.pfx) directly through your key vault.</span></span> <span data-ttu-id="21ca4-170">Hello Azure compute-resursprovidern kräver dock nycklar toobe lagras i ett specialformat JavaScript Object Notation (JSON).</span><span class="sxs-lookup"><span data-stu-id="21ca4-170">However, hello Azure compute resource provider requires keys toobe stored in a special JavaScript Object Notation (JSON) format.</span></span> <span data-ttu-id="21ca4-171">hello-formatet innehåller hello .pfx-fil som en base 64-kodad sträng och lösenordet för hello privata nyckeln.</span><span class="sxs-lookup"><span data-stu-id="21ca4-171">hello format includes hello .pfx file as a base 64-encoded string and hello private key password.</span></span> <span data-ttu-id="21ca4-172">tooaccommodate kraven hello nycklar måste placeras i en JSON-sträng och sedan lagras som ”hemligheter” i hello nyckeln valvet.</span><span class="sxs-lookup"><span data-stu-id="21ca4-172">tooaccommodate these requirements, hello keys must be placed in a JSON string and then stored as "secrets" in hello key vault.</span></span>

<span data-ttu-id="21ca4-173">toomake detta enklare och bearbeta en [PowerShell-modulen finns på GitHub][service-fabric-rp-helpers].</span><span class="sxs-lookup"><span data-stu-id="21ca4-173">toomake this process easier, a [PowerShell module is available on GitHub][service-fabric-rp-helpers].</span></span> <span data-ttu-id="21ca4-174">toouse hello modulen hello följande:</span><span class="sxs-lookup"><span data-stu-id="21ca4-174">toouse hello module, do hello following:</span></span>

1. <span data-ttu-id="21ca4-175">Hämta hello hela innehållet i hello lagringsplatsen till en lokal katalog.</span><span class="sxs-lookup"><span data-stu-id="21ca4-175">Download hello entire contents of hello repo into a local directory.</span></span>
2. <span data-ttu-id="21ca4-176">Gå toohello lokal katalog.</span><span class="sxs-lookup"><span data-stu-id="21ca4-176">Go toohello local directory.</span></span>
2. <span data-ttu-id="21ca4-177">Importera hello ServiceFabricRPHelpers modul i PowerShell-fönstret:</span><span class="sxs-lookup"><span data-stu-id="21ca4-177">Import hello ServiceFabricRPHelpers module in your PowerShell window:</span></span>

```powershell

 Import-Module "C:\..\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"

```

<span data-ttu-id="21ca4-178">Hej `Invoke-AddCertToKeyVault` kommandot i PowerShell-modulen automatiskt formaterar en certifikatets privata nyckel till en JSON-sträng och överförs toohello nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="21ca4-178">hello `Invoke-AddCertToKeyVault` command in this PowerShell module automatically formats a certificate private key into a JSON string and uploads it toohello key vault.</span></span> <span data-ttu-id="21ca4-179">Använd hello kommandot tooadd hello klustret certifikat och nyckelvalv några ytterligare certifikat toohello.</span><span class="sxs-lookup"><span data-stu-id="21ca4-179">Use hello command tooadd hello cluster certificate and any additional application certificates toohello key vault.</span></span> <span data-ttu-id="21ca4-180">Upprepa det här steget för några ytterligare certifikat som du vill använda tooinstall i klustret.</span><span class="sxs-lookup"><span data-stu-id="21ca4-180">Repeat this step for any additional certificates you want tooinstall in your cluster.</span></span>

#### <a name="uploading-an-existing-certificate"></a><span data-ttu-id="21ca4-181">Ladda upp ett befintligt certifikat</span><span class="sxs-lookup"><span data-stu-id="21ca4-181">Uploading an existing certificate</span></span>

```powershell

 Invoke-AddCertToKeyVault -SubscriptionId <guid> -ResourceGroupName westus-mykeyvault -Location "West US" -VaultName mywestusvault -CertificateName mycert -Password "<password>" -UseExistingCertificate -ExistingPfxFilePath "C:\path\to\mycertkey.pfx"

```

<span data-ttu-id="21ca4-182">Om du får ett felmeddelande, till exempel hello som visas här är oftast det att det finns en konflikt för resurs-URL.</span><span class="sxs-lookup"><span data-stu-id="21ca4-182">If you get an error, such as hello one shown here, it usually means that you have a resource URL conflict.</span></span> <span data-ttu-id="21ca4-183">tooresolve hello konflikt, ändra hello nyckelvalv namn.</span><span class="sxs-lookup"><span data-stu-id="21ca4-183">tooresolve hello conflict, change hello key vault name.</span></span>

```
Set-AzureKeyVaultSecret : hello remote name could not be resolved: 'westuskv.vault.azure.net'
At C:\Users\chackdan\Documents\GitHub\Service-Fabric\Scripts\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1:440 char:11
+ $secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $Certif ...
+           ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Set-AzureKeyVaultSecret], WebException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.KeyVault.SetAzureKeyVaultSecret

```

<span data-ttu-id="21ca4-184">När hello konflikten har lösts hello utdata ska se ut så här:</span><span class="sxs-lookup"><span data-stu-id="21ca4-184">After hello conflict is resolved, hello output should look like this:</span></span>

```

    Switching context tooSubscriptionId <guid>
    Ensuring ResourceGroup westus-mykeyvault in West US
    WARNING: hello output object type of this cmdlet is going toobe modified in a future release.
    Using existing value mywestusvault in West US
    Reading pfx file from C:\path\to\key.pfx
    Writing secret toomywestusvault in vault mywestusvault


Name  : CertificateThumbprint
Value : E21DBC64B183B5BF355C34C46E03409FEEAEF58D

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/westus-mykeyvault/providers/Microsoft.KeyVault/vaults/mywestusvault

Name  : CertificateURL
Value : https://mywestusvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47

```

>[!NOTE]
><span data-ttu-id="21ca4-185">Du måste hello tre föregående strängar, CertificateThumbprint och SourceVault CertificateURL tooset av säker Service Fabric-kluster och tooobtain programcertifikat som du kan använda för programsäkerhet.</span><span class="sxs-lookup"><span data-stu-id="21ca4-185">You need hello three preceding strings, CertificateThumbprint, SourceVault, and CertificateURL, tooset up a secure Service Fabric cluster and tooobtain any application certificates that you might be using for application security.</span></span> <span data-ttu-id="21ca4-186">Om du inte sparar hello strängar kan vara det svårt tooretrieve dem genom att fråga hello nyckeln valvet senare.</span><span class="sxs-lookup"><span data-stu-id="21ca4-186">If you do not save hello strings, it can be difficult tooretrieve them by querying hello key vault later.</span></span>

<a id="add-self-signed-certificate-to-key-vault"></a>

#### <a name="creating-a-self-signed-certificate-and-uploading-it-toohello-key-vault"></a><span data-ttu-id="21ca4-187">Skapa ett självsignerat certifikat och överföra den toohello nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="21ca4-187">Creating a self-signed certificate and uploading it toohello key vault</span></span>

<span data-ttu-id="21ca4-188">Hoppa över det här steget om du redan har överfört certifikat toohello nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="21ca4-188">If you have already uploaded your certificates toohello key vault, skip this step.</span></span> <span data-ttu-id="21ca4-189">Det här steget är för att generera ett nytt självsignerat certifikat och överföra den tooyour nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="21ca4-189">This step is for generating a new self-signed certificate and uploading it tooyour key vault.</span></span> <span data-ttu-id="21ca4-190">När du ändrar hello parametrar i hello följande skript och kör sedan det ska efterfrågas ett lösenord för certifikatet.</span><span class="sxs-lookup"><span data-stu-id="21ca4-190">After you change hello parameters in hello following script and then run it, you should be prompted for a certificate password.</span></span>  

```powershell

$ResourceGroup = "chackowestuskv"
$VName = "chackokv2"
$SubID = "6c653126-e4ba-42cd-a1dd-f7bf96ae7a47"
$locationRegion = "westus"
$newCertName = "chackotestcertificate1"
$dnsName = "www.mycluster.westus.mydomain.com" #hello certificate's subject name must match hello domain used tooaccess hello Service Fabric cluster.
$localCertPath = "C:\MyCertificates" # location where you want hello .PFX toobe stored

 Invoke-AddCertToKeyVault -SubscriptionId $SubID -ResourceGroupName $ResourceGroup -Location $locationRegion -VaultName $VName -CertificateName $newCertName -CreateSelfSignedCertificate -DnsName $dnsName -OutputPath $localCertPath

```

<span data-ttu-id="21ca4-191">Om du får ett felmeddelande, till exempel hello som visas här är oftast det att det finns en konflikt för resurs-URL.</span><span class="sxs-lookup"><span data-stu-id="21ca4-191">If you get an error, such as hello one shown here, it usually means that you have a resource URL conflict.</span></span> <span data-ttu-id="21ca4-192">tooresolve hello konflikt, ändra hello nyckelvalv namn, RG namn och så vidare.</span><span class="sxs-lookup"><span data-stu-id="21ca4-192">tooresolve hello conflict, change hello key vault name, RG name, and so forth.</span></span>

```
Set-AzureKeyVaultSecret : hello remote name could not be resolved: 'westuskv.vault.azure.net'
At C:\Users\chackdan\Documents\GitHub\Service-Fabric\Scripts\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1:440 char:11
+ $secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $Certif ...
+           ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Set-AzureKeyVaultSecret], WebException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.KeyVault.SetAzureKeyVaultSecret

```

<span data-ttu-id="21ca4-193">När hello konflikten har lösts hello utdata ska se ut så här:</span><span class="sxs-lookup"><span data-stu-id="21ca4-193">After hello conflict is resolved, hello output should look like this:</span></span>

```
PS C:\Users\chackdan\Documents\GitHub\Service-Fabric\Scripts\ServiceFabricRPHelpers> Invoke-AddCertToKeyVault -SubscriptionId $SubID -ResourceGroupName $ResouceGroup -Location $locationRegion -VaultName $VName -CertificateName $newCertName -Password $certPassword -CreateSelfSignedCertificate -DnsName $dnsName -OutputPath $localCertPath
Switching context tooSubscriptionId 6c343126-e4ba-52cd-a1dd-f8bf96ae7a47
Ensuring ResourceGroup chackowestuskv in westus
WARNING: hello output object type of this cmdlet will be modified in a future release.
Creating new vault westuskv1 in westus
Creating new self signed certificate at C:\MyCertificates\chackonewcertificate1.pfx
Reading pfx file from C:\MyCertificates\chackonewcertificate1.pfx
Writing secret toochackonewcertificate1 in vault westuskv1


Name  : CertificateThumbprint
Value : 96BB3CC234F9D43C25D4B547sd8DE7B569F413EE

Name  : SourceVault
Value : /subscriptions/6c653126-e4ba-52cd-a1dd-f8bf96ae7a47/resourceGroups/chackowestuskv/providers/Microsoft.KeyVault/vaults/westuskv1

Name  : CertificateURL
Value : https://westuskv1.vault.azure.net:443/secrets/chackonewcertificate1/ee247291e45d405b8c8bbf81782d12bd

```

>[!NOTE]
><span data-ttu-id="21ca4-194">Du måste hello tre föregående strängar, CertificateThumbprint och SourceVault CertificateURL tooset av säker Service Fabric-kluster och tooobtain programcertifikat som du kan använda för programsäkerhet.</span><span class="sxs-lookup"><span data-stu-id="21ca4-194">You need hello three preceding strings, CertificateThumbprint, SourceVault, and CertificateURL, tooset up a secure Service Fabric cluster and tooobtain any application certificates that you might be using for application security.</span></span> <span data-ttu-id="21ca4-195">Om du inte sparar hello strängar kan vara det svårt tooretrieve dem genom att fråga hello nyckeln valvet senare.</span><span class="sxs-lookup"><span data-stu-id="21ca4-195">If you do not save hello strings, it can be difficult tooretrieve them by querying hello key vault later.</span></span>

 <span data-ttu-id="21ca4-196">Du bör nu ha hello följande element på plats:</span><span class="sxs-lookup"><span data-stu-id="21ca4-196">At this point, you should have hello following elements in place:</span></span>

* <span data-ttu-id="21ca4-197">Hej nyckelvalv resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="21ca4-197">hello key vault resource group.</span></span>
* <span data-ttu-id="21ca4-198">Hej nyckelvalvet och Webbadressen (kallas SourceVault i hello föregående PowerShell-utdata).</span><span class="sxs-lookup"><span data-stu-id="21ca4-198">hello key vault and its URL (called SourceVault in hello preceding PowerShell output).</span></span>
* <span data-ttu-id="21ca4-199">hello klustret certifikat för serverautentisering och URL: en i hello nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="21ca4-199">hello cluster server authentication certificate and its URL in hello key vault.</span></span>
* <span data-ttu-id="21ca4-200">hello programmet certifikat och deras URL: er i hello nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="21ca4-200">hello application certificates and their URLs in hello key vault.</span></span>


<a id="add-AAD-for-client"></a>

## <a name="set-up-azure-active-directory-for-client-authentication"></a><span data-ttu-id="21ca4-201">Konfigurera Azure Active Directory för klientautentisering</span><span class="sxs-lookup"><span data-stu-id="21ca4-201">Set up Azure Active Directory for client authentication</span></span>

<span data-ttu-id="21ca4-202">Azure AD kan organisationer (kallas klienter) toomanage användaren åtkomst tooapplications.</span><span class="sxs-lookup"><span data-stu-id="21ca4-202">Azure AD enables organizations (known as tenants) toomanage user access tooapplications.</span></span> <span data-ttu-id="21ca4-203">Program är indelade i dem med en webbaserad inloggning användargränssnitt och de med inbyggda klientupplevelsen.</span><span class="sxs-lookup"><span data-stu-id="21ca4-203">Applications are divided into those with a web-based sign-in UI and those with a native client experience.</span></span> <span data-ttu-id="21ca4-204">I den här artikeln förutsätter vi att du redan har skapat en klient.</span><span class="sxs-lookup"><span data-stu-id="21ca4-204">In this article, we assume that you have already created a tenant.</span></span> <span data-ttu-id="21ca4-205">Om du inte har börja med att läsa [hur tooget ett Azure Active Directory-klient][active-directory-howto-tenant].</span><span class="sxs-lookup"><span data-stu-id="21ca4-205">If you have not, start by reading [How tooget an Azure Active Directory tenant][active-directory-howto-tenant].</span></span>

<span data-ttu-id="21ca4-206">Ett Service Fabric-kluster erbjuder flera posten pekar tooits hanteringsfunktioner, inklusive hello webbaserade [Service Fabric Explorer] [ service-fabric-visualizing-your-cluster] och [Visual Studio] [service-fabric-manage-application-in-visual-studio].</span><span class="sxs-lookup"><span data-stu-id="21ca4-206">A Service Fabric cluster offers several entry points tooits management functionality, including hello web-based [Service Fabric Explorer][service-fabric-visualizing-your-cluster] and [Visual Studio][service-fabric-manage-application-in-visual-studio].</span></span> <span data-ttu-id="21ca4-207">Därför kan skapa du två Azure AD-program toocontrol åtkomst toohello klustret, ett webbprogram och en programspecifika.</span><span class="sxs-lookup"><span data-stu-id="21ca4-207">As a result, you create two Azure AD applications toocontrol access toohello cluster, one web application and one native application.</span></span>

<span data-ttu-id="21ca4-208">toosimplify några av hello stegen som är involverad i Konfigurera Azure AD med ett Service Fabric-kluster, vi har skapat en uppsättning Windows PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="21ca4-208">toosimplify some of hello steps involved in configuring Azure AD with a Service Fabric cluster, we have created a set of Windows PowerShell scripts.</span></span>

> [!NOTE]
> <span data-ttu-id="21ca4-209">Du måste slutföra följande steg innan du skapar klustret hello hello.</span><span class="sxs-lookup"><span data-stu-id="21ca4-209">You must complete hello following steps before you create hello cluster.</span></span> <span data-ttu-id="21ca4-210">Eftersom hello skript räknar klusternamn och slutpunkter, bör planeras hello värden och värden inte som du redan har skapat.</span><span class="sxs-lookup"><span data-stu-id="21ca4-210">Because hello scripts expect cluster names and endpoints, hello values should be planned and not values that you have already created.</span></span>

1. <span data-ttu-id="21ca4-211">[Hämta hello skript] [ sf-aad-ps-script-download] tooyour dator.</span><span class="sxs-lookup"><span data-stu-id="21ca4-211">[Download hello scripts][sf-aad-ps-script-download] tooyour computer.</span></span>
2. <span data-ttu-id="21ca4-212">Högerklicka på hello zip-filen, Välj **egenskaper**väljer hello **avblockera** kryssrutan och klicka sedan på **tillämpa**.</span><span class="sxs-lookup"><span data-stu-id="21ca4-212">Right-click hello zip file, select **Properties**, select hello **Unblock** check box, and then click **Apply**.</span></span>
3. <span data-ttu-id="21ca4-213">Extrahera hello zip-filen.</span><span class="sxs-lookup"><span data-stu-id="21ca4-213">Extract hello zip file.</span></span>
4. <span data-ttu-id="21ca4-214">Kör `SetupApplications.ps1`, och ange hello TenantId, klusternamn och WebApplicationReplyUrl som parametrar.</span><span class="sxs-lookup"><span data-stu-id="21ca4-214">Run `SetupApplications.ps1`, and provide hello TenantId, ClusterName, and WebApplicationReplyUrl as parameters.</span></span> <span data-ttu-id="21ca4-215">Exempel:</span><span class="sxs-lookup"><span data-stu-id="21ca4-215">For example:</span></span>

    ```powershell
    .\SetupApplications.ps1 -TenantId '690ec069-8200-4068-9d01-5aaf188e557a' -ClusterName 'mycluster' -WebApplicationReplyUrl 'https://mycluster.westus.cloudapp.azure.com:19080/Explorer/index.html'
    ```

    <span data-ttu-id="21ca4-216">Du kan hitta din TenantId genom att köra PowerShell-kommando för hello `Get-AzureSubscription`.</span><span class="sxs-lookup"><span data-stu-id="21ca4-216">You can find your TenantId by executing hello PowerShell command `Get-AzureSubscription`.</span></span> <span data-ttu-id="21ca4-217">Kör det här kommandot visar hello TenantId för varje prenumeration.</span><span class="sxs-lookup"><span data-stu-id="21ca4-217">Executing this command displays hello TenantId for every subscription.</span></span>

    <span data-ttu-id="21ca4-218">Klusternamn är används tooprefix hello Azure AD-program som skapats av hello-skriptet.</span><span class="sxs-lookup"><span data-stu-id="21ca4-218">ClusterName is used tooprefix hello Azure AD applications that are created by hello script.</span></span> <span data-ttu-id="21ca4-219">Det behöver inte toomatch hello faktiska klusternamnet exakt.</span><span class="sxs-lookup"><span data-stu-id="21ca4-219">It does not need toomatch hello actual cluster name exactly.</span></span> <span data-ttu-id="21ca4-220">Det är avsedda enbart toomake den enklare toomap Azure AD-artefakter toohello Service Fabric-kluster som de som används med.</span><span class="sxs-lookup"><span data-stu-id="21ca4-220">It is intended only toomake it easier toomap Azure AD artifacts toohello Service Fabric cluster that they're being used with.</span></span>

    <span data-ttu-id="21ca4-221">WebApplicationReplyUrl är hello standardslutpunkt som Azure AD returnerar tooyour användare när de har loggat in.</span><span class="sxs-lookup"><span data-stu-id="21ca4-221">WebApplicationReplyUrl is hello default endpoint that Azure AD returns tooyour users after they finish signing in.</span></span> <span data-ttu-id="21ca4-222">Ange den här slutpunkten som hello Service Fabric Explorer slutpunkt för klustret, vilket som standard är:</span><span class="sxs-lookup"><span data-stu-id="21ca4-222">Set this endpoint as hello Service Fabric Explorer endpoint for your cluster, which by default is:</span></span>

    <span data-ttu-id="21ca4-223">https://&lt;cluster_domain&gt;: 19080/Explorer</span><span class="sxs-lookup"><span data-stu-id="21ca4-223">https://&lt;cluster_domain&gt;:19080/Explorer</span></span>

    <span data-ttu-id="21ca4-224">Du kan ange toosign i tooan konto som har administratörsbehörighet för hello Azure AD-klient.</span><span class="sxs-lookup"><span data-stu-id="21ca4-224">You are prompted toosign in tooan account that has administrative privileges for hello Azure AD tenant.</span></span> <span data-ttu-id="21ca4-225">När du loggar in hello skriptet skapar hello webb- och interna program toorepresent Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="21ca4-225">After you sign in, hello script creates hello web and native applications toorepresent your Service Fabric cluster.</span></span> <span data-ttu-id="21ca4-226">Om du tittar på hello klient program i hello [klassiska Azure-portalen][azure-classic-portal], bör du se två nya poster:</span><span class="sxs-lookup"><span data-stu-id="21ca4-226">If you look at hello tenant's applications in hello [Azure classic portal][azure-classic-portal], you should see two new entries:</span></span>

   * <span data-ttu-id="21ca4-227">*Klusternamn*\_kluster</span><span class="sxs-lookup"><span data-stu-id="21ca4-227">*ClusterName*\_Cluster</span></span>
   * <span data-ttu-id="21ca4-228">*Klusternamn*\_klienten</span><span class="sxs-lookup"><span data-stu-id="21ca4-228">*ClusterName*\_Client</span></span>

   <span data-ttu-id="21ca4-229">hello skript skrivs hello JSON som krävs av hello Azure Resource Manager-mall när du skapar hello kluster i hello nästa avsnitt, så det är en bra idé tookeep hello PowerShell-fönstret öppnas.</span><span class="sxs-lookup"><span data-stu-id="21ca4-229">hello script prints hello JSON required by hello Azure Resource Manager template when you create hello cluster in hello next section, so it's a good idea tookeep hello PowerShell window open.</span></span>

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

## <a name="create-a-service-fabric-cluster-resource-manager-template"></a><span data-ttu-id="21ca4-230">Skapa en mall för Service Fabric-kluster Resource Manager</span><span class="sxs-lookup"><span data-stu-id="21ca4-230">Create a Service Fabric cluster Resource Manager template</span></span>
<span data-ttu-id="21ca4-231">I det här avsnittet hello matar ut av hello föregående PowerShell-kommandon som används i en Service Fabric-kluster Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="21ca4-231">In this section, hello outputs of hello preceding PowerShell commands are used in a Service Fabric cluster Resource Manager template.</span></span>

<span data-ttu-id="21ca4-232">Exempel Resource Manager-mallar är tillgängliga i hello [Azure Snabbkurs mallgalleriet på GitHub][azure-quickstart-templates].</span><span class="sxs-lookup"><span data-stu-id="21ca4-232">Sample Resource Manager templates are available in hello [Azure quick-start template gallery on GitHub][azure-quickstart-templates].</span></span> <span data-ttu-id="21ca4-233">Dessa mallar kan användas som en startpunkt för mallen för klustret.</span><span class="sxs-lookup"><span data-stu-id="21ca4-233">These templates can be used as a starting point for your cluster template.</span></span>

### <a name="create-hello-resource-manager-template"></a><span data-ttu-id="21ca4-234">Skapa hello Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="21ca4-234">Create hello Resource Manager template</span></span>
<span data-ttu-id="21ca4-235">Den här guiden använder hello [säker kluster med 5] [ service-fabric-secure-cluster-5-node-1-nodetype] exempel mallen och mallparametrar.</span><span class="sxs-lookup"><span data-stu-id="21ca4-235">This guide uses hello [5-node secure cluster][service-fabric-secure-cluster-5-node-1-nodetype] example template and template parameters.</span></span> <span data-ttu-id="21ca4-236">Hämta `azuredeploy.json` och `azuredeploy.parameters.json` tooyour datorn och öppna filer i valfri textredigerare.</span><span class="sxs-lookup"><span data-stu-id="21ca4-236">Download `azuredeploy.json` and `azuredeploy.parameters.json` tooyour computer and open both files in your favorite text editor.</span></span>

### <a name="add-certificates"></a><span data-ttu-id="21ca4-237">Lägg till certifikat</span><span class="sxs-lookup"><span data-stu-id="21ca4-237">Add certificates</span></span>
<span data-ttu-id="21ca4-238">Du kan lägga till certifikat tooa klustret Resource Manager-mall genom att referera hello nyckelvalv som innehåller hello certifikatnycklar.</span><span class="sxs-lookup"><span data-stu-id="21ca4-238">You add certificates tooa cluster Resource Manager template by referencing hello key vault that contains hello certificate keys.</span></span> <span data-ttu-id="21ca4-239">Vi rekommenderar att du placerar hello nyckelvalvet värden i en parameterfil för Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="21ca4-239">We recommend that you place hello key-vault values in a Resource Manager template parameters file.</span></span> <span data-ttu-id="21ca4-240">Då behåller hello Resource Manager mallfilen återanvändbara och utan värden specifika tooa distribution.</span><span class="sxs-lookup"><span data-stu-id="21ca4-240">Doing so keeps hello Resource Manager template file reusable and free of values specific tooa deployment.</span></span>

#### <a name="add-all-certificates-toohello-virtual-machine-scale-set-osprofile"></a><span data-ttu-id="21ca4-241">Lägg till alla certifikat toohello virtuella skaluppsättning osProfile</span><span class="sxs-lookup"><span data-stu-id="21ca4-241">Add all certificates toohello virtual machine scale set osProfile</span></span>
<span data-ttu-id="21ca4-242">Alla certifikat som har installerats i hello klustret måste konfigureras under hello osProfile i hello scale set resurs (Microsoft.Compute/virtualMachineScaleSets).</span><span class="sxs-lookup"><span data-stu-id="21ca4-242">Every certificate that's installed in hello cluster must be configured in hello osProfile section of hello scale set resource (Microsoft.Compute/virtualMachineScaleSets).</span></span> <span data-ttu-id="21ca4-243">Den här åtgärden instruerar hello resource provider tooinstall hello certifikatet på hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="21ca4-243">This action instructs hello resource provider tooinstall hello certificate on hello VMs.</span></span> <span data-ttu-id="21ca4-244">Den här installationen innehåller både hello klustret certifikat och några security-certifikat för programmet som du planerar toouse för dina program:</span><span class="sxs-lookup"><span data-stu-id="21ca4-244">This installation includes both hello cluster certificate and any application security certificates that you plan toouse for your applications:</span></span>

```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "osProfile": {
      ...
      "secrets": [
        {
          "sourceVault": {
            "id": "[parameters('sourceVaultValue')]"
          },
          "vaultCertificates": [
            {
              "certificateStore": "[parameters('clusterCertificateStorevalue')]",
              "certificateUrl": "[parameters('clusterCertificateUrlValue')]"
            },
            {
              "certificateStore": "[parameters('applicationCertificateStorevalue')",
              "certificateUrl": "[parameters('applicationCertificateUrlValue')]"
            },
            ...
          ]
        }
      ]
    }
  }
}
```

#### <a name="configure-hello-service-fabric-cluster-certificate"></a><span data-ttu-id="21ca4-245">Konfigurera certifikat för hello Service Fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="21ca4-245">Configure hello Service Fabric cluster certificate</span></span>
<span data-ttu-id="21ca4-246">hello autentiseringscertifikat för klustret måste konfigureras i båda hello Service Fabric-klusterresursen (Microsoft.ServiceFabric/clusters) och hello Service Fabric-tillägget för virtuell dator skala anger i hello virtuella scale set datorresurser.</span><span class="sxs-lookup"><span data-stu-id="21ca4-246">hello cluster authentication certificate must be configured in both hello Service Fabric cluster resource (Microsoft.ServiceFabric/clusters) and hello Service Fabric extension for virtual machine scale sets in hello virtual machine scale set resource.</span></span> <span data-ttu-id="21ca4-247">Den här ordningen tillåter hello Service Fabric resource provider tooconfigure den som ska användas för autentisering för klustret och serverautentisering för av hanteringsslutpunkter.</span><span class="sxs-lookup"><span data-stu-id="21ca4-247">This arrangement allows hello Service Fabric resource provider tooconfigure it for use for cluster authentication and server authentication for management endpoints.</span></span>

##### <a name="virtual-machine-scale-set-resource"></a><span data-ttu-id="21ca4-248">Virtuella skaluppsättning för resursen:</span><span class="sxs-lookup"><span data-stu-id="21ca4-248">Virtual machine scale set resource:</span></span>
```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "virtualMachineProfile": {
      "extensionProfile": {
        "extensions": [
          {
            "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
            "properties": {
              ...
              "settings": {
                ...
                "certificate": {
                  "thumbprint": "[parameters('clusterCertificateThumbprint')]",
                  "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
                },
                ...
              }
            }
          }
        ]
      }
    }
  }
}
```

##### <a name="service-fabric-resource"></a><span data-ttu-id="21ca4-249">Service Fabric-resurs:</span><span class="sxs-lookup"><span data-stu-id="21ca4-249">Service Fabric resource:</span></span>
```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  "location": "[parameters('clusterLocation')]",
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('supportLogStorageAccountName'))]"
  ],
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
    },
    ...
  }
}
```

### <a name="insert-azure-ad-configuration"></a><span data-ttu-id="21ca4-250">Infoga Azure AD-konfiguration</span><span class="sxs-lookup"><span data-stu-id="21ca4-250">Insert Azure AD configuration</span></span>
<span data-ttu-id="21ca4-251">hello Azure AD-konfiguration som du skapade tidigare kan infogas direkt i Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="21ca4-251">hello Azure AD configuration that you created earlier can be inserted directly into your Resource Manager template.</span></span> <span data-ttu-id="21ca4-252">Vi rekommenderar dock att du först extrahera hello värden till en parametrar filen tookeep hello Resource Manager-mall kan återanvändas och utan värden specifika tooa distribution.</span><span class="sxs-lookup"><span data-stu-id="21ca4-252">However, we recommended that you first extract hello values into a parameters file tookeep hello Resource Manager template reusable and free of values specific tooa deployment.</span></span>

```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  ...
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStorevalue')]"
    },
    ...
    "azureActiveDirectory": {
      "tenantId": "[parameters('aadTenantId')]",
      "clusterApplication": "[parameters('aadClusterApplicationId')]",
      "clientApplication": "[parameters('aadClientApplicationId')]"
    },
    ...
  }
}
```

### <span data-ttu-id="21ca4-253">< en ”konfigurera arm” ></a>konfigurera Hanteraren för filserverresurser mallparametrar</span><span class="sxs-lookup"><span data-stu-id="21ca4-253"><a "configure-arm" ></a>Configure Resource Manager template parameters</span></span>
<!--- Loc Comment: It seems that <a "configure-arm" > must be replaced with <a name="configure-arm"></a> since hello link seems not toobe redirecting correctly --->
<span data-ttu-id="21ca4-254">Använd slutligen hello utdatavärden från hello nyckelvalvet och Azure AD PowerShell-kommandon toopopulate hello parameterfilen:</span><span class="sxs-lookup"><span data-stu-id="21ca4-254">Finally, use hello output values from hello key vault and Azure AD PowerShell commands toopopulate hello parameters file:</span></span>

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        ...
        "clusterCertificateStoreValue": {
            "value": "My"
        },
        "clusterCertificateThumbprint": {
            "value": "<thumbprint>"
        },
        "clusterCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myclustercert/4d087088df974e869f1c0978cb100e47"
        },
        "applicationCertificateStorevalue": {
            "value": "My"
        },
        "applicationCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myapplicationcert/2e035058ae274f869c4d0348ca100f08"
        },
        "sourceVaultvalue": {
            "value": "/subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault"
        },
        "aadTenantId": {
            "value": "<guid>"
        },
        "aadClusterApplicationId": {
            "value": "<guid>"
        },
        "aadClientApplicationId": {
            "value": "<guid>"
        },
        ...
    }
}
```
<span data-ttu-id="21ca4-255">Du bör nu ha hello följande element på plats:</span><span class="sxs-lookup"><span data-stu-id="21ca4-255">At this point, you should have hello following elements in place:</span></span>

* <span data-ttu-id="21ca4-256">Nyckelvalv resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="21ca4-256">Key vault resource group</span></span>
  * <span data-ttu-id="21ca4-257">Nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="21ca4-257">Key vault</span></span>
  * <span data-ttu-id="21ca4-258">Klustret certifikat för serverautentisering</span><span class="sxs-lookup"><span data-stu-id="21ca4-258">Cluster server authentication certificate</span></span>
  * <span data-ttu-id="21ca4-259">Data chiffrering av certifikat</span><span class="sxs-lookup"><span data-stu-id="21ca4-259">Data encipherment certificate</span></span>
* <span data-ttu-id="21ca4-260">Azure Active Directory-klient</span><span class="sxs-lookup"><span data-stu-id="21ca4-260">Azure Active Directory tenant</span></span>
  * <span data-ttu-id="21ca4-261">Azure AD-program för Webbaserad hantering och Service Fabric Explorer</span><span class="sxs-lookup"><span data-stu-id="21ca4-261">Azure AD application for web-based management and Service Fabric Explorer</span></span>
  * <span data-ttu-id="21ca4-262">Azure AD-program för interna klienthantering</span><span class="sxs-lookup"><span data-stu-id="21ca4-262">Azure AD application for native client management</span></span>
  * <span data-ttu-id="21ca4-263">Användare och deras tilldelade roller</span><span class="sxs-lookup"><span data-stu-id="21ca4-263">Users and their assigned roles</span></span>
* <span data-ttu-id="21ca4-264">Service Fabric-kluster Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="21ca4-264">Service Fabric cluster Resource Manager template</span></span>
  * <span data-ttu-id="21ca4-265">Certifikat konfigureras via nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="21ca4-265">Certificates configured through key vault</span></span>
  * <span data-ttu-id="21ca4-266">Azure Active Directory konfigurerat</span><span class="sxs-lookup"><span data-stu-id="21ca4-266">Azure Active Directory configured</span></span>

<span data-ttu-id="21ca4-267">hello följande diagram illustrerar där ditt nyckelvalv och Azure AD-konfiguration passar i Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="21ca4-267">hello following diagram illustrates where your key vault and Azure AD configuration fit into your Resource Manager template.</span></span>

![Hanteraren för filserverresurser beroendekarta för anslutningar][cluster-security-arm-dependency-map]

## <a name="create-hello-cluster"></a><span data-ttu-id="21ca4-269">Skapa hello-kluster</span><span class="sxs-lookup"><span data-stu-id="21ca4-269">Create hello cluster</span></span>
<span data-ttu-id="21ca4-270">Nu är du redo toocreate hello kluster med hjälp av [Azure-resurs malldistribution][resource-group-template-deploy].</span><span class="sxs-lookup"><span data-stu-id="21ca4-270">You are now ready toocreate hello cluster by using [Azure resource template deployment][resource-group-template-deploy].</span></span>

#### <a name="test-it"></a><span data-ttu-id="21ca4-271">testa den</span><span class="sxs-lookup"><span data-stu-id="21ca4-271">Test it</span></span>
<span data-ttu-id="21ca4-272">Använd följande PowerShell-kommandot tootest hello Resource Manager-mall med en fil med parametrar:</span><span class="sxs-lookup"><span data-stu-id="21ca4-272">Use hello following PowerShell command tootest your Resource Manager template with a parameters file:</span></span>

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

#### <a name="deploy-it"></a><span data-ttu-id="21ca4-273">distribuera den</span><span class="sxs-lookup"><span data-stu-id="21ca4-273">Deploy it</span></span>
<span data-ttu-id="21ca4-274">Om hello Resource Manager-mall testet lyckas, använder du följande PowerShell-kommandot toodeploy hello Resource Manager-mall med en fil med parametrar:</span><span class="sxs-lookup"><span data-stu-id="21ca4-274">If hello Resource Manager template test passes, use hello following PowerShell command toodeploy your Resource Manager template with a parameters file:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

<a name="assign-roles"></a>

## <a name="assign-users-tooroles"></a><span data-ttu-id="21ca4-275">Tilldela användare tooroles</span><span class="sxs-lookup"><span data-stu-id="21ca4-275">Assign users tooroles</span></span>
<span data-ttu-id="21ca4-276">När du har skapat hello program toorepresent klustret, tilldela dina användare toohello roller som stöds av Service Fabric: skrivskyddade och administratör. Du kan tilldela hello roller med hjälp av hello [klassiska Azure-portalen][azure-classic-portal].</span><span class="sxs-lookup"><span data-stu-id="21ca4-276">After you have created hello applications toorepresent your cluster, assign your users toohello roles supported by Service Fabric: read-only and admin. You can assign hello roles by using hello [Azure classic portal][azure-classic-portal].</span></span>

1. <span data-ttu-id="21ca4-277">Gå tooyour klient i hello Azure-portalen, och välj sedan **program**.</span><span class="sxs-lookup"><span data-stu-id="21ca4-277">In hello Azure portal, go tooyour tenant, and then select **Applications**.</span></span>
2. <span data-ttu-id="21ca4-278">Välj hello webbprogram som har ett namn som `myTestCluster_Cluster`.</span><span class="sxs-lookup"><span data-stu-id="21ca4-278">Select hello web application, which has a name like `myTestCluster_Cluster`.</span></span>
3. <span data-ttu-id="21ca4-279">Klicka på hello **användare** fliken.</span><span class="sxs-lookup"><span data-stu-id="21ca4-279">Click hello **Users** tab.</span></span>
4. <span data-ttu-id="21ca4-280">Välj en användare tooassign och klicka sedan på hello **tilldela** knappen längst ned hello hello-skärmen.</span><span class="sxs-lookup"><span data-stu-id="21ca4-280">Select a user tooassign, and then click hello **Assign** button at hello bottom of hello screen.</span></span>

    ![Tilldela användare tooroles knappen][assign-users-to-roles-button]
5. <span data-ttu-id="21ca4-282">Välj hello rollen tooassign toohello användare.</span><span class="sxs-lookup"><span data-stu-id="21ca4-282">Select hello role tooassign toohello user.</span></span>

    ![”Tilldela användare” dialogrutan][assign-users-to-roles-dialog]

> [!NOTE]
> <span data-ttu-id="21ca4-284">Mer information om roller i Service Fabric finns [rollbaserad åtkomstkontroll för Service Fabric-klienter](service-fabric-cluster-security-roles.md).</span><span class="sxs-lookup"><span data-stu-id="21ca4-284">For more information about roles in Service Fabric, see [Role-based access control for Service Fabric clients](service-fabric-cluster-security-roles.md).</span></span>
>
>

 <a name="secure-linux-clusters"></a>
 <!--- Loc Comment: It seems that letter S in cluster was missing, which caused hello wrong redirection of hello link --->

## <a name="create-secure-clusters-on-linux"></a><span data-ttu-id="21ca4-285">Skapa skyddade kluster på Linux</span><span class="sxs-lookup"><span data-stu-id="21ca4-285">Create secure clusters on Linux</span></span>
<span data-ttu-id="21ca4-286">toomake hello processen enklare, som vi har angett en [helper skriptet](http://github.com/ChackDan/Service-Fabric/tree/master/Scripts/CertUpload4Linux).</span><span class="sxs-lookup"><span data-stu-id="21ca4-286">toomake hello process easier, we have provided a [helper script](http://github.com/ChackDan/Service-Fabric/tree/master/Scripts/CertUpload4Linux).</span></span> <span data-ttu-id="21ca4-287">Innan du använder skriptet helper se till att du redan har Azure-kommandoradsgränssnittet (CLI) installerad och den är i din sökväg.</span><span class="sxs-lookup"><span data-stu-id="21ca4-287">Before you use this helper script, ensure that you already have Azure command-line interface (CLI) installed, and it is in your path.</span></span> <span data-ttu-id="21ca4-288">Se till att hello skript har behörigheter tooexecute genom att köra `chmod +x cert_helper.py` när du har hämtat den.</span><span class="sxs-lookup"><span data-stu-id="21ca4-288">Make sure that hello script has permissions tooexecute by running `chmod +x cert_helper.py` after downloading it.</span></span> <span data-ttu-id="21ca4-289">hello första steget är toosign i tooyour Azure-konto med hjälp av CLI med hello `azure login` kommando.</span><span class="sxs-lookup"><span data-stu-id="21ca4-289">hello first step is toosign in tooyour Azure account by using CLI with hello `azure login` command.</span></span> <span data-ttu-id="21ca4-290">När du har registrerat i tooyour Azure-konto, signerade Använd hello helper skript med Certifikatutfärdaren certifikat, som följande kommando visar hello:</span><span class="sxs-lookup"><span data-stu-id="21ca4-290">After signing in tooyour Azure account, use hello helper script with your CA signed certificate, as hello following command shows:</span></span>

```sh
./cert_helper.py [-h] CERT_TYPE [-ifile INPUT_CERT_FILE] [-sub SUBSCRIPTION_ID] [-rgname RESOURCE_GROUP_NAME] [-kv KEY_VAULT_NAME] [-sname CERTIFICATE_NAME] [-l LOCATION] [-p PASSWORD]
```

<span data-ttu-id="21ca4-291">hello - ifile parameter kan ta en .pfx-fil eller en PEM-filen som indata med hello certifikattyp (pfx eller pem eller ss om det är ett självsignerat certifikat).</span><span class="sxs-lookup"><span data-stu-id="21ca4-291">hello -ifile parameter can take a .pfx file or a .pem file as input, with hello certificate type (pfx or pem, or ss if it is a self-signed certificate).</span></span>
<span data-ttu-id="21ca4-292">hello parametern -h skriver ut hello hjälptexten.</span><span class="sxs-lookup"><span data-stu-id="21ca4-292">hello parameter -h prints out hello help text.</span></span>


<span data-ttu-id="21ca4-293">Det här kommandot returnerar hello följande tre strängar som hello utdata:</span><span class="sxs-lookup"><span data-stu-id="21ca4-293">This command returns hello following three strings as hello output:</span></span>

* <span data-ttu-id="21ca4-294">SourceVaultID som är hello-ID för hello nya KeyVault ResourceGroup den skapas automatiskt</span><span class="sxs-lookup"><span data-stu-id="21ca4-294">SourceVaultID, which is hello ID for hello new KeyVault ResourceGroup it created for you</span></span>
* <span data-ttu-id="21ca4-295">CertificateUrl för att komma åt hello certifikat</span><span class="sxs-lookup"><span data-stu-id="21ca4-295">CertificateUrl for accessing hello certificate</span></span>
* <span data-ttu-id="21ca4-296">CertificateThumbprint som används för autentisering</span><span class="sxs-lookup"><span data-stu-id="21ca4-296">CertificateThumbprint, which is used for authentication</span></span>

<span data-ttu-id="21ca4-297">hello som följande exempel visar hur toouse hello kommando:</span><span class="sxs-lookup"><span data-stu-id="21ca4-297">hello following example shows how toouse hello command:</span></span>

```sh
./cert_helper.py pfx -sub "fffffff-ffff-ffff-ffff-ffffffffffff"  -rgname "mykvrg" -kv "mykevname" -ifile "/home/test/cert.pfx" -sname "mycert" -l "East US" -p "pfxtest"
```
<span data-ttu-id="21ca4-298">Köra hello före kommandot ger du hello tre strängar på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="21ca4-298">Executing hello preceding command gives you hello three strings as follows:</span></span>

```sh
SourceVault: /subscriptions/fffffff-ffff-ffff-ffff-ffffffffffff/resourceGroups/mykvrg/providers/Microsoft.KeyVault/vaults/mykvname
CertificateUrl: https://myvault.vault.azure.net/secrets/mycert/00000000000000000000000000000000
CertificateThumbprint: 0xfffffffffffffffffffffffffffffffffffffffff
```

<span data-ttu-id="21ca4-299">hello certifikatets ämnesnamn måste matcha hello-domän som du använder tooaccess hello Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="21ca4-299">hello certificate's subject name must match hello domain that you use tooaccess hello Service Fabric cluster.</span></span> <span data-ttu-id="21ca4-300">Matchningen är obligatoriska tooprovide en SSL för hello klustrets HTTPS management slutpunkter och Service Fabric Explorer.</span><span class="sxs-lookup"><span data-stu-id="21ca4-300">This match is required tooprovide an SSL for hello cluster's HTTPS management endpoints and Service Fabric Explorer.</span></span> <span data-ttu-id="21ca4-301">Du kan skaffa ett SSL-certifikat från en Certifikatutfärdare för hello `.cloudapp.azure.com` domän.</span><span class="sxs-lookup"><span data-stu-id="21ca4-301">You cannot obtain an SSL certificate from a CA for hello `.cloudapp.azure.com` domain.</span></span> <span data-ttu-id="21ca4-302">Du måste skaffa ett anpassat domännamn för klustret.</span><span class="sxs-lookup"><span data-stu-id="21ca4-302">You must obtain a custom domain name for your cluster.</span></span> <span data-ttu-id="21ca4-303">När du begär ett certifikat från en Certifikatutfärdare måste hello certifikatets ämnesnamn matcha hello domännamn som du använder för klustret.</span><span class="sxs-lookup"><span data-stu-id="21ca4-303">When you request a certificate from a CA, hello certificate's subject name must match hello custom domain name that you use for your cluster.</span></span>

<span data-ttu-id="21ca4-304">Dessa ämnesnamn är hello poster du behöver toocreate säker Service Fabric-klustret (utan Azure AD), enligt beskrivningen i [konfigurera Hanteraren för filserverresurser mallparametrar](#configure-arm).</span><span class="sxs-lookup"><span data-stu-id="21ca4-304">These subject names are hello entries you need toocreate a secure Service Fabric cluster (without Azure AD), as described at [Configure Resource Manager template parameters](#configure-arm).</span></span> <span data-ttu-id="21ca4-305">Du kan ansluta toohello säker klustret genom att följa hello anvisningar för [autentisera klient åtkomst tooa kluster](service-fabric-connect-to-secure-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="21ca4-305">You can connect toohello secure cluster by following hello instructions for [authenticating client access tooa cluster](service-fabric-connect-to-secure-cluster.md).</span></span> <span data-ttu-id="21ca4-306">Linux preview kluster stöder inte Azure AD-autentisering.</span><span class="sxs-lookup"><span data-stu-id="21ca4-306">Linux preview clusters do not support Azure AD authentication.</span></span> <span data-ttu-id="21ca4-307">Du kan tilldela roller admin och klienten enligt beskrivningen i hello [tilldela roller toousers](#assign-roles) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="21ca4-307">You can assign admin and client roles as described in hello [Assign roles toousers](#assign-roles) section.</span></span> <span data-ttu-id="21ca4-308">När du anger admin och klienten roller för en Linux-kluster i förhandsgranskningen har tooprovide certifikattumavtryck för autentisering.</span><span class="sxs-lookup"><span data-stu-id="21ca4-308">When you specify admin and client roles for a Linux preview cluster, you have tooprovide certificate thumbprints for authentication.</span></span> <span data-ttu-id="21ca4-309">(Du inte anger hello ämnesnamn eftersom ingen verifiering av certifikatkedjan eller återkallade utförs i den här förhandsversionen.)</span><span class="sxs-lookup"><span data-stu-id="21ca4-309">(You do not provide hello subject name, because no chain validation or revocation is being performed in this preview release.)</span></span>

<span data-ttu-id="21ca4-310">Om du vill toouse ett självsignerat certifikat för testning, kan du använda hello samma skript toogenerate en.</span><span class="sxs-lookup"><span data-stu-id="21ca4-310">If you want toouse a self-signed certificate for testing, you can use hello same script toogenerate one.</span></span> <span data-ttu-id="21ca4-311">Du kan sedan ladda upp certifikatet hello tooyour nyckelvalv genom att tillhandahålla hello flaggan `ss` i stället för att tillhandahålla hello certifikatets sökväg och certifikatet namn.</span><span class="sxs-lookup"><span data-stu-id="21ca4-311">You can then upload hello certificate tooyour key vault by providing hello flag `ss` instead of providing hello certificate path and certificate name.</span></span> <span data-ttu-id="21ca4-312">Se exempelvis hello följande kommando för att skapa och ladda upp ett självsignerat certifikat:</span><span class="sxs-lookup"><span data-stu-id="21ca4-312">For example, see hello following command for creating and uploading a self-signed certificate:</span></span>

```sh
./cert_helper.py ss -rgname "mykvrg" -sub "fffffff-ffff-ffff-ffff-ffffffffffff" -kv "mykevname"   -sname "mycert" -l "East US" -p "selftest" -subj "mytest.eastus.cloudapp.net"
```
<span data-ttu-id="21ca4-313">Det här kommandot returnerar hello samma tre strängar: SourceVault, CertificateUrl och CertificateThumbprint.</span><span class="sxs-lookup"><span data-stu-id="21ca4-313">This command returns hello same three strings: SourceVault, CertificateUrl, and CertificateThumbprint.</span></span> <span data-ttu-id="21ca4-314">Du kan sedan använda hello strängar toocreate både en säker Linux-kluster och en plats där hello självsignerat certifikat är placerad.</span><span class="sxs-lookup"><span data-stu-id="21ca4-314">You can then use hello strings toocreate both a secure Linux cluster and a location where hello self-signed certificate is placed.</span></span> <span data-ttu-id="21ca4-315">Du måste hello självsignerat certifikat tooconnect toohello klustret.</span><span class="sxs-lookup"><span data-stu-id="21ca4-315">You need hello self-signed certificate tooconnect toohello cluster.</span></span> <span data-ttu-id="21ca4-316">Du kan ansluta toohello säker klustret genom att följa hello anvisningar för [autentisera klient åtkomst tooa kluster](service-fabric-connect-to-secure-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="21ca4-316">You can connect toohello secure cluster by following hello instructions for [authenticating client access tooa cluster](service-fabric-connect-to-secure-cluster.md).</span></span>

<span data-ttu-id="21ca4-317">hello certifikatets ämnesnamn måste matcha hello-domän som du använder tooaccess hello Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="21ca4-317">hello certificate's subject name must match hello domain that you use tooaccess hello Service Fabric cluster.</span></span> <span data-ttu-id="21ca4-318">Matchningen är obligatoriska tooprovide en SSL för hello klustrets HTTPS management slutpunkter och Service Fabric Explorer.</span><span class="sxs-lookup"><span data-stu-id="21ca4-318">This match is required tooprovide an SSL for hello cluster's HTTPS management endpoints and Service Fabric Explorer.</span></span> <span data-ttu-id="21ca4-319">Du kan skaffa ett SSL-certifikat från en Certifikatutfärdare för hello `.cloudapp.azure.com` domän.</span><span class="sxs-lookup"><span data-stu-id="21ca4-319">You cannot obtain an SSL certificate from a CA for hello `.cloudapp.azure.com` domain.</span></span> <span data-ttu-id="21ca4-320">Du måste skaffa ett anpassat domännamn för klustret.</span><span class="sxs-lookup"><span data-stu-id="21ca4-320">You must obtain a custom domain name for your cluster.</span></span> <span data-ttu-id="21ca4-321">När du begär ett certifikat från en Certifikatutfärdare måste hello certifikatets ämnesnamn matcha hello domännamn som du använder för klustret.</span><span class="sxs-lookup"><span data-stu-id="21ca4-321">When you request a certificate from a CA, hello certificate's subject name must match hello custom domain name that you use for your cluster.</span></span>

<span data-ttu-id="21ca4-322">Du kan fylla hello parametrar från hello helper skriptet i hello Azure-portalen, enligt beskrivningen i hello [skapar ett kluster i hello Azure-portalen](service-fabric-cluster-creation-via-portal.md#create-cluster-in-the-azure-portal) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="21ca4-322">You can fill hello parameters from hello helper script in hello Azure portal, as described in hello [Create a cluster in hello Azure portal](service-fabric-cluster-creation-via-portal.md#create-cluster-in-the-azure-portal) section.</span></span>

## <a name="next-steps"></a><span data-ttu-id="21ca4-323">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="21ca4-323">Next steps</span></span>
<span data-ttu-id="21ca4-324">Du har nu en säker kluster med Azure Active Directory med management-autentisering.</span><span class="sxs-lookup"><span data-stu-id="21ca4-324">At this point, you have a secure cluster with Azure Active Directory providing management authentication.</span></span> <span data-ttu-id="21ca4-325">Nästa [ansluta tooyour klustret](service-fabric-connect-to-secure-cluster.md) och lära dig hur för[hantera program hemligheter](service-fabric-application-secret-management.md).</span><span class="sxs-lookup"><span data-stu-id="21ca4-325">Next, [connect tooyour cluster](service-fabric-connect-to-secure-cluster.md) and learn how too[manage application secrets](service-fabric-application-secret-management.md).</span></span>

## <a name="troubleshoot-setting-up-azure-active-directory-for-client-authentication"></a><span data-ttu-id="21ca4-326">Felsöka ställa in Azure Active Directory för klientautentisering</span><span class="sxs-lookup"><span data-stu-id="21ca4-326">Troubleshoot setting up Azure Active Directory for client authentication</span></span>
<span data-ttu-id="21ca4-327">Om du stöter på ett problem när du konfigurerar Azure AD för klientautentisering, granska hello möjliga lösningar i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="21ca4-327">If you run into an issue while you're setting up Azure AD for client authentication, review hello potential solutions in this section.</span></span>

### <a name="service-fabric-explorer-prompts-you-tooselect-a-certificate"></a><span data-ttu-id="21ca4-328">Service Fabric Explorer uppmanar tooselect ett certifikat</span><span class="sxs-lookup"><span data-stu-id="21ca4-328">Service Fabric Explorer prompts you tooselect a certificate</span></span>
#### <a name="problem"></a><span data-ttu-id="21ca4-329">Problem</span><span class="sxs-lookup"><span data-stu-id="21ca4-329">Problem</span></span>
<span data-ttu-id="21ca4-330">När du loggar in har tooAzure AD i Service Fabric Explorer hello webbläsare returnerar toohello startsidan men får ett meddelande om tooselect ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="21ca4-330">After you sign in successfully tooAzure AD in Service Fabric Explorer, hello browser returns toohello home page but a message prompts you tooselect a certificate.</span></span>

![Dialogrutan för SFX Välj certifikat][sfx-select-certificate-dialog]

#### <a name="reason"></a><span data-ttu-id="21ca4-332">Orsak</span><span class="sxs-lookup"><span data-stu-id="21ca4-332">Reason</span></span>
<span data-ttu-id="21ca4-333">hello användaren inte är tilldelad en roll i hello Azure AD-program för klustret.</span><span class="sxs-lookup"><span data-stu-id="21ca4-333">hello user isn’t assigned a role in hello Azure AD cluster application.</span></span> <span data-ttu-id="21ca4-334">Azure AD-autentiseringen misslyckas därför på Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="21ca4-334">Thus, Azure AD authentication fails on Service Fabric cluster.</span></span> <span data-ttu-id="21ca4-335">Service Fabric Explorer använder toocertificate autentisering.</span><span class="sxs-lookup"><span data-stu-id="21ca4-335">Service Fabric Explorer falls back toocertificate authentication.</span></span>

#### <a name="solution"></a><span data-ttu-id="21ca4-336">Lösning</span><span class="sxs-lookup"><span data-stu-id="21ca4-336">Solution</span></span>
<span data-ttu-id="21ca4-337">Följ hello instruktioner för att konfigurera Azure AD och tilldela användarroller.</span><span class="sxs-lookup"><span data-stu-id="21ca4-337">Follow hello instructions for setting up Azure AD, and assign user roles.</span></span> <span data-ttu-id="21ca4-338">Dessutom vi rekommenderar att du aktiverar ”användare tilldelning krävs tooaccess app” som `SetupApplications.ps1` har.</span><span class="sxs-lookup"><span data-stu-id="21ca4-338">Also, we recommend that you turn on “User assignment required tooaccess app,” as `SetupApplications.ps1` does.</span></span>

### <a name="connection-with-powershell-fails-with-an-error-hello-specified-credentials-are-invalid"></a><span data-ttu-id="21ca4-339">Anslutning med PowerShell misslyckas med felmeddelandet: ”hello angivna autentiseringsuppgifterna är ogiltiga”</span><span class="sxs-lookup"><span data-stu-id="21ca4-339">Connection with PowerShell fails with an error: "hello specified credentials are invalid"</span></span>
#### <a name="problem"></a><span data-ttu-id="21ca4-340">Problem</span><span class="sxs-lookup"><span data-stu-id="21ca4-340">Problem</span></span>
<span data-ttu-id="21ca4-341">När du använder PowerShell tooconnect toohello kluster med hjälp av ”AzureActiveDirectory” säkerhetsläget när du loggar in har tooAzure AD hello anslutningen misslyckas med felmeddelandet: ”hello angivna autentiseringsuppgifterna är ogiltiga”.</span><span class="sxs-lookup"><span data-stu-id="21ca4-341">When you use PowerShell tooconnect toohello cluster by using “AzureActiveDirectory” security mode, after you sign in successfully tooAzure AD, hello connection fails with an error: "hello specified credentials are invalid."</span></span>

#### <a name="solution"></a><span data-ttu-id="21ca4-342">Lösning</span><span class="sxs-lookup"><span data-stu-id="21ca4-342">Solution</span></span>
<span data-ttu-id="21ca4-343">Den här lösningen är hello samma som hello före en.</span><span class="sxs-lookup"><span data-stu-id="21ca4-343">This solution is hello same as hello preceding one.</span></span>

### <a name="service-fabric-explorer-returns-a-failure-when-you-sign-in-aadsts50011"></a><span data-ttu-id="21ca4-344">Service Fabric Explorer returnerar ett fel när du loggar in: ”AADSTS50011”</span><span class="sxs-lookup"><span data-stu-id="21ca4-344">Service Fabric Explorer returns a failure when you sign in: "AADSTS50011"</span></span>
#### <a name="problem"></a><span data-ttu-id="21ca4-345">Problem</span><span class="sxs-lookup"><span data-stu-id="21ca4-345">Problem</span></span>
<span data-ttu-id="21ca4-346">När du försöker toosign i tooAzure AD i Service Fabric Explorer hello sidan returnerar ett fel ”: AADSTS50011: hello svarsadressen &lt;url&gt; matchar inte hello svar adresser som har konfigurerats för programmet hello: &lt;guid&gt;."</span><span class="sxs-lookup"><span data-stu-id="21ca4-346">When you try toosign in tooAzure AD in Service Fabric Explorer, hello page returns a failure: "AADSTS50011: hello reply address &lt;url&gt; does not match hello reply addresses configured for hello application: &lt;guid&gt;."</span></span>

![SFX svarsadressen matchar inte][sfx-reply-address-not-match]

#### <a name="reason"></a><span data-ttu-id="21ca4-348">Orsak</span><span class="sxs-lookup"><span data-stu-id="21ca4-348">Reason</span></span>
<span data-ttu-id="21ca4-349">försöker hello kluster (webb) program som representerar Service Fabric Explorer tooauthenticate mot Azure AD och som en del av hello begäran ger RETUR hello omdirigerings-URL.</span><span class="sxs-lookup"><span data-stu-id="21ca4-349">hello cluster (web) application that represents Service Fabric Explorer attempts tooauthenticate against Azure AD, and as part of hello request it provides hello redirect return URL.</span></span> <span data-ttu-id="21ca4-350">Men hello URL visas i hello Azure AD-program **REPLY URL** lista.</span><span class="sxs-lookup"><span data-stu-id="21ca4-350">But hello URL is not listed in hello Azure AD application **REPLY URL** list.</span></span>

#### <a name="solution"></a><span data-ttu-id="21ca4-351">Lösning</span><span class="sxs-lookup"><span data-stu-id="21ca4-351">Solution</span></span>
<span data-ttu-id="21ca4-352">På hello **konfigurera** för hello (webbprogram) bör du lägga till hello URL för Service Fabric Explorer toohello **REPLY URL** listan eller ersätta en hello objekt i hello lista.</span><span class="sxs-lookup"><span data-stu-id="21ca4-352">On hello **Configure** tab of hello cluster (web) application, add hello URL of Service Fabric Explorer toohello **REPLY URL** list or replace one of hello items in hello list.</span></span> <span data-ttu-id="21ca4-353">När du är klar kan du spara ändringen.</span><span class="sxs-lookup"><span data-stu-id="21ca4-353">When you have finished, save your change.</span></span>

![Url till webbprogram svar][web-application-reply-url]

### <a name="connect-hello-cluster-by-using-azure-ad-authentication-via-powershell"></a><span data-ttu-id="21ca4-355">Ansluta hello kluster med hjälp av Azure AD-autentisering via PowerShell</span><span class="sxs-lookup"><span data-stu-id="21ca4-355">Connect hello cluster by using Azure AD authentication via PowerShell</span></span>
<span data-ttu-id="21ca4-356">tooconnect hello Service Fabric-kluster använder hello följande exempel för PowerShell-kommando:</span><span class="sxs-lookup"><span data-stu-id="21ca4-356">tooconnect hello Service Fabric cluster, use hello following PowerShell command example:</span></span>

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <endpoint> -KeepAliveIntervalInSec 10 -AzureActiveDirectory -ServerCertThumbprint <thumbprint>
```

<span data-ttu-id="21ca4-357">toolearn om hello Connect-ServiceFabricCluster: se [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx).</span><span class="sxs-lookup"><span data-stu-id="21ca4-357">toolearn about hello Connect-ServiceFabricCluster cmdlet, see [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx).</span></span>

### <a name="can-i-reuse-hello-same-azure-ad-tenant-in-multiple-clusters"></a><span data-ttu-id="21ca4-358">Kan jag återanvända hello samma Azure AD-klient i flera kluster?</span><span class="sxs-lookup"><span data-stu-id="21ca4-358">Can I reuse hello same Azure AD tenant in multiple clusters?</span></span>
<span data-ttu-id="21ca4-359">Ja.</span><span class="sxs-lookup"><span data-stu-id="21ca4-359">Yes.</span></span> <span data-ttu-id="21ca4-360">Men kom ihåg tooadd hello URL för Service Fabric Explorer tooyour kluster (webb) program.</span><span class="sxs-lookup"><span data-stu-id="21ca4-360">But remember tooadd hello URL of Service Fabric Explorer tooyour cluster (web) application.</span></span> <span data-ttu-id="21ca4-361">Annars fungerar Service Fabric Explorer inte.</span><span class="sxs-lookup"><span data-stu-id="21ca4-361">Otherwise, Service Fabric Explorer doesn’t work.</span></span>

### <a name="why-do-i-still-need-a-server-certificate-while-azure-ad-is-enabled"></a><span data-ttu-id="21ca4-362">Varför måste jag göra fortfarande ett servercertifikat medan Azure AD är aktiverad?</span><span class="sxs-lookup"><span data-stu-id="21ca4-362">Why do I still need a server certificate while Azure AD is enabled?</span></span>
<span data-ttu-id="21ca4-363">FabricClient och FabricGateway utför en ömsesidig autentisering.</span><span class="sxs-lookup"><span data-stu-id="21ca4-363">FabricClient and FabricGateway perform a mutual authentication.</span></span> <span data-ttu-id="21ca4-364">Integrering med Azure AD tillhandahåller en identitet toohello server under Azure AD-autentisering och hello servercertifikatet är används tooverify hello-serveridentitet.</span><span class="sxs-lookup"><span data-stu-id="21ca4-364">During Azure AD authentication, Azure AD integration provides a client identity toohello server, and hello server certificate is used tooverify hello server identity.</span></span> <span data-ttu-id="21ca4-365">Mer information om Service Fabric-certifikat finns [X.509-certifikat och Service Fabric][x509-certificates-and-service-fabric].</span><span class="sxs-lookup"><span data-stu-id="21ca4-365">For more information about Service Fabric certificates, see [X.509 certificates and Service Fabric][x509-certificates-and-service-fabric].</span></span>

<!-- Links -->
[azure-powershell]:https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[aad-graph-api-docs]:https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog
[azure-classic-portal]: https://manage.windowsazure.com
[service-fabric-rp-helpers]: https://github.com/ChackDan/Service-Fabric/tree/master/Scripts/ServiceFabricRPHelpers
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[active-directory-howto-tenant]: ../active-directory/active-directory-howto-tenant.md
[service-fabric-visualizing-your-cluster]: service-fabric-visualizing-your-cluster.md
[service-fabric-manage-application-in-visual-studio]: service-fabric-manage-application-in-visual-studio.md
[sf-aad-ps-script-download]:http://servicefabricsdkstorage.blob.core.windows.net/publicrelease/MicrosoftAzureServiceFabric-AADHelpers.zip
[azure-quickstart-templates]: https://github.com/Azure/azure-quickstart-templates
[service-fabric-secure-cluster-5-node-1-nodetype]: https://github.com/Azure/azure-quickstart-templates/blob/master/service-fabric-secure-cluster-5-node-1-nodetype/
[resource-group-template-deploy]: https://azure.microsoft.com/documentation/articles/resource-group-template-deploy/
[x509-certificates-and-service-fabric]: service-fabric-cluster-security.md#x509-certificates-and-service-fabric

<!-- Images -->
[cluster-security-arm-dependency-map]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-arm-dependency-map.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png
[assign-users-to-roles-button]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles-button.png
[assign-users-to-roles-dialog]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles.png
[sfx-select-certificate-dialog]: ./media/service-fabric-cluster-creation-via-arm/sfx-select-certificate-dialog.png
[sfx-reply-address-not-match]: ./media/service-fabric-cluster-creation-via-arm/sfx-reply-address-not-match.png
[web-application-reply-url]: ./media/service-fabric-cluster-creation-via-arm/web-application-reply-url.png

