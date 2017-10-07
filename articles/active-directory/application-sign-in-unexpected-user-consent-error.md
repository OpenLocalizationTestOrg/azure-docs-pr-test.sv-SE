---
title: "aaa oväntat fel när du utför medgivande tooan program | Microsoft Docs"
description: "Beskriver fel som kan uppstå under hello principer tooan program och vad du kan göra om dem."
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
ms.openlocfilehash: dfee35f3a10e3cc4313109cedd972499452320f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="unexpected-error-when-performing-consent-tooan-application"></a><span data-ttu-id="79a5c-103">Oväntat fel när du utför medgivande tooan program</span><span class="sxs-lookup"><span data-stu-id="79a5c-103">Unexpected error when performing consent tooan application</span></span>

<span data-ttu-id="79a5c-104">Den här artikeln beskrivs fel som kan uppstå under hello av principer tooan program.</span><span class="sxs-lookup"><span data-stu-id="79a5c-104">This article discusses errors that can occur during hello process of consenting tooan application.</span></span> <span data-ttu-id="79a5c-105">Om du felsöka oväntat medgivande prompter som inte innehåller några felmeddelanden, se [Autentiseringsscenarier för Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios).</span><span class="sxs-lookup"><span data-stu-id="79a5c-105">If you are Troubleshoot unexpected consent prompts that do not contain any error messages, see [Authentication Scenarios for Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios).</span></span>

<span data-ttu-id="79a5c-106">Många program som integreras med Azure Active Directory kräver behörighet tooaccess andra resurser i ordning toofunction.</span><span class="sxs-lookup"><span data-stu-id="79a5c-106">Many applications that integrate with Azure Active Directory require permissions tooaccess other resources in order toofunction.</span></span> <span data-ttu-id="79a5c-107">När resurserna är även integrerad med Azure Active Directory, medgivande behörigheter tooaccess dem begärs ofta med hjälp av gemensamma hello framework.</span><span class="sxs-lookup"><span data-stu-id="79a5c-107">When these resources are also integrated with Azure Active Directory, permissions tooaccess them is often requested using hello common consent framework.</span></span> 

<span data-ttu-id="79a5c-108">Detta resulterar i en fråga om medgivande visas, vilket sker vanligtvis hello första gången ett program som används men kan också inträffa om en efterföljande användning av programmet hello.</span><span class="sxs-lookup"><span data-stu-id="79a5c-108">This results in a consent prompt being displayed, which generally occurs hello first time an application is used but can also occur on a subsequent use of hello application.</span></span>

<span data-ttu-id="79a5c-109">Vissa villkor måste vara true för en användare tooconsent toohello behörighet som krävs för ett program.</span><span class="sxs-lookup"><span data-stu-id="79a5c-109">Certain conditions must be true for a user tooconsent toohello permissions an application requires.</span></span> <span data-ttu-id="79a5c-110">Om dessa villkor inte uppfylls, kan det uppstå fel.</span><span class="sxs-lookup"><span data-stu-id="79a5c-110">If these conditions are not met, various errors can occur.</span></span> <span data-ttu-id="79a5c-111">Exempel på dessa är:</span><span class="sxs-lookup"><span data-stu-id="79a5c-111">These include:</span></span>

## <a name="requesting-not-authorized-permissions-error"></a><span data-ttu-id="79a5c-112">Begär inte auktoriserad Behörighetsfel</span><span class="sxs-lookup"><span data-stu-id="79a5c-112">Requesting not authorized permissions error</span></span>
* <span data-ttu-id="79a5c-113">**AADSTS90093:** &lt;clientAppDisplayName&gt; begär en eller flera behörigheter som du inte är auktoriserad toogrant.</span><span class="sxs-lookup"><span data-stu-id="79a5c-113">**AADSTS90093:** &lt;clientAppDisplayName&gt; is requesting one or more permissions that you are not authorized toogrant.</span></span> <span data-ttu-id="79a5c-114">Kontakta en administratör som kan godkänna toothis program för din räkning.</span><span class="sxs-lookup"><span data-stu-id="79a5c-114">Contact an administrator, who can consent toothis application on your behalf.</span></span>

<span data-ttu-id="79a5c-115">Det här felet uppstår när en användare som inte är en företagsadministratör toouse ett program som begär behörigheter som bara en administratör kan ge.</span><span class="sxs-lookup"><span data-stu-id="79a5c-115">This error occurs when a user who is not a company administrator attempts toouse an application that is requesting permissions that only an administrator can grant.</span></span> <span data-ttu-id="79a5c-116">Det här felet kan lösas av en administratör bevilja åtkomst toohello program för organisationen.</span><span class="sxs-lookup"><span data-stu-id="79a5c-116">This error can be resolved by an administrator granting access toohello application on behalf of their organization.</span></span>

## <a name="policy-prevents-granting-permissions-error"></a><span data-ttu-id="79a5c-117">Princip förhindrar att bevilja behörigheter fel</span><span class="sxs-lookup"><span data-stu-id="79a5c-117">Policy prevents granting permissions error</span></span>
* <span data-ttu-id="79a5c-118">**AADSTS90093:** administratör för &lt;tenantDisplayName&gt; har angett en princip som hindrar dig från att bevilja &lt;namn på appen&gt; hello behörigheter som begärs.</span><span class="sxs-lookup"><span data-stu-id="79a5c-118">**AADSTS90093:** An administrator of &lt;tenantDisplayName&gt; has set a policy that prevents you from granting &lt;name of app&gt; hello permissions it is requesting.</span></span> <span data-ttu-id="79a5c-119">Kontakta en administratör av &lt;tenantDisplayName&gt;, som kan bevilja behörigheter toothis appen å dina vägnar.</span><span class="sxs-lookup"><span data-stu-id="79a5c-119">Contact an administrator of &lt;tenantDisplayName&gt;, who can grant permissions toothis app on your behalf.</span></span>

<span data-ttu-id="79a5c-120">Felet uppstår när en företagsadministratör inaktiverar hello möjligheten för användare tooconsent tooapplications och sedan försöker toouse ett program som kräver godkännande för en icke-administratörer.</span><span class="sxs-lookup"><span data-stu-id="79a5c-120">This error occurs when a company administrator turns off hello ability for users tooconsent tooapplications, then a non-administrator user attempts toouse an application that requires consent.</span></span> <span data-ttu-id="79a5c-121">Det här felet kan lösas av en administratör bevilja åtkomst toohello program för organisationen.</span><span class="sxs-lookup"><span data-stu-id="79a5c-121">This error can be resolved by an administrator granting access toohello application on behalf of their organization.</span></span>

## <a name="intermittent-problem-error"></a><span data-ttu-id="79a5c-122">Återkommande problem fel</span><span class="sxs-lookup"><span data-stu-id="79a5c-122">Intermittent problem error</span></span>
* <span data-ttu-id="79a5c-123">**AADSTS90090:** det verkar som om vi påträffade ett tillfälligt problem inspelning hello behörigheter som du försökte toogrant för&lt;clientAppDisplayName&gt;.</span><span class="sxs-lookup"><span data-stu-id="79a5c-123">**AADSTS90090:** It looks like we encountered an intermittent problem recording hello permissions you attempted toogrant too&lt;clientAppDisplayName&gt;.</span></span> <span data-ttu-id="79a5c-124">Försök igen senare.</span><span class="sxs-lookup"><span data-stu-id="79a5c-124">try again later.</span></span>

<span data-ttu-id="79a5c-125">Det här felet indikerar att en återkommande på klientsidan problemet har uppstått.</span><span class="sxs-lookup"><span data-stu-id="79a5c-125">This error indicates that an intermittent service side issue has occurred.</span></span> <span data-ttu-id="79a5c-126">Det kan lösas genom att försöka tooconsent toohello programmet igen.</span><span class="sxs-lookup"><span data-stu-id="79a5c-126">It can be resolved by attempting tooconsent toohello application again.</span></span>

## <a name="resource-not-available-error"></a><span data-ttu-id="79a5c-127">Resursen inte tillgängliga fel</span><span class="sxs-lookup"><span data-stu-id="79a5c-127">Resource not available error</span></span>
* <span data-ttu-id="79a5c-128">**AADSTS65005:** hello app &lt;clientAppDisplayName&gt; begärde behörigheter tooaccess en resurs &lt;resourceAppDisplayName&gt; som inte är tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="79a5c-128">**AADSTS65005:** hello app &lt;clientAppDisplayName&gt; requested permissions tooaccess a resource &lt;resourceAppDisplayName&gt; that is not available.</span></span> 

<span data-ttu-id="79a5c-129">Kontakta hello programutvecklaren.</span><span class="sxs-lookup"><span data-stu-id="79a5c-129">Contact hello application developer.</span></span>

##  <a name="resource-not-available-in-tenant-error"></a><span data-ttu-id="79a5c-130">Resursen är inte tillgängligt i klient-fel</span><span class="sxs-lookup"><span data-stu-id="79a5c-130">Resource not available in tenant error</span></span>
* <span data-ttu-id="79a5c-131">**AADSTS65005:** &lt;clientAppDisplayName&gt; begär åtkomst tooa resurs &lt;resourceAppDisplayName&gt; som inte är tillgängligt i din organisation &lt; tenantDisplayName&gt;.</span><span class="sxs-lookup"><span data-stu-id="79a5c-131">**AADSTS65005:** &lt;clientAppDisplayName&gt; is requesting access tooa resource &lt;resourceAppDisplayName&gt; that is not available in your organization &lt;tenantDisplayName&gt;.</span></span> 

<span data-ttu-id="79a5c-132">Kontrollera att resursen är tillgänglig eller kontakta en administratör av &lt;tenantDisplayName&gt;.</span><span class="sxs-lookup"><span data-stu-id="79a5c-132">Ensure that this resource is available or contact an administrator of &lt;tenantDisplayName&gt;.</span></span>

## <a name="permissions-mismatch-error"></a><span data-ttu-id="79a5c-133">Matchningsfel för behörigheter</span><span class="sxs-lookup"><span data-stu-id="79a5c-133">Permissions mismatch error</span></span>
* <span data-ttu-id="79a5c-134">**AADSTS65005:** hello app begärd medgivande tooaccess resurs &lt;resourceAppDisplayName&gt;.</span><span class="sxs-lookup"><span data-stu-id="79a5c-134">**AADSTS65005:** hello app requested consent tooaccess resource &lt;resourceAppDisplayName&gt;.</span></span> <span data-ttu-id="79a5c-135">Denna begäran misslyckades eftersom den inte matchar hur hello appen var förkonfigurerade under app registreringen.</span><span class="sxs-lookup"><span data-stu-id="79a5c-135">This request failed because it does not match how hello app was pre-configured during app registration.</span></span> <span data-ttu-id="79a5c-136">Kontakta hello app vendor.* *</span><span class="sxs-lookup"><span data-stu-id="79a5c-136">Contact hello app vendor.**</span></span>

<span data-ttu-id="79a5c-137">Dessa alla fel uppstår när programmet hello en användare försöker tooconsent toois begär behörighet tooaccess ett resursprogram som inte kan hittas i hello organisations katalog (klient).</span><span class="sxs-lookup"><span data-stu-id="79a5c-137">These errors all occur when hello application a user is trying tooconsent toois requesting permissions tooaccess a resource application that cannot be found in hello organization’s directory (tenant).</span></span> <span data-ttu-id="79a5c-138">Detta kan inträffa av olika orsaker:</span><span class="sxs-lookup"><span data-stu-id="79a5c-138">This can occur for several reasons:</span></span>

-   <span data-ttu-id="79a5c-139">hello klienten programutvecklaren har sina program felaktigt konfigurerad, vilket toorequest åtkomst tooan ogiltig resurs.</span><span class="sxs-lookup"><span data-stu-id="79a5c-139">hello client application developer has configured their application incorrectly, causing it toorequest access tooan invalid resource.</span></span> <span data-ttu-id="79a5c-140">I det här fallet måste hello programutvecklaren uppdatera hello konfigurationen av hello klienten programmet tooresolve problemet.</span><span class="sxs-lookup"><span data-stu-id="79a5c-140">In this case, hello application developer must update hello configuration of hello client application tooresolve this issue.</span></span>

-   <span data-ttu-id="79a5c-141">Ett huvudnamn för tjänsten som representerar hello målprogrammet resursen finns inte i hello organisation, eller har funnits i tidigare hello men har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="79a5c-141">A Service Principal representing hello target resource application does not exist in hello organization, or existed in hello past but has been removed.</span></span> <span data-ttu-id="79a5c-142">i detta fall är ett huvudnamn för tjänsten för hello resursprogram måste etableras i hello organisation så hello klientprogrammet kan begära behörigheter tooit tooresolve.</span><span class="sxs-lookup"><span data-stu-id="79a5c-142">tooresolve this issue, a Service Principal for hello resource application must be provisioned in hello organization so hello client application can request permissions tooit.</span></span> <span data-ttu-id="79a5c-143">Detta kan hända i ett antal olika sätt beroende på hello typ av program, inklusive:</span><span class="sxs-lookup"><span data-stu-id="79a5c-143">This can happen in an number of ways, depending on hello type of application, including:</span></span>

    -   <span data-ttu-id="79a5c-144">Skaffa en prenumeration för hello resursprogram (Microsoft publicerade program)</span><span class="sxs-lookup"><span data-stu-id="79a5c-144">Acquiring a subscription for hello resource application (Microsoft published applications)</span></span>

    -   <span data-ttu-id="79a5c-145">Principer toohello resursprogram</span><span class="sxs-lookup"><span data-stu-id="79a5c-145">Consenting toohello resource application</span></span>

    -   <span data-ttu-id="79a5c-146">Bevilja behörigheter för hello program via hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="79a5c-146">Granting hello application permissions via hello Azure Portal</span></span>

    -   <span data-ttu-id="79a5c-147">Lägga till hello program från hello Azure AD Application Gallery</span><span class="sxs-lookup"><span data-stu-id="79a5c-147">Adding hello application from hello Azure AD Application Gallery</span></span>

## <a name="next-steps"></a><span data-ttu-id="79a5c-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="79a5c-148">Next steps</span></span> 

[<span data-ttu-id="79a5c-149">Appar, behörigheter och medgivande i Azure Active Directory (v1 slutpunkten)</span><span class="sxs-lookup"><span data-stu-id="79a5c-149">Apps, permissions, and consent in Azure Active Directory (v1 endpoint)</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent)<br>

[<span data-ttu-id="79a5c-150">Scope, behörigheter och medgivande i hello Azure Active Directory (v2.0-slutpunkten)</span><span class="sxs-lookup"><span data-stu-id="79a5c-150">Scopes, permissions, and consent in hello Azure Active Directory (v2.0 endpoint)</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)


