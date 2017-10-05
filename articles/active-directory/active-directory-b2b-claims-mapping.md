---
title: "B2B-samarbete användaranspråk mappning i Azure Active Directory | Microsoft Docs"
description: "anspråk mappning referens för Azure Active Directory B2B-samarbete"
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
ms.date: 03/15/2017
ms.author: sasubram
ms.openlocfilehash: 5f8559450b24effd40a38879aeae3a8dd03944a3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="b2b-collaboration-user-claims-mapping-in-azure-active-directory"></a><span data-ttu-id="4d4b0-103">B2B-samarbete användaranspråk mappning i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="4d4b0-103">B2B collaboration user claims mapping in Azure Active Directory</span></span>

<span data-ttu-id="4d4b0-104">Azure Active Directory (AD Azure) stöder anpassa anspråk som utfärdats i SAML-token för B2B-samarbete användare.</span><span class="sxs-lookup"><span data-stu-id="4d4b0-104">Azure Active Directory (Azure AD) supports customizing the claims issued in the SAML token for B2B collaboration users.</span></span> <span data-ttu-id="4d4b0-105">När en användare autentiseras för utfärdar Azure AD en SAML-token till appen som innehåller information (eller anspråk) om användaren som unikt identifierar dem.</span><span class="sxs-lookup"><span data-stu-id="4d4b0-105">When a user authenticates to the application, Azure AD issues a SAML token to the app that contains information (or claims) about the user that uniquely identifies them.</span></span> <span data-ttu-id="4d4b0-106">Som standard innehåller detta användarens användarnamn, e-postadress, Förnamn och efternamn.</span><span class="sxs-lookup"><span data-stu-id="4d4b0-106">By default, this includes the user's user name, email address, first name, and last name.</span></span> <span data-ttu-id="4d4b0-107">Du kan visa eller redigera anspråk skickas i SAML-token till programmet under fliken attribut.</span><span class="sxs-lookup"><span data-stu-id="4d4b0-107">You can view or edit the claims sent in the SAML token to the application under the Attributes tab.</span></span>

<span data-ttu-id="4d4b0-108">Det finns två möjliga orsaker till varför du kan behöva redigera anspråk som utfärdats i SAML-token.</span><span class="sxs-lookup"><span data-stu-id="4d4b0-108">There are two possible reasons why you might need to edit the claims issued in the SAML token.</span></span>

1. <span data-ttu-id="4d4b0-109">Programmet har skrivits till kräver en annan uppsättning anspråk URI: er eller anspråksvärden</span><span class="sxs-lookup"><span data-stu-id="4d4b0-109">The application has been written to require a different set of claim URIs or claim values</span></span>

2. <span data-ttu-id="4d4b0-110">NameIdentifier att anspråket ska vara något annat än användarens huvudnamn som lagras i Azure Active Directory krävs för ditt program.</span><span class="sxs-lookup"><span data-stu-id="4d4b0-110">Your application requires the NameIdentifier claim to be something other than the user principal name stored in Azure Active Directory.</span></span>

  ![Visa anspråk i SAML-token](media/active-directory-b2b-claims-mapping/view-claims-in-saml-token.png)

<span data-ttu-id="4d4b0-112">Information om hur du lägger till och redigera anspråk finns i följande artikel på anspråk anpassning [anpassa anspråk som utfärdats i SAML-token för förintegrerade appar i Azure Active Directory](develop/active-directory-saml-claims-customization.md).</span><span class="sxs-lookup"><span data-stu-id="4d4b0-112">For information on how to add and edit claims, check out this article on claims customization, [Customizing claims issued in the SAML token for pre-integrated apps in Azure Active Directory](develop/active-directory-saml-claims-customization.md).</span></span> <span data-ttu-id="4d4b0-113">Användare, mappning NameID och UPN cross-klient som inte av säkerhetsskäl för B2B-samarbete.</span><span class="sxs-lookup"><span data-stu-id="4d4b0-113">For B2B collaboration users, mapping NameID and UPN cross-tenant are prevented for security reasons.</span></span>


## <a name="next-steps"></a><span data-ttu-id="4d4b0-114">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4d4b0-114">Next steps</span></span>

<span data-ttu-id="4d4b0-115">Läs andra artiklar om Azure AD B2B-samarbete:</span><span class="sxs-lookup"><span data-stu-id="4d4b0-115">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="4d4b0-116">Vad är Azure AD B2B-samarbete?</span><span class="sxs-lookup"><span data-stu-id="4d4b0-116">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="4d4b0-117">Egenskaper för användare av B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="4d4b0-117">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="4d4b0-118">Lägger till en B2B-samarbete användare till en roll</span><span class="sxs-lookup"><span data-stu-id="4d4b0-118">Adding a B2B collaboration user to a role</span></span>](active-directory-b2b-add-guest-to-role.md)
* [<span data-ttu-id="4d4b0-119">Delegera B2bB samarbete inbjudningar</span><span class="sxs-lookup"><span data-stu-id="4d4b0-119">Delegate B2bB collaboration invitations</span></span>](active-directory-b2b-delegate-invitations.md)
* [<span data-ttu-id="4d4b0-120">Dynamiska grupper och B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="4d4b0-120">Dynamic groups and B2B collaboration</span></span>](active-directory-b2b-dynamic-groups.md)
* [<span data-ttu-id="4d4b0-121">B2B-samarbete kod och PowerShell-exempel</span><span class="sxs-lookup"><span data-stu-id="4d4b0-121">B2B collaboration code and PowerShell samples</span></span>](active-directory-b2b-code-samples.md)
* [<span data-ttu-id="4d4b0-122">Konfigurera SaaS-appar för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="4d4b0-122">Configure SaaS apps for B2B collaboration</span></span>](active-directory-b2b-configure-saas-apps.md)
* [<span data-ttu-id="4d4b0-123">Extern delning av Office 365</span><span class="sxs-lookup"><span data-stu-id="4d4b0-123">Office 365 external sharing</span></span>](active-directory-b2b-o365-external-user.md)
* [<span data-ttu-id="4d4b0-124">Användartoken för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="4d4b0-124">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="4d4b0-125">Aktuella begränsningar för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="4d4b0-125">B2B collaboration current limitations</span></span>](active-directory-b2b-current-limitations.md)
