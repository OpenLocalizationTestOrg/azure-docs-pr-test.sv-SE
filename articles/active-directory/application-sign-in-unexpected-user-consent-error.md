---
title: "Oväntat fel när du utför medgivande till ett program | Microsoft Docs"
description: "Beskriver fel som kan uppstå under processen att samtycka till ett program och vad du kan göra om dem."
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
ms.openlocfilehash: fd500fdd4c8642bad96dcf71eebcf1fad461a35f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="unexpected-error-when-performing-consent-to-an-application"></a><span data-ttu-id="c3a54-103">Oväntat fel när du utför medgivande till ett program</span><span class="sxs-lookup"><span data-stu-id="c3a54-103">Unexpected error when performing consent to an application</span></span>

<span data-ttu-id="c3a54-104">Den här artikeln beskrivs fel som kan uppstå under processen att samtycka till ett program.</span><span class="sxs-lookup"><span data-stu-id="c3a54-104">This article discusses errors that can occur during the process of consenting to an application.</span></span> <span data-ttu-id="c3a54-105">Om du felsöka oväntat medgivande prompter som inte innehåller några felmeddelanden, se [Autentiseringsscenarier för Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios).</span><span class="sxs-lookup"><span data-stu-id="c3a54-105">If you are Troubleshoot unexpected consent prompts that do not contain any error messages, see [Authentication Scenarios for Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios).</span></span>

<span data-ttu-id="c3a54-106">Många program som integreras med Azure Active Directory kräver behörighet att komma åt andra resurser för att fungera.</span><span class="sxs-lookup"><span data-stu-id="c3a54-106">Many applications that integrate with Azure Active Directory require permissions to access other resources in order to function.</span></span> <span data-ttu-id="c3a54-107">När dessa resurser också är integrerade med Azure Active Directory, behörighet att komma åt dem begärs ofta använder vanliga medgivande framework.</span><span class="sxs-lookup"><span data-stu-id="c3a54-107">When these resources are also integrated with Azure Active Directory, permissions to access them is often requested using the common consent framework.</span></span> 

<span data-ttu-id="c3a54-108">Detta resulterar i en fråga om medgivande visas, vilket sker vanligtvis första gången ett program som används men kan också inträffa om en efterföljande användning av programmet.</span><span class="sxs-lookup"><span data-stu-id="c3a54-108">This results in a consent prompt being displayed, which generally occurs the first time an application is used but can also occur on a subsequent use of the application.</span></span>

<span data-ttu-id="c3a54-109">Vissa villkor måste vara true för en användare ska godkänna de behörigheter som krävs för ett program.</span><span class="sxs-lookup"><span data-stu-id="c3a54-109">Certain conditions must be true for a user to consent to the permissions an application requires.</span></span> <span data-ttu-id="c3a54-110">Om dessa villkor inte uppfylls, kan det uppstå fel.</span><span class="sxs-lookup"><span data-stu-id="c3a54-110">If these conditions are not met, various errors can occur.</span></span> <span data-ttu-id="c3a54-111">Exempel på dessa är:</span><span class="sxs-lookup"><span data-stu-id="c3a54-111">These include:</span></span>

## <a name="requesting-not-authorized-permissions-error"></a><span data-ttu-id="c3a54-112">Begär inte auktoriserad Behörighetsfel</span><span class="sxs-lookup"><span data-stu-id="c3a54-112">Requesting not authorized permissions error</span></span>
* <span data-ttu-id="c3a54-113">**AADSTS90093:** &lt;clientAppDisplayName&gt; begär en eller flera behörigheter som du har inte behörighet för att bevilja.</span><span class="sxs-lookup"><span data-stu-id="c3a54-113">**AADSTS90093:** &lt;clientAppDisplayName&gt; is requesting one or more permissions that you are not authorized to grant.</span></span> <span data-ttu-id="c3a54-114">Kontakta en administratör som kan godkänna det här programmet för din räkning.</span><span class="sxs-lookup"><span data-stu-id="c3a54-114">Contact an administrator, who can consent to this application on your behalf.</span></span>

<span data-ttu-id="c3a54-115">Det här felet uppstår när en användare som inte är en företagsadministratör försöker använda ett program som begär behörigheter som bara en administratör kan ge.</span><span class="sxs-lookup"><span data-stu-id="c3a54-115">This error occurs when a user who is not a company administrator attempts to use an application that is requesting permissions that only an administrator can grant.</span></span> <span data-ttu-id="c3a54-116">Det här felet kan lösas av en administratör beviljas åtkomst till programmet för organisationen.</span><span class="sxs-lookup"><span data-stu-id="c3a54-116">This error can be resolved by an administrator granting access to the application on behalf of their organization.</span></span>

## <a name="policy-prevents-granting-permissions-error"></a><span data-ttu-id="c3a54-117">Princip förhindrar att bevilja behörigheter fel</span><span class="sxs-lookup"><span data-stu-id="c3a54-117">Policy prevents granting permissions error</span></span>
* <span data-ttu-id="c3a54-118">**AADSTS90093:** administratör för &lt;tenantDisplayName&gt; har angett en princip som hindrar dig från att bevilja &lt;namn på appen&gt; de behörigheter som begärs.</span><span class="sxs-lookup"><span data-stu-id="c3a54-118">**AADSTS90093:** An administrator of &lt;tenantDisplayName&gt; has set a policy that prevents you from granting &lt;name of app&gt; the permissions it is requesting.</span></span> <span data-ttu-id="c3a54-119">Kontakta en administratör av &lt;tenantDisplayName&gt;, som kan ge behörigheter till den här appen å dina vägnar.</span><span class="sxs-lookup"><span data-stu-id="c3a54-119">Contact an administrator of &lt;tenantDisplayName&gt;, who can grant permissions to this app on your behalf.</span></span>

<span data-ttu-id="c3a54-120">Felet uppstår när en företagsadministratör inaktiveras möjligheten för användare att samtycka till program och sedan en användare försöker använda ett program som kräver godkännande.</span><span class="sxs-lookup"><span data-stu-id="c3a54-120">This error occurs when a company administrator turns off the ability for users to consent to applications, then a non-administrator user attempts to use an application that requires consent.</span></span> <span data-ttu-id="c3a54-121">Det här felet kan lösas av en administratör beviljas åtkomst till programmet för organisationen.</span><span class="sxs-lookup"><span data-stu-id="c3a54-121">This error can be resolved by an administrator granting access to the application on behalf of their organization.</span></span>

## <a name="intermittent-problem-error"></a><span data-ttu-id="c3a54-122">Återkommande problem fel</span><span class="sxs-lookup"><span data-stu-id="c3a54-122">Intermittent problem error</span></span>
* <span data-ttu-id="c3a54-123">**AADSTS90090:** det verkar som om vi påträffade ett tillfälligt problem spela in de behörigheter som du försökte att ge till &lt;clientAppDisplayName&gt;.</span><span class="sxs-lookup"><span data-stu-id="c3a54-123">**AADSTS90090:** It looks like we encountered an intermittent problem recording the permissions you attempted to grant to &lt;clientAppDisplayName&gt;.</span></span> <span data-ttu-id="c3a54-124">Försök igen senare.</span><span class="sxs-lookup"><span data-stu-id="c3a54-124">try again later.</span></span>

<span data-ttu-id="c3a54-125">Det här felet indikerar att en återkommande på klientsidan problemet har uppstått.</span><span class="sxs-lookup"><span data-stu-id="c3a54-125">This error indicates that an intermittent service side issue has occurred.</span></span> <span data-ttu-id="c3a54-126">Det kan lösas genom att försöka samtycker till att programmet igen.</span><span class="sxs-lookup"><span data-stu-id="c3a54-126">It can be resolved by attempting to consent to the application again.</span></span>

## <a name="resource-not-available-error"></a><span data-ttu-id="c3a54-127">Resursen inte tillgängliga fel</span><span class="sxs-lookup"><span data-stu-id="c3a54-127">Resource not available error</span></span>
* <span data-ttu-id="c3a54-128">**AADSTS65005:** appen &lt;clientAppDisplayName&gt; begärt behörighet att komma åt en resurs &lt;resourceAppDisplayName&gt; som inte är tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="c3a54-128">**AADSTS65005:** The app &lt;clientAppDisplayName&gt; requested permissions to access a resource &lt;resourceAppDisplayName&gt; that is not available.</span></span> 

<span data-ttu-id="c3a54-129">Kontakta programutvecklaren.</span><span class="sxs-lookup"><span data-stu-id="c3a54-129">Contact the application developer.</span></span>

##  <a name="resource-not-available-in-tenant-error"></a><span data-ttu-id="c3a54-130">Resursen är inte tillgängligt i klient-fel</span><span class="sxs-lookup"><span data-stu-id="c3a54-130">Resource not available in tenant error</span></span>
* <span data-ttu-id="c3a54-131">**AADSTS65005:** &lt;clientAppDisplayName&gt; begär åtkomst till en resurs &lt;resourceAppDisplayName&gt; som inte är tillgängligt i din organisation &lt;tenantDisplayName&gt;.</span><span class="sxs-lookup"><span data-stu-id="c3a54-131">**AADSTS65005:** &lt;clientAppDisplayName&gt; is requesting access to a resource &lt;resourceAppDisplayName&gt; that is not available in your organization &lt;tenantDisplayName&gt;.</span></span> 

<span data-ttu-id="c3a54-132">Kontrollera att resursen är tillgänglig eller kontakta en administratör av &lt;tenantDisplayName&gt;.</span><span class="sxs-lookup"><span data-stu-id="c3a54-132">Ensure that this resource is available or contact an administrator of &lt;tenantDisplayName&gt;.</span></span>

## <a name="permissions-mismatch-error"></a><span data-ttu-id="c3a54-133">Matchningsfel för behörigheter</span><span class="sxs-lookup"><span data-stu-id="c3a54-133">Permissions mismatch error</span></span>
* <span data-ttu-id="c3a54-134">**AADSTS65005:** appen begärt medgivande till åtkomst till resursen &lt;resourceAppDisplayName&gt;.</span><span class="sxs-lookup"><span data-stu-id="c3a54-134">**AADSTS65005:** The app requested consent to access resource &lt;resourceAppDisplayName&gt;.</span></span> <span data-ttu-id="c3a54-135">Denna begäran misslyckades eftersom den inte matchar hur appen har redan konfigurerats under app registreringen.</span><span class="sxs-lookup"><span data-stu-id="c3a54-135">This request failed because it does not match how the app was pre-configured during app registration.</span></span> <span data-ttu-id="c3a54-136">Kontakta app vendor.* *</span><span class="sxs-lookup"><span data-stu-id="c3a54-136">Contact the app vendor.**</span></span>

<span data-ttu-id="c3a54-137">Dessa fel alla inträffar när en användare försöker samtycker till att programmet begär behörighet att komma åt en resursprogram som inte kan hittas i organisationens katalog (klient).</span><span class="sxs-lookup"><span data-stu-id="c3a54-137">These errors all occur when the application a user is trying to consent to is requesting permissions to access a resource application that cannot be found in the organization’s directory (tenant).</span></span> <span data-ttu-id="c3a54-138">Detta kan inträffa av olika orsaker:</span><span class="sxs-lookup"><span data-stu-id="c3a54-138">This can occur for several reasons:</span></span>

-   <span data-ttu-id="c3a54-139">Klienten programutvecklaren har sina program felaktigt konfigurerad, orsakar det att begära åtkomst till en ogiltig resurs.</span><span class="sxs-lookup"><span data-stu-id="c3a54-139">The client application developer has configured their application incorrectly, causing it to request access to an invalid resource.</span></span> <span data-ttu-id="c3a54-140">I det här fallet måste programutvecklaren uppdatera konfigurationen av klientprogram att lösa problemet.</span><span class="sxs-lookup"><span data-stu-id="c3a54-140">In this case, the application developer must update the configuration of the client application to resolve this issue.</span></span>

-   <span data-ttu-id="c3a54-141">Ett huvudnamn för tjänsten som representerar målprogrammet för resursen finns inte i organisationen, eller har funnits i tidigare men har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="c3a54-141">A Service Principal representing the target resource application does not exist in the organization, or existed in the past but has been removed.</span></span> <span data-ttu-id="c3a54-142">Lös problemet, måste ett huvudnamn för tjänsten för programmets resurs etableras i organisationen så att klientprogrammet kan begära åtkomst till den.</span><span class="sxs-lookup"><span data-stu-id="c3a54-142">To resolve this issue, a Service Principal for the resource application must be provisioned in the organization so the client application can request permissions to it.</span></span> <span data-ttu-id="c3a54-143">Detta kan hända i ett antal olika sätt beroende på vilken typ av program, inklusive:</span><span class="sxs-lookup"><span data-stu-id="c3a54-143">This can happen in an number of ways, depending on the type of application, including:</span></span>

    -   <span data-ttu-id="c3a54-144">Skaffa en prenumeration för resursprogram (Microsoft publicerade program)</span><span class="sxs-lookup"><span data-stu-id="c3a54-144">Acquiring a subscription for the resource application (Microsoft published applications)</span></span>

    -   <span data-ttu-id="c3a54-145">Principer för resursprogrammet</span><span class="sxs-lookup"><span data-stu-id="c3a54-145">Consenting to the resource application</span></span>

    -   <span data-ttu-id="c3a54-146">Tillståndsbeviljande program via Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c3a54-146">Granting the application permissions via the Azure Portal</span></span>

    -   <span data-ttu-id="c3a54-147">Att lägga till programmet från Azure AD Application Gallery</span><span class="sxs-lookup"><span data-stu-id="c3a54-147">Adding the application from the Azure AD Application Gallery</span></span>

## <a name="next-steps"></a><span data-ttu-id="c3a54-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c3a54-148">Next steps</span></span> 

[<span data-ttu-id="c3a54-149">Appar, behörigheter och medgivande i Azure Active Directory (v1 slutpunkten)</span><span class="sxs-lookup"><span data-stu-id="c3a54-149">Apps, permissions, and consent in Azure Active Directory (v1 endpoint)</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent)<br>

[<span data-ttu-id="c3a54-150">Scope, behörigheter och medgivande i Azure Active Directory (v2.0-slutpunkten)</span><span class="sxs-lookup"><span data-stu-id="c3a54-150">Scopes, permissions, and consent in the Azure Active Directory (v2.0 endpoint)</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)


