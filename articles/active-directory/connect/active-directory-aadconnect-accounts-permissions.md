---
title: "Azure AD Connect: Konton och behörigheter | Microsoft Docs"
description: "Det här avsnittet beskrivs används och skapa hello-konton och behörigheter som krävs."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.reviewer: cychua
ms.assetid: b93e595b-354a-479d-85ec-a95553dd9cc2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: billmath
ms.openlocfilehash: 70a7013e0353d74714ec8a3ff54b3e811789a0b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-accounts-and-permissions"></a>Azure AD Connect: Konton och behörigheter
Installationsguiden för hello Azure AD Connect innehåller två olika sökvägar:

* I inställningarna för Express kräver hello-guiden fler privilegier.  Detta är så att den kan ställa in din konfiguration enkelt, utan att du toocreate användare eller konfigurera behörigheter.
* I anpassade inställningar erbjuder hello guiden flera alternativ och alternativ. Det finns dock vissa situationer där du behöver tooensure som du har behörigheter för hello själv.

## <a name="related-documentation"></a>Relaterad dokumentation
Om du inte läste hello dokumentation på [integrera dina lokala identiteter med Azure Active Directory](../active-directory-aadconnect.md), hello följande tabell innehåller länkar toorelated avsnitt.

|Avsnitt |Länk|  
| --- | --- |
|Ladda ned Azure AD Connect | [Ladda ned Azure AD Connect](http://go.microsoft.com/fwlink/?LinkId=615771)|
|Installera med standardinställningar | [Snabbinstallation av Azure AD Connect](./active-directory-aadconnect-get-started-express.md)|
|Installera med anpassade inställningar | [Anpassad installation av Azure AD Connect](./active-directory-aadconnect-get-started-custom.md)|
|Uppgradera från DirSync | [Uppgradera från Azure AD-synkroniseringsverktyg (DirSync)](./active-directory-aadconnect-dirsync-upgrade-get-started.md)|
|Efter installationen | [Kontrollera hello installationen och tilldela licenser](active-directory-aadconnect-whats-next.md)|

## <a name="express-settings-installation"></a>Snabbinstallation inställningar
I inställningarna för snabb begär hello installationsguiden autentiseringsuppgifter som företagsadministratör för AD DS.  Detta är så att din lokala Active Directory kan konfigureras med behörigheter som krävs för Azure AD Connect. Om du uppgraderar från DirSync hello AD DS Företagsadministratörer autentiseringsuppgifter används tooreset hello lösenord anges för hello-kontot som används av DirSync. Du måste också autentiseringsuppgifter som Global administratör för Azure AD.

| Sidan i guiden | Autentiseringsuppgifter samlas in | Behörigheter som krävs | Används för |
| --- | --- | --- | --- |
| Saknas |Användare kör hello installationsguiden |Administratören för hello lokal server |<li>Skapar hello lokalt konto som används som hello [synkronisera motorn tjänstkonto](#azure-ad-connect-sync-service-account). |
| Ansluta tooAzure AD |Autentiseringsuppgifter för Azure AD-katalog |Rollen som global administratör i Azure AD |<li>Aktiverar synkronisering i hello Azure AD-katalog.</li>  <li>Skapa hello [Azure AD-kontot](#azure-ad-service-account) som används för pågående synkroniseringsåtgärder i Azure AD.</li> |
| Ansluta tooAD DS |Lokala Active Directory-autentiseringsuppgifter |Medlem i gruppen för hello Enterprise administratörer (EA) i Active Directory |<li>Skapar en [konto](#active-directory-account) i Active Directory och ger behörighet tooit. Det här kontot skapades är tooread och skriva directory information som används under synkroniseringen.</li> |

### <a name="enterprise-admin-credentials"></a>Autentiseringsuppgifter som företagsadministratör
Dessa autentiseringsuppgifter används bara när hello installationen och används inte när hello-installationen har slutförts. hello företagsadministratör, inte hello Domain Admin bör kontrollera hello behörigheter i Active Directory kan anges i alla domäner.

### <a name="global-admin-credentials"></a>Autentiseringsuppgifter som global administratör
Dessa autentiseringsuppgifter används bara när hello installationen och används inte när hello-installationen har slutförts. Det är används toocreate hello [Azure AD-kontot](#azure-ad-service-account) används för att synkronisera ändringar tooAzure AD. hello-konto kan också synkronisera som en funktion i Azure AD.

### <a name="permissions-for-hello-created-ad-ds-account-for-express-settings"></a>Behörigheter för hello skapat AD DS-konto för standardinställningar
Hej [konto](#active-directory-account) skapas för läsning och skrivning tooAD DS har hello följande behörigheter när de skapas med standardinställningar:

| Behörighet | Används för |
| --- | --- |
| <li>Replikera katalogändringar</li><li>Replikera katalogen ändras alla |Lösenordssynkronisering |
| Läs/Skriv alla egenskaper för användaren |Import- och Exchange-hybrid |
| Läs/Skriv alla egenskaper iNetOrgPerson |Import- och Exchange-hybrid |
| Gruppen för läsning och skrivning alla egenskaper |Import- och Exchange-hybrid |
| Läsning och skrivning kontaktar du alla egenskaper |Import- och Exchange-hybrid |
| Återställa lösenord |Förberedelser för att aktivera tillbakaskrivning av lösenord |

## <a name="custom-settings-installation"></a>Installation av anpassade inställningar
Azure AD Connect version 1.1.524.0 och senare har hello alternativet toolet hello Azure AD Connect-guiden Skapa hello konto som används för tooconnect tooActive Directory.  Tidigare versioner kräver att hello-konto har skapats innan hello-installationen. hello behörighet måste du bevilja det här kontot finns i [skapa hello AD DS-konto](#create-the-ad-ds-account). 

| Sidan i guiden | Autentiseringsuppgifter samlas in | Behörigheter som krävs | Används för |
| --- | --- | --- | --- |
| Saknas |Användare kör hello installationsguiden |<li>Administratören för hello lokal server</li><li>Om du använder en fullständig SQL Server måste hello användaren vara systemadministratörskontot (SA) i SQL</li> |Som standard skapar hello lokalt konto som används som hello [synkronisera motorn tjänstkonto](#azure-ad-connect-sync-service-account). hello kontot skapas endast när Hej administratör inte anger ett visst konto. |
| Installera synkroniseringstjänsterna kontoalternativet för tjänsten |AD eller autentiseringsuppgifter för lokal användare |Användare, behörigheter beviljas av hello installationsguiden |Hej administratör anger ett konto, används det här kontot som hello-tjänstkontot för hello-synkroniseringstjänsten. |
| Ansluta tooAzure AD |Autentiseringsuppgifter för Azure AD-katalog |Rollen som global administratör i Azure AD |<li>Aktiverar synkronisering i hello Azure AD-katalog.</li>  <li>Skapa hello [Azure AD-kontot](#azure-ad-service-account) som används för pågående synkroniseringsåtgärder i Azure AD.</li> |
| Anslut dina kataloger |Lokala Active Directory-autentiseringsuppgifter för varje skog som är anslutna tooAzure AD |hello behörigheter beror på vilka funktioner du aktiverar och finns i [skapa hello AD DS-konto](#create-the-ad-ds-account) |Det här kontot är tooread och skriva directory information som används under synkroniseringen. |
| AD FS-servrar |För varje server i listan hello hello guiden samlar in autentiseringsuppgifter när hello inloggningsuppgifterna för hello användare som kör guiden hello är otillräcklig tooconnect |Domänadministratören |Installation och konfiguration av hello AD FS-serverrollen. |
| Webbprogramproxyservrarna |För varje server i listan hello hello guiden samlar in autentiseringsuppgifter när hello inloggningsuppgifterna för hello användare som kör guiden hello är otillräcklig tooconnect |Lokal administratör på hello måldatorn |Installation och konfiguration av WAP-serverrollen. |
| Autentiseringsuppgifter för proxy-förtroende |Federation service förtroende autentiseringsuppgifter (hello autentiseringsuppgifter hello-proxyn använder tooenroll för ett förtroende certifikat från hello FS |Domänkonto som är en lokal administratör för hello AD FS-servern |Första registreringen av FS WAP betrott certifikat. |
| Sidan för AD FS-tjänstkontot, ”använda kontoalternativet en domän användare” |Autentiseringsuppgifter för användarkontot AD |Domänanvändare |hello AD konto vars autentiseringsuppgifter tillhandahålls används som hello inloggningskontot för hello AD FS-tjänsten. |

### <a name="create-hello-ad-ds-account"></a>Skapa hello AD DS-konto
Hej konto som du anger på hello **Anslut dina kataloger** sidan måste finnas i Active Directory tidigare tooinstallation.  Det måste också ha behörigheterna hello krävs. Installationsguiden för hello verifierar inte hello behörigheter och eventuella problem som endast hittades under synkroniseringen.

Vilka behörigheter som du behöver beror på hello valfria funktioner aktiveras. Om du har flera domäner beviljas hello behörigheter för alla domäner i skogen hello. Om du inte aktivera någon av dessa funktioner, hello standard **domänanvändare** har rätt behörighet.

| Funktion | Behörigheter |
| --- | --- |
| msDS-ConsistencyGuid funktion |Behörighet att skriva toohello msDS-ConsistencyGuid attributet dokumenterade i [designbegrepp - med msDS-ConsistencyGuid som sourceAnchor](active-directory-aadconnect-design-concepts.md#using-msds-consistencyguid-as-sourceanchor). | 
| Lösenordssynkronisering |<li>Replikera katalogändringar</li>  <li>Replikera katalogen ändras alla |
| Exchange-hybridinstallation |Skriva behörigheter toohello attribut dokumenterade i [Exchange hybrid tillbakaskrivning](active-directory-aadconnectsync-attributes-synchronized.md#exchange-hybrid-writeback) för användare, grupper och kontakter. |
| Offentlig mapp för Exchange-e-post |Läsa behörigheter toohello attribut dokumenterade i [offentlig mapp för Exchange-e-post](active-directory-aadconnectsync-attributes-synchronized.md#exchange-mail-public-folder) för offentliga mappar. | 
| Tillbakaskrivning av lösenord |Skriva behörigheter toohello attribut dokumenterade i [komma igång med lösenordshantering](../active-directory-passwords-writeback.md) för användare. |
| Tillbakaskrivning av enheter |Behörigheterna med ett PowerShell-skript som beskrivs i [tillbakaskrivning av enheter](active-directory-aadconnect-feature-device-writeback.md). |
| Tillbakaskrivning av grupp |Läsa, skapa, uppdatera och ta bort grupp objekt för synkroniserats **Office 365-grupper**.  Mer information finns i [tillbakaskrivning av grupp](active-directory-aadconnect-feature-preview.md#group-writeback).|

## <a name="upgrade"></a>Uppgradera
När du uppgraderar från en version av Azure AD Connect tooa ny version, måste du hello följande behörigheter:

| Huvudnamn | Behörigheter som krävs | Används för |
| --- | --- | --- |
| Användare kör hello installationsguiden |Administratören för hello lokal server |Uppdatera binärfiler. |
| Användare kör hello installationsguiden |Medlem i ADSyncAdmins |Göra ändringar tooSync regler och annan konfiguration. |
| Användare kör hello installationsguiden |Om du använder en fullständig SQLServer: DBO (eller liknande) av motorn hello sync-databas |Gör databasen nivån ändringar, till exempel uppdaterar tabeller med nya kolumner. |

## <a name="more-about-hello-created-accounts"></a>Mer information om hello skapat konton
### <a name="active-directory-account"></a>Active Directory-konto
Om du använder standardinställningarna skapas ett konto i Active Directory som används för synkronisering. hello som skapade kontot finns i hello skogsrotdomän i behållaren för hello-användare och har namnet med prefixet **MSOL_**. hello-kontot har skapats med ett långt komplexa lösenord som inte upphör. Om du har en lösenordsprincip i din domän, se till att långa och komplexa lösenord skulle vara tillgängliga för det här kontot.

![AD-kontot](./media/active-directory-aadconnect-accounts-permissions/adsyncserviceaccount.png)

Om du använder anpassade inställningar, är du ansvarar för att skapa hello konto innan du påbörjar installationen av hello.

### <a name="azure-ad-connect-sync-service-account"></a>Azure AD Connect sync-tjänstkonto
hello sync-tjänsten kan köras under olika konton. Det kan köras en **virtuella tjänstkonto** (VSA), en **Grupphanterat tjänstkonto** (gMSA/sMSA), eller ett vanligt användarkonto. hello stöds alternativ har ändrats med hello 2017 April versionen av Connect när du gör en ny installation. Dessa ytterligare alternativ är inte tillgängliga om du uppgraderar från en tidigare version av Azure AD Connect.

| Typ av konto | Installationsalternativ | Beskrivning |
| --- | --- | --- |
| [Virtuella tjänstkonto](#virtual-service-account) | Snabb och anpassade, 2017 April och senare | Detta är hello-alternativ som används för alla express-installationer, förutom installationer på en domänkontrollant. Anpassad är det hello standardalternativet såvida inte används av ett annat alternativ. |
| [Grupphanterat tjänstkonto](#group-managed-service-account) | Anpassade, 2017 April och senare | Om du använder en fjärransluten SQLServer, så vi rekommenderar toouse en grupp Hanterat tjänstkonto. |
| [Användarkonto](#user-account) | Snabb och anpassade, 2017 April och senare | Ett användarkonto med AAD_ prefixet skapas endast under installationen installeras på Windows Server 2008 och installeras på en domänkontrollant. |
| [Användarkonto](#user-account) | Snabb och anpassade, 2017 mars och tidigare | Ett lokalt konto med AAD_ prefixet skapas under installationen. När du använder anpassad installation, går att ange ett annat konto. |

Om du använder Anslut med en version från 2017 mars eller tidigare, och sedan inte bör du återställa hello lösenordet för tjänstkontot för hello sedan Windows förstör hello krypteringsnycklar av säkerhetsskäl. Du kan inte ändra hello konto tooany andra konto utan att installera om Azure AD Connect. Om du uppgraderar tooa build från 2017 April eller senare, sedan är det stöds toochange hello lösenordet för tjänstkontot för hello men inte ändra hello-konto som används.

> [!Important]
> Du kan bara ange hello-tjänstkontot på första installationen. Det är inte stöds toochange hello-tjänstkontot efter hello-installationen har slutförts.

Detta är en tabell med hello standard rekommenderas, och alternativ som stöds för hello synkronisera tjänstkonto.

Förklaringen:

- **Fetstil** anger hello standardalternativet och i de flesta fall hello rekommenderas.
- *Kursiv* anger hello rekommenderat alternativ när det inte är hello standardalternativet.
- 2008 - standardalternativet när installerad på Windows Server 2008
- Icke-bold - alternativ som stöds
- Lokalt konto - lokala användarkontot på hello-server
- Domänkonto - domänanvändarkonto
- sMSA - [fristående Hanterat tjänstkonto](https://technet.microsoft.com/library/dd548356.aspx)
- gMSA - [gruppen Hanterat tjänstkonto](https://technet.microsoft.com/library/hh831782.aspx)

| | LocalDB</br>Express | LocalDB/LocalSQL</br>Anpassat | Fjärr-SQL</br>Anpassat |
| --- | --- | --- | --- |
| **fristående eller arbetsgrupp datorn** | Stöds inte | **VSA**</br>Lokalt konto (2008)</br>Lokalt konto |  Stöds inte |
| **domänanslutna datorer** | **VSA**</br>Lokalt konto (2008) | **VSA**</br>Lokalt konto (2008)</br>Lokalt konto</br>Domänkonto</br>sMSA gMSA | **gMSA**</br>Domänkonto |
| **Domänkontrollant** | **Domänkonto** | *gMSA*</br>**Domänkonto**</br>sMSA| *gMSA*</br>**Domänkonto**|

#### <a name="virtual-service-account"></a>Virtuella tjänstkonto
En virtuell tjänstkontot är en särskild typ av konto som inte har ett lösenord och hanteras av Windows.

![VSA](./media/active-directory-aadconnect-accounts-permissions/aadsyncvsa.png)

hello VSA är avsedda toobe som används med scenarier där hello Synkroniseringsmotorn och SQL är på hello samma server. Om du använder fjärr-SQL, så vi rekommenderar toouse en [Grupphanterat tjänstkonto](#managed-service-account) i stället.

Den här funktionen kräver Windows Server 2008 R2 eller senare. Om du installerar Azure AD Connect på Windows Server 2008 och sedan hello installationen faller tillbaka toousing en [användarkontot](#user-account) i stället.

#### <a name="group-managed-service-account"></a>Grupphanterade tjänstkontot
Om du använder en fjärransluten SQLServer, så vi rekommenderar toousing en **grupp Hanterat tjänstkonto**. Mer information om hur tooprepare din Active Directory för gruppen Hanterat tjänstkonto, se [grupp hanterade tjänstkonton – översikt](https://technet.microsoft.com/library/hh831782.aspx).

Det här alternativet på hello toouse [installera nödvändiga komponenter](active-directory-aadconnect-get-started-custom.md#install-required-components) väljer **använder ett befintligt tjänstkonto**, och välj **Hanterat tjänstkonto**.  
![VSA](./media/active-directory-aadconnect-accounts-permissions/serviceaccount.png)  
Det är också stöds toouse en [fristående Hanterat tjänstkonto](https://technet.microsoft.com/library/dd548356.aspx). Men de kan bara användas på den lokala datorn för hello och det finns ingen fördel toouse dem över hello standard virtuella-tjänstkontot.

Den här funktionen kräver Windows Server 2012 eller senare. Om du behöver toouse ett äldre operativsystem och använda fjärr-SQL och sedan måste du använda en [användarkontot](#user-account).

#### <a name="user-account"></a>Användarkonto
Ett lokalt tjänstkonto som skapas av installationsguiden för hello (såvida du inte anger hello konto toouse i anpassade inställningar). hello konto prefixet **AAD_** och används för hello faktiska sync service toorun som. Om du installerar Azure AD Connect på en domänkontrollant skapas hello konto hello domän. Hej **AAD_** -tjänstkontot måste finnas i hello domän om:
   - du använder en fjärrserver som kör SQLServer
   - du använder en proxyserver som kräver autentisering

![Synkroniseringstjänstkontot](./media/active-directory-aadconnect-accounts-permissions/syncserviceaccount.png)

hello-kontot har skapats med ett långt komplexa lösenord som inte upphör.

Det här kontot har använt toostore hello lösenorden för hello andra konton på ett säkert sätt. Andra konton lösenord lagras krypterade i hello-databasen. hello privata nycklar för hello krypteringsnycklar är skyddade med hello kryptografiska tjänster hemlig nyckel kryptering med Windows Data Protection API (DPAPI).

Om du använder en fullständig SQL Server är hello-tjänstkontot hello DBO hello Synkroniseringsmotorn hello skapade databasen. hello-tjänsten kommer inte att fungera som avsett med andra behörigheter. En SQL-inloggning skapas också.

hello kontot också ges behörighet toofiles, registernycklar och andra relaterade objekt toohello Synkroniseringsmotorn.

### <a name="azure-ad-service-account"></a>Azure AD-tjänstkonto
Ett konto i Azure AD har skapats för användning av tjänsten hello-synkronisering. Det här kontot kan identifieras av dess namn.

![AD-kontot](./media/active-directory-aadconnect-accounts-permissions/aadsyncserviceaccount.png)

hello namnet på hello serverkontot hello används på kan identifieras i hello andra delen av hello användarnamn. Hello-servernamn är FABRIKAMCON i hello bild. Om du har mellanlagring servrar har varje server ett eget konto.

hello-tjänstkontot har skapats med ett långt komplexa lösenord som inte upphör. Det har beviljats en särskild roll **synkronisering Katalogkonton** som har synkroniseringar behörigheter endast tooperform directory. Den här särskilda inbyggda rollen kan inte ges utanför hello Azure AD Connect-guiden. hello Azure-portalen visar det här kontot med hello rollen **användaren**.

Det finns en gräns på 20 tjänstkonton för synkronisering i Azure AD. tooget hello lista över befintliga Azure AD-tjänstkonton i din Azure AD kör hello följande Azure AD PowerShell-cmdlet:`Get-AzureADDirectoryRole | where {$_.DisplayName -eq "Directory Synchronization Accounts"} | Get-AzureADDirectoryRoleMember`

tooremove oanvända Azure AD-tjänstkonton, kör hello följande Azure AD PowerShell-cmdlet:`Remove-AzureADUser -ObjectId <ObjectId-of-the-account-you-wish-to-remove>`

## <a name="next-steps"></a>Nästa steg
Läs mer om hur du [integrerar dina lokala identiteter med Azure Active Directory](../active-directory-aadconnect.md).
