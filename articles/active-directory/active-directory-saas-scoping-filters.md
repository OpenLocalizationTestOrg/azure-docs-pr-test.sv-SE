---
title: "Etablering appar med Omfångsfilter | Microsoft Docs"
description: "Lär dig hur du använder målgrupp filter för att förhindra att objekt i appar som stöder automatisk användaretablering från tillhandahålls faktiskt om ett objekt inte uppfyller dina affärsbehov."
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
ms.openlocfilehash: 109635052e2ded33831b050eb12d50745944091b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="attribute-based-application-provisioning-with-scoping-filters"></a>Attributbaserad programmet etablering med Omfångsfilter
Syftet med det här avsnittet är som förklarar hur du använder målgrupp filter för att definiera attributbaserade regler som bestämmer vilka användare som har etablerats till programmet.

## <a name="clauses-and-scope-groups"></a>Satser och grupper
![Omfångsfilter][1] 

Målgrupp filter definieras av en eller flera **omfång grupper**, varje av som innehåller en eller flera **satser**. Visa satser för en viss scope-grupp, expandera den genom att klicka på pilen till vänster om gruppnamnet.

En **satsen** avgör vilka användare som får passera filtret målgrupp genom utvärdering av attribut för varje användare. Du kan till exempel ha en sats som kräver att en användares 'state'-attribut är lika med New York, så att endast användare i New York tillhandahålls av programmet.

![Målgrupp gruppnamn][2] 

Varje **omfång grupp** börjar med en obligatorisk **satsen**som visas i skärmbilden ovan. Den här satsen anger bara att användaren måste först tilldelas programmet innan det används av din målgrupp filter. Den här instruktionen kan inte tas bort eller ändras.

Du kan lägga till nya satser eller nya grupper genom att trycka på knappen. Du kan namnge varje scope-grupp genom att redigera dess **omfång gruppnamn** egenskapen.

## <a name="how-scoping-filters-are-evaluated"></a>Hur scope filter utvärderas
Under etableringen, kan vi Testa varje tilldelad användare mot din målgrupp filter för att avgöra om användaren ska ha åtkomst till programmet. Du kan tänka dig varje instruktion som ett test som måste skickas för användare att undvika komma filtreras bort. 

Om du har flera grupper som har definierats kan skicka varje användare minst en av dem åtkomst till programmet. I varje scope-grupp men måste användaren klara alla-satsen för att skicka gruppen specifikt omfång. 

Med andra ord kan jämföras med grupper som villkor och du kan tänka dig satser i dem som och villkor. Anta till exempel att filtret målgrupp nedan:

![Målgrupp gruppnamn][3]  

Användarna måste uppfylla följande kriterier som ska etableras med enligt det här målgrupp filtret:

1. De måste tilldelas till programmet.
2. De måste arbeta i avdelningen tekniker
3. De måste vara fungerar i San Francisco eller Kanada.

## <a name="related-articles"></a>Relaterade artiklar
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)
* [Automatisera användaren etablering och avetablering för SaaS-program](active-directory-saas-app-provisioning.md)
* [Anpassa attributmappning för Användaretablering](active-directory-saas-customizing-attribute-mappings.md)
* [Skriva uttryck för attributmappning](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Kontot etablering meddelanden](active-directory-saas-account-provisioning-notifications.md)
* [Använda SCIM för att aktivera automatisk etablering av användare och grupper från Azure Active Directory till program](active-directory-scim-provisioning.md)
* [Lista över självstudier om hur du integrerar SaaS-appar](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-scoping-filters/ic782811.png
[2]: ./media/active-directory-saas-scoping-filters/ic782812.png
[3]: ./media/active-directory-saas-scoping-filters/ic782813.png
