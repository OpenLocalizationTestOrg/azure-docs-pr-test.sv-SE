---
title: aaaAzure Active Directory B2B collaboration API och anpassning | Microsoft Docs
description: "Azure Active Directory B2B-samarbete stöder företagsomfattande relationer genom att aktivera business partners tooselectively åtkomst till företagets program"
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
ms.openlocfilehash: 2609971ffa5d2ebc9466c61f4e4af11f5b045ecb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2b-collaboration-api-and-customization"></a><span data-ttu-id="24557-103">Azure Active Directory B2B-samarbete API och anpassning</span><span class="sxs-lookup"><span data-stu-id="24557-103">Azure Active Directory B2B collaboration API and customization</span></span>

<span data-ttu-id="24557-104">Vi har fått många kunder berätta om de vill toocustomize hello inbjudan processen på ett sätt som passar bäst för deras organisationer.</span><span class="sxs-lookup"><span data-stu-id="24557-104">We've had many customers tell us that they want toocustomize hello invitation process in a way that works best for their organizations.</span></span> <span data-ttu-id="24557-105">Du kan göra precis som med vårt API.</span><span class="sxs-lookup"><span data-stu-id="24557-105">With our API, you can do just that.</span></span> [<span data-ttu-id="24557-106">https://Developer.microsoft.com/Graph/docs/API-Reference/v1.0/Resources/Invitation</span><span class="sxs-lookup"><span data-stu-id="24557-106">https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation</span></span>](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation)

## <a name="capabilities-of-hello-invitation-api"></a><span data-ttu-id="24557-107">Funktionerna i hello inbjudan API</span><span class="sxs-lookup"><span data-stu-id="24557-107">Capabilities of hello invitation API</span></span>
<span data-ttu-id="24557-108">hello API ger hello följande funktioner:</span><span class="sxs-lookup"><span data-stu-id="24557-108">hello API offers hello following capabilities:</span></span>

1. <span data-ttu-id="24557-109">Bjud in en extern användare med *alla* e-postadress.</span><span class="sxs-lookup"><span data-stu-id="24557-109">Invite an external user with *any* email address.</span></span>

    ```
    "invitedUserDisplayName": "Sam"
    "invitedUserEmailAddress": "gsamoogle@gmail.com"
    ```

2. <span data-ttu-id="24557-110">Anpassa där du vill att dina användare tooland när de accepterar deras inbjudan.</span><span class="sxs-lookup"><span data-stu-id="24557-110">Customize where you want your users tooland after they accept their invitation.</span></span>

    ```
    "inviteRedirectUrl": "https://myapps.microsoft.com/"
    ```

3. <span data-ttu-id="24557-111">Välj toosend hello standard inbjudan e-post via oss</span><span class="sxs-lookup"><span data-stu-id="24557-111">Choose toosend hello standard invitation mail through us</span></span>

    ```
    "sendInvitationMessage": true
    ```

  <span data-ttu-id="24557-112">med en mottagare toohello som du kan anpassa</span><span class="sxs-lookup"><span data-stu-id="24557-112">with a message toohello recipient that you can customize</span></span>

    ```
    "customizedMessageBody": "Hello Sam, let's collaborate!"
    ```

4. <span data-ttu-id="24557-113">Och välj toocc: personer som du vill tookeep i hello slinga om att din bjuda in deltagare i den här.</span><span class="sxs-lookup"><span data-stu-id="24557-113">And choose toocc: people you want tookeep in hello loop about your inviting this collaborator.</span></span>

5. <span data-ttu-id="24557-114">Eller helt anpassa din inbjudan och onboarding arbetsflöde genom att välja inte toosend meddelanden via Azure AD.</span><span class="sxs-lookup"><span data-stu-id="24557-114">Or completely customize your invitation and onboarding workflow by choosing not toosend notifications through Azure AD.</span></span>

    ```
    "sendInvitationMessage": false
    ```

  <span data-ttu-id="24557-115">I så fall måste hämta du tillbaka en inlösning URL från hello API som du kan bädda in i en postmall för e-, Snabbmeddelanden eller andra distributionsmetod du väljer.</span><span class="sxs-lookup"><span data-stu-id="24557-115">In this case, you get back a redemption URL from hello API that you can embed in an email template, IM, or other distribution method of your choice.</span></span>

6. <span data-ttu-id="24557-116">Slutligen, om du är administratör kan du tooinvite hello användaren som medlem.</span><span class="sxs-lookup"><span data-stu-id="24557-116">Finally, if you are an admin, you can choose tooinvite hello user as member.</span></span>

    ```
    "invitedUserType": "Member"
    ```


## <a name="authorization-model"></a><span data-ttu-id="24557-117">Auktoriseringsmodellen</span><span class="sxs-lookup"><span data-stu-id="24557-117">Authorization model</span></span>
<span data-ttu-id="24557-118">hello API kan köras i hello följande auktorisering lägen:</span><span class="sxs-lookup"><span data-stu-id="24557-118">hello API can be run in hello following authorization modes:</span></span>

### <a name="app--user-mode"></a><span data-ttu-id="24557-119">Appen + användarläge</span><span class="sxs-lookup"><span data-stu-id="24557-119">App + User mode</span></span>
<span data-ttu-id="24557-120">I det här läget den som använder hello API behov toohave hello behörigheter toobe skapa B2B inbjudningar.</span><span class="sxs-lookup"><span data-stu-id="24557-120">In this mode, whoever is using hello API needs toohave hello permissions toobe create B2B invitations.</span></span>

### <a name="app-only-mode"></a><span data-ttu-id="24557-121">Appen läge</span><span class="sxs-lookup"><span data-stu-id="24557-121">App only mode</span></span>
<span data-ttu-id="24557-122">I appen endast sammanhang måste hello appen hello User.ReadWrite.All eller Directory.ReadWrite.All omfång för hello inbjudan toosucceed.</span><span class="sxs-lookup"><span data-stu-id="24557-122">In app only context, hello app needs hello User.ReadWrite.All or Directory.ReadWrite.All scopes for hello invitation toosucceed.</span></span>

<span data-ttu-id="24557-123">Mer information finns på: https://graph.microsoft.io/docs/authorization/permission_scopes</span><span class="sxs-lookup"><span data-stu-id="24557-123">For more information, refer to: https://graph.microsoft.io/docs/authorization/permission_scopes</span></span>


## <a name="powershell"></a><span data-ttu-id="24557-124">PowerShell</span><span class="sxs-lookup"><span data-stu-id="24557-124">PowerShell</span></span>
<span data-ttu-id="24557-125">Det är nu möjligt toouse PowerShell tooadd och inbjudan externa användare tooan organisation enkelt.</span><span class="sxs-lookup"><span data-stu-id="24557-125">It is now possible toouse PowerShell tooadd and invite external users tooan organization easily.</span></span> <span data-ttu-id="24557-126">Skapa en inbjudan med hello-cmdlet:</span><span class="sxs-lookup"><span data-stu-id="24557-126">Create an invitation using hello cmdlet:</span></span>

```
New-AzureADMSInvitation
```

<span data-ttu-id="24557-127">Du kan använda hello följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="24557-127">You can use hello following options:</span></span>

* <span data-ttu-id="24557-128">-InvitedUserDisplayName</span><span class="sxs-lookup"><span data-stu-id="24557-128">-InvitedUserDisplayName</span></span>
* <span data-ttu-id="24557-129">-InvitedUserEmailAddress</span><span class="sxs-lookup"><span data-stu-id="24557-129">-InvitedUserEmailAddress</span></span>
* <span data-ttu-id="24557-130">-SendInvitationMessage</span><span class="sxs-lookup"><span data-stu-id="24557-130">-SendInvitationMessage</span></span>
* <span data-ttu-id="24557-131">-InvitedUserMessageInfo</span><span class="sxs-lookup"><span data-stu-id="24557-131">-InvitedUserMessageInfo</span></span>

<span data-ttu-id="24557-132">Du kan också kontrollera ut hello inbjudan API-referens i [https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation)</span><span class="sxs-lookup"><span data-stu-id="24557-132">You can also check out hello invitation API reference in [https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation)</span></span>

## <a name="next-steps"></a><span data-ttu-id="24557-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="24557-133">Next steps</span></span>

<span data-ttu-id="24557-134">Läs andra artiklar om Azure AD B2B-samarbete:</span><span class="sxs-lookup"><span data-stu-id="24557-134">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="24557-135">Vad är Azure AD B2B-samarbete?</span><span class="sxs-lookup"><span data-stu-id="24557-135">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="24557-136">Hur lägger Azure Active Directory-administratörer till B2B-samarbete användare?</span><span class="sxs-lookup"><span data-stu-id="24557-136">How do Azure Active Directory admins add B2B collaboration users?</span></span>](active-directory-b2b-admin-add-users.md)
* [<span data-ttu-id="24557-137">Hur lägger informationsarbetare till B2B-samarbete användare?</span><span class="sxs-lookup"><span data-stu-id="24557-137">How do information workers add B2B collaboration users?</span></span>](active-directory-b2b-iw-add-users.md)
* [<span data-ttu-id="24557-138">hello-element i hello e-postinbjudan för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="24557-138">hello elements of hello B2B collaboration invitation email</span></span>](active-directory-b2b-invitation-email.md)
* [<span data-ttu-id="24557-139">B2B-samarbete inbjudan inlösning</span><span class="sxs-lookup"><span data-stu-id="24557-139">B2B collaboration invitation redemption</span></span>](active-directory-b2b-redemption-experience.md)
* [<span data-ttu-id="24557-140">Azure AD B2B-samarbete och licensiering</span><span class="sxs-lookup"><span data-stu-id="24557-140">Azure AD B2B collaboration licensing</span></span>](active-directory-b2b-licensing.md)
* [<span data-ttu-id="24557-141">Felsökning av Azure Active Directory B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="24557-141">Troubleshooting Azure Active Directory B2B collaboration</span></span>](active-directory-b2b-troubleshooting.md)
* [<span data-ttu-id="24557-142">Vanliga och frågor svar om Azure Active Directory B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="24557-142">Azure Active Directory B2B collaboration frequently asked questions (FAQ)</span></span>](active-directory-b2b-faq.md)
* [<span data-ttu-id="24557-143">Multi-Factor Authentication för användare av B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="24557-143">Multi-factor authentication for B2B collaboration users</span></span>](active-directory-b2b-mfa-instructions.md)
* [<span data-ttu-id="24557-144">Lägg till B2B-samarbete användare utan inbjudan</span><span class="sxs-lookup"><span data-stu-id="24557-144">Add B2B collaboration users without an invitation</span></span>](active-directory-b2b-add-user-without-invite.md)
* [<span data-ttu-id="24557-145">Artikelindex för programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="24557-145">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
