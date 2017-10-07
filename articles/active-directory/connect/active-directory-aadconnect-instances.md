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
ms.openlocfilehash: 2a0d8a599cf84cd6530bdbb24951156510d2cf3f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-special-considerations-for-instances"></a><span data-ttu-id="9a06c-103">Azure AD Connect: Speciella överväganden vid instanser</span><span class="sxs-lookup"><span data-stu-id="9a06c-103">Azure AD Connect: Special considerations for instances</span></span>
<span data-ttu-id="9a06c-104">Azure AD Connect används oftast med hello world wide instans av Azure AD och Office 365.</span><span class="sxs-lookup"><span data-stu-id="9a06c-104">Azure AD Connect is most commonly used with hello world-wide instance of Azure AD and Office 365.</span></span> <span data-ttu-id="9a06c-105">Men det finns också andra instanser och de har olika krav för URL: er och andra saker.</span><span class="sxs-lookup"><span data-stu-id="9a06c-105">But there are also other instances and these have different requirements for URLs and other special considerations.</span></span>

## <a name="microsoft-cloud-germany"></a><span data-ttu-id="9a06c-106">Microsoft Cloud Germany</span><span class="sxs-lookup"><span data-stu-id="9a06c-106">Microsoft Cloud Germany</span></span>
<span data-ttu-id="9a06c-107">Hej [Microsoft Cloud Tyskland](http://www.microsoft.de/cloud-deutschland) är ett suveräna moln som drivs av en förvaltare tyska data.</span><span class="sxs-lookup"><span data-stu-id="9a06c-107">hello [Microsoft Cloud Germany](http://www.microsoft.de/cloud-deutschland) is a sovereign cloud operated by a German data trustee.</span></span>

| <span data-ttu-id="9a06c-108">URL: er tooopen i proxyserver</span><span class="sxs-lookup"><span data-stu-id="9a06c-108">URLs tooopen in proxy server</span></span> |
| --- |
| <span data-ttu-id="9a06c-109">\*. microsoftonline.de</span><span class="sxs-lookup"><span data-stu-id="9a06c-109">\*.microsoftonline.de</span></span> |
| <span data-ttu-id="9a06c-110">\*.windows.net</span><span class="sxs-lookup"><span data-stu-id="9a06c-110">\*.windows.net</span></span> |
| <span data-ttu-id="9a06c-111">+ Listor över återkallade certifikat</span><span class="sxs-lookup"><span data-stu-id="9a06c-111">+Certificate Revocation Lists</span></span> |

<span data-ttu-id="9a06c-112">När du loggar in tooyour Azure AD-klient måste du använda ett konto i hello onmicrosoft.de domän.</span><span class="sxs-lookup"><span data-stu-id="9a06c-112">When you sign in tooyour Azure AD tenant, you must use an account in hello onmicrosoft.de domain.</span></span>

<span data-ttu-id="9a06c-113">Funktioner som finns för närvarande inte i hello Microsoft Cloud Tyskland:</span><span class="sxs-lookup"><span data-stu-id="9a06c-113">Features currently not present in hello Microsoft Cloud Germany:</span></span>

* <span data-ttu-id="9a06c-114">**Azure AD Connect Health** är inte tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="9a06c-114">**Azure AD Connect Health** is not available.</span></span>
* <span data-ttu-id="9a06c-115">**Automatiska uppdateringar** är inte tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="9a06c-115">**Automatic updates** is not available.</span></span>
* <span data-ttu-id="9a06c-116">**Tillbakaskrivning av lösenord** är tillgängliga för förhandsgranskning med Azure AD Connect-version 1.1.570.0 och efter.</span><span class="sxs-lookup"><span data-stu-id="9a06c-116">**Password writeback** is available for preview with Azure AD Connect version 1.1.570.0 and after.</span></span>
* <span data-ttu-id="9a06c-117">Andra Azure AD Premium-tjänster är inte tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="9a06c-117">Other Azure AD Premium services are not available.</span></span>

## <a name="microsoft-azure-government-cloud"></a><span data-ttu-id="9a06c-118">Microsoft Azure Government-moln</span><span class="sxs-lookup"><span data-stu-id="9a06c-118">Microsoft Azure Government cloud</span></span>
<span data-ttu-id="9a06c-119">Hej [Microsoft Azure Government molnet](https://azure.microsoft.com/features/gov/) är en molntjänst för USA: S regering.</span><span class="sxs-lookup"><span data-stu-id="9a06c-119">hello [Microsoft Azure Government cloud](https://azure.microsoft.com/features/gov/) is a cloud for US government.</span></span>

<span data-ttu-id="9a06c-120">Detta moln har stöd av tidigare versioner av DirSync.</span><span class="sxs-lookup"><span data-stu-id="9a06c-120">This cloud has been supported by earlier releases of DirSync.</span></span> <span data-ttu-id="9a06c-121">Från build 1.1.180 av Azure AD Connect stöds hello nästa generation av hello molnet.</span><span class="sxs-lookup"><span data-stu-id="9a06c-121">From build 1.1.180 of Azure AD Connect, hello next generation of hello cloud is supported.</span></span> <span data-ttu-id="9a06c-122">Den här generationen använder endast USA-baserade slutpunkter och har en lista över URL: er tooopen i din proxyserver.</span><span class="sxs-lookup"><span data-stu-id="9a06c-122">This generation is using US-only based endpoints and have a different list of URLs tooopen in your proxy server.</span></span>

| <span data-ttu-id="9a06c-123">URL: er tooopen i proxyserver</span><span class="sxs-lookup"><span data-stu-id="9a06c-123">URLs tooopen in proxy server</span></span> |
| --- |
| <span data-ttu-id="9a06c-124">\*.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="9a06c-124">\*.microsoftonline.com</span></span> |
| <span data-ttu-id="9a06c-125">\*. microsoftonline.us</span><span class="sxs-lookup"><span data-stu-id="9a06c-125">\*.microsoftonline.us</span></span> |
| <span data-ttu-id="9a06c-126">\*. gov.us.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="9a06c-126">\*.gov.us.microsoftonline.com</span></span> |
| <span data-ttu-id="9a06c-127">+ Listor över återkallade certifikat</span><span class="sxs-lookup"><span data-stu-id="9a06c-127">+Certificate Revocation Lists</span></span> |

<span data-ttu-id="9a06c-128">Azure AD Connect kan inte tooautomatically identifiera att Azure AD-klienten finns i hello offentliga moln.</span><span class="sxs-lookup"><span data-stu-id="9a06c-128">Azure AD Connect is not able tooautomatically detect that your Azure AD tenant is located in hello Government cloud.</span></span> <span data-ttu-id="9a06c-129">Du måste i stället tootake hello följande åtgärder när du installerar Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="9a06c-129">Instead you need tootake hello following actions when you install Azure AD Connect.</span></span>

1. <span data-ttu-id="9a06c-130">Starta hello Azure AD Connect-installationen.</span><span class="sxs-lookup"><span data-stu-id="9a06c-130">Start hello Azure AD Connect installation.</span></span>
2. <span data-ttu-id="9a06c-131">När du ser hello första sidan där du bör tooaccept hello licensavtal, behöver du inte fortsätta men lämna hello installationsguiden körs.</span><span class="sxs-lookup"><span data-stu-id="9a06c-131">When you see hello first page where you are supposed tooaccept hello EULA, do not continue but leave hello installation wizard running.</span></span>
3. <span data-ttu-id="9a06c-132">Starta regedit och ändra hello registernyckeln `HKLM\SOFTWARE\Microsoft\Azure AD Connect\AzureInstance` toohello värdet `2`.</span><span class="sxs-lookup"><span data-stu-id="9a06c-132">Start regedit and change hello registry key `HKLM\SOFTWARE\Microsoft\Azure AD Connect\AzureInstance` toohello value `2`.</span></span>
4. <span data-ttu-id="9a06c-133">Gå tillbaka toohello Azure AD Connect-guiden för installation, acceptera hello LICENSAVTALET och fortsätt.</span><span class="sxs-lookup"><span data-stu-id="9a06c-133">Go back toohello Azure AD Connect installation wizard, accept hello EULA, and continue.</span></span> <span data-ttu-id="9a06c-134">Under installationen gör att toouse hello **anpassad konfiguration** installation sökväg (och inte snabbinstallationen).</span><span class="sxs-lookup"><span data-stu-id="9a06c-134">During installation, make sure toouse hello **custom configuration** installation path (and not Express installation).</span></span> <span data-ttu-id="9a06c-135">Fortsätt sedan hello installationen som vanligt.</span><span class="sxs-lookup"><span data-stu-id="9a06c-135">Then continue hello installation as usual.</span></span>

<span data-ttu-id="9a06c-136">Funktioner som finns för närvarande inte i hello Microsoft Azure Government molnet:</span><span class="sxs-lookup"><span data-stu-id="9a06c-136">Features currently not present in hello Microsoft Azure Government cloud:</span></span>

* <span data-ttu-id="9a06c-137">**Azure AD Connect Health** är inte tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="9a06c-137">**Azure AD Connect Health** is not available.</span></span>
* <span data-ttu-id="9a06c-138">**Automatiska uppdateringar** är inte tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="9a06c-138">**Automatic updates** is not available.</span></span>
* <span data-ttu-id="9a06c-139">**Tillbakaskrivning av lösenord** är tillgängliga för förhandsgranskning med Azure AD Connect-version 1.1.570.0 och efter.</span><span class="sxs-lookup"><span data-stu-id="9a06c-139">**Password writeback**  is available for preview with Azure AD Connect version 1.1.570.0 and after.</span></span>
* <span data-ttu-id="9a06c-140">Andra Azure AD Premium-tjänster är inte tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="9a06c-140">Other Azure AD Premium services are not available.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9a06c-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9a06c-141">Next steps</span></span>
<span data-ttu-id="9a06c-142">Läs mer om hur du [integrerar dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="9a06c-142">Learn more about [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>
