---
title: "Azure AD Connect-synkronisering: ändra hello Azure AD Connect Sync-tjänstkontot | Microsoft Docs"
description: "Här avsnittet beskrivs hello krypteringsnyckeln och hur tooabandon det efter hello lösenord har ändrats."
services: active-directory
keywords: "Azure AD-synkroniseringstjänstkontot, lösenord"
documentationcenter: 
author: cychua
manager: femila
editor: 
ms.assetid: 76b19162-8b16-4960-9e22-bd64e6675ecc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 11948ac4662f722e4f684ef6c9b9ccdc6387e60f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="changing-hello-azure-ad-connect-sync-service-account-password"></a>Ändra hello Azure AD Connect sync kontolösenord
Om du ändrar hello Azure AD Connect sync kontolösenord kommer hello-synkroniseringstjänsten inte att kunna starta korrekt förrän du har avbrutna hello krypteringsnyckeln och initieras hello Azure AD Connect sync kontolösenord. 

Azure AD Connect, som en del av hello synkroniseringstjänster använder en kryptering viktiga toostore hello lösenord hello AD DS och Azure AD-tjänstkonton.  Dessa konton krypteras innan de lagras i hello-databasen. 

hello krypteringsnyckeln är säkrad via [Windows Data Protection (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx). DPAPI skyddar hello krypteringsnyckeln med hjälp av hello **lösenordet för tjänstkontot för hello Azure AD Connect sync**. 

Om du behöver toochange hello tjänstkontots lösenord kan du använda hello procedurer i [Abandoning hello Azure AD Connect Sync krypteringsnyckeln](#abandoning-the-azure-ad-connect-sync-encryption-key) tooaccomplish detta.  De här procedurerna bör också användas om du behöver tooabandon hello krypteringsnyckeln av någon anledning.

##<a name="issues-that-arise-from-changing-hello-password"></a>Problem som uppstår när du ändrar hello lösenord
Det finns två saker som behöver toobe när du ändrar hello kontolösenord.

Du måste först toochange hello lösenord under hello Windows Service Control Manager.  Tills problemet är löst visas följande fel:


- Om du försöker toostart hello synkroniseringstjänsten i Windows Service Control Manager felmeddelandet hello ”**Windows kunde inte starta hello Microsoft Azure AD Sync-tjänsten på den lokala datorn**”. **Fel 1069: hello-tjänsten kunde inte starta på grund av tooa misslyckades.** "
- Under Windows Loggboken hello systemets händelselogg innehåller ett fel med **händelse-ID 7038** och meddelandet ”**hello ADSync-tjänsten har toolog på som hello konfigurerat lösenord på grund av toohello följande fel: hello-användarnamnet eller lösenordet är felaktigt.** "

Andra kan vissa villkor kan om hello lösenordet uppdateras hello synkroniseringstjänsten inte längre hämta hello krypteringsnyckeln via DPAPI. Utan hello krypteringsnyckel hello synkroniseringstjänsten inte kan dekryptera hello lösenord krävs toosynchronize till och från lokala AD och Azure AD.
Du ser fel som:

- Under Windows Service Control Manager om du försöker toostart hello-synkroniseringstjänsten och det går inte att hämta hello krypteringsnyckeln misslyckas med felet ”** Windows kunde inte starta hello Microsoft Azure AD Sync på den lokala datorn. Mer information finns i händelseloggen för hello System. Om detta är en icke-Microsoft-tjänst, kontakta leverantören för hello-tjänsten och hittar tooservice-specifika felkoden **-21451857952 *** ”.
- Under Windows Loggboken hello programmets händelselogg innehåller ett fel med **händelse-ID 6028** och felmeddelande *”**hello krypteringsnyckeln på servern kan inte nås.* *”*

tooensure att du inte får dessa fel följa hello procedurerna i [Abandoning hello Azure AD Connect Sync krypteringsnyckeln](#abandoning-the-azure-ad-connect-sync-encryption-key) när du ändrar hello lösenord.
 
## <a name="abandoning-hello-azure-ad-connect-sync-encryption-key"></a>Överges hello Azure AD Connect Sync-krypteringsnyckeln
>[!IMPORTANT]
>hello följande procedurer gäller endast tooAzure AD Connect build 1.1.443.0 eller äldre.

Använd följande procedurer tooabandon hello krypteringsnyckeln hello.

### <a name="what-toodo-if-you-need-tooabandon-hello-encryption-key"></a>Vilka toodo om du behöver tooabandon hello krypteringsnyckeln

Om du behöver tooabandon hello krypteringsnyckeln används hello följande procedurer tooaccomplish.

1. [Avbryt hello befintliga krypteringsnyckel](#abandon-the-existing-encryption-key)

2. [Ange hello lösenord för hello AD DS-konto](#provide-the-password-of-the-ad-ds-account)

3. [Initiera om hello lösenordet för hello Azure AD sync-kontot](#reinitialize-the-password-of-the-azure-ad-sync-account)

4. [Starta hello-synkroniseringstjänsten](#start-the-synchronization-service)

#### <a name="abandon-hello-existing-encryption-key"></a>Avbryt hello befintliga krypteringsnyckel
Avbryt hello befintliga krypteringsnyckel så att nya krypteringsnyckeln kan skapas:

1. Logga in tooyour Azure AD Connect-servern som administratör.

2. Starta en ny PowerShell-session.

3. Navigera toofolder:`$env:Program Files\Microsoft Azure AD Sync\bin\`

4. Kör hello-kommando:`./miiskmu.exe /a`

![Azure AD Connect Sync kryptering viktiga verktyg](media/active-directory-aadconnectsync-encryption-key/key5.png)

#### <a name="provide-hello-password-of-hello-ad-ds-account"></a>Ange hello lösenord för hello AD DS-konto
Som hello befintliga lösenord som lagrats i hello databasen kan inte längre dekrypteras så behöver tooprovide hello synkroniseringstjänsten med hello lösenord för hello AD DS-konto. hello synkroniseringstjänsten krypterar hello lösenord med hello nya krypteringsnyckeln:

1. Starta hello Synchronization Service Manager (START → synkroniseringstjänsten).
</br>![Synkronisering av Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)  
2. Gå toohello **kopplingar** fliken.
3. Välj hello **AD-koppling** som motsvarar tooyour lokala AD. Upprepa hello följande steg för dem om du har flera AD-koppling.
4. Under **åtgärder**väljer **egenskaper**.
5. I popup-dialogrutan hello väljer **ansluta tooActive Directory-skog**:
6. Ange hello lösenord för hello AD DS-konto i hello **lösenord** textruta. Om du inte vet lösenordet måste du ange den tooa känt värde innan du utför det här steget.
7. Klicka på **OK** toosave hello nytt lösenord och Stäng hello popup-fönstret.
![Azure AD Connect Sync kryptering viktiga verktyg](media/active-directory-aadconnectsync-encryption-key/key6.png)

#### <a name="reinitialize-hello-password-of-hello-azure-ad-sync-account"></a>Initiera om hello lösenordet för hello Azure AD sync-kontot
Du kan inte direkt ange hello lösenordet för hello Azure AD-tjänsten konto toohello synkroniseringstjänsten. I så fall måste toouse hello cmdlet **Lägg till ADSyncAADServiceAccount** tooreinitialize hello Azure AD-tjänstkontot. hello cmdlet återställer lösenordet för hello och gör den tillgänglig toohello synkroniseringstjänsten:

1. Starta en ny PowerShell-session på hello Azure AD Connect-servern.
2. Kör cmdlet `Add-ADSyncAADServiceAccount`.
3. I hello popup-fönstret, anger du autentiseringsuppgifter för hello Azure AD Global administratör för Azure AD-klienten.
![Azure AD Connect Sync kryptering viktiga verktyg](media/active-directory-aadconnectsync-encryption-key/key7.png)
4. Om det lyckas visas hello PowerShell-Kommandotolken.

#### <a name="start-hello-synchronization-service"></a>Starta hello-synkroniseringstjänsten
Hello synkroniseringstjänsten har åtkomst toohello krypteringsnyckel och alla hello lösenord som behövs, kan du starta om tjänsten hello i hello Windows Service Control Manager:


1. Gå tooWindows Service Control Manager (START → Services).
2. Välj **Microsoft Azure AD Sync** och klickar på Starta om.

## <a name="next-steps"></a>Nästa steg
**Översiktsavsnitt**

* [Azure AD Connect-synkronisering: Förstå och anpassa synkronisering](active-directory-aadconnectsync-whatis.md)

* [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md)
