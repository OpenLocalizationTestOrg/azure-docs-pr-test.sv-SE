---
title: 'Azure AD Connect-synkronisering: aktivera AD-Papperskorgen | Microsoft Docs'
description: "Det här avsnittet rekommenderar användning av AD-Papperskorgen funktionen med Azure AD Connect."
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
ms.openlocfilehash: eb455477547f3db8245cf3601576eba9c6fdc56f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-sync-enable-ad-recycle-bin"></a><span data-ttu-id="1a3c3-104">Azure AD Connect-synkronisering: aktivera AD-Papperskorgen</span><span class="sxs-lookup"><span data-stu-id="1a3c3-104">Azure AD Connect sync: Enable AD recycle bin</span></span>
<span data-ttu-id="1a3c3-105">Det rekommenderas att du aktiverar AD Papperskorgen för din lokala Active kataloger, som synkroniseras till Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1a3c3-105">It is recommended that you enable the AD Recycle Bin feature for your on-premises Active Directories, which are synchronized to Azure AD.</span></span> 

<span data-ttu-id="1a3c3-106">Om du av misstag tar bort en lokal AD användarobjektet och återställning med hjälp av funktionen, Azure AD återställs motsvarande Azure AD-användarobjektet.</span><span class="sxs-lookup"><span data-stu-id="1a3c3-106">If you accidentally deleted an on-premises AD user object and restore it using the feature, Azure AD restores the corresponding Azure AD user object.</span></span>  <span data-ttu-id="1a3c3-107">Information om funktionen för AD-Papperskorgen finns i artikeln [översikt över scenarier för att återställa borttagna Active Directory-objekt](https://technet.microsoft.com/library/dd379542.aspx).</span><span class="sxs-lookup"><span data-stu-id="1a3c3-107">For information about the AD Recycle Bin feature, refer to article [Scenario Overview for Restoring Deleted Active Directory Objects](https://technet.microsoft.com/library/dd379542.aspx).</span></span>

## <a name="benefits-of-enabling-the-ad-recycle-bin"></a><span data-ttu-id="1a3c3-108">Fördelarna med att aktivera AD-Papperskorgen</span><span class="sxs-lookup"><span data-stu-id="1a3c3-108">Benefits of enabling the AD recycle bin</span></span>
<span data-ttu-id="1a3c3-109">Den här funktionen hjälper med att återställa användarobjekt i Azure AD genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="1a3c3-109">This feature helps with restoring Azure AD user objects by doing the following:</span></span>

* <span data-ttu-id="1a3c3-110">Om du av misstag tar bort en lokal AD-användare objekt tas bort i nästa synkronisering cykel motsvarande Azure AD-användarobjektet.</span><span class="sxs-lookup"><span data-stu-id="1a3c3-110">If you accidentally deleted an on-premises AD user object, the corresponding Azure AD user object will be deleted in the next sync cycle.</span></span> <span data-ttu-id="1a3c3-111">Som standard sparas Azure AD den borttagna Azure AD-användarobjektet i ej permanent borttagna tillstånd för 30 dagar.</span><span class="sxs-lookup"><span data-stu-id="1a3c3-111">By default, Azure AD keeps the deleted Azure AD user object in soft-deleted state for 30 days.</span></span>

* <span data-ttu-id="1a3c3-112">Om du har lokala AD Återanvänd Bin-funktionen aktiverad, kan du återställa den borttagna lokala AD-användarobjektet utan att ändra värdet Källfästpunkt.</span><span class="sxs-lookup"><span data-stu-id="1a3c3-112">If you have on-premises AD Recycle Bin feature enabled, you can restore the deleted on-premises AD user object without changing its Source Anchor value.</span></span> <span data-ttu-id="1a3c3-113">När den återställda lokalt AD användarobjektet synkroniseras till Azure AD Azure AD kommer återställning motsvarande ej permanent borttagna Azure AD-användarobjektet.</span><span class="sxs-lookup"><span data-stu-id="1a3c3-113">When the recovered on-premises AD user object is synchronized to Azure AD, Azure AD will restore the corresponding soft-deleted Azure AD user object.</span></span> <span data-ttu-id="1a3c3-114">Information om Källfästpunkten attribut finns i artikel [Azure AD Connect: Designbegreppen](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#sourceanchor).</span><span class="sxs-lookup"><span data-stu-id="1a3c3-114">For information about Source Anchor attribute, refer to article [Azure AD Connect: Design concepts](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#sourceanchor).</span></span>

* <span data-ttu-id="1a3c3-115">Om du inte har lokala AD-Papperskorgen funktionen är aktiverad kan du bli ombedd att skapa en AD-användarobjektet för att ersätta det borttagna objektet.</span><span class="sxs-lookup"><span data-stu-id="1a3c3-115">If you do not have on-premises AD Recycle Bin feature enabled, you may be required to create an AD user object to replace the deleted object.</span></span> <span data-ttu-id="1a3c3-116">Om Azure AD Connect-synkroniseringstjänsten konfigureras för att använda AD systemgenererat attribut (till exempel ObjectGuid) för attributet Källfästpunkten nyligen skapade AD användarobjektet inte Källfästpunkten samma värde som AD-objekt för borttagna användare.</span><span class="sxs-lookup"><span data-stu-id="1a3c3-116">If Azure AD Connect Synchronization Service is configured to use system-generated AD attribute (such as ObjectGuid) for the Source Anchor attribute, the newly created AD user object will not have the same Source Anchor value as the deleted AD user object.</span></span> <span data-ttu-id="1a3c3-117">När AD-objekt för nyligen skapade användaren synkroniseras till Azure AD Azure AD skapar en ny Azure AD-användarobjektet i stället för att återställa det ej permanent borttagna objektet för Azure AD-användare.</span><span class="sxs-lookup"><span data-stu-id="1a3c3-117">When the newly created AD user object is synchronized to Azure AD, Azure AD creates a new Azure AD user object instead of restoring the soft-deleted Azure AD user object.</span></span>

> [!NOTE]
> <span data-ttu-id="1a3c3-118">Som standard bort behåller Azure AD Azure AD-användarobjekt i ej permanent borttagna tillstånd i 30 dagar innan de tas bort permanent.</span><span class="sxs-lookup"><span data-stu-id="1a3c3-118">By default, Azure AD keeps deleted Azure AD user objects in soft-deleted state for 30 days before they are permanently deleted.</span></span> <span data-ttu-id="1a3c3-119">Administratörer kan dock accelerera borttagningen av sådana objekt.</span><span class="sxs-lookup"><span data-stu-id="1a3c3-119">However, administrators can accelerate the deletion of such objects.</span></span> <span data-ttu-id="1a3c3-120">När objekt tas bort permanent de inte längre kan återställas-, även om lokala AD-Papperskorgen är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="1a3c3-120">Once the objects are permanently deleted, they can no longer be recovered, even if on-premises AD Recycle Bin feature is enabled.</span></span>



## <a name="next-steps"></a><span data-ttu-id="1a3c3-121">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1a3c3-121">Next steps</span></span>
<span data-ttu-id="1a3c3-122">**Översiktsavsnitt**</span><span class="sxs-lookup"><span data-stu-id="1a3c3-122">**Overview topics**</span></span>

* [<span data-ttu-id="1a3c3-123">Azure AD Connect-synkronisering: Förstå och anpassa synkronisering</span><span class="sxs-lookup"><span data-stu-id="1a3c3-123">Azure AD Connect sync: Understand and customize synchronization</span></span>](active-directory-aadconnectsync-whatis.md)

* [<span data-ttu-id="1a3c3-124">Integrera dina lokala identiteter med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1a3c3-124">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
