---
title: "Azure AD Connect flera domäner"
description: "Det här dokumentet beskriver hur du konfigurerar och konfigurera flera domäner på toppnivå med O365 och Azure AD."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: curtand
ms.assetid: 5595fb2f-2131-4304-8a31-c52559128ea4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 8e3f496c2868cc3430e0efd47805aec2205168aa
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="multiple-domain-support-for-federating-with-azure-ad"></a><span data-ttu-id="ad311-103">Stöd för flera domäner för federering med Azure AD</span><span class="sxs-lookup"><span data-stu-id="ad311-103">Multiple Domain Support for Federating with Azure AD</span></span>
<span data-ttu-id="ad311-104">Följande dokumentation innehåller information om hur du använder flera domäner på toppnivå och underdomäner när federering med Office 365 eller Azure AD-domäner.</span><span class="sxs-lookup"><span data-stu-id="ad311-104">The following documentation provides guidance on how to use multiple top-level domains and sub-domains when federating with Office 365 or Azure AD domains.</span></span>

## <a name="multiple-top-level-domain-support"></a><span data-ttu-id="ad311-105">Stöd för flera toppdomäner</span><span class="sxs-lookup"><span data-stu-id="ad311-105">Multiple top-level domain support</span></span>
<span data-ttu-id="ad311-106">Federering flera domäner på toppnivå med Azure AD kräver ytterligare konfiguration som inte krävs när federerar med en toppnivådomän.</span><span class="sxs-lookup"><span data-stu-id="ad311-106">Federating multiple, top-level domains with Azure AD requires some additional configuration that is not required when federating with one top-level domain.</span></span>

<span data-ttu-id="ad311-107">När en domän är federerat med Azure AD, ange flera egenskaper på domänen i Azure.</span><span class="sxs-lookup"><span data-stu-id="ad311-107">When a domain is federated with Azure AD, several properties are set on the domain in Azure.</span></span>  <span data-ttu-id="ad311-108">Ett är viktigt IssuerUri.</span><span class="sxs-lookup"><span data-stu-id="ad311-108">One important one is IssuerUri.</span></span>  <span data-ttu-id="ad311-109">Det här är en URI som används av Azure AD för att identifiera den domän som token som är associerad med.</span><span class="sxs-lookup"><span data-stu-id="ad311-109">This is a URI that is used by Azure AD to identify the domain that the token is associated with.</span></span>  <span data-ttu-id="ad311-110">URI: N behöver inte matcha till något annat än det måste vara en giltig URI.</span><span class="sxs-lookup"><span data-stu-id="ad311-110">The URI doesn’t need to resolve to anything but it must be a valid URI.</span></span>  <span data-ttu-id="ad311-111">Standard Azure AD anger detta till värdet för federationstjänstidentifieraren i din lokala AD FS konfiguration.</span><span class="sxs-lookup"><span data-stu-id="ad311-111">By default, Azure AD sets this to the value of the federation service identifier in your on-premises AD FS configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="ad311-112">Identifierare för federation service är en URI som unikt identifierar en federationstjänst.</span><span class="sxs-lookup"><span data-stu-id="ad311-112">The federation service identifier is a URI that uniquely identifies a federation service.</span></span>  <span data-ttu-id="ad311-113">Federationstjänsten är en instans av AD FS som fungerar som säkerhetstokentjänsten.</span><span class="sxs-lookup"><span data-stu-id="ad311-113">The federation service is an instance of AD FS that functions as the security token service.</span></span> 
> 
> 

<span data-ttu-id="ad311-114">Du kan visa IssuerUri med hjälp av PowerShell-kommando `Get-MsolDomainFederationSettings -DomainName <your domain>`.</span><span class="sxs-lookup"><span data-stu-id="ad311-114">You can vew IssuerUri by using the PowerShell command `Get-MsolDomainFederationSettings -DomainName <your domain>`.</span></span>

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

<span data-ttu-id="ad311-116">Ett problem uppstår när vi vill lägga till fler än en toppnivådomän.</span><span class="sxs-lookup"><span data-stu-id="ad311-116">A problem arises when we want to add more than one top-level domain.</span></span>  <span data-ttu-id="ad311-117">Till exempel anta att du har installationen federation mellan Azure AD och din lokala miljö.</span><span class="sxs-lookup"><span data-stu-id="ad311-117">For example, let's say you have setup federation between Azure AD and your on-premises environment.</span></span>  <span data-ttu-id="ad311-118">Jag använder bmcontoso.com för det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="ad311-118">For this document I am using bmcontoso.com.</span></span>  <span data-ttu-id="ad311-119">Nu jag har lagt till en andra, på den översta nivån domän, bmfabrikam.com.</span><span class="sxs-lookup"><span data-stu-id="ad311-119">Now I have added a second, top-level domain, bmfabrikam.com.</span></span>

![Domäner](./media/active-directory-multiple-domains/domains.png)

<span data-ttu-id="ad311-121">När vi försöker konvertera vår bmfabrikam.com domän ska vara federerad får vi ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="ad311-121">When we attempt to convert our bmfabrikam.com domain to be federated, we receive an error.</span></span>  <span data-ttu-id="ad311-122">Anledningen är Azure AD har en begränsning som inte tillåter att egenskapen IssuerUri ska ha samma värde för fler än en domän.</span><span class="sxs-lookup"><span data-stu-id="ad311-122">The reason for this is, Azure AD has a constraint that does not allow the IssuerUri property to have the same value for more than one domain.</span></span>  

![Federation fel](./media/active-directory-multiple-domains/error.png)

### <a name="supportmultipledomain-parameter"></a><span data-ttu-id="ad311-124">SupportMultipleDomain Parameter</span><span class="sxs-lookup"><span data-stu-id="ad311-124">SupportMultipleDomain Parameter</span></span>
<span data-ttu-id="ad311-125">För att lösa detta, behöver vi lägga till en annan IssuerUri som kan göras med hjälp av den `-SupportMultipleDomain` parameter.</span><span class="sxs-lookup"><span data-stu-id="ad311-125">To workaround this, we need to add a different IssuerUri which can be done by using the `-SupportMultipleDomain` parameter.</span></span>  <span data-ttu-id="ad311-126">Den här parametern används tillsammans med följande cmdlets:</span><span class="sxs-lookup"><span data-stu-id="ad311-126">This parameter is used with the following cmdlets:</span></span>

* `New-MsolFederatedDomain`
* `Convert-MsolDomaintoFederated`
* `Update-MsolFederatedDomain`

<span data-ttu-id="ad311-127">Denna parameter gör Azure AD som konfigurerar IssuerUri så att den är baserad på namnet på domänen.</span><span class="sxs-lookup"><span data-stu-id="ad311-127">This parameter makes Azure AD configure the IssuerUri so that it is based on the name of the domain.</span></span>  <span data-ttu-id="ad311-128">Detta är unik i kataloger i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ad311-128">This will be unique across directories in Azure AD.</span></span>  <span data-ttu-id="ad311-129">Med hjälp av parametern kan PowerShell-kommandot ska slutföras.</span><span class="sxs-lookup"><span data-stu-id="ad311-129">Using the parameter allows the PowerShell command to complete successfully.</span></span>

![Federation fel](./media/active-directory-multiple-domains/convert.png)

<span data-ttu-id="ad311-131">Tittar på inställningarna på vår nya bmfabrikam.com domänen som du kan se följande:</span><span class="sxs-lookup"><span data-stu-id="ad311-131">Looking at the settings of our new bmfabrikam.com domain you can see the following:</span></span>

![Federation fel](./media/active-directory-multiple-domains/settings.png)

<span data-ttu-id="ad311-133">Observera att `-SupportMultipleDomain` ändras inte de andra slutpunkterna som fortfarande är konfigurerade för att peka till vår federationstjänsten på adfs.bmcontoso.com.</span><span class="sxs-lookup"><span data-stu-id="ad311-133">Note that `-SupportMultipleDomain` does not change the other endpoints which are still configured to point to our federation service on adfs.bmcontoso.com.</span></span>

<span data-ttu-id="ad311-134">En annan sak som `-SupportMultipleDomain` har är det garanterar att AD FS-system innehåller värdet för rätt utfärdaren i token som utfärdats för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ad311-134">Another thing that `-SupportMultipleDomain` does is that it ensures that the AD FS system includes the proper Issuer value in tokens issued for Azure AD.</span></span> <span data-ttu-id="ad311-135">Detta åstadkoms genom att använda den som domändel av användarna UPN och inställningar som domänen i IssuerUri, d.v.s. https://{upn suffix} / adfs-services-förtroendet.</span><span class="sxs-lookup"><span data-stu-id="ad311-135">It does this by taking the domain portion of the users UPN and setting this as the domain in the IssuerUri, i.e. https://{upn suffix}/adfs/services/trust.</span></span> 

<span data-ttu-id="ad311-136">Därmed under autentiseringen till Azure AD eller Office 365, elementet IssuerUri i användarens token som används för att leta upp domänen i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ad311-136">Thus during authentication to Azure AD or Office 365, the IssuerUri element in the user’s token is used to locate the domain in Azure AD.</span></span>  <span data-ttu-id="ad311-137">Om en matchning inte hittas autentiseringen misslyckas.</span><span class="sxs-lookup"><span data-stu-id="ad311-137">If a match cannot be found the authentication will fail.</span></span> 

<span data-ttu-id="ad311-138">Till exempel om en användares UPN är bsimon@bmcontoso.com, elementet IssuerUri i token AD FS problem kommer att ställas in http://bmcontoso.com/adfs/services/trust.</span><span class="sxs-lookup"><span data-stu-id="ad311-138">For example, if a user’s UPN is bsimon@bmcontoso.com, the IssuerUri element in the token AD FS issues will be set to http://bmcontoso.com/adfs/services/trust.</span></span> <span data-ttu-id="ad311-139">Detta motsvarar Azure AD-konfigurationen och autentiseringen lyckas.</span><span class="sxs-lookup"><span data-stu-id="ad311-139">This will match the Azure AD configuration, and authentication will succeed.</span></span>

<span data-ttu-id="ad311-140">Följande är anpassade anspråksregeln som implementerar denna logik:</span><span class="sxs-lookup"><span data-stu-id="ad311-140">The following is the customized claim rule that implements this logic:</span></span>

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));


> [!IMPORTANT]
> <span data-ttu-id="ad311-141">För att kunna använda växeln - SupportMultipleDomain när du försöker lägga till nya eller konvertera redan lagts till domäner, som du behöver ha installationsprogrammet din federerat förtroende som ursprungligen stöder dessa.</span><span class="sxs-lookup"><span data-stu-id="ad311-141">In order to use the -SupportMultipleDomain switch when attempting to add new or convert already added domains, you need to have setup your federated trust to support them originally.</span></span>  
> 
> 

## <a name="how-to-update-the-trust-between-ad-fs-and-azure-ad"></a><span data-ttu-id="ad311-142">Så här uppdaterar du förtroende mellan AD FS och Azure AD</span><span class="sxs-lookup"><span data-stu-id="ad311-142">How to update the trust between AD FS and Azure AD</span></span>
<span data-ttu-id="ad311-143">Om du inte konfigurera federerat förtroende mellan AD FS och din instans av Azure AD, kan du behöva återskapa förtroendet.</span><span class="sxs-lookup"><span data-stu-id="ad311-143">If you did not setup the federated trust between AD FS and your instance of Azure AD, you may need to re-create this trust.</span></span>  <span data-ttu-id="ad311-144">Detta beror på att när den är ursprungligen installationen utan den `-SupportMultipleDomain` parameter, IssuerUri anges med standardvärdet.</span><span class="sxs-lookup"><span data-stu-id="ad311-144">This is because, when it is originally setup without the `-SupportMultipleDomain` parameter, the IssuerUri is set with the default value.</span></span>  <span data-ttu-id="ad311-145">I skärmbilden nedan du ser IssuerUri är inställd på https://adfs.bmcontoso.com/adfs/services/trust.</span><span class="sxs-lookup"><span data-stu-id="ad311-145">In the screenshot below you can see the IssuerUri is set to https://adfs.bmcontoso.com/adfs/services/trust.</span></span>

<span data-ttu-id="ad311-146">Så nu, om vi har lagt till en ny domän i Azure AD-portalen och försök sedan att konvertera den med hjälp av `Convert-MsolDomaintoFederated -DomainName <your domain>`, vi använder följande felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="ad311-146">So now, if we have successfully added an new domain in the Azure AD portal and then attempt to convert it using `Convert-MsolDomaintoFederated -DomainName <your domain>`, we get the following error.</span></span>

![Federation fel](./media/active-directory-multiple-domains/trust1.png)

<span data-ttu-id="ad311-148">Om du försöker lägga till den `-SupportMultipleDomain` växel som vi får du följande felmeddelande:</span><span class="sxs-lookup"><span data-stu-id="ad311-148">If you try to add the `-SupportMultipleDomain` switch we will receive the following error:</span></span>

![Federation fel](./media/active-directory-multiple-domains/trust2.png)

<span data-ttu-id="ad311-150">Bara försöker köra `Update-MsolFederatedDomain -DomainName <your domain> -SupportMultipleDomain` på den ursprungliga domänen också resulterar i ett fel.</span><span class="sxs-lookup"><span data-stu-id="ad311-150">Simply trying to run `Update-MsolFederatedDomain -DomainName <your domain> -SupportMultipleDomain` on the original domain will also result in an error.</span></span>

![Federation fel](./media/active-directory-multiple-domains/trust3.png)

<span data-ttu-id="ad311-152">Följ anvisningarna nedan för att lägga till en ytterligare toppnivådomän.</span><span class="sxs-lookup"><span data-stu-id="ad311-152">Use the steps below to add an additional top-level domain.</span></span>  <span data-ttu-id="ad311-153">Om du redan har lagt till en domän och inte har använt den `-SupportMultipleDomain` parametern börjar med stegen för att ta bort och uppdatera din ursprungliga domän.</span><span class="sxs-lookup"><span data-stu-id="ad311-153">If you have already added a domain and did not use the `-SupportMultipleDomain` parameter start with the steps for removing and updating your original domain.</span></span>  <span data-ttu-id="ad311-154">Om du inte har lagt till en toppnivådomän kan ännu du med steg för att lägga till en domän med hjälp av PowerShell för Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="ad311-154">If you have not added a top-level domain yet you can start with the steps for adding a domain using PowerShell of Azure AD Connect.</span></span>

<span data-ttu-id="ad311-155">Använd följande steg för att ta bort Microsoft Online-förtroende och uppdatera din ursprungliga domän.</span><span class="sxs-lookup"><span data-stu-id="ad311-155">Use the following steps to remove the Microsoft Online trust and update your original domain.</span></span>

1. <span data-ttu-id="ad311-156">På din AD FS-federationsserver öppna **AD FS-hantering.**</span><span class="sxs-lookup"><span data-stu-id="ad311-156">On your AD FS federation server open **AD FS Management.**</span></span> 
2. <span data-ttu-id="ad311-157">Till vänster, expandera **förtroenden** och **förtroende för förlitande part**</span><span class="sxs-lookup"><span data-stu-id="ad311-157">On the left, expand **Trust Relationships** and **Relying Party Trusts**</span></span>
3. <span data-ttu-id="ad311-158">Ta bort till höger i **Identitetsplattformen för Microsoft Office 365** post.</span><span class="sxs-lookup"><span data-stu-id="ad311-158">On the right, delete the **Microsoft Office 365 Identity Platform** entry.</span></span>
   <span data-ttu-id="ad311-159">![Ta bort Microsoft Online](./media/active-directory-multiple-domains/trust4.png)</span><span class="sxs-lookup"><span data-stu-id="ad311-159">![Remove Microsoft Online](./media/active-directory-multiple-domains/trust4.png)</span></span>
4. <span data-ttu-id="ad311-160">På en dator som har [Azure Active Directory-modulen för Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) installerad kör du följande: `$cred=Get-Credential`.</span><span class="sxs-lookup"><span data-stu-id="ad311-160">On a machine that has [Azure Active Directory Module for Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) installed on it run the following: `$cred=Get-Credential`.</span></span>  
5. <span data-ttu-id="ad311-161">Ange användarnamn och lösenord för en global administratör för Azure AD-domänen du federerar med</span><span class="sxs-lookup"><span data-stu-id="ad311-161">Enter the username and password of a global administrator for the Azure AD domain you are federating with</span></span>
6. <span data-ttu-id="ad311-162">Ange i PowerShell`Connect-MsolService -Credential $cred`</span><span class="sxs-lookup"><span data-stu-id="ad311-162">In PowerShell enter `Connect-MsolService -Credential $cred`</span></span>
7. <span data-ttu-id="ad311-163">Ange i PowerShell `Update-MSOLFederatedDomain -DomainName <Federated Domain Name> -SupportMultipleDomain`.</span><span class="sxs-lookup"><span data-stu-id="ad311-163">In PowerShell enter `Update-MSOLFederatedDomain -DomainName <Federated Domain Name> -SupportMultipleDomain`.</span></span>  <span data-ttu-id="ad311-164">Detta är den ursprungliga domänen.</span><span class="sxs-lookup"><span data-stu-id="ad311-164">This is for the original domain.</span></span>  <span data-ttu-id="ad311-165">Det med hjälp av ovanstående domäner som den skulle ha:`Update-MsolFederatedDomain -DomainName bmcontoso.com -SupportMultipleDomain`</span><span class="sxs-lookup"><span data-stu-id="ad311-165">So using the above domains it would be:  `Update-MsolFederatedDomain -DomainName bmcontoso.com -SupportMultipleDomain`</span></span>

<span data-ttu-id="ad311-166">Använd följande steg för att lägga till den nya översta domänen med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="ad311-166">Use the following steps to add the new top-level domain using PowerShell</span></span>

1. <span data-ttu-id="ad311-167">På en dator som har [Azure Active Directory-modulen för Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) installerad kör du följande: `$cred=Get-Credential`.</span><span class="sxs-lookup"><span data-stu-id="ad311-167">On a machine that has [Azure Active Directory Module for Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) installed on it run the following: `$cred=Get-Credential`.</span></span>  
2. <span data-ttu-id="ad311-168">Ange användarnamn och lösenord för en global administratör för Azure AD-domänen du federerar med</span><span class="sxs-lookup"><span data-stu-id="ad311-168">Enter the username and password of a global administrator for the Azure AD domain you are federating with</span></span>
3. <span data-ttu-id="ad311-169">Ange i PowerShell`Connect-MsolService -Credential $cred`</span><span class="sxs-lookup"><span data-stu-id="ad311-169">In PowerShell enter `Connect-MsolService -Credential $cred`</span></span>
4. <span data-ttu-id="ad311-170">Ange i PowerShell`New-MsolFederatedDomain –SupportMultipleDomain –DomainName`</span><span class="sxs-lookup"><span data-stu-id="ad311-170">In PowerShell enter `New-MsolFederatedDomain –SupportMultipleDomain –DomainName`</span></span>

<span data-ttu-id="ad311-171">Använd följande steg för att lägga till den nya översta domänen med Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="ad311-171">Use the following steps to add the new top-level domain using Azure AD Connect.</span></span>

1. <span data-ttu-id="ad311-172">Starta Azure AD Connect från skrivbordet eller start-menyn</span><span class="sxs-lookup"><span data-stu-id="ad311-172">Launch Azure AD Connect from the desktop or start menu</span></span>
2. <span data-ttu-id="ad311-173">Välj ”Lägg till en ytterligare Azure AD-domän” ![Lägg till en Azure AD-domän](./media/active-directory-multiple-domains/add1.png)</span><span class="sxs-lookup"><span data-stu-id="ad311-173">Choose “Add an additional Azure AD Domain” ![Add an additional Azure AD domain](./media/active-directory-multiple-domains/add1.png)</span></span>
3. <span data-ttu-id="ad311-174">Ange din Azure AD och Active Directory-autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="ad311-174">Enter your Azure AD and Active Directory credentials</span></span>
4. <span data-ttu-id="ad311-175">Välj den andra domänen som du vill konfigurera för federation.</span><span class="sxs-lookup"><span data-stu-id="ad311-175">Select the second domain you wish to configure for federation.</span></span>
   <span data-ttu-id="ad311-176">![Lägg till en Azure AD-domän](./media/active-directory-multiple-domains/add2.png)</span><span class="sxs-lookup"><span data-stu-id="ad311-176">![Add an additional Azure AD domain](./media/active-directory-multiple-domains/add2.png)</span></span>
5. <span data-ttu-id="ad311-177">Klicka på Installera</span><span class="sxs-lookup"><span data-stu-id="ad311-177">Click Install</span></span>

### <a name="verify-the-new-top-level-domain"></a><span data-ttu-id="ad311-178">Kontrollera den nya översta domänen</span><span class="sxs-lookup"><span data-stu-id="ad311-178">Verify the new top-level domain</span></span>
<span data-ttu-id="ad311-179">Med hjälp av PowerShell-kommando `Get-MsolDomainFederationSettings -DomainName <your domain>`du kan visa den uppdaterade IssuerUri.</span><span class="sxs-lookup"><span data-stu-id="ad311-179">By using the PowerShell command `Get-MsolDomainFederationSettings -DomainName <your domain>`you can view the updated IssuerUri.</span></span>  <span data-ttu-id="ad311-180">Skärmbilden nedan visar federationen inställningarna har uppdaterats i vår ursprungliga domän http://bmcontoso.com/adfs/services/trust</span><span class="sxs-lookup"><span data-stu-id="ad311-180">The screenshot below shows the federation settings were updated on our original domain http://bmcontoso.com/adfs/services/trust</span></span>

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

<span data-ttu-id="ad311-182">Och IssuerUri på vår nya domänen har ställts in på https://bmfabrikam.com/adfs/services/trust</span><span class="sxs-lookup"><span data-stu-id="ad311-182">And the IssuerUri on our new domain has been set to https://bmfabrikam.com/adfs/services/trust</span></span>

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/settings2.png)

## <a name="support-for-sub-domains"></a><span data-ttu-id="ad311-184">Stöd för underordnade domäner</span><span class="sxs-lookup"><span data-stu-id="ad311-184">Support for Sub-domains</span></span>
<span data-ttu-id="ad311-185">När du lägger till en underordnad domän, på grund av hur Azure AD hanterade domäner ärver den inställningarna för överordnat.</span><span class="sxs-lookup"><span data-stu-id="ad311-185">When you add a sub-domain, because of the way Azure AD handled domains, it will inherit the settings of the parent.</span></span>  <span data-ttu-id="ad311-186">Det innebär att IssuerUri måste matcha överordnade.</span><span class="sxs-lookup"><span data-stu-id="ad311-186">This means that the IssuerUri needs to match the parents.</span></span>

<span data-ttu-id="ad311-187">Så kan anta till exempel att jag har bmcontoso.com och Lägg sedan till corp.bmcontoso.com.</span><span class="sxs-lookup"><span data-stu-id="ad311-187">So lets say for example that I have bmcontoso.com and then add corp.bmcontoso.com.</span></span>  <span data-ttu-id="ad311-188">Det innebär att IssuerUri för en användare från corp.bmcontoso.com måste vara **http://bmcontoso.com/adfs/services/trust.**</span><span class="sxs-lookup"><span data-stu-id="ad311-188">This means that the IssuerUri for a user from corp.bmcontoso.com will need to be **http://bmcontoso.com/adfs/services/trust.**</span></span>  <span data-ttu-id="ad311-189">Men standard regeln implementerats ovan för Azure AD, genererar en token med en utfärdare som **http://corp.bmcontoso.com/adfs/services/trust.**</span><span class="sxs-lookup"><span data-stu-id="ad311-189">However the standard rule implemented above for Azure AD, will generate a token with an issuer as **http://corp.bmcontoso.com/adfs/services/trust.**</span></span> <span data-ttu-id="ad311-190">som inte matchar domänens krävs värdet och autentiseringen misslyckas.</span><span class="sxs-lookup"><span data-stu-id="ad311-190">which will not match the domain's required value and authentication will fail.</span></span>

### <a name="how-to-enable-support-for-sub-domains"></a><span data-ttu-id="ad311-191">Så här aktiverar du stöd för underordnade domäner</span><span class="sxs-lookup"><span data-stu-id="ad311-191">How To enable support for sub-domains</span></span>
<span data-ttu-id="ad311-192">Förlitande part för Microsoft Online måste uppdateras för att komma runt detta AD FS.</span><span class="sxs-lookup"><span data-stu-id="ad311-192">In order to work around this the AD FS relying party trust for Microsoft Online needs to be updated.</span></span>  <span data-ttu-id="ad311-193">Om du vill göra detta måste konfigurera du en anpassad anspråksregel så att den tar ut alla underordnade domäner från användarens UPN-suffixet när man skapar anpassade utfärdaren värdet.</span><span class="sxs-lookup"><span data-stu-id="ad311-193">To do this, you must configure a custom claim rule so that it strips off any sub-domains from the user’s UPN suffix when constructing the custom Issuer value.</span></span> 

<span data-ttu-id="ad311-194">Följande anspråk kommer att göra det:</span><span class="sxs-lookup"><span data-stu-id="ad311-194">The following claim will do this:</span></span>

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^.*@([^.]+\.)*?(?<domain>([^.]+\.?){2})$", "http://${domain}/adfs/services/trust/"));

[!NOTE]
<span data-ttu-id="ad311-195">Det sista numret i det reguljära uttrycket ange hur många överordnade domäner som finns i rotdomänen.</span><span class="sxs-lookup"><span data-stu-id="ad311-195">The last number in the regular expression set the how many parent domains there is in your root domain.</span></span> <span data-ttu-id="ad311-196">Här finns i bmcontoso.com så att två överordnade domäner krävs.</span><span class="sxs-lookup"><span data-stu-id="ad311-196">Here i have bmcontoso.com so two parent domains are necessary.</span></span> <span data-ttu-id="ad311-197">Om tre överordnade domäner har hållas (d.v.s.: corp.bmcontoso.com), och sedan antalet skulle ha varit tre.</span><span class="sxs-lookup"><span data-stu-id="ad311-197">If three parent domains were to be kept (i.e.: corp.bmcontoso.com), then the number would have been three.</span></span> <span data-ttu-id="ad311-198">Eventualy kan ett intervall anges matchningen görs alltid att matcha maximalt antalet domäner.</span><span class="sxs-lookup"><span data-stu-id="ad311-198">Eventualy a range can be indicated, the match will always be made to match the maximum of domains.</span></span> <span data-ttu-id="ad311-199">{2,3}-matchar två till tre domäner (d.v.s.: bmfabrikam.com och corp.bmcontoso.com).</span><span class="sxs-lookup"><span data-stu-id="ad311-199">"{2,3}" will match two to three domains (i.e.: bmfabrikam.com and corp.bmcontoso.com).</span></span>

<span data-ttu-id="ad311-200">Använd följande steg om du vill lägga till ett anpassat anspråk för att stödja underdomäner.</span><span class="sxs-lookup"><span data-stu-id="ad311-200">Use the following steps to add a custom claim to support sub-domains.</span></span>

1. <span data-ttu-id="ad311-201">Öppna AD FS-hantering</span><span class="sxs-lookup"><span data-stu-id="ad311-201">Open AD FS Management</span></span>
2. <span data-ttu-id="ad311-202">Högerklicka på Microsoft Online RP-förtroende och väljer Redigera anspråksregler</span><span class="sxs-lookup"><span data-stu-id="ad311-202">Right click the Microsoft Online RP trust and choose Edit Claim rules</span></span>
3. <span data-ttu-id="ad311-203">Välj den tredje anspråksregeln och Ersätt ![redigera anspråk](./media/active-directory-multiple-domains/sub1.png)</span><span class="sxs-lookup"><span data-stu-id="ad311-203">Select the third claim rule, and replace ![Edit claim](./media/active-directory-multiple-domains/sub1.png)</span></span>
4. <span data-ttu-id="ad311-204">Ersätt det aktuella anspråket:</span><span class="sxs-lookup"><span data-stu-id="ad311-204">Replace the current claim:</span></span>
   
        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)","http://${domain}/adfs/services/trust/"));
   
       with
   
        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^.*@([^.]+\.)*?(?<domain>([^.]+\.?){2})$", "http://${domain}/adfs/services/trust/"));

    ![Ersätt anspråk](./media/active-directory-multiple-domains/sub2.png)

5. <span data-ttu-id="ad311-206">Klicka på Ok.</span><span class="sxs-lookup"><span data-stu-id="ad311-206">Click Ok.</span></span>  <span data-ttu-id="ad311-207">Klicka på Använd.</span><span class="sxs-lookup"><span data-stu-id="ad311-207">Click Apply.</span></span>  <span data-ttu-id="ad311-208">Klicka på Ok.</span><span class="sxs-lookup"><span data-stu-id="ad311-208">Click Ok.</span></span>  <span data-ttu-id="ad311-209">Stäng AD FS-hantering.</span><span class="sxs-lookup"><span data-stu-id="ad311-209">Close AD FS Management.</span></span>

