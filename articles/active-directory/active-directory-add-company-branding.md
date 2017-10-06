---
title: "aaaAdd företagsanpassning tooyour inloggnings- och åtkomstpanelsidan"
description: "Lär dig hur tooadd en företagets företagsanpassning toohello Azure inloggning sida och hello åt åtkomstpanelsidan"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: f74621b4-4ef0-4899-8c0e-0c20347a8c31
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/23/2017
ms.author: curtand
ms.openlocfilehash: b1c6442028393552bff3d380312c8cd1bfda5d31
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-company-branding-tooyour-sign-in-and-access-panel-pages"></a>Lägga till företagsanpassning tooyour inloggnings- och åtkomstpanelsidan
tooavoid förvirring vill många företag tooapply ett konsekvent utseende på alla hello webbplatser och tjänster som de hanterar. Azure Active Directory erbjuder den här funktionen genom att låta dig toocustomize hello utseendet på hello följande webbsidor med företagets logotyp och egna färgscheman:

* **Inloggningssidan** -detta är hello sida som visas när du loggar in tooOffice 365 eller andra webbaserade program som använder Azure AD som identitetsprovider. Du interagera med den här sidan antingen under en identifiering av startsfär eller tooenter dina autentiseringsuppgifter. hello identifieringen av Startsfären gör hello system tooredirect federerade användare tootheir lokala STS (till exempel AD FS).
* **Åtkomstpanelsidan** - hello åtkomstpanelen är en webbaserad portal där du tooview och starta hello molnbaserade program Azure AD-administratör har gett dig åtkomst till. tooaccess hello åtkomstpanelen, Använd hello följande URL: [https://myapps.microsoft.com](https://myapps.microsoft.com).

Det här avsnittet beskrivs hur du kan anpassa hello inloggningssidan och åtkomstpanelsidan hello.

> [!NOTE]
> * Företagsanpassning är en funktion som är bara tillgängligt om du har uppgraderat toohello Premium eller Basic-versionen av Azure Active Directory eller använder Office 365. Mer information finns i [Azure Active Directory-versioner](active-directory-editions.md).
> * Azure Active Directory Premium och Basic är tillgängliga för kunder i Kina hello globala instansen av Azure Active Directory. Azure Active Directory Premium och Basic stöds inte för närvarande i hello Microsoft Azure-tjänsten som drivs av 21Vianet i Kina. Mer information kontaktar du oss på hello [Azure Active Directory-forumet](https://feedback.azure.com/forums/169401-azure-active-directory/).
>
>

## <a name="customizing-hello-sign-in-page"></a>Anpassa hello-inloggningssida
Om du behöver webbläsarbaserad åtkomst tooyour molnappar och tjänster som din organisation prenumererar på, använder du normalt hello-inloggningssida.

Om du har tillämpat ändringar tooyour inloggningssidan kan ta det upp tooan timme för hello ändringar tooappear.

En företagsanpassad inloggningssida visas bara när du besöker en tjänst med en URL som är specifik för en klientorganisation, till exempel https://outlook.com/**contoso**.com eller https://mail.**contoso**.com.

När du besöker en tjänst med en URL som inte är specifik för en klientorganisation (t.ex. https://mail.office365.com) visas en icke företagsanpassad inloggningssida. I så fall visas din anpassning först när du har angett ditt användar-ID eller då du har valt en användarikon.

> [!NOTE]
> * Domännamnet måste visas som ”aktiv” i hello **Active Directory** > **Directory** > **domäner** avsnitt i hello klassiska Azure-portalen Om du har konfigurerat anpassningen.
> * Inloggningssidan företagsanpassning överföra inte toohello konsumenten inloggningssidan för Microsoft. Om du loggar in med ett personligt microsoftkonto kan du se en företagsanpassad lista över användarikoner som återges av Azure AD, men hello anpassning av din organisation gäller inte toohello Microsoft inloggningssidan.
>
>

Om du vill tooshow ditt företags varumärke, färger och andra anpassningsbara element på den här sidan, finns i följande bilder toounderstand hello skillnaden mellan två hello-upplevelser hello.

Hej följande skärmbild visar och exempel för hello Office 365-inloggningssidan på en stationär dator **innan** en anpassning:

![Office 365-inloggningssida före anpassning][1]

Hej följande skärmbild visar och exempel för hello Office 365-inloggningssidan på en stationär dator **när** en anpassning:

![Office 365-inloggningssida efter anpassning][2]

hello följande skärmbild visar ett exempel på hello Office 365-inloggningssidan på en mobil enhet **innan** en anpassning:

![Office 365-inloggningssida före anpassning][3]

hello följande skärmbild visar ett exempel på hello Office 365-inloggningssidan på en mobil enhet **när** en anpassning:

![Office 365-inloggningssida efter anpassning][4]

När du ändrar storlek på ett webbläsarfönster Hej stor bild, som hello som visas tidigare beskärs ofta tooaccommodate olika skärmproportioner. Med detta i åtanke bör du försöka tookeep hello viktiga visuella element i hello bild så att de alltid visas i hello övre vänstra hörnet (längst upp till höger för höger-till-vänster-språk). Detta är viktigt eftersom storleksändringar vanligtvis sker från hello nedre högra hörnet kommer mot hello översta vänstra eller hello botten mot hello upp.

hello följande bild visar hur hello bilden beskärs när hello webbläsaren är storleksändrade toohello vänster:

![][6]

Här är hur den visas när hello webbläsaren har storleksändrats mot hello upp:

![][7]

## <a name="what-elements-on-hello-page-can-i-customize"></a>Vilka element på sidan hello kan jag Anpassa?
Du kan anpassa följande element på inloggningssidan för hello hello:

![][5]

| Sidelement | Plats på hello sida |
|:--- | --- |
| Banderollslogotyp |Visa hello längst upp till höger i hello-sidan. Ersätter hello logotypen hello målplatsen du loggar in toodisplays (till exempel. Office 365 eller Azure). |
| Stor bild/bakgrundsfärg |Visa hello till vänster i hello-sidan. Ersätter hello avbildningen hello målplatsen du loggar in toodisplays. hello bakgrundsfärgen kan visas i stället hello stora bilden vid anslutningar med låg bandbredd eller på smala skärmar. |
| Håll mig inloggad |Visas under hello lösenordsrutan. |
| Text på inloggningssidan |Visas ovanför sidfoten hello när du behöver tooconvey användbar information innan du loggar in med ett arbets- eller skolkonto. Du kanske till exempel tooinclude hello phone nummer tooyour att supportavdelningen eller ett juridiskt meddelande. |

> [!NOTE]
> Alla element är valfria. Om du anger en Banderollslogotyp men ingen stor bild visar hello inloggningssida visas din logotyp och hello bild för hello mål (det vill säga hello Kalifornien för Office 365 motorväg bild).
>
>

På sidan logga in hello **Håll mig inloggad** checkbox tillåter en användare tooremain inloggad när de stänga och öppna webbläsaren igen. Den påverkar inte sessionens längd. Du kan dölja hello kryssruta på hello Azure Active Directory-inloggningssida.

Om kryssrutan hello visas beror på hello-inställningen **Dölj KMSI**.

![][9]

toohide Hej kryssrutan, konfigurera den här inställningen för**Hidden**.

> [!NOTE]
> Vissa funktioner i SharePoint Online och Office 2010 är beroende av användare kan toocheck den här rutan. Om du konfigurerar den här inställningen toohidden kan användarna se ytterligare och oväntat prompter toosign i.
>
>

Du kan också lokalisera alla element på den här sidan. När du har konfigurerat en standarduppsättning med anpassningselement kan du konfigurera fler versioner för olika språk. Du kan också blanda och matcha olika element. Du kan till exempel:

* Skapa en förvald stor bild som passar alla kulturer och sedan skapa specifika versioner för engelska och franska. När du ställer in din webbläsare tooone av dessa språk visas hello specifika bilden, medan hello standardbilden visas för andra språk.
* Konfigurera olika logotyper för din organisation (t.ex. japanska och hebreiska versioner).

## <a name="access-panel-page-customization"></a>Anpassning av åtkomstpanelsidan
Hej åtkomstpanelsidan är i grunden en Portalsida för snabb åtkomst toohello molnappar som du har beviljats åtkomst till tooby administratören. På den här sidan visas dina appar som klickbara programpaneler.

hello följande skärmbild visar ett exempel på en åtkomstpanelsida efter anpassningen.

![][8]

## <a name="configure-your-directory-with-company-branding"></a>Konfigurera katalogen med företagsinformation
Du kan konfigurera en standarduppsättning med anpassningsbara element för varje katalog i hello klassiska Azure-portalen. När hello standardinställningarna har sparats kan en administratör kan lägga till lokaliserade versioner av varje element för olika språk / språk. Alla anpassningsbara element är valfria.

Till exempel om du konfigurerar en standardbanderollslogotyp men ingen stor bild visas hello-inloggningssida din logotyp hello övre högra hörnet. Men hello standard illustration av hello platsen visas.

Tänk dig hello följande konfiguration:

* En standardbanderollslogotyp och text på engelska på en inloggningssida
* Text på tyska på en språkspecifik inloggningssida

Om din språkinställning är tyska, hämta hello standard Banderollslogotyp men hello tyska texten.

Medan tekniskt sett kan du konfigurera olika uppsättningar för varje språk som stöds av Azure AD, rekommenderar vi att du behåller hello antalet variationer låg för underhåll och prestandaskäl.

> [!IMPORTANT]
> Yammer matchar inte visa hello Azure AD-märkta inloggningssidan förrän efter hello användaren loggar in. hello användaren ser hello generiska Office 365-inloggningssida först och sedan hello märkta sida efter.   
 
 
**tooadd företagets företagsanpassning tooyour directory, utföra hello följande steg:**

1. Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com) som administratör för hello directory du vill toocustomize.
2. Välj hello-katalog som du vill toocustomize.
3. Klicka i hello verktygsfältet hello längst upp **konfigurera**.
4. Klicka på **Anpassa profilering**.
5. Ändra hello-element som du vill toocustomize. Alla fält är valfria.
6. Klicka på **Spara**.

Det kan ta upp tooan timme innan nya ändringar gjorts toohello inloggningssidan företagsanpassning tooappear.

**tooadd språkspecifik företagsanpassning, utför följande steg hello:**

1. Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com) som administratör för hello directory du vill toocustomize.
2. Välj hello-katalog som du vill toocustomize.
fs3. Klicka i hello verktygsfältet hello längst upp **konfigurera**.
4. Klicka på **Anpassa profilering**.
5. Klicka på **Lägg till företagsanpassning för ett specifikt språk**.
6. Välj hello språk du toocustomize hello logotypen för och klicka sedan på **nästa**.
7. Redigera endast hello element som du vill tooconfigure språkspecifika åsidosättningar. Alla fält är valfria. Om ett fält lämnas tomt sedan hello anpassade standardvärdet visas i stället (eller hello Microsofts standardvärde om anpassade standard inte har konfigurerats).
8. Klicka på **Spara**.

**tooremove företagsanpassning från katalogen, utför följande steg hello:**

1. Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com) som administratör för hello directory du vill toocustomize.
2. Välj hello-katalog som du vill toocustomize.
3. Klicka i hello verktygsfältet hello längst upp **konfigurera**.
4. Klicka på **Anpassa profilering**.
5. Hello anpassa profilering på sidan Välj **redigera befintliga anpassningsinställningar** och gå sedan toohello nästa sida.
6. Beroende på vilka element du vill tooremove, gör något av följande hello:

    a. Under **Banderollslogotyp** väljer du **Ta bort den uppladdade logotypen**.

    b. Under **Ikonlogotyp** väljer du **Ta bort den uppladdade logotypen**.

    c. Ta bort hello texten från alla textrutor.

    d. Klicka på **Next**.

    e. Ta bort hello texten från alla textrutor.
7. Klicka på **spara** tooremove hello element.
8. Om det behövs klickar du på **anpassa profilering** igen och upprepa stegen för all språkspecifik anpassning som behöver toobe bort.
    Alla inställningar för företagsanpassning har tagits bort när du klickar på **anpassa profilering** och se hello **anpassa Standardanpassning** formulär med några konfigurerade inställningar.

## <a name="testing-and-examples"></a>Testning och exempel
Vi rekommenderar att du experimenterar med en testklient innan du gör ändringar i produktionsmiljön.

**tooverify om din företagsanpassning har tillämpats:**

1. Öppna en InPrivate- eller Incognito-webbläsarsession.
2. Besök https://outlook.com/contoso.com och Ersätt contoso.com med hello-domän som du har anpassat.

Detta fungerar även med domäner som ser ut som contoso.onmicrosoft.com.

toohelp du skapa effektiva anpassningar uppsättningar, har vi anpassat följande två fiktiva inloggningssidor hello:

* [http://aka.ms/aaddemo001](http://aka.ms/aaddemo001)
* [http://aka.ms/aaddemo002](http://aka.ms/aaddemo002)

tootest hello språkspecifika inställningarna du behöver toomodify hello standardspråkinställningarna i ditt webbprogram webbläsare tooa språk som du har angett i anpassningen. I Internet Explorer kan du konfigurera detta i hello **Internetalternativ** menyn.

## <a name="customizable-elements"></a>Anpassningsbara element
Vissa anpassningsbara element i Azure AD har flera användningsscenarier. Du kan konfigurera en företagslogotyp en gång per katalog och används på både hello-inloggning och åtkomstpanelsidan. Vissa anpassningsbara element är specifika endast toohello-inloggningssida. hello följande tabell innehåller information för hello olika anpassningsbara elementen.

| Namn | Beskrivning | Villkor | Rekommendationer |
| --- | --- | --- | --- |
| Banderollslogotyp |Hej Banderollslogotyp visas på inloggningssidan för hello och hello åtkomstpanelen. |<p>JPG eller PNG</p><p>60 × 280 bildpunkter</p><p>10 kB</p> |<p>Använd organisationens hela logotyp (inklusive grafik och text)</p><p>Se till att den under 30 bildpunkter hög tooavoid att rullningslistor visas på mobila enheter</p><p>Se till att den inte är större än 4 kB</p><p>Använd en transparent PNG (förutsätt inte att hello inloggningssidan alltid har en vit bakgrund)</p> |
| Ikonlogotyp |(för närvarande inte används i hello-inloggningssida) I framtida hello kanske den här texten används tooreplace hello Allmänt ”arbets- eller skolkonto” grafik på olika ställen i hello upplevelse. |<p>JPG eller PNG</p><p>120 × 120 bildpunkter</p><p>10 kB</p> |<p>Håll det enkelt (ingen liten text) som den här avbildningen är storleksändrade too50% |
| </p> | | | |
| Etikett för användarnamn på inloggningssidan |(för närvarande inte används i hello-inloggningssida) I framtida hello kanske den här texten används tooreplace hello Allmänt ”arbets- eller skolkonto” sträng på olika ställen i hello upplevelse. Du kan ange den toosomething som ”Contoso-konto” eller ”Contoso-ID” |<p>Unicode-text, upp too50 tecken</p><p>Endast oformaterad text (inga länkar eller HTML-taggar)</p> |<p>Håll det kort och enkelt</p><p>Fråga användarna hur de vanligtvis refererar toohello arbets- eller skolkonto du ger dem..</p> |
| Text på inloggningssidan |Den här ”standardtext” visas under hello-inloggningssida form och kan inte använda toocommunicate ytterligare instruktioner eller där tooget hjälp och support. |<p>Unicode-text, upp too256 tecken</p><p>Endast oformaterad text (inga länkar eller HTML-taggar)</p> |Se till att den inte är längre än 250 tecken (cirka 3 rader med text) |
| Bild av inloggningssidan |hello bild är en stor bild som visas på hello-inloggningssida toohello vänster hello-inloggningssida formuläret. |<p>JPG eller PNG</p><p>1420 × 1200</p><p>500 kB</p> |<p>1420 × 1200 bildpunkter</p><p>Viktigt! Håll den så liten som möjligt, helst under 200 kB. Om den här bilden är för stor påverkas hello-inloggningssida hello prestanda när hello bilden inte cachelagras</p><p>Den här bilden beskärs ofta tooaccommodate olika skärmproportioner. Behåll hello primära visuella element i hello övre vänstra hörnet (längst upp till höger för RTL-språk) eftersom storleksändringen görs från hello nedre högra hörnet och upp mot hello översta vänstra som hello webbläsarfönstret krymper.</p> |
| Bakgrundsfärg på inloggningssidan |bakgrundsfärgen för hello inloggningssidan används hello området toohello till vänster i hello-inloggningssida formuläret. |Måste vara en RGB-färg i hexadecimalt format (exempel: #FFFFFF) |<p>hello bakgrundsfärgen kan visas i stället hello stora bilden vid anslutningar med låg bandbredd</p><p>Vi rekommenderar att du väljer hello primära färgen i hello Banderollslogotyp</p> |

## <a name="next-steps"></a>Nästa steg
* [Komma igång med Azure Active Directory Premium](active-directory-get-started-premium.md)
* [Visa åtkomst- och användningsrapporterna](active-directory-view-access-usage-reports.md)

<!--Image references-->
[1]: ./media/active-directory-add-company-branding/SignInPage_beforecustomization.png
[2]: ./media/active-directory-add-company-branding/SignInPage_aftercustomization.png
[3]: ./media/active-directory-add-company-branding/SignInPage_mobile_beforecustomization.png
[4]: ./media/active-directory-add-company-branding/SignInPage_mobile_aftercustomization.png
[5]: ./media/active-directory-add-company-branding/SignInPage_aftercustomization_elements.png
[6]: ./media/active-directory-add-company-branding/SignInPage_aftercustomization_croppedleft.png
[7]: ./media/active-directory-add-company-branding/SignInPage_aftercustomization_croppedtop.png
[8]: ./media/active-directory-add-company-branding/APBranding.png
[9]: ./media/active-directory-add-company-branding/hidekmsi.png
