---
title: "aaaAzure Multi-Factor Authentication vanliga frågor och svar | Microsoft Docs"
description: "Vanliga frågor och svar relaterade tooAzure Multi-Factor Authentication. Multi-Factor Authentication är en metod för att verifiera en användares identitet som kräver mer än ett användarnamn och lösenord. Det ger ett extra lager av säkerhet toouser inloggnings- och transaktioner."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 50bb8ac3-5559-4d8b-a96a-799a74978b14
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: kgremban
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c8305cf4c41bf8e9802df192fecdb7e463eff9eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions-about-azure-multi-factor-authentication"></a>Vanliga frågor och svar om Azure Multi-Factor Authentication
Det här avsnittet får du svar vanliga frågor om Azure Multi-Factor Authentication och hello Multi-Factor Authentication-tjänsten. Den är uppdelad i frågor om hello service i allmänhet modeller, användarupplevelser, fakturering och felsökning.

## <a name="general"></a>Allmänt
**F: hur hanterar användardata i Azure Multi-Factor Authentication-servern?**

Användarinformationen är lagrad endast på hello lokala servrar med Multi-Factor Authentication-servern. Ingen beständig användarinformationen är lagrad i hello molnet. När hello användaren utför tvåstegsverifiering, skickar servern för flerfunktionsautentisering data toohello Azure Multi-Factor Authentication-Molntjänsten för autentisering. Kommunikation mellan servern för flerfunktionsautentisering och hello Multifaktorautentisering molntjänst använder Secure Sockets Layer (SSL) eller Transport Layer Security (TLS) via port 443 utgående.

När autentiseringsbegäranden skickas toohello Molntjänsten kan data samlas in för autentiserings- och rapporter. Datafält som ingår i två steg verifiering loggar är som följer:

* **Unikt ID** (antingen namn eller lokala Multi-Factor Authentication Server Användaridentitet)
* **Första och sista namnet** (valfritt)
* **E-postadress** (valfritt)
* **Telefonnummer** (när du använder en röstsamtal eller SMS-autentisering)
* **Enhetstoken** (när du använder mobilappautentisering)
* **Autentiseringsläge**
* **Resultat för autentisering**
* **Multi-Factor Authentication-servernamn**
* **Multi-Factor Authentication-servern IP**
* **Klienten IP** (om tillgängligt)

hello valfria fält kan konfigureras i Multi-Factor Authentication-servern.

Hej verifiering resultatet (lyckades eller nekades) och hello orsak om den nekades lagras med hello autentiseringsdata. Dessa data är tillgängliga i autentisering och användningsrapporter.

## <a name="billing"></a>Fakturering
De flesta faktureringsfrågor kan lösas genom att referera tooeither hello [prissättning för multi-Factor Authentication](https://azure.microsoft.com/pricing/details/multi-factor-authentication/) eller hello dokumentation om [hur tooget Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md).

**F: är min organisation debiteras för att skicka hello telefonsamtal och SMS-meddelanden som används för autentisering?**

Nej, du debiteras inte för enskilda telefonsamtal placeras eller textmeddelanden skickas toousers via Azure Multi-Factor Authentication. Om du använder en MFA-leverantör per autentisering, debiteras du för varje autentisering men inte för hello-metod som används.

Användarna kan debiteras för hello telefonsamtal eller SMS de får bl.a tootheir personliga telefontjänst.

**F: hello per användare faktureringsmodell som tillämpas debiterar mig för alla aktiverade användare eller bara hello dem som utförde tvåstegsverifiering?**

Fakturering baseras på hello antalet användare som har konfigurerats toouse Multifaktorautentisering, oavsett om de utförs tvåstegsverifiering den månaden.

**F: hur fungerar Multi-Factor Authentication fakturering?**

När du skapar en per användare eller per autentisering MFA-leverantören faktureras organisationens Azure-prenumeration månadsvis baserat på användning. Fakturering modellen liknar toohow Azure debiterar för användning av virtuella datorer och webbplatser.

När du köper en prenumeration för Azure Multi-Factor Authentication betalar organisationen endast hello årlig licens avgift för varje användare. MFA licenser och Office 365, Azure AD Premium eller Enterprise Mobility + säkerhetspaket debiteras det här sättet. 

Mer information om alternativen i [hur tooget Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md).

**F: finns det en gratisversionen av Azure Multi-Factor Authentication?**

I vissa fall Ja.

Multi-Factor Authentication för administratörer för Azure erbjuder en delmängd av Azure MFA-funktioner utan kostnad för åtkomst tooMicrosoft onlinetjänster, inklusive hello Azure och Office 365-administratören portaler. Det här erbjudandet gäller endast tooglobal administratörer i Azure Active Directory-instanser som inte har hello fullständiga versionen av Azure MFA via en MFA-licens, ett paket eller en fristående förbrukningsbaserad provider. Om dina administratörer använda hello kostnadsfria versionen och sedan du köpte en fullständig version av Azure MFA, är alla globala administratörer utökade toohello betald version automatiskt.

Multi-Factor Authentication för Office 365-användare erbjuder en delmängd av Azure MFA-funktioner utan kostnad för åtkomst tooOffice 365-tjänster, inklusive Exchange Online och SharePoint Online. Det här erbjudandet gäller toousers som har en Office 365-licens som tilldelats, när hello motsvarande instansen av Azure Active Directory inte har hello fullständiga versionen av Azure MFA via en MFA-licens, ett paket eller en fristående förbrukningsbaserad provider.

**F: kan min organisation växla mellan per användare och per autentisering förbrukning fakturering när som helst?**

Om organisationen köper MFA som en fristående tjänst med förbrukningsbaserad fakturering kan välja du en faktureringsmodell som tillämpas när du skapar en MFA-leverantören. Du kan inte ändra hello faktureringsmodell som tillämpas när en MFA-provider har skapats. Du kan dock ta bort hello MFA-leverantör och sedan skapa en med en annan fakturering modell.

När en MFA-provider har skapats kan vara det länkade tooan Azure Active Directory (även kallat ”Azure AD-klient”). Om hello aktuella MFA-leverantören är länkade tooan Azure AD-klient, kan du på ett säkert sätt bort hello MFA-leverantör och skapar en som är länkade toohello samma Azure AD-klient. Alternativt, om du har tillräckligt med MFA, Azure AD Premium eller Enterprise Mobility + Security (EMS) licenser toocover alla användare som har aktiverats för MFA måste du kan ta bort hello MFA-leverantören helt och hållet.

Om MFA-leverantören är *inte* länkade tooan Azure AD-klient eller länka hello nya MFA-provider tooa olika Azure AD-klient, överförs inte användarinställningar och konfigurationsalternativ. Dessutom måste befintliga Azure MFA-servrar toobe återaktivera med lösenordsaktiveringsuppgifter som skapades genom hello nya MFA-leverantören. Återaktivera hello MFA servrar toolink dem toohello nya MFA-providern påverkar inte telefonsamtal och SMS-autentisering, men mobilappen meddelanden slutar fungera för alla användare tills de återaktivera hello mobila appar.

Mer information om MFA-leverantörer i [komma igång med en Azure leverantör av Multifaktorautent](multi-factor-authentication-get-started-auth-provider.md).

**F: kan min organisation växla mellan förbrukningsbaserad fakturering och prenumerationer (en licens-baserade model) när som helst?**

I vissa fall Ja.

Om din katalog har en *per användare* Azure Multi-Factor Authentication-providern, kan du lägga till MFA-licenser. Användare med licenser som räknas inte i hello per användare förbrukningsbaserad fakturering. Användare utan licenser kan fortfarande vara aktiverat för MFA via hello MFA-leverantör. Om du köper och tilldela licenser för alla användare som konfigurerats toouse Multifaktorautentisering, kan du ta bort hello Azure Multi-Factor Authentication-leverantör. Du kan alltid skapa en annan användares MFA-leverantör om du har fler användare än licenser i hello framtida.

Om din katalog har en *per autentisering* Azure Multi-Factor Authentication provider alltid debiteras du för varje autentisering, så länge hello MFA-leverantören är länkade tooyour prenumeration. Du kan tilldela MFA licenser toousers, men du fortfarande faktureras för varje begäran om identitetsverifiering tvåstegsverifiering, om det kommer från någon med en MFA-licens eller inte.

**F: kan organisationen ha toouse och synkronisera identiteter toouse Azure Multi-Factor Authentication?**

Om organisationen använder en förbrukningsbaserad fakturering modell, är Azure Active Directory valfritt, men krävs inte. Om MFA-leverantör inte är länkade tooan Azure AD-klient kan du bara distribuera Azure Multi-Factor Authentication-servern eller hello Azure Multi-Factor Authentication SDK lokalt.

Azure Active Directory krävs för hello licens modellen eftersom licenser läggs toohello Azure AD-klient när du köper och tilldela dem toousers i hello directory.

## <a name="manage-and-support-user-accounts"></a>Hantera och stöd för användarkonton

**F: Vad ska jag säga Mina användare toodo om de inte kan ta emot ett svar på telefonen eller inte har telefonen med dem?**

Alla användare konfigureras förhoppningsvis mer än en verifieringsmetod. Berätta tootry logga in igen, men väljer en annan verifieringsmetod på hello-inloggningssida.

Du kan ange användare-toohello [slutanvändarens felsökningsguiden](./end-user/multi-factor-authentication-end-user-troubleshoot.md).


**F: Vad gör jag om en av användarna inte kan hämta i tootheir konto?**

Du kan återställa hello användarens konto genom att göra dem toogo via hello registreringsprocessen igen. Lär dig mer om [hantera inställningar för användare och enhet med Azure Multi-Factor Authentication i molnet hello](multi-factor-authentication-manage-users-and-devices.md).

**F: Vad gör jag om en av Mina användare förlorar en telefon som använder applösenord?**

tooprevent obehörig åtkomst, ta bort alla hello användarens applösenord. När hello användare har en motsvarighet enhet, kan de återskapa hello lösenord. Lär dig mer om [hantera inställningar för användare och enhet med Azure Multi-Factor Authentication i molnet hello](multi-factor-authentication-manage-users-and-devices.md).

**F: Vad händer om en användare kan logga in toonon-webbläsarbaserade appar?**

Om din organisation använder fortfarande äldre klienter och [tillåtna hello användning av applösenord](multi-factor-authentication-whats-next.md#app-passwords), och användarna inte kan logga in toothese äldre klienter med sitt användarnamn och lösenord. I stället de behöver för[ställa in applösenord](./end-user/multi-factor-authentication-end-user-app-passwords.md). Användarna måste rensa (ta bort) sin inloggningsinformation, starta om hello appen och loggar sedan in med sina användarnamn och *applösenord* i stället för vanliga lösenordet.

Om din organisation inte har äldre klienter, bör inte användarna toocreate applösenord.

> [!NOTE]
> Modern autentisering för Office 2013-klienter
>
> Applösenord krävs endast för appar som inte stöder modern autentisering. Office 2013-klienter stöder modern autentiseringsprotokoll, men måste toobe konfigurerats. Nyare Office-klienterna har automatiskt stöd för modern autentiseringsprotokoll. Mer information finns i hello [Office 2013 modern autentisering förhandsversion meddelande](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).

**F: Mina användare säger att ibland de får inte hello SMS eller de svarar tootwo sätt textmeddelanden men hello verifiering timeout.**

Överföringen av textmeddelanden och mottagande av svar i dubbelriktad SMS är inte garanterat eftersom det finns fungerar faktorer som kan påverka hello tillförlitlighet hello-tjänsten. Dessa faktorer är hello land och hello mobiltelefon operatör hello signalstyrka.

Om användarna ofta har problem med att ta emot textmeddelanden på ett tillförlitligt sätt kan berätta toouse hello mobila app eller telefonsamtal metoden i stället. Hej mobilappar kan få meddelanden både över mobilnät och Wi-Fi-anslutningar. Hello mobila appar kan dessutom generera verifieringskoder även när hello enhet har ingen signal alls. hello Microsoft Authenticator-appen är tillgänglig för [Android](http://go.microsoft.com/fwlink/?Linkid=825072), [IOS](http://go.microsoft.com/fwlink/?Linkid=825073), och [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071).

Om du måste använda textmeddelanden, bör du använda enkelriktad SMS i stället för dubbelriktad SMS när det är möjligt. Enkelriktad SMS är mer tillförlitlig och förhindrar du att användarna medför globala SMS avgifter från svarande tooa textmeddelande som skickats från ett annat land.

**F: kan jag ändra hello tidsperiod Mina användare har tooenter hello Verifieringskod från meddelandet innan tidsgränsen uppnås för hello system?**

I vissa fall Ja. 

För enkelriktad SMS med Azure MFA-Server v7.0 eller högre, kan du konfigurera hello timeout inställningen genom att ange en registernyckel. När hello MFA Molntjänsten skickar hello SMS, returneras hello Verifieringskod (eller engångskod) toohello MFA-servern. Hej MFA-servern lagrar hello koden i minnet i 300 sekunder som standard. Om hello användare inte ange hello kod innan hello 300 sekunder, deras authentication nekas. Använd de här stegen toochange hello standardtimeout inställningen:

1. Gå tooHKLM\Software\Wow6432Node\Positive Networks\PhoneFactor.
2. Skapa en DWORD-registernyckel som kallas **pfsvc_pendingSmsTimeoutSeconds** och ange hello tid i sekunder som du vill hello Azure MFA-Server toostore enstaka lösenord.

>[!TIP] 
>Om du har flera servrar för MFA vet hello som bearbetas hello ursprungliga autentiseringsbegäran hello verifieringskoden som skickades toohello användare. När hello användare anger hello kod kan hello autentisering begäran toovalidate det måste skickas toohello samma server. Om validering av hello skickas tooa annan server, nekas hello authentication. 

Du kan konfigurera hello timeout-inställningen i hello MFA-hanteringsportalen för dubbelriktad SMS med Azure MFA-Server. Om användarna inte svarar toohello SMS inom tidsgränsen för hello definierade nekas sin autentisering. 

Du kan konfigurera hello timeout-inställningen för enkelriktad SMS med Azure MFA i hello molntjänster (inklusive hello tillägg för AD FS-kort eller hello Network Policy Server). Azure AD lagras hello Verifieringskod 180 sekunder. 

**F: kan jag använda maskinvarutoken med Azure Multi-Factor Authentication-servern?**

Om du använder Azure Multi-Factor Authentication-servern, kan du importera från tredje part öppna autentisering (OATH) utifrån enstaka lösenord (TOTP)-token och använda dem för tvåstegsverifiering.

Du kan använda ActiveIdentity token som TOTP OATH-token om du placerar hello hemlig nyckel i en CSV-fil och importera tooAzure Multi-Factor Authentication-servern. Du kan använda OATH-token med Active Directory Federation Services (ADFS), formulärbaserad autentisering för Internet Information Server (IIS) och RADIUS Remote Authentication Dial-In User Service () som hello klientsystemet kan acceptera hello indata från användaren.

Du kan importera från tredje part TOTP OATH-token med hello följande format:  

- Bärbar symmetriska nyckelbehållare (PSKC)  
- CSV om hello-filen innehåller ett serienummer, hemlig nyckel i Base-32-format och ett tidsintervall  

**F: kan jag använda Azure Multi-Factor Authentication-servern toosecure Terminal Services?**

Ja, men om du använder Windows Server 2012 R2 eller senare du bara skydda Terminal Services genom att använda fjärrskrivbordsgateway (RD Gateway).

Ändringarna i Windows Server 2012 R2 kan du ändra hur Azure Multi-Factor Authentication-servern ansluter toohello Local Security Authority (LSA) säkerhetspaketet i Windows Server 2012 och tidigare versioner. Versioner av Terminal Services i Windows Server 2012 eller tidigare, kan du [ett program med Windows-autentisering](multi-factor-authentication-get-started-server-windows.md#to-secure-an-application-with-windows-authentication-use-the-following-procedure). Om du använder Windows Server 2012 R2 måste RD Gateway.

**F: jag konfigurerats Uppringarens ID i MFA-Server, men användarna fortfarande ta emot Multi-Factor Authentication-samtal från en anonym anroparen.**

När Multi-Factor Authentication-samtal görs via hello offentliga telefonnätverk, dirigeras ibland de via en operatör som inte stöder anroparen-ID. Därför kan Uppringarens ID inte garanteras, även om hello Multi-Factor Authentication-systemet skickar alltid.

**F: Varför Mina användare som ska ange tooregister deras säkerhetsinformation?**
Det finns flera orsaker som kunde användare att ange tooregister deras säkerhetsinformation:

- hello användare har aktiverats för MFA av en administratör i Azure AD, men saknar säkerhetsinformation ännu registrerats för sitt konto.
- hello användaren har aktiverats för självbetjäning för återställning av lösenord i Azure AD. hello säkerhetsinformation kommer hjälpa dem återställa sina lösenord i hello framtida om de skulle glömma.
- hello användare komma åt ett program som har en villkorlig åtkomst princip toorequire MFA och tidigare har inte registrerats för MFA.
- hello användare som registrerar en enhet med Azure AD (inklusive Azure AD Join), och din organisation kräver MFA för registrering av enheten, men hello användaren tidigare har registrerats inte för MFA.
- hello användare genererar Windows Hello för företag i Windows 10 (vilket kräver MFA) och tidigare har inte registrerats för MFA.
- hello organisation har skapat och aktiverat en princip för MFA-registrering som har varit tillämpade toohello användare.
- hello användaren tidigare registrerad för MFA, men det har valt en verifieringsmetod som en administratör har inaktiverat sedan. hello användaren måste därför genomgå MFA-registrering igen tooselect en ny standard verifieringsmetod.


## <a name="errors"></a>Fel
**F: vad användarna gör om visas ett felmeddelande om ”verifieringsbegäran avser inte ett aktiverat konto” när du använder meddelanden för mobila appar?**

Berätta toofollow den här proceduren tooremove sitt konto från hello mobilapp, Lägg sedan till den igen:

1. Gå för[Azure portal profilen](https://account.activedirectory.windowsazure.com/profile/) och logga in med ditt organisationskonto.
2. Välj **ytterligare säkerhetsverifiering**.
3. Ta bort hello befintligt konto från hello mobila appar.
4. Klicka på **konfigurera**, och följ sedan hello instruktioner tooreconfigure hello mobila appar.

**F: vad användarna gör om visas ett felmeddelande om 0x800434D4L när du loggar in tooa icke-webbläsarprogram?**

Hej 0x800434D4L fel uppstår när du försöker toosign i tooa icke-webbläsarbaserade program installeras på en lokal dator som inte fungerar med konton som kräver tvåstegsverifiering.

En lösning för det här felet är toohave separata användarkonton för admin-relaterade och åtgärder för icke-administratörer. Senare kan du länka postlådor mellan din administratörskonto och icke-administratörskontot så att du kan logga in tooOutlook med ditt administratörskonto. Mer information om den här lösningen lär du dig hur för[och ger en administratör hello möjlighet tooopen och visa hello innehåll på en användares postlåda](http://help.outlook.com/141/gg709759.aspx?sl=1).

## <a name="next-steps"></a>Nästa steg
Om din fråga inte besvaras här, lämna den i hello kommentarer längst ned hello hello-sidan. Eller ytterligare alternativ för att få hjälp:

* Sök hello [Microsoft Support Knowledge Base](https://www.microsoft.com/en-us/Search/result.aspx?form=mssupport&q=phonefactor&form=mssupport) lösningar toocommon tekniska problem.
* Söka efter och bläddra tekniska frågor och svar från hello community eller egna fråga i hello [Azure Active Directory-forum](https://social.msdn.microsoft.com/Forums/azure/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).
* Om du är en äldre PhoneFactor-kund och du har frågor eller behöver hjälp med att återställa ett lösenord, använda hello [lösenordsåterställning](mailto:phonefactorsupport@microsoft.com) länka tooopen ett supportärende.
* Kontakta en supporttekniker via [stöd för Azure Multi-Factor Authentication Server (PhoneFactor)](https://support.microsoft.com/oas/default.aspx?prid=14947). När du kontaktar oss är det bra om du kan ange så mycket information om problemet som möjligt. Information som du kan ange innehåller hello sida där du såg hello fel, hello felkod, hello specifika sessions-ID och hello-ID för hello användare som såg hello-fel.
