---
title: "aaaUse NPS-servrar tooprovide Azure MFA möjligheterna | Microsoft Docs"
description: "hello NPS-tillägget för Azure Multi-Factor Authentication är en enkel lösning tooadd molnbaserade tvåstegsverifiering vericiation funktioner tooyour befintliga autentisering infrastruktur."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: H1Hack27Feb2017; it-pro
ms.openlocfilehash: e9fc495b06873d45f06233f1aa618db9d94f8bd9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-your-existing-nps-infrastructure-with-azure-multi-factor-authentication"></a>Integrera din befintliga infrastruktur för NPS med Azure Multi-Factor Authentication

hello Server (NPS)-tillägg för Azure MFA lägger till molnbaserade MFA funktioner tooyour autentisering infrastruktur med hjälp av din befintliga servrar. Med hello NPS-tillägget, kan du lägga till telefonsamtal, SMS eller telefon app verifiering tooyour befintliga autentiseringsflödet utan tooinstall, konfigurerar och underhåller nya servrar. 

Det här tillägget har skapats för organisationer som vill tooprotect VPN-anslutningar utan att distribuera hello Azure MFA-Server. hello NPS-tillägg som fungerar som ett kort mellan RADIUS och molnbaserad Azure MFA tooprovide tvåfaktorsautentisering för federerade eller synkroniserade användare.

När du använder hello NPS-tillägg för Azure MFA innehåller hello autentiseringsflödet hello följande komponenter: 

1. **NAS-VPN-Server** tar emot förfrågningar från VPN-klienter och konverterar dem till RADIUS-begäranden tooNPS servrar. 
2. **NPS-servern** ansluter tooActive Directory tooperform hello primär autentisering för hello RADIUS-begäranden och vid lyckades, skickar hello begäran tooany installerat tillägg.  
3. **NPS-tillägget** utlöser begäran tooAzure MFA för hello sekundära autentiseringen. När hello tillägget tar emot hello svar och hello MFA-kontrollen lyckas, Slutför hello autentiseringsbegäran genom att ge säkerhetstoken som innehåller ett MFA-anspråk som utfärdats av Azure STS hello NPS-servern.  
4. **Azure MFA** kommunicerar med Azure Active Directory tooretrieve hello användarens information och utför hello sekundära autentiseringen med hjälp av en verifiering metod konfigurerats toohello användare.

hello följande diagram illustrerar det här övergripande autentiseringsflödet för begäran: 

![Flödesdiagram för autentisering](./media/multi-factor-authentication-nps-extension/auth-flow.png)

## <a name="plan-your-deployment"></a>Planera distributionen

hello NPS tillägget hanterar automatiskt redundans, så du behöver en särskild konfiguration.

Du kan skapa så många Azure MFA-aktiverade NPS-servrar som du behöver. Om du installerar flera servrar, bör du använda ett certifikat för skillnaden för var och en av dem. När du skapar ett certifikat för varje server innebär att du kan uppdatera varje cert individuellt och oroa dig inte om driftstopp på alla servrar.

VPN-servrar vidarebefordra autentiseringsbegäranden, så de måste toobe medveten om hello nya Azure MFA-aktiverade NPS-servrar.

## <a name="prerequisites"></a>Krav

hello NPS-tillägget är avsedd toowork med den befintliga infrastrukturen. Kontrollera att du har hello följande krav innan du börjar.

### <a name="licenses"></a>Licenser

hello NPS-tillägg för Azure MFA är tillgängliga toocustomers med [licenser för Azure Multi-Factor Authentication](multi-factor-authentication.md) (ingår i Azure AD Premium, EMS eller en MFA-prenumeration).

### <a name="software"></a>Programvara

Windows Server 2008 R2 SP1 eller senare.

### <a name="libraries"></a>Bibliotek

Dessa bibliotek installeras automatiskt med hello tillägg.
-   [Visual C++ Redistributable-paket för Visual Studio 2013 (X64)](https://www.microsoft.com/download/details.aspx?id=40784)
-   [Microsoft Azure Active Directory-modulen för Windows PowerShell version 1.1.166.0](https://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185)

### <a name="azure-active-directory"></a>Azure Active Directory

Alla som använder hello NPS tillägget måste vara synkroniserade tooAzure Active Directory med Azure AD Connect och måste vara registrerad för Multifaktorautentisering.

När du installerar tillägget för hello behöver du hello-directory-ID och admin-autentiseringsuppgifter för Azure AD-klienten. Du hittar din katalog-ID i hello [Azure-portalen](https://portal.azure.com). Logga in som administratör, Välj hello **Azure Active Directory** ikonen hello vänster, välj sedan **egenskaper**. Kopiera hello GUID i hello **katalog-ID** och spara den. Använd detta GUID som hello klient-ID när du installerar hello NPS-tillägget.

![Hitta din katalog-ID under Azure Active Directory-egenskaper](./media/multi-factor-authentication-nps-extension/find-directory-id.png)

## <a name="prepare-your-environment"></a>Förbered din miljö

Innan du installerar hello NPS tillägg du vill tooprepare du miljön toohandle hello autentiseringstrafik.

### <a name="enable-hello-nps-role-on-a-domain-joined-server"></a>Aktivera hello NPS-rollen på en domänansluten server

hello NPS-servern ansluter tooAzure Active Directory och autentiserar hello MFA begäranden. Välj en server för den här rollen. Vi rekommenderar att du väljer en server som inte hanterar förfrågningar från andra tjänster, eftersom hello NPS tillägget genererar fel för alla begäranden som inte är RADIUS.

1. Öppna på servern, hello **guiden Lägg till roller och funktioner** från hello Serverhanteraren Quickstart-menyn.
2. Välj **rollbaserad eller funktionsbaserad installation** för din installation.
3. Välj hello **nätverksprincip och åtkomsttjänster** -serverrollen. Ett fönster kan visas tooinform du toorun nödvändiga funktioner för den här rollen.
4. Fortsätta hello guiden förrän hello bekräftelsesidan. Välj **installera**.

Nu när du har en server som är avsedd för NPS bör du också konfigurera den här servern toohandle inkommande RADIUS-begäranden från hello VPN-lösning.

### <a name="configure-your-vpn-solution-toocommunicate-with-hello-nps-server"></a>Konfigurera din VPN-lösningen toocommunicate med hello NPS-server

Hej steg tooconfigure din princip för RADIUS-autentisering varierar beroende på vilken VPN-lösning som du använder. Konfigurera den här principen toopoint tooyour RADIUS NPS-servern.

### <a name="sync-domain-users-toohello-cloud"></a>Synkronisera domänen användare toohello moln

Det här steget kan redan vara klar på din klient, men det är bra toodouble Kontrollera att Azure AD Connect har synkroniserat dina databaser nyligen.

1. Logga in toohello [Azure-portalen](https://portal.azure.com) som administratör.
2. Välj **Azure Active Directory** > **Azure AD Connect**
3. Kontrollera att din synkroniseringsstatus **aktiverad** och att dina senaste synkronisering har mindre än en timme sedan.

Om du behöver tookick en ny runda för synkronisering av oss hello instruktionerna i [Azure AD Connect-synkronisering: Schemaläggaren](../active-directory/connect/active-directory-aadconnectsync-feature-scheduler.md#start-the-scheduler).

### <a name="determine-which-authentication-methods-your-users-can-use"></a>Bestämma vilka autentiseringsmetoder som användarna kan använda

Det finns två faktorer som påverkar vilka autentiseringsmetoder som är tillgängliga med en NPS-tillägg-distribution:

1. hello lösenord krypteringsalgoritm som används mellan hello RADIUS-klient (VPN, Netscaler server eller andra) och hello NPS-servrar.
   - **PAP** stöder alla hello-autentiseringsmetoder för Azure MFA i molnet hello: telefonsamtal, enkelriktade SMS, mobilappavisering och Verifieringskod för mobila appar.
   - **CHAPv2** och **EAP** stöd för telefonsamtal och mobilappavisering.
2. hello inkommande metoder som hello klientprogram (VPN, Netscaler server eller andra) kan hantera. Till exempel har hello VPN-klienten vissa innebär tooallow hello användaren tootype i en Verifieringskod från text- eller mobilapp?

När du distribuerar hello NPS tillägget kan använda dessa faktorer tooevaluate vilka metoder som är tillgängliga för användarna. Om RADIUS-klienten stöder PAP, men hello klienten UX saknar inmatningsfält för en Verifieringskod, sedan telefonsamtal och mobilappavisering är hello två alternativ som stöds.

Du kan [inaktivera stöds inte autentiseringsmetoder](multi-factor-authentication-whats-next.md#selectable-verification-methods) i Azure.

### <a name="enable-users-for-mfa"></a>Aktivera användare för MFA

Innan du distribuerar hello fullständig NPS tillägg måste tooenable MFA för hello användare som du vill tooperform tvåstegsverifiering. Fler omedelbart tootest hello tillägg som du distribuerar det, behöver du minst ett test-konto som är fullständigt registrerad för Multifaktorautentisering.

Använd de här stegen tooget ett testkonto igång:
1. Logga in för[https://aka.ms/mfasetup](https://aka.ms/mfasetup) med ett testkonto. 
2. Följ hello prompter tooset upp en verifieringsmetod.
3. Skapa en princip för villkorlig åtkomst eller [ändra hello användartillstånd](multi-factor-authentication-get-started-user-states.md) toorequire tvåstegsverifiering för hello testkonto. 

Användarna måste också toofollow dessa steg tooenroll innan de kan autentisera med hello NPS-tillägg.

## <a name="install-hello-nps-extension"></a>Installera hello NPS-tillägg

> [!IMPORTANT]
> Installera hello NPS tillägg på en annan server än hello VPN åtkomstpunkt.

### <a name="download-and-install-hello-nps-extension-for-azure-mfa"></a>Hämta och installera hello NPS-tillägg för Azure MFA

1.  [Hämta hello NPS tillägget](https://aka.ms/npsmfa) från hello Microsoft Download Center.
2.  Kopiera hello binära toohello Network Policy Server du vill tooconfigure.
3.  Kör *setup.exe* och följ anvisningarna för installation av hello. Om det uppstår fel kontrollerar du att hello två bibliotek från hello nödvändiga avsnitt har installerats.

### <a name="run-hello-powershell-script"></a>Kör hello PowerShell-skript

hello installationsprogrammet skapar ett PowerShell-skript på den här platsen: `C:\Program Files\Microsoft\AzureMfa\Config` (där C:\ är installationsenheten). Detta PowerShell-skript utför hello följande åtgärder:

-   Skapa ett självsignerat certifikat.
-   Associera hello offentliga nyckeln för hello certifikat toohello tjänstens huvudnamn på Azure AD.
-   Lagra hello cert i hello lokala datorns certifikatarkiv.
-   Bevilja åtkomst toohello certifikatets privata nyckel tooNetwork användare.
-   Starta om hello NPS.

Om du inte vill toouse egna kör certifikat (i stället för hello självsignerade certifikat som hello PowerShell-skriptet genererar), hello PowerShell-skript toocomplete hello installation. Om du installerar tillägget för hello på flera servrar, ha var och en sina egna certifikat.

1. Kör Windows PowerShell som administratör.
2. Ändra kataloger.

   `cd "C:\Program Files\Microsoft\AzureMfa\Config"`

3. Kör hello PowerShell-skript som skapats av hello installer.

   `.\AzureMfaNpsExtnConfigSetup.ps1`

4. PowerShell frågar efter klient-ID. Använd hello Directory ID GUID som du kopierade från hello Azure-portalen i avsnittet förutsättningar för hello.
5. Logga in som administratör tooAzure AD.
6. PowerShell visar ett meddelande när hello skriptet har slutförts.  

Upprepa dessa steg på ytterligare NPS-servrar som du vill tooset för belastningsutjämning.

>[!NOTE]
>Om du använder egna certifikat i stället för att certifikat med hello PowerShell-skript, se till att de matchar toohello NPS namngivningskonvention. hello ämnesnamn måste vara **CN =\<TenantID\>, OU = Microsoft NPS tillägget**. 

## <a name="configure-your-nps-extension"></a>Konfigurera NPS-tillägget

Det här avsnittet innehåller överväganden vid utformning och förslag för lyckad distribution för NPS-tillägget.

### <a name="configuration-limitations"></a>Begränsningar för konfiguration

- hello NPS-tillägg för Azure MFA inkluderar inte verktyg toomigrate användare och inställningar från MFA-servern toohello moln. Därför föreslår vi använder hello-tillägg för nya distributioner i stället för befintlig distribution. Om du använder hello tillägg på en befintlig distribution kan användarna har tooperform bevis upp igen toopopulate sina MFA information i hello molnet.  
- hello NPS tillägget använder hello UPN från hello lokala Active directory tooidentify hello-användare på Azure MFA för att utföra hello sekundära auktor hello tillägget kan vara konfigurerade toouse en annan identifierare som alternativt inloggnings-ID eller anpassade Active Directory fältet än UPN. Se [avancerade konfigurationsalternativ för hello NPS-tillägget för Multifaktorautentisering](multi-factor-authentication-advanced-vpn-configurations.md) för mer information.
- Inte alla krypteringsprotokoll stöder alla verifieringsmetoderna.
   - **PAP** stöder telefonsamtal, enkelriktade SMS, mobilappavisering och Verifieringskod för mobila appar
   - **CHAPv2** och **EAP** stöd för telefonsamtal och mobilappavisering

### <a name="control-radius-clients-that-require-mfa"></a>Kontrollen RADIUS-klienter som kräver MFA

När du aktiverar Multifaktorautentisering för en RADIUS-klient som använder hello NPS tillägget är alla autentiseringar för den här klienten nödvändiga tooperform MFA. Om du vill tooenable MFA för vissa RADIUS-klienter kan du konfigurera två NPS-servrar och installera hello tillägg på endast en av dem. Konfigurera RADIUS-klienter som du vill toorequire MFA toosend begäranden toohello NPS-server konfigurerad med hello tillägg och andra RADIUS-klienter toohello NPS-servern inte har konfigurerats med hello-tillägget.

### <a name="prepare-for-users-that-arent-enrolled-for-mfa"></a>Förbereda för användare som inte är registrerade för MFA

Om du har användare som inte är registrerade för MFA, kan du bestämma vad som händer när de försöker tooauthenticate. Använd hello registerinställning *REQUIRE_USER_MATCH* i hello registersökväg *HKLM\Software\Microsoft\AzureMFA* toocontrol hello funktionen beteende. Den här inställningen har ett enda konfigurationsalternativ:

| Nyckel | Värde | Standard |
| --- | ----- | ------- |
| REQUIRE_USER_MATCH | SANT/FALSKT | Ange inte (motsvarande tooTRUE) |

Hej syftet med den här inställningen är toodetermine vilka toodo när en användare inte har registrerats för MFA. När hello nyckeln finns inte, har inte angetts eller har angetts tooTRUE, och hello användaren har registrerats inte och sedan hello tillägget misslyckas hello MFA-kontrollen. När hello nyckeln anges tooFALSE och hello användaren inte har registrerats, fortsätter autentiseringen utan att utföra MFA.

Du kan välja toocreate den här nyckeln och ange den tooFALSE medan användarna är onboarding och kan inte registreras för Azure MFA ännu. Eftersom inställningen hello nyckeln tillåter att användare som inte är registrerade för MFA toosign i, bör du dock ta bort den här nyckeln innan pågående tooproduction.

## <a name="troubleshooting"></a>Felsökning

### <a name="how-do-i-verify-that-hello-client-cert-is-installed-as-expected"></a>Hur kan jag bekräfta att hello klientens certifikat har installerats som förväntat?

Titta efter hello självsignerade certifikat som skapas av hello installer i hello certifikatarkiv och kontrollera att hello privat nyckel har behörigheterna toouser **NÄTVERKSTJÄNSTEN**. hello certifikat har ett ämnesnamn **CN \<tenantid\>, OU = Microsoft NPS-tillägg**

-------------------------------------------------------------

### <a name="how-can-i-verify-that-my-client-cert-is-associated-toomy-tenant-in-azure-active-directory"></a>Hur kan jag bekräfta att Mina objekt är kopplade toomy klient i Azure Active Directory?

Öppna PowerShell-Kommandotolken och kör hello följande kommandon:

```
import-module MSOnline
Connect-MsolService
Get-MsolServicePrincipalCredential -AppPrincipalId "981f26a1-7f43-403b-a875-f8b09b8cd720" -ReturnKeyValues 1 
```

Dessa kommandon ut alla hello certifikat associera din klient med din instans av hello NPS-tillägget i PowerShell-sessionen. Leta efter certifikatet genom att exportera certifikatet klienten som en ”Base64-kodat X.509(.cer)” fil utan hello privata nyckeln och jämför den med hello lista från PowerShell.

Giltigt-från- och -tills tidsstämplar som är läsbart format, kan bara använda toofilter ut uppenbara misfits om hello kommando returnerar mer än ett certifikat.

-------------------------------------------------------------

### <a name="why-are-my-requests-failing-with-adal-token-error"></a>Varför är min begäranden inte med ADAL fel?

Detta fel kan bero på grund av tooone av flera skäl. Följ dessa steg toohelp felsöka:

1. Starta om NPS-servern.
2. Kontrollera att certifikat som klienten är installerad som förväntat.
3. Kontrollera att hello-certifikatet är kopplat till din klient på Azure AD.
4. Kontrollera att https://login.microsoftonline.com/ är tillgänglig från hello-server som kör hello-tillägget.

-------------------------------------------------------------

### <a name="why-does-authentication-fail-with-an-error-in-http-logs-stating-that-hello-user-is-not-found"></a>Varför misslyckas autentiseringen med ett fel i HTTP-loggarna som om det gick inte att hitta användaren hello?

Kontrollera att AD Connect är igång och att användaren hello finns i både Windows Active Directory och Azure Active Directory.

------------------------------------------------------------

### <a name="why-do-i-see-http-connect-errors-in-logs-with-all-my-authentications-failing"></a>Varför ser HTTP connect-fel i loggarna med min autentiseringar misslyckas?

Kontrollera att https://adnotifications.windowsazure.com kan nås från hello-server som kör hello NPS-tillägget.


## <a name="next-steps"></a>Nästa steg

- Konfigurera alternativa ID för inloggning eller ställa in en undantagslista för IP-adresser som inte får utföra tvåstegsverifiering i [avancerade konfigurationsalternativ för hello NPS-tillägg för Multifaktorautentisering](nps-extension-advanced-configuration.md)

- [Lösa felmeddelanden från hello NPS-tillägg för Azure Multi-Factor Authentication](multi-factor-authentication-nps-errors.md)
