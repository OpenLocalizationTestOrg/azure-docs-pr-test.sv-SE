---
title: "aaaConnect på ett säkert sätt tooan Azure Service Fabric-kluster | Microsoft Docs"
description: "Beskriver hur tooauthenticate klienten komma åt tooa Service Fabric-kluster och toosecure kommunikationen mellan klienterna och ett kluster."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 759a539e-e5e6-4055-bff5-d38804656e10
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/01/2017
ms.author: ryanwi
ms.openlocfilehash: 1b6a87a1fefaddce2043c604ca53751157232170
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooa-secure-cluster"></a><span data-ttu-id="739e9-103">Ansluta tooa säker kluster</span><span class="sxs-lookup"><span data-stu-id="739e9-103">Connect tooa secure cluster</span></span>

<span data-ttu-id="739e9-104">När en klient ansluter tooa Service Fabric-nod i klustret, kan hello-klient vara autentiserad och säker kommunikation upprättas med hjälp av certifikatet säkerhets- eller Azure Active Directory (AAD).</span><span class="sxs-lookup"><span data-stu-id="739e9-104">When a client connects tooa Service Fabric cluster node, hello client can be authenticated and secure communication established using certificate security or Azure Active Directory (AAD).</span></span> <span data-ttu-id="739e9-105">Den här autentiseringen säkerställer att endast auktoriserade användare kan komma åt hello kluster och distribuerade program och utföra administrativa uppgifter.</span><span class="sxs-lookup"><span data-stu-id="739e9-105">This authentication ensures that only authorized users can access hello cluster and deployed applications and perform management tasks.</span></span>  <span data-ttu-id="739e9-106">Certifikat eller AAD säkerhet måste ha tidigare aktiverats på hello klustret när hello klustret har skapats.</span><span class="sxs-lookup"><span data-stu-id="739e9-106">Certificate or AAD security must have been previously enabled on hello cluster when hello cluster was created.</span></span>  <span data-ttu-id="739e9-107">Mer information om kluster säkerhetsscenarier finns [kluster säkerhet](service-fabric-cluster-security.md).</span><span class="sxs-lookup"><span data-stu-id="739e9-107">For more information on cluster security scenarios, see [Cluster security](service-fabric-cluster-security.md).</span></span> <span data-ttu-id="739e9-108">Om du ansluter tooa kluster skyddas med certifikat, [konfigurera hello klientcertifikat](service-fabric-connect-to-secure-cluster.md#connectsecureclustersetupclientcert) på hello-dator som ansluter toohello klustret.</span><span class="sxs-lookup"><span data-stu-id="739e9-108">If you are connecting tooa cluster secured with certificates, [set up hello client certificate](service-fabric-connect-to-secure-cluster.md#connectsecureclustersetupclientcert) on hello computer that connects toohello cluster.</span></span> 

<a id="connectsecureclustercli"></a> 

## <a name="connect-tooa-secure-cluster-using-azure-service-fabric-cli-sfctl"></a><span data-ttu-id="739e9-109">Ansluta tooa säker kluster med hjälp av Azure Service Fabric CLI (sfctl)</span><span class="sxs-lookup"><span data-stu-id="739e9-109">Connect tooa secure cluster using Azure Service Fabric CLI (sfctl)</span></span>

<span data-ttu-id="739e9-110">Det finns några olika sätt tooconnect tooa säker kluster med hjälp av hello Service Fabric CLI (sfctl).</span><span class="sxs-lookup"><span data-stu-id="739e9-110">There are a few different ways tooconnect tooa secure cluster using hello Service Fabric CLI (sfctl).</span></span> <span data-ttu-id="739e9-111">När du använder ett klientcertifikat för autentisering, distribueras hello certifikat information måste matcha ett certifikat toohello klusternoder.</span><span class="sxs-lookup"><span data-stu-id="739e9-111">When using a client certificate for authentication, hello certificate details must match a certificate deployed toohello cluster nodes.</span></span> <span data-ttu-id="739e9-112">Om ditt certifikat har certifikatutfärdare (CA), måste du tooadditionally ange hello betrodda certifikatutfärdare.</span><span class="sxs-lookup"><span data-stu-id="739e9-112">If your certificate has Certificate Authorities (CAs), you need tooadditionally specify hello trusted CAs.</span></span>

<span data-ttu-id="739e9-113">Du kan ansluta tooa kluster med hjälp av hello `sfctl cluster select` kommando.</span><span class="sxs-lookup"><span data-stu-id="739e9-113">You can connect tooa cluster using hello `sfctl cluster select` command.</span></span>

<span data-ttu-id="739e9-114">Klientcertifikat kan anges i två olika sätt, som ett certifikat och nyckelpar, eller som en enda pem-fil.</span><span class="sxs-lookup"><span data-stu-id="739e9-114">Client certificates can be specified in two different fashions, either as a cert and key pair, or as a single pem file.</span></span> <span data-ttu-id="739e9-115">För lösenordsskyddade `pem` filer, kommer du att uppmanas automatiskt tooenter hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="739e9-115">For password protected `pem` files, you will be prompted automatically tooenter hello password.</span></span>

<span data-ttu-id="739e9-116">toospecify hello klientcertifikat som en pem-fil, ange sökväg till hello i hello `--pem` argumentet.</span><span class="sxs-lookup"><span data-stu-id="739e9-116">toospecify hello client certificate as a pem file, specify hello file path in hello `--pem` argument.</span></span> <span data-ttu-id="739e9-117">Exempel:</span><span class="sxs-lookup"><span data-stu-id="739e9-117">For example:</span></span>

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

<span data-ttu-id="739e9-118">Lösenordet skyddas pem-filer ombeds du att lösenordet tidigare toorunning ett kommando.</span><span class="sxs-lookup"><span data-stu-id="739e9-118">Password protected pem files will prompt for password prior toorunning any command.</span></span>

<span data-ttu-id="739e9-119">toospecify ett certifikat, nyckelpar använda hello `--cert` och `--key` argument toospecify hello sökvägar tooeach respektive fil.</span><span class="sxs-lookup"><span data-stu-id="739e9-119">toospecify a cert, key pair use hello `--cert` and `--key` arguments toospecify hello file paths tooeach respective file.</span></span>

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --cert ./client.crt --key ./keyfile.key
```

<span data-ttu-id="739e9-120">Certifikat används ibland toosecure test eller dev kluster misslyckas valideringen av servercertifikatet.</span><span class="sxs-lookup"><span data-stu-id="739e9-120">Sometimes certificates used toosecure test or dev clusters fail certificate validation.</span></span> <span data-ttu-id="739e9-121">toobypass certifikat verifiering, ange hello `--no-verify` alternativet.</span><span class="sxs-lookup"><span data-stu-id="739e9-121">toobypass certificate verification, specify hello `--no-verify` option.</span></span> <span data-ttu-id="739e9-122">Exempel:</span><span class="sxs-lookup"><span data-stu-id="739e9-122">For example:</span></span>

> [!WARNING]
> <span data-ttu-id="739e9-123">Använd inte hello `no-verify` alternativ när du ansluter tooproduction Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="739e9-123">Do not use hello `no-verify` option when connecting tooproduction Service Fabric clusters.</span></span>

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem --no-verify
```

<span data-ttu-id="739e9-124">Du kan dessutom ange sökvägar toodirectories av certifikat för betrodd Certifikatutfärdare eller enskilda certifikat.</span><span class="sxs-lookup"><span data-stu-id="739e9-124">In addition, you can specify paths toodirectories of trusted CA certs, or individual certs.</span></span> <span data-ttu-id="739e9-125">toospecify dessa sökvägar använda hello `--ca` argumentet.</span><span class="sxs-lookup"><span data-stu-id="739e9-125">toospecify these paths, use hello `--ca` argument.</span></span> <span data-ttu-id="739e9-126">Exempel:</span><span class="sxs-lookup"><span data-stu-id="739e9-126">For example:</span></span>

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem --ca ./trusted_ca
```

<span data-ttu-id="739e9-127">När du ansluter du borde för[köra andra sfctl kommandon](service-fabric-cli.md) toointeract med hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="739e9-127">After you connect, you should be able too[run other sfctl commands](service-fabric-cli.md) toointeract with hello cluster.</span></span>

<a id="connectsecurecluster"></a>

## <a name="connect-tooa-cluster-using-powershell"></a><span data-ttu-id="739e9-128">Ansluta tooa kluster med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="739e9-128">Connect tooa cluster using PowerShell</span></span>
<span data-ttu-id="739e9-129">Upprätta en anslutning toohello klustret innan du utför åtgärder på ett kluster med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="739e9-129">Before you perform operations on a cluster through PowerShell, first establish a connection toohello cluster.</span></span> <span data-ttu-id="739e9-130">hello klustret anslutningen används för alla efterföljande kommandon i hello angivna PowerShell-session.</span><span class="sxs-lookup"><span data-stu-id="739e9-130">hello cluster connection is used for all subsequent commands in hello given PowerShell session.</span></span>

### <a name="connect-tooan-unsecure-cluster"></a><span data-ttu-id="739e9-131">Ansluta tooan oskyddade kluster</span><span class="sxs-lookup"><span data-stu-id="739e9-131">Connect tooan unsecure cluster</span></span>

<span data-ttu-id="739e9-132">tooconnect tooan oskyddade kluster, ange hello klustret endpoint adress toohello **Connect-ServiceFabricCluster** kommando:</span><span class="sxs-lookup"><span data-stu-id="739e9-132">tooconnect tooan unsecure cluster, provide hello cluster endpoint address toohello **Connect-ServiceFabricCluster** command:</span></span>

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 
```

### <a name="connect-tooa-secure-cluster-using-azure-active-directory"></a><span data-ttu-id="739e9-133">Ansluta tooa säker kluster med hjälp av Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="739e9-133">Connect tooa secure cluster using Azure Active Directory</span></span>

<span data-ttu-id="739e9-134">tooconnect tooa säker kluster som använder Azure Active Directory tooauthorize klustret administratörsåtkomst, ange hello tumavtryck för klustret och använda hello *AzureActiveDirectory* flaggan.</span><span class="sxs-lookup"><span data-stu-id="739e9-134">tooconnect tooa secure cluster that uses Azure Active Directory tooauthorize cluster administrator access, provide hello cluster certificate thumbprint and use hello *AzureActiveDirectory* flag.</span></span>  

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
-ServerCertThumbprint <Server Certificate Thumbprint> `
-AzureActiveDirectory
```

### <a name="connect-tooa-secure-cluster-using-a-client-certificate"></a><span data-ttu-id="739e9-135">Ansluta tooa säker kluster med ett klientcertifikat</span><span class="sxs-lookup"><span data-stu-id="739e9-135">Connect tooa secure cluster using a client certificate</span></span>
<span data-ttu-id="739e9-136">Kör hello följande PowerShell-kommando tooconnect tooa säker kluster som använder klienten certifikat tooauthorize administratörsåtkomst.</span><span class="sxs-lookup"><span data-stu-id="739e9-136">Run hello following PowerShell command tooconnect tooa secure cluster that uses client certificates tooauthorize administrator access.</span></span> <span data-ttu-id="739e9-137">Ange hello tumavtryck för klustret och hello tumavtryck hello klientcertifikat som har beviljats behörigheter för klusterhantering.</span><span class="sxs-lookup"><span data-stu-id="739e9-137">Provide hello cluster certificate thumbprint and hello thumbprint of hello client certificate that has been granted permissions for cluster management.</span></span> <span data-ttu-id="739e9-138">information om hello certifikat måste matcha ett certifikat på hello klusternoder.</span><span class="sxs-lookup"><span data-stu-id="739e9-138">hello certificate details must match a certificate on hello cluster nodes.</span></span>

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint <Certificate Thumbprint> `
          -FindType FindByThumbprint -FindValue <Certificate Thumbprint> `
          -StoreLocation CurrentUser -StoreName My
```

<span data-ttu-id="739e9-139">*ServerCertThumbprint* är hello tumavtryck hello servercertifikat installerat på hello klusternoder.</span><span class="sxs-lookup"><span data-stu-id="739e9-139">*ServerCertThumbprint* is hello thumbprint of hello server certificate installed on hello cluster nodes.</span></span> <span data-ttu-id="739e9-140">*FindValue* är hello hello admin klienten certifikatets tumavtryck.</span><span class="sxs-lookup"><span data-stu-id="739e9-140">*FindValue* is hello thumbprint of hello admin client certificate.</span></span>
<span data-ttu-id="739e9-141">När hello parametrar är ifyllda hello kommando som ser ut så hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="739e9-141">When hello parameters are filled in, hello command looks like hello following example:</span></span> 

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint clustername.westus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint A8136758F4AB8962AF2BF3F27921BE1DF67F4326 `
          -FindType FindByThumbprint -FindValue 71DE04467C9ED0544D021098BCD44C71E183414E `
          -StoreLocation CurrentUser -StoreName My
```

### <a name="connect-tooa-secure-cluster-using-windows-active-directory"></a><span data-ttu-id="739e9-142">Ansluta tooa säker kluster med hjälp av Windows Active Directory</span><span class="sxs-lookup"><span data-stu-id="739e9-142">Connect tooa secure cluster using Windows Active Directory</span></span>
<span data-ttu-id="739e9-143">Om fristående klustret distribueras med hjälp av AD-säkerhetsgrupp, kan du ansluta toohello klustret genom att lägga till hello växel ”WindowsCredential”.</span><span class="sxs-lookup"><span data-stu-id="739e9-143">If your standalone cluster is deployed using AD security, connect toohello cluster by appending hello switch "WindowsCredential".</span></span>

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
          -WindowsCredential
```

<a id="connectsecureclusterfabricclient"></a>

## <a name="connect-tooa-cluster-using-hello-fabricclient-apis"></a><span data-ttu-id="739e9-144">Ansluta tooa kluster med hjälp av hello FabricClient APIs</span><span class="sxs-lookup"><span data-stu-id="739e9-144">Connect tooa cluster using hello FabricClient APIs</span></span>
<span data-ttu-id="739e9-145">hello Service Fabric SDK innehåller hello [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) klass för klusterhantering.</span><span class="sxs-lookup"><span data-stu-id="739e9-145">hello Service Fabric SDK provides hello [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) class for cluster management.</span></span> <span data-ttu-id="739e9-146">toouse hello FabricClient APIs hämta hello Microsoft.ServiceFabric NuGet-paketet.</span><span class="sxs-lookup"><span data-stu-id="739e9-146">toouse hello FabricClient APIs, get hello Microsoft.ServiceFabric NuGet package.</span></span>

### <a name="connect-tooan-unsecure-cluster"></a><span data-ttu-id="739e9-147">Ansluta tooan oskyddade kluster</span><span class="sxs-lookup"><span data-stu-id="739e9-147">Connect tooan unsecure cluster</span></span>

<span data-ttu-id="739e9-148">tooconnect tooa oskyddade kluster, skapa en FabricClient-instans och ange hello klusteradress:</span><span class="sxs-lookup"><span data-stu-id="739e9-148">tooconnect tooa remote unsecured cluster, create a FabricClient instance and provide hello cluster address:</span></span>

```csharp
FabricClient fabricClient = new FabricClient("clustername.westus.cloudapp.azure.com:19000");
```

<span data-ttu-id="739e9-149">För kod som körs från inom ett kluster, till exempel i en tillförlitlig tjänst, skapa en FabricClient *utan* att ange hello klusteradress.</span><span class="sxs-lookup"><span data-stu-id="739e9-149">For code that is running from within a cluster, for example, in a Reliable Service, create a FabricClient *without* specifying hello cluster address.</span></span> <span data-ttu-id="739e9-150">FabricClient ansluter toohello lokal hantering gateway på hello nod hello kod körs på, undvika en extra nätverket hopp.</span><span class="sxs-lookup"><span data-stu-id="739e9-150">FabricClient connects toohello local management gateway on hello node hello code is currently running on, avoiding an extra network hop.</span></span>

```csharp
FabricClient fabricClient = new FabricClient();
```

### <a name="connect-tooa-secure-cluster-using-a-client-certificate"></a><span data-ttu-id="739e9-151">Ansluta tooa säker kluster med ett klientcertifikat</span><span class="sxs-lookup"><span data-stu-id="739e9-151">Connect tooa secure cluster using a client certificate</span></span>

<span data-ttu-id="739e9-152">hello-noder i klustret hello måste ha giltiga certifikat vars nätverksnamn eller DNS-namnet i SAN visas i hello [RemoteCommonNames egenskapen](https://docs.microsoft.com/dotnet/api/system.fabric.x509credentials#System_Fabric_X509Credentials_RemoteCommonNames) på [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient).</span><span class="sxs-lookup"><span data-stu-id="739e9-152">hello nodes in hello cluster must have valid certificates whose common name or DNS name in SAN appears in hello [RemoteCommonNames property](https://docs.microsoft.com/dotnet/api/system.fabric.x509credentials#System_Fabric_X509Credentials_RemoteCommonNames) set on [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient).</span></span> <span data-ttu-id="739e9-153">Efter den här processen kan ömsesidig autentisering mellan hello-klienten och hello klusternoder.</span><span class="sxs-lookup"><span data-stu-id="739e9-153">Following this process enables mutual authentication between hello client and hello cluster nodes.</span></span>

```csharp
using System.Fabric;
using System.Security.Cryptography.X509Certificates;

string clientCertThumb = "71DE04467C9ED0544D021098BCD44C71E183414E";
string serverCertThumb = "A8136758F4AB8962AF2BF3F27921BE1DF67F4326";
string CommonName = "www.clustername.westus.azure.com";
string connection = "clustername.westus.cloudapp.azure.com:19000";

var xc = GetCredentials(clientCertThumb, serverCertThumb, CommonName);
var fc = new FabricClient(xc, connection);

try
{
    var ret = fc.ClusterManager.GetClusterManifestAsync().Result;
    Console.WriteLine(ret.ToString());
}
catch (Exception e)
{
    Console.WriteLine("Connect failed: {0}", e.Message);
}

static X509Credentials GetCredentials(string clientCertThumb, string serverCertThumb, string name)
{
    X509Credentials xc = new X509Credentials();
    xc.StoreLocation = StoreLocation.CurrentUser;
    xc.StoreName = "My";
    xc.FindType = X509FindType.FindByThumbprint;
    xc.FindValue = clientCertThumb;
    xc.RemoteCommonNames.Add(name);
    xc.RemoteCertThumbprints.Add(serverCertThumb);
    xc.ProtectionLevel = ProtectionLevel.EncryptAndSign;
    return xc;
}
```

### <a name="connect-tooa-secure-cluster-interactively-using-azure-active-directory"></a><span data-ttu-id="739e9-154">Ansluta tooa säker klustret interaktivt med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="739e9-154">Connect tooa secure cluster interactively using Azure Active Directory</span></span>

<span data-ttu-id="739e9-155">hello följande exempel använder Azure Active Directory för identitets- och klientcertifikat för serveridentitet.</span><span class="sxs-lookup"><span data-stu-id="739e9-155">hello following example uses Azure Active Directory for client identity and server certificate for server identity.</span></span>

<span data-ttu-id="739e9-156">En dialogruta visas automatiskt för interaktiv inloggning vid anslutning toohello klustret.</span><span class="sxs-lookup"><span data-stu-id="739e9-156">A dialog window automatically pops up for interactive sign-in upon connecting toohello cluster.</span></span>

```csharp
string serverCertThumb = "A8136758F4AB8962AF2BF3F27921BE1DF67F4326";
string connection = "clustername.westus.cloudapp.azure.com:19000";

var claimsCredentials = new ClaimsCredentials();
claimsCredentials.ServerThumbprints.Add(serverCertThumb);

var fc = new FabricClient(claimsCredentials, connection);

try
{
    var ret = fc.ClusterManager.GetClusterManifestAsync().Result;
    Console.WriteLine(ret.ToString());
}
catch (Exception e)
{
    Console.WriteLine("Connect failed: {0}", e.Message);
}
```

### <a name="connect-tooa-secure-cluster-non-interactively-using-azure-active-directory"></a><span data-ttu-id="739e9-157">Ansluta tooa säker klustret icke-interaktivt med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="739e9-157">Connect tooa secure cluster non-interactively using Azure Active Directory</span></span>

<span data-ttu-id="739e9-158">hello följande exempel förlitar sig på Microsoft.IdentityModel.Clients.ActiveDirectory, Version: 2.19.208020213.</span><span class="sxs-lookup"><span data-stu-id="739e9-158">hello following example relies on Microsoft.IdentityModel.Clients.ActiveDirectory, Version: 2.19.208020213.</span></span>

<span data-ttu-id="739e9-159">Mer information om AAD-token förvärv finns [Microsoft.IdentityModel.Clients.ActiveDirectory](https://msdn.microsoft.com/library/microsoft.identitymodel.clients.activedirectory.aspx).</span><span class="sxs-lookup"><span data-stu-id="739e9-159">For more information on AAD token acquisition, see [Microsoft.IdentityModel.Clients.ActiveDirectory](https://msdn.microsoft.com/library/microsoft.identitymodel.clients.activedirectory.aspx).</span></span>

```csharp
string tenantId = "C15CFCEA-02C1-40DC-8466-FBD0EE0B05D2";
string clientApplicationId = "118473C2-7619-46E3-A8E4-6DA8D5F56E12";
string webApplicationId = "53E6948C-0897-4DA6-B26A-EE2A38A690B4";

string token = GetAccessToken(
    tenantId,
    webApplicationId,
    clientApplicationId,
    "urn:ietf:wg:oauth:2.0:oob");

string serverCertThumb = "A8136758F4AB8962AF2BF3F27921BE1DF67F4326";
string connection = "clustername.westus.cloudapp.azure.com:19000";

var claimsCredentials = new ClaimsCredentials();
claimsCredentials.ServerThumbprints.Add(serverCertThumb);
claimsCredentials.LocalClaims = token;

var fc = new FabricClient(claimsCredentials, connection);

try
{
    var ret = fc.ClusterManager.GetClusterManifestAsync().Result;
    Console.WriteLine(ret.ToString());
}
catch (Exception e)
{
    Console.WriteLine("Connect failed: {0}", e.Message);
}

...

static string GetAccessToken(
    string tenantId,
    string resource,
    string clientId,
    string redirectUri)
{
    string authorityFormat = @"https://login.microsoftonline.com/{0}";
    string authority = string.Format(CultureInfo.InvariantCulture, authorityFormat, tenantId);
    var authContext = new AuthenticationContext(authority);

    var authResult = authContext.AcquireToken(
        resource,
        clientId,
        new UserCredential("TestAdmin@clustenametenant.onmicrosoft.com", "TestPassword"));
    return authResult.AccessToken;
}

```

### <a name="connect-tooa-secure-cluster-without-prior-metadata-knowledge-using-azure-active-directory"></a><span data-ttu-id="739e9-160">Ansluta tooa säker kluster utan föregående metadata kunskaper med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="739e9-160">Connect tooa secure cluster without prior metadata knowledge using Azure Active Directory</span></span>

<span data-ttu-id="739e9-161">hello följande exempel använder icke-interaktiv token förvärv, men hello samma metod kan vara används toobuild en anpassad interaktiva token förvärv upplevelse.</span><span class="sxs-lookup"><span data-stu-id="739e9-161">hello following example uses non-interactive token acquisition, but hello same approach can be used toobuild a custom interactive token acquisition experience.</span></span> <span data-ttu-id="739e9-162">hello Azure Active Directory metadata som behövs för token läses från klusterkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="739e9-162">hello Azure Active Directory metadata needed for token acquisition is read from cluster configuration.</span></span>

```csharp
string serverCertThumb = "A8136758F4AB8962AF2BF3F27921BE1DF67F4326";
string connection = "clustername.westus.cloudapp.azure.com:19000";

var claimsCredentials = new ClaimsCredentials();
claimsCredentials.ServerThumbprints.Add(serverCertThumb);

var fc = new FabricClient(claimsCredentials, connection);

fc.ClaimsRetrieval += (o, e) =>
{
    return GetAccessToken(e.AzureActiveDirectoryMetadata);
};

try
{
    var ret = fc.ClusterManager.GetClusterManifestAsync().Result;
    Console.WriteLine(ret.ToString());
}
catch (Exception e)
{
    Console.WriteLine("Connect failed: {0}", e.Message);
}

...

static string GetAccessToken(AzureActiveDirectoryMetadata aad)
{
    var authContext = new AuthenticationContext(aad.Authority);

    var authResult = authContext.AcquireToken(
        aad.ClusterApplication,
        aad.ClientApplication,
        new UserCredential("TestAdmin@clustenametenant.onmicrosoft.com", "TestPassword"));
    return authResult.AccessToken;
}

```

<a id="connectsecureclustersfx"></a>

## <a name="connect-tooa-secure-cluster-using-service-fabric-explorer"></a><span data-ttu-id="739e9-163">Ansluta tooa säker kluster med hjälp av Service Fabric Explorer</span><span class="sxs-lookup"><span data-stu-id="739e9-163">Connect tooa secure cluster using Service Fabric Explorer</span></span>
<span data-ttu-id="739e9-164">tooreach [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) ett kluster, peka i webbläsaren:</span><span class="sxs-lookup"><span data-stu-id="739e9-164">tooreach [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) for a given cluster, point your browser to:</span></span>

`http://<your-cluster-endpoint>:19080/Explorer`

<span data-ttu-id="739e9-165">hello fullständiga URL: en är också tillgängligt i hello klustret essentials rutan hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="739e9-165">hello full URL is also available in hello cluster essentials pane of hello Azure portal.</span></span>

### <a name="connect-tooa-secure-cluster-using-azure-active-directory"></a><span data-ttu-id="739e9-166">Ansluta tooa säker kluster med hjälp av Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="739e9-166">Connect tooa secure cluster using Azure Active Directory</span></span>

<span data-ttu-id="739e9-167">tooconnect tooa kluster som skyddas med AAD, peka webbläsaren:</span><span class="sxs-lookup"><span data-stu-id="739e9-167">tooconnect tooa cluster that is secured with AAD, point your browser to:</span></span>

`https://<your-cluster-endpoint>:19080/Explorer`

<span data-ttu-id="739e9-168">Du är automatiskt att ange toolog in med AAD.</span><span class="sxs-lookup"><span data-stu-id="739e9-168">You are automatically be prompted toolog in with AAD.</span></span>

### <a name="connect-tooa-secure-cluster-using-a-client-certificate"></a><span data-ttu-id="739e9-169">Ansluta tooa säker kluster med ett klientcertifikat</span><span class="sxs-lookup"><span data-stu-id="739e9-169">Connect tooa secure cluster using a client certificate</span></span>

<span data-ttu-id="739e9-170">tooconnect tooa kluster som skyddas med certifikat, peka webbläsaren:</span><span class="sxs-lookup"><span data-stu-id="739e9-170">tooconnect tooa cluster that is secured with certificates, point your browser to:</span></span>

`https://<your-cluster-endpoint>:19080/Explorer`

<span data-ttu-id="739e9-171">Du är automatiskt att ange tooselect ett klientcertifikat.</span><span class="sxs-lookup"><span data-stu-id="739e9-171">You are automatically be prompted tooselect a client certificate.</span></span>

<a id="connectsecureclustersetupclientcert"></a>
## <a name="set-up-a-client-certificate-on-hello-remote-computer"></a><span data-ttu-id="739e9-172">Ställ in ett klientcertifikat på hello fjärrdator</span><span class="sxs-lookup"><span data-stu-id="739e9-172">Set up a client certificate on hello remote computer</span></span>
<span data-ttu-id="739e9-173">Minst två certifikat ska användas för att skydda hello kluster, ett för hello klustret och server-certifikat och en annan för klientåtkomst.</span><span class="sxs-lookup"><span data-stu-id="739e9-173">At least two certificates should be used for securing hello cluster, one for hello cluster and server certificate and another for client access.</span></span>  <span data-ttu-id="739e9-174">Vi rekommenderar att du också använda ytterligare sekundära certifikat- och klientcertifikat åtkomst.</span><span class="sxs-lookup"><span data-stu-id="739e9-174">We recommend that you also use additional secondary certificates and client access certificates.</span></span>  <span data-ttu-id="739e9-175">toosecure hello kommunikation mellan en klient och en nod med säkerhet och du behöver tooobtain och installera hello klientcertifikat.</span><span class="sxs-lookup"><span data-stu-id="739e9-175">toosecure hello communication between a client and a cluster node using certificate security, you first need tooobtain and install hello client certificate.</span></span> <span data-ttu-id="739e9-176">hello certifikat installeras i hello (min) datorarkivet hello lokal dator eller hello aktuell användare.</span><span class="sxs-lookup"><span data-stu-id="739e9-176">hello certificate can be installed into hello Personal (My) store of hello local computer or hello current user.</span></span>  <span data-ttu-id="739e9-177">Du måste också hello tumavtryck hello servercertifikatet så att hello klienten kan autentisera hello klustret.</span><span class="sxs-lookup"><span data-stu-id="739e9-177">You also need hello thumbprint of hello server certificate so that hello client can authenticate hello cluster.</span></span>

<span data-ttu-id="739e9-178">Kör följande PowerShell-cmdlet tooset in hello klientcertifikatet på hello datorn från vilken du åtkomst till klustret hello hello.</span><span class="sxs-lookup"><span data-stu-id="739e9-178">Run hello following PowerShell cmdlet tooset up hello client certificate on hello computer from which you access hello cluster.</span></span>

```powershell
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\docDemo\certs\DocDemoClusterCert.pfx `
        -Password (ConvertTo-SecureString -String test -AsPlainText -Force)
```

<span data-ttu-id="739e9-179">Om det är ett självsignerat certifikat måste tooimport den tooyour datorn ”betrodda personer” lagra innan du kan använda det här certifikatet tooconnect tooa säker klustret.</span><span class="sxs-lookup"><span data-stu-id="739e9-179">If it is a self-signed certificate, you need tooimport it tooyour machine's "trusted people" store before you can use this certificate tooconnect tooa secure cluster.</span></span>

```powershell
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\TrustedPeople `
-FilePath C:\docDemo\certs\DocDemoClusterCert.pfx `
-Password (ConvertTo-SecureString -String test -AsPlainText -Force)
```

## <a name="next-steps"></a><span data-ttu-id="739e9-180">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="739e9-180">Next steps</span></span>

* [<span data-ttu-id="739e9-181">Uppgraderingsprocessen för Service Fabric-kluster och förväntningar från dig</span><span class="sxs-lookup"><span data-stu-id="739e9-181">Service Fabric Cluster upgrade process and expectations from you</span></span>](service-fabric-cluster-upgrade.md)
* [<span data-ttu-id="739e9-182">Hantera dina Service Fabric-program i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="739e9-182">Managing your Service Fabric applications in Visual Studio</span></span>](service-fabric-manage-application-in-visual-studio.md)
* [<span data-ttu-id="739e9-183">Tjänstens hälsa för Fabric modellen introduktion</span><span class="sxs-lookup"><span data-stu-id="739e9-183">Service Fabric Health model introduction</span></span>](service-fabric-health-introduction.md)
* [<span data-ttu-id="739e9-184">Säkerhet och RunAs</span><span class="sxs-lookup"><span data-stu-id="739e9-184">Application Security and RunAs</span></span>](service-fabric-application-runas-security.md)
* [<span data-ttu-id="739e9-185">Kom igång med Service Fabric CLI</span><span class="sxs-lookup"><span data-stu-id="739e9-185">Getting started with Service Fabric CLI</span></span>](service-fabric-cli.md)
