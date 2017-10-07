---
title: "aaaUser etablering hantering för företagsappar i hello Azure Active Directory | Microsoft Docs"
description: "Lär dig hur toomanage användarkontot etablering för företagsappar som använder hello Azure Active Directory"
services: active-directory
documentationcenter: 
author: asmalser
manager: femila
editor: 
ms.assetid: 34ac4028-a5aa-40d9-a93b-0db4e0abd793
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/26/2017
ms.author: asmalser
ms.reviewer: asmalser
ms.openlocfilehash: a613f844c8f51e04b92e62b488313a78ab85f7ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="managing-user-account-provisioning-for-enterprise-apps-in-hello-azure-portal"></a>Hantera användarkonto etablering för företagsappar i hello Azure-portalen
Den här artikeln beskriver hur toouse hello [Azure-portalen](https://portal.azure.com) toomanage automatisk användarkonto etablering och avetablering för program som stöder detta, särskilt de som har lagts till från hello ”aktuella” kategori Hej [Azure Active Directory-programgalleriet](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery). toolearn mer om automatisk konto användaretablering och hur det fungerar, se [automatisera Användaretablering och avetablering tooSaaS program med Azure Active Directory](active-directory-saas-app-provisioning.md).

## <a name="finding-your-apps-in-hello-portal"></a>Hitta dina appar i hello-portalen
Alla program som är konfigurerade för enkel inloggning i en katalog med en directory-administratör med hjälp av hello [Azure Active Directory-programgalleriet](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery), kan visas och hanteras i hello [Azure-portalen](https://portal.azure.com). hello program finns i hello **fler tjänster** &gt; **företagsprogram** på hello portal. Enterprise-appar är appar som har distribuerats och används i din organisation.

![Företagsprogram bladet][0]

Du väljer hello **alla program** länken hello vänster visar en lista över alla appar som har konfigurerats, inklusive appar som har lagts till från hello-galleriet. Markera en app laddas hello resursbladet för appen, där rapporter kan visas för appen och en rad olika inställningar som kan hanteras.

Användarkontot provisioning-inställningar kan hanteras genom att välja **etablering** hello vänster.

![Programmet resursbladet][1]

## <a name="provisioning-modes"></a>Etablering lägen
Hej **etablering** bladet börjar med en **läge** menyn som visar vilka etablering lägen stöds för ett företagsprogram och låter dem toobe konfigurerats. hello tillgängliga alternativ inkluderar:

* **Automatisk** -det här alternativet visas om Azure AD stöder automatisk API-baserad etablering och/eller ta bort etableringen av användarprogram konton toothis. Om du väljer det här läget visas ett gränssnitt som hjälper administratörer via konfigurerar Azure AD tooconnect toohello programmets Användarhantering API, skapa mappningar av åtgärdskonto och arbetsflöden som definierar hur användarkonton ska flöda mellan Azure AD och hello appen och hantera hello Azure AD allokering-tjänsten.
* **Manuell** -det här alternativet visas om Azure AD inte stöder automatisk etablering av användarprogram konton toothis. Det här alternativet innebär att kontot användarposter lagras i programmet hello måste hanteras med hjälp av en extern process, baserat på hello användaren hantering och etablering funktioner som tillhandahålls av programmet (som kan omfatta etablering av SAML-in).

## <a name="configuring-automatic-user-account-provisioning"></a>Konfigurera automatisk användarens konto-etablering
Du väljer hello **automatisk** visas en skärm som är indelat i fyra avsnitt:

### <a name="admin-credentials"></a>Admin-autentiseringsuppgifter
Detta är där hello autentiseringsuppgifter som krävs för Azure AD tooconnect toohello programmets Användarhantering API anges. hello-indata som krävs varierar beroende på programmet hello. toolearn om hello autentiseringstyper och krav för specifika program finns hello [configuration självstudierna för det specifika programmet](active-directory-saas-app-provisioning.md#list-of-apps-that-support-automated-user-provisioning).

Du väljer hello **Testanslutningen** knappen kan du tootest hello autentiseringsuppgifter genom att låta Azure AD försöker tooconnect toohello app etablering app med hello angivna autentiseringsuppgifter.

### <a name="mappings"></a>Mappningar
Det är där administratörer kan visa och redigera vad användaren attribut flödet mellan Azure AD och hello målprogrammet när användarkonton etablerades eller uppdaterades.

Det finns en förkonfigurerad uppsättning mappningar mellan användarobjekt i Azure AD och användarobjekt för varje SaaS-app. Vissa appar hantera andra typer av objekt, till exempel grupper eller kontakter. Att välja ett av dessa mappningar i hello tabellen visar hello mappning editor toohello höger, där de kan visas och anpassat.

![Programmet resursbladet][2]

Anpassningar som stöds är:

* Aktivera och inaktivera mappningar för specifika objekt, till exempel hello Azure AD användaren objektet toohello SaaS appens användarobjektet.
* Ändra vilka attribut som flödar från hello Azure AD användaren objektet toohello appens användarobjektet. Mer information om attributmappning finns [förstå mappning attributtyper](active-directory-saas-customizing-attribute-mappings.md#understanding-attribute-mapping-types).
* Filter hello etablering åtgärder som Azure AD utför hello riktad program. I stället för med Azure AD fullt-Synkronisera objekt, kan du begränsa hello-åtgärder som utförs. Till exempel genom att bara välja **uppdatering**, Azure AD endast uppdateringar befintliga konton i ett program och inte skapa nya. Genom att bara välja **skapa**, Azure endast skapar nya användarkonton men inte uppdatera befintliga. Den här funktionen kan administratörer toocreate olika mappningar för kontot skapa och uppdatera arbetsflöden.

### <a name="settings"></a>Inställningar
Det här avsnittet kan administratörer toostart och stoppa hello Azure AD-etablering tjänsten för hello valt program, samt om du vill rensa hello etablering cachelagra och starta om hello-tjänsten.

Om etablering har aktiverats för hello första gången för ett program, aktivera hello-tjänsten genom att ändra hello **Status för etablering** för**på**. Detta medför hello Azure AD etablering service tooperform en inledande synkronisering, där det står hello-användare som är tilldelade i hello **användare och grupper** avsnittet frågar hello målprogrammet för dem och utför sedan hello etablering åtgärder som definierats i hello Azure AD **mappningar** avsnitt. Under den här processen lagrar hello etableras cachelagrade data om vilka användarkonton som hanteras så att icke-hanterade konton i hello målprogram som fanns inte i omfånget för tilldelning inte påverkas av avetablering åtgärder. När hello synkroniseras inledande synkronisering, hello etableras automatiskt användar- och gruppobjekt på tio minuter.

Ändra hello **Status för etablering** för**av** bara pausar hello etablering service. I det här tillståndet Azure inte skapa, uppdatera eller ta bort användare eller gruppobjekt i hello app. Om du ändrar hello tillstånd tillbaka tooon resulterar hello service toopick där den avbröts.

Du väljer hello **Rensa aktuell status och starta om synkroniseringen** kryssrutan och spara stoppar Hej etablering service, Dumpar hello cachelagrade data om vilka Azure AD-konton som hanterar, startar om hello tjänster och utför hello inledande synkroniseringen igen. Det här alternativet kan administratörer toostart hello etablering distributionsprocessen över igen.

### <a name="synchronization-details"></a>Information om synkronisering
Det här avsnittet innehåller ytterligare information om hello driften av hello etableras, inklusive hello första och sista gånger hello etablering tjänsten kördes mot hello program och hur många objekt för användare och grupper hanteras.

Länkar finns toohello **etablering aktivitetsrapport**, som innehåller en logg över alla användare och grupper som skapas, uppdateras och tas bort mellan Azure AD och hello målprogrammet och toohello **etablering fel rapporten** som ger mer detaljerade felmeddelanden för användare och grupp objekt som misslyckade toobe läsa, skapa, uppdateras eller tas bort. 

##<a name="feedback"></a>Feedback

Vi hoppas att du som din Azure AD-upplevelse. Skriv ned hello feedback kommer! Publicera din feedback och förslag på förbättringar i hello **administrationsportalen** avsnitt i vår [Feedbackforum](https://feedback.azure.com/forums/169401-azure-active-directory/category/162510-admin-portal).  Vi få är taggade om hur du skapar nya nya produkter varje dag, och använda vägledning-tooshape och definiera vad vi bygga härnäst.


[0]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-blade.PNG
[1]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-provisioning.PNG
[2]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-provisioning-mapping.PNG
