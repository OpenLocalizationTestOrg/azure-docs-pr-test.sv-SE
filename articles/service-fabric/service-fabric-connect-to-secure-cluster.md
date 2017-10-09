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
# <a name="connect-tooa-secure-cluster"></a>Ansluta tooa säker kluster

När en klient ansluter tooa Service Fabric-nod i klustret, kan hello-klient vara autentiserad och säker kommunikation upprättas med hjälp av certifikatet säkerhets- eller Azure Active Directory (AAD). Den här autentiseringen säkerställer att endast auktoriserade användare kan komma åt hello kluster och distribuerade program och utföra administrativa uppgifter.  Certifikat eller AAD säkerhet måste ha tidigare aktiverats på hello klustret när hello klustret har skapats.  Mer information om kluster säkerhetsscenarier finns [kluster säkerhet](service-fabric-cluster-security.md). Om du ansluter tooa kluster skyddas med certifikat, [konfigurera hello klientcertifikat](service-fabric-connect-to-secure-cluster.md#connectsecureclustersetupclientcert) på hello-dator som ansluter toohello klustret. 

<a id="connectsecureclustercli"></a> 

## <a name="connect-tooa-secure-cluster-using-azure-service-fabric-cli-sfctl"></a>Ansluta tooa säker kluster med hjälp av Azure Service Fabric CLI (sfctl)

Det finns några olika sätt tooconnect tooa säker kluster med hjälp av hello Service Fabric CLI (sfctl). När du använder ett klientcertifikat för autentisering, distribueras hello certifikat information måste matcha ett certifikat toohello klusternoder. Om ditt certifikat har certifikatutfärdare (CA), måste du tooadditionally ange hello betrodda certifikatutfärdare.

Du kan ansluta tooa kluster med hjälp av hello `sfctl cluster select` kommando.

Klientcertifikat kan anges i två olika sätt, som ett certifikat och nyckelpar, eller som en enda pem-fil. För lösenordsskyddade `pem` filer, kommer du att uppmanas automatiskt tooenter hello lösenord.

toospecify hello klientcertifikat som en pem-fil, ange sökväg till hello i hello `--pem` argumentet. Exempel:

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

Lösenordet skyddas pem-filer ombeds du att lösenordet tidigare toorunning ett kommando.

toospecify ett certifikat, nyckelpar använda hello `--cert` och `--key` argument toospecify hello sökvägar tooeach respektive fil.

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --cert ./client.crt --key ./keyfile.key
```

Certifikat används ibland toosecure test eller dev kluster misslyckas valideringen av servercertifikatet. toobypass certifikat verifiering, ange hello `--no-verify` alternativet. Exempel:

> [!WARNING]
> Använd inte hello `no-verify` alternativ när du ansluter tooproduction Service Fabric-kluster.

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem --no-verify
```

Du kan dessutom ange sökvägar toodirectories av certifikat för betrodd Certifikatutfärdare eller enskilda certifikat. toospecify dessa sökvägar använda hello `--ca` argumentet. Exempel:

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem --ca ./trusted_ca
```

När du ansluter du borde för[köra andra sfctl kommandon](service-fabric-cli.md) toointeract med hello-kluster.

<a id="connectsecurecluster"></a>

## <a name="connect-tooa-cluster-using-powershell"></a>Ansluta tooa kluster med hjälp av PowerShell
Upprätta en anslutning toohello klustret innan du utför åtgärder på ett kluster med hjälp av PowerShell. hello klustret anslutningen används för alla efterföljande kommandon i hello angivna PowerShell-session.

### <a name="connect-tooan-unsecure-cluster"></a>Ansluta tooan oskyddade kluster

tooconnect tooan oskyddade kluster, ange hello klustret endpoint adress toohello **Connect-ServiceFabricCluster** kommando:

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 
```

### <a name="connect-tooa-secure-cluster-using-azure-active-directory"></a>Ansluta tooa säker kluster med hjälp av Azure Active Directory

tooconnect tooa säker kluster som använder Azure Active Directory tooauthorize klustret administratörsåtkomst, ange hello tumavtryck för klustret och använda hello *AzureActiveDirectory* flaggan.  

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
-ServerCertThumbprint <Server Certificate Thumbprint> `
-AzureActiveDirectory
```

### <a name="connect-tooa-secure-cluster-using-a-client-certificate"></a>Ansluta tooa säker kluster med ett klientcertifikat
Kör hello följande PowerShell-kommando tooconnect tooa säker kluster som använder klienten certifikat tooauthorize administratörsåtkomst. Ange hello tumavtryck för klustret och hello tumavtryck hello klientcertifikat som har beviljats behörigheter för klusterhantering. information om hello certifikat måste matcha ett certifikat på hello klusternoder.

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint <Certificate Thumbprint> `
          -FindType FindByThumbprint -FindValue <Certificate Thumbprint> `
          -StoreLocation CurrentUser -StoreName My
```

*ServerCertThumbprint* är hello tumavtryck hello servercertifikat installerat på hello klusternoder. *FindValue* är hello hello admin klienten certifikatets tumavtryck.
När hello parametrar är ifyllda hello kommando som ser ut så hello följande exempel: 

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint clustername.westus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint A8136758F4AB8962AF2BF3F27921BE1DF67F4326 `
          -FindType FindByThumbprint -FindValue 71DE04467C9ED0544D021098BCD44C71E183414E `
          -StoreLocation CurrentUser -StoreName My
```

### <a name="connect-tooa-secure-cluster-using-windows-active-directory"></a>Ansluta tooa säker kluster med hjälp av Windows Active Directory
Om fristående klustret distribueras med hjälp av AD-säkerhetsgrupp, kan du ansluta toohello klustret genom att lägga till hello växel ”WindowsCredential”.

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
          -WindowsCredential
```

<a id="connectsecureclusterfabricclient"></a>

## <a name="connect-tooa-cluster-using-hello-fabricclient-apis"></a>Ansluta tooa kluster med hjälp av hello FabricClient APIs
hello Service Fabric SDK innehåller hello [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) klass för klusterhantering. toouse hello FabricClient APIs hämta hello Microsoft.ServiceFabric NuGet-paketet.

### <a name="connect-tooan-unsecure-cluster"></a>Ansluta tooan oskyddade kluster

tooconnect tooa oskyddade kluster, skapa en FabricClient-instans och ange hello klusteradress:

```csharp
FabricClient fabricClient = new FabricClient("clustername.westus.cloudapp.azure.com:19000");
```

För kod som körs från inom ett kluster, till exempel i en tillförlitlig tjänst, skapa en FabricClient *utan* att ange hello klusteradress. FabricClient ansluter toohello lokal hantering gateway på hello nod hello kod körs på, undvika en extra nätverket hopp.

```csharp
FabricClient fabricClient = new FabricClient();
```

### <a name="connect-tooa-secure-cluster-using-a-client-certificate"></a>Ansluta tooa säker kluster med ett klientcertifikat

hello-noder i klustret hello måste ha giltiga certifikat vars nätverksnamn eller DNS-namnet i SAN visas i hello [RemoteCommonNames egenskapen](https://docs.microsoft.com/dotnet/api/system.fabric.x509credentials#System_Fabric_X509Credentials_RemoteCommonNames) på [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient). Efter den här processen kan ömsesidig autentisering mellan hello-klienten och hello klusternoder.

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

### <a name="connect-tooa-secure-cluster-interactively-using-azure-active-directory"></a>Ansluta tooa säker klustret interaktivt med Azure Active Directory

hello följande exempel använder Azure Active Directory för identitets- och klientcertifikat för serveridentitet.

En dialogruta visas automatiskt för interaktiv inloggning vid anslutning toohello klustret.

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

### <a name="connect-tooa-secure-cluster-non-interactively-using-azure-active-directory"></a>Ansluta tooa säker klustret icke-interaktivt med Azure Active Directory

hello följande exempel förlitar sig på Microsoft.IdentityModel.Clients.ActiveDirectory, Version: 2.19.208020213.

Mer information om AAD-token förvärv finns [Microsoft.IdentityModel.Clients.ActiveDirectory](https://msdn.microsoft.com/library/microsoft.identitymodel.clients.activedirectory.aspx).

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

### <a name="connect-tooa-secure-cluster-without-prior-metadata-knowledge-using-azure-active-directory"></a>Ansluta tooa säker kluster utan föregående metadata kunskaper med Azure Active Directory

hello följande exempel använder icke-interaktiv token förvärv, men hello samma metod kan vara används toobuild en anpassad interaktiva token förvärv upplevelse. hello Azure Active Directory metadata som behövs för token läses från klusterkonfigurationen.

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

## <a name="connect-tooa-secure-cluster-using-service-fabric-explorer"></a>Ansluta tooa säker kluster med hjälp av Service Fabric Explorer
tooreach [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) ett kluster, peka i webbläsaren:

`http://<your-cluster-endpoint>:19080/Explorer`

hello fullständiga URL: en är också tillgängligt i hello klustret essentials rutan hello Azure-portalen.

### <a name="connect-tooa-secure-cluster-using-azure-active-directory"></a>Ansluta tooa säker kluster med hjälp av Azure Active Directory

tooconnect tooa kluster som skyddas med AAD, peka webbläsaren:

`https://<your-cluster-endpoint>:19080/Explorer`

Du är automatiskt att ange toolog in med AAD.

### <a name="connect-tooa-secure-cluster-using-a-client-certificate"></a>Ansluta tooa säker kluster med ett klientcertifikat

tooconnect tooa kluster som skyddas med certifikat, peka webbläsaren:

`https://<your-cluster-endpoint>:19080/Explorer`

Du är automatiskt att ange tooselect ett klientcertifikat.

<a id="connectsecureclustersetupclientcert"></a>
## <a name="set-up-a-client-certificate-on-hello-remote-computer"></a>Ställ in ett klientcertifikat på hello fjärrdator
Minst två certifikat ska användas för att skydda hello kluster, ett för hello klustret och server-certifikat och en annan för klientåtkomst.  Vi rekommenderar att du också använda ytterligare sekundära certifikat- och klientcertifikat åtkomst.  toosecure hello kommunikation mellan en klient och en nod med säkerhet och du behöver tooobtain och installera hello klientcertifikat. hello certifikat installeras i hello (min) datorarkivet hello lokal dator eller hello aktuell användare.  Du måste också hello tumavtryck hello servercertifikatet så att hello klienten kan autentisera hello klustret.

Kör följande PowerShell-cmdlet tooset in hello klientcertifikatet på hello datorn från vilken du åtkomst till klustret hello hello.

```powershell
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\docDemo\certs\DocDemoClusterCert.pfx `
        -Password (ConvertTo-SecureString -String test -AsPlainText -Force)
```

Om det är ett självsignerat certifikat måste tooimport den tooyour datorn ”betrodda personer” lagra innan du kan använda det här certifikatet tooconnect tooa säker klustret.

```powershell
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\TrustedPeople `
-FilePath C:\docDemo\certs\DocDemoClusterCert.pfx `
-Password (ConvertTo-SecureString -String test -AsPlainText -Force)
```

## <a name="next-steps"></a>Nästa steg

* [Uppgraderingsprocessen för Service Fabric-kluster och förväntningar från dig](service-fabric-cluster-upgrade.md)
* [Hantera dina Service Fabric-program i Visual Studio](service-fabric-manage-application-in-visual-studio.md)
* [Tjänstens hälsa för Fabric modellen introduktion](service-fabric-health-introduction.md)
* [Säkerhet och RunAs](service-fabric-application-runas-security.md)
* [Kom igång med Service Fabric CLI](service-fabric-cli.md)
