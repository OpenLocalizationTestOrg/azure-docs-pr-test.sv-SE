---
title: "Azure AD Connect: Synkronisera tjänstinstanser | Microsoft Docs"
description: "Den här sidan dokument att tänka på för Azure AD-instanser."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: f340ea11-8ff5-4ae6-b09d-e939c76355a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: e8321c3d16253226a5931cacbce6fa5d50b697bd
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-special-considerations-for-instances"></a><span data-ttu-id="d4798-103">Azure AD Connect: Speciella överväganden vid instanser</span><span class="sxs-lookup"><span data-stu-id="d4798-103">Azure AD Connect: Special considerations for instances</span></span>
<span data-ttu-id="d4798-104">Azure AD Connect används oftast med globalt instans av Azure AD och Office 365.</span><span class="sxs-lookup"><span data-stu-id="d4798-104">Azure AD Connect is most commonly used with the world-wide instance of Azure AD and Office 365.</span></span> <span data-ttu-id="d4798-105">Men det finns också andra instanser och de har olika krav för URL: er och andra saker.</span><span class="sxs-lookup"><span data-stu-id="d4798-105">But there are also other instances and these have different requirements for URLs and other special considerations.</span></span>

## <a name="microsoft-cloud-germany"></a><span data-ttu-id="d4798-106">Microsoft Cloud Germany</span><span class="sxs-lookup"><span data-stu-id="d4798-106">Microsoft Cloud Germany</span></span>
<span data-ttu-id="d4798-107">Den [Microsoft Cloud Tyskland](http://www.microsoft.de/cloud-deutschland) är ett suveräna moln som drivs av en förvaltare tyska data.</span><span class="sxs-lookup"><span data-stu-id="d4798-107">The [Microsoft Cloud Germany](http://www.microsoft.de/cloud-deutschland) is a sovereign cloud operated by a German data trustee.</span></span>

| <span data-ttu-id="d4798-108">URL: er för att öppna i proxyserver</span><span class="sxs-lookup"><span data-stu-id="d4798-108">URLs to open in proxy server</span></span> |
| --- |
| <span data-ttu-id="d4798-109">\*. microsoftonline.de</span><span class="sxs-lookup"><span data-stu-id="d4798-109">\*.microsoftonline.de</span></span> |
| <span data-ttu-id="d4798-110">\*.windows.net</span><span class="sxs-lookup"><span data-stu-id="d4798-110">\*.windows.net</span></span> |
| <span data-ttu-id="d4798-111">+ Listor över återkallade certifikat</span><span class="sxs-lookup"><span data-stu-id="d4798-111">+Certificate Revocation Lists</span></span> |

<span data-ttu-id="d4798-112">När du loggar in på Azure AD-klient måste du använda ett konto i domänen onmicrosoft.de.</span><span class="sxs-lookup"><span data-stu-id="d4798-112">When you sign in to your Azure AD tenant, you must use an account in the onmicrosoft.de domain.</span></span>

<span data-ttu-id="d4798-113">Funktioner som för närvarande inte finns i Microsoft Cloud-Tyskland:</span><span class="sxs-lookup"><span data-stu-id="d4798-113">Features currently not present in the Microsoft Cloud Germany:</span></span>

* <span data-ttu-id="d4798-114">**Azure AD Connect Health** är inte tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="d4798-114">**Azure AD Connect Health** is not available.</span></span>
* <span data-ttu-id="d4798-115">**Automatiska uppdateringar** är inte tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="d4798-115">**Automatic updates** is not available.</span></span>
* <span data-ttu-id="d4798-116">**Tillbakaskrivning av lösenord** är tillgängliga för förhandsgranskning med Azure AD Connect-version 1.1.570.0 och efter.</span><span class="sxs-lookup"><span data-stu-id="d4798-116">**Password writeback** is available for preview with Azure AD Connect version 1.1.570.0 and after.</span></span>
* <span data-ttu-id="d4798-117">Andra Azure AD Premium-tjänster är inte tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="d4798-117">Other Azure AD Premium services are not available.</span></span>

## <a name="microsoft-azure-government-cloud"></a><span data-ttu-id="d4798-118">Microsoft Azure Government-moln</span><span class="sxs-lookup"><span data-stu-id="d4798-118">Microsoft Azure Government cloud</span></span>
<span data-ttu-id="d4798-119">Den [Microsoft Azure Government molnet](https://azure.microsoft.com/features/gov/) är en molntjänst för USA: S regering.</span><span class="sxs-lookup"><span data-stu-id="d4798-119">The [Microsoft Azure Government cloud](https://azure.microsoft.com/features/gov/) is a cloud for US government.</span></span>

<span data-ttu-id="d4798-120">Detta moln har stöd av tidigare versioner av DirSync.</span><span class="sxs-lookup"><span data-stu-id="d4798-120">This cloud has been supported by earlier releases of DirSync.</span></span> <span data-ttu-id="d4798-121">Nästa generation av molnet stöds från build 1.1.180 av Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="d4798-121">From build 1.1.180 of Azure AD Connect, the next generation of the cloud is supported.</span></span> <span data-ttu-id="d4798-122">Den här generationen använder endast USA-baserade slutpunkter och har en annan lista över URL: er för att öppna i din proxyserver.</span><span class="sxs-lookup"><span data-stu-id="d4798-122">This generation is using US-only based endpoints and have a different list of URLs to open in your proxy server.</span></span>

| <span data-ttu-id="d4798-123">URL: er för att öppna i proxyserver</span><span class="sxs-lookup"><span data-stu-id="d4798-123">URLs to open in proxy server</span></span> |
| --- |
| <span data-ttu-id="d4798-124">\*.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="d4798-124">\*.microsoftonline.com</span></span> |
| <span data-ttu-id="d4798-125">\*. microsoftonline.us</span><span class="sxs-lookup"><span data-stu-id="d4798-125">\*.microsoftonline.us</span></span> |
| <span data-ttu-id="d4798-126">\*. gov.us.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="d4798-126">\*.gov.us.microsoftonline.com</span></span> |
| <span data-ttu-id="d4798-127">+ Listor över återkallade certifikat</span><span class="sxs-lookup"><span data-stu-id="d4798-127">+Certificate Revocation Lists</span></span> |

<span data-ttu-id="d4798-128">Azure AD Connect kan inte identifieras automatiskt att Azure AD-klienten finns i det offentliga molnet.</span><span class="sxs-lookup"><span data-stu-id="d4798-128">Azure AD Connect is not able to automatically detect that your Azure AD tenant is located in the Government cloud.</span></span> <span data-ttu-id="d4798-129">I stället måste du vidta följande åtgärder när du installerar Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="d4798-129">Instead you need to take the following actions when you install Azure AD Connect.</span></span>

1. <span data-ttu-id="d4798-130">Starta installationen av Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="d4798-130">Start the Azure AD Connect installation.</span></span>
2. <span data-ttu-id="d4798-131">När du ser den första sidan där du ska godkänna LICENSAVTALET, behöver du inte fortsätta men lämna installationsguiden körs.</span><span class="sxs-lookup"><span data-stu-id="d4798-131">When you see the first page where you are supposed to accept the EULA, do not continue but leave the installation wizard running.</span></span>
3. <span data-ttu-id="d4798-132">Starta regedit och ändra registernyckeln `HKLM\SOFTWARE\Microsoft\Azure AD Connect\AzureInstance` till värdet `2`.</span><span class="sxs-lookup"><span data-stu-id="d4798-132">Start regedit and change the registry key `HKLM\SOFTWARE\Microsoft\Azure AD Connect\AzureInstance` to the value `2`.</span></span>
4. <span data-ttu-id="d4798-133">Gå tillbaka till installationsguiden för Azure AD Connect acceptera LICENSAVTALET och fortsätta.</span><span class="sxs-lookup"><span data-stu-id="d4798-133">Go back to the Azure AD Connect installation wizard, accept the EULA, and continue.</span></span> <span data-ttu-id="d4798-134">Under installationen, se till att använda den **anpassad konfiguration** installation sökväg (och inte snabbinstallationen).</span><span class="sxs-lookup"><span data-stu-id="d4798-134">During installation, make sure to use the **custom configuration** installation path (and not Express installation).</span></span> <span data-ttu-id="d4798-135">Fortsätt sedan installationen som vanligt.</span><span class="sxs-lookup"><span data-stu-id="d4798-135">Then continue the installation as usual.</span></span>

<span data-ttu-id="d4798-136">Funktioner som för närvarande inte finns i Microsoft Azure Government-molnet:</span><span class="sxs-lookup"><span data-stu-id="d4798-136">Features currently not present in the Microsoft Azure Government cloud:</span></span>

* <span data-ttu-id="d4798-137">**Azure AD Connect Health** är inte tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="d4798-137">**Azure AD Connect Health** is not available.</span></span>
* <span data-ttu-id="d4798-138">**Automatiska uppdateringar** är inte tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="d4798-138">**Automatic updates** is not available.</span></span>
* <span data-ttu-id="d4798-139">**Tillbakaskrivning av lösenord** är tillgängliga för förhandsgranskning med Azure AD Connect-version 1.1.570.0 och efter.</span><span class="sxs-lookup"><span data-stu-id="d4798-139">**Password writeback**  is available for preview with Azure AD Connect version 1.1.570.0 and after.</span></span>
* <span data-ttu-id="d4798-140">Andra Azure AD Premium-tjänster är inte tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="d4798-140">Other Azure AD Premium services are not available.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d4798-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d4798-141">Next steps</span></span>
<span data-ttu-id="d4798-142">Läs mer om hur du [integrerar dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="d4798-142">Learn more about [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>
