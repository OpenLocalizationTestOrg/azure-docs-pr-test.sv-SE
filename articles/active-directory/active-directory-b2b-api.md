---
title: Azure Active Directory B2B-samarbete API och anpassning | Microsoft Docs
description: "Azure Active Directory B2B-samarbete ger stöd för dina företagsomfattande relationer genom att tilldela affärspartner selektiv åtkomst till dina affärsprogram"
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
ms.date: 04/11/2017
ms.author: sasubram
ms.openlocfilehash: c85e05b38b4a9525e13ec510a17b7ef4841198d7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-active-directory-b2b-collaboration-api-and-customization"></a><span data-ttu-id="58870-103">Azure Active Directory B2B-samarbete API och anpassning</span><span class="sxs-lookup"><span data-stu-id="58870-103">Azure Active Directory B2B collaboration API and customization</span></span>

<span data-ttu-id="58870-104">Vi har fått många kunder berätta för oss att de vill anpassa inbjudan processen på ett sätt som passar bäst för deras organisationer.</span><span class="sxs-lookup"><span data-stu-id="58870-104">We've had many customers tell us that they want to customize the invitation process in a way that works best for their organizations.</span></span> <span data-ttu-id="58870-105">Du kan göra precis som med vårt API.</span><span class="sxs-lookup"><span data-stu-id="58870-105">With our API, you can do just that.</span></span> [<span data-ttu-id="58870-106">https://Developer.microsoft.com/Graph/docs/API-Reference/v1.0/Resources/Invitation</span><span class="sxs-lookup"><span data-stu-id="58870-106">https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation</span></span>](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation)

## <a name="capabilities-of-the-invitation-api"></a><span data-ttu-id="58870-107">Funktionerna i inbjudan API</span><span class="sxs-lookup"><span data-stu-id="58870-107">Capabilities of the invitation API</span></span>
<span data-ttu-id="58870-108">API: et finns följande funktioner:</span><span class="sxs-lookup"><span data-stu-id="58870-108">The API offers the following capabilities:</span></span>

1. <span data-ttu-id="58870-109">Bjud in en extern användare med *alla* e-postadress.</span><span class="sxs-lookup"><span data-stu-id="58870-109">Invite an external user with *any* email address.</span></span>

    ```
    "invitedUserDisplayName": "Sam"
    "invitedUserEmailAddress": "gsamoogle@gmail.com"
    ```

2. <span data-ttu-id="58870-110">Anpassa där du vill att användarna ska hamna när de accepterar deras inbjudan.</span><span class="sxs-lookup"><span data-stu-id="58870-110">Customize where you want your users to land after they accept their invitation.</span></span>

    ```
    "inviteRedirectUrl": "https://myapps.microsoft.com/"
    ```

3. <span data-ttu-id="58870-111">Välj att skicka e-post för standard inbjudan via oss</span><span class="sxs-lookup"><span data-stu-id="58870-111">Choose to send the standard invitation mail through us</span></span>

    ```
    "sendInvitationMessage": true
    ```

  <span data-ttu-id="58870-112">ett meddelande som mottagaren som du kan anpassa</span><span class="sxs-lookup"><span data-stu-id="58870-112">with a message to the recipient that you can customize</span></span>

    ```
    "customizedMessageBody": "Hello Sam, let's collaborate!"
    ```

4. <span data-ttu-id="58870-113">Och välj att skicka en kopia: personer som du vill behålla i loop om att din bjuda in deltagare i den här.</span><span class="sxs-lookup"><span data-stu-id="58870-113">And choose to cc: people you want to keep in the loop about your inviting this collaborator.</span></span>

5. <span data-ttu-id="58870-114">Eller helt anpassa din inbjudan och onboarding arbetsflöde genom att välja att inte skicka meddelanden via Azure AD.</span><span class="sxs-lookup"><span data-stu-id="58870-114">Or completely customize your invitation and onboarding workflow by choosing not to send notifications through Azure AD.</span></span>

    ```
    "sendInvitationMessage": false
    ```

  <span data-ttu-id="58870-115">I så fall måste få du tillbaka en inlösning URL från API: et som du kan bädda in i en postmall för e-, Snabbmeddelanden eller andra distributionsmetod du väljer.</span><span class="sxs-lookup"><span data-stu-id="58870-115">In this case, you get back a redemption URL from the API that you can embed in an email template, IM, or other distribution method of your choice.</span></span>

6. <span data-ttu-id="58870-116">Om du är administratör kan välja du dessutom att bjuda in användare som medlem.</span><span class="sxs-lookup"><span data-stu-id="58870-116">Finally, if you are an admin, you can choose to invite the user as member.</span></span>

    ```
    "invitedUserType": "Member"
    ```


## <a name="authorization-model"></a><span data-ttu-id="58870-117">Auktoriseringsmodellen</span><span class="sxs-lookup"><span data-stu-id="58870-117">Authorization model</span></span>
<span data-ttu-id="58870-118">API: et kan köras i följande tillstånd lägen:</span><span class="sxs-lookup"><span data-stu-id="58870-118">The API can be run in the following authorization modes:</span></span>

### <a name="app--user-mode"></a><span data-ttu-id="58870-119">Appen + användarläge</span><span class="sxs-lookup"><span data-stu-id="58870-119">App + User mode</span></span>
<span data-ttu-id="58870-120">I detta läge gäller den som använder API: N måste ha behörighet att skapa B2B inbjudningar.</span><span class="sxs-lookup"><span data-stu-id="58870-120">In this mode, whoever is using the API needs to have the permissions to be create B2B invitations.</span></span>

### <a name="app-only-mode"></a><span data-ttu-id="58870-121">Appen läge</span><span class="sxs-lookup"><span data-stu-id="58870-121">App only mode</span></span>
<span data-ttu-id="58870-122">Appen måste User.ReadWrite.All eller Directory.ReadWrite.All omfång för inbjudan ska lyckas i appen endast sammanhang.</span><span class="sxs-lookup"><span data-stu-id="58870-122">In app only context, the app needs the User.ReadWrite.All or Directory.ReadWrite.All scopes for the invitation to succeed.</span></span>

<span data-ttu-id="58870-123">Mer information finns på: https://graph.microsoft.io/docs/authorization/permission_scopes</span><span class="sxs-lookup"><span data-stu-id="58870-123">For more information, refer to: https://graph.microsoft.io/docs/authorization/permission_scopes</span></span>


## <a name="powershell"></a><span data-ttu-id="58870-124">PowerShell</span><span class="sxs-lookup"><span data-stu-id="58870-124">PowerShell</span></span>
<span data-ttu-id="58870-125">Nu är det möjligt att använda PowerShell för att lägga till och bjuda in externa användare till en organisation enkelt.</span><span class="sxs-lookup"><span data-stu-id="58870-125">It is now possible to use PowerShell to add and invite external users to an organization easily.</span></span> <span data-ttu-id="58870-126">Skapa en inbjudan med hjälp av cmdlet:</span><span class="sxs-lookup"><span data-stu-id="58870-126">Create an invitation using the cmdlet:</span></span>

```
New-AzureADMSInvitation
```

<span data-ttu-id="58870-127">Du kan använda följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="58870-127">You can use the following options:</span></span>

* <span data-ttu-id="58870-128">-InvitedUserDisplayName</span><span class="sxs-lookup"><span data-stu-id="58870-128">-InvitedUserDisplayName</span></span>
* <span data-ttu-id="58870-129">-InvitedUserEmailAddress</span><span class="sxs-lookup"><span data-stu-id="58870-129">-InvitedUserEmailAddress</span></span>
* <span data-ttu-id="58870-130">-SendInvitationMessage</span><span class="sxs-lookup"><span data-stu-id="58870-130">-SendInvitationMessage</span></span>
* <span data-ttu-id="58870-131">-InvitedUserMessageInfo</span><span class="sxs-lookup"><span data-stu-id="58870-131">-InvitedUserMessageInfo</span></span>

<span data-ttu-id="58870-132">Du kan också kontrollera ut inbjudan API-referens i [https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation)</span><span class="sxs-lookup"><span data-stu-id="58870-132">You can also check out the invitation API reference in [https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation)</span></span>

## <a name="next-steps"></a><span data-ttu-id="58870-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="58870-133">Next steps</span></span>

<span data-ttu-id="58870-134">Läs andra artiklar om Azure AD B2B-samarbete:</span><span class="sxs-lookup"><span data-stu-id="58870-134">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="58870-135">Vad är Azure AD B2B-samarbete?</span><span class="sxs-lookup"><span data-stu-id="58870-135">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="58870-136">Hur lägger Azure Active Directory-administratörer till B2B-samarbete användare?</span><span class="sxs-lookup"><span data-stu-id="58870-136">How do Azure Active Directory admins add B2B collaboration users?</span></span>](active-directory-b2b-admin-add-users.md)
* [<span data-ttu-id="58870-137">Hur lägger informationsarbetare till B2B-samarbete användare?</span><span class="sxs-lookup"><span data-stu-id="58870-137">How do information workers add B2B collaboration users?</span></span>](active-directory-b2b-iw-add-users.md)
* [<span data-ttu-id="58870-138">Elementen i e-postinbjudan B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="58870-138">The elements of the B2B collaboration invitation email</span></span>](active-directory-b2b-invitation-email.md)
* [<span data-ttu-id="58870-139">B2B-samarbete inbjudan inlösning</span><span class="sxs-lookup"><span data-stu-id="58870-139">B2B collaboration invitation redemption</span></span>](active-directory-b2b-redemption-experience.md)
* [<span data-ttu-id="58870-140">Azure AD B2B-samarbete och licensiering</span><span class="sxs-lookup"><span data-stu-id="58870-140">Azure AD B2B collaboration licensing</span></span>](active-directory-b2b-licensing.md)
* [<span data-ttu-id="58870-141">Felsökning av Azure Active Directory B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="58870-141">Troubleshooting Azure Active Directory B2B collaboration</span></span>](active-directory-b2b-troubleshooting.md)
* [<span data-ttu-id="58870-142">Vanliga och frågor svar om Azure Active Directory B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="58870-142">Azure Active Directory B2B collaboration frequently asked questions (FAQ)</span></span>](active-directory-b2b-faq.md)
* [<span data-ttu-id="58870-143">Multi-Factor Authentication för användare av B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="58870-143">Multi-factor authentication for B2B collaboration users</span></span>](active-directory-b2b-mfa-instructions.md)
* [<span data-ttu-id="58870-144">Lägg till B2B-samarbete användare utan inbjudan</span><span class="sxs-lookup"><span data-stu-id="58870-144">Add B2B collaboration users without an invitation</span></span>](active-directory-b2b-add-user-without-invite.md)
* [<span data-ttu-id="58870-145">Artikelindex för programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="58870-145">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
