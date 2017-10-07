---
title: "aaaB2B samarbete användaranspråk mappning i Azure Active Directory | Microsoft Docs"
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
ms.openlocfilehash: 9e26085e91a6004b2f11286ae9c1df133bd47341
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="b2b-collaboration-user-claims-mapping-in-azure-active-directory"></a><span data-ttu-id="6c703-103">B2B-samarbete användaranspråk mappning i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6c703-103">B2B collaboration user claims mapping in Azure Active Directory</span></span>

<span data-ttu-id="6c703-104">Azure Active Directory (AD Azure) stöder anpassa hello anspråk som utfärdats i hello SAML-token för B2B-samarbete användare.</span><span class="sxs-lookup"><span data-stu-id="6c703-104">Azure Active Directory (Azure AD) supports customizing hello claims issued in hello SAML token for B2B collaboration users.</span></span> <span data-ttu-id="6c703-105">Azure AD utfärdar en SAML-token toohello app som innehåller information (eller anspråk) om hello-användaren som unikt identifierar dem när en användare autentiseras toohello program.</span><span class="sxs-lookup"><span data-stu-id="6c703-105">When a user authenticates toohello application, Azure AD issues a SAML token toohello app that contains information (or claims) about hello user that uniquely identifies them.</span></span> <span data-ttu-id="6c703-106">Som standard innehåller detta hello användarens användarnamn, e-postadress, Förnamn och efternamn.</span><span class="sxs-lookup"><span data-stu-id="6c703-106">By default, this includes hello user's user name, email address, first name, and last name.</span></span> <span data-ttu-id="6c703-107">Du kan visa eller redigera hello anspråk skickas i hello SAML-token toohello programmet under fliken för hello-attribut.</span><span class="sxs-lookup"><span data-stu-id="6c703-107">You can view or edit hello claims sent in hello SAML token toohello application under hello Attributes tab.</span></span>

<span data-ttu-id="6c703-108">Det finns två möjliga orsaker till varför du kanske behöver tooedit hello anspråk som utfärdats i hello SAML-token.</span><span class="sxs-lookup"><span data-stu-id="6c703-108">There are two possible reasons why you might need tooedit hello claims issued in hello SAML token.</span></span>

1. <span data-ttu-id="6c703-109">programmet hello har skrivits toorequire en annan uppsättning anspråk URI: er eller anspråksvärden</span><span class="sxs-lookup"><span data-stu-id="6c703-109">hello application has been written toorequire a different set of claim URIs or claim values</span></span>

2. <span data-ttu-id="6c703-110">Programmet kräver hello NameIdentifier anspråk toobe något annat än hello användarens huvudnamn lagras i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="6c703-110">Your application requires hello NameIdentifier claim toobe something other than hello user principal name stored in Azure Active Directory.</span></span>

  ![Visa anspråk i SAML-token](media/active-directory-b2b-claims-mapping/view-claims-in-saml-token.png)

<span data-ttu-id="6c703-112">Information om hur tooadd och redigera anspråk checka ut denna artikel på anspråk anpassning [anpassa anspråk som utfärdats i hello SAML-token för förintegrerade appar i Azure Active Directory](develop/active-directory-saml-claims-customization.md).</span><span class="sxs-lookup"><span data-stu-id="6c703-112">For information on how tooadd and edit claims, check out this article on claims customization, [Customizing claims issued in hello SAML token for pre-integrated apps in Azure Active Directory](develop/active-directory-saml-claims-customization.md).</span></span> <span data-ttu-id="6c703-113">Användare, mappning NameID och UPN cross-klient som inte av säkerhetsskäl för B2B-samarbete.</span><span class="sxs-lookup"><span data-stu-id="6c703-113">For B2B collaboration users, mapping NameID and UPN cross-tenant are prevented for security reasons.</span></span>


## <a name="next-steps"></a><span data-ttu-id="6c703-114">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6c703-114">Next steps</span></span>

<span data-ttu-id="6c703-115">Läs andra artiklar om Azure AD B2B-samarbete:</span><span class="sxs-lookup"><span data-stu-id="6c703-115">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="6c703-116">Vad är Azure AD B2B-samarbete?</span><span class="sxs-lookup"><span data-stu-id="6c703-116">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="6c703-117">Egenskaper för användare av B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="6c703-117">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="6c703-118">Lägga till en B2B-samarbete tooa användarroll</span><span class="sxs-lookup"><span data-stu-id="6c703-118">Adding a B2B collaboration user tooa role</span></span>](active-directory-b2b-add-guest-to-role.md)
* [<span data-ttu-id="6c703-119">Delegera B2bB samarbete inbjudningar</span><span class="sxs-lookup"><span data-stu-id="6c703-119">Delegate B2bB collaboration invitations</span></span>](active-directory-b2b-delegate-invitations.md)
* [<span data-ttu-id="6c703-120">Dynamiska grupper och B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="6c703-120">Dynamic groups and B2B collaboration</span></span>](active-directory-b2b-dynamic-groups.md)
* [<span data-ttu-id="6c703-121">B2B-samarbete kod och PowerShell-exempel</span><span class="sxs-lookup"><span data-stu-id="6c703-121">B2B collaboration code and PowerShell samples</span></span>](active-directory-b2b-code-samples.md)
* [<span data-ttu-id="6c703-122">Konfigurera SaaS-appar för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="6c703-122">Configure SaaS apps for B2B collaboration</span></span>](active-directory-b2b-configure-saas-apps.md)
* [<span data-ttu-id="6c703-123">Extern delning av Office 365</span><span class="sxs-lookup"><span data-stu-id="6c703-123">Office 365 external sharing</span></span>](active-directory-b2b-o365-external-user.md)
* [<span data-ttu-id="6c703-124">Användartoken för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="6c703-124">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="6c703-125">Aktuella begränsningar för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="6c703-125">B2B collaboration current limitations</span></span>](active-directory-b2b-current-limitations.md)
