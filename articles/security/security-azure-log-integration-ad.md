---
title: aaaAzure Log-integrering med Azure Active Directory-granskningsloggar | Microsoft Docs
description: "Lär dig hur tooinstall hello Azure Log integrationstjänsten och integrera loggar från Azure-granskningsloggar"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomShinder
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ums.workload: na
ms.date: 08/08/2017
ms.author: barclayn
ms.custom: azlog
ms.openlocfilehash: 3ee8fa3b8b5e9bd33202e57ed5327cd8d3127f00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-active-directory-audit-logs"></a><span data-ttu-id="e369f-103">Integrera Azure Active Directory-granskningsloggar</span><span class="sxs-lookup"><span data-stu-id="e369f-103">Integrate Azure Active Directory audit logs</span></span>

<span data-ttu-id="e369f-104">Granskningshändelser för Azure Active Directory (AD Azure) hjälper dig att identifiera Privilegierade åtgärder som har inträffat i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e369f-104">Azure Active Directory (Azure AD) audit events help you identify privileged actions that occurred in Azure Active Directory.</span></span> <span data-ttu-id="e369f-105">Du kan se hello typer av händelser som du kan följa genom att granska [Azure Active Directory-granskningsrapporthändelser](/active-directory/active-directory-reporting-audit-events#list-of-audit-report-events.md).</span><span class="sxs-lookup"><span data-stu-id="e369f-105">You can see hello types of events that you can track by reviewing [Azure Active Directory audit report events](/active-directory/active-directory-reporting-audit-events#list-of-audit-report-events.md).</span></span>

> [!NOTE]
> <span data-ttu-id="e369f-106">Innan du försöker hello stegen i den här artikeln, bör du granska hello [Kom igång](security-azure-log-integration-get-started.md) artikel och slutföra hello stegen.</span><span class="sxs-lookup"><span data-stu-id="e369f-106">Before you attempt hello steps in this article, you must review hello [Get started](security-azure-log-integration-get-started.md) article and complete hello steps there.</span></span>

## <a name="steps-toointegrate-azure-active-directory-audit-logs"></a><span data-ttu-id="e369f-107">Steg toointegrate Azure Active directory-granskningsloggar</span><span class="sxs-lookup"><span data-stu-id="e369f-107">Steps toointegrate Azure Active directory audit logs</span></span>

1. <span data-ttu-id="e369f-108">Öppna hello-kommandotolk och kör det här kommandot:</span><span class="sxs-lookup"><span data-stu-id="e369f-108">Open hello command prompt and run this command:</span></span>

   ``cd c:\Program Files\Microsoft Azure Log Integration``

2. <span data-ttu-id="e369f-109">Kör följande kommando:</span><span class="sxs-lookup"><span data-stu-id="e369f-109">Run this command:</span></span> 
 
   ``azlog createazureid``

   <span data-ttu-id="e369f-110">Det här kommandot uppmanas du att din Azure-inloggning.</span><span class="sxs-lookup"><span data-stu-id="e369f-110">This command prompts you for your Azure login.</span></span> <span data-ttu-id="e369f-111">hello kommando skapar sedan ett Azure Active Directory tjänstens huvudnamn i hello Azure AD-klienter som är värdar för hello Azure-prenumerationer som hello inloggade användare är administratör, en medadministratör eller en ägare.</span><span class="sxs-lookup"><span data-stu-id="e369f-111">hello command then creates an Azure Active Directory service principal in hello Azure AD tenants that host hello Azure subscriptions in which hello logged-in user is an administrator, a co-administrator, or an owner.</span></span> <span data-ttu-id="e369f-112">hello kommandot misslyckas om hello inloggade användaren är endast gästanvändaren i hello Azure AD-klient.</span><span class="sxs-lookup"><span data-stu-id="e369f-112">hello command will fail if hello logged-in user is only a guest user in hello Azure AD tenant.</span></span> <span data-ttu-id="e369f-113">Autentisering tooAzure görs via Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e369f-113">Authentication tooAzure is done through Azure AD.</span></span> <span data-ttu-id="e369f-114">Skapa ett huvudnamn för tjänsten för Azure Log integrering skapar hello Azure AD identity som ges åtkomst tooread från Azure-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="e369f-114">Creating a service principal for Azure Log Integration creates hello Azure AD identity that is given access tooread from Azure subscriptions.</span></span>

3. <span data-ttu-id="e369f-115">Kör följande kommando tooprovide hello klient-ID.</span><span class="sxs-lookup"><span data-stu-id="e369f-115">Run hello following command tooprovide your tenant ID.</span></span> <span data-ttu-id="e369f-116">Du måste toobe tillhör hello klient admin rollen toorun hello kommando.</span><span class="sxs-lookup"><span data-stu-id="e369f-116">You need toobe member of hello tenant admin role toorun hello command.</span></span>

   ``Azlog.exe authorizedirectoryreader tenantId``

   <span data-ttu-id="e369f-117">Exempel:</span><span class="sxs-lookup"><span data-stu-id="e369f-117">Example:</span></span>

   ``AZLOG.exe authorizedirectoryreader ba2c0000-d24b-4f4e-92b1-48c4469999``

4. <span data-ttu-id="e369f-118">Kontrollera att följande mappar tooconfirm som hello JSON för Azure Active Directory-granskningsloggarna hello skapas i dem:</span><span class="sxs-lookup"><span data-stu-id="e369f-118">Check hello following folders tooconfirm that hello Azure Active Directory audit log JSON files are created in them:</span></span>

   * <span data-ttu-id="e369f-119">**C:\Users\azlog\AzureActiveDirectoryJson**</span><span class="sxs-lookup"><span data-stu-id="e369f-119">**C:\Users\azlog\AzureActiveDirectoryJson**</span></span>
   * <span data-ttu-id="e369f-120">**C:\Users\azlog\AzureActiveDirectoryJsonLD**</span><span class="sxs-lookup"><span data-stu-id="e369f-120">**C:\Users\azlog\AzureActiveDirectoryJsonLD**</span></span>

<span data-ttu-id="e369f-121">hello visar följande videoklipp hello stegen som beskrivs i den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="e369f-121">hello following video demonstrates hello steps covered in this article:</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Azure-AD-Integration/player]


> [!NOTE]
> <span data-ttu-id="e369f-122">Specifika anvisningar för att hämta hello information i hello JSON-filer till din säkerhetsinformation och Händelsehanteringssystem (SIEM), försäljaren SIEM.</span><span class="sxs-lookup"><span data-stu-id="e369f-122">For specific instructions on bringing hello information in hello JSON files into your security information and event management (SIEM) system, contact your SIEM vendor.</span></span>

<span data-ttu-id="e369f-123">Community-hjälp är tillgänglig via hello [Azure Log Integration MSDN-Forum](https://social.msdn.microsoft.com/Forums/office/home?forum=AzureLogIntegration).</span><span class="sxs-lookup"><span data-stu-id="e369f-123">Community assistance is available through hello [Azure Log Integration MSDN Forum](https://social.msdn.microsoft.com/Forums/office/home?forum=AzureLogIntegration).</span></span> <span data-ttu-id="e369f-124">Det här forumet kan personer i hello Azure Log-integrering community toosupport varandra med frågor, svar, tips och råd.</span><span class="sxs-lookup"><span data-stu-id="e369f-124">This forum enables people in hello Azure Log Integration community toosupport each other with questions, answers, tips, and tricks.</span></span> <span data-ttu-id="e369f-125">Dessutom övervakar det här forumet hello Azure Log-integrering team och hjälper när den kan.</span><span class="sxs-lookup"><span data-stu-id="e369f-125">In addition, hello Azure Log Integration team monitors this forum and helps whenever it can.</span></span>

<span data-ttu-id="e369f-126">Du kan även öppna en [supportbegäran](../azure-supportability/how-to-create-azure-support-request.md).</span><span class="sxs-lookup"><span data-stu-id="e369f-126">You can also open a [support request](../azure-supportability/how-to-create-azure-support-request.md).</span></span> <span data-ttu-id="e369f-127">Välj **loggen Integration** som hello-tjänst som du begär support.</span><span class="sxs-lookup"><span data-stu-id="e369f-127">Select **Log Integration** as hello service for which you are requesting support.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e369f-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e369f-128">Next steps</span></span>
<span data-ttu-id="e369f-129">toolearn mer om Azure Log-integrering finns:</span><span class="sxs-lookup"><span data-stu-id="e369f-129">toolearn more about Azure Log Integration, see:</span></span>

* <span data-ttu-id="e369f-130">[Microsoft Azure Log-integrering för Azure loggar](https://www.microsoft.com/download/details.aspx?id=53324): den här Download Center sidan ger information, systemkrav och Installationsinstruktioner för Azure Log-integrering.</span><span class="sxs-lookup"><span data-stu-id="e369f-130">[Microsoft Azure Log Integration for Azure logs](https://www.microsoft.com/download/details.aspx?id=53324): This Download Center page gives details, system requirements, and installation instructions for Azure Log Integration.</span></span>
* <span data-ttu-id="e369f-131">[Introduktion tooAzure loggen Integration](security-azure-log-integration-overview.md): den här artikeln introducerar tooAzure loggen Integration, de viktigaste funktionerna och hur det fungerar.</span><span class="sxs-lookup"><span data-stu-id="e369f-131">[Introduction tooAzure Log Integration](security-azure-log-integration-overview.md): This article introduces you tooAzure Log Integration, its key capabilities, and how it works.</span></span>
* <span data-ttu-id="e369f-132">[Samarbeta konfigurationssteg](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/): det här blogginlägget visar hur tooconfigure Azure Log-integrering toowork med partnerlösningar Splunk, HP ArcSight och IBM QRadar.</span><span class="sxs-lookup"><span data-stu-id="e369f-132">[Partner configuration steps](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/): This blog post shows you how tooconfigure Azure Log Integration toowork with partner solutions Splunk, HP ArcSight, and IBM QRadar.</span></span>
* <span data-ttu-id="e369f-133">[Azure Log Integration FAQ](security-azure-log-integration-faq.md): den här artikeln innehåller svar på frågor om Azure Log-integrering.</span><span class="sxs-lookup"><span data-stu-id="e369f-133">[Azure Log Integration FAQ](security-azure-log-integration-faq.md): This article answers questions about Azure Log Integration.</span></span>
* <span data-ttu-id="e369f-134">[Integrera Security Center-aviseringar med Azure Log-integrering](../security-center/security-center-integrating-alerts-with-log-integration.md): den här artikeln visar hur toosync Security Center aviseringar, tillsammans med virtuella säkerhetshändelser samlas in av Azure-diagnostik och Azure granskningsloggar med din logganalys eller SIEM-lösning.</span><span class="sxs-lookup"><span data-stu-id="e369f-134">[Integrating Security Center alerts with Azure Log Integration](../security-center/security-center-integrating-alerts-with-log-integration.md): This article shows you how toosync Security Center alerts, along with virtual machine security events collected by Azure Diagnostics and Azure audit logs, with your log analytics or SIEM solution.</span></span>
* <span data-ttu-id="e369f-135">[Nya funktioner för Azure-diagnostik och Azure-granskningsloggar](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/): det här blogginlägget beskriver tooAzure granskningsloggar och andra funktioner som hjälper dig få insikter om hello operations resurserna i Azure.</span><span class="sxs-lookup"><span data-stu-id="e369f-135">[New features for Azure Diagnostics and Azure audit logs](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/): This blog post introduces you tooAzure audit logs and other features that help you gain insights into hello operations of your Azure resources.</span></span>
