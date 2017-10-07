---
title: "aaaCreate anpassade DNS-poster för en webbapp | Microsoft Docs"
description: "Hur toocreate domänen DNS-poster för webbapp med Azure DNS."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 6c16608c-4819-44e7-ab88-306cf4d6efe5
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2016
ms.author: gwallace
ms.openlocfilehash: 070c808a55bab922eb624d99ae5c275d8eaa5aaa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-dns-records-for-a-web-app-in-a-custom-domain"></a><span data-ttu-id="75344-103">Skapa DNS-poster för ett webbprogram i en anpassad domän</span><span class="sxs-lookup"><span data-stu-id="75344-103">Create DNS records for a web app in a custom domain</span></span>

<span data-ttu-id="75344-104">Du kan använda Azure DNS toohost en anpassad domän för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="75344-104">You can use Azure DNS toohost a custom domain for your web apps.</span></span> <span data-ttu-id="75344-105">Till exempel du skapar en Azure webbapp och du vill att dina användare tooaccess den genom att antingen använda contoso.com eller www.contoso.com som ett fullständigt domännamn.</span><span class="sxs-lookup"><span data-stu-id="75344-105">For example, you are creating an Azure web app and you want your users tooaccess it by either using contoso.com, or www.contoso.com as an FQDN.</span></span>

<span data-ttu-id="75344-106">toodo, toocreate två poster:</span><span class="sxs-lookup"><span data-stu-id="75344-106">toodo this, you have toocreate two records:</span></span>

* <span data-ttu-id="75344-107">En rot ”A” poster peka toocontoso.com</span><span class="sxs-lookup"><span data-stu-id="75344-107">A root "A" record pointing toocontoso.com</span></span>
* <span data-ttu-id="75344-108">”CNAME”-posten för hello www namn som pekar toohello en post</span><span class="sxs-lookup"><span data-stu-id="75344-108">A "CNAME" record for hello www name that points toohello A record</span></span>

<span data-ttu-id="75344-109">Tänk på att om du skapar en A-post för en webbapp i Azure hello en post måste uppdateras manuellt om hello underliggande IP-adress för hello web app-ändringar.</span><span class="sxs-lookup"><span data-stu-id="75344-109">Keep in mind that if you create an A record for a web app in Azure, hello A record must be manually updated if hello underlying IP address for hello web app changes.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="75344-110">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="75344-110">Before you begin</span></span>

<span data-ttu-id="75344-111">Innan du börjar måste du först skapa en DNS-zon i Azure DNS och delegera hello zonen i din registrator tooAzure DNS.</span><span class="sxs-lookup"><span data-stu-id="75344-111">Before you begin, you must first create a DNS zone in Azure DNS, and delegate hello zone in your registrar tooAzure DNS.</span></span>

1. <span data-ttu-id="75344-112">toocreate en DNS-zon gör hello i [skapa en DNS-zon](dns-getstarted-create-dnszone.md).</span><span class="sxs-lookup"><span data-stu-id="75344-112">toocreate a DNS zone, follow hello steps in [Create a DNS zone](dns-getstarted-create-dnszone.md).</span></span>
2. <span data-ttu-id="75344-113">toodelegate din DNS-tooAzure DNS, så hello i [DNS-delegering i domänen](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="75344-113">toodelegate your DNS tooAzure DNS, follow hello steps in [DNS domain delegation](dns-domain-delegation.md).</span></span>

<span data-ttu-id="75344-114">När du skapar en zon och delegera den tooAzure DNS kan skapar du sedan poster för domänen.</span><span class="sxs-lookup"><span data-stu-id="75344-114">After creating a zone and delegating it tooAzure DNS, you can then create records for your custom domain.</span></span>

## <a name="1-create-an-a-record-for-your-custom-domain"></a><span data-ttu-id="75344-115">1. Skapa en A-post för den anpassade domänen</span><span class="sxs-lookup"><span data-stu-id="75344-115">1. Create an A record for your custom domain</span></span>

<span data-ttu-id="75344-116">En A-post är används toomap namn tooits IP-adress.</span><span class="sxs-lookup"><span data-stu-id="75344-116">An A record is used toomap a name tooits IP address.</span></span> <span data-ttu-id="75344-117">I följande exempel hello tilldelas vi som en A-post tooan IPv4-adress:</span><span class="sxs-lookup"><span data-stu-id="75344-117">In hello following example we will assign @ as an A record tooan IPv4 address:</span></span>

### <a name="step-1"></a><span data-ttu-id="75344-118">Steg 1</span><span class="sxs-lookup"><span data-stu-id="75344-118">Step 1</span></span>

<span data-ttu-id="75344-119">Skapa en A-post och tilldela tooa variabeln $rs</span><span class="sxs-lookup"><span data-stu-id="75344-119">Create an A record and assign tooa variable $rs</span></span>

```powershell
$rs= New-AzureRMDnsRecordSet -Name "@" -RecordType "A" -ZoneName "contoso.com" -ResourceGroupName "MyAzureResourceGroup" -Ttl 600
```

### <a name="step-2"></a><span data-ttu-id="75344-120">Steg 2</span><span class="sxs-lookup"><span data-stu-id="75344-120">Step 2</span></span>

<span data-ttu-id="75344-121">Lägg till hello IPv4 toohello tidigare skapade poster värdet ”@” Hej $rs används som tilldelats.</span><span class="sxs-lookup"><span data-stu-id="75344-121">Add hello IPv4 value toohello previously created record set "@" using hello $rs variable assigned.</span></span> <span data-ttu-id="75344-122">hello IPv4-värdet som tilldelas blir hello IP-adressen för din webbapp.</span><span class="sxs-lookup"><span data-stu-id="75344-122">hello IPv4 value assigned will be hello IP address for your web app.</span></span>

<span data-ttu-id="75344-123">toofind hello IP-adressen för en webbapp gör hello i [konfigurera ett anpassat domännamn i Azure App Service](../app-service-web/app-service-web-tutorial-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="75344-123">toofind hello IP address for a web app, follow hello steps in [Configure a custom domain name in Azure App Service](../app-service-web/app-service-web-tutorial-custom-domain.md).</span></span>

```powershell
Add-AzureRMDnsRecordConfig -RecordSet $rs -Ipv4Address "<your web app IP address>"
```

### <a name="step-3"></a><span data-ttu-id="75344-124">Steg 3</span><span class="sxs-lookup"><span data-stu-id="75344-124">Step 3</span></span>

<span data-ttu-id="75344-125">Genomför ändringar hello toohello postuppsättning.</span><span class="sxs-lookup"><span data-stu-id="75344-125">Commit hello changes toohello record set.</span></span> <span data-ttu-id="75344-126">Använd `Set-AzureRMDnsRecordSet` tooupload hello ändras toohello postuppsättning tooAzure DNS:</span><span class="sxs-lookup"><span data-stu-id="75344-126">Use `Set-AzureRMDnsRecordSet` tooupload hello changes toohello record set tooAzure DNS:</span></span>

```powershell
Set-AzureRMDnsRecordSet -RecordSet $rs
```

## <a name="2-create-a-cname-record-for-your-custom-domain"></a><span data-ttu-id="75344-127">2. Skapa en CNAME-post för den anpassade domänen</span><span class="sxs-lookup"><span data-stu-id="75344-127">2. Create a CNAME record for your custom domain</span></span>

<span data-ttu-id="75344-128">Om din domän hanteras redan av Azure DNS (se [DNS-delegering i domänen](dns-domain-delegation.md), kan du använda följande hello exempel toocreate en CNAME-post för contoso.azurewebsites.net hello.</span><span class="sxs-lookup"><span data-stu-id="75344-128">If your domain is already managed by Azure DNS (see [DNS domain delegation](dns-domain-delegation.md), you can use hello following hello example toocreate a CNAME record for contoso.azurewebsites.net.</span></span>

### <a name="step-1"></a><span data-ttu-id="75344-129">Steg 1</span><span class="sxs-lookup"><span data-stu-id="75344-129">Step 1</span></span>

<span data-ttu-id="75344-130">Öppna PowerShell och skapa en ny CNAME-postuppsättning och tilldelar tooa variabeln $rs.</span><span class="sxs-lookup"><span data-stu-id="75344-130">Open PowerShell and create a new CNAME record set and assign tooa variable $rs.</span></span> <span data-ttu-id="75344-131">Det här exemplet skapar en postuppsättning typen CNAME med en ”toolive” 600 sekunder i DNS-zonen med namnet ”contoso.com”.</span><span class="sxs-lookup"><span data-stu-id="75344-131">This example will create a record set type CNAME with a "time toolive" of 600 seconds in DNS zone named "contoso.com".</span></span>

```powershell
$rs = New-AzureRMDnsRecordSet -ZoneName contoso.com -ResourceGroupName myresourcegroup -Name "www" -RecordType "CNAME" -Ttl 600
```

<span data-ttu-id="75344-132">följande exempel hello är hello svar.</span><span class="sxs-lookup"><span data-stu-id="75344-132">hello following example is hello response.</span></span>

```
Name              : www
ZoneName          : contoso.com
ResourceGroupName : myresourcegroup
Ttl               : 600
Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
RecordType        : CNAME
Records           : {}
Tags              : {}
```

### <a name="step-2"></a><span data-ttu-id="75344-133">Steg 2</span><span class="sxs-lookup"><span data-stu-id="75344-133">Step 2</span></span>

<span data-ttu-id="75344-134">När du har skapat hello CNAME-postuppsättning måste toocreate ett Aliasvärde som pekar toohello webbprogram.</span><span class="sxs-lookup"><span data-stu-id="75344-134">Once hello CNAME record set is created, you need toocreate an alias value which will point toohello web app.</span></span>

<span data-ttu-id="75344-135">Med hjälp av hello som tidigare tilldelats variabeln ”$rs” kan du använda hello PowerShell-kommandot nedan toocreate hello-alias för hello web app contoso.azurewebsites.net.</span><span class="sxs-lookup"><span data-stu-id="75344-135">Using hello previously assigned variable "$rs" you can use hello PowerShell command below toocreate hello alias for hello web app contoso.azurewebsites.net.</span></span>

```powershell
Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "contoso.azurewebsites.net"
```

<span data-ttu-id="75344-136">följande exempel hello är hello svar.</span><span class="sxs-lookup"><span data-stu-id="75344-136">hello following example is hello response.</span></span>

```
    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {contoso.azurewebsites.net}
    Tags              : {}
```

### <a name="step-3"></a><span data-ttu-id="75344-137">Steg 3</span><span class="sxs-lookup"><span data-stu-id="75344-137">Step 3</span></span>

<span data-ttu-id="75344-138">Genomför hello ändringar med hjälp av hello `Set-AzureRMDnsRecordSet` cmdlet:</span><span class="sxs-lookup"><span data-stu-id="75344-138">Commit hello changes using hello `Set-AzureRMDnsRecordSet` cmdlet:</span></span>

```powershell
Set-AzureRMDnsRecordSet -RecordSet $rs
```

<span data-ttu-id="75344-139">Du kan validera hello posten skapades korrekt genom att fråga hello ”www.contoso.com” med nslookup, enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="75344-139">You can validate hello record was created correctly by querying hello "www.contoso.com" using nslookup, as shown below:</span></span>

```
PS C:\> nslookup
Default Server:  Default
Address:  192.168.0.1

> www.contoso.com
Server:  default server
Address:  192.168.0.1

Non-authoritative answer:
Name:    <instance of web app service>.cloudapp.net
Address:  <ip of web app service>
Aliases:  www.contoso.com
contoso.azurewebsites.net
<instance of web app service>.vip.azurewebsites.windows.net
```

## <a name="create-an-awverify-record-for-web-apps"></a><span data-ttu-id="75344-140">Skapa en ”awverify” post för webbprogram</span><span class="sxs-lookup"><span data-stu-id="75344-140">Create an "awverify" record for web apps</span></span>

<span data-ttu-id="75344-141">Om du väljer toouse en A-post för ditt webbprogram, måste du gå via en verifiering processen tooensure du egna hello anpassade domäner.</span><span class="sxs-lookup"><span data-stu-id="75344-141">If you decide toouse an A record for your web app, you must go through a verification process tooensure you own hello custom domain.</span></span> <span data-ttu-id="75344-142">Den här kontrollen görs genom att skapa en särskild CNAME-post med namnet ”awverify”.</span><span class="sxs-lookup"><span data-stu-id="75344-142">This verification step is done by creating a special CNAME record named "awverify".</span></span> <span data-ttu-id="75344-143">Det här avsnittet gäller enbart tooA poster.</span><span class="sxs-lookup"><span data-stu-id="75344-143">This section applies tooA records only.</span></span>

### <a name="step-1"></a><span data-ttu-id="75344-144">Steg 1</span><span class="sxs-lookup"><span data-stu-id="75344-144">Step 1</span></span>

<span data-ttu-id="75344-145">Skapa hello ”awverify” post.</span><span class="sxs-lookup"><span data-stu-id="75344-145">Create hello "awverify" record.</span></span> <span data-ttu-id="75344-146">I hello exemplet nedan skapar vi hello ”aweverify” post för contoso.com tooverify ägarskap för hello anpassade domäner.</span><span class="sxs-lookup"><span data-stu-id="75344-146">In hello example below, we will create hello "aweverify" record for contoso.com tooverify ownership for hello custom domain.</span></span>

```powershell
$rs = New-AzureRMDnsRecordSet -ZoneName "contoso.com" -ResourceGroupName "myresourcegroup" -Name "awverify" -RecordType "CNAME" -Ttl 600
```

<span data-ttu-id="75344-147">följande exempel hello är hello svar.</span><span class="sxs-lookup"><span data-stu-id="75344-147">hello following example is hello response.</span></span>

```
Name              : awverify
ZoneName          : contoso.com
ResourceGroupName : myresourcegroup
Ttl               : 600
Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
RecordType        : CNAME
Records           : {}
Tags              : {}
```

### <a name="step-2"></a><span data-ttu-id="75344-148">Steg 2</span><span class="sxs-lookup"><span data-stu-id="75344-148">Step 2</span></span>

<span data-ttu-id="75344-149">När hello postuppsättning ”awverify” har skapats kan du tilldela hello CNAME-postuppsättning alias.</span><span class="sxs-lookup"><span data-stu-id="75344-149">Once hello record set "awverify" is created, assign hello CNAME record set alias.</span></span> <span data-ttu-id="75344-150">I hello exemplet nedan, kommer vi tilldela hello CNAMe postuppsättning alias tooawverify.contoso.azurewebsites.net.</span><span class="sxs-lookup"><span data-stu-id="75344-150">In hello example below, we will assign hello CNAMe record set alias tooawverify.contoso.azurewebsites.net.</span></span>

```powershell
Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "awverify.contoso.azurewebsites.net"
```

<span data-ttu-id="75344-151">följande exempel hello är hello svar.</span><span class="sxs-lookup"><span data-stu-id="75344-151">hello following example is hello response.</span></span>

```
    Name              : awverify
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {awverify.contoso.azurewebsites.net}
    Tags              : {}
```

### <a name="step-3"></a><span data-ttu-id="75344-152">Steg 3</span><span class="sxs-lookup"><span data-stu-id="75344-152">Step 3</span></span>

<span data-ttu-id="75344-153">Genomför hello ändringar med hjälp av hello `Set-AzureRMDnsRecordSet cmdlet`som visas i hello kommandot nedan.</span><span class="sxs-lookup"><span data-stu-id="75344-153">Commit hello changes using hello `Set-AzureRMDnsRecordSet cmdlet`, as shown in hello command below.</span></span>

```powershell
Set-AzureRMDnsRecordSet -RecordSet $rs
```

## <a name="next-steps"></a><span data-ttu-id="75344-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="75344-154">Next steps</span></span>

<span data-ttu-id="75344-155">Gör så hello i [konfigurera ett anpassat domännamn för Apptjänst](../app-service-web/web-sites-custom-domain-name.md) tooconfigure din web app toouse anpassade domäner.</span><span class="sxs-lookup"><span data-stu-id="75344-155">Follow hello steps in [Configuring a custom domain name for App Service](../app-service-web/web-sites-custom-domain-name.md) tooconfigure your web app toouse a custom domain.</span></span>
