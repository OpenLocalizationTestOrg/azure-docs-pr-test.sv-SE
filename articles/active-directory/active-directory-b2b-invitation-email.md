---
title: Elementen i e-postinbjudan Azure Active Directory B2B-samarbete | Microsoft Docs
description: Azure Active Directory B2B-samarbete inbjudan e-postmall
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
ms.date: 05/23/2017
ms.author: sasubram
ms.openlocfilehash: 458a2cab13b7e83f120e0926a95d454070181dfb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="the-elements-of-the-b2b-collaboration-invitation-email"></a><span data-ttu-id="2a021-103">Elementen i e-postinbjudan B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="2a021-103">The elements of the B2B collaboration invitation email</span></span>

<span data-ttu-id="2a021-104">E-postmeddelanden för inbjudan är en kritisk komponent återinföra partners på tåget som B2B-samarbete användare i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2a021-104">Invitation emails are a critical component to bring partners on board as B2B collaboration users in Azure AD.</span></span> <span data-ttu-id="2a021-105">Du kan använda dem för att öka mottagarens förtroende.</span><span class="sxs-lookup"><span data-stu-id="2a021-105">You can use them to increase the recipient's trust.</span></span> <span data-ttu-id="2a021-106">Du kan lägga till är giltiga och sociala bevis till e-post, se till mottagaren känns bekväm med att välja den **Kom igång** för att tacka ja till inbjudan.</span><span class="sxs-lookup"><span data-stu-id="2a021-106">you can add legitimacy and social proof to the email, to make sure the recipient feels comfortable with selecting the **Get Started** button to accept the invitation.</span></span> <span data-ttu-id="2a021-107">Förtroendet är en nyckel som innebär att minska delning friktion.</span><span class="sxs-lookup"><span data-stu-id="2a021-107">This trust is a key means to reduce sharing friction.</span></span> <span data-ttu-id="2a021-108">Och du vill se e-postmeddelandet ser bra ut!</span><span class="sxs-lookup"><span data-stu-id="2a021-108">And you also want to make the email look great!</span></span>

![Azure AD B2b inbjudan e-post](media/active-directory-b2b-invitation-email/invitation-email.png)

## <a name="explaining-the-email"></a><span data-ttu-id="2a021-110">Förklarar e-postmeddelandet</span><span class="sxs-lookup"><span data-stu-id="2a021-110">Explaining the email</span></span>
<span data-ttu-id="2a021-111">Nu ska vi titta på några elementen i e-postmeddelandet så att du vet hur du bäst för att använda deras funktioner.</span><span class="sxs-lookup"><span data-stu-id="2a021-111">Let's look at a few elements of the email so you know how best to use their capabilities.</span></span>

### <a name="subject"></a><span data-ttu-id="2a021-112">Ämne</span><span class="sxs-lookup"><span data-stu-id="2a021-112">Subject</span></span>
<span data-ttu-id="2a021-113">Ämnet för e-postmeddelandet efter följande mönster: du inbjuds att den &lt;tenantname&gt; organisation</span><span class="sxs-lookup"><span data-stu-id="2a021-113">The subject of the email follows the following pattern: You're invited to the &lt;tenantname&gt; organization</span></span>

### <a name="from-address"></a><span data-ttu-id="2a021-114">Från-adress</span><span class="sxs-lookup"><span data-stu-id="2a021-114">From address</span></span>
<span data-ttu-id="2a021-115">Vi använder ett LinkedIn-liknande mönster för från-adressen.</span><span class="sxs-lookup"><span data-stu-id="2a021-115">We use a LinkedIn-like pattern for the From address.</span></span>  <span data-ttu-id="2a021-116">Du bör vara tydlig som avsändaren av inbjudan och e-postadress som företagets och även tydliggöra att e-postmeddelandet kommer från Microsoft.</span><span class="sxs-lookup"><span data-stu-id="2a021-116">You should be clear who the inviter is and from which company, and also clarify that the email is coming from a Microsoft email address.</span></span> <span data-ttu-id="2a021-117">Formatet är: &lt;visningsnamnet för bjuder in&gt; från &lt;tenantname&gt; (via Microsoft) <invites@microsoft.com&gt;</span><span class="sxs-lookup"><span data-stu-id="2a021-117">The format is: &lt;Display name of inviter&gt; from &lt;tenantname&gt; (via Microsoft) <invites@microsoft.com&gt;</span></span>

### <a name="reply-to"></a><span data-ttu-id="2a021-118">Svara på</span><span class="sxs-lookup"><span data-stu-id="2a021-118">Reply To</span></span>
<span data-ttu-id="2a021-119">Svara till e-postmeddelandet har angetts till den bjuder in e-post när det är tillgängligt, så att svara på e-postmeddelandet skickar ett e-postmeddelande till avsändaren av inbjudan.</span><span class="sxs-lookup"><span data-stu-id="2a021-119">The reply-to email is set to the inviter's email when available, so that replying to the email sends an email back to the inviter.</span></span>

### <a name="branding"></a><span data-ttu-id="2a021-120">Anpassning</span><span class="sxs-lookup"><span data-stu-id="2a021-120">Branding</span></span>
<span data-ttu-id="2a021-121">E-postmeddelanden för inbjudan från din klient användning företagsanpassning som du kanske har ställt in för din klient.</span><span class="sxs-lookup"><span data-stu-id="2a021-121">The invitation emails from your tenant use the company branding that you may have set up for your tenant.</span></span> <span data-ttu-id="2a021-122">Om du vill dra nytta av den här funktionen [här](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) finns information om hur du konfigurerar den.</span><span class="sxs-lookup"><span data-stu-id="2a021-122">If you want to take advantage of this capability, [here](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) are the details on how to configure it.</span></span> <span data-ttu-id="2a021-123">Banderollslogotypen visas i e-postmeddelandet.</span><span class="sxs-lookup"><span data-stu-id="2a021-123">The banner logo appears in the email.</span></span> <span data-ttu-id="2a021-124">Följ bildstorleken och kvalitet instruktioner [här](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) för bästa resultat.</span><span class="sxs-lookup"><span data-stu-id="2a021-124">Follow the image size and quality instructions [here](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) for best results.</span></span> <span data-ttu-id="2a021-125">Dessutom visas företagsnamnet även i anropet till åtgärd.</span><span class="sxs-lookup"><span data-stu-id="2a021-125">In addition, the company name also shows up in the call to action.</span></span>

### <a name="call-to-action"></a><span data-ttu-id="2a021-126">Anrop till åtgärd</span><span class="sxs-lookup"><span data-stu-id="2a021-126">Call to action</span></span>
<span data-ttu-id="2a021-127">Anropet till åtgärd består av två delar: förklarar varför mottagaren har tagit emot e-postmeddelandet och vad mottagaren blir ombedd att göra om den.</span><span class="sxs-lookup"><span data-stu-id="2a021-127">The call to action consists of two parts: explaining why the recipient has received the mail and what the recipient is being asked to do about it.</span></span>
- <span data-ttu-id="2a021-128">Avsnittet ”Varför” kan åtgärdas med följande mönster: du har bjudits komma åt program i den &lt;tenantname&gt; organisation</span><span class="sxs-lookup"><span data-stu-id="2a021-128">The "why" section can be addressed using the following pattern: You've been invited to access applications in the &lt;tenantname&gt; organization</span></span>

- <span data-ttu-id="2a021-129">Och den ”vad du att uppmanas att göra” avsnittet anges av förekomst av den **Kom igång** knappen.</span><span class="sxs-lookup"><span data-stu-id="2a021-129">And the "what you're being asked to do" section is indicated by the presence of the **Get Started** button.</span></span> <span data-ttu-id="2a021-130">När mottagaren har lagts utan att behöva inbjudningar kan visas den här knappen inte.</span><span class="sxs-lookup"><span data-stu-id="2a021-130">When the recipient has been added without the need for invitations, this button doesn't show up.</span></span>

### <a name="inviters-information"></a><span data-ttu-id="2a021-131">Bjuder INS information</span><span class="sxs-lookup"><span data-stu-id="2a021-131">Inviter's information</span></span>
<span data-ttu-id="2a021-132">Visningsnamn för den bjuder in ingår i e-postmeddelandet.</span><span class="sxs-lookup"><span data-stu-id="2a021-132">The inviter's display name is included in the email.</span></span> <span data-ttu-id="2a021-133">Och dessutom om du har konfigurerat en profilbild för Azure AD-kontot, bjuda in e-postmeddelandet tas samt bilden.</span><span class="sxs-lookup"><span data-stu-id="2a021-133">And in addition, if you've set up a profile picture for your Azure AD account, the inviting email will include that picture as well.</span></span> <span data-ttu-id="2a021-134">Båda är avsedda att öka mottagarens förtroende för e-postmeddelandet.</span><span class="sxs-lookup"><span data-stu-id="2a021-134">Both are intended to increase your recipient's confidence in the email.</span></span>

<span data-ttu-id="2a021-135">Om du ännu inte har konfigurerat din profilbild visas en ikon med den bjuder in initialer i stället för bilden:</span><span class="sxs-lookup"><span data-stu-id="2a021-135">If you haven't yet set up your profile picture, an icon with the inviter's initials in place of the picture is shown:</span></span>

  ![Visa den bjuder in initialer](media/active-directory-b2b-invitation-email/inviters-initials.png)

### <a name="body"></a><span data-ttu-id="2a021-137">Innehåll</span><span class="sxs-lookup"><span data-stu-id="2a021-137">Body</span></span>
<span data-ttu-id="2a021-138">Texten innehåller meddelandet som avsändaren av inbjudan composes eller skickas via inbjudan API.</span><span class="sxs-lookup"><span data-stu-id="2a021-138">The body contains the message that the inviter composes or is passed through the invitation API.</span></span> <span data-ttu-id="2a021-139">Det är ett textområde så inte bearbetar HTML-taggar av säkerhetsskäl.</span><span class="sxs-lookup"><span data-stu-id="2a021-139">It is a text area, so it does not process HTML tags for security reasons.</span></span>

### <a name="footer-section"></a><span data-ttu-id="2a021-140">Sidfotsavsnittet</span><span class="sxs-lookup"><span data-stu-id="2a021-140">Footer section</span></span>
<span data-ttu-id="2a021-141">Sidfoten innehåller Microsoft företags varumärke och gör att mottagaren om e-postmeddelandet skickades från meddelandet.</span><span class="sxs-lookup"><span data-stu-id="2a021-141">The footer contains the Microsoft company brand and lets the recipient know if the email was sent from an unmonitored alias.</span></span> <span data-ttu-id="2a021-142">Särskilda fall:</span><span class="sxs-lookup"><span data-stu-id="2a021-142">Special cases:</span></span>

- <span data-ttu-id="2a021-143">Avsändaren av inbjudan har inte en e-postadress i bjuda in innehavare</span><span class="sxs-lookup"><span data-stu-id="2a021-143">The inviter doesn't have an email address in the inviting tenancy</span></span>

  ![Bild av bjuder in har inte en e-postadress i bjuda in innehavare](media/active-directory-b2b-invitation-email/inviter-no-email.png)


- <span data-ttu-id="2a021-145">Mottagaren behöver inte lösa inbjudan</span><span class="sxs-lookup"><span data-stu-id="2a021-145">The recipient doesn't need to redeem the invitation</span></span>

  ![När mottagaren inte behöver lösa inbjudan](media/active-directory-b2b-invitation-email/when-recipient-doesnt-redeem.png)


## <a name="next-steps"></a><span data-ttu-id="2a021-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2a021-147">Next steps</span></span>

<span data-ttu-id="2a021-148">Läs andra artiklar om Azure AD B2B-samarbete:</span><span class="sxs-lookup"><span data-stu-id="2a021-148">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="2a021-149">Vad är Azure AD B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="2a021-149">What is Azure AD B2B collaboration</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="2a021-150">Hur lägger Azure Active Directory-administratörer till B2B-samarbete användare?</span><span class="sxs-lookup"><span data-stu-id="2a021-150">How do Azure Active Directory admins add B2B collaboration users?</span></span>](active-directory-b2b-admin-add-users.md)
* [<span data-ttu-id="2a021-151">Hur lägger informationsarbetare till B2B-samarbete användare?</span><span class="sxs-lookup"><span data-stu-id="2a021-151">How do information workers add B2B collaboration users?</span></span>](active-directory-b2b-iw-add-users.md)
* [<span data-ttu-id="2a021-152">B2B-samarbete inbjudan inlösning</span><span class="sxs-lookup"><span data-stu-id="2a021-152">B2B collaboration invitation redemption</span></span>](active-directory-b2b-redemption-experience.md)
* [<span data-ttu-id="2a021-153">Azure AD B2B-samarbete och licensiering</span><span class="sxs-lookup"><span data-stu-id="2a021-153">Azure AD B2B collaboration licensing</span></span>](active-directory-b2b-licensing.md)
* [<span data-ttu-id="2a021-154">Felsökning av Azure Active Directory B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="2a021-154">Troubleshooting Azure Active Directory B2B collaboration</span></span>](active-directory-b2b-troubleshooting.md)
* [<span data-ttu-id="2a021-155">Vanliga och frågor svar om Azure Active Directory B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="2a021-155">Azure Active Directory B2B collaboration frequently asked questions (FAQ)</span></span>](active-directory-b2b-faq.md)
* [<span data-ttu-id="2a021-156">Azure Active Directory B2B-samarbete API och anpassning</span><span class="sxs-lookup"><span data-stu-id="2a021-156">Azure Active Directory B2B collaboration API and customization</span></span>](active-directory-b2b-api.md)
* [<span data-ttu-id="2a021-157">Multi-Factor Authentication för användare av B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="2a021-157">Multi-factor authentication for B2B collaboration users</span></span>](active-directory-b2b-mfa-instructions.md)
* [<span data-ttu-id="2a021-158">Lägg till B2B-samarbete användare utan inbjudan</span><span class="sxs-lookup"><span data-stu-id="2a021-158">Add B2B collaboration users without an invitation</span></span>](active-directory-b2b-add-user-without-invite.md)
* [<span data-ttu-id="2a021-159">Artikelindex för programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2a021-159">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
