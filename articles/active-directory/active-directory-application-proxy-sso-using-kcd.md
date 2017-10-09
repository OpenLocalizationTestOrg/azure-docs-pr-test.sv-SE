---
title: aaaSingle-inloggning med Application Proxy | Microsoft Docs
description: Beskriver hur tooprovide enkel inloggning med Azure AD Application Proxy.
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: ded0d9c9-45f6-47d7-bd0f-3f7fd99ab621
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: H1Hack27Feb2017, it-pro
ms.openlocfilehash: 0047e834cd42e057a75ebc0c5dcf860734464a05
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="kerberos-constrained-delegation-for-single-sign-on-tooyour-apps-with-application-proxy"></a>Kerberos-begränsad delegering för enkel inloggning tooyour appar med Application Proxy

Du kan tillhandahålla enkel inloggning för lokalt program som publicerats via programproxy som skyddas med integrerad Windows-autentisering. Dessa program kräver en Kerberos-biljett för åtkomst till. Programproxy använder Kerberos-begränsad delegering (KCD) toosupport dessa program. 

Du kan aktivera enkel inloggning tooyour program med integrerad autentisering IWA (Windows) genom att ge Application Proxy kopplingar behörigheter i Active Directory tooimpersonate användare. hello kopplingar som använder den här behörigheten toosend och ta emot token åt.

## <a name="how-single-sign-on-with-kcd-works"></a>Hur enkel inloggning med KCD fungerar
Det här diagrammet beskrivs hello flödet när en användare försöker tooaccess ett lokalt program som använder IWA.

![Flödesdiagram för Microsoft AAD-autentisering](./media/active-directory-application-proxy-sso-using-kcd/AuthDiagram.png)

1. hello användaren anger hello URL tooaccess hello lokala program via Webbprogramproxy.
2. Application Proxy dirigerar hello begäran tooAzure AD authentication services toopreauthenticate. Nu är gäller Azure AD alla tillämpliga autentiserings- och auktoriseringsprinciper, till exempel flerfunktionsautentisering. Om hello användaren har verifierats Azure AD skapar en token och skickar den toohello användaren.
3. hello användaren skickar hello token tooApplication Proxy.
4. Programproxy validerar hello token hämtar hello UPN (User Principal Name) från den och sedan skickar hello begäran hello UPN och hello namn SPN (Service Principal) toohello anslutningen via en säker kanal dually autentiserad.
5. hello Connector utför Kerberos-begränsad delegering (KCD)-förhandling med hello lokal AD, personifiering hello användaren tooget ett token toohello Kerberos-program.
6. Active Directory skickar hello Kerberos-token för hello programmet toohello koppling.
7. hello Connector skickar hello ursprungliga begäran toohello programserver med hjälp av hello Kerberos-token togs emot från AD.
8. hello skickar hello svar toohello Connector, som då returneras toohello Application Proxy-tjänsten och slutligen toohello användare.

## <a name="prerequisites"></a>Krav
Innan du börjar med enkel inloggning för IWA program, kontrollera att din miljö är redo med hello följande inställningar och konfigurationer:

* Dina appar, t.ex SharePoint Web apps ställs toouse integrerad Windows-autentisering. Mer information finns i [aktivera stöd för Kerberos-autentisering](https://technet.microsoft.com/library/dd759186.aspx), eller för SharePoint finns [planera för Kerberos-autentisering i SharePoint 2013](https://technet.microsoft.com/library/ee806870.aspx).
* Alla dina appar har [Service Principal Names](https://social.technet.microsoft.com/wiki/contents/articles/717.service-principal-names-spns-setspn-syntax-setspn-exe.aspx).
* hello server körs hello koppling och hello-server som kör hello app är ansluten till en domän och en del av hello samma domän eller betrodda domäner. Mer information om domänanslutning finns [ansluta en dator tooa domän](https://technet.microsoft.com/library/dd807102.aspx).
* hello-server som kör hello Connector har åtkomst tooread hello TokenGroupsGlobalAndUniversal attribut för användare. Den här standardinställningen kanske har påverkats av säkerhet härdning hello-miljö.

### <a name="configure-active-directory"></a>Konfigurera Active Directory
hello Active Directory-konfigurationen varierar, beroende på om dina Application Proxy connector och hello Programserver är i hello samma domän eller inte.

#### <a name="connector-and-application-server-in-hello-same-domain"></a>Koppling och programserver i hello samma domän
1. I Active Directory, gå för**verktyg** > **användare och datorer**.
2. Välj hello-server som kör hello-anslutningen.
3. Högerklicka och välj **egenskaper** > **delegering**.
4. Välj **förtroende för den här datorn för delegering toospecified services**. 
5. Under **Services toowhich det här kontot kan ge delegerade autentiseringsuppgifter** Lägg till hello värde för hello SPN-identitet hello-programserver. Detta gör hello Application Proxy Connector tooimpersonate användare i AD mot hello-program som anges i hello-listan.

   ![Skärmbild av Connector SVR egenskaper](./media/active-directory-application-proxy-sso-using-kcd/Properties.jpg)

#### <a name="connector-and-application-server-in-different-domains"></a>Koppling och programservrar i olika domäner
1. En lista över förutsättningar för att arbeta med KCD över domäner finns [Kerberos-begränsad delegering över domäner](https://technet.microsoft.com/library/hh831477.aspx).
2. Använd hello `principalsallowedtodelegateto` egenskapen hello Connector server tooenable hello Application Proxy toodelegate för hello-anslutningsservern. hello programservern `sharepointserviceaccount` och hello delegera server är `connectormachineaccount`. Använd den här koden som ett exempel för Windows 2012 R2:

        $connector= Get-ADComputer -Identity connectormachineaccount -server dc.connectordomain.com

        Set-ADComputer -Identity sharepointserviceaccount -PrincipalsAllowedToDelegateToAccount $connector

        Get-ADComputer sharepointserviceaccount -Properties PrincipalsAllowedToDelegateToAccount

Sharepointserviceaccount kan vara hello Service Pack datorkontot eller ett tjänstkonto som programpoolen körs under vilka hello Service Pack.

## <a name="configure-single-sign-on"></a>Konfigurera enkel inloggning 
1. Publicera programmet enligt instruktionerna i toohello [publicera program med programproxy](application-proxy-publish-azure-portal.md). Se till att tooselect **Azure Active Directory** som hello **förautentisering metoden**.
2. När programmet visas i hello lista över företagsprogram, markerar du den och klickar på **enkel inloggning**.
3. Ställ in hello enkel inloggning för**integrerad Windows-autentisering**.  
4. Ange hello **interna program SPN** hello-programserver. I det här exemplet är hello SPN för vårt publicerade program http/www.contoso.com. Den här SPN måste toobe i hello lista över tjänster toowhich hello connector kan ge delegerade autentiseringsuppgifter. 
5. Välj hello **delegerad inloggningen identitet** för hello connector toouse åt dina användare. Mer information finns i [arbeta med olika lokala och molnidentiteter](#Working-with-different-on-premises-and-cloud-identities)

   ![Avancerad konfiguration](./media/active-directory-application-proxy-sso-using-kcd/cwap_auth2.png)  


## <a name="sso-for-non-windows-apps"></a>Enkel inloggning för Windows-appar
hello flödet av Kerberos-delegering i Azure AD Application Proxy startar när Azure AD autentiserar hello användaren i hello molnet. När hello begäran kommer lokalt, hello hello Azure AD Application Proxy-anslutningen utfärdar en Kerberos-biljett hello användarens räkning genom att interagera med lokala Active Directory. Den här processen är refererad tooas Kerberos-begränsad delegering (KCD). I hello skickas nästa fas en begäran toohello backend-programmet med Kerberos-biljett. Det finns flera protokoll som definierar hur toosend sådana begäranden. De flesta Windows-servrar förväntas Negotiate/SPNego som stöds nu på Azure AD Application Proxy.

Mer information om Kerberos finns [alla önskade tooknow om Kerberos-begränsad delegering (KCD)](https://blogs.technet.microsoft.com/applicationproxyblog/2015/09/21/all-you-want-to-know-about-kerberos-constrained-delegation-kcd).

Appar för icke-Windows vanligtvis användaren användarnamn eller SAM-namnen i stället för domänen e-postadresser. Om det inträffar tooyour program måste tooconfigure hello delegerad inloggningen identitet fältet tooconnect moln identiteter tooyour programmet identiteter. 

## <a name="working-with-different-on-premises-and-cloud-identities"></a>Arbeta med olika lokala och molnidentiteter
Programproxy förutsätter att användarna har exakt hello samma identitet i hello molnet och lokalt. Om detta inte är fallet hello kan du kan fortfarande använda KCD för enkel inloggning. Konfigurera en **delegerade identitet för nätverksinloggning** för varje program toospecify vilken identitet som ska användas när du utför enkel inloggning.  

Den här funktionen gör att många organisationer som har en annan lokal och molnet identiteter toohave SSO från hello tooon lokala molnappar utan hello användare tooenter olika användarnamn och lösenord. Detta inkluderar organisationer som:

* Har flera domäner internt (joe@us.contoso.com, joe@eu.contoso.com) och en enda domän i hello molnet (joe@contoso.com).
* Ha icke-dirigerbara domännamn internt (joe@contoso.usa) och en juridiska i hello molnet.
* Använd inte domännamn internt (Johan)
* Använd olika alias på lokalt och i hello molnet. Till exempel joe-johns@contoso.com vs.joej@contoso.com  

Du kan välja vilka identitet toouse tooobtain hello Kerberos-biljetten med Application Proxy. Den här inställningen är per program. Vissa av dessa alternativ är lämpliga för system som inte accepterar e-postadressformat, andra har utformats för alternativa inloggningen.

![Delegerad inloggningen identitet parametern skärmbild](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_upn.png)

Om delegerade inloggningen identitet används kanske hello värde inte unika mellan alla hello domäner och skogar i organisationen. Du kan undvika det här problemet genom att publicera programmen två gånger med två olika Connector-grupper. Eftersom varje program har en annan målgrupp, kan du ansluta till dess kopplingar tooa annan domän.

### <a name="configure-sso-for-different-identities"></a>Konfigurera enkel inloggning för olika identiteter
1. Konfigurera inställningar för Azure AD Connect så hello huvudsakliga identitet är hello e-postadress (e-post). Detta görs som en del av hello anpassa processen genom att ändra hello **användarens huvudnamn** i hello synkroniseringsinställningar. De här inställningarna avgör också hur användare loggar in tooOffice365, Windows10 enheter och andra program som använder Azure AD som deras identitet store.  
   ![Identifiera användare skärmbild - UPN-namnet listrutan](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_connect_settings.png)  
2. Hello programkonfigurationen inställningarna för hello programmet vill toomodify, Välj hello **delegerad inloggningen identitet** toobe används:

   * Användarens huvudnamn (till exempel joe@contoso.com)
   * Alternativa UPN-namnet (till exempel joed@contoso.local)
   * Användarnamnet som en del av användarens huvudnamn (till exempel Johan)
   * Användarnamnet som en del av alternativt UPN-namn (till exempel joed)
   * Lokala SAM-kontonamn (beroende på konfigurationen av domänkontrollanten hello)

### <a name="troubleshooting-sso-for-different-identities"></a>Felsökning av enkel inloggning för olika identiteter
Om det finns ett fel i hello SSO-processen, visas den i hello connector datorn händelseloggen som beskrivs i [felsökning](application-proxy-back-end-kerberos-constrained-delegation-how-to.md).
Men i vissa fall hello begäran skickas har toohello backend-programmet när det här programmet svarar i olika HTTP-svar. Felsökning av dessa fall ska starta genom att undersöka händelsenummer 24029 på hello connector dator i händelseloggen för hello Application Proxy-sessionen. hello användar-ID som användes för delegering visas i hello ”användare” fältet inom hello händelseinformation. tooturn på sessionsloggen, Välj **visa analytiska loggar och felsökningsloggar** hello event viewer Visa-menyn.

## <a name="next-steps"></a>Nästa steg

* [Hur tooconfigure en programproxy programmet toouse Kerberos-begränsad delegering](application-proxy-back-end-kerberos-constrained-delegation-how-to.md)
* [Felsökning av problem med Application Proxy](active-directory-application-proxy-troubleshoot.md)


Hello senaste nyheterna och uppdateringarna finns på hello [bloggen om Application Proxy](http://blogs.technet.com/b/applicationproxyblog/)

