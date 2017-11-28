---
title: "attribut för aaaAzure AD Connect sync shadow | Microsoft Docs"
description: "Beskriver hur shadow attribut fungerar i Azure AD Connect-synkroniseringstjänsten."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 1b8665e7488c6078b655f8a3e35519145bacd898
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-service-shadow-attributes"></a><span data-ttu-id="82c1c-103">Azure AD Connect sync shadow attribut</span><span class="sxs-lookup"><span data-stu-id="82c1c-103">Azure AD Connect sync service shadow attributes</span></span>
<span data-ttu-id="82c1c-104">De flesta attribut representeras hello samma sätt i Azure AD som i din lokala Active Directory.</span><span class="sxs-lookup"><span data-stu-id="82c1c-104">Most attributes are represented hello same way in Azure AD as they are in your on-premises Active Directory.</span></span> <span data-ttu-id="82c1c-105">Men vissa attribut har en särskild hantering och hello attributvärdet i Azure AD kan vara annorlunda än Azure AD Connect synkroniserar.</span><span class="sxs-lookup"><span data-stu-id="82c1c-105">But some attributes have some special handling and hello attribute value in Azure AD might be different than what Azure AD Connect synchronizes.</span></span>

## <a name="introducing-shadow-attributes"></a><span data-ttu-id="82c1c-106">Introduktion till shadow attribut</span><span class="sxs-lookup"><span data-stu-id="82c1c-106">Introducing shadow attributes</span></span>
<span data-ttu-id="82c1c-107">Vissa attribut har två representationer i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="82c1c-107">Some attributes have two representations in Azure AD.</span></span> <span data-ttu-id="82c1c-108">Både hello lokalt värde och ett beräknat värde som lagras.</span><span class="sxs-lookup"><span data-stu-id="82c1c-108">Both hello on-premises value and a calculated value are stored.</span></span> <span data-ttu-id="82c1c-109">Dessa extra attribut kallas shadow attribut.</span><span class="sxs-lookup"><span data-stu-id="82c1c-109">These extra attributes are called shadow attributes.</span></span> <span data-ttu-id="82c1c-110">hello två vanligaste attributen där du ser det här beteendet är **userPrincipalName** och **proxyAddress**.</span><span class="sxs-lookup"><span data-stu-id="82c1c-110">hello two most common attributes where you see this behavior are **userPrincipalName** and **proxyAddress**.</span></span> <span data-ttu-id="82c1c-111">hello förändring attributvärden händer när det finns värden i attributen som motsvarar icke verifierade domäner.</span><span class="sxs-lookup"><span data-stu-id="82c1c-111">hello change in attribute values happens when there are values in these attributes representing non-verified domains.</span></span> <span data-ttu-id="82c1c-112">Men hello Synkroniseringsmotorn i Connect läser hello värde i hello shadow attributet så från dess perspektiv, hello-attributet har bekräftats av Azure AD.</span><span class="sxs-lookup"><span data-stu-id="82c1c-112">But hello sync engine in Connect reads hello value in hello shadow attribute so from its perspective, hello attribute has been confirmed by Azure AD.</span></span>

<span data-ttu-id="82c1c-113">Du kan inte se hello shadow attribut med hello Azure portal eller PowerShell.</span><span class="sxs-lookup"><span data-stu-id="82c1c-113">You cannot see hello shadow attributes using hello Azure portal or with PowerShell.</span></span> <span data-ttu-id="82c1c-114">Men förstå hello konceptet kan du tootroubleshoot vissa scenarier där hello-attributet har olika värden på lokalt och i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="82c1c-114">But understanding hello concept helps you tootroubleshoot certain scenarios where hello attribute has different values on-premises and in hello cloud.</span></span>

<span data-ttu-id="82c1c-115">toobetter förstå hello beteende, titta på det här exemplet från Fabrikam:</span><span class="sxs-lookup"><span data-stu-id="82c1c-115">toobetter understand hello behavior, look at this example from Fabrikam:</span></span>  
![Domäner](./media/active-directory-aadconnectsyncservice-shadow-attributes/domains.png)  
<span data-ttu-id="82c1c-117">De har flera UPN-suffix i sina lokala Active Directory, men de bara har verifierat en.</span><span class="sxs-lookup"><span data-stu-id="82c1c-117">They have multiple UPN suffixes in their on-premises Active Directory, but they have only verified one.</span></span>

### <a name="userprincipalname"></a><span data-ttu-id="82c1c-118">UserPrincipalName</span><span class="sxs-lookup"><span data-stu-id="82c1c-118">userPrincipalName</span></span>
<span data-ttu-id="82c1c-119">En användare har hello följande attributvärden i en icke-verifierad domän:</span><span class="sxs-lookup"><span data-stu-id="82c1c-119">A user has hello following attribute values in a non-verified domain:</span></span>

| <span data-ttu-id="82c1c-120">Attribut</span><span class="sxs-lookup"><span data-stu-id="82c1c-120">Attribute</span></span> | <span data-ttu-id="82c1c-121">Värde</span><span class="sxs-lookup"><span data-stu-id="82c1c-121">Value</span></span> |
| --- | --- |
| <span data-ttu-id="82c1c-122">lokala userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="82c1c-122">on-premises userPrincipalName</span></span> | lee.sperry@fabrikam.com |
| <span data-ttu-id="82c1c-123">Azure AD-shadowUserPrincipalName</span><span class="sxs-lookup"><span data-stu-id="82c1c-123">Azure AD shadowUserPrincipalName</span></span> | lee.sperry@fabrikam.com |
| <span data-ttu-id="82c1c-124">Azure AD-userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="82c1c-124">Azure AD userPrincipalName</span></span> | lee.sperry@fabrikam.onmicrosoft.com |

<span data-ttu-id="82c1c-125">attributet för hello userPrincipalName är hello värdet när du använder PowerShell.</span><span class="sxs-lookup"><span data-stu-id="82c1c-125">hello userPrincipalName attribute is hello value you see when using PowerShell.</span></span>

<span data-ttu-id="82c1c-126">Eftersom hello verkliga lokalt attributvärdet lagras i Azure AD kan du verifiera när hello fabrikam.com domän, uppdaterar Azure AD hello userPrincipalName attributet med hello-värde från hello shadowUserPrincipalName.</span><span class="sxs-lookup"><span data-stu-id="82c1c-126">Since hello real on-premises attribute value is stored in Azure AD, when you verify hello fabrikam.com domain, Azure AD updates hello userPrincipalName attribute with hello value from hello shadowUserPrincipalName.</span></span> <span data-ttu-id="82c1c-127">Du har inte toosynchronize ändringar från Azure AD Connect för dessa värden toobe uppdateras.</span><span class="sxs-lookup"><span data-stu-id="82c1c-127">You do not have toosynchronize any changes from Azure AD Connect for these values toobe updated.</span></span>

### <a name="proxyaddresses"></a><span data-ttu-id="82c1c-128">proxyAddresses</span><span class="sxs-lookup"><span data-stu-id="82c1c-128">proxyAddresses</span></span>
<span data-ttu-id="82c1c-129">hello samma process för att bara inkludera verifierade domäner inträffar också för proxyAddresses men med vissa ytterligare logik.</span><span class="sxs-lookup"><span data-stu-id="82c1c-129">hello same process for only including verified domains also occurs for proxyAddresses, but with some additional logic.</span></span> <span data-ttu-id="82c1c-130">hello Sök efter verifierade domäner sker endast för postlådan användare.</span><span class="sxs-lookup"><span data-stu-id="82c1c-130">hello check for verified domains only happens for mailbox users.</span></span> <span data-ttu-id="82c1c-131">En e-postaktiverade användare eller kontakta representerar en användare i en annan Exchange-organisation och du kan lägga till alla värden i proxyAddresses toothese objekt.</span><span class="sxs-lookup"><span data-stu-id="82c1c-131">A mail-enabled user or contact represent a user in another Exchange organization and you can add any values in proxyAddresses toothese objects.</span></span>

<span data-ttu-id="82c1c-132">För en postlåda användare, antingen lokalt eller i Exchange Online visas bara värden för verifierade domäner.</span><span class="sxs-lookup"><span data-stu-id="82c1c-132">For a mailbox user, either on-premises or in Exchange Online, only values for verified domains appear.</span></span> <span data-ttu-id="82c1c-133">Det kan se ut så här:</span><span class="sxs-lookup"><span data-stu-id="82c1c-133">It could look like this:</span></span>

| <span data-ttu-id="82c1c-134">Attribut</span><span class="sxs-lookup"><span data-stu-id="82c1c-134">Attribute</span></span> | <span data-ttu-id="82c1c-135">Värde</span><span class="sxs-lookup"><span data-stu-id="82c1c-135">Value</span></span> |
| --- | --- |
| <span data-ttu-id="82c1c-136">lokala proxyAddresses</span><span class="sxs-lookup"><span data-stu-id="82c1c-136">on-premises proxyAddresses</span></span> | SMTP:abbie.spencer@fabrikamonline.com</br>smtp:abbie.spencer@fabrikam.com</br>smtp:abbie@fabrikamonline.com |
| <span data-ttu-id="82c1c-137">Exchange Online proxyAddresses</span><span class="sxs-lookup"><span data-stu-id="82c1c-137">Exchange Online proxyAddresses</span></span> | SMTP:abbie.spencer@fabrikamonline.com</br>smtp:abbie@fabrikamonline.com</br>SIP:abbie.spencer@fabrikamonline.com |

<span data-ttu-id="82c1c-138">I det här fallet  **smtp:abbie.spencer@fabrikam.com**  har tagits bort eftersom den domänen inte har verifierats.</span><span class="sxs-lookup"><span data-stu-id="82c1c-138">In this case **smtp:abbie.spencer@fabrikam.com** was removed since that domain has not been verified.</span></span> <span data-ttu-id="82c1c-139">Men har även lagt till Exchange  **SIP:abbie.spencer@fabrikamonline.com** . Fabrikam inte har använt Lync/Skype lokal, men Azure AD och Exchange Online förbereda för den.</span><span class="sxs-lookup"><span data-stu-id="82c1c-139">But Exchange also added **SIP:abbie.spencer@fabrikamonline.com**. Fabrikam has not used Lync/Skype on-premises, but Azure AD and Exchange Online prepare for it.</span></span>

<span data-ttu-id="82c1c-140">Den här logiken för proxyAddresses är refererad tooas **ProxyCalc**.</span><span class="sxs-lookup"><span data-stu-id="82c1c-140">This logic for proxyAddresses is referred tooas **ProxyCalc**.</span></span> <span data-ttu-id="82c1c-141">ProxyCalc anropas med varje ändring för en användare när:</span><span class="sxs-lookup"><span data-stu-id="82c1c-141">ProxyCalc is invoked with every change on a user when:</span></span>

- <span data-ttu-id="82c1c-142">hello användaren har tilldelats en serviceplan som innehåller Exchange Online, även om hello användaren inte har licensierats för Exchange.</span><span class="sxs-lookup"><span data-stu-id="82c1c-142">hello user has been assigned a service plan that includes Exchange Online even if hello user was not licensed for Exchange.</span></span> <span data-ttu-id="82c1c-143">Till exempel om hello användaren har tilldelats hello Office E3 SKU, men endast har tilldelats SharePoint Online.</span><span class="sxs-lookup"><span data-stu-id="82c1c-143">For example, if hello user is assigned hello Office E3 SKU, but only was assigned SharePoint Online.</span></span> <span data-ttu-id="82c1c-144">Detta gäller även om postlådan är fortfarande lokalt.</span><span class="sxs-lookup"><span data-stu-id="82c1c-144">This is true even if your mailbox is still on-premises.</span></span>
- <span data-ttu-id="82c1c-145">hello attributet msExchRecipientTypeDetails har ett värde.</span><span class="sxs-lookup"><span data-stu-id="82c1c-145">hello attribute msExchRecipientTypeDetails has a value.</span></span>
- <span data-ttu-id="82c1c-146">Du gör en ändring tooproxyAddresses eller userPrincipalName.</span><span class="sxs-lookup"><span data-stu-id="82c1c-146">You make a change tooproxyAddresses or userPrincipalName.</span></span>

<span data-ttu-id="82c1c-147">ProxyCalc kan ta viss tid tooprocess ändras för en användare och är inte synkroniserad med hello Azure AD Connect exporten.</span><span class="sxs-lookup"><span data-stu-id="82c1c-147">ProxyCalc might take some time tooprocess a change on a user and is not synchronous with hello Azure AD Connect export process.</span></span>

> [!NOTE]
> <span data-ttu-id="82c1c-148">Hej ProxyCalc logik har vissa ytterligare beteenden för avancerade scenarier som inte beskrivs i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="82c1c-148">hello ProxyCalc logic has some additional behaviors for advanced scenarios not documented in this topic.</span></span> <span data-ttu-id="82c1c-149">Det här avsnittet är du toounderstand hello beteendet inte dokumentera alla internt.</span><span class="sxs-lookup"><span data-stu-id="82c1c-149">This topic is provided for you toounderstand hello behavior and not document all internal logic.</span></span>

### <a name="quarantined-attribute-values"></a><span data-ttu-id="82c1c-150">I karantän attributvärden</span><span class="sxs-lookup"><span data-stu-id="82c1c-150">Quarantined attribute values</span></span>
<span data-ttu-id="82c1c-151">Shadow-attribut används också när det finns dubblerade attributvärden.</span><span class="sxs-lookup"><span data-stu-id="82c1c-151">Shadow attributes are also used when there are duplicate attribute values.</span></span> <span data-ttu-id="82c1c-152">Mer information finns i [dubblettattribut återhämtning](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md).</span><span class="sxs-lookup"><span data-stu-id="82c1c-152">For more information, see [duplicate attribute resiliency](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="82c1c-153">Se även</span><span class="sxs-lookup"><span data-stu-id="82c1c-153">See also</span></span>
* [<span data-ttu-id="82c1c-154">Azure AD Connect-synkronisering</span><span class="sxs-lookup"><span data-stu-id="82c1c-154">Azure AD Connect sync</span></span>](active-directory-aadconnectsync-whatis.md)
* <span data-ttu-id="82c1c-155">[Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="82c1c-155">[Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>
