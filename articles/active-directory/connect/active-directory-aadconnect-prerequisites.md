---
title: 'Azure AD Connect: Krav och maskinvara | Microsoft Docs'
description: "Det här avsnittet beskrivs kraven för hello och hello maskinvarukrav för Azure AD Connect"
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 91b88fda-bca6-49a8-898f-8d906a661f07
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 2ec8b5da9440c92e8f9d6702d5e5e20de952b588
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="prerequisites-for-azure-ad-connect"></a>Krav för Azure AD Connect
Det här avsnittet beskrivs kraven för hello och hello maskinvarukrav för Azure AD Connect.

## <a name="before-you-install-azure-ad-connect"></a>Innan du installerar Azure AD Connect
Innan du installerar Azure AD Connect, finns det några saker som du behöver.

### <a name="azure-ad"></a>Azure AD
* En Azure-prenumeration eller en [Azure-provprenumeration](https://azure.microsoft.com/pricing/free-trial/). Den här prenumerationen har bara krävs för att komma åt hello Azure-portalen och inte med Azure AD Connect. Om du använder PowerShell eller Office 365, behöver du inte ha en Azure-prenumeration toouse Azure AD Connect. Om du har en licens för Office 365 kan du också använda hello Office 365-portalen. Med en betald Office 365-licens, kan du också få till hello Azure-portalen från hello Office 365-portalen.
  * Du kan också använda hello [Azure-portalen](https://portal.azure.com). Den här portalen kräver inte en Azure AD-licens.
* [Lägga till och verifiera domänen hello](../active-directory-domains-add-azure-portal.md) du planerar toouse i Azure AD. Om du planerar toouse contoso.com för användarna och sedan kontrollera att den här domänen har verifierats och du bara använder inte hello contoso.onmicrosoft.com standarddomän.
* En Azure AD-klient kan av standardobjekt 50 kB. När du verifierar din domän är hello gränsen bättre too300k objekt. Om du behöver ännu mer objekt i Azure AD, måste tooopen en supportgränsen case toohave hello ökade ytterligare. Om du behöver mer än 500 kB objekt behöver du en licens, till exempel Office 365, Azure AD Basic, Azure AD Premium eller Enterprise Mobility and Security.

### <a name="prepare-your-on-premises-data"></a>Förbereda dina lokala data
* Använd [IdFix](https://support.office.com/article/Install-and-run-the-Office-365-IdFix-tool-f4bd2439-3e41-4169-99f6-3fabdfa326ac) tooidentify fel som dubbletter och formatering problem i din katalog innan du synkroniserar tooAzure AD och Office 365.
* Granska [valfria sync-funktioner som du kan aktivera i Azure AD](active-directory-aadconnectsyncservice-features.md) och utvärdera vilka funktioner som du bör aktivera.

### <a name="on-premises-active-directory"></a>Lokalt Active Directory
* hello AD schemat version och skogens funktionsnivå måste vara Windows Server 2003 eller senare. hello-domänkontrollanter kan köra någon version så länge hello schema- och krav är uppfyllda.
* Om du planerar toouse hello funktionen **tillbakaskrivning av lösenord**, hello domänkontrollanter måste vara på Windows Server 2008 (med senaste SP) eller senare. Om din domänkontrollanter är 2008 (pre-R2) och du måste också använda [snabbkorrigering KB2386717](http://support.microsoft.com/kb/2386717).
* hello-domänkontrollanten som används av Azure AD måste vara skrivbar. Det är **stöds inte** toouse en RODC (skrivskyddad domänkontrollant) och Azure AD Connect inte följer någon skriva omdirigeringar.
* Det är **stöds inte** toouse lokala skogar och domäner med SLDs (enkel etikett domäner).
* Det är **stöds inte** toouse lokala skogar och domäner med ”prickad” (med en punkt i namnet ””.) NetBIOS-namn.
* Det rekommenderas för[aktivera hello Active Directory-Papperskorgen](active-directory-aadconnectsync-recycle-bin.md).

### <a name="azure-ad-connect-server"></a>Azure AD Connect-servern
* Azure AD Connect kan inte installeras på Small Business Server eller Windows Server Essentials. hello-servern måste använda Windows Server standard eller bättre.
* hello Azure AD Connect-servern måste ha ett fullständigt grafiskt användargränssnitt installerat. Det är **stöds inte** tooinstall på server core.
* Azure AD Connect måste installeras på Windows Server 2008 eller senare. Den här servern kan vara en domänkontrollant eller en medlemsserver när du använder standardinställningar. Om du använder anpassade inställningar, hello-server kan också vara fristående och har inte toobe kopplade tooa domän.
* Om du installerar Azure AD Connect på Windows Server 2008 eller Windows Server 2008 R2, kontrollerar du att tooapply hello senaste snabbkorrigeringar från Windows Update. hello-installationen är inte kan toostart med en okorrigerade server.
* Om du planerar toouse hello funktionen **Lösenordssynkronisering**, hello Azure AD Connect-servern måste vara Windows Server 2008 R2 SP1 eller senare.
* Om du planerar toouse en **grupp Hanterat tjänstkonto**, hello Azure AD Connect-servern måste vara Windows Server 2012 eller senare.
* hello Azure AD Connect-servern måste ha [.NET Framework 4.5.1](#component-prerequisites) eller senare och [Microsoft PowerShell 3.0](#component-prerequisites) eller senare installerat.
* hello Azure AD Connect-servern får inte ha PowerShell skrivfel Grupprincip har aktiverats.
* Om Active Directory Federation Services distribueras hello servrar där AD FS eller Web Application Proxy är installerade måste vara Windows Server 2012 R2 eller senare. [Windows fjärrhantering](#windows-remote-management) måste vara aktiverat på dessa servrar för fjärrinstallation.
* Om Active Directory Federation Services distribueras måste [SSL-certifikat](#ssl-certificate-requirements).
* Om Active Directory Federation Services distribueras, måste du tooconfigure [namnmatchning](#name-resolution-for-federation-servers).
* Om dina globala administratörer har MFA är aktiverat kan sedan hello URL **https://secure.aadcdn.microsoftonline-p.com** måste finnas i listan med betrodda platser hello. Du kan ange tooadd plats toohello betrodda platslistan när du tillfrågas om MFA-kontrollen och inte har lagt till innan. Du kan använda Internet Explorer tooadd den tooyour betrodda platser.

### <a name="sql-server-used-by-azure-ad-connect"></a>SQL Server som används av Azure AD Connect
* Azure AD Connect kräver en SQL Server-databasen toostore identity-data. En SQL Server 2012 Express LocalDB (enkel version av SQL Server Express) installeras som standard. SQL Server Express har en storleksgräns för 10GB som du kan använda toomanage cirka 100 000 objekt. Om du behöver toomanage ett ökat antal katalogobjekt måste toopoint hello installationen guiden tooa annan installation av SQL Server.
* Om du använder en separat SQL Server, gäller dessa krav:
  * Azure AD Connect har stöd för alla varianter av Microsoft SQL Server från SQL Server 2008 (med senaste Service Pack) tooSQL Server 2016 SP1. Microsoft Azure SQL Database är **stöds inte** som en-databas.
  * Du måste använda en icke skiftlägeskänslig sortering i SQL. Dessa sorteringar identifieras med en \_CI_ i sina namn. Det är **stöds inte** toouse en skiftlägeskänslig sortering identifieras av \_CS_ i sina namn.
  * Du kan bara ha en Synkroniseringsmotorn per SQL-instans. Det är **stöds inte** tooshare en SQL-instans med FIM/MIM-synkronisering, DirSync eller Azure AD Sync.

### <a name="accounts"></a>Konton
* En Global administratör för Azure AD-konto för hello Azure AD-klient som du vill toointegrate med. Det här kontot måste vara en **Skol- eller organisation konto** och kan inte vara en **Microsoft-konto**.
* Om du använder standardinställningar eller uppgradera från DirSync måste du ha ett Enterprise-administratörskonto för din lokala Active Directory.
* [Konton i Active Directory](active-directory-aadconnect-accounts-permissions.md) om du använder hello installationssökväg för anpassade inställningar.

### <a name="connectivity"></a>Anslutning
* hello Azure AD Connect-servern måste DNS-matchning för både intranät och internet. hello DNS-server måste kunna tooresolve namn både tooyour lokala Active Directory och hello Azure AD-slutpunkter.
* Om du har brandväggar på intranätet och du behöver tooopen portar mellan hello Azure AD Connect-servrar och domänkontrollanter, se [Azure AD Connect-portar](active-directory-aadconnect-ports.md) för mer information.
* Om proxyservern eller brandväggen gränsen som kan komma åt URL: er, hello sedan URL: er som beskrivs i [Office 365-URL: er och IP-adressintervall](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) måste öppnas.
  * Om du använder hello Microsoft Cloud i Tyskland och hello Microsoft Azure Government molnet och sedan se [Azure AD Connect-synkroniseringstjänsten instanser överväganden](active-directory-aadconnect-instances.md) för URL: er.
* Azure AD Connect är som standard med hjälp av TLS 1.0 toocommunicate med Azure AD. Du kan ändra den här tooTLS 1.2 genom att följa stegen hello i [Aktivera TLS 1.2 för Azure AD Connect](#enable-tls-12-for-azure-ad-connect).
* Om du använder en utgående proxy för att ansluta toohello Internet hello följande inställning i hello **C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config** filen måste läggas till för hello installationsguiden och Azure AD Connect sync toobe kan tooconnect toohello Internet och Azure AD. Den här texten måste anges längst ned hello hello-filen. I den här koden &lt;PROXYADRESS&gt; representerar hello faktiska proxyserverns IP-adressen eller värdnamnet namn.

```
    <system.net>
        <defaultProxy>
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

* Om proxyservern kräver autentisering måste hello [tjänstkonto](active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-account) måste finnas i hello domän och du måste använda hello anpassade inställningar för installation sökvägen toospecify en [anpassade tjänstkonto](active-directory-aadconnect-get-started-custom.md#install-required-components). Du måste också en annan ändring toomachine.config. Med den här ändringen i machine.config hello installationen guiden och sync motor svara tooauthentication begäranden från hello proxyserver. I alla guidesidor för installation, exklusive hello **konfigurera** sidan hello inloggad användarens autentiseringsuppgifter används. På hello **konfigurera** hello slutet av installationsguiden för hello hello kontext är avstängda toohello [tjänstkonto](active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-account) som har skapats av dig. hello machine.config avsnitt bör se ut så här.

```
    <system.net>
        <defaultProxy enabled="true" useDefaultCredentials="true">
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

* När Azure AD Connect skickar en web begäran tooAzure AD som en del av katalogsynkronisering, tar too5 minuter toorespond Azure AD. Det är vanligt att servrar toohave anslutning timeout vid inaktivitet proxykonfiguration. Kontrollera att hello konfigurationen är inställd tooat minst 6 minuter eller mer.

Mer information finns i MSDN om hello [standard proxy elementet](https://msdn.microsoft.com/library/kd3cf2ex.aspx).  
Mer information om du har problem med anslutningen finns [Felsöka anslutningsproblem](active-directory-aadconnect-troubleshoot-connectivity.md).

### <a name="other"></a>Annat
* Valfritt: Ett test konto tooverify synkronisering av användare.

## <a name="component-prerequisites"></a>Förutsättningar för komponent
### <a name="powershell-and-net-framework"></a>PowerShell och .net Framework
Azure AD Connect är beroende av Microsoft PowerShell och .NET Framework 4.5.1. Du behöver den här versionen eller en senare version installerad på servern. Beroende på din Windows Server-version hello följande:

* Windows Server 2012 R2
  * Microsoft PowerShell installeras som standard. Ingen åtgärd krävs.
  * .NET framework 4.5.1 eller senare versioner erbjuds via Windows Update. Kontrollera att du har installerat hello senaste uppdateringar tooWindows Server i hello på Kontrollpanelen.
* Windows Server 2008R2 och Windows Server 2012
  * hello senaste versionen av Microsoft PowerShell finns i **Windows Management Framework 4.0**, tillgänglig på [Microsoft Download Center](http://www.microsoft.com/downloads).
  * .NET framework 4.5.1 och senare versioner är tillgängliga på [Microsoft Download Center](http://www.microsoft.com/downloads).
* Windows Server 2008
  * hello stöds senaste version av PowerShell finns i **Windows Management Framework 3.0**, tillgänglig på [Microsoft Download Center](http://www.microsoft.com/downloads).
  * .NET framework 4.5.1 och senare versioner är tillgängliga på [Microsoft Download Center](http://www.microsoft.com/downloads).

### <a name="enable-tls-12-for-azure-ad-connect"></a>Aktivera TLS 1.2 för Azure AD Connect
Azure AD Connect använder TLS 1.0 som standard för att kryptera hello kommunikation mellan hello motorn synkroniseringsserver och Azure AD. Du kan ändra detta genom att konfigurera .net-program toouse TLS 1.2 som standard på hello-servern. Mer information om TLS 1.2 kan hittas i [Microsoft Security Advisory 2960358](https://technet.microsoft.com/security/advisory/2960358).

1. TLS 1.2 kan inte aktiveras på Windows Server 2008. Du behöver Windows Server 2008R2 eller senare. Se till att du har hello .net 4.5.1 snabbkorrigering installerad för ditt operativsystem, se [Microsoft Security Advisory 2960358](https://technet.microsoft.com/security/advisory/2960358). Du kanske snabbkorrigeringen eller en senare version redan installerat på servern.
2. Om du använder Windows Server 2008R2 Kontrollera TLS 1.2 är aktiverad. På Windows Server 2012-server och senare versioner, vara TLS 1.2 redan aktiverat.
   ```
   [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2]
   [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
   [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
   ```
3. Ange registernyckeln för alla operativsystem och starta om hello-server.
   ```
   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319
   "SchUseStrongCrypto"=dword:00000001
   ```
4. Om du vill använda tooenable TLS 1.2 mellan hello motorn synkroniseringsserver och en fjärransluten SQL Server och sedan kontrollera att du har hello krävs versioner installerade för [TLS 1.2-stöd för Microsoft SQL Server](https://support.microsoft.com/kb/3135244).

## <a name="prerequisites-for-federation-installation-and-configuration"></a>Krav för federation installation och konfiguration
### <a name="windows-remote-management"></a>Windows Remote Management
När du använder Azure AD Connect toodeploy Active Directory Federation Services eller hello Web Application Proxy, kontrollera dessa krav:

* Om målservern hello domänansluten sedan kontrollera att Windows Remote hanteras är aktiverat
  * I ett upphöjt PSH kommando-fönster kommandot`Enable-PSRemoting –force`
* Om hello-målservern är en icke-domänanslutna WAP-dator, så det finns ett antal ytterligare krav
  * På måldatorn för hello (WAP-dator):
    * Se till att hello winrm (Windows Remote Management / WS-Management) tjänsten körs via hello snapin-modulen tjänster
    * I ett upphöjt PSH kommando-fönster kommandot`Enable-PSRemoting –force`
  * På hello dator på vilken hello guiden körs (om hello måldatorn är icke-domän domänansluten eller ej betrodd domän):
    * I ett upphöjt PSH kommando-fönster kommandot hello`Set-Item WSMan:\localhost\Client\TrustedHosts –Value <DMZServerFQDN> -Force –Concatenate`
    * I Serverhanteraren:
      * Lägg till DMZ WAP värden toomachine pool (Serverhanteraren -> Hantera -> Lägg till servrar... Använd fliken DNS)
      * Fliken Serverhanteraren alla servrar: Högerklicka på server för WAP och välj Hantera som..., ange inloggningsuppgifter för lokala (inte domän) för hello WAP-dator
      * toovalidate PSH fjärranslutningar, i Serverhanteraren alla servrar fliken hello: Högerklicka på server för WAP och välj Windows PowerShell. En fjärrsession PSH ska öppna PowerShell tooensure fjärrsessioner kan upprättas.

### <a name="ssl-certificate-requirements"></a>Kraven för SSL-certifikat
* Vi rekommenderar starkt toouse hello samma SSL-certifikat på alla noder i AD FS-gruppen och alla proxyservrar med webbprogrammet.
* hello certifikatet måste vara X509 certifikat.
* Du kan använda ett självsignerat certifikat på federationsservrar i en testmiljö. För en produktionsmiljö rekommenderar vi dock att du hämtar hello certifikat från en offentlig Certifikatutfärdare.
  * Om du använder ett certifikat som inte är betrodd offentlig Kontrollera hello certifikatet installeras på varje server Web Application Proxy är betrodd på både lokala hello-servern och på alla federationsservrar
* hello identitet hello certifikatet måste matcha hello federationstjänstens namn (t.ex, sts.contoso.com).
  * hello identitet är antingen ett ämne alternativt namn (SAN)-tillägg av typen dNSName eller, om det finns inget SAN-transaktioner, hello ämnesnamnet som ett eget namn.  
  * Flera SAN-poster kan finnas i hello certifikat, under förutsättning att en av dem matchar hello federationstjänstens namn.
  * Om du planerar toouse till arbetsplatsen krävs ett ytterligare SAN med hello värdet **enterpriseregistration.** följt av hello UPN (User Principal Name) suffix för din organisation, till exempel **företagsregistrering.contoso.com**.
* Certifikat som baseras på CryptoAPI nästa generation (CNG) nycklar och nyckellagring stöds inte. Detta innebär att du måste använda ett certifikat som baseras på en CSP (cryptographic service provider) och inte en KSP (key storage provider).
* Jokertecken certifikat som stöds.

### <a name="name-resolution-for-federation-servers"></a>Namnmatchning för federationsservrar
* Konfigurera DNS-poster för hello AD FS federationstjänstens namn (till exempel sts.contoso.com) för både hello intranät (din interna DNS-server) och hello extranät (offentliga DNS via din domänregistrator). Se till att du använder en för hello intranätets DNS-posten poster och inte CNAME-poster. Detta krävs för windows-autentisering toowork korrekt från en domänansluten dator.
* Om du distribuerar flera AD FS-servern eller webbprogramproxyservern och sedan kontrollera att du har konfigurerat din belastningsutjämnare och att hello DNS-poster för hello AD FS-federation service name (till exempel sts.contoso.com) punkt toohello belastningsutjämnare.
* Se till att hello AD FS federationstjänstens namn (till exempel sts.contoso.com) har lagts till toohello intranät i Internet Explorer för windows-integrerad autentisering toowork för webbläsarprogram som använder Internet Explorer i intranätet. Detta kan kontrolleras via grupprinciper och distribuerade tooall din domänanslutna datorer.

## <a name="azure-ad-connect-supporting-components"></a>Azure AD Connect stöder komponenter
hello följer en lista över komponenter som Azure AD Connect installeras på hello servern där Azure AD Connect är installerat. Den här listan är för en grundläggande Expressinstallation. Om du väljer toouse en annan SQL Server på hello installera Tjänstesida för synkronisering, sedan SQL Express LocalDB är inte installerad lokalt.

* Azure AD Connect Health
* Microsoft Online Services-inloggningsassistent för IT-proffs (installerad men inga beroende)
* Microsoft SQL Server 2012-kommandoradsverktyg
* Microsoft SQL Server 2012 Express LocalDB
* Microsoft SQL Server 2012 Native Client
* Microsoft Visual C++ 2013 omfördelning paketet

## <a name="hardware-requirements-for-azure-ad-connect"></a>Maskinvarukrav för Azure AD Connect
hello tabellen nedan visar hello minimikraven för hello Azure AD Connect sync-datorn.

| Antal objekt i Active Directory | Processor | Minne | Hårddiskstorlek |
| --- | --- | --- | --- |
| Färre än 10 000 |1,6 GHz |4 GB |70 GB |
| 10,000–50,000 |1,6 GHz |4 GB |70 GB |
| 50,000–100,000 |1,6 GHz |16 GB |100 GB |
| För 100 000 eller fler objekt krävs hello fullständig version av SQL Server | | | |
| 100,000–300,000 |1,6 GHz |32 GB |300 GB |
| 300,000–600,000 |1,6 GHz |32 GB |450 GB |
| Mer än 600 000 |1,6 GHz |32 GB |500 GB |

Hej minimikraven för datorer som kör AD FS eller webbprogramservrar är hello följande:

* Processor: Dual core 1,6 GHz eller snabbare
* MINNE: 2 GB eller högre
* Azure VM: A2-konfiguration eller högre

## <a name="next-steps"></a>Nästa steg
Läs mer om hur du [integrerar dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).
