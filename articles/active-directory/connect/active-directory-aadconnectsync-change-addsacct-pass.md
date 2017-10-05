---
title: "Azure AD Connect-synkronisering: ändra lösenordet för AD DS | Microsoft Docs"
description: "Det här avsnittet dokumentet beskriver hur du uppdaterar Azure AD Connect när lösenordet för AD DS-konto har ändrats."
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
ms.openlocfilehash: 14e16a238e60ecfeeb3cbf88c3922a79349dcc75
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="changing-the-ad-ds-account-password"></a><span data-ttu-id="6dfb7-104">Ändra lösenordet för AD DS</span><span class="sxs-lookup"><span data-stu-id="6dfb7-104">Changing the AD DS account password</span></span>
<span data-ttu-id="6dfb7-105">AD DS-konto refererar till det användarkonto som används av Azure AD Connect för att kommunicera med lokala Active Directory.</span><span class="sxs-lookup"><span data-stu-id="6dfb7-105">The AD DS account refers to the user account used by Azure AD Connect to communicate with on-premises Active Directory.</span></span> <span data-ttu-id="6dfb7-106">Om du ändrar lösenordet för AD DS-konto, måste du uppdatera Azure AD Connect-synkroniseringstjänsten med det nya lösenordet.</span><span class="sxs-lookup"><span data-stu-id="6dfb7-106">If you change the password of the AD DS account, you must update Azure AD Connect Synchronization Service with the new password.</span></span> <span data-ttu-id="6dfb7-107">Annars synkronisering kan inte längre synkronisera korrekt med lokala Active Directory och visas följande felmeddelanden:</span><span class="sxs-lookup"><span data-stu-id="6dfb7-107">Otherwise, the Synchronization can no longer synchronize correctly with the on-premises Active Directory and you will encounter the following errors:</span></span>

* <span data-ttu-id="6dfb7-108">I Synchronization Service Manager, alla importera eller exportera igen med lokala AD misslyckas med **inga start autentiseringsuppgifter** fel.</span><span class="sxs-lookup"><span data-stu-id="6dfb7-108">In the Synchronization Service Manager, any import or export operation with on-premises AD fails with **no-start-credentials** error.</span></span>

* <span data-ttu-id="6dfb7-109">Under Windows Loggboken i programmets händelselogg innehåller ett fel med **händelse-ID 6000** och meddelandet **'hanteringsagenten ”contoso.com” kunde inte köras eftersom autentiseringsuppgifterna var ogiltiga '**.</span><span class="sxs-lookup"><span data-stu-id="6dfb7-109">Under Windows Event Viewer, the application event log contains an error with **Event ID 6000** and message **'The management agent "contoso.com" failed to run because the credentials were invalid'**.</span></span>


## <a name="how-to-update-the-synchronization-service-with-new-password-for-ad-ds-account"></a><span data-ttu-id="6dfb7-110">Hur du uppdaterar synkroniseringstjänsten med nytt lösenord för AD DS-konto</span><span class="sxs-lookup"><span data-stu-id="6dfb7-110">How to update the Synchronization Service with new password for AD DS account</span></span>
<span data-ttu-id="6dfb7-111">Uppdatera synkroniseringstjänsten med det nya lösenordet:</span><span class="sxs-lookup"><span data-stu-id="6dfb7-111">To update the Synchronization Service with the new password:</span></span>

1. <span data-ttu-id="6dfb7-112">Starta hanteraren för synkroniseringstjänsten (START → synkroniseringstjänsten).</span><span class="sxs-lookup"><span data-stu-id="6dfb7-112">Start the Synchronization Service Manager (START → Synchronization Service).</span></span>
</br><span data-ttu-id="6dfb7-113">![Synkronisering av Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)</span><span class="sxs-lookup"><span data-stu-id="6dfb7-113">![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)</span></span>  

2. <span data-ttu-id="6dfb7-114">Gå till den **kopplingar** fliken.</span><span class="sxs-lookup"><span data-stu-id="6dfb7-114">Go to the **Connectors** tab.</span></span>

3. <span data-ttu-id="6dfb7-115">Välj den **AD-koppling** som motsvarar AD DS-konto som dess lösenord har ändrats.</span><span class="sxs-lookup"><span data-stu-id="6dfb7-115">Select the **AD Connector** that corresponds to the AD DS account for which its password was changed.</span></span>

4. <span data-ttu-id="6dfb7-116">Under **åtgärder**väljer **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="6dfb7-116">Under **Actions**, select **Properties**.</span></span>

5. <span data-ttu-id="6dfb7-117">I popup-fönstret väljer **Anslut till Active Directory-skog**:</span><span class="sxs-lookup"><span data-stu-id="6dfb7-117">In the pop-up dialog, select **Connect to Active Directory Forest**:</span></span>

6. <span data-ttu-id="6dfb7-118">Ange det nya lösenordet för AD DS-konto i den **lösenord** textruta.</span><span class="sxs-lookup"><span data-stu-id="6dfb7-118">Enter the new password of the AD DS account in the **Password** textbox.</span></span>

7. <span data-ttu-id="6dfb7-119">Klicka på **OK** att spara det nya lösenordet och stänga popup-fönstret.</span><span class="sxs-lookup"><span data-stu-id="6dfb7-119">Click **OK** to save the new password and close the pop-up dialog.</span></span>

8. <span data-ttu-id="6dfb7-120">Starta om Azure AD Connect synkroniseringstjänsten under Windows Service Control Manager.</span><span class="sxs-lookup"><span data-stu-id="6dfb7-120">Restart the Azure AD Connect Synchronization Service under Windows Service Control Manager.</span></span> <span data-ttu-id="6dfb7-121">Detta är att säkerställa att alla referenser till det gamla lösenordet tas bort från cacheminnet.</span><span class="sxs-lookup"><span data-stu-id="6dfb7-121">This is to ensure that any reference to the old password is removed from the memory cache.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6dfb7-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6dfb7-122">Next steps</span></span>
<span data-ttu-id="6dfb7-123">**Översiktsavsnitt**</span><span class="sxs-lookup"><span data-stu-id="6dfb7-123">**Overview topics**</span></span>

* [<span data-ttu-id="6dfb7-124">Azure AD Connect-synkronisering: Förstå och anpassa synkronisering</span><span class="sxs-lookup"><span data-stu-id="6dfb7-124">Azure AD Connect sync: Understand and customize synchronization</span></span>](active-directory-aadconnectsync-whatis.md)

* [<span data-ttu-id="6dfb7-125">Integrera dina lokala identiteter med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6dfb7-125">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
