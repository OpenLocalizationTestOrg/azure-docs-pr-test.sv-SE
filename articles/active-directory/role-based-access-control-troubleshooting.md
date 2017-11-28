---
title: "Felsöka Azure RBAC | Microsoft Docs"
description: "Få hjälp med eller frågor om rollbaserad åtkomstkontroll resurser."
services: azure-portal
documentationcenter: na
author: andredm7
manager: femila
ms.assetid: df42cca2-02d6-4f3c-9d56-260e1eb7dc44
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.openlocfilehash: 407c030ea159915d4d7ac21760a3d17ec2204372
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="role-based-access-control-troubleshooting"></a><span data-ttu-id="81556-103">Rollbaserad åtkomstkontroll felsökning</span><span class="sxs-lookup"><span data-stu-id="81556-103">Role-Based Access Control troubleshooting</span></span>

<span data-ttu-id="81556-104">Det här dokumentet artikeln innehåller svar på vanliga frågor om särskilda behörigheter som beviljas med roller, så att du vet vad som händer när du använder roller i Azure-portalen och kan felsöka problem med åtkomst till.</span><span class="sxs-lookup"><span data-stu-id="81556-104">This document article answers common questions about the specific access rights that are granted with roles, so that you know what to expect when using the roles in the Azure portal and can troubleshoot access problems.</span></span> <span data-ttu-id="81556-105">Dessa roller omfattar alla typer av resurser:</span><span class="sxs-lookup"><span data-stu-id="81556-105">These three roles cover all resource types:</span></span>

* <span data-ttu-id="81556-106">Ägare</span><span class="sxs-lookup"><span data-stu-id="81556-106">Owner</span></span>  
* <span data-ttu-id="81556-107">Deltagare</span><span class="sxs-lookup"><span data-stu-id="81556-107">Contributor</span></span>  
* <span data-ttu-id="81556-108">Läsare</span><span class="sxs-lookup"><span data-stu-id="81556-108">Reader</span></span>  

<span data-ttu-id="81556-109">Ha fullständig åtkomst till hanteringen av ägare och deltagare, men en deltagare kan inte ge åtkomst till andra användare eller grupper.</span><span class="sxs-lookup"><span data-stu-id="81556-109">Owners and contributors both have full access to the management experience, but a contributor can’t give access to other users or groups.</span></span> <span data-ttu-id="81556-110">Det blir lite mer intressant med läsarrollen så att den där vi tillbringar lite tid.</span><span class="sxs-lookup"><span data-stu-id="81556-110">Things get a little more interesting with the reader role, so that’s where we'll spend some time.</span></span> <span data-ttu-id="81556-111">Finns det [rollbaserad åtkomstkontroll komma-igång artikel](role-based-access-control-configure.md) mer information om hur du beviljar åtkomst.</span><span class="sxs-lookup"><span data-stu-id="81556-111">See the [Role-Based Access Control get-started article](role-based-access-control-configure.md) for details on how to grant access.</span></span>

## <a name="app-service-workloads"></a><span data-ttu-id="81556-112">App service arbetsbelastningar</span><span class="sxs-lookup"><span data-stu-id="81556-112">App service workloads</span></span>
### <a name="write-access-capabilities"></a><span data-ttu-id="81556-113">Skrivåtkomst funktioner</span><span class="sxs-lookup"><span data-stu-id="81556-113">Write access capabilities</span></span>
<span data-ttu-id="81556-114">Om du ger en användare läsåtkomst till en enkel webbapp inaktiveras vissa funktioner som du kan förvänta inte sig.</span><span class="sxs-lookup"><span data-stu-id="81556-114">If you grant a user read-only access to a single web app, some features are disabled that you might not expect.</span></span> <span data-ttu-id="81556-115">Följande hanteringsfunktioner kräver **skriva** åtkomst till en webbapp (medarbetare eller ägare), och inte är tillgängliga i skrivskyddat läge scenarier.</span><span class="sxs-lookup"><span data-stu-id="81556-115">The following management capabilities require **write** access to a web app (either Contributor or Owner), and aren’t available in any read-only scenario.</span></span>

* <span data-ttu-id="81556-116">Kommandon (till exempel start, stopp, etc.)</span><span class="sxs-lookup"><span data-stu-id="81556-116">Commands (like start, stop, etc.)</span></span>
* <span data-ttu-id="81556-117">Ändra inställningar för allmän konfiguration, inställningar, inställningar för säkerhetskopiering och inställningarna för övervakning</span><span class="sxs-lookup"><span data-stu-id="81556-117">Changing settings like general configuration, scale settings, backup settings, and monitoring settings</span></span>
* <span data-ttu-id="81556-118">Åtkomst till publishing autentiseringsuppgifter och andra hemligheter som app-inställningar och anslutningssträngar</span><span class="sxs-lookup"><span data-stu-id="81556-118">Accessing publishing credentials and other secrets like app settings and connection strings</span></span>
* <span data-ttu-id="81556-119">Direktuppspelningsloggar</span><span class="sxs-lookup"><span data-stu-id="81556-119">Streaming logs</span></span>
* <span data-ttu-id="81556-120">Diagnostikloggar konfiguration</span><span class="sxs-lookup"><span data-stu-id="81556-120">Diagnostic logs configuration</span></span>
* <span data-ttu-id="81556-121">Konsol (Kommandotolken)</span><span class="sxs-lookup"><span data-stu-id="81556-121">Console (command prompt)</span></span>
* <span data-ttu-id="81556-122">Aktiva och senaste distributioner (för kontinuerlig lokal git-distribution)</span><span class="sxs-lookup"><span data-stu-id="81556-122">Active and recent deployments (for local git continuous deployment)</span></span>
* <span data-ttu-id="81556-123">Uppskattad tillbringar</span><span class="sxs-lookup"><span data-stu-id="81556-123">Estimated spend</span></span>
* <span data-ttu-id="81556-124">Webbtester</span><span class="sxs-lookup"><span data-stu-id="81556-124">Web tests</span></span>
* <span data-ttu-id="81556-125">Virtuella nätverk (endast visas för en läsare om ett virtuellt nätverk redan har konfigurerats av en användare med skrivbehörighet).</span><span class="sxs-lookup"><span data-stu-id="81556-125">Virtual network (only visible to a reader if a virtual network has previously been configured by a user with write access).</span></span>

<span data-ttu-id="81556-126">Om du inte kommer åt dessa paneler kan behöva du be administratören för deltagare åtkomst till webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="81556-126">If you can't access any of these tiles, you need to ask your administrator for Contributor access to the web app.</span></span>

### <a name="dealing-with-related-resources"></a><span data-ttu-id="81556-127">Hantera relaterade resurser</span><span class="sxs-lookup"><span data-stu-id="81556-127">Dealing with related resources</span></span>
<span data-ttu-id="81556-128">Webbprogram komplicerade av förekomsten av ett par olika resurser som samspelet.</span><span class="sxs-lookup"><span data-stu-id="81556-128">Web apps are complicated by the presence of a few different resources that interplay.</span></span> <span data-ttu-id="81556-129">Här är en typisk resursgrupp med några webbplatser:</span><span class="sxs-lookup"><span data-stu-id="81556-129">Here is a typical resource group with a couple websites:</span></span>

![Resursgruppen för Web app](./media/role-based-access-control-troubleshooting/website-resource-model.png)

<span data-ttu-id="81556-131">Därför, om du ger någon åtkomst till bara webbappen många funktioner på webbplatsen bladet i Azure-portalen är inaktiverat.</span><span class="sxs-lookup"><span data-stu-id="81556-131">As a result, if you grant someone access to just the web app, much of the functionality on the website blade in the Azure portal is disabled.</span></span>

<span data-ttu-id="81556-132">Dessa artiklar kräver **skriva** åtkomst till den **programtjänstplanen** som motsvarar din webbplats:</span><span class="sxs-lookup"><span data-stu-id="81556-132">These items require **write** access to the **App Service plan** that corresponds to your website:</span></span>  

* <span data-ttu-id="81556-133">Visa webbprogrammet har prisnivån (lediga eller Standard)</span><span class="sxs-lookup"><span data-stu-id="81556-133">Viewing the web app’s pricing tier (Free or Standard)</span></span>  
* <span data-ttu-id="81556-134">Skala configuration (antal instanser, storlek på virtuell dator, Autoskala inställningar)</span><span class="sxs-lookup"><span data-stu-id="81556-134">Scale configuration (number of instances, virtual machine size, autoscale settings)</span></span>  
* <span data-ttu-id="81556-135">Kvoter (lagring, bandbredd, CPU)</span><span class="sxs-lookup"><span data-stu-id="81556-135">Quotas (storage, bandwidth, CPU)</span></span>  

<span data-ttu-id="81556-136">Dessa artiklar kräver **skriva** åtkomst till hela **resursgruppen** som innehåller din webbplats:</span><span class="sxs-lookup"><span data-stu-id="81556-136">These items require **write** access to the whole **Resource group** that contains your website:</span></span>  

* <span data-ttu-id="81556-137">SSL-certifikat och bindningar (SSL-certifikat kan delas mellan platser i samma resursgrupp och geografiska plats)</span><span class="sxs-lookup"><span data-stu-id="81556-137">SSL Certificates and bindings (SSL certificates can be shared between sites in the same resource group and geo-location)</span></span>  
* <span data-ttu-id="81556-138">Aviseringsregler</span><span class="sxs-lookup"><span data-stu-id="81556-138">Alert rules</span></span>  
* <span data-ttu-id="81556-139">Autoskala inställningar</span><span class="sxs-lookup"><span data-stu-id="81556-139">Autoscale settings</span></span>  
* <span data-ttu-id="81556-140">Application insights-komponenter</span><span class="sxs-lookup"><span data-stu-id="81556-140">Application insights components</span></span>  
* <span data-ttu-id="81556-141">Webbtester</span><span class="sxs-lookup"><span data-stu-id="81556-141">Web tests</span></span>  

## <a name="virtual-machine-workloads"></a><span data-ttu-id="81556-142">Arbetsbelastningar på virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="81556-142">Virtual machine workloads</span></span>
<span data-ttu-id="81556-143">Mycket precis som med webbappar, vissa funktioner på bladet för virtuella datorer kräver skrivbehörighet till den virtuella datorn eller till andra resurser i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="81556-143">Much like with web apps, some features on the virtual machine blade require write access to the virtual machine, or to other resources in the resource group.</span></span>

<span data-ttu-id="81556-144">Virtuella datorer är relaterade till domänen namn, virtuella nätverk, storage-konton och Varningsregler.</span><span class="sxs-lookup"><span data-stu-id="81556-144">Virtual machines are related to Domain names, virtual networks, storage accounts, and alert rules.</span></span>

<span data-ttu-id="81556-145">Dessa artiklar kräver **skriva** åtkomst till den **virtuella**:</span><span class="sxs-lookup"><span data-stu-id="81556-145">These items require **write** access to the **Virtual machine**:</span></span>

* <span data-ttu-id="81556-146">Slutpunkter</span><span class="sxs-lookup"><span data-stu-id="81556-146">Endpoints</span></span>  
* <span data-ttu-id="81556-147">IP-adresser</span><span class="sxs-lookup"><span data-stu-id="81556-147">IP addresses</span></span>  
* <span data-ttu-id="81556-148">Diskar</span><span class="sxs-lookup"><span data-stu-id="81556-148">Disks</span></span>  
* <span data-ttu-id="81556-149">Tillägg</span><span class="sxs-lookup"><span data-stu-id="81556-149">Extensions</span></span>  

<span data-ttu-id="81556-150">Dessa kräver **skriva** åtkomst till både den **virtuella**, och **resursgruppen** (tillsammans med domännamn) som den är i:</span><span class="sxs-lookup"><span data-stu-id="81556-150">These require **write** access to both the **Virtual machine**, and the **Resource group** (along with the Domain name) that it is in:</span></span>  

* <span data-ttu-id="81556-151">Tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="81556-151">Availability set</span></span>  
* <span data-ttu-id="81556-152">Belastningsutjämnade uppsättningen</span><span class="sxs-lookup"><span data-stu-id="81556-152">Load balanced set</span></span>  
* <span data-ttu-id="81556-153">Aviseringsregler</span><span class="sxs-lookup"><span data-stu-id="81556-153">Alert rules</span></span>  

<span data-ttu-id="81556-154">Fråga din administratör om du inte kan komma åt någon av dessa paneler för deltagare åtkomst till resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="81556-154">If you can't access any of these tiles, ask your administrator for Contributor access to the Resource group.</span></span>

## <a name="see-more"></a><span data-ttu-id="81556-155">Mer information</span><span class="sxs-lookup"><span data-stu-id="81556-155">See more</span></span>
* <span data-ttu-id="81556-156">[Rollbaserad åtkomstkontroll](role-based-access-control-configure.md): komma igång med RBAC på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="81556-156">[Role Based Access Control](role-based-access-control-configure.md): Get started with RBAC in the Azure portal.</span></span>
* <span data-ttu-id="81556-157">[Inbyggda roller](role-based-access-built-in-roles.md): få information om de roller som levereras som standard i RBAC.</span><span class="sxs-lookup"><span data-stu-id="81556-157">[Built-in roles](role-based-access-built-in-roles.md): Get details about the roles that come standard in RBAC.</span></span>
* <span data-ttu-id="81556-158">[Anpassade roller i Azure RBAC](role-based-access-control-custom-roles.md): Lär dig att skapa anpassade roller som passar dina åtkomstbehov.</span><span class="sxs-lookup"><span data-stu-id="81556-158">[Custom roles in Azure RBAC](role-based-access-control-custom-roles.md): Learn how to create custom roles to fit your access needs.</span></span>
* <span data-ttu-id="81556-159">[Skapa en profil över åtkomständringshistorik](role-based-access-control-access-change-history-report.md): hålla reda på hur du ändrar rolltilldelningar i RBAC.</span><span class="sxs-lookup"><span data-stu-id="81556-159">[Create an access change history report](role-based-access-control-access-change-history-report.md): Keep track of changing role assignments in RBAC.</span></span>

