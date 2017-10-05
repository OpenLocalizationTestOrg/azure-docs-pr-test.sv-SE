---
title: "Importera och exportera en domän zonfil till Azure DNS med hjälp av Azure CLI 1.0 | Microsoft Docs"
description: "Lär dig hur du importerar och exporterar en DNS-zonfilen till Azure DNS med hjälp av Azure CLI 1.0"
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: f5797782-3005-4663-a488-ac0089809010
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2016
ms.author: gwallace
ms.openlocfilehash: d6d3fa7aa0e8b2462b3a6b4b66d3d87ab5535314
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="import-and-export-a-dns-zone-file-using-the-azure-cli-10"></a><span data-ttu-id="cd9cb-103">Importera och exportera en DNS-zonfilen som använder Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="cd9cb-103">Import and export a DNS zone file using the Azure CLI 1.0</span></span> 

<span data-ttu-id="cd9cb-104">Den här artikeln får du veta hur du importerar och exporterar DNS-zon för filerna för Azure DNS med hjälp av Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-104">This article walks you through how to import and export DNS zone files for Azure DNS using the Azure CLI 1.0.</span></span>

## <a name="introduction-to-dns-zone-migration"></a><span data-ttu-id="cd9cb-105">Introduktion till DNS-zonen migrering</span><span class="sxs-lookup"><span data-stu-id="cd9cb-105">Introduction to DNS zone migration</span></span>

<span data-ttu-id="cd9cb-106">En DNS-zonfilen är en textfil som innehåller information om alla Domain Name System (DNS)-poster i zonen.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-106">A DNS zone file is a text file that contains details of every Domain Name System (DNS) record in the zone.</span></span> <span data-ttu-id="cd9cb-107">Här följer ett standardformat, vilket gör den lämplig för överföring av DNS-poster mellan DNS-system.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-107">It follows a standard format, making it suitable for transferring DNS records between DNS systems.</span></span> <span data-ttu-id="cd9cb-108">Med hjälp av en zonfil är en snabb, tillförlitlig och enkelt sätt att överföra en DNS-zon i eller utanför Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-108">Using a zone file is a quick, reliable, and convenient way to transfer a DNS zone into or out of Azure DNS.</span></span>

<span data-ttu-id="cd9cb-109">Azure DNS stöder import och export av filer med hjälp av Azure-kommandoradsgränssnittet (CLI).</span><span class="sxs-lookup"><span data-stu-id="cd9cb-109">Azure DNS supports importing and exporting zone files by using the Azure command-line interface (CLI).</span></span> <span data-ttu-id="cd9cb-110">Zonen filimport är **inte** stöds för närvarande via Azure PowerShell eller Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-110">Zone file import is **not** currently supported via Azure PowerShell or the Azure portal.</span></span>

<span data-ttu-id="cd9cb-111">Azure CLI 1.0 är ett kommandoradsverktyg för flera plattformar som används för att hantera Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-111">The Azure CLI 1.0 is a cross-platform command-line tool used for managing Azure services.</span></span> <span data-ttu-id="cd9cb-112">Den är tillgänglig för Windows, Mac och Linux-plattformarna från den [Azure nedladdningar](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="cd9cb-112">It is available for the Windows, Mac, and Linux platforms from the [Azure downloads page](https://azure.microsoft.com/downloads/).</span></span> <span data-ttu-id="cd9cb-113">Plattformsoberoende stöd är viktigt för att importera och exportera filer, eftersom namnet vanligaste serverprogrammet [BINDA](https://www.isc.org/downloads/bind/), vanligtvis körs på Linux.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-113">Cross-platform support is important for importing and exporting zone files, because the most common name server software, [BIND](https://www.isc.org/downloads/bind/), typically runs on Linux.</span></span>

> [!NOTE]
> <span data-ttu-id="cd9cb-114">Det finns två versioner av Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-114">There are currently two versions of the Azure CLI.</span></span> <span data-ttu-id="cd9cb-115">CLI1.0 baseras på Node.js och har kommandon som börjar med ”azure”.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-115">CLI1.0 is based on Node.js, and has commands beginning with "azure".</span></span>
> <span data-ttu-id="cd9cb-116">CLI2.0 baseras på Python och har kommandon som börjar med ”az”.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-116">CLI2.0 is based on Python and has commands beginning with "az".</span></span> <span data-ttu-id="cd9cb-117">När zonen filimport stöds i båda versionerna, bör du använda kommandona CLI1.0 som beskrivs i den här sidan.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-117">While zone file import is supported in both versions, we recommend using the CLI1.0 commands, as described in this page.</span></span>

## <a name="obtain-your-existing-dns-zone-file"></a><span data-ttu-id="cd9cb-118">Hämta din befintliga DNS-zonfilen</span><span class="sxs-lookup"><span data-stu-id="cd9cb-118">Obtain your existing DNS zone file</span></span>

<span data-ttu-id="cd9cb-119">Du måste ha en kopia av filen zonen innan du importerar en DNS-zonfilen till Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-119">Before you import a DNS zone file into Azure DNS, you need to obtain a copy of the zone file.</span></span> <span data-ttu-id="cd9cb-120">Källan för den här filen är beroende av DNS-zon som finns för närvarande.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-120">The source of this file depends on where the DNS zone is currently hosted.</span></span>

* <span data-ttu-id="cd9cb-121">Om DNS-zonen finns av en partner-tjänst (till exempel en domänregistrator, dedikerad DNS-värdtjänsten eller alternativa molnprovider), ska tjänsten ger möjlighet att hämta DNS-zonfilen.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-121">If your DNS zone is hosted by a partner service (such as a domain registrar, dedicated DNS hosting provider, or alternative cloud provider), that service should provide the ability to download the DNS zone file.</span></span>
* <span data-ttu-id="cd9cb-122">Om DNS-zonen finns på Windows DNS standardmappen för zonen filerna är **%systemroot%\system32\dns**.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-122">If your DNS zone is hosted on Windows DNS, the default folder for the zone files is **%systemroot%\system32\dns**.</span></span> <span data-ttu-id="cd9cb-123">Den fullständiga sökvägen till en fil för varje zon visas också på den **allmänna** för DNS-konsolen.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-123">The full path to each zone file also shows on the **General** tab of the DNS console.</span></span>
* <span data-ttu-id="cd9cb-124">Om DNS-zonen finns med hjälp av BIND, platsen för zonfilen för varje zon har angetts i konfigurationsfilen BIND **named.conf**.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-124">If your DNS zone is hosted by using BIND, the location of the zone file for each zone is specified in the BIND configuration file **named.conf**.</span></span>

> [!NOTE]
> <span data-ttu-id="cd9cb-125">Zonen filer som hämtas från GoDaddy har ett avvikande något format.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-125">Zone files downloaded from GoDaddy have a slightly nonstandard format.</span></span> <span data-ttu-id="cd9cb-126">Du behöver åtgärda detta innan du importerar dessa filer till Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-126">You need to correct this before you import these zone files into Azure DNS.</span></span>
>
> <span data-ttu-id="cd9cb-127">DNS-namn i RDATA för varje DNS-posten har angetts som ett fullständigt kvalificerat namn, men de inte har en avslutande ””. Det innebär att de tolkas av andra DNS-system som relativa namn.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-127">DNS names in the RDATA of each DNS record are specified as fully qualified names, but they don't have a terminating "." This means they are interpreted by other DNS systems as relative names.</span></span> <span data-ttu-id="cd9cb-128">Du måste redigera zonfilen för att lägga till den avslutande ””. i sina namn innan du importerar dem till Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-128">You need to edit the zone file to append the terminating "." to their names before you import them into Azure DNS.</span></span>
>
> <span data-ttu-id="cd9cb-129">Till exempel bör CNAME-posten ”www 3600 i CNAME contoso.com” ändras till ”www 3600 i CNAME contoso.com”.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-129">For example, the CNAME record "www 3600 IN CNAME contoso.com" should be changed to "www 3600 IN CNAME contoso.com."</span></span>
> <span data-ttu-id="cd9cb-130">(med en avslutande ””.).</span><span class="sxs-lookup"><span data-stu-id="cd9cb-130">(with a terminating ".").</span></span>

## <a name="import-a-dns-zone-file-into-azure-dns"></a><span data-ttu-id="cd9cb-131">Importera en DNS-zonfilen till Azure DNS</span><span class="sxs-lookup"><span data-stu-id="cd9cb-131">Import a DNS zone file into Azure DNS</span></span>

<span data-ttu-id="cd9cb-132">Om du importerar en zonfil skapas en ny zon i Azure DNS om det inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-132">Importing a zone file creates a new zone in Azure DNS if one does not already exist.</span></span> <span data-ttu-id="cd9cb-133">Om zonen redan slås postuppsättningar i zonen filen samman med de befintliga postuppsättningar.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-133">If the zone already exists, the record sets in the zone file must be merged with the existing record sets.</span></span>

### <a name="merge-behavior"></a><span data-ttu-id="cd9cb-134">Sammanfoga beteende</span><span class="sxs-lookup"><span data-stu-id="cd9cb-134">Merge behavior</span></span>

* <span data-ttu-id="cd9cb-135">Som standard kombineras befintliga och nya postuppsättningar.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-135">By default, existing and new record sets are merged.</span></span> <span data-ttu-id="cd9cb-136">Identiska poster i en sammanfogad postuppsättning är ta bort dubbletter.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-136">Identical records within a merged record set are de-duplicated.</span></span>
* <span data-ttu-id="cd9cb-137">Du kan också genom att ange den `--force` alternativ, importera process-ersätter befintliga postuppsättningar med nya postuppsättningar.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-137">Alternatively, by specifying the `--force` option, the import process replaces existing record sets with new record sets.</span></span> <span data-ttu-id="cd9cb-138">Befintliga postuppsättningar som saknar motsvarande post i filen importerade zonen är inte tas bort.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-138">Existing record sets that do not have a corresponding record set in the imported zone file are not be removed.</span></span>
* <span data-ttu-id="cd9cb-139">Time to live (TTL) redan befintliga uppsättningar av poster används när postuppsättningar slås samman.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-139">When record sets are merged, the time to live (TTL) of preexisting record sets is used.</span></span> <span data-ttu-id="cd9cb-140">När `--force` är används, TTL-värdet för den nya uppsättningen av poster används.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-140">When `--force` is used, the TTL of the new record set is used.</span></span>
* <span data-ttu-id="cd9cb-141">Start av Authority (SOA)-parametrar (utom `host`) alltid tas från importerade zonfilen, oavsett om `--force` används.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-141">Start of Authority (SOA) parameters (except `host`) are always taken from the imported zone file, regardless of whether `--force` is used.</span></span> <span data-ttu-id="cd9cb-142">På samma sätt för namnserverposten inställd på zonens apex, hämtas TTL-värdet alltid från den importera zonfilen.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-142">Similarly, for the name server record set at the zone apex, the TTL is always taken from the imported zone file.</span></span>
* <span data-ttu-id="cd9cb-143">En importerad CNAME ersätter inte en befintlig CNAME-post med samma namn om inte den `--force` parameter har angetts.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-143">An imported CNAME record does not replace an existing CNAME record with the same name unless the `--force` parameter is specified.</span></span>
* <span data-ttu-id="cd9cb-144">En konflikt uppstår mellan en CNAME-post och en annan post med samma namn men med annan typ (oavsett vilken finns en befintlig eller ny), bevaras den befintliga posten.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-144">When a conflict arises between a CNAME record and another record of the same name but different type (regardless of which is existing or new), the existing record is retained.</span></span> <span data-ttu-id="cd9cb-145">Det är oberoende av användningen av `--force`.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-145">This is independent of the use of `--force`.</span></span>

### <a name="additional-information-about-importing"></a><span data-ttu-id="cd9cb-146">Mer information om hur du importerar</span><span class="sxs-lookup"><span data-stu-id="cd9cb-146">Additional information about importing</span></span>

<span data-ttu-id="cd9cb-147">Följande information ger ytterligare teknisk information om zonen import-processen.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-147">The following notes provide additional technical details about the zone import process.</span></span>

* <span data-ttu-id="cd9cb-148">Den `$TTL` direktiv är valfritt och stöds.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-148">The `$TTL` directive is optional, and it is supported.</span></span> <span data-ttu-id="cd9cb-149">När ingen `$TTL` direktiv ges, utan en explicit TTL-poster är importerade inställd på en standard-TTL 3600 sekunder.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-149">When no `$TTL` directive is given, records without an explicit TTL are imported set to a default TTL of 3600 seconds.</span></span> <span data-ttu-id="cd9cb-150">När två poster i samma postuppsättning ange olika TTLs, används det lägsta värdet.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-150">When two records in the same record set specify different TTLs, the lower value is used.</span></span>
* <span data-ttu-id="cd9cb-151">Den `$ORIGIN` direktiv är valfritt och stöds.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-151">The `$ORIGIN` directive is optional, and it is supported.</span></span> <span data-ttu-id="cd9cb-152">När ingen `$ORIGIN` har angetts används standardvärdet är zonnamnet som anges på kommandoraden (plus den avslutande ””.).</span><span class="sxs-lookup"><span data-stu-id="cd9cb-152">When no `$ORIGIN` is set, the default value used is the zone name as specified on the command line (plus the terminating ".").</span></span>
* <span data-ttu-id="cd9cb-153">Den `$INCLUDE` och `$GENERATE` direktiven stöds inte.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-153">The `$INCLUDE` and `$GENERATE` directives are not supported.</span></span>
* <span data-ttu-id="cd9cb-154">Dessa typer stöds: A, AAAA, CNAME, MX, NS, SOA, SRV och TXT.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-154">These record types are supported: A, AAAA, CNAME, MX, NS, SOA, SRV, and TXT.</span></span>
* <span data-ttu-id="cd9cb-155">SOA-posten skapas automatiskt av Azure DNS när en zon skapas.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-155">The SOA record is created automatically by Azure DNS when a zone is created.</span></span> <span data-ttu-id="cd9cb-156">När du importerar en zonfil alla SOA-parametrarna hämtas från zonfilen *utom* den `host` parameter.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-156">When you import a zone file, all SOA parameters are taken from the zone file *except* the `host` parameter.</span></span> <span data-ttu-id="cd9cb-157">Den här parametern används det värde som tillhandahålls av Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-157">This parameter uses the value provided by Azure DNS.</span></span> <span data-ttu-id="cd9cb-158">Det beror på att den här parametern måste referera till den primära namnservern som tillhandahålls av Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-158">This is because this parameter must refer to the primary name server provided by Azure DNS.</span></span>
* <span data-ttu-id="cd9cb-159">Ange på zonens apex namnserverposten skapas också automatiskt av Azure DNS när zonen skapas.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-159">The name server record set at the zone apex is also created automatically by Azure DNS when the zone is created.</span></span> <span data-ttu-id="cd9cb-160">Endast TTL-värdet för den här postuppsättning importeras.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-160">Only the TTL of this record set is imported.</span></span> <span data-ttu-id="cd9cb-161">Dessa poster innehålla servernamnen namn tillhandahålls av Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-161">These records contain the name server names provided by Azure DNS.</span></span> <span data-ttu-id="cd9cb-162">Postdata skrivs inte över med de värden som finns i filen importerade zoner.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-162">The record data is not overwritten by the values contained in the imported zone file.</span></span>
* <span data-ttu-id="cd9cb-163">Under Public Preview stöder Azure DNS bara en sträng TXT-poster.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-163">During Public Preview, Azure DNS supports only single-string TXT records.</span></span> <span data-ttu-id="cd9cb-164">Flersträngiga TXT-poster är sammanfogas och trunkeras till 255 tecken.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-164">Multistring TXT records are be concatenated and truncated to 255 characters.</span></span>

### <a name="cli-format-and-values"></a><span data-ttu-id="cd9cb-165">CLI-format och värden</span><span class="sxs-lookup"><span data-stu-id="cd9cb-165">CLI format and values</span></span>

<span data-ttu-id="cd9cb-166">Formatet på Azure CLI-kommandot för att importera en DNS-zon är:</span><span class="sxs-lookup"><span data-stu-id="cd9cb-166">The format of the Azure CLI command to import a DNS zone is:</span></span>

```azurecli
azure network dns zone import [options] <resource group> <zone name> <zone file name>
```

<span data-ttu-id="cd9cb-167">Värden:</span><span class="sxs-lookup"><span data-stu-id="cd9cb-167">Values:</span></span>

* <span data-ttu-id="cd9cb-168">`<resource group>`är namnet på resursgruppen för zonen i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-168">`<resource group>` is the name of the resource group for the zone in Azure DNS.</span></span>
* <span data-ttu-id="cd9cb-169">`<zone name>`är namnet på zonen.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-169">`<zone name>` is the name of the zone.</span></span>
* <span data-ttu-id="cd9cb-170">`<zone file name>`är sökvägen för zonen-fil som ska importeras.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-170">`<zone file name>` is the path/name of the zone file to be imported.</span></span>

<span data-ttu-id="cd9cb-171">Om en zon med det här namnet inte finns i resursgruppen, skapas den åt dig.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-171">If a zone with this name does not exist in the resource group, it is created for you.</span></span> <span data-ttu-id="cd9cb-172">Om zonen redan slås importerade postuppsättningar samman med befintliga postuppsättningar.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-172">If the zone already exists, the imported record sets are merged with existing record sets.</span></span> <span data-ttu-id="cd9cb-173">Om du vill skriva över de befintliga postuppsättningar använder den `--force` alternativet.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-173">To overwrite the existing record sets, use the `--force` option.</span></span>

<span data-ttu-id="cd9cb-174">Använd för att kontrollera formatet för en zonfil utan att faktiskt importera dem i `--parse-only` alternativet.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-174">To verify the format of a zone file without actually importing it, use the `--parse-only` option.</span></span>

### <a name="step-1-import-a-zone-file"></a><span data-ttu-id="cd9cb-175">Steg 1.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-175">Step 1.</span></span> <span data-ttu-id="cd9cb-176">Importera en zonfil</span><span class="sxs-lookup"><span data-stu-id="cd9cb-176">Import a zone file</span></span>

<span data-ttu-id="cd9cb-177">Så här importerar du en zonfil för zonen **contoso.com**.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-177">To import a zone file for the zone **contoso.com**.</span></span>

1. <span data-ttu-id="cd9cb-178">Logga in på din Azure-prenumeration med hjälp av Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-178">Sign in to your Azure subscription by using the Azure CLI 1.0.</span></span>

    ```azurecli
    azure login
    ```

2. <span data-ttu-id="cd9cb-179">Välj den prenumeration där du vill skapa en ny DNS-zon.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-179">Select the subscription where you want to create your new DNS zone.</span></span>

    ```azurecli
    azure account set <subscription name>
    ```

3. <span data-ttu-id="cd9cb-180">Azure DNS är en Azure Resource Manager-only-tjänst, så Azure CLI måste du växla till Resource Manager-läget.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-180">Azure DNS is an Azure Resource Manager-only service, so the Azure CLI must be switched to Resource Manager mode.</span></span>

    ```azurecli
    azure config mode arm
    ```

4. <span data-ttu-id="cd9cb-181">Innan du använder tjänsten Azure DNS måste du registrera prenumerationen för att använda resursen Microsoft.Network-providern.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-181">Before you use the Azure DNS service, you must register your subscription to use the Microsoft.Network resource provider.</span></span> <span data-ttu-id="cd9cb-182">(Detta är en engångsåtgärd för varje prenumeration.)</span><span class="sxs-lookup"><span data-stu-id="cd9cb-182">(This is a one-time operation for each subscription.)</span></span>

    ```azurecli
    azure provider register Microsoft.Network
    ```

5. <span data-ttu-id="cd9cb-183">Om du inte har någon redan måste du också skapa en resursgrupp för hanteraren för filserverresurser.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-183">If you don't have one already, you also need to create a Resource Manager resource group.</span></span>

    ```azurecli
    azure group create myresourcegroup westeurope
    ```

6. <span data-ttu-id="cd9cb-184">Så här importerar du zonen **contoso.com** från filen **contoso.com.txt** till en ny DNS-zon i resursgruppen **myresourcegroup**, kör kommandot `azure network dns zone import`.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-184">To import the zone **contoso.com** from the file **contoso.com.txt** into a new DNS zone in the resource group **myresourcegroup**, run the command `azure network dns zone import`.</span></span><BR><span data-ttu-id="cd9cb-185">Det här kommandot laddar zonfilen och analysera den.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-185">This command loads the zone file and parse it.</span></span> <span data-ttu-id="cd9cb-186">Kommandot Kör en serie kommandon på Azure DNS-tjänsten för att skapa zonen och alla postuppsättningar i zonen.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-186">The command executes a series of commands on the Azure DNS service to create the zone and all the record sets in the zone.</span></span> <span data-ttu-id="cd9cb-187">Kommandot rapporterar förlopp i konsolfönstret, tillsammans med eventuella fel eller varningar.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-187">The command reports progress in the console window, along with any errors or warnings.</span></span> <span data-ttu-id="cd9cb-188">Det kan ta några minuter att importera en stor zonfil eftersom postuppsättningar skapas i serien.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-188">Because record sets are created in series, it may take a few minutes to import a large zone file.</span></span>

    ```azurecli
    azure network dns zone import myresourcegroup contoso.com contoso.com.txt
    ```

### <a name="step-2-verify-the-zone"></a><span data-ttu-id="cd9cb-189">Steg 2.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-189">Step 2.</span></span> <span data-ttu-id="cd9cb-190">Verifiera zonen</span><span class="sxs-lookup"><span data-stu-id="cd9cb-190">Verify the zone</span></span>

<span data-ttu-id="cd9cb-191">För att verifiera DNS-zonen när du importerar filen, kan du använda någon av följande metoder:</span><span class="sxs-lookup"><span data-stu-id="cd9cb-191">To verify the DNS zone after you import the file, you can use any one of the following methods:</span></span>

* <span data-ttu-id="cd9cb-192">Du kan ange poster med hjälp av följande Azure CLI-kommando:</span><span class="sxs-lookup"><span data-stu-id="cd9cb-192">You can list the records by using the following Azure CLI command:</span></span>

    ```azurecli
    azure network dns record-set list myresourcegroup contoso.com
    ```

* <span data-ttu-id="cd9cb-193">Du kan visa en lista med poster med hjälp av PowerShell-cmdleten `Get-AzureRmDnsRecordSet`.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-193">You can list the records by using the PowerShell cmdlet `Get-AzureRmDnsRecordSet`.</span></span>
* <span data-ttu-id="cd9cb-194">Du kan använda `nslookup` att verifiera namnmatchning för posterna.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-194">You can use `nslookup` to verify name resolution for the records.</span></span> <span data-ttu-id="cd9cb-195">Eftersom zonen inte delegerad ännu, måste du ange rätt Azure DNS-namnservrar explicit.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-195">Because the zone isn't delegated yet, you need to specify the correct Azure DNS name servers explicitly.</span></span> <span data-ttu-id="cd9cb-196">I följande exempel visas hur du hämtar servernamnen namn tilldelas zonen.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-196">The following sample shows how to retrieve the name server names assigned to the zone.</span></span> <span data-ttu-id="cd9cb-197">IT visar även hur du fråga ”www”-post med hjälp av `nslookup`.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-197">IT also shows how to query the "www" record by using `nslookup`.</span></span>

        C:\>azure network dns record-set show myresourcegroup contoso.com @ NS
        info:Executing command network dns record-set show
        + Looking up the DNS Record Set "@" of type "NS"
        data:Id: /subscriptions/.../resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/NS/@
        data:Name: @
        data:Type: Microsoft.Network/dnszones/NS
        data:Location: global
        data:TTL : 3600
        data:NS records
        data:Name server domain name : ns1-01.azure-dns.com
        data:Name server domain name : ns2-01.azure-dns.net
        data:Name server domain name : ns3-01.azure-dns.org
        data:Name server domain name : ns4-01.azure-dns.info
        data:
        info:network dns record-set show command OK

        C:\> nslookup www.contoso.com ns1-01.azure-dns.com

        Server: ns1-01.azure-dns.com
        Address:  40.90.4.1

        Name:www.contoso.com
        Addresses:  134.170.185.46
        134.170.188.221

### <a name="step-3-update-dns-delegation"></a><span data-ttu-id="cd9cb-198">Steg 3.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-198">Step 3.</span></span> <span data-ttu-id="cd9cb-199">Uppdatera DNS-delegering</span><span class="sxs-lookup"><span data-stu-id="cd9cb-199">Update DNS delegation</span></span>

<span data-ttu-id="cd9cb-200">När du har kontrollerat att zonen har importerats korrekt, måste du uppdatera DNS-delegeringen för att peka till Azure DNS-namnservrar.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-200">After you have verified that the zone has been imported correctly, you need to update the DNS delegation to point to the Azure DNS name servers.</span></span> <span data-ttu-id="cd9cb-201">Mer information finns i artikeln [uppdatera DNS-delegeringen](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="cd9cb-201">For more information, see the article [Update the DNS delegation](dns-domain-delegation.md).</span></span>

## <a name="export-a-dns-zone-file-from-azure-dns"></a><span data-ttu-id="cd9cb-202">Exportera en DNS-zonfilen från Azure DNS</span><span class="sxs-lookup"><span data-stu-id="cd9cb-202">Export a DNS zone file from Azure DNS</span></span>

<span data-ttu-id="cd9cb-203">Formatet på Azure CLI-kommandot för att importera en DNS-zon är:</span><span class="sxs-lookup"><span data-stu-id="cd9cb-203">The format of the Azure CLI command to import a DNS zone is:</span></span>

```azurecli
azure network dns zone export [options] <resource group> <zone name> <zone file name>
```

<span data-ttu-id="cd9cb-204">Värden:</span><span class="sxs-lookup"><span data-stu-id="cd9cb-204">Values:</span></span>

* <span data-ttu-id="cd9cb-205">`<resource group>`är namnet på resursgruppen för zonen i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-205">`<resource group>` is the name of the resource group for the zone in Azure DNS.</span></span>
* <span data-ttu-id="cd9cb-206">`<zone name>`är namnet på zonen.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-206">`<zone name>` is the name of the zone.</span></span>
* <span data-ttu-id="cd9cb-207">`<zone file name>`är sökvägen för zonen-fil som ska exporteras.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-207">`<zone file name>` is the path/name of the zone file to be exported.</span></span>

<span data-ttu-id="cd9cb-208">Som med importen zon måste du först logga in, väljer din prenumeration och konfigurera Azure CLI för att använda Resource Manager-läge.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-208">As with the zone import, you first need to sign in, choose your subscription, and configure the Azure CLI to use Resource Manager mode.</span></span>

### <a name="to-export-a-zone-file"></a><span data-ttu-id="cd9cb-209">Så här exporterar du en zonfil</span><span class="sxs-lookup"><span data-stu-id="cd9cb-209">To export a zone file</span></span>

1. <span data-ttu-id="cd9cb-210">Logga in på din Azure-prenumeration med hjälp av Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-210">Sign in to your Azure subscription by using the Azure CLI.</span></span>

    ```azurecli
    azure login
    ```

2. <span data-ttu-id="cd9cb-211">Välj den prenumeration där du vill skapa en DNS-zon.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-211">Select the subscription where you want to create your DNS zone.</span></span>

    ```azurecli
    azure account set <subscription name>
    ```

3. <span data-ttu-id="cd9cb-212">Azure DNS är en Azure Resource Manager-only-tjänst.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-212">Azure DNS is an Azure Resource Manager-only service.</span></span> <span data-ttu-id="cd9cb-213">Azure CLI måste du växla till Resource Manager-läget.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-213">The Azure CLI must be switched to Resource Manager mode.</span></span>

    ```azurecli
    azure config mode arm
    ```

4. <span data-ttu-id="cd9cb-214">Så här exporterar du den befintliga Azure DNS-zonen **contoso.com** i resursgruppen **myresourcegroup** till filen **contoso.com.txt** (i den aktuella mappen), kör `azure network dns zone export`.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-214">To export the existing Azure DNS zone **contoso.com** in resource group **myresourcegroup** to the file **contoso.com.txt** (in the current folder), run `azure network dns zone export`.</span></span> <span data-ttu-id="cd9cb-215">Det här kommandot anropar Azure DNS-tjänsten för att räkna upp postuppsättningar i zonen och exportera resultaten till en kompatibel BINDNING zonfil.</span><span class="sxs-lookup"><span data-stu-id="cd9cb-215">This command  calls the Azure DNS service to enumerate record sets in the zone and export the results to a BIND-compatible zone file.</span></span>

    ```azurecli
    azure network dns zone export myresourcegroup contoso.com contoso.com.txt
    ```
