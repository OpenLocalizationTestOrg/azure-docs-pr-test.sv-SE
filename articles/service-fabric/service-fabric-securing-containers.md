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
# <a name="container-security"></a>Behållaren säkerhet

Service Fabric är en mekanism för tjänster i en behållare tooaccess ett certifikat som är installerad på hello noder i ett Windows- eller Linux-kluster (version 5.7 eller högre). Service Fabric stöder dessutom också gMSA (grupphanterade tjänstkonton) för Windows-behållare. 

## <a name="certificate-management-for-containers"></a>Certifikathantering för behållare

Du kan skydda dina behållartjänster genom att ange ett certifikat. hello certifikatet måste installeras på hello klusternoder hello. hello information om certifikat finns i hello programmanifestet under hello `ContainerHostPolicies` tagg som hello följande fragment visas:

```xml
  <ContainerHostPolicies CodePackageRef="NodeContainerService.Code">
    <CertificateRef Name="MyCert1" X509StoreName="My" X509FindValue="[Thumbprint1]"/>
    <CertificateRef Name="MyCert2" X509FindValue="[Thumbprint2]"/>
 ```

När du startar programmet hello hello runtime läser hello certifikat och genererar en PFX-filen och lösenordet för varje certifikat. Det här PFX-filen och lösenordet är tillgängliga i hello behållaren med hjälp av följande miljövariabler hello: 

* **Certificate_ [CodePackageName] _ [CertName] _PFX**
* **Certificate_ [CodePackageName] _ [CertName] _Password**

hello behållartjänsten eller process ansvarar för att importera hello PFX-filen till hello behållare. tooimport hello certifikat som du kan använda `setupentrypoint.sh` skript eller anpassad kod i hello behållaren processen körs. Följande exempelkod i C# för att importera hello PFX-filen:

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
Den här PFX-certifikat kan användas för att autentisera hello program eller tjänst eller säker commmunication med andra tjänster.


## <a name="set-up-gmsa-for-windows-containers"></a>Ställ in gMSA för Windows-behållare

tooset in gMSA (grupp hanterade tjänstkonton), en fil för specifikation av autentiseringsuppgifter (`credspec`) är placerad på alla noder i klustret hello. hello-filen kan kopieras på alla noder som använder ett tillägg för virtuell dator.  Hej `credspec` -filen måste innehålla hello gMSA-kontoinformation. Mer information om hello `credspec` fil, se [tjänstkonton](https://github.com/MicrosoftDocs/Virtualization-Documentation/tree/live/windows-server-container-tools/ServiceAccounts). hello credential-specifikationen och hello `Hostname` taggen har angetts i hello programmanifestet. Hej `Hostname` taggen måste matcha hello gMSA-kontonamn med hello behållare körs under.  Hej `Hostname` tillåter hello behållaren tooauthenticate själva tooother tjänster i hello domän med Kerberos-autentisering.  Ett exempel för att ange hello `Hostname` och hello `credspec` i hello programmanifestet visas i följande fragment hello:

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="process" Hostname="gMSAAccountName">
    <SecurityOption Value="credentialspec=file://WebApplication1.json"/>
  </ContainerHostPolicies>
</Policies>
```
## <a name="next-steps"></a>Nästa steg

* [Distribuera en Windows-behållaren tooService Fabric på Windows Server 2016](service-fabric-get-started-containers.md)
* [Distribuera en Docker-behållare tooService Fabric på Linux](service-fabric-get-started-containers-linux.md)
