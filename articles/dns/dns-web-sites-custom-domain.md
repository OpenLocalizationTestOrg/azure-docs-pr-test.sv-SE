---
title: "Skapa anpassade DNS-poster för en webbapp | Microsoft Docs"
description: "Så här skapar du anpassade domäner DNS-poster för webbapp med Azure DNS."
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
ms.openlocfilehash: b054a41ecd69ee1c802d8403fe4b25128f016e3c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-dns-records-for-a-web-app-in-a-custom-domain"></a><span data-ttu-id="1ce37-103">Skapa DNS-poster för ett webbprogram i en anpassad domän</span><span class="sxs-lookup"><span data-stu-id="1ce37-103">Create DNS records for a web app in a custom domain</span></span>

<span data-ttu-id="1ce37-104">Du kan använda DNS för Azure som värd för en anpassad domän för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="1ce37-104">You can use Azure DNS to host a custom domain for your web apps.</span></span> <span data-ttu-id="1ce37-105">Till exempel du skapar en Azure webbapp och du vill att användarna ska komma åt den genom att antingen använda contoso.com eller www.contoso.com som ett fullständigt domännamn.</span><span class="sxs-lookup"><span data-stu-id="1ce37-105">For example, you are creating an Azure web app and you want your users to access it by either using contoso.com, or www.contoso.com as an FQDN.</span></span>

<span data-ttu-id="1ce37-106">Om du vill göra detta måste du skapa två poster:</span><span class="sxs-lookup"><span data-stu-id="1ce37-106">To do this, you have to create two records:</span></span>

* <span data-ttu-id="1ce37-107">En rot ”A”-post som pekar på contoso.com</span><span class="sxs-lookup"><span data-stu-id="1ce37-107">A root "A" record pointing to contoso.com</span></span>
* <span data-ttu-id="1ce37-108">En ”CNAME” för www-namn som pekar på A-post</span><span class="sxs-lookup"><span data-stu-id="1ce37-108">A "CNAME" record for the www name that points to the A record</span></span>

<span data-ttu-id="1ce37-109">Kom ihåg att om du skapar en A-post för en webbapp i Azure, A-posten måste vara manuellt uppdateras om den underliggande IP-adressen för web app-ändringar.</span><span class="sxs-lookup"><span data-stu-id="1ce37-109">Keep in mind that if you create an A record for a web app in Azure, the A record must be manually updated if the underlying IP address for the web app changes.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="1ce37-110">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="1ce37-110">Before you begin</span></span>

<span data-ttu-id="1ce37-111">Innan du börjar måste du först skapa en DNS-zon i Azure DNS och delegera zonen i Registratorn till Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="1ce37-111">Before you begin, you must first create a DNS zone in Azure DNS, and delegate the zone in your registrar to Azure DNS.</span></span>

1. <span data-ttu-id="1ce37-112">Om du vill skapa en DNS-zon, följer du stegen i [skapa en DNS-zon](dns-getstarted-create-dnszone.md).</span><span class="sxs-lookup"><span data-stu-id="1ce37-112">To create a DNS zone, follow the steps in [Create a DNS zone](dns-getstarted-create-dnszone.md).</span></span>
2. <span data-ttu-id="1ce37-113">Om du vill delegera din DNS-server till Azure DNS, följer du stegen i [DNS-delegering i domänen](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="1ce37-113">To delegate your DNS to Azure DNS, follow the steps in [DNS domain delegation](dns-domain-delegation.md).</span></span>

<span data-ttu-id="1ce37-114">När du skapar en zon och delegera till Azure DNS kan skapar du sedan poster för domänen.</span><span class="sxs-lookup"><span data-stu-id="1ce37-114">After creating a zone and delegating it to Azure DNS, you can then create records for your custom domain.</span></span>

## <a name="1-create-an-a-record-for-your-custom-domain"></a><span data-ttu-id="1ce37-115">1. Skapa en A-post för den anpassade domänen</span><span class="sxs-lookup"><span data-stu-id="1ce37-115">1. Create an A record for your custom domain</span></span>

<span data-ttu-id="1ce37-116">En A-post används för att mappa ett namn till dess IP-adress.</span><span class="sxs-lookup"><span data-stu-id="1ce37-116">An A record is used to map a name to its IP address.</span></span> <span data-ttu-id="1ce37-117">I följande exempel kommer vi tilldela som en A-post till en IPv4-adress:</span><span class="sxs-lookup"><span data-stu-id="1ce37-117">In the following example we will assign @ as an A record to an IPv4 address:</span></span>

### <a name="step-1"></a><span data-ttu-id="1ce37-118">Steg 1</span><span class="sxs-lookup"><span data-stu-id="1ce37-118">Step 1</span></span>

<span data-ttu-id="1ce37-119">Skapa en A-post och tilldela en variabel $rs</span><span class="sxs-lookup"><span data-stu-id="1ce37-119">Create an A record and assign to a variable $rs</span></span>

```powershell
$rs= New-AzureRMDnsRecordSet -Name "@" -RecordType "A" -ZoneName "contoso.com" -ResourceGroupName "MyAzureResourceGroup" -Ttl 600
```

### <a name="step-2"></a><span data-ttu-id="1ce37-120">Steg 2</span><span class="sxs-lookup"><span data-stu-id="1ce37-120">Step 2</span></span>

<span data-ttu-id="1ce37-121">Lägg till IPv4-värde i den tidigare skapade postuppsättningen ”@” med hjälp av variabeln $rs tilldelas.</span><span class="sxs-lookup"><span data-stu-id="1ce37-121">Add the IPv4 value to the previously created record set "@" using the $rs variable assigned.</span></span> <span data-ttu-id="1ce37-122">IPv4-värdet som tilldelas kommer att IP-adressen för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="1ce37-122">The IPv4 value assigned will be the IP address for your web app.</span></span>

<span data-ttu-id="1ce37-123">För att hitta IP-adressen för en webbapp, följer du stegen i [konfigurera ett anpassat domännamn i Azure App Service](../app-service-web/app-service-web-tutorial-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="1ce37-123">To find the IP address for a web app, follow the steps in [Configure a custom domain name in Azure App Service](../app-service-web/app-service-web-tutorial-custom-domain.md).</span></span>

```powershell
Add-AzureRMDnsRecordConfig -RecordSet $rs -Ipv4Address "<your web app IP address>"
```

### <a name="step-3"></a><span data-ttu-id="1ce37-124">Steg 3</span><span class="sxs-lookup"><span data-stu-id="1ce37-124">Step 3</span></span>

<span data-ttu-id="1ce37-125">Ändringarna i uppsättningen av poster.</span><span class="sxs-lookup"><span data-stu-id="1ce37-125">Commit the changes to the record set.</span></span> <span data-ttu-id="1ce37-126">Använd `Set-AzureRMDnsRecordSet` så att ändringarna till posten till Azure DNS:</span><span class="sxs-lookup"><span data-stu-id="1ce37-126">Use `Set-AzureRMDnsRecordSet` to upload the changes to the record set to Azure DNS:</span></span>

```powershell
Set-AzureRMDnsRecordSet -RecordSet $rs
```

## <a name="2-create-a-cname-record-for-your-custom-domain"></a><span data-ttu-id="1ce37-127">2. Skapa en CNAME-post för den anpassade domänen</span><span class="sxs-lookup"><span data-stu-id="1ce37-127">2. Create a CNAME record for your custom domain</span></span>

<span data-ttu-id="1ce37-128">Om din domän hanteras redan av Azure DNS (se [DNS-delegering i domänen](dns-domain-delegation.md), så kan du använda följande exempel för att skapa en CNAME-post för contoso.azurewebsites.net.</span><span class="sxs-lookup"><span data-stu-id="1ce37-128">If your domain is already managed by Azure DNS (see [DNS domain delegation](dns-domain-delegation.md), you can use the following the example to create a CNAME record for contoso.azurewebsites.net.</span></span>

### <a name="step-1"></a><span data-ttu-id="1ce37-129">Steg 1</span><span class="sxs-lookup"><span data-stu-id="1ce37-129">Step 1</span></span>

<span data-ttu-id="1ce37-130">Öppna PowerShell och skapa en ny CNAME-postuppsättning och tilldela en variabel $rs.</span><span class="sxs-lookup"><span data-stu-id="1ce37-130">Open PowerShell and create a new CNAME record set and assign to a variable $rs.</span></span> <span data-ttu-id="1ce37-131">Det här exemplet skapas en postuppsättning typen CNAME med en ”time to live” 600 sekunder i DNS-zonen med namnet ”contoso.com”.</span><span class="sxs-lookup"><span data-stu-id="1ce37-131">This example will create a record set type CNAME with a "time to live" of 600 seconds in DNS zone named "contoso.com".</span></span>

```powershell
$rs = New-AzureRMDnsRecordSet -ZoneName contoso.com -ResourceGroupName myresourcegroup -Name "www" -RecordType "CNAME" -Ttl 600
```

<span data-ttu-id="1ce37-132">Följande exempel är svaret.</span><span class="sxs-lookup"><span data-stu-id="1ce37-132">The following example is the response.</span></span>

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

### <a name="step-2"></a><span data-ttu-id="1ce37-133">Steg 2</span><span class="sxs-lookup"><span data-stu-id="1ce37-133">Step 2</span></span>

<span data-ttu-id="1ce37-134">När du har skapat CNAME-postuppsättning måste du skapa ett Aliasvärde som pekar på webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="1ce37-134">Once the CNAME record set is created, you need to create an alias value which will point to the web app.</span></span>

<span data-ttu-id="1ce37-135">Med hjälp av variabeln tidigare tilldelade ”$rs” använda du PowerShell-kommandot nedan för att skapa alias för web app contoso.azurewebsites.net.</span><span class="sxs-lookup"><span data-stu-id="1ce37-135">Using the previously assigned variable "$rs" you can use the PowerShell command below to create the alias for the web app contoso.azurewebsites.net.</span></span>

```powershell
Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "contoso.azurewebsites.net"
```

<span data-ttu-id="1ce37-136">Följande exempel är svaret.</span><span class="sxs-lookup"><span data-stu-id="1ce37-136">The following example is the response.</span></span>

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

### <a name="step-3"></a><span data-ttu-id="1ce37-137">Steg 3</span><span class="sxs-lookup"><span data-stu-id="1ce37-137">Step 3</span></span>

<span data-ttu-id="1ce37-138">Genomför ändringarna med det `Set-AzureRMDnsRecordSet` cmdlet:</span><span class="sxs-lookup"><span data-stu-id="1ce37-138">Commit the changes using the `Set-AzureRMDnsRecordSet` cmdlet:</span></span>

```powershell
Set-AzureRMDnsRecordSet -RecordSet $rs
```

<span data-ttu-id="1ce37-139">Du kan verifiera posten skapades korrekt genom att fråga den ”www.contoso.com” med nslookup, enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="1ce37-139">You can validate the record was created correctly by querying the "www.contoso.com" using nslookup, as shown below:</span></span>

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

## <a name="create-an-awverify-record-for-web-apps"></a><span data-ttu-id="1ce37-140">Skapa en ”awverify” post för webbprogram</span><span class="sxs-lookup"><span data-stu-id="1ce37-140">Create an "awverify" record for web apps</span></span>

<span data-ttu-id="1ce37-141">Om du vill använda en A-post för ditt webbprogram måste du gå igenom en verifieringsprocessen för att säkerställa att du äger den anpassade domänen.</span><span class="sxs-lookup"><span data-stu-id="1ce37-141">If you decide to use an A record for your web app, you must go through a verification process to ensure you own the custom domain.</span></span> <span data-ttu-id="1ce37-142">Den här kontrollen görs genom att skapa en särskild CNAME-post med namnet ”awverify”.</span><span class="sxs-lookup"><span data-stu-id="1ce37-142">This verification step is done by creating a special CNAME record named "awverify".</span></span> <span data-ttu-id="1ce37-143">Det här avsnittet gäller enbart poster.</span><span class="sxs-lookup"><span data-stu-id="1ce37-143">This section applies to A records only.</span></span>

### <a name="step-1"></a><span data-ttu-id="1ce37-144">Steg 1</span><span class="sxs-lookup"><span data-stu-id="1ce37-144">Step 1</span></span>

<span data-ttu-id="1ce37-145">Skapa posten ”awverify”.</span><span class="sxs-lookup"><span data-stu-id="1ce37-145">Create the "awverify" record.</span></span> <span data-ttu-id="1ce37-146">I exemplet nedan skapar vi ”aweverify” post för contoso.com verifiera ägarskapet för den anpassade domänen.</span><span class="sxs-lookup"><span data-stu-id="1ce37-146">In the example below, we will create the "aweverify" record for contoso.com to verify ownership for the custom domain.</span></span>

```powershell
$rs = New-AzureRMDnsRecordSet -ZoneName "contoso.com" -ResourceGroupName "myresourcegroup" -Name "awverify" -RecordType "CNAME" -Ttl 600
```

<span data-ttu-id="1ce37-147">Följande exempel är svaret.</span><span class="sxs-lookup"><span data-stu-id="1ce37-147">The following example is the response.</span></span>

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

### <a name="step-2"></a><span data-ttu-id="1ce37-148">Steg 2</span><span class="sxs-lookup"><span data-stu-id="1ce37-148">Step 2</span></span>

<span data-ttu-id="1ce37-149">Tilldela den CNAME-postuppsättning alias när postuppsättning ”awverify” har skapats.</span><span class="sxs-lookup"><span data-stu-id="1ce37-149">Once the record set "awverify" is created, assign the CNAME record set alias.</span></span> <span data-ttu-id="1ce37-150">I exemplet nedan tilldelas vi ange alias till awverify.contoso.azurewebsites.net CNAMe-posten.</span><span class="sxs-lookup"><span data-stu-id="1ce37-150">In the example below, we will assign the CNAMe record set alias to awverify.contoso.azurewebsites.net.</span></span>

```powershell
Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "awverify.contoso.azurewebsites.net"
```

<span data-ttu-id="1ce37-151">Följande exempel är svaret.</span><span class="sxs-lookup"><span data-stu-id="1ce37-151">The following example is the response.</span></span>

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

### <a name="step-3"></a><span data-ttu-id="1ce37-152">Steg 3</span><span class="sxs-lookup"><span data-stu-id="1ce37-152">Step 3</span></span>

<span data-ttu-id="1ce37-153">Genomför ändringarna med det `Set-AzureRMDnsRecordSet cmdlet`som visas i kommandot nedan.</span><span class="sxs-lookup"><span data-stu-id="1ce37-153">Commit the changes using the `Set-AzureRMDnsRecordSet cmdlet`, as shown in the command below.</span></span>

```powershell
Set-AzureRMDnsRecordSet -RecordSet $rs
```

## <a name="next-steps"></a><span data-ttu-id="1ce37-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1ce37-154">Next steps</span></span>

<span data-ttu-id="1ce37-155">Följ stegen i [konfigurera ett anpassat domännamn för Apptjänst](../app-service-web/web-sites-custom-domain-name.md) att konfigurera ditt webbprogram om du vill använda en anpassad domän.</span><span class="sxs-lookup"><span data-stu-id="1ce37-155">Follow the steps in [Configuring a custom domain name for App Service](../app-service-web/web-sites-custom-domain-name.md) to configure your web app to use a custom domain.</span></span>
