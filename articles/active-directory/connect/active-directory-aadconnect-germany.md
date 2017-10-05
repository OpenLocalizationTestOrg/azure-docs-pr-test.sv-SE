---
title: Azure AD Connect i Microsoft Cloud Tyskland
description: "Azure AD Connect integrerar dina lokala kataloger med Azure Active Directory. På så sätt kan du tillhandahålla en gemensam identitet för Office 365-, Azure- och SaaS-program som är integrerade med Azure AD."
keywords: "introduktion till Azure AD Connect, översikt över Azure AD Connect, vad är Azure AD Connect, installera Active Directory, Tyskland, Schwarzwald"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 2bcb0caf-5d97-46cb-8c32-bda66cc22dad
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 4c55b116c0dc080ae316caca873a7b693caa793b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-in-microsoft-cloud-germany---public-preview"></a><span data-ttu-id="0f2d6-105">Azure AD Connect i Microsoft Cloud Tyskland –offentlig förhandsversion</span><span class="sxs-lookup"><span data-stu-id="0f2d6-105">Azure AD Connect in Microsoft Cloud Germany - Public Preview</span></span>
## <a name="introduction"></a><span data-ttu-id="0f2d6-106">Introduktion</span><span class="sxs-lookup"><span data-stu-id="0f2d6-106">Introduction</span></span>
<span data-ttu-id="0f2d6-107">Azure AD Connect tillhandahåller synkronisering mellan din lokala Active Directory och Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0f2d6-107">Azure AD Connect provides synchronization between your on-premises Active Directory and Azure Active Directory.</span></span>
<span data-ttu-id="0f2d6-108">För närvarande måste många scenarier i [Microsoft Cloud Tyskland](https://www.microsoft.com/de-de/cloud/deutschland/default.aspx) utföras av operatören.</span><span class="sxs-lookup"><span data-stu-id="0f2d6-108">Currently, many of the scenarios in [Microsoft Cloud Germany](https://www.microsoft.com/de-de/cloud/deutschland/default.aspx) must be done by the operator.</span></span> <span data-ttu-id="0f2d6-109">När du använder Microsoft Cloud Tyskland måste du vara medveten om följande:</span><span class="sxs-lookup"><span data-stu-id="0f2d6-109">When using Microsoft Cloud Germany, you must be aware of the following:</span></span>

* <span data-ttu-id="0f2d6-110">Följande URL: er måste vara öppna på en proxyserver för att synkroniseringen ska lyckas:</span><span class="sxs-lookup"><span data-stu-id="0f2d6-110">The following URLs must be opened on a proxy server for synchronization to occur successfully:</span></span>
  
  * <span data-ttu-id="0f2d6-111">*.microsoftonline.de</span><span class="sxs-lookup"><span data-stu-id="0f2d6-111">*.microsoftonline.de</span></span>
  * <span data-ttu-id="0f2d6-112">*.windows.net</span><span class="sxs-lookup"><span data-stu-id="0f2d6-112">*.windows.net</span></span>
  * * <span data-ttu-id="0f2d6-113">Listor över återkallade certifikat</span><span class="sxs-lookup"><span data-stu-id="0f2d6-113">Certificate Revocation Lists</span></span>
* <span data-ttu-id="0f2d6-114">När du loggar in på din Azure AD-katalog måste du använda ett konto i domänen onmicrosoft.de.</span><span class="sxs-lookup"><span data-stu-id="0f2d6-114">When you sign in to your Azure AD directory, you must use an account in the onmicrosoft.de domain.</span></span>
* <span data-ttu-id="0f2d6-115">Följande funktioner är inte tillgängliga:</span><span class="sxs-lookup"><span data-stu-id="0f2d6-115">The following features are not available:</span></span>
  * <span data-ttu-id="0f2d6-116">Azure AD Connect Health</span><span class="sxs-lookup"><span data-stu-id="0f2d6-116">Azure AD Connect Health</span></span>
  * <span data-ttu-id="0f2d6-117">Automatiska uppdateringar</span><span class="sxs-lookup"><span data-stu-id="0f2d6-117">Automatic updates</span></span>
 
## <a name="download"></a><span data-ttu-id="0f2d6-118">Ladda ned</span><span class="sxs-lookup"><span data-stu-id="0f2d6-118">Download</span></span>
<span data-ttu-id="0f2d6-119">Du kan hämta Azure AD Connect från Azure AD Connect-bladet på portalen.</span><span class="sxs-lookup"><span data-stu-id="0f2d6-119">You can download Azure AD Connect from the Azure AD Connect blade within the portal.</span></span>  <span data-ttu-id="0f2d6-120">Följ anvisningarna nedan för att hitta Azure AD Connect-bladet.</span><span class="sxs-lookup"><span data-stu-id="0f2d6-120">Use the instructions below to locate the Azure AD Connect blade.</span></span>

### <a name="the-azure-ad-connect-blade"></a><span data-ttu-id="0f2d6-121">Azure AD Connect-bladet</span><span class="sxs-lookup"><span data-stu-id="0f2d6-121">The Azure AD Connect Blade</span></span>
<span data-ttu-id="0f2d6-122">När du har loggat in på Azure-portalen gör du följande:</span><span class="sxs-lookup"><span data-stu-id="0f2d6-122">Once you have signed in to the Azure portal, do the following:</span></span>

1. <span data-ttu-id="0f2d6-123">Gå till Bläddra</span><span class="sxs-lookup"><span data-stu-id="0f2d6-123">Go to Browse</span></span>
2. <span data-ttu-id="0f2d6-124">Välj Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0f2d6-124">Select Azure Active Directory</span></span>
3. <span data-ttu-id="0f2d6-125">Välj sedan Azure AD Connect</span><span class="sxs-lookup"><span data-stu-id="0f2d6-125">Then select Azure AD Connect</span></span>

<span data-ttu-id="0f2d6-126">Du bör se följande:</span><span class="sxs-lookup"><span data-stu-id="0f2d6-126">You should see the following:</span></span>

![Azure AD Connect-bladet](media/active-directory-aadconnect-germany/germany1.png)

<span data-ttu-id="0f2d6-128">I följande tabell beskrivs de funktioner som visas på bladet.</span><span class="sxs-lookup"><span data-stu-id="0f2d6-128">The following table describes the features shown in the blade.</span></span>

| <span data-ttu-id="0f2d6-129">Rubrik</span><span class="sxs-lookup"><span data-stu-id="0f2d6-129">Title</span></span> | <span data-ttu-id="0f2d6-130">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0f2d6-130">Description</span></span> |
| --- | --- |
| <span data-ttu-id="0f2d6-131">SYNKRONISERINGSSTATUS</span><span class="sxs-lookup"><span data-stu-id="0f2d6-131">SYNC STATUS</span></span> |<span data-ttu-id="0f2d6-132">Visar om synkronisering är aktiverat eller inaktiverat.</span><span class="sxs-lookup"><span data-stu-id="0f2d6-132">Let's you know whether synchronization is enabled or disabled.</span></span> |
| <span data-ttu-id="0f2d6-133">SENASTE SYNKRONISERING</span><span class="sxs-lookup"><span data-stu-id="0f2d6-133">LAST SYNC</span></span> |<span data-ttu-id="0f2d6-134">Senast tidpunkten då en lyckad synkronisering slutfördes.</span><span class="sxs-lookup"><span data-stu-id="0f2d6-134">The last time a successful sync completed.</span></span> |
| <span data-ttu-id="0f2d6-135">FEDERERADE DOMÄNER</span><span class="sxs-lookup"><span data-stu-id="0f2d6-135">FEDERATED DOMAINS</span></span> |<span data-ttu-id="0f2d6-136">Visar antalet federerade domäner som är konfigurerade för tillfället.</span><span class="sxs-lookup"><span data-stu-id="0f2d6-136">Shows the number of federated domains currently configured.</span></span> |

## <a name="installation"></a><span data-ttu-id="0f2d6-137">Installation</span><span class="sxs-lookup"><span data-stu-id="0f2d6-137">Installation</span></span>
<span data-ttu-id="0f2d6-138">Du kan använda dokumentationen [här](active-directory-aadconnect.md#install-azure-ad-connect) för att installera Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="0f2d6-138">To install Azure AD Connect, you can use the documentation [here](active-directory-aadconnect.md#install-azure-ad-connect).</span></span>

## <a name="advanced-features-and-additional-information"></a><span data-ttu-id="0f2d6-139">Avancerade funktioner och ytterligare information</span><span class="sxs-lookup"><span data-stu-id="0f2d6-139">Advanced features and Additional Information</span></span>
<span data-ttu-id="0f2d6-140">För ytterligare information och riktlinjer för anpassade inställningar eller avancerade konfigurationer kan du börja med [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="0f2d6-140">For additional information and guidance on custom settings or advanced configurations, start with [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>  <span data-ttu-id="0f2d6-141">Den här sidan innehåller information och länkar till ytterligare vägledning.</span><span class="sxs-lookup"><span data-stu-id="0f2d6-141">This page provides information and links to additional guidance.</span></span>

