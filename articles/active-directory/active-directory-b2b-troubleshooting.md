---
title: aaaTroubleshooting Azure Active Directory B2B-samarbete | Microsoft Docs
description: "Kompensation för vanliga problem med Azure Active Directory B2B-samarbete"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/25/2017
ms.author: sasubram
ms.openlocfilehash: 6fcfd7e543cd7bb833225f8aa56e332e7a989faf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-azure-active-directory-b2b-collaboration"></a><span data-ttu-id="bc31b-103">Felsökning av Azure Active Directory B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="bc31b-103">Troubleshooting Azure Active Directory B2B collaboration</span></span>

<span data-ttu-id="bc31b-104">Här följer några lösningar på vanliga problem med Azure Active Directory (AD Azure) B2B-samarbete.</span><span class="sxs-lookup"><span data-stu-id="bc31b-104">Here are some remedies for common problems with Azure Active Directory (Azure AD) B2B collaboration.</span></span>


## <a name="ive-added-an-external-user-but-do-not-see-them-in-my-global-address-book-or-in-hello-people-picker"></a><span data-ttu-id="bc31b-105">Jag har lagt till en extern användare men se inte dem i den globala adressboken eller hello personer objektväljare</span><span class="sxs-lookup"><span data-stu-id="bc31b-105">I’ve added an external user but do not see them in my Global Address Book or in hello people picker</span></span>

<span data-ttu-id="bc31b-106">I fall där externa användare inte fylls i listan hello kan hello-objekt ta några minuter tooreplicate.</span><span class="sxs-lookup"><span data-stu-id="bc31b-106">In cases where external users are not populated in hello list, hello object might take a few minutes tooreplicate.</span></span>

## <a name="a-b2b-guest-user-is-not-showing-up-in-sharepoint-onlineonedrive-people-picker"></a><span data-ttu-id="bc31b-107">Gästanvändare B2B syns inte i SharePoint Online/OneDrive personer väljare</span><span class="sxs-lookup"><span data-stu-id="bc31b-107">A B2B guest user is not showing up in SharePoint Online/OneDrive people picker</span></span> 
 
<span data-ttu-id="bc31b-108">hello möjlighet toosearch för befintliga gästanvändare i hello SharePoint Online (SPO) personer väljare är av äldre standardbeteendet för toomatch.</span><span class="sxs-lookup"><span data-stu-id="bc31b-108">hello ability toosearch for existing guest users in hello SharePoint Online (SPO) people picker is OFF by default toomatch legacy behavior.</span></span>

<span data-ttu-id="bc31b-109">Du kan aktivera den här funktionen med hjälp av hello inställningen 'ShowPeoplePickerSuggestionsForGuestUsers' på hello klient och webbplatssamling nivå.</span><span class="sxs-lookup"><span data-stu-id="bc31b-109">You can enable this feature by using hello setting 'ShowPeoplePickerSuggestionsForGuestUsers' at hello tenant and site collection level.</span></span> <span data-ttu-id="bc31b-110">Du kan ange hello-funktionen med hjälp av hello Set SPOTenant och Set-SPOSite cmdlet: ar, vilket tillåter medlemmar toosearch alla befintliga gästanvändare i hello directory.</span><span class="sxs-lookup"><span data-stu-id="bc31b-110">You can set hello feature using hello Set-SPOTenant and Set-SPOSite cmdlets, which allow members toosearch all existing guest users in hello directory.</span></span> <span data-ttu-id="bc31b-111">Ändringar i hello klientorganisationsområde påverkar inte redan etablerade SPO platser.</span><span class="sxs-lookup"><span data-stu-id="bc31b-111">Changes in hello tenant scope do not affect already provisioned SPO sites.</span></span>

## <a name="invitations-have-been-disabled-for-directory"></a><span data-ttu-id="bc31b-112">Inbjudningar har inaktiverats för katalogen</span><span class="sxs-lookup"><span data-stu-id="bc31b-112">Invitations have been disabled for directory</span></span>

<span data-ttu-id="bc31b-113">Kontrollera att ditt användarkonto är auktoriserade tooinvite externa användare under Inställningar om du får ett meddelande om att du inte har behörigheter tooinvite användare:</span><span class="sxs-lookup"><span data-stu-id="bc31b-113">If you are notified that you do not have permissions tooinvite users, verify that your user account is authorized tooinvite external users under User Settings:</span></span>

![](media/active-directory-b2b-troubleshooting/external-user-settings.png)

<span data-ttu-id="bc31b-114">Om du nyligen har ändrat inställningarna eller tilldelad hello gäst bjuder in rollen tooa användare det kan finnas en fördröjning i 60 15 minut innan hello ändringarna börjar gälla.</span><span class="sxs-lookup"><span data-stu-id="bc31b-114">If you have recently modified these settings or assigned hello Guest Inviter role tooa user, there might be a 15-60 minute delay before hello changes take effect.</span></span>

## <a name="hello-user-that-i-invited-is-receiving-an-error-during-redemption"></a><span data-ttu-id="bc31b-115">hello-användaren som jag inbjuden tar emot ett fel under inlösning</span><span class="sxs-lookup"><span data-stu-id="bc31b-115">hello user that I invited is receiving an error during redemption</span></span>

<span data-ttu-id="bc31b-116">Vanliga fel är:</span><span class="sxs-lookup"><span data-stu-id="bc31b-116">Common errors include:</span></span>

### <a name="invitees-admin-has-disallowed-emailverified-users-from-being-created-in-their-tenant"></a><span data-ttu-id="bc31b-117">När en användare Admin har otillåten EmailVerified användare skapas i klientorganisationen</span><span class="sxs-lookup"><span data-stu-id="bc31b-117">Invitee’s Admin has disallowed EmailVerified Users from being created in their tenant</span></span>

<span data-ttu-id="bc31b-118">När användarna vars organisation använder Azure Active Directory, men där hello specifikt användarkonto finns inte (till exempel hello användaren finns inte i Azure AD contoso.com).</span><span class="sxs-lookup"><span data-stu-id="bc31b-118">When inviting users whose organization is using Azure Active Directory, but where hello specific user’s account does not exist (for example, hello user does not exist in Azure AD contoso.com).</span></span> <span data-ttu-id="bc31b-119">Hej administratör contoso.com kan har en princip för att förhindra att användare skapas.</span><span class="sxs-lookup"><span data-stu-id="bc31b-119">hello administrator of contoso.com may have a policy in place preventing users from being created.</span></span> <span data-ttu-id="bc31b-120">hello användaren måste kontrollera med deras admin toodetermine om externa användare tillåts.</span><span class="sxs-lookup"><span data-stu-id="bc31b-120">hello user must check with their admin toodetermine if external users are allowed.</span></span> <span data-ttu-id="bc31b-121">hello admin för extern användare behöva tooallow e-post verifieras användare i sin domän (se den här [artikel](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0) på e-post verifieras användare).</span><span class="sxs-lookup"><span data-stu-id="bc31b-121">hello external user’s admin may need tooallow Email Verified users in their domain (see this [article](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0) on allowing Email Verified Users).</span></span>

![](media/active-directory-b2b-troubleshooting/allow-email-verified-users.png)

### <a name="external-user-does-not-exist-already-in-a-federated-domain"></a><span data-ttu-id="bc31b-122">Externa användare finns inte redan i en federerad domän</span><span class="sxs-lookup"><span data-stu-id="bc31b-122">External user does not exist already in a federated domain</span></span>

<span data-ttu-id="bc31b-123">Om du använder federation-autentisering och hello användaren inte redan finns i Azure Active Directory, kan du inte bjudas in hello användare.</span><span class="sxs-lookup"><span data-stu-id="bc31b-123">If you are using federation authentication and hello user does not already exist in Azure Active Directory, hello user cannot be invited.</span></span>

<span data-ttu-id="bc31b-124">i detta fall är hello tooresolve extern användare administratören måste synkronisera hello användarens konto tooAzure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="bc31b-124">tooresolve this issue, hello external user’s admin must synchronize hello user’s account tooAzure Active Directory.</span></span>

## <a name="how-does--which-is-not-normally-a-valid-character-sync-with-azure-ad"></a><span data-ttu-id="bc31b-125">Hur fungerar\#', som inte är vanligtvis ett giltigt tecken synkronisering med Azure AD?</span><span class="sxs-lookup"><span data-stu-id="bc31b-125">How does ‘\#’, which is not normally a valid character, sync with Azure AD?</span></span>

<span data-ttu-id="bc31b-126">”\#” är ett reserverat tecken i UPN-namnen för Azure AD B2B-samarbete eller externa användare eftersom hello inbjuden konto user@contoso.com blir user_contoso.com#EXT@fabrikam.onmicrosoft.com. Därför \# i UPN-namn som kommer från lokala tillåts inte toosign i toohello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="bc31b-126">“\#” is a reserved character in UPNs for Azure AD B2B collaboration or external users, because hello invited account user@contoso.com becomes user_contoso.com#EXT@fabrikam.onmicrosoft.com. Therefore, \# in UPNs coming from on-premises aren't allowed toosign in toohello Azure portal.</span></span> 

## <a name="i-receive-an-error-when-adding-external-users-tooa-synchronized-group"></a><span data-ttu-id="bc31b-127">Ett felmeddelande när du lägger till externa användare tooa synkroniserar grupp</span><span class="sxs-lookup"><span data-stu-id="bc31b-127">I receive an error when adding external users tooa synchronized group</span></span>

<span data-ttu-id="bc31b-128">Externa användare kan läggas till endast för ”tilldelade” eller ”säkerhetsgrupper” och inte toogroups som hanterat lokalt.</span><span class="sxs-lookup"><span data-stu-id="bc31b-128">External users can be added only too“assigned” or “Security” groups and not toogroups that are mastered on-premises.</span></span>

## <a name="my-external-user-did-not-receive-an-email-tooredeem"></a><span data-ttu-id="bc31b-129">Min extern användare tog inte emot ett e-tooredeem</span><span class="sxs-lookup"><span data-stu-id="bc31b-129">My external user did not receive an email tooredeem</span></span>

<span data-ttu-id="bc31b-130">Kontrollera hello bjudits in med sitt Internetleverantören eller tillåts skräppost filter tooensure som hello följande adress:Invites@microsoft.com</span><span class="sxs-lookup"><span data-stu-id="bc31b-130">hello invitee should check with their ISP or spam filter tooensure that hello following address is allowed: Invites@microsoft.com</span></span>

## <a name="i-notice-that-hello-custom-message-does-not-get-included-with-invitation-messages-at-times"></a><span data-ttu-id="bc31b-131">Jag har sett att anpassade hello-meddelande inte får ingå i inbjudan meddelanden vid tillfällen</span><span class="sxs-lookup"><span data-stu-id="bc31b-131">I notice that hello custom message does not get included with invitation messages at times</span></span>

<span data-ttu-id="bc31b-132">toocomply med sekretesslagar våra API: er inte inkluderar anpassade meddelanden i hello e-postinbjudan när:</span><span class="sxs-lookup"><span data-stu-id="bc31b-132">toocomply with privacy laws, our APIs do not include custom messages in hello email invitation when:</span></span>

- <span data-ttu-id="bc31b-133">hello bjuder in har inte en e-postadress i hello bjuda in klient</span><span class="sxs-lookup"><span data-stu-id="bc31b-133">hello inviter doesn’t have an email address in hello inviting tenant</span></span>
- <span data-ttu-id="bc31b-134">När en apptjänst-principal skickar hello inbjudan</span><span class="sxs-lookup"><span data-stu-id="bc31b-134">When an appservice principal sends hello invitation</span></span>

<span data-ttu-id="bc31b-135">Om det här scenariot är viktiga tooyou kan du ignorera e-postinbjudan våra API och skicka den via hello e-mekanismen för ditt val.</span><span class="sxs-lookup"><span data-stu-id="bc31b-135">If this scenario is important tooyou, you can suppress our API invitation email, and send it through hello email mechanism of your choice.</span></span> <span data-ttu-id="bc31b-136">Kontakta din organisations juridiskt biträde toomake till e-postmeddelandet du skickar det här sättet även uppfyller sekretesslagar.</span><span class="sxs-lookup"><span data-stu-id="bc31b-136">Consult your organization’s legal counsel toomake sure any email you send this way also complies with privacy laws.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bc31b-137">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bc31b-137">Next steps</span></span>

<span data-ttu-id="bc31b-138">Läs andra artiklar om Azure AD B2B-samarbete:</span><span class="sxs-lookup"><span data-stu-id="bc31b-138">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="bc31b-139">Vad är Azure AD B2B-samarbete?</span><span class="sxs-lookup"><span data-stu-id="bc31b-139">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="bc31b-140">Hur lägger Azure Active Directory-administratörer till B2B-samarbete användare?</span><span class="sxs-lookup"><span data-stu-id="bc31b-140">How do Azure Active Directory admins add B2B collaboration users?</span></span>](active-directory-b2b-admin-add-users.md)
* [<span data-ttu-id="bc31b-141">Hur lägger informationsarbetare till B2B-samarbete användare?</span><span class="sxs-lookup"><span data-stu-id="bc31b-141">How do information workers add B2B collaboration users?</span></span>](active-directory-b2b-iw-add-users.md)
* [<span data-ttu-id="bc31b-142">hello-element i hello e-postinbjudan för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="bc31b-142">hello elements of hello B2B collaboration invitation email</span></span>](active-directory-b2b-invitation-email.md)
* [<span data-ttu-id="bc31b-143">B2B-samarbete inbjudan inlösning</span><span class="sxs-lookup"><span data-stu-id="bc31b-143">B2B collaboration invitation redemption</span></span>](active-directory-b2b-redemption-experience.md)
* [<span data-ttu-id="bc31b-144">Azure AD B2B-samarbete och licensiering</span><span class="sxs-lookup"><span data-stu-id="bc31b-144">Azure AD B2B collaboration licensing</span></span>](active-directory-b2b-licensing.md)
* [<span data-ttu-id="bc31b-145">Vanliga och frågor svar om Azure Active Directory B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="bc31b-145">Azure Active Directory B2B collaboration frequently asked questions (FAQ)</span></span>](active-directory-b2b-faq.md)
* [<span data-ttu-id="bc31b-146">Azure Active Directory B2B-samarbete API och anpassning</span><span class="sxs-lookup"><span data-stu-id="bc31b-146">Azure Active Directory B2B collaboration API and customization</span></span>](active-directory-b2b-api.md)
* [<span data-ttu-id="bc31b-147">Multi-Factor Authentication för användare av B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="bc31b-147">Multi-factor authentication for B2B collaboration users</span></span>](active-directory-b2b-mfa-instructions.md)
* [<span data-ttu-id="bc31b-148">Lägg till B2B-samarbete användare utan inbjudan</span><span class="sxs-lookup"><span data-stu-id="bc31b-148">Add B2B collaboration users without an invitation</span></span>](active-directory-b2b-add-user-without-invite.md)
* [<span data-ttu-id="bc31b-149">Artikelindex för programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="bc31b-149">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
