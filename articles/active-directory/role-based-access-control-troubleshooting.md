---
title: aaaTroubleshoot Azure RBAC | Microsoft Docs
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
ms.openlocfilehash: 15feced32d8459d90c4c246d335932f90e1fc91f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="role-based-access-control-troubleshooting"></a><span data-ttu-id="40da3-103">Rollbaserad åtkomstkontroll felsökning</span><span class="sxs-lookup"><span data-stu-id="40da3-103">Role-Based Access Control troubleshooting</span></span>

<span data-ttu-id="40da3-104">Det här dokumentet artikeln innehåller svar på vanliga frågor om hello särskilda behörigheter som beviljas med roller, så att du vet vilka tooexpect när använder hello roller i hello Azure-portalen och kan felsöka problem med åtkomst till.</span><span class="sxs-lookup"><span data-stu-id="40da3-104">This document article answers common questions about hello specific access rights that are granted with roles, so that you know what tooexpect when using hello roles in hello Azure portal and can troubleshoot access problems.</span></span> <span data-ttu-id="40da3-105">Dessa roller omfattar alla typer av resurser:</span><span class="sxs-lookup"><span data-stu-id="40da3-105">These three roles cover all resource types:</span></span>

* <span data-ttu-id="40da3-106">Ägare</span><span class="sxs-lookup"><span data-stu-id="40da3-106">Owner</span></span>  
* <span data-ttu-id="40da3-107">Deltagare</span><span class="sxs-lookup"><span data-stu-id="40da3-107">Contributor</span></span>  
* <span data-ttu-id="40da3-108">Läsare</span><span class="sxs-lookup"><span data-stu-id="40da3-108">Reader</span></span>  

<span data-ttu-id="40da3-109">Ägare och deltagare har fullständig åtkomst toohello management upplevelse, men en deltagare som inte kan ge åtkomst tooother användare eller grupper.</span><span class="sxs-lookup"><span data-stu-id="40da3-109">Owners and contributors both have full access toohello management experience, but a contributor can’t give access tooother users or groups.</span></span> <span data-ttu-id="40da3-110">Det blir lite mer intressant med rollen för hello läsare så att den där vi tillbringar lite tid.</span><span class="sxs-lookup"><span data-stu-id="40da3-110">Things get a little more interesting with hello reader role, so that’s where we'll spend some time.</span></span> <span data-ttu-id="40da3-111">Se hello [rollbaserad åtkomstkontroll komma-igång artikel](role-based-access-control-configure.md) information om hur toogrant åt.</span><span class="sxs-lookup"><span data-stu-id="40da3-111">See hello [Role-Based Access Control get-started article](role-based-access-control-configure.md) for details on how toogrant access.</span></span>

## <a name="app-service-workloads"></a><span data-ttu-id="40da3-112">App service arbetsbelastningar</span><span class="sxs-lookup"><span data-stu-id="40da3-112">App service workloads</span></span>
### <a name="write-access-capabilities"></a><span data-ttu-id="40da3-113">Skrivåtkomst funktioner</span><span class="sxs-lookup"><span data-stu-id="40da3-113">Write access capabilities</span></span>
<span data-ttu-id="40da3-114">Om du ger en användare läsåtkomst tooa enkel webbapp inaktiveras vissa funktioner som du kan förvänta inte sig.</span><span class="sxs-lookup"><span data-stu-id="40da3-114">If you grant a user read-only access tooa single web app, some features are disabled that you might not expect.</span></span> <span data-ttu-id="40da3-115">följande funktioner för hantering av hello kräver **skriva** komma åt tooa webbprogram (medarbetare eller ägare) och inte är tillgängliga i skrivskyddat läge scenarier.</span><span class="sxs-lookup"><span data-stu-id="40da3-115">hello following management capabilities require **write** access tooa web app (either Contributor or Owner), and aren’t available in any read-only scenario.</span></span>

* <span data-ttu-id="40da3-116">Kommandon (till exempel start, stopp, etc.)</span><span class="sxs-lookup"><span data-stu-id="40da3-116">Commands (like start, stop, etc.)</span></span>
* <span data-ttu-id="40da3-117">Ändra inställningar för allmän konfiguration, inställningar, inställningar för säkerhetskopiering och inställningarna för övervakning</span><span class="sxs-lookup"><span data-stu-id="40da3-117">Changing settings like general configuration, scale settings, backup settings, and monitoring settings</span></span>
* <span data-ttu-id="40da3-118">Åtkomst till publishing autentiseringsuppgifter och andra hemligheter som app-inställningar och anslutningssträngar</span><span class="sxs-lookup"><span data-stu-id="40da3-118">Accessing publishing credentials and other secrets like app settings and connection strings</span></span>
* <span data-ttu-id="40da3-119">Direktuppspelningsloggar</span><span class="sxs-lookup"><span data-stu-id="40da3-119">Streaming logs</span></span>
* <span data-ttu-id="40da3-120">Diagnostikloggar konfiguration</span><span class="sxs-lookup"><span data-stu-id="40da3-120">Diagnostic logs configuration</span></span>
* <span data-ttu-id="40da3-121">Konsol (Kommandotolken)</span><span class="sxs-lookup"><span data-stu-id="40da3-121">Console (command prompt)</span></span>
* <span data-ttu-id="40da3-122">Aktiva och senaste distributioner (för kontinuerlig lokal git-distribution)</span><span class="sxs-lookup"><span data-stu-id="40da3-122">Active and recent deployments (for local git continuous deployment)</span></span>
* <span data-ttu-id="40da3-123">Uppskattad tillbringar</span><span class="sxs-lookup"><span data-stu-id="40da3-123">Estimated spend</span></span>
* <span data-ttu-id="40da3-124">Webbtester</span><span class="sxs-lookup"><span data-stu-id="40da3-124">Web tests</span></span>
* <span data-ttu-id="40da3-125">Virtuella nätverk (endast synliga tooa läsare om ett virtuellt nätverk redan har konfigurerats av en användare med skrivbehörighet).</span><span class="sxs-lookup"><span data-stu-id="40da3-125">Virtual network (only visible tooa reader if a virtual network has previously been configured by a user with write access).</span></span>

<span data-ttu-id="40da3-126">Om du inte kommer åt dessa paneler, måste tooask administratören för deltagare åtkomst toohello webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="40da3-126">If you can't access any of these tiles, you need tooask your administrator for Contributor access toohello web app.</span></span>

### <a name="dealing-with-related-resources"></a><span data-ttu-id="40da3-127">Hantera relaterade resurser</span><span class="sxs-lookup"><span data-stu-id="40da3-127">Dealing with related resources</span></span>
<span data-ttu-id="40da3-128">Webbprogram komplicerade av hello förekomsten av ett par olika resurser som samspelet.</span><span class="sxs-lookup"><span data-stu-id="40da3-128">Web apps are complicated by hello presence of a few different resources that interplay.</span></span> <span data-ttu-id="40da3-129">Här är en typisk resursgrupp med några webbplatser:</span><span class="sxs-lookup"><span data-stu-id="40da3-129">Here is a typical resource group with a couple websites:</span></span>

![Resursgruppen för Web app](./media/role-based-access-control-troubleshooting/website-resource-model.png)

<span data-ttu-id="40da3-131">Därför om du ger någon åtkomst toojust hello-webbprogram, är mycket hello funktioner hello webbplatsens blad i hello Azure-portalen inaktiverad.</span><span class="sxs-lookup"><span data-stu-id="40da3-131">As a result, if you grant someone access toojust hello web app, much of hello functionality on hello website blade in hello Azure portal is disabled.</span></span>

<span data-ttu-id="40da3-132">Dessa artiklar kräver **skriva** komma åt toohello **programtjänstplanen** som motsvarar tooyour webbplats:</span><span class="sxs-lookup"><span data-stu-id="40da3-132">These items require **write** access toohello **App Service plan** that corresponds tooyour website:</span></span>  

* <span data-ttu-id="40da3-133">Visa hello webbprogrammet prisnivån (lediga eller Standard)</span><span class="sxs-lookup"><span data-stu-id="40da3-133">Viewing hello web app’s pricing tier (Free or Standard)</span></span>  
* <span data-ttu-id="40da3-134">Skala configuration (antal instanser, storlek på virtuell dator, Autoskala inställningar)</span><span class="sxs-lookup"><span data-stu-id="40da3-134">Scale configuration (number of instances, virtual machine size, autoscale settings)</span></span>  
* <span data-ttu-id="40da3-135">Kvoter (lagring, bandbredd, CPU)</span><span class="sxs-lookup"><span data-stu-id="40da3-135">Quotas (storage, bandwidth, CPU)</span></span>  

<span data-ttu-id="40da3-136">Dessa artiklar kräver **skriva** åtkomst toohello hela **resursgruppen** som innehåller din webbplats:</span><span class="sxs-lookup"><span data-stu-id="40da3-136">These items require **write** access toohello whole **Resource group** that contains your website:</span></span>  

* <span data-ttu-id="40da3-137">SSL-certifikat och bindningar (SSL-certifikat kan delas mellan platser i hello samma resursgrupp och geografiska plats)</span><span class="sxs-lookup"><span data-stu-id="40da3-137">SSL Certificates and bindings (SSL certificates can be shared between sites in hello same resource group and geo-location)</span></span>  
* <span data-ttu-id="40da3-138">Aviseringsregler</span><span class="sxs-lookup"><span data-stu-id="40da3-138">Alert rules</span></span>  
* <span data-ttu-id="40da3-139">Autoskala inställningar</span><span class="sxs-lookup"><span data-stu-id="40da3-139">Autoscale settings</span></span>  
* <span data-ttu-id="40da3-140">Application insights-komponenter</span><span class="sxs-lookup"><span data-stu-id="40da3-140">Application insights components</span></span>  
* <span data-ttu-id="40da3-141">Webbtester</span><span class="sxs-lookup"><span data-stu-id="40da3-141">Web tests</span></span>  

## <a name="virtual-machine-workloads"></a><span data-ttu-id="40da3-142">Arbetsbelastningar på virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="40da3-142">Virtual machine workloads</span></span>
<span data-ttu-id="40da3-143">Mycket som med web apps vissa funktioner på hello virtuella bladet kräver skrivbehörighet toohello virtuell dator eller tooother resurser i hello resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="40da3-143">Much like with web apps, some features on hello virtual machine blade require write access toohello virtual machine, or tooother resources in hello resource group.</span></span>

<span data-ttu-id="40da3-144">Virtuella datorer är relaterade tooDomain namn, virtuella nätverk, storage-konton och Varningsregler.</span><span class="sxs-lookup"><span data-stu-id="40da3-144">Virtual machines are related tooDomain names, virtual networks, storage accounts, and alert rules.</span></span>

<span data-ttu-id="40da3-145">Dessa artiklar kräver **skriva** komma åt toohello **virtuella**:</span><span class="sxs-lookup"><span data-stu-id="40da3-145">These items require **write** access toohello **Virtual machine**:</span></span>

* <span data-ttu-id="40da3-146">Slutpunkter</span><span class="sxs-lookup"><span data-stu-id="40da3-146">Endpoints</span></span>  
* <span data-ttu-id="40da3-147">IP-adresser</span><span class="sxs-lookup"><span data-stu-id="40da3-147">IP addresses</span></span>  
* <span data-ttu-id="40da3-148">Diskar</span><span class="sxs-lookup"><span data-stu-id="40da3-148">Disks</span></span>  
* <span data-ttu-id="40da3-149">Tillägg</span><span class="sxs-lookup"><span data-stu-id="40da3-149">Extensions</span></span>  

<span data-ttu-id="40da3-150">Dessa kräver **skriva** åtkomst tooboth hello **virtuella**, och hello **resursgruppen** (tillsammans med hello domännamn) som den är i:</span><span class="sxs-lookup"><span data-stu-id="40da3-150">These require **write** access tooboth hello **Virtual machine**, and hello **Resource group** (along with hello Domain name) that it is in:</span></span>  

* <span data-ttu-id="40da3-151">Tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="40da3-151">Availability set</span></span>  
* <span data-ttu-id="40da3-152">Belastningsutjämnade uppsättningen</span><span class="sxs-lookup"><span data-stu-id="40da3-152">Load balanced set</span></span>  
* <span data-ttu-id="40da3-153">Aviseringsregler</span><span class="sxs-lookup"><span data-stu-id="40da3-153">Alert rules</span></span>  

<span data-ttu-id="40da3-154">Om du inte kommer åt dessa paneler kan du be administratören för deltagare åtkomst toohello resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="40da3-154">If you can't access any of these tiles, ask your administrator for Contributor access toohello Resource group.</span></span>

## <a name="see-more"></a><span data-ttu-id="40da3-155">Mer information</span><span class="sxs-lookup"><span data-stu-id="40da3-155">See more</span></span>
* <span data-ttu-id="40da3-156">[Rollbaserad åtkomstkontroll](role-based-access-control-configure.md): komma igång med RBAC på hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="40da3-156">[Role Based Access Control](role-based-access-control-configure.md): Get started with RBAC in hello Azure portal.</span></span>
* <span data-ttu-id="40da3-157">[Inbyggda roller](role-based-access-built-in-roles.md): få information om hello roller som redan finns i RBAC.</span><span class="sxs-lookup"><span data-stu-id="40da3-157">[Built-in roles](role-based-access-built-in-roles.md): Get details about hello roles that come standard in RBAC.</span></span>
* <span data-ttu-id="40da3-158">[Anpassade roller i Azure RBAC](role-based-access-control-custom-roles.md): Lär dig hur toocreate anpassade roller toofit din behövs.</span><span class="sxs-lookup"><span data-stu-id="40da3-158">[Custom roles in Azure RBAC](role-based-access-control-custom-roles.md): Learn how toocreate custom roles toofit your access needs.</span></span>
* <span data-ttu-id="40da3-159">[Skapa en profil över åtkomständringshistorik](role-based-access-control-access-change-history-report.md): hålla reda på hur du ändrar rolltilldelningar i RBAC.</span><span class="sxs-lookup"><span data-stu-id="40da3-159">[Create an access change history report](role-based-access-control-access-change-history-report.md): Keep track of changing role assignments in RBAC.</span></span>

