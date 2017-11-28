---
title: "aaaAzure Service Fabric-behållaren säkerhet | Microsoft Docs"
description: "Lär dig nu toosecure behållartjänster."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 88faf4e8f949c2f7743756b6272ca672d9710630
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="container-security"></a><span data-ttu-id="cd1a3-103">Behållaren säkerhet</span><span class="sxs-lookup"><span data-stu-id="cd1a3-103">Container security</span></span>

<span data-ttu-id="cd1a3-104">Service Fabric är en mekanism för tjänster i en behållare tooaccess ett certifikat som är installerad på hello noder i ett Windows- eller Linux-kluster (version 5.7 eller högre).</span><span class="sxs-lookup"><span data-stu-id="cd1a3-104">Service Fabric provides a mechanism for services inside a container tooaccess a certificate that is installed on hello nodes in a Windows or Linux cluster (version 5.7 or higher).</span></span> <span data-ttu-id="cd1a3-105">Service Fabric stöder dessutom också gMSA (grupphanterade tjänstkonton) för Windows-behållare.</span><span class="sxs-lookup"><span data-stu-id="cd1a3-105">In addition, Service Fabric also supports gMSA (group Managed Service Accounts) for Windows containers.</span></span> 

## <a name="certificate-management-for-containers"></a><span data-ttu-id="cd1a3-106">Certifikathantering för behållare</span><span class="sxs-lookup"><span data-stu-id="cd1a3-106">Certificate management for containers</span></span>

<span data-ttu-id="cd1a3-107">Du kan skydda dina behållartjänster genom att ange ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="cd1a3-107">You can secure your container services by specifying a certificate.</span></span> <span data-ttu-id="cd1a3-108">hello certifikatet måste installeras på hello klusternoder hello.</span><span class="sxs-lookup"><span data-stu-id="cd1a3-108">hello certificate must be installed on hello nodes of hello cluster.</span></span> <span data-ttu-id="cd1a3-109">hello information om certifikat finns i hello programmanifestet under hello `ContainerHostPolicies` tagg som hello följande fragment visas:</span><span class="sxs-lookup"><span data-stu-id="cd1a3-109">hello certificate information is provided in hello application manifest under hello `ContainerHostPolicies` tag as hello following snippet shows:</span></span>

```xml
  <ContainerHostPolicies CodePackageRef="NodeContainerService.Code">
    <CertificateRef Name="MyCert1" X509StoreName="My" X509FindValue="[Thumbprint1]"/>
    <CertificateRef Name="MyCert2" X509FindValue="[Thumbprint2]"/>
 ```

<span data-ttu-id="cd1a3-110">När du startar programmet hello hello runtime läser hello certifikat och genererar en PFX-filen och lösenordet för varje certifikat.</span><span class="sxs-lookup"><span data-stu-id="cd1a3-110">When starting hello application, hello runtime reads hello certificates and generates a PFX file and password for each certificate.</span></span> <span data-ttu-id="cd1a3-111">Det här PFX-filen och lösenordet är tillgängliga i hello behållaren med hjälp av följande miljövariabler hello:</span><span class="sxs-lookup"><span data-stu-id="cd1a3-111">This PFX file and password are accessible inside hello container using hello following environment variables:</span></span> 

* <span data-ttu-id="cd1a3-112">**Certificate_ [CodePackageName] _ [CertName] _PFX**</span><span class="sxs-lookup"><span data-stu-id="cd1a3-112">**Certificate_[CodePackageName]_[CertName]_PFX**</span></span>
* <span data-ttu-id="cd1a3-113">**Certificate_ [CodePackageName] _ [CertName] _Password**</span><span class="sxs-lookup"><span data-stu-id="cd1a3-113">**Certificate_[CodePackageName]_[CertName]_Password**</span></span>

<span data-ttu-id="cd1a3-114">hello behållartjänsten eller process ansvarar för att importera hello PFX-filen till hello behållare.</span><span class="sxs-lookup"><span data-stu-id="cd1a3-114">hello container service or process is responsible for importing hello PFX file into hello container.</span></span> <span data-ttu-id="cd1a3-115">tooimport hello certifikat som du kan använda `setupentrypoint.sh` skript eller anpassad kod i hello behållaren processen körs.</span><span class="sxs-lookup"><span data-stu-id="cd1a3-115">tooimport hello certificate, you can use `setupentrypoint.sh` scripts or executed custom code within hello container process.</span></span> <span data-ttu-id="cd1a3-116">Följande exempelkod i C# för att importera hello PFX-filen:</span><span class="sxs-lookup"><span data-stu-id="cd1a3-116">Sample code in C# for importing hello PFX file follows:</span></span>

```c#
    string certificateFilePath = Environment.GetEnvironmentVariable("Certificate_NodeContainerService.Code_MyCert1_PFX");
    string passwordFilePath = Environment.GetEnvironmentVariable("Certificate_NodeContainerService.Code_MyCert1_Password");
    X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
    string password = File.ReadAllLines(passwordFilePath, Encoding.Default)[0];
    password = password.Replace("\0", string.Empty);
    X509Certificate2 cert = new X509Certificate2(certificateFilePath, password, X509KeyStorageFlags.MachineKeySet | X509KeyStorageFlags.PersistKeySet);
    store.Open(OpenFlags.ReadWrite);
    store.Add(cert);
    store.Close();
```
<span data-ttu-id="cd1a3-117">Den här PFX-certifikat kan användas för att autentisera hello program eller tjänst eller säker commmunication med andra tjänster.</span><span class="sxs-lookup"><span data-stu-id="cd1a3-117">This PFX certificate can be used for authenticate hello application or service or secure commmunication with other services.</span></span>


## <a name="set-up-gmsa-for-windows-containers"></a><span data-ttu-id="cd1a3-118">Ställ in gMSA för Windows-behållare</span><span class="sxs-lookup"><span data-stu-id="cd1a3-118">Set up gMSA for Windows containers</span></span>

<span data-ttu-id="cd1a3-119">tooset in gMSA (grupp hanterade tjänstkonton), en fil för specifikation av autentiseringsuppgifter (`credspec`) är placerad på alla noder i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="cd1a3-119">tooset up gMSA (group Managed Service Accounts), a credential specification file (`credspec`) is placed on all nodes in hello cluster.</span></span> <span data-ttu-id="cd1a3-120">hello-filen kan kopieras på alla noder som använder ett tillägg för virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="cd1a3-120">hello file can be copied on all nodes using a VM extension.</span></span>  <span data-ttu-id="cd1a3-121">Hej `credspec` -filen måste innehålla hello gMSA-kontoinformation.</span><span class="sxs-lookup"><span data-stu-id="cd1a3-121">hello `credspec` file must contain hello gMSA account information.</span></span> <span data-ttu-id="cd1a3-122">Mer information om hello `credspec` fil, se [tjänstkonton](https://github.com/MicrosoftDocs/Virtualization-Documentation/tree/live/windows-server-container-tools/ServiceAccounts).</span><span class="sxs-lookup"><span data-stu-id="cd1a3-122">For more information on hello `credspec` file, see [Service Accounts](https://github.com/MicrosoftDocs/Virtualization-Documentation/tree/live/windows-server-container-tools/ServiceAccounts).</span></span> <span data-ttu-id="cd1a3-123">hello credential-specifikationen och hello `Hostname` taggen har angetts i hello programmanifestet.</span><span class="sxs-lookup"><span data-stu-id="cd1a3-123">hello credential specification and hello `Hostname` tag are specified in hello application manifest.</span></span> <span data-ttu-id="cd1a3-124">Hej `Hostname` taggen måste matcha hello gMSA-kontonamn med hello behållare körs under.</span><span class="sxs-lookup"><span data-stu-id="cd1a3-124">hello `Hostname` tag must match hello gMSA account name that hello container runs under.</span></span>  <span data-ttu-id="cd1a3-125">Hej `Hostname` tillåter hello behållaren tooauthenticate själva tooother tjänster i hello domän med Kerberos-autentisering.</span><span class="sxs-lookup"><span data-stu-id="cd1a3-125">hello `Hostname` tag allows hello container tooauthenticate itself tooother services in hello domain using Kerberos authentication.</span></span>  <span data-ttu-id="cd1a3-126">Ett exempel för att ange hello `Hostname` och hello `credspec` i hello programmanifestet visas i följande fragment hello:</span><span class="sxs-lookup"><span data-stu-id="cd1a3-126">A sample for specifying hello `Hostname` and hello `credspec` in hello application manifest is shown in hello following snippet:</span></span>

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="process" Hostname="gMSAAccountName">
    <SecurityOption Value="credentialspec=file://WebApplication1.json"/>
  </ContainerHostPolicies>
</Policies>
```
## <a name="next-steps"></a><span data-ttu-id="cd1a3-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cd1a3-127">Next steps</span></span>

* [<span data-ttu-id="cd1a3-128">Distribuera en Windows-behållaren tooService Fabric på Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="cd1a3-128">Deploy a Windows container tooService Fabric on Windows Server 2016</span></span>](service-fabric-get-started-containers.md)
* [<span data-ttu-id="cd1a3-129">Distribuera en Docker-behållare tooService Fabric på Linux</span><span class="sxs-lookup"><span data-stu-id="cd1a3-129">Deploy a Docker container tooService Fabric on Linux</span></span>](service-fabric-get-started-containers-linux.md)
