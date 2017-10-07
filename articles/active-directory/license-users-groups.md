---
title: "aaaLicense användare i Azure Active Directory | Microsoft Docs"
description: "Lär dig hur toolicense dig och dina användare i Azure Active Directory."
services: active-directory
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: f8b932bc-8b4f-42b5-a2d3-f2c076234a78
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: jeffgilb
custom: it-pro
ms.openlocfilehash: ae0bc030fa02b79d1dd01ca961b4e96e6b9c470d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="quickstart-license-users-in-azure-active-directory"></a>Snabbstart: Licensiera användare i Azure Active Directory
Licens-baserad Azure AD-tjänster fungerar genom att aktivera en Azure Active Directory (Azure AD)-prenumeration i Azure-klient. När hello prenumeration är aktiv, hanteras av Azure AD-administratörer funktioner och används av licensierade användare. När du köper Enterprise Mobility + Security, Azure AD Premium eller Azure AD Basic, din klient uppdateras med hello-prenumeration, inklusive giltighetsperioden och förskottsbetald licenser. Din prenumerationsinformation hello antalet tilldelade eller tillgängliga licenser är tillgängliga via hello Azure portal under **Azure Active Directory** genom att öppna hello **licenser** panelen. Hej **licenser** bladet är också hello bästa plats toomanage licenstilldelningar.

Även om hur du skaffar en prenumeration är alla måste tooconfigure betald funktioner, måste du fortfarande tilldela användarlicenser för betald Azure AD betalda funktioner. Alla användare som ska ha åtkomst till eller som hanteras via en Azure AD betald funktionen måste tilldelas en licens. Licenstilldelning finns en mappning mellan en användare och en köpta tjänst, till exempel Azure AD Premium, Basic eller Enterprise Mobility + Security.

Du kan använda [gruppbaserade licenstilldelning](active-directory-licensing-whatis-azure-portal.md) tooset regler, till exempel hello följande:
* Alla användare i din katalog få automatiskt en licens
* Alla med hello lämpliga befattning hämtar en licens
* Du kan delegera hello beslut tooother chefer i hello organisation (med hjälp av [självbetjäning grupper](active-directory-accessmanagement-self-service-group-management.md))

> [!TIP]
> En detaljerad beskrivning av licens tilldelning toogroups, inklusive avancerade scenarier och Office 365-licensiering scenarier, finns i [tilldela licenser toousers genom medlemskap i Azure Active Directory](active-directory-licensing-group-assignment-azure-portal.md).

## <a name="assign-licenses-toousers-and-groups"></a>Tilldela licenser toousers och grupper
Med en aktiv prenumeration kan du först tilldela en licens tooyourself och uppdatera din webbläsare tooensure som du ser alla hello förväntade funktioner som ingår i prenumerationen. hello nästa steg är tooassign licenser toohello användare som behöver åtkomst toopaid Azure AD-funktioner. Ett enkelt sätt tooassign licenser är tooassign licenser toogroups för användare i stället för tooindividuals. När du tilldelar licenser tooa grupp har alla medlemmar tilldelats en licens. Om användare läggs till eller tas bort från hello-gruppen, tilldelas hello lämplig licens automatiskt eller tas bort. 

> [!NOTE]
> Vissa Microsoft-tjänster är inte tillgängliga på alla platser. Innan en licens kan tilldelas tooa användare, hello administratören måste ange hello **användningsplats** egenskapen för hello användare. Du kan ange egenskapen under **användare** &gt; **profil** &gt; **inställningar** i hello Azure-portalen. När du använder gruppen licenstilldelning ärver alla användare vars användningsplats anges hello platsen för hello katalog.

tooassign en licens under **Azure Active Directory** &gt; **licenser** &gt; **alla produkter**, Välj en eller flera produkter och väljer sedan **Tilldela** i hello kommandofält.

![Välj en licens tooassign](media/license-users-groups/select-license-to-assign.png)

Du kan använda hello **användare och grupper** bladet toochoose flera användare, grupper eller toodisable service-planer i hello produkten. Använd hello sökrutan på översta toosearch för användar-och gruppnamn.

![Välj en användare eller grupp för licenstilldelning](media/license-users-groups/select-user-for-license-assignment.png)

När du tilldelar licenser tooa grupp, kan det ta lite tid innan alla användare ärver hello licens beroende på hello storlek i hello-gruppen. Du kan kontrollera status för bearbetning av hello på hello **grupp** bladet under hello **licenser** panelen.

![Licensstatus för tilldelning](media/license-users-groups/license-assignment-status.png)

Tilldelningsfel kan uppstå under Azure AD-licenstilldelning men är relativt ovanliga vid hantering av Azure AD och Enterprise Mobility + Security-produkter. Potentiella Tilldelningsfel är begränsade till:
- Tilldelningen konflikt: när en användare tidigare tilldelats en licens som inte är kompatibel med aktuellt hello-licens. I det här fallet kräver tilldela hello ny licens att ta bort hello aktuella.
- Överskred tillgängliga licenser: när hello antalet användare i tilldelade grupper överskrider hello tillgängliga licenser, status för en användares klienttilldelning visar en tooassign misslyckades på grund av toomissing licenser.

### <a name="azure-ad-b2b-collaboration-licensing"></a>Azure AD B2B-samarbete och licensiering

B2B-samarbete kan du tooinvite gästanvändare i din Azure AD-klient tooprovide åtkomst tooAzure AD-tjänster och alla Azure-resurser som du gör tillgängliga.  

Det är gratis för bjuda in B2B-användare och tilldela dem tooan program i Azure AD. In too10 appar per gäst kan användar- och 3 grundläggande rapporter även för B2B-samarbete användare. Om din gästanvändare har lämpliga licenser tilldelade i hello partner Azure AD-klient, ska de licensieras med din egen samt.

Det krävs inte, men om du vill tooprovide åtkomstfunktioner toopaid Azure AD B2B gäst användarna måste ha licens med lämpliga licenser för Azure AD. En bjuda in klient med en Azure AD betald licens kan tilldela användarrättigheter för B2B-samarbete tooan ytterligare fem gästanvändare inbjuden toohello klient. Scenarier och information finns i [B2B-samarbete licensiering vägledning](active-directory-b2b-licensing.md).

## <a name="view-assigned-licenses"></a>Visa tilldelade licenser

En sammanfattning av tilldelade och tillgängliga licenser visas under **Azure Active Directory** &gt; **licenser** &gt; **alla produkter**.

![Visa licens sammanfattning](media/license-users-groups/view-license-summary.png)

En detaljerad lista över tilldelade användare och grupper är tillgänglig när du väljer en viss produkt. Hej **licensierade användare** listan visas alla användare som för närvarande använder en licens och om hello licens tilldelades direkt toohello användare eller om den har ärvts från en grupp.

![Visa information om licens](media/license-users-groups/view-license-detail.png)

På liknande sätt hello **licensierad grupper** listan visas alla grupper toowhich licenser har tilldelats. Välj en användare eller grupp tooopen hello **licenser** som visar alla licenser som tilldelats toothat objekt.

## <a name="remove-a-license"></a>Ta bort en licens

tooremove en licens gå toohello användaren eller gruppen och öppna hello **licenser** panelen. Välj hello licens och klicka på **ta bort**.

![Ta bort en licens](media/license-users-groups/remove-license.png)

Licenser som ärvs av hello användaren från en grupp kan inte tas bort direkt. I stället tar du bort hello användare från hello-grupp som de ska ärva hello-licens.


## <a name="next-steps"></a>Nästa steg
I den här snabbstarten du har lärt dig hur tooassign licenser toousers och grupper i Azure AD-katalog. 

Du kan använda hello följande länk tooconfigure prenumeration licenstilldelning i Azure AD från hello Azure-portalen.

> [!div class="nextstepaction"]
> [Tilldela licenser för Azure AD](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/LicensesMenuBlade/Overview) 