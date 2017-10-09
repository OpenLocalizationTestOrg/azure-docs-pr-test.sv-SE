---
title: "Azure AD Connect-synkronisering: Förstå användare och kontakter | Microsoft Docs"
description: "Beskriver användare och kontakter i Azure AD Connect-synkronisering."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 8d204647-213a-4519-bd62-49563c421602
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: markvi;andkjell
ms.openlocfilehash: 4d80648c53a2981eb2626338913ed2282423f183
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-understanding-users-and-contacts"></a>Azure AD Connect-synkronisering: Förstå användare och kontakter
Det finns flera olika skäl varför har du flera Active Directory-skogar och det finns flera olika distributionstopologier. Vanliga modeller innehåller en konto-resurs-distribution och GAL sync'ed skogar efter en fusion & förvärv. Men även om det finns endast modeller, hybridmodeller är vanliga samt. hello standardkonfigurationen i Azure AD Connect-synkronisering förutsätter inte någon särskild modell men beroende på hur användaren matchar valdes i installationsguiden för hello olika beteenden kan observeras.

I det här avsnittet lär du dig hur hello standardkonfigurationen ska fungera i vissa topologier. Vi går igenom hello konfiguration och hello Redigeraren för regler för synkronisering kan vara används toolook hello-konfigurationen.

Det finns några allmänna regler hello konfiguration utgår från:

* Oavsett vilken ordning vi importera från hello datakälla Active kataloger ska hello slutresultatet alltid vara hello samma.
* Ett aktivt konto kommer alltid att bidra inloggningsinformation, inklusive **userPrincipalName** och **sourceAnchor**.
* Ett inaktiverat konto bidrar userPrincipalName och sourceAnchor, såvida det inte är en länkad postlåda, om det finns inga aktiva konton toobe hittades.
* Ett konto med en länkad postlåda ska aldrig användas för userPrincipalName och sourceAnchor. Det förutsätts att ett aktivt konto kommer att hittas senare.
* En kontaktobjekt vara etablerade tooAzure AD som en kontakt eller som en användare. Du verkligen vet inte förrän alla källa Active Directory-skogar har bearbetats.

## <a name="contacts"></a>Contacts
Med kontakter som representerar en användare i en annan skog är vanliga när en fusion & förvärv där en GALSync lösning bryggning två eller flera Exchange-skogar. hello kontaktobjekt Koppla alltid från hello connector utrymme toohello metaversum med hello e-attributet. Om det finns redan en kontaktobjekt eller användarobjekt med hello samma e-postadress, hello objekt är sammankopplade. Detta är konfigurerat i hello regeln **i från AD-ansluta Kontakta**. Det finns också en regel med namnet **i från AD – kontakta vanliga** med ett attribut flödet toohello metaversum-attribut **sourceObjectType** med hello konstant **Kontakta**. Den här regeln har mycket låg prioritet så om alla användarobjekt är domänansluten toohello samma metaversumobjekt och sedan hello regeln **i från AD-användaren vanliga** bidrar hello värdet toothis användarattribut. Med den här regeln har det här attributet hello värdet kontakta om ingen användare har anslutit och hello värdet användaren om minst en användare har hittats.

Hello utgående regel för att etablera en objektet tooAzure AD **ut tooAAD – kontakta ansluta** skapar en kontaktobjekt om hello attribut för metaversum **sourceObjectType** har angetts för**kontakta** . Om det här attributet har angetts för**användare**, sedan hello regeln **ut tooAAD – användare ansluta** skapar ett användarobjekt i stället.
Det är möjligt att ett objekt höjs från att kontakta tooUser när flera källa Active kataloger importeras och synkroniseras.

Till exempel i en topologi med GALSync hittar vi kontaktobjekt för alla i hello andra skogen när vi importerar hello första skogen. Detta kommer mellanlagra nya kontaktobjekt i hello AAD-koppling. När vi senare importera och synkronisera hello andra skogen kommer att hitta hello verkliga användare och Anslut dem toohello befintliga metaversum-objekt. Vi tar sedan bort hello kontaktobjekt i AAD och skapa ett nytt användarobjekt i stället.

Om du har en topologi där användare representeras som kontakter, kontrollera att du väljer toomatch användare på hello e-postattribut i hello installationsguiden. Om du väljer ett annat alternativ att du har ett beroende konfiguration ordning. Kontaktobjekt ansluta till alltid på hello e-postattribut men användarobjekt kommer bara att ansluta till på hello e-postattribut om det här alternativet har valts i hello installationsguiden. Du kan sedan hamna med två olika objekt i hello metaversum med hello samma e-postattribut om hello kontaktobjekt importerades innan hello användarobjektet. Under exporten tooAzure AD genereras ett fel. Det här beteendet är inbyggt och visar att det finns felaktiga data eller den hello topologin kunde inte identifieras korrekt under installationen av hello.

## <a name="disabled-accounts"></a>Inaktiva konton
Inaktiverade konton synkroniseras samt tooAzure AD. Inaktiva konton är vanliga toorepresent resurser i Exchange, till exempel konferensrum. hello undantaget är användare med en länkad postlåda; som tidigare nämnts etablerar dessa aldrig ett konto tooAzure AD.

hello antagandet är att om ett inaktiverat konto hittas, och sedan vi kommer inte att hitta active ett annat konto senare och hello-objektet är etablerad tooAzure AD med hello userPrincipalName och sourceAnchor hittades. Aktiva användarkonton i fall kommer att ansluta till toohello samma metaversumobjekt sedan dess userPrincipalName och sourceAnchor kommer att användas.

## <a name="changing-sourceanchor"></a>Ändra sourceAnchor
När ett objekt har exporterats är tooAzure AD då inte tillåtna toochange hello sourceAnchor längre. När hello objekt har varit exporterade hello metaversum-attribut **cloudSourceAnchor** anges med hello **sourceAnchor** värdet som godkänns av Azure AD. Om **sourceAnchor** ändras och inte matchar **cloudSourceAnchor**, hello regeln **ut tooAAD – användare ansluta** hello fel genereras **attributet sourceAnchor har ändrats**. I det här fallet hello konfigurationen eller måste korrigeras så hello samma sourceAnchor finns i hello metaversum igen innan du kan synkronisera hello objektet igen.

## <a name="additional-resources"></a>Ytterligare resurser
* [Azure AD Connect Sync: Anpassa synkroniseringsalternativ](active-directory-aadconnectsync-whatis.md)
* [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md)

