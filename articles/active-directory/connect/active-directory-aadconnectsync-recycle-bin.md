---
title: 'Azure AD Connect-synkronisering: aktivera AD-Papperskorgen | Microsoft Docs'
description: "Det här avsnittet rekommenderar hello funktionen används för AD-Papperskorgen med Azure AD Connect."
services: active-directory
keywords: "AD-Papperskorgen, oavsiktlig borttagning, källfästpunkten"
documentationcenter: 
author: cychua
manager: femila
editor: 
ms.assetid: afec4207-74f7-4cdd-b13a-574af5223a90
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 2bb4827d677ccecfd8d2861f2a2fcf73b8cc2d95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-enable-ad-recycle-bin"></a><span data-ttu-id="bcc40-104">Azure AD Connect-synkronisering: aktivera AD-Papperskorgen</span><span class="sxs-lookup"><span data-stu-id="bcc40-104">Azure AD Connect sync: Enable AD recycle bin</span></span>
<span data-ttu-id="bcc40-105">Det rekommenderas att du aktiverar hello AD Papperskorgen för din lokala Active kataloger, som är synkroniserade tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="bcc40-105">It is recommended that you enable hello AD Recycle Bin feature for your on-premises Active Directories, which are synchronized tooAzure AD.</span></span> 

<span data-ttu-id="bcc40-106">Om du av misstag tar bort en lokal AD användarobjektet och återställning med hello-funktionen, Azure AD återställs hello motsvarande Azure AD-användarobjekt.</span><span class="sxs-lookup"><span data-stu-id="bcc40-106">If you accidentally deleted an on-premises AD user object and restore it using hello feature, Azure AD restores hello corresponding Azure AD user object.</span></span>  <span data-ttu-id="bcc40-107">Information om hello AD Papperskorgen finns tooarticle [översikt över scenarier för att återställa borttagna Active Directory-objekt](https://technet.microsoft.com/library/dd379542.aspx).</span><span class="sxs-lookup"><span data-stu-id="bcc40-107">For information about hello AD Recycle Bin feature, refer tooarticle [Scenario Overview for Restoring Deleted Active Directory Objects](https://technet.microsoft.com/library/dd379542.aspx).</span></span>

## <a name="benefits-of-enabling-hello-ad-recycle-bin"></a><span data-ttu-id="bcc40-108">Fördelarna med att aktivera hello AD-Papperskorgen</span><span class="sxs-lookup"><span data-stu-id="bcc40-108">Benefits of enabling hello AD recycle bin</span></span>
<span data-ttu-id="bcc40-109">Den här funktionen hjälper med att återställa Azure AD-användarobjekt hello följande:</span><span class="sxs-lookup"><span data-stu-id="bcc40-109">This feature helps with restoring Azure AD user objects by doing hello following:</span></span>

* <span data-ttu-id="bcc40-110">Om du av misstag tar bort en lokal AD-användare objekt hello motsvarande Azure AD-användarobjektet kommer att tas bort i hello nästa synkronisering cykel.</span><span class="sxs-lookup"><span data-stu-id="bcc40-110">If you accidentally deleted an on-premises AD user object, hello corresponding Azure AD user object will be deleted in hello next sync cycle.</span></span> <span data-ttu-id="bcc40-111">Som standard sparas Azure AD hello bort Azure AD-användarobjektet i ej permanent borttagna tillstånd i 30 dagar.</span><span class="sxs-lookup"><span data-stu-id="bcc40-111">By default, Azure AD keeps hello deleted Azure AD user object in soft-deleted state for 30 days.</span></span>

* <span data-ttu-id="bcc40-112">Om du har lokala AD-Papperskorgen funktionen är aktiverad kan du återställa hello bort lokala AD-användarobjektet utan att ändra värdet Källfästpunkt.</span><span class="sxs-lookup"><span data-stu-id="bcc40-112">If you have on-premises AD Recycle Bin feature enabled, you can restore hello deleted on-premises AD user object without changing its Source Anchor value.</span></span> <span data-ttu-id="bcc40-113">När hello återställts lokalt AD användarobjektet synkroniseras tooAzure AD Azure AD återställs hello motsvarande ej permanent borttagna objektet för Azure AD-användare.</span><span class="sxs-lookup"><span data-stu-id="bcc40-113">When hello recovered on-premises AD user object is synchronized tooAzure AD, Azure AD will restore hello corresponding soft-deleted Azure AD user object.</span></span> <span data-ttu-id="bcc40-114">Information om Källfästpunkten attributet finns tooarticle [Azure AD Connect: Designbegreppen](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#sourceanchor).</span><span class="sxs-lookup"><span data-stu-id="bcc40-114">For information about Source Anchor attribute, refer tooarticle [Azure AD Connect: Design concepts](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#sourceanchor).</span></span>

* <span data-ttu-id="bcc40-115">Om du inte har lokala AD Återanvänd Bin funktionen är aktiverad kan du vara nödvändiga toocreate ett AD-objekt tooreplace hello borttagna användarobjekt.</span><span class="sxs-lookup"><span data-stu-id="bcc40-115">If you do not have on-premises AD Recycle Bin feature enabled, you may be required toocreate an AD user object tooreplace hello deleted object.</span></span> <span data-ttu-id="bcc40-116">Om Azure AD Connect-synkroniseringstjänsten finns konfigurerade toouse systemgenererade AD attribut (till exempel ObjectGuid) för hello Källfästpunkten attribut, kommer hello nyskapade AD användarobjektet inte har hello samma Källfästpunkten värde eftersom hello bort användarobjekt i AD.</span><span class="sxs-lookup"><span data-stu-id="bcc40-116">If Azure AD Connect Synchronization Service is configured toouse system-generated AD attribute (such as ObjectGuid) for hello Source Anchor attribute, hello newly created AD user object will not have hello same Source Anchor value as hello deleted AD user object.</span></span> <span data-ttu-id="bcc40-117">När hello nyskapade AD användarobjektet är synkroniserade tooAzure AD, Azure AD skapar en ny Azure AD-användarobjektet i stället för att återställa hello ej permanent borttagna objektet för Azure AD-användare.</span><span class="sxs-lookup"><span data-stu-id="bcc40-117">When hello newly created AD user object is synchronized tooAzure AD, Azure AD creates a new Azure AD user object instead of restoring hello soft-deleted Azure AD user object.</span></span>

> [!NOTE]
> <span data-ttu-id="bcc40-118">Som standard bort behåller Azure AD Azure AD-användarobjekt i ej permanent borttagna tillstånd i 30 dagar innan de tas bort permanent.</span><span class="sxs-lookup"><span data-stu-id="bcc40-118">By default, Azure AD keeps deleted Azure AD user objects in soft-deleted state for 30 days before they are permanently deleted.</span></span> <span data-ttu-id="bcc40-119">Administratörer kan dock accelerera hello borttagning av dessa objekt.</span><span class="sxs-lookup"><span data-stu-id="bcc40-119">However, administrators can accelerate hello deletion of such objects.</span></span> <span data-ttu-id="bcc40-120">När hello objekt tas bort permanent de inte längre kan återställas-, även om lokala AD-Papperskorgen är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="bcc40-120">Once hello objects are permanently deleted, they can no longer be recovered, even if on-premises AD Recycle Bin feature is enabled.</span></span>



## <a name="next-steps"></a><span data-ttu-id="bcc40-121">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bcc40-121">Next steps</span></span>
<span data-ttu-id="bcc40-122">**Översiktsavsnitt**</span><span class="sxs-lookup"><span data-stu-id="bcc40-122">**Overview topics**</span></span>

* [<span data-ttu-id="bcc40-123">Azure AD Connect-synkronisering: Förstå och anpassa synkronisering</span><span class="sxs-lookup"><span data-stu-id="bcc40-123">Azure AD Connect sync: Understand and customize synchronization</span></span>](active-directory-aadconnectsync-whatis.md)

* [<span data-ttu-id="bcc40-124">Integrera dina lokala identiteter med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="bcc40-124">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
