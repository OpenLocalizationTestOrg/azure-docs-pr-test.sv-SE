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
# <a name="secure-a-standalone-cluster-on-windows-using-x509-certificates"></a><span data-ttu-id="8fcb4-103">Skydda ett fristående kluster på Windows med X.509-certifikat</span><span class="sxs-lookup"><span data-stu-id="8fcb4-103">Secure a standalone cluster on Windows using X.509 certificates</span></span>
<span data-ttu-id="8fcb4-104">Den här artikeln beskriver hur toosecure hello kommunikation mellan hello olika noder för fristående Windows-kluster och om hur tooauthenticate anslutande klienter toothis kluster med X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-104">This article describes how toosecure hello communication between hello various nodes of your standalone Windows cluster, as well as how tooauthenticate clients connecting toothis cluster, using X.509 certificates.</span></span> <span data-ttu-id="8fcb4-105">Detta säkerställer att endast auktoriserade användare kommer åt hello klustret hello distribuerade program och utföra administrativa uppgifter.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-105">This ensures that only authorized users can access hello cluster, hello deployed applications and perform management tasks.</span></span>  <span data-ttu-id="8fcb4-106">Certifikatsäkerhet ska aktiveras på hello klustret när hello klustret har skapats.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-106">Certificate security should be enabled on hello cluster when hello cluster is created.</span></span>  

<span data-ttu-id="8fcb4-107">Mer information om kluster nod till nod säkerhet, klient-till-nod säkerhet och rollbaserad åtkomstkontroll finns [kluster säkerhetsscenarier](service-fabric-cluster-security.md).</span><span class="sxs-lookup"><span data-stu-id="8fcb4-107">For more information on cluster security such as node-to-node security, client-to-node security, and role-based access control, see [Cluster security scenarios](service-fabric-cluster-security.md).</span></span>

## <a name="which-certificates-will-you-need"></a><span data-ttu-id="8fcb4-108">Vilka certifikat behöver du?</span><span class="sxs-lookup"><span data-stu-id="8fcb4-108">Which certificates will you need?</span></span>
<span data-ttu-id="8fcb4-109">toostart med [paketet hello fristående klustret](service-fabric-cluster-creation-for-windows-server.md#downloadpackage) tooone hello noder i klustret.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-109">toostart with, [download hello standalone cluster package](service-fabric-cluster-creation-for-windows-server.md#downloadpackage) tooone of hello nodes in your cluster.</span></span> <span data-ttu-id="8fcb4-110">I hello hämtade paketet, hittar du en **ClusterConfig.X509.MultiMachine.json** fil.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-110">In hello downloaded package, you will find a **ClusterConfig.X509.MultiMachine.json** file.</span></span> <span data-ttu-id="8fcb4-111">Öppna hello-filen och granska hello avsnittet **säkerhet** under hello **egenskaper** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="8fcb4-111">Open hello file and review hello section for **security** under hello **properties** section:</span></span>

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

<span data-ttu-id="8fcb4-112">Det här avsnittet beskriver hello-certifikat som du behöver för att skydda din fristående Windows-kluster.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-112">This section describes hello certificates that you need for securing your standalone Windows cluster.</span></span> <span data-ttu-id="8fcb4-113">Om du anger ett certifikat för klustret, ange hello-värde **ClusterCredentialType** too_**X509**_. Om du anger servercertifikat för utanför anslutningar måste ange hello **ServerCredentialType** för_**X509**_. Även om det är inte obligatoriskt, rekommenderar vi toohave båda certifikaten för ett kluster med ordentligt. Om du värdena för*X509* måste du också ange hello motsvarande certifikat eller Service Fabric genereras ett undantagsfel. I vissa fall kanske du bara vill toospecify hello _ClientCertificateThumbprints_ eller _ReverseProxyCertificate_. I dessa scenarier, behöver du inte ange _ClusterCredentialType_ eller _ServerCredentialType_ too_X509_.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-113">If you are specifying a cluster certificate, set hello value of **ClusterCredentialType** too_**X509**_. If you are specifying server certificate for outside connections, set hello **ServerCredentialType** too_**X509**_. Although not mandatory, we recommend toohave both these certificates for a properly secured cluster. If you set these values too*X509* then you must also specify hello corresponding certificates or Service Fabric will throw an exception. In some scenarios, you may only want toospecify hello _ClientCertificateThumbprints_ or _ReverseProxyCertificate_. In those scenarios, you need not set _ClusterCredentialType_ or _ServerCredentialType_ too_X509_.</span></span>


> [!NOTE]
> <span data-ttu-id="8fcb4-114">En [tumavtrycket](https://en.wikipedia.org/wiki/Public_key_fingerprint) hello primära identiteten för ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-114">A [thumbprint](https://en.wikipedia.org/wiki/Public_key_fingerprint) is hello primary identity of a certificate.</span></span> <span data-ttu-id="8fcb4-115">Läs [hur tooretrieve tumavtrycket för ett certifikat](https://msdn.microsoft.com/library/ms734695.aspx) toofind ut hello tumavtryck hello-certifikat som du skapar.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-115">Read [How tooretrieve thumbprint of a certificate](https://msdn.microsoft.com/library/ms734695.aspx) toofind out hello thumbprint of hello certificates that you create.</span></span>
> 
> 

<span data-ttu-id="8fcb4-116">hello visar följande tabell hello certifikat som du behöver på din konfiguration:</span><span class="sxs-lookup"><span data-stu-id="8fcb4-116">hello following table lists hello certificates that you will need on your cluster setup:</span></span>

| <span data-ttu-id="8fcb4-117">**CertificateInformation inställning**</span><span class="sxs-lookup"><span data-stu-id="8fcb4-117">**CertificateInformation Setting**</span></span> | <span data-ttu-id="8fcb4-118">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="8fcb4-118">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="8fcb4-119">ClusterCertificate</span><span class="sxs-lookup"><span data-stu-id="8fcb4-119">ClusterCertificate</span></span> |<span data-ttu-id="8fcb4-120">Vi rekommenderar för testmiljö.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-120">Recommended for test environment.</span></span> <span data-ttu-id="8fcb4-121">Det här certifikatet är obligatoriska toosecure hello kommunikation mellan hello noder i ett kluster.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-121">This certificate is required toosecure hello communication between hello nodes on a cluster.</span></span> <span data-ttu-id="8fcb4-122">Du kan använda två olika certifikat, en primär och sekundär för uppgradering.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-122">You can use two different certificates, a primary and a secondary for upgrade.</span></span> <span data-ttu-id="8fcb4-123">Ange hello tumavtrycket för certifikatet för primär hello i hello **tumavtrycket** avsnittet och som sekundär i hello hello **ThumbprintSecondary** variabler.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-123">Set hello thumbprint of hello primary certificate in hello **Thumbprint** section and that of hello secondary in hello **ThumbprintSecondary** variables.</span></span> |
| <span data-ttu-id="8fcb4-124">ClusterCertificateCommonNames</span><span class="sxs-lookup"><span data-stu-id="8fcb4-124">ClusterCertificateCommonNames</span></span> |<span data-ttu-id="8fcb4-125">Vi rekommenderar för produktionsmiljön.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-125">Recommended for production environment.</span></span> <span data-ttu-id="8fcb4-126">Det här certifikatet är obligatoriska toosecure hello kommunikation mellan hello noder i ett kluster.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-126">This certificate is required toosecure hello communication between hello nodes on a cluster.</span></span> <span data-ttu-id="8fcb4-127">Du kan använda en eller två certifikat vanliga klusternamn.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-127">You can use one or two cluster certificate common names.</span></span> |
| <span data-ttu-id="8fcb4-128">ServerCertificate</span><span class="sxs-lookup"><span data-stu-id="8fcb4-128">ServerCertificate</span></span> |<span data-ttu-id="8fcb4-129">Vi rekommenderar för testmiljö.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-129">Recommended for test environment.</span></span> <span data-ttu-id="8fcb4-130">Det här certifikatet visas toohello klienten när den försöker tooconnect toothis klustret.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-130">This certificate is presented toohello client when it tries tooconnect toothis cluster.</span></span> <span data-ttu-id="8fcb4-131">För enkelhetens skull kan du välja toouse hello samma certifikat för *ClusterCertificate* och *ServerCertificate*.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-131">For convenience, you can choose toouse hello same certificate for *ClusterCertificate* and *ServerCertificate*.</span></span> <span data-ttu-id="8fcb4-132">Du kan använda två olika servercertifikat, en primär och sekundär för uppgradering.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-132">You can use two different server certificates, a primary and a secondary for upgrade.</span></span> <span data-ttu-id="8fcb4-133">Ange hello tumavtrycket för certifikatet för primär hello i hello **tumavtrycket** avsnittet och som sekundär i hello hello **ThumbprintSecondary** variabler.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-133">Set hello thumbprint of hello primary certificate in hello **Thumbprint** section and that of hello secondary in hello **ThumbprintSecondary** variables.</span></span> |
| <span data-ttu-id="8fcb4-134">ServerCertificateCommonNames</span><span class="sxs-lookup"><span data-stu-id="8fcb4-134">ServerCertificateCommonNames</span></span> |<span data-ttu-id="8fcb4-135">Vi rekommenderar för produktionsmiljön.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-135">Recommended for production environment.</span></span> <span data-ttu-id="8fcb4-136">Det här certifikatet visas toohello klienten när den försöker tooconnect toothis klustret.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-136">This certificate is presented toohello client when it tries tooconnect toothis cluster.</span></span> <span data-ttu-id="8fcb4-137">För enkelhetens skull kan du välja toouse hello samma certifikat för *ClusterCertificateCommonNames* och *ServerCertificateCommonNames*.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-137">For convenience, you can choose toouse hello same certificate for *ClusterCertificateCommonNames* and *ServerCertificateCommonNames*.</span></span> <span data-ttu-id="8fcb4-138">Du kan använda en eller två vanliga namn servercertifikat.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-138">You can use one or two server certificate common names.</span></span> |
| <span data-ttu-id="8fcb4-139">ClientCertificateThumbprints</span><span class="sxs-lookup"><span data-stu-id="8fcb4-139">ClientCertificateThumbprints</span></span> |<span data-ttu-id="8fcb4-140">Det här är en uppsättning certifikat som du vill tooinstall på hello autentiserade klienter.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-140">This is a set of certificates that you want tooinstall on hello authenticated clients.</span></span> <span data-ttu-id="8fcb4-141">Du kan ha ett antal olika klientcertifikat som är installerad på hello datorer som du vill tooallow åtkomst toohello klustret.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-141">You can have a number of different client certificates installed on hello machines that you want tooallow access toohello cluster.</span></span> <span data-ttu-id="8fcb4-142">Ange hello varje certifikatets tumavtryck i hello **CertificateThumbprint** variabeln.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-142">Set hello thumbprint of each certificate in hello **CertificateThumbprint** variable.</span></span> <span data-ttu-id="8fcb4-143">Om du ställer in hello **IsAdmin** för*SANT*, och sedan hello-klient med det här certifikatet installeras på den kan göra administratören hanteringsaktiviteter på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-143">If you set hello **IsAdmin** too*true*, then hello client with this certificate installed on it can do administrator management activities on hello cluster.</span></span> <span data-ttu-id="8fcb4-144">Om hello **IsAdmin** är *FALSKT*, hello-klient med det här certifikatet kan bara utföra hello-åtgärder som tillåts för behörighet normalt skrivskyddade.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-144">If hello **IsAdmin** is *false*, hello client with this certificate can only perform hello actions allowed for user access rights, typically read-only.</span></span> <span data-ttu-id="8fcb4-145">Mer information om roller [rollbaserad åtkomstkontroll (RBAC)](service-fabric-cluster-security.md#role-based-access-control-rbac)</span><span class="sxs-lookup"><span data-stu-id="8fcb4-145">For more information on roles read [Role based access control (RBAC)](service-fabric-cluster-security.md#role-based-access-control-rbac)</span></span> |
| <span data-ttu-id="8fcb4-146">ClientCertificateCommonNames</span><span class="sxs-lookup"><span data-stu-id="8fcb4-146">ClientCertificateCommonNames</span></span> |<span data-ttu-id="8fcb4-147">Ange hello nätverksnamnet för hello första klientcertifikatet för hello **CertificateCommonName**.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-147">Set hello common name of hello first client certificate for hello **CertificateCommonName**.</span></span> <span data-ttu-id="8fcb4-148">Hej **CertificateIssuerThumbprint** är hello tumavtrycket för hello utfärdaren av det här certifikatet.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-148">hello **CertificateIssuerThumbprint** is hello thumbprint for hello issuer of this certificate.</span></span> <span data-ttu-id="8fcb4-149">Läs [arbetar med certifikat](https://msdn.microsoft.com/library/ms731899.aspx) tooknow mer om vanliga namn och hello utfärdare.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-149">Read [Working with certificates](https://msdn.microsoft.com/library/ms731899.aspx) tooknow more about common names and hello issuer.</span></span> |
| <span data-ttu-id="8fcb4-150">ReverseProxyCertificate</span><span class="sxs-lookup"><span data-stu-id="8fcb4-150">ReverseProxyCertificate</span></span> |<span data-ttu-id="8fcb4-151">Vi rekommenderar för testmiljö.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-151">Recommended for test environment.</span></span> <span data-ttu-id="8fcb4-152">Detta är ett valfritt certifikat som kan anges om du vill toosecure din [omvänd Proxy](service-fabric-reverseproxy.md).</span><span class="sxs-lookup"><span data-stu-id="8fcb4-152">This is an optional certificate that can be specified if you want toosecure your [Reverse Proxy](service-fabric-reverseproxy.md).</span></span> <span data-ttu-id="8fcb4-153">Kontrollera att reverseProxyEndpointPort har angetts i nodetypes får om du använder det här certifikatet.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-153">Make sure reverseProxyEndpointPort is set in nodeTypes if you are using this certificate.</span></span> |
| <span data-ttu-id="8fcb4-154">ReverseProxyCertificateCommonNames</span><span class="sxs-lookup"><span data-stu-id="8fcb4-154">ReverseProxyCertificateCommonNames</span></span> |<span data-ttu-id="8fcb4-155">Vi rekommenderar för produktionsmiljön.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-155">Recommended for production environment.</span></span> <span data-ttu-id="8fcb4-156">Detta är ett valfritt certifikat som kan anges om du vill toosecure din [omvänd Proxy](service-fabric-reverseproxy.md).</span><span class="sxs-lookup"><span data-stu-id="8fcb4-156">This is an optional certificate that can be specified if you want toosecure your [Reverse Proxy](service-fabric-reverseproxy.md).</span></span> <span data-ttu-id="8fcb4-157">Kontrollera att reverseProxyEndpointPort har angetts i nodetypes får om du använder det här certifikatet.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-157">Make sure reverseProxyEndpointPort is set in nodeTypes if you are using this certificate.</span></span> |

<span data-ttu-id="8fcb4-158">Här är exempel klusterkonfigurationen där hello kluster, servrar och klientdatorer certifikat har angetts.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-158">Here is example cluster configuration where hello Cluster, Server, and Client certificates have been provided.</span></span> <span data-ttu-id="8fcb4-159">Observera att för kluster / server / reverseProxy certifikat, stämpel och eget namn är inte tillåtna toobe konfigurerats tillsammans för hello samma certifikattyp.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-159">Please note that for cluster/ server/ reverseProxy certificates, thumbprint and common name are not allowed toobe configured together for hello same cert type.</span></span>

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

## <a name="certificate-roll-over"></a><span data-ttu-id="8fcb4-160">Sammanslagning av certifikat över</span><span class="sxs-lookup"><span data-stu-id="8fcb4-160">Certificate roll over</span></span>
<span data-ttu-id="8fcb4-161">När du använder certifikatets unika namn i stället för tumavtrycket kräver inte konfiguration av klusteruppgradering certifikat sammanslagning via.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-161">When using certificate common name instead of thumbprint, certificate roll over doesn't require cluster configuration upgrade.</span></span>
<span data-ttu-id="8fcb4-162">Om certifikatet sammanslagning via omfattar utfärdaren rulla över, kom hello gamla utfärdaren cert hello certifikatarkiv på minst 2 timmar när du har installerat hello nya utfärdaren certifikat.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-162">If certificate roll over involves issuer roll over, please keep hello old issuer cert in hello cert store at least 2 hours after installing hello new issuer cert.</span></span>

## <a name="acquire-hello-x509-certificates"></a><span data-ttu-id="8fcb4-163">Hämta hello X.509-certifikat</span><span class="sxs-lookup"><span data-stu-id="8fcb4-163">Acquire hello X.509 certificates</span></span>
<span data-ttu-id="8fcb4-164">toosecure kommunikation inom klustret hello först behöver du tooobtain X.509-certifikat för klusternoderna.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-164">toosecure communication within hello cluster, you will first need tooobtain X.509 certificates for your cluster nodes.</span></span> <span data-ttu-id="8fcb4-165">Dessutom toolimit anslutning toothis tooauthorized datorer och användare bör du behöver tooobtain och installera certifikat för hello-klientdatorer.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-165">Additionally, toolimit connection toothis cluster tooauthorized machines/users, you will need tooobtain and install certificates for hello client machines.</span></span>

<span data-ttu-id="8fcb4-166">För kluster som kör produktionsarbetsbelastningar, bör du använda en [certifikatutfärdare (CA)](https://en.wikipedia.org/wiki/Certificate_authority) signerade X.509-certifikat toosecure hello klustret.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-166">For clusters that are running production workloads, you should use a [Certificate Authority (CA)](https://en.wikipedia.org/wiki/Certificate_authority) signed X.509 certificate toosecure hello cluster.</span></span> <span data-ttu-id="8fcb4-167">Mer information om hur du skaffar dessa certifikat, gå för[så här: skaffa ett certifikat](http://msdn.microsoft.com/library/aa702761.aspx).</span><span class="sxs-lookup"><span data-stu-id="8fcb4-167">For details on obtaining these certificates, go too[How to: Obtain a Certificate](http://msdn.microsoft.com/library/aa702761.aspx).</span></span>

<span data-ttu-id="8fcb4-168">Du kan välja ett självsignerat certifikat toouse för kluster som du använder för testning.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-168">For clusters that you use for test purposes, you can choose toouse a self-signed certificate.</span></span>

## <a name="optional-create-a-self-signed-certificate"></a><span data-ttu-id="8fcb4-169">Valfritt: Skapa ett självsignerat certifikat</span><span class="sxs-lookup"><span data-stu-id="8fcb4-169">Optional: Create a self-signed certificate</span></span>
<span data-ttu-id="8fcb4-170">Enkelriktade toocreate ett självsignerat certifikat som kan skyddas korrekt är toouse hello *CertSetup.ps1* skriptet i hello Service Fabric SDK-mappen i hello directory *C:\Program Files\Microsoft SDKs\Service Fabric\ ClusterSetup\Secure*.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-170">One way toocreate a self-signed cert that can be secured correctly is toouse hello *CertSetup.ps1* script in hello Service Fabric SDK folder in hello directory *C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\Secure*.</span></span> <span data-ttu-id="8fcb4-171">Redigera den här filen toochange hello standardnamnet hello-certifikat (leta efter hello värde *CN = ServiceFabricDevClusterCert*).</span><span class="sxs-lookup"><span data-stu-id="8fcb4-171">Edit this file toochange hello default name of hello certificate (look for hello value *CN=ServiceFabricDevClusterCert*).</span></span> <span data-ttu-id="8fcb4-172">Kör skriptet som `.\CertSetup.ps1 -Install`.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-172">Run this script as `.\CertSetup.ps1 -Install`.</span></span>

<span data-ttu-id="8fcb4-173">Nu exportera hello certifikatets tooa PFX-fil med ett skyddat lösenord.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-173">Now export hello certificate tooa PFX file with a protected password.</span></span> <span data-ttu-id="8fcb4-174">Först hämta hello hello certifikatets tumavtryck.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-174">First get hello thumbprint of hello certificate.</span></span> <span data-ttu-id="8fcb4-175">Från hello *starta* menyn kör hello *hantera datorcertifikat*.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-175">From hello *Start* menu, run hello *Manage computer certificates*.</span></span> <span data-ttu-id="8fcb4-176">Navigera toohello **lokala personliga** mappen och hitta hello certifikat du just skapade.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-176">Navigate toohello **Local Computer\Personal** folder and find hello certificate you just created.</span></span> <span data-ttu-id="8fcb4-177">Dubbelklicka på hello certifikat tooopen det, väljer hello *information* och rulla ned toohello *tumavtrycket* fältet.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-177">Double-click hello certificate tooopen it, select hello *Details* tab and scroll down toohello *Thumbprint* field.</span></span> <span data-ttu-id="8fcb4-178">Kopiera hello tumavtrycksvärde till hello PowerShell-kommandot nedan, när du tar bort hello blanksteg.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-178">Copy hello thumbprint value into hello PowerShell command below, after removing hello spaces.</span></span>  <span data-ttu-id="8fcb4-179">Ändra hello `String` värdet tooa lämplig säkert lösenord tooprotect den och kör hello följande i PowerShell:</span><span class="sxs-lookup"><span data-stu-id="8fcb4-179">Change hello `String` value tooa suitable secure password tooprotect it and run hello following in PowerShell:</span></span>

```powershell   
$pswd = ConvertTo-SecureString -String "1234" -Force –AsPlainText
Get-ChildItem -Path cert:\localMachine\my\<Thumbprint> | Export-PfxCertificate -FilePath C:\mypfx.pfx -Password $pswd
```

<span data-ttu-id="8fcb4-180">toosee hello information om ett certifikat installerat på hello datorn du kan köra hello följande PowerShell-kommando:</span><span class="sxs-lookup"><span data-stu-id="8fcb4-180">toosee hello details of a certificate installed on hello machine you can run hello following PowerShell command:</span></span>

```powershell
$cert = Get-Item Cert:\LocalMachine\My\<Thumbprint>
Write-Host $cert.ToString($true)
```

<span data-ttu-id="8fcb4-181">Alternativt, om du har en Azure-prenumeration följer hello avsnittet [Lägg till certifikat tooKey valvet](service-fabric-cluster-creation-via-arm.md#add-certificate-to-key-vault).</span><span class="sxs-lookup"><span data-stu-id="8fcb4-181">Alternatively, if you have an Azure subscription, follow hello section [Add certificates tooKey Vault](service-fabric-cluster-creation-via-arm.md#add-certificate-to-key-vault).</span></span>

## <a name="install-hello-certificates"></a><span data-ttu-id="8fcb4-182">Installera hello certifikat</span><span class="sxs-lookup"><span data-stu-id="8fcb4-182">Install hello certificates</span></span>
<span data-ttu-id="8fcb4-183">När du har certifikat kan installera du dem på hello klusternoder.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-183">Once you have certificate(s), you can install them on hello cluster nodes.</span></span> <span data-ttu-id="8fcb4-184">Noderna behöver toohave hello senaste Windows PowerShell 3.x installerad.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-184">Your nodes need toohave hello latest Windows PowerShell 3.x installed on them.</span></span> <span data-ttu-id="8fcb4-185">Du behöver toorepeat de här stegen på varje nod för både kluster- och servercertifikat och alla sekundära certifikat.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-185">You will need toorepeat these steps on each node, for both Cluster and Server certificates and any secondary certificates.</span></span>

1. <span data-ttu-id="8fcb4-186">Kopiera hello .pfx fil(er) toohello nod.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-186">Copy hello .pfx file(s) toohello node.</span></span>
2. <span data-ttu-id="8fcb4-187">Öppna ett PowerShell-fönster som administratör och ange hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-187">Open a PowerShell window as an administrator and enter hello following commands.</span></span> <span data-ttu-id="8fcb4-188">Ersätt hello *$pswd* med hello lösenord som du använde toocreate det här certifikatet.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-188">Replace hello *$pswd* with hello password that you used toocreate this certificate.</span></span> <span data-ttu-id="8fcb4-189">Ersätt hello *$PfxFilePath* med den fullständiga sökvägen för hello hello .pfx kopierade toothis noden.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-189">Replace hello *$PfxFilePath* with hello full path of hello .pfx copied toothis node.</span></span>
   
    ```powershell
    $pswd = "1234"
    $PfxFilePath ="C:\mypfx.pfx"
    Import-PfxCertificate -Exportable -CertStoreLocation Cert:\LocalMachine\My -FilePath $PfxFilePath -Password (ConvertTo-SecureString -String $pswd -AsPlainText -Force)
    ```
3. <span data-ttu-id="8fcb4-190">Ange hello åtkomstkontroll på det här certifikatet så att hello Service Fabric-processen, som körs under hello Network Service-kontot kan använda genom att köra hello följande skript.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-190">Now set hello access control on this certificate so that hello Service Fabric process, which runs under hello Network Service account, can use it by running hello following script.</span></span> <span data-ttu-id="8fcb4-191">Ange hello tumavtryck hello certifikat och ”NÄTVERKSTJÄNST” för hello-tjänstkontot.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-191">Provide hello thumbprint of hello certificate and "NETWORK SERVICE" for hello service account.</span></span> <span data-ttu-id="8fcb4-192">Du kan kontrollera att hello ACL: er på hello certifikat är korrekta genom att öppna hello certifikat i *starta* > *hantera datorcertifikat* och titta på *alla aktiviteter*  >  *Hantera privata nycklar*.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-192">You can check that hello ACLs on hello certificate are correct by opening hello certificate in *Start* > *Manage computer certificates* and looking at *All Tasks* > *Manage Private Keys*.</span></span>
   
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
4. <span data-ttu-id="8fcb4-193">Upprepa hello stegen ovan för varje servercertifikat.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-193">Repeat hello steps above for each server certificate.</span></span> <span data-ttu-id="8fcb4-194">Du kan också använda dessa steg tooinstall hello klientcertifikat på hello datorer som du vill tooallow åtkomst toohello klustret.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-194">You can also use these steps tooinstall hello client certificates on hello machines that you want tooallow access toohello cluster.</span></span>

## <a name="create-hello-secure-cluster"></a><span data-ttu-id="8fcb4-195">Skapa säker hello-kluster</span><span class="sxs-lookup"><span data-stu-id="8fcb4-195">Create hello secure cluster</span></span>
<span data-ttu-id="8fcb4-196">När du har konfigurerat hello **säkerhet** avsnitt i hello **ClusterConfig.X509.MultiMachine.json** fil, du kan fortsätta för[skapa klustret](service-fabric-cluster-creation-for-windows-server.md#createcluster) avsnittet tooconfigure Hej noder och skapa hello fristående kluster.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-196">After configuring hello **security** section of hello **ClusterConfig.X509.MultiMachine.json** file, you can proceed too[Create your cluster](service-fabric-cluster-creation-for-windows-server.md#createcluster) section tooconfigure hello nodes and create hello standalone cluster.</span></span> <span data-ttu-id="8fcb4-197">Kom ihåg toouse hello **ClusterConfig.X509.MultiMachine.json** medan du skapar hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-197">Remember toouse hello **ClusterConfig.X509.MultiMachine.json** file while creating hello cluster.</span></span> <span data-ttu-id="8fcb4-198">Kommandot kan till exempel se ut hello följande:</span><span class="sxs-lookup"><span data-stu-id="8fcb4-198">For example, your command might look like hello following:</span></span>

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.X509.MultiMachine.json
```

<span data-ttu-id="8fcb4-199">När du har hello säker fristående Windows-kluster som kör och installationsprogrammet Hej autentiserade klienter tooconnect tooit, följ hello avsnittet [Anslut tooa säker kluster med hjälp av PowerShell](service-fabric-connect-to-secure-cluster.md#connectsecurecluster) tooconnect tooit.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-199">Once you have hello secure standalone Windows cluster successfully running, and have setup hello authenticated clients tooconnect tooit, follow hello section [Connect tooa secure cluster using PowerShell](service-fabric-connect-to-secure-cluster.md#connectsecurecluster) tooconnect tooit.</span></span> <span data-ttu-id="8fcb4-200">Exempel:</span><span class="sxs-lookup"><span data-stu-id="8fcb4-200">For example:</span></span>

```powershell
$ConnectArgs = @{  ConnectionEndpoint = '10.7.0.5:19000';  X509Credential = $True;  StoreLocation = 'LocalMachine';  StoreName = "MY";  ServerCertThumbprint = "057b9544a6f2733e0c8d3a60013a58948213f551";  FindType = 'FindByThumbprint';  FindValue = "057b9544a6f2733e0c8d3a60013a58948213f551"   }
Connect-ServiceFabricCluster $ConnectArgs
```

<span data-ttu-id="8fcb4-201">Du kan sedan köra toowork andra PowerShell-kommandon med det här klustret.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-201">You can then run other PowerShell commands toowork with this cluster.</span></span> <span data-ttu-id="8fcb4-202">Till exempel [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode.md?view=azureservicefabricps) tooshow en lista över noderna i klustret säker.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-202">For example, [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode.md?view=azureservicefabricps) tooshow a list of nodes on this secure cluster.</span></span>


<span data-ttu-id="8fcb4-203">tooremove hello klustret ansluta toohello nod på hello klustret där du hämtade hello Service Fabric-paket, öppna Kommandotolken och navigera toohello paketmappen.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-203">tooremove hello cluster, connect toohello node on hello cluster where you downloaded hello Service Fabric package, open a command line and navigate toohello package folder.</span></span> <span data-ttu-id="8fcb4-204">Kör nu hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="8fcb4-204">Now run hello following command:</span></span>

```powershell
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.X509.MultiMachine.json
```

> [!NOTE]
> <span data-ttu-id="8fcb4-205">Felaktig konfiguration kan förhindra hello klustret kommande under distributionen.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-205">Incorrect certificate configuration may prevent hello cluster from coming up during deployment.</span></span> <span data-ttu-id="8fcb4-206">tooself-diagnostisera säkerhetsproblem, tittar du i event viewer gruppen *program- och tjänstloggar* > *Microsoft Service Fabric*.</span><span class="sxs-lookup"><span data-stu-id="8fcb4-206">tooself-diagnose security issues, please look in event viewer group *Applications and Services Logs* > *Microsoft-Service Fabric*.</span></span>
> 
> 

