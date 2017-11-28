---
title: aaaHow tooadd programgalleriet ett program med flera innehavare toohello Azure AD | Microsoft Docs
description: Beskriver hur du kan ange anpassade utvecklade flera innehavare program i hello Azure AD Application Gallery
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 2dc6e0d783835d2639a7e6dda172110ee860a977
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooadd-a-multi-tenant-application-toohello-azure-ad-application-gallery"></a><span data-ttu-id="040be-103">Hur tooadd ett program med flera innehavare toohello Azure AD-programgalleriet</span><span class="sxs-lookup"><span data-stu-id="040be-103">How tooadd a multi-tenant application toohello Azure AD application gallery</span></span>

## <a name="what-is-hello-azure-ad-application-gallery"></a><span data-ttu-id="040be-104">Vad är hello Azure AD Application Gallery?</span><span class="sxs-lookup"><span data-stu-id="040be-104">What is hello Azure AD Application Gallery?</span></span>

<span data-ttu-id="040be-105">hello Azure AD Application Gallery är ett bra sätt tooget programmet framför alla hello miljoner av Azure Active Directory kunder toobroaden hello effekt och räckvidden hos ditt program i hello marketplace.</span><span class="sxs-lookup"><span data-stu-id="040be-105">hello Azure AD Application Gallery is a great way tooget your application in front of all hello millions of Azure Active Directory customers toobroaden hello impact and reach of your application in hello marketplace.</span></span> <span data-ttu-id="040be-106">hello nedanstående steg förklarar hur du kan visa ditt program i hello Azure AD Application Gallery.</span><span class="sxs-lookup"><span data-stu-id="040be-106">hello below steps explain how you can list your application in hello Azure AD Application Gallery.</span></span>

## <a name="if-your-application-supports-saml-or-openidconnect"></a><span data-ttu-id="040be-107">Om programmet stöder SAML eller OpenIDConnect</span><span class="sxs-lookup"><span data-stu-id="040be-107">If your application supports SAML or OpenIDConnect</span></span>
<span data-ttu-id="040be-108">Om du har ett program för flera innehavare som du vill att toolist i hello Azure AD Application Gallery, måste du först kontrollera att ditt program stöder en av följande tekniker för enkel inloggning hello:</span><span class="sxs-lookup"><span data-stu-id="040be-108">If you have a multi-tenant application you'd like toolist in hello Azure AD Application Gallery, you must first make sure that your application supports one of hello following single sign-on technologies:</span></span>

1. <span data-ttu-id="040be-109">**OpenID Connect** - Direktintegrering med Azure AD med OpenID Connect för autentisering och hello Azure AD medgivande API för konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="040be-109">**OpenID Connect** - Direct integration with Azure AD using OpenID Connect for authentication and hello Azure AD consent API for configuration.</span></span> <span data-ttu-id="040be-110">Om du bara startar en integration och programmet har inte stöd för SAML är hello rekommenderar läge.</span><span class="sxs-lookup"><span data-stu-id="040be-110">If you are just starting an integration and your application does not support SAML, then this is hello recommend mode.</span></span>
2. <span data-ttu-id="040be-111">**SAML** – ditt program redan har hello möjlighet tooconfigure från tredje part identitetsleverantörer med hello SAML-protokoll.</span><span class="sxs-lookup"><span data-stu-id="040be-111">**SAML** – Your application already has hello ability tooconfigure third-party identity providers using hello SAML protocol.</span></span>

<span data-ttu-id="040be-112">Om programmet stöder en av dessa lägen för enkel inloggning och du vill att toolist tillämpningsprogrammet flera innehavare i hello Azure AD Application Gallery som du kan följa hello stegen i hello dokumentet nedan.</span><span class="sxs-lookup"><span data-stu-id="040be-112">If your application supports one of these single sign-on modes and you'd like toolist your multi-tenant application in hello Azure AD Application Gallery, you can follow hello steps in hello document below.</span></span> <span data-ttu-id="040be-113">tooget igång snabbt skicka ett e-postmeddelande för**waadpartners@microsoft.com**.</span><span class="sxs-lookup"><span data-stu-id="040be-113">tooget started quickly send an email too**waadpartners@microsoft.com**.</span></span>

## <a name="if-your-application-does-not-support-saml-or-openidconnect"></a><span data-ttu-id="040be-114">Om ditt program inte stöder SAML eller OpenIDConnect</span><span class="sxs-lookup"><span data-stu-id="040be-114">If your application does not support SAML or OpenIDConnect</span></span>
<span data-ttu-id="040be-115">Även om programmet inte stöder en av dessa lägen, integrera vi fortfarande den i vår galleriet med vår lösenord Single Sign-on-teknik.</span><span class="sxs-lookup"><span data-stu-id="040be-115">Even if your application does not support one of these modes, we can still integrate it into our gallery using our Password Single Sign-on technology.</span></span> <span data-ttu-id="040be-116">Om du vill att tooexplore det här alternativet kan du skicka ett e-postmeddelande för**waadpartners@microsoft.com**.</span><span class="sxs-lookup"><span data-stu-id="040be-116">If you'd like tooexplore this option, you can send an email too**waadpartners@microsoft.com**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="040be-117">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="040be-117">Next steps</span></span>
[<span data-ttu-id="040be-118">Hur toolist ditt program i hello Azure Active Directory-programgalleriet</span><span class="sxs-lookup"><span data-stu-id="040be-118">How toolist your application in hello Azure Active Directory application gallery</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)
