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
# <a name="azure-key-vault-net-20---release-notes-and-migration-guide"></a><span data-ttu-id="e4de2-103">Azure Key Vault .NET 2.0 - viktig information och Migreringsguide</span><span class="sxs-lookup"><span data-stu-id="e4de2-103">Azure Key Vault .NET 2.0 - Release Notes and Migration Guide</span></span>
<span data-ttu-id="e4de2-104">hello följande information och vägledning är för utvecklare som arbetar med Azure Key Vault .NET / C#-biblioteket.</span><span class="sxs-lookup"><span data-stu-id="e4de2-104">hello following notes and guidance are for developers working with Azure Key Vault .NET / C# library.</span></span> <span data-ttu-id="e4de2-105">I hello övergång från hello 1.0 version toohello 2.0 version, ett antal uppdateringar har gjorts så kräver migrering arbete i koden för toobenefit från hello funktionella förbättringarna och funktionen tillägg som **Key Vault certifikat** stöd.</span><span class="sxs-lookup"><span data-stu-id="e4de2-105">In hello transition from hello 1.0 version toohello 2.0 version, a number of updates have been made that will require migration work in your code in order for you toobenefit from hello functional improvements and feature additions such as **Key Vault certificates** support.</span></span>

## <a name="key-vault-certificates"></a><span data-ttu-id="e4de2-106">Key Vault-certifikat</span><span class="sxs-lookup"><span data-stu-id="e4de2-106">Key Vault certificates</span></span>

<span data-ttu-id="e4de2-107">Stöd för Key Vault-certifikat ger hantering av din x509 certifikat och hello följande funktioner:</span><span class="sxs-lookup"><span data-stu-id="e4de2-107">Key Vault certificates support provides for management of your x509 certificates and hello following behaviors:</span></span>  

* <span data-ttu-id="e4de2-108">Tillåter att ett certifikat ägare toocreate ett certifikat via en process för att skapa Nyckelvalvet eller hello import av ett befintligt certifikat.</span><span class="sxs-lookup"><span data-stu-id="e4de2-108">Allows a certificate owner toocreate a certificate through a Key Vault creation process or through hello import of an existing certificate.</span></span> <span data-ttu-id="e4de2-109">Detta inkluderar både självsignerat och certifikatutfärdare genereras certifikat.</span><span class="sxs-lookup"><span data-stu-id="e4de2-109">This includes both self-signed and Certificate Authority generated certificates.</span></span>
* <span data-ttu-id="e4de2-110">Ger en Key Vault certifikat ägaren tooimplement säker lagring och hantering av X509 certifikat utan interaktion med material för privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="e4de2-110">Allows a Key Vault certificate owner tooimplement secure storage and management of X509 certificates without interaction with private key material.</span></span>  
* <span data-ttu-id="e4de2-111">Tillåter ett certifikat ägare toocreate en princip som leder Key Vault toomanage hello livscykeln för ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="e4de2-111">Allows a certificate owner toocreate a policy that directs Key Vault toomanage hello life-cycle of a certificate.</span></span>  
* <span data-ttu-id="e4de2-112">Tillåter certifikat ägare tooprovide kontaktinformation för meddelande om livscykeln händelser för förfallodatum och förnyelse av certifikat.</span><span class="sxs-lookup"><span data-stu-id="e4de2-112">Allows certificate owners tooprovide contact information for notification about life-cycle events of expiration and renewal of certificate.</span></span>  
* <span data-ttu-id="e4de2-113">Stöder automatisk förnyelse med valda utfärdare - Key Vault partner X509 certifikat providers / certifikatutfärdare.</span><span class="sxs-lookup"><span data-stu-id="e4de2-113">Supports automatic renewal with selected issuers - Key Vault partner X509 certificate providers / certificate authorities.</span></span>
  
  * <span data-ttu-id="e4de2-114">Observera - ej tillsammans providers/myndigheter kan också, men stöder inte hello automatisk förnyelse av funktionen.</span><span class="sxs-lookup"><span data-stu-id="e4de2-114">NOTE - Non-partnered providers/authorities are also allowed but, will not support hello auto renewal feature.</span></span>

## <a name="net-support"></a><span data-ttu-id="e4de2-115">Stöd för .NET</span><span class="sxs-lookup"><span data-stu-id="e4de2-115">.NET support</span></span>

* <span data-ttu-id="e4de2-116">**.NET 4.0** stöds inte av hello 2.0-versionen av hello Azure Key Vault .NET / C#-biblioteket</span><span class="sxs-lookup"><span data-stu-id="e4de2-116">**.NET 4.0** is not supported by hello 2.0 version of hello Azure Key Vault .NET/C# library</span></span>
* <span data-ttu-id="e4de2-117">**.NET core** stöds av hello 2.0-versionen av hello Azure Key Vault .NET / C#-biblioteket</span><span class="sxs-lookup"><span data-stu-id="e4de2-117">**.NET Core** is supported by hello 2.0 version of hello Azure Key Vault .NET/C# library</span></span>

## <a name="namespaces"></a><span data-ttu-id="e4de2-118">namnområden</span><span class="sxs-lookup"><span data-stu-id="e4de2-118">Namespaces</span></span>

* <span data-ttu-id="e4de2-119">Hej namnområdet för **modeller** ändras från **Microsoft.Azure.KeyVault** för**Microsoft.Azure.KeyVault.Models**.</span><span class="sxs-lookup"><span data-stu-id="e4de2-119">hello namespace for **models** is changed from **Microsoft.Azure.KeyVault** too**Microsoft.Azure.KeyVault.Models**.</span></span>
* <span data-ttu-id="e4de2-120">Hej **Microsoft.Azure.KeyVault.Internal** namnområde har släppts.</span><span class="sxs-lookup"><span data-stu-id="e4de2-120">hello **Microsoft.Azure.KeyVault.Internal** namespace is dropped.</span></span>
* <span data-ttu-id="e4de2-121">hello Azure SDK beroenden namnområde ändras från **Hyak.Common** och **Hyak.Common.Internals** för**Microsoft.Rest** och  **Microsoft.Rest.Serialization**</span><span class="sxs-lookup"><span data-stu-id="e4de2-121">hello Azure SDK dependencies namespace are changed from **Hyak.Common** and **Hyak.Common.Internals** too**Microsoft.Rest** and **Microsoft.Rest.Serialization**</span></span>

## <a name="type-changes"></a><span data-ttu-id="e4de2-122">Ändringar av typen</span><span class="sxs-lookup"><span data-stu-id="e4de2-122">Type changes</span></span>

* <span data-ttu-id="e4de2-123">*Hemligt* ändras för*SecretBundle*</span><span class="sxs-lookup"><span data-stu-id="e4de2-123">*Secret* changed too*SecretBundle*</span></span>
* <span data-ttu-id="e4de2-124">*Ordlistan* ändras för*IDictionary*</span><span class="sxs-lookup"><span data-stu-id="e4de2-124">*Dictionary* changed too*IDictionary*</span></span>
* <span data-ttu-id="e4de2-125">*Lista<T>, string []* ändras för*IList<T>*</span><span class="sxs-lookup"><span data-stu-id="e4de2-125">*List<T>, string []* changed too*IList<T>*</span></span>
* <span data-ttu-id="e4de2-126">*NextList* ändras för *NextPageLink*</span><span class="sxs-lookup"><span data-stu-id="e4de2-126">*NextList* changed too *NextPageLink*</span></span>

## <a name="return-types"></a><span data-ttu-id="e4de2-127">Returtyper</span><span class="sxs-lookup"><span data-stu-id="e4de2-127">Return types</span></span>

* <span data-ttu-id="e4de2-128">**KeyList** och **SecretList** returnerar *IPage<T>*  i stället för *ListKeysResponseMessage*</span><span class="sxs-lookup"><span data-stu-id="e4de2-128">**KeyList** and **SecretList** will return *IPage<T>* instead of *ListKeysResponseMessage*</span></span>
* <span data-ttu-id="e4de2-129">hello genereras **BackupKeyAsync** returnerar *BackupKeyResult* som innehåller *värdet* (säkerhetskopiera blob).</span><span class="sxs-lookup"><span data-stu-id="e4de2-129">hello generated **BackupKeyAsync** will return *BackupKeyResult* which contains *Value* (back-up blob).</span></span> <span data-ttu-id="e4de2-130">Före hello var metoden omsluten och returnerar bara hello-värde.</span><span class="sxs-lookup"><span data-stu-id="e4de2-130">Before hello method was wrapped and returning only hello value.</span></span>

## <a name="exceptions"></a><span data-ttu-id="e4de2-131">Undantag</span><span class="sxs-lookup"><span data-stu-id="e4de2-131">Exceptions</span></span>

* <span data-ttu-id="e4de2-132">*KeyVaultClientException* ändras för*KeyVaultErrorException*</span><span class="sxs-lookup"><span data-stu-id="e4de2-132">*KeyVaultClientException* is changed too*KeyVaultErrorException*</span></span>
* <span data-ttu-id="e4de2-133">fel i tjänsten hello ändras från *undantag. Fel* för*undantag. Body.Error.Message*.</span><span class="sxs-lookup"><span data-stu-id="e4de2-133">hello service error is changed from *exception.Error* too*exception.Body.Error.Message*.</span></span>
* <span data-ttu-id="e4de2-134">Ytterligare information bort från hello felmeddelande för **[JsonExtensionData]**.</span><span class="sxs-lookup"><span data-stu-id="e4de2-134">Removed additional info from hello error message for **[JsonExtensionData]**.</span></span>

## <a name="constructors"></a><span data-ttu-id="e4de2-135">Konstruktorer</span><span class="sxs-lookup"><span data-stu-id="e4de2-135">Constructors</span></span>

* <span data-ttu-id="e4de2-136">I stället för att acceptera ett *HttpClient* som ett konstruktorargument hello konstruktorn godkänner enbart *HttpClientHandler* eller *DelegatingHandler []*.</span><span class="sxs-lookup"><span data-stu-id="e4de2-136">Instead of accepting an *HttpClient* as a constructor argument, hello constructor only accepts *HttpClientHandler* or *DelegatingHandler[]*.</span></span>

## <a name="downloaded-packages"></a><span data-ttu-id="e4de2-137">Hämtade paket</span><span class="sxs-lookup"><span data-stu-id="e4de2-137">Downloaded packages</span></span>

<span data-ttu-id="e4de2-138">När en klient bearbetar ett beroende på har Key Vault hello följande hämtats</span><span class="sxs-lookup"><span data-stu-id="e4de2-138">When a client is processing a  dependency on Key Vault hello following were downloaded</span></span>

### <a name="previous-package-list"></a><span data-ttu-id="e4de2-139">Tidigare paketlistan</span><span class="sxs-lookup"><span data-stu-id="e4de2-139">Previous package list</span></span>

* <span data-ttu-id="e4de2-140">paketet id="Hyak.Common” version = ”1.0.2” targetFramework = ”net45”</span><span class="sxs-lookup"><span data-stu-id="e4de2-140">package id="Hyak.Common" version="1.0.2" targetFramework="net45"</span></span>
* <span data-ttu-id="e4de2-141">paketet id="Microsoft.Azure.Common” version = ”2.0.4” targetFramework = ”net45”</span><span class="sxs-lookup"><span data-stu-id="e4de2-141">package id="Microsoft.Azure.Common" version="2.0.4" targetFramework="net45"</span></span>
* <span data-ttu-id="e4de2-142">paketet id="Microsoft.Azure.Common.Dependencies” version = ”1.0.0” targetFramework = ”net45”</span><span class="sxs-lookup"><span data-stu-id="e4de2-142">package id="Microsoft.Azure.Common.Dependencies" version="1.0.0" targetFramework="net45"</span></span>
* <span data-ttu-id="e4de2-143">paketet id="Microsoft.Azure.KeyVault” version = ”1.0.0” targetFramework = ”net45”</span><span class="sxs-lookup"><span data-stu-id="e4de2-143">package id="Microsoft.Azure.KeyVault" version="1.0.0" targetFramework="net45"</span></span>
* <span data-ttu-id="e4de2-144">paketet id="Microsoft.Bcl” version = ”1.1.9” targetFramework = ”net45”</span><span class="sxs-lookup"><span data-stu-id="e4de2-144">package id="Microsoft.Bcl" version="1.1.9" targetFramework="net45"</span></span>
* <span data-ttu-id="e4de2-145">paketet id="Microsoft.Bcl.Async” version = ”1.0.168” targetFramework = ”net45”</span><span class="sxs-lookup"><span data-stu-id="e4de2-145">package id="Microsoft.Bcl.Async" version="1.0.168" targetFramework="net45"</span></span>
* <span data-ttu-id="e4de2-146">paketet id="Microsoft.Bcl.Build” version = ”1.0.14” targetFramework = ”net45”</span><span class="sxs-lookup"><span data-stu-id="e4de2-146">package id="Microsoft.Bcl.Build" version="1.0.14" targetFramework="net45"</span></span>
* <span data-ttu-id="e4de2-147">paketet id="Microsoft.Net.Http” version = ”2.2.22” targetFramework = ”net45”</span><span class="sxs-lookup"><span data-stu-id="e4de2-147">package id="Microsoft.Net.Http" version="2.2.22" targetFramework="net45"</span></span>

### <a name="current-package-list"></a><span data-ttu-id="e4de2-148">Aktuella paketlistan</span><span class="sxs-lookup"><span data-stu-id="e4de2-148">Current package list</span></span>

* <span data-ttu-id="e4de2-149">paketet id="Microsoft.Azure.KeyVault” version = ”2.0.0-preview” targetFramework = ”net45”</span><span class="sxs-lookup"><span data-stu-id="e4de2-149">package id="Microsoft.Azure.KeyVault" version="2.0.0-preview" targetFramework="net45"</span></span>
* <span data-ttu-id="e4de2-150">paketet id="Microsoft.Rest.ClientRuntime” version = ”2.2.0” targetFramework = ”net45”</span><span class="sxs-lookup"><span data-stu-id="e4de2-150">package id="Microsoft.Rest.ClientRuntime" version="2.2.0" targetFramework="net45"</span></span>
* <span data-ttu-id="e4de2-151">paketet id="Microsoft.Rest.ClientRuntime.Azure” version = ”3.2.0” targetFramework = ”net45”</span><span class="sxs-lookup"><span data-stu-id="e4de2-151">package id="Microsoft.Rest.ClientRuntime.Azure" version="3.2.0" targetFramework="net45"</span></span>

## <a name="class-changes"></a><span data-ttu-id="e4de2-152">Klassen ändringar</span><span class="sxs-lookup"><span data-stu-id="e4de2-152">Class changes</span></span>

* <span data-ttu-id="e4de2-153">**UnixEpoch** klass har tagits bort</span><span class="sxs-lookup"><span data-stu-id="e4de2-153">**UnixEpoch** class has been removed</span></span>
* <span data-ttu-id="e4de2-154">**Base64UrlConverter** klass har bytt namn för**Base64UrlJsonConverter**</span><span class="sxs-lookup"><span data-stu-id="e4de2-154">**Base64UrlConverter** class is renamed too**Base64UrlJsonConverter**</span></span>

## <a name="other-changes"></a><span data-ttu-id="e4de2-155">Andra ändringar</span><span class="sxs-lookup"><span data-stu-id="e4de2-155">Other changes</span></span>

* <span data-ttu-id="e4de2-156">Stöd för hello konfigurationen av KV åtgärden återförsöksprincip på tillfälliga fel har lagts toothis hello API-version.</span><span class="sxs-lookup"><span data-stu-id="e4de2-156">Support for hello configuration of KV operation retry policy on transient failures has been added toothis version of hello API.</span></span>

## <a name="microsoftazuremanagementkeyvault-nuget"></a><span data-ttu-id="e4de2-157">Microsoft.Azure.Management.KeyVault NuGet</span><span class="sxs-lookup"><span data-stu-id="e4de2-157">Microsoft.Azure.Management.KeyVault NuGet</span></span>

* <span data-ttu-id="e4de2-158">För hello-åtgärder som returnerade en *valvet*, hello returtyp har en klass som innehåller en egenskap för valvet.</span><span class="sxs-lookup"><span data-stu-id="e4de2-158">For hello operations that returned a *vault*, hello return type was a class that contained a Vault property.</span></span> <span data-ttu-id="e4de2-159">hello returtyp är nu *valvet*.</span><span class="sxs-lookup"><span data-stu-id="e4de2-159">hello return type is now *Vault*.</span></span>
* <span data-ttu-id="e4de2-160">*PermissionsToKeys* och *PermissionsToSecrets* är nu *Permissions.Keys* och *Permissions.Secrets*</span><span class="sxs-lookup"><span data-stu-id="e4de2-160">*PermissionsToKeys* and *PermissionsToSecrets* are now *Permissions.Keys* and *Permissions.Secrets*</span></span>
* <span data-ttu-id="e4de2-161">Vissa av hello returnera typer ändringarna toohello-kontroll-plan samt.</span><span class="sxs-lookup"><span data-stu-id="e4de2-161">Some of hello return types changes apply toohello control-plane as well.</span></span>

## <a name="microsoftazurekeyvaultextensions-nuget"></a><span data-ttu-id="e4de2-162">Microsoft.Azure.KeyVault.Extensions NuGet</span><span class="sxs-lookup"><span data-stu-id="e4de2-162">Microsoft.Azure.KeyVault.Extensions NuGet</span></span>

* <span data-ttu-id="e4de2-163">hello-paketet har delats upp för**Microsoft.Azure.KeyVault.Extensions** och **Microsoft.Azure.KeyVault.Cryptography** för hello kryptografiska åtgärder.</span><span class="sxs-lookup"><span data-stu-id="e4de2-163">hello package is broken up too**Microsoft.Azure.KeyVault.Extensions** and **Microsoft.Azure.KeyVault.Cryptography** for hello cryptography operations.</span></span>

