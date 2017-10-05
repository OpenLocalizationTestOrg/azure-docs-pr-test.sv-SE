---
title: "Skapa ett Azure Service Fabric-kluster från en mall | Microsoft Docs"
description: "Den här artikeln beskriver hur du ställer in en säker Service Fabric-kluster i Azure med Azure Resource Manager, Azure Key Vault och Azure Active Directory (Azure AD) för klientautentisering."
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
ms.openlocfilehash: 420ea486e626763af65a23e49ce04033ea418fb4
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-service-fabric-cluster-by-using-azure-resource-manager"></a><span data-ttu-id="7cf72-103">Skapa ett Service Fabric-kluster med hjälp av Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="7cf72-103">Create a Service Fabric cluster by using Azure Resource Manager</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7cf72-104">Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="7cf72-104">Azure Resource Manager</span></span>](service-fabric-cluster-creation-via-arm.md)
> * [<span data-ttu-id="7cf72-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="7cf72-105">Azure portal</span></span>](service-fabric-cluster-creation-via-portal.md)
>
>

<span data-ttu-id="7cf72-106">Den här stegvisa guiden vägleder dig genom att skapa en säker Azure Service Fabric-kluster i Azure med hjälp av Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="7cf72-106">This step-by-step guide walks you through setting up a secure Azure Service Fabric cluster in Azure by using Azure Resource Manager.</span></span> <span data-ttu-id="7cf72-107">Vi bekräftar att artikeln är långt.</span><span class="sxs-lookup"><span data-stu-id="7cf72-107">We acknowledge that the article is long.</span></span> <span data-ttu-id="7cf72-108">Dock om du redan är väl bekanta dig med innehållet, måste du följa varje steg noggrant.</span><span class="sxs-lookup"><span data-stu-id="7cf72-108">Nevertheless, unless you are already thoroughly familiar with the content, be sure to follow each step carefully.</span></span>

<span data-ttu-id="7cf72-109">Guiden innehåller följande procedurer:</span><span class="sxs-lookup"><span data-stu-id="7cf72-109">The guide covers the following procedures:</span></span>

* <span data-ttu-id="7cf72-110">Ställa in en Azure key vault för att ladda upp certifikat för kluster- och säkerhet</span><span class="sxs-lookup"><span data-stu-id="7cf72-110">Setting up an Azure key vault to upload certificates for cluster and application security</span></span>
* <span data-ttu-id="7cf72-111">Skapar en skyddad kluster i Azure med hjälp av Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="7cf72-111">Creating a secured cluster in Azure by using Azure Resource Manager</span></span>
* <span data-ttu-id="7cf72-112">Autentisering av användare med hjälp av Azure Active Directory (Azure AD) för hantering av kluster</span><span class="sxs-lookup"><span data-stu-id="7cf72-112">Authenticating users by using Azure Active Directory (Azure AD) for cluster management</span></span>

<span data-ttu-id="7cf72-113">En säker klustret är ett kluster som förhindrar obehörig åtkomst till hanteringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="7cf72-113">A secure cluster is a cluster that prevents unauthorized access to management operations.</span></span> <span data-ttu-id="7cf72-114">Detta inkluderar distribution, uppgradera och ta bort program, tjänster och de data som de innehåller.</span><span class="sxs-lookup"><span data-stu-id="7cf72-114">This includes deploying, upgrading, and deleting applications, services, and the data they contain.</span></span> <span data-ttu-id="7cf72-115">Ett oskyddat klustret är ett kluster alla kan ansluta till när som helst och utföra hanteringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="7cf72-115">An unsecure cluster is a cluster that anyone can connect to at any time and perform management operations.</span></span> <span data-ttu-id="7cf72-116">Men det är möjligt att skapa ett oskyddade kluster, rekommenderar vi starkt att du skapar en säker klustret redan från början.</span><span class="sxs-lookup"><span data-stu-id="7cf72-116">Although it is possible to create an unsecure cluster, we highly recommend that you create a secure cluster from the outset.</span></span> <span data-ttu-id="7cf72-117">Eftersom ett oskyddade kluster inte kan säkras senare, måste du skapa ett nytt kluster.</span><span class="sxs-lookup"><span data-stu-id="7cf72-117">Because an unsecure cluster cannot be secured later, a new cluster must be created.</span></span>

<span data-ttu-id="7cf72-118">Konceptet för att skapa skyddade kluster är densamma, oavsett om de är Linux eller Windows-kluster.</span><span class="sxs-lookup"><span data-stu-id="7cf72-118">The concept of creating secure clusters is the same, whether they are Linux or Windows clusters.</span></span> <span data-ttu-id="7cf72-119">Mer information och hjälp skript för att skapa säkra Linux-kluster, se [att skapa skyddade kluster på Linux](#secure-linux-clusters).</span><span class="sxs-lookup"><span data-stu-id="7cf72-119">For more information and helper scripts for creating secure Linux clusters, see [Creating secure clusters on Linux](#secure-linux-clusters).</span></span>

## <a name="sign-in-to-your-azure-account"></a><span data-ttu-id="7cf72-120">Logga in på ditt Azure-konto</span><span class="sxs-lookup"><span data-stu-id="7cf72-120">Sign in to your Azure account</span></span>
<span data-ttu-id="7cf72-121">Den här guiden använder [Azure PowerShell][azure-powershell].</span><span class="sxs-lookup"><span data-stu-id="7cf72-121">This guide uses [Azure PowerShell][azure-powershell].</span></span> <span data-ttu-id="7cf72-122">När du startar en ny PowerShell-session, logga in på ditt Azure-konto och välja din prenumeration innan du kan köra kommandon för Azure.</span><span class="sxs-lookup"><span data-stu-id="7cf72-122">When you start a new PowerShell session, sign in to your Azure account and select your subscription before you execute Azure commands.</span></span>

<span data-ttu-id="7cf72-123">Logga in på ditt Azure-konto:</span><span class="sxs-lookup"><span data-stu-id="7cf72-123">Sign in to your Azure account:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="7cf72-124">Välj din prenumeration:</span><span class="sxs-lookup"><span data-stu-id="7cf72-124">Select your subscription:</span></span>

```powershell
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

## <a name="set-up-a-key-vault"></a><span data-ttu-id="7cf72-125">Ställ in ett nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="7cf72-125">Set up a key vault</span></span>
<span data-ttu-id="7cf72-126">Det här avsnittet beskrivs skapa nyckelvalvet för Service Fabric-kluster i Azure och för Service Fabric-program.</span><span class="sxs-lookup"><span data-stu-id="7cf72-126">This section discusses creating a key vault for a Service Fabric cluster in Azure and for Service Fabric applications.</span></span> <span data-ttu-id="7cf72-127">En fullständig guide till Azure Key Vault finns i den [Key Vault Kom igång med][key-vault-get-started].</span><span class="sxs-lookup"><span data-stu-id="7cf72-127">For a complete guide to Azure Key Vault, refer to the [Key Vault getting started guide][key-vault-get-started].</span></span>

<span data-ttu-id="7cf72-128">Service Fabric använder X.509-certifikat för att skydda ett kluster och säkerhetsfunktioner för programmet.</span><span class="sxs-lookup"><span data-stu-id="7cf72-128">Service Fabric uses X.509 certificates to secure a cluster and provide application security features.</span></span> <span data-ttu-id="7cf72-129">Du kan använda Nyckelvalv för att hantera certifikat för Service Fabric-kluster i Azure.</span><span class="sxs-lookup"><span data-stu-id="7cf72-129">You use Key Vault to manage certificates for Service Fabric clusters in Azure.</span></span> <span data-ttu-id="7cf72-130">När ett kluster distribueras i Azure, Azure-resursprovider som ansvarar för att skapa Service Fabric-kluster hämtar certifikat från Nyckelvalvet och installerar dem på klustret virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="7cf72-130">When a cluster is deployed in Azure, the Azure resource provider that's responsible for creating Service Fabric clusters pulls certificates from Key Vault and installs them on the cluster VMs.</span></span>

<span data-ttu-id="7cf72-131">Följande diagram illustrerar förhållandet mellan Azure Key Vault, Service Fabric-kluster och Azure-resursprovider som använder certifikat som lagras i en nyckelvalvet när den skapar ett kluster:</span><span class="sxs-lookup"><span data-stu-id="7cf72-131">The following diagram illustrates the relationship between Azure Key Vault, a Service Fabric cluster, and the Azure resource provider that uses certificates stored in a key vault when it creates a cluster:</span></span>

![Diagram över certifikatinstallation][cluster-security-cert-installation]

### <a name="create-a-resource-group"></a><span data-ttu-id="7cf72-133">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="7cf72-133">Create a resource group</span></span>
<span data-ttu-id="7cf72-134">Det första steget är att skapa en resursgrupp för nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="7cf72-134">The first step is to create a resource group specifically for your key vault.</span></span> <span data-ttu-id="7cf72-135">Vi rekommenderar att du placera nyckelvalvet i sin egen resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="7cf72-135">We recommend that you put the key vault into its own resource group.</span></span> <span data-ttu-id="7cf72-136">Den här åtgärden kan du ta bort beräkning och lagring resursgrupper, inklusive resursgruppen som innehåller Service Fabric-klustret utan att förlora dina nycklar och hemligheter.</span><span class="sxs-lookup"><span data-stu-id="7cf72-136">This action lets you remove the compute and storage resource groups, including the resource group that contains your Service Fabric cluster, without losing your keys and secrets.</span></span> <span data-ttu-id="7cf72-137">Resursgruppen som innehåller nyckelvalvet _måste vara i samma region_ som klustret som använder den.</span><span class="sxs-lookup"><span data-stu-id="7cf72-137">The resource group that contains your key vault _must be in the same region_ as the cluster that is using it.</span></span>

<span data-ttu-id="7cf72-138">Om du planerar att distribuera kluster i flera områden, föreslår vi att du namnger resursgruppen och nyckeln valvet på ett sätt som anger vilken region som den tillhör.</span><span class="sxs-lookup"><span data-stu-id="7cf72-138">If you plan to deploy clusters in multiple regions, we suggest that you name the resource group and the key vault in a way that indicates which region it belongs to.</span></span>  

```powershell

    New-AzureRmResourceGroup -Name westus-mykeyvault -Location 'West US'
```
<span data-ttu-id="7cf72-139">Resultatet bör se ut så här:</span><span class="sxs-lookup"><span data-stu-id="7cf72-139">The output should look like this:</span></span>

```powershell

    WARNING: The output object type of this cmdlet is going to be modified in a future release.

    ResourceGroupName : westus-mykeyvault
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/<guid>/resourceGroups/westus-mykeyvault

```
<a id="new-key-vault"></a>

### <a name="create-a-key-vault-in-the-new-resource-group"></a><span data-ttu-id="7cf72-140">Skapa en nyckelvalvet i den nya resursgruppen</span><span class="sxs-lookup"><span data-stu-id="7cf72-140">Create a key vault in the new resource group</span></span>
<span data-ttu-id="7cf72-141">Nyckelvalvet _måste vara aktiverat för distribution_ att compute-resursprovidern så att hämta certifikat från det och installera det på instanser för virtuella datorer:</span><span class="sxs-lookup"><span data-stu-id="7cf72-141">The key vault _must be enabled for deployment_ to allow the compute resource provider to get certificates from it and install it on virtual machine instances:</span></span>

```powershell

    New-AzureRmKeyVault -VaultName 'mywestusvault' -ResourceGroupName 'westus-mykeyvault' -Location 'West US' -EnabledForDeployment

```

<span data-ttu-id="7cf72-142">Resultatet bör se ut så här:</span><span class="sxs-lookup"><span data-stu-id="7cf72-142">The output should look like this:</span></span>

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
                                       Permissions to Keys      :    get, create, delete, list, update, import, backup, restore
                                       Permissions to Secrets   :    all


    Tags                             :
```
<a id="existing-key-vault"></a>

## <a name="use-an-existing-key-vault"></a><span data-ttu-id="7cf72-143">Använd en befintlig nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="7cf72-143">Use an existing key vault</span></span>

<span data-ttu-id="7cf72-144">Att använda en befintlig nyckelvalv du _måste aktivera den för distribution_ att compute-resursprovidern så att hämta certifikat från det och installera den på klusternoder:</span><span class="sxs-lookup"><span data-stu-id="7cf72-144">To use an existing key vault, you _must enable it for deployment_ to allow the compute resource provider to get certificates from it and install it on cluster nodes:</span></span>

```powershell

Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

```

<a id="add-certificate-to-key-vault"></a>

## <a name="add-certificates-to-your-key-vault"></a><span data-ttu-id="7cf72-145">Lägga till certifikat i nyckelvalvet</span><span class="sxs-lookup"><span data-stu-id="7cf72-145">Add certificates to your key vault</span></span>

<span data-ttu-id="7cf72-146">Certifikat används i Service Fabric till att autentisera och kryptera olika delar av ett kluster och de program som körs där.</span><span class="sxs-lookup"><span data-stu-id="7cf72-146">Certificates are used in Service Fabric to provide authentication and encryption to secure various aspects of a cluster and its applications.</span></span> <span data-ttu-id="7cf72-147">Mer information om hur du använder certifikat i Service Fabric finns [säkerhetsscenarier för Service Fabric-klustret][service-fabric-cluster-security].</span><span class="sxs-lookup"><span data-stu-id="7cf72-147">For more information on how certificates are used in Service Fabric, see [Service Fabric cluster security scenarios][service-fabric-cluster-security].</span></span>

### <a name="cluster-and-server-certificate-required"></a><span data-ttu-id="7cf72-148">Kluster- och certifikat (krävs)</span><span class="sxs-lookup"><span data-stu-id="7cf72-148">Cluster and server certificate (required)</span></span>
<span data-ttu-id="7cf72-149">Detta certifikat krävs för att skydda ett kluster och förhindra obehörig åtkomst till den.</span><span class="sxs-lookup"><span data-stu-id="7cf72-149">This certificate is required to secure a cluster and prevent unauthorized access to it.</span></span> <span data-ttu-id="7cf72-150">Det ger säkerhet i klustret på två sätt:</span><span class="sxs-lookup"><span data-stu-id="7cf72-150">It provides cluster security in two ways:</span></span>

* <span data-ttu-id="7cf72-151">Autentisering för klustret: autentiserar nod till nod kommunikation för klustret.</span><span class="sxs-lookup"><span data-stu-id="7cf72-151">Cluster authentication: Authenticates node-to-node communication for cluster federation.</span></span> <span data-ttu-id="7cf72-152">Endast noder som kan bevisa sin identitet med det här certifikatet kan ansluta till klustret.</span><span class="sxs-lookup"><span data-stu-id="7cf72-152">Only nodes that can prove their identity with this certificate can join the cluster.</span></span>
* <span data-ttu-id="7cf72-153">Serverautentisering: verifierar att klustret management slutpunkter management-klienten så att klienten management vet pratar till verkliga klustret.</span><span class="sxs-lookup"><span data-stu-id="7cf72-153">Server authentication: Authenticates the cluster management endpoints to a management client, so that the management client knows it is talking to the real cluster.</span></span> <span data-ttu-id="7cf72-154">Det här certifikatet ger också en SSL för HTTPS-hanterings-API och för Service Fabric Explorer via HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7cf72-154">This certificate also provides an SSL for the HTTPS management API and for Service Fabric Explorer over HTTPS.</span></span>

<span data-ttu-id="7cf72-155">Om du vill hantera dessa ändamål måste certifikatet uppfylla följande krav:</span><span class="sxs-lookup"><span data-stu-id="7cf72-155">To serve these purposes, the certificate must meet the following requirements:</span></span>

* <span data-ttu-id="7cf72-156">Certifikatet måste innehålla en privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="7cf72-156">The certificate must contain a private key.</span></span>
* <span data-ttu-id="7cf72-157">Certifikatet måste skapas för nyckelutbyte, som kan exporteras till en Personal Information Exchange (.pfx)-fil.</span><span class="sxs-lookup"><span data-stu-id="7cf72-157">The certificate must be created for key exchange, which is exportable to a Personal Information Exchange (.pfx) file.</span></span>
* <span data-ttu-id="7cf72-158">Certifikatets ämnesnamn måste matcha den domän som du använder för att få åtkomst till Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="7cf72-158">The certificate's subject name must match the domain that you use to access the Service Fabric cluster.</span></span> <span data-ttu-id="7cf72-159">Den här matchar krävs att tillhandahålla en SSL för klustrets HTTPS management slutpunkter och Service Fabric Explorer.</span><span class="sxs-lookup"><span data-stu-id="7cf72-159">This matching is required to provide an SSL for the cluster's HTTPS management endpoints and Service Fabric Explorer.</span></span> <span data-ttu-id="7cf72-160">Du kan skaffa ett SSL-certifikat från en certifikatutfärdare (CA) för det. cloudapp.azure.com domän.</span><span class="sxs-lookup"><span data-stu-id="7cf72-160">You cannot obtain an SSL certificate from a certificate authority (CA) for the .cloudapp.azure.com domain.</span></span> <span data-ttu-id="7cf72-161">Du måste skaffa ett anpassat domännamn för klustret.</span><span class="sxs-lookup"><span data-stu-id="7cf72-161">You must obtain a custom domain name for your cluster.</span></span> <span data-ttu-id="7cf72-162">När du begär ett certifikat från en Certifikatutfärdare måste certifikatets ämnesnamn matcha det anpassade domännamnet som du använder för klustret.</span><span class="sxs-lookup"><span data-stu-id="7cf72-162">When you request a certificate from a CA, the certificate's subject name must match the custom domain name that you use for your cluster.</span></span>

### <a name="application-certificates-optional"></a><span data-ttu-id="7cf72-163">Certifikat för programmet (valfritt)</span><span class="sxs-lookup"><span data-stu-id="7cf72-163">Application certificates (optional)</span></span>
<span data-ttu-id="7cf72-164">Valfritt antal ytterligare certifikat kan installeras på ett kluster av säkerhetsskäl för programmet.</span><span class="sxs-lookup"><span data-stu-id="7cf72-164">Any number of additional certificates can be installed on a cluster for application security purposes.</span></span> <span data-ttu-id="7cf72-165">Tänk på applikationssäkerhets-scenarier som kräver ett certifikat installeras på noderna, som innan du skapar klustret:</span><span class="sxs-lookup"><span data-stu-id="7cf72-165">Before creating your cluster, consider the application security scenarios that require a certificate to be installed on the nodes, such as:</span></span>

* <span data-ttu-id="7cf72-166">Kryptering och dekryptering av konfigurationsvärden för programmet.</span><span class="sxs-lookup"><span data-stu-id="7cf72-166">Encryption and decryption of application configuration values.</span></span>
* <span data-ttu-id="7cf72-167">Kryptering av data mellan noderna under replikeringen.</span><span class="sxs-lookup"><span data-stu-id="7cf72-167">Encryption of data across nodes during replication.</span></span>

### <a name="formatting-certificates-for-azure-resource-provider-use"></a><span data-ttu-id="7cf72-168">Formatering certifikat för användning av Azure resource provider</span><span class="sxs-lookup"><span data-stu-id="7cf72-168">Formatting certificates for Azure resource provider use</span></span>
<span data-ttu-id="7cf72-169">Du kan lägga till och använda privata nyckelfiler (.pfx) direkt via nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="7cf72-169">You can add and use private key files (.pfx) directly through your key vault.</span></span> <span data-ttu-id="7cf72-170">Azure compute-resursprovidern kräver dock nycklar lagras i en särskild JavaScript Object Notation (JSON)-format.</span><span class="sxs-lookup"><span data-stu-id="7cf72-170">However, the Azure compute resource provider requires keys to be stored in a special JavaScript Object Notation (JSON) format.</span></span> <span data-ttu-id="7cf72-171">Formatet som innehåller PFX-filen som en base 64-kodad sträng och lösenordet för privata nyckeln.</span><span class="sxs-lookup"><span data-stu-id="7cf72-171">The format includes the .pfx file as a base 64-encoded string and the private key password.</span></span> <span data-ttu-id="7cf72-172">Om du vill anpassa dessa krav ska nycklarna placeras i en JSON-sträng och sedan lagras som ”hemligheter” i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="7cf72-172">To accommodate these requirements, the keys must be placed in a JSON string and then stored as "secrets" in the key vault.</span></span>

<span data-ttu-id="7cf72-173">Att göra den här processen enklare, en [PowerShell-modulen finns på GitHub][service-fabric-rp-helpers].</span><span class="sxs-lookup"><span data-stu-id="7cf72-173">To make this process easier, a [PowerShell module is available on GitHub][service-fabric-rp-helpers].</span></span> <span data-ttu-id="7cf72-174">Om du vill använda modulen, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="7cf72-174">To use the module, do the following:</span></span>

1. <span data-ttu-id="7cf72-175">Hämta hela innehållet på lagringsplatsen till en lokal katalog.</span><span class="sxs-lookup"><span data-stu-id="7cf72-175">Download the entire contents of the repo into a local directory.</span></span>
2. <span data-ttu-id="7cf72-176">Gå till den lokala katalogen.</span><span class="sxs-lookup"><span data-stu-id="7cf72-176">Go to the local directory.</span></span>
2. <span data-ttu-id="7cf72-177">Importera modulen ServiceFabricRPHelpers i PowerShell-fönstret:</span><span class="sxs-lookup"><span data-stu-id="7cf72-177">Import the ServiceFabricRPHelpers module in your PowerShell window:</span></span>

```powershell

 Import-Module "C:\..\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"

```

<span data-ttu-id="7cf72-178">Den `Invoke-AddCertToKeyVault` kommandot i PowerShell-modulen automatiskt formaterar en certifikatets privata nyckel till en JSON-sträng och överförs till nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="7cf72-178">The `Invoke-AddCertToKeyVault` command in this PowerShell module automatically formats a certificate private key into a JSON string and uploads it to the key vault.</span></span> <span data-ttu-id="7cf72-179">Använd kommandot för att lägga till kluster-certifikat och några ytterligare certifikat i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="7cf72-179">Use the command to add the cluster certificate and any additional application certificates to the key vault.</span></span> <span data-ttu-id="7cf72-180">Upprepa det här steget för några ytterligare certifikat som du vill installera i klustret.</span><span class="sxs-lookup"><span data-stu-id="7cf72-180">Repeat this step for any additional certificates you want to install in your cluster.</span></span>

#### <a name="uploading-an-existing-certificate"></a><span data-ttu-id="7cf72-181">Ladda upp ett befintligt certifikat</span><span class="sxs-lookup"><span data-stu-id="7cf72-181">Uploading an existing certificate</span></span>

```powershell

 Invoke-AddCertToKeyVault -SubscriptionId <guid> -ResourceGroupName westus-mykeyvault -Location "West US" -VaultName mywestusvault -CertificateName mycert -Password "<password>" -UseExistingCertificate -ExistingPfxFilePath "C:\path\to\mycertkey.pfx"

```

<span data-ttu-id="7cf72-182">Om du får ett felmeddelande som visas här är oftast det att det finns en konflikt för resurs-URL.</span><span class="sxs-lookup"><span data-stu-id="7cf72-182">If you get an error, such as the one shown here, it usually means that you have a resource URL conflict.</span></span> <span data-ttu-id="7cf72-183">Ändra namnet på nyckelvalv för att lösa konflikten.</span><span class="sxs-lookup"><span data-stu-id="7cf72-183">To resolve the conflict, change the key vault name.</span></span>

```
Set-AzureKeyVaultSecret : The remote name could not be resolved: 'westuskv.vault.azure.net'
At C:\Users\chackdan\Documents\GitHub\Service-Fabric\Scripts\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1:440 char:11
+ $secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $Certif ...
+           ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Set-AzureKeyVaultSecret], WebException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.KeyVault.SetAzureKeyVaultSecret

```

<span data-ttu-id="7cf72-184">Efter att konflikten löses utdata ska se ut så här:</span><span class="sxs-lookup"><span data-stu-id="7cf72-184">After the conflict is resolved, the output should look like this:</span></span>

```

    Switching context to SubscriptionId <guid>
    Ensuring ResourceGroup westus-mykeyvault in West US
    WARNING: The output object type of this cmdlet is going to be modified in a future release.
    Using existing value mywestusvault in West US
    Reading pfx file from C:\path\to\key.pfx
    Writing secret to mywestusvault in vault mywestusvault


Name  : CertificateThumbprint
Value : E21DBC64B183B5BF355C34C46E03409FEEAEF58D

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/westus-mykeyvault/providers/Microsoft.KeyVault/vaults/mywestusvault

Name  : CertificateURL
Value : https://mywestusvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47

```

>[!NOTE]
><span data-ttu-id="7cf72-185">Du måste de tre föregående strängar, CertificateThumbprint, SourceVault och CertificateURL att ställa in en säker Service Fabric-klustret och att hämta programmet certifikat som du kan använda för programsäkerhet.</span><span class="sxs-lookup"><span data-stu-id="7cf72-185">You need the three preceding strings, CertificateThumbprint, SourceVault, and CertificateURL, to set up a secure Service Fabric cluster and to obtain any application certificates that you might be using for application security.</span></span> <span data-ttu-id="7cf72-186">Om du inte sparar strängar kan det vara svårt att hämta dem genom att fråga nyckelvalvet senare.</span><span class="sxs-lookup"><span data-stu-id="7cf72-186">If you do not save the strings, it can be difficult to retrieve them by querying the key vault later.</span></span>

<a id="add-self-signed-certificate-to-key-vault"></a>

#### <a name="creating-a-self-signed-certificate-and-uploading-it-to-the-key-vault"></a><span data-ttu-id="7cf72-187">Skapa ett självsignerat certifikat och överföra den till nyckelvalvet</span><span class="sxs-lookup"><span data-stu-id="7cf72-187">Creating a self-signed certificate and uploading it to the key vault</span></span>

<span data-ttu-id="7cf72-188">Hoppa över det här steget om du redan har överfört din certifikat till nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="7cf72-188">If you have already uploaded your certificates to the key vault, skip this step.</span></span> <span data-ttu-id="7cf72-189">Det här steget är för att generera ett nytt självsignerat certifikat och överföra den till nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="7cf72-189">This step is for generating a new self-signed certificate and uploading it to your key vault.</span></span> <span data-ttu-id="7cf72-190">När du ändrar parametrar i följande skript och kör det ska efterfrågas ett lösenord för certifikatet.</span><span class="sxs-lookup"><span data-stu-id="7cf72-190">After you change the parameters in the following script and then run it, you should be prompted for a certificate password.</span></span>  

```powershell

$ResourceGroup = "chackowestuskv"
$VName = "chackokv2"
$SubID = "6c653126-e4ba-42cd-a1dd-f7bf96ae7a47"
$locationRegion = "westus"
$newCertName = "chackotestcertificate1"
$dnsName = "www.mycluster.westus.mydomain.com" #The certificate's subject name must match the domain used to access the Service Fabric cluster.
$localCertPath = "C:\MyCertificates" # location where you want the .PFX to be stored

 Invoke-AddCertToKeyVault -SubscriptionId $SubID -ResourceGroupName $ResourceGroup -Location $locationRegion -VaultName $VName -CertificateName $newCertName -CreateSelfSignedCertificate -DnsName $dnsName -OutputPath $localCertPath

```

<span data-ttu-id="7cf72-191">Om du får ett felmeddelande som visas här är oftast det att det finns en konflikt för resurs-URL.</span><span class="sxs-lookup"><span data-stu-id="7cf72-191">If you get an error, such as the one shown here, it usually means that you have a resource URL conflict.</span></span> <span data-ttu-id="7cf72-192">Lös konflikten genom att ändra nyckelvalv namn, RG namn och så vidare.</span><span class="sxs-lookup"><span data-stu-id="7cf72-192">To resolve the conflict, change the key vault name, RG name, and so forth.</span></span>

```
Set-AzureKeyVaultSecret : The remote name could not be resolved: 'westuskv.vault.azure.net'
At C:\Users\chackdan\Documents\GitHub\Service-Fabric\Scripts\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1:440 char:11
+ $secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $Certif ...
+           ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Set-AzureKeyVaultSecret], WebException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.KeyVault.SetAzureKeyVaultSecret

```

<span data-ttu-id="7cf72-193">Efter att konflikten löses utdata ska se ut så här:</span><span class="sxs-lookup"><span data-stu-id="7cf72-193">After the conflict is resolved, the output should look like this:</span></span>

```
PS C:\Users\chackdan\Documents\GitHub\Service-Fabric\Scripts\ServiceFabricRPHelpers> Invoke-AddCertToKeyVault -SubscriptionId $SubID -ResourceGroupName $ResouceGroup -Location $locationRegion -VaultName $VName -CertificateName $newCertName -Password $certPassword -CreateSelfSignedCertificate -DnsName $dnsName -OutputPath $localCertPath
Switching context to SubscriptionId 6c343126-e4ba-52cd-a1dd-f8bf96ae7a47
Ensuring ResourceGroup chackowestuskv in westus
WARNING: The output object type of this cmdlet will be modified in a future release.
Creating new vault westuskv1 in westus
Creating new self signed certificate at C:\MyCertificates\chackonewcertificate1.pfx
Reading pfx file from C:\MyCertificates\chackonewcertificate1.pfx
Writing secret to chackonewcertificate1 in vault westuskv1


Name  : CertificateThumbprint
Value : 96BB3CC234F9D43C25D4B547sd8DE7B569F413EE

Name  : SourceVault
Value : /subscriptions/6c653126-e4ba-52cd-a1dd-f8bf96ae7a47/resourceGroups/chackowestuskv/providers/Microsoft.KeyVault/vaults/westuskv1

Name  : CertificateURL
Value : https://westuskv1.vault.azure.net:443/secrets/chackonewcertificate1/ee247291e45d405b8c8bbf81782d12bd

```

>[!NOTE]
><span data-ttu-id="7cf72-194">Du måste de tre föregående strängar, CertificateThumbprint, SourceVault och CertificateURL att ställa in en säker Service Fabric-klustret och att hämta programmet certifikat som du kan använda för programsäkerhet.</span><span class="sxs-lookup"><span data-stu-id="7cf72-194">You need the three preceding strings, CertificateThumbprint, SourceVault, and CertificateURL, to set up a secure Service Fabric cluster and to obtain any application certificates that you might be using for application security.</span></span> <span data-ttu-id="7cf72-195">Om du inte sparar strängar kan det vara svårt att hämta dem genom att fråga nyckelvalvet senare.</span><span class="sxs-lookup"><span data-stu-id="7cf72-195">If you do not save the strings, it can be difficult to retrieve them by querying the key vault later.</span></span>

 <span data-ttu-id="7cf72-196">Du bör nu ha följande element på plats:</span><span class="sxs-lookup"><span data-stu-id="7cf72-196">At this point, you should have the following elements in place:</span></span>

* <span data-ttu-id="7cf72-197">Resursgruppen för nyckelvalv.</span><span class="sxs-lookup"><span data-stu-id="7cf72-197">The key vault resource group.</span></span>
* <span data-ttu-id="7cf72-198">Nyckelvalvet och dess URL (kallas SourceVault i föregående PowerShell utdata).</span><span class="sxs-lookup"><span data-stu-id="7cf72-198">The key vault and its URL (called SourceVault in the preceding PowerShell output).</span></span>
* <span data-ttu-id="7cf72-199">Certifikat för serverautentisering i klustret och dess URL: en i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="7cf72-199">The cluster server authentication certificate and its URL in the key vault.</span></span>
* <span data-ttu-id="7cf72-200">Programcertifikat och deras URL: er i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="7cf72-200">The application certificates and their URLs in the key vault.</span></span>


<a id="add-AAD-for-client"></a>

## <a name="set-up-azure-active-directory-for-client-authentication"></a><span data-ttu-id="7cf72-201">Konfigurera Azure Active Directory för klientautentisering</span><span class="sxs-lookup"><span data-stu-id="7cf72-201">Set up Azure Active Directory for client authentication</span></span>

<span data-ttu-id="7cf72-202">Azure AD kan organisationer (kallas även klienter) för att hantera användarnas åtkomst till program.</span><span class="sxs-lookup"><span data-stu-id="7cf72-202">Azure AD enables organizations (known as tenants) to manage user access to applications.</span></span> <span data-ttu-id="7cf72-203">Program är indelade i dem med en webbaserad inloggning användargränssnitt och de med inbyggda klientupplevelsen.</span><span class="sxs-lookup"><span data-stu-id="7cf72-203">Applications are divided into those with a web-based sign-in UI and those with a native client experience.</span></span> <span data-ttu-id="7cf72-204">I den här artikeln förutsätter vi att du redan har skapat en klient.</span><span class="sxs-lookup"><span data-stu-id="7cf72-204">In this article, we assume that you have already created a tenant.</span></span> <span data-ttu-id="7cf72-205">Om du inte har börja med att läsa [skaffa en Azure Active Directory-klient][active-directory-howto-tenant].</span><span class="sxs-lookup"><span data-stu-id="7cf72-205">If you have not, start by reading [How to get an Azure Active Directory tenant][active-directory-howto-tenant].</span></span>

<span data-ttu-id="7cf72-206">Ett Service Fabric-kluster erbjuder flera startpunkter till dess hanteringsfunktioner, inklusive den webbaserade [Service Fabric Explorer] [ service-fabric-visualizing-your-cluster] och [Visual Studio] [ service-fabric-manage-application-in-visual-studio].</span><span class="sxs-lookup"><span data-stu-id="7cf72-206">A Service Fabric cluster offers several entry points to its management functionality, including the web-based [Service Fabric Explorer][service-fabric-visualizing-your-cluster] and [Visual Studio][service-fabric-manage-application-in-visual-studio].</span></span> <span data-ttu-id="7cf72-207">Därför kan skapa du två Azure AD-program för att styra åtkomsten till klustret, ett webbprogram och en programspecifika.</span><span class="sxs-lookup"><span data-stu-id="7cf72-207">As a result, you create two Azure AD applications to control access to the cluster, one web application and one native application.</span></span>

<span data-ttu-id="7cf72-208">Vi har skapat en uppsättning Windows PowerShell-skript för att förenkla vissa av stegen som ingår i att konfigurera Azure AD med ett Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="7cf72-208">To simplify some of the steps involved in configuring Azure AD with a Service Fabric cluster, we have created a set of Windows PowerShell scripts.</span></span>

> [!NOTE]
> <span data-ttu-id="7cf72-209">Du måste slutföra följande steg innan du skapar klustret.</span><span class="sxs-lookup"><span data-stu-id="7cf72-209">You must complete the following steps before you create the cluster.</span></span> <span data-ttu-id="7cf72-210">Eftersom skripten förväntas klusternamn och slutpunkter kan värdena bör planeras och värden inte som du redan har skapat.</span><span class="sxs-lookup"><span data-stu-id="7cf72-210">Because the scripts expect cluster names and endpoints, the values should be planned and not values that you have already created.</span></span>

1. <span data-ttu-id="7cf72-211">[Hämta skripten] [ sf-aad-ps-script-download] till datorn.</span><span class="sxs-lookup"><span data-stu-id="7cf72-211">[Download the scripts][sf-aad-ps-script-download] to your computer.</span></span>
2. <span data-ttu-id="7cf72-212">Högerklicka på zip-filen, Välj **egenskaper**, Välj den **avblockera** kryssrutan och klicka sedan på **tillämpa**.</span><span class="sxs-lookup"><span data-stu-id="7cf72-212">Right-click the zip file, select **Properties**, select the **Unblock** check box, and then click **Apply**.</span></span>
3. <span data-ttu-id="7cf72-213">Extrahera zip-filen.</span><span class="sxs-lookup"><span data-stu-id="7cf72-213">Extract the zip file.</span></span>
4. <span data-ttu-id="7cf72-214">Kör `SetupApplications.ps1`, och anger TenantId, klusternamn och WebApplicationReplyUrl som parametrar.</span><span class="sxs-lookup"><span data-stu-id="7cf72-214">Run `SetupApplications.ps1`, and provide the TenantId, ClusterName, and WebApplicationReplyUrl as parameters.</span></span> <span data-ttu-id="7cf72-215">Exempel:</span><span class="sxs-lookup"><span data-stu-id="7cf72-215">For example:</span></span>

    ```powershell
    .\SetupApplications.ps1 -TenantId '690ec069-8200-4068-9d01-5aaf188e557a' -ClusterName 'mycluster' -WebApplicationReplyUrl 'https://mycluster.westus.cloudapp.azure.com:19080/Explorer/index.html'
    ```

    <span data-ttu-id="7cf72-216">Du kan hitta din TenantId genom att köra PowerShell-kommando `Get-AzureSubscription`.</span><span class="sxs-lookup"><span data-stu-id="7cf72-216">You can find your TenantId by executing the PowerShell command `Get-AzureSubscription`.</span></span> <span data-ttu-id="7cf72-217">Kör det här kommandot visar TenantId för varje prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7cf72-217">Executing this command displays the TenantId for every subscription.</span></span>

    <span data-ttu-id="7cf72-218">Klusternamn används som prefix i Azure AD-program som skapats av skriptet.</span><span class="sxs-lookup"><span data-stu-id="7cf72-218">ClusterName is used to prefix the Azure AD applications that are created by the script.</span></span> <span data-ttu-id="7cf72-219">Det behöver inte matcha exakt faktiska klustrets namn.</span><span class="sxs-lookup"><span data-stu-id="7cf72-219">It does not need to match the actual cluster name exactly.</span></span> <span data-ttu-id="7cf72-220">Det är endast avsedd att göra det enklare att mappa Service Fabric-klustret som de som används med Azure AD-artefakter.</span><span class="sxs-lookup"><span data-stu-id="7cf72-220">It is intended only to make it easier to map Azure AD artifacts to the Service Fabric cluster that they're being used with.</span></span>

    <span data-ttu-id="7cf72-221">WebApplicationReplyUrl är standardslutpunkten som Azure AD tillbaka till användarna när de har loggat in.</span><span class="sxs-lookup"><span data-stu-id="7cf72-221">WebApplicationReplyUrl is the default endpoint that Azure AD returns to your users after they finish signing in.</span></span> <span data-ttu-id="7cf72-222">Ange den här slutpunkten som Service Fabric Explorer-slutpunkt för klustret, vilket som standard är:</span><span class="sxs-lookup"><span data-stu-id="7cf72-222">Set this endpoint as the Service Fabric Explorer endpoint for your cluster, which by default is:</span></span>

    <span data-ttu-id="7cf72-223">https://&lt;cluster_domain&gt;: 19080/Explorer</span><span class="sxs-lookup"><span data-stu-id="7cf72-223">https://&lt;cluster_domain&gt;:19080/Explorer</span></span>

    <span data-ttu-id="7cf72-224">Du uppmanas att logga in på ett konto som har administratörsbehörighet för Azure AD-klient.</span><span class="sxs-lookup"><span data-stu-id="7cf72-224">You are prompted to sign in to an account that has administrative privileges for the Azure AD tenant.</span></span> <span data-ttu-id="7cf72-225">När du loggar in skapar skriptet webb- och interna program för att representera Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="7cf72-225">After you sign in, the script creates the web and native applications to represent your Service Fabric cluster.</span></span> <span data-ttu-id="7cf72-226">Om du tittar på klientens program i den [klassiska Azure-portalen][azure-classic-portal], bör du se två nya poster:</span><span class="sxs-lookup"><span data-stu-id="7cf72-226">If you look at the tenant's applications in the [Azure classic portal][azure-classic-portal], you should see two new entries:</span></span>

   * <span data-ttu-id="7cf72-227">*Klusternamn*\_kluster</span><span class="sxs-lookup"><span data-stu-id="7cf72-227">*ClusterName*\_Cluster</span></span>
   * <span data-ttu-id="7cf72-228">*Klusternamn*\_klienten</span><span class="sxs-lookup"><span data-stu-id="7cf72-228">*ClusterName*\_Client</span></span>

   <span data-ttu-id="7cf72-229">Skriptet skriver ut JSON som krävs av Azure Resource Manager-mallen när du skapar klustret i nästa avsnitt, så det är en bra idé att öppna PowerShell-fönstret.</span><span class="sxs-lookup"><span data-stu-id="7cf72-229">The script prints the JSON required by the Azure Resource Manager template when you create the cluster in the next section, so it's a good idea to keep the PowerShell window open.</span></span>

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

## <a name="create-a-service-fabric-cluster-resource-manager-template"></a><span data-ttu-id="7cf72-230">Skapa en mall för Service Fabric-kluster Resource Manager</span><span class="sxs-lookup"><span data-stu-id="7cf72-230">Create a Service Fabric cluster Resource Manager template</span></span>
<span data-ttu-id="7cf72-231">I det här avsnittet används utdata av föregående PowerShell-kommandon i Service Fabric-kluster Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="7cf72-231">In this section, the outputs of the preceding PowerShell commands are used in a Service Fabric cluster Resource Manager template.</span></span>

<span data-ttu-id="7cf72-232">Exempel Resource Manager-mallar är tillgängliga i den [Azure Snabbkurs mallgalleriet på GitHub][azure-quickstart-templates].</span><span class="sxs-lookup"><span data-stu-id="7cf72-232">Sample Resource Manager templates are available in the [Azure quick-start template gallery on GitHub][azure-quickstart-templates].</span></span> <span data-ttu-id="7cf72-233">Dessa mallar kan användas som en startpunkt för mallen för klustret.</span><span class="sxs-lookup"><span data-stu-id="7cf72-233">These templates can be used as a starting point for your cluster template.</span></span>

### <a name="create-the-resource-manager-template"></a><span data-ttu-id="7cf72-234">Skapa Resource Manager-mallen</span><span class="sxs-lookup"><span data-stu-id="7cf72-234">Create the Resource Manager template</span></span>
<span data-ttu-id="7cf72-235">Den här guiden använder den [säker kluster med 5] [ service-fabric-secure-cluster-5-node-1-nodetype] exempel mallen och mallparametrar.</span><span class="sxs-lookup"><span data-stu-id="7cf72-235">This guide uses the [5-node secure cluster][service-fabric-secure-cluster-5-node-1-nodetype] example template and template parameters.</span></span> <span data-ttu-id="7cf72-236">Hämta `azuredeploy.json` och `azuredeploy.parameters.json` till din dator och öppna filer i valfri textredigerare.</span><span class="sxs-lookup"><span data-stu-id="7cf72-236">Download `azuredeploy.json` and `azuredeploy.parameters.json` to your computer and open both files in your favorite text editor.</span></span>

### <a name="add-certificates"></a><span data-ttu-id="7cf72-237">Lägg till certifikat</span><span class="sxs-lookup"><span data-stu-id="7cf72-237">Add certificates</span></span>
<span data-ttu-id="7cf72-238">Du lägga till certifikat i ett kluster Resource Manager-mall genom att referera till nyckelvalvet som innehåller certifikatnycklarna.</span><span class="sxs-lookup"><span data-stu-id="7cf72-238">You add certificates to a cluster Resource Manager template by referencing the key vault that contains the certificate keys.</span></span> <span data-ttu-id="7cf72-239">Vi rekommenderar att du placera nyckelvalvet värden i en parameterfil för Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="7cf72-239">We recommend that you place the key-vault values in a Resource Manager template parameters file.</span></span> <span data-ttu-id="7cf72-240">Gör det håller resurshanteraren mallfilen återanvändbara och utan värden specifika till en distribution.</span><span class="sxs-lookup"><span data-stu-id="7cf72-240">Doing so keeps the Resource Manager template file reusable and free of values specific to a deployment.</span></span>

#### <a name="add-all-certificates-to-the-virtual-machine-scale-set-osprofile"></a><span data-ttu-id="7cf72-241">Lägg till alla certifikat i virtual machine scale set osProfile</span><span class="sxs-lookup"><span data-stu-id="7cf72-241">Add all certificates to the virtual machine scale set osProfile</span></span>
<span data-ttu-id="7cf72-242">Alla certifikat som installeras i klustret måste konfigureras i avsnittet osProfile i scale set-resurs (Microsoft.Compute/virtualMachineScaleSets).</span><span class="sxs-lookup"><span data-stu-id="7cf72-242">Every certificate that's installed in the cluster must be configured in the osProfile section of the scale set resource (Microsoft.Compute/virtualMachineScaleSets).</span></span> <span data-ttu-id="7cf72-243">Den här åtgärden instruerar resursprovidern att installera certifikatet på virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="7cf72-243">This action instructs the resource provider to install the certificate on the VMs.</span></span> <span data-ttu-id="7cf72-244">Den här installationen innehåller både certifikatet kluster och alla program security-certifikat som du planerar att använda för dina program:</span><span class="sxs-lookup"><span data-stu-id="7cf72-244">This installation includes both the cluster certificate and any application security certificates that you plan to use for your applications:</span></span>

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

#### <a name="configure-the-service-fabric-cluster-certificate"></a><span data-ttu-id="7cf72-245">Konfigurera certifikat för Service Fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="7cf72-245">Configure the Service Fabric cluster certificate</span></span>
<span data-ttu-id="7cf72-246">Certifikatet för autentisering av klustret måste konfigureras i båda Service Fabric klusterresursen (Microsoft.ServiceFabric/clusters) och Service Fabric-tillägget för virtuell dator skala anger i den virtuella dator skala set resursen.</span><span class="sxs-lookup"><span data-stu-id="7cf72-246">The cluster authentication certificate must be configured in both the Service Fabric cluster resource (Microsoft.ServiceFabric/clusters) and the Service Fabric extension for virtual machine scale sets in the virtual machine scale set resource.</span></span> <span data-ttu-id="7cf72-247">Den här ordningen kan Service Fabric-resursprovidern så att konfigurera den som ska användas för autentisering för klustret och serverautentisering för av hanteringsslutpunkter.</span><span class="sxs-lookup"><span data-stu-id="7cf72-247">This arrangement allows the Service Fabric resource provider to configure it for use for cluster authentication and server authentication for management endpoints.</span></span>

##### <a name="virtual-machine-scale-set-resource"></a><span data-ttu-id="7cf72-248">Virtuella skaluppsättning för resursen:</span><span class="sxs-lookup"><span data-stu-id="7cf72-248">Virtual machine scale set resource:</span></span>
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

##### <a name="service-fabric-resource"></a><span data-ttu-id="7cf72-249">Service Fabric-resurs:</span><span class="sxs-lookup"><span data-stu-id="7cf72-249">Service Fabric resource:</span></span>
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

### <a name="insert-azure-ad-configuration"></a><span data-ttu-id="7cf72-250">Infoga Azure AD-konfiguration</span><span class="sxs-lookup"><span data-stu-id="7cf72-250">Insert Azure AD configuration</span></span>
<span data-ttu-id="7cf72-251">Azure AD-konfiguration som du skapade tidigare kan infogas direkt i Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="7cf72-251">The Azure AD configuration that you created earlier can be inserted directly into your Resource Manager template.</span></span> <span data-ttu-id="7cf72-252">Vi rekommenderar dock att du först extrahera värdena i en fil med parametrar att hålla resurshanteraren mall kan återanvändas och fria från värden som är specifika för en distribution.</span><span class="sxs-lookup"><span data-stu-id="7cf72-252">However, we recommended that you first extract the values into a parameters file to keep the Resource Manager template reusable and free of values specific to a deployment.</span></span>

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

### <span data-ttu-id="7cf72-253">< en ”konfigurera arm” ></a>konfigurera Hanteraren för filserverresurser mallparametrar</span><span class="sxs-lookup"><span data-stu-id="7cf72-253"><a "configure-arm" ></a>Configure Resource Manager template parameters</span></span>
<!--- Loc Comment: It seems that <a "configure-arm" > must be replaced with <a name="configure-arm"></a> since the link seems not to be redirecting correctly --->
<span data-ttu-id="7cf72-254">Använd slutligen utdatavärden från nyckelvalvet och Azure AD PowerShell-kommandon för att fylla i parameterfilen:</span><span class="sxs-lookup"><span data-stu-id="7cf72-254">Finally, use the output values from the key vault and Azure AD PowerShell commands to populate the parameters file:</span></span>

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
<span data-ttu-id="7cf72-255">Du bör nu ha följande element på plats:</span><span class="sxs-lookup"><span data-stu-id="7cf72-255">At this point, you should have the following elements in place:</span></span>

* <span data-ttu-id="7cf72-256">Nyckelvalv resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="7cf72-256">Key vault resource group</span></span>
  * <span data-ttu-id="7cf72-257">Nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="7cf72-257">Key vault</span></span>
  * <span data-ttu-id="7cf72-258">Klustret certifikat för serverautentisering</span><span class="sxs-lookup"><span data-stu-id="7cf72-258">Cluster server authentication certificate</span></span>
  * <span data-ttu-id="7cf72-259">Data chiffrering av certifikat</span><span class="sxs-lookup"><span data-stu-id="7cf72-259">Data encipherment certificate</span></span>
* <span data-ttu-id="7cf72-260">Azure Active Directory-klient</span><span class="sxs-lookup"><span data-stu-id="7cf72-260">Azure Active Directory tenant</span></span>
  * <span data-ttu-id="7cf72-261">Azure AD-program för Webbaserad hantering och Service Fabric Explorer</span><span class="sxs-lookup"><span data-stu-id="7cf72-261">Azure AD application for web-based management and Service Fabric Explorer</span></span>
  * <span data-ttu-id="7cf72-262">Azure AD-program för interna klienthantering</span><span class="sxs-lookup"><span data-stu-id="7cf72-262">Azure AD application for native client management</span></span>
  * <span data-ttu-id="7cf72-263">Användare och deras tilldelade roller</span><span class="sxs-lookup"><span data-stu-id="7cf72-263">Users and their assigned roles</span></span>
* <span data-ttu-id="7cf72-264">Service Fabric-kluster Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="7cf72-264">Service Fabric cluster Resource Manager template</span></span>
  * <span data-ttu-id="7cf72-265">Certifikat konfigureras via nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="7cf72-265">Certificates configured through key vault</span></span>
  * <span data-ttu-id="7cf72-266">Azure Active Directory konfigurerat</span><span class="sxs-lookup"><span data-stu-id="7cf72-266">Azure Active Directory configured</span></span>

<span data-ttu-id="7cf72-267">Följande diagram illustrerar där ditt nyckelvalv och Azure AD-konfiguration passar i Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="7cf72-267">The following diagram illustrates where your key vault and Azure AD configuration fit into your Resource Manager template.</span></span>

![Hanteraren för filserverresurser beroendekarta för anslutningar][cluster-security-arm-dependency-map]

## <a name="create-the-cluster"></a><span data-ttu-id="7cf72-269">Skapa klustret</span><span class="sxs-lookup"><span data-stu-id="7cf72-269">Create the cluster</span></span>
<span data-ttu-id="7cf72-270">Du är nu redo att skapa klustret med hjälp av [Azure-resurs malldistribution][resource-group-template-deploy].</span><span class="sxs-lookup"><span data-stu-id="7cf72-270">You are now ready to create the cluster by using [Azure resource template deployment][resource-group-template-deploy].</span></span>

#### <a name="test-it"></a><span data-ttu-id="7cf72-271">testa den</span><span class="sxs-lookup"><span data-stu-id="7cf72-271">Test it</span></span>
<span data-ttu-id="7cf72-272">Använd följande PowerShell-kommando för att testa Resource Manager-mall med en fil med parametrar:</span><span class="sxs-lookup"><span data-stu-id="7cf72-272">Use the following PowerShell command to test your Resource Manager template with a parameters file:</span></span>

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

#### <a name="deploy-it"></a><span data-ttu-id="7cf72-273">distribuera den</span><span class="sxs-lookup"><span data-stu-id="7cf72-273">Deploy it</span></span>
<span data-ttu-id="7cf72-274">Om hanteraren för filserverresurser mallen testet lyckas, kan du använda följande PowerShell-kommando för att distribuera Resource Manager-mall med en fil med parametrar:</span><span class="sxs-lookup"><span data-stu-id="7cf72-274">If the Resource Manager template test passes, use the following PowerShell command to deploy your Resource Manager template with a parameters file:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

<a name="assign-roles"></a>

## <a name="assign-users-to-roles"></a><span data-ttu-id="7cf72-275">Tilldela användare till roller</span><span class="sxs-lookup"><span data-stu-id="7cf72-275">Assign users to roles</span></span>
<span data-ttu-id="7cf72-276">När du har skapat de program som ska representera klustret tilldelar användarna till de roller som stöds av Service Fabric: skrivskyddade och administratör. Du kan tilldela roller med hjälp av den [klassiska Azure-portalen][azure-classic-portal].</span><span class="sxs-lookup"><span data-stu-id="7cf72-276">After you have created the applications to represent your cluster, assign your users to the roles supported by Service Fabric: read-only and admin. You can assign the roles by using the [Azure classic portal][azure-classic-portal].</span></span>

1. <span data-ttu-id="7cf72-277">Gå till din klient i Azure-portalen och väljer sedan **program**.</span><span class="sxs-lookup"><span data-stu-id="7cf72-277">In the Azure portal, go to your tenant, and then select **Applications**.</span></span>
2. <span data-ttu-id="7cf72-278">Välj webbprogram som har ett namn som `myTestCluster_Cluster`.</span><span class="sxs-lookup"><span data-stu-id="7cf72-278">Select the web application, which has a name like `myTestCluster_Cluster`.</span></span>
3. <span data-ttu-id="7cf72-279">Klicka på den **användare** fliken.</span><span class="sxs-lookup"><span data-stu-id="7cf72-279">Click the **Users** tab.</span></span>
4. <span data-ttu-id="7cf72-280">Välj en användare att tilldela och klicka sedan på den **tilldela** längst ned på skärmen.</span><span class="sxs-lookup"><span data-stu-id="7cf72-280">Select a user to assign, and then click the **Assign** button at the bottom of the screen.</span></span>

    ![Tilldela användare till roller knappen][assign-users-to-roles-button]
5. <span data-ttu-id="7cf72-282">Välj rollen för att tilldela användaren.</span><span class="sxs-lookup"><span data-stu-id="7cf72-282">Select the role to assign to the user.</span></span>

    ![”Tilldela användare” dialogrutan][assign-users-to-roles-dialog]

> [!NOTE]
> <span data-ttu-id="7cf72-284">Mer information om roller i Service Fabric finns [rollbaserad åtkomstkontroll för Service Fabric-klienter](service-fabric-cluster-security-roles.md).</span><span class="sxs-lookup"><span data-stu-id="7cf72-284">For more information about roles in Service Fabric, see [Role-based access control for Service Fabric clients](service-fabric-cluster-security-roles.md).</span></span>
>
>

 <a name="secure-linux-clusters"></a>
 <!--- Loc Comment: It seems that letter S in cluster was missing, which caused the wrong redirection of the link --->

## <a name="create-secure-clusters-on-linux"></a><span data-ttu-id="7cf72-285">Skapa skyddade kluster på Linux</span><span class="sxs-lookup"><span data-stu-id="7cf72-285">Create secure clusters on Linux</span></span>
<span data-ttu-id="7cf72-286">Du kan förenkla processen har vi angett ett [helper skriptet](http://github.com/ChackDan/Service-Fabric/tree/master/Scripts/CertUpload4Linux).</span><span class="sxs-lookup"><span data-stu-id="7cf72-286">To make the process easier, we have provided a [helper script](http://github.com/ChackDan/Service-Fabric/tree/master/Scripts/CertUpload4Linux).</span></span> <span data-ttu-id="7cf72-287">Innan du använder skriptet helper se till att du redan har Azure-kommandoradsgränssnittet (CLI) installerad och den är i din sökväg.</span><span class="sxs-lookup"><span data-stu-id="7cf72-287">Before you use this helper script, ensure that you already have Azure command-line interface (CLI) installed, and it is in your path.</span></span> <span data-ttu-id="7cf72-288">Kontrollera att skriptet har behörighet att köra genom att köra `chmod +x cert_helper.py` när du har hämtat den.</span><span class="sxs-lookup"><span data-stu-id="7cf72-288">Make sure that the script has permissions to execute by running `chmod +x cert_helper.py` after downloading it.</span></span> <span data-ttu-id="7cf72-289">Det första steget är att logga in på ditt Azure-konto med hjälp av CLI med den `azure login` kommando.</span><span class="sxs-lookup"><span data-stu-id="7cf72-289">The first step is to sign in to your Azure account by using CLI with the `azure login` command.</span></span> <span data-ttu-id="7cf72-290">När du loggar in på ditt Azure-konto, använder du helper skriptet med din CA-signerat certifikat, så som visas i följande kommando:</span><span class="sxs-lookup"><span data-stu-id="7cf72-290">After signing in to your Azure account, use the helper script with your CA signed certificate, as the following command shows:</span></span>

```sh
./cert_helper.py [-h] CERT_TYPE [-ifile INPUT_CERT_FILE] [-sub SUBSCRIPTION_ID] [-rgname RESOURCE_GROUP_NAME] [-kv KEY_VAULT_NAME] [-sname CERTIFICATE_NAME] [-l LOCATION] [-p PASSWORD]
```

<span data-ttu-id="7cf72-291">Parametern - ifile kan ta en .pfx-fil eller en PEM-filen som indata med certifikattyp (pfx eller pem eller ss om det är ett självsignerat certifikat).</span><span class="sxs-lookup"><span data-stu-id="7cf72-291">The -ifile parameter can take a .pfx file or a .pem file as input, with the certificate type (pfx or pem, or ss if it is a self-signed certificate).</span></span>
<span data-ttu-id="7cf72-292">I parametern -h skriver ut den hjälptext som visas.</span><span class="sxs-lookup"><span data-stu-id="7cf72-292">The parameter -h prints out the help text.</span></span>


<span data-ttu-id="7cf72-293">Det här kommandot returnerar följande tre strängar som utdata:</span><span class="sxs-lookup"><span data-stu-id="7cf72-293">This command returns the following three strings as the output:</span></span>

* <span data-ttu-id="7cf72-294">SourceVaultID, vilket är ID för nya KeyVault ResourceGroup den skapas automatiskt</span><span class="sxs-lookup"><span data-stu-id="7cf72-294">SourceVaultID, which is the ID for the new KeyVault ResourceGroup it created for you</span></span>
* <span data-ttu-id="7cf72-295">CertificateUrl för åtkomst till certifikatet</span><span class="sxs-lookup"><span data-stu-id="7cf72-295">CertificateUrl for accessing the certificate</span></span>
* <span data-ttu-id="7cf72-296">CertificateThumbprint som används för autentisering</span><span class="sxs-lookup"><span data-stu-id="7cf72-296">CertificateThumbprint, which is used for authentication</span></span>

<span data-ttu-id="7cf72-297">I följande exempel visas hur du använder kommandot:</span><span class="sxs-lookup"><span data-stu-id="7cf72-297">The following example shows how to use the command:</span></span>

```sh
./cert_helper.py pfx -sub "fffffff-ffff-ffff-ffff-ffffffffffff"  -rgname "mykvrg" -kv "mykevname" -ifile "/home/test/cert.pfx" -sname "mycert" -l "East US" -p "pfxtest"
```
<span data-ttu-id="7cf72-298">Kör kommandot ovan ger dig de tre strängarna på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="7cf72-298">Executing the preceding command gives you the three strings as follows:</span></span>

```sh
SourceVault: /subscriptions/fffffff-ffff-ffff-ffff-ffffffffffff/resourceGroups/mykvrg/providers/Microsoft.KeyVault/vaults/mykvname
CertificateUrl: https://myvault.vault.azure.net/secrets/mycert/00000000000000000000000000000000
CertificateThumbprint: 0xfffffffffffffffffffffffffffffffffffffffff
```

<span data-ttu-id="7cf72-299">Certifikatets ämnesnamn måste matcha den domän som du använder för att få åtkomst till Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="7cf72-299">The certificate's subject name must match the domain that you use to access the Service Fabric cluster.</span></span> <span data-ttu-id="7cf72-300">Matchningen krävs för att tillhandahålla en SSL för klustrets HTTPS management slutpunkter och Service Fabric Explorer.</span><span class="sxs-lookup"><span data-stu-id="7cf72-300">This match is required to provide an SSL for the cluster's HTTPS management endpoints and Service Fabric Explorer.</span></span> <span data-ttu-id="7cf72-301">Du kan skaffa ett SSL-certifikat från en Certifikatutfärdare för den `.cloudapp.azure.com` domän.</span><span class="sxs-lookup"><span data-stu-id="7cf72-301">You cannot obtain an SSL certificate from a CA for the `.cloudapp.azure.com` domain.</span></span> <span data-ttu-id="7cf72-302">Du måste skaffa ett anpassat domännamn för klustret.</span><span class="sxs-lookup"><span data-stu-id="7cf72-302">You must obtain a custom domain name for your cluster.</span></span> <span data-ttu-id="7cf72-303">När du begär ett certifikat från en Certifikatutfärdare måste certifikatets ämnesnamn matcha det anpassade domännamnet som du använder för klustret.</span><span class="sxs-lookup"><span data-stu-id="7cf72-303">When you request a certificate from a CA, the certificate's subject name must match the custom domain name that you use for your cluster.</span></span>

<span data-ttu-id="7cf72-304">Dessa ämnesnamn är transaktioner som du behöver skapa en säker Service Fabric-klustret (utan Azure AD), enligt beskrivningen i [konfigurera Hanteraren för filserverresurser mallparametrar](#configure-arm).</span><span class="sxs-lookup"><span data-stu-id="7cf72-304">These subject names are the entries you need to create a secure Service Fabric cluster (without Azure AD), as described at [Configure Resource Manager template parameters](#configure-arm).</span></span> <span data-ttu-id="7cf72-305">Du kan ansluta till det säkra klustret genom att följa anvisningarna för [autentisera klientåtkomst till ett kluster](service-fabric-connect-to-secure-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="7cf72-305">You can connect to the secure cluster by following the instructions for [authenticating client access to a cluster](service-fabric-connect-to-secure-cluster.md).</span></span> <span data-ttu-id="7cf72-306">Linux preview kluster stöder inte Azure AD-autentisering.</span><span class="sxs-lookup"><span data-stu-id="7cf72-306">Linux preview clusters do not support Azure AD authentication.</span></span> <span data-ttu-id="7cf72-307">Du kan tilldela roller admin och klienten enligt beskrivningen i den [tilldela roller till användare](#assign-roles) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7cf72-307">You can assign admin and client roles as described in the [Assign roles to users](#assign-roles) section.</span></span> <span data-ttu-id="7cf72-308">Du måste ange tumavtryck för certifikatet för autentisering när du anger admin och klienten roller för en Linux-kluster för förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="7cf72-308">When you specify admin and client roles for a Linux preview cluster, you have to provide certificate thumbprints for authentication.</span></span> <span data-ttu-id="7cf72-309">(Du inte anger namnet på certifikatmottagaren eftersom ingen verifiering av certifikatkedjan eller återkallade utförs i den här förhandsversionen.)</span><span class="sxs-lookup"><span data-stu-id="7cf72-309">(You do not provide the subject name, because no chain validation or revocation is being performed in this preview release.)</span></span>

<span data-ttu-id="7cf72-310">Du kan använda samma skript för att generera en om du vill använda ett självsignerat certifikat för testning.</span><span class="sxs-lookup"><span data-stu-id="7cf72-310">If you want to use a self-signed certificate for testing, you can use the same script to generate one.</span></span> <span data-ttu-id="7cf72-311">Du kan sedan överföra certifikatet till nyckelvalvet genom att ange flaggan `ss` i stället för att ange certifikatets sökväg och certifikatets namn.</span><span class="sxs-lookup"><span data-stu-id="7cf72-311">You can then upload the certificate to your key vault by providing the flag `ss` instead of providing the certificate path and certificate name.</span></span> <span data-ttu-id="7cf72-312">Se exempelvis följande kommando för att skapa och ladda upp ett självsignerat certifikat:</span><span class="sxs-lookup"><span data-stu-id="7cf72-312">For example, see the following command for creating and uploading a self-signed certificate:</span></span>

```sh
./cert_helper.py ss -rgname "mykvrg" -sub "fffffff-ffff-ffff-ffff-ffffffffffff" -kv "mykevname"   -sname "mycert" -l "East US" -p "selftest" -subj "mytest.eastus.cloudapp.net"
```
<span data-ttu-id="7cf72-313">Det här kommandot returnerar samma tre strängar: SourceVault, CertificateUrl och CertificateThumbprint.</span><span class="sxs-lookup"><span data-stu-id="7cf72-313">This command returns the same three strings: SourceVault, CertificateUrl, and CertificateThumbprint.</span></span> <span data-ttu-id="7cf72-314">Du kan sedan använda strängarna för att skapa både en säker Linux-kluster och en plats där det självsignerade certifikatet är placerad.</span><span class="sxs-lookup"><span data-stu-id="7cf72-314">You can then use the strings to create both a secure Linux cluster and a location where the self-signed certificate is placed.</span></span> <span data-ttu-id="7cf72-315">Du måste det självsignerade certifikatet för att ansluta till klustret.</span><span class="sxs-lookup"><span data-stu-id="7cf72-315">You need the self-signed certificate to connect to the cluster.</span></span> <span data-ttu-id="7cf72-316">Du kan ansluta till det säkra klustret genom att följa anvisningarna för [autentisera klientåtkomst till ett kluster](service-fabric-connect-to-secure-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="7cf72-316">You can connect to the secure cluster by following the instructions for [authenticating client access to a cluster](service-fabric-connect-to-secure-cluster.md).</span></span>

<span data-ttu-id="7cf72-317">Certifikatets ämnesnamn måste matcha den domän som du använder för att få åtkomst till Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="7cf72-317">The certificate's subject name must match the domain that you use to access the Service Fabric cluster.</span></span> <span data-ttu-id="7cf72-318">Matchningen krävs för att tillhandahålla en SSL för klustrets HTTPS management slutpunkter och Service Fabric Explorer.</span><span class="sxs-lookup"><span data-stu-id="7cf72-318">This match is required to provide an SSL for the cluster's HTTPS management endpoints and Service Fabric Explorer.</span></span> <span data-ttu-id="7cf72-319">Du kan skaffa ett SSL-certifikat från en Certifikatutfärdare för den `.cloudapp.azure.com` domän.</span><span class="sxs-lookup"><span data-stu-id="7cf72-319">You cannot obtain an SSL certificate from a CA for the `.cloudapp.azure.com` domain.</span></span> <span data-ttu-id="7cf72-320">Du måste skaffa ett anpassat domännamn för klustret.</span><span class="sxs-lookup"><span data-stu-id="7cf72-320">You must obtain a custom domain name for your cluster.</span></span> <span data-ttu-id="7cf72-321">När du begär ett certifikat från en Certifikatutfärdare måste certifikatets ämnesnamn matcha det anpassade domännamnet som du använder för klustret.</span><span class="sxs-lookup"><span data-stu-id="7cf72-321">When you request a certificate from a CA, the certificate's subject name must match the custom domain name that you use for your cluster.</span></span>

<span data-ttu-id="7cf72-322">Du kan fylla parametrarna från helper-skriptet i Azure-portalen, enligt beskrivningen i den [skapa ett kluster i Azure portal](service-fabric-cluster-creation-via-portal.md#create-cluster-in-the-azure-portal) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7cf72-322">You can fill the parameters from the helper script in the Azure portal, as described in the [Create a cluster in the Azure portal](service-fabric-cluster-creation-via-portal.md#create-cluster-in-the-azure-portal) section.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7cf72-323">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7cf72-323">Next steps</span></span>
<span data-ttu-id="7cf72-324">Du har nu en säker kluster med Azure Active Directory med management-autentisering.</span><span class="sxs-lookup"><span data-stu-id="7cf72-324">At this point, you have a secure cluster with Azure Active Directory providing management authentication.</span></span> <span data-ttu-id="7cf72-325">Nästa [ansluta till klustret](service-fabric-connect-to-secure-cluster.md) och lära dig hur du [hantera program hemligheter](service-fabric-application-secret-management.md).</span><span class="sxs-lookup"><span data-stu-id="7cf72-325">Next, [connect to your cluster](service-fabric-connect-to-secure-cluster.md) and learn how to [manage application secrets](service-fabric-application-secret-management.md).</span></span>

## <a name="troubleshoot-setting-up-azure-active-directory-for-client-authentication"></a><span data-ttu-id="7cf72-326">Felsöka ställa in Azure Active Directory för klientautentisering</span><span class="sxs-lookup"><span data-stu-id="7cf72-326">Troubleshoot setting up Azure Active Directory for client authentication</span></span>
<span data-ttu-id="7cf72-327">Om du stöter på ett problem när du konfigurerar Azure AD för klientautentisering, granska möjliga lösningar i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="7cf72-327">If you run into an issue while you're setting up Azure AD for client authentication, review the potential solutions in this section.</span></span>

### <a name="service-fabric-explorer-prompts-you-to-select-a-certificate"></a><span data-ttu-id="7cf72-328">Service Fabric Explorer uppmanar dig att välja ett certifikat</span><span class="sxs-lookup"><span data-stu-id="7cf72-328">Service Fabric Explorer prompts you to select a certificate</span></span>
#### <a name="problem"></a><span data-ttu-id="7cf72-329">Problem</span><span class="sxs-lookup"><span data-stu-id="7cf72-329">Problem</span></span>
<span data-ttu-id="7cf72-330">När du lyckas logga in till Azure AD i Service Fabric Explorer webbläsaren tillbaka till startsidan men uppmanas du att välja ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="7cf72-330">After you sign in successfully to Azure AD in Service Fabric Explorer, the browser returns to the home page but a message prompts you to select a certificate.</span></span>

![Dialogrutan för SFX Välj certifikat][sfx-select-certificate-dialog]

#### <a name="reason"></a><span data-ttu-id="7cf72-332">Orsak</span><span class="sxs-lookup"><span data-stu-id="7cf72-332">Reason</span></span>
<span data-ttu-id="7cf72-333">Användaren är inte tilldelad en roll i Azure AD-programmet för klustret.</span><span class="sxs-lookup"><span data-stu-id="7cf72-333">The user isn’t assigned a role in the Azure AD cluster application.</span></span> <span data-ttu-id="7cf72-334">Azure AD-autentiseringen misslyckas därför på Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="7cf72-334">Thus, Azure AD authentication fails on Service Fabric cluster.</span></span> <span data-ttu-id="7cf72-335">Service Fabric Explorer faller tillbaka till certifikatautentisering.</span><span class="sxs-lookup"><span data-stu-id="7cf72-335">Service Fabric Explorer falls back to certificate authentication.</span></span>

#### <a name="solution"></a><span data-ttu-id="7cf72-336">Lösning</span><span class="sxs-lookup"><span data-stu-id="7cf72-336">Solution</span></span>
<span data-ttu-id="7cf72-337">Följ instruktionerna för att konfigurera Azure AD och tilldela användarroller.</span><span class="sxs-lookup"><span data-stu-id="7cf72-337">Follow the instructions for setting up Azure AD, and assign user roles.</span></span> <span data-ttu-id="7cf72-338">Dessutom vi rekommenderar att du aktiverar ”Användartilldelning krävs för att komma åt appen”, som `SetupApplications.ps1` har.</span><span class="sxs-lookup"><span data-stu-id="7cf72-338">Also, we recommend that you turn on “User assignment required to access app,” as `SetupApplications.ps1` does.</span></span>

### <a name="connection-with-powershell-fails-with-an-error-the-specified-credentials-are-invalid"></a><span data-ttu-id="7cf72-339">Anslutning med PowerShell misslyckas med felmeddelandet: ”de angivna autentiseringsuppgifterna är ogiltiga”</span><span class="sxs-lookup"><span data-stu-id="7cf72-339">Connection with PowerShell fails with an error: "The specified credentials are invalid"</span></span>
#### <a name="problem"></a><span data-ttu-id="7cf72-340">Problem</span><span class="sxs-lookup"><span data-stu-id="7cf72-340">Problem</span></span>
<span data-ttu-id="7cf72-341">När du använder PowerShell för att ansluta till klustret med hjälp av ”AzureActiveDirectory” säkerhetsläget när du loggar in har i Azure AD, om anslutningen misslyckas med felmeddelandet: ”de angivna autentiseringsuppgifterna är ogiltiga”.</span><span class="sxs-lookup"><span data-stu-id="7cf72-341">When you use PowerShell to connect to the cluster by using “AzureActiveDirectory” security mode, after you sign in successfully to Azure AD, the connection fails with an error: "The specified credentials are invalid."</span></span>

#### <a name="solution"></a><span data-ttu-id="7cf72-342">Lösning</span><span class="sxs-lookup"><span data-stu-id="7cf72-342">Solution</span></span>
<span data-ttu-id="7cf72-343">Den här lösningen är samma som det föregående.</span><span class="sxs-lookup"><span data-stu-id="7cf72-343">This solution is the same as the preceding one.</span></span>

### <a name="service-fabric-explorer-returns-a-failure-when-you-sign-in-aadsts50011"></a><span data-ttu-id="7cf72-344">Service Fabric Explorer returnerar ett fel när du loggar in: ”AADSTS50011”</span><span class="sxs-lookup"><span data-stu-id="7cf72-344">Service Fabric Explorer returns a failure when you sign in: "AADSTS50011"</span></span>
#### <a name="problem"></a><span data-ttu-id="7cf72-345">Problem</span><span class="sxs-lookup"><span data-stu-id="7cf72-345">Problem</span></span>
<span data-ttu-id="7cf72-346">När du försöker logga in på Azure AD i Service Fabric Explorer sidan returnerar ett fel ”: AADSTS50011: svarsadressen &lt;url&gt; matchar inte reply-adresser som har konfigurerats för programmet: &lt;guid&gt;”.</span><span class="sxs-lookup"><span data-stu-id="7cf72-346">When you try to sign in to Azure AD in Service Fabric Explorer, the page returns a failure: "AADSTS50011: The reply address &lt;url&gt; does not match the reply addresses configured for the application: &lt;guid&gt;."</span></span>

![SFX svarsadressen matchar inte][sfx-reply-address-not-match]

#### <a name="reason"></a><span data-ttu-id="7cf72-348">Orsak</span><span class="sxs-lookup"><span data-stu-id="7cf72-348">Reason</span></span>
<span data-ttu-id="7cf72-349">Klustret (webb) som representerar Service Fabric Explorer försöker autentisera mot Azure AD och som en del av begäran ger den returnera omdirigerings-URL.</span><span class="sxs-lookup"><span data-stu-id="7cf72-349">The cluster (web) application that represents Service Fabric Explorer attempts to authenticate against Azure AD, and as part of the request it provides the redirect return URL.</span></span> <span data-ttu-id="7cf72-350">Men URL-Adressen ingår inte i Azure AD-programmet **REPLY URL** lista.</span><span class="sxs-lookup"><span data-stu-id="7cf72-350">But the URL is not listed in the Azure AD application **REPLY URL** list.</span></span>

#### <a name="solution"></a><span data-ttu-id="7cf72-351">Lösning</span><span class="sxs-lookup"><span data-stu-id="7cf72-351">Solution</span></span>
<span data-ttu-id="7cf72-352">På den **konfigurera** fliken i klustret (webb)-programmet att lägga till URL: en för Service Fabric Explorer till den **REPLY URL** listan eller ersätta ett av objekten i listan.</span><span class="sxs-lookup"><span data-stu-id="7cf72-352">On the **Configure** tab of the cluster (web) application, add the URL of Service Fabric Explorer to the **REPLY URL** list or replace one of the items in the list.</span></span> <span data-ttu-id="7cf72-353">När du är klar kan du spara ändringen.</span><span class="sxs-lookup"><span data-stu-id="7cf72-353">When you have finished, save your change.</span></span>

![Url till webbprogram svar][web-application-reply-url]

### <a name="connect-the-cluster-by-using-azure-ad-authentication-via-powershell"></a><span data-ttu-id="7cf72-355">Ansluta till klustret med hjälp av Azure AD-autentisering via PowerShell</span><span class="sxs-lookup"><span data-stu-id="7cf72-355">Connect the cluster by using Azure AD authentication via PowerShell</span></span>
<span data-ttu-id="7cf72-356">Använd följande PowerShell-kommandot exempel för att ansluta Service Fabric-kluster:</span><span class="sxs-lookup"><span data-stu-id="7cf72-356">To connect the Service Fabric cluster, use the following PowerShell command example:</span></span>

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <endpoint> -KeepAliveIntervalInSec 10 -AzureActiveDirectory -ServerCertThumbprint <thumbprint>
```

<span data-ttu-id="7cf72-357">Läs om cmdlet Connect-ServiceFabricCluster i [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx).</span><span class="sxs-lookup"><span data-stu-id="7cf72-357">To learn about the Connect-ServiceFabricCluster cmdlet, see [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx).</span></span>

### <a name="can-i-reuse-the-same-azure-ad-tenant-in-multiple-clusters"></a><span data-ttu-id="7cf72-358">Kan jag återanvända samma Azure AD-klienten i flera kluster?</span><span class="sxs-lookup"><span data-stu-id="7cf72-358">Can I reuse the same Azure AD tenant in multiple clusters?</span></span>
<span data-ttu-id="7cf72-359">Ja.</span><span class="sxs-lookup"><span data-stu-id="7cf72-359">Yes.</span></span> <span data-ttu-id="7cf72-360">Men kom ihåg att lägga till URL: en för Service Fabric Explorer i ditt program i klustret (webb).</span><span class="sxs-lookup"><span data-stu-id="7cf72-360">But remember to add the URL of Service Fabric Explorer to your cluster (web) application.</span></span> <span data-ttu-id="7cf72-361">Annars fungerar Service Fabric Explorer inte.</span><span class="sxs-lookup"><span data-stu-id="7cf72-361">Otherwise, Service Fabric Explorer doesn’t work.</span></span>

### <a name="why-do-i-still-need-a-server-certificate-while-azure-ad-is-enabled"></a><span data-ttu-id="7cf72-362">Varför måste jag göra fortfarande ett servercertifikat medan Azure AD är aktiverad?</span><span class="sxs-lookup"><span data-stu-id="7cf72-362">Why do I still need a server certificate while Azure AD is enabled?</span></span>
<span data-ttu-id="7cf72-363">FabricClient och FabricGateway utför en ömsesidig autentisering.</span><span class="sxs-lookup"><span data-stu-id="7cf72-363">FabricClient and FabricGateway perform a mutual authentication.</span></span> <span data-ttu-id="7cf72-364">Azure AD-integreringen ger en klientens identitet till servern under Azure AD-autentisering, och certifikatet används för att kontrollera serveridentitet.</span><span class="sxs-lookup"><span data-stu-id="7cf72-364">During Azure AD authentication, Azure AD integration provides a client identity to the server, and the server certificate is used to verify the server identity.</span></span> <span data-ttu-id="7cf72-365">Mer information om Service Fabric-certifikat finns [X.509-certifikat och Service Fabric][x509-certificates-and-service-fabric].</span><span class="sxs-lookup"><span data-stu-id="7cf72-365">For more information about Service Fabric certificates, see [X.509 certificates and Service Fabric][x509-certificates-and-service-fabric].</span></span>

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

