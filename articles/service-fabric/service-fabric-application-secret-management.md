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
# <a name="managing-secrets-in-service-fabric-applications"></a>Hantera hemligheter i Service Fabric-program
Den här guiden vägleder dig genom hello stegen för att hantera hemligheter i ett Service Fabric-program. Hemligheter kan vara känslig information, till exempel storage-anslutningssträngar, lösenord eller andra värden som inte ska hanteras i oformaterad text.

Den här guiden använder Azure Key Vault toomanage nycklar och hemligheter. Dock *med* hemligheter i ett program är moln plattformsoberoende tooallow program toobe distribuerade tooa klustret finnas var som helst. 

## <a name="overview"></a>Översikt
hello rekommenderade sättet toomanage konfigurationsinställningar är via [tjänsten konfigurationspaket][config-package]. Konfigurationspaket namnges och uppdateras via hanterade samlade uppgraderingar med hälsovalideringsutdrag och automatisk återställning. Detta är prioriterade tooglobal konfiguration eftersom det minskar hello risken för ett avbrott i tjänsten för global. Krypterade hemligheter är inget undantag. Service Fabric har inbyggda funktioner för att kryptera och dekryptera värden i en paketet Settings.xml konfigurationsfil med hjälp av för certifikatkryptering.

hello illustrerar följande diagram hello grundläggande flöde för hemliga management i ett Service Fabric-program:

![Översikt över hemliga management][overview]

Det finns fyra steg i det här flödet:

1. Skaffa ett certifikat för chiffrering av data.
2. Installera hello certifikat i klustret.
3. Kryptera hemliga värden när du distribuerar ett program med hello certifikat och mata in dem i en tjänst Settings.xml konfigurationsfilen.
4. Läs krypterade värden utanför Settings.xml genom att dekryptera med hello samma chiffrering av certifikat. 

[Azure Key Vault] [ key-vault-get-started] används här som en säker lagringsplats för certifikat och som ett sätt tooget certifikat som är installerade på Service Fabric-kluster i Azure. Om du inte distribuerar tooAzure, behöver du inte toouse Key Vault toomanage hemligheter i Service Fabric-program.

## <a name="data-encipherment-certificate"></a>Data chiffrering av certifikat
Data chiffrering certifikat används enbart för kryptering och dekryptering av konfiguration av värdena i en tjänst Settings.xml och är inte användas för autentisering eller signering av chiffertext. hello certifikat måste uppfylla följande krav hello:

* hello certifikatet måste innehålla en privat nyckel.
* hello certifikat måste skapas för nyckelutbyte, exportera tooa Personal Information Exchange (.pfx)-fil.
* hello nyckelanvändning för certifikatet måste innehålla Data chiffrering (10) och får inte innehålla serverautentisering och klientautentisering. 
  
  Till exempel när du skapar ett självsignerat certifikat med hjälp av PowerShell hello `KeyUsage` flagga måste anges för`DataEncipherment`:
  
  ```powershell
  New-SelfSignedCertificate -Type DocumentEncryptionCert -KeyUsage DataEncipherment -Subject mydataenciphermentcert -Provider 'Microsoft Enhanced Cryptographic Provider v1.0'
  ```

## <a name="install-hello-certificate-in-your-cluster"></a>Installera hello certifikat i klustret
Det här certifikatet måste installeras på varje nod i klustret hello. Den kommer att användas vid körning toodecrypt värden som lagras i Settings.xml för en tjänst. Se [hur toocreate ett kluster med Azure Resource Manager] [ service-fabric-cluster-creation-via-arm] för instruktioner. 

## <a name="encrypt-application-secrets"></a>Kryptera hemligheter i programmet
hello Service Fabric SDK har inbyggda hemliga funktioner för kryptering och dekryptering. Hemliga värden kan krypteras på inbyggda tid och sedan dekryptera och läsa programmässigt i Tjänstkod. 

hello följande PowerShell-kommandot är används tooencrypt en hemlighet. Det här kommandot endast krypterar hello värde. Det gör **inte** logga hello chiffertext. Du måste använda hello samma chiffrering av certifikat som har installerats i din klustret tooproduce ciphertext för hemliga värden:

```powershell
Invoke-ServiceFabricEncryptText -CertStore -CertThumbprint "<thumbprint>" -Text "mysecret" -StoreLocation CurrentUser -StoreName My
```

hello resulterande Base64-sträng innehåller både hello hemliga ciphertext samt information om hello-certifikat som har använt tooencrypt den.  hello Base64-kodad sträng kan infogas i en parameter i konfigurationsfilen för din tjänst Settings.xml med hello `IsEncrypted` -attributet inställt för`true`:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MySettings">
    <Parameter Name="MySecret" IsEncrypted="true" Value="I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM=" />
  </Section>
</Settings>
```

### <a name="inject-application-secrets-into-application-instances"></a>Mata in programmet hemligheter i programinstanser
Vi rekommenderar att distributionen toodifferent miljöer är som automatisk som möjligt. Detta kan åstadkommas genom att utföra hemliga kryptering i en kompileringsmiljö och hello krypterade hemligheter som parametrar när du skapar programinstanser.

#### <a name="use-overridable-parameters-in-settingsxml"></a>Använd åsidosättningsbara parametrar i Settings.xml
konfigurationsfilen för hello Settings.xml kan åsidosättningsbara parametrar som kan tillhandahållas vid skapandet för programmet. Använd hello `MustOverride` attribut i stället för att tillhandahålla ett värde för en parameter:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MySettings">
    <Parameter Name="MySecret" IsEncrypted="true" Value="" MustOverride="true" />
  </Section>
</Settings>
```

toooverride värden i Settings.xml, deklarera override-parametern för hello i ApplicationManifest.xml:

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

Nu hello värdet kan anges som en *programmet parametern* när du skapar en instans av programmet hello. Skapa en instans av programmet kan skriptas med hjälp av PowerShell eller skrivna i C# för enkel integrering i en build-process.

Med PowerShell hello-parametern är angivna toohello `New-ServiceFabricApplication` kommandot som en [hash-tabell](https://technet.microsoft.com/library/ee692803.aspx):

```powershell
PS C:\Users\vturecek> New-ServiceFabricApplication -ApplicationName fabric:/MyApp -ApplicationTypeName MyAppType -ApplicationTypeVersion 1.0.0 -ApplicationParameter @{"MySecret" = "I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM="}
```

Med hjälp av C# programmet parametrar har angetts i en `ApplicationDescription` som en `NameValueCollection`:

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

## <a name="decrypt-secrets-from-service-code"></a>Dekryptera hemligheter från Tjänstkod
Tjänster i Service Fabric körs under NÄTVERKSTJÄNST som standard i Windows och inte har åtkomst toocertificates installerat på hello nod utan några extra installationen.

När du använder ett certifikat för chiffrering av data, måste toomake att NETWORK SERVICE eller oavsett användarens konto hello-tjänsten körs under har åtkomst till toohello certifikatets privata nyckel. Service Fabric hanterar åtkomst beviljades för din tjänst automatiskt om du konfigurerar den toodo så. Den här konfigurationen kan göras i ApplicationManifest.xml genom att definiera användare och säkerhetsprinciper för certifikat. I följande exempel hello, får hello NETWORK SERVICE-kontot läsbehörighet tooa certifikat som definieras av dess tumavtryck:

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
> När du kopierar ett tumavtryck för certifikat från hello certifikat lagra snapin-modulen i Windows, ett osynligt tecken är placerad hello början av hello tumavtrycket sträng. Osynliga tecknet kan orsaka att ett fel vid försök toolocate ett certifikat med tumavtrycket, så var att toodelete detta extra tecken.
> 
> 

### <a name="use-application-secrets-in-service-code"></a>Använd programmet hemligheter i service-kod
hello API för åtkomst till konfigurationsvärden från Settings.xml i ett konfigurationspaket kan enkelt dekryptering av värden som har hello `IsEncrypted` -attributet inställt för`true`. Eftersom hello krypterade texten innehåller information om hello-certifikat som används för kryptering, behöver inte toomanually hitta hello certifikat. hello certifikat måste bara toobe installerad på hello-noden som hello-tjänsten körs på. Anropar hello `DecryptValue()` metoden tooretrieve hello ursprungliga hemligt värde:

```csharp
ConfigurationPackage configPackage = this.Context.CodePackageActivationContext.GetConfigurationPackageObject("Config");
SecureString mySecretValue = configPackage.Settings.Sections["MySettings"].Parameters["MySecret"].DecryptValue()
```

## <a name="next-steps"></a>Nästa steg
Lär dig mer om [program som körs med olika behörigheter](service-fabric-application-runas-security.md)

<!-- Links -->
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[config-package]: service-fabric-application-model.md
[service-fabric-cluster-creation-via-arm]: service-fabric-cluster-creation-via-arm.md

<!-- Images -->
[overview]:./media/service-fabric-application-secret-management/overview.png
