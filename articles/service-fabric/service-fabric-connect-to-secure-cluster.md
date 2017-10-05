---
title: "Ansluta säkert till ett Azure Service Fabric-kluster | Microsoft Docs"
description: "Beskriver hur du autentiserar klientåtkomst till ett Service Fabric-kluster och hur för säker kommunikation mellan klienter och ett kluster."
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
ms.openlocfilehash: d6a13ceb8ccd9207ecacc166247535d496d5dec7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="connect-to-a-secure-cluster"></a><span data-ttu-id="16699-103">Ansluta till ett säkert kluster</span><span class="sxs-lookup"><span data-stu-id="16699-103">Connect to a secure cluster</span></span>

<span data-ttu-id="16699-104">När en klient ansluter till en klusternod för Service Fabric, kan klienten vara autentiserad och säker kommunikation upprättas med hjälp av certifikatet säkerhets- eller Azure Active Directory (AAD).</span><span class="sxs-lookup"><span data-stu-id="16699-104">When a client connects to a Service Fabric cluster node, the client can be authenticated and secure communication established using certificate security or Azure Active Directory (AAD).</span></span> <span data-ttu-id="16699-105">Den här autentiseringen säkerställer att endast behöriga användare har åtkomst till klustret och distribuerade program och utföra administrativa uppgifter.</span><span class="sxs-lookup"><span data-stu-id="16699-105">This authentication ensures that only authorized users can access the cluster and deployed applications and perform management tasks.</span></span>  <span data-ttu-id="16699-106">Certifikat eller AAD säkerhet måste ha tidigare aktiverats på klustret när klustret skapades.</span><span class="sxs-lookup"><span data-stu-id="16699-106">Certificate or AAD security must have been previously enabled on the cluster when the cluster was created.</span></span>  <span data-ttu-id="16699-107">Mer information om kluster säkerhetsscenarier finns [kluster säkerhet](service-fabric-cluster-security.md).</span><span class="sxs-lookup"><span data-stu-id="16699-107">For more information on cluster security scenarios, see [Cluster security](service-fabric-cluster-security.md).</span></span> <span data-ttu-id="16699-108">Om du ansluter till ett kluster som skyddas med certifikat, [konfigurera klientcertifikatet](service-fabric-connect-to-secure-cluster.md#connectsecureclustersetupclientcert) på den dator som ansluter till klustret.</span><span class="sxs-lookup"><span data-stu-id="16699-108">If you are connecting to a cluster secured with certificates, [set up the client certificate](service-fabric-connect-to-secure-cluster.md#connectsecureclustersetupclientcert) on the computer that connects to the cluster.</span></span> 

<a id="connectsecureclustercli"></a> 

## <a name="connect-to-a-secure-cluster-using-azure-service-fabric-cli-sfctl"></a><span data-ttu-id="16699-109">Ansluta till en säker kluster med hjälp av Azure Service Fabric CLI (sfctl)</span><span class="sxs-lookup"><span data-stu-id="16699-109">Connect to a secure cluster using Azure Service Fabric CLI (sfctl)</span></span>

<span data-ttu-id="16699-110">Det finns några olika sätt att ansluta till en säker kluster med hjälp av Service Fabric-CLI (sfctl).</span><span class="sxs-lookup"><span data-stu-id="16699-110">There are a few different ways to connect to a secure cluster using the Service Fabric CLI (sfctl).</span></span> <span data-ttu-id="16699-111">När du använder ett klientcertifikat för autentisering måste certifikatinformationen matcha ett certifikat som distribuerats till klusternoderna.</span><span class="sxs-lookup"><span data-stu-id="16699-111">When using a client certificate for authentication, the certificate details must match a certificate deployed to the cluster nodes.</span></span> <span data-ttu-id="16699-112">Om ditt certifikat har certifikatutfärdare (CA), måste du dessutom ange betrodda certifikatutfärdare.</span><span class="sxs-lookup"><span data-stu-id="16699-112">If your certificate has Certificate Authorities (CAs), you need to additionally specify the trusted CAs.</span></span>

<span data-ttu-id="16699-113">Du kan ansluta till ett kluster med hjälp av den `sfctl cluster select` kommando.</span><span class="sxs-lookup"><span data-stu-id="16699-113">You can connect to a cluster using the `sfctl cluster select` command.</span></span>

<span data-ttu-id="16699-114">Klientcertifikat kan anges i två olika sätt, som ett certifikat och nyckelpar, eller som en enda pem-fil.</span><span class="sxs-lookup"><span data-stu-id="16699-114">Client certificates can be specified in two different fashions, either as a cert and key pair, or as a single pem file.</span></span> <span data-ttu-id="16699-115">För lösenordsskyddade `pem` filer, uppmanas du att automatiskt för att ange lösenordet.</span><span class="sxs-lookup"><span data-stu-id="16699-115">For password protected `pem` files, you will be prompted automatically to enter the password.</span></span>

<span data-ttu-id="16699-116">Om du vill ange klientcertifikatet som en pem-fil, ange sökvägen till filen i den `--pem` argumentet.</span><span class="sxs-lookup"><span data-stu-id="16699-116">To specify the client certificate as a pem file, specify the file path in the `--pem` argument.</span></span> <span data-ttu-id="16699-117">Exempel:</span><span class="sxs-lookup"><span data-stu-id="16699-117">For example:</span></span>

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

<span data-ttu-id="16699-118">Lösenordsskyddad pem-filer ska fråga efter lösenord innan du kör ett kommando.</span><span class="sxs-lookup"><span data-stu-id="16699-118">Password protected pem files will prompt for password prior to running any command.</span></span>

<span data-ttu-id="16699-119">Nyckeln par används för att ange ett certifikat, de `--cert` och `--key` argument för att ange sökvägar till varje respektive fil.</span><span class="sxs-lookup"><span data-stu-id="16699-119">To specify a cert, key pair use the `--cert` and `--key` arguments to specify the file paths to each respective file.</span></span>

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --cert ./client.crt --key ./keyfile.key
```

<span data-ttu-id="16699-120">Certifikat som används för att säkra test- eller dev kluster misslyckas ibland valideringen av servercertifikatet.</span><span class="sxs-lookup"><span data-stu-id="16699-120">Sometimes certificates used to secure test or dev clusters fail certificate validation.</span></span> <span data-ttu-id="16699-121">Om du vill kringgå certifikatverifiering, ange den `--no-verify` alternativet.</span><span class="sxs-lookup"><span data-stu-id="16699-121">To bypass certificate verification, specify the `--no-verify` option.</span></span> <span data-ttu-id="16699-122">Exempel:</span><span class="sxs-lookup"><span data-stu-id="16699-122">For example:</span></span>

> [!WARNING]
> <span data-ttu-id="16699-123">Använd inte den `no-verify` alternativ när du ansluter till produktion Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="16699-123">Do not use the `no-verify` option when connecting to production Service Fabric clusters.</span></span>

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem --no-verify
```

<span data-ttu-id="16699-124">Du kan dessutom ange sökvägar till kataloger av certifikat för betrodd Certifikatutfärdare eller enskilda certifikat.</span><span class="sxs-lookup"><span data-stu-id="16699-124">In addition, you can specify paths to directories of trusted CA certs, or individual certs.</span></span> <span data-ttu-id="16699-125">Använd för att ange dessa sökvägar i `--ca` argumentet.</span><span class="sxs-lookup"><span data-stu-id="16699-125">To specify these paths, use the `--ca` argument.</span></span> <span data-ttu-id="16699-126">Exempel:</span><span class="sxs-lookup"><span data-stu-id="16699-126">For example:</span></span>

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem --ca ./trusted_ca
```

<span data-ttu-id="16699-127">När du ansluter du ska kunna [köra andra sfctl kommandon](service-fabric-cli.md) att interagera med klustret.</span><span class="sxs-lookup"><span data-stu-id="16699-127">After you connect, you should be able to [run other sfctl commands](service-fabric-cli.md) to interact with the cluster.</span></span>

<a id="connectsecurecluster"></a>

## <a name="connect-to-a-cluster-using-powershell"></a><span data-ttu-id="16699-128">Ansluta till ett kluster med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="16699-128">Connect to a cluster using PowerShell</span></span>
<span data-ttu-id="16699-129">Innan du utför åtgärder på ett kluster med hjälp av PowerShell först upprätta en anslutning till klustret.</span><span class="sxs-lookup"><span data-stu-id="16699-129">Before you perform operations on a cluster through PowerShell, first establish a connection to the cluster.</span></span> <span data-ttu-id="16699-130">Kluster-anslutning används för alla efterföljande kommandon i den angivna PowerShell-sessionen.</span><span class="sxs-lookup"><span data-stu-id="16699-130">The cluster connection is used for all subsequent commands in the given PowerShell session.</span></span>

### <a name="connect-to-an-unsecure-cluster"></a><span data-ttu-id="16699-131">Ansluta till ett oskyddat kluster</span><span class="sxs-lookup"><span data-stu-id="16699-131">Connect to an unsecure cluster</span></span>

<span data-ttu-id="16699-132">För att ansluta till ett oskyddat kluster, ger slutpunktsadress klustret till det **Connect-ServiceFabricCluster** kommando:</span><span class="sxs-lookup"><span data-stu-id="16699-132">To connect to an unsecure cluster, provide the cluster endpoint address to the **Connect-ServiceFabricCluster** command:</span></span>

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 
```

### <a name="connect-to-a-secure-cluster-using-azure-active-directory"></a><span data-ttu-id="16699-133">Ansluta till en säker kluster med hjälp av Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="16699-133">Connect to a secure cluster using Azure Active Directory</span></span>

<span data-ttu-id="16699-134">Ange tumavtrycket klustret för att ansluta till en säker kluster som använder Azure Active Directory för att auktorisera administratörsåtkomst för klustret, och använda den *AzureActiveDirectory* flaggan.</span><span class="sxs-lookup"><span data-stu-id="16699-134">To connect to a secure cluster that uses Azure Active Directory to authorize cluster administrator access, provide the cluster certificate thumbprint and use the *AzureActiveDirectory* flag.</span></span>  

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
-ServerCertThumbprint <Server Certificate Thumbprint> `
-AzureActiveDirectory
```

### <a name="connect-to-a-secure-cluster-using-a-client-certificate"></a><span data-ttu-id="16699-135">Ansluta till en säker kluster som använder ett klientcertifikat</span><span class="sxs-lookup"><span data-stu-id="16699-135">Connect to a secure cluster using a client certificate</span></span>
<span data-ttu-id="16699-136">Kör följande PowerShell-kommando för att ansluta till en säker kluster som använder klientcertifikat för att auktorisera administratörsåtkomst.</span><span class="sxs-lookup"><span data-stu-id="16699-136">Run the following PowerShell command to connect to a secure cluster that uses client certificates to authorize administrator access.</span></span> <span data-ttu-id="16699-137">Ange tumavtrycket kluster och tumavtrycket för det klientcertifikat som har beviljats behörigheter för klusterhantering.</span><span class="sxs-lookup"><span data-stu-id="16699-137">Provide the cluster certificate thumbprint and the thumbprint of the client certificate that has been granted permissions for cluster management.</span></span> <span data-ttu-id="16699-138">Certifikatinformation måste matcha ett certifikat på klusternoderna.</span><span class="sxs-lookup"><span data-stu-id="16699-138">The certificate details must match a certificate on the cluster nodes.</span></span>

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint <Certificate Thumbprint> `
          -FindType FindByThumbprint -FindValue <Certificate Thumbprint> `
          -StoreLocation CurrentUser -StoreName My
```

<span data-ttu-id="16699-139">*ServerCertThumbprint* är tumavtrycket för certifikatet installeras på klusternoderna.</span><span class="sxs-lookup"><span data-stu-id="16699-139">*ServerCertThumbprint* is the thumbprint of the server certificate installed on the cluster nodes.</span></span> <span data-ttu-id="16699-140">*FindValue* är tumavtrycket för admin-klientcertifikat.</span><span class="sxs-lookup"><span data-stu-id="16699-140">*FindValue* is the thumbprint of the admin client certificate.</span></span>
<span data-ttu-id="16699-141">När parametrarna är ifyllda ser kommandot ut som i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="16699-141">When the parameters are filled in, the command looks like the following example:</span></span> 

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint clustername.westus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint A8136758F4AB8962AF2BF3F27921BE1DF67F4326 `
          -FindType FindByThumbprint -FindValue 71DE04467C9ED0544D021098BCD44C71E183414E `
          -StoreLocation CurrentUser -StoreName My
```

### <a name="connect-to-a-secure-cluster-using-windows-active-directory"></a><span data-ttu-id="16699-142">Ansluta till en säker kluster med hjälp av Windows Active Directory</span><span class="sxs-lookup"><span data-stu-id="16699-142">Connect to a secure cluster using Windows Active Directory</span></span>
<span data-ttu-id="16699-143">Om fristående klustret distribueras med hjälp av AD-säkerhetsgrupp, kan du ansluta till klustret genom att lägga till växeln ”WindowsCredential”.</span><span class="sxs-lookup"><span data-stu-id="16699-143">If your standalone cluster is deployed using AD security, connect to the cluster by appending the switch "WindowsCredential".</span></span>

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
          -WindowsCredential
```

<a id="connectsecureclusterfabricclient"></a>

## <a name="connect-to-a-cluster-using-the-fabricclient-apis"></a><span data-ttu-id="16699-144">Ansluta till ett kluster med FabricClient APIs</span><span class="sxs-lookup"><span data-stu-id="16699-144">Connect to a cluster using the FabricClient APIs</span></span>
<span data-ttu-id="16699-145">Service Fabric-SDK tillhandahåller den [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) klass för klusterhantering.</span><span class="sxs-lookup"><span data-stu-id="16699-145">The Service Fabric SDK provides the [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) class for cluster management.</span></span> <span data-ttu-id="16699-146">Om du vill använda FabricClient APIs hämta Microsoft.ServiceFabric NuGet-paketet.</span><span class="sxs-lookup"><span data-stu-id="16699-146">To use the FabricClient APIs, get the Microsoft.ServiceFabric NuGet package.</span></span>

### <a name="connect-to-an-unsecure-cluster"></a><span data-ttu-id="16699-147">Ansluta till ett oskyddat kluster</span><span class="sxs-lookup"><span data-stu-id="16699-147">Connect to an unsecure cluster</span></span>

<span data-ttu-id="16699-148">Skapa en instans av FabricClient för att ansluta till en oskyddad fjärrkluster och tillhandahålla klusteradress:</span><span class="sxs-lookup"><span data-stu-id="16699-148">To connect to a remote unsecured cluster, create a FabricClient instance and provide the cluster address:</span></span>

```csharp
FabricClient fabricClient = new FabricClient("clustername.westus.cloudapp.azure.com:19000");
```

<span data-ttu-id="16699-149">För kod som körs från inom ett kluster, till exempel i en tillförlitlig tjänst, skapa en FabricClient *utan* anger klusteradress.</span><span class="sxs-lookup"><span data-stu-id="16699-149">For code that is running from within a cluster, for example, in a Reliable Service, create a FabricClient *without* specifying the cluster address.</span></span> <span data-ttu-id="16699-150">FabricClient ansluter till lokala management-gateway på noden koden körs på, undvika en extra nätverket hopp.</span><span class="sxs-lookup"><span data-stu-id="16699-150">FabricClient connects to the local management gateway on the node the code is currently running on, avoiding an extra network hop.</span></span>

```csharp
FabricClient fabricClient = new FabricClient();
```

### <a name="connect-to-a-secure-cluster-using-a-client-certificate"></a><span data-ttu-id="16699-151">Ansluta till en säker kluster som använder ett klientcertifikat</span><span class="sxs-lookup"><span data-stu-id="16699-151">Connect to a secure cluster using a client certificate</span></span>

<span data-ttu-id="16699-152">Noder i klustret måste ha giltiga certifikat vars nätverksnamn eller DNS-namnet i SAN visas i den [RemoteCommonNames egenskapen](https://docs.microsoft.com/dotnet/api/system.fabric.x509credentials#System_Fabric_X509Credentials_RemoteCommonNames) på [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient).</span><span class="sxs-lookup"><span data-stu-id="16699-152">The nodes in the cluster must have valid certificates whose common name or DNS name in SAN appears in the [RemoteCommonNames property](https://docs.microsoft.com/dotnet/api/system.fabric.x509credentials#System_Fabric_X509Credentials_RemoteCommonNames) set on [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient).</span></span> <span data-ttu-id="16699-153">Efter den här processen kan ömsesidig autentisering mellan klienten och klusternoder.</span><span class="sxs-lookup"><span data-stu-id="16699-153">Following this process enables mutual authentication between the client and the cluster nodes.</span></span>

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

### <a name="connect-to-a-secure-cluster-interactively-using-azure-active-directory"></a><span data-ttu-id="16699-154">Ansluta till en säker interaktivt med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="16699-154">Connect to a secure cluster interactively using Azure Active Directory</span></span>

<span data-ttu-id="16699-155">I följande exempel används Azure Active Directory för identitets- och klientcertifikat för serveridentitet.</span><span class="sxs-lookup"><span data-stu-id="16699-155">The following example uses Azure Active Directory for client identity and server certificate for server identity.</span></span>

<span data-ttu-id="16699-156">En dialogruta visas automatiskt för interaktiv inloggning vid anslutning till klustret.</span><span class="sxs-lookup"><span data-stu-id="16699-156">A dialog window automatically pops up for interactive sign-in upon connecting to the cluster.</span></span>

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

### <a name="connect-to-a-secure-cluster-non-interactively-using-azure-active-directory"></a><span data-ttu-id="16699-157">Ansluta till en säker icke-interaktivt med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="16699-157">Connect to a secure cluster non-interactively using Azure Active Directory</span></span>

<span data-ttu-id="16699-158">I följande exempel är beroende av Microsoft.IdentityModel.Clients.ActiveDirectory, Version: 2.19.208020213.</span><span class="sxs-lookup"><span data-stu-id="16699-158">The following example relies on Microsoft.IdentityModel.Clients.ActiveDirectory, Version: 2.19.208020213.</span></span>

<span data-ttu-id="16699-159">Mer information om AAD-token förvärv finns [Microsoft.IdentityModel.Clients.ActiveDirectory](https://msdn.microsoft.com/library/microsoft.identitymodel.clients.activedirectory.aspx).</span><span class="sxs-lookup"><span data-stu-id="16699-159">For more information on AAD token acquisition, see [Microsoft.IdentityModel.Clients.ActiveDirectory](https://msdn.microsoft.com/library/microsoft.identitymodel.clients.activedirectory.aspx).</span></span>

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

### <a name="connect-to-a-secure-cluster-without-prior-metadata-knowledge-using-azure-active-directory"></a><span data-ttu-id="16699-160">Ansluta till en säker kluster utan föregående metadata kunskaper med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="16699-160">Connect to a secure cluster without prior metadata knowledge using Azure Active Directory</span></span>

<span data-ttu-id="16699-161">I följande exempel används icke-interaktiv token förvärv, men samma metod som kan användas för att skapa en anpassad interaktiva token förvärv upplevelse.</span><span class="sxs-lookup"><span data-stu-id="16699-161">The following example uses non-interactive token acquisition, but the same approach can be used to build a custom interactive token acquisition experience.</span></span> <span data-ttu-id="16699-162">Azure Active Directory-metadata som behövs för token läses från klusterkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="16699-162">The Azure Active Directory metadata needed for token acquisition is read from cluster configuration.</span></span>

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

## <a name="connect-to-a-secure-cluster-using-service-fabric-explorer"></a><span data-ttu-id="16699-163">Ansluta till en säker kluster med hjälp av Service Fabric Explorer</span><span class="sxs-lookup"><span data-stu-id="16699-163">Connect to a secure cluster using Service Fabric Explorer</span></span>
<span data-ttu-id="16699-164">Att nå [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) ett kluster, peka i webbläsaren:</span><span class="sxs-lookup"><span data-stu-id="16699-164">To reach [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) for a given cluster, point your browser to:</span></span>

`http://<your-cluster-endpoint>:19080/Explorer`

<span data-ttu-id="16699-165">Fullständig URL finns också i fönstret klustret essentials i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="16699-165">The full URL is also available in the cluster essentials pane of the Azure portal.</span></span>

### <a name="connect-to-a-secure-cluster-using-azure-active-directory"></a><span data-ttu-id="16699-166">Ansluta till en säker kluster med hjälp av Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="16699-166">Connect to a secure cluster using Azure Active Directory</span></span>

<span data-ttu-id="16699-167">Peka webbläsaren för att ansluta till ett kluster som skyddas med AAD:</span><span class="sxs-lookup"><span data-stu-id="16699-167">To connect to a cluster that is secured with AAD, point your browser to:</span></span>

`https://<your-cluster-endpoint>:19080/Explorer`

<span data-ttu-id="16699-168">Du uppmanas automatiskt att logga in med AAD.</span><span class="sxs-lookup"><span data-stu-id="16699-168">You are automatically be prompted to log in with AAD.</span></span>

### <a name="connect-to-a-secure-cluster-using-a-client-certificate"></a><span data-ttu-id="16699-169">Ansluta till en säker kluster som använder ett klientcertifikat</span><span class="sxs-lookup"><span data-stu-id="16699-169">Connect to a secure cluster using a client certificate</span></span>

<span data-ttu-id="16699-170">Peka webbläsaren för att ansluta till ett kluster som skyddas av certifikat:</span><span class="sxs-lookup"><span data-stu-id="16699-170">To connect to a cluster that is secured with certificates, point your browser to:</span></span>

`https://<your-cluster-endpoint>:19080/Explorer`

<span data-ttu-id="16699-171">Du uppmanas automatiskt att välja ett klientcertifikat.</span><span class="sxs-lookup"><span data-stu-id="16699-171">You are automatically be prompted to select a client certificate.</span></span>

<a id="connectsecureclustersetupclientcert"></a>
## <a name="set-up-a-client-certificate-on-the-remote-computer"></a><span data-ttu-id="16699-172">Ställ in ett klientcertifikat på fjärrdatorn</span><span class="sxs-lookup"><span data-stu-id="16699-172">Set up a client certificate on the remote computer</span></span>
<span data-ttu-id="16699-173">Minst två certifikat ska användas för att skydda klustret, en för klustret och server-certifikat och en annan för klientåtkomst.</span><span class="sxs-lookup"><span data-stu-id="16699-173">At least two certificates should be used for securing the cluster, one for the cluster and server certificate and another for client access.</span></span>  <span data-ttu-id="16699-174">Vi rekommenderar att du också använda ytterligare sekundära certifikat- och klientcertifikat åtkomst.</span><span class="sxs-lookup"><span data-stu-id="16699-174">We recommend that you also use additional secondary certificates and client access certificates.</span></span>  <span data-ttu-id="16699-175">För att säkra kommunikationen mellan en klient och en klusternod som använder för Certifikatsäkerhet, måste du först hämta och installera klientcertifikatet.</span><span class="sxs-lookup"><span data-stu-id="16699-175">To secure the communication between a client and a cluster node using certificate security, you first need to obtain and install the client certificate.</span></span> <span data-ttu-id="16699-176">Du kan installera certifikatet till det personliga (min) arkivet på den lokala datorn eller den aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="16699-176">The certificate can be installed into the Personal (My) store of the local computer or the current user.</span></span>  <span data-ttu-id="16699-177">Du måste också tumavtrycket för certifikatet så att klienten kan autentisera till klustret.</span><span class="sxs-lookup"><span data-stu-id="16699-177">You also need the thumbprint of the server certificate so that the client can authenticate the cluster.</span></span>

<span data-ttu-id="16699-178">Kör följande PowerShell-cmdlet för att ställa in klientcertifikat på den dator där du får åtkomst till klustret.</span><span class="sxs-lookup"><span data-stu-id="16699-178">Run the following PowerShell cmdlet to set up the client certificate on the computer from which you access the cluster.</span></span>

```powershell
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\docDemo\certs\DocDemoClusterCert.pfx `
        -Password (ConvertTo-SecureString -String test -AsPlainText -Force)
```

<span data-ttu-id="16699-179">Om det är ett självsignerat certifikat, måste du importera den till din dator ”betrodda personer” store innan du kan använda det här certifikatet för att ansluta till en säker kluster.</span><span class="sxs-lookup"><span data-stu-id="16699-179">If it is a self-signed certificate, you need to import it to your machine's "trusted people" store before you can use this certificate to connect to a secure cluster.</span></span>

```powershell
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\TrustedPeople `
-FilePath C:\docDemo\certs\DocDemoClusterCert.pfx `
-Password (ConvertTo-SecureString -String test -AsPlainText -Force)
```

## <a name="next-steps"></a><span data-ttu-id="16699-180">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="16699-180">Next steps</span></span>

* [<span data-ttu-id="16699-181">Uppgraderingsprocessen för Service Fabric-kluster och förväntningar från dig</span><span class="sxs-lookup"><span data-stu-id="16699-181">Service Fabric Cluster upgrade process and expectations from you</span></span>](service-fabric-cluster-upgrade.md)
* [<span data-ttu-id="16699-182">Hantera dina Service Fabric-program i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="16699-182">Managing your Service Fabric applications in Visual Studio</span></span>](service-fabric-manage-application-in-visual-studio.md)
* [<span data-ttu-id="16699-183">Tjänstens hälsa för Fabric modellen introduktion</span><span class="sxs-lookup"><span data-stu-id="16699-183">Service Fabric Health model introduction</span></span>](service-fabric-health-introduction.md)
* [<span data-ttu-id="16699-184">Säkerhet och RunAs</span><span class="sxs-lookup"><span data-stu-id="16699-184">Application Security and RunAs</span></span>](service-fabric-application-runas-security.md)
* [<span data-ttu-id="16699-185">Kom igång med Service Fabric CLI</span><span class="sxs-lookup"><span data-stu-id="16699-185">Getting started with Service Fabric CLI</span></span>](service-fabric-cli.md)
