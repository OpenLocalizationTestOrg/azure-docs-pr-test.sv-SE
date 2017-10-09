---
title: "aaaProvisioning appar med Omfångsfilter | Microsoft Docs"
description: "Lär dig hur toouse scope filtrerar tooprevent objekt i appar som stöder automatisk användaretablering från tillhandahålls faktiskt om ett objekt inte uppfyller dina affärsbehov."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: bcfbda74-e4d4-4859-83bc-06b104df3918
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: markvi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f0299390dc3fdb70aa9d271e835069a08827d635
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="attribute-based-application-provisioning-with-scoping-filters"></a>Attributbaserad programmet etablering med Omfångsfilter
hello syftet med det här avsnittet är tooexplain hur toouse scope filtrerar toodefine attributbaserade regler som bestämmer vilka användare som är etablerad toohello program.

## <a name="clauses-and-scope-groups"></a>Satser och grupper
![Omfångsfilter][1] 

Målgrupp filter definieras av en eller flera **omfång grupper**, varje av som innehåller en eller flera **satser**. toosee hello-satser för en viss scope-grupp, expandera den genom att klicka på hello pilen toohello till vänster i hello gruppnamn.

En **satsen** avgör vilka användare som får toopass via hello Omfångsfilter genom utvärdering av attribut för varje användare. Till exempel kanske du en sats som kräver att en användare ”tillstånd” attributet lika New York, så att endast användare i New York tillhandahålls i hello program.

![Målgrupp gruppnamn][2] 

Varje **omfång grupp** börjar med en obligatorisk **satsen**, som visas i hello skärmbilden ovan. Den här satsen bara säger hello användaren först tilldelas toohello programmet innan det används av din målgrupp filter. Den här instruktionen kan inte tas bort eller ändras.

Du kan lägga till nya satser eller nya grupper genom att trycka på hello knapp. Du kan namnge varje scope-grupp genom att redigera dess **omfång gruppnamn** egenskapen.

## <a name="how-scoping-filters-are-evaluated"></a>Hur scope filter utvärderas
Under etableringen testa vi varje tilldelad användare mot din målgrupp filter toodetermine om användaren ska ha åtkomst toohello program. Du kan tänka dig varje instruktion som ett test som måste skickas för hello användaren tooavoid komma filtreras bort. 

Om du har flera grupper som definierats måste varje användare skicka minst en av dem tooaccess hello program. I varje scope-grupp dock måste hello användaren klara alla satsen toopass gruppen specifikt omfång. 

Med andra ord kan jämföras med grupper som villkor och du kan tänka dig hello-satser i dem som och villkor. Anta till exempel att hello Omfångsfilter nedan:

![Målgrupp gruppnamn][3]  

Enligt toothis Omfångsfilter användare måste uppfylla hello följande villkor, toobe etableras:

1. De måste tilldelas toohello program.
2. De måste arbeta på hello tekniker avdelning
3. De måste vara fungerar i San Francisco eller Kanada.

## <a name="related-articles"></a>Relaterade artiklar
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)
* [Automatisera Användaretablering och avetablering tooSaaS program](active-directory-saas-app-provisioning.md)
* [Anpassa attributmappning för Användaretablering](active-directory-saas-customizing-attribute-mappings.md)
* [Skriva uttryck för attributmappning](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Kontot etablering meddelanden](active-directory-saas-account-provisioning-notifications.md)
* [Använda SCIM tooenable Automatisk etablering av användare och grupper från Azure Active Directory tooapplications](active-directory-scim-provisioning.md)
* [Lista över självstudier om hur tooIntegrate SaaS-appar](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-scoping-filters/ic782811.png
[2]: ./media/active-directory-saas-scoping-filters/ic782812.png
[3]: ./media/active-directory-saas-scoping-filters/ic782813.png
