---
title: "Visa och ändra värdnamn | Microsoft Docs"
description: "Hur du visa och ändra värdnamn för virtuella Azure-datorer, webb- och arbetsroller för namnmatchning"
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
ms.openlocfilehash: 9a3a1e1b58dcb828e2d2d09c18f1aab6d46051aa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="viewing-and-modifying-hostnames"></a><span data-ttu-id="2b657-103">Visa och ändra värdnamn</span><span class="sxs-lookup"><span data-stu-id="2b657-103">Viewing and modifying hostnames</span></span>
<span data-ttu-id="2b657-104">Om du vill att dina rollinstanser refereras efter värdnamn, måste du ange värde för värddatorns namn i tjänstkonfigurationsfilen för varje roll.</span><span class="sxs-lookup"><span data-stu-id="2b657-104">To allow your role instances to be referenced by host name, you must set the value for the host name in the service configuration file for each role.</span></span> <span data-ttu-id="2b657-105">Det gör du genom att lägga till önskade värdnamnet som den **vmName** attribut för den **rollen** element.</span><span class="sxs-lookup"><span data-stu-id="2b657-105">You do that by adding the desired host name to the **vmName** attribute of the **Role** element.</span></span> <span data-ttu-id="2b657-106">Värdet för den **vmName** attributet används som bas för värdnamnet för varje rollinstans.</span><span class="sxs-lookup"><span data-stu-id="2b657-106">The value of the **vmName** attribute is used as a base for the host name of each role instance.</span></span> <span data-ttu-id="2b657-107">Till exempel om **vmName** är *webrole* och det finns tre instanser av rollen, värdnamn av instanserna blir *webrole0*, *webrole1*, och *webrole2*.</span><span class="sxs-lookup"><span data-stu-id="2b657-107">For example, if **vmName** is *webrole* and there are three instances of that role, the host names of the instances will be *webrole0*, *webrole1*, and *webrole2*.</span></span> <span data-ttu-id="2b657-108">Du behöver inte ange ett värdnamn för virtuella datorer i konfigurationsfilen, eftersom värdnamnet för en virtuell dator har fyllts i baserat på namnet på virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="2b657-108">You do not need to specify a host name for virtual machines in the configuration file, because the host name for a virtual machine is populated based on the virtual machine name.</span></span> <span data-ttu-id="2b657-109">Mer information om hur du konfigurerar en Microsoft Azure-tjänst finns [Konfigurationsschemat för Azure-tjänsten (.cscfg-filen)](https://msdn.microsoft.com/library/azure/ee758710.aspx)</span><span class="sxs-lookup"><span data-stu-id="2b657-109">For more information about configuring a Microsoft Azure service, see [Azure Service Configuration Schema (.cscfg File)](https://msdn.microsoft.com/library/azure/ee758710.aspx)</span></span>

## <a name="viewing-hostnames"></a><span data-ttu-id="2b657-110">Visa värdnamn</span><span class="sxs-lookup"><span data-stu-id="2b657-110">Viewing hostnames</span></span>
<span data-ttu-id="2b657-111">Du kan visa värdnamn för virtuella datorer och rollinstanser i en molntjänst med hjälp av verktygen nedan.</span><span class="sxs-lookup"><span data-stu-id="2b657-111">You can view the host names of virtual machines and role instances in a cloud service by using any of the tools below.</span></span>

### <a name="azure-portal"></a><span data-ttu-id="2b657-112">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2b657-112">Azure Portal</span></span>
<span data-ttu-id="2b657-113">Du kan använda den [Azure-portalen](http://portal.azure.com) visa värdnamn för virtuella datorer på bladet översikt för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="2b657-113">You can use the [Azure portal](http://portal.azure.com) to view the host names for virtual machines on the overview blade for a virtual machine.</span></span> <span data-ttu-id="2b657-114">Tänk på att bladet visar ett värde för **namn** och **värdnamn**.</span><span class="sxs-lookup"><span data-stu-id="2b657-114">Keep in mind that the blade shows a value for **Name** and **Host Name**.</span></span> <span data-ttu-id="2b657-115">Även om de är från början samma, ändrar ändra värdnamnet inte namnet på den virtuella datorn eller instansen.</span><span class="sxs-lookup"><span data-stu-id="2b657-115">Although they are initially the same, changing the host name will not change the name of the virtual machine or role instance.</span></span>

<span data-ttu-id="2b657-116">Rollinstanser kan även visas i Azure-portalen, men när du listar instanser i en molnbaserad tjänst värdnamnet visas inte.</span><span class="sxs-lookup"><span data-stu-id="2b657-116">Role instances can also be viewed in the Azure portal, but when you list the instances in a cloud service, the host name is not displayed.</span></span> <span data-ttu-id="2b657-117">Du ser ett namn för varje instans, men det här namnet representerar inte värdnamnet.</span><span class="sxs-lookup"><span data-stu-id="2b657-117">You will see a name for each instance, but that name does not represent the host name.</span></span>

### <a name="service-configuration-file"></a><span data-ttu-id="2b657-118">Tjänstkonfigurationsfil</span><span class="sxs-lookup"><span data-stu-id="2b657-118">Service configuration file</span></span>
<span data-ttu-id="2b657-119">Du kan hämta tjänstkonfigurationsfilen för en distribuerad tjänst från den **konfigurera** bladet för tjänsten i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2b657-119">You can download the service configuration file for a deployed service from the **Configure** blade of the service in the Azure portal.</span></span> <span data-ttu-id="2b657-120">Du kan sedan söka efter den **vmName** attributet för den **rollnamn** element att se värdnamnet.</span><span class="sxs-lookup"><span data-stu-id="2b657-120">You can then look for the **vmName** attribute for the **Role name** element to see the host name.</span></span> <span data-ttu-id="2b657-121">Tänk på att det här värdnamnet används som bas för värdnamnet för varje rollinstans.</span><span class="sxs-lookup"><span data-stu-id="2b657-121">Keep in mind that this host name is used as a base for the host name of each role instance.</span></span> <span data-ttu-id="2b657-122">Till exempel om **vmName** är *webrole* och det finns tre instanser av rollen, värdnamn av instanserna blir *webrole0*, *webrole1*, och *webrole2*.</span><span class="sxs-lookup"><span data-stu-id="2b657-122">For example, if **vmName** is *webrole* and there are three instances of that role, the host names of the instances will be *webrole0*, *webrole1*, and *webrole2*.</span></span>

### <a name="remote-desktop"></a><span data-ttu-id="2b657-123">Fjärrskrivbord</span><span class="sxs-lookup"><span data-stu-id="2b657-123">Remote Desktop</span></span>
<span data-ttu-id="2b657-124">När du aktiverar fjärrskrivbord (Windows), Windows PowerShell-fjärrkommunikation (Windows) eller SSH (Linux och Windows)-anslutningar till dina virtuella datorer eller rollinstanser, kan du visa värdnamnet från en aktiv anslutning till fjärrskrivbord på olika sätt:</span><span class="sxs-lookup"><span data-stu-id="2b657-124">After you enable Remote Desktop (Windows), Windows PowerShell remoting (Windows), or SSH (Linux and Windows) connections to your virtual machines or role instances, you can view the host name from an active Remote Desktop connection in various ways:</span></span>

* <span data-ttu-id="2b657-125">Ange värdnamnet i Kommandotolken eller SSH-terminal.</span><span class="sxs-lookup"><span data-stu-id="2b657-125">Type hostname at the command prompt or SSH terminal.</span></span>
* <span data-ttu-id="2b657-126">Skriv ipconfig/alla vid Kommandotolken (endast Windows).</span><span class="sxs-lookup"><span data-stu-id="2b657-126">Type ipconfig /all at the command prompt (Windows only).</span></span>
* <span data-ttu-id="2b657-127">Visa namnet på datorn i Systeminställningar (endast Windows).</span><span class="sxs-lookup"><span data-stu-id="2b657-127">View the computer name in the system settings (Windows only).</span></span>

### <a name="azure-service-management-rest-api"></a><span data-ttu-id="2b657-128">Azure Service Management REST API</span><span class="sxs-lookup"><span data-stu-id="2b657-128">Azure Service Management REST API</span></span>
<span data-ttu-id="2b657-129">Följ dessa instruktioner från en REST-klient:</span><span class="sxs-lookup"><span data-stu-id="2b657-129">From a REST client, follow these instructions:</span></span>

1. <span data-ttu-id="2b657-130">Se till att du har ett klientcertifikat för att ansluta till Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2b657-130">Ensure that you have a client certificate to connect to the Azure portal.</span></span> <span data-ttu-id="2b657-131">För att erhålla ett klientcertifikat följer du stegen som visas i [så här: hämta och importera Publiceringsinställningar och prenumerationsinformation](https://msdn.microsoft.com/library/dn385850.aspx).</span><span class="sxs-lookup"><span data-stu-id="2b657-131">To obtain a client certificate, follow the steps presented in [How to: Download and Import Publish Settings and Subscription Information](https://msdn.microsoft.com/library/dn385850.aspx).</span></span> 
2. <span data-ttu-id="2b657-132">Ange en rubrikpost med namnet x-ms-version med värdet 2013-11-01.</span><span class="sxs-lookup"><span data-stu-id="2b657-132">Set a header entry named x-ms-version with a value of 2013-11-01.</span></span>
3. <span data-ttu-id="2b657-133">Skicka en begäran i följande format: https://management.core.windows.net/\<subscrition-id\>/services/hostedservices/\<Tjänstenamn\>? bädda in detail = true</span><span class="sxs-lookup"><span data-stu-id="2b657-133">Send a request in the following format: https://management.core.windows.net/\<subscrition-id\>/services/hostedservices/\<service-name\>?embed-detail=true</span></span>
4. <span data-ttu-id="2b657-134">Leta efter den **värdnamn** element för varje **RoleInstance** element.</span><span class="sxs-lookup"><span data-stu-id="2b657-134">Look for the **HostName** element for each **RoleInstance** element.</span></span>

> [!WARNING]
> <span data-ttu-id="2b657-135">Du kan också visa interna domänsuffixet för din molntjänst från svaret för REST-anrop genom att kontrollera den **InternalDnsSuffix** elementet eller genom att köra ipconfig/alla från Kommandotolken i en fjärrskrivbordssession (Windows) eller genom att Kör cat /etc/resolv.conf från en terminal SSH (Linux).</span><span class="sxs-lookup"><span data-stu-id="2b657-135">You can also view the internal domain suffix for your cloud service from the REST call response by checking the **InternalDnsSuffix** element, or by running ipconfig /all from a command prompt in a Remote Desktop session (Windows), or by running cat /etc/resolv.conf from an SSH terminal (Linux).</span></span>
> 
> 

## <a name="modifying-a-hostname"></a><span data-ttu-id="2b657-136">Ändra ett värdnamn</span><span class="sxs-lookup"><span data-stu-id="2b657-136">Modifying a hostname</span></span>
<span data-ttu-id="2b657-137">Du kan ändra värdnamnet för en virtuell dator eller rollinstans genom att ladda upp en ändrade tjänstkonfigurationsfilen eller genom att byta namn på datorn från en fjärrskrivbordssession.</span><span class="sxs-lookup"><span data-stu-id="2b657-137">You can modify the host name for any virtual machine or role instance by uploading a modified service configuration file, or by renaming the computer from a Remote Desktop session.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2b657-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2b657-138">Next steps</span></span>
[<span data-ttu-id="2b657-139">Namnmatchning (DNS)</span><span class="sxs-lookup"><span data-stu-id="2b657-139">Name Resolution (DNS)</span></span>](virtual-networks-name-resolution-for-vms-and-role-instances.md)

[<span data-ttu-id="2b657-140">Konfigurationsschemat för Azure-tjänsten (.cscfg)</span><span class="sxs-lookup"><span data-stu-id="2b657-140">Azure Service Configuration Schema (.cscfg)</span></span>](https://msdn.microsoft.com/library/windowsazure/ee758710.aspx)

[<span data-ttu-id="2b657-141">Virtuella Azure-nätverket Konfigurationsschemat</span><span class="sxs-lookup"><span data-stu-id="2b657-141">Azure Virtual Network Configuration Schema</span></span>](http://go.microsoft.com/fwlink/?LinkId=248093)

[<span data-ttu-id="2b657-142">Ange DNS-inställningar med hjälp av network configuration-filer</span><span class="sxs-lookup"><span data-stu-id="2b657-142">Specify DNS settings using network configuration files</span></span>](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md)

