---
title: "aaaAutomated SaaS app användaretablering i Azure AD | Microsoft Docs"
description: "En introduktion toohow som du kan använda Azure AD tooautomatically etablera avetablera och kontinuerligt uppdatera användarkonton över flera SaaS-program från tredje part."
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
ms.openlocfilehash: a1f3ecdd513e2b603f8ad9901e9f551b3b982b2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="automate-user-provisioning-and-deprovisioning-toosaas-applications-with-azure-active-directory"></a>Automatisera användaren etablering och borttagning tooSaaS program med Azure Active Directory
## <a name="what-is-automated-user-provisioning-for-saas-apps"></a>Vad är automatisk användaretablering för SaaS-appar?
Azure Active Directory (Azure AD) kan du skapa en tooautomate hello, underhåll och borttagningen av användaridentiteter i molnet ([SaaS](https://azure.microsoft.com/overview/what-is-saas/)) program som Dropbox, Salesforce, ServiceNow med mera.

**Här följer några exempel på vad den här funktionen kan du toodo:**

* Automatiskt skapa nya konton i hello rätt SaaS-appar för nya användare när de ansluter till ditt team.
* Inaktivera konton från SaaS-appar automatiskt när personer lämnar oundvikligen hello-teamet.
* Se till att hello identiteter i SaaS-appar hålls in toodate baserat på ändringar i hello directory.
* Etablera icke-användare objekt, till exempel grupper, tooSaaS appar som stöder dessa.

**Automatisk användaretablering innehåller också hello följande funktioner:**

* Hej möjlighet toomatch befintliga identiteter mellan Azure AD och SaaS-appar.
* Anpassning alternativ toohelp Azure AD passar hello aktuella konfigurationer av hello SaaS-appar som din organisation använder för tillfället.
* Valfri e-postaviseringar för att etablera fel.
* Rapporterings- och aktivitet loggar toohelp med övervakning och felsökning.

## <a name="why-use-automated-provisioning"></a>Varför använda Automatisk etablering?
Några vanliga motiveringen för att använda den här funktionen är:

* tooavoid hello kostnader, ineffektiviteter och handhavarfel vid manuell etablering processer.
* toosecure din organisation genom att direkt ta bort användarnas identiteter från key SaaS-appar när de lämnar hello organisation.
* tooeasily importera bulk antalet användare i ett visst SaaS-program.
* tooenjoy hello nyttan av etablering lösningen kör från hello samma åtkomstprinciper för appar som du har definierat för Azure AD enkel inloggning.

## <a name="frequently-asked-questions"></a>Vanliga frågor och svar
**Hur ofta skriva directory ändringar toohello SaaS app i Azure AD?**

Azure AD som söker efter ändringar var femte minut tooten. Om hello SaaS app returnerar flera fel (till exempel som hello fallet med ogiltig administratörsautentiseringsuppgifter), kommer Azure AD gradvis att försämra dess frekvens tooup tooonce per dag förrän hello fel har åtgärdats.

**Hur lång tid tar det tooprovision Mina användare?**

Inkrementella ändringar sker nästan omedelbart men om du försöker tooprovision de flesta för din katalog och sedan det beror på hello antalet användare och grupper som du har. Liten kataloger tar bara några minuter, medelstor kataloger kan ta några minuter och mycket stora kataloger kan ta flera timmar.

**Hur kan jag spåra hello fortskrider hello aktuella Etableringsjobbet?**

Du kan granska hello etablering rapport under hello rapportavsnittet i din katalog. Ett annat alternativ är toovisit hello instrumentpanelen fliken för hello SaaS-program som du etablerar till och tittar du under hello ”Integration Status” avsnittet hello nedre delen av hello-sidan.

**Hur vet jag om användarna inte tooget etablerats korrekt?**

Hello slutet av hello etablering guiden Konfigurera det är en alternativ toosubscribe tooemail meddelanden för att etablera fel. Du kan också kontrollera hello etablering fel rapporten toosee vilka användare misslyckades toobe etablerad och varför.

**Azure AD kan skriva ändringar från hello SaaS tillbaka toohello programkatalogen?**

För de flesta SaaS-appar kan etablering utgående endast, vilket innebär att användare skrivs från hello directory toohello program och ändringar från hello program inte kan skrivas tillbaka toohello directory. För [Workday](https://msdn.microsoft.com/library/azure/dn762434.aspx)dock etablering är inkommande endast, vilket innebär att användare som importerats till hello directory från Workday och likaså ändringar i hello directory inte skrivs tillbaka till Workday.

**Hur kan jag skicka feedback toohello teknikteamet?**

Kontakta oss via hello [Azure Active Directory Feedbackforum](https://feedback.azure.com/forums/169401-azure-active-directory/).

## <a name="how-does-automated-provisioning-work"></a>Hur fungerar automatisk etablering arbete?
Azure AD etablerar användare tooSaaS appar genom att ansluta tooprovisioning slutpunkter som tillhandahålls av leverantören av tillämpningsprogrammet. Dessa slutpunkter låta Azure AD tooprogrammatically skapa, uppdatera och ta bort användare. Nedan finns en kort översikt över hello olika steg för att Azure AD tar tooautomate etablering.

1. När du aktiverar etablering för ett program för hello första gången utförs hello följande åtgärder:
   * Azure AD försöker toomatch eventuella befintliga användare i hello SaaS-appen med sina motsvarande identiteter i hello directory. När en användare matchas, de är *inte* automatiskt aktiverad för enkel inloggning. För en toohave åtkomst toohello användarprogram måste de tilldelas toohello app i Azure AD, antingen direkt eller via gruppmedlemskap.
   * Om du redan har angett vilka användare som ska tilldelas toohello program och Azure AD misslyckas toofind befintliga konton för dessa användare, etablera nya konton för dem i hello program med Azure AD.
2. När hello inledande synkroniseringen har slutförts enligt ovan, kommer Azure AD Kontrollera var tionde minut för hello följande ändringar:
   * Om nya användare har tilldelats toohello program (direkt eller via gruppmedlemskap) kommer de att etablera ett nytt konto i hello SaaS-app.
   * Om en användare har tagits bort och sedan sitt konto i hello SaaS app kommer att markeras som inaktiverade (användarna aldrig fullständigt bort, vilket skyddar du från dataförlust i hello händelse av en felaktig konfiguration).
   * Om en användare nyligen har tilldelats toohello program och de redan har ett konto i hello SaaS app att kontot kommer att markeras som aktiverad och vissa användaregenskaper kan uppdateras om de är inaktuella jämfört med toohello directory.
   * Om en användares information (t.ex telefonnummer, arbetsplats och så vidare) har ändrats i hello directory, uppdatera informationen också i hello SaaS-program.

Mer information om hur attribut mappas mellan Azure AD och din SaaS-appar finns hello artikel på [anpassa attributmappning](active-directory-saas-customizing-attribute-mappings.md).

## <a name="list-of-apps-that-support-automated-user-provisioning"></a>Lista över appar som stöder automatiserad etablering av användare
Alla hello ”aktuell” appar i hello Azure AD application gallery stöder automatisk användaretablering. [hello lista över aktuella appar kan visas här.](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps?page=1&subcategories=featured)

För ett program toosupport automatisk användaretablering, måste det först ange hello nödvändiga slutpunkter som för externa program tooautomate hello skapande, underhåll och borttagning av användare. Därför inte alla SaaS-appar som är kompatibla med den här funktionen. För appar som stöder detta hello Azure AD-Utvecklingsteamet tas sedan kan toobuild en allokering connector toothose appar, och detta verk prioriteras av hello behoven hos aktuella och potentiella kunder.

toocontact hello Azure AD-tekniker team toorequest etablering stöd för fler program, så skicka ett meddelande via hello [Azure Active Directory Feedbackforum](https://feedback.azure.com/forums/374982-azure-active-directory-application-requests/category/172035-user-provisioning).

## <a name="related-articles"></a>Relaterade artiklar
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)
* [Anpassa attributmappning för Användaretablering](active-directory-saas-customizing-attribute-mappings.md)
* [Skriva uttryck för attributmappning](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Omfångsfilter för Användaretablering](active-directory-saas-scoping-filters.md)
* [Använda SCIM tooenable Automatisk etablering av användare och grupper från Azure Active Directory tooapplications](active-directory-scim-provisioning.md)
* [Kontot etablering meddelanden](active-directory-saas-account-provisioning-notifications.md)
* [Lista över självstudier om hur tooIntegrate SaaS-appar](active-directory-saas-tutorial-list.md)

