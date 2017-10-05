---
title: Nyckeln valvet .NET-API 2.x viktig information | Microsoft Docs
description: ".NET-utvecklare använder detta API till kod för Azure Key Vault"
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
ms.openlocfilehash: c5b5fd7f16faf17d16ecc82269fb1264adf4dd06
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-key-vault-net-20---release-notes-and-migration-guide"></a><span data-ttu-id="98c58-103">Azure Key Vault .NET 2.0 - viktig information och Migreringsguide</span><span class="sxs-lookup"><span data-stu-id="98c58-103">Azure Key Vault .NET 2.0 - Release Notes and Migration Guide</span></span>
<span data-ttu-id="98c58-104">Följande information och vägledning är för utvecklare som arbetar med Azure Key Vault .NET / C#-biblioteket.</span><span class="sxs-lookup"><span data-stu-id="98c58-104">The following notes and guidance are for developers working with Azure Key Vault .NET / C# library.</span></span> <span data-ttu-id="98c58-105">I övergången från version 1.0 till version 2.0 kan ett antal uppdateringar har gjorts som kräver migrering arbete i koden för att du kan dra nytta av de funktionella förbättringarna och funktion som tillägg kommer **Key Vault-certifikat**  stöder.</span><span class="sxs-lookup"><span data-stu-id="98c58-105">In the transition from the 1.0 version to the 2.0 version, a number of updates have been made that will require migration work in your code in order for you to benefit from the functional improvements and feature additions such as **Key Vault certificates** support.</span></span>

## <a name="key-vault-certificates"></a><span data-ttu-id="98c58-106">Key Vault-certifikat</span><span class="sxs-lookup"><span data-stu-id="98c58-106">Key Vault certificates</span></span>

<span data-ttu-id="98c58-107">Stöd för Key Vault-certifikat ger hantering av din x509 certifikat och följande:</span><span class="sxs-lookup"><span data-stu-id="98c58-107">Key Vault certificates support provides for management of your x509 certificates and the following behaviors:</span></span>  

* <span data-ttu-id="98c58-108">Gör en certifikat-ägare att skapa ett certifikat via en process för att skapa Nyckelvalvet eller importera ett befintligt certifikat.</span><span class="sxs-lookup"><span data-stu-id="98c58-108">Allows a certificate owner to create a certificate through a Key Vault creation process or through the import of an existing certificate.</span></span> <span data-ttu-id="98c58-109">Detta inkluderar både självsignerat och certifikatutfärdare genereras certifikat.</span><span class="sxs-lookup"><span data-stu-id="98c58-109">This includes both self-signed and Certificate Authority generated certificates.</span></span>
* <span data-ttu-id="98c58-110">Gör en Key Vault certifikat ägare att implementera säker lagring och hantering av X509 certifikat utan interaktion med material för privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="98c58-110">Allows a Key Vault certificate owner to implement secure storage and management of X509 certificates without interaction with private key material.</span></span>  
* <span data-ttu-id="98c58-111">Gör en certifikat-ägare att skapa en princip som leder Key Vault för att hantera livscykeln för ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="98c58-111">Allows a certificate owner to create a policy that directs Key Vault to manage the life-cycle of a certificate.</span></span>  
* <span data-ttu-id="98c58-112">Gör att certifikatet ägare att tillhandahålla kontaktinformation för meddelande om livscykeln händelser för förfallodatum och förnyelse av certifikat.</span><span class="sxs-lookup"><span data-stu-id="98c58-112">Allows certificate owners to provide contact information for notification about life-cycle events of expiration and renewal of certificate.</span></span>  
* <span data-ttu-id="98c58-113">Stöder automatisk förnyelse med valda utfärdare - Key Vault partner X509 certifikat providers / certifikatutfärdare.</span><span class="sxs-lookup"><span data-stu-id="98c58-113">Supports automatic renewal with selected issuers - Key Vault partner X509 certificate providers / certificate authorities.</span></span>
  
  * <span data-ttu-id="98c58-114">Observera - ej tillsammans providers/myndigheter kan också, men stöder inte automatisk förnyelse av funktionen.</span><span class="sxs-lookup"><span data-stu-id="98c58-114">NOTE - Non-partnered providers/authorities are also allowed but, will not support the auto renewal feature.</span></span>

## <a name="net-support"></a><span data-ttu-id="98c58-115">Stöd för .NET</span><span class="sxs-lookup"><span data-stu-id="98c58-115">.NET support</span></span>

* <span data-ttu-id="98c58-116">**.NET 4.0** stöds inte av version 2.0 av Azure Key Vault .NET / C#-biblioteket</span><span class="sxs-lookup"><span data-stu-id="98c58-116">**.NET 4.0** is not supported by the 2.0 version of the Azure Key Vault .NET/C# library</span></span>
* <span data-ttu-id="98c58-117">**.NET core** stöds av version 2.0 av Azure Key Vault .NET / C#-biblioteket</span><span class="sxs-lookup"><span data-stu-id="98c58-117">**.NET Core** is supported by the 2.0 version of the Azure Key Vault .NET/C# library</span></span>

## <a name="namespaces"></a><span data-ttu-id="98c58-118">namnområden</span><span class="sxs-lookup"><span data-stu-id="98c58-118">Namespaces</span></span>

* <span data-ttu-id="98c58-119">Namnområdet för **modeller** ändras från **Microsoft.Azure.KeyVault** till **Microsoft.Azure.KeyVault.Models**.</span><span class="sxs-lookup"><span data-stu-id="98c58-119">The namespace for **models** is changed from **Microsoft.Azure.KeyVault** to **Microsoft.Azure.KeyVault.Models**.</span></span>
* <span data-ttu-id="98c58-120">Den **Microsoft.Azure.KeyVault.Internal** namnområde har släppts.</span><span class="sxs-lookup"><span data-stu-id="98c58-120">The **Microsoft.Azure.KeyVault.Internal** namespace is dropped.</span></span>
* <span data-ttu-id="98c58-121">Azure SDK beroenden namnområdet ändras från **Hyak.Common** och **Hyak.Common.Internals** till **Microsoft.Rest** och  **Microsoft.Rest.Serialization**</span><span class="sxs-lookup"><span data-stu-id="98c58-121">The Azure SDK dependencies namespace are changed from **Hyak.Common** and **Hyak.Common.Internals** to **Microsoft.Rest** and **Microsoft.Rest.Serialization**</span></span>

## <a name="type-changes"></a><span data-ttu-id="98c58-122">Ändringar av typen</span><span class="sxs-lookup"><span data-stu-id="98c58-122">Type changes</span></span>

* <span data-ttu-id="98c58-123">*Hemligt* ändras till *SecretBundle*</span><span class="sxs-lookup"><span data-stu-id="98c58-123">*Secret* changed to *SecretBundle*</span></span>
* <span data-ttu-id="98c58-124">*Ordlistan* ändras till *IDictionary*</span><span class="sxs-lookup"><span data-stu-id="98c58-124">*Dictionary* changed to *IDictionary*</span></span>
* <span data-ttu-id="98c58-125">*Lista<T>, string []* ändras till *IList<T>*</span><span class="sxs-lookup"><span data-stu-id="98c58-125">*List<T>, string []* changed to *IList<T>*</span></span>
* <span data-ttu-id="98c58-126">*NextList* ändras till *NextPageLink*</span><span class="sxs-lookup"><span data-stu-id="98c58-126">*NextList* changed to  *NextPageLink*</span></span>

## <a name="return-types"></a><span data-ttu-id="98c58-127">Returtyper</span><span class="sxs-lookup"><span data-stu-id="98c58-127">Return types</span></span>

* <span data-ttu-id="98c58-128">**KeyList** och **SecretList** returnerar *IPage<T>*  i stället för *ListKeysResponseMessage*</span><span class="sxs-lookup"><span data-stu-id="98c58-128">**KeyList** and **SecretList** will return *IPage<T>* instead of *ListKeysResponseMessage*</span></span>
* <span data-ttu-id="98c58-129">Den genererade **BackupKeyAsync** returnerar *BackupKeyResult* som innehåller *värdet* (säkerhetskopiera blob).</span><span class="sxs-lookup"><span data-stu-id="98c58-129">The generated **BackupKeyAsync** will return *BackupKeyResult* which contains *Value* (back-up blob).</span></span> <span data-ttu-id="98c58-130">Innan metoden har omslutna och returnerar värdet.</span><span class="sxs-lookup"><span data-stu-id="98c58-130">Before the method was wrapped and returning only the value.</span></span>

## <a name="exceptions"></a><span data-ttu-id="98c58-131">Undantag</span><span class="sxs-lookup"><span data-stu-id="98c58-131">Exceptions</span></span>

* <span data-ttu-id="98c58-132">*KeyVaultClientException* ändras till *KeyVaultErrorException*</span><span class="sxs-lookup"><span data-stu-id="98c58-132">*KeyVaultClientException* is changed to *KeyVaultErrorException*</span></span>
* <span data-ttu-id="98c58-133">Fel i tjänsten har ändrats från *undantag. Fel* till *undantag. Body.Error.Message*.</span><span class="sxs-lookup"><span data-stu-id="98c58-133">The service error is changed from *exception.Error* to *exception.Body.Error.Message*.</span></span>
* <span data-ttu-id="98c58-134">Ytterligare information bort från felmeddelandet för **[JsonExtensionData]**.</span><span class="sxs-lookup"><span data-stu-id="98c58-134">Removed additional info from the error message for **[JsonExtensionData]**.</span></span>

## <a name="constructors"></a><span data-ttu-id="98c58-135">Konstruktorer</span><span class="sxs-lookup"><span data-stu-id="98c58-135">Constructors</span></span>

* <span data-ttu-id="98c58-136">I stället för att acceptera ett *HttpClient* som ett konstruktorargument konstruktorn accepterar bara *HttpClientHandler* eller *DelegatingHandler []*.</span><span class="sxs-lookup"><span data-stu-id="98c58-136">Instead of accepting an *HttpClient* as a constructor argument, the constructor only accepts *HttpClientHandler* or *DelegatingHandler[]*.</span></span>

## <a name="downloaded-packages"></a><span data-ttu-id="98c58-137">Hämtade paket</span><span class="sxs-lookup"><span data-stu-id="98c58-137">Downloaded packages</span></span>

<span data-ttu-id="98c58-138">När en klient beroende av Key Vault hämtades följande</span><span class="sxs-lookup"><span data-stu-id="98c58-138">When a client is processing a  dependency on Key Vault the following were downloaded</span></span>

### <a name="previous-package-list"></a><span data-ttu-id="98c58-139">Tidigare paketlistan</span><span class="sxs-lookup"><span data-stu-id="98c58-139">Previous package list</span></span>

* <span data-ttu-id="98c58-140">paketet id="Hyak.Common” version = ”1.0.2” targetFramework = ”net45”</span><span class="sxs-lookup"><span data-stu-id="98c58-140">package id="Hyak.Common" version="1.0.2" targetFramework="net45"</span></span>
* <span data-ttu-id="98c58-141">paketet id="Microsoft.Azure.Common” version = ”2.0.4” targetFramework = ”net45”</span><span class="sxs-lookup"><span data-stu-id="98c58-141">package id="Microsoft.Azure.Common" version="2.0.4" targetFramework="net45"</span></span>
* <span data-ttu-id="98c58-142">paketet id="Microsoft.Azure.Common.Dependencies” version = ”1.0.0” targetFramework = ”net45”</span><span class="sxs-lookup"><span data-stu-id="98c58-142">package id="Microsoft.Azure.Common.Dependencies" version="1.0.0" targetFramework="net45"</span></span>
* <span data-ttu-id="98c58-143">paketet id="Microsoft.Azure.KeyVault” version = ”1.0.0” targetFramework = ”net45”</span><span class="sxs-lookup"><span data-stu-id="98c58-143">package id="Microsoft.Azure.KeyVault" version="1.0.0" targetFramework="net45"</span></span>
* <span data-ttu-id="98c58-144">paketet id="Microsoft.Bcl” version = ”1.1.9” targetFramework = ”net45”</span><span class="sxs-lookup"><span data-stu-id="98c58-144">package id="Microsoft.Bcl" version="1.1.9" targetFramework="net45"</span></span>
* <span data-ttu-id="98c58-145">paketet id="Microsoft.Bcl.Async” version = ”1.0.168” targetFramework = ”net45”</span><span class="sxs-lookup"><span data-stu-id="98c58-145">package id="Microsoft.Bcl.Async" version="1.0.168" targetFramework="net45"</span></span>
* <span data-ttu-id="98c58-146">paketet id="Microsoft.Bcl.Build” version = ”1.0.14” targetFramework = ”net45”</span><span class="sxs-lookup"><span data-stu-id="98c58-146">package id="Microsoft.Bcl.Build" version="1.0.14" targetFramework="net45"</span></span>
* <span data-ttu-id="98c58-147">paketet id="Microsoft.Net.Http” version = ”2.2.22” targetFramework = ”net45”</span><span class="sxs-lookup"><span data-stu-id="98c58-147">package id="Microsoft.Net.Http" version="2.2.22" targetFramework="net45"</span></span>

### <a name="current-package-list"></a><span data-ttu-id="98c58-148">Aktuella paketlistan</span><span class="sxs-lookup"><span data-stu-id="98c58-148">Current package list</span></span>

* <span data-ttu-id="98c58-149">paketet id="Microsoft.Azure.KeyVault” version = ”2.0.0-preview” targetFramework = ”net45”</span><span class="sxs-lookup"><span data-stu-id="98c58-149">package id="Microsoft.Azure.KeyVault" version="2.0.0-preview" targetFramework="net45"</span></span>
* <span data-ttu-id="98c58-150">paketet id="Microsoft.Rest.ClientRuntime” version = ”2.2.0” targetFramework = ”net45”</span><span class="sxs-lookup"><span data-stu-id="98c58-150">package id="Microsoft.Rest.ClientRuntime" version="2.2.0" targetFramework="net45"</span></span>
* <span data-ttu-id="98c58-151">paketet id="Microsoft.Rest.ClientRuntime.Azure” version = ”3.2.0” targetFramework = ”net45”</span><span class="sxs-lookup"><span data-stu-id="98c58-151">package id="Microsoft.Rest.ClientRuntime.Azure" version="3.2.0" targetFramework="net45"</span></span>

## <a name="class-changes"></a><span data-ttu-id="98c58-152">Klassen ändringar</span><span class="sxs-lookup"><span data-stu-id="98c58-152">Class changes</span></span>

* <span data-ttu-id="98c58-153">**UnixEpoch** klass har tagits bort</span><span class="sxs-lookup"><span data-stu-id="98c58-153">**UnixEpoch** class has been removed</span></span>
* <span data-ttu-id="98c58-154">**Base64UrlConverter** klass har bytt namn till **Base64UrlJsonConverter**</span><span class="sxs-lookup"><span data-stu-id="98c58-154">**Base64UrlConverter** class is renamed to **Base64UrlJsonConverter**</span></span>

## <a name="other-changes"></a><span data-ttu-id="98c58-155">Andra ändringar</span><span class="sxs-lookup"><span data-stu-id="98c58-155">Other changes</span></span>

* <span data-ttu-id="98c58-156">Stöd för konfigurationen av KV åtgärden återförsöksprincip på tillfälliga fel har lagts till den här versionen av API: et.</span><span class="sxs-lookup"><span data-stu-id="98c58-156">Support for the configuration of KV operation retry policy on transient failures has been added to this version of the API.</span></span>

## <a name="microsoftazuremanagementkeyvault-nuget"></a><span data-ttu-id="98c58-157">Microsoft.Azure.Management.KeyVault NuGet</span><span class="sxs-lookup"><span data-stu-id="98c58-157">Microsoft.Azure.Management.KeyVault NuGet</span></span>

* <span data-ttu-id="98c58-158">För de åtgärder som returnerade en *valvet*, Returtypen har en klass som innehåller en egenskap för valvet.</span><span class="sxs-lookup"><span data-stu-id="98c58-158">For the operations that returned a *vault*, the return type was a class that contained a Vault property.</span></span> <span data-ttu-id="98c58-159">Returtypen är nu *valvet*.</span><span class="sxs-lookup"><span data-stu-id="98c58-159">The return type is now *Vault*.</span></span>
* <span data-ttu-id="98c58-160">*PermissionsToKeys* och *PermissionsToSecrets* är nu *Permissions.Keys* och *Permissions.Secrets*</span><span class="sxs-lookup"><span data-stu-id="98c58-160">*PermissionsToKeys* and *PermissionsToSecrets* are now *Permissions.Keys* and *Permissions.Secrets*</span></span>
* <span data-ttu-id="98c58-161">Några av de returtyper ändringarna gäller för kontrollen-plan samt.</span><span class="sxs-lookup"><span data-stu-id="98c58-161">Some of the return types changes apply to the control-plane as well.</span></span>

## <a name="microsoftazurekeyvaultextensions-nuget"></a><span data-ttu-id="98c58-162">Microsoft.Azure.KeyVault.Extensions NuGet</span><span class="sxs-lookup"><span data-stu-id="98c58-162">Microsoft.Azure.KeyVault.Extensions NuGet</span></span>

* <span data-ttu-id="98c58-163">Paketet har brutits till **Microsoft.Azure.KeyVault.Extensions** och **Microsoft.Azure.KeyVault.Cryptography** för kryptografiska åtgärder.</span><span class="sxs-lookup"><span data-stu-id="98c58-163">The package is broken up to **Microsoft.Azure.KeyVault.Extensions** and **Microsoft.Azure.KeyVault.Cryptography** for the cryptography operations.</span></span>

