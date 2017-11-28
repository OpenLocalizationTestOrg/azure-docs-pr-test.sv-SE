---
title: "Azure AD Connect: Direkt autentisering - Smart kontoutelåsning | Microsoft Docs"
description: "Den här artikeln beskrivs hur Azure Active Directory (AD Azure) direkt autentisering skyddar dina lokala konton från brute force-attacker lösenord i hello molnet."
services: active-directory
keywords: "Azure AD Connect direkt-autentisering, installera Active Directory, nödvändiga komponenter för Azure AD, SSO, Single Sign-on"
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
ms.openlocfilehash: b02e315c3cc3eae00ca6408d735a416f34c2cdc3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-pass-through-authentication-smart-lockout"></a><span data-ttu-id="42c25-104">: Azure Active Directory direkt Smart Autentiseringsutelåsning</span><span class="sxs-lookup"><span data-stu-id="42c25-104">Azure Active Directory Pass-through Authentication: Smart Lockout</span></span>

## <a name="overview"></a><span data-ttu-id="42c25-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="42c25-105">Overview</span></span>

<span data-ttu-id="42c25-106">Azure AD skyddar mot brute force-attacker lösenord och förhindrar att äkta användare som låsts ute från sina Office 365 och SaaS-program.</span><span class="sxs-lookup"><span data-stu-id="42c25-106">Azure AD protects against brute force password attacks and prevents genuine users from being locked out of their Office 365 and SaaS applications.</span></span> <span data-ttu-id="42c25-107">Den här funktionen kallas **Smart kontoutelåsning**, stöds när du använder direkt-autentisering som din metod för att logga in.</span><span class="sxs-lookup"><span data-stu-id="42c25-107">This capability, called **Smart Lockout**, is supported when you use Pass-through Authentication as your sign-in method.</span></span> <span data-ttu-id="42c25-108">Smart kontoutelåsning är aktiverad som standard för alla klienter och skyddar dina användarkonton alla hello tillfälle. Det finns inget behov av tooturn på.</span><span class="sxs-lookup"><span data-stu-id="42c25-108">Smart Lockout is enabled by default for all tenants and are protecting your user accounts all hello time; there is no need tooturn it on.</span></span>

<span data-ttu-id="42c25-109">Smart kontoutelåsning fungerar genom att misslyckade inloggningsförsök och efter ett visst **tröskelvärde för kontoutelåsning**början en **Utelåsningstiden**.</span><span class="sxs-lookup"><span data-stu-id="42c25-109">Smart Lockout works by keeping track of failed sign-in attempts, and after a certain **Lockout Threshold**, starting a **Lockout Duration**.</span></span> <span data-ttu-id="42c25-110">Alla försök från angripare hello under hello Utelåsningstiden avvisas.</span><span class="sxs-lookup"><span data-stu-id="42c25-110">Any sign-in attempts from hello attacker during hello Lockout Duration are rejected.</span></span> <span data-ttu-id="42c25-111">Om hello attack kvarstår inloggningsförsök hello efterföljande misslyckade när hello Utelåsningstiden slutar resultatet i längre varaktighet för kontoutelåsning.</span><span class="sxs-lookup"><span data-stu-id="42c25-111">If hello attack continues, hello subsequent failed sign-in attempts after hello Lockout Duration ends result in longer Lockout Durations.</span></span>

>[!NOTE]
><span data-ttu-id="42c25-112">hello standard tröskelvärde för kontoutelåsning är 10 misslyckade försök och hello standard Utelåsningstiden är 60 sekunder.</span><span class="sxs-lookup"><span data-stu-id="42c25-112">hello default Lockout Threshold is 10 failed attempts, and hello default Lockout Duration is 60 seconds.</span></span>

<span data-ttu-id="42c25-113">Smart kontoutelåsning skiljer också inloggningar från äkta användare och från angripare och endast utelåses hello angripare i de flesta fall.</span><span class="sxs-lookup"><span data-stu-id="42c25-113">Smart Lockout also distinguishes between sign-ins from genuine users and from attackers and only locks out hello attackers in most cases.</span></span> <span data-ttu-id="42c25-114">Den här funktionen förhindrar att angripare medvetet låsa ute äkta användare.</span><span class="sxs-lookup"><span data-stu-id="42c25-114">This functionality prevents attackers from maliciously locking out genuine users.</span></span> <span data-ttu-id="42c25-115">Vi använder tidigare inloggning beteende användarnas enheter & webbläsare och andra signaler toodistinguish mellan äkta användare och angripare.</span><span class="sxs-lookup"><span data-stu-id="42c25-115">We use past sign-in behavior, users’ devices & browsers and other signals toodistinguish between genuine users and attackers.</span></span> <span data-ttu-id="42c25-116">Vi ständigt förbättra våra algoritmer.</span><span class="sxs-lookup"><span data-stu-id="42c25-116">We are constantly improving our algorithms.</span></span>

<span data-ttu-id="42c25-117">Eftersom direkt autentisering vidarebefordrar lösenord validering begäranden till din lokala Active Directory (AD), måste tooprevent angripare från att låsa ute användarnas AD-konton.</span><span class="sxs-lookup"><span data-stu-id="42c25-117">Because Pass-through Authentication forwards password validation requests onto your on-premises Active Directory (AD), you need tooprevent attackers from locking out your users’ AD accounts.</span></span> <span data-ttu-id="42c25-118">Eftersom du har AD kontoutelåsning principer (särskilt [ **tröskelvärde för kontoutelåsning** ](https://technet.microsoft.com/library/hh994574(v=ws.11).aspx) och [ **Återställ konto kontoutelåsning räknaren efter principer** ](https://technet.microsoft.com/library/hh994568(v=ws.11).aspx)), behöver du tooconfigure Azure AD-tröskelvärde för kontoutelåsning och Utelåsningstiden värden korrekt toofilter ut attacker i hello moln innan de når dina lokala AD.</span><span class="sxs-lookup"><span data-stu-id="42c25-118">Since you have your own AD Account Lockout policies (specifically, [**Account Lockout Threshold**](https://technet.microsoft.com/library/hh994574(v=ws.11).aspx) and [**Reset Account Lockout Counter After policies**](https://technet.microsoft.com/library/hh994568(v=ws.11).aspx)), you need tooconfigure Azure AD’s Lockout Threshold and Lockout Duration values appropriately toofilter out attacks in hello cloud before they reach your on-premises AD.</span></span>

>[!NOTE]
><span data-ttu-id="42c25-119">hello Smart kontoutelåsning funktion är ledig och är _på_ som standard för alla kunder.</span><span class="sxs-lookup"><span data-stu-id="42c25-119">hello Smart Lockout feature is free and is _on_ by default for all customers.</span></span> <span data-ttu-id="42c25-120">Ändra Azure AD tröskelvärde för kontoutelåsning och kontoutelåsning Duration-värden med hjälp av Graph API måste emellertid din klientlicens toohave minst en Azure AD Premium P2.</span><span class="sxs-lookup"><span data-stu-id="42c25-120">However, modifying Azure AD’s Lockout Threshold and Lockout Duration values using Graph API needs your tenant toohave at least one Azure AD Premium P2 license.</span></span> <span data-ttu-id="42c25-121">Du behöver en Azure AD Premium P2-licens _per användare_ tooget hello Smart kontoutelåsning funktion med direkt-autentisering.</span><span class="sxs-lookup"><span data-stu-id="42c25-121">You don't need an Azure AD Premium P2 license _per user_ tooget hello Smart Lockout feature with Pass-through Authentication.</span></span>

<span data-ttu-id="42c25-122">tooensure att användarnas lokala AD-konton är väl skyddade måste du tooensure som:</span><span class="sxs-lookup"><span data-stu-id="42c25-122">tooensure that your users’ on-premises AD accounts are well protected, you need tooensure that:</span></span>

1.  <span data-ttu-id="42c25-123">Tröskelvärde för kontoutelåsning för Azure AD är _mindre_ än Annonsens tröskelvärde för kontoutelåsning.</span><span class="sxs-lookup"><span data-stu-id="42c25-123">Azure AD’s Lockout Threshold is _less_ than AD’s Account Lockout Threshold.</span></span> <span data-ttu-id="42c25-124">Vi rekommenderar att du ställer in hello värden så att Annonsens tröskelvärde för kontoutelåsning är minst två eller tre gånger tröskelvärde för kontoutelåsning för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="42c25-124">We recommend that you set hello values such that AD’s Account Lockout Threshold is at least two or three times Azure AD’s Lockout Threshold.</span></span>
2.  <span data-ttu-id="42c25-125">Azure AD-Utelåsningstiden (som visas i sekunder) är _längre_ än Annonsens Återställ konto kontoutelåsning räknaren efter (angiven i minuter).</span><span class="sxs-lookup"><span data-stu-id="42c25-125">Azure AD’s Lockout Duration (represented in seconds) is _longer_ than AD’s Reset Account Lockout Counter After (represented in minutes).</span></span>

## <a name="verify-your-ad-account-lockout-policies"></a><span data-ttu-id="42c25-126">Kontrollera dina principer för kontoutelåsning för AD</span><span class="sxs-lookup"><span data-stu-id="42c25-126">Verify your AD Account Lockout policies</span></span>

<span data-ttu-id="42c25-127">Använd följande instruktioner tooverify hello principer för kontoutelåsning för AD:</span><span class="sxs-lookup"><span data-stu-id="42c25-127">Use hello following instructions tooverify your AD Account Lockout policies:</span></span>

1.  <span data-ttu-id="42c25-128">Öppna hello verktyg för hantering av Grupprincip.</span><span class="sxs-lookup"><span data-stu-id="42c25-128">Open hello Group Policy Management tool.</span></span>
2.  <span data-ttu-id="42c25-129">Redigera hello Grupprincip som är kopplade tooall användare, till exempel hello domänens standardprincip.</span><span class="sxs-lookup"><span data-stu-id="42c25-129">Edit hello group policy that is applied tooall users, for example, hello Default Domain Policy.</span></span>
3.  <span data-ttu-id="42c25-130">Navigera tooComputer Datorkonfiguration\Principer\Windows datorkonfiguration\windowsinställningar\säkerhetsinställningar\avancerad kontoprinciper för kontoutelåsning.</span><span class="sxs-lookup"><span data-stu-id="42c25-130">Navigate tooComputer Configuration\Policies\Windows Settings\Security Settings\Account Policies\Account Lockout Policy.</span></span>
4.  <span data-ttu-id="42c25-131">Kontrollera dina tröskelvärde för kontoutelåsning och återställa kontot kontoutelåsning räknaren efter värden.</span><span class="sxs-lookup"><span data-stu-id="42c25-131">Verify your Account Lockout Threshold and Reset Account Lockout Counter After values.</span></span>

![Principer för kontoutelåsning för AD](./media/active-directory-aadconnect-pass-through-authentication/pta5.png)

## <a name="use-hello-graph-api-toomanage-your-tenants-smart-lockout-values"></a><span data-ttu-id="42c25-133">Använd hello Graph API toomanage din klient Smart kontoutelåsning värden</span><span class="sxs-lookup"><span data-stu-id="42c25-133">Use hello Graph API toomanage your tenant’s Smart Lockout values</span></span>

>[!IMPORTANT]
><span data-ttu-id="42c25-134">Ändra Azure AD tröskelvärde för kontoutelåsning och kontoutelåsning Duration-värden med hjälp av Graph API är en Azure AD Premium P2-funktion.</span><span class="sxs-lookup"><span data-stu-id="42c25-134">Modifying Azure AD’s Lockout Threshold and Lockout Duration values using Graph API is an Azure AD Premium P2 feature.</span></span> <span data-ttu-id="42c25-135">Det måste du också toobe en Global administratör för din klient.</span><span class="sxs-lookup"><span data-stu-id="42c25-135">It also needs you toobe a Global Administrator on your tenant.</span></span>

<span data-ttu-id="42c25-136">Du kan använda [diagram Explorer](https://developer.microsoft.com/graph/graph-explorer) tooread, ange och uppdatera Azure AD Smart kontoutelåsning värden.</span><span class="sxs-lookup"><span data-stu-id="42c25-136">You can use [Graph Explorer](https://developer.microsoft.com/graph/graph-explorer) tooread, set, and update Azure AD’s Smart Lockout values.</span></span> <span data-ttu-id="42c25-137">Men du kan också göra dessa åtgärder via programmering.</span><span class="sxs-lookup"><span data-stu-id="42c25-137">But you can also do these operations programmatically.</span></span>

### <a name="read-smart-lockout-values"></a><span data-ttu-id="42c25-138">Läs Smart kontoutelåsning värden</span><span class="sxs-lookup"><span data-stu-id="42c25-138">Read Smart Lockout values</span></span>

<span data-ttu-id="42c25-139">Följ dessa steg tooread din klient Smart kontoutelåsning värden:</span><span class="sxs-lookup"><span data-stu-id="42c25-139">Follow these steps tooread your tenant’s Smart Lockout values:</span></span>

1. <span data-ttu-id="42c25-140">Logga in på diagrammet Explorer som Global administratör för din klient.</span><span class="sxs-lookup"><span data-stu-id="42c25-140">Sign into Graph Explorer as a Global Administrator of your tenant.</span></span> <span data-ttu-id="42c25-141">Om du uppmanas begärt bevilja åtkomst för hello behörigheter.</span><span class="sxs-lookup"><span data-stu-id="42c25-141">If prompted, grant access for hello requested permissions.</span></span>
2. <span data-ttu-id="42c25-142">Klicka på ”Ändra behörigheter” och väljer hello ”Directory.ReadWrite.All” behörighet.</span><span class="sxs-lookup"><span data-stu-id="42c25-142">Click “Modify permissions” and select hello “Directory.ReadWrite.All” permission.</span></span>
3. <span data-ttu-id="42c25-143">Konfigurera hello Graph API-begäran på följande sätt: ange version för ”BETAVERSIONEN” typ av begäran för ”GET” och URL: en för`https://graph.microsoft.com/beta/<your-tenant-domain>/settings`.</span><span class="sxs-lookup"><span data-stu-id="42c25-143">Configure hello Graph API request as follows: Set version too“BETA”, request type too“GET” and URL too`https://graph.microsoft.com/beta/<your-tenant-domain>/settings`.</span></span>
4. <span data-ttu-id="42c25-144">Klicka på ”Kör fråga” toosee din klient Smart kontoutelåsning värden.</span><span class="sxs-lookup"><span data-stu-id="42c25-144">Click "Run Query" toosee your tenant's Smart Lockout values.</span></span> <span data-ttu-id="42c25-145">Om du inte har angett din klient värden innan ser du en tom uppsättning.</span><span class="sxs-lookup"><span data-stu-id="42c25-145">If you haven't set your tenant's values before, you see an empty set.</span></span>

### <a name="set-smart-lockout-values"></a><span data-ttu-id="42c25-146">Ange värden för Smart kontoutelåsning</span><span class="sxs-lookup"><span data-stu-id="42c25-146">Set Smart Lockout values</span></span>

<span data-ttu-id="42c25-147">Följ dessa steg tooset din klient Smart kontoutelåsning värden (för hello första gången):</span><span class="sxs-lookup"><span data-stu-id="42c25-147">Follow these steps tooset your tenant’s Smart Lockout values (for hello first time only):</span></span>

1. <span data-ttu-id="42c25-148">Logga in på diagrammet Explorer som Global administratör för din klient.</span><span class="sxs-lookup"><span data-stu-id="42c25-148">Sign into Graph Explorer as a Global Administrator of your tenant.</span></span> <span data-ttu-id="42c25-149">Om du uppmanas begärt bevilja åtkomst för hello behörigheter.</span><span class="sxs-lookup"><span data-stu-id="42c25-149">If prompted, grant access for hello requested permissions.</span></span>
2. <span data-ttu-id="42c25-150">Klicka på ”Ändra behörigheter” och väljer hello ”Directory.ReadWrite.All” behörighet.</span><span class="sxs-lookup"><span data-stu-id="42c25-150">Click “Modify permissions” and select hello “Directory.ReadWrite.All” permission.</span></span>
3. <span data-ttu-id="42c25-151">Konfigurera hello Graph API-begäran på följande sätt: ange version för ”BETAVERSIONEN” typ av begäran för ”publicera” och URL: en för`https://graph.microsoft.com/beta/<your-tenant-domain>/settings`.</span><span class="sxs-lookup"><span data-stu-id="42c25-151">Configure hello Graph API request as follows: Set version too“BETA”, request type too“POST” and URL too`https://graph.microsoft.com/beta/<your-tenant-domain>/settings`.</span></span>
4. <span data-ttu-id="42c25-152">Kopiera och klistra in hello följande JSON-förfrågan till hello ”Begärandetexten” fält.</span><span class="sxs-lookup"><span data-stu-id="42c25-152">Copy and paste hello following JSON request into hello "Request Body" field.</span></span> <span data-ttu-id="42c25-153">Ändra hello Smart kontoutelåsning värden efter behov och använda en slumpmässig GUID för `templateId`.</span><span class="sxs-lookup"><span data-stu-id="42c25-153">Change hello Smart Lockout values as appropriate and use a random GUID for `templateId`.</span></span>
5. <span data-ttu-id="42c25-154">Klicka på ”Kör fråga” tooset din klient Smart kontoutelåsning värden.</span><span class="sxs-lookup"><span data-stu-id="42c25-154">Click "Run Query" tooset your tenant's Smart Lockout values.</span></span>

```
{
  "templateId": "5cf42378-d67d-4f36-ba46-e8b86229381d",
  "values": [
    {
      "name": "LockoutDurationInSeconds",
      "value": "300"
    },
    {
      "name": "LockoutThreshold",
      "value": "5"
    },
    {
      "name" : "BannedPasswordList",
      "value": ""
    },
    {
      "name" : "EnableBannedPasswordCheck",
      "value": "false"
    }
  ]
}
```

>[!NOTE]
><span data-ttu-id="42c25-155">Om du inte använder dem, kan du lämna hello **BannedPasswordList** och **EnableBannedPasswordCheck** värden som tom (””) och ”false” respektive.</span><span class="sxs-lookup"><span data-stu-id="42c25-155">If you are not using them, you can leave hello **BannedPasswordList** and **EnableBannedPasswordCheck** values as empty ("") and "false" respectively.</span></span>

<span data-ttu-id="42c25-156">Kontrollera att du har angett din klient Smart kontoutelåsning värden korrekt med [här](#read-smart-lockout-values).</span><span class="sxs-lookup"><span data-stu-id="42c25-156">Verify that you have set your tenant's Smart Lockout values correctly using [these steps](#read-smart-lockout-values).</span></span>

### <a name="update-smart-lockout-values"></a><span data-ttu-id="42c25-157">Uppdatera Smart kontoutelåsning värden</span><span class="sxs-lookup"><span data-stu-id="42c25-157">Update Smart Lockout values</span></span>

<span data-ttu-id="42c25-158">Följ dessa steg tooupdate din klient Smart kontoutelåsning värden (om du redan har angett dem innan):</span><span class="sxs-lookup"><span data-stu-id="42c25-158">Follow these steps tooupdate your tenant’s Smart Lockout values (if you have already set them before):</span></span>

1. <span data-ttu-id="42c25-159">Logga in på diagrammet Explorer som Global administratör för din klient.</span><span class="sxs-lookup"><span data-stu-id="42c25-159">Sign into Graph Explorer as a Global Administrator of your tenant.</span></span> <span data-ttu-id="42c25-160">Om du uppmanas begärt bevilja åtkomst för hello behörigheter.</span><span class="sxs-lookup"><span data-stu-id="42c25-160">If prompted, grant access for hello requested permissions.</span></span>
2. <span data-ttu-id="42c25-161">Klicka på ”Ändra behörigheter” och väljer hello ”Directory.ReadWrite.All” behörighet.</span><span class="sxs-lookup"><span data-stu-id="42c25-161">Click “Modify permissions” and select hello “Directory.ReadWrite.All” permission.</span></span>
3. <span data-ttu-id="42c25-162">[Följ dessa steg tooread värden för din klient Smart kontoutelåsning](#read-smart-lockout-values).</span><span class="sxs-lookup"><span data-stu-id="42c25-162">[Follow these steps tooread your tenant's Smart Lockout values](#read-smart-lockout-values).</span></span> <span data-ttu-id="42c25-163">Kopiera hello `id` värde (en GUID) för hello-objekt med ”visningsnamn” som ”PasswordRuleSettings”.</span><span class="sxs-lookup"><span data-stu-id="42c25-163">Copy hello `id` value (a GUID) of hello item with "displayName" as "PasswordRuleSettings".</span></span>
4. <span data-ttu-id="42c25-164">Konfigurera hello Graph API-begäran på följande sätt: ange version för ”BETAVERSIONEN” typ av begäran för ”uppdatera” och URL: en för`https://graph.microsoft.com/beta/<your-tenant-domain>/settings/<id>` -Använd hello GUID från steg3 för `<id>`.</span><span class="sxs-lookup"><span data-stu-id="42c25-164">Configure hello Graph API request as follows: Set version too“BETA”, request type too“PATCH” and URL too`https://graph.microsoft.com/beta/<your-tenant-domain>/settings/<id>` - use hello GUID from Step 3 for `<id>`.</span></span>
5. <span data-ttu-id="42c25-165">Kopiera och klistra in hello följande JSON-förfrågan till hello ”Begärandetexten” fält.</span><span class="sxs-lookup"><span data-stu-id="42c25-165">Copy and paste hello following JSON request into hello "Request Body" field.</span></span> <span data-ttu-id="42c25-166">Ändra hello Smart kontoutelåsning värden efter behov.</span><span class="sxs-lookup"><span data-stu-id="42c25-166">Change hello Smart Lockout values as appropriate.</span></span>
6. <span data-ttu-id="42c25-167">Klicka på ”Kör fråga” tooupdate din klient Smart kontoutelåsning värden.</span><span class="sxs-lookup"><span data-stu-id="42c25-167">Click "Run Query" tooupdate your tenant's Smart Lockout values.</span></span>

```
{
  "values": [
    {
      "name": "LockoutDurationInSeconds",
      "value": "30"
    },
    {
      "name": "LockoutThreshold",
      "value": "4"
    },
    {
      "name" : "BannedPasswordList",
      "value": ""
    },
    {
      "name" : "EnableBannedPasswordCheck",
      "value": "false"
    }
  ]
}
```

<span data-ttu-id="42c25-168">Kontrollera att du har uppdaterat din klient Smart kontoutelåsning värden korrekt med [här](#read-smart-lockout-values).</span><span class="sxs-lookup"><span data-stu-id="42c25-168">Verify that you have updated your tenant's Smart Lockout values correctly using [these steps](#read-smart-lockout-values).</span></span>

## <a name="next-steps"></a><span data-ttu-id="42c25-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="42c25-169">Next steps</span></span>
- <span data-ttu-id="42c25-170">[**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – för arkivering av nya funktioner som efterfrågas.</span><span class="sxs-lookup"><span data-stu-id="42c25-170">[**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - For filing new feature requests.</span></span>
