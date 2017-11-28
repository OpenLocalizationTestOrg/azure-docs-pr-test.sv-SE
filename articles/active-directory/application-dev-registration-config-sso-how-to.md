---
title: "aaaHow tooconfigure ett nytt program för flera innehavare | Microsoft Docs"
description: "Hur tooconfigure enkel inloggning för ett anpassat program som du utvecklar och registreras med Azure AD."
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
ms.openlocfilehash: 4d3499d8885933516d6597fa9f87bcf88cd5a428
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-a-new-multi-tenant-application"></a><span data-ttu-id="8a2fa-103">Hur tooconfigure ett nytt program för flera innehavare</span><span class="sxs-lookup"><span data-stu-id="8a2fa-103">How tooconfigure a new multi-tenant application</span></span>

<span data-ttu-id="8a2fa-104">Aktivera federerad enkel inloggning (SSO) i din app aktiveras automatiskt när federering via Azure AD för OpenID Connect, SAML 2.0 och WS-Fed.</span><span class="sxs-lookup"><span data-stu-id="8a2fa-104">Enabling federated single sign-on (SSO) in your app is automatically enabled when federating through Azure AD for OpenID Connect, SAML 2.0, or WS-Fed.</span></span> <span data-ttu-id="8a2fa-105">Om slutanvändarna har toosign i trots redan har en befintlig session med Azure AD, är det troligt att din app kan vara felkonfigurerad.</span><span class="sxs-lookup"><span data-stu-id="8a2fa-105">If your end users are having toosign in despite already having an existing session with Azure AD, it’s likely your app may be misconfigured.</span></span>

* <span data-ttu-id="8a2fa-106">Om du använder ADAL/MSAL, kontrollera att du har **PromptBehavior** ställa in också**automatisk** snarare än **alltid**.</span><span class="sxs-lookup"><span data-stu-id="8a2fa-106">If you’re using ADAL/MSAL, make sure you have **PromptBehavior** set too**Auto** rather than **Always**.</span></span>

* <span data-ttu-id="8a2fa-107">Om du utvecklar en mobil app eventuellt ytterligare konfigurationer tooenable asynkrona eller icke-asynkrona enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="8a2fa-107">If you’re building a mobile app, you may need additional configurations tooenable brokered or non-brokered SSO.</span></span>

<span data-ttu-id="8a2fa-108">För Android, se [aktivera Cross App SSO i Android](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-android).</span><span class="sxs-lookup"><span data-stu-id="8a2fa-108">For Android, see [Enabling Cross App SSO in Android](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-android).</span></span><br>

<span data-ttu-id="8a2fa-109">För iOS, se [aktivera Cross App SSO i iOS](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-ios).</span><span class="sxs-lookup"><span data-stu-id="8a2fa-109">For iOS, see [Enabling Cross App SSO in iOS](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-ios).</span></span>

## <a name="next-steps"></a><span data-ttu-id="8a2fa-110">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8a2fa-110">Next steps</span></span>

[<span data-ttu-id="8a2fa-111">Azure AD för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8a2fa-111">Azure AD SSO</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)<br>

[<span data-ttu-id="8a2fa-112">Aktivera mellan appen SSO i Android</span><span class="sxs-lookup"><span data-stu-id="8a2fa-112">Enabling Cross App SSO in Android</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-android)<br>

[<span data-ttu-id="8a2fa-113">Aktivera Cross App SSO i iOS</span><span class="sxs-lookup"><span data-stu-id="8a2fa-113">Enabling Cross App SSO in iOS</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-ios)<br>

[<span data-ttu-id="8a2fa-114">Integrera appar tooAzureAD</span><span class="sxs-lookup"><span data-stu-id="8a2fa-114">Integrating Apps tooAzureAD</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)<br>

[<span data-ttu-id="8a2fa-115">Medgivande och Permissioning för AzureAD v2.0 konvergerat appar</span><span class="sxs-lookup"><span data-stu-id="8a2fa-115">Consent and Permissioning for AzureAD v2.0 converged Apps</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[<span data-ttu-id="8a2fa-116">AzureAD StackOverflow</span><span class="sxs-lookup"><span data-stu-id="8a2fa-116">AzureAD StackOverflow</span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)
