---
title: "Användningsvillkor för Azure Active Directory | Microsoft Docs"
description: "Användningsvillkoren för Azure AD låter dig och ditt företag tillhandahålla användningsvillkor till användarna av Azure AD-tjänsterna."
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: d55872ef-7e45-4de5-a9a0-3298e3de3565
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/14/2017
ms.author: billmath
ms.openlocfilehash: c01ea7c6a05c77244ab29fe6eec00ed1e49b6f7e
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/15/2017
---
# <a name="azure-active-directory-terms-of-use-feature-preview"></a>Användningsvillkorsfunktion för Azure Active Directory (förhandsversion)
Användningsvillkoren för Azure AD tillhandahåller en enkel metod som organisationer kan använda för att presentera information för slutanvändare.  Det gör att användarna kan se relevanta ansvarsfriskrivningar för juridiska krav eller efterlevnadskrav.

Användningsvillkoren för Azure AD presenteras i pdf-format.   Pdf-filen kan innehålla valfritt innehåll, som befintliga kontraktdokument, så att du kan samla in slutanvändaravtal under inloggningen.  Du kan använda användningsvillkoren för program och användargrupper eller om du har flera användningsvillkor för olika ändamål.

Resten av det här dokumentet beskriver hur du kommer igång med användningsvillkoren för Azure AD.  

## <a name="why-use-azure-ad-terms-of-use"></a>Varför ska man använda användningsvillkor för Azure AD?
Tycker du att det är svårt att få medarbetare eller gäster att godkänna dina användningsvillkor före de får åtkomst? Behöver du hjälp med att komma på vem som har eller inte har godkänt företagets användningsvillkor?  Användningsvillkoren för Azure AD tillhandahåller en enkel metod som organisationer kan använda för att presentera information för slutanvändare.  Det gör att de kan se relevanta ansvarsfriskrivningar för juridiska krav eller efterlevnadskrav.

Användningsvillkoren för Azure AD kan användas i följande scenarier:
-   Allmänna villkor för användningsvillkor för alla användare i din organisation.
-   Specifika användningsvillkor baserat på användarattribut (t.ex. läkare jämfört med sjuksköterskor eller inhemska jämfört med internationella medarbetare, som görs av [dynamiska grupper](https://azure.microsoft.com/updates/azure-active-directory-dynamic-membership-for-groups)).
-   Specifika användningsvillkor baserat på åtkomst till HBI-appar (high business impact), som Salesforce.


## <a name="prerequisites"></a>Krav
Så här konfigurerar du användningsvillkoren för Azure AD:

1. Logga in på Azure AD med en global administratör, säkerhetsadministratör eller administratör för villkorlig åtkomst för den katalog för vilken du vill konfigurera användningsvillkoren för Azure AD.
2. Kontrollera att katalogen har en Azure AD Premium P1-, P2-, EMS E3- eller EMS E5-prenumeration.  Om den inte har det ska du [hämta Azure AD Premium](active-directory-get-started-premium.md) eller [starta en utvärderingsperiod](https://azure.microsoft.com/trial/get-started-active-directory/).
3. Visa instrumentpanelen med användningsvillkoren för Azure AD på [https://aka.ms/catou](https://aka.ms/catou).

>[!IMPORTANT]
>Kontrollerna för principer för villkorlig åtkomst (inklusive användningsvillkoren) stöder inte tvingande för tjänstkonton.  Vi rekommenderar att du exkluderar alla tjänstkonton från principen för villkorlig åtkomst.

## <a name="add-company-terms-of-use"></a>Lägga till företagets användningsvillkor
När du har skapat dina användningsvillkor använder du följande metod för att lägga till dem.

### <a name="to-add-terms-of-use"></a>Så här lägger du till användningsvillkor
1. Gå till instrumentpanelen på [https://aka.ms/catou](https://aka.ms/catou)
2. Klicka på Add (Lägg till).</br>
![Lägga till användningsvillkor](media/active-directory-tou/tou12.png)
3. Ange **namn** för användningsvillkoren
4. Ange **Visningsnamn**.  Den här rubriken ser användarna när de loggar in.
5. **Bläddra** till dina färdigställda användningsvillkor i pdf-form och markera dem.  Den rekommenderade teckenstorlek är 24.
6. **Välj** ett språk för användningsvillkoren.  Via språkalternativet kan du ladda upp flera användningsvillkor, vart och ett med olika språk.  Versionen av användningsvillkoren som en användare ser baseras på deras inställningar för webbläsaren.
7. Välj antingen aktivera eller inaktivera för **Kräv att användarna expanderar användningsvillkoren**.  Om det här är aktiverat måste slutanvändarna läsa villkoren innan de accepterar dem.
8. Under **Villkorlig åtkomst** kan du **Framtvinga** de uppladdade användningsvillkoren genom att använda en mall eller en anpassad villkorlig åtkomstprincip.  Anpassade villkorliga åtkomstprinciper möjliggör detaljerade användningsvillkor för ett specifikt molnprogram eller grupp med användare.  Mer information finns i [configuring conditional access policies](active-directory-conditional-access-best-practices.md) (konfigurera villkorliga åtkomstprinciper)
9. Klicka på **Skapa**.
10. Om du har markerat en mall för villkorlig åtkomst visas en ny skärm där du kan anpassa certifikatutfärdarprincipen.
11. Nu ska du inte längre se dina nya användningsvillkor.</br>

![Lägga till användningsvillkor](media/active-directory-tou/tou3.png)

## <a name="delete-terms-of-use"></a>Ta bort användningsvillkor
Du kan ta bort gamla användningsvillkor på följande sätt:

### <a name="to-delete-terms-of-use"></a>Så här tar du bort användningsvillkor
1. Gå till instrumentpanelen på [https://aka.ms/catou](https://aka.ms/catou)
2. Välj de användarvillkor du vill ta bort.
3. Klicka på **Ta bort**.
4. Nu ska du inte längre se dina nya användningsvillkor.


## <a name="audit-terms-of-use"></a>Granska användningsvillkor
Användningsvillkoren för Azure AD tillhandahåller lättanvänd granskning så att du kan se vem som har godkänt dem, och när.  Så här kommer du igång med granskning av följande steg:

### <a name="to-audit-terms-of-use"></a>Så här granskar du användningsvillkor
1. Gå till instrumentpanelen på [https://aka.ms/catou](https://aka.ms/catou)
2. Klicka på Granska händelse.</br>
![Granska händelse](media/active-directory-tou/tou8.png)
3.  På skärmen med Azure AD-granskningsloggar kan du filtrera informationen med listrutorna som finns för att rikta in dig mot specifik granskningslogginformation.
![Granska händelse](media/active-directory-tou/tou9.png)
4.  Du kan också ladda ned informationen i en .csv-fil och använda den lokalt.

## <a name="what-users-see"></a>Det här ser användarna
Användare som ingår i området ser följande när användningsvillkoren har skapats och tillämpats.  De ser de här skärmbilderna när de loggar in.
-   Den bästa metoden är att använda teckensnitt i storlek 24 i PDF-filen.
![Granska händelse](media/active-directory-tou/tou10.png)
-   Så här ser det ut på mobila enheter</br></br>
![Granska händelse](media/active-directory-tou/tou11.png)

### <a name="review-terms-of-use"></a>Granska användningsvillkoren
Användare kan granska och se användningsvillkoren som de har godkänt.  Detta kan göras på följande sätt:

1. Navigera till och logga in på [https://myapps.microsoft.com](https://myapps.microsoft.com).
2. Klicka på ditt namn i övre högra hörnet och välj **Profil** i listrutan.
![Profil](media/active-directory-tou/tou14.png)

3. Klicka på **Granska användningsvillkoren** på din profil.
![Granska händelse](media/active-directory-tou/tou13a.png)

4.  Därifrån kan du granska användningsvillkoren som du har accepterat. 


## <a name="additional-information"></a>Ytterligare information
Följande information är något som du bör känna till, och det kan hjälpa till med användningsvillkor.

Användare inom området behöver logga ut och logga in för att uppfylla en ny princip om:
 - en villkorlig åtkomstprincip är aktiverad för ett användningsvillkor
 - eller andra användningsvillkor skapas

Det beror på att principer för villkorlig åtkomst börjar gälla omedelbart. När detta händer börjar administratören se ”sorgsna moln” eller "Azure AD-tokenärenden". Administratören måste logga ut och logga in igen för att uppfylla den nya principen.





## <a name="frequently-asked-questions"></a>Vanliga frågor och svar

**F: Hur ser jag om/när en användare har godkänt användningsvillkoren?**</br>
S: En användare som godkänner användningsvillkoren skrivs in i spårningsloggen. Du kan söka i Azure AD-spårningsloggen för att se resultatet.  

**F: Behöver användarna godkänna användningsvillkoren igen om de ändras?**</br>
S: Ja, en administratör kan ändra användningsvillkoren och de nya villkoren måste godkännas.

**F: Kan användningsvillkor ha stöd för flera språk?**</br>
S: Nej, för närvarande går det inte att ha flera språk i ett användningsvillkor.  Men du kan rikta dig mot en grupp (användningsvillkoren i Frankrike exempelvis skiljer sig från dem för Storbritannien). 

**F: När tillämpas användningsvillkoren?**</br>
S: Användningsvillkoren tillämpas under inloggningen.

**F: Vilka program kan jag rikta användningsvillkor mot?**</br>
S: Du kan skapa en princip för villkorlig åtkomst för företagsprogram som använder modern autentisering.  Mer information finns i [enterprise applications](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-view-azure-portal) (företagsprogram).

**F: Kan jag lägga till flera användningsvillkor till en viss användare eller app?**</br>
S: Ja, genom att skapa flera principer för villkorlig åtkomst för dessa grupper eller appar. Om en användare som ingår i omfånget för flera användningsvillkor godkänner de ett användningsvillkor i taget.
 
**F: Vad händer om en användare nekar användningsvillkoren?**</br>
S: Användare blockeras från åtkomst till programmet. Användaren måste logga ut och in igen och acceptera villkoren för att få åtkomst.