---
title: "aaaAzure AD ansluta flera domäner"
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
ms.openlocfilehash: 91d87875ceacee4e34f132938e4bb0294fb954e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="multiple-domain-support-for-federating-with-azure-ad"></a><span data-ttu-id="83333-103">Stöd för flera domäner för federering med Azure AD</span><span class="sxs-lookup"><span data-stu-id="83333-103">Multiple Domain Support for Federating with Azure AD</span></span>
<span data-ttu-id="83333-104">hello följande dokumentation innehåller information om hur toouse flera domäner på toppnivå och underdomäner när federering med Office 365 eller Azure AD-domäner.</span><span class="sxs-lookup"><span data-stu-id="83333-104">hello following documentation provides guidance on how toouse multiple top-level domains and sub-domains when federating with Office 365 or Azure AD domains.</span></span>

## <a name="multiple-top-level-domain-support"></a><span data-ttu-id="83333-105">Stöd för flera toppdomäner</span><span class="sxs-lookup"><span data-stu-id="83333-105">Multiple top-level domain support</span></span>
<span data-ttu-id="83333-106">Federering flera domäner på toppnivå med Azure AD kräver ytterligare konfiguration som inte krävs när federerar med en toppnivådomän.</span><span class="sxs-lookup"><span data-stu-id="83333-106">Federating multiple, top-level domains with Azure AD requires some additional configuration that is not required when federating with one top-level domain.</span></span>

<span data-ttu-id="83333-107">När en domän är federerat med Azure AD kan flera egenskaper anges på hello domän i Azure.</span><span class="sxs-lookup"><span data-stu-id="83333-107">When a domain is federated with Azure AD, several properties are set on hello domain in Azure.</span></span>  <span data-ttu-id="83333-108">Ett är viktigt IssuerUri.</span><span class="sxs-lookup"><span data-stu-id="83333-108">One important one is IssuerUri.</span></span>  <span data-ttu-id="83333-109">Detta är en URI som används av Azure AD tooidentify hello-domän som hello token som är associerad med.</span><span class="sxs-lookup"><span data-stu-id="83333-109">This is a URI that is used by Azure AD tooidentify hello domain that hello token is associated with.</span></span>  <span data-ttu-id="83333-110">hello URI behöver inte tooresolve tooanything men det måste vara en giltig URI.</span><span class="sxs-lookup"><span data-stu-id="83333-110">hello URI doesn’t need tooresolve tooanything but it must be a valid URI.</span></span>  <span data-ttu-id="83333-111">Standard Azure AD anger toohello värdet för hello federationstjänstidentifierare i din lokala AD FS konfiguration.</span><span class="sxs-lookup"><span data-stu-id="83333-111">By default, Azure AD sets this toohello value of hello federation service identifier in your on-premises AD FS configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="83333-112">Hej federationstjänstidentifierare är en URI som unikt identifierar en federationstjänst.</span><span class="sxs-lookup"><span data-stu-id="83333-112">hello federation service identifier is a URI that uniquely identifies a federation service.</span></span>  <span data-ttu-id="83333-113">hello federationstjänsten är en instans av AD FS som fungerar som hello säkerhetstokentjänsten.</span><span class="sxs-lookup"><span data-stu-id="83333-113">hello federation service is an instance of AD FS that functions as hello security token service.</span></span> 
> 
> 

<span data-ttu-id="83333-114">Du kan visa IssuerUri med hello PowerShell-kommandot `Get-MsolDomainFederationSettings -DomainName <your domain>`.</span><span class="sxs-lookup"><span data-stu-id="83333-114">You can vew IssuerUri by using hello PowerShell command `Get-MsolDomainFederationSettings -DomainName <your domain>`.</span></span>

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

<span data-ttu-id="83333-116">Ett problem uppstår när vi vill tooadd mer än en toppnivådomän.</span><span class="sxs-lookup"><span data-stu-id="83333-116">A problem arises when we want tooadd more than one top-level domain.</span></span>  <span data-ttu-id="83333-117">Till exempel anta att du har installationen federation mellan Azure AD och din lokala miljö.</span><span class="sxs-lookup"><span data-stu-id="83333-117">For example, let's say you have setup federation between Azure AD and your on-premises environment.</span></span>  <span data-ttu-id="83333-118">Jag använder bmcontoso.com för det här dokumentet.  Nu jag har lagt till en andra, på den översta nivån domän, bmfabrikam.com.</span><span class="sxs-lookup"><span data-stu-id="83333-118">For this document I am using bmcontoso.com.  Now I have added a second, top-level domain, bmfabrikam.com.</span></span>

![Domäner](./media/active-directory-multiple-domains/domains.png)

<span data-ttu-id="83333-120">När vi försöker tooconvert våra bmfabrikam.com domän toobe federerad felmeddelande vi ett.</span><span class="sxs-lookup"><span data-stu-id="83333-120">When we attempt tooconvert our bmfabrikam.com domain toobe federated, we receive an error.</span></span>  <span data-ttu-id="83333-121">hello anledningen är Azure AD har en begränsning som inte tillåter hello IssuerUri egenskapen toohave hello samma värde för fler än en domän.</span><span class="sxs-lookup"><span data-stu-id="83333-121">hello reason for this is, Azure AD has a constraint that does not allow hello IssuerUri property toohave hello same value for more than one domain.</span></span>  

![Federation fel](./media/active-directory-multiple-domains/error.png)

### <a name="supportmultipledomain-parameter"></a><span data-ttu-id="83333-123">SupportMultipleDomain Parameter</span><span class="sxs-lookup"><span data-stu-id="83333-123">SupportMultipleDomain Parameter</span></span>
<span data-ttu-id="83333-124">tooworkaround detta, behöver vi tooadd en annan IssuerUri som kan göras med hjälp av hello `-SupportMultipleDomain` parameter.</span><span class="sxs-lookup"><span data-stu-id="83333-124">tooworkaround this, we need tooadd a different IssuerUri which can be done by using hello `-SupportMultipleDomain` parameter.</span></span>  <span data-ttu-id="83333-125">Den här parametern används med hello följande cmdlets:</span><span class="sxs-lookup"><span data-stu-id="83333-125">This parameter is used with hello following cmdlets:</span></span>

* `New-MsolFederatedDomain`
* `Convert-MsolDomaintoFederated`
* `Update-MsolFederatedDomain`

<span data-ttu-id="83333-126">Denna parameter gör Azure AD konfigurera hello IssuerUri så att den är baserad på hello namnet på hello domän.</span><span class="sxs-lookup"><span data-stu-id="83333-126">This parameter makes Azure AD configure hello IssuerUri so that it is based on hello name of hello domain.</span></span>  <span data-ttu-id="83333-127">Detta är unik i kataloger i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="83333-127">This will be unique across directories in Azure AD.</span></span>  <span data-ttu-id="83333-128">Med hjälp av parametern hello kan hello PowerShell-kommandot toocomplete har.</span><span class="sxs-lookup"><span data-stu-id="83333-128">Using hello parameter allows hello PowerShell command toocomplete successfully.</span></span>

![Federation fel](./media/active-directory-multiple-domains/convert.png)

<span data-ttu-id="83333-130">Tittar på hello inställningar på vår nya bmfabrikam.com domänen som du kan se hello följande:</span><span class="sxs-lookup"><span data-stu-id="83333-130">Looking at hello settings of our new bmfabrikam.com domain you can see hello following:</span></span>

![Federation fel](./media/active-directory-multiple-domains/settings.png)

<span data-ttu-id="83333-132">Observera att `-SupportMultipleDomain` ändras inte hello slutpunkter som är fortfarande konfigurerade toopoint tooour federationstjänsten på adfs.bmcontoso.com.</span><span class="sxs-lookup"><span data-stu-id="83333-132">Note that `-SupportMultipleDomain` does not change hello other endpoints which are still configured toopoint tooour federation service on adfs.bmcontoso.com.</span></span>

<span data-ttu-id="83333-133">En annan sak som `-SupportMultipleDomain` har är det garanterar att hello AD FS system innehåller hello rätt utfärdaren värdet i token som utfärdats för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="83333-133">Another thing that `-SupportMultipleDomain` does is that it ensures that hello AD FS system includes hello proper Issuer value in tokens issued for Azure AD.</span></span> <span data-ttu-id="83333-134">Detta åstadkoms genom att använda hello domän delen av hello användare UPN och ange detta som hello domän i hello IssuerUri, d.v.s. https://{upn suffix} / adfs-services-förtroendet.</span><span class="sxs-lookup"><span data-stu-id="83333-134">It does this by taking hello domain portion of hello users UPN and setting this as hello domain in hello IssuerUri, i.e. https://{upn suffix}/adfs/services/trust.</span></span> 

<span data-ttu-id="83333-135">Under autentiseringen tooAzure AD eller Office 365 är hello IssuerUri element i hello användartoken därför används toolocate hello domän i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="83333-135">Thus during authentication tooAzure AD or Office 365, hello IssuerUri element in hello user’s token is used toolocate hello domain in Azure AD.</span></span>  <span data-ttu-id="83333-136">Om en matchning inte hittas hello autentiseringen misslyckas.</span><span class="sxs-lookup"><span data-stu-id="83333-136">If a match cannot be found hello authentication will fail.</span></span> 

<span data-ttu-id="83333-137">Till exempel om en användares UPN är bsimon@bmcontoso.com, hello IssuerUri element i hello token AD FS problem anges toohttp://bmcontoso.com/adfs/services/trust.</span><span class="sxs-lookup"><span data-stu-id="83333-137">For example, if a user’s UPN is bsimon@bmcontoso.com, hello IssuerUri element in hello token AD FS issues will be set toohttp://bmcontoso.com/adfs/services/trust.</span></span> <span data-ttu-id="83333-138">Detta kommer att matcha hello Azure AD-konfiguration och autentiseringen lyckas.</span><span class="sxs-lookup"><span data-stu-id="83333-138">This will match hello Azure AD configuration, and authentication will succeed.</span></span>

<span data-ttu-id="83333-139">hello följer hello anpassad anspråksregel som implementerar denna logik:</span><span class="sxs-lookup"><span data-stu-id="83333-139">hello following is hello customized claim rule that implements this logic:</span></span>

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));


> [!IMPORTANT]
> <span data-ttu-id="83333-140">I ordning toouse hello - SupportMultipleDomain växla vid försök tooadd som är nya eller konvertera redan lagts till domäner, måste du toohave installationsprogrammet federerat förtroende-toosupport dem ursprungligen.</span><span class="sxs-lookup"><span data-stu-id="83333-140">In order toouse hello -SupportMultipleDomain switch when attempting tooadd new or convert already added domains, you need toohave setup your federated trust toosupport them originally.</span></span>  
> 
> 

## <a name="how-tooupdate-hello-trust-between-ad-fs-and-azure-ad"></a><span data-ttu-id="83333-141">Hur tooupdate hello förtroende mellan AD FS och Azure AD</span><span class="sxs-lookup"><span data-stu-id="83333-141">How tooupdate hello trust between AD FS and Azure AD</span></span>
<span data-ttu-id="83333-142">Om du inte konfigurera hello federerat förtroende mellan AD FS och din instans av Azure AD, måste du kanske toore-skapa förtroendet.</span><span class="sxs-lookup"><span data-stu-id="83333-142">If you did not setup hello federated trust between AD FS and your instance of Azure AD, you may need toore-create this trust.</span></span>  <span data-ttu-id="83333-143">Detta beror på att när den är ursprungligen installationen utan hello `-SupportMultipleDomain` parametern hello IssuerUri anges med hello standardvärdet.</span><span class="sxs-lookup"><span data-stu-id="83333-143">This is because, when it is originally setup without hello `-SupportMultipleDomain` parameter, hello IssuerUri is set with hello default value.</span></span>  <span data-ttu-id="83333-144">I hello ser skärmbilden nedan du hello IssuerUri anges toohttps://adfs.bmcontoso.com/adfs/services/trust.</span><span class="sxs-lookup"><span data-stu-id="83333-144">In hello screenshot below you can see hello IssuerUri is set toohttps://adfs.bmcontoso.com/adfs/services/trust.</span></span>

<span data-ttu-id="83333-145">Så om vi har lagt till en ny domän i hello Azure AD-portalen och försök sedan tooconvert nu den med hjälp av `Convert-MsolDomaintoFederated -DomainName <your domain>`, vi hämta hello följande fel.</span><span class="sxs-lookup"><span data-stu-id="83333-145">So now, if we have successfully added an new domain in hello Azure AD portal and then attempt tooconvert it using `Convert-MsolDomaintoFederated -DomainName <your domain>`, we get hello following error.</span></span>

![Federation fel](./media/active-directory-multiple-domains/trust1.png)

<span data-ttu-id="83333-147">Om du försöker tooadd hello `-SupportMultipleDomain` växel som vi får hello följande fel:</span><span class="sxs-lookup"><span data-stu-id="83333-147">If you try tooadd hello `-SupportMultipleDomain` switch we will receive hello following error:</span></span>

![Federation fel](./media/active-directory-multiple-domains/trust2.png)

<span data-ttu-id="83333-149">Bara försök toorun `Update-MsolFederatedDomain -DomainName <your domain> -SupportMultipleDomain` på hello ursprungliga domänen också resulterar i ett fel.</span><span class="sxs-lookup"><span data-stu-id="83333-149">Simply trying toorun `Update-MsolFederatedDomain -DomainName <your domain> -SupportMultipleDomain` on hello original domain will also result in an error.</span></span>

![Federation fel](./media/active-directory-multiple-domains/trust3.png)

<span data-ttu-id="83333-151">Använd hello steg nedan tooadd en ytterligare toppnivådomän.</span><span class="sxs-lookup"><span data-stu-id="83333-151">Use hello steps below tooadd an additional top-level domain.</span></span>  <span data-ttu-id="83333-152">Om du redan har lagt till en domän och inte har använt hello `-SupportMultipleDomain` parametern start med hello steg för att ta bort och uppdatera din ursprungliga domän.</span><span class="sxs-lookup"><span data-stu-id="83333-152">If you have already added a domain and did not use hello `-SupportMultipleDomain` parameter start with hello steps for removing and updating your original domain.</span></span>  <span data-ttu-id="83333-153">Om du inte har lagt till en toppnivådomän kan än du starta med hello steg för att lägga till en domän med hjälp av PowerShell för Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="83333-153">If you have not added a top-level domain yet you can start with hello steps for adding a domain using PowerShell of Azure AD Connect.</span></span>

<span data-ttu-id="83333-154">Använd följande steg tooremove hello Microsoft Online förtroende hello och uppdatera din ursprungliga domän.</span><span class="sxs-lookup"><span data-stu-id="83333-154">Use hello following steps tooremove hello Microsoft Online trust and update your original domain.</span></span>

1. <span data-ttu-id="83333-155">På din AD FS-federationsserver öppna **AD FS-hantering.**</span><span class="sxs-lookup"><span data-stu-id="83333-155">On your AD FS federation server open **AD FS Management.**</span></span> 
2. <span data-ttu-id="83333-156">Hello vänster, expandera **förtroenden** och **förtroende för förlitande part**</span><span class="sxs-lookup"><span data-stu-id="83333-156">On hello left, expand **Trust Relationships** and **Relying Party Trusts**</span></span>
3. <span data-ttu-id="83333-157">I högra hello, tar du bort hello **Identitetsplattformen för Microsoft Office 365** post.</span><span class="sxs-lookup"><span data-stu-id="83333-157">On hello right, delete hello **Microsoft Office 365 Identity Platform** entry.</span></span>
   <span data-ttu-id="83333-158">![Ta bort Microsoft Online](./media/active-directory-multiple-domains/trust4.png)</span><span class="sxs-lookup"><span data-stu-id="83333-158">![Remove Microsoft Online](./media/active-directory-multiple-domains/trust4.png)</span></span>
4. <span data-ttu-id="83333-159">På en dator som har [Azure Active Directory-modulen för Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) installerad kör hello följande: `$cred=Get-Credential`.</span><span class="sxs-lookup"><span data-stu-id="83333-159">On a machine that has [Azure Active Directory Module for Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) installed on it run hello following: `$cred=Get-Credential`.</span></span>  
5. <span data-ttu-id="83333-160">Ange hello användarnamn och lösenord för en global administratör för hello Azure AD-domän du federerar med</span><span class="sxs-lookup"><span data-stu-id="83333-160">Enter hello username and password of a global administrator for hello Azure AD domain you are federating with</span></span>
6. <span data-ttu-id="83333-161">Ange i PowerShell`Connect-MsolService -Credential $cred`</span><span class="sxs-lookup"><span data-stu-id="83333-161">In PowerShell enter `Connect-MsolService -Credential $cred`</span></span>
7. <span data-ttu-id="83333-162">Ange i PowerShell `Update-MSOLFederatedDomain -DomainName <Federated Domain Name> -SupportMultipleDomain`.</span><span class="sxs-lookup"><span data-stu-id="83333-162">In PowerShell enter `Update-MSOLFederatedDomain -DomainName <Federated Domain Name> -SupportMultipleDomain`.</span></span>  <span data-ttu-id="83333-163">Detta är hello ursprungliga domän.</span><span class="sxs-lookup"><span data-stu-id="83333-163">This is for hello original domain.</span></span>  <span data-ttu-id="83333-164">Det med hjälp av hello över domäner som den skulle ha:`Update-MsolFederatedDomain -DomainName bmcontoso.com -SupportMultipleDomain`</span><span class="sxs-lookup"><span data-stu-id="83333-164">So using hello above domains it would be:  `Update-MsolFederatedDomain -DomainName bmcontoso.com -SupportMultipleDomain`</span></span>

<span data-ttu-id="83333-165">Använd hello följande steg tooadd hello ny domän på toppnivå med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="83333-165">Use hello following steps tooadd hello new top-level domain using PowerShell</span></span>

1. <span data-ttu-id="83333-166">På en dator som har [Azure Active Directory-modulen för Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) installerad kör hello följande: `$cred=Get-Credential`.</span><span class="sxs-lookup"><span data-stu-id="83333-166">On a machine that has [Azure Active Directory Module for Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) installed on it run hello following: `$cred=Get-Credential`.</span></span>  
2. <span data-ttu-id="83333-167">Ange hello användarnamn och lösenord för en global administratör för hello Azure AD-domän du federerar med</span><span class="sxs-lookup"><span data-stu-id="83333-167">Enter hello username and password of a global administrator for hello Azure AD domain you are federating with</span></span>
3. <span data-ttu-id="83333-168">Ange i PowerShell`Connect-MsolService -Credential $cred`</span><span class="sxs-lookup"><span data-stu-id="83333-168">In PowerShell enter `Connect-MsolService -Credential $cred`</span></span>
4. <span data-ttu-id="83333-169">Ange i PowerShell`New-MsolFederatedDomain –SupportMultipleDomain –DomainName`</span><span class="sxs-lookup"><span data-stu-id="83333-169">In PowerShell enter `New-MsolFederatedDomain –SupportMultipleDomain –DomainName`</span></span>

<span data-ttu-id="83333-170">Använd hello följande steg tooadd hello ny domän på toppnivå med Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="83333-170">Use hello following steps tooadd hello new top-level domain using Azure AD Connect.</span></span>

1. <span data-ttu-id="83333-171">Starta Azure AD Connect från hello skrivbordet eller start-menyn</span><span class="sxs-lookup"><span data-stu-id="83333-171">Launch Azure AD Connect from hello desktop or start menu</span></span>
2. <span data-ttu-id="83333-172">Välj ”Lägg till en ytterligare Azure AD-domän” ![Lägg till en Azure AD-domän](./media/active-directory-multiple-domains/add1.png)</span><span class="sxs-lookup"><span data-stu-id="83333-172">Choose “Add an additional Azure AD Domain” ![Add an additional Azure AD domain](./media/active-directory-multiple-domains/add1.png)</span></span>
3. <span data-ttu-id="83333-173">Ange din Azure AD och Active Directory-autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="83333-173">Enter your Azure AD and Active Directory credentials</span></span>
4. <span data-ttu-id="83333-174">Välj andra hello domän som du vill tooconfigure för federation.</span><span class="sxs-lookup"><span data-stu-id="83333-174">Select hello second domain you wish tooconfigure for federation.</span></span>
   <span data-ttu-id="83333-175">![Lägg till en Azure AD-domän](./media/active-directory-multiple-domains/add2.png)</span><span class="sxs-lookup"><span data-stu-id="83333-175">![Add an additional Azure AD domain](./media/active-directory-multiple-domains/add2.png)</span></span>
5. <span data-ttu-id="83333-176">Klicka på Installera</span><span class="sxs-lookup"><span data-stu-id="83333-176">Click Install</span></span>

### <a name="verify-hello-new-top-level-domain"></a><span data-ttu-id="83333-177">Kontrollera hello nya toppnivådomänen</span><span class="sxs-lookup"><span data-stu-id="83333-177">Verify hello new top-level domain</span></span>
<span data-ttu-id="83333-178">Med hjälp av PowerShell-kommando för hello `Get-MsolDomainFederationSettings -DomainName <your domain>`kan du visa hello uppdateras IssuerUri.</span><span class="sxs-lookup"><span data-stu-id="83333-178">By using hello PowerShell command `Get-MsolDomainFederationSettings -DomainName <your domain>`you can view hello updated IssuerUri.</span></span>  <span data-ttu-id="83333-179">hello skärmbilden nedan visar hello federation inställningarna har uppdaterats i vår ursprungliga domän http://bmcontoso.com/adfs/services/trust</span><span class="sxs-lookup"><span data-stu-id="83333-179">hello screenshot below shows hello federation settings were updated on our original domain http://bmcontoso.com/adfs/services/trust</span></span>

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

<span data-ttu-id="83333-181">Och hello IssuerUri på vår nya domänen har ställts in toohttps://bmfabrikam.com/adfs/services/trust</span><span class="sxs-lookup"><span data-stu-id="83333-181">And hello IssuerUri on our new domain has been set toohttps://bmfabrikam.com/adfs/services/trust</span></span>

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/settings2.png)

## <a name="support-for-sub-domains"></a><span data-ttu-id="83333-183">Stöd för underordnade domäner</span><span class="sxs-lookup"><span data-stu-id="83333-183">Support for Sub-domains</span></span>
<span data-ttu-id="83333-184">När du lägger till en underordnad domän på grund av hello sätt Azure AD hanterade domäner, ärver den hello inställningar för hello överordnade.</span><span class="sxs-lookup"><span data-stu-id="83333-184">When you add a sub-domain, because of hello way Azure AD handled domains, it will inherit hello settings of hello parent.</span></span>  <span data-ttu-id="83333-185">Det innebär att hello IssuerUri måste toomatch hello överordnade.</span><span class="sxs-lookup"><span data-stu-id="83333-185">This means that hello IssuerUri needs toomatch hello parents.</span></span>

<span data-ttu-id="83333-186">Så kan anta till exempel att jag har bmcontoso.com och Lägg sedan till corp.bmcontoso.com.  Det innebär att hello IssuerUri för en användare från corp.bmcontoso.com måste toobe **http://bmcontoso.com/adfs/services/trust.**</span><span class="sxs-lookup"><span data-stu-id="83333-186">So lets say for example that I have bmcontoso.com and then add corp.bmcontoso.com.  This means that hello IssuerUri for a user from corp.bmcontoso.com will need toobe **http://bmcontoso.com/adfs/services/trust.**</span></span>  <span data-ttu-id="83333-187">Men hello standard regeln implementeras ovan för Azure AD ska generera en token med en utfärdare som **http://corp.bmcontoso.com/adfs/services/trust.**</span><span class="sxs-lookup"><span data-stu-id="83333-187">However hello standard rule implemented above for Azure AD, will generate a token with an issuer as **http://corp.bmcontoso.com/adfs/services/trust.**</span></span> <span data-ttu-id="83333-188">som inte matchar hello domän obligatoriskt värde och autentiseringen misslyckas.</span><span class="sxs-lookup"><span data-stu-id="83333-188">which will not match hello domain's required value and authentication will fail.</span></span>

### <a name="how-tooenable-support-for-sub-domains"></a><span data-ttu-id="83333-189">Hur tooenable stöd för underordnade domäner</span><span class="sxs-lookup"><span data-stu-id="83333-189">How tooenable support for sub-domains</span></span>
<span data-ttu-id="83333-190">I ordning toowork runt den här hello måste AD FS förtroende för förlitande part för Microsoft Online toobe uppdateras.</span><span class="sxs-lookup"><span data-stu-id="83333-190">In order toowork around this hello AD FS relying party trust for Microsoft Online needs toobe updated.</span></span>  <span data-ttu-id="83333-191">toodo, måste du konfigurera en anpassad anspråksregel så att den tar ut alla underordnade domäner från hello användarens UPN-suffixet vid hello anpassat utfärdaren värde.</span><span class="sxs-lookup"><span data-stu-id="83333-191">toodo this, you must configure a custom claim rule so that it strips off any sub-domains from hello user’s UPN suffix when constructing hello custom Issuer value.</span></span> 

<span data-ttu-id="83333-192">hello följande anspråk kommer att göra det:</span><span class="sxs-lookup"><span data-stu-id="83333-192">hello following claim will do this:</span></span>

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^.*@([^.]+\.)*?(?<domain>([^.]+\.?){2})$", "http://${domain}/adfs/services/trust/"));

[!NOTE]
<span data-ttu-id="83333-193">hello sista nummer i hello reguljärt uttryck ange hello hur många överordnade domäner finns i rotdomänen.</span><span class="sxs-lookup"><span data-stu-id="83333-193">hello last number in hello regular expression set hello how many parent domains there is in your root domain.</span></span> <span data-ttu-id="83333-194">Här finns i bmcontoso.com så att två överordnade domäner krävs.</span><span class="sxs-lookup"><span data-stu-id="83333-194">Here i have bmcontoso.com so two parent domains are necessary.</span></span> <span data-ttu-id="83333-195">Om tre överordnade domäner har toobe behålls (d.v.s.: corp.bmcontoso.com), och sedan hello nummer skulle ha varit tre.</span><span class="sxs-lookup"><span data-stu-id="83333-195">If three parent domains were toobe kept (i.e.: corp.bmcontoso.com), then hello number would have been three.</span></span> <span data-ttu-id="83333-196">Eventualy en intervallet kan anges, hello matchar görs alltid toomatch hello maximalt domäner.</span><span class="sxs-lookup"><span data-stu-id="83333-196">Eventualy a range can be indicated, hello match will always be made toomatch hello maximum of domains.</span></span> <span data-ttu-id="83333-197">{2,3}-matchar två toothree domäner (d.v.s.: bmfabrikam.com och corp.bmcontoso.com).</span><span class="sxs-lookup"><span data-stu-id="83333-197">"{2,3}" will match two toothree domains (i.e.: bmfabrikam.com and corp.bmcontoso.com).</span></span>

<span data-ttu-id="83333-198">Använd följande steg tooadd hello ett anpassat anspråk toosupport underordnade domäner.</span><span class="sxs-lookup"><span data-stu-id="83333-198">Use hello following steps tooadd a custom claim toosupport sub-domains.</span></span>

1. <span data-ttu-id="83333-199">Öppna AD FS-hantering</span><span class="sxs-lookup"><span data-stu-id="83333-199">Open AD FS Management</span></span>
2. <span data-ttu-id="83333-200">Högerklicka på hello Microsoft Online RP förtroende och väljer Redigera anspråksregler</span><span class="sxs-lookup"><span data-stu-id="83333-200">Right click hello Microsoft Online RP trust and choose Edit Claim rules</span></span>
3. <span data-ttu-id="83333-201">Välj hello tredje anspråksregel och Ersätt ![redigera anspråk](./media/active-directory-multiple-domains/sub1.png)</span><span class="sxs-lookup"><span data-stu-id="83333-201">Select hello third claim rule, and replace ![Edit claim](./media/active-directory-multiple-domains/sub1.png)</span></span>
4. <span data-ttu-id="83333-202">Ersätt hello aktuella anspråk:</span><span class="sxs-lookup"><span data-stu-id="83333-202">Replace hello current claim:</span></span>
   
        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)","http://${domain}/adfs/services/trust/"));
   
       with
   
        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^.*@([^.]+\.)*?(?<domain>([^.]+\.?){2})$", "http://${domain}/adfs/services/trust/"));

    ![Ersätt anspråk](./media/active-directory-multiple-domains/sub2.png)

5. <span data-ttu-id="83333-204">Klicka på Ok.</span><span class="sxs-lookup"><span data-stu-id="83333-204">Click Ok.</span></span>  <span data-ttu-id="83333-205">Klicka på Använd.</span><span class="sxs-lookup"><span data-stu-id="83333-205">Click Apply.</span></span>  <span data-ttu-id="83333-206">Klicka på Ok.</span><span class="sxs-lookup"><span data-stu-id="83333-206">Click Ok.</span></span>  <span data-ttu-id="83333-207">Stäng AD FS-hantering.</span><span class="sxs-lookup"><span data-stu-id="83333-207">Close AD FS Management.</span></span>

