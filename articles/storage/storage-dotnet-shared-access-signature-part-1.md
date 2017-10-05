---
title: "Använda signaturer för delad åtkomst (SAS) i Azure Storage | Microsoft Docs"
description: "Lär dig använda signaturer för delad åtkomst (SAS) för att delegera åtkomst till Azure Storage-resurser, inklusive blobbar, köer, tabeller och filer."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 46fd99d7-36b3-4283-81e3-f214b29f1152
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 04/18/2017
ms.author: marsma
ms.openlocfilehash: ae944eae4e62132e47bf9069d83817a1a6779ff7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="using-shared-access-signatures-sas"></a><span data-ttu-id="a6df3-103">Använda signaturer för delad åtkomst (SAS)</span><span class="sxs-lookup"><span data-stu-id="a6df3-103">Using shared access signatures (SAS)</span></span>

<span data-ttu-id="a6df3-104">En signatur för delad åtkomst (SAS) ger dig ett sätt att ge begränsad åtkomst till objekt i ditt lagringskonto till andra klienter, utan att utsätta din kontonyckel.</span><span class="sxs-lookup"><span data-stu-id="a6df3-104">A shared access signature (SAS) provides you with a way to grant limited access to objects in your storage account to other clients, without exposing your account key.</span></span> <span data-ttu-id="a6df3-105">I den här artikeln vi ger en översikt över SAS-modellen, metodtips för SAS och titta på några exempel.</span><span class="sxs-lookup"><span data-stu-id="a6df3-105">In this article, we provide an overview of the SAS model, review SAS best practices, and look at some examples.</span></span>

<span data-ttu-id="a6df3-106">Ytterligare kodexempel med hjälp av SAS utöver de som visas här finns [komma igång med Azure Blob Storage i .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/) och andra exempel som är tillgängliga i den [Azure kodexempel](https://azure.microsoft.com/documentation/samples/?service=storage) bibliotek.</span><span class="sxs-lookup"><span data-stu-id="a6df3-106">For additional code examples using SAS beyond those presented here, see [Getting Started with Azure Blob Storage in .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/) and other samples available in the [Azure Code Samples](https://azure.microsoft.com/documentation/samples/?service=storage) library.</span></span> <span data-ttu-id="a6df3-107">Du kan hämta exempelprogrammen och köra dem eller bläddra koden på GitHub.</span><span class="sxs-lookup"><span data-stu-id="a6df3-107">You can download the sample applications and run them, or browse the code on GitHub.</span></span>

## <a name="what-is-a-shared-access-signature"></a><span data-ttu-id="a6df3-108">Vad är en signatur för delad åtkomst?</span><span class="sxs-lookup"><span data-stu-id="a6df3-108">What is a shared access signature?</span></span>
<span data-ttu-id="a6df3-109">En signatur för delad åtkomst ger delegerad åtkomst till resurser i ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="a6df3-109">A shared access signature provides delegated access to resources in your storage account.</span></span> <span data-ttu-id="a6df3-110">Med en SAS beviljar du klienter åtkomst till resurser i ditt lagringskonto, utan att dela dina nycklar för kontot.</span><span class="sxs-lookup"><span data-stu-id="a6df3-110">With a SAS, you can grant clients access to resources in your storage account, without sharing your account keys.</span></span> <span data-ttu-id="a6df3-111">Detta är den nyckel använder signaturer för delad åtkomst i dina program – en SAS är ett säkert sätt att dela dina lagringsresurser utan att kompromissa med dina nycklar för kontot.</span><span class="sxs-lookup"><span data-stu-id="a6df3-111">This is the key point of using shared access signatures in your applications--a SAS is a secure way to share your storage resources without compromising your account keys.</span></span>

[!INCLUDE [storage-account-key-note-include](../../includes/storage-account-key-note-include.md)]

<span data-ttu-id="a6df3-112">En SAS ger detaljerad kontroll över typ av åtkomst som du har gett till klienter som har SAS, inklusive:</span><span class="sxs-lookup"><span data-stu-id="a6df3-112">A SAS gives you granular control over the type of access you grant to clients who have the SAS, including:</span></span>

* <span data-ttu-id="a6df3-113">Tidsintervall som SAS är giltiga, inklusive starttid och förfallotiden.</span><span class="sxs-lookup"><span data-stu-id="a6df3-113">The interval over which the SAS is valid, including the start time and the expiry time.</span></span>
* <span data-ttu-id="a6df3-114">De behörigheter som utfärdats av SAS.</span><span class="sxs-lookup"><span data-stu-id="a6df3-114">The permissions granted by the SAS.</span></span> <span data-ttu-id="a6df3-115">Till exempel kan en SAS för en blob bevilja läsbehörighet och skrivbehörighet till blobben, men inte att ta bort behörigheter.</span><span class="sxs-lookup"><span data-stu-id="a6df3-115">For example, a SAS for a blob might grant read and write permissions to that blob, but not delete permissions.</span></span>
* <span data-ttu-id="a6df3-116">En valfri IP-adress eller intervall av IP-adresser som Azure Storage godkänner SAS.</span><span class="sxs-lookup"><span data-stu-id="a6df3-116">An optional IP address or range of IP addresses from which Azure Storage will accept the SAS.</span></span> <span data-ttu-id="a6df3-117">Du kan till exempel ange ett intervall med IP-adresser som tillhör din organisation.</span><span class="sxs-lookup"><span data-stu-id="a6df3-117">For example, you might specify a range of IP addresses belonging to your organization.</span></span>
* <span data-ttu-id="a6df3-118">Det protokoll som du ska ta emot SAS Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="a6df3-118">The protocol over which Azure Storage will accept the SAS.</span></span> <span data-ttu-id="a6df3-119">Du kan använda den här valfria parametern för att begränsa åtkomsten till klienter som använder HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a6df3-119">You can use this optional parameter to restrict access to clients using HTTPS.</span></span>

## <a name="when-should-you-use-a-shared-access-signature"></a><span data-ttu-id="a6df3-120">När bör du använda en signatur för delad åtkomst?</span><span class="sxs-lookup"><span data-stu-id="a6df3-120">When should you use a shared access signature?</span></span>
<span data-ttu-id="a6df3-121">Du kan använda en SAS när du vill ge åtkomst till resurser i ditt lagringskonto till alla klienter som inte har åtkomstnycklarna för ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="a6df3-121">You can use a SAS when you want to provide access to resources in your storage account to any client not possessing your storage account's access keys.</span></span> <span data-ttu-id="a6df3-122">Ditt lagringskonto innehåller både en primär och sekundär snabbtangent, som båda ha administrativ åtkomst till ditt konto och alla resurser i den.</span><span class="sxs-lookup"><span data-stu-id="a6df3-122">Your storage account includes both a primary and secondary access key, both of which grant administrative access to your account, and all resources within it.</span></span> <span data-ttu-id="a6df3-123">Ditt konto möjligheten att skadlig eller bristande öppnas exponering av någon av dessa nycklar.</span><span class="sxs-lookup"><span data-stu-id="a6df3-123">Exposing either of these keys opens your account to the possibility of malicious or negligent use.</span></span> <span data-ttu-id="a6df3-124">Signaturer för delad åtkomst är en säker alternativ som tillåter klienter att läsa, skriva och ta bort data i ditt lagringskonto enligt de behörigheter som du uttryckligen har beviljats och utan behovet av att någon kontonyckel.</span><span class="sxs-lookup"><span data-stu-id="a6df3-124">Shared access signatures provide a safe alternative that allows clients to read, write, and delete data in your storage account according to the permissions you've explicitly granted, and without need for an account key.</span></span>

<span data-ttu-id="a6df3-125">Ett vanligt scenario där en SAS är användbar är en tjänst där användare läsa och skriva sina egna data till ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="a6df3-125">A common scenario where a SAS is useful is a service where users read and write their own data to your storage account.</span></span> <span data-ttu-id="a6df3-126">Det finns två vanliga designmönster i ett scenario där ett lagringskonto lagrar användardata:</span><span class="sxs-lookup"><span data-stu-id="a6df3-126">In a scenario where a storage account stores user data, there are two typical design patterns:</span></span>

1. <span data-ttu-id="a6df3-127">Klienter ladda upp och hämta data via en frontend-proxytjänst som utför autentisering.</span><span class="sxs-lookup"><span data-stu-id="a6df3-127">Clients upload and download data via a front-end proxy service, which performs authentication.</span></span> <span data-ttu-id="a6df3-128">Proxytjänsten frontend-har fördelen att det tillåter validering av affärsregler, men för stora mängder data eller omfattande transaktioner, skapa en tjänst som kan skalas för att möta efterfrågan kan vara dyra eller svårt.</span><span class="sxs-lookup"><span data-stu-id="a6df3-128">This front-end proxy service has the advantage of allowing validation of business rules, but for large amounts of data or high-volume transactions, creating a service that can scale to match demand may be expensive or difficult.</span></span>

  ![Scenariot diagram: frontend-proxytjänst][sas-storage-fe-proxy-service]

1. <span data-ttu-id="a6df3-130">En förenklad tjänst autentiserar klienten efter behov och genererar en SAS.</span><span class="sxs-lookup"><span data-stu-id="a6df3-130">A lightweight service authenticates the client as needed and then generates a SAS.</span></span> <span data-ttu-id="a6df3-131">När klienten får SAS, kan de komma åt kontot lagringsresurser direkt med de behörigheter som definierats av SAS och för intervallet som tillåts av SAS.</span><span class="sxs-lookup"><span data-stu-id="a6df3-131">Once the client receives the SAS, they can access storage account resources directly with the permissions defined by the SAS and for the interval allowed by the SAS.</span></span> <span data-ttu-id="a6df3-132">SAS minskar behovet av att alla data via proxytjänsten frontend-routning.</span><span class="sxs-lookup"><span data-stu-id="a6df3-132">The SAS mitigates the need for routing all data through the front-end proxy service.</span></span>

  ![Scenariot diagram: SAS-providertjänst][sas-storage-provider-service]

<span data-ttu-id="a6df3-134">Många verkliga tjänster kan använda en kombination av dessa två sätt.</span><span class="sxs-lookup"><span data-stu-id="a6df3-134">Many real-world services may use a hybrid of these two approaches.</span></span> <span data-ttu-id="a6df3-135">Vissa data kan till exempel bearbetas och godkänts via frontend proxy, medan andra data är sparade och/eller läsa med hjälp av SAS.</span><span class="sxs-lookup"><span data-stu-id="a6df3-135">For example, some data might be processed and validated via the front-end proxy, while other data is saved and/or read directly using SAS.</span></span>

<span data-ttu-id="a6df3-136">Dessutom behöver du använda en SAS för att autentisera källobjektet i en kopieringsåtgärd i vissa scenarier:</span><span class="sxs-lookup"><span data-stu-id="a6df3-136">Additionally, you will need to use a SAS to authenticate the source object in a copy operation in certain scenarios:</span></span>

* <span data-ttu-id="a6df3-137">När du kopierar en blobb till en annan blob som finns i ett annat lagringskonto måste du använda en SAS för att autentisera käll-blob.</span><span class="sxs-lookup"><span data-stu-id="a6df3-137">When you copy a blob to another blob that resides in a different storage account, you must use a SAS to authenticate the source blob.</span></span> <span data-ttu-id="a6df3-138">Du kan eventuellt använda en SAS för att autentisera samt mål-blob.</span><span class="sxs-lookup"><span data-stu-id="a6df3-138">You can optionally use a SAS to authenticate the destination blob as well.</span></span>
* <span data-ttu-id="a6df3-139">När du kopierar en fil till en annan fil som finns i ett annat lagringskonto, måste du använda en SAS för att autentisera källfilen.</span><span class="sxs-lookup"><span data-stu-id="a6df3-139">When you copy a file to another file that resides in a different storage account, you must use a SAS to authenticate the source file.</span></span> <span data-ttu-id="a6df3-140">Du kan eventuellt använda en SAS för att autentisera till målfilen.</span><span class="sxs-lookup"><span data-stu-id="a6df3-140">You can optionally use a SAS to authenticate the destination file as well.</span></span>
* <span data-ttu-id="a6df3-141">När du kopierar en blobb till en fil eller en fil till en blobb, måste du använda en SAS för att autentisera källobjektet, även om käll- och objekt som finns inom samma lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="a6df3-141">When you copy a blob to a file, or a file to a blob, you must use a SAS to authenticate the source object, even if the source and destination objects reside within the same storage account.</span></span>

## <a name="types-of-shared-access-signatures"></a><span data-ttu-id="a6df3-142">Typer av signaturer för delad åtkomst</span><span class="sxs-lookup"><span data-stu-id="a6df3-142">Types of shared access signatures</span></span>
<span data-ttu-id="a6df3-143">Du kan skapa två typer av signaturer för delad åtkomst:</span><span class="sxs-lookup"><span data-stu-id="a6df3-143">You can create two types of shared access signatures:</span></span>

* <span data-ttu-id="a6df3-144">**Tjänst-SAS.**</span><span class="sxs-lookup"><span data-stu-id="a6df3-144">**Service SAS.**</span></span> <span data-ttu-id="a6df3-145">En tjänst-SAS delegerar åtkomst till en resurs i en av lagringstjänsterna: blobb-, kö-, tabell- eller filtjänsten.</span><span class="sxs-lookup"><span data-stu-id="a6df3-145">The service SAS delegates access to a resource in just one of the storage services: the Blob, Queue, Table, or File service.</span></span> <span data-ttu-id="a6df3-146">Se [hur du skapar en tjänst-SAS](https://msdn.microsoft.com/library/dn140255.aspx) och [Service SAS exempel](https://msdn.microsoft.com/library/dn140256.aspx) för detaljerad information om hur du skapar service SAS-token.</span><span class="sxs-lookup"><span data-stu-id="a6df3-146">See [Constructing a Service SAS](https://msdn.microsoft.com/library/dn140255.aspx) and [Service SAS Examples](https://msdn.microsoft.com/library/dn140256.aspx) for in-depth information about constructing the service SAS token.</span></span>
* <span data-ttu-id="a6df3-147">**Konto-SAS.**</span><span class="sxs-lookup"><span data-stu-id="a6df3-147">**Account SAS.**</span></span> <span data-ttu-id="a6df3-148">Den konto SAS delegater åtkomsten till resurser i en eller flera av lagringstjänsterna.</span><span class="sxs-lookup"><span data-stu-id="a6df3-148">The account SAS delegates access to resources in one or more of the storage services.</span></span> <span data-ttu-id="a6df3-149">Alla åtgärder som är tillgängliga via en tjänst-SAS finns också tillgängliga via en konto-SAS.</span><span class="sxs-lookup"><span data-stu-id="a6df3-149">All of the operations available via a service SAS are also available via an account SAS.</span></span> <span data-ttu-id="a6df3-150">Med konto-SAS, kan du dessutom delegera åtkomst till åtgärder som gäller för en viss tjänst som **Get/Set-tjänstegenskaper** och **få statistik för tjänsten**.</span><span class="sxs-lookup"><span data-stu-id="a6df3-150">Additionally, with the account SAS, you can delegate access to operations that apply to a given service, such as **Get/Set Service Properties** and **Get Service Stats**.</span></span> <span data-ttu-id="a6df3-151">Du kan också delegera åtkomst till läs-, skriv- och borttagningsåtgärder i blobbbehållare, tabeller, köer och filresurser som inte tillåts med en tjänst-SAS.</span><span class="sxs-lookup"><span data-stu-id="a6df3-151">You can also delegate access to read, write, and delete operations on blob containers, tables, queues, and file shares that are not permitted with a service SAS.</span></span> <span data-ttu-id="a6df3-152">Se [hur du skapar ett konto-SAS](https://msdn.microsoft.com/library/mt584140.aspx) för detaljerad information om hur du skapar kontot SAS-token.</span><span class="sxs-lookup"><span data-stu-id="a6df3-152">See [Constructing an Account SAS](https://msdn.microsoft.com/library/mt584140.aspx) for in-depth information about constructing the account SAS token.</span></span>

## <a name="how-a-shared-access-signature-works"></a><span data-ttu-id="a6df3-153">Så här fungerar en signatur för delad åtkomst</span><span class="sxs-lookup"><span data-stu-id="a6df3-153">How a shared access signature works</span></span>
<span data-ttu-id="a6df3-154">En signatur för delad åtkomst är en signerad URI som pekar på en eller flera lagringsresurser och innehåller en token som innehåller en särskild uppsättning Frågeparametrar.</span><span class="sxs-lookup"><span data-stu-id="a6df3-154">A shared access signature is a signed URI that points to one or more storage resources and includes a token that contains a special set of query parameters.</span></span> <span data-ttu-id="a6df3-155">Token som anger hur resurserna som kan användas av klienten.</span><span class="sxs-lookup"><span data-stu-id="a6df3-155">The token indicates how the resources may be accessed by the client.</span></span> <span data-ttu-id="a6df3-156">En av frågeparametrarna signatur, skapas från SAS-parametrar och signerad med kontonyckel.</span><span class="sxs-lookup"><span data-stu-id="a6df3-156">One of the query parameters, the signature, is constructed from the SAS parameters and signed with the account key.</span></span> <span data-ttu-id="a6df3-157">Den här signaturen används av Azure Storage för att autentisera SAS.</span><span class="sxs-lookup"><span data-stu-id="a6df3-157">This signature is used by Azure Storage to authenticate the SAS.</span></span>

<span data-ttu-id="a6df3-158">Här är ett exempel på en SAS-URI, visar resurs-URI och SAS-token:</span><span class="sxs-lookup"><span data-stu-id="a6df3-158">Here's an example of a SAS URI, showing the resource URI and the SAS token:</span></span>

![Komponenter för en SAS-URI][sas-storage-uri]

<span data-ttu-id="a6df3-160">SAS-token är en sträng som du skapar på den *klienten* sida (finns i [SAS exempel](#sas-examples) avsnittet kodexempel).</span><span class="sxs-lookup"><span data-stu-id="a6df3-160">The SAS token is a string you generate on the *client* side (see the [SAS examples](#sas-examples) section for code examples).</span></span> <span data-ttu-id="a6df3-161">En SAS-token som du skapar med storage-klientbiblioteket, till exempel spåras inte av Azure Storage på något sätt.</span><span class="sxs-lookup"><span data-stu-id="a6df3-161">A SAS token you generate with the storage client library, for example, is not tracked by Azure Storage in any way.</span></span> <span data-ttu-id="a6df3-162">Du kan skapa ett obegränsat antal SAS-token på klientsidan.</span><span class="sxs-lookup"><span data-stu-id="a6df3-162">You can create an unlimited number of SAS tokens on the client side.</span></span>

<span data-ttu-id="a6df3-163">När en klient innehåller ett SAS-URI till Azure Storage som en del av en begäran, kontrollerar tjänsten SAS parametrar och signatur för att kontrollera att den är giltig för att autentisera begäran.</span><span class="sxs-lookup"><span data-stu-id="a6df3-163">When a client provides a SAS URI to Azure Storage as part of a request, the service checks the SAS parameters and signature to verify that it is valid for authenticating the request.</span></span> <span data-ttu-id="a6df3-164">Om tjänsten verifierar att signaturen är giltig och sedan autentisera begäran.</span><span class="sxs-lookup"><span data-stu-id="a6df3-164">If the service verifies that the signature is valid, then the request is authenticated.</span></span> <span data-ttu-id="a6df3-165">Annars har begäran nekats med felkoden 403 (förbjuden).</span><span class="sxs-lookup"><span data-stu-id="a6df3-165">Otherwise, the request is declined with error code 403 (Forbidden).</span></span>

## <a name="shared-access-signature-parameters"></a><span data-ttu-id="a6df3-166">Parametrar för signatur för delad åtkomst</span><span class="sxs-lookup"><span data-stu-id="a6df3-166">Shared access signature parameters</span></span>
<span data-ttu-id="a6df3-167">Konto-SAS och tjänsten SAS-token innehåller några vanliga parametrar och också tar några parametrar som är olika.</span><span class="sxs-lookup"><span data-stu-id="a6df3-167">The account SAS and service SAS tokens include some common parameters, and also take a few parameters that that are different.</span></span>

### <a name="parameters-common-to-account-sas-and-service-sas-tokens"></a><span data-ttu-id="a6df3-168">Parametrar som är gemensamma för konto-SAS och tjänsten SAS-token</span><span class="sxs-lookup"><span data-stu-id="a6df3-168">Parameters common to account SAS and service SAS tokens</span></span>
* <span data-ttu-id="a6df3-169">**API-versionen** en valfri parameter som anger storage service-version du använder för att utföra begäran.</span><span class="sxs-lookup"><span data-stu-id="a6df3-169">**Api version** An optional parameter that specifies the storage service version to use to execute the request.</span></span>
* <span data-ttu-id="a6df3-170">**Service-version** en obligatorisk parameter som anger storage service-version du använder för att autentisera begäran.</span><span class="sxs-lookup"><span data-stu-id="a6df3-170">**Service version** A required parameter that specifies the storage service version to use to authenticate the request.</span></span>
* <span data-ttu-id="a6df3-171">**Starttid.**</span><span class="sxs-lookup"><span data-stu-id="a6df3-171">**Start time.**</span></span> <span data-ttu-id="a6df3-172">Det här är den tid då SAS börjar gälla.</span><span class="sxs-lookup"><span data-stu-id="a6df3-172">This is the time at which the SAS becomes valid.</span></span> <span data-ttu-id="a6df3-173">Starttiden för en signatur för delad åtkomst är valfritt.</span><span class="sxs-lookup"><span data-stu-id="a6df3-173">The start time for a shared access signature is optional.</span></span> <span data-ttu-id="a6df3-174">Om en starttid utelämnas är SAS gälla omedelbart.</span><span class="sxs-lookup"><span data-stu-id="a6df3-174">If a start time is omitted, the SAS is effective immediately.</span></span> <span data-ttu-id="a6df3-175">Starttiden måste anges i UTC (Coordinated Universal Time) med en särskild UTC beteckning (”Z”), till exempel `1994-11-05T13:15:30Z`.</span><span class="sxs-lookup"><span data-stu-id="a6df3-175">The start time must be expressed in UTC (Coordinated Universal Time), with a special UTC designator ("Z"), for example `1994-11-05T13:15:30Z`.</span></span>
* <span data-ttu-id="a6df3-176">**Förfallotiden.**</span><span class="sxs-lookup"><span data-stu-id="a6df3-176">**Expiry time.**</span></span> <span data-ttu-id="a6df3-177">Detta är den tid då SAS inte är giltig längre.</span><span class="sxs-lookup"><span data-stu-id="a6df3-177">This is the time after which the SAS is no longer valid.</span></span> <span data-ttu-id="a6df3-178">Bästa praxis rekommenderar att du antingen ange en förfallotiden för en SAS eller koppla den till en princip för lagrade åtkomst.</span><span class="sxs-lookup"><span data-stu-id="a6df3-178">Best practices recommend that you either specify an expiry time for a SAS, or associate it with a stored access policy.</span></span> <span data-ttu-id="a6df3-179">Förfallotid måste anges i UTC (Coordinated Universal Time) med en särskild UTC beteckning (”Z”), till exempel `1994-11-05T13:15:30Z` (se mer nedan).</span><span class="sxs-lookup"><span data-stu-id="a6df3-179">The expiry time must be expressed in UTC (Coordinated Universal Time), with a special UTC designator ("Z"), for example `1994-11-05T13:15:30Z` (see more below).</span></span>
* <span data-ttu-id="a6df3-180">**Behörigheter.**</span><span class="sxs-lookup"><span data-stu-id="a6df3-180">**Permissions.**</span></span> <span data-ttu-id="a6df3-181">Behörigheter som anges på SAS visar vilka åtgärder som klienten kan utföra mot storage-resursen med hjälp av SAS.</span><span class="sxs-lookup"><span data-stu-id="a6df3-181">The permissions specified on the SAS indicate what operations the client can perform against the storage resource using the SAS.</span></span> <span data-ttu-id="a6df3-182">Tillgängliga behörigheter skiljer sig för en konto-SAS och en tjänst-SAS.</span><span class="sxs-lookup"><span data-stu-id="a6df3-182">Available permissions differ for an account SAS and a service SAS.</span></span>
* <span data-ttu-id="a6df3-183">**IP-ADRESSEN.**</span><span class="sxs-lookup"><span data-stu-id="a6df3-183">**IP.**</span></span> <span data-ttu-id="a6df3-184">En valfri parameter som anger en IP-adress eller ett intervall med IP-adresser utanför Azure (finns i avsnittet [konfiguration av Routning sessionstillstånd](../expressroute/expressroute-workflows.md#routing-session-configuration-state) för Express Route) som du vill acceptera förfrågningar från.</span><span class="sxs-lookup"><span data-stu-id="a6df3-184">An optional parameter that specifies an IP address or a range of IP addresses outside of Azure (see the section [Routing session configuration state](../expressroute/expressroute-workflows.md#routing-session-configuration-state) for Express Route) from which to accept requests.</span></span>
* <span data-ttu-id="a6df3-185">**Protokoll.**</span><span class="sxs-lookup"><span data-stu-id="a6df3-185">**Protocol.**</span></span> <span data-ttu-id="a6df3-186">En valfri parameter som anger vilket protokoll som tillåts för en begäran.</span><span class="sxs-lookup"><span data-stu-id="a6df3-186">An optional parameter that specifies the protocol permitted for a request.</span></span> <span data-ttu-id="a6df3-187">Möjliga värden är HTTPS- och HTTP (`https,http`), vilket är standardvärdet eller HTTPS endast (`https`).</span><span class="sxs-lookup"><span data-stu-id="a6df3-187">Possible values are both HTTPS and HTTP (`https,http`), which is the default value, or HTTPS only (`https`).</span></span> <span data-ttu-id="a6df3-188">Observera att HTTP endast inte är tillåtna värdet.</span><span class="sxs-lookup"><span data-stu-id="a6df3-188">Note that HTTP only is not a permitted value.</span></span>
* <span data-ttu-id="a6df3-189">**Signatur.**</span><span class="sxs-lookup"><span data-stu-id="a6df3-189">**Signature.**</span></span> <span data-ttu-id="a6df3-190">Signaturen har skapats från de andra parametrar som angetts som en del token och sedan krypteras.</span><span class="sxs-lookup"><span data-stu-id="a6df3-190">The signature is constructed from the other parameters specified as part token and then encrypted.</span></span> <span data-ttu-id="a6df3-191">Den används för att autentisera SAS.</span><span class="sxs-lookup"><span data-stu-id="a6df3-191">It's used to authenticate the SAS.</span></span>

### <a name="parameters-for-a-service-sas-token"></a><span data-ttu-id="a6df3-192">Parametrar för en tjänst-SAS-token</span><span class="sxs-lookup"><span data-stu-id="a6df3-192">Parameters for a service SAS token</span></span>
* <span data-ttu-id="a6df3-193">**Storage-resursen.**</span><span class="sxs-lookup"><span data-stu-id="a6df3-193">**Storage resource.**</span></span> <span data-ttu-id="a6df3-194">Storage-resurser som du kan delegera åtkomst med en tjänst SAS inkluderar:</span><span class="sxs-lookup"><span data-stu-id="a6df3-194">Storage resources for which you can delegate access with a service SAS include:</span></span>
  * <span data-ttu-id="a6df3-195">Behållare och blobbar</span><span class="sxs-lookup"><span data-stu-id="a6df3-195">Containers and blobs</span></span>
  * <span data-ttu-id="a6df3-196">Filresurser och filer</span><span class="sxs-lookup"><span data-stu-id="a6df3-196">File shares and files</span></span>
  * <span data-ttu-id="a6df3-197">Köer</span><span class="sxs-lookup"><span data-stu-id="a6df3-197">Queues</span></span>
  * <span data-ttu-id="a6df3-198">Tabeller och intervallen för tabellentiteter.</span><span class="sxs-lookup"><span data-stu-id="a6df3-198">Tables and ranges of table entities.</span></span>

### <a name="parameters-for-an-account-sas-token"></a><span data-ttu-id="a6df3-199">Parametrar för ett konto SAS-token</span><span class="sxs-lookup"><span data-stu-id="a6df3-199">Parameters for an account SAS token</span></span>
* <span data-ttu-id="a6df3-200">**Tjänsten eller tjänster.**</span><span class="sxs-lookup"><span data-stu-id="a6df3-200">**Service or services.**</span></span> <span data-ttu-id="a6df3-201">En konto-SAS kan delegera åtkomst till en eller flera av lagringstjänsterna.</span><span class="sxs-lookup"><span data-stu-id="a6df3-201">An account SAS can delegate access to one or more of the storage services.</span></span> <span data-ttu-id="a6df3-202">Du kan till exempel skapa en konto-SAS som ombud åtkomsten till tjänsten Blob och filen.</span><span class="sxs-lookup"><span data-stu-id="a6df3-202">For example, you can create an account SAS that delegates access to the Blob and File service.</span></span> <span data-ttu-id="a6df3-203">Eller så kan du skapa en SAS att delegater åtkomst till alla fyra tjänster (Blob, kön, tabell och filen).</span><span class="sxs-lookup"><span data-stu-id="a6df3-203">Or you can create a SAS that delegates access to all four services (Blob, Queue, Table, and File).</span></span>
* <span data-ttu-id="a6df3-204">**Typer av lagring.**</span><span class="sxs-lookup"><span data-stu-id="a6df3-204">**Storage resource types.**</span></span> <span data-ttu-id="a6df3-205">Ett konto SAS gäller för en eller flera klasser av lagringsresurser, i stället för en viss resurs.</span><span class="sxs-lookup"><span data-stu-id="a6df3-205">An account SAS applies to one or more classes of storage resources, rather than a specific resource.</span></span> <span data-ttu-id="a6df3-206">Du kan skapa en konto-SAS att delegera åtkomst till:</span><span class="sxs-lookup"><span data-stu-id="a6df3-206">You can create an account SAS to delegate access to:</span></span>
  * <span data-ttu-id="a6df3-207">Servicenivå API: er, som kallas mot lagringsresurs för kontot.</span><span class="sxs-lookup"><span data-stu-id="a6df3-207">Service-level APIs, which are called against the storage account resource.</span></span> <span data-ttu-id="a6df3-208">Exempel inkluderar **Get/Set-tjänstegenskaper**, **få statistik för tjänsten**, och **lista behållare/köer/tabeller/resurser**.</span><span class="sxs-lookup"><span data-stu-id="a6df3-208">Examples include **Get/Set Service Properties**, **Get Service Stats**, and **List Containers/Queues/Tables/Shares**.</span></span>
  * <span data-ttu-id="a6df3-209">Behållare på objektnivå API: er, som kallas mot behållaren objekten för varje tjänst: blob-behållare, köer, tabeller och filresurser.</span><span class="sxs-lookup"><span data-stu-id="a6df3-209">Container-level APIs, which are called against the container objects for each service: blob containers, queues, tables, and file shares.</span></span> <span data-ttu-id="a6df3-210">Exempel inkluderar **skapa/ta bort behållaren**, **skapa/ta bort kön**, **skapa/ta bort tabellen**, **skapa/ta bort resursen**, och  **Lista över BLOB-filer och kataloger**.</span><span class="sxs-lookup"><span data-stu-id="a6df3-210">Examples include **Create/Delete Container**, **Create/Delete Queue**, **Create/Delete Table**, **Create/Delete Share**, and **List Blobs/Files and Directories**.</span></span>
  * <span data-ttu-id="a6df3-211">På objektnivå API: er, som kallas mot BLOB, Kömeddelanden, tabellentiteter och filer.</span><span class="sxs-lookup"><span data-stu-id="a6df3-211">Object-level APIs, which are called against blobs, queue messages, table entities, and files.</span></span> <span data-ttu-id="a6df3-212">Till exempel **placera Blob**, **frågan entiteten**, **få meddelanden**, och **Create File**.</span><span class="sxs-lookup"><span data-stu-id="a6df3-212">For example, **Put Blob**, **Query Entity**, **Get Messages**, and **Create File**.</span></span>

## <a name="examples-of-sas-uris"></a><span data-ttu-id="a6df3-213">Exempel på SAS URI: er</span><span class="sxs-lookup"><span data-stu-id="a6df3-213">Examples of SAS URIs</span></span>

### <a name="service-sas-uri-example"></a><span data-ttu-id="a6df3-214">Tjänsten SAS-URI-exempel</span><span class="sxs-lookup"><span data-stu-id="a6df3-214">Service SAS URI example</span></span>

<span data-ttu-id="a6df3-215">Här är ett exempel på en tjänst SAS-URI som ger Läs- och skrivbehörighet till en blob.</span><span class="sxs-lookup"><span data-stu-id="a6df3-215">Here is an example of a service SAS URI that provides read and write permissions to a blob.</span></span> <span data-ttu-id="a6df3-216">Tabellen uppdelad varje del för att förstå hur den bidrar till SAS-URI:</span><span class="sxs-lookup"><span data-stu-id="a6df3-216">The table breaks down each part of the URI to understand how it contributes to the SAS:</span></span>

```
https://myaccount.blob.core.windows.net/sascontainer/sasblob.txt?sv=2015-04-05&st=2015-04-29T22%3A18%3A26Z&se=2015-04-30T02%3A23%3A26Z&sr=b&sp=rw&sip=168.1.5.60-168.1.5.70&spr=https&sig=Z%2FRHIX5Xcg0Mq2rqI3OlWTjEg2tYkboXr1P9ZUXDtkk%3D
```

| <span data-ttu-id="a6df3-217">Namn</span><span class="sxs-lookup"><span data-stu-id="a6df3-217">Name</span></span> | <span data-ttu-id="a6df3-218">SAS-delen</span><span class="sxs-lookup"><span data-stu-id="a6df3-218">SAS portion</span></span> | <span data-ttu-id="a6df3-219">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a6df3-219">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a6df3-220">BLOB-URI</span><span class="sxs-lookup"><span data-stu-id="a6df3-220">Blob URI</span></span> |`https://myaccount.blob.core.windows.net/sascontainer/sasblob.txt` |<span data-ttu-id="a6df3-221">Adressen till blob.</span><span class="sxs-lookup"><span data-stu-id="a6df3-221">The address of the blob.</span></span> <span data-ttu-id="a6df3-222">Observera att med hjälp av HTTPS rekommenderas starkt.</span><span class="sxs-lookup"><span data-stu-id="a6df3-222">Note that using HTTPS is highly recommended.</span></span> |
| <span data-ttu-id="a6df3-223">Storage services version</span><span class="sxs-lookup"><span data-stu-id="a6df3-223">Storage services version</span></span> |`sv=2015-04-05` |<span data-ttu-id="a6df3-224">För lagringstjänster version 12-02-2012 och senare kan den här parametern anger versionen som ska användas.</span><span class="sxs-lookup"><span data-stu-id="a6df3-224">For storage services version 2012-02-12 and later, this parameter indicates the version to use.</span></span> |
| <span data-ttu-id="a6df3-225">Starttid</span><span class="sxs-lookup"><span data-stu-id="a6df3-225">Start time</span></span> |`st=2015-04-29T22%3A18%3A26Z` |<span data-ttu-id="a6df3-226">Angivet i UTC-tid.</span><span class="sxs-lookup"><span data-stu-id="a6df3-226">Specified in UTC time.</span></span> <span data-ttu-id="a6df3-227">Om du vill SAS ska gälla omedelbart, utelämna starttiden.</span><span class="sxs-lookup"><span data-stu-id="a6df3-227">If you want the SAS to be valid immediately, omit the start time.</span></span> |
| <span data-ttu-id="a6df3-228">Förfallotiden</span><span class="sxs-lookup"><span data-stu-id="a6df3-228">Expiry time</span></span> |`se=2015-04-30T02%3A23%3A26Z` |<span data-ttu-id="a6df3-229">Angivet i UTC-tid.</span><span class="sxs-lookup"><span data-stu-id="a6df3-229">Specified in UTC time.</span></span> |
| <span data-ttu-id="a6df3-230">Resurs</span><span class="sxs-lookup"><span data-stu-id="a6df3-230">Resource</span></span> |`sr=b` |<span data-ttu-id="a6df3-231">Resursen är en blob.</span><span class="sxs-lookup"><span data-stu-id="a6df3-231">The resource is a blob.</span></span> |
| <span data-ttu-id="a6df3-232">Behörigheter</span><span class="sxs-lookup"><span data-stu-id="a6df3-232">Permissions</span></span> |`sp=rw` |<span data-ttu-id="a6df3-233">De behörigheter som utfärdats av SAS omfattar Read (r) och skriva (b).</span><span class="sxs-lookup"><span data-stu-id="a6df3-233">The permissions granted by the SAS include Read (r) and Write (w).</span></span> |
| <span data-ttu-id="a6df3-234">IP-intervall</span><span class="sxs-lookup"><span data-stu-id="a6df3-234">IP range</span></span> |`sip=168.1.5.60-168.1.5.70` |<span data-ttu-id="a6df3-235">Intervallet av IP-adresser som en begäran kommer att accepteras.</span><span class="sxs-lookup"><span data-stu-id="a6df3-235">The range of IP addresses from which a request will be accepted.</span></span> |
| <span data-ttu-id="a6df3-236">Protokoll</span><span class="sxs-lookup"><span data-stu-id="a6df3-236">Protocol</span></span> |`spr=https` |<span data-ttu-id="a6df3-237">Endast de begäranden som använder HTTPS tillåts.</span><span class="sxs-lookup"><span data-stu-id="a6df3-237">Only requests using HTTPS are permitted.</span></span> |
| <span data-ttu-id="a6df3-238">Signatur</span><span class="sxs-lookup"><span data-stu-id="a6df3-238">Signature</span></span> |`sig=Z%2FRHIX5Xcg0Mq2rqI3OlWTjEg2tYkboXr1P9ZUXDtkk%3D` |<span data-ttu-id="a6df3-239">Används för att autentisera åtkomst till blob.</span><span class="sxs-lookup"><span data-stu-id="a6df3-239">Used to authenticate access to the blob.</span></span> <span data-ttu-id="a6df3-240">Signaturen är en HMAC beräknas över ett sträng-inloggning och nyckeln med hjälp av SHA256-algoritmen och sedan kodad med Base64-kodning.</span><span class="sxs-lookup"><span data-stu-id="a6df3-240">The signature is an HMAC computed over a string-to-sign and key using the SHA256 algorithm, and then encoded using Base64 encoding.</span></span> |

### <a name="account-sas-uri-example"></a><span data-ttu-id="a6df3-241">Kontot SAS-URI-exempel</span><span class="sxs-lookup"><span data-stu-id="a6df3-241">Account SAS URI example</span></span>

<span data-ttu-id="a6df3-242">Här är ett exempel på en konto-SAS som använder samma gemensamma parametrar i token.</span><span class="sxs-lookup"><span data-stu-id="a6df3-242">Here is an example of an account SAS that uses the same common parameters on the token.</span></span> <span data-ttu-id="a6df3-243">Eftersom dessa parametrar beskrivs ovan, beskrivs de inte här.</span><span class="sxs-lookup"><span data-stu-id="a6df3-243">Since these parameters are described above, they are not described here.</span></span> <span data-ttu-id="a6df3-244">Endast parametrarna som är specifika för kontot SAS beskrivs i tabellen nedan.</span><span class="sxs-lookup"><span data-stu-id="a6df3-244">Only the parameters that are specific to account SAS are described in the table below.</span></span>

```
https://myaccount.blob.core.windows.net/?restype=service&comp=properties&sv=2015-04-05&ss=bf&srt=s&st=2015-04-29T22%3A18%3A26Z&se=2015-04-30T02%3A23%3A26Z&sr=b&sp=rw&sip=168.1.5.60-168.1.5.70&spr=https&sig=F%6GRVAZ5Cdj2Pw4tgU7IlSTkWgn7bUkkAg8P6HESXwmf%4B
```

| <span data-ttu-id="a6df3-245">Namn</span><span class="sxs-lookup"><span data-stu-id="a6df3-245">Name</span></span> | <span data-ttu-id="a6df3-246">SAS-delen</span><span class="sxs-lookup"><span data-stu-id="a6df3-246">SAS portion</span></span> | <span data-ttu-id="a6df3-247">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a6df3-247">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a6df3-248">Resurs-URI</span><span class="sxs-lookup"><span data-stu-id="a6df3-248">Resource URI</span></span> |`https://myaccount.blob.core.windows.net/?restype=service&comp=properties` |<span data-ttu-id="a6df3-249">Blob-tjänstens slutpunkt, med parametrar för att hämta tjänstegenskaper (när den anropas med GET) eller ange tjänstegenskaper (när den anropas med).</span><span class="sxs-lookup"><span data-stu-id="a6df3-249">The Blob service endpoint, with parameters for getting service properties (when called with GET) or setting service properties (when called with SET).</span></span> |
| <span data-ttu-id="a6df3-250">Tjänster</span><span class="sxs-lookup"><span data-stu-id="a6df3-250">Services</span></span> |`ss=bf` |<span data-ttu-id="a6df3-251">SAS gäller Blob och Filtjänster</span><span class="sxs-lookup"><span data-stu-id="a6df3-251">The SAS applies to the Blob and File services</span></span> |
| <span data-ttu-id="a6df3-252">Typer av resurser</span><span class="sxs-lookup"><span data-stu-id="a6df3-252">Resource types</span></span> |`srt=s` |<span data-ttu-id="a6df3-253">SAS gäller för servicenivå operationer.</span><span class="sxs-lookup"><span data-stu-id="a6df3-253">The SAS applies to service-level operations.</span></span> |
| <span data-ttu-id="a6df3-254">Behörigheter</span><span class="sxs-lookup"><span data-stu-id="a6df3-254">Permissions</span></span> |`sp=rw` |<span data-ttu-id="a6df3-255">Behörigheterna som beviljar åtkomst Läs-och skrivåtgärder.</span><span class="sxs-lookup"><span data-stu-id="a6df3-255">The permissions grant access to read and write operations.</span></span> |

<span data-ttu-id="a6df3-256">Med hänsyn till att behörigheter är begränsade till servicenivån tillgängliga åtgärder med den här SAS är **hämta egenskaper för Blob-tjänsten** (läsa) och **ange egenskaper för Blob-tjänsten** (Skriv).</span><span class="sxs-lookup"><span data-stu-id="a6df3-256">Given that permissions are restricted to the service level, accessible operations with this SAS are **Get Blob Service Properties** (read) and **Set Blob Service Properties** (write).</span></span> <span data-ttu-id="a6df3-257">Men med en annan resurs-URI, samma SAS-token kan även användas att delegera åtkomst till **få statistik för Blob-tjänsten** (läsa).</span><span class="sxs-lookup"><span data-stu-id="a6df3-257">However, with a different resource URI, the same SAS token could also be used to delegate access to **Get Blob Service Stats** (read).</span></span>

## <a name="controlling-a-sas-with-a-stored-access-policy"></a><span data-ttu-id="a6df3-258">Kontrollera en SAS med en princip lagrade åtkomst</span><span class="sxs-lookup"><span data-stu-id="a6df3-258">Controlling a SAS with a stored access policy</span></span>
<span data-ttu-id="a6df3-259">En signatur för delad åtkomst kan göra något av två typer:</span><span class="sxs-lookup"><span data-stu-id="a6df3-259">A shared access signature can take one of two forms:</span></span>

* <span data-ttu-id="a6df3-260">**Ad hoc-SAS:** när du skapar en ad hoc-SAS starttid, förfallotid, och behörigheterna för SAS är alla angivna i SAS-URI (eller underförstådda, i de fall där starttiden utelämnas).</span><span class="sxs-lookup"><span data-stu-id="a6df3-260">**Ad hoc SAS:** When you create an ad hoc SAS, the start time, expiry time, and permissions for the SAS are all specified in the SAS URI (or implied, in the case where start time is omitted).</span></span> <span data-ttu-id="a6df3-261">Den här typen av SAS kan skapas som ett konto SAS eller en tjänst-SAS.</span><span class="sxs-lookup"><span data-stu-id="a6df3-261">This type of SAS can be created as an account SAS or a service SAS.</span></span>
* <span data-ttu-id="a6df3-262">**SAS med lagrade åtkomstprincip:** lagrade åtkomstprincip har definierats för en resurs behållare – en blob-behållare, tabell, kö, eller filresurs-- och kan användas för att hantera begränsningar för en eller flera signaturer för delad åtkomst.</span><span class="sxs-lookup"><span data-stu-id="a6df3-262">**SAS with stored access policy:** A stored access policy is defined on a resource container--a blob container, table, queue, or file share--and can be used to manage constraints for one or more shared access signatures.</span></span> <span data-ttu-id="a6df3-263">När du kopplar en SAS med en lagrad till ärver SAS begränsningarna--starttid, förfallotiden och behörigheter--har definierats för den lagrade åtkomstprincipen.</span><span class="sxs-lookup"><span data-stu-id="a6df3-263">When you associate a SAS with a stored access policy, the SAS inherits the constraints--the start time, expiry time, and permissions--defined for the stored access policy.</span></span>

> [!NOTE]
> <span data-ttu-id="a6df3-264">För närvarande måste en konto-SAS vara en ad hoc-SAS.</span><span class="sxs-lookup"><span data-stu-id="a6df3-264">Currently, an account SAS must be an ad hoc SAS.</span></span> <span data-ttu-id="a6df3-265">Lagrade åtkomst principer ännu inte stöds för konto-SAS.</span><span class="sxs-lookup"><span data-stu-id="a6df3-265">Stored access policies are not yet supported for account SAS.</span></span>

<span data-ttu-id="a6df3-266">Skillnaden mellan de två formulär är viktigt för ett key-scenario: återkallade certifikat.</span><span class="sxs-lookup"><span data-stu-id="a6df3-266">The difference between the two forms is important for one key scenario: revocation.</span></span> <span data-ttu-id="a6df3-267">Eftersom en SAS-URI är en URL, kan alla som erhåller SAS använda det, oavsett vem som skapade den.</span><span class="sxs-lookup"><span data-stu-id="a6df3-267">Because a SAS URI is a URL, anyone that obtains the SAS can use it, regardless of who originally created it.</span></span> <span data-ttu-id="a6df3-268">Om en SAS publiceras offentligt, kan den användas av vem som helst i världen.</span><span class="sxs-lookup"><span data-stu-id="a6df3-268">If a SAS is published publicly, it can be used by anyone in the world.</span></span> <span data-ttu-id="a6df3-269">En SAS ger åtkomst till resurser till alla som har den tills något av följande händer:</span><span class="sxs-lookup"><span data-stu-id="a6df3-269">A SAS grants access to resources to anyone possessing it until one of four things happens:</span></span>

1. <span data-ttu-id="a6df3-270">Förfallotiden som anges på SAS har nåtts.</span><span class="sxs-lookup"><span data-stu-id="a6df3-270">The expiry time specified on the SAS is reached.</span></span>
2. <span data-ttu-id="a6df3-271">Förfallotiden som angetts på den lagrade åtkomstprincip som refereras av SAS har uppnåtts (om en lagrad åtkomstprincip refereras, och det anger en förfallotiden).</span><span class="sxs-lookup"><span data-stu-id="a6df3-271">The expiry time specified on the stored access policy referenced by the SAS is reached (if a stored access policy is referenced, and if it specifies an expiry time).</span></span> <span data-ttu-id="a6df3-272">Detta kan inträffa eftersom intervallet långa eller eftersom du har ändrat lagrade åtkomstprincipen med en förfallotiden tidigare, vilket är ett sätt att återkalla SAS.</span><span class="sxs-lookup"><span data-stu-id="a6df3-272">This can occur either because the interval elapses, or because you've modified the stored access policy with an expiry time in the past, which is one way to revoke the SAS.</span></span>
3. <span data-ttu-id="a6df3-273">Lagrade åtkomstprincipen som refereras av SAS tas bort, vilket är ett annat sätt att återkalla SAS.</span><span class="sxs-lookup"><span data-stu-id="a6df3-273">The stored access policy referenced by the SAS is deleted, which is another way to revoke the SAS.</span></span> <span data-ttu-id="a6df3-274">Observera att om du återskapa den lagrade åtkomstprincipen med exakt samma namn, alla befintliga SAS-token igen är giltiga enligt de behörigheter som är kopplade till den lagrade åtkomstprincipen (förutsatt att som förfallotiden på SAS inte har passerats).</span><span class="sxs-lookup"><span data-stu-id="a6df3-274">Note that if you recreate the stored access policy with exactly the same name, all existing SAS tokens will again be valid according to the permissions associated with that stored access policy (assuming that the expiry time on the SAS has not passed).</span></span> <span data-ttu-id="a6df3-275">Om du avsikten är att återkalla SAS, måste du använda ett annat namn om du återskapa åtkomstprincipen med ett förfallodatum i framtiden.</span><span class="sxs-lookup"><span data-stu-id="a6df3-275">If you are intending to revoke the SAS, be sure to use a different name if you recreate the access policy with an expiry time in the future.</span></span>
4. <span data-ttu-id="a6df3-276">Nyckeln för kontot som används för att skapa SAS genereras.</span><span class="sxs-lookup"><span data-stu-id="a6df3-276">The account key that was used to create the SAS is regenerated.</span></span> <span data-ttu-id="a6df3-277">Återskapande av någon kontonyckel kommer alla programkomponenter som använder nyckeln för att kunna autentisera förrän de har uppdaterats för att använda den andra nyckeln i giltigt konto eller nyligen återskapats kontonyckel.</span><span class="sxs-lookup"><span data-stu-id="a6df3-277">Regenerating an account key will cause all application components using that key to fail to authenticate until they're updated to use either the other valid account key or the newly regenerated account key.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a6df3-278">En signatur för delad åtkomst URI som är associerad med kontonyckel som används för att skapa signaturen och den associerade lagras åtkomstprincip (eventuella).</span><span class="sxs-lookup"><span data-stu-id="a6df3-278">A shared access signature URI is associated with the account key used to create the signature, and the associated stored access policy (if any).</span></span> <span data-ttu-id="a6df3-279">Om inga lagrade åtkomstprincip anges är det enda sättet att återkalla en signatur för delad åtkomst att ändra nyckeln för kontot.</span><span class="sxs-lookup"><span data-stu-id="a6df3-279">If no stored access policy is specified, the only way to revoke a shared access signature is to change the account key.</span></span>

## <a name="authenticating-from-a-client-application-with-a-sas"></a><span data-ttu-id="a6df3-280">Autentisering från ett klientprogram med en SAS</span><span class="sxs-lookup"><span data-stu-id="a6df3-280">Authenticating from a client application with a SAS</span></span>
<span data-ttu-id="a6df3-281">En klient som innehar en SAS kan använda SAS för att autentisera en begäran mot ett lagringskonto som de inte har nycklar för kontot.</span><span class="sxs-lookup"><span data-stu-id="a6df3-281">A client who is in possession of a SAS can use the SAS to authenticate a request against a storage account for which they do not possess the account keys.</span></span> <span data-ttu-id="a6df3-282">En SAS kan ingå i en anslutningssträng eller användas direkt från lämplig konstruktor eller metod.</span><span class="sxs-lookup"><span data-stu-id="a6df3-282">A SAS can be included in a connection string, or used directly from the appropriate constructor or method.</span></span>

### <a name="using-a-sas-in-a-connection-string"></a><span data-ttu-id="a6df3-283">Med hjälp av en SAS i en anslutningssträng</span><span class="sxs-lookup"><span data-stu-id="a6df3-283">Using a SAS in a connection string</span></span>
[!INCLUDE [storage-use-sas-in-connection-string-include](../../includes/storage-use-sas-in-connection-string-include.md)]

### <a name="using-a-sas-in-a-constructor-or-method"></a><span data-ttu-id="a6df3-284">Med hjälp av en SAS i en konstruktor eller metod</span><span class="sxs-lookup"><span data-stu-id="a6df3-284">Using a SAS in a constructor or method</span></span>
<span data-ttu-id="a6df3-285">Flera Azure Storage client biblioteket konstruktorer och metoden överlagringar erbjuder en SAS-parameter, så att du kan autentisera en begäran till tjänsten med en SAS.</span><span class="sxs-lookup"><span data-stu-id="a6df3-285">Several Azure Storage client library constructors and method overloads offer a SAS parameter, so that you can authenticate a request to the service with a SAS.</span></span>

<span data-ttu-id="a6df3-286">Till exempel används här SAS-URI för att skapa en referens till en blockblobb.</span><span class="sxs-lookup"><span data-stu-id="a6df3-286">For example, here a SAS URI is used to create a reference to a block blob.</span></span> <span data-ttu-id="a6df3-287">SAS innehåller de enda autentiseringsuppgifter som krävs för begäran.</span><span class="sxs-lookup"><span data-stu-id="a6df3-287">The SAS provides the only credentials needed for the request.</span></span> <span data-ttu-id="a6df3-288">Blockblobben används sedan för en skrivåtgärd:</span><span class="sxs-lookup"><span data-stu-id="a6df3-288">The block blob reference is then used for a write operation:</span></span>

```csharp
string sasUri = "https://storagesample.blob.core.windows.net/sample-container/" +
    "sampleBlob.txt?sv=2015-07-08&sr=b&sig=39Up9JzHkxhUIhFEjEH9594DJxe7w6cIRCg0V6lCGSo%3D" +
    "&se=2016-10-18T21%3A51%3A37Z&sp=rcw";

CloudBlockBlob blob = new CloudBlockBlob(new Uri(sasUri));

// Create operation: Upload a blob with the specified name to the container.
// If the blob does not exist, it will be created. If it does exist, it will be overwritten.
try
{
    MemoryStream msWrite = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
    msWrite.Position = 0;
    using (msWrite)
    {
        await blob.UploadFromStreamAsync(msWrite);
    }

    Console.WriteLine("Create operation succeeded for SAS {0}", sasUri);
    Console.WriteLine();
}
catch (StorageException e)
{
    if (e.RequestInformation.HttpStatusCode == 403)
    {
        Console.WriteLine("Create operation failed for SAS {0}", sasUri);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }
    else
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}

```

## <a name="best-practices-when-using-sas"></a><span data-ttu-id="a6df3-289">Bästa metoder när du med hjälp av SAS</span><span class="sxs-lookup"><span data-stu-id="a6df3-289">Best practices when using SAS</span></span>
<span data-ttu-id="a6df3-290">När du använder signaturer för delad åtkomst i dina program, måste du vara medveten om två potentiella risker:</span><span class="sxs-lookup"><span data-stu-id="a6df3-290">When you use shared access signatures in your applications, you need to be aware of two potential risks:</span></span>

* <span data-ttu-id="a6df3-291">Om en SAS har läckts kan den användas av alla som erhåller, vilket potentiellt kan påverka ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="a6df3-291">If a SAS is leaked, it can be used by anyone who obtains it, which can potentially compromise your storage account.</span></span>
* <span data-ttu-id="a6df3-292">Om en SAS som ett klientprogram upphör att gälla och programmet kan inte hämta en ny SAS från din tjänst och programmets funktioner kan hindras.</span><span class="sxs-lookup"><span data-stu-id="a6df3-292">If a SAS provided to a client application expires and the application is unable to retrieve a new SAS from your service, then the application's functionality may be hindered.</span></span>

<span data-ttu-id="a6df3-293">Följande rekommendationer för att använda signaturer för delad åtkomst kan minska riskerna:</span><span class="sxs-lookup"><span data-stu-id="a6df3-293">The following recommendations for using shared access signatures can help mitigate these risks:</span></span>

1. <span data-ttu-id="a6df3-294">**Använder alltid HTTPS** att skapa eller distribuera en SAS.</span><span class="sxs-lookup"><span data-stu-id="a6df3-294">**Always use HTTPS** to create or distribute a SAS.</span></span> <span data-ttu-id="a6df3-295">Om en SAS skickas via HTTP och snappas upp, är en obehörig som utför en man-in-the-middle-attacker kan läsa SAS och sedan använda den precis som den avsedda användaren kan ha potentiellt kompromettera känsliga data eller så att data skadas av skadliga användare.</span><span class="sxs-lookup"><span data-stu-id="a6df3-295">If a SAS is passed over HTTP and intercepted, an attacker performing a man-in-the-middle attack is able to read the SAS and then use it just as the intended user could have, potentially compromising sensitive data or allowing for data corruption by the malicious user.</span></span>
2. <span data-ttu-id="a6df3-296">**Referens lagrad åtkomstprinciper där det är möjligt.**</span><span class="sxs-lookup"><span data-stu-id="a6df3-296">**Reference stored access policies where possible.**</span></span> <span data-ttu-id="a6df3-297">Lagrade åtkomstprinciper ger dig möjlighet att återkalla behörigheter utan att behöva återskapa lagringskontonycklar.</span><span class="sxs-lookup"><span data-stu-id="a6df3-297">Stored access policies give you the option to revoke permissions without having to regenerate the storage account keys.</span></span> <span data-ttu-id="a6df3-298">Ange upphör att gälla på dessa mycket långt fram i tiden (eller oändlig) och kontrollera att uppdateras regelbundet flyttas längre i framtiden.</span><span class="sxs-lookup"><span data-stu-id="a6df3-298">Set the expiration on these very far in the future (or infinite) and make sure it's regularly updated to move it farther into the future.</span></span>
3. <span data-ttu-id="a6df3-299">**Använd kortsiktig giltighetstid gånger på en ad hoc-SAS.**</span><span class="sxs-lookup"><span data-stu-id="a6df3-299">**Use near-term expiration times on an ad hoc SAS.**</span></span> <span data-ttu-id="a6df3-300">På så sätt kan även om en SAS äventyras, är det bara giltig för en kort tid.</span><span class="sxs-lookup"><span data-stu-id="a6df3-300">In this way, even if a SAS is compromised, it's valid only for a short time.</span></span> <span data-ttu-id="a6df3-301">Den här övningen är särskilt viktigt om du inte kan referera till en princip för lagrade åtkomst.</span><span class="sxs-lookup"><span data-stu-id="a6df3-301">This practice is especially important if you cannot reference a stored access policy.</span></span> <span data-ttu-id="a6df3-302">Kortsiktig giltighetstid gånger också begränsa mängden data som kan skrivas till en blobb genom att begränsa tid som är tillgängliga att överföra till den.</span><span class="sxs-lookup"><span data-stu-id="a6df3-302">Near-term expiration times also limit the amount of data that can be written to a blob by limiting the time available to upload to it.</span></span>
4. <span data-ttu-id="a6df3-303">**Ha klienter automatiskt förnya SAS om det behövs.**</span><span class="sxs-lookup"><span data-stu-id="a6df3-303">**Have clients automatically renew the SAS if necessary.**</span></span> <span data-ttu-id="a6df3-304">Klienter måste förnya SAS före förfallodatum, för att ge tid för nya försök om att tillhandahålla SAS-tjänsten är inte tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="a6df3-304">Clients should renew the SAS well before the expiration, in order to allow time for retries if the service providing the SAS is unavailable.</span></span> <span data-ttu-id="a6df3-305">Om din SAS är avsedd att användas för ett litet antal omedelbara och tillfällig åtgärder som förväntas slutföras inom den förfallotid perioden, sedan kan det vara onödiga som SAS inte förväntas förnyas.</span><span class="sxs-lookup"><span data-stu-id="a6df3-305">If your SAS is meant to be used for a small number of immediate, short-lived operations that are expected to be completed within the expiration period, then this may be unnecessary as the SAS is not expected to be renewed.</span></span> <span data-ttu-id="a6df3-306">Men om du har klienten som regelbundet göra begäranden via SAS kommer sedan möjligheten att upphör att gälla in i play.</span><span class="sxs-lookup"><span data-stu-id="a6df3-306">However, if you have client that is routinely making requests via SAS, then the possibility of expiration comes into play.</span></span> <span data-ttu-id="a6df3-307">Nyckelfaktor är att balansera behovet av att vara tillfällig SAS (som tidigare nämnts anger) med behovet av att säkerställa att klienten begär förnyelse tidigt nog (för att undvika störningar på grund av SAS ut före lyckade förnyelse).</span><span class="sxs-lookup"><span data-stu-id="a6df3-307">The key consideration is to balance the need for the SAS to be short-lived (as previously stated) with the need to ensure that the client is requesting renewal early enough (to avoid disruption due to the SAS expiring prior to successful renewal).</span></span>
5. <span data-ttu-id="a6df3-308">**Var försiktig med SAS starttiden.**</span><span class="sxs-lookup"><span data-stu-id="a6df3-308">**Be careful with SAS start time.**</span></span> <span data-ttu-id="a6df3-309">Om du anger starttiden för en SAS för **nu**, sedan skeva (skillnader i aktuell tid enligt olika datorer) på grund av klockan, fel kan observeras ibland för först få minuter.</span><span class="sxs-lookup"><span data-stu-id="a6df3-309">If you set the start time for a SAS to **now**, then due to clock skew (differences in current time according to different machines), failures may be observed intermittently for the first few minutes.</span></span> <span data-ttu-id="a6df3-310">I allmänhet Ange starttid ska vara minst 15 minuter i förflutna.</span><span class="sxs-lookup"><span data-stu-id="a6df3-310">In general, set the start time to be at least 15 minutes in the past.</span></span> <span data-ttu-id="a6df3-311">Eller inte anger den, vilket gör det giltiga omedelbart i samtliga fall.</span><span class="sxs-lookup"><span data-stu-id="a6df3-311">Or, don't set it at all, which will make it valid immediately in all cases.</span></span> <span data-ttu-id="a6df3-312">Samma gäller vanligtvis förfallotiden samt--Kom ihåg att du kan se upp till 15 minuter klockan skeva i vardera riktning på varje begäran.</span><span class="sxs-lookup"><span data-stu-id="a6df3-312">The same generally applies to expiry time as well--remember that you may observe up to 15 minutes of clock skew in either direction on any request.</span></span> <span data-ttu-id="a6df3-313">För klienter som använder en REST-version innan 2012-02-12, är maximal varaktighet för en Signatur som inte refererar till en lagrad åtkomstprincip 1 timme och policyer för att ange längre sikt än vad som kommer att misslyckas.</span><span class="sxs-lookup"><span data-stu-id="a6df3-313">For clients using a REST version prior to 2012-02-12, the maximum duration for a SAS that does not reference a stored access policy is 1 hour, and any policies specifying longer term than that will fail.</span></span>
6. <span data-ttu-id="a6df3-314">**Måste vara specifikt med resursen som ska användas.**</span><span class="sxs-lookup"><span data-stu-id="a6df3-314">**Be specific with the resource to be accessed.**</span></span> <span data-ttu-id="a6df3-315">Av säkerhetsskäl är att ge en användare de minsta behörigheter som krävs.</span><span class="sxs-lookup"><span data-stu-id="a6df3-315">A security best practice is to provide a user with the minimum required privileges.</span></span> <span data-ttu-id="a6df3-316">Om en användare behöver bara läsbehörighet till en enda enhet, ge dem tillgång till läsåtkomst till den enda entiteten och inte läsa/skriva och ta bort åtkomst till alla enheter.</span><span class="sxs-lookup"><span data-stu-id="a6df3-316">If a user only needs read access to a single entity, then grant them read access to that single entity, and not read/write/delete access to all entities.</span></span> <span data-ttu-id="a6df3-317">Det hjälper även minska skadan om en SAS äventyras eftersom SAS har mindre ström i händerna på en angripare.</span><span class="sxs-lookup"><span data-stu-id="a6df3-317">This also helps lessen the damage if a SAS is compromised because the SAS has less power in the hands of an attacker.</span></span>
7. <span data-ttu-id="a6df3-318">**Förstå att ditt konto kommer att debiteras för användning, inklusive göra med SAS.**</span><span class="sxs-lookup"><span data-stu-id="a6df3-318">**Understand that your account will be billed for any usage, including that done with SAS.**</span></span> <span data-ttu-id="a6df3-319">Om du anger skrivåtkomst till en blob kan kan en användare välja att överföra en blob 200GB.</span><span class="sxs-lookup"><span data-stu-id="a6df3-319">If you provide write access to a blob, a user may choose to upload a 200GB blob.</span></span> <span data-ttu-id="a6df3-320">Om du har gett dem samt läsbehörighet, kan de välja att hämta den 10 gånger medför 2 TB i kostnader för nätverksegress för dig.</span><span class="sxs-lookup"><span data-stu-id="a6df3-320">If you've given them read access as well, they may choose to download it 10 times, incurring 2 TB in egress costs for you.</span></span> <span data-ttu-id="a6df3-321">Igen, ange begränsade behörigheter för att minska de potentiella åtgärderna av obehöriga användare.</span><span class="sxs-lookup"><span data-stu-id="a6df3-321">Again, provide limited permissions to help mitigate the potential actions of malicious users.</span></span> <span data-ttu-id="a6df3-322">Använd tillfällig SAS för att minska hotet (men Tänk också på klockavvikelser på sluttid).</span><span class="sxs-lookup"><span data-stu-id="a6df3-322">Use short-lived SAS to reduce this threat (but be mindful of clock skew on the end time).</span></span>
8. <span data-ttu-id="a6df3-323">**Validera data som skrivs med hjälp av SAS.**</span><span class="sxs-lookup"><span data-stu-id="a6df3-323">**Validate data written using SAS.**</span></span> <span data-ttu-id="a6df3-324">När ett klientprogram skriver data till ditt lagringskonto, Kom ihåg att det kan finnas problem med dessa data.</span><span class="sxs-lookup"><span data-stu-id="a6df3-324">When a client application writes data to your storage account, keep in mind that there can be problems with that data.</span></span> <span data-ttu-id="a6df3-325">Om ditt program kräver att dessa data att verifieras eller behörighet innan den är klar att använda, bör du utföra verifieringen när data skrivs och innan den används av ditt program.</span><span class="sxs-lookup"><span data-stu-id="a6df3-325">If your application requires that that data be validated or authorized before it is ready to use, you should perform this validation after the data is written and before it is used by your application.</span></span> <span data-ttu-id="a6df3-326">Den här övningen även skyddar mot skadad eller skadliga data skrivs till ditt konto, antingen av en användare som korrekt förvärvade SAS eller av en användare som utnyttjar en känd SAS.</span><span class="sxs-lookup"><span data-stu-id="a6df3-326">This practice also protects against corrupt or malicious data being written to your account, either by a user who properly acquired the SAS, or by a user exploiting a leaked SAS.</span></span>
9. <span data-ttu-id="a6df3-327">**Inte alltid att använda SAS.**</span><span class="sxs-lookup"><span data-stu-id="a6df3-327">**Don't always use SAS.**</span></span> <span data-ttu-id="a6df3-328">Ibland uppväger risker med en viss åtgärd mot ditt lagringskonto fördelarna med SAS.</span><span class="sxs-lookup"><span data-stu-id="a6df3-328">Sometimes the risks associated with a particular operation against your storage account outweigh the benefits of SAS.</span></span> <span data-ttu-id="a6df3-329">Skapa en mellannivå-tjänst som skriver till ditt lagringskonto efter att ha genomfört företag för dessa åtgärder regel verifiering, autentisering och granskning.</span><span class="sxs-lookup"><span data-stu-id="a6df3-329">For such operations, create a middle-tier service that writes to your storage account after performing business rule validation, authentication, and auditing.</span></span> <span data-ttu-id="a6df3-330">Dessutom är ibland det enklare att hantera åtkomst på annat sätt.</span><span class="sxs-lookup"><span data-stu-id="a6df3-330">Also, sometimes it's simpler to manage access in other ways.</span></span> <span data-ttu-id="a6df3-331">Till exempel om du vill se alla blobbar i en behållare kan läsas offentligt du behållaren allmänheten, i stället för att tillhandahålla en SAS till alla klienter för åtkomst.</span><span class="sxs-lookup"><span data-stu-id="a6df3-331">For example, if you want to make all blobs in a container publically readable, you can make the container Public, rather than providing a SAS to every client for access.</span></span>
10. <span data-ttu-id="a6df3-332">**Använd Storage Analytics för övervakning av programmet.**</span><span class="sxs-lookup"><span data-stu-id="a6df3-332">**Use Storage Analytics to monitor your application.**</span></span> <span data-ttu-id="a6df3-333">Du kan använda loggning och mått om du vill se alla topp i antal autentiseringsfel på grund av ett avbrott i tjänsten din SAS-provider eller oavsiktlig borttagning av en princip för lagrade åtkomst.</span><span class="sxs-lookup"><span data-stu-id="a6df3-333">You can use logging and metrics to observe any spike in authentication failures due to an outage in your SAS provider service or to the inadvertent removal of a stored access policy.</span></span> <span data-ttu-id="a6df3-334">Finns det [Azure Storage-teamets blogg](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/08/03/windows-azure-storage-logging-using-logs-to-track-storage-requests.aspx) för ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="a6df3-334">See the [Azure Storage Team Blog](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/08/03/windows-azure-storage-logging-using-logs-to-track-storage-requests.aspx) for additional information.</span></span>

## <a name="sas-examples"></a><span data-ttu-id="a6df3-335">SAS-exempel</span><span class="sxs-lookup"><span data-stu-id="a6df3-335">SAS examples</span></span>
<span data-ttu-id="a6df3-336">Nedan följer några exempel på båda typerna av signaturer för delad åtkomst, konto-SAS och tjänst-SAS.</span><span class="sxs-lookup"><span data-stu-id="a6df3-336">Below are some examples of both types of shared access signatures, account SAS and service SAS.</span></span>

<span data-ttu-id="a6df3-337">Om du vill köra dessa C#-exempel, måste du referera till följande NuGet-paketen i projektet:</span><span class="sxs-lookup"><span data-stu-id="a6df3-337">To run these C# examples, you need to reference the following NuGet packages in your project:</span></span>

* <span data-ttu-id="a6df3-338">[Azure Storage-klientbibliotek för .NET](http://www.nuget.org/packages/WindowsAzure.Storage), version 6.x eller senare (för att använda kontot SAS).</span><span class="sxs-lookup"><span data-stu-id="a6df3-338">[Azure Storage Client Library for .NET](http://www.nuget.org/packages/WindowsAzure.Storage), version 6.x or later (to use account SAS).</span></span>
* [<span data-ttu-id="a6df3-339">Azure Configuration Manager</span><span class="sxs-lookup"><span data-stu-id="a6df3-339">Azure Configuration Manager</span></span>](http://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager)

<span data-ttu-id="a6df3-340">Ytterligare exempel som visar hur du skapar och testar en SAS finns [Azure kodexempel för lagring](https://azure.microsoft.com/documentation/samples/?service=storage).</span><span class="sxs-lookup"><span data-stu-id="a6df3-340">For additional examples that show how to create and test a SAS, see [Azure Code Samples for Storage](https://azure.microsoft.com/documentation/samples/?service=storage).</span></span>

### <a name="example-create-and-use-an-account-sas"></a><span data-ttu-id="a6df3-341">Exempel: Skapa och använda en konto-SAS</span><span class="sxs-lookup"><span data-stu-id="a6df3-341">Example: Create and use an account SAS</span></span>
<span data-ttu-id="a6df3-342">Följande exempel skapar ett konto SA som är giltig för Blob och Filtjänster och ger klienten behörigheterna Läs-, Skriv- och listbehörigheter servicenivå API: er.</span><span class="sxs-lookup"><span data-stu-id="a6df3-342">The following code example creates an account SAS that is valid for the Blob and File services, and gives the client permissions read, write, and list permissions to access service-level APIs.</span></span> <span data-ttu-id="a6df3-343">Konto-SAS begränsar protokollet till HTTPS, så begäran måste göras med HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a6df3-343">The account SAS restricts the protocol to HTTPS, so the request must be made with HTTPS.</span></span>

```csharp
static string GetAccountSASToken()
{
    // To create the account SAS, you need to use your shared key credentials. Modify for your account.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

    // Create a new access policy for the account.
    SharedAccessAccountPolicy policy = new SharedAccessAccountPolicy()
        {
            Permissions = SharedAccessAccountPermissions.Read | SharedAccessAccountPermissions.Write | SharedAccessAccountPermissions.List,
            Services = SharedAccessAccountServices.Blob | SharedAccessAccountServices.File,
            ResourceTypes = SharedAccessAccountResourceTypes.Service,
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Protocols = SharedAccessProtocol.HttpsOnly
        };

    // Return the SAS token.
    return storageAccount.GetSharedAccessSignature(policy);
}
```

<span data-ttu-id="a6df3-344">Skapa ett objekt för Blob med hjälp av SAS- och slutpunkt för Blob-lagring för lagringskontot för att använda kontot SAS servicenivå API: er för Blob-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a6df3-344">To use the account SAS to access service-level APIs for the Blob service, construct a Blob client object using the SAS and the Blob storage endpoint for your storage account.</span></span>

```csharp
static void UseAccountSAS(string sasToken)
{
    // Create new storage credentials using the SAS token.
    StorageCredentials accountSAS = new StorageCredentials(sasToken);
    // Use these credentials and the account name to create a Blob service client.
    CloudStorageAccount accountWithSAS = new CloudStorageAccount(accountSAS, "account-name", endpointSuffix: null, useHttps: true);
    CloudBlobClient blobClientWithSAS = accountWithSAS.CreateCloudBlobClient();

    // Now set the service properties for the Blob client created with the SAS.
    blobClientWithSAS.SetServiceProperties(new ServiceProperties()
    {
        HourMetrics = new MetricsProperties()
        {
            MetricsLevel = MetricsLevel.ServiceAndApi,
            RetentionDays = 7,
            Version = "1.0"
        },
        MinuteMetrics = new MetricsProperties()
        {
            MetricsLevel = MetricsLevel.ServiceAndApi,
            RetentionDays = 7,
            Version = "1.0"
        },
        Logging = new LoggingProperties()
        {
            LoggingOperations = LoggingOperations.All,
            RetentionDays = 14,
            Version = "1.0"
        }
    });

    // The permissions granted by the account SAS also permit you to retrieve service properties.
    ServiceProperties serviceProperties = blobClientWithSAS.GetServiceProperties();
    Console.WriteLine(serviceProperties.HourMetrics.MetricsLevel);
    Console.WriteLine(serviceProperties.HourMetrics.RetentionDays);
    Console.WriteLine(serviceProperties.HourMetrics.Version);
}
```

### <a name="example-create-a-stored-access-policy"></a><span data-ttu-id="a6df3-345">Exempel: Skapa en princip för lagrade åtkomst</span><span class="sxs-lookup"><span data-stu-id="a6df3-345">Example: Create a stored access policy</span></span>
<span data-ttu-id="a6df3-346">Följande kod skapar en åtkomstprincip som är lagrade på en behållare.</span><span class="sxs-lookup"><span data-stu-id="a6df3-346">The following code creates a stored access policy on a container.</span></span> <span data-ttu-id="a6df3-347">Du kan använda för att ange begränsningar för en tjänst-SAS på behållaren eller dess blobbar.</span><span class="sxs-lookup"><span data-stu-id="a6df3-347">You can use the access policy to specify constraints for a service SAS on the container or its blobs.</span></span>

```csharp
private static async Task CreateSharedAccessPolicyAsync(CloudBlobContainer container, string policyName)
{
    // Create a new shared access policy and define its constraints.
    // The access policy provides create, write, read, list, and delete permissions.
    SharedAccessBlobPolicy sharedPolicy = new SharedAccessBlobPolicy()
    {
        // When the start time for the SAS is omitted, the start time is assumed to be the time when the storage service receives the request.
        // Omitting the start time for a SAS that is effective immediately helps to avoid clock skew.
        SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
        Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.List |
            SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Create | SharedAccessBlobPermissions.Delete
    };

    // Get the container's existing permissions.
    BlobContainerPermissions permissions = await container.GetPermissionsAsync();

    // Add the new policy to the container's permissions, and set the container's permissions.
    permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
    await container.SetPermissionsAsync(permissions);
}
```

### <a name="example-create-a-service-sas-on-a-container"></a><span data-ttu-id="a6df3-348">Exempel: Skapa en tjänst-SAS på en behållare</span><span class="sxs-lookup"><span data-stu-id="a6df3-348">Example: Create a service SAS on a container</span></span>
<span data-ttu-id="a6df3-349">Följande kod skapar en SAS för en behållare.</span><span class="sxs-lookup"><span data-stu-id="a6df3-349">The following code creates a SAS on a container.</span></span> <span data-ttu-id="a6df3-350">Om namnet på en befintlig lagrade åtkomstprincip är principen associerad med SAS.</span><span class="sxs-lookup"><span data-stu-id="a6df3-350">If the name of an existing stored access policy is provided, that policy is associated with the SAS.</span></span> <span data-ttu-id="a6df3-351">Om inga lagrade åtkomstprincip skapar koden sedan en ad hoc-SAS på behållaren.</span><span class="sxs-lookup"><span data-stu-id="a6df3-351">If no stored access policy is provided, then the code creates an ad-hoc SAS on the container.</span></span>

```csharp
private static string GetContainerSasUri(CloudBlobContainer container, string storedPolicyName = null)
{
    string sasContainerToken;

    // If no stored policy is specified, create a new access policy and define its constraints.
    if (storedPolicyName == null)
    {
        // Note that the SharedAccessBlobPolicy class is used both to define the parameters of an ad-hoc SAS, and
        // to construct a shared access policy that is saved to the container's shared access policies.
        SharedAccessBlobPolicy adHocPolicy = new SharedAccessBlobPolicy()
        {
            // When the start time for the SAS is omitted, the start time is assumed to be the time when the storage service receives the request.
            // Omitting the start time for a SAS that is effective immediately helps to avoid clock skew.
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List
        };

        // Generate the shared access signature on the container, setting the constraints directly on the signature.
        sasContainerToken = container.GetSharedAccessSignature(adHocPolicy, null);

        Console.WriteLine("SAS for blob container (ad hoc): {0}", sasContainerToken);
        Console.WriteLine();
    }
    else
    {
        // Generate the shared access signature on the container. In this case, all of the constraints for the
        // shared access signature are specified on the stored access policy, which is provided by name.
        // It is also possible to specify some constraints on an ad-hoc SAS and others on the stored access policy.
        sasContainerToken = container.GetSharedAccessSignature(null, storedPolicyName);

        Console.WriteLine("SAS for blob container (stored access policy): {0}", sasContainerToken);
        Console.WriteLine();
    }

    // Return the URI string for the container, including the SAS token.
    return container.Uri + sasContainerToken;
}
```

### <a name="example-create-a-service-sas-on-a-blob"></a><span data-ttu-id="a6df3-352">Exempel: Skapa en tjänst-SAS på en blob</span><span class="sxs-lookup"><span data-stu-id="a6df3-352">Example: Create a service SAS on a blob</span></span>
<span data-ttu-id="a6df3-353">Följande kod skapar en SAS för en blob.</span><span class="sxs-lookup"><span data-stu-id="a6df3-353">The following code creates a SAS on a blob.</span></span> <span data-ttu-id="a6df3-354">Om namnet på en befintlig lagrade åtkomstprincip är principen associerad med SAS.</span><span class="sxs-lookup"><span data-stu-id="a6df3-354">If the name of an existing stored access policy is provided, that policy is associated with the SAS.</span></span> <span data-ttu-id="a6df3-355">Om inga lagrade åtkomstprincip skapar koden en ad hoc-SAS på blob.</span><span class="sxs-lookup"><span data-stu-id="a6df3-355">If no stored access policy is provided, then the code creates an ad-hoc SAS on the blob.</span></span>

```csharp
private static string GetBlobSasUri(CloudBlobContainer container, string blobName, string policyName = null)
{
    string sasBlobToken;

    // Get a reference to a blob within the container.
    // Note that the blob may not exist yet, but a SAS can still be created for it.
    CloudBlockBlob blob = container.GetBlockBlobReference(blobName);

    if (policyName == null)
    {
        // Create a new access policy and define its constraints.
        // Note that the SharedAccessBlobPolicy class is used both to define the parameters of an ad-hoc SAS, and
        // to construct a shared access policy that is saved to the container's shared access policies.
        SharedAccessBlobPolicy adHocSAS = new SharedAccessBlobPolicy()
        {
            // When the start time for the SAS is omitted, the start time is assumed to be the time when the storage service receives the request.
            // Omitting the start time for a SAS that is effective immediately helps to avoid clock skew.
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Create
        };

        // Generate the shared access signature on the blob, setting the constraints directly on the signature.
        sasBlobToken = blob.GetSharedAccessSignature(adHocSAS);

        Console.WriteLine("SAS for blob (ad hoc): {0}", sasBlobToken);
        Console.WriteLine();
    }
    else
    {
        // Generate the shared access signature on the blob. In this case, all of the constraints for the
        // shared access signature are specified on the container's stored access policy.
        sasBlobToken = blob.GetSharedAccessSignature(null, policyName);

        Console.WriteLine("SAS for blob (stored access policy): {0}", sasBlobToken);
        Console.WriteLine();
    }

    // Return the URI string for the container, including the SAS token.
    return blob.Uri + sasBlobToken;
}
```

## <a name="conclusion"></a><span data-ttu-id="a6df3-356">Slutsats</span><span class="sxs-lookup"><span data-stu-id="a6df3-356">Conclusion</span></span>
<span data-ttu-id="a6df3-357">Signaturer för delad åtkomst är användbara för att tillhandahålla begränsade behörigheter till ditt lagringskonto till klienter som inte ska ha kontonyckel.</span><span class="sxs-lookup"><span data-stu-id="a6df3-357">Shared access signatures are useful for providing limited permissions to your storage account to clients that should not have the account key.</span></span> <span data-ttu-id="a6df3-358">Därför att de är en viktig del av säkerhetsmodellen för alla program som använder Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="a6df3-358">As such, they are a vital part of the security model for any application using Azure Storage.</span></span> <span data-ttu-id="a6df3-359">Om du följer bästa praxis som anges här kan du använda SAS för större flexibilitet för åtkomst till resurser i ditt lagringskonto, utan att kompromettera säkerheten för ditt program.</span><span class="sxs-lookup"><span data-stu-id="a6df3-359">If you follow the best practices listed here, you can use SAS to provide greater flexibility of access to resources in your storage account, without compromising the security of your application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a6df3-360">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a6df3-360">Next Steps</span></span>
* [<span data-ttu-id="a6df3-361">Hantera anonym läsbehörighet till behållare och blobbar</span><span class="sxs-lookup"><span data-stu-id="a6df3-361">Manage anonymous read access to containers and blobs</span></span>](storage-manage-access-to-resources.md)
* [<span data-ttu-id="a6df3-362">Delegera åtkomst med signatur för delad åtkomst</span><span class="sxs-lookup"><span data-stu-id="a6df3-362">Delegating Access with a Shared Access Signature</span></span>](http://msdn.microsoft.com/library/azure/ee395415.aspx)
* [<span data-ttu-id="a6df3-363">Introduktion till tabellen och kön SAS</span><span class="sxs-lookup"><span data-stu-id="a6df3-363">Introducing Table and Queue SAS</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx)

[sas-storage-fe-proxy-service]: ./media/storage-dotnet-shared-access-signature-part-1/sas-storage-fe-proxy-service.png
[sas-storage-provider-service]: ./media/storage-dotnet-shared-access-signature-part-1/sas-storage-provider-service.png
[sas-storage-uri]: ./media/storage-dotnet-shared-access-signature-part-1/sas-storage-uri.png
