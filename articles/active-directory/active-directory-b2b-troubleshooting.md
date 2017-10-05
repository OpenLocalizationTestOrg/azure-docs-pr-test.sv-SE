---
title: "Felsökning av Azure Active Directory B2B-samarbete | Microsoft Docs"
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
ms.openlocfilehash: 2009cfc956a2703e268c9364996aa2d0fbd8f279
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-azure-active-directory-b2b-collaboration"></a><span data-ttu-id="8396a-103">Felsökning av Azure Active Directory B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="8396a-103">Troubleshooting Azure Active Directory B2B collaboration</span></span>

<span data-ttu-id="8396a-104">Här följer några lösningar på vanliga problem med Azure Active Directory (AD Azure) B2B-samarbete.</span><span class="sxs-lookup"><span data-stu-id="8396a-104">Here are some remedies for common problems with Azure Active Directory (Azure AD) B2B collaboration.</span></span>


## <a name="ive-added-an-external-user-but-do-not-see-them-in-my-global-address-book-or-in-the-people-picker"></a><span data-ttu-id="8396a-105">Jag har lagt till en extern användare men se inte dem i den globala adressboken eller i Personväljaren</span><span class="sxs-lookup"><span data-stu-id="8396a-105">I’ve added an external user but do not see them in my Global Address Book or in the people picker</span></span>

<span data-ttu-id="8396a-106">Objektet kan ta några minuter att replikera i fall där externa användare inte fylls i listan.</span><span class="sxs-lookup"><span data-stu-id="8396a-106">In cases where external users are not populated in the list, the object might take a few minutes to replicate.</span></span>

## <a name="a-b2b-guest-user-is-not-showing-up-in-sharepoint-onlineonedrive-people-picker"></a><span data-ttu-id="8396a-107">Gästanvändare B2B syns inte i SharePoint Online/OneDrive personer väljare</span><span class="sxs-lookup"><span data-stu-id="8396a-107">A B2B guest user is not showing up in SharePoint Online/OneDrive people picker</span></span> 
 
<span data-ttu-id="8396a-108">Möjligheten att söka efter befintliga gästanvändare i Personväljaren SharePoint Online (SPO) är OFF som standard så att den matchar äldre beteende.</span><span class="sxs-lookup"><span data-stu-id="8396a-108">The ability to search for existing guest users in the SharePoint Online (SPO) people picker is OFF by default to match legacy behavior.</span></span>

<span data-ttu-id="8396a-109">Du kan aktivera den här funktionen med hjälp av inställningen 'ShowPeoplePickerSuggestionsForGuestUsers' på samlingsnivå klient och plats.</span><span class="sxs-lookup"><span data-stu-id="8396a-109">You can enable this feature by using the setting 'ShowPeoplePickerSuggestionsForGuestUsers' at the tenant and site collection level.</span></span> <span data-ttu-id="8396a-110">Du kan ange funktionen med hjälp av Set-SPOTenant och Set-SPOSite cmdlets som gör att medlemmar för att söka efter alla befintliga gästanvändare i katalogen.</span><span class="sxs-lookup"><span data-stu-id="8396a-110">You can set the feature using the Set-SPOTenant and Set-SPOSite cmdlets, which allow members to search all existing guest users in the directory.</span></span> <span data-ttu-id="8396a-111">Ändringar i klientorganisationsområdet påverkar inte redan etablerade SPO platser.</span><span class="sxs-lookup"><span data-stu-id="8396a-111">Changes in the tenant scope do not affect already provisioned SPO sites.</span></span>

## <a name="invitations-have-been-disabled-for-directory"></a><span data-ttu-id="8396a-112">Inbjudningar har inaktiverats för katalogen</span><span class="sxs-lookup"><span data-stu-id="8396a-112">Invitations have been disabled for directory</span></span>

<span data-ttu-id="8396a-113">Om du får ett meddelande om att du inte har behörighet att bjuda in användare kontrollerar du att ditt användarkonto har behörighet att bjuda in externa användare under användarinställningar:</span><span class="sxs-lookup"><span data-stu-id="8396a-113">If you are notified that you do not have permissions to invite users, verify that your user account is authorized to invite external users under User Settings:</span></span>

![](media/active-directory-b2b-troubleshooting/external-user-settings.png)

<span data-ttu-id="8396a-114">Om du nyligen har ändrat inställningarna eller tilldelas en användare med rollen gäst bjuder in, det kan finnas en fördröjning 15 – 60 minuter innan ändringarna börjar gälla.</span><span class="sxs-lookup"><span data-stu-id="8396a-114">If you have recently modified these settings or assigned the Guest Inviter role to a user, there might be a 15-60 minute delay before the changes take effect.</span></span>

## <a name="the-user-that-i-invited-is-receiving-an-error-during-redemption"></a><span data-ttu-id="8396a-115">Användaren som jag inbjuden tar emot ett fel under inlösning</span><span class="sxs-lookup"><span data-stu-id="8396a-115">The user that I invited is receiving an error during redemption</span></span>

<span data-ttu-id="8396a-116">Vanliga fel är:</span><span class="sxs-lookup"><span data-stu-id="8396a-116">Common errors include:</span></span>

### <a name="invitees-admin-has-disallowed-emailverified-users-from-being-created-in-their-tenant"></a><span data-ttu-id="8396a-117">När en användare Admin har otillåten EmailVerified användare skapas i klientorganisationen</span><span class="sxs-lookup"><span data-stu-id="8396a-117">Invitee’s Admin has disallowed EmailVerified Users from being created in their tenant</span></span>

<span data-ttu-id="8396a-118">När användarna vars organisation använder Azure Active Directory, men där användarens konto inte finns (till exempel användaren finns inte i Azure AD contoso.com).</span><span class="sxs-lookup"><span data-stu-id="8396a-118">When inviting users whose organization is using Azure Active Directory, but where the specific user’s account does not exist (for example, the user does not exist in Azure AD contoso.com).</span></span> <span data-ttu-id="8396a-119">Administratören för contoso.com kan har en princip för att förhindra att användare skapas.</span><span class="sxs-lookup"><span data-stu-id="8396a-119">The administrator of contoso.com may have a policy in place preventing users from being created.</span></span> <span data-ttu-id="8396a-120">Användaren måste kontrollera med administratören för att avgöra om externa användare tillåts.</span><span class="sxs-lookup"><span data-stu-id="8396a-120">The user must check with their admin to determine if external users are allowed.</span></span> <span data-ttu-id="8396a-121">Den externa användaren administratör kan behöva användarna verifierad e-post i sin domän (se den här [artikel](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0) på e-post verifieras användare).</span><span class="sxs-lookup"><span data-stu-id="8396a-121">The external user’s admin may need to allow Email Verified users in their domain (see this [article](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0) on allowing Email Verified Users).</span></span>

![](media/active-directory-b2b-troubleshooting/allow-email-verified-users.png)

### <a name="external-user-does-not-exist-already-in-a-federated-domain"></a><span data-ttu-id="8396a-122">Externa användare finns inte redan i en federerad domän</span><span class="sxs-lookup"><span data-stu-id="8396a-122">External user does not exist already in a federated domain</span></span>

<span data-ttu-id="8396a-123">Om du använder federation-autentisering och användaren inte redan finns i Azure Active Directory, kan användaren inte bjudas in.</span><span class="sxs-lookup"><span data-stu-id="8396a-123">If you are using federation authentication and the user does not already exist in Azure Active Directory, the user cannot be invited.</span></span>

<span data-ttu-id="8396a-124">Den externa användaren administratör måste synkronisera användarens konto i Azure Active Directory för att lösa problemet.</span><span class="sxs-lookup"><span data-stu-id="8396a-124">To resolve this issue, the external user’s admin must synchronize the user’s account to Azure Active Directory.</span></span>

## <a name="how-does--which-is-not-normally-a-valid-character-sync-with-azure-ad"></a><span data-ttu-id="8396a-125">Hur fungerar\#', som inte är vanligtvis ett giltigt tecken synkronisering med Azure AD?</span><span class="sxs-lookup"><span data-stu-id="8396a-125">How does ‘\#’, which is not normally a valid character, sync with Azure AD?</span></span>

<span data-ttu-id="8396a-126">”\#” är ett reserverat tecken i UPN-namnen för Azure AD B2B-samarbete eller externa användare eftersom kontot inbjudna user@contoso.com blir user_contoso.com#EXT@fabrikam.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="8396a-126">“\#” is a reserved character in UPNs for Azure AD B2B collaboration or external users, because the invited account user@contoso.com becomes user_contoso.com#EXT@fabrikam.onmicrosoft.com.</span></span> <span data-ttu-id="8396a-127">Därför \# i UPN-namn som kommer från lokala tillåts inte att logga in på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8396a-127">Therefore, \# in UPNs coming from on-premises aren't allowed to sign in to the Azure portal.</span></span> 

## <a name="i-receive-an-error-when-adding-external-users-to-a-synchronized-group"></a><span data-ttu-id="8396a-128">Ett felmeddelande när du lägger till externa användare till en synkroniserade grupp</span><span class="sxs-lookup"><span data-stu-id="8396a-128">I receive an error when adding external users to a synchronized group</span></span>

<span data-ttu-id="8396a-129">Externa användare kan läggas till bara ”har tilldelats” eller ”-” säkerhetsgrupper och inte grupper som är hanterat lokalt.</span><span class="sxs-lookup"><span data-stu-id="8396a-129">External users can be added only to “assigned” or “Security” groups and not to groups that are mastered on-premises.</span></span>

## <a name="my-external-user-did-not-receive-an-email-to-redeem"></a><span data-ttu-id="8396a-130">Min extern användare tog inte emot ett e-postmeddelande för att lösa in</span><span class="sxs-lookup"><span data-stu-id="8396a-130">My external user did not receive an email to redeem</span></span>

<span data-ttu-id="8396a-131">När en användare bör kontrollera med deras Internet- eller skräppost filter så att du får följande adress:Invites@microsoft.com</span><span class="sxs-lookup"><span data-stu-id="8396a-131">The invitee should check with their ISP or spam filter to ensure that the following address is allowed: Invites@microsoft.com</span></span>

## <a name="i-notice-that-the-custom-message-does-not-get-included-with-invitation-messages-at-times"></a><span data-ttu-id="8396a-132">Jag Lägg märke till att det anpassade meddelandet inte får ingå i inbjudan meddelanden vid tillfällen</span><span class="sxs-lookup"><span data-stu-id="8396a-132">I notice that the custom message does not get included with invitation messages at times</span></span>

<span data-ttu-id="8396a-133">Om du vill följa sekretesslagar våra API: er inte inkluderar anpassade meddelanden i e-postinbjudan när:</span><span class="sxs-lookup"><span data-stu-id="8396a-133">To comply with privacy laws, our APIs do not include custom messages in the email invitation when:</span></span>

- <span data-ttu-id="8396a-134">Avsändaren av inbjudan har inte en e-postadress i bjuda in klienten</span><span class="sxs-lookup"><span data-stu-id="8396a-134">The inviter doesn’t have an email address in the inviting tenant</span></span>
- <span data-ttu-id="8396a-135">När en apptjänst-principal skickar inbjudan</span><span class="sxs-lookup"><span data-stu-id="8396a-135">When an appservice principal sends the invitation</span></span>

<span data-ttu-id="8396a-136">Om det här scenariot är viktigt att du kan du ignorera e-postinbjudan våra API och skicka den via e-mekanismen för ditt val.</span><span class="sxs-lookup"><span data-stu-id="8396a-136">If this scenario is important to you, you can suppress our API invitation email, and send it through the email mechanism of your choice.</span></span> <span data-ttu-id="8396a-137">Kontakta din organisations juridiskt biträde kontrollera e-postmeddelandet du skickar det här sättet även uppfyller sekretesslagar.</span><span class="sxs-lookup"><span data-stu-id="8396a-137">Consult your organization’s legal counsel to make sure any email you send this way also complies with privacy laws.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8396a-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8396a-138">Next steps</span></span>

<span data-ttu-id="8396a-139">Läs andra artiklar om Azure AD B2B-samarbete:</span><span class="sxs-lookup"><span data-stu-id="8396a-139">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="8396a-140">Vad är Azure AD B2B-samarbete?</span><span class="sxs-lookup"><span data-stu-id="8396a-140">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="8396a-141">Hur lägger Azure Active Directory-administratörer till B2B-samarbete användare?</span><span class="sxs-lookup"><span data-stu-id="8396a-141">How do Azure Active Directory admins add B2B collaboration users?</span></span>](active-directory-b2b-admin-add-users.md)
* [<span data-ttu-id="8396a-142">Hur lägger informationsarbetare till B2B-samarbete användare?</span><span class="sxs-lookup"><span data-stu-id="8396a-142">How do information workers add B2B collaboration users?</span></span>](active-directory-b2b-iw-add-users.md)
* [<span data-ttu-id="8396a-143">Elementen i e-postinbjudan B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="8396a-143">The elements of the B2B collaboration invitation email</span></span>](active-directory-b2b-invitation-email.md)
* [<span data-ttu-id="8396a-144">B2B-samarbete inbjudan inlösning</span><span class="sxs-lookup"><span data-stu-id="8396a-144">B2B collaboration invitation redemption</span></span>](active-directory-b2b-redemption-experience.md)
* [<span data-ttu-id="8396a-145">Azure AD B2B-samarbete och licensiering</span><span class="sxs-lookup"><span data-stu-id="8396a-145">Azure AD B2B collaboration licensing</span></span>](active-directory-b2b-licensing.md)
* [<span data-ttu-id="8396a-146">Vanliga och frågor svar om Azure Active Directory B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="8396a-146">Azure Active Directory B2B collaboration frequently asked questions (FAQ)</span></span>](active-directory-b2b-faq.md)
* [<span data-ttu-id="8396a-147">Azure Active Directory B2B-samarbete API och anpassning</span><span class="sxs-lookup"><span data-stu-id="8396a-147">Azure Active Directory B2B collaboration API and customization</span></span>](active-directory-b2b-api.md)
* [<span data-ttu-id="8396a-148">Multi-Factor Authentication för användare av B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="8396a-148">Multi-factor authentication for B2B collaboration users</span></span>](active-directory-b2b-mfa-instructions.md)
* [<span data-ttu-id="8396a-149">Lägg till B2B-samarbete användare utan inbjudan</span><span class="sxs-lookup"><span data-stu-id="8396a-149">Add B2B collaboration users without an invitation</span></span>](active-directory-b2b-add-user-without-invite.md)
* [<span data-ttu-id="8396a-150">Artikelindex för programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8396a-150">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
