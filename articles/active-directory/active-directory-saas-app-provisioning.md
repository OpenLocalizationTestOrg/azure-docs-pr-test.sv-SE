---
title: "Automatisk SaaS app användaretablering i Azure AD | Microsoft Docs"
description: "En introduktion till hur du kan använda Azure AD för att automatiskt etablera avetablera och kontinuerligt uppdatera användarkonton över flera SaaS-program från tredje part."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 58c5fa2d-bb33-4fba-8742-4441adf2cb62
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/13/2017
ms.author: curtand
ms.openlocfilehash: 7cb780117d64d67449146b9757f8162e23e65d1e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="automate-user-provisioning-and-deprovisioning-to-saas-applications-with-azure-active-directory"></a>Automatisera användaren etablering och avetablering för SaaS-program med Azure Active Directory
## <a name="what-is-automated-user-provisioning-for-saas-apps"></a>Vad är automatisk användaretablering för SaaS-appar?
Azure Active Directory (Azure AD) kan du automatisera skapandet, underhållet och borttagningen av användaridentiteter i molnet ([SaaS](https://azure.microsoft.com/overview/what-is-saas/)) program som Dropbox, Salesforce, ServiceNow med mera.

**Här följer några exempel på vad den här funktionen kan du göra:**

* Automatiskt skapa nya konton i rätt SaaS-appar för nya användare när de ansluter till ditt team.
* Inaktivera konton från SaaS-appar automatiskt när personer lämnar oundvikligen teamet.
* Se till att identiteter i SaaS-appar hålls uppdaterade baserat på ändringar i katalogen.
* Etablera icke-användare objekt, till exempel grupper till SaaS-appar som stöder dessa.

**Automatisk användaretablering innehåller även följande funktioner:**

* Möjligheten att matcha befintliga identiteter mellan Azure AD och SaaS-appar.
* Anpassningsalternativ i Azure AD-hjälpen passar aktuella konfigurationer av SaaS-appar som din organisation använder för tillfället.
* Valfri e-postaviseringar för att etablera fel.
* Rapporterings- och aktivitet loggar för bättre övervakning och felsökning.

## <a name="why-use-automated-provisioning"></a>Varför använda Automatisk etablering?
Några vanliga motiveringen för att använda den här funktionen är:

* Att undvika kostnader, ineffektiviteter och handhavarfel vid manuell etablering processer.
* Att skydda din organisation genom att direkt ta bort användarens identitet från viktiga SaaS-appar när de lämnar organisationen.
* Enkelt importera ett bulk antal användare till ett visst SaaS-program.
* Om du vill få nyttan av din etablering köra lösningen från samma app åtkomstprinciper som du har definierat för Azure AD enkel inloggning.

## <a name="frequently-asked-questions"></a>Vanliga frågor och svar
**Hur ofta kan Azure AD för att skriva katalogändringar till SaaS-app?**

Azure AD som söker efter ändringar varje fem till tio minuter. Om appen SaaS returnerar flera fel (till exempel som i fallet med ogiltig administratörsautentiseringsuppgifter), kommer Azure AD gradvis att minska frekvensen på högst en gång per dag förrän felen har åtgärdats.

**Hur lång tid tar det för att etablera Mina användare?**

Inkrementella ändringar sker nästan omedelbart, men om du försöker att etablera de flesta av din katalog sedan det beror på antalet användare och grupper som du har. Liten kataloger tar bara några minuter, medelstor kataloger kan ta några minuter och mycket stora kataloger kan ta flera timmar.

**Hur kan jag spåra förloppet för den aktuella Etableringsjobbet?**

Du kan granska konto etablering rapporten under avsnittet rapporter för din katalog. Ett annat alternativ är att besöka fliken instrumentpanel för SaaS-program som du etablerar till och tittar du under avsnittet ”Integration Status” längst ned på sidan.

**Hur vet jag om användare ska kunna hämta etablerats korrekt?**

I slutet av etablering konfigurationen är guiden det ett alternativ för att prenumerera på e-postaviseringar för att etablera fel. Du kan också kontrollera rapporten etablering fel om du vill se vilka användare som inte gick att etableras och varför.

**Kan Azure AD skriva ändringar i SaaS-appen tillbaka till katalogen?**

För de flesta SaaS-appar kan etablering utgående endast, vilket innebär att användare skrivs från katalogen till programmet och ändringar av programmet kan inte skrivas tillbaka till katalogen. För [Workday](https://msdn.microsoft.com/library/azure/dn762434.aspx)dock etablering är inkommande endast, vilket innebär att användare som importeras till katalogen från Workday och likaså ändringar i katalogen inte skrivs tillbaka till Workday.

**Hur kan jag skicka feedback till teknikteamet?**

Kontakta oss via den [Azure Active Directory Feedbackforum](https://feedback.azure.com/forums/169401-azure-active-directory/).

## <a name="how-does-automated-provisioning-work"></a>Hur fungerar automatisk etablering arbete?
Azure AD etablerar användare till SaaS-appar genom att ansluta till att etablera slutpunkter som tillhandahålls av leverantören av tillämpningsprogrammet. Dessa slutpunkter kan Azure AD för att skapa, uppdatera och ta bort användare. Nedan finns en kort översikt över de olika steg som Azure AD tar för att automatisera etablering.

1. När du aktiverar etablering för ett program för första gången, utförs följande åtgärder:
   * Azure AD försöker matcha eventuella befintliga användare i SaaS-appen med sina motsvarande identiteter i katalogen. När en användare matchas, de är *inte* automatiskt aktiverad för enkel inloggning. För en användare får åtkomst till programmet, måste de uttryckligen tilldelas till appen i Azure AD antingen direkt eller via gruppmedlemskap.
   * Om du redan har angett vilka användare som ska tilldelas till programmet och Azure AD inte gick att hitta befintliga konton för dessa användare, etablera Azure AD nya konton för dem i programmet.
2. När den inledande synkroniseringen har slutförts enligt ovan, kommer Azure AD Kontrollera var tionde minut för följande ändringar:
   * Om nya användare har tilldelats till programmet (direkt eller via gruppmedlemskap), kommer de att vara etablerade ett nytt konto i SaaS-app.
   * Om en användare har tagits bort och sedan sitt konto i SaaS-appen kommer att markeras som inaktiverade (användarna aldrig fullständigt bort, vilket skyddar du förlorar data vid en felaktig konfiguration).
   * Om en användare nyligen har tilldelats till programmet och de redan har ett konto i SaaS-app, kontot kommer att markeras som aktiverad och vissa användaregenskaper kan uppdateras om de är inaktuella jämfört med katalogen.
   * Om en användares information (t.ex telefonnummer, arbetsplats och så vidare) har ändrats i katalogen, kommer också att uppdateras informationen i SaaS-program.

Mer information om hur attribut mappas mellan Azure AD och din SaaS-appar, finns i artikeln om [anpassa attributmappning](active-directory-saas-customizing-attribute-mappings.md).

## <a name="list-of-apps-that-support-automated-user-provisioning"></a>Lista över appar som stöder automatiserad etablering av användare
Alla ”aktuell” appar i Azure AD application gallery stöder automatisk användaretablering. [Lista över aktuella appar kan visas här.](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps?page=1&subcategories=featured)

För ett program med stöd för automatisk användaretablering, måste det först ange de nödvändiga slutpunkterna som tillåter för externa program att automatisera skapande, underhåll och borttagning av användare. Därför inte alla SaaS-appar som är kompatibla med den här funktionen. Azure AD-Utvecklingsteamet kommer sedan att kunna skapa en allokering koppling till apparna för appar som stöder detta, och detta verk prioriteras av behoven hos aktuella och potentiella kunder.

Kontakta Azure AD engineering team för att begära etablering support för ytterligare program, skicka ett meddelande via den [Azure Active Directory Feedbackforum](https://feedback.azure.com/forums/374982-azure-active-directory-application-requests/category/172035-user-provisioning).

## <a name="related-articles"></a>Relaterade artiklar
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)
* [Anpassa attributmappning för Användaretablering](active-directory-saas-customizing-attribute-mappings.md)
* [Skriva uttryck för attributmappning](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Omfångsfilter för Användaretablering](active-directory-saas-scoping-filters.md)
* [Använda SCIM för att aktivera automatisk etablering av användare och grupper från Azure Active Directory till program](active-directory-scim-provisioning.md)
* [Kontot etablering meddelanden](active-directory-saas-account-provisioning-notifications.md)
* [Lista över självstudier om hur du integrerar SaaS-appar](active-directory-saas-tutorial-list.md)

