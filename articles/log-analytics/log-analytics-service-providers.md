---
title: "Logga analysfunktioner för tjänstleverantörer | Microsoft Docs"
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
ms.openlocfilehash: 8a67d9a9d345682e9e6c8f5c7779204a038f5f6a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="log-analytics-features-for-service-providers"></a><span data-ttu-id="912c1-103">Log Analytics funktioner för leverantörer</span><span class="sxs-lookup"><span data-stu-id="912c1-103">Log Analytics features for Service Providers</span></span>
<span data-ttu-id="912c1-104">Logganalys hjälper hanterade tjänsteleverantörer (MSPs), stora företag, oberoende programvaruleverantörer (ISV) och värdleverantörer hantera och övervaka servrar i kundens lokala eller molninfrastruktur.</span><span class="sxs-lookup"><span data-stu-id="912c1-104">Log Analytics can help managed service providers (MSPs), large enterprises, independent software vendors (ISVs), and hosting service providers manage and monitor servers in customer's on-premises or cloud infrastructure.</span></span> 

<span data-ttu-id="912c1-105">Stora företag dela många likheter med leverantörer, särskilt när det finns en central IT-teamet som ansvarar för att hantera IT-avdelningen för många olika affärsenheter.</span><span class="sxs-lookup"><span data-stu-id="912c1-105">Large enterprises share many similarities with service providers, particularly when there is a centralized IT team that is responsible for managing IT for many different business units.</span></span> <span data-ttu-id="912c1-106">För enkelhetens skull använder det här dokumentet termen *tjänstleverantör* men samma funktion är också tillgängligt för företag och andra kunder.</span><span class="sxs-lookup"><span data-stu-id="912c1-106">For simplicity, this document uses the term *service provider* but the same functionality is also available for enterprises and other customers.</span></span>

## <a name="cloud-solution-provider"></a><span data-ttu-id="912c1-107">Cloud Solution Provider</span><span class="sxs-lookup"><span data-stu-id="912c1-107">Cloud Solution Provider</span></span>
<span data-ttu-id="912c1-108">Partners och leverantörer som är en del av den [Cloud Solution Providers (CSP)](https://partner.microsoft.com/Solutions/cloud-reseller-overview) program, Log Analytics är en av Azure-tjänster på en CSP-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="912c1-108">For partners and service providers who are part of the [Cloud Solution Provider (CSP)](https://partner.microsoft.com/Solutions/cloud-reseller-overview) program, Log Analytics is one of the Azure services available on a CSP subscription.</span></span> 

<span data-ttu-id="912c1-109">För Log Analytics följande funktioner är aktiverade i *leverantör* prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="912c1-109">For Log Analytics, the following capabilities are enabled in *Cloud Solution Provider* subscriptions.</span></span>

<span data-ttu-id="912c1-110">Som en *leverantör* kan du:</span><span class="sxs-lookup"><span data-stu-id="912c1-110">As a *Cloud Solution Provider* you can:</span></span>

* <span data-ttu-id="912c1-111">Skapa logganalys arbetsytor i prenumeration för en klient (kundens).</span><span class="sxs-lookup"><span data-stu-id="912c1-111">Create Log Analytics workspaces in a tenant (customer's) subscription.</span></span>
* <span data-ttu-id="912c1-112">Åtkomst arbetsytor som skapats av klienter.</span><span class="sxs-lookup"><span data-stu-id="912c1-112">Access workspaces created by tenants.</span></span> 
* <span data-ttu-id="912c1-113">Lägg till och ta bort användaråtkomst till arbetsytan med Azure användarhantering.</span><span class="sxs-lookup"><span data-stu-id="912c1-113">Add and remove user access to the workspace using Azure user management.</span></span> <span data-ttu-id="912c1-114">När en klient arbetsytan i OMS-portalen på Användarhantering sidan inställningar är inte tillgängligt</span><span class="sxs-lookup"><span data-stu-id="912c1-114">When in a tenant’s workspace in the OMS portal the user management page under Settings is not available</span></span>
  * <span data-ttu-id="912c1-115">Log Analytics har inte stöd för rollbaserad åtkomst ännu -ge en användare `reader` i Azure-portalen tillåter dem att göra konfigurationsändringar i OMS-portalen</span><span class="sxs-lookup"><span data-stu-id="912c1-115">Log Analytics does not support role-based access yet - giving a user `reader` permission in the Azure portal allows them to make configuration changes in the OMS portal</span></span>

<span data-ttu-id="912c1-116">Du måste ange klient-ID för att logga in till en klient prenumeration.</span><span class="sxs-lookup"><span data-stu-id="912c1-116">To log in to a tenant’s subscription, you need to specify the tenant identifier.</span></span> <span data-ttu-id="912c1-117">Klient-ID är ofta den sista delen av e-postadressen används för att logga in.</span><span class="sxs-lookup"><span data-stu-id="912c1-117">The tenant identifier is often that last part of the e-mail address used to sign in.</span></span>

* <span data-ttu-id="912c1-118">Lägg till i OMS-portalen `?tenant=contoso.com` i URL: en för portalen.</span><span class="sxs-lookup"><span data-stu-id="912c1-118">In the OMS portal, add `?tenant=contoso.com` in the URL for the portal.</span></span> <span data-ttu-id="912c1-119">Till exempel, `mms.microsoft.com/?tenant=contoso.com`</span><span class="sxs-lookup"><span data-stu-id="912c1-119">For example, `mms.microsoft.com/?tenant=contoso.com`</span></span>
* <span data-ttu-id="912c1-120">I PowerShell använder den `-Tenant contoso.com` parameter när du använder `Add-AzureRmAccount` cmdlet</span><span class="sxs-lookup"><span data-stu-id="912c1-120">In PowerShell, use the `-Tenant contoso.com` parameter when using `Add-AzureRmAccount` cmdlet</span></span>
* <span data-ttu-id="912c1-121">Klient-ID läggs till automatiskt när du använder den `OMS portal` länk från Azure portal för att öppna och logga in på OMS-portalen för den valda arbetsytan</span><span class="sxs-lookup"><span data-stu-id="912c1-121">The tenant identifier is automatically added when you use the `OMS portal` link from the Azure portal to open and log in to the OMS portal for the selected workspace</span></span>

<span data-ttu-id="912c1-122">Som en *kunden* av en leverantör kan du:</span><span class="sxs-lookup"><span data-stu-id="912c1-122">As a *customer* of a Cloud Solution Provider you can:</span></span>

* <span data-ttu-id="912c1-123">Skapa log analytics arbetsytor i en CSP-prenumeration</span><span class="sxs-lookup"><span data-stu-id="912c1-123">Create log analytics workspaces in a CSP subscription</span></span>
* <span data-ttu-id="912c1-124">Åtkomst arbetsytor som skapats av Kryptografiprovidern</span><span class="sxs-lookup"><span data-stu-id="912c1-124">Access workspaces created by the CSP</span></span>
  * <span data-ttu-id="912c1-125">Använd den `OMS portal` länk från Azure portal för att öppna och logga in på OMS-portalen för den valda arbetsytan</span><span class="sxs-lookup"><span data-stu-id="912c1-125">Use the `OMS portal` link from the Azure portal to open and log in to the OMS portal for the selected workspace</span></span>
* <span data-ttu-id="912c1-126">Visa och använda sidan för användarhantering under inställningar i OMS-portalen</span><span class="sxs-lookup"><span data-stu-id="912c1-126">View and use the user management page under Settings in the OMS portal</span></span>

> [!NOTE]
> <span data-ttu-id="912c1-127">Med säkerhetskopiering och återställning av lösningar för Log Analytics kan inte ansluta till Recovery Services-valvet och inte kan konfigureras i en CSP-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="912c1-127">The included Backup and Site Recovery solutions for Log Analytics are not able to connect to a Recovery Services vault and cannot be configured in a CSP subscription.</span></span> 
> 
> 

## <a name="managing-multiple-customers-using-log-analytics"></a><span data-ttu-id="912c1-128">Hantera flera kunder som använder logganalys</span><span class="sxs-lookup"><span data-stu-id="912c1-128">Managing multiple customers using Log Analytics</span></span>
<span data-ttu-id="912c1-129">Vi rekommenderar att du skapar en logganalys-arbetsytan för varje kund som du hanterar.</span><span class="sxs-lookup"><span data-stu-id="912c1-129">It is recommended that you create a Log Analytics workspace for each customer you manage.</span></span> <span data-ttu-id="912c1-130">Logganalys-arbetsytan innehåller:</span><span class="sxs-lookup"><span data-stu-id="912c1-130">A Log Analytics workspace provides:</span></span>

* <span data-ttu-id="912c1-131">En geografisk plats för data som ska lagras.</span><span class="sxs-lookup"><span data-stu-id="912c1-131">A geographic location for data to be stored.</span></span> 
* <span data-ttu-id="912c1-132">Precision för fakturering</span><span class="sxs-lookup"><span data-stu-id="912c1-132">Granularity for billing</span></span> 
* <span data-ttu-id="912c1-133">Dataisolering</span><span class="sxs-lookup"><span data-stu-id="912c1-133">Data isolation</span></span> 
* <span data-ttu-id="912c1-134">Unik konfiguration</span><span class="sxs-lookup"><span data-stu-id="912c1-134">Unique configuration</span></span>

<span data-ttu-id="912c1-135">Genom att skapa en arbetsyta per kund kan kan du behålla data för varje kund separat och även spåra användningen av varje kund.</span><span class="sxs-lookup"><span data-stu-id="912c1-135">By creating a workspace per customer, you are able to keep each customer’s data separate and also track the usage of each customer.</span></span>

<span data-ttu-id="912c1-136">Mer information om när och varför ska du skapa flera arbetsytor beskrivs i [hantera åtkomst för att logga analytics](log-analytics-manage-access.md#determine-the-number-of-workspaces-you-need).</span><span class="sxs-lookup"><span data-stu-id="912c1-136">More details on when and why to create multiple workspaces is described in [manage access to log analytics](log-analytics-manage-access.md#determine-the-number-of-workspaces-you-need).</span></span>

<span data-ttu-id="912c1-137">Skapa och konfiguration av customer arbetsytor kan automatiseras med hjälp av [PowerShell](log-analytics-powershell-workspace-configuration.md), [Resource Manager-mallar](log-analytics-template-workspace-configuration.md), eller med hjälp av den [REST API](https://www.nuget.org/packages/Microsoft.Azure.Management.OperationalInsights/).</span><span class="sxs-lookup"><span data-stu-id="912c1-137">Creation and configuration of customer workspaces can be automated using [PowerShell](log-analytics-powershell-workspace-configuration.md), [Resource Manager templates](log-analytics-template-workspace-configuration.md), or using the [REST API](https://www.nuget.org/packages/Microsoft.Azure.Management.OperationalInsights/).</span></span>

<span data-ttu-id="912c1-138">Användningen av Resource Manager-mallar för arbetsytan konfiguration kan du ha en master-konfiguration som kan användas för att skapa och konfigurera arbetsytor.</span><span class="sxs-lookup"><span data-stu-id="912c1-138">The use of Resource Manager templates for workspace configuration allows you to have a master configuration that can be used to create and configure workspaces.</span></span> <span data-ttu-id="912c1-139">Du kan vara säker på att som arbetsytor skapas för kunder de konfigureras automatiskt enligt dina behov.</span><span class="sxs-lookup"><span data-stu-id="912c1-139">You can be confident that as workspaces are created for customers they are automatically configured to your requirements.</span></span> <span data-ttu-id="912c1-140">När du uppdaterar dina krav mallen uppdateras och sedan på nytt befintliga arbetsytor.</span><span class="sxs-lookup"><span data-stu-id="912c1-140">When you update your requirements, the template is updated and then reapplied the existing workspaces.</span></span> <span data-ttu-id="912c1-141">Den här processen säkerställer att även befintliga arbetsytor uppfyller dina nya.</span><span class="sxs-lookup"><span data-stu-id="912c1-141">This process ensures that even existing workspaces meet your new standards.</span></span>    

<span data-ttu-id="912c1-142">När du hanterar flera logganalys arbetsytor, rekommenderar vi integrerar varje arbetsyta med din befintliga biljettsystem / operations-konsolen använder den [aviseringar](log-analytics-alerts.md) funktioner.</span><span class="sxs-lookup"><span data-stu-id="912c1-142">When managing multiple Log Analytics workspaces, we recommend integrating each workspace with your existing ticketing system / operations console using the [Alerts](log-analytics-alerts.md) functionality.</span></span> <span data-ttu-id="912c1-143">Genom att integrera med befintliga system kan supportpersonal fortsätta att följa sina bekant processer.</span><span class="sxs-lookup"><span data-stu-id="912c1-143">By integrating with your existing systems, support staff can continue to follow their familiar processes.</span></span> <span data-ttu-id="912c1-144">Logganalys regelbundet kontrollerar varje arbetsyta mot aviseringsvillkoren som du anger och genererar en avisering när åtgärd krävs.</span><span class="sxs-lookup"><span data-stu-id="912c1-144">Log Analytics regularly checks each workspace against the alert criteria you specify and generates an alert when action is needed.</span></span>

<span data-ttu-id="912c1-145">Anpassade vyer av data använder den [instrumentpanelen](../azure-portal/azure-portal-dashboards.md) kapaciteten i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="912c1-145">For personalized views of data, use the [dashboard](../azure-portal/azure-portal-dashboards.md) capability in the Azure portal.</span></span>  

<span data-ttu-id="912c1-146">För verkställande nivån rapporter som summerar data över arbetsytor kan du använda integrationen mellan logganalys och [PowerBI](log-analytics-powerbi.md).</span><span class="sxs-lookup"><span data-stu-id="912c1-146">For executive level reports that summarize data across workspaces you can use the integration between Log Analytics and [PowerBI](log-analytics-powerbi.md).</span></span> <span data-ttu-id="912c1-147">Om du behöver integrera med en annan rapporteringssystem du kan använda Sök-API (via PowerShell eller [REST](log-analytics-log-search-api.md)) att köra frågor och exportera sökresultaten.</span><span class="sxs-lookup"><span data-stu-id="912c1-147">If you need to integrate with another reporting system, you can use the Search API (via PowerShell or [REST](log-analytics-log-search-api.md)) to run queries and export search results.</span></span>

## <a name="next-steps"></a><span data-ttu-id="912c1-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="912c1-148">Next Steps</span></span>
* <span data-ttu-id="912c1-149">Automatisera skapandet och konfiguration av arbetsytor med [Resource Manager-mallar](log-analytics-template-workspace-configuration.md)</span><span class="sxs-lookup"><span data-stu-id="912c1-149">Automate creation and configuration of workspaces using [Resource Manager templates](log-analytics-template-workspace-configuration.md)</span></span>
* <span data-ttu-id="912c1-150">Automatisera genereringen av arbetsytor med [PowerShell](log-analytics-powershell-workspace-configuration.md)</span><span class="sxs-lookup"><span data-stu-id="912c1-150">Automate creation of workspaces using [PowerShell](log-analytics-powershell-workspace-configuration.md)</span></span> 
* <span data-ttu-id="912c1-151">Använd [aviseringar](log-analytics-alerts.md) att integrera med befintliga system</span><span class="sxs-lookup"><span data-stu-id="912c1-151">Use [Alerts](log-analytics-alerts.md) to integrate with existing systems</span></span>
* <span data-ttu-id="912c1-152">Generera sammanfattningsrapporter med [PowerBI](log-analytics-powerbi.md)</span><span class="sxs-lookup"><span data-stu-id="912c1-152">Generate summary reports using [PowerBI](log-analytics-powerbi.md)</span></span>

