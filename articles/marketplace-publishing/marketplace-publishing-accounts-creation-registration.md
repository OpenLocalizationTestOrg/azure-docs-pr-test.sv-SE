---
title: aaaCreating och registrera hello publisher konto | Microsoft Docs
description: "Instruktioner för att skapa ett Microsoft Developer-konto så vid godkännande kommer du säljer olika erbjuder typer på hello Azure Marketplace."
services: Azure Marketplace
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 5a2fe68d-2967-463f-8af6-42bed07e3eaa
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/04/2017
ms.author: hascipio
ms.openlocfilehash: 27847d5a0e5c7579111cd6f7278f5d74dfea37a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-microsoft-developer-account"></a>Skapa ett Microsoft Developer-konto
Den här artikeln vägleder dig genom hello nödvändiga konto skapas och registrering processen toobecome en godkänd Microsoft Developer för hello Azure Marketplace.

## <a name="1-create-a-microsoft-account"></a>1. Skapa ett Microsoft-konto
toostart hello publiceringsprocessen, behöver du toocreate ett Microsoft-konto. Det här kontot kommer att använda tooregister i tooboth hello **Microsoft Developer Center** och **Azure Publishing Portal**. Du bör ha endast ett Microsoft-konto för Azure Marketplace-erbjudanden. Det får inte vara specifika tooservices eller erbjuder.

hello-adressen som utgör hello användarnamn ska finnas på din domän och styrs av din IT-teamet. Alla hello publicering av relaterade aktiviteter ska utföras via det här kontot.

> [!WARNING]
> Ord som **”Azure”** och **”Microsoft”** stöds inte för registrering av Microsoft-konto. Undvik att använda dessa ord toocomplete hello konto skapas och registreringsprocessen.
>
>

### <a name="guidelines-for-company-accounts"></a>Riktlinjer för företag
När du skapar ett företagskonto, följer du dessa riktlinjer om mer än en person behöver tooaccess hello-konto genom att logga in med hello Microsoft-konto som har öppnat hello-konto.

> [!Important]
> Viktiga tooallow flera användare tooaccess Dev Center-konto bör du använda Azure Active Directory tooassign roller tooindividual användare, som kan komma åt hello konto genom att logga in med sina individuella autentiseringsuppgifter för Azure AD. Mer information finns i [hantera kontoanvändare](https://msdn.microsoft.com/en-us/windows/uwp/publish/manage-account-users).

* Skapa ditt Microsoft-konto med hjälp av en e-postadress som tillhör tooyour företagets domän, men inte tooa enskilda användare, till exempel windowsapps@fabrikam.com.
* Begränsa åtkomst toothis Microsoft-konto toohello minsta möjliga antal utvecklare.
* Ställ in en distributionslista för företagets e-post som innehåller alla som behöver tooaccess hello-utvecklarkonto och Lägg till den här säkerhetsinformationen för e-postadress tooyour. Detta gör att alla hello anställda på hello listan tooreceive säkerhetskoder vid behov och toomanage säkerhetsinformation för ditt Microsoft-konto. Om hur du konfigurerar en distributionslista inte är möjligt hello ägare av hello enskilda e-postkonto behöver toobe tillgängliga tooaccess och dela hello säkerhetskod när du uppmanas (t.ex när nya säkerhetsinformation läggs toohello konto eller när den måste kunna nås från en ny enhet).
* Lägga till ett telefonnummer för företag som inte kräver ett tillägg och är tillgänglig tookey gruppmedlemmar.
* I allmänhet har utvecklare använda betrodda enheter toolog i tooyour företagets utvecklarkonto. Alla nycklar teammedlemmar ska ha åtkomst toothese betrodda enheter. Detta minskar hello behovet av att säkerheten koder toobe skickas vid åtkomst till hello-konto.
* Om du behöver tooallow toohello åtkomstkonto från en icke betrodd dator kan begränsa den åtkomst tooa högst fem utvecklare. Vi rekommenderar dessa utvecklare ska komma åt hello konto från datorer som delar hello samma geografiska och nätverksplats.
* Granska ofta företagets säkerhetsinformation på [https://account.live.com/proofs/Manage](https://account.live.com/proofs/Manage) toomake är alla aktuella.

Ditt utvecklarkonto ska användas i första hand från betrodda datorer. Detta är viktigt eftersom det finns en gräns toohello antal koder som genererats per konto per vecka. Det gör också hello smidigast inloggningen.

Mer information om ytterligare developer konto riktlinjer och säkerhet, klickar du på [här](https://msdn.microsoft.com/en-us/windows/uwp/publish/opening-a-developer-account#additional-guidelines-for-company-accounts).

### <a name="instructions"></a>Instruktioner
1. Öppna en ny Chrome Incognito eller Internet Explorer InPrivate webbläsarens session tooensure som du inte är inloggad tooan befintligt konto.
2. Registrera hello e-post (per hello ovanstående riktlinjer t.ex. windowsapp@fabrikam.com) som ett Microsoft-konto med hjälp av länken hello [https://signup.live.com/signup.aspx](https://signup.live.com/signup.aspx). Följ anvisningarna nedan för hello.

   1. Under registrera ditt konto som ett Microsoft-konto, behöver du tooprovide ett giltigt telefonnummer för hello system toosend du en verifiering för kontot code som ett SMS eller ett automatsamtal.
   2. Under registrera ditt konto som ett Microsoft-konto, behöver du tooprovide en giltig e-id för mottagning av en automatisk e-post för verifiering för kontot.
3. Kontrollera hello e-postadress skickas toohello DL.
4. Nu är du redo toouse hello nya Microsoft-konto i hello Microsoft Developer Center.

## <a name="2-register-your-account-in-microsoft-developer-center"></a>2. Registrera ditt konto i Microsoft Developer Center
hello Microsoft Developer Center är används tooregister hello företagsinformation en gång. Hej registrant måste vara en giltig representativ för hello företag och måste ange deras egen personliga information som ett sätt toovalidate sin identitet. hello person som registrerar måste använda ett Microsoft-konto som är gemensam för hello företagets **och hello samma konto måste användas i hello Azure Publishing Portal.** Du bör kontrollera att ditt företag inte redan har ett Microsoft Developer Center-konto innan du försöker toocreate en toomake. Under hello processen kommer samla in adressen företagsinformation, bank kontoinformation och skatt information. Denna information kan du vanligtvis få från finans- eller affärsavdelningen.

> [!IMPORTANT]
> Du måste slutföra hello följande Developer profil komponenter i ordning tooprogress via hello olika faser i erbjudandet skapande och distribution.
>
>

| Profil för utvecklare | toostart utkast | Mellanlagring | Publicera kostnadsfritt och lösningsmall | Publicera kommersiella |
| --- | --- | --- | --- | --- |
| Registrering av företagets |Måste ha |Måste ha |Måste ha |Måste ha |
| Skatt profil-ID |Valfri |Valfri |Valfri |Måste ha |
| Konto |Valfri |Valfri |Valfri |Måste ha |

> [!NOTE]
> Ta med din egen licens (BYOL) stöds bara för virtuella datorer och anses vara en **ledigt** erbjudande.
>
>

### <a name="register-your-company-account"></a>Registrera ditt företagskonto
1. Öppna en ny Internet Explorer InPrivate- eller Incognito Chrome webbläsarens session tooensure som du inte är inloggad tooa personligt konto.
2. Gå för[http://dev.windows.com/registration?accountprogram=azure](http://dev.windows.com/registration?accountprogram=azure) tooregister själv som en säljare i hello Dev Center. Läs hello följande viktigt innan du fortsätter.

   > [!IMPORTANT]
   > Kontrollera hello e-post-id eller en distributionsplats listan (en distributionslista rekommenderas tooremove beroende från personer) som du ska använda för att registrera i hello Dev Center på först registrerad som ett Microsoft-konto. Om det inte finns sedan registrera med den här [länk](https://signup.live.com/signup?uaid=e479342fe2824efeb0c3d92c8f961fd3&lic=1). Dessutom **alla e-post-id under hello Microsoft företagsdomän d.v.s. @microsoft.com går inte att använda** för registrering av Dev Center.
   >
   >

    ![Rita][img-signin]
3. Slutför hello ”hjälp oss att skydda ditt konto” guiden, som verifierar din identitet via telefonnummer eller e-postadress.

    ![Rita][img-verify]
4. I hello ”registrering kontoinformation” avsnittet, väljer din **konto land/region** från hello listrutan.

    ![Rita](media/marketplace-publishing-accounts-creation-registration/imgRegisterCo_04.png)

   > [!WARNING]
   > **”Säljer från” länder:** i order toosell dina tjänster på hello Azure Marketplace, registrerade entiteten måste toobe från en hello godkända ”säljer-från-länder ovan. Den här begränsningen är beloppet och skatt skäl. Vi söker aktivt tooexpand denna lista över länder i hello nära framtiden, så Håll ögonen öppna. Mer information finns i hello [Marketplace deltagande principer](http://go.microsoft.com/fwlink/?LinkID=526833).
   >
   >
5. Välj ”typ av konto” som **företagets** och klicka sedan på hello **nästa** knappen.

   > [!IMPORTANT]
   > toobetter förstå kontotyper och som är bäst för du toochoose, visa sidan [konto typer, platser och avgifter](https://msdn.microsoft.com/library/windows/apps/jj863494.aspx)
   >
   >

    ![Rita](media/marketplace-publishing-accounts-creation-registration/imgRegisterCo_05.png)
6. Ange hello **Publisher visningsnamn**, vanligtvis hello namnet på ditt företag.

   > [!TIP]
   > hello publisher visningsnamn som angetts i hello Dev Center inte visas i hello Azure Marketplace när erbjudandet går listan. Men måste vara fylld toocomplete hello registreringsprocessen.
   >
   >
7. Ange hello **kontaktuppgifter** för hello konto verifiering.

   > [!IMPORTANT]
   > Du måste ange korrekt kontaktinformation eftersom den används i våra verifieringsprocessen för ditt företag toobe godkännas i hello Developer Center.
   >
   >
8. Ange hello kontaktinformation för hello **företagets godkännare**. Företagets godkännare är hello person som kan verifiera att du är behörig toocreate ett konto i hello Dev Center uppdrag av din organisation. Klicka på **nästa** toomove toohello **”betalning avsnittet”** när du är klar.

    ![Rita](media/marketplace-publishing-accounts-creation-registration/imgRegisterCo_08.png)
9. Ange din betalning info toopay för ditt konto. Om du har en kampanjkod som täcker hello kostnaden för registrering av anger du som här. Ange annars kreditkort info (eller PayPal stöds marknader). När du är klar klickar du på **nästa** toomove på toohello **”granska skärmen”**.

    ![Rita](media/marketplace-publishing-accounts-creation-registration/imgRegisterCo_09.png)
10. Granska din kontoinformation och bekräfta att allt är korrekt. Läs och acceptera hello villkor för hello [avtalet för Microsoft Azure Marketplace Publisher](http://go.microsoft.com/fwlink/?LinkID=699560). Kontrollera hello rutan tooindicate du har läst och accepterat villkoren.
11. Klicka på **Slutför** tooconfirm registreringen. Vi skickar en bekräftelse meddelandet tooyour e-postadress.
12. Om du planerar toopublish endast kostnadsfria erbjudanden, klickar du på **gå tooAzure Marketplace Publishing Portal** och du kan hoppa över toosection 3 i det här dokumentet [registrera ditt konto i hello publishing portal](#3-register-your-account-in-the-publishing-portal).

Om du planerar toopublish kommersiella erbjuder (t.ex. virtuella erbjuder med timvis faktureringsmodell som tillämpas), klickar du på **uppdatera din kontoinformation** där du måste fylla i hello skatt och bankinformation i Developer Center konto.

Om du föredrar tooupdate skatt och bank informationen senare, så du kan flytta toohello nästa avsnitt d.v.s. avsnitt 3 i det här dokumentet [registrera ditt konto i hello publishing portal](#3-register-your-account-in-the-publishing-portal), och kom tillbaka senare genom att använda länkar i hello Azure Publishing Portal.

> [!IMPORTANT]
> Kommersiella erbjudanden, inte kommer du att kunna toopush dina erbjudanden tooproduction utan att slutföra hello skatt och bank kontoinformation.
>
>

Om du vill tooupdate skatt och bank informationen senare kan du gå toosection 3, [registrera ditt konto i hello publishing portal](#3-register-your-account-in-the-publishing-portal), och kom tillbaka senare genom att använda länkar i hello Azure Publishing Portal.

### <a name="add-tax-and-banking-information"></a>Lägg till skatt och bankinformation
 Om du vill toopublish kommersiella erbjuder för inköp kan du också behöver tooadd beloppet och skatt information och skicka den för verifiering i hello Developer Center. Om du vill publicera endast kostnadsfria erbjudanden (eller BYOL erbjuder) sedan behöver du inte tooadd informationen. Du kan lägga till den senare, men det tar tid toovalidate hello skatteinformation. Om du vet att du kommer att erbjuda kommersiella erbjudanden för inköp, rekommenderar vi att du lägger till den så snart som möjligt.

**Bankinformation**

1. Logga in toohello [Microsoft Developer Center](http://dev.windows.com/registration?accountprogram=azure) med ditt Microsoft-konto.
2. Klicka på **beloppet konto** i hello kvar menyn under **Välj betalningsmetod** klickar du på **konto** eller **PayPal**.

   > [!IMPORTANT]
   > Om du har kommersiella erbjudanden som kunder köper i hello Marketplace är hello kontot där du får beloppet för dessa inköp.
   >
   >
3. Ange hello betalningsinformation och klicka på **spara** när du är nöjd.

   > [!IMPORTANT]
   > Om du behöver tooupdate eller ändrar ditt beloppet konto, följ hello samma steg ovan ersätter hello aktuella info med hello ny information. Om du ändrar kontot beloppet kan fördröja betalningar genom upp tooone betalning. Den här fördröjningen beror på att vi behöver tooverify hello konto ändras, precis som vi gjorde när du först ställa in hello beloppet konto. Du måste fortfarande få betalt hello hela beloppet när ditt konto har verifierats; betalningar på grund av hello aktuella betalning cykel ska vara tillagda toohello nästa.
   >
   >
4. Klicka på **Nästa**.

**Information om skatt**

1. Logga in toohello [Microsoft Developer Center](http://dev.windows.com/registration?accountprogram=azure) med ditt Microsoft-konto (vid behov).
2. Klicka på **skatt profil** hello vänstra menyn.
3. På hello **konfigurera skatt formuläret** väljer hello land eller region där du har permanenta land, och väljer sedan hello land eller region där du håller primära medborgarskap. Klicka på **Nästa**.
4. Ange information om skatt och klicka sedan på **nästa**.

> [!WARNING]
> Du kommer inte att kunna toopush tooproduction din kommersiell erbjuder utan att slutföra hello skatt och konto information i Microsoft Developer Center-konto.
>
>

Om du har problem med registreringen Developer Center logga ett supportärende som nedan

1. Gå toohello supportlänk [https://developer.microsoft.com/windows/support](https://developer.microsoft.com/windows/support)
2. Under **Kontakta oss** avsnittet, klicka på hello **skicka en incident** (som visas i hello skärmbilden nedan)

    ![Rita](media/marketplace-publishing-accounts-creation-registration/imgAddTax_02.png)
3. Välj ”hjälp med Dev Center” som **problemtyp** och ”publicera och hantera appar” som **kategori**. Klicka på hello efter ”starta e-post”.

    ![Rita](media/marketplace-publishing-accounts-creation-registration/imgAddTax_03.png)
4. Du följer med en inloggningssida. Använd alla Microsoft-konto logga in. Om du inte har ett Microsoft-konto skapar en med hjälp av det här [länk](https://signup.live.com/signup?uaid=0089f09ccae94043a0f07c2aaf928831&lic=1).
5. Fyll i hello information om hello problemet och subit hello biljett genom att klicka på hello **skicka** knappen.

    ![Rita](media/marketplace-publishing-accounts-creation-registration/imgAddTax_05.png)

## <a name="3-register-your-account-in-hello-publishing-portal"></a>3. Registrera ditt konto i hello publishing portal
Hej [publicering portal](http://publish.windowsazure.com) är används toopublish och hantera dina erbjudanden.

1. Öppna en ny Chrome Incognito eller Internet Explorer InPrivate webbläsarens session tooensure som du inte är inloggad tooa personligt konto.
2. Gå för[http://publish.windowsazure.com](http://publish.windowsazure.com).
3. Om du är en ny användare och logga in toohello publicera portal för hello första gången du måste logga in med hello samma e-id för vilken Dev Center-konto har registrerats. På så sätt länkas Dev Center-konto och publicera portalkonto med varandra. Du kan senare lägga till hello andra medlemmar av hello företag som arbetar med programmet hello som medadministratör i hello publicering portal genom att följa hello steg nedan.

Om du har lagts till som medadministratör i hello publicera portal, du kan logga in med ditt medadministratör.

> [!TIP]
> hello deltagande principer beskrivs på hello [Azure-webbplatsen](https://azure.microsoft.com/support/legal/marketplace/participation-policies/).
>
>

## <a name="4-steps-tooadd-a-co-admin-in-hello-publishing-portal"></a>4. Tooadd steg för en medadministratör i hello publicering av portalen
**Förutsatt att du är Hej administratör** anges nedan är hello steg tooadd co-administratör.

> [!NOTE]
> **För hello nya användare,** innan du lägger till en medadministratör hello publicering portal, se till att du har skapat minst ett program i hello publicering av portalen. Detta krävs som hello **UTGIVARE** fliken visas endast när du har skapat minst ett program i hello publicering av portalen.
>
>

1. Se till att hello medadministratör e-post-id är ett Microsoft-account(MSA). Om inte, registrera den som en MSA med detta [länk](https://signup.live.com/signup?uaid=0089f09ccae94043a0f07c2aaf928831&lic=1).
2. Kontrollera att det finns minst ett program under hello administratörskonto innan du försöker tooadd co-administratör.
3. Hello senare steg när du är klar, logga in toohello publicera portal med hello medadministratör e-id och sedan logga ut.
4. Nu inloggningen toohello publicering portal med ID: t för hello admin-e-post.
5. Navigera tooPublishers -> ditt konto -> Välj administratörer -> Lägg till hello medadministratör (skärmbilden nedan)

   ![Rita](media/marketplace-publishing-accounts-creation-registration/imgAddAdmin_05.png)

## <a name="5-steps-toodelete-a-co-admin-in-hello-publishing-portal"></a>5. Toodelete steg för en medadministratör i hello publicering av portalen
**Förutsatt att du är Hej administratör** anges nedan är hello steg toodelete co-administratör.

1. Inloggningen toohello publicering portal med ID: t för hello admin-e-post.
2. Navigera för**utgivare** -> Välj ditt konto -> **administratörer** -> **Medadministratörer**.
3. Klicka på hello **X** knappen Nästa toohello medadministratör du vill ta bort totalvärde (skärmbilden nedan).

    ![Rita](media/marketplace-publishing-accounts-creation-registration/imgDeleteAdmin_03.png)

## <a name="next-steps"></a>Nästa steg
Nu när ditt konto skapas och registreras, se till att du uppfyller eller uppfyller alla hello icke-tekniska krav toopublish erbjudandet genom att granska [icke-tekniska krav](marketplace-publishing-pre-requisites.md).

## <a name="see-also"></a>Se även
* [Komma igång: hur toopublish ett erbjudande toohello Azure Marketplace](marketplace-publishing-getting-started.md)

[img-msalive]:media/marketplace-publishing-accounts-creation-registration/creating-msa-account-msa-live.jpg
[img-email]:media/marketplace-publishing-accounts-creation-registration/creating-msa-account-msa-verifyemail.jpg
[img-sd-url]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-incognito.jpg
[img-signin]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-login.jpg
[img-verify]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-verify.jpg
[img-sd-top]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-personal-acc-details.jpg
[img-sd-info]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-personal.jpg
[img-sd-type]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-personal-acc-type.jpg
[img-sd-mktg1]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-personal-comp-det1.jpg
[img-sd-mktg2]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-personal-comp-det2.jpg
[img-sd-addr]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-personal-comp-add.jpg
[img-sd-legal]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-personal-cmp.jpg
[img-sd-submit]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-approval.jpg

[link-msdndoc]: https://msdn.microsoft.com/library/jj552460.aspx
[link-sellerdashboard]: http://sellerdashboard.microsoft.com/
[link-pubportal]: https://publish.windowsazure.com
[link-single-vm]:marketplace-publishing-vm-image-creation.md
[link-single-vm-prereq]:marketplace-publishing-vm-image-creation-prerequisites.md
[link-multi-vm]:marketplace-publishing-solution-template-creation.md
[link-multi-vm-prereq]:marketplace-publishing-solution-template-creation-prerequisites.md
[link-datasvc]:marketplace-publishing-data-service-creation.md
[link-datasvc-prereq]:marketplace-publishing-data-service-creation-prerequisites.md
[link-devsvc]:marketplace-publishing-dev-service-creation.md
[link-devsvc-prereq]:marketplace-publishing-dev-service-creation-prerequisites.md
[link-pushstaging]:marketplace-publishing-push-to-staging.md
