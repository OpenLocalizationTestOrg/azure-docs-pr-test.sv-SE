---
title: aaaKey valvet .NET 2.x API viktig information | Microsoft Docs
description: ".NET-utvecklare ska använda den här API-toocode för Azure Key Vault"
services: key-vault
author: BrucePerlerMS
manager: mbaldwin
editor: bruceper
ms.assetid: 1cccf21b-5be9-4a49-8145-483b695124ba
ms.service: key-vault
ms.devlang: CSharp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/02/2017
ms.author: bruceper
ms.openlocfilehash: d95b84cf73c155f117f37e93893f27b02a75855c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-key-vault-net-20---release-notes-and-migration-guide"></a>Azure Key Vault .NET 2.0 - viktig information och Migreringsguide
hello följande information och vägledning är för utvecklare som arbetar med Azure Key Vault .NET / C#-biblioteket. I hello övergång från hello 1.0 version toohello 2.0 version, ett antal uppdateringar har gjorts så kräver migrering arbete i koden för toobenefit från hello funktionella förbättringarna och funktionen tillägg som **Key Vault certifikat** stöd.

## <a name="key-vault-certificates"></a>Key Vault-certifikat

Stöd för Key Vault-certifikat ger hantering av din x509 certifikat och hello följande funktioner:  

* Tillåter att ett certifikat ägare toocreate ett certifikat via en process för att skapa Nyckelvalvet eller hello import av ett befintligt certifikat. Detta inkluderar både självsignerat och certifikatutfärdare genereras certifikat.
* Ger en Key Vault certifikat ägaren tooimplement säker lagring och hantering av X509 certifikat utan interaktion med material för privat nyckel.  
* Tillåter ett certifikat ägare toocreate en princip som leder Key Vault toomanage hello livscykeln för ett certifikat.  
* Tillåter certifikat ägare tooprovide kontaktinformation för meddelande om livscykeln händelser för förfallodatum och förnyelse av certifikat.  
* Stöder automatisk förnyelse med valda utfärdare - Key Vault partner X509 certifikat providers / certifikatutfärdare.
  
  * Observera - ej tillsammans providers/myndigheter kan också, men stöder inte hello automatisk förnyelse av funktionen.

## <a name="net-support"></a>Stöd för .NET

* **.NET 4.0** stöds inte av hello 2.0-versionen av hello Azure Key Vault .NET / C#-biblioteket
* **.NET core** stöds av hello 2.0-versionen av hello Azure Key Vault .NET / C#-biblioteket

## <a name="namespaces"></a>namnområden

* Hej namnområdet för **modeller** ändras från **Microsoft.Azure.KeyVault** för**Microsoft.Azure.KeyVault.Models**.
* Hej **Microsoft.Azure.KeyVault.Internal** namnområde har släppts.
* hello Azure SDK beroenden namnområde ändras från **Hyak.Common** och **Hyak.Common.Internals** för**Microsoft.Rest** och  **Microsoft.Rest.Serialization**

## <a name="type-changes"></a>Ändringar av typen

* *Hemligt* ändras för*SecretBundle*
* *Ordlistan* ändras för*IDictionary*
* *Lista<T>, string []* ändras för*IList<T>*
* *NextList* ändras för *NextPageLink*

## <a name="return-types"></a>Returtyper

* **KeyList** och **SecretList** returnerar *IPage<T>*  i stället för *ListKeysResponseMessage*
* hello genereras **BackupKeyAsync** returnerar *BackupKeyResult* som innehåller *värdet* (säkerhetskopiera blob). Före hello var metoden omsluten och returnerar bara hello-värde.

## <a name="exceptions"></a>Undantag

* *KeyVaultClientException* ändras för*KeyVaultErrorException*
* fel i tjänsten hello ändras från *undantag. Fel* för*undantag. Body.Error.Message*.
* Ytterligare information bort från hello felmeddelande för **[JsonExtensionData]**.

## <a name="constructors"></a>Konstruktorer

* I stället för att acceptera ett *HttpClient* som ett konstruktorargument hello konstruktorn godkänner enbart *HttpClientHandler* eller *DelegatingHandler []*.

## <a name="downloaded-packages"></a>Hämtade paket

När en klient bearbetar ett beroende på har Key Vault hello följande hämtats

### <a name="previous-package-list"></a>Tidigare paketlistan

* paketet id="Hyak.Common” version = ”1.0.2” targetFramework = ”net45”
* paketet id="Microsoft.Azure.Common” version = ”2.0.4” targetFramework = ”net45”
* paketet id="Microsoft.Azure.Common.Dependencies” version = ”1.0.0” targetFramework = ”net45”
* paketet id="Microsoft.Azure.KeyVault” version = ”1.0.0” targetFramework = ”net45”
* paketet id="Microsoft.Bcl” version = ”1.1.9” targetFramework = ”net45”
* paketet id="Microsoft.Bcl.Async” version = ”1.0.168” targetFramework = ”net45”
* paketet id="Microsoft.Bcl.Build” version = ”1.0.14” targetFramework = ”net45”
* paketet id="Microsoft.Net.Http” version = ”2.2.22” targetFramework = ”net45”

### <a name="current-package-list"></a>Aktuella paketlistan

* paketet id="Microsoft.Azure.KeyVault” version = ”2.0.0-preview” targetFramework = ”net45”
* paketet id="Microsoft.Rest.ClientRuntime” version = ”2.2.0” targetFramework = ”net45”
* paketet id="Microsoft.Rest.ClientRuntime.Azure” version = ”3.2.0” targetFramework = ”net45”

## <a name="class-changes"></a>Klassen ändringar

* **UnixEpoch** klass har tagits bort
* **Base64UrlConverter** klass har bytt namn för**Base64UrlJsonConverter**

## <a name="other-changes"></a>Andra ändringar

* Stöd för hello konfigurationen av KV åtgärden återförsöksprincip på tillfälliga fel har lagts toothis hello API-version.

## <a name="microsoftazuremanagementkeyvault-nuget"></a>Microsoft.Azure.Management.KeyVault NuGet

* För hello-åtgärder som returnerade en *valvet*, hello returtyp har en klass som innehåller en egenskap för valvet. hello returtyp är nu *valvet*.
* *PermissionsToKeys* och *PermissionsToSecrets* är nu *Permissions.Keys* och *Permissions.Secrets*
* Vissa av hello returnera typer ändringarna toohello-kontroll-plan samt.

## <a name="microsoftazurekeyvaultextensions-nuget"></a>Microsoft.Azure.KeyVault.Extensions NuGet

* hello-paketet har delats upp för**Microsoft.Azure.KeyVault.Extensions** och **Microsoft.Azure.KeyVault.Cryptography** för hello kryptografiska åtgärder.

