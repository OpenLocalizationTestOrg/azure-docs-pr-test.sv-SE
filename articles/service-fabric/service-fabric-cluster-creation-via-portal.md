---
title: Skapa Service Fabric-kluster i Azure portal | Microsoft Docs
description: "Den här artikeln beskriver hur du ställer in en säker Service Fabric-kluster i Azure med Azure-portalen och Azure Key Vault."
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
ms.openlocfilehash: 7dda9520ce3d93bf0e86bd2481ad06c268d087c7
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-service-fabric-cluster-in-azure-using-the-azure-portal"></a><span data-ttu-id="262be-103">Skapa ett Service Fabric-kluster i Azure med Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="262be-103">Create a Service Fabric cluster in Azure using the Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="262be-104">Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="262be-104">Azure Resource Manager</span></span>](service-fabric-cluster-creation-via-arm.md)
> * [<span data-ttu-id="262be-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="262be-105">Azure portal</span></span>](service-fabric-cluster-creation-via-portal.md)
> 
> 

<span data-ttu-id="262be-106">Det här är en stegvis guide som vägleder dig genom stegen för att skapa en säker Service Fabric-kluster i Azure med Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="262be-106">This is a step-by-step guide that walks you through the steps of setting up a secure Service Fabric cluster in Azure using the Azure portal.</span></span> <span data-ttu-id="262be-107">Den här guiden vägleder dig genom följande steg:</span><span class="sxs-lookup"><span data-stu-id="262be-107">This guide walks you through the following steps:</span></span>

* <span data-ttu-id="262be-108">Konfigurera Key Vault för att hantera nycklar för säkerhet för klustret.</span><span class="sxs-lookup"><span data-stu-id="262be-108">Set up Key Vault to manage keys for cluster security.</span></span>
* <span data-ttu-id="262be-109">Skapa ett skyddat kluster i Azure via Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="262be-109">Create a secured cluster in Azure through the Azure portal.</span></span>
* <span data-ttu-id="262be-110">Autentisera administratörer som använder certifikat.</span><span class="sxs-lookup"><span data-stu-id="262be-110">Authenticate administrators using certificates.</span></span>

> [!NOTE]
> <span data-ttu-id="262be-111">För mer avancerade säkerhetsalternativ, till exempel autentisering med Azure Active Directory och hur du konfigurerar certifikat för programmet säkerhet [skapar ett kluster med Azure Resource Manager][create-cluster-arm].</span><span class="sxs-lookup"><span data-stu-id="262be-111">For more advanced security options, such as user authentication with Azure Active Directory and setting up certificates for application security, [create your cluster using Azure Resource Manager][create-cluster-arm].</span></span>
> 
> 

<span data-ttu-id="262be-112">En säker klustret är ett kluster som förhindrar obehörig åtkomst till hanteringsåtgärder, vilket innefattar distribution, uppgradera och ta bort program, tjänster och de data som de innehåller.</span><span class="sxs-lookup"><span data-stu-id="262be-112">A secure cluster is a cluster that prevents unauthorized access to management operations, which includes deploying, upgrading, and deleting applications, services, and the data they contain.</span></span> <span data-ttu-id="262be-113">Ett oskyddat klustret är ett kluster alla kan ansluta till när som helst och utföra hanteringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="262be-113">An unsecure cluster is a cluster that anyone can connect to at any time and perform management operations.</span></span> <span data-ttu-id="262be-114">Även om det är möjligt att skapa ett oskyddade kluster, är det **rekommenderar vi att skapa en säker kluster**.</span><span class="sxs-lookup"><span data-stu-id="262be-114">Although it is possible to create an unsecure cluster, it is **highly recommended to create a secure cluster**.</span></span> <span data-ttu-id="262be-115">Ett oskyddade kluster **går inte att senare säkra** -måste du skapa ett nytt kluster.</span><span class="sxs-lookup"><span data-stu-id="262be-115">An unsecure cluster **cannot be secured later** - a new cluster must be created.</span></span>

<span data-ttu-id="262be-116">Begreppen är samma för att skapa skyddade kluster om klustren är Linux-kluster eller Windows-kluster.</span><span class="sxs-lookup"><span data-stu-id="262be-116">The concepts are the same for creating secure clusters, whether the clusters are Linux clusters or Windows clusters.</span></span> <span data-ttu-id="262be-117">Mer information och hjälp skript för att skapa säkra Linux-kluster, se [att skapa skyddade kluster på Linux](service-fabric-cluster-creation-via-arm.md#secure-linux-clusters).</span><span class="sxs-lookup"><span data-stu-id="262be-117">For more information and helper scripts for creating secure Linux clusters, please see [Creating secure clusters on Linux](service-fabric-cluster-creation-via-arm.md#secure-linux-clusters).</span></span> <span data-ttu-id="262be-118">De parametrar som erhålls genom helper skriptet som kan mata direkt i portalen enligt beskrivningen i avsnittet [skapa ett kluster i Azure portal](#create-cluster-portal).</span><span class="sxs-lookup"><span data-stu-id="262be-118">The parameters obtained by the helper script provided can be input directly into the portal as described in the section [Create a cluster in the Azure portal](#create-cluster-portal).</span></span>

## <a name="log-in-to-azure"></a><span data-ttu-id="262be-119">Logga in på Azure</span><span class="sxs-lookup"><span data-stu-id="262be-119">Log in to Azure</span></span>
<span data-ttu-id="262be-120">Den här guiden använder [Azure PowerShell][azure-powershell].</span><span class="sxs-lookup"><span data-stu-id="262be-120">This guide uses [Azure PowerShell][azure-powershell].</span></span> <span data-ttu-id="262be-121">När du startar en ny PowerShell-session, logga in på ditt Azure-konto och välja din prenumeration innan du kör kommandon för Azure.</span><span class="sxs-lookup"><span data-stu-id="262be-121">When starting a new PowerShell session, log in to your Azure account and select your subscription before executing Azure commands.</span></span>

<span data-ttu-id="262be-122">Logga in på ditt azure-konto:</span><span class="sxs-lookup"><span data-stu-id="262be-122">Log in to your azure account:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="262be-123">Välj din prenumeration:</span><span class="sxs-lookup"><span data-stu-id="262be-123">Select your subscription:</span></span>

```powershell
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

## <a name="set-up-key-vault"></a><span data-ttu-id="262be-124">Konfigurera Key Vault</span><span class="sxs-lookup"><span data-stu-id="262be-124">Set up Key Vault</span></span>
<span data-ttu-id="262be-125">Den här delen av guiden vägleder dig genom att skapa ett Nyckelvalv för Service Fabric-kluster i Azure och för Service Fabric-program.</span><span class="sxs-lookup"><span data-stu-id="262be-125">This part of the guide walks you through creating a Key Vault for a Service Fabric cluster in Azure and for Service Fabric applications.</span></span> <span data-ttu-id="262be-126">En fullständig guide på Key Vault finns i [Key Vault Kom igång med][key-vault-get-started].</span><span class="sxs-lookup"><span data-stu-id="262be-126">For a complete guide on Key Vault, see the [Key Vault getting started guide][key-vault-get-started].</span></span>

<span data-ttu-id="262be-127">Service Fabric använder X.509-certifikat för att skydda ett kluster.</span><span class="sxs-lookup"><span data-stu-id="262be-127">Service Fabric uses X.509 certificates to secure a cluster.</span></span> <span data-ttu-id="262be-128">Azure Key Vault används för att hantera certifikat för Service Fabric-kluster i Azure.</span><span class="sxs-lookup"><span data-stu-id="262be-128">Azure Key Vault is used to manage certificates for Service Fabric clusters in Azure.</span></span> <span data-ttu-id="262be-129">När ett kluster distribueras i Azure, Azure resursprovidern ansvarar för att skapa Service Fabric-kluster hämtar certifikat från Nyckelvalvet och installerar dem på klustret virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="262be-129">When a cluster is deployed in Azure, the Azure resource provider responsible for creating Service Fabric clusters pulls certificates from Key Vault and installs them on the cluster VMs.</span></span>

<span data-ttu-id="262be-130">Följande diagram illustrerar förhållandet mellan Nyckelvalvet, Service Fabric-kluster och Azure-resursprovider som använder certifikat som lagras i Nyckelvalvet när den skapar ett kluster:</span><span class="sxs-lookup"><span data-stu-id="262be-130">The following diagram illustrates the relationship between Key Vault, a Service Fabric cluster, and the Azure resource provider that uses certificates stored in Key Vault when it creates a cluster:</span></span>

![Certifikatinstallation][cluster-security-cert-installation]

### <a name="create-a-resource-group"></a><span data-ttu-id="262be-132">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="262be-132">Create a Resource Group</span></span>
<span data-ttu-id="262be-133">Det första steget är att skapa en ny resursgrupp specifikt för Nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="262be-133">The first step is to create a new resource group specifically for Key Vault.</span></span> <span data-ttu-id="262be-134">Placera Nyckelvalvet i sin egen resursgruppen rekommenderas så att du kan ta bort beräkning och lagring resursgrupper – exempelvis den resursgrupp som har Service Fabric-kluster - utan att förlora dina nycklar och hemligheter.</span><span class="sxs-lookup"><span data-stu-id="262be-134">Putting Key Vault into its own resource group is recommended so that you can remove compute and storage resource groups - such as the resource group that has your Service Fabric cluster - without losing your keys and secrets.</span></span> <span data-ttu-id="262be-135">Den resursgrupp som har ditt Nyckelvalv måste vara i samma region som det kluster som använder den.</span><span class="sxs-lookup"><span data-stu-id="262be-135">The resource group that has your Key Vault must be in the same region as the cluster that is using it.</span></span>

```powershell

    PS C:\Users\vturecek> New-AzureRmResourceGroup -Name mycluster-keyvault -Location 'West US'
    WARNING: The output object type of this cmdlet will be modified in a future release.

    ResourceGroupName : mycluster-keyvault
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/<guid>/resourceGroups/mycluster-keyvault

```

### <a name="create-key-vault"></a><span data-ttu-id="262be-136">Skapa Nyckelvalvet</span><span class="sxs-lookup"><span data-stu-id="262be-136">Create Key Vault</span></span>
<span data-ttu-id="262be-137">Skapa ett Nyckelvalv i den nya resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="262be-137">Create a Key Vault in the new resource group.</span></span> <span data-ttu-id="262be-138">Nyckelvalvet **måste vara aktiverat för distribution** att Service Fabric-resursprovidern så att få certifikat från den och installera på klusternoder:</span><span class="sxs-lookup"><span data-stu-id="262be-138">The Key Vault **must be enabled for deployment** to allow the Service Fabric resource provider to get certificates from it and install on cluster nodes:</span></span>

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
                                       Permissions to Keys      :    get, create, delete, list, update, import, backup, restore
                                       Permissions to Secrets   :    all


    Tags                             :
```

<span data-ttu-id="262be-139">Om du har en befintlig Key Vault kan aktivera du den för distribution med Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="262be-139">If you have an existing Key Vault, you can enable it for deployment using Azure CLI:</span></span>

```cli
> azure login
> azure account set "your account"
> azure config mode arm 
> azure keyvault list
> azure keyvault set-policy --vault-name "your vault name" --enabled-for-deployment true
```


## <a name="add-certificates-to-key-vault"></a><span data-ttu-id="262be-140">Lägg till certifikat Key Vault</span><span class="sxs-lookup"><span data-stu-id="262be-140">Add certificates to Key Vault</span></span>
<span data-ttu-id="262be-141">Certifikat används i Service Fabric till att autentisera och kryptera olika delar av ett kluster och de program som körs där.</span><span class="sxs-lookup"><span data-stu-id="262be-141">Certificates are used in Service Fabric to provide authentication and encryption to secure various aspects of a cluster and its applications.</span></span> <span data-ttu-id="262be-142">Mer information om hur du använder certifikat i Service Fabric finns [säkerhetsscenarier för Service Fabric-klustret][service-fabric-cluster-security].</span><span class="sxs-lookup"><span data-stu-id="262be-142">For more information on how certificates are used in Service Fabric, see [Service Fabric cluster security scenarios][service-fabric-cluster-security].</span></span>

### <a name="cluster-and-server-certificate-required"></a><span data-ttu-id="262be-143">Kluster- och certifikat (krävs)</span><span class="sxs-lookup"><span data-stu-id="262be-143">Cluster and server certificate (required)</span></span>
<span data-ttu-id="262be-144">Detta certifikat krävs för att skydda ett kluster och förhindra obehörig åtkomst till den.</span><span class="sxs-lookup"><span data-stu-id="262be-144">This certificate is required to secure a cluster and prevent unauthorized access to it.</span></span> <span data-ttu-id="262be-145">Det ger säkerhet i klustret på ett par olika sätt:</span><span class="sxs-lookup"><span data-stu-id="262be-145">It provides cluster security in a couple ways:</span></span>

* <span data-ttu-id="262be-146">**Autentisering för klustret:** autentiserar nod till nod kommunikation för klustret.</span><span class="sxs-lookup"><span data-stu-id="262be-146">**Cluster authentication:** Authenticates node-to-node communication for cluster federation.</span></span> <span data-ttu-id="262be-147">Endast noder som kan bevisa sin identitet med det här certifikatet kan ansluta till klustret.</span><span class="sxs-lookup"><span data-stu-id="262be-147">Only nodes that can prove their identity with this certificate can join the cluster.</span></span>
* <span data-ttu-id="262be-148">**Serverautentisering:** verifierar att klustret management slutpunkter management-klienten så att klienten management vet pratar till verkliga klustret.</span><span class="sxs-lookup"><span data-stu-id="262be-148">**Server authentication:** Authenticates the cluster management endpoints to a management client, so that the management client knows it is talking to the real cluster.</span></span> <span data-ttu-id="262be-149">Det här certifikatet ger också SSL för HTTPS-hanterings-API och för Service Fabric Explorer via HTTPS.</span><span class="sxs-lookup"><span data-stu-id="262be-149">This certificate also provides SSL for the HTTPS management API and for Service Fabric Explorer over HTTPS.</span></span>

<span data-ttu-id="262be-150">Om du vill hantera dessa ändamål måste certifikatet uppfylla följande krav:</span><span class="sxs-lookup"><span data-stu-id="262be-150">To serve these purposes, the certificate must meet the following requirements:</span></span>

* <span data-ttu-id="262be-151">Certifikatet måste innehålla en privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="262be-151">The certificate must contain a private key.</span></span>
* <span data-ttu-id="262be-152">Certifikatet måste skapas för nyckelutbyte, kan exporteras till en Personal Information Exchange (.pfx)-fil.</span><span class="sxs-lookup"><span data-stu-id="262be-152">The certificate must be created for key exchange, exportable to a Personal Information Exchange (.pfx) file.</span></span>
* <span data-ttu-id="262be-153">Certifikatets ämnesnamn måste matcha den domän som används för åtkomst till Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="262be-153">The certificate's subject name must match the domain used to access the Service Fabric cluster.</span></span> <span data-ttu-id="262be-154">Detta krävs för att tillhandahålla SSL för klustrets HTTPS management slutpunkter och Service Fabric Explorer.</span><span class="sxs-lookup"><span data-stu-id="262be-154">This is required to provide SSL for the cluster's HTTPS management endpoints and Service Fabric Explorer.</span></span> <span data-ttu-id="262be-155">Du kan skaffa ett SSL-certifikat från en certifikatutfärdare (CA) för den `.cloudapp.azure.com` domän.</span><span class="sxs-lookup"><span data-stu-id="262be-155">You cannot obtain an SSL certificate from a certificate authority (CA) for the `.cloudapp.azure.com` domain.</span></span> <span data-ttu-id="262be-156">Hämta ett anpassat domännamn för klustret.</span><span class="sxs-lookup"><span data-stu-id="262be-156">Acquire a custom domain name for your cluster.</span></span> <span data-ttu-id="262be-157">När du begär ett certifikat från en Certifikatutfärdare måste certifikatets ämnesnamn matcha det anpassade domännamnet som används för klustret.</span><span class="sxs-lookup"><span data-stu-id="262be-157">When you request a certificate from a CA the certificate's subject name must match the custom domain name used for your cluster.</span></span>

### <a name="client-authentication-certificates"></a><span data-ttu-id="262be-158">Certifikat för klientautentisering</span><span class="sxs-lookup"><span data-stu-id="262be-158">Client authentication certificates</span></span>
<span data-ttu-id="262be-159">Ytterligare klientcertifikat autentisera administratörer för hanteringsuppgifter för klustret.</span><span class="sxs-lookup"><span data-stu-id="262be-159">Additional client certificates authenticate administrators for cluster management tasks.</span></span> <span data-ttu-id="262be-160">Service Fabric har två åtkomstnivåer: **admin** och **skrivskyddade användaren**.</span><span class="sxs-lookup"><span data-stu-id="262be-160">Service Fabric has two access levels: **admin** and **read-only user**.</span></span> <span data-ttu-id="262be-161">Minst ska ett enda certifikat för administrativ åtkomst användas.</span><span class="sxs-lookup"><span data-stu-id="262be-161">At minimum, a single certificate for administrative access should be used.</span></span> <span data-ttu-id="262be-162">För ytterligare användarrättigheter, måste ett separat certifikat tillhandahållas.</span><span class="sxs-lookup"><span data-stu-id="262be-162">For additional user-level access, a separate certificate must be provided.</span></span> <span data-ttu-id="262be-163">Mer information om åtkomst roller finns [rollbaserad åtkomstkontroll för Service Fabric-klienter][service-fabric-cluster-security-roles].</span><span class="sxs-lookup"><span data-stu-id="262be-163">For more information on access roles, see [role-based access control for Service Fabric clients][service-fabric-cluster-security-roles].</span></span>

<span data-ttu-id="262be-164">Du behöver inte överför certifikat för klientautentisering till Key Vault att arbeta med Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="262be-164">You do not need to upload Client authentication certificates to Key Vault to work with Service Fabric.</span></span> <span data-ttu-id="262be-165">Dessa certifikat behöver bara anges för användare som har behörighet för hantering av kluster.</span><span class="sxs-lookup"><span data-stu-id="262be-165">These certificates only need to be provided to users who are authorized for cluster management.</span></span> 

> [!NOTE]
> <span data-ttu-id="262be-166">Azure Active Directory är det rekommenderade sättet att autentisera klienter för klusterhanteringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="262be-166">Azure Active Directory is the recommended way to authenticate clients for cluster management operations.</span></span> <span data-ttu-id="262be-167">Om du vill använda Azure Active Directory, måste du [skapa ett kluster med Azure Resource Manager][create-cluster-arm].</span><span class="sxs-lookup"><span data-stu-id="262be-167">To use Azure Active Directory, you must [create a cluster using Azure Resource Manager][create-cluster-arm].</span></span>
> 
> 

### <a name="application-certificates-optional"></a><span data-ttu-id="262be-168">Certifikat för programmet (valfritt)</span><span class="sxs-lookup"><span data-stu-id="262be-168">Application certificates (optional)</span></span>
<span data-ttu-id="262be-169">Valfritt antal ytterligare certifikat kan installeras på ett kluster av säkerhetsskäl för programmet.</span><span class="sxs-lookup"><span data-stu-id="262be-169">Any number of additional certificates can be installed on a cluster for application security purposes.</span></span> <span data-ttu-id="262be-170">Tänk på applikationssäkerhets-scenarier som kräver ett certifikat installeras på noderna, som innan du skapar klustret:</span><span class="sxs-lookup"><span data-stu-id="262be-170">Before creating your cluster, consider the application security scenarios that require a certificate to be installed on the nodes, such as:</span></span>

* <span data-ttu-id="262be-171">Kryptering och dekryptering av konfigurationsvärden för programmet</span><span class="sxs-lookup"><span data-stu-id="262be-171">Encryption and decryption of application configuration values</span></span>
* <span data-ttu-id="262be-172">Kryptering av data mellan noderna vid replikering</span><span class="sxs-lookup"><span data-stu-id="262be-172">Encryption of data across nodes during replication</span></span> 

<span data-ttu-id="262be-173">Certifikat för programmet kan inte konfigureras när du skapar ett kluster via Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="262be-173">Application certificates cannot be configured when creating a cluster through the Azure portal.</span></span> <span data-ttu-id="262be-174">Om du vill konfigurera programmet certifikat vid klustret konfigurationen måste du [skapa ett kluster med Azure Resource Manager][create-cluster-arm].</span><span class="sxs-lookup"><span data-stu-id="262be-174">To configure application certificates at cluster setup time, you must [create a cluster using Azure Resource Manager][create-cluster-arm].</span></span> <span data-ttu-id="262be-175">Du kan också lägga till programmet certifikat till klustret när den har skapats.</span><span class="sxs-lookup"><span data-stu-id="262be-175">You can also add application certificates to the cluster after it has been created.</span></span>

### <a name="formatting-certificates-for-azure-resource-provider-use"></a><span data-ttu-id="262be-176">Formatering certifikat för användning av Azure resource provider</span><span class="sxs-lookup"><span data-stu-id="262be-176">Formatting certificates for Azure resource provider use</span></span>
<span data-ttu-id="262be-177">Privata nyckelfiler (.pfx) kan läggas till och användas direkt via Nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="262be-177">Private key files (.pfx) can be added and used directly through Key Vault.</span></span> <span data-ttu-id="262be-178">Azure-resursprovider kräver dock nycklar lagras i en särskild JSON-format som innehåller PFX som en Base64-kodad sträng och lösenordet för privata nyckeln.</span><span class="sxs-lookup"><span data-stu-id="262be-178">However, the Azure resource provider requires keys to be stored in a special JSON format that includes the .pfx as a base-64 encoded string and the private key password.</span></span> <span data-ttu-id="262be-179">Om du vill anpassa dessa krav nycklar måste placeras i en JSON-sträng och sedan lagras som *hemligheter* i Nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="262be-179">To accommodate these requirements, keys must be placed in a JSON string and then stored as *secrets* in Key Vault.</span></span>

<span data-ttu-id="262be-180">För att underlätta processen, en PowerShell-modul är [finns på GitHub][service-fabric-rp-helpers].</span><span class="sxs-lookup"><span data-stu-id="262be-180">To make this process easier, a PowerShell module is [available on GitHub][service-fabric-rp-helpers].</span></span> <span data-ttu-id="262be-181">Följ dessa steg om du vill använda modulen:</span><span class="sxs-lookup"><span data-stu-id="262be-181">Follow these steps to use the module:</span></span>

1. <span data-ttu-id="262be-182">Hämta hela innehållet på lagringsplatsen till en lokal katalog.</span><span class="sxs-lookup"><span data-stu-id="262be-182">Download the entire contents of the repo into a local directory.</span></span> 
2. <span data-ttu-id="262be-183">Importera modulen i PowerShell-fönstret:</span><span class="sxs-lookup"><span data-stu-id="262be-183">Import the module in your PowerShell window:</span></span>

```powershell
  PS C:\Users\vturecek> Import-Module "C:\users\vturecek\Documents\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"
```

<span data-ttu-id="262be-184">Den `Invoke-AddCertToKeyVault` kommandot i PowerShell-modulen automatiskt formaterar en certifikatets privata nyckel till en JSON-sträng och överförs till Nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="262be-184">The `Invoke-AddCertToKeyVault` command in this PowerShell module automatically formats a certificate private key into a JSON string and uploads it to Key Vault.</span></span> <span data-ttu-id="262be-185">Använd den för att lägga till kluster-certifikat och några ytterligare certifikat Key Vault.</span><span class="sxs-lookup"><span data-stu-id="262be-185">Use it to add the cluster certificate and any additional application certificates to Key Vault.</span></span> <span data-ttu-id="262be-186">Upprepa det här steget för några ytterligare certifikat som du vill installera i klustret.</span><span class="sxs-lookup"><span data-stu-id="262be-186">Repeat this step for any additional certificates you want to install in your cluster.</span></span>

```powershell
PS C:\Users\vturecek> Invoke-AddCertToKeyVault -SubscriptionId <guid> -ResourceGroupName mycluster-keyvault -Location "West US" -VaultName myvault -CertificateName mycert -Password "<password>" -UseExistingCertificate -ExistingPfxFilePath "C:\path\to\mycertkey.pfx"

    Switching context to SubscriptionId <guid>
    Ensuring ResourceGroup mycluster-keyvault in West US
    WARNING: The output object type of this cmdlet will be modified in a future release.
    Using existing valut myvault in West US
    Reading pfx file from C:\path\to\key.pfx
    Writing secret to myvault in vault myvault


Name  : CertificateThumbprint
Value : <value>

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault

Name  : CertificateURL
Value : https://myvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47

```

<span data-ttu-id="262be-187">Dessa är alla Key Vault-krav för att konfigurera ett Service Fabric-kluster Resource Manager-mall som installerar certifikat för noden autentisering, hantering av slutpunktssäkerhet och autentisering och eventuella ytterligare programsäkerhet funktioner som använder X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="262be-187">These are all the Key Vault prerequisites for configuring a Service Fabric cluster Resource Manager template that installs certificates for node authentication, management endpoint security and authentication, and any additional application security features that use X.509 certificates.</span></span> <span data-ttu-id="262be-188">Nu ska du nu har följande inställningar i Azure:</span><span class="sxs-lookup"><span data-stu-id="262be-188">At this point, you should now have the following setup in Azure:</span></span>

* <span data-ttu-id="262be-189">Key Vault resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="262be-189">Key Vault resource group</span></span>
  * <span data-ttu-id="262be-190">Key Vault</span><span class="sxs-lookup"><span data-stu-id="262be-190">Key Vault</span></span>
    * <span data-ttu-id="262be-191">Klustret certifikat för serverautentisering</span><span class="sxs-lookup"><span data-stu-id="262be-191">Cluster server authentication certificate</span></span>

<span data-ttu-id="262be-192">< /a ”Skapa kluster-portal” ></a></span><span class="sxs-lookup"><span data-stu-id="262be-192"></a "create-cluster-portal" ></a></span></span>

## <a name="create-cluster-in-the-azure-portal"></a><span data-ttu-id="262be-193">Skapa kluster i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="262be-193">Create cluster in the Azure portal</span></span>
### <a name="search-for-the-service-fabric-cluster-resource"></a><span data-ttu-id="262be-194">Sök efter klusterresursen Service Fabric</span><span class="sxs-lookup"><span data-stu-id="262be-194">Search for the Service Fabric cluster resource</span></span>
![Sök efter mallen för Service Fabric-kluster på Azure portal.][SearchforServiceFabricClusterTemplate]

1. <span data-ttu-id="262be-196">Logga in på [Azure Portal][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="262be-196">Sign in to the [Azure portal][azure-portal].</span></span>
2. <span data-ttu-id="262be-197">Klicka på **ny** att lägga till en ny resursmall för.</span><span class="sxs-lookup"><span data-stu-id="262be-197">Click **New** to add a new resource template.</span></span> <span data-ttu-id="262be-198">Sök efter mallen Service Fabric-kluster i den **Marketplace** under **allt**.</span><span class="sxs-lookup"><span data-stu-id="262be-198">Search for the Service Fabric Cluster template in the **Marketplace** under **Everything**.</span></span>
3. <span data-ttu-id="262be-199">Välj **Service Fabric-kluster** från listan.</span><span class="sxs-lookup"><span data-stu-id="262be-199">Select **Service Fabric Cluster** from the list.</span></span>
4. <span data-ttu-id="262be-200">Navigera till den **Service Fabric-kluster** bladet, klickar du på **skapa**,</span><span class="sxs-lookup"><span data-stu-id="262be-200">Navigate to the **Service Fabric Cluster** blade, click **Create**,</span></span>
5. <span data-ttu-id="262be-201">Den **skapar Service Fabric-kluster** bladet har följande fyra steg.</span><span class="sxs-lookup"><span data-stu-id="262be-201">The **Create Service Fabric cluster** blade has the following four steps.</span></span>

#### <a name="1-basics"></a><span data-ttu-id="262be-202">1. Grundläggande inställningar</span><span class="sxs-lookup"><span data-stu-id="262be-202">1. Basics</span></span>
![Skärmbild som visar hur du skapar en ny resursgrupp.][CreateRG]

<span data-ttu-id="262be-204">I bladet grundläggande behöver du ange grundläggande information för klustret.</span><span class="sxs-lookup"><span data-stu-id="262be-204">In the Basics blade you need to provide the basic details for your cluster.</span></span>

1. <span data-ttu-id="262be-205">Ange namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="262be-205">Enter the name of your cluster.</span></span>
2. <span data-ttu-id="262be-206">Ange en **användarnamn** och **lösenord** Fjärrskrivbord för de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="262be-206">Enter a **user name** and **password** for Remote Desktop for the VMs.</span></span>
3. <span data-ttu-id="262be-207">Se till att välja den **prenumeration** som du vill klustret som ska distribueras till, särskilt om du har flera prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="262be-207">Make sure to select the **Subscription** that you want your cluster to be deployed to, especially if you have multiple subscriptions.</span></span>
4. <span data-ttu-id="262be-208">Skapa en **ny resursgrupp**.</span><span class="sxs-lookup"><span data-stu-id="262be-208">Create a **new resource group**.</span></span> <span data-ttu-id="262be-209">Det är bäst att ge det samma namn som klustret, eftersom det hjälper dig att hitta dem senare, särskilt när du försöker göra ändringar i din distribution eller ta bort klustret.</span><span class="sxs-lookup"><span data-stu-id="262be-209">It is best to give it the same name as the cluster, since it helps in finding them later, especially when you are trying to make changes to your deployment or delete your cluster.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="262be-210">Även om du kan välja att använda en befintlig resursgrupp, är det en bra idé att skapa en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="262be-210">Although you can decide to use an existing resource group, it is a good practice to create a new resource group.</span></span> <span data-ttu-id="262be-211">Detta gör det enkelt att ta bort kluster som du inte behöver.</span><span class="sxs-lookup"><span data-stu-id="262be-211">This makes it easy to delete clusters that you do not need.</span></span>
   > 
   > 
5. <span data-ttu-id="262be-212">Välj den **region** som du vill skapa klustret.</span><span class="sxs-lookup"><span data-stu-id="262be-212">Select the **region** in which you want to create the cluster.</span></span> <span data-ttu-id="262be-213">Du måste använda samma region som ditt Nyckelvalv.</span><span class="sxs-lookup"><span data-stu-id="262be-213">You must use the same region that your Key Vault is in.</span></span>

#### <a name="2-cluster-configuration"></a><span data-ttu-id="262be-214">2. Klusterkonfiguration</span><span class="sxs-lookup"><span data-stu-id="262be-214">2. Cluster configuration</span></span>
![Skapa en nodtyp][CreateNodeType]

<span data-ttu-id="262be-216">Konfigurera klusternoderna.</span><span class="sxs-lookup"><span data-stu-id="262be-216">Configure your cluster nodes.</span></span> <span data-ttu-id="262be-217">Nodtyper definiera storlek på Virtuella datorer, hur många virtuella datorer och deras egenskaper.</span><span class="sxs-lookup"><span data-stu-id="262be-217">Node types define the VM sizes, the number of VMs, and their properties.</span></span> <span data-ttu-id="262be-218">Klustret kan ha fler än en nodtyp, men den primära nodtypen (den första som du definierar i portalen) måste ha minst fem virtuella datorer, eftersom det är nodtypen där Service Fabric systemtjänster placeras.</span><span class="sxs-lookup"><span data-stu-id="262be-218">Your cluster can have more than one node type, but the primary node type (the first one that you define on the portal) must have at least five VMs, as this is the node type where Service Fabric system services are placed.</span></span> <span data-ttu-id="262be-219">Konfigurera inte **placeringsegenskaper** eftersom en standardegenskap placering av ”NodeTypeName” läggs till automatiskt.</span><span class="sxs-lookup"><span data-stu-id="262be-219">Do not configure **Placement Properties** because a default placement property of "NodeTypeName" is added automatically.</span></span>

> [!NOTE]
> <span data-ttu-id="262be-220">Ett vanligt scenario för flera nodtyper är ett program som innehåller en frontend-tjänst och en backend tjänst.</span><span class="sxs-lookup"><span data-stu-id="262be-220">A common scenario for multiple node types is an application that contains a front-end service and a back-end service.</span></span> <span data-ttu-id="262be-221">Du vill publicera frontend-tjänsten på mindre virtuella datorer (VM-storlekar som D2) med öppna portar till Internet, men du vill placera backend-tjänst på större VM: ar (med VM-storlekar som D4, D6, D15 och så vidare) utan Internet-riktade öppna portar.</span><span class="sxs-lookup"><span data-stu-id="262be-221">You want to put the front-end service on smaller VMs (VM sizes like D2) with ports open to the Internet, but you want to put the back-end service on larger VMs (with VM sizes like D4, D6, D15, and so on) with no Internet-facing ports open.</span></span>
> 
> 

1. <span data-ttu-id="262be-222">Välj ett namn för din nodtypen (1 till 12 tecken som innehåller endast bokstäver och siffror).</span><span class="sxs-lookup"><span data-stu-id="262be-222">Choose a name for your node type (1 to 12 characters containing only letters and numbers).</span></span>
2. <span data-ttu-id="262be-223">Minst **storlek** för virtuella datorer för den primära noden typen drivs av den **hållbarhet** nivå som du väljer för klustret.</span><span class="sxs-lookup"><span data-stu-id="262be-223">The minimum **size** of VMs for the primary node type is driven by the **durability** tier you choose for the cluster.</span></span> <span data-ttu-id="262be-224">Standardvärdet för hållbarhet skiktet är Brons.</span><span class="sxs-lookup"><span data-stu-id="262be-224">The default for the durability tier is bronze.</span></span> <span data-ttu-id="262be-225">Mer information om hållbarhet finns [hur du väljer Service Fabric-kluster tillförlitlighet och hållbarhet][service-fabric-cluster-capacity].</span><span class="sxs-lookup"><span data-stu-id="262be-225">For more information on durability, see [how to choose the Service Fabric cluster reliability and durability][service-fabric-cluster-capacity].</span></span>
3. <span data-ttu-id="262be-226">Välj prisnivå och VM-storlek.</span><span class="sxs-lookup"><span data-stu-id="262be-226">Select the VM size and pricing tier.</span></span> <span data-ttu-id="262be-227">D-serien virtuella datorer ha SSD-enheterna och rekommenderas för tillståndskänsliga program.</span><span class="sxs-lookup"><span data-stu-id="262be-227">D-series VMs have SSD drives and are highly recommended for stateful applications.</span></span> <span data-ttu-id="262be-228">Använd inte VM-SKU som har delvis kärnor eller vara mindre än 7 GB tillgängligt diskutrymme.</span><span class="sxs-lookup"><span data-stu-id="262be-228">Do not use any VM SKU that has partial cores or have less than 7 GB of available disk capacity.</span></span> 
4. <span data-ttu-id="262be-229">Minst **nummer** för virtuella datorer för den primära noden typen drivs av den **tillförlitlighet** nivå som du väljer.</span><span class="sxs-lookup"><span data-stu-id="262be-229">The minimum **number** of VMs for the primary node type is driven by the **reliability** tier you choose.</span></span> <span data-ttu-id="262be-230">Standardvärdet för tillförlitlighetsnivån är Silver.</span><span class="sxs-lookup"><span data-stu-id="262be-230">The default for the reliability tier is Silver.</span></span> <span data-ttu-id="262be-231">Mer information om tillförlitlighet finns [hur du väljer Service Fabric-kluster tillförlitlighet och hållbarhet][service-fabric-cluster-capacity].</span><span class="sxs-lookup"><span data-stu-id="262be-231">For more information on reliability, see [how to choose the Service Fabric cluster reliability and durability][service-fabric-cluster-capacity].</span></span>
5. <span data-ttu-id="262be-232">Välj antalet virtuella datorer för nodtypen.</span><span class="sxs-lookup"><span data-stu-id="262be-232">Choose the number of VMs for the node type.</span></span> <span data-ttu-id="262be-233">Du kan skala upp eller ned antalet virtuella datorer i en nodtyp vid ett senare tillfälle, men på den primära nodtypen minst drivs av den nivå för tillförlitlighet som du har valt.</span><span class="sxs-lookup"><span data-stu-id="262be-233">You can scale up or down the number of VMs in a node type later on, but on the primary node type, the minimum is driven by the reliability level that you have chosen.</span></span> <span data-ttu-id="262be-234">Andra nodtyper kan ha minst 1 VM.</span><span class="sxs-lookup"><span data-stu-id="262be-234">Other node types can have a minimum of 1 VM.</span></span>
6. <span data-ttu-id="262be-235">Konfigurera anpassade slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="262be-235">Configure custom endpoints.</span></span> <span data-ttu-id="262be-236">Det här fältet kan du ange en kommaavgränsad lista över portar som du vill exponera via Azure-belastningsutjämnaren till det offentliga Internet för dina program.</span><span class="sxs-lookup"><span data-stu-id="262be-236">This field allows you to enter a comma-separated list of ports that you want to expose through the Azure Load Balancer to the public Internet for your applications.</span></span> <span data-ttu-id="262be-237">Om du planerar att distribuera ett program i klustret, t.ex ”80” här för att tillåta trafik på port 80 till klustret.</span><span class="sxs-lookup"><span data-stu-id="262be-237">For example, if you plan to deploy a web application to your cluster, enter "80" here to allow traffic on port 80 into your cluster.</span></span> <span data-ttu-id="262be-238">Läs mer på slutpunkter [kommunicera med program][service-fabric-connect-and-communicate-with-services]</span><span class="sxs-lookup"><span data-stu-id="262be-238">For more information on endpoints, see [communicating with applications][service-fabric-connect-and-communicate-with-services]</span></span>
7. <span data-ttu-id="262be-239">Konfigurera klustret **diagnostik**.</span><span class="sxs-lookup"><span data-stu-id="262be-239">Configure cluster **diagnostics**.</span></span> <span data-ttu-id="262be-240">Som standard aktiveras diagnostik på klustret att underlätta felsökning av problem.</span><span class="sxs-lookup"><span data-stu-id="262be-240">By default, diagnostics are enabled on your cluster to assist with troubleshooting issues.</span></span> <span data-ttu-id="262be-241">Om du vill inaktivera diagnostik ändra den **Status** växla till **av**.</span><span class="sxs-lookup"><span data-stu-id="262be-241">If you want to disable diagnostics change the **Status** toggle to **Off**.</span></span> <span data-ttu-id="262be-242">Om du inaktiverar diagnostik är **inte** rekommenderas.</span><span class="sxs-lookup"><span data-stu-id="262be-242">Turning off diagnostics is **not** recommended.</span></span>
8. <span data-ttu-id="262be-243">Välj Fabric Uppgraderingsläge du vill ange ditt kluster till.</span><span class="sxs-lookup"><span data-stu-id="262be-243">Select the Fabric upgrade mode you want set your cluster to.</span></span> <span data-ttu-id="262be-244">Välj **automatisk**, om du vill att hämta den senaste tillgängliga versionen och försök att uppgradera ditt kluster till det automatiskt.</span><span class="sxs-lookup"><span data-stu-id="262be-244">Select **Automatic**, if you want the system to automatically pick up the latest available version and try to upgrade your cluster to it.</span></span> <span data-ttu-id="262be-245">Ange läget som **manuell**, om du vill välja en version som stöds.</span><span class="sxs-lookup"><span data-stu-id="262be-245">Set the mode to **Manual**, if you want to choose a supported version.</span></span>

> [!NOTE]
> <span data-ttu-id="262be-246">Vi stöder endast kluster som kör versioner av service Fabric stöds.</span><span class="sxs-lookup"><span data-stu-id="262be-246">We support only clusters that are running supported versions of service Fabric.</span></span> <span data-ttu-id="262be-247">Genom att välja den **manuell** läge du tar med ansvar att uppgradera ditt kluster till en version som stöds.</span><span class="sxs-lookup"><span data-stu-id="262be-247">By selecting the **Manual** mode, you are taking on the responsibility to upgrade your cluster to a supported version.</span></span> <span data-ttu-id="262be-248">För mer information om infrastrukturen uppgradera läge finns det [service fabric-kluster-uppgradering dokumentet.][service-fabric-cluster-upgrade]</span><span class="sxs-lookup"><span data-stu-id="262be-248">For more details on the Fabric upgrade mode see the [service-fabric-cluster-upgrade document.][service-fabric-cluster-upgrade]</span></span>
> 
> 

#### <a name="3-security"></a><span data-ttu-id="262be-249">3. Säkerhet</span><span class="sxs-lookup"><span data-stu-id="262be-249">3. Security</span></span>
![Skärmbild som visar säkerhetskonfigurationer på Azure-portalen.][SecurityConfigs]

<span data-ttu-id="262be-251">Det sista steget är att tillhandahålla information om certifikat för att skydda klustret med Key Vault och certifikatet information har skapat tidigare.</span><span class="sxs-lookup"><span data-stu-id="262be-251">The final step is to provide certificate information to secure the cluster using the Key Vault and certificate information created earlier.</span></span>

* <span data-ttu-id="262be-252">Fylla i certifikatet för primär-fält med utdata från ladda upp den **klustret certifikat** till Nyckelvalvet med hjälp av den `Invoke-AddCertToKeyVault` PowerShell-kommando.</span><span class="sxs-lookup"><span data-stu-id="262be-252">Populate the primary certificate fields with the output obtained from uploading the **cluster certificate** to Key Vault using the `Invoke-AddCertToKeyVault` PowerShell command.</span></span>

```powershell
Name  : CertificateThumbprint
Value : <value>

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault

Name  : CertificateURL
Value : https://myvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47
```

* <span data-ttu-id="262be-253">Kontrollera den **konfigurera avancerade inställningar** om du vill ange klientcertifikat för **administratörsklient** och **skrivskyddad klient**.</span><span class="sxs-lookup"><span data-stu-id="262be-253">Check the **Configure advanced settings** box to enter client certificates for **admin client** and **read-only client**.</span></span> <span data-ttu-id="262be-254">I dessa fält, anger du tumavtrycket för admin klientcertifikatet och tumavtrycket för klientcertifikatet skrivskyddade användaren, om tillämpligt.</span><span class="sxs-lookup"><span data-stu-id="262be-254">In these fields, enter the thumbprint of your admin client certificate and the thumbprint of your read-only user client certificate, if applicable.</span></span> <span data-ttu-id="262be-255">När administratörer försöker ansluta till klustret kan beviljas åtkomst endast om de har ett certifikat med ett tumavtryck som matchar tumavtrycket värden anges här.</span><span class="sxs-lookup"><span data-stu-id="262be-255">When administrators attempt to connect to the cluster, they are granted access only if they have a certificate with a thumbprint that matches the thumbprint values entered here.</span></span>  

#### <a name="4-summary"></a><span data-ttu-id="262be-256">4. Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="262be-256">4. Summary</span></span>
![<span data-ttu-id="262be-257">Skärmbild av start-kort som visar ”distribuera Service Fabric-kluster”.</span><span class="sxs-lookup"><span data-stu-id="262be-257">Screen shot of the start board displaying "Deploying Service Fabric Cluster."</span></span> ][Notifications]

<span data-ttu-id="262be-258">Skapa ett kluster, klicka på **sammanfattning** att se de konfigurationer som du har angett eller hämta en Azure Resource Manager-mall som används för att distribuera klustret.</span><span class="sxs-lookup"><span data-stu-id="262be-258">To complete the cluster creation, click **Summary** to see the configurations that you have provided, or download the Azure Resource Manager template that that used to deploy your cluster.</span></span> <span data-ttu-id="262be-259">När du har angett obligatoriska inställningar i **OK** knappen blir grön och du kan starta klusterskapandeprocessen genom att klicka på den.</span><span class="sxs-lookup"><span data-stu-id="262be-259">After you have provided the mandatory settings, the **OK** button becomes green and you can start the cluster creation process by clicking it.</span></span>

<span data-ttu-id="262be-260">Du kan se förloppet bland aviseringarna.</span><span class="sxs-lookup"><span data-stu-id="262be-260">You can see the creation progress in the notifications.</span></span> <span data-ttu-id="262be-261">(Klicka på klockikonen nära statusfältet uppe till höger på skärmen.) Om du klickade på **fäst på startsidan** när du skapar klustret måste du se **distribuerar Service Fabric-kluster** Fäst den **starta** kort.</span><span class="sxs-lookup"><span data-stu-id="262be-261">(Click the "Bell" icon near the status bar at the upper right of your screen.) If you clicked **Pin to Startboard** while creating the cluster, you will see **Deploying Service Fabric Cluster** pinned to the **Start** board.</span></span>

### <a name="view-your-cluster-status"></a><span data-ttu-id="262be-262">Visa klusterstatus för</span><span class="sxs-lookup"><span data-stu-id="262be-262">View your cluster status</span></span>
![Skärmbild som visar information om kluster på instrumentpanelen.][ClusterDashboard]

<span data-ttu-id="262be-264">När klustret har skapats kan du granska klustret i portalen:</span><span class="sxs-lookup"><span data-stu-id="262be-264">Once your cluster is created, you can inspect your cluster in the portal:</span></span>

1. <span data-ttu-id="262be-265">Gå till **Bläddra** och på **Service Fabric-kluster**.</span><span class="sxs-lookup"><span data-stu-id="262be-265">Go to **Browse** and click **Service Fabric Clusters**.</span></span>
2. <span data-ttu-id="262be-266">Leta upp ditt kluster och klicka på den.</span><span class="sxs-lookup"><span data-stu-id="262be-266">Locate your cluster and click it.</span></span>
3. <span data-ttu-id="262be-267">Där kan du se information om klustret på instrumentpanelen, bland annat klustrets offentliga slutpunkter och en länk till Service Fabric Explorer.</span><span class="sxs-lookup"><span data-stu-id="262be-267">You can now see the details of your cluster in the dashboard, including the cluster's public endpoint and a link to Service Fabric Explorer.</span></span>

<span data-ttu-id="262be-268">Den **nod övervakaren** avsnitt på klustrets instrumentpanel blad anger hur många virtuella datorer som är felfria och inte felfri.</span><span class="sxs-lookup"><span data-stu-id="262be-268">The **Node Monitor** section on the cluster's dashboard blade indicates the number of VMs that are healthy and not healthy.</span></span> <span data-ttu-id="262be-269">Du hittar mer information om klustrets hälsa i [Service Fabric hälsa modellen introduktion][service-fabric-health-introduction].</span><span class="sxs-lookup"><span data-stu-id="262be-269">You can find more details about the cluster's health at [Service Fabric health model introduction][service-fabric-health-introduction].</span></span>

> [!NOTE]
> <span data-ttu-id="262be-270">Service Fabric-kluster kräver ett visst antal noder krävs för att alltid underhålla tillgänglighet och bevara tillstånd - kallas ”underhålla kvorum”.</span><span class="sxs-lookup"><span data-stu-id="262be-270">Service Fabric clusters require a certain number of nodes to be up always to maintain availability and preserve state - referred to as "maintaining quorum".</span></span> <span data-ttu-id="262be-271">Therfore, det vanligtvis inte är säkert att stänga av alla datorer i klustret om du först har utfört en [fullständig säkerhetskopiering av din tillstånd][service-fabric-reliable-services-backup-restore].</span><span class="sxs-lookup"><span data-stu-id="262be-271">Therfore, it is typically not safe to shut down all machines in the cluster unless you have first performed a [full backup of your state][service-fabric-reliable-services-backup-restore].</span></span>
> 
> 

## <a name="remote-connect-to-a-virtual-machine-scale-set-instance-or-a-cluster-node"></a><span data-ttu-id="262be-272">Fjärranslutning till en Virtual Machine Scale Set-instans eller en klusternod</span><span class="sxs-lookup"><span data-stu-id="262be-272">Remote connect to a Virtual Machine Scale Set instance or a cluster node</span></span>
<span data-ttu-id="262be-273">Var och en av nodetypes får du ange i klustret resultaten i en virtuell dator Skaluppsättning komma installation.</span><span class="sxs-lookup"><span data-stu-id="262be-273">Each of the NodeTypes you specify in your cluster results in a Virtual Machine Scale Set getting set-up.</span></span> <span data-ttu-id="262be-274">Se [fjärranslutning till en Virtual Machine Scale Set] [ remote-connect-to-a-vm-scale-set] mer information.</span><span class="sxs-lookup"><span data-stu-id="262be-274">See [Remote connect to a Virtual Machine Scale Set instance][remote-connect-to-a-vm-scale-set] for details.</span></span>

## <a name="next-steps"></a><span data-ttu-id="262be-275">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="262be-275">Next steps</span></span>
<span data-ttu-id="262be-276">Du har nu ett säker kluster som använder certifikat för autentisering för hantering.</span><span class="sxs-lookup"><span data-stu-id="262be-276">At this point, you have a secure cluster using certificates for management authentication.</span></span> <span data-ttu-id="262be-277">Nästa [ansluta till klustret](service-fabric-connect-to-secure-cluster.md) och lära dig hur du [hantera program hemligheter](service-fabric-application-secret-management.md).</span><span class="sxs-lookup"><span data-stu-id="262be-277">Next, [connect to your cluster](service-fabric-connect-to-secure-cluster.md) and learn how to [manage application secrets](service-fabric-application-secret-management.md).</span></span>  <span data-ttu-id="262be-278">Lär dig dessutom [Service Fabric supportalternativ](service-fabric-support.md).</span><span class="sxs-lookup"><span data-stu-id="262be-278">Also, learn about [Service Fabric support options](service-fabric-support.md).</span></span>

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
