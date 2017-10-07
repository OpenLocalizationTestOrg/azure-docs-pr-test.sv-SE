---
title: "Azure Active Directory Connect: Vanliga frågor och svar - | Microsoft Docs"
description: "Den här sidan har vanliga frågor om Azure AD Connect."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
ms.assetid: 4e47a087-ebcd-4b63-9574-0c31907a39a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 39c0b127d3dcf6f45607ad8b4647a9ad79dfc732
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions-for-azure-active-directory-connect"></a>Vanliga frågor om Azure Active Directory Connect

## <a name="general-installation"></a>Allmän installation
**F: installationen fungerar om hello Azure AD Global administratör har aktiverat 2FA?**  
Med hello versioner från februari 2016 stöds detta.

**F: finns det ett sätt tooinstall Azure AD Connect obevakad?**  
Det är bara stöds tooinstall Azure AD Connect med hello installationsguiden. En obevakad och tyst installation stöds inte.

**F: Jag har en skog där en domän inte kan kontaktas. Hur installerar Azure AD Connect?**  
Med hello versioner från februari 2016 stöds detta.

**F: kan hello AD DS health agent arbete på server core?**  
Ja. Du kan slutföra hello registreringen med hello följande PowerShell-cmdlet när du har installerat hello agent: 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

## <a name="network"></a>Nätverk
**F: Jag har en brandvägg, nätverksenhet eller något annat som begränsar hello maximal tid anslutningar kan vara öppen i nätverket. Hur lång tid ska min klient sida timeout tröskelvärdet vara när du använder Azure AD Connect?**  
Alla nätverksprogramvara fysiska enheter eller något annat som gränser hello maximal tid anslutningar kan förbli öppen bör använda ett tröskelvärde på minst 5 minuter (300 sekunder) för anslutningen mellan hello servern där hello Azure AD Connect-klienten är installerad och Azure Active Directory. Detta gäller även tooall som tidigare lanserats synkroniseringsverktyg för Microsoft Identity.

**F: är SLDs (enkel etikett domäner) stöds?**  
Nej, Azure AD Connect har inte stöd för lokala skogar och domäner med SLDs.

**F: är ”prickad” NetBios-namnet stöds?**  
Nej, Azure AD Connect har inte stöd för lokala skogar och domäner där hello NetBios-namn innehåller en punkt ””. i hello namn.

## <a name="federation"></a>Federation
**F: Vad gör jag om jag får ett e-postmeddelande som ber mig toorenew mitt Office 365-certifikat**  
Använda hello vägledningen som beskrivs i hello [förnya certifikat](active-directory-aadconnect-o365-certs.md) avsnitt om hur toorenew hello certifikat.

**F: Jag har ”Uppdatera automatiskt förlitande part” för O365 förlitande part. Måste jag tootake någon åtgärd när min certifikatet för tokensignering automatiskt rullar över?**  
Använda hello vägledningen som beskrivs i artikel hello [förnya certifikat](active-directory-aadconnect-o365-certs.md).

## <a name="environment"></a>Miljö
**F: är it stöds toorename hello server när du har installerat Azure AD Connect?**  
Nej. Ändra servernamnet för hello blir orsak hello sync motorn toonot kan tooconnect toohello SQL-databas och hello-tjänsten inte kan toostart.

## <a name="identity-data"></a>Identitetsdata
**F: hello UPN (userPrincipalName) attribut i Azure AD matchar inte hello lokala UPN - varför?**  
Finns i följande artiklar:

* [Användarnamnen i Office 365, Azure eller Intune inte matchar hello lokal UPN eller alternativ inloggnings-ID](https://support.microsoft.com/en-us/kb/2523192)
* [Ändringarna synkroniserats inte med hello Azure Active Directory Sync tool när du har ändrat hello UPN-namnet för en användare konto toouse en annan federerad domän](https://support.microsoft.com/en-us/kb/2669550)

Du kan också konfigurera Azure AD tooallow hello sync motorn tooupdate hello userPrincipalName som beskrivs i [Azure AD Connect sync tjänstens funktioner](active-directory-aadconnectsyncservice-features.md).

**F: är it stöds toosoft matchar lokala AD-gruppen/kontakt-objekt med den befintliga Azure AD-grupp/kontakta objekt?**  
Nej, detta stöds för närvarande inte.

**F: är it stöds toomanually ImmutableId attributet på befintliga Azure AD-gruppen/kontakta objekt toohard matchar den tooon lokala AD-gruppen/kontakt objekt?**  
Nej, detta stöds för närvarande inte.



## <a name="custom-configuration"></a>Anpassad konfiguration
**F: var finns hello PowerShell-cmdlets för Azure AD Connect dokumenterade?**  
Andra PowerShell-cmdletar finns i Azure AD Connect stöds inte för kundens användning med hello undantag av hello-cmdletarna som beskrivs i den här platsen.

**F: kan jag använda ”Server exportservern/importera” hittades i *Synchronization Service Manager* toomove konfiguration mellan servrar?**  
Nej. Det här alternativet kommer inte att hämta alla konfigurationsinställningar och ska inte användas. Du bör i stället hello guiden toocreate hello grundläggande konfigurationen på andra hello-server och använda hello sync regeln editor toogenerate PowerShell-skript toomove en anpassad regel mellan servrar. Se [svänga migrering](active-directory-aadconnect-upgrade-previous-version.md#swing-migration).

**F: kan cachelagras lösenord för hello Azure-inloggning och kan detta förhindras eftersom den innehåller ett lösenord indataelementet med hello autocomplete = ”false” attributet?**</br>
Vi stöder för närvarande inte ändra hello HTML-attribut för hello inkommande lösenordsfält, inklusive hello autocomplete-tagg. Vi arbetar på en funktion som tillåter för anpassad Javascript som tillåter tooadd eventuella attributfält toohello lösenord. Detta ska vara tillgängliga senare del av 2017.

**F: på hello Azure-inloggningssida visas användarnamn för användare som har tidigare har loggat in.  Det här beteendet stängas av?**</br>
Vi stöder för närvarande inte ändra hello HTML-attribut för hello-inloggningssida. Vi arbetar på en funktion som tillåter för anpassad Javascript som tillåter tooadd eventuella attributfält toohello lösenord. Detta ska vara tillgängliga senare del av 2017.

**F: finns det ett sätt tooprevent samtidiga sessioner?**</br>
Nej.



## <a name="troubleshooting"></a>Felsökning
**F: hur kan jag få hjälp med Azure AD Connect?**

[Sök hello Microsoft Knowledge Base (KB)](https://www.microsoft.com/en-us/Search/result.aspx?q=azure%20active%20directory%20connect&form=mssupport)

* Sök hello Microsoft Knowledge Base (KB) för tekniska lösningar toocommon reparations problem med stöd för Azure AD Connect.

[Microsoft Azure Active Directory-forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

* Du kan söka och Bläddra för tekniska frågor och svar från hello community eller egna fråga genom att klicka på [här](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).

[Teknisk support för Azure AD Connect](https://manage.windowsazure.com/?getsupport=true)

* Använd den här länken tooget support via hello Azure-portalen.

