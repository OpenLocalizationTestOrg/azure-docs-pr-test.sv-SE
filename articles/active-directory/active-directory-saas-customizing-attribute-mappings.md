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
# <a name="customizing-user-provisioning-attribute-mappings-for-saas-applications-in-azure-active-directory"></a>Anpassa attributmappning för Användaretablering för SaaS-program i Azure Active Directory
Microsoft Azure AD har stöd för etablering toothird parts SaaS-program, till exempel Salesforce, Google Apps och andra användare. Om du har användaretablering för en tredje parts SaaS-program aktiverat styr hello Azure-hanteringsportalen dess attributvärden i form av en konfiguration som kallas ”attributmappning”.

Det finns en förkonfigurerad uppsättning mappningar mellan användarobjekt i Azure AD och användarobjekt för varje SaaS-app. Vissa appar hantera andra typer av objekt, till exempel grupper eller kontakter. <br> 
 Du kan anpassa hello attributet standardmappningar enligt tooyour affärsbehov. Detta innebär, kan du ändra eller ta bort befintliga attributmappningar för eller skapa nya attributmappning.

I hello Azure AD-portalen kan du komma åt den här funktionen genom att klicka på en **mappningar** konfiguration under **etablering** i hello **hantera** avsnitt i en  **Företagsprogram**.


![Salesforce][5] 

Klicka på en **mappningar** konfiguration, öppnar hello relaterade **attributmappning** bladet.  
Det finns attributmappning som krävs av en SaaS-program toofunction korrekt. Obligatoriska attribut hello **ta bort** funktionen är inte tillgänglig.


![Salesforce][6]  

I hello-exemplet ovan kan du se att hello **användarnamn** attribut för ett hanterat objekt i Salesforce fylls med hello **userPrincipalName** värdet för hello länkade Azure Active Directory-objekt.

Du kan anpassa befintliga **attributmappning** genom att klicka på en mappning. Då öppnas hello **Redigera attribut** bladet.

![Salesforce][7]  


  

## <a name="understanding-attribute-mapping-types"></a>Förstå attributtyper mappning
Med attributmappning styra hur attribut fylls i ett SaaS-program från tredje part. Det finns fyra olika Mappningstyper som stöds:

* **Direkt** – hello målattribut fylls med hello-värdet för ett attribut av hello länkade objekt i Azure AD.
* **Konstant** – hello målattribut fylls med en specifik sträng som du har angett.
* **Uttrycket** -hello målattribut fylls i baserat på hello resultatet av ett skript-liknande uttryck. 
  Mer information finns i [skriva uttryck för attributmappning i Azure Active Directory](active-directory-saas-writing-expressions-for-attribute-mappings.md).
* **Ingen** -hello målattribut lämnas oförändrade. Om hello Målattributet är någonsin tom fylls den dock med hello standardvärde som du anger.

I tillägg toothese fyra grundläggande mappning attributtyper stöder anpassade attributmappning hello begreppet en valfri **standard** värdet tilldelning. tilldelning av hello standard värdet säkerställer att målattribut fylls med ett värde om det finns varken ett värde i Azure AD eller hello målobjektet. de vanligaste hello-konfigurationen är tooleave detta tomt.


## <a name="understanding-attribute-mapping-properties"></a>Förstå egenskaper för mappning av attribut

I föregående avsnitt hello har redan introducerades toohello attributet mappning egenskap.
I tillägg toothis egenskapen stöder också attributmappning hello följande attribut:

- **Källattributet** -hello användarattribut från källsystemet hello (t.ex.: Azure Active Directory).
- **Målattribut** – hello användarattribut i målsystemet hello (t.ex.: ServiceNow).
- **Matcha objekt med hjälp av det här attributet** – oavsett om den här mappningen används toouniquely identifiera användare mellan hello käll- och system. Detta är normalt inställt på hello userPrincipalName eller e-postattribut i Azure AD, som normalt är mappad tooa användarnamn fält i ett målprogram.
- **Matchar prioritet** – flera matchande attribut kan anges. När det finns flera, utvärderas de i hello ordning som anges av det här fältet. När en matchning hittas matchar ingen ytterligare attribut utvärderas.
- **Använd den här mappningen**
    - **Alltid** – tillämpa den här mappningen för både skapa användare och uppdatera åtgärder
    - **Endast under skapa** -mappningen tillämpas endast på användare creation-åtgärder


## <a name="what-you-should-know"></a>Vad du bör känna till

Microsoft Azure AD tillhandahåller en effektiv implementering av processen för synkronisering. I en miljö som initierats bearbetas bara de objekt som kräver uppdateringar under en synkroniseringscykel. Uppdatera attributmappning påverkar hello prestanda för en synkroniseringscykel. En uppdatering toohello attributet mappningskonfigurationen kräver alla hanterade objekt toobe reevaluated. Det är ett rekommenderad bästa praxis tookeep hello antal på varandra följande ändringar tooyour attributmappning minst.

## <a name="next-steps"></a>Nästa steg

* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)
* [Automatisera användaren etablering/avetablering tooSaaS appar](active-directory-saas-app-provisioning.md)
* [Skriva uttryck för attributmappning](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Omfångsfilter för Användaretablering](active-directory-saas-scoping-filters.md)
* [Använda SCIM tooenable Automatisk etablering av användare och grupper från Azure Active Directory tooapplications](active-directory-scim-provisioning.md)
* [Kontot etablering meddelanden](active-directory-saas-account-provisioning-notifications.md)
* [Lista över självstudier om hur tooIntegrate SaaS-appar](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-customizing-attribute-mappings/ic765497.png
[2]: ./media/active-directory-saas-customizing-attribute-mappings/ic775419.png
[3]: ./media/active-directory-saas-customizing-attribute-mappings/ic775420.png
[4]: ./media/active-directory-saas-customizing-attribute-mappings/ic775421.png
[5]: ./media/active-directory-saas-customizing-attribute-mappings/21.png
[6]: ./media/active-directory-saas-customizing-attribute-mappings/22.png
[7]: ./media/active-directory-saas-customizing-attribute-mappings/23.png

