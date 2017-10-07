---
title: "Azure AD Connect: Sömlös Single Sign-On - hur det fungerar | Microsoft Docs"
description: "Den här artikeln beskriver hur hello Azure Active Directory sömlös enkel inloggning funktionen fungerar."
services: active-directory
keywords: "Vad är Azure AD Connect, installera Active Directory, nödvändiga komponenter för Azure AD, SSO, Single Sign-on"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: billmath
ms.openlocfilehash: 17ce35b32832d241068ab878cf7aac42deab74ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-seamless-single-sign-on-technical-deep-dive"></a><span data-ttu-id="99cc5-104">Azure Active Directory sömlös enkel inloggning: tekniska ingående</span><span class="sxs-lookup"><span data-stu-id="99cc5-104">Azure Active Directory Seamless Single Sign-On: Technical deep dive</span></span>

<span data-ttu-id="99cc5-105">Den här artikeln får du teknisk information till hur hello Azure Active Directory sömlös enkel inloggning (SSO sömlös) fungerar.</span><span class="sxs-lookup"><span data-stu-id="99cc5-105">This article gives you technical details into how hello Azure Active Directory Seamless Single Sign-On (Seamless SSO) feature works.</span></span>

>[!IMPORTANT]
><span data-ttu-id="99cc5-106">hello sömlös SSO-funktionen är för närvarande under förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="99cc5-106">hello Seamless SSO feature is currently in preview.</span></span>

## <a name="how-does-seamless-sso-work"></a><span data-ttu-id="99cc5-107">Hur fungerar sömlös SSO?</span><span class="sxs-lookup"><span data-stu-id="99cc5-107">How does Seamless SSO work?</span></span>

<span data-ttu-id="99cc5-108">Det här avsnittet har två delar tooit:</span><span class="sxs-lookup"><span data-stu-id="99cc5-108">This section has two parts tooit:</span></span>
1. <span data-ttu-id="99cc5-109">hello inställning av hello sömlös SSO-funktionen.</span><span class="sxs-lookup"><span data-stu-id="99cc5-109">hello setup of hello Seamless SSO feature.</span></span>
2. <span data-ttu-id="99cc5-110">Hur fungerar en enskild användare logga in transaktion med sömlös SSO.</span><span class="sxs-lookup"><span data-stu-id="99cc5-110">How a single user sign-in transaction works with Seamless SSO.</span></span>

### <a name="how-does-set-up-work"></a><span data-ttu-id="99cc5-111">Hur ställa in arbete?</span><span class="sxs-lookup"><span data-stu-id="99cc5-111">How does set up work?</span></span>

<span data-ttu-id="99cc5-112">Sömlös SSO aktiveras med Azure AD Connect enligt [här](active-directory-aadconnect-sso-quick-start.md).</span><span class="sxs-lookup"><span data-stu-id="99cc5-112">Seamless SSO is enabled using Azure AD Connect as shown [here](active-directory-aadconnect-sso-quick-start.md).</span></span> <span data-ttu-id="99cc5-113">Att aktivera funktionen hello inträffa hello följande:</span><span class="sxs-lookup"><span data-stu-id="99cc5-113">While enabling hello feature, hello following steps occur:</span></span>
- <span data-ttu-id="99cc5-114">Ett konto med namnet `AZUREADSSOACCT` (som motsvarar Azure AD) skapas i din lokala Active Directory (AD).</span><span class="sxs-lookup"><span data-stu-id="99cc5-114">A computer account named `AZUREADSSOACCT` (which represents Azure AD) is created in your on-premises Active Directory (AD).</span></span>
- <span data-ttu-id="99cc5-115">hello datorkontos Kerberos dekrypteringsnyckeln delas på ett säkert sätt med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="99cc5-115">hello computer account's Kerberos decryption key is shared securely with Azure AD.</span></span>
- <span data-ttu-id="99cc5-116">Dessutom skapas två Kerberos tjänstens huvudnamn (SPN) toorepresent två URL: er som används under Azure AD-inloggning.</span><span class="sxs-lookup"><span data-stu-id="99cc5-116">In addition, two Kerberos service principal names (SPNs) are created toorepresent two URLs that are used during Azure AD sign-in.</span></span>

>[!NOTE]
> <span data-ttu-id="99cc5-117">hello datorkonto och Kerberos SPN du hello skapas i varje AD-skog du synkronisera tooAzure AD (med Azure AD Connect) och vars användare du vill ha sömlös SSO.</span><span class="sxs-lookup"><span data-stu-id="99cc5-117">hello computer account and hello Kerberos SPNs are created in each AD forest you synchronize tooAzure AD (using Azure AD Connect) and for whose users you want Seamless SSO.</span></span> <span data-ttu-id="99cc5-118">Flytta hello `AZUREADSSOACCT` datorn konto tooan organisationsenhet (OU) där andra datorkonton är lagrade tooensure som hanteras i hello samma sätt och tas inte bort.</span><span class="sxs-lookup"><span data-stu-id="99cc5-118">Move hello `AZUREADSSOACCT` computer account tooan Organization Unit (OU) where other computer accounts are stored tooensure that it is managed in hello same way and is not deleted.</span></span>

>[!IMPORTANT]
><span data-ttu-id="99cc5-119">Hög rekommenderar vi att du [rulla över hello Kerberos dekrypteringsnyckeln](active-directory-aadconnect-sso-faq.md#how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacct-computer-account) av hello `AZUREADSSOACCT` datorkonto minst var 30 dagar.</span><span class="sxs-lookup"><span data-stu-id="99cc5-119">We highly recommend that you [roll over hello Kerberos decryption key](active-directory-aadconnect-sso-faq.md#how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacct-computer-account) of hello `AZUREADSSOACCT` computer account at least every 30 days.</span></span>

### <a name="how-does-sign-in-with-seamless-sso-work"></a><span data-ttu-id="99cc5-120">Hur logga in med sömlös SSO arbete?</span><span class="sxs-lookup"><span data-stu-id="99cc5-120">How does sign-in with Seamless SSO work?</span></span>

<span data-ttu-id="99cc5-121">När hello installation är klar, fungerar sömlös SSO hello samma sätt som andra inloggning som använder integrerad Windows Windowsautentisering (IWA).</span><span class="sxs-lookup"><span data-stu-id="99cc5-121">Once hello set-up is complete, Seamless SSO works hello same way as any other sign-in that uses Integrated Windows Authentication (IWA).</span></span> <span data-ttu-id="99cc5-122">hello flödet är följande:</span><span class="sxs-lookup"><span data-stu-id="99cc5-122">hello flow is as follows:</span></span>

1. <span data-ttu-id="99cc5-123">hello användare försöker tooaccess ett program (till exempel hello Outlook Web App - https://outlook.office365.com/owa/) från en domänansluten företagets enheter i företagsnätverket.</span><span class="sxs-lookup"><span data-stu-id="99cc5-123">hello user tries tooaccess an application (for example, hello Outlook Web App - https://outlook.office365.com/owa/) from a domain-joined corporate device inside your corporate network.</span></span>
2. <span data-ttu-id="99cc5-124">Om hello användaren inte redan har loggat in är hello användaren omdirigerade toohello Azure AD-inloggningssida.</span><span class="sxs-lookup"><span data-stu-id="99cc5-124">If hello user is not already signed in, hello user is redirected toohello Azure AD sign-in page.</span></span>

  >[!NOTE]
  ><span data-ttu-id="99cc5-125">Om hello Azure AD-inloggning begäran innehåller en `domain_hint` (identifiera klient - till exempel, contoso.onmicrosoft.com) eller `login_hint` (identifiera hello användare – till exempel user@contoso.onmicrosoft.com eller user@contoso.com) parametern sedan steg 2 hoppas över.</span><span class="sxs-lookup"><span data-stu-id="99cc5-125">If hello Azure AD sign-in request includes a `domain_hint` (identifying your tenant- for example, contoso.onmicrosoft.com) or `login_hint` (identifying hello user - for example, user@contoso.onmicrosoft.com or user@contoso.com) parameter, then step 2 is skipped.</span></span>

3. <span data-ttu-id="99cc5-126">hello användaren skriver sitt användarnamn i hello Azure AD-inloggningssida.</span><span class="sxs-lookup"><span data-stu-id="99cc5-126">hello user types in their user name into hello Azure AD sign-in page.</span></span>
4. <span data-ttu-id="99cc5-127">Azure AD anropar med hjälp av JavaScript i bakgrunden hello hello webbläsare via ett 401 obehörig svar, tooprovide en Kerberos-biljett.</span><span class="sxs-lookup"><span data-stu-id="99cc5-127">Using JavaScript in hello background, Azure AD challenges hello browser, via a 401 Unauthorized response, tooprovide a Kerberos ticket.</span></span>
5. <span data-ttu-id="99cc5-128">hello webbläsare i sin tur begär en tjänstbiljett från Active Directory för hello `AZUREADSSOACCT` datorkontot (som representerar Azure AD).</span><span class="sxs-lookup"><span data-stu-id="99cc5-128">hello browser, in turn, requests a ticket from Active Directory for hello `AZUREADSSOACCT` computer account (which represents Azure AD).</span></span>
6. <span data-ttu-id="99cc5-129">Active Directory hittar hello datorkontot och returnerar en Kerberos-biljett toohello webbläsare krypteras med hello datorkontos hemlighet.</span><span class="sxs-lookup"><span data-stu-id="99cc5-129">Active Directory locates hello computer account and returns a Kerberos ticket toohello browser encrypted with hello computer account's secret.</span></span>
7. <span data-ttu-id="99cc5-130">hello webbläsare vidarebefordrar hello Kerberos-biljetten som hämtades från Active Directory tooAzure AD (på en av hello [Azure AD-URL: er som tidigare lagts till toohello webbläsarens intranät Zoninställningar](active-directory-aadconnect-sso-quick-start.md#step-3-roll-out-the-feature)).</span><span class="sxs-lookup"><span data-stu-id="99cc5-130">hello browser forwards hello Kerberos ticket it acquired from Active Directory tooAzure AD (on one of hello [Azure AD URLs previously added toohello browser's Intranet zone settings](active-directory-aadconnect-sso-quick-start.md#step-3-roll-out-the-feature)).</span></span>
8. <span data-ttu-id="99cc5-131">Azure AD dekrypterar hello Kerberos-biljett, som innehåller hello identitet hello användaren loggat in hello företagets enheter med hjälp av hello tidigare delad nyckel.</span><span class="sxs-lookup"><span data-stu-id="99cc5-131">Azure AD decrypts hello Kerberos ticket, which includes hello identity of hello user signed into hello corporate device, using hello previously shared key.</span></span>
9. <span data-ttu-id="99cc5-132">Efter utvärderingen, Azure AD returnerar ett token tillbaka toohello program eller begär hello användaren tooperform ytterligare bevis, till exempel Multi-Factor Authentication.</span><span class="sxs-lookup"><span data-stu-id="99cc5-132">After evaluation, Azure AD either returns a token back toohello application or asks hello user tooperform additional proofs, such as Multi-Factor Authentication.</span></span>
10. <span data-ttu-id="99cc5-133">Om hello användarens inloggning lyckas är hello användaren kan tooaccess hello program.</span><span class="sxs-lookup"><span data-stu-id="99cc5-133">If hello user sign-in is successful, hello user is able tooaccess hello application.</span></span>

<span data-ttu-id="99cc5-134">hello följande diagram illustrerar alla hello komponenter och hello stegen.</span><span class="sxs-lookup"><span data-stu-id="99cc5-134">hello following diagram illustrates all hello components and hello steps involved.</span></span>

![Sömlös för enkel inloggning](./media/active-directory-aadconnect-sso/sso2.png)

<span data-ttu-id="99cc5-136">Sömlös SSO är opportunistisk, vilket innebär att om det misslyckas hello inloggningen faller tillbaka tooits reguljära beteende - engångsfaktorautentisering, hello användare behöver tooenter sina lösenord toosign i.</span><span class="sxs-lookup"><span data-stu-id="99cc5-136">Seamless SSO is opportunistic, which means if it fails, hello sign-in experience falls back tooits regular behavior - i.e, hello user needs tooenter their password toosign in.</span></span>

## <a name="next-steps"></a><span data-ttu-id="99cc5-137">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="99cc5-137">Next steps</span></span>

- <span data-ttu-id="99cc5-138">[**Snabbstart** ](active-directory-aadconnect-sso-quick-start.md) – få och kör Azure AD sömlös SSO.</span><span class="sxs-lookup"><span data-stu-id="99cc5-138">[**Quick Start**](active-directory-aadconnect-sso-quick-start.md) - Get up and running Azure AD Seamless SSO.</span></span>
- <span data-ttu-id="99cc5-139">[**Vanliga frågor och svar** ](active-directory-aadconnect-sso-faq.md) -toofrequently frågor och svar.</span><span class="sxs-lookup"><span data-stu-id="99cc5-139">[**Frequently Asked Questions**](active-directory-aadconnect-sso-faq.md) - Answers toofrequently asked questions.</span></span>
- <span data-ttu-id="99cc5-140">[**Felsöka** ](active-directory-aadconnect-troubleshoot-sso.md) – Lär dig hur tooresolve vanliga problem med hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="99cc5-140">[**Troubleshoot**](active-directory-aadconnect-troubleshoot-sso.md) - Learn how tooresolve common issues with hello feature.</span></span>
- <span data-ttu-id="99cc5-141">[**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – för arkivering av nya funktioner som efterfrågas.</span><span class="sxs-lookup"><span data-stu-id="99cc5-141">[**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - For filing new feature requests.</span></span>
