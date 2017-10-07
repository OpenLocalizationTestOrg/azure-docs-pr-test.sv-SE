---
title: aaaSecure ett Azure Service Fabric-kluster i Windows med certifikat | Microsoft Docs
description: "Den här artikeln beskriver hur toosecure kommunikation inom hello fristående eller privat kluster samt mellan klienter och hello klustret."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: fe0ed74c-9af5-44e9-8d62-faf1849af68c
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: dekapur
ms.openlocfilehash: f0d411963615349a84edfc8125dec4ee5908f146
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="secure-a-standalone-cluster-on-windows-using-x509-certificates"></a>Skydda ett fristående kluster på Windows med X.509-certifikat
Den här artikeln beskriver hur toosecure hello kommunikation mellan hello olika noder för fristående Windows-kluster och om hur tooauthenticate anslutande klienter toothis kluster med X.509-certifikat. Detta säkerställer att endast auktoriserade användare kommer åt hello klustret hello distribuerade program och utföra administrativa uppgifter.  Certifikatsäkerhet ska aktiveras på hello klustret när hello klustret har skapats.  

Mer information om kluster nod till nod säkerhet, klient-till-nod säkerhet och rollbaserad åtkomstkontroll finns [kluster säkerhetsscenarier](service-fabric-cluster-security.md).

## <a name="which-certificates-will-you-need"></a>Vilka certifikat behöver du?
toostart med [paketet hello fristående klustret](service-fabric-cluster-creation-for-windows-server.md#downloadpackage) tooone hello noder i klustret. I hello hämtade paketet, hittar du en **ClusterConfig.X509.MultiMachine.json** fil. Öppna hello-filen och granska hello avsnittet **säkerhet** under hello **egenskaper** avsnitt:

```JSON
"security": {
    "metadata": "hello Credential type X509 indicates this is cluster is secured using X509 Certificates. hello thumbprint format is - d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
    "ClusterCredentialType": "X509",
    "ServerCredentialType": "X509",
    "CertificateInformation": {
        "ClusterCertificate": {
            "Thumbprint": "[Thumbprint]",
            "ThumbprintSecondary": "[Thumbprint]",
            "X509StoreName": "My"
        },        
        "ClusterCertificateCommonNames": {
            "CommonNames": [
            {
                "CertificateCommonName": "[CertificateCommonName]"
            }
            ],
            "X509StoreName": "My"
        },
        "ServerCertificate": {
            "Thumbprint": "[Thumbprint]",
            "ThumbprintSecondary": "[Thumbprint]",
            "X509StoreName": "My"
        },
        "ServerCertificateCommonNames": {
            "CommonNames": [
            {
                "CertificateCommonName": "[CertificateCommonName]"
            }
            ],
            "X509StoreName": "My"
        },
        "ClientCertificateThumbprints": [
            {
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": false
            },
            {
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": true
            }
        ],
        "ClientCertificateCommonNames": [
            {
                "CertificateCommonName": "[CertificateCommonName]",
                "CertificateIssuerThumbprint": "[Thumbprint]",
                "IsAdmin": true
            }
        ],
        "ReverseProxyCertificate": {
            "Thumbprint": "[Thumbprint]",
            "ThumbprintSecondary": "[Thumbprint]",
            "X509StoreName": "My"
        },
        "ReverseProxyCertificateCommonNames": {
            "CommonNames": [
                {
                "CertificateCommonName": "[CertificateCommonName]"
                }
            ],
            "X509StoreName": "My"
        }
    }
},
```

Det här avsnittet beskriver hello-certifikat som du behöver för att skydda din fristående Windows-kluster. Om du anger ett certifikat för klustret, ange hello-värde **ClusterCredentialType** too_**X509**_. Om du anger servercertifikat för utanför anslutningar måste ange hello **ServerCredentialType** för_**X509**_. Även om det är inte obligatoriskt, rekommenderar vi toohave båda certifikaten för ett kluster med ordentligt. Om du värdena för*X509* måste du också ange hello motsvarande certifikat eller Service Fabric genereras ett undantagsfel. I vissa fall kanske du bara vill toospecify hello _ClientCertificateThumbprints_ eller _ReverseProxyCertificate_. I dessa scenarier, behöver du inte ange _ClusterCredentialType_ eller _ServerCredentialType_ too_X509_.


> [!NOTE]
> En [tumavtrycket](https://en.wikipedia.org/wiki/Public_key_fingerprint) hello primära identiteten för ett certifikat. Läs [hur tooretrieve tumavtrycket för ett certifikat](https://msdn.microsoft.com/library/ms734695.aspx) toofind ut hello tumavtryck hello-certifikat som du skapar.
> 
> 

hello visar följande tabell hello certifikat som du behöver på din konfiguration:

| **CertificateInformation inställning** | **Beskrivning** |
| --- | --- |
| ClusterCertificate |Vi rekommenderar för testmiljö. Det här certifikatet är obligatoriska toosecure hello kommunikation mellan hello noder i ett kluster. Du kan använda två olika certifikat, en primär och sekundär för uppgradering. Ange hello tumavtrycket för certifikatet för primär hello i hello **tumavtrycket** avsnittet och som sekundär i hello hello **ThumbprintSecondary** variabler. |
| ClusterCertificateCommonNames |Vi rekommenderar för produktionsmiljön. Det här certifikatet är obligatoriska toosecure hello kommunikation mellan hello noder i ett kluster. Du kan använda en eller två certifikat vanliga klusternamn. |
| ServerCertificate |Vi rekommenderar för testmiljö. Det här certifikatet visas toohello klienten när den försöker tooconnect toothis klustret. För enkelhetens skull kan du välja toouse hello samma certifikat för *ClusterCertificate* och *ServerCertificate*. Du kan använda två olika servercertifikat, en primär och sekundär för uppgradering. Ange hello tumavtrycket för certifikatet för primär hello i hello **tumavtrycket** avsnittet och som sekundär i hello hello **ThumbprintSecondary** variabler. |
| ServerCertificateCommonNames |Vi rekommenderar för produktionsmiljön. Det här certifikatet visas toohello klienten när den försöker tooconnect toothis klustret. För enkelhetens skull kan du välja toouse hello samma certifikat för *ClusterCertificateCommonNames* och *ServerCertificateCommonNames*. Du kan använda en eller två vanliga namn servercertifikat. |
| ClientCertificateThumbprints |Det här är en uppsättning certifikat som du vill tooinstall på hello autentiserade klienter. Du kan ha ett antal olika klientcertifikat som är installerad på hello datorer som du vill tooallow åtkomst toohello klustret. Ange hello varje certifikatets tumavtryck i hello **CertificateThumbprint** variabeln. Om du ställer in hello **IsAdmin** för*SANT*, och sedan hello-klient med det här certifikatet installeras på den kan göra administratören hanteringsaktiviteter på hello klustret. Om hello **IsAdmin** är *FALSKT*, hello-klient med det här certifikatet kan bara utföra hello-åtgärder som tillåts för behörighet normalt skrivskyddade. Mer information om roller [rollbaserad åtkomstkontroll (RBAC)](service-fabric-cluster-security.md#role-based-access-control-rbac) |
| ClientCertificateCommonNames |Ange hello nätverksnamnet för hello första klientcertifikatet för hello **CertificateCommonName**. Hej **CertificateIssuerThumbprint** är hello tumavtrycket för hello utfärdaren av det här certifikatet. Läs [arbetar med certifikat](https://msdn.microsoft.com/library/ms731899.aspx) tooknow mer om vanliga namn och hello utfärdare. |
| ReverseProxyCertificate |Vi rekommenderar för testmiljö. Detta är ett valfritt certifikat som kan anges om du vill toosecure din [omvänd Proxy](service-fabric-reverseproxy.md). Kontrollera att reverseProxyEndpointPort har angetts i nodetypes får om du använder det här certifikatet. |
| ReverseProxyCertificateCommonNames |Vi rekommenderar för produktionsmiljön. Detta är ett valfritt certifikat som kan anges om du vill toosecure din [omvänd Proxy](service-fabric-reverseproxy.md). Kontrollera att reverseProxyEndpointPort har angetts i nodetypes får om du använder det här certifikatet. |

Här är exempel klusterkonfigurationen där hello kluster, servrar och klientdatorer certifikat har angetts. Observera att för kluster / server / reverseProxy certifikat, stämpel och eget namn är inte tillåtna toobe konfigurerats tillsammans för hello samma certifikattyp.

 ```JSON
 {
    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "2016-09-26",
    "nodes": [{
        "nodeName": "vm0",
        "metadata": "Replace hello localhost below with valid IP address or FQDN",
        "iPAddress": "10.7.0.5",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
        "nodeName": "vm1",
        "metadata": "Replace hello localhost with valid IP address or FQDN",
        "iPAddress": "10.7.0.4",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
        "iPAddress": "10.7.0.6",
        "metadata": "Replace hello localhost with valid IP address or FQDN",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],
    "properties": {
        "diagnosticsStore": {
        "metadata":  "Please replace hello diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
        }
        "security": {
            "metadata": "hello Credential type X509 indicates this is cluster is secured using X509 Certificates. hello thumbprint format is - d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
            "ClusterCredentialType": "X509",
            "ServerCredentialType": "X509",
            "CertificateInformation": {
                "ClusterCertificateCommonNames": {
                  "CommonNames": [
                    {
                      "CertificateCommonName": "myClusterCertCommonName"
                    }
                  ],
                  "X509StoreName": "My"
                },
                "ServerCertificateCommonNames": {
                  "CommonNames": [
                    {
                      "CertificateCommonName": "myServerCertCommonName"
                    }
                  ],
                  "X509StoreName": "My"
                },
                "ClientCertificateThumbprints": [{
                    "CertificateThumbprint": "c4 c18 8e aa a8 58 77 98 65 f8 61 4a 0d da 4c 13 c5 a1 37 6e",
                    "IsAdmin": false
                }, {
                    "CertificateThumbprint": "71 de 04 46 7c 9e d0 54 4d 02 10 98 bc d4 4c 71 e1 83 41 4e",
                    "IsAdmin": true
                }]
            }
        },
        "reliabilityLevel": "Bronze",
        "nodeTypes": [{
            "name": "NodeType0",
            "clientConnectionEndpointPort": "19000",
            "clusterConnectionEndpointPort": "19001",
            "leaseDriverEndpointPort": "19002",
            "serviceConnectionEndpointPort": "19003",
            "httpGatewayEndpointPort": "19080",
            "applicationPorts": {
                "startPort": "20001",
                "endPort": "20031"
            },
            "ephemeralPorts": {
                "startPort": "20032",
                "endPort": "20062"
            },
            "isPrimary": true
        }
         ],
        "fabricSettings": [{
            "name": "Setup",
            "parameters": [{
                "name": "FabricDataRoot",
                "value": "C:\\ProgramData\\SF"
            }, {
                "name": "FabricLogRoot",
                "value": "C:\\ProgramData\\SF\\Log"
            }]
        }]
    }
}
 ```

## <a name="certificate-roll-over"></a>Sammanslagning av certifikat över
När du använder certifikatets unika namn i stället för tumavtrycket kräver inte konfiguration av klusteruppgradering certifikat sammanslagning via.
Om certifikatet sammanslagning via omfattar utfärdaren rulla över, kom hello gamla utfärdaren cert hello certifikatarkiv på minst 2 timmar när du har installerat hello nya utfärdaren certifikat.

## <a name="acquire-hello-x509-certificates"></a>Hämta hello X.509-certifikat
toosecure kommunikation inom klustret hello först behöver du tooobtain X.509-certifikat för klusternoderna. Dessutom toolimit anslutning toothis tooauthorized datorer och användare bör du behöver tooobtain och installera certifikat för hello-klientdatorer.

För kluster som kör produktionsarbetsbelastningar, bör du använda en [certifikatutfärdare (CA)](https://en.wikipedia.org/wiki/Certificate_authority) signerade X.509-certifikat toosecure hello klustret. Mer information om hur du skaffar dessa certifikat, gå för[så här: skaffa ett certifikat](http://msdn.microsoft.com/library/aa702761.aspx).

Du kan välja ett självsignerat certifikat toouse för kluster som du använder för testning.

## <a name="optional-create-a-self-signed-certificate"></a>Valfritt: Skapa ett självsignerat certifikat
Enkelriktade toocreate ett självsignerat certifikat som kan skyddas korrekt är toouse hello *CertSetup.ps1* skriptet i hello Service Fabric SDK-mappen i hello directory *C:\Program Files\Microsoft SDKs\Service Fabric\ ClusterSetup\Secure*. Redigera den här filen toochange hello standardnamnet hello-certifikat (leta efter hello värde *CN = ServiceFabricDevClusterCert*). Kör skriptet som `.\CertSetup.ps1 -Install`.

Nu exportera hello certifikatets tooa PFX-fil med ett skyddat lösenord. Först hämta hello hello certifikatets tumavtryck. Från hello *starta* menyn kör hello *hantera datorcertifikat*. Navigera toohello **lokala personliga** mappen och hitta hello certifikat du just skapade. Dubbelklicka på hello certifikat tooopen det, väljer hello *information* och rulla ned toohello *tumavtrycket* fältet. Kopiera hello tumavtrycksvärde till hello PowerShell-kommandot nedan, när du tar bort hello blanksteg.  Ändra hello `String` värdet tooa lämplig säkert lösenord tooprotect den och kör hello följande i PowerShell:

```powershell   
$pswd = ConvertTo-SecureString -String "1234" -Force –AsPlainText
Get-ChildItem -Path cert:\localMachine\my\<Thumbprint> | Export-PfxCertificate -FilePath C:\mypfx.pfx -Password $pswd
```

toosee hello information om ett certifikat installerat på hello datorn du kan köra hello följande PowerShell-kommando:

```powershell
$cert = Get-Item Cert:\LocalMachine\My\<Thumbprint>
Write-Host $cert.ToString($true)
```

Alternativt, om du har en Azure-prenumeration följer hello avsnittet [Lägg till certifikat tooKey valvet](service-fabric-cluster-creation-via-arm.md#add-certificate-to-key-vault).

## <a name="install-hello-certificates"></a>Installera hello certifikat
När du har certifikat kan installera du dem på hello klusternoder. Noderna behöver toohave hello senaste Windows PowerShell 3.x installerad. Du behöver toorepeat de här stegen på varje nod för både kluster- och servercertifikat och alla sekundära certifikat.

1. Kopiera hello .pfx fil(er) toohello nod.
2. Öppna ett PowerShell-fönster som administratör och ange hello följande kommandon. Ersätt hello *$pswd* med hello lösenord som du använde toocreate det här certifikatet. Ersätt hello *$PfxFilePath* med den fullständiga sökvägen för hello hello .pfx kopierade toothis noden.
   
    ```powershell
    $pswd = "1234"
    $PfxFilePath ="C:\mypfx.pfx"
    Import-PfxCertificate -Exportable -CertStoreLocation Cert:\LocalMachine\My -FilePath $PfxFilePath -Password (ConvertTo-SecureString -String $pswd -AsPlainText -Force)
    ```
3. Ange hello åtkomstkontroll på det här certifikatet så att hello Service Fabric-processen, som körs under hello Network Service-kontot kan använda genom att köra hello följande skript. Ange hello tumavtryck hello certifikat och ”NÄTVERKSTJÄNST” för hello-tjänstkontot. Du kan kontrollera att hello ACL: er på hello certifikat är korrekta genom att öppna hello certifikat i *starta* > *hantera datorcertifikat* och titta på *alla aktiviteter*  >  *Hantera privata nycklar*.
   
    ```powershell
    param
    (
    [Parameter(Position=1, Mandatory=$true)]
    [ValidateNotNullOrEmpty()]
    [string]$pfxThumbPrint,
   
    [Parameter(Position=2, Mandatory=$true)]
    [ValidateNotNullOrEmpty()]
    [string]$serviceAccount
    )
   
    $cert = Get-ChildItem -Path cert:\LocalMachine\My | Where-Object -FilterScript { $PSItem.ThumbPrint -eq $pfxThumbPrint; }
   
    # Specify hello user, hello permissions and hello permission type
    $permission = "$($serviceAccount)","FullControl","Allow"
    $accessRule = New-Object -TypeName System.Security.AccessControl.FileSystemAccessRule -ArgumentList $permission
   
    # Location of hello machine related keys
    $keyPath = Join-Path -Path $env:ProgramData -ChildPath "\Microsoft\Crypto\RSA\MachineKeys"
    $keyName = $cert.PrivateKey.CspKeyContainerInfo.UniqueKeyContainerName
    $keyFullPath = Join-Path -Path $keyPath -ChildPath $keyName
   
    # Get hello current acl of hello private key
    $acl = (Get-Item $keyFullPath).GetAccessControl('Access')
   
    # Add hello new ace toohello acl of hello private key
    $acl.SetAccessRule($accessRule)
   
    # Write back hello new acl
    Set-Acl -Path $keyFullPath -AclObject $acl -ErrorAction Stop
   
    # Observe hello access rights currently assigned toothis certificate.
    get-acl $keyFullPath| fl
    ```
4. Upprepa hello stegen ovan för varje servercertifikat. Du kan också använda dessa steg tooinstall hello klientcertifikat på hello datorer som du vill tooallow åtkomst toohello klustret.

## <a name="create-hello-secure-cluster"></a>Skapa säker hello-kluster
När du har konfigurerat hello **säkerhet** avsnitt i hello **ClusterConfig.X509.MultiMachine.json** fil, du kan fortsätta för[skapa klustret](service-fabric-cluster-creation-for-windows-server.md#createcluster) avsnittet tooconfigure Hej noder och skapa hello fristående kluster. Kom ihåg toouse hello **ClusterConfig.X509.MultiMachine.json** medan du skapar hello-kluster. Kommandot kan till exempel se ut hello följande:

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.X509.MultiMachine.json
```

När du har hello säker fristående Windows-kluster som kör och installationsprogrammet Hej autentiserade klienter tooconnect tooit, följ hello avsnittet [Anslut tooa säker kluster med hjälp av PowerShell](service-fabric-connect-to-secure-cluster.md#connectsecurecluster) tooconnect tooit. Exempel:

```powershell
$ConnectArgs = @{  ConnectionEndpoint = '10.7.0.5:19000';  X509Credential = $True;  StoreLocation = 'LocalMachine';  StoreName = "MY";  ServerCertThumbprint = "057b9544a6f2733e0c8d3a60013a58948213f551";  FindType = 'FindByThumbprint';  FindValue = "057b9544a6f2733e0c8d3a60013a58948213f551"   }
Connect-ServiceFabricCluster $ConnectArgs
```

Du kan sedan köra toowork andra PowerShell-kommandon med det här klustret. Till exempel [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode.md?view=azureservicefabricps) tooshow en lista över noderna i klustret säker.


tooremove hello klustret ansluta toohello nod på hello klustret där du hämtade hello Service Fabric-paket, öppna Kommandotolken och navigera toohello paketmappen. Kör nu hello följande kommando:

```powershell
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.X509.MultiMachine.json
```

> [!NOTE]
> Felaktig konfiguration kan förhindra hello klustret kommande under distributionen. tooself-diagnostisera säkerhetsproblem, tittar du i event viewer gruppen *program- och tjänstloggar* > *Microsoft Service Fabric*.
> 
> 

