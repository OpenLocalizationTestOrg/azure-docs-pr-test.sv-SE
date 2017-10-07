---
title: "aaaEnable fjärråtkomst tooSharePoint med Azure AD Application Proxy | Microsoft Docs"
description: Beskriver hello grunderna om hur toointegrate ett lokalt SharePoint server med Azure AD Application Proxy.
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 6ab413462fcaf387e150449df9c97505c4108bcf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-remote-access-toosharepoint-with-azure-ad-application-proxy"></a>Aktivera fjärråtkomst tooSharePoint med Azure AD Application Proxy

Den här artikeln beskrivs hur toointegrate ett lokalt SharePoint server med Azure Active Directory (Azure AD) Application Proxy.

tooenable fjärråtkomst tooSharePoint med Azure AD Application Proxy Följ hello avsnitt i den här artikeln steg för steg.

## <a name="prerequisites"></a>Krav

Den här artikeln förutsätter att du redan har SharePoint 2013 eller senare i din miljö. Tänk dessutom hello följande krav:

* SharePoint innehåller inbyggt stöd för Kerberos. Användare som ansluter till interna webbplatser via fjärranslutning via Azure AD Application Proxy kan därför anta toohave en enkel inloggning (SSO) uppstår.

* Du måste toomake några ändringar tooyour SharePoint konfigurationsservern. Vi rekommenderar att du använder en fristående miljö. På så sätt kan du kan göra uppdateringar tooyour mellanlagring server först och underlätta en tester cykel innan du fortsätter till produktionen.

* Vi förutsätter att du har ställt in SSL för SharePoint, eftersom vi kräver SSL på hello publicerade URL. Du måste toohave SSL aktiverat på webbplatsen interna tooensure att länkar skickas/mappas korrekt. Om du inte har konfigurerat SSL, se [Konfigurera SSL för SharePoint 2013](https://blogs.msdn.microsoft.com/fabdulwahab/2013/01/20/configure-ssl-for-sharepoint-2013) anvisningar. Kontrollera också att hello connector förtroenden hello datorcertifikat som du skickar. (hello certifikat behöver inte toobe offentligt utfärdade.)

## <a name="step-1-set-up-single-sign-on-toosharepoint"></a>Steg 1: Konfigurera enkel inloggning tooSharePoint

Våra kunder vill hello bästa SSO-upplevelse för deras backend-program, SharePoint-servern i det här fallet. I det här vanliga scenariot i Azure AD hello användaren autentiseras bara en gång, eftersom de inte uppmanas att autentiseringen igen.

Du kan uppnå SSO för lokala program som kräver eller använder Windows-autentisering med hjälp av hello Kerberos-autentiseringsprotokollet och en funktion som kallas Kerberos-begränsad delegering (KCD). KCD när konfigurerad, kan hello Application Proxy connector tooobtain windows biljett/token för en användare, även om hello användaren inte har loggat in tooWindows direkt. toolearn mer om KCD, se [Kerberos-begränsad delegering översikt](https://technet.microsoft.com/library/jj553400.aspx).

tooset in KCD för en SharePoint-server, Använd hello procedurerna i hello följande sekventiella avsnitt:

### <a name="ensure-that-sharepoint-is-running-under-a-service-account"></a>Kontrollera att SharePoint körs under ett tjänstkonto

Kontrollera först att SharePoint körs under en definierad tjänstkonto--inte lokalt system, lokal tjänst eller Nätverkstjänst. Gör så att du kan koppla tjänstens huvudnamn (SPN) tooa giltigt konto. SPN-namn är hur hello Kerberos-protokollet identifierar olika tjänster. Och du behöver hello konto senare tooconfigure hello KCD.

tooensure som dina platser körs under en definierad tjänstkonto utför hello följande steg:

1. Öppna hello **Central Administration av SharePoint 2013** plats.
2. Gå för**säkerhet** och välj **Konfigurera tjänstkonton**.
3. Välj **Web programpoolen - SharePoint - 80**. hello alternativ kan avvika något baserat på hello namnet på din webbserver-pool eller om hello web pool använder SSL som standard.

  ![Alternativ för att konfigurera ett tjänstkonto](./media/application-proxy-remote-sharepoint/remote-sharepoint-service-web-application.png)

4. Om **Markera ett konto för den här komponenten** är **lokal tjänst** eller **nätverkstjänsten**, behöver du toocreate ett konto. Om inte du är klar och kan flytta toohello nästa avsnitt.
5. Välj **registrera nytt hanterat konto**. När ditt konto har skapats måste du ange **webbprogrampoolen** innan du kan använda hello-konto.

> [!NOTE]
Du behöver toohave en tidigare skapade Azure AD-kontot för hello-tjänsten. Vi rekommenderar att du tillåter en automatisk ändring av lösenord. Mer information om hello fullständig uppsättning steg och felsökningsproblem finns [konfigurera automatisk ändring av lösenord i SharePoint 2013](https://technet.microsoft.com/library/ff724280.aspx).

### <a name="configure-sharepoint-for-kerberos"></a>Konfigurera SharePoint för Kerberos

Du använder KCD tooperform enkel inloggning toohello SharePoint server och det fungerar bara med Kerberos.

tooconfigure för din SharePoint-webbplatsen för Kerberos-autentisering:

1. Öppna hello **Central Administration av SharePoint 2013** plats.
2. Gå för**programhantering**väljer **hantera webbprogram**, och välj din SharePoint-webbplats. I det här exemplet är det **SharePoint - 80**.

  ![Att välja hello SharePoint-webbplats](./media/application-proxy-remote-sharepoint/remote-sharepoint-manage-web-applications.png)

3. Klicka på **autentiseringsproviders** hello i verktygsfältet.
4. I hello **autentiseringsproviders** klickar du på **standardzonen** tooview hello inställningar.
5. I hello **Redigera autentisering** dialogrutan rutan, bläddra nedåt tills du ser **Anspråksautentiseringstyper** och kontrollera att både **aktivera Windows-autentisering** och  **Integrerad Windows-autentisering** har valts.
6. I hello listrutan, se till att **förhandla (Kerberos)** är markerad.

  ![Dialogrutan Redigera autentisering](./media/application-proxy-remote-sharepoint/remote-sharepoint-service-edit-authentication.png)

7. Längst ned hello hello **Redigera autentisering** dialogrutan klickar du på **spara**.

### <a name="set-a-service-principal-name-for-hello-sharepoint-service-account"></a>Ange tjänstens huvudnamn för hello SharePoint-tjänstkonto

Innan du konfigurerar hello KCD måste tooidentify hello SharePoint-tjänsten körs som hello-tjänstkonto som du har konfigurerat. Det gör du genom att ange ett SPN-namn. Mer information finns i [Service Principal Names](https://technet.microsoft.com/library/cc961723.aspx).

hello SPN-format är:

```
<service class>/<host>:<port>
```

Hello SPN-format:

* _tjänsten klassen_ är ett unikt namn för hello-tjänsten. För SharePoint, använder du **HTTP**.

* _värden_ är hello fullständigt kvalificerade domännamnet eller NetBIOS-namnet på hello värddatorn hello tjänsten körs på. Den här texten kanske måste toobe hello URL hello plats, beroende på hello version av IIS som du använder för en SharePoint-webbplats.

* _port_ är valfritt.

Om hello hello SharePoint serverns FQDN är:

```
sharepoint.demo.o365identity.us
```

Sedan hello SPN är:

```
HTTP/ sharepoint.demo.o365identity.us demo
```

Du kanske också tooset SPN för specifika webbplatser på servern. Mer information finns i [konfigurera Kerberos-autentisering](https://technet.microsoft.com/library/cc263449(v=office.12).aspx). Betala uppmärksam toohello avsnittet ”Skapa tjänstens huvudnamn för ditt webbprogram med hjälp av Kerberos-autentisering”.

Hej enklast för tooset SPN toofollow hello SPN format som kanske redan finns i dina platser. Kopiera dessa SPN tooregister mot hello-tjänstkontot. toodo detta:

1. Bläddra toohello plats med hello SPN från en annan dator.
 När du gör hello cachelagras relevanta uppsättning Kerberos-biljetter på hello datorn. Dessa biljetter innehåller hello SPN för hello målwebbplatsen som du har öppnat.

2. Du kan dra hello SPN för den platsen med hjälp av verktyget [Klist](http://web.mit.edu/kerberos/krb5-devel/doc/user/user_commands/klist.html). I ett kommandofönster som körs i hello hello samma kontext som hello användare som öppnade hello plats i hello webbläsaren kan köra följande kommando:
```
Klist
```
Klist returnerar sedan hello uppsättning mål SPN-namn. I det här exemplet är hello markerade värdet hello SPN som behövs:

  ![Exempel Klist resultat](./media/application-proxy-remote-sharepoint/remote-sharepoint-target-service.png)

4. Nu när du har hello SPN måste toomake du att den är korrekt konfigurerade på hello-tjänstkonto som du skapat tidigare hello-webbprogram. Kör följande kommando från hello kommandotolk som administratör för hello domän hello:

 ```
 setspn -S http/sharepoint.demo.o365identity.us demo\sp_svc
 ```

 Det här kommandot anger hello SPN för hello SharePoint service-kontot körs som _demo\sp_svc_.

 Ersätt _http/sharepoint.demo.o365identity.us_ med hello SPN för servern och _demo\sp_svc_ med hello-tjänstkontot i din miljö. hello Setspn kommando söker efter hello SPN innan den lägger till den. I det här fallet kan du se en **dubbla SPN-värdet** fel. Om du ser det här felet, se till att hello-värdet är associerad med hello-tjänstkontot.

Du kan verifiera att SPN har lagts till genom att köra hello Setspn kommando med hello -l alternativet hello. toolearn mer om det här kommandot finns [Setspn](https://technet.microsoft.com/library/cc731241.aspx).

### <a name="ensure-that-hello-connector-is-set-as-a-trusted-delegate-toosharepoint"></a>Se till att kopplingen hello har angetts som en betrodd delegera tooSharePoint

Konfigurera hello KCD så att hello Azure AD Application Proxy-tjänsten kan delegera användaren identiteter toohello SharePoint-tjänsten. Du gör detta genom att aktivera hello Application Proxy connector tooretrieve Kerberos-biljetter för de användare som har verifierats i Azure AD. Servern skickar sedan hello kontexten toohello målprogrammet eller SharePoint i det här fallet.

tooconfigure hello KCD, upprepa hello följande steg för varje koppling dator:

1. Logga in som en domän administratören tooa DC och öppna sedan **Active Directory-användare och datorer**.
2. Hitta hello-dator som hello connector körs på. I det här exemplet har den hello samma SharePoint-servern.
3. Dubbelklicka på hello datorn och klicka sedan på hello **delegering** fliken.
4. Se till att hello delegeringsinställningar är inställda för**förtroende för den här datorn för delegering toohello angetts services endast**, och välj sedan **Använd valfritt autentiseringsprotokoll**.

  ![Inställningarna för standarddelegering](./media/application-proxy-remote-sharepoint/remote-sharepoint-delegation-box.png)

5. Klicka på hello **Lägg till** , klicka på **användare eller datorer**, och leta upp hello-tjänstkontot.

  ![Att lägga till hello SPN för hello-tjänstkontot](./media/application-proxy-remote-sharepoint/remote-sharepoint-users-computers.png)

6. I hello lista över SPN-namn, väljer du hello som du skapade tidigare för hello-tjänstkontot.
7. Klicka på **OK**. Klicka på **OK** igen toosave hello ändringar.

## <a name="step-2-enable-remote-access-toosharepoint"></a>Steg 2: Aktivera fjärråtkomst tooSharePoint

Nu när du har aktiverat SharePoint för Kerberos och konfigurerat KCD, är du redo tooset in tooSharePoint för enkel inloggning. Du kan sedan publicera hello SharePoint-servergruppen för fjärråtkomst via Azure AD Application Proxy från hello-anslutningen.

tooperform hello följande steg, behöver du toobe medlem i hello rollen Global administratör i organisationens Azure Active Directory-konto.

1. Logga in toohello [Azure-portalen](https://manage.windowsazure.com) och hitta din Azure AD-klient.
2. Klicka på **program**, och klicka sedan på **Lägg till**.
3. Välj **Publicera ett program som ska vara tillgängligt utanför ditt nätverk**. Om du inte ser det här alternativet, se till att du har Azure AD Basic eller Premium som angetts i hello-klient.
4. Slutför hello alternativ enligt följande:
 * **Namnet**: Använd ett värde som du vill till exempel **SharePoint**.
 * **Intern URL**: Detta är hello URL: en för SharePoint-webbplatsen för hello internt, såsom **https://SharePoint/**. I det här exemplet, se till att toouse **https**.
 * **Förautentiseringsmetod**: Välj **Azure Active Directory**.

  ![Alternativ för att lägga till ett program](./media/application-proxy-remote-sharepoint/remote-sharepoint-add-application.png)

5. När hello appen publiceras, klickar du på hello **konfigurera** fliken.
6. Bläddra nedåt toohello alternativet **översätta URL: en i sidhuvuden**. hello standardvärdet är **Ja**. Ändra också**nr**.

 SharePoint använder hello _värdadressen_ värdet toolook upp hello plats. Den genererar även länkar baserat på det här värdet. hello net effekten är toomake till att alla länkar som SharePoint genererar är en publicerade URL som är korrekt inställd toouse hello externa URL: en. Om hello värdet för**Ja** också aktiverar hello tooforward hello begäran toohello backend-anslutningsprogrammet. Men om hello värdet för**nr** innebär att hello anslutningen kommer inte att skicka hello interna värdnamn. I stället skickar hello connector hello värdadressen som hello publicerade URL toohello serverprogrammet.

 Dessutom tooensure att SharePoint accepterar den här URL: en måste du toocomplete en mer konfiguration på hello SharePoint-server. Gör du det i nästa avsnitt om hello.

7. Ändra **inre autentiseringsmetod** för**integrerad Windows-autentisering**. Om Azure AD-klienten använder en UPN i hello moln som skiljer sig från hello UPN lokalt, kan du komma ihåg tooupdate **delegerad inloggningen identitet** samt.
8. Ange **interna program SPN** toohello värdet som du angett tidigare. Till exempel använda **http/sharepoint.demo.o365identity.us**.
9. Tilldela hello programmet tooyour target användare.

Ditt program bör se ut ungefär toohello följande exempel:

  ![Färdiga programmet](./media/application-proxy-remote-sharepoint/remote-sharepoint-internal-application-spn.png)

## <a name="step-3-ensure-that-sharepoint-knows-about-hello-external-url"></a>Steg 3: Kontrollera att SharePoint känner hello extern URL

Din senaste steg tooensure som SharePoint hittar hello plats baserat på hello externa URL: en, så att den blir länkar baserat på den externa URL: en. Det gör du genom att konfigurera alternativa åtkomstmappningar för hello SharePoint-webbplats.

1. Öppna hello **Central Administration av SharePoint 2013** plats.
2. Under **systeminställningar**väljer **konfigurera alternativa åtkomstmappningar**.

 Då öppnas hello **alternativa åtkomstmappningar** rutan.

  ![Alternativa åtkomstmappningar rutan](./media/application-proxy-remote-sharepoint/remote-sharepoint-alternate-access1.png)

3. I hello listrutan bredvid **alternativa åtkomst mappning samling**väljer **ändra alternativa åtkomst mappning samlingen**.
4. Välj plats – till exempel **SharePoint - 80**.

  ![Du väljer en plats](./media/application-proxy-remote-sharepoint/remote-sharepoint-alternate-access2.png)

5. Du kan välja tooadd hello publicerade URL: en som en intern URL eller en offentlig URL. Det här exemplet använder en offentlig URL som hello extranät.
6. Klicka på **redigera offentliga URL: er** i hello **extranät** sökväg, och ange sedan hello sökvägen för hello publicerat program som hello föregående steg. Ange till exempel **https://sharepoint-iddemo.msappproxy.net**.

  ![Att ange hello sökväg](./media/application-proxy-remote-sharepoint/remote-sharepoint-alternate-access3.png)

7. Klicka på **Spara**.

 Du kan nu komma åt hello SharePoint-webbplatsen externt via Azure AD Application Proxy.

## <a name="next-steps"></a>Nästa steg

- [Hur säkra tooprovide fjärråtkomst tooon lokala program](active-directory-application-proxy-get-started.md)
- [Förstå Azure AD Application Proxy-kopplingar](application-proxy-understand-connectors.md)
- [Publicering av SharePoint 2016 och Office Online Server med Azure AD Application Proxy](https://blogs.technet.microsoft.com/dawiese/2016/06/09/publishing-sharepoint-2016-and-office-online-server-with-azure-ad-application-proxy/)
