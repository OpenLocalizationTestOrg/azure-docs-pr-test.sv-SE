---
title: "aaaImport och exportera en domänzonen filen tooAzure DNS med hjälp av Azure CLI 1.0 | Microsoft Docs"
description: "Lär dig hur tooimport och exportera ett DNS zonen filen tooAzure DNS med hjälp av Azure CLI 1.0"
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
ms.openlocfilehash: 4c3163395e151e9934c730349b828c612491016f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="import-and-export-a-dns-zone-file-using-hello-azure-cli-10"></a><span data-ttu-id="f063b-103">Importera och exportera en DNS-zonfilen med hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="f063b-103">Import and export a DNS zone file using hello Azure CLI 1.0</span></span> 

<span data-ttu-id="f063b-104">Den här artikeln får du veta hur tooimport export DNS-zonen i filerna och Azure DNS med hello Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="f063b-104">This article walks you through how tooimport and export DNS zone files for Azure DNS using hello Azure CLI 1.0.</span></span>

## <a name="introduction-toodns-zone-migration"></a><span data-ttu-id="f063b-105">Introduktion tooDNS zon migrering</span><span class="sxs-lookup"><span data-stu-id="f063b-105">Introduction tooDNS zone migration</span></span>

<span data-ttu-id="f063b-106">En DNS-zonfilen är en textfil som innehåller information om alla poster för Domain Name System (DNS) i hello zonen.</span><span class="sxs-lookup"><span data-stu-id="f063b-106">A DNS zone file is a text file that contains details of every Domain Name System (DNS) record in hello zone.</span></span> <span data-ttu-id="f063b-107">Här följer ett standardformat, vilket gör den lämplig för överföring av DNS-poster mellan DNS-system.</span><span class="sxs-lookup"><span data-stu-id="f063b-107">It follows a standard format, making it suitable for transferring DNS records between DNS systems.</span></span> <span data-ttu-id="f063b-108">Med hjälp av en zonfil är en snabb, tillförlitligt och praktiskt sätt tootransfer en DNS-zon till eller från Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="f063b-108">Using a zone file is a quick, reliable, and convenient way tootransfer a DNS zone into or out of Azure DNS.</span></span>

<span data-ttu-id="f063b-109">Azure DNS stöder import och export av filer med hjälp av hello Azure-kommandoradsgränssnittet (CLI).</span><span class="sxs-lookup"><span data-stu-id="f063b-109">Azure DNS supports importing and exporting zone files by using hello Azure command-line interface (CLI).</span></span> <span data-ttu-id="f063b-110">Zonen filimport är **inte** stöds för närvarande via Azure PowerShell eller hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f063b-110">Zone file import is **not** currently supported via Azure PowerShell or hello Azure portal.</span></span>

<span data-ttu-id="f063b-111">hello Azure CLI 1.0 är ett kommandoradsverktyg för flera plattformar som används för att hantera Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="f063b-111">hello Azure CLI 1.0 is a cross-platform command-line tool used for managing Azure services.</span></span> <span data-ttu-id="f063b-112">Den är tillgänglig för hello Windows, Mac och Linux-plattformarna från hello [Azure nedladdningar](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="f063b-112">It is available for hello Windows, Mac, and Linux platforms from hello [Azure downloads page](https://azure.microsoft.com/downloads/).</span></span> <span data-ttu-id="f063b-113">Plattformsoberoende stöd är viktigt för import och export av filer, eftersom hello vanligaste namn serverprogramvara [BINDA](https://www.isc.org/downloads/bind/), vanligtvis körs på Linux.</span><span class="sxs-lookup"><span data-stu-id="f063b-113">Cross-platform support is important for importing and exporting zone files, because hello most common name server software, [BIND](https://www.isc.org/downloads/bind/), typically runs on Linux.</span></span>

> [!NOTE]
> <span data-ttu-id="f063b-114">Det finns två versioner av hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="f063b-114">There are currently two versions of hello Azure CLI.</span></span> <span data-ttu-id="f063b-115">CLI1.0 baseras på Node.js och har kommandon som börjar med ”azure”.</span><span class="sxs-lookup"><span data-stu-id="f063b-115">CLI1.0 is based on Node.js, and has commands beginning with "azure".</span></span>
> <span data-ttu-id="f063b-116">CLI2.0 baseras på Python och har kommandon som börjar med ”az”.</span><span class="sxs-lookup"><span data-stu-id="f063b-116">CLI2.0 is based on Python and has commands beginning with "az".</span></span> <span data-ttu-id="f063b-117">När zonen filimport stöds i båda versionerna, bör du använda hello CLI1.0 kommandon som beskrivs i den här sidan.</span><span class="sxs-lookup"><span data-stu-id="f063b-117">While zone file import is supported in both versions, we recommend using hello CLI1.0 commands, as described in this page.</span></span>

## <a name="obtain-your-existing-dns-zone-file"></a><span data-ttu-id="f063b-118">Hämta din befintliga DNS-zonfilen</span><span class="sxs-lookup"><span data-stu-id="f063b-118">Obtain your existing DNS zone file</span></span>

<span data-ttu-id="f063b-119">Innan du importerar en DNS-zonfilen till Azure DNS måste tooobtain en kopia av hello zonfilen.</span><span class="sxs-lookup"><span data-stu-id="f063b-119">Before you import a DNS zone file into Azure DNS, you need tooobtain a copy of hello zone file.</span></span> <span data-ttu-id="f063b-120">hello källan för den här filen är beroende av hello DNS-zon som finns för närvarande.</span><span class="sxs-lookup"><span data-stu-id="f063b-120">hello source of this file depends on where hello DNS zone is currently hosted.</span></span>

* <span data-ttu-id="f063b-121">Om DNS-zonen finns på en partner-tjänst (till exempel en domänregistrator, dedikerad DNS-värdtjänsten eller alternativa molnprovider), tillhandahålla tjänsten hello möjlighet toodownload hello DNS-zonfilen.</span><span class="sxs-lookup"><span data-stu-id="f063b-121">If your DNS zone is hosted by a partner service (such as a domain registrar, dedicated DNS hosting provider, or alternative cloud provider), that service should provide hello ability toodownload hello DNS zone file.</span></span>
* <span data-ttu-id="f063b-122">Om DNS-zonen finns på Windows DNS hello standardmappen för hello filer är **%systemroot%\system32\dns**.</span><span class="sxs-lookup"><span data-stu-id="f063b-122">If your DNS zone is hosted on Windows DNS, hello default folder for hello zone files is **%systemroot%\system32\dns**.</span></span> <span data-ttu-id="f063b-123">hello fullständig sökväg tooeach zonfilen visar även hello **allmänna** för hello DNS-konsolen.</span><span class="sxs-lookup"><span data-stu-id="f063b-123">hello full path tooeach zone file also shows on hello **General** tab of hello DNS console.</span></span>
* <span data-ttu-id="f063b-124">Om DNS-zonen finns med hjälp av BIND hello platsen för hello-zonfilen för varje zon som anges i konfigurationsfilen för hello BIND **named.conf**.</span><span class="sxs-lookup"><span data-stu-id="f063b-124">If your DNS zone is hosted by using BIND, hello location of hello zone file for each zone is specified in hello BIND configuration file **named.conf**.</span></span>

> [!NOTE]
> <span data-ttu-id="f063b-125">Zonen filer som hämtas från GoDaddy har ett avvikande något format.</span><span class="sxs-lookup"><span data-stu-id="f063b-125">Zone files downloaded from GoDaddy have a slightly nonstandard format.</span></span> <span data-ttu-id="f063b-126">Du behöver toocorrect detta innan du importerar dessa filer till Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="f063b-126">You need toocorrect this before you import these zone files into Azure DNS.</span></span>
>
> <span data-ttu-id="f063b-127">DNS-namn i hello RDATA för varje DNS-posten har angetts som ett fullständigt kvalificerat namn, men de inte har en avslutande ””. Det innebär att de tolkas av andra DNS-system som relativa namn.</span><span class="sxs-lookup"><span data-stu-id="f063b-127">DNS names in hello RDATA of each DNS record are specified as fully qualified names, but they don't have a terminating "." This means they are interpreted by other DNS systems as relative names.</span></span> <span data-ttu-id="f063b-128">Du behöver tooedit hello zon filen tooappend hello avslutande ””. tootheir namn innan du importerar dem till Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="f063b-128">You need tooedit hello zone file tooappend hello terminating "." tootheir names before you import them into Azure DNS.</span></span>
>
> <span data-ttu-id="f063b-129">Till exempel hello CNAME-post ”www 3600 i CNAME contoso.com” ska ändras för ”www 3600 IN CNAME contoso.com”.</span><span class="sxs-lookup"><span data-stu-id="f063b-129">For example, hello CNAME record "www 3600 IN CNAME contoso.com" should be changed too"www 3600 IN CNAME contoso.com."</span></span>
> <span data-ttu-id="f063b-130">(med en avslutande ””.).</span><span class="sxs-lookup"><span data-stu-id="f063b-130">(with a terminating ".").</span></span>

## <a name="import-a-dns-zone-file-into-azure-dns"></a><span data-ttu-id="f063b-131">Importera en DNS-zonfilen till Azure DNS</span><span class="sxs-lookup"><span data-stu-id="f063b-131">Import a DNS zone file into Azure DNS</span></span>

<span data-ttu-id="f063b-132">Om du importerar en zonfil skapas en ny zon i Azure DNS om det inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="f063b-132">Importing a zone file creates a new zone in Azure DNS if one does not already exist.</span></span> <span data-ttu-id="f063b-133">Om det redan finns hello zonen måste hello postuppsättningar i hello zonfil slås samman med hello befintliga postuppsättningar.</span><span class="sxs-lookup"><span data-stu-id="f063b-133">If hello zone already exists, hello record sets in hello zone file must be merged with hello existing record sets.</span></span>

### <a name="merge-behavior"></a><span data-ttu-id="f063b-134">Sammanfoga beteende</span><span class="sxs-lookup"><span data-stu-id="f063b-134">Merge behavior</span></span>

* <span data-ttu-id="f063b-135">Som standard kombineras befintliga och nya postuppsättningar.</span><span class="sxs-lookup"><span data-stu-id="f063b-135">By default, existing and new record sets are merged.</span></span> <span data-ttu-id="f063b-136">Identiska poster i en sammanfogad postuppsättning är ta bort dubbletter.</span><span class="sxs-lookup"><span data-stu-id="f063b-136">Identical records within a merged record set are de-duplicated.</span></span>
* <span data-ttu-id="f063b-137">Du kan också genom att ange hello `--force` alternativ, hello importera processen ersätter befintliga postuppsättningar med nya postuppsättningar.</span><span class="sxs-lookup"><span data-stu-id="f063b-137">Alternatively, by specifying hello `--force` option, hello import process replaces existing record sets with new record sets.</span></span> <span data-ttu-id="f063b-138">Befintliga postuppsättningar som saknar motsvarande post i hello importerade zonfilen är inte tas bort.</span><span class="sxs-lookup"><span data-stu-id="f063b-138">Existing record sets that do not have a corresponding record set in hello imported zone file are not be removed.</span></span>
* <span data-ttu-id="f063b-139">När postuppsättningar sammanfogas används hello toolive TTL (time) redan befintliga uppsättningar av poster.</span><span class="sxs-lookup"><span data-stu-id="f063b-139">When record sets are merged, hello time toolive (TTL) of preexisting record sets is used.</span></span> <span data-ttu-id="f063b-140">När `--force` är används, hello TTL-värde på hello nya postuppsättning används.</span><span class="sxs-lookup"><span data-stu-id="f063b-140">When `--force` is used, hello TTL of hello new record set is used.</span></span>
* <span data-ttu-id="f063b-141">Start av Authority (SOA)-parametrar (utom `host`) alltid tas från hello importerade zonfilen, oavsett om `--force` används.</span><span class="sxs-lookup"><span data-stu-id="f063b-141">Start of Authority (SOA) parameters (except `host`) are always taken from hello imported zone file, regardless of whether `--force` is used.</span></span> <span data-ttu-id="f063b-142">På samma sätt för hello namnserverposten på hello zonens apex, hämtas hello TTL alltid från hello importerade zonfilen.</span><span class="sxs-lookup"><span data-stu-id="f063b-142">Similarly, for hello name server record set at hello zone apex, hello TTL is always taken from hello imported zone file.</span></span>
* <span data-ttu-id="f063b-143">En importerad CNAME-post inte ersätta en befintlig CNAME registrera med samma namn om hello hello `--force` parameter har angetts.</span><span class="sxs-lookup"><span data-stu-id="f063b-143">An imported CNAME record does not replace an existing CNAME record with hello same name unless hello `--force` parameter is specified.</span></span>
* <span data-ttu-id="f063b-144">När en konflikt uppstår mellan en CNAME-post och en annan post med samma namn men olika hello Skriv (oavsett vilket finns en befintlig eller ny), behålls hello befintlig post.</span><span class="sxs-lookup"><span data-stu-id="f063b-144">When a conflict arises between a CNAME record and another record of hello same name but different type (regardless of which is existing or new), hello existing record is retained.</span></span> <span data-ttu-id="f063b-145">Det är oberoende av hello användning av `--force`.</span><span class="sxs-lookup"><span data-stu-id="f063b-145">This is independent of hello use of `--force`.</span></span>

### <a name="additional-information-about-importing"></a><span data-ttu-id="f063b-146">Mer information om hur du importerar</span><span class="sxs-lookup"><span data-stu-id="f063b-146">Additional information about importing</span></span>

<span data-ttu-id="f063b-147">hello ger följande information ytterligare teknisk information om hello zonen import-processen.</span><span class="sxs-lookup"><span data-stu-id="f063b-147">hello following notes provide additional technical details about hello zone import process.</span></span>

* <span data-ttu-id="f063b-148">Hej `$TTL` direktiv är valfritt och stöds.</span><span class="sxs-lookup"><span data-stu-id="f063b-148">hello `$TTL` directive is optional, and it is supported.</span></span> <span data-ttu-id="f063b-149">När ingen `$TTL` direktiv ges, utan en explicit TTL importeras tooa standard TTL 3600 sekunder.</span><span class="sxs-lookup"><span data-stu-id="f063b-149">When no `$TTL` directive is given, records without an explicit TTL are imported set tooa default TTL of 3600 seconds.</span></span> <span data-ttu-id="f063b-150">När två poster i hello samma postuppsättning ange olika TTLs, hello lägre värde används.</span><span class="sxs-lookup"><span data-stu-id="f063b-150">When two records in hello same record set specify different TTLs, hello lower value is used.</span></span>
* <span data-ttu-id="f063b-151">Hej `$ORIGIN` direktiv är valfritt och stöds.</span><span class="sxs-lookup"><span data-stu-id="f063b-151">hello `$ORIGIN` directive is optional, and it is supported.</span></span> <span data-ttu-id="f063b-152">När ingen `$ORIGIN` anges hello standard-värde som används är hello zonnamnet som anges på kommandoraden för hello (plus avslutar hello ””.).</span><span class="sxs-lookup"><span data-stu-id="f063b-152">When no `$ORIGIN` is set, hello default value used is hello zone name as specified on hello command line (plus hello terminating ".").</span></span>
* <span data-ttu-id="f063b-153">Hej `$INCLUDE` och `$GENERATE` direktiven stöds inte.</span><span class="sxs-lookup"><span data-stu-id="f063b-153">hello `$INCLUDE` and `$GENERATE` directives are not supported.</span></span>
* <span data-ttu-id="f063b-154">Dessa typer stöds: A, AAAA, CNAME, MX, NS, SOA, SRV och TXT.</span><span class="sxs-lookup"><span data-stu-id="f063b-154">These record types are supported: A, AAAA, CNAME, MX, NS, SOA, SRV, and TXT.</span></span>
* <span data-ttu-id="f063b-155">hello SOA-post skapas automatiskt av Azure DNS när en zon skapas.</span><span class="sxs-lookup"><span data-stu-id="f063b-155">hello SOA record is created automatically by Azure DNS when a zone is created.</span></span> <span data-ttu-id="f063b-156">När du importerar en zonfil alla SOA-parametrarna hämtas från hello zonfilen *utom* hello `host` parameter.</span><span class="sxs-lookup"><span data-stu-id="f063b-156">When you import a zone file, all SOA parameters are taken from hello zone file *except* hello `host` parameter.</span></span> <span data-ttu-id="f063b-157">Den här parametern används hello-värde som tillhandahålls av Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="f063b-157">This parameter uses hello value provided by Azure DNS.</span></span> <span data-ttu-id="f063b-158">Det beror på att den här parametern måste referera toohello namn på primär server som ingår i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="f063b-158">This is because this parameter must refer toohello primary name server provided by Azure DNS.</span></span>
* <span data-ttu-id="f063b-159">hello namnserverposten på hello zonens apex skapas också automatiskt av Azure DNS när hello zon skapas.</span><span class="sxs-lookup"><span data-stu-id="f063b-159">hello name server record set at hello zone apex is also created automatically by Azure DNS when hello zone is created.</span></span> <span data-ttu-id="f063b-160">Endast importeras hello TTL-värde på den här postuppsättning.</span><span class="sxs-lookup"><span data-stu-id="f063b-160">Only hello TTL of this record set is imported.</span></span> <span data-ttu-id="f063b-161">Dessa poster innehålla hello servernamn tillhandahålls av Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="f063b-161">These records contain hello name server names provided by Azure DNS.</span></span> <span data-ttu-id="f063b-162">hello registrera data skrivs inte över av hello värden i hello importerade zonfilen.</span><span class="sxs-lookup"><span data-stu-id="f063b-162">hello record data is not overwritten by hello values contained in hello imported zone file.</span></span>
* <span data-ttu-id="f063b-163">Under Public Preview stöder Azure DNS bara en sträng TXT-poster.</span><span class="sxs-lookup"><span data-stu-id="f063b-163">During Public Preview, Azure DNS supports only single-string TXT records.</span></span> <span data-ttu-id="f063b-164">Flersträngiga TXT-poster är vara sammansatta och trunkerat too255 tecken.</span><span class="sxs-lookup"><span data-stu-id="f063b-164">Multistring TXT records are be concatenated and truncated too255 characters.</span></span>

### <a name="cli-format-and-values"></a><span data-ttu-id="f063b-165">CLI-format och värden</span><span class="sxs-lookup"><span data-stu-id="f063b-165">CLI format and values</span></span>

<span data-ttu-id="f063b-166">hello Azure CLI kommandot tooimport en DNS-zon hello format är:</span><span class="sxs-lookup"><span data-stu-id="f063b-166">hello format of hello Azure CLI command tooimport a DNS zone is:</span></span>

```azurecli
azure network dns zone import [options] <resource group> <zone name> <zone file name>
```

<span data-ttu-id="f063b-167">Värden:</span><span class="sxs-lookup"><span data-stu-id="f063b-167">Values:</span></span>

* <span data-ttu-id="f063b-168">`<resource group>`är hello namn hello resursgruppen för hello zonen i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="f063b-168">`<resource group>` is hello name of hello resource group for hello zone in Azure DNS.</span></span>
* <span data-ttu-id="f063b-169">`<zone name>`är hello namnet på hello zonen.</span><span class="sxs-lookup"><span data-stu-id="f063b-169">`<zone name>` is hello name of hello zone.</span></span>
* <span data-ttu-id="f063b-170">`<zone file name>`är hello/sökvägsnamn hello zon filen toobe importeras.</span><span class="sxs-lookup"><span data-stu-id="f063b-170">`<zone file name>` is hello path/name of hello zone file toobe imported.</span></span>

<span data-ttu-id="f063b-171">Om en zon med det här namnet inte finns i resursgruppen hello, skapas den åt dig.</span><span class="sxs-lookup"><span data-stu-id="f063b-171">If a zone with this name does not exist in hello resource group, it is created for you.</span></span> <span data-ttu-id="f063b-172">Hello zonen redan hello importerade postuppsättningar slås samman med befintliga postuppsättningar.</span><span class="sxs-lookup"><span data-stu-id="f063b-172">If hello zone already exists, hello imported record sets are merged with existing record sets.</span></span> <span data-ttu-id="f063b-173">toooverwrite hello befintliga postuppsättningar, använda hello `--force` alternativet.</span><span class="sxs-lookup"><span data-stu-id="f063b-173">toooverwrite hello existing record sets, use hello `--force` option.</span></span>

<span data-ttu-id="f063b-174">tooverify hello filformatet zon utan att faktiskt importera dem, Använd hello `--parse-only` alternativet.</span><span class="sxs-lookup"><span data-stu-id="f063b-174">tooverify hello format of a zone file without actually importing it, use hello `--parse-only` option.</span></span>

### <a name="step-1-import-a-zone-file"></a><span data-ttu-id="f063b-175">Steg 1.</span><span class="sxs-lookup"><span data-stu-id="f063b-175">Step 1.</span></span> <span data-ttu-id="f063b-176">Importera en zonfil</span><span class="sxs-lookup"><span data-stu-id="f063b-176">Import a zone file</span></span>

<span data-ttu-id="f063b-177">tooimport en zonfil för hello zonen **contoso.com**.</span><span class="sxs-lookup"><span data-stu-id="f063b-177">tooimport a zone file for hello zone **contoso.com**.</span></span>

1. <span data-ttu-id="f063b-178">Logga in tooyour Azure-prenumeration med hjälp av hello Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="f063b-178">Sign in tooyour Azure subscription by using hello Azure CLI 1.0.</span></span>

    ```azurecli
    azure login
    ```

2. <span data-ttu-id="f063b-179">Välj hello prenumeration där du vill att toocreate ny DNS-zon.</span><span class="sxs-lookup"><span data-stu-id="f063b-179">Select hello subscription where you want toocreate your new DNS zone.</span></span>

    ```azurecli
    azure account set <subscription name>
    ```

3. <span data-ttu-id="f063b-180">Azure DNS är en Azure Resource Manager-only-tjänst så hello Azure CLI måste vara avstängda tooResource Manager-läget.</span><span class="sxs-lookup"><span data-stu-id="f063b-180">Azure DNS is an Azure Resource Manager-only service, so hello Azure CLI must be switched tooResource Manager mode.</span></span>

    ```azurecli
    azure config mode arm
    ```

4. <span data-ttu-id="f063b-181">Innan du använder hello Azure DNS-tjänsten måste du registrera din prenumeration toouse hello Microsoft.Network-resursprovidern.</span><span class="sxs-lookup"><span data-stu-id="f063b-181">Before you use hello Azure DNS service, you must register your subscription toouse hello Microsoft.Network resource provider.</span></span> <span data-ttu-id="f063b-182">(Detta är en engångsåtgärd för varje prenumeration.)</span><span class="sxs-lookup"><span data-stu-id="f063b-182">(This is a one-time operation for each subscription.)</span></span>

    ```azurecli
    azure provider register Microsoft.Network
    ```

5. <span data-ttu-id="f063b-183">Om du inte har någon redan måste du också toocreate en Resource Manager-resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="f063b-183">If you don't have one already, you also need toocreate a Resource Manager resource group.</span></span>

    ```azurecli
    azure group create myresourcegroup westeurope
    ```

6. <span data-ttu-id="f063b-184">tooimport hello zonen **contoso.com** från hello filen **contoso.com.txt** till en ny DNS-zon i hello resursgruppen **myresourcegroup**, kör hello kommando `azure network dns zone import`.</span><span class="sxs-lookup"><span data-stu-id="f063b-184">tooimport hello zone **contoso.com** from hello file **contoso.com.txt** into a new DNS zone in hello resource group **myresourcegroup**, run hello command `azure network dns zone import`.</span></span><BR><span data-ttu-id="f063b-185">Det här kommandot laddar hello zonfilen och analysera den.</span><span class="sxs-lookup"><span data-stu-id="f063b-185">This command loads hello zone file and parse it.</span></span> <span data-ttu-id="f063b-186">hello kommandot Kör en serie kommandon på hello Azure DNS-tjänsten toocreate hello zonen och alla hello postuppsättningar i zonen hello.</span><span class="sxs-lookup"><span data-stu-id="f063b-186">hello command executes a series of commands on hello Azure DNS service toocreate hello zone and all hello record sets in hello zone.</span></span> <span data-ttu-id="f063b-187">hello kommandot rapporterar förlopp i hello konsolfönstret, tillsammans med eventuella fel eller varningar.</span><span class="sxs-lookup"><span data-stu-id="f063b-187">hello command reports progress in hello console window, along with any errors or warnings.</span></span> <span data-ttu-id="f063b-188">Eftersom postuppsättningar skapas i serien, kan det ta några minuter tooimport en stor zonfil.</span><span class="sxs-lookup"><span data-stu-id="f063b-188">Because record sets are created in series, it may take a few minutes tooimport a large zone file.</span></span>

    ```azurecli
    azure network dns zone import myresourcegroup contoso.com contoso.com.txt
    ```

### <a name="step-2-verify-hello-zone"></a><span data-ttu-id="f063b-189">Steg 2.</span><span class="sxs-lookup"><span data-stu-id="f063b-189">Step 2.</span></span> <span data-ttu-id="f063b-190">Kontrollera hello zon</span><span class="sxs-lookup"><span data-stu-id="f063b-190">Verify hello zone</span></span>

<span data-ttu-id="f063b-191">tooverify hello DNS-zonen när du har importerat hello-fil, du kan använda någon av följande metoder hello:</span><span class="sxs-lookup"><span data-stu-id="f063b-191">tooverify hello DNS zone after you import hello file, you can use any one of hello following methods:</span></span>

* <span data-ttu-id="f063b-192">Du kan ange hello poster med hjälp av hello följande Azure CLI-kommando:</span><span class="sxs-lookup"><span data-stu-id="f063b-192">You can list hello records by using hello following Azure CLI command:</span></span>

    ```azurecli
    azure network dns record-set list myresourcegroup contoso.com
    ```

* <span data-ttu-id="f063b-193">Du kan ange hello poster med hjälp av PowerShell-cmdlet för hello `Get-AzureRmDnsRecordSet`.</span><span class="sxs-lookup"><span data-stu-id="f063b-193">You can list hello records by using hello PowerShell cmdlet `Get-AzureRmDnsRecordSet`.</span></span>
* <span data-ttu-id="f063b-194">Du kan använda `nslookup` tooverify namnmatchning för hello poster.</span><span class="sxs-lookup"><span data-stu-id="f063b-194">You can use `nslookup` tooverify name resolution for hello records.</span></span> <span data-ttu-id="f063b-195">Eftersom hello zonen inte delegerad ännu, måste toospecify hello rätt Azure DNS-namnservrar explicit.</span><span class="sxs-lookup"><span data-stu-id="f063b-195">Because hello zone isn't delegated yet, you need toospecify hello correct Azure DNS name servers explicitly.</span></span> <span data-ttu-id="f063b-196">hello visar följande exempel hur tooretrieve hello servernamn som tilldelats toohello zon.</span><span class="sxs-lookup"><span data-stu-id="f063b-196">hello following sample shows how tooretrieve hello name server names assigned toohello zone.</span></span> <span data-ttu-id="f063b-197">IT visar även hur tooquery hello ”www” registrera med `nslookup`.</span><span class="sxs-lookup"><span data-stu-id="f063b-197">IT also shows how tooquery hello "www" record by using `nslookup`.</span></span>

        C:\>azure network dns record-set show myresourcegroup contoso.com @ NS
        info:Executing command network dns record-set show
        + Looking up hello DNS Record Set "@" of type "NS"
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

### <a name="step-3-update-dns-delegation"></a><span data-ttu-id="f063b-198">Steg 3.</span><span class="sxs-lookup"><span data-stu-id="f063b-198">Step 3.</span></span> <span data-ttu-id="f063b-199">Uppdatera DNS-delegering</span><span class="sxs-lookup"><span data-stu-id="f063b-199">Update DNS delegation</span></span>

<span data-ttu-id="f063b-200">När du har kontrollerat att hello zonen har importerats korrekt behöver tooupdate hello DNS-delegering toopoint toohello namnservrar Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="f063b-200">After you have verified that hello zone has been imported correctly, you need tooupdate hello DNS delegation toopoint toohello Azure DNS name servers.</span></span> <span data-ttu-id="f063b-201">Mer information finns i artikeln hello [uppdatera hello DNS-delegering](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="f063b-201">For more information, see hello article [Update hello DNS delegation](dns-domain-delegation.md).</span></span>

## <a name="export-a-dns-zone-file-from-azure-dns"></a><span data-ttu-id="f063b-202">Exportera en DNS-zonfilen från Azure DNS</span><span class="sxs-lookup"><span data-stu-id="f063b-202">Export a DNS zone file from Azure DNS</span></span>

<span data-ttu-id="f063b-203">hello Azure CLI kommandot tooimport en DNS-zon hello format är:</span><span class="sxs-lookup"><span data-stu-id="f063b-203">hello format of hello Azure CLI command tooimport a DNS zone is:</span></span>

```azurecli
azure network dns zone export [options] <resource group> <zone name> <zone file name>
```

<span data-ttu-id="f063b-204">Värden:</span><span class="sxs-lookup"><span data-stu-id="f063b-204">Values:</span></span>

* <span data-ttu-id="f063b-205">`<resource group>`är hello namn hello resursgruppen för hello zonen i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="f063b-205">`<resource group>` is hello name of hello resource group for hello zone in Azure DNS.</span></span>
* <span data-ttu-id="f063b-206">`<zone name>`är hello namnet på hello zonen.</span><span class="sxs-lookup"><span data-stu-id="f063b-206">`<zone name>` is hello name of hello zone.</span></span>
* <span data-ttu-id="f063b-207">`<zone file name>`är hello/sökvägsnamn hello zon filen toobe exporteras.</span><span class="sxs-lookup"><span data-stu-id="f063b-207">`<zone file name>` is hello path/name of hello zone file toobe exported.</span></span>

<span data-ttu-id="f063b-208">Som med hello zonen import, behöver du först toosign i, väljer din prenumeration och konfigurerar hello Azure CLI toouse Resource Manager-läget.</span><span class="sxs-lookup"><span data-stu-id="f063b-208">As with hello zone import, you first need toosign in, choose your subscription, and configure hello Azure CLI toouse Resource Manager mode.</span></span>

### <a name="tooexport-a-zone-file"></a><span data-ttu-id="f063b-209">tooexport en zonfil</span><span class="sxs-lookup"><span data-stu-id="f063b-209">tooexport a zone file</span></span>

1. <span data-ttu-id="f063b-210">Logga in tooyour Azure-prenumeration med hjälp av hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="f063b-210">Sign in tooyour Azure subscription by using hello Azure CLI.</span></span>

    ```azurecli
    azure login
    ```

2. <span data-ttu-id="f063b-211">Välj hello prenumeration där du vill att toocreate DNS-zon.</span><span class="sxs-lookup"><span data-stu-id="f063b-211">Select hello subscription where you want toocreate your DNS zone.</span></span>

    ```azurecli
    azure account set <subscription name>
    ```

3. <span data-ttu-id="f063b-212">Azure DNS är en Azure Resource Manager-only-tjänst.</span><span class="sxs-lookup"><span data-stu-id="f063b-212">Azure DNS is an Azure Resource Manager-only service.</span></span> <span data-ttu-id="f063b-213">hello Azure CLI måste vara avstängda tooResource Manager-läget.</span><span class="sxs-lookup"><span data-stu-id="f063b-213">hello Azure CLI must be switched tooResource Manager mode.</span></span>

    ```azurecli
    azure config mode arm
    ```

4. <span data-ttu-id="f063b-214">tooexport hello Azure DNS-zon **contoso.com** i resursgruppen **myresourcegroup** toohello filen **contoso.com.txt** (kör helloaktuellamapp)`azure network dns zone export`.</span><span class="sxs-lookup"><span data-stu-id="f063b-214">tooexport hello existing Azure DNS zone **contoso.com** in resource group **myresourcegroup** toohello file **contoso.com.txt** (in hello current folder), run `azure network dns zone export`.</span></span> <span data-ttu-id="f063b-215">Det här kommandot anrop hello Azure DNS-tjänsten tooenumerate postuppsättningar i zonen hello och exportera zonfilen för hello resultat tooa BIND-kompatibel.</span><span class="sxs-lookup"><span data-stu-id="f063b-215">This command  calls hello Azure DNS service tooenumerate record sets in hello zone and export hello results tooa BIND-compatible zone file.</span></span>

    ```azurecli
    azure network dns zone export myresourcegroup contoso.com contoso.com.txt
    ```
