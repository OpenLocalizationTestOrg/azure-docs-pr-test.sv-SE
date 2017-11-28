---
title: aaaManaging hemligheter i Service Fabric-program | Microsoft Docs
description: "Den här artikeln beskriver hur toosecure hemlighet värden i ett Service Fabric-program."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 94a67e45-7094-4fbd-9c88-51f4fc3c523a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: b8cafcb681d95aaa1b8e9a1afaac78ba5b7f58b0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="managing-secrets-in-service-fabric-applications"></a><span data-ttu-id="a622d-103">Hantera hemligheter i Service Fabric-program</span><span class="sxs-lookup"><span data-stu-id="a622d-103">Managing secrets in Service Fabric applications</span></span>
<span data-ttu-id="a622d-104">Den här guiden vägleder dig genom hello stegen för att hantera hemligheter i ett Service Fabric-program.</span><span class="sxs-lookup"><span data-stu-id="a622d-104">This guide walks you through hello steps of managing secrets in a Service Fabric application.</span></span> <span data-ttu-id="a622d-105">Hemligheter kan vara känslig information, till exempel storage-anslutningssträngar, lösenord eller andra värden som inte ska hanteras i oformaterad text.</span><span class="sxs-lookup"><span data-stu-id="a622d-105">Secrets can be any sensitive information, such as storage connection strings, passwords, or other values that should not be handled in plain text.</span></span>

<span data-ttu-id="a622d-106">Den här guiden använder Azure Key Vault toomanage nycklar och hemligheter.</span><span class="sxs-lookup"><span data-stu-id="a622d-106">This guide uses Azure Key Vault toomanage keys and secrets.</span></span> <span data-ttu-id="a622d-107">Dock *med* hemligheter i ett program är moln plattformsoberoende tooallow program toobe distribuerade tooa klustret finnas var som helst.</span><span class="sxs-lookup"><span data-stu-id="a622d-107">However, *using* secrets in an application is cloud platform-agnostic tooallow applications toobe deployed tooa cluster hosted anywhere.</span></span> 

## <a name="overview"></a><span data-ttu-id="a622d-108">Översikt</span><span class="sxs-lookup"><span data-stu-id="a622d-108">Overview</span></span>
<span data-ttu-id="a622d-109">hello rekommenderade sättet toomanage konfigurationsinställningar är via [tjänsten konfigurationspaket][config-package].</span><span class="sxs-lookup"><span data-stu-id="a622d-109">hello recommended way toomanage service configuration settings is through [service configuration packages][config-package].</span></span> <span data-ttu-id="a622d-110">Konfigurationspaket namnges och uppdateras via hanterade samlade uppgraderingar med hälsovalideringsutdrag och automatisk återställning.</span><span class="sxs-lookup"><span data-stu-id="a622d-110">Configuration packages are versioned and updatable through managed rolling upgrades with health-validation and auto rollback.</span></span> <span data-ttu-id="a622d-111">Detta är prioriterade tooglobal konfiguration eftersom det minskar hello risken för ett avbrott i tjänsten för global.</span><span class="sxs-lookup"><span data-stu-id="a622d-111">This is preferred tooglobal configuration as it reduces hello chances of a global service outage.</span></span> <span data-ttu-id="a622d-112">Krypterade hemligheter är inget undantag.</span><span class="sxs-lookup"><span data-stu-id="a622d-112">Encrypted secrets are no exception.</span></span> <span data-ttu-id="a622d-113">Service Fabric har inbyggda funktioner för att kryptera och dekryptera värden i en paketet Settings.xml konfigurationsfil med hjälp av för certifikatkryptering.</span><span class="sxs-lookup"><span data-stu-id="a622d-113">Service Fabric has built-in features for encrypting and decrypting values in a configuration package Settings.xml file using certificate encryption.</span></span>

<span data-ttu-id="a622d-114">hello illustrerar följande diagram hello grundläggande flöde för hemliga management i ett Service Fabric-program:</span><span class="sxs-lookup"><span data-stu-id="a622d-114">hello following diagram illustrates hello basic flow for secret management in a Service Fabric application:</span></span>

![Översikt över hemliga management][overview]

<span data-ttu-id="a622d-116">Det finns fyra steg i det här flödet:</span><span class="sxs-lookup"><span data-stu-id="a622d-116">There are four main steps in this flow:</span></span>

1. <span data-ttu-id="a622d-117">Skaffa ett certifikat för chiffrering av data.</span><span class="sxs-lookup"><span data-stu-id="a622d-117">Obtain a data encipherment certificate.</span></span>
2. <span data-ttu-id="a622d-118">Installera hello certifikat i klustret.</span><span class="sxs-lookup"><span data-stu-id="a622d-118">Install hello certificate in your cluster.</span></span>
3. <span data-ttu-id="a622d-119">Kryptera hemliga värden när du distribuerar ett program med hello certifikat och mata in dem i en tjänst Settings.xml konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="a622d-119">Encrypt secret values when deploying an application with hello certificate and inject them into a service's Settings.xml configuration file.</span></span>
4. <span data-ttu-id="a622d-120">Läs krypterade värden utanför Settings.xml genom att dekryptera med hello samma chiffrering av certifikat.</span><span class="sxs-lookup"><span data-stu-id="a622d-120">Read encrypted values out of Settings.xml by decrypting with hello same encipherment certificate.</span></span> 

<span data-ttu-id="a622d-121">[Azure Key Vault] [ key-vault-get-started] används här som en säker lagringsplats för certifikat och som ett sätt tooget certifikat som är installerade på Service Fabric-kluster i Azure.</span><span class="sxs-lookup"><span data-stu-id="a622d-121">[Azure Key Vault][key-vault-get-started] is used here as a safe storage location for certificates and as a way tooget certificates installed on Service Fabric clusters in Azure.</span></span> <span data-ttu-id="a622d-122">Om du inte distribuerar tooAzure, behöver du inte toouse Key Vault toomanage hemligheter i Service Fabric-program.</span><span class="sxs-lookup"><span data-stu-id="a622d-122">If you are not deploying tooAzure, you do not need toouse Key Vault toomanage secrets in Service Fabric applications.</span></span>

## <a name="data-encipherment-certificate"></a><span data-ttu-id="a622d-123">Data chiffrering av certifikat</span><span class="sxs-lookup"><span data-stu-id="a622d-123">Data encipherment certificate</span></span>
<span data-ttu-id="a622d-124">Data chiffrering certifikat används enbart för kryptering och dekryptering av konfiguration av värdena i en tjänst Settings.xml och är inte användas för autentisering eller signering av chiffertext.</span><span class="sxs-lookup"><span data-stu-id="a622d-124">A data encipherment certificate is used strictly for encryption and decryption of configuration values in a service's Settings.xml and is not used for authentication or signing of cipher text.</span></span> <span data-ttu-id="a622d-125">hello certifikat måste uppfylla följande krav hello:</span><span class="sxs-lookup"><span data-stu-id="a622d-125">hello certificate must meet hello following requirements:</span></span>

* <span data-ttu-id="a622d-126">hello certifikatet måste innehålla en privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="a622d-126">hello certificate must contain a private key.</span></span>
* <span data-ttu-id="a622d-127">hello certifikat måste skapas för nyckelutbyte, exportera tooa Personal Information Exchange (.pfx)-fil.</span><span class="sxs-lookup"><span data-stu-id="a622d-127">hello certificate must be created for key exchange, exportable tooa Personal Information Exchange (.pfx) file.</span></span>
* <span data-ttu-id="a622d-128">hello nyckelanvändning för certifikatet måste innehålla Data chiffrering (10) och får inte innehålla serverautentisering och klientautentisering.</span><span class="sxs-lookup"><span data-stu-id="a622d-128">hello certificate key usage must include Data Encipherment (10), and should not include Server Authentication or Client Authentication.</span></span> 
  
  <span data-ttu-id="a622d-129">Till exempel när du skapar ett självsignerat certifikat med hjälp av PowerShell hello `KeyUsage` flagga måste anges för`DataEncipherment`:</span><span class="sxs-lookup"><span data-stu-id="a622d-129">For example, when creating a self-signed certificate using PowerShell, hello `KeyUsage` flag must be set too`DataEncipherment`:</span></span>
  
  ```powershell
  New-SelfSignedCertificate -Type DocumentEncryptionCert -KeyUsage DataEncipherment -Subject mydataenciphermentcert -Provider 'Microsoft Enhanced Cryptographic Provider v1.0'
  ```

## <a name="install-hello-certificate-in-your-cluster"></a><span data-ttu-id="a622d-130">Installera hello certifikat i klustret</span><span class="sxs-lookup"><span data-stu-id="a622d-130">Install hello certificate in your cluster</span></span>
<span data-ttu-id="a622d-131">Det här certifikatet måste installeras på varje nod i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="a622d-131">This certificate must be installed on each node in hello cluster.</span></span> <span data-ttu-id="a622d-132">Den kommer att användas vid körning toodecrypt värden som lagras i Settings.xml för en tjänst.</span><span class="sxs-lookup"><span data-stu-id="a622d-132">It will be used at runtime toodecrypt values stored in a service's Settings.xml.</span></span> <span data-ttu-id="a622d-133">Se [hur toocreate ett kluster med Azure Resource Manager] [ service-fabric-cluster-creation-via-arm] för instruktioner.</span><span class="sxs-lookup"><span data-stu-id="a622d-133">See [how toocreate a cluster using Azure Resource Manager][service-fabric-cluster-creation-via-arm] for setup instructions.</span></span> 

## <a name="encrypt-application-secrets"></a><span data-ttu-id="a622d-134">Kryptera hemligheter i programmet</span><span class="sxs-lookup"><span data-stu-id="a622d-134">Encrypt application secrets</span></span>
<span data-ttu-id="a622d-135">hello Service Fabric SDK har inbyggda hemliga funktioner för kryptering och dekryptering.</span><span class="sxs-lookup"><span data-stu-id="a622d-135">hello Service Fabric SDK has built-in secret encryption and decryption functions.</span></span> <span data-ttu-id="a622d-136">Hemliga värden kan krypteras på inbyggda tid och sedan dekryptera och läsa programmässigt i Tjänstkod.</span><span class="sxs-lookup"><span data-stu-id="a622d-136">Secret values can be encrypted at built-time and then decrypted and read programmatically in service code.</span></span> 

<span data-ttu-id="a622d-137">hello följande PowerShell-kommandot är används tooencrypt en hemlighet.</span><span class="sxs-lookup"><span data-stu-id="a622d-137">hello following PowerShell command is used tooencrypt a secret.</span></span> <span data-ttu-id="a622d-138">Det här kommandot endast krypterar hello värde. Det gör **inte** logga hello chiffertext.</span><span class="sxs-lookup"><span data-stu-id="a622d-138">This command only encrypts hello value; it does **not** sign hello cipher text.</span></span> <span data-ttu-id="a622d-139">Du måste använda hello samma chiffrering av certifikat som har installerats i din klustret tooproduce ciphertext för hemliga värden:</span><span class="sxs-lookup"><span data-stu-id="a622d-139">You must use hello same encipherment certificate that is installed in your cluster tooproduce ciphertext for secret values:</span></span>

```powershell
Invoke-ServiceFabricEncryptText -CertStore -CertThumbprint "<thumbprint>" -Text "mysecret" -StoreLocation CurrentUser -StoreName My
```

<span data-ttu-id="a622d-140">hello resulterande Base64-sträng innehåller både hello hemliga ciphertext samt information om hello-certifikat som har använt tooencrypt den.</span><span class="sxs-lookup"><span data-stu-id="a622d-140">hello resulting base-64 string contains both hello secret ciphertext as well as information about hello certificate that was used tooencrypt it.</span></span>  <span data-ttu-id="a622d-141">hello Base64-kodad sträng kan infogas i en parameter i konfigurationsfilen för din tjänst Settings.xml med hello `IsEncrypted` -attributet inställt för`true`:</span><span class="sxs-lookup"><span data-stu-id="a622d-141">hello base-64 encoded string can be inserted into a parameter in your service's Settings.xml configuration file with hello `IsEncrypted` attribute set too`true`:</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MySettings">
    <Parameter Name="MySecret" IsEncrypted="true" Value="I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM=" />
  </Section>
</Settings>
```

### <a name="inject-application-secrets-into-application-instances"></a><span data-ttu-id="a622d-142">Mata in programmet hemligheter i programinstanser</span><span class="sxs-lookup"><span data-stu-id="a622d-142">Inject application secrets into application instances</span></span>
<span data-ttu-id="a622d-143">Vi rekommenderar att distributionen toodifferent miljöer är som automatisk som möjligt.</span><span class="sxs-lookup"><span data-stu-id="a622d-143">Ideally, deployment toodifferent environments should be as automated as possible.</span></span> <span data-ttu-id="a622d-144">Detta kan åstadkommas genom att utföra hemliga kryptering i en kompileringsmiljö och hello krypterade hemligheter som parametrar när du skapar programinstanser.</span><span class="sxs-lookup"><span data-stu-id="a622d-144">This can be accomplished by performing secret encryption in a build environment and providing hello encrypted secrets as parameters when creating application instances.</span></span>

#### <a name="use-overridable-parameters-in-settingsxml"></a><span data-ttu-id="a622d-145">Använd åsidosättningsbara parametrar i Settings.xml</span><span class="sxs-lookup"><span data-stu-id="a622d-145">Use overridable parameters in Settings.xml</span></span>
<span data-ttu-id="a622d-146">konfigurationsfilen för hello Settings.xml kan åsidosättningsbara parametrar som kan tillhandahållas vid skapandet för programmet.</span><span class="sxs-lookup"><span data-stu-id="a622d-146">hello Settings.xml configuration file allows overridable parameters that can be provided at application creation time.</span></span> <span data-ttu-id="a622d-147">Använd hello `MustOverride` attribut i stället för att tillhandahålla ett värde för en parameter:</span><span class="sxs-lookup"><span data-stu-id="a622d-147">Use hello `MustOverride` attribute instead of providing a value for a parameter:</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MySettings">
    <Parameter Name="MySecret" IsEncrypted="true" Value="" MustOverride="true" />
  </Section>
</Settings>
```

<span data-ttu-id="a622d-148">toooverride värden i Settings.xml, deklarera override-parametern för hello i ApplicationManifest.xml:</span><span class="sxs-lookup"><span data-stu-id="a622d-148">toooverride values in Settings.xml, declare an override parameter for hello service in ApplicationManifest.xml:</span></span>

```xml
<ApplicationManifest ... >
  <Parameters>
    <Parameter Name="MySecret" DefaultValue="" />
  </Parameters>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateful1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides>
      <ConfigOverride Name="Config">
        <Settings>
          <Section Name="MySettings">
            <Parameter Name="MySecret" Value="[MySecret]" IsEncrypted="true" />
          </Section>
        </Settings>
      </ConfigOverride>
    </ConfigOverrides>
  </ServiceManifestImport>
 ```

<span data-ttu-id="a622d-149">Nu hello värdet kan anges som en *programmet parametern* när du skapar en instans av programmet hello.</span><span class="sxs-lookup"><span data-stu-id="a622d-149">Now hello value can be specified as an *application parameter* when creating an instance of hello application.</span></span> <span data-ttu-id="a622d-150">Skapa en instans av programmet kan skriptas med hjälp av PowerShell eller skrivna i C# för enkel integrering i en build-process.</span><span class="sxs-lookup"><span data-stu-id="a622d-150">Creating an application instance can be scripted using PowerShell, or written in C#, for easy integration in a build process.</span></span>

<span data-ttu-id="a622d-151">Med PowerShell hello-parametern är angivna toohello `New-ServiceFabricApplication` kommandot som en [hash-tabell](https://technet.microsoft.com/library/ee692803.aspx):</span><span class="sxs-lookup"><span data-stu-id="a622d-151">Using PowerShell, hello parameter is supplied toohello `New-ServiceFabricApplication` command as a [hash table](https://technet.microsoft.com/library/ee692803.aspx):</span></span>

```powershell
PS C:\Users\vturecek> New-ServiceFabricApplication -ApplicationName fabric:/MyApp -ApplicationTypeName MyAppType -ApplicationTypeVersion 1.0.0 -ApplicationParameter @{"MySecret" = "I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM="}
```

<span data-ttu-id="a622d-152">Med hjälp av C# programmet parametrar har angetts i en `ApplicationDescription` som en `NameValueCollection`:</span><span class="sxs-lookup"><span data-stu-id="a622d-152">Using C#, application parameters are specified in an `ApplicationDescription` as a `NameValueCollection`:</span></span>

```csharp
FabricClient fabricClient = new FabricClient();

NameValueCollection applicationParameters = new NameValueCollection();
applicationParameters["MySecret"] = "I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM=";

ApplicationDescription applicationDescription = new ApplicationDescription(
    applicationName: new Uri("fabric:/MyApp"),
    applicationTypeName: "MyAppType",
    applicationTypeVersion: "1.0.0",
    applicationParameters: applicationParameters)
);

await fabricClient.ApplicationManager.CreateApplicationAsync(applicationDescription);
```

## <a name="decrypt-secrets-from-service-code"></a><span data-ttu-id="a622d-153">Dekryptera hemligheter från Tjänstkod</span><span class="sxs-lookup"><span data-stu-id="a622d-153">Decrypt secrets from service code</span></span>
<span data-ttu-id="a622d-154">Tjänster i Service Fabric körs under NÄTVERKSTJÄNST som standard i Windows och inte har åtkomst toocertificates installerat på hello nod utan några extra installationen.</span><span class="sxs-lookup"><span data-stu-id="a622d-154">Services in Service Fabric run under NETWORK SERVICE by default on Windows and don't have access toocertificates installed on hello node without some extra setup.</span></span>

<span data-ttu-id="a622d-155">När du använder ett certifikat för chiffrering av data, måste toomake att NETWORK SERVICE eller oavsett användarens konto hello-tjänsten körs under har åtkomst till toohello certifikatets privata nyckel.</span><span class="sxs-lookup"><span data-stu-id="a622d-155">When using a data encipherment certificate, you need toomake sure NETWORK SERVICE or whatever user account hello service is running under has access toohello certificate's private key.</span></span> <span data-ttu-id="a622d-156">Service Fabric hanterar åtkomst beviljades för din tjänst automatiskt om du konfigurerar den toodo så.</span><span class="sxs-lookup"><span data-stu-id="a622d-156">Service Fabric will handle granting access for your service automatically if you configure it toodo so.</span></span> <span data-ttu-id="a622d-157">Den här konfigurationen kan göras i ApplicationManifest.xml genom att definiera användare och säkerhetsprinciper för certifikat.</span><span class="sxs-lookup"><span data-stu-id="a622d-157">This configuration can be done in ApplicationManifest.xml by defining users and security policies for certificates.</span></span> <span data-ttu-id="a622d-158">I följande exempel hello, får hello NETWORK SERVICE-kontot läsbehörighet tooa certifikat som definieras av dess tumavtryck:</span><span class="sxs-lookup"><span data-stu-id="a622d-158">In hello following example, hello NETWORK SERVICE account is given read access tooa certificate defined by its thumbprint:</span></span>

```xml
<ApplicationManifest … >
    <Principals>
        <Users>
            <User Name="Service1" AccountType="NetworkService" />
        </Users>
    </Principals>
  <Policies>
    <SecurityAccessPolicies>
      <SecurityAccessPolicy GrantRights=”Read” PrincipalRef="Service1" ResourceRef="MyCert" ResourceType="Certificate"/>
    </SecurityAccessPolicies>
  </Policies>
  <Certificates>
    <SecretsCertificate Name="MyCert" X509FindType="FindByThumbprint" X509FindValue="[YourCertThumbrint]"/>
  </Certificates>
</ApplicationManifest>
```

> [!NOTE]
> <span data-ttu-id="a622d-159">När du kopierar ett tumavtryck för certifikat från hello certifikat lagra snapin-modulen i Windows, ett osynligt tecken är placerad hello början av hello tumavtrycket sträng.</span><span class="sxs-lookup"><span data-stu-id="a622d-159">When copying a certificate thumbprint from hello certificate store snap-in on Windows, an invisible character is placed at hello beginning of hello thumbprint string.</span></span> <span data-ttu-id="a622d-160">Osynliga tecknet kan orsaka att ett fel vid försök toolocate ett certifikat med tumavtrycket, så var att toodelete detta extra tecken.</span><span class="sxs-lookup"><span data-stu-id="a622d-160">This invisible character can cause an error when trying toolocate a certificate by thumbprint, so be sure toodelete this extra character.</span></span>
> 
> 

### <a name="use-application-secrets-in-service-code"></a><span data-ttu-id="a622d-161">Använd programmet hemligheter i service-kod</span><span class="sxs-lookup"><span data-stu-id="a622d-161">Use application secrets in service code</span></span>
<span data-ttu-id="a622d-162">hello API för åtkomst till konfigurationsvärden från Settings.xml i ett konfigurationspaket kan enkelt dekryptering av värden som har hello `IsEncrypted` -attributet inställt för`true`.</span><span class="sxs-lookup"><span data-stu-id="a622d-162">hello API for accessing configuration values from Settings.xml in a configuration package allows for easy decrypting of values that have hello `IsEncrypted` attribute set too`true`.</span></span> <span data-ttu-id="a622d-163">Eftersom hello krypterade texten innehåller information om hello-certifikat som används för kryptering, behöver inte toomanually hitta hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="a622d-163">Since hello encrypted text contains information about hello certificate used for encryption, you do not need toomanually find hello certificate.</span></span> <span data-ttu-id="a622d-164">hello certifikat måste bara toobe installerad på hello-noden som hello-tjänsten körs på.</span><span class="sxs-lookup"><span data-stu-id="a622d-164">hello certificate just needs toobe installed on hello node that hello service is running on.</span></span> <span data-ttu-id="a622d-165">Anropar hello `DecryptValue()` metoden tooretrieve hello ursprungliga hemligt värde:</span><span class="sxs-lookup"><span data-stu-id="a622d-165">Simply call hello `DecryptValue()` method tooretrieve hello original secret value:</span></span>

```csharp
ConfigurationPackage configPackage = this.Context.CodePackageActivationContext.GetConfigurationPackageObject("Config");
SecureString mySecretValue = configPackage.Settings.Sections["MySettings"].Parameters["MySecret"].DecryptValue()
```

## <a name="next-steps"></a><span data-ttu-id="a622d-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a622d-166">Next Steps</span></span>
<span data-ttu-id="a622d-167">Lär dig mer om [program som körs med olika behörigheter](service-fabric-application-runas-security.md)</span><span class="sxs-lookup"><span data-stu-id="a622d-167">Learn more about [running applications with different security permissions](service-fabric-application-runas-security.md)</span></span>

<!-- Links -->
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[config-package]: service-fabric-application-model.md
[service-fabric-cluster-creation-via-arm]: service-fabric-cluster-creation-via-arm.md

<!-- Images -->
[overview]:./media/service-fabric-application-secret-management/overview.png
