---
title: Anpassa Azure AD-attributmappning | Microsoft Docs
description: "Lär dig vilka attributmappning för SaaS-appar i Azure Active Directory är hur du kan ändra dem för att adressera dina affärsbehov."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 549e0b8c-87ce-4c9b-b487-b7bf0155dc77
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: markvi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6ca2fdc9c68ea0030d938eeaebd57aafa0e2790f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="customizing-user-provisioning-attribute-mappings-for-saas-applications-in-azure-active-directory"></a><span data-ttu-id="99360-103">Anpassa attributmappning för Användaretablering för SaaS-program i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="99360-103">Customizing User Provisioning Attribute Mappings for SaaS Applications in Azure Active Directory</span></span>
<span data-ttu-id="99360-104">Microsoft Azure AD har stöd för användaretablering för SaaS-program från tredje part, till exempel Salesforce, Google Apps och andra.</span><span class="sxs-lookup"><span data-stu-id="99360-104">Microsoft Azure AD provides support for user provisioning to third-party SaaS applications such as Salesforce, Google Apps and others.</span></span> <span data-ttu-id="99360-105">Om du har användaretablering för en tredje parts SaaS-program aktiverat styr Azure-hanteringsportalen dess attributvärden i form av en konfiguration som kallas ”attributmappning”.</span><span class="sxs-lookup"><span data-stu-id="99360-105">If you have user provisioning for a third-party SaaS application enabled, the Azure Management Portal controls its attribute values in form of a configuration called “attribute mapping.”</span></span>

<span data-ttu-id="99360-106">Det finns en förkonfigurerad uppsättning mappningar mellan användarobjekt i Azure AD och användarobjekt för varje SaaS-app.</span><span class="sxs-lookup"><span data-stu-id="99360-106">There is a preconfigured set of attribute mappings between Azure AD user objects and each SaaS app’s user objects.</span></span> <span data-ttu-id="99360-107">Vissa appar hantera andra typer av objekt, till exempel grupper eller kontakter.</span><span class="sxs-lookup"><span data-stu-id="99360-107">Some apps manage other types of objects, such as Groups or Contacts.</span></span> <br> 
 <span data-ttu-id="99360-108">Du kan anpassa attributet standardmappningar enligt dina behov.</span><span class="sxs-lookup"><span data-stu-id="99360-108">You can customize the default attribute mappings according to your business needs.</span></span> <span data-ttu-id="99360-109">Detta innebär, kan du ändra eller ta bort befintliga attributmappningar för eller skapa nya attributmappning.</span><span class="sxs-lookup"><span data-stu-id="99360-109">This means, you can change or delete existing attribute mappings or create new attribute mappings.</span></span>

<span data-ttu-id="99360-110">I Azure AD-portalen kan du komma åt den här funktionen genom att klicka på en **mappningar** konfiguration under **etablering** i den **hantera** avsnitt i en **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="99360-110">In the Azure AD portal, you can access this feature by clicking a **Mappings** configuration under **Provisioning** in the **Manage** section of an **Enterprise application**.</span></span>


![Salesforce][5] 

<span data-ttu-id="99360-112">Klicka på en **mappningar** konfiguration, öppnar det relaterade **attributmappning** bladet.</span><span class="sxs-lookup"><span data-stu-id="99360-112">Clicking a **Mappings** configuration, opens the related **Attribute Mapping** blade.</span></span>  
<span data-ttu-id="99360-113">Det finns attributmappning som krävs av ett SaaS-program ska fungera korrekt.</span><span class="sxs-lookup"><span data-stu-id="99360-113">There are attribute mappings that are required by a SaaS application to function correctly.</span></span> <span data-ttu-id="99360-114">För obligatoriska attribut i **ta bort** funktionen är inte tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="99360-114">For required attributes, the **Delete** feature is unavailable.</span></span>


![Salesforce][6]  

<span data-ttu-id="99360-116">I exemplet ovan kan du se att den **användarnamn** attribut för ett hanterat objekt i Salesforce fylls med den **userPrincipalName** värdet för den länkade Azure Active Directory-objekt.</span><span class="sxs-lookup"><span data-stu-id="99360-116">In the example above, you can see that the **Username** attribute of a managed object in Salesforce is populated with the **userPrincipalName** value of the linked Azure Active Directory Object.</span></span>

<span data-ttu-id="99360-117">Du kan anpassa befintliga **attributmappning** genom att klicka på en mappning.</span><span class="sxs-lookup"><span data-stu-id="99360-117">You can customize existing **Attribute Mappings** by clicking a mapping.</span></span> <span data-ttu-id="99360-118">Då öppnas den **Redigera attribut** bladet.</span><span class="sxs-lookup"><span data-stu-id="99360-118">This opens the **Edit Attribute** blade.</span></span>

![Salesforce][7]  


  

## <a name="understanding-attribute-mapping-types"></a><span data-ttu-id="99360-120">Förstå attributtyper mappning</span><span class="sxs-lookup"><span data-stu-id="99360-120">Understanding attribute mapping types</span></span>
<span data-ttu-id="99360-121">Med attributmappning styra hur attribut fylls i ett SaaS-program från tredje part.</span><span class="sxs-lookup"><span data-stu-id="99360-121">With attribute mappings, you control how attributes are populated in a third-party SaaS application.</span></span> <span data-ttu-id="99360-122">Det finns fyra olika Mappningstyper som stöds:</span><span class="sxs-lookup"><span data-stu-id="99360-122">There are four different mapping types supported:</span></span>

* <span data-ttu-id="99360-123">**Direkt** – Målattributet fylls i med värdet för ett attribut för det länkade objektet i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="99360-123">**Direct** – the target attribute is populated with the value of an attribute of the linked object in Azure AD.</span></span>
* <span data-ttu-id="99360-124">**Konstant** – Målattributet fylls med en specifik sträng som du har angett.</span><span class="sxs-lookup"><span data-stu-id="99360-124">**Constant** – the target attribute is populated with a specific string you have specified.</span></span>
* <span data-ttu-id="99360-125">**Uttrycket** -Målattributet fylls i baserat på resultatet av ett skript-liknande uttryck.</span><span class="sxs-lookup"><span data-stu-id="99360-125">**Expression** - the target attribute is populated based on the result of a script-like expression.</span></span> 
  <span data-ttu-id="99360-126">Mer information finns i [skriva uttryck för attributmappning i Azure Active Directory](active-directory-saas-writing-expressions-for-attribute-mappings.md).</span><span class="sxs-lookup"><span data-stu-id="99360-126">For more information, see [Writing Expressions for Attribute Mappings in Azure Active Directory](active-directory-saas-writing-expressions-for-attribute-mappings.md).</span></span>
* <span data-ttu-id="99360-127">**Ingen** -Målattributet lämnas oförändrade.</span><span class="sxs-lookup"><span data-stu-id="99360-127">**None** - the target attribute is left unmodified.</span></span> <span data-ttu-id="99360-128">Men om Målattributet är någonsin tom fylls det i med standardvärdet som du anger.</span><span class="sxs-lookup"><span data-stu-id="99360-128">However, if the target attribute is ever empty, it is populated with the Default value that you specify.</span></span>

<span data-ttu-id="99360-129">Förutom dessa fyra grundläggande attributet Mappningstyper anpassade attributmappning stöder begreppet en valfri **standard** värdet tilldelning.</span><span class="sxs-lookup"><span data-stu-id="99360-129">In addition to these four basic attribute mapping types, custom attribute mappings support the concept of an optional **default** value assignment.</span></span> <span data-ttu-id="99360-130">Default-tilldelningen värdet säkerställer att målattribut fylls med ett värde om det finns varken ett värde i Azure AD eller i målobjektet.</span><span class="sxs-lookup"><span data-stu-id="99360-130">The default value assignment ensures that a target attribute is populated with a value if there is neither a value in Azure AD nor on the target object.</span></span> <span data-ttu-id="99360-131">Den vanligaste konfigurationen är att du lämnar fältet tomt.</span><span class="sxs-lookup"><span data-stu-id="99360-131">The most common configuration is to leave this blank.</span></span>


## <a name="understanding-attribute-mapping-properties"></a><span data-ttu-id="99360-132">Förstå egenskaper för mappning av attribut</span><span class="sxs-lookup"><span data-stu-id="99360-132">Understanding attribute mapping properties</span></span>

<span data-ttu-id="99360-133">I det föregående avsnittet har du redan har lagts till mappningen type-egenskapen för attributet.</span><span class="sxs-lookup"><span data-stu-id="99360-133">In the previous section, you have already been introduced to the attribute mapping type property.</span></span>
<span data-ttu-id="99360-134">Förutom den här egenskapen stöder också attributmappning följande attribut:</span><span class="sxs-lookup"><span data-stu-id="99360-134">In addition to this property, attribute mappings do also support the following attributes:</span></span>

- <span data-ttu-id="99360-135">**Källattributet** -användarattribut från källsystemet (t.ex.: Azure Active Directory).</span><span class="sxs-lookup"><span data-stu-id="99360-135">**Source attribute** - The user attribute from the source system (e.g.: Azure Active Directory).</span></span>
- <span data-ttu-id="99360-136">**Målattribut** – användarattribut i målsystemet (t.ex.: ServiceNow).</span><span class="sxs-lookup"><span data-stu-id="99360-136">**Target attribute** – The user attribute in the target system (e.g.: ServiceNow).</span></span>
- <span data-ttu-id="99360-137">**Matcha objekt med hjälp av det här attributet** – oavsett om den här mappningen som ska användas för att unikt identifiera användarna mellan käll-och mål.</span><span class="sxs-lookup"><span data-stu-id="99360-137">**Match objects using this attribute** – Whether or not this mapping should be used to uniquely identify users between the source and target systems.</span></span> <span data-ttu-id="99360-138">Detta anges normalt i Azure AD, som vanligtvis är mappad till ett fält för användarnamn i ett målprogram på attributet userPrincipalName eller e-post.</span><span class="sxs-lookup"><span data-stu-id="99360-138">This is typically set on the userPrincipalName or mail attribute in Azure AD, which is typically mapped to a username field in a target application.</span></span>
- <span data-ttu-id="99360-139">**Matchar prioritet** – flera matchande attribut kan anges.</span><span class="sxs-lookup"><span data-stu-id="99360-139">**Matching precedence** – Multiple matching attributes can be set.</span></span> <span data-ttu-id="99360-140">När det finns flera, utvärderas de i den ordning som anges av det här fältet.</span><span class="sxs-lookup"><span data-stu-id="99360-140">When there are multiple, they are evaluated in the order defined by this field.</span></span> <span data-ttu-id="99360-141">När en matchning hittas matchar ingen ytterligare attribut utvärderas.</span><span class="sxs-lookup"><span data-stu-id="99360-141">As soon as a match is found, no further matching attributes are evaluated.</span></span>
- <span data-ttu-id="99360-142">**Använd den här mappningen**</span><span class="sxs-lookup"><span data-stu-id="99360-142">**Apply this mapping**</span></span>
    - <span data-ttu-id="99360-143">**Alltid** – tillämpa den här mappningen för både skapa användare och uppdatera åtgärder</span><span class="sxs-lookup"><span data-stu-id="99360-143">**Always** – Apply this mapping on both user creation and update actions</span></span>
    - <span data-ttu-id="99360-144">**Endast under skapa** -mappningen tillämpas endast på användare creation-åtgärder</span><span class="sxs-lookup"><span data-stu-id="99360-144">**Only during creation** - Apply this mapping only on user creation actions</span></span>


## <a name="what-you-should-know"></a><span data-ttu-id="99360-145">Vad du bör känna till</span><span class="sxs-lookup"><span data-stu-id="99360-145">What you should know</span></span>

<span data-ttu-id="99360-146">Microsoft Azure AD tillhandahåller en effektiv implementering av processen för synkronisering.</span><span class="sxs-lookup"><span data-stu-id="99360-146">Microsoft Azure AD provides an efficient implementation of a synchronization process.</span></span> <span data-ttu-id="99360-147">I en miljö som initierats bearbetas bara de objekt som kräver uppdateringar under en synkroniseringscykel.</span><span class="sxs-lookup"><span data-stu-id="99360-147">In an initialized environment, only objects requiring updates are processed during a synchronization cycle.</span></span> <span data-ttu-id="99360-148">Uppdatera attributmappning påverkar prestandan hos en synkroniseringscykel.</span><span class="sxs-lookup"><span data-stu-id="99360-148">Updating attribute mappings has an impact on the performance of a synchronization cycle.</span></span> <span data-ttu-id="99360-149">En uppdatering för attributet mappningskonfigurationen kräver alla hanterade objekt som ska vara reevaluated.</span><span class="sxs-lookup"><span data-stu-id="99360-149">An update to the attribute mapping configuration requires all managed objects to be reevaluated.</span></span> <span data-ttu-id="99360-150">Det är rekommenderad praxis att hålla din attributmappning minst antal på varandra följande ändringar.</span><span class="sxs-lookup"><span data-stu-id="99360-150">It is a recommended best practice to keep the number of consecutive changes to your attribute mappings at a minimum.</span></span>

## <a name="next-steps"></a><span data-ttu-id="99360-151">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="99360-151">Next steps</span></span>

* [<span data-ttu-id="99360-152">Artikelindex för programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="99360-152">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="99360-153">Automatisera användaren etablering/avetablering för SaaS-appar</span><span class="sxs-lookup"><span data-stu-id="99360-153">Automate User Provisioning/Deprovisioning to SaaS Apps</span></span>](active-directory-saas-app-provisioning.md)
* [<span data-ttu-id="99360-154">Skriva uttryck för attributmappning</span><span class="sxs-lookup"><span data-stu-id="99360-154">Writing Expressions for Attribute Mappings</span></span>](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [<span data-ttu-id="99360-155">Omfångsfilter för Användaretablering</span><span class="sxs-lookup"><span data-stu-id="99360-155">Scoping Filters for User Provisioning</span></span>](active-directory-saas-scoping-filters.md)
* [<span data-ttu-id="99360-156">Använda SCIM för att aktivera automatisk etablering av användare och grupper från Azure Active Directory till program</span><span class="sxs-lookup"><span data-stu-id="99360-156">Using SCIM to enable automatic provisioning of users and groups from Azure Active Directory to applications</span></span>](active-directory-scim-provisioning.md)
* [<span data-ttu-id="99360-157">Kontot etablering meddelanden</span><span class="sxs-lookup"><span data-stu-id="99360-157">Account Provisioning Notifications</span></span>](active-directory-saas-account-provisioning-notifications.md)
* [<span data-ttu-id="99360-158">Lista över självstudier om hur du integrerar SaaS-appar</span><span class="sxs-lookup"><span data-stu-id="99360-158">List of Tutorials on How to Integrate SaaS Apps</span></span>](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-customizing-attribute-mappings/ic765497.png
[2]: ./media/active-directory-saas-customizing-attribute-mappings/ic775419.png
[3]: ./media/active-directory-saas-customizing-attribute-mappings/ic775420.png
[4]: ./media/active-directory-saas-customizing-attribute-mappings/ic775421.png
[5]: ./media/active-directory-saas-customizing-attribute-mappings/21.png
[6]: ./media/active-directory-saas-customizing-attribute-mappings/22.png
[7]: ./media/active-directory-saas-customizing-attribute-mappings/23.png

