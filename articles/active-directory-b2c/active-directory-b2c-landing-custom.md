---
title: 'Azure Active Directory B2C: Anpassade principer (landningssida) | Microsoft Docs'
description: Utveckla konsumentinriktade program med Azure Active Directory B2C med anpassade principer
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: f2079f53-a637-4f2d-b3a0-61a9647ad433
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 5/06/2017
ms.author: parakhj
ms.openlocfilehash: 28a39f282488b81fc9e2ab7841b5f2cb4cd58715
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-sign-up-and-sign-in-consumers-in-your-applications-using-custom-policies"></a><span data-ttu-id="448b1-103">Azure Active Directory B2C: Registrera och logga in användare i dina program med anpassade principer</span><span class="sxs-lookup"><span data-stu-id="448b1-103">Azure Active Directory B2C: Sign up and sign in consumers in your applications using custom policies</span></span>
<span data-ttu-id="448b1-104">Anpassade principer är konfigurationsfiler som definierar hello beteende för din Azure AD B2C-klient.</span><span class="sxs-lookup"><span data-stu-id="448b1-104">Custom policies are configuration files that define hello behavior of your Azure AD B2C tenant.</span></span> <span data-ttu-id="448b1-105">De kan vara helt redigeras av en identitet developer toocomplete en nära obegränsat antal uppgifter.</span><span class="sxs-lookup"><span data-stu-id="448b1-105">They can be fully edited by an identity developer toocomplete a near unlimited number of tasks.</span></span>

## <a name="how-tooarticles"></a><span data-ttu-id="448b1-106">Hur tooarticles</span><span class="sxs-lookup"><span data-stu-id="448b1-106">How-tooarticles</span></span>
<span data-ttu-id="448b1-107">Lär dig hur toouse specifika funktioner i Azure Active Directory B2C:</span><span class="sxs-lookup"><span data-stu-id="448b1-107">Learn how toouse specific Azure Active Directory B2C features:</span></span>

* [<span data-ttu-id="448b1-108">Kom igång</span><span class="sxs-lookup"><span data-stu-id="448b1-108">Get Started</span></span>](active-directory-b2c-overview-custom.md)
* <span data-ttu-id="448b1-109">Konfigurera OIDC-leverantörer som [Azure AD](active-directory-b2c-setup-aad-custom.md).</span><span class="sxs-lookup"><span data-stu-id="448b1-109">Configure OIDC Providers such as [Azure AD](active-directory-b2c-setup-aad-custom.md).</span></span>
* <span data-ttu-id="448b1-110">Konfigurera SAML-leverantörer som [Salesforce](active-directory-b2c-setup-sf-app-custom.md).</span><span class="sxs-lookup"><span data-stu-id="448b1-110">Configure SAML Providers such as [Salesforce](active-directory-b2c-setup-sf-app-custom.md).</span></span>
* <span data-ttu-id="448b1-111">Integrera RESTful-API:er:</span><span class="sxs-lookup"><span data-stu-id="448b1-111">Integrate RESTful APIs:</span></span>
    * <span data-ttu-id="448b1-112">[Hämta ytterligare anspråk](active-directory-b2c-rest-api-step-custom.md).</span><span class="sxs-lookup"><span data-stu-id="448b1-112">[Obtain additional claims](active-directory-b2c-rest-api-step-custom.md).</span></span>
    * <span data-ttu-id="448b1-113">[Verifiera användarinmatning](active-directory-b2c-rest-api-validation-custom.md).</span><span class="sxs-lookup"><span data-stu-id="448b1-113">[Validate user input](active-directory-b2c-rest-api-validation-custom.md).</span></span>
* <span data-ttu-id="448b1-114">Anpassa inloggning:</span><span class="sxs-lookup"><span data-stu-id="448b1-114">Customize Login:</span></span>
    * [<span data-ttu-id="448b1-115">Konfigurera användarinmatning</span><span class="sxs-lookup"><span data-stu-id="448b1-115">Configure User Input</span></span>](active-directory-b2c-configure-signup-self-asserted-custom.md)
    * [<span data-ttu-id="448b1-116">Anpassa användargränssnitt</span><span class="sxs-lookup"><span data-stu-id="448b1-116">Customize UI</span></span>](active-directory-b2c-ui-customization-custom.md)
    * [<span data-ttu-id="448b1-117">Anpassa token</span><span class="sxs-lookup"><span data-stu-id="448b1-117">Customize Token</span></span>](active-directory-b2c-reference-manage-sso-and-token-configuration.md)
* <span data-ttu-id="448b1-118">Felsöka genom att [samla in loggar med Application Insights](active-directory-b2c-troubleshoot-custom.md).</span><span class="sxs-lookup"><span data-stu-id="448b1-118">Troubleshoot by [collecting logs using Application Insights](active-directory-b2c-troubleshoot-custom.md).</span></span>

## <a name="whats-new"></a><span data-ttu-id="448b1-119">Nyheter</span><span class="sxs-lookup"><span data-stu-id="448b1-119">What's new</span></span>
<span data-ttu-id="448b1-120">Kom tillbaka ofta toolearn om framtida ändringar toohello Azure Active Directory B2C.</span><span class="sxs-lookup"><span data-stu-id="448b1-120">Check back here often toolearn about future changes toohello Azure Active Directory B2C.</span></span> <span data-ttu-id="448b1-121">Vi kommer även att twittra om eventuella uppdateringar med @AzureAD.</span><span class="sxs-lookup"><span data-stu-id="448b1-121">We'll also tweet about any updates by using @AzureAD.</span></span>



