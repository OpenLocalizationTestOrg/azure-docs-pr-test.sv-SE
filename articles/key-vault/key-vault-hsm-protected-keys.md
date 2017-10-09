---
title: "aaaHow toogenerate och överför HSM-skyddade nycklar för Azure Key Vault | Microsoft Docs"
description: "Använd den här artikeln toohelp du planera för generera och överföra din egen HSM-skyddade nycklar toouse med Azure Key Vault. Kallas även BYOK eller egna nycklar."
services: key-vault
documentationcenter: 
author: cabailey
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 51abafa1-812b-460f-a129-d714fdc391da
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2017
ms.author: ambapat
ms.openlocfilehash: 3bb234bd1c4b81770542ccf7110e256385ca3309
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toogenerate-and-transfer-hsm-protected-keys-for-azure-key-vault"></a>Hur toogenerate och överför HSM-skyddade nycklar för Azure Key Vault
## <a name="introduction"></a>Introduktion
För ytterligare säkerhet när du använder Azure Key Vault kan importera eller generera nycklar i maskinvarusäkerhetsmoduler (HSM) som lämnar aldrig hello HSM gräns. Det här scenariot är ofta hänvisas tooas *egna nycklar*, eller BYOK. hello HSM är FIPS 140-2 Level 2-verifierade. Azure Key Vault använder Thales nShield-familjen för HSM tooprotect dina nycklar.

Använd hello information i det här avsnittet toohelp du planera för generera och överföra din egen HSM-skyddade nycklar toouse med Azure Key Vault.

Den här funktionen är inte tillgänglig för Azure Kina.

> [!NOTE]
> Mer information om Azure Key Vault finns [vad är Azure Key Vault?](key-vault-whatis.md)  
>
> En komma igång-kursen, vilket innefattar att skapa nyckelvalvet för HSM-skyddade nycklar, se [Kom igång med Azure Key Vault](key-vault-get-started.md).
>
>

Mer information om generera och överför en HSM-skyddad nyckel över hello Internet:

* Du kan generera hello nyckel från en arbetsstation som offline, vilket minskar risken för angrepp hello.
* hello nyckeln krypteras med en KeyExchange-nyckel (KEK), som är krypterad tills den är överförda toohello Azure Key Vault HSM. Endast hello krypterad version av din nyckel lämnar hello ursprungliga arbetsstationen.
* hello-verktyg anger egenskaperna för din klientnyckel som binder din nyckel toohello Azure Key Vault-säkerhetsvärlden. Så när hello Azure Key Vault HSM tagit emot och dekrypterat din nyckel, kan bara dessa HSM: er använda den. Din nyckel kan inte exporteras. Den här bindningen är påtvingad av hello Thales HSM: er.
* Hej KeyExchange-nyckel (KEK) som används tooencrypt din nyckel genereras inuti hello Azure Key Vault HSM och kan inte exporteras. hello HSM: erna genomdriver att det kan finnas någon klartextversion av hello KEK utanför hello HSM. Dessutom innehåller hello verktygsuppsättningen intyg från Thales om att hello KEK inte kan exporteras och genereras inuti en äkta HSM som har tillverkats av Thales.
* hello verktygsuppsättningen innehåller intyg från Thales om att hello Azure Key Vault säkerhetsvärld genererades i en äkta HSM tillverkad av Thales. Detta bevisar tooyou att Microsoft använder äkta maskinvara.
* Microsoft använder separata KeyExchange-nycklar och separata Säkerhetsvärldar i varje geografisk region. Den här separationen säkerställer att din nyckel kan användas i datacenter i hello region där den krypterades. Exempelvis kan en nyckel från en europeisk kund inte användas i datacenter i Nordamerika eller Asien.

## <a name="more-information-about-thales-hsms-and-microsoft-services"></a>Mer information om Thales HSM: er och Microsoft-tjänster
Thales e-Security är en ledande global leverantör av kryptering av data och cyber security lösningar toohello finansiella tjänster, avancerad teknik, tillverkning, myndigheter och informationsteknik. Thales lösningar som används av fyra hello fem största energi- och aerospace företag med en 40 år spåra post för att skydda företagets och myndigheter information. Sina lösningar används också av 22 NATO-länder och skyddar mer än 80 procent av transaktioner över hela världen.

Microsoft har samarbetat med Thales tooenhance hello tillstånd bilder för HSM. Dessa förbättringar kan du tooget hello vanliga fördelarna med värdtjänster utan att behöva lämna ifrån dig kontrollen över dina nycklar. Mer specifikt att förbättringarna Microsoft hanterar hello HSM: er så att du inte behöver. Som ett moln som tjänsten, Azure Key Vault därmed utökas med kort varsel toomeet organisationens användning ger spikar i diagrammet. AT hello samma time, skyddas din nyckel i Microsofts HSM: du behålla kontrollen över hello livscykel eftersom du generera hello nyckel och överför den Toomicrosoft's HSM.

## <a name="implementing-bring-your-own-key-byok-for-azure-key-vault"></a>Implementera byok (BYOK) för Azure Key Vault
Använd hello följande information och procedurer om du ska skapa egna HSM-skyddad nyckel och sedan överföra tooAzure Key Vault – hello ta med din egen nyckel (BYOK)-scenario.

## <a name="prerequisites-for-byok"></a>Krav för BYOK
Se hello i den följande tabellen för en lista över förutsättningarna för att överföra din egen nyckel (BYOK) för Azure Key Vault.

| Krav | Mer information |
| --- | --- |
| En prenumeration tooAzure |toocreate ett Azure Key Vault du behöver en Azure-prenumeration: [registrera dig för en kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/) |
| hello Azure Key Vault Premium-nivån toosupport HSM-skyddad för tjänsten |Mer information om hello tjänstnivåer och funktioner för Azure Key Vault finns hello [priser för Azure Key Vault](https://azure.microsoft.com/pricing/details/key-vault/) webbplats. |
| Thales HSM, smartkort och hjälpprogram |Du måste ha åtkomst till tooa maskinvarusäkerhetsmodul och grundläggande operativa kunskaper om Thales HSM: er. Se [maskinvarusäkerhetsmodul](https://www.thales-esecurity.com/msrms/buy) hello lista över kompatibla modeller eller toopurchase en HSM om du inte har någon. |
| Hej följande maskinvara och programvara:<ol><li>Ett offline x64 arbetsstation med minst Windows-operativsystemet Windows 7 och Thales nShield-programvara som är minst version 11.50.<br/><br/>Om den här arbetsstationen kör Windows 7, måste du [installera Microsoft .NET Framework 4.5](http://download.microsoft.com/download/b/a/4/ba4a7e71-2906-4b2d-a0e1-80cf16844f5f/dotnetfx45_full_x86_x64.exe).</li><li>En arbetsstation som är anslutna toohello Internet och har en minsta Windows-operativsystemet Windows 7 och [Azure PowerShell](/powershell/azure/overview) **minimiversionen 1.1.0** installerad.</li><li>En USB-enhet eller annan bärbar lagringsenhet som har minst 16 MB ledigt utrymme.</li></ol> |Av säkerhetsskäl rekommenderar vi att hello första arbetsstationen inte är anslutna tooa nätverk. Men är den här rekommendationen inte programmässigt tvingande.<br/><br/>Observera att den här arbetsstationen i hello instruktionerna som följer är refererad tooas hello frånkopplade arbetsstationen.</p></blockquote><br/>Om din klientnyckel är avsedd för ett produktionsnätverk, rekommenderar vi dessutom att du använder en andra, separat arbetsstation toodownload hello verktygen och överföra hello klientnyckel. Men i testsyfte kan du använda hello samma arbetsstation som hello första.<br/><br/>Observera att den här andra arbetsstationen i hello instruktionerna som följer är refererad tooas hello Internetanslutna arbetsstationen.</p></blockquote><br/> |

## <a name="generate-and-transfer-your-key-tooazure-key-vault-hsm"></a>Generera och överför din nyckel tooAzure Key Vault HSM
Du vill använda hello följande fem steg toogenerate och överför din nyckel tooan Azure Key Vault HSM:

* [Steg 1: Förbered din Internetanslutna arbetsstation](#step-1-prepare-your-internet-connected-workstation)
* [Steg 2: Förbered din frånkopplade arbetsstation](#step-2-prepare-your-disconnected-workstation)
* [Steg 3: Skapa din nyckel](#step-3-generate-your-key)
* [Steg 4: Förbereda din nyckel för överföring](#step-4-prepare-your-key-for-transfer)
* [Steg 5: Överför din nyckel tooAzure Key Vault](#step-5-transfer-your-key-to-azure-key-vault)

## <a name="step-1-prepare-your-internet-connected-workstation"></a>Steg 1: Förbered din Internetanslutna arbetsstation
I den här första steget hello följande procedurer på en arbetsstation som är anslutna toohello Internet.

### <a name="step-11-install-azure-powershell"></a>Steg 1.1: Installera Azure PowerShell
Hämta och installera hello Azure PowerShell-modulen som innehåller hello cmdlets toomanage Azure Key Vault från hello Internetanslutna arbetsstationen. Detta kräver en lägsta version av 0.8.13.

Installationsanvisningar finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).

### <a name="step-12-get-your-azure-subscription-id"></a>Steg 1.2: Hämta ditt Azure prenumerations-ID
Starta en Azure PowerShell-session och logga in tooyour Azure-konto med hjälp av hello följande kommando:

        Add-AzureAccount
Ange ditt användarnamn för Azure-konto och lösenord i hello popup-webbläsarfönstret. Använd sedan hello [Get-AzureSubscription](/powershell/module/azure/get-azuresubscription?view=azuresmps-3.7.0) kommando:

        Get-AzureSubscription
Hitta hello-ID för hello-prenumeration som du ska använda för Azure Key Vault från hello-utdata. Du behöver den här prenumerations-ID senare.

Stäng inte hello Azure PowerShell-fönster.

### <a name="step-13-download-hello-byok-toolset-for-azure-key-vault"></a>Steg 1.3: Hämta hello BYOK-verktyg för Azure Key Vault
Gå toohello Microsoft Download Center och [hämta hello Azure Key Vault BYOK-verktygen](http://www.microsoft.com/download/details.aspx?id=45345) för ett geografiskt område eller instans av Azure. Använder hello följande information tooidentify hello paketet namnet toodownload och dess motsvarande SHA-256 paketet hash:

- - -
**USA:**

KeyVault-BYOK-verktyg-Förenade States.zip

760EE9BD6445C87CFF0E8B032577118704B3BEAA045AA55977C10EF68BC67E2B

- - -
**Europa:**

KeyVault-BYOK-verktyg-Europe.zip

7A64B94225F59B847C5C27C2200BAD7D16C901E1687767EDBBB8B09BB285011D

- - -
**Asien:**

KeyVault-BYOK-verktyg-AsiaPacific.zip

813DC94B23079CF7A5CEA71D5B444E86B292F463C53EE47AED25D4F7CD58E7D8

- - -
**Latinamerika:**

KeyVault-BYOK-verktyg-LatinAmerica.zip

3F29069E3500F95C0E156F4B8914E1DC60C20FB64B464306A299EA5145D755C0

- - -
**Japan:**

KeyVault-BYOK-verktyg-Japan.zip

453FFEA2F8F410720B68B8BAC4CF79135A7F37F4E491FF840BE9E69E88A98C90

- - -
**Korea:**

KeyVault-BYOK-verktyg-Korea.zip

C17B7E93224DA80F5668E09CF7DAE2F92527E8226179995BBE2E43DA4323595A

- - -
**Australien:**

KeyVault-BYOK-verktyg-Australia.zip

4AD893396E86F2D2A71682876A6A8EA59E3C7895BEAD2F7E7C8516682582C34B

- - -
[**Azure Government:**](https://azure.microsoft.com/features/gov/)

KeyVault-BYOK-verktyg-USGovCloud.zip

3AAE1A96B9D15B899B8126CFC0380719EB54FDF2EA94489B43FAD21ECC745F64

- - -
**USA: S regering DOD:**

KeyVault-BYOK-verktyg-USGovernmentDoD.zip

A61E78297B0732DF2682FDE63D7B572CE4D23B0BC27CC48AFF620BD060BB9E9D

- - -
**Kanada:**

KeyVault-BYOK-verktyg-Canada.zip

30B87A0BA8208F6B7241C30C794FED1C370D7445ACA179685816E4E156CD2AF7

- - -
**Tyskland:**

KeyVault-BYOK-verktyg-Germany.zip

5E3E4AA54715E4F93C3C145035B18275B7C6815A06D7ABB212E7FADBF2929261

- - -
**Indien:**

KeyVault-BYOK-verktyg-India.zip

136733A6C6A71D75571BB80819B3D55A9B83CCAD5C996C686BC5682A3F369BF7

- - -
**Storbritannien:**

KeyVault-BYOK-verktyg-UnitedKingdom.zip

ED331A6F1D34A402317D3F27D5396046AF0E5C2D44B5D10CCCE293472942D268

- - -

toovalidate hello integriteten hos dina hämtade BYOK-verktyg från Azure PowerShell-sessionen, Använd hello [Get-FileHash](https://technet.microsoft.com/library/dn520872.aspx) cmdlet.

    Get-FileHash KeyVault-BYOK-Tools-*.zip

hello verktygsuppsättningen innehåller hello följande:

* Ett paket för KeyExchange-nyckel (KEK) som har ett namn som börjar med **BYOK-KEK - pkg-.**
* Ett säkerhetsvärldspaket som har ett namn som börjar med **BYOK-SecurityWorld - pkg-.**
* Python-skriptet **verifykeypackage.py.**
* En kommandorad körbara filen **KeyTransferRemote.exe** och tillhörande DLL-filer.
* Ett Visual C++ Redistributable Package med namnet **vcredist_x64.exe.**

Kopiera hello paketet tooa USB-enhet eller annan bärbar lagringsenhet.

## <a name="step-2-prepare-your-disconnected-workstation"></a>Steg 2: Förbered din frånkopplade arbetsstation
För den här andra steget hello följande procedurer hello arbetsstation som inte är anslutna tooa nätverk (hello Internet eller ditt interna nätverk).

### <a name="step-21-prepare-hello-disconnected-workstation-with-thales-hsm"></a>Steg 2.1: Förbered hello frånkopplade arbetsstationen med Thales HSM
Installera programvara hello nCipher (Thales) på en Windows-dator och sedan koppla en Thales HSM toothat dator.

Se till att hello Thales-verktygen finns i din sökväg (**%nfast_home%\bin**). Skriv till exempel hello följande:

        set PATH=%PATH%;"%nfast_home%\bin"

Mer information finns i hello användarhandboken som medföljer hello Thales HSM.

### <a name="step-22-install-hello-byok-toolset-on-hello-disconnected-workstation"></a>Steg 2.2: Installera hello BYOK-verktygen på hello frånkopplade arbetsstationen
Kopiera hello BYOK-verktygspaketet från hello USB-enhet eller annan bärbar lagringsenhet och sedan hello följande:

1. Extrahera hello filer från hello hämtade paketet till en annan mapp.
2. Kör vcredist_x64.exe från denna mapp.
3. Följ instruktionerna för hello toohello installerar hello Visual C++-körtidskomponenter för Visual Studio 2013.

## <a name="step-3-generate-your-key"></a>Steg 3: Skapa din nyckel
För det här tredje steget hello följande procedurer på hello frånkopplade arbetsstationen. toocomplete det här steget din HSM måste vara i läget för initiering. 


### <a name="step-31-change-hello-hsm-mode-tooi"></a>Steg 3.1: Ändra hello HSM läge too'I'
Om du använder Thales nShield gränsen, toochange hello läge: 1. Använd hello läge knappen toohighlight hello krävs läge. 2. Tryck och håll hello Rensa-knapp för några sekunder inom några sekunder. Om hello läge ändras hello nya läget Indikator slutar blinka och fortsätter att lysa. hello Status Indikator kan flash oregelbundet under några sekunder och sedan blinkar regelbundet när hello enheten är klar. Annars hello enheten fortfarande hello aktuellt läge med hello lämpliga läge Indikator upplysta.

### <a name="step-32-create-a-security-world"></a>Steg 3.2: Skapa en säkerhetsvärld
Starta en kommandotolk och kör hello Thales nya världen program.

    new-world.exe --initialize --cipher-suite=DLf1024s160mRijndael --module=1 --acs-quorum=2/3

Det här programmet skapar en **Säkerhetsvärld** filen i % NFAST_KMDATA%\local\world, vilket motsvarar toohello C:\ProgramData\nCipher\Key Management Data\local mapp. Du kan använda olika värden för hello kvorum men i vårt exempel är tooenter ange tre tomma kort och PIN-koder för varje kriterium. Sedan ger två valfria kort fullständig åtkomst toohello säkerhetsvärlden. Dessa kort blir hello **Administratörskortsuppsättningen** för hello nya säkerhetsvärlden.

Sedan hello följande:

* Säkerhetskopiera hello world-filen. Säkra och skydda hello world-fil, hello Administratörskort och sina PIN-koder och se till att ingen enskild person får åtkomst toomore än ett kort.

### <a name="step-33-change-hello-hsm-mode-tooo"></a>Steg 3.3: Ändra hello HSM läge too'O'
Om du använder Thales nShield gränsen, toochange hello läge: 1. Använd hello läge knappen toohighlight hello krävs läge. 2. Tryck och håll hello Rensa-knapp för några sekunder inom några sekunder. Om hello läge ändras hello nya läget Indikator slutar blinka och fortsätter att lysa. hello Status Indikator kan flash oregelbundet under några sekunder och sedan blinkar regelbundet när hello enheten är klar. Annars hello enheten fortfarande hello aktuellt läge med hello lämpliga läge Indikator upplysta.


### <a name="step-34-validate-hello-downloaded-package"></a>Steg 3.4: Verifiera hello hämtade paketet
Det här steget är valfritt men rekommenderas så att du kan validera hello följande:

* Hej KeyExchange-nyckeln som ingår i hello verktygsuppsättningen har genererats från en äkta Thales HSM.
* hello-hash för hello Säkerhetsvärld som ingår i hello verktygsuppsättningen har genererats från en äkta Thales HSM.
* Hej KeyExchange-nyckel är inte kan exporteras.

> [!NOTE]
> toovalidate hello hämtade paketet, hello HSM måste vara ansluten, påslagen, och måste ha en säkerhetsvärld (till exempel hello något du nyss skapat).
>
>

toovalidate hello hämtade paketet:

1. Kör skript för hello verifykeypackage.py genom att skriva en hello följande, beroende på din geografiskt område eller en instans av Azure:

   * För Nordamerika:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-NA-1 -w BYOK-SecurityWorld-pkg-NA-1
   * För Europa:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-EU-1 -w BYOK-SecurityWorld-pkg-EU-1
   * För Asien:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-AP-1 -w BYOK-SecurityWorld-pkg-AP-1
   * För Latinamerika:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-LATAM-1 -w BYOK-SecurityWorld-pkg-LATAM-1
   * För Japan:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-JPN-1 -w BYOK-SecurityWorld-pkg-JPN-1
   * För Korea:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-KOREA-1 -w BYOK-SecurityWorld-pkg-KOREA-1
   * För Australien:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-AUS-1 -w BYOK-SecurityWorld-pkg-AUS-1
   * För [Azure Government](https://azure.microsoft.com/features/gov/), som använder hello US government instans av Azure:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-USGOV-1 -w BYOK-SecurityWorld-pkg-USGOV-1
   * För USA: S regering DOD:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-USDOD-1 -w BYOK-SecurityWorld-pkg-USDOD-1
   * För Kanada:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-CANADA-1 -w BYOK-SecurityWorld-pkg-CANADA-1
   * Tyskland:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-GERMANY-1 -w BYOK-SecurityWorld-pkg-GERMANY-1
   * För Indien:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-INDIA-1 -w BYOK-SecurityWorld-pkg-INDIA-1
     > [!TIP]
     > Hej Thales-programvaran innehåller python på %NFAST_HOME%\python\bin
     >
     >
2. Bekräfta att du ser hello följande, vilket betyder att valideringen har lyckats: **resultat: lyckades**

Det här skriptet validerar hello undertecknarkedjan upp toohello Thales rotnyckel. hello-hash för den här rotnyckeln är inbäddad i hello skript och dess värde bör vara **59178a47 de508c3f 291277ee 184f46c4 f1d9c639**. Du kan också bekräfta det här värdet separat genom att besöka hello [Thales webbplats](http://www.thalesesec.com/).

Nu är du redo toocreate en ny nyckel.

### <a name="step-35-create-a-new-key"></a>Steg 3.5: Skapa en ny nyckel
Generera en nyckel med hjälp av hello Thales **generatekey** program.

Kör följande toogenerate hello tangent hello:

    generatekey --generate simple type=RSA size=2048 protect=module ident=contosokey plainname=contosokey nvram=no pubexp=

Följ dessa instruktioner när du kör det här kommandot:

* hello parametern *skydda* måste anges värdet toohello **modulen**, som visas. Detta skapar en modulskyddad nyckel. Hej BYOK-verktygen har inte stöd för OCS-skyddade nycklar.
* Ersätt hello värdet för *contosokey* för hello **ident** och **plainname** med valfritt strängvärde. toominimize administrativa kostnaderna och minska hello risken för fel, rekommenderar vi att du använder hello samma värde för båda. Hej **ident** värdet måste innehålla endast siffror, bindestreck och gemener.
* Hej pubexp lämnas tomt (standard) i det här exemplet, men du kan ange specifika värden. Mer information finns i hello Thales-dokumentationen.

Det här kommandot skapar en fil för Tokeniserad nyckel i mappen %NFAST_KMDATA%\local med ett namn som börjar med **key_simple_**, följt av hello **ident** som angavs i hello-kommandot. Till exempel: **key_simple_contosokey**. Den här filen innehåller en krypterad nyckel.

Säkerhetskopiera filen för Tokeniserad nyckel på en säker plats.

> [!IMPORTANT]
> När du senare överför din nyckel tooAzure Key Vault kan inte Microsoft exportera den här nyckeln tillbaka tooyou så att det är mycket viktigt att säkerhetskopiera din nyckel och säkerhetsvärld på ett säkert sätt. Kontakta Thales för vägledning och bästa praxis för säkerhetskopiering av din nyckel.
>
>

Du är nu redo tootransfer din nyckel tooAzure Key Vault.

## <a name="step-4-prepare-your-key-for-transfer"></a>Steg 4: Förbereda din nyckel för överföring
För det här fjärde steget hello följande procedurer på hello frånkopplade arbetsstationen.

### <a name="step-41-create-a-copy-of-your-key-with-reduced-permissions"></a>Steg 4.1: Skapa en kopia av din nyckel med minskade behörigheter

Öppna en ny kommandotolk och ändra hello aktuella toohello katalogplatsen där du uppackade hello BYOK zip-filen. tooreduce hello behörigheter på din nyckel från en kommandotolk kör något av hello följande, beroende på din geografiskt område eller en instans av Azure:

* För Nordamerika:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1
* För Europa:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1
* För Asien:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1
* För Latinamerika:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1
* För Japan:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1
* För Korea:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-KOREA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-KOREA-1
* För Australien:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1
* För [Azure Government](https://azure.microsoft.com/features/gov/), som använder hello US government instans av Azure:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1
* För USA: S regering DOD:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USDOD-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USDOD-1
* För Kanada:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1
* Tyskland:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1
* För Indien:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1

När du kör det här kommandot ersätter *contosokey* med hello samma värde som du angav i **steg 3.5: skapa en ny nyckel** från hello [generera din nyckel](#step-3-generate-your-key) steg.

Du uppmanas tooplug i dina säkerhet world admin-kort.

När hello-kommandot har slutförts visas **resultat: lyckades** och hello kopia av din nyckel med minskade behörigheter finns i hello filen key_xferacid_<contosokey>.

Du kan kontrolleras hello ACL: er med hjälp av följande kommandon med hjälp av hello Thales-verktygen:

* aclprint.PY:

        "%nfast_home%\bin\preload.exe" -m 1 -A xferacld -K contosokey "%nfast_home%\python\bin\python" "%nfast_home%\python\examples\aclprint.py"
* kmfile-dump.exe:

        "%nfast_home%\bin\kmfile-dump.exe" "%NFAST_KMDATA%\local\key_xferacld_contosokey"
  När du kör kommandona, ersätter contosokey med samma värde som du angav i hello **steg 3.5: skapa en ny nyckel** från hello [generera din nyckel](#step-3-generate-your-key) steg.

### <a name="step-42-encrypt-your-key-by-using-microsofts-key-exchange-key"></a>Steg 4.2: Kryptera nyckeln med hjälp av Microsofts KeyExchange-nyckel
Kör något av följande kommandon, beroende på din geografiskt område eller en instans av Azure hello:

* För Nordamerika:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* För Europa:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* För Asien:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* För Latinamerika:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* För Japan:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* För Korea:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-KOREA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-KOREA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* För Australien:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* För [Azure Government](https://azure.microsoft.com/features/gov/), som använder hello US government instans av Azure:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* För USA: S regering DOD:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USDOD-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USDOD-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* För Kanada:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Tyskland:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* För Indien:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey

Följ dessa instruktioner när du kör det här kommandot:

* Ersätt *contosokey* med hello-identifierare som du använde toogenerate hello nyckeln i **steg 3.5: skapa en ny nyckel** från hello [generera din nyckel](#step-3-generate-your-key) steg.
* Ersätt *SubscriptionID* med hello ID hello Azure-prenumeration som innehåller nyckelvalvet. Du har hämtat värdet tidigare i **steg 1.2: hämta ditt Azure-prenumeration ID** från hello [Förbered din Internetanslutna arbetsstation](#step-1-prepare-your-internet-connected-workstation) steg.
* Ersätt *ContosoFirstHSMKey* med en etikett som används för utdata-filnamn.

När detta är klar visas **resultat: lyckades** och det finns en ny fil i hello aktuella mappen hello efter namnet: keytransferpackage*ContosoFirstHSMkey*.byok

### <a name="step-43-copy-your-key-transfer-package-toohello-internet-connected-workstation"></a>Steg 4.3: Kopiera överföringen nyckeln paketet toohello Internetanslutna arbetsstationen
Använd en USB-enhet eller annan bärbar lagringsenhet toocopy hello utdatafilen från hello föregående steg (KeyTransferPackage ContosoFirstHSMkey.byok) tooyour Internetanslutna arbetsstationen.

## <a name="step-5-transfer-your-key-tooazure-key-vault"></a>Steg 5: Överför din nyckel tooAzure Key Vault
Använd hello på hello Internetanslutna arbetsstationen för det här sista steget, [Lägg till AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurermkeyvaultkey) cmdlet tooupload hello nyckelöverföringspaketet som du kopierade från hello frånkopplade arbetsstationen toohello Azure Key Vault HSM:

    Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMkey' -KeyFilePath 'c:\KeyTransferPackage-ContosoFirstHSMkey.byok' -Destination 'HSM'

Om hello uppladdningen lyckas finns visas hello egenskaper hello-nyckel som du just lagt till.

## <a name="next-steps"></a>Nästa steg
Du kan nu använda den här HSM-skyddad nyckel i nyckelvalvet. Mer information finns i hello **om du vill toouse en maskinvarusäkerhetsmodul (HSM)** avsnitt i hello [komma igång med Azure Key Vault](key-vault-get-started.md) kursen.
