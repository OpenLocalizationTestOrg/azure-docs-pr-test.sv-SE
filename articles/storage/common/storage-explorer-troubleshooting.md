---
title: "aaaAzure Lagringsutforskaren felsökningsguiden | Microsoft Docs"
description: "Översikt över hello två felsökningsfunktionen Azure"
services: virtual-machines
documentationcenter: 
author: Deland-Han
manager: cshepard
editor: 
ms.assetid: 
ms.service: virtual-machines
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: delhan
ms.openlocfilehash: 5152f70418707d65c0a4bce9a916336829956219
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-explorer-troubleshooting-guide"></a>Azure Lagringsutforskaren felsökningsguiden

## <a name="introduction"></a>Introduktion

Microsoft Azure Lagringsutforskaren (förhandsversion) är en fristående app som du kan använda tooeasily fungerar med Azure Storage-data i Windows, macOS och Linux. hello app kan ansluta toStorage konton finns i Azure, statliga moln och Azure-stacken.

Den här guiden beskrivs lösningar på vanliga problem som visas i Lagringsutforskaren.

## <a name="sign-in-issues"></a>Logga in problem

Innan du fortsätter försök att starta om programmet och se om hello problem kan åtgärdas.

### <a name="error-self-signed-certificate-in-certificate-chain"></a>Fel: Självsignerat certifikat i certifikatkedjan

Det finns flera skäl till varför detta fel kan uppstå och två vanligaste orsakerna till hello är följande:

1. hello app är anslutna via en ”transparent proxy”, vilket innebär att en server (till exempel företagets servern) avlyssna HTTPS-trafik, att dekryptera den och kryptera den med hjälp av ett självsignerat certifikat.

2. Du kör ett program, till exempel antivirusprogram, vilket är att injicera ett självsignerat SSL-certifikat i hello HTTPS-meddelanden som visas.

När Lagringsutforskaren påträffar ett problem med hello, kan den inte längre vet om emot HTTPS hälsningsmeddelande har ändrats. Om du har en kopia av hello självsignerade certifikat, kan du låta Lagringsutforskaren litar på den. Om du är osäker på som är att injicera hello certifikatet följer du dessa steg toofind den:

1. Installera öppna SSL

    - [Windows](https://slproweb.com/products/Win32OpenSSL.html) (alla hello lätta versionerna bör vara tillräckligt)

    - Mac- och Linux: ska ingå i ditt operativsystem

2. Kör öppna SSL

    - Windows: öppna hello installationskatalogen, klicka på **/bin/**, och dubbelklicka sedan på **openssl.exe**.
    - Mac- och Linux: kör **openssl** från en terminal.

3. Köra s_client - showcerts-ansluta microsoft.com:443

4. Leta efter självsignerade certifikat. Om du inte vet vilket är självsignerade letar var som helst hello ämne (”s:”) och utfärdaren (”i:”) är hello samma.

5. När du har hittat självsignerade certifikat för var och en, kopiera och klistra in allt från och med **---BEGIN CERTIFICATE---** för**---END CERTIFICATE---** tooa nya .cer-fil.

6. Öppna Lagringsutforskaren, klicka på **redigera** > **SSL-certifikat** > **Importera certifikat**, och sedan använda hello filen Väljaren toofind, Välj, och öppna hello CER-filer som du skapade.

Om du inte hittar någon självsignerade certifikat med hjälp av hello senare steg kan du kontakta oss via hello feedbackverktyg för mer hjälp.

### <a name="unable-tooretrieve-subscriptions"></a>Det går inte tooretrieve prenumerationer

Om du tooretrieve dina prenumerationer när du logga in, följer du dessa steg tootroubleshoot problemet:

- Kontrollera att ditt konto har åtkomst toohello prenumerationer loggar in på hello Azure-portalen.

- Kontrollera att du har loggat in med hjälp av hello rätt miljö (Azure, Azure Kina, Tyskland Azure, Azure som tillhör amerikanska myndigheter eller anpassad miljö-/ Azure-stacken).

- Om du är bakom en proxyserver kan du kontrollera att du har konfigurerat korrekt hello Lagringsutforskaren proxy.

- Försök att ta bort och readding hello-konto.

- Försök att ta bort hello följande filer från din rotkatalog (det vill säga C:\Users\ContosoUser) och sedan lägga till hello konto:

    - .adalcache

    - .devaccounts

    - .extaccounts

- Titta på hello developer-konsolen (genom att trycka på F12) när du loggar in för eventuella felmeddelanden:

![Utvecklingsverktyg](./media/storage-explorer-troubleshooting/4022501_en_2.png)

### <a name="unable-toosee-hello-authentication-page"></a>Det går inte toosee hello autentiseringssidan

Om du toosee hello autentiseringssidan, följer du dessa steg tootroubleshoot problemet:

- Beroende på anslutningens hello hastighet, kan det ta en stund innan hello-inloggningssida tooload, vänta minst en minut innan du stänger dialogrutan hello.

- Om du är bakom en proxyserver kan du kontrollera att du har konfigurerat korrekt hello Lagringsutforskaren proxy.

- Visa hello developer-konsolen genom att trycka på F12 för hello. Titta på hello svar från hello developer-konsolen och se om du hittar en ledtråd för varför autentisering fungerar inte.

### <a name="cannot-remove-account"></a>Det går inte att ta bort kontot

Om du tooremove ett konto, eller om hello återautentisera länken inte göra något, följer du dessa steg tootroubleshoot problemet:

- Försök att ta bort hello följande filer från din rotkatalog och readding hello konto:

    - .adalcache

    - .devaccounts

    - .extaccounts

- Om du vill tooremove SAS kopplade lagringsresurser, tar du bort hello följande filer:

    - %AppData%/StorageExplorer mapp för Windows

    - /Users/ < ditt_namn >/bibliotek/Flersvalsstart SUpport/StorageExplorer för Mac

    - ~/.config/StorageExplorer för Linux

> [!NOTE]
>  Du måste tooreenter dina autentiseringsuppgifter om du tar bort dessa filer.

## <a name="proxy-issues"></a>Proxy-problem

Kontrollera först att hello följande information som du angett är korrekta:

- Hej Proxyserver och port tal

- Användarnamn och lösenord om det krävs av hello-proxy

### <a name="common-solutions"></a>Vanliga lösningar

Om du fortfarande har problem, följer du dessa steg tootroubleshoot dem:

- Om du kan ansluta toohello Internet utan att använda proxyservern kan du kontrollera att Lagringsutforskaren fungerar utan aktiverade proxyinställningar. Om så är fallet hello kan det finnas ett problem med proxyinställningarna. Arbeta med dina proxy administratören tooidentify hello problem.

- Kontrollera att andra program som använder hello proxyserver fungerar som förväntat.

- Kontrollera att du kan ansluta med hjälp av webbläsaren toohello Microsoft Azure-portalen

- Kontrollera att du kan få svar från dina Tjänsteslutpunkter. Ange en slutpunkt-URL i webbläsaren. Om du kan ansluta bör du få ett InvalidQueryParameterValue eller liknande XML-svaret.

- Om någon annan också använder Lagringsutforskaren med proxyservern, kontrollera att de kan ansluta. Om de kan ansluta kanske toocontact proxy server administratören.

### <a name="tools-for-diagnosing-issues"></a>Verktyg för att diagnostisera problem

Om du har nätverk verktyg, till exempel Fiddler för Windows kan kanske du kan toodiagnose hello problem på följande sätt:

- Om du har toowork via din proxyserver kan kanske du tooconfigure ditt nätverk verktyget tooconnect via hello proxy.

- Kontrollera hello-portnumret som används av ditt nätverk.

- Ange hello lokala Värdadressen och hello nätverk verktygets portnummer som proxyinställningarna i Lagringsutforskaren. Om den här isdone korrekt, ditt nätverk verktyget börjar loggning nätverksbegäranden med Lagringsutforskaren toomanagement och tjänsten slutpunkter. Ange till exempel https://cawablobgrs.blob.core.windows.net/ för blob-slutpunkten i en webbläsare och du får ett svar liknar hello följande, vilket tyder på hello resursen finns, även om du inte kommer åt den.

![kodexempel](./media/storage-explorer-troubleshooting/4022502_en_2.png)

### <a name="contact-proxy-server-admin"></a>Kontakta serveradministratören för proxy

Om proxyinställningarna är rätt måste du kanske toocontact administratören proxy-server och

- Se till att proxyservern inte blockerar trafik tooAzure hanterings- eller slutpunkter.

- Kontrollera hello autentiseringsprotokoll som används av proxyservern. Lagringsutforskaren stöder för närvarande inte NTLM-proxyservrar.

## <a name="unable-tooretrieve-children-error-message"></a>Felmeddelandet ”Det gick inte tooRetrieve underordnade”

Om du är ansluten tooAzure via en proxyserver kan du verifiera att proxyinställningarna är korrekta. Om du har beviljat åtkomst tooa resurs från hello ägare hello prenumeration eller konto, kontrollera att du har läst eller visa en lista med behörigheter för resursen.

### <a name="issues-with-sas-url"></a>Problem med SAS-URL
Om du ansluter tooa tjänsten med hjälp av en SAS-URL och det här felet:

- Kontrollera att hello URL innehåller hello behörighet tooread eller lista resurser.

- Kontrollera att hello URL inte har upphört att gälla.

- Om hello SAS-URL är baserat på en åtkomstprincip, kontrollerar du att hello åtkomstprincip inte har återkallats.

## <a name="next-steps"></a>Nästa steg

Om ingen av hello lösningar fungerar för dig skicka din via hello feedbackverktyg med din e-post och så många detaljer om hello problem som du kan, så att vi kan kontakta dig för att åtgärda hello problem.

toodo, genom att välja **hjälp** -menyn och klicka sedan på **skicka Feedback**.

![Feedback](./media/storage-explorer-troubleshooting/4022503_en_1.png)
