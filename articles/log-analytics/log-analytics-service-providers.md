---
title: "aaaLog analysfunktioner för tjänstleverantörer | Microsoft Docs"
description: "Logganalys hjälper hanteras tjänsteleverantörer (MSPs), stora företag oberoende programvara leverantörer (ISV) och värdleverantörer hantera och övervaka servrar i kundens lokala eller molninfrastruktur."
services: log-analytics
documentationcenter: 
author: richrundmsft
manager: jochan
editor: 
ms.assetid: c07f0b9f-ec37-480d-91ec-d9bcf6786464
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/22/2016
ms.author: richrund
ms.openlocfilehash: 3c0a93232293f90385c6c724be436cee1cf855f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="log-analytics-features-for-service-providers"></a><span data-ttu-id="3ff6b-103">Log Analytics funktioner för leverantörer</span><span class="sxs-lookup"><span data-stu-id="3ff6b-103">Log Analytics features for Service Providers</span></span>
<span data-ttu-id="3ff6b-104">Logganalys hjälper hanterade tjänsteleverantörer (MSPs), stora företag, oberoende programvaruleverantörer (ISV) och värdleverantörer hantera och övervaka servrar i kundens lokala eller molninfrastruktur.</span><span class="sxs-lookup"><span data-stu-id="3ff6b-104">Log Analytics can help managed service providers (MSPs), large enterprises, independent software vendors (ISVs), and hosting service providers manage and monitor servers in customer's on-premises or cloud infrastructure.</span></span> 

<span data-ttu-id="3ff6b-105">Stora företag dela många likheter med leverantörer, särskilt när det finns en central IT-teamet som ansvarar för att hantera IT-avdelningen för många olika affärsenheter.</span><span class="sxs-lookup"><span data-stu-id="3ff6b-105">Large enterprises share many similarities with service providers, particularly when there is a centralized IT team that is responsible for managing IT for many different business units.</span></span> <span data-ttu-id="3ff6b-106">För enkelhetens skull använder det här dokumentet hello termen *tjänstleverantör* men hello samma funktion är också tillgängligt för företag och andra kunder.</span><span class="sxs-lookup"><span data-stu-id="3ff6b-106">For simplicity, this document uses hello term *service provider* but hello same functionality is also available for enterprises and other customers.</span></span>

## <a name="cloud-solution-provider"></a><span data-ttu-id="3ff6b-107">Cloud Solution Provider</span><span class="sxs-lookup"><span data-stu-id="3ff6b-107">Cloud Solution Provider</span></span>
<span data-ttu-id="3ff6b-108">Partners och leverantörer som ingår i hello [Cloud Solution Providers (CSP)](https://partner.microsoft.com/Solutions/cloud-reseller-overview) programmet Log Analytics är en av hello Azure-tjänster på en CSP-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="3ff6b-108">For partners and service providers who are part of hello [Cloud Solution Provider (CSP)](https://partner.microsoft.com/Solutions/cloud-reseller-overview) program, Log Analytics is one of hello Azure services available on a CSP subscription.</span></span> 

<span data-ttu-id="3ff6b-109">För Log Analytics hello följande funktioner är aktiverade i *leverantör* prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="3ff6b-109">For Log Analytics, hello following capabilities are enabled in *Cloud Solution Provider* subscriptions.</span></span>

<span data-ttu-id="3ff6b-110">Som en *leverantör* kan du:</span><span class="sxs-lookup"><span data-stu-id="3ff6b-110">As a *Cloud Solution Provider* you can:</span></span>

* <span data-ttu-id="3ff6b-111">Skapa logganalys arbetsytor i prenumeration för en klient (kundens).</span><span class="sxs-lookup"><span data-stu-id="3ff6b-111">Create Log Analytics workspaces in a tenant (customer's) subscription.</span></span>
* <span data-ttu-id="3ff6b-112">Åtkomst arbetsytor som skapats av klienter.</span><span class="sxs-lookup"><span data-stu-id="3ff6b-112">Access workspaces created by tenants.</span></span> 
* <span data-ttu-id="3ff6b-113">Lägg till och ta bort användaren åtkomst toohello arbetsytan med hjälp av Azure användarhantering.</span><span class="sxs-lookup"><span data-stu-id="3ff6b-113">Add and remove user access toohello workspace using Azure user management.</span></span> <span data-ttu-id="3ff6b-114">När en klient arbetsytan i hello OMS portal hello användaren hanteringssidan under inställningar inte är tillgänglig</span><span class="sxs-lookup"><span data-stu-id="3ff6b-114">When in a tenant’s workspace in hello OMS portal hello user management page under Settings is not available</span></span>
  * <span data-ttu-id="3ff6b-115">Log Analytics har inte stöd för rollbaserad åtkomst ännu -ge en användare `reader` i hello Azure-portalen tillåter dem toomake konfigurationsändringar i hello OMS-portalen</span><span class="sxs-lookup"><span data-stu-id="3ff6b-115">Log Analytics does not support role-based access yet - giving a user `reader` permission in hello Azure portal allows them toomake configuration changes in hello OMS portal</span></span>

<span data-ttu-id="3ff6b-116">toolog i tooa innehavarens prenumeration behöver du toospecify hello klient identifierare.</span><span class="sxs-lookup"><span data-stu-id="3ff6b-116">toolog in tooa tenant’s subscription, you need toospecify hello tenant identifier.</span></span> <span data-ttu-id="3ff6b-117">hello klient-ID är ofta den sista delen av hello e-postadress används toosign i.</span><span class="sxs-lookup"><span data-stu-id="3ff6b-117">hello tenant identifier is often that last part of hello e-mail address used toosign in.</span></span>

* <span data-ttu-id="3ff6b-118">Hello OMS-portalen, lägga till `?tenant=contoso.com` i hello URL: en för hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="3ff6b-118">In hello OMS portal, add `?tenant=contoso.com` in hello URL for hello portal.</span></span> <span data-ttu-id="3ff6b-119">Till exempel, `mms.microsoft.com/?tenant=contoso.com`</span><span class="sxs-lookup"><span data-stu-id="3ff6b-119">For example, `mms.microsoft.com/?tenant=contoso.com`</span></span>
* <span data-ttu-id="3ff6b-120">Använd hello i PowerShell `-Tenant contoso.com` parameter när du använder `Add-AzureRmAccount` cmdlet</span><span class="sxs-lookup"><span data-stu-id="3ff6b-120">In PowerShell, use hello `-Tenant contoso.com` parameter when using `Add-AzureRmAccount` cmdlet</span></span>
* <span data-ttu-id="3ff6b-121">hello klient-ID läggs till automatiskt när du använder hello `OMS portal` länk från hello Azure portal tooopen och logga in toohello OMS-portalen för hello valda arbetsytan</span><span class="sxs-lookup"><span data-stu-id="3ff6b-121">hello tenant identifier is automatically added when you use hello `OMS portal` link from hello Azure portal tooopen and log in toohello OMS portal for hello selected workspace</span></span>

<span data-ttu-id="3ff6b-122">Som en *kunden* av en leverantör kan du:</span><span class="sxs-lookup"><span data-stu-id="3ff6b-122">As a *customer* of a Cloud Solution Provider you can:</span></span>

* <span data-ttu-id="3ff6b-123">Skapa log analytics arbetsytor i en CSP-prenumeration</span><span class="sxs-lookup"><span data-stu-id="3ff6b-123">Create log analytics workspaces in a CSP subscription</span></span>
* <span data-ttu-id="3ff6b-124">Åtkomst arbetsytor som skapats av hello CSP</span><span class="sxs-lookup"><span data-stu-id="3ff6b-124">Access workspaces created by hello CSP</span></span>
  * <span data-ttu-id="3ff6b-125">Använd hello `OMS portal` länk från hello Azure portal tooopen och logga in toohello OMS-portalen för hello valda arbetsytan</span><span class="sxs-lookup"><span data-stu-id="3ff6b-125">Use hello `OMS portal` link from hello Azure portal tooopen and log in toohello OMS portal for hello selected workspace</span></span>
* <span data-ttu-id="3ff6b-126">Visa och använda hello management användarsidan under inställningar i hello OMS-portalen</span><span class="sxs-lookup"><span data-stu-id="3ff6b-126">View and use hello user management page under Settings in hello OMS portal</span></span>

> [!NOTE]
> <span data-ttu-id="3ff6b-127">hello ingår säkerhetskopiering och Site Recovery-lösningar för Log Analytics är inte kan tooconnect tooa Recovery Services-valvet och inte kan konfigureras i en CSP-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="3ff6b-127">hello included Backup and Site Recovery solutions for Log Analytics are not able tooconnect tooa Recovery Services vault and cannot be configured in a CSP subscription.</span></span> 
> 
> 

## <a name="managing-multiple-customers-using-log-analytics"></a><span data-ttu-id="3ff6b-128">Hantera flera kunder som använder logganalys</span><span class="sxs-lookup"><span data-stu-id="3ff6b-128">Managing multiple customers using Log Analytics</span></span>
<span data-ttu-id="3ff6b-129">Vi rekommenderar att du skapar en logganalys-arbetsytan för varje kund som du hanterar.</span><span class="sxs-lookup"><span data-stu-id="3ff6b-129">It is recommended that you create a Log Analytics workspace for each customer you manage.</span></span> <span data-ttu-id="3ff6b-130">Logganalys-arbetsytan innehåller:</span><span class="sxs-lookup"><span data-stu-id="3ff6b-130">A Log Analytics workspace provides:</span></span>

* <span data-ttu-id="3ff6b-131">En geografisk plats för data toobe lagras.</span><span class="sxs-lookup"><span data-stu-id="3ff6b-131">A geographic location for data toobe stored.</span></span> 
* <span data-ttu-id="3ff6b-132">Precision för fakturering</span><span class="sxs-lookup"><span data-stu-id="3ff6b-132">Granularity for billing</span></span> 
* <span data-ttu-id="3ff6b-133">Dataisolering</span><span class="sxs-lookup"><span data-stu-id="3ff6b-133">Data isolation</span></span> 
* <span data-ttu-id="3ff6b-134">Unik konfiguration</span><span class="sxs-lookup"><span data-stu-id="3ff6b-134">Unique configuration</span></span>

<span data-ttu-id="3ff6b-135">Genom att skapa en arbetsyta per kund kan du kan tookeep varje kund data separat och även spåra hello användning av varje kund.</span><span class="sxs-lookup"><span data-stu-id="3ff6b-135">By creating a workspace per customer, you are able tookeep each customer’s data separate and also track hello usage of each customer.</span></span>

<span data-ttu-id="3ff6b-136">Mer information om när och varför toocreate flera arbetsytor beskrivs i [hantera åtkomst toolog analytics](log-analytics-manage-access.md#determine-the-number-of-workspaces-you-need).</span><span class="sxs-lookup"><span data-stu-id="3ff6b-136">More details on when and why toocreate multiple workspaces is described in [manage access toolog analytics](log-analytics-manage-access.md#determine-the-number-of-workspaces-you-need).</span></span>

<span data-ttu-id="3ff6b-137">Skapa och konfiguration av customer arbetsytor kan automatiseras med hjälp av [PowerShell](log-analytics-powershell-workspace-configuration.md), [Resource Manager-mallar](log-analytics-template-workspace-configuration.md), eller med hjälp av hello [REST API](https://www.nuget.org/packages/Microsoft.Azure.Management.OperationalInsights/).</span><span class="sxs-lookup"><span data-stu-id="3ff6b-137">Creation and configuration of customer workspaces can be automated using [PowerShell](log-analytics-powershell-workspace-configuration.md), [Resource Manager templates](log-analytics-template-workspace-configuration.md), or using hello [REST API](https://www.nuget.org/packages/Microsoft.Azure.Management.OperationalInsights/).</span></span>

<span data-ttu-id="3ff6b-138">hello kan med Resource Manager-mallar för arbetsytan konfiguration du toohave en master konfiguration som kan använda toocreate och konfigurera arbetsytor.</span><span class="sxs-lookup"><span data-stu-id="3ff6b-138">hello use of Resource Manager templates for workspace configuration allows you toohave a master configuration that can be used toocreate and configure workspaces.</span></span> <span data-ttu-id="3ff6b-139">Du kan vara säker på att som arbetsytor skapas för kunder som de är automatiskt konfigurerade tooyour krav.</span><span class="sxs-lookup"><span data-stu-id="3ff6b-139">You can be confident that as workspaces are created for customers they are automatically configured tooyour requirements.</span></span> <span data-ttu-id="3ff6b-140">När du uppdaterar dina krav hello mallen uppdateras och sedan in igen hello befintliga arbetsytor.</span><span class="sxs-lookup"><span data-stu-id="3ff6b-140">When you update your requirements, hello template is updated and then reapplied hello existing workspaces.</span></span> <span data-ttu-id="3ff6b-141">Den här processen säkerställer att även befintliga arbetsytor uppfyller dina nya.</span><span class="sxs-lookup"><span data-stu-id="3ff6b-141">This process ensures that even existing workspaces meet your new standards.</span></span>    

<span data-ttu-id="3ff6b-142">När du hanterar flera logganalys arbetsytor, rekommenderar vi integrerar varje arbetsyta med din befintliga biljettsystem / operations-konsolen använder hello [aviseringar](log-analytics-alerts.md) funktioner.</span><span class="sxs-lookup"><span data-stu-id="3ff6b-142">When managing multiple Log Analytics workspaces, we recommend integrating each workspace with your existing ticketing system / operations console using hello [Alerts](log-analytics-alerts.md) functionality.</span></span> <span data-ttu-id="3ff6b-143">Genom att integrera med befintliga system kan supportpersonal toofollow fortsätta sina bekant processer.</span><span class="sxs-lookup"><span data-stu-id="3ff6b-143">By integrating with your existing systems, support staff can continue toofollow their familiar processes.</span></span> <span data-ttu-id="3ff6b-144">Logganalys regelbundet kontrollerar varje arbetsyta mot hello aviseringsvillkoren som du anger och genererar en avisering när åtgärd krävs.</span><span class="sxs-lookup"><span data-stu-id="3ff6b-144">Log Analytics regularly checks each workspace against hello alert criteria you specify and generates an alert when action is needed.</span></span>

<span data-ttu-id="3ff6b-145">För anpassade vyer av data använder du hello [instrumentpanelen](../azure-portal/azure-portal-dashboards.md) funktionen hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3ff6b-145">For personalized views of data, use hello [dashboard](../azure-portal/azure-portal-dashboards.md) capability in hello Azure portal.</span></span>  

<span data-ttu-id="3ff6b-146">För verkställande nivå rapporter som summerar data över arbetsytor som du kan använda hello integrering mellan logganalys och [PowerBI](log-analytics-powerbi.md).</span><span class="sxs-lookup"><span data-stu-id="3ff6b-146">For executive level reports that summarize data across workspaces you can use hello integration between Log Analytics and [PowerBI](log-analytics-powerbi.md).</span></span> <span data-ttu-id="3ff6b-147">Om du behöver toointegrate med ett annat reporting system, kan du använda hello Sök-API (via PowerShell eller [REST](log-analytics-log-search-api.md)) toorun frågor och exportera sökresultat.</span><span class="sxs-lookup"><span data-stu-id="3ff6b-147">If you need toointegrate with another reporting system, you can use hello Search API (via PowerShell or [REST](log-analytics-log-search-api.md)) toorun queries and export search results.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3ff6b-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3ff6b-148">Next Steps</span></span>
* <span data-ttu-id="3ff6b-149">Automatisera skapandet och konfiguration av arbetsytor med [Resource Manager-mallar](log-analytics-template-workspace-configuration.md)</span><span class="sxs-lookup"><span data-stu-id="3ff6b-149">Automate creation and configuration of workspaces using [Resource Manager templates](log-analytics-template-workspace-configuration.md)</span></span>
* <span data-ttu-id="3ff6b-150">Automatisera genereringen av arbetsytor med [PowerShell](log-analytics-powershell-workspace-configuration.md)</span><span class="sxs-lookup"><span data-stu-id="3ff6b-150">Automate creation of workspaces using [PowerShell](log-analytics-powershell-workspace-configuration.md)</span></span> 
* <span data-ttu-id="3ff6b-151">Använd [aviseringar](log-analytics-alerts.md) toointegrate med befintliga system</span><span class="sxs-lookup"><span data-stu-id="3ff6b-151">Use [Alerts](log-analytics-alerts.md) toointegrate with existing systems</span></span>
* <span data-ttu-id="3ff6b-152">Generera sammanfattningsrapporter med [PowerBI](log-analytics-powerbi.md)</span><span class="sxs-lookup"><span data-stu-id="3ff6b-152">Generate summary reports using [PowerBI](log-analytics-powerbi.md)</span></span>

