---
title: "Azure AD Connect-synkronisering: ändra hello AD DS-kontolösenord | Microsoft Docs"
description: "Det här avsnittet beskrivs hur tooupdate Azure AD Connect efter hello lösenord för hello AD DS-konto har ändrats."
services: active-directory
keywords: "AD DS-konto, Active Directory-konto, lösenord"
documentationcenter: 
author: cychua
manager: femila
editor: 
ms.assetid: 76b19162-8b16-4960-9e22-bd64e6675ecc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 2707c9246446612f8d083ecde876b3b4fb2435ca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="changing-hello-ad-ds-account-password"></a><span data-ttu-id="3286f-104">Ändra lösenordet för hello AD DS-konto</span><span class="sxs-lookup"><span data-stu-id="3286f-104">Changing hello AD DS account password</span></span>
<span data-ttu-id="3286f-105">hello AD DS-konto refererar toohello användarkontot som används av Azure AD Connect toocommunicate med lokala Active Directory.</span><span class="sxs-lookup"><span data-stu-id="3286f-105">hello AD DS account refers toohello user account used by Azure AD Connect toocommunicate with on-premises Active Directory.</span></span> <span data-ttu-id="3286f-106">Om du ändrar hello lösenordet för hello AD DS-konto, måste du uppdatera Azure AD Connect-synkroniseringstjänsten med hello nytt lösenord.</span><span class="sxs-lookup"><span data-stu-id="3286f-106">If you change hello password of hello AD DS account, you must update Azure AD Connect Synchronization Service with hello new password.</span></span> <span data-ttu-id="3286f-107">Annars hello synkronisering kan inte längre synkronisera korrekt med hello lokala Active Directory och du kommer märka hello följande fel:</span><span class="sxs-lookup"><span data-stu-id="3286f-107">Otherwise, hello Synchronization can no longer synchronize correctly with hello on-premises Active Directory and you will encounter hello following errors:</span></span>

* <span data-ttu-id="3286f-108">I hello Synchronization Service Manager, alla importera eller exportera igen med lokala AD misslyckas med **inga start autentiseringsuppgifter** fel.</span><span class="sxs-lookup"><span data-stu-id="3286f-108">In hello Synchronization Service Manager, any import or export operation with on-premises AD fails with **no-start-credentials** error.</span></span>

* <span data-ttu-id="3286f-109">Under Windows Loggboken hello programmets händelselogg innehåller ett fel med **händelse-ID 6000** och meddelandet **'hello hanteringsagenten ”contoso.com” misslyckade toorun eftersom hello autentiseringsuppgifter var ogiltiga'** .</span><span class="sxs-lookup"><span data-stu-id="3286f-109">Under Windows Event Viewer, hello application event log contains an error with **Event ID 6000** and message **'hello management agent "contoso.com" failed toorun because hello credentials were invalid'**.</span></span>


## <a name="how-tooupdate-hello-synchronization-service-with-new-password-for-ad-ds-account"></a><span data-ttu-id="3286f-110">Hur tooupdate hello synkroniseringstjänsten med nytt lösenord för AD DS-konto</span><span class="sxs-lookup"><span data-stu-id="3286f-110">How tooupdate hello Synchronization Service with new password for AD DS account</span></span>
<span data-ttu-id="3286f-111">tooupdate hello synkroniseringstjänsten med hello nytt lösenord:</span><span class="sxs-lookup"><span data-stu-id="3286f-111">tooupdate hello Synchronization Service with hello new password:</span></span>

1. <span data-ttu-id="3286f-112">Starta hello Synchronization Service Manager (START → synkroniseringstjänsten).</span><span class="sxs-lookup"><span data-stu-id="3286f-112">Start hello Synchronization Service Manager (START → Synchronization Service).</span></span>
</br><span data-ttu-id="3286f-113">![Synkronisering av Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)</span><span class="sxs-lookup"><span data-stu-id="3286f-113">![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)</span></span>  

2. <span data-ttu-id="3286f-114">Gå toohello **kopplingar** fliken.</span><span class="sxs-lookup"><span data-stu-id="3286f-114">Go toohello **Connectors** tab.</span></span>

3. <span data-ttu-id="3286f-115">Välj hello **AD-koppling** som motsvarar toohello AD DS-konto som dess lösenord har ändrats.</span><span class="sxs-lookup"><span data-stu-id="3286f-115">Select hello **AD Connector** that corresponds toohello AD DS account for which its password was changed.</span></span>

4. <span data-ttu-id="3286f-116">Under **åtgärder**väljer **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="3286f-116">Under **Actions**, select **Properties**.</span></span>

5. <span data-ttu-id="3286f-117">I popup-dialogrutan hello väljer **ansluta tooActive Directory-skog**:</span><span class="sxs-lookup"><span data-stu-id="3286f-117">In hello pop-up dialog, select **Connect tooActive Directory Forest**:</span></span>

6. <span data-ttu-id="3286f-118">Ange hello nya lösenord för hello AD DS-konto i hello **lösenord** textruta.</span><span class="sxs-lookup"><span data-stu-id="3286f-118">Enter hello new password of hello AD DS account in hello **Password** textbox.</span></span>

7. <span data-ttu-id="3286f-119">Klicka på **OK** toosave hello nytt lösenord och Stäng hello popup-fönstret.</span><span class="sxs-lookup"><span data-stu-id="3286f-119">Click **OK** toosave hello new password and close hello pop-up dialog.</span></span>

8. <span data-ttu-id="3286f-120">Starta om hello Azure AD Connect-synkroniseringstjänsten under Windows Service Control Manager.</span><span class="sxs-lookup"><span data-stu-id="3286f-120">Restart hello Azure AD Connect Synchronization Service under Windows Service Control Manager.</span></span> <span data-ttu-id="3286f-121">Detta är tooensure som alla referens toohello gamla lösenord tas bort från cache-minnet hello.</span><span class="sxs-lookup"><span data-stu-id="3286f-121">This is tooensure that any reference toohello old password is removed from hello memory cache.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3286f-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3286f-122">Next steps</span></span>
<span data-ttu-id="3286f-123">**Översiktsavsnitt**</span><span class="sxs-lookup"><span data-stu-id="3286f-123">**Overview topics**</span></span>

* [<span data-ttu-id="3286f-124">Azure AD Connect-synkronisering: Förstå och anpassa synkronisering</span><span class="sxs-lookup"><span data-stu-id="3286f-124">Azure AD Connect sync: Understand and customize synchronization</span></span>](active-directory-aadconnectsync-whatis.md)

* [<span data-ttu-id="3286f-125">Integrera dina lokala identiteter med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3286f-125">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
