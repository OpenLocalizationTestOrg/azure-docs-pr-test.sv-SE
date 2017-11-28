---
title: "aaaViewing och ändra värdnamn | Microsoft Docs"
description: "Hur tooview och ändra värdnamn för Azure-datorer, webb- och arbetsroller för namnmatchning"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: c668cd8e-4e43-4d05-acc3-db64fa78d828
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2016
ms.author: jdial
ms.openlocfilehash: 17d0dd7911754a94db3f37b924b4687da1c70aca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="viewing-and-modifying-hostnames"></a><span data-ttu-id="5c431-103">Visa och ändra värdnamn</span><span class="sxs-lookup"><span data-stu-id="5c431-103">Viewing and modifying hostnames</span></span>
<span data-ttu-id="5c431-104">tooallow din roll instanser toobe refereras av värdnamn, måste du ange hello värde för hello värdnamn i hello tjänstkonfigurationsfilen för varje roll.</span><span class="sxs-lookup"><span data-stu-id="5c431-104">tooallow your role instances toobe referenced by host name, you must set hello value for hello host name in hello service configuration file for each role.</span></span> <span data-ttu-id="5c431-105">Det gör du genom att lägga till hello önskade värden namnet toohello **vmName** attribut för hello **rollen** element.</span><span class="sxs-lookup"><span data-stu-id="5c431-105">You do that by adding hello desired host name toohello **vmName** attribute of hello **Role** element.</span></span> <span data-ttu-id="5c431-106">Hej värdet för hello **vmName** attributet används som bas för hello värdnamnet för varje rollinstans.</span><span class="sxs-lookup"><span data-stu-id="5c431-106">hello value of hello **vmName** attribute is used as a base for hello host name of each role instance.</span></span> <span data-ttu-id="5c431-107">Till exempel om **vmName** är *webrole* och det finns tre instanser av rollen, hello värdnamn hello instanser *webrole0*, *webrole1 *, och *webrole2*.</span><span class="sxs-lookup"><span data-stu-id="5c431-107">For example, if **vmName** is *webrole* and there are three instances of that role, hello host names of hello instances will be *webrole0*, *webrole1*, and *webrole2*.</span></span> <span data-ttu-id="5c431-108">Du behöver inte toospecify ett värdnamn för virtuella datorer i hello konfigurationsfilen eftersom hello värdnamnet för en virtuell dator har fyllts i baserat på hello namn på virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="5c431-108">You do not need toospecify a host name for virtual machines in hello configuration file, because hello host name for a virtual machine is populated based on hello virtual machine name.</span></span> <span data-ttu-id="5c431-109">Mer information om hur du konfigurerar en Microsoft Azure-tjänst finns [Konfigurationsschemat för Azure-tjänsten (.cscfg-filen)](https://msdn.microsoft.com/library/azure/ee758710.aspx)</span><span class="sxs-lookup"><span data-stu-id="5c431-109">For more information about configuring a Microsoft Azure service, see [Azure Service Configuration Schema (.cscfg File)](https://msdn.microsoft.com/library/azure/ee758710.aspx)</span></span>

## <a name="viewing-hostnames"></a><span data-ttu-id="5c431-110">Visa värdnamn</span><span class="sxs-lookup"><span data-stu-id="5c431-110">Viewing hostnames</span></span>
<span data-ttu-id="5c431-111">Du kan visa hello värdnamn för virtuella datorer och rollinstanser i en molntjänst med hjälp av hello verktygen nedan.</span><span class="sxs-lookup"><span data-stu-id="5c431-111">You can view hello host names of virtual machines and role instances in a cloud service by using any of hello tools below.</span></span>

### <a name="azure-portal"></a><span data-ttu-id="5c431-112">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="5c431-112">Azure Portal</span></span>
<span data-ttu-id="5c431-113">Du kan använda hello [Azure-portalen](http://portal.azure.com) tooview hello värdnamn för virtuella datorer på hello översikt bladet för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="5c431-113">You can use hello [Azure portal](http://portal.azure.com) tooview hello host names for virtual machines on hello overview blade for a virtual machine.</span></span> <span data-ttu-id="5c431-114">Tänk på att hello bladet visar ett värde för **namn** och **värdnamn**.</span><span class="sxs-lookup"><span data-stu-id="5c431-114">Keep in mind that hello blade shows a value for **Name** and **Host Name**.</span></span> <span data-ttu-id="5c431-115">Trots att de ursprungligen hello samma ändra hello värdnamnet inte att ändra hello namnet på hello virtuell dator eller rollinstans.</span><span class="sxs-lookup"><span data-stu-id="5c431-115">Although they are initially hello same, changing hello host name will not change hello name of hello virtual machine or role instance.</span></span>

<span data-ttu-id="5c431-116">Rollinstanser kan även visas i hello Azure-portalen, men när du listar hello-instanser i en molnbaserad tjänst hello värdnamn visas inte.</span><span class="sxs-lookup"><span data-stu-id="5c431-116">Role instances can also be viewed in hello Azure portal, but when you list hello instances in a cloud service, hello host name is not displayed.</span></span> <span data-ttu-id="5c431-117">Du ser ett namn för varje instans, men det här namnet representerar inte hello värdnamn.</span><span class="sxs-lookup"><span data-stu-id="5c431-117">You will see a name for each instance, but that name does not represent hello host name.</span></span>

### <a name="service-configuration-file"></a><span data-ttu-id="5c431-118">Tjänstkonfigurationsfil</span><span class="sxs-lookup"><span data-stu-id="5c431-118">Service configuration file</span></span>
<span data-ttu-id="5c431-119">Du kan hämta hello tjänstkonfigurationsfilen för en distribuerad tjänst från hello **konfigurera** bladet hello tjänsterna i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="5c431-119">You can download hello service configuration file for a deployed service from hello **Configure** blade of hello service in hello Azure portal.</span></span> <span data-ttu-id="5c431-120">Du kan sedan söka efter hello **vmName** attribut för hello **rollnamn** elementet toosee hello värdnamn.</span><span class="sxs-lookup"><span data-stu-id="5c431-120">You can then look for hello **vmName** attribute for hello **Role name** element toosee hello host name.</span></span> <span data-ttu-id="5c431-121">Tänk på att det här värdnamnet används som bas för hello värdnamnet för varje rollinstans.</span><span class="sxs-lookup"><span data-stu-id="5c431-121">Keep in mind that this host name is used as a base for hello host name of each role instance.</span></span> <span data-ttu-id="5c431-122">Till exempel om **vmName** är *webrole* och det finns tre instanser av rollen, hello värdnamn hello instanser *webrole0*, *webrole1 *, och *webrole2*.</span><span class="sxs-lookup"><span data-stu-id="5c431-122">For example, if **vmName** is *webrole* and there are three instances of that role, hello host names of hello instances will be *webrole0*, *webrole1*, and *webrole2*.</span></span>

### <a name="remote-desktop"></a><span data-ttu-id="5c431-123">Fjärrskrivbord</span><span class="sxs-lookup"><span data-stu-id="5c431-123">Remote Desktop</span></span>
<span data-ttu-id="5c431-124">När du aktiverar fjärrskrivbord (Windows), Windows PowerShell-fjärrkommunikation (Windows) eller SSH (Linux och Windows) anslutningar tooyour virtuella datorer eller rollinstanser, kan du visa hello värdnamn från en aktiv anslutning till fjärrskrivbord på olika sätt:</span><span class="sxs-lookup"><span data-stu-id="5c431-124">After you enable Remote Desktop (Windows), Windows PowerShell remoting (Windows), or SSH (Linux and Windows) connections tooyour virtual machines or role instances, you can view hello host name from an active Remote Desktop connection in various ways:</span></span>

* <span data-ttu-id="5c431-125">Skriv värdnamn vid hello kommandotolk eller SSH-terminal.</span><span class="sxs-lookup"><span data-stu-id="5c431-125">Type hostname at hello command prompt or SSH terminal.</span></span>
* <span data-ttu-id="5c431-126">Skriv ipconfig/alla i hello kommandot Kommandotolken (endast Windows).</span><span class="sxs-lookup"><span data-stu-id="5c431-126">Type ipconfig /all at hello command prompt (Windows only).</span></span>
* <span data-ttu-id="5c431-127">Visa hello datornamn i hello system inställningar (Windows).</span><span class="sxs-lookup"><span data-stu-id="5c431-127">View hello computer name in hello system settings (Windows only).</span></span>

### <a name="azure-service-management-rest-api"></a><span data-ttu-id="5c431-128">Azure Service Management REST API</span><span class="sxs-lookup"><span data-stu-id="5c431-128">Azure Service Management REST API</span></span>
<span data-ttu-id="5c431-129">Följ dessa instruktioner från en REST-klient:</span><span class="sxs-lookup"><span data-stu-id="5c431-129">From a REST client, follow these instructions:</span></span>

1. <span data-ttu-id="5c431-130">Se till att du har en klient certifikat tooconnect toohello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="5c431-130">Ensure that you have a client certificate tooconnect toohello Azure portal.</span></span> <span data-ttu-id="5c431-131">tooobtain ett klientcertifikat gör hello presenteras i [så här: hämta och importera Publiceringsinställningar och prenumerationsinformation](https://msdn.microsoft.com/library/dn385850.aspx).</span><span class="sxs-lookup"><span data-stu-id="5c431-131">tooobtain a client certificate, follow hello steps presented in [How to: Download and Import Publish Settings and Subscription Information](https://msdn.microsoft.com/library/dn385850.aspx).</span></span> 
2. <span data-ttu-id="5c431-132">Ange en rubrikpost med namnet x-ms-version med värdet 2013-11-01.</span><span class="sxs-lookup"><span data-stu-id="5c431-132">Set a header entry named x-ms-version with a value of 2013-11-01.</span></span>
3. <span data-ttu-id="5c431-133">Skicka en begäran i hello följande format: https://management.core.windows.net/\<subscrition-id\>/services/hostedservices/\<Tjänstenamn\>? bädda in detail = true</span><span class="sxs-lookup"><span data-stu-id="5c431-133">Send a request in hello following format: https://management.core.windows.net/\<subscrition-id\>/services/hostedservices/\<service-name\>?embed-detail=true</span></span>
4. <span data-ttu-id="5c431-134">Leta efter hello **värdnamn** element för varje **RoleInstance** element.</span><span class="sxs-lookup"><span data-stu-id="5c431-134">Look for hello **HostName** element for each **RoleInstance** element.</span></span>

> [!WARNING]
> <span data-ttu-id="5c431-135">Du kan också visa hello interna domänsuffixet för din molntjänst från hello REST-anrop svar genom att kontrollera hello **InternalDnsSuffix** elementet eller genom att köra ipconfig/allt från en kommandotolk i en fjärrskrivbordssession (Windows) eller genom att köra cat /etc/resolv.conf från en SSH-terminal (Linux).</span><span class="sxs-lookup"><span data-stu-id="5c431-135">You can also view hello internal domain suffix for your cloud service from hello REST call response by checking hello **InternalDnsSuffix** element, or by running ipconfig /all from a command prompt in a Remote Desktop session (Windows), or by running cat /etc/resolv.conf from an SSH terminal (Linux).</span></span>
> 
> 

## <a name="modifying-a-hostname"></a><span data-ttu-id="5c431-136">Ändra ett värdnamn</span><span class="sxs-lookup"><span data-stu-id="5c431-136">Modifying a hostname</span></span>
<span data-ttu-id="5c431-137">Du kan ändra hello värdnamn för virtuell dator eller rollinstans genom att ladda upp en ändrade tjänstkonfigurationsfilen eller genom att byta namn på hello dator från en fjärrskrivbordssession.</span><span class="sxs-lookup"><span data-stu-id="5c431-137">You can modify hello host name for any virtual machine or role instance by uploading a modified service configuration file, or by renaming hello computer from a Remote Desktop session.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5c431-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5c431-138">Next steps</span></span>
[<span data-ttu-id="5c431-139">Namnmatchning (DNS)</span><span class="sxs-lookup"><span data-stu-id="5c431-139">Name Resolution (DNS)</span></span>](virtual-networks-name-resolution-for-vms-and-role-instances.md)

[<span data-ttu-id="5c431-140">Konfigurationsschemat för Azure-tjänsten (.cscfg)</span><span class="sxs-lookup"><span data-stu-id="5c431-140">Azure Service Configuration Schema (.cscfg)</span></span>](https://msdn.microsoft.com/library/windowsazure/ee758710.aspx)

[<span data-ttu-id="5c431-141">Virtuella Azure-nätverket Konfigurationsschemat</span><span class="sxs-lookup"><span data-stu-id="5c431-141">Azure Virtual Network Configuration Schema</span></span>](http://go.microsoft.com/fwlink/?LinkId=248093)

[<span data-ttu-id="5c431-142">Ange DNS-inställningar med hjälp av network configuration-filer</span><span class="sxs-lookup"><span data-stu-id="5c431-142">Specify DNS settings using network configuration files</span></span>](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md)

