---
title: aaaCustomizing attributmappning Azure AD | Microsoft Docs
description: "Lär dig vilka attributmappning för SaaS-appar i Azure Active Directory är hur du kan ändra dem tooaddress ditt företag behöver."
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
ms.openlocfilehash: 14db5303f06fc8df3b07a0a8b75713312e71bbfd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="customizing-user-provisioning-attribute-mappings-for-saas-applications-in-azure-active-directory"></a><span data-ttu-id="193d7-103">Anpassa attributmappning för Användaretablering för SaaS-program i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="193d7-103">Customizing User Provisioning Attribute Mappings for SaaS Applications in Azure Active Directory</span></span>
<span data-ttu-id="193d7-104">Microsoft Azure AD har stöd för etablering toothird parts SaaS-program, till exempel Salesforce, Google Apps och andra användare.</span><span class="sxs-lookup"><span data-stu-id="193d7-104">Microsoft Azure AD provides support for user provisioning toothird-party SaaS applications such as Salesforce, Google Apps and others.</span></span> <span data-ttu-id="193d7-105">Om du har användaretablering för en tredje parts SaaS-program aktiverat styr hello Azure-hanteringsportalen dess attributvärden i form av en konfiguration som kallas ”attributmappning”.</span><span class="sxs-lookup"><span data-stu-id="193d7-105">If you have user provisioning for a third-party SaaS application enabled, hello Azure Management Portal controls its attribute values in form of a configuration called “attribute mapping.”</span></span>

<span data-ttu-id="193d7-106">Det finns en förkonfigurerad uppsättning mappningar mellan användarobjekt i Azure AD och användarobjekt för varje SaaS-app.</span><span class="sxs-lookup"><span data-stu-id="193d7-106">There is a preconfigured set of attribute mappings between Azure AD user objects and each SaaS app’s user objects.</span></span> <span data-ttu-id="193d7-107">Vissa appar hantera andra typer av objekt, till exempel grupper eller kontakter.</span><span class="sxs-lookup"><span data-stu-id="193d7-107">Some apps manage other types of objects, such as Groups or Contacts.</span></span> <br> 
 <span data-ttu-id="193d7-108">Du kan anpassa hello attributet standardmappningar enligt tooyour affärsbehov.</span><span class="sxs-lookup"><span data-stu-id="193d7-108">You can customize hello default attribute mappings according tooyour business needs.</span></span> <span data-ttu-id="193d7-109">Detta innebär, kan du ändra eller ta bort befintliga attributmappningar för eller skapa nya attributmappning.</span><span class="sxs-lookup"><span data-stu-id="193d7-109">This means, you can change or delete existing attribute mappings or create new attribute mappings.</span></span>

<span data-ttu-id="193d7-110">I hello Azure AD-portalen kan du komma åt den här funktionen genom att klicka på en **mappningar** konfiguration under **etablering** i hello **hantera** avsnitt i en  **Företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="193d7-110">In hello Azure AD portal, you can access this feature by clicking a **Mappings** configuration under **Provisioning** in hello **Manage** section of an **Enterprise application**.</span></span>


![Salesforce][5] 

<span data-ttu-id="193d7-112">Klicka på en **mappningar** konfiguration, öppnar hello relaterade **attributmappning** bladet.</span><span class="sxs-lookup"><span data-stu-id="193d7-112">Clicking a **Mappings** configuration, opens hello related **Attribute Mapping** blade.</span></span>  
<span data-ttu-id="193d7-113">Det finns attributmappning som krävs av en SaaS-program toofunction korrekt.</span><span class="sxs-lookup"><span data-stu-id="193d7-113">There are attribute mappings that are required by a SaaS application toofunction correctly.</span></span> <span data-ttu-id="193d7-114">Obligatoriska attribut hello **ta bort** funktionen är inte tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="193d7-114">For required attributes, hello **Delete** feature is unavailable.</span></span>


![Salesforce][6]  

<span data-ttu-id="193d7-116">I hello-exemplet ovan kan du se att hello **användarnamn** attribut för ett hanterat objekt i Salesforce fylls med hello **userPrincipalName** värdet för hello länkade Azure Active Directory-objekt.</span><span class="sxs-lookup"><span data-stu-id="193d7-116">In hello example above, you can see that hello **Username** attribute of a managed object in Salesforce is populated with hello **userPrincipalName** value of hello linked Azure Active Directory Object.</span></span>

<span data-ttu-id="193d7-117">Du kan anpassa befintliga **attributmappning** genom att klicka på en mappning.</span><span class="sxs-lookup"><span data-stu-id="193d7-117">You can customize existing **Attribute Mappings** by clicking a mapping.</span></span> <span data-ttu-id="193d7-118">Då öppnas hello **Redigera attribut** bladet.</span><span class="sxs-lookup"><span data-stu-id="193d7-118">This opens hello **Edit Attribute** blade.</span></span>

![Salesforce][7]  


  

## <a name="understanding-attribute-mapping-types"></a><span data-ttu-id="193d7-120">Förstå attributtyper mappning</span><span class="sxs-lookup"><span data-stu-id="193d7-120">Understanding attribute mapping types</span></span>
<span data-ttu-id="193d7-121">Med attributmappning styra hur attribut fylls i ett SaaS-program från tredje part.</span><span class="sxs-lookup"><span data-stu-id="193d7-121">With attribute mappings, you control how attributes are populated in a third-party SaaS application.</span></span> <span data-ttu-id="193d7-122">Det finns fyra olika Mappningstyper som stöds:</span><span class="sxs-lookup"><span data-stu-id="193d7-122">There are four different mapping types supported:</span></span>

* <span data-ttu-id="193d7-123">**Direkt** – hello målattribut fylls med hello-värdet för ett attribut av hello länkade objekt i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="193d7-123">**Direct** – hello target attribute is populated with hello value of an attribute of hello linked object in Azure AD.</span></span>
* <span data-ttu-id="193d7-124">**Konstant** – hello målattribut fylls med en specifik sträng som du har angett.</span><span class="sxs-lookup"><span data-stu-id="193d7-124">**Constant** – hello target attribute is populated with a specific string you have specified.</span></span>
* <span data-ttu-id="193d7-125">**Uttrycket** -hello målattribut fylls i baserat på hello resultatet av ett skript-liknande uttryck.</span><span class="sxs-lookup"><span data-stu-id="193d7-125">**Expression** - hello target attribute is populated based on hello result of a script-like expression.</span></span> 
  <span data-ttu-id="193d7-126">Mer information finns i [skriva uttryck för attributmappning i Azure Active Directory](active-directory-saas-writing-expressions-for-attribute-mappings.md).</span><span class="sxs-lookup"><span data-stu-id="193d7-126">For more information, see [Writing Expressions for Attribute Mappings in Azure Active Directory](active-directory-saas-writing-expressions-for-attribute-mappings.md).</span></span>
* <span data-ttu-id="193d7-127">**Ingen** -hello målattribut lämnas oförändrade.</span><span class="sxs-lookup"><span data-stu-id="193d7-127">**None** - hello target attribute is left unmodified.</span></span> <span data-ttu-id="193d7-128">Om hello Målattributet är någonsin tom fylls den dock med hello standardvärde som du anger.</span><span class="sxs-lookup"><span data-stu-id="193d7-128">However, if hello target attribute is ever empty, it is populated with hello Default value that you specify.</span></span>

<span data-ttu-id="193d7-129">I tillägg toothese fyra grundläggande mappning attributtyper stöder anpassade attributmappning hello begreppet en valfri **standard** värdet tilldelning.</span><span class="sxs-lookup"><span data-stu-id="193d7-129">In addition toothese four basic attribute mapping types, custom attribute mappings support hello concept of an optional **default** value assignment.</span></span> <span data-ttu-id="193d7-130">tilldelning av hello standard värdet säkerställer att målattribut fylls med ett värde om det finns varken ett värde i Azure AD eller hello målobjektet.</span><span class="sxs-lookup"><span data-stu-id="193d7-130">hello default value assignment ensures that a target attribute is populated with a value if there is neither a value in Azure AD nor on hello target object.</span></span> <span data-ttu-id="193d7-131">de vanligaste hello-konfigurationen är tooleave detta tomt.</span><span class="sxs-lookup"><span data-stu-id="193d7-131">hello most common configuration is tooleave this blank.</span></span>


## <a name="understanding-attribute-mapping-properties"></a><span data-ttu-id="193d7-132">Förstå egenskaper för mappning av attribut</span><span class="sxs-lookup"><span data-stu-id="193d7-132">Understanding attribute mapping properties</span></span>

<span data-ttu-id="193d7-133">I föregående avsnitt hello har redan introducerades toohello attributet mappning egenskap.</span><span class="sxs-lookup"><span data-stu-id="193d7-133">In hello previous section, you have already been introduced toohello attribute mapping type property.</span></span>
<span data-ttu-id="193d7-134">I tillägg toothis egenskapen stöder också attributmappning hello följande attribut:</span><span class="sxs-lookup"><span data-stu-id="193d7-134">In addition toothis property, attribute mappings do also support hello following attributes:</span></span>

- <span data-ttu-id="193d7-135">**Källattributet** -hello användarattribut från källsystemet hello (t.ex.: Azure Active Directory).</span><span class="sxs-lookup"><span data-stu-id="193d7-135">**Source attribute** - hello user attribute from hello source system (e.g.: Azure Active Directory).</span></span>
- <span data-ttu-id="193d7-136">**Målattribut** – hello användarattribut i målsystemet hello (t.ex.: ServiceNow).</span><span class="sxs-lookup"><span data-stu-id="193d7-136">**Target attribute** – hello user attribute in hello target system (e.g.: ServiceNow).</span></span>
- <span data-ttu-id="193d7-137">**Matcha objekt med hjälp av det här attributet** – oavsett om den här mappningen används toouniquely identifiera användare mellan hello käll- och system.</span><span class="sxs-lookup"><span data-stu-id="193d7-137">**Match objects using this attribute** – Whether or not this mapping should be used toouniquely identify users between hello source and target systems.</span></span> <span data-ttu-id="193d7-138">Detta är normalt inställt på hello userPrincipalName eller e-postattribut i Azure AD, som normalt är mappad tooa användarnamn fält i ett målprogram.</span><span class="sxs-lookup"><span data-stu-id="193d7-138">This is typically set on hello userPrincipalName or mail attribute in Azure AD, which is typically mapped tooa username field in a target application.</span></span>
- <span data-ttu-id="193d7-139">**Matchar prioritet** – flera matchande attribut kan anges.</span><span class="sxs-lookup"><span data-stu-id="193d7-139">**Matching precedence** – Multiple matching attributes can be set.</span></span> <span data-ttu-id="193d7-140">När det finns flera, utvärderas de i hello ordning som anges av det här fältet.</span><span class="sxs-lookup"><span data-stu-id="193d7-140">When there are multiple, they are evaluated in hello order defined by this field.</span></span> <span data-ttu-id="193d7-141">När en matchning hittas matchar ingen ytterligare attribut utvärderas.</span><span class="sxs-lookup"><span data-stu-id="193d7-141">As soon as a match is found, no further matching attributes are evaluated.</span></span>
- <span data-ttu-id="193d7-142">**Använd den här mappningen**</span><span class="sxs-lookup"><span data-stu-id="193d7-142">**Apply this mapping**</span></span>
    - <span data-ttu-id="193d7-143">**Alltid** – tillämpa den här mappningen för både skapa användare och uppdatera åtgärder</span><span class="sxs-lookup"><span data-stu-id="193d7-143">**Always** – Apply this mapping on both user creation and update actions</span></span>
    - <span data-ttu-id="193d7-144">**Endast under skapa** -mappningen tillämpas endast på användare creation-åtgärder</span><span class="sxs-lookup"><span data-stu-id="193d7-144">**Only during creation** - Apply this mapping only on user creation actions</span></span>


## <a name="what-you-should-know"></a><span data-ttu-id="193d7-145">Vad du bör känna till</span><span class="sxs-lookup"><span data-stu-id="193d7-145">What you should know</span></span>

<span data-ttu-id="193d7-146">Microsoft Azure AD tillhandahåller en effektiv implementering av processen för synkronisering.</span><span class="sxs-lookup"><span data-stu-id="193d7-146">Microsoft Azure AD provides an efficient implementation of a synchronization process.</span></span> <span data-ttu-id="193d7-147">I en miljö som initierats bearbetas bara de objekt som kräver uppdateringar under en synkroniseringscykel.</span><span class="sxs-lookup"><span data-stu-id="193d7-147">In an initialized environment, only objects requiring updates are processed during a synchronization cycle.</span></span> <span data-ttu-id="193d7-148">Uppdatera attributmappning påverkar hello prestanda för en synkroniseringscykel.</span><span class="sxs-lookup"><span data-stu-id="193d7-148">Updating attribute mappings has an impact on hello performance of a synchronization cycle.</span></span> <span data-ttu-id="193d7-149">En uppdatering toohello attributet mappningskonfigurationen kräver alla hanterade objekt toobe reevaluated.</span><span class="sxs-lookup"><span data-stu-id="193d7-149">An update toohello attribute mapping configuration requires all managed objects toobe reevaluated.</span></span> <span data-ttu-id="193d7-150">Det är ett rekommenderad bästa praxis tookeep hello antal på varandra följande ändringar tooyour attributmappning minst.</span><span class="sxs-lookup"><span data-stu-id="193d7-150">It is a recommended best practice tookeep hello number of consecutive changes tooyour attribute mappings at a minimum.</span></span>

## <a name="next-steps"></a><span data-ttu-id="193d7-151">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="193d7-151">Next steps</span></span>

* [<span data-ttu-id="193d7-152">Artikelindex för programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="193d7-152">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="193d7-153">Automatisera användaren etablering/avetablering tooSaaS appar</span><span class="sxs-lookup"><span data-stu-id="193d7-153">Automate User Provisioning/Deprovisioning tooSaaS Apps</span></span>](active-directory-saas-app-provisioning.md)
* [<span data-ttu-id="193d7-154">Skriva uttryck för attributmappning</span><span class="sxs-lookup"><span data-stu-id="193d7-154">Writing Expressions for Attribute Mappings</span></span>](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [<span data-ttu-id="193d7-155">Omfångsfilter för Användaretablering</span><span class="sxs-lookup"><span data-stu-id="193d7-155">Scoping Filters for User Provisioning</span></span>](active-directory-saas-scoping-filters.md)
* [<span data-ttu-id="193d7-156">Använda SCIM tooenable Automatisk etablering av användare och grupper från Azure Active Directory tooapplications</span><span class="sxs-lookup"><span data-stu-id="193d7-156">Using SCIM tooenable automatic provisioning of users and groups from Azure Active Directory tooapplications</span></span>](active-directory-scim-provisioning.md)
* [<span data-ttu-id="193d7-157">Kontot etablering meddelanden</span><span class="sxs-lookup"><span data-stu-id="193d7-157">Account Provisioning Notifications</span></span>](active-directory-saas-account-provisioning-notifications.md)
* [<span data-ttu-id="193d7-158">Lista över självstudier om hur tooIntegrate SaaS-appar</span><span class="sxs-lookup"><span data-stu-id="193d7-158">List of Tutorials on How tooIntegrate SaaS Apps</span></span>](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-customizing-attribute-mappings/ic765497.png
[2]: ./media/active-directory-saas-customizing-attribute-mappings/ic775419.png
[3]: ./media/active-directory-saas-customizing-attribute-mappings/ic775420.png
[4]: ./media/active-directory-saas-customizing-attribute-mappings/ic775421.png
[5]: ./media/active-directory-saas-customizing-attribute-mappings/21.png
[6]: ./media/active-directory-saas-customizing-attribute-mappings/22.png
[7]: ./media/active-directory-saas-customizing-attribute-mappings/23.png

