---
title: aaaAzure AD Connect i Microsoft Cloud Tyskland
description: "Azure AD Connect integrerar dina lokala kataloger med Azure Active Directory. Detta gör att du tooprovide en gemensam identitet för Office 365 och Azure SaaS-program som är integrerade med Azure AD."
keywords: "Introduktion tooAzure AD Connect, Azure AD Connect översikt över vad är Azure AD Connect, installera active directory, Tyskland svart skog"
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
ms.openlocfilehash: f32194fa6c365614f68e5d1ddcf0dac44d223292
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-in-microsoft-cloud-germany---public-preview"></a><span data-ttu-id="6bc40-105">Azure AD Connect i Microsoft Cloud Tyskland –offentlig förhandsversion</span><span class="sxs-lookup"><span data-stu-id="6bc40-105">Azure AD Connect in Microsoft Cloud Germany - Public Preview</span></span>
## <a name="introduction"></a><span data-ttu-id="6bc40-106">Introduktion</span><span class="sxs-lookup"><span data-stu-id="6bc40-106">Introduction</span></span>
<span data-ttu-id="6bc40-107">Azure AD Connect tillhandahåller synkronisering mellan din lokala Active Directory och Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="6bc40-107">Azure AD Connect provides synchronization between your on-premises Active Directory and Azure Active Directory.</span></span>
<span data-ttu-id="6bc40-108">För närvarande många hello scenarier i [Microsoft Cloud Tyskland](https://www.microsoft.com/de-de/cloud/deutschland/default.aspx) måste utföras av hello-operator.</span><span class="sxs-lookup"><span data-stu-id="6bc40-108">Currently, many of hello scenarios in [Microsoft Cloud Germany](https://www.microsoft.com/de-de/cloud/deutschland/default.aspx) must be done by hello operator.</span></span> <span data-ttu-id="6bc40-109">När du använder Microsoft Cloud Tyskland måste du vara medveten om följande hello:</span><span class="sxs-lookup"><span data-stu-id="6bc40-109">When using Microsoft Cloud Germany, you must be aware of hello following:</span></span>

* <span data-ttu-id="6bc40-110">hello måste följande URL: er vara öppna på en proxyserver för synkronisering toooccur har:</span><span class="sxs-lookup"><span data-stu-id="6bc40-110">hello following URLs must be opened on a proxy server for synchronization toooccur successfully:</span></span>
  
  * <span data-ttu-id="6bc40-111">*.microsoftonline.de</span><span class="sxs-lookup"><span data-stu-id="6bc40-111">*.microsoftonline.de</span></span>
  * <span data-ttu-id="6bc40-112">*.windows.net</span><span class="sxs-lookup"><span data-stu-id="6bc40-112">*.windows.net</span></span>
  * * <span data-ttu-id="6bc40-113">Listor över återkallade certifikat</span><span class="sxs-lookup"><span data-stu-id="6bc40-113">Certificate Revocation Lists</span></span>
* <span data-ttu-id="6bc40-114">När du loggar in tooyour Azure AD-katalog måste du använda ett konto i hello onmicrosoft.de domän.</span><span class="sxs-lookup"><span data-stu-id="6bc40-114">When you sign in tooyour Azure AD directory, you must use an account in hello onmicrosoft.de domain.</span></span>
* <span data-ttu-id="6bc40-115">hello följande funktioner är inte tillgängliga:</span><span class="sxs-lookup"><span data-stu-id="6bc40-115">hello following features are not available:</span></span>
  * <span data-ttu-id="6bc40-116">Azure AD Connect Health</span><span class="sxs-lookup"><span data-stu-id="6bc40-116">Azure AD Connect Health</span></span>
  * <span data-ttu-id="6bc40-117">Automatiska uppdateringar</span><span class="sxs-lookup"><span data-stu-id="6bc40-117">Automatic updates</span></span>
 
## <a name="download"></a><span data-ttu-id="6bc40-118">Ladda ned</span><span class="sxs-lookup"><span data-stu-id="6bc40-118">Download</span></span>
<span data-ttu-id="6bc40-119">Du kan hämta Azure AD Connect från hello Azure AD Connect-bladet i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="6bc40-119">You can download Azure AD Connect from hello Azure AD Connect blade within hello portal.</span></span>  <span data-ttu-id="6bc40-120">Använd hello instruktionerna nedan toolocate hello Azure AD Connect-bladet.</span><span class="sxs-lookup"><span data-stu-id="6bc40-120">Use hello instructions below toolocate hello Azure AD Connect blade.</span></span>

### <a name="hello-azure-ad-connect-blade"></a><span data-ttu-id="6bc40-121">hello Azure AD Connect-bladet</span><span class="sxs-lookup"><span data-stu-id="6bc40-121">hello Azure AD Connect Blade</span></span>
<span data-ttu-id="6bc40-122">När du har loggat in toohello Azure-portalen, hello följande:</span><span class="sxs-lookup"><span data-stu-id="6bc40-122">Once you have signed in toohello Azure portal, do hello following:</span></span>

1. <span data-ttu-id="6bc40-123">Gå tooBrowse</span><span class="sxs-lookup"><span data-stu-id="6bc40-123">Go tooBrowse</span></span>
2. <span data-ttu-id="6bc40-124">Välj Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6bc40-124">Select Azure Active Directory</span></span>
3. <span data-ttu-id="6bc40-125">Välj sedan Azure AD Connect</span><span class="sxs-lookup"><span data-stu-id="6bc40-125">Then select Azure AD Connect</span></span>

<span data-ttu-id="6bc40-126">Du bör se hello följande:</span><span class="sxs-lookup"><span data-stu-id="6bc40-126">You should see hello following:</span></span>

![Azure AD Connect-bladet](media/active-directory-aadconnect-germany/germany1.png)

<span data-ttu-id="6bc40-128">hello följande tabell beskrivs hello-funktioner som visas i hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="6bc40-128">hello following table describes hello features shown in hello blade.</span></span>

| <span data-ttu-id="6bc40-129">Rubrik</span><span class="sxs-lookup"><span data-stu-id="6bc40-129">Title</span></span> | <span data-ttu-id="6bc40-130">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="6bc40-130">Description</span></span> |
| --- | --- |
| <span data-ttu-id="6bc40-131">SYNKRONISERINGSSTATUS</span><span class="sxs-lookup"><span data-stu-id="6bc40-131">SYNC STATUS</span></span> |<span data-ttu-id="6bc40-132">Visar om synkronisering är aktiverat eller inaktiverat.</span><span class="sxs-lookup"><span data-stu-id="6bc40-132">Let's you know whether synchronization is enabled or disabled.</span></span> |
| <span data-ttu-id="6bc40-133">SENASTE SYNKRONISERING</span><span class="sxs-lookup"><span data-stu-id="6bc40-133">LAST SYNC</span></span> |<span data-ttu-id="6bc40-134">Hej senast en lyckad synkronisering har slutförts.</span><span class="sxs-lookup"><span data-stu-id="6bc40-134">hello last time a successful sync completed.</span></span> |
| <span data-ttu-id="6bc40-135">FEDERERADE DOMÄNER</span><span class="sxs-lookup"><span data-stu-id="6bc40-135">FEDERATED DOMAINS</span></span> |<span data-ttu-id="6bc40-136">Visar hello antal externa domäner som har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="6bc40-136">Shows hello number of federated domains currently configured.</span></span> |

## <a name="installation"></a><span data-ttu-id="6bc40-137">Installation</span><span class="sxs-lookup"><span data-stu-id="6bc40-137">Installation</span></span>
<span data-ttu-id="6bc40-138">tooinstall Azure AD Connect, kan du använda hello dokumentationen [här](active-directory-aadconnect.md#install-azure-ad-connect).</span><span class="sxs-lookup"><span data-stu-id="6bc40-138">tooinstall Azure AD Connect, you can use hello documentation [here](active-directory-aadconnect.md#install-azure-ad-connect).</span></span>

## <a name="advanced-features-and-additional-information"></a><span data-ttu-id="6bc40-139">Avancerade funktioner och ytterligare information</span><span class="sxs-lookup"><span data-stu-id="6bc40-139">Advanced features and Additional Information</span></span>
<span data-ttu-id="6bc40-140">För ytterligare information och riktlinjer för anpassade inställningar eller avancerade konfigurationer kan du börja med [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="6bc40-140">For additional information and guidance on custom settings or advanced configurations, start with [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>  <span data-ttu-id="6bc40-141">Den här sidan innehåller information och länkar tooadditional vägledning.</span><span class="sxs-lookup"><span data-stu-id="6bc40-141">This page provides information and links tooadditional guidance.</span></span>

