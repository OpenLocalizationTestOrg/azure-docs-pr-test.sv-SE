---
title: "aaaAzure Active Directory Connect Health vanliga frågor och svar - Azure | Microsoft Docs"
description: "Det här avsnittet får du svar frågor om Azure AD Connect Health. Det här avsnittet ger frågor om hur du använder hello-tjänsten, inklusive hello fakturering modellen, funktioner, begränsningar och support."
services: active-directory
documentationcenter: 
author: billmath
manager: samueld
editor: curtand
ms.assetid: f1b851aa-54d7-4cb4-8f5c-60680e2ce866
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 3f15d9e9b557b09ef74ceff85806579dd51f2412
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-health-frequently-asked-questions"></a>Vanliga frågor och svar om Azure AD Connect Health
Den här artikeln innehåller svar toofrequently frågor (FAQ) och svar om Azure Active Directory (AD Azure) Connect Health. Dessa vanliga frågor och svar tar upp frågor om hur toouse hello-tjänsten, som innehåller hello fakturering modellen, funktioner, begränsningar och support.

## <a name="general-questions"></a>Allmänna frågor
**F: jag hantera flera Azure AD-kataloger. Hur växlar toohello har Azure Active Directory Premium?**

tooswitch mellan olika Azure AD innehavare väljer hello för närvarande inloggad **användarnamn** på hello övre högra hörnet och välj sedan hello lämpligt konto. Om hello konto inte anges här, väljer **logga ut**, och sedan använda hello global administratör autentiseringsuppgifterna för hello directory som har Azure Active Directory Premium aktiverat toosign i.

**F: vilken version av identiteten roller som stöds av Azure AD Connect Health?**

hello följande tabell visar hello roller och operativsystemversioner som stöds.

|Roll| Operativsystem / Version|
|--|--|
|Active Directory Federation Services (AD FS)| <ul> <li> Windows Server 2008 R2 </li><li> Windows Server 2012  </li> <li>Windows Server 2012 R2 </li> <li> Windows Server 2016  </li> </ul>|
|Azure AD Connect | Version 1.0.9125 eller högre|
|Active Directory Domain Services (AD DS)| <ul> <li> Windows Server 2008 R2 </li><li> Windows Server 2012  </li> <li>Windows Server 2012 R2 </li> <li> Windows Server 2016  </li> </ul>|

Observera att hello funktioner som tillhandahålls av hello-tjänsten kan skilja sig baserat på hello roll och hello-operativsystemet. Med andra ord kanske alla hello-funktioner inte tillgänglig för alla versioner av operativsystemet. Se hello funktionsbeskrivning mer information.

**F: hur många licenser gör jag behöver toomonitor min infrastruktur?**

* hello kräver första Connect Health Agent minst en Azure AD Premium-licens.
* Varje ytterligare registrerade agent kräver 25 ytterligare Azure AD Premium-licenser.
* Agentantal är likvärdiga toohello totala antalet agenter som registrerats över alla övervakade roller (AD FS, Azure AD Connect och AD DS).

Licensieringsinformation finns även på hello [priser för Azure AD-sidan](https://aka.ms/aadpricing).

Exempel:

| Registrerade agenter | Licenser som behövs | Exempel övervakningskonfiguration |
| ------ | --------------- | --- |
| 1 | 1 | 1 azure AD Connect-server |
| 2 | 26| 1 azure AD Connect-servern och domänkontrollanten för 1 |
| 3 | 51 | 1 server för active Directory Federation Services (AD FS), 1 AD FS-proxy och 1-domänkontrollant |
| 4 | 76 | 1 AD FS-servern, 1 AD FS-proxy och 2 domänkontrollanter |
| 5 | 101 | 1 azure AD Connect-servern, 1 AD FS-servern, 1 AD FS-proxy och 2-domänkontrollanter |


## <a name="installation-questions"></a>Frågor om installation

**F: Vad är hello effekten av installerar hello Azure AD Connect Health Agent på enskilda servrar?**

hello effekten av installerar hello-Microsoft Azure AD Connect Health Agent, AD FS-webbprogramproxyservrarna, Azure AD Connect (synkronisering)-servrar, domänkontrollanter är minimal med avseende toohello CPU, minne används, nätverkets bandbredd och lagring.

hello följande siffror är en uppskattning:

* CPU-förbrukning: ökning ~ 1-5%.
* Minnesförbrukning: too10% systemminne hello totalt.

> [!NOTE]
> Om hello agent inte kan kommunicera med Azure lagrar hello agent hello data lokalt för en definierad högsta gräns. hello agent skriver över hello ”” Cachedata på grundval av ”minst senast underhålls”.
>
>

* Bufferten för lokal lagring för Azure AD Connect Health-agenter: ~ 20 MB.
* För AD FS-servrar rekommenderar vi att du etablerar ett utrymme på 1 024 MB (1 GB) för hello AD FS-granskning kanal för Azure AD Connect Health-agenter tooprocess alla hello granskningsdata innan den skrivs över.

**F: måste jag tooreboot Mina servrar under hello installationen av hello Azure AD Connect Health-agenter?**

Nej. hello-installation av hello agenter kräver inte du tooreboot hello-servern. Installationen av vissa nödvändiga steg kräva en omstart av hello-server.

Exempelvis kräver installation av .NET Framework för 4.5 på Windows Server 2008 R2 servern startas om.

**F: kan Azure AD Connect Health arbete via en direkt HTTP-proxy?**

Ja. Du kan konfigurera en HTTP-proxy tooforward utgående HTTP-begäranden hello Health Agent toouse för pågående åtgärder.
Läs mer om [konfigurera HTTP-Proxy för Health-agenter](active-directory-aadconnect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy).

Om du behöver tooconfigure en proxy under agentregistreringen kan behöva du toomodify proxyinställningarna för Internet Explorer i förväg.

1. Öppna Internet Explorer > **inställningar** > **Internetalternativ** > **anslutningar** > **LAN-inställningar** .
2. Välj **använder en proxyserver för nätverket**.
3. Välj **Avancerat** om du har en annan proxyserver portar för HTTP och HTTPS/Secure.

**F: kan Azure AD Connect Health stöder grundläggande autentisering när du ansluter tooHTTP proxyservrar?**

Nej. En mekanism toospecify ett godtyckligt användarnamn och lösenord för grundläggande autentisering stöds inte för närvarande.

**F: vad brandväggsportar göra jag behöver tooopen för hello Azure AD Connect Health Agent toowork?**

Se hello [avsnittet krav](active-directory-aadconnect-health-agent-install.md#requirements) hello lista över portar i brandväggen och andra anslutningskrav.

**F: Varför visas två servrar med samma namn i hello Azure AD Connect Health-portalen hello?**

När du tar bort en agent från en server är hello-servern inte bort automatiskt från hello Azure AD Connect Health-portalen. Om du manuellt ta bort en agent från en server eller ta bort själva hello-servern måste ta bort toomanually hello serverpost från hello Azure AD Connect Health-portalen.

Du kan återavbilda en server eller skapa en ny server med hello samma information (till exempel datornamnet). Om du inte ta bort hello redan är registrerad server från hello Azure AD Connect Health-portalen, och du har installerat hello-agenten på hello ny server, kan du se två poster med hello samma namn.

I det här fallet manuellt ta bort hello post som tillhör toohello äldre server. hello-data för den här servern vara inaktuell.

## <a name="health-agent-registration-and-data-freshness"></a>Health Agent registrerings- och dokumentens

**F: Vad är vanliga orsaker till hello Health Agent-registreringsfel och hur felsöker problem?**

hello health-agenten kan misslyckas tooregister på grund av toohello följande möjliga orsaker:

* hello-agenten kan inte kommunicera med hello krävs slutpunkter eftersom en brandvägg blockerar trafik. Detta är särskilt vanligt på webbprogramproxyservrarna. Kontrollera att du har tillåtit utgående kommunikation toohello krävs slutpunkter och portar. Se hello [avsnittet krav](active-directory-aadconnect-health-agent-install.md#requirements) mer information.
* Utgående kommunikation är utsatts tooan SSL-kontroll av hello nätverksnivån. Detta medför hello certifikat att hello agenten använder toobe ersättas med hello inspektion server/entiteten och hello steg toocomplete hello agentregistreringen misslyckas.
* hello användaren har inte åtkomst tooperform hello registrering av hello agent. Globala administratörer har åtkomst som standard. Du kan använda [rollbaserad åtkomstkontroll](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control) toodelegate-tooother användare.

**F: Jag är få ett meddelande om att ”Hälsotjänstens data inte är in toodate”. Hur felsöker hello problemet?**

Azure AD Connect Health genererar hello avisering när den inte får alla hello datapunkter från hello-server i hello senaste två timmarna. Det kan finnas flera orsaker till den här aviseringen.

* hello-agenten kan inte kommunicera med hello krävs slutpunkter eftersom en brandvägg blockerar trafik. Detta är särskilt vanligt på webbprogramproxyservrarna. Kontrollera att du har tillåtit slutpunkter för utgående kommunikation toohello som krävs och portar. Se hello [avsnittet krav](active-directory-aadconnect-health-agent-install.md#requirements) mer information.
* Utgående kommunikation är utsatts tooan SSL-kontroll av hello nätverksnivån. Detta medför hello certifikat att hello agenten använder toobe ersättas med hello inspektion server/entiteten och hello misslyckas tooupload data toohello Azure AD Connect Health-tjänsten.
* Du kan använda hello anslutningskommando inbyggd hello agent. [Läs mer](active-directory-aadconnect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service).
* hello-agenter stöder också utgående anslutning via en oautentiserad HTTP-Proxy. [Läs mer](active-directory-aadconnect-health-agent-install.md##configure-azure-ad-connect-health-agents-to-use-http-proxy).

## <a name="operations-questions"></a>Operations frågor
**F: måste tooenable granskning på hello webbprogramproxyservrarna?**

Nej, granskning behöver inte aktiverad på hello webbprogramproxyservrarna toobe.

**F: hur Azure AD Connect Health-aviseringar löses?**

Azure AD Connect Health-aviseringar löses på ett lyckat tillstånd. Azure AD Connect Health-agenter identifieras och rapportera hello lyckade villkor toohello service med jämna mellanrum. Hello Undertryckning är några aviseringar baseras på tidpunkt. Med andra ord löses hello samma fel måste iakttas inom 72 timmar från varningsgenereringen automatiskt hello avisering.

**F: Jag är få ett meddelande om att ”Test autentiseringsbegäran (syntetisk transaktion) misslyckades tooobtain en token”. Hur felsöker hello problemet?**

Azure AD Connect Health för AD FS genererar aviseringen när hello Health Agent installeras på en AD FS-servern inte tooobtain en token som en del av en syntetisk transaktion som initieras av hello Health-agenten. hello Health-agenten använder hello lokala systemsammanhanget och försöker tooget en token för en självutfärdad förlitande part. Det här är en fångar in alla test tooensure som AD FS är i ett tillstånd för att utfärda token.

Det här testet misslyckas oftast eftersom hello Health-agenten är tooresolve hello AD FS-servergruppsnamnet. Detta kan inträffa om hello AD FS-servrar som finns bakom en belastningsutjämnare för nätverket och hello begäran hämtar initieras från en nod som är bakom hello belastningsutjämnare (som tooa skillnad från vanliga klient som är framför hello belastningsutjämnare). Detta kan åtgärdas genom att uppdatera hello ”” värdfilen som finns under ”C:\Windows\System32\drivers\etc” tooinclude hello IP-adress hello AD FS-server eller en loopback-IP-adress (127.0.0.1) för hello AD FS-servergruppnamn (t.ex sts.contoso.com). Att lägga till hello värdfilen kommer kortslutning hello nätverksanrop, vilket ger hello Health Agent tooget hello token.

**F: Jag har fått ett e-postmeddelande som anger mina datorer inte korrigeras för hello senaste ransomeware attacker. Varför fick e-postmeddelandet?**

Azure AD Connect Health service genomsöks alla hello datorer som den övervakar tooensure hello krävs korrigeringsfiler har installerats. hello e-postmeddelandet skickades toohello innehavaradministratörer Om minst en dator inte hade hello kritiska korrigeringar. Hej följande logik har använt toomake den bedömningen.
1. Hitta alla installerade på datorn hello hello snabbkorrigeringar.
2. Kontrollera att minst en av hello snabbkorrigeringar från hello definierat lista finns.
3. Om Ja, är hello datorn skyddad. Om inte, hello datorn är i fara för hello angrepp.

Du kan använda följande PowerShell-skriptet tooperform hello kontrollen manuellt. Den implementerar hello ovan logik.

```
Function CheckForMS17-010 ()
{
    $hotfixes = "KB3205409", "KB3210720", "KB3210721", "KB3212646", "KB3213986", "KB4012212", "KB4012213", "KB4012214", "KB4012215", "KB4012216", "KB4012217", "KB4012218", "KB4012220", "KB4012598", "KB4012606", "KB4013198", "KB4013389", "KB4013429", "KB4015217", "KB4015438", "KB4015546", "KB4015547", "KB4015548", "KB4015549", "KB4015550", "KB4015551", "KB4015552", "KB4015553", "KB4015554", "KB4016635", "KB4019213", "KB4019214", "KB4019215", "KB4019216", "KB4019263", "KB4019264", "KB4019472", "KB4015221", "KB4019474", "KB4015219", "KB4019473"

    #checks hello computer it's run on if any of hello listed hotfixes are present
    $hotfix = Get-HotFix -ComputerName $env:computername | Where-Object {$hotfixes -contains $_.HotfixID} | Select-Object -property "HotFixID"

    #confirms whether hotfix is found or not
    if (Get-HotFix | Where-Object {$hotfixes -contains $_.HotfixID})
    {
        "Found HotFix: " + $hotfix.HotFixID
    } else {
        "Didn't Find HotFix"
    }
}

CheckForMS17-010

```



## <a name="related-links"></a>Relaterade länkar
* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Installation av Azure AD Connect Health Agent](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect Health-åtgärder](active-directory-aadconnect-health-operations.md)
* [Använda Azure AD Connect Health med AD FS](active-directory-aadconnect-health-adfs.md)
* [Använda Azure AD Connect Health för synkronisering](active-directory-aadconnect-health-sync.md)
* [Använda Azure AD Connect Health med AD DS](active-directory-aadconnect-health-adds.md)
* [Versionshistorik för Azure AD Connect Health](active-directory-aadconnect-health-version-history.md)
