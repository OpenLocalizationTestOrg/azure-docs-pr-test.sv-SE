---
title: "säkerhet för aaaStorSimple 8000-serien | Microsoft Docs"
description: "Beskriver hello säkerhet och sekretessfunktioner som skyddar din StorSimple-tjänsten, enhet och data lokalt och i molnet hello."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/02/2017
ms.author: alkohli
ms.openlocfilehash: 48dd449d2908c21fe05d0ed37a4dc6f3e306e43b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-security-and-data-protection"></a>StorSimple-säkerhet och dataskydd

## <a name="overview"></a>Översikt

Säkerhet är en stor utmaning för alla som inför en ny teknik, särskilt när hello teknik används med konfidentiella eller skyddade data. Medan du utvärderar olika tekniker måste du ökade risker och kostnader för dataskydd. Microsoft Azure StorSimple ger både säkerhet och sekretess lösning för skydd av data, hjälper tooensure:

* **Sekretess** – endast entiteter auktoriserade kan visa dina data.
* **Integritet** – endast behöriga entiteter kan ändra eller ta bort dina data.

hello Microsoft Azure StorSimple-lösningen består av fyra huvudsakliga komponenter som samverkar med varandra:

* **Enhetshanteraren för StorSimple-tjänst som finns i Microsoft Azure** – hello management-tjänsten för att du använder tooconfigure och etablera hello StorSimple-enhet.
* **StorSimple-enhet** – en fysisk enhet som är installerad i ditt datacenter. Alla värdar och -klienter som genererar data ansluta toohello StorSimple-enhet och hello enheten hanterar hello data och flyttar det toohello Azure-molnet vid behov.
* **Klienter/värdar anslutna toohello enhet** – hello klienter i din infrastruktur som ansluter toohello StorSimple-enhet och generera data som behöver toobe skyddas.
* **Molnlagring** – hello plats i hello Azure-molnet var data lagras.

hello beskrivs följande avsnitt hello StorSimple säkerhetsfunktioner som skyddar var och en av dessa komponenter och hello data som lagras på dem. Den innehåller också en lista med frågor som kan uppstå om Microsoft Azure StorSimple-säkerhet och motsvarande hello-svar.

## <a name="storsimple-device-manager-service-protection"></a>StorSimple Enhetshanteraren service skydd

Hej StorSimple enheten Manager-tjänsten är en management-tjänst finns i Microsoft Azure och används toomanage alla StorSimple-enheter som din organisation har skaffat. Du kan komma åt hello StorSimple enheten Manager-tjänsten med hjälp av din organisations autentiseringsuppgifter toolog på toohello Azure-portalen via en webbläsare.

Åtkomst toohello StorSimple enheten Manager-tjänsten kräver att din organisation har en Azure-prenumeration som innehåller StorSimple. Din prenumeration styr hello-funktioner som du kan komma åt i hello Azure-portalen. Om din organisation inte har en Azure-prenumeration och du vill toolearn mer om dem, se [registrera dig för Azure som en organisation](../active-directory/sign-up-organization.md).

Eftersom hello StorSimple enheten Manager-tjänsten finns i Azure, skyddas den av hello Azure säkerhetsfunktioner. Mer information om hello säkerhetsfunktioner som tillhandahålls av Microsoft Azure finns i toohello [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/security/).

## <a name="storsimple-device-protection"></a>Skydd för StorSimple-enhet

Hej StorSimple-enhet är en lokal hybrid-lagringsenhet som innehåller solid-state-hårddiskar (SSD) och hårddiskenheter (HDD) tillsammans med funktioner för automatisk redundans och redundanta styrenheter. hello domänkontrollanter hantera lagring skiktning, placera för närvarande används (eller varm) data på lokal lagring (i hello StorSimple-enhet eller lokala servrar) när du flyttar data som används mindre ofta toohello moln.

Enbart behöriga StorSimple enheter tillåts toojoin hello Enhetshanteraren för StorSimple-tjänsten som du skapade i Azure-prenumerationen. tooauthorize en enhet, måste du registrera den med hello StorSimple enheten Manager-tjänsten genom att tillhandahålla hello nyckel för tjänstregistrering. hello nyckel för tjänstregistrering är en 128-bitars slumpmässig nyckel genereras i hello Azure-portalen.

![Nyckel för tjänstregistrering](./media/storsimple-security/ServiceRegistrationKey.png)

toolearn så får en service registrering går för[steg 2: hämta hello nyckel för tjänstregistrering](storsimple-8000-deployment-walkthrough-u2.md#step-2-get-the-service-registration-key).

hello nyckel för tjänstregistrering är en lång nyckel som innehåller 100 tecken. Du kan kopiera hello nyckeln och spara den i en textfil på en säker plats så att du kan använda den tooauthorize ytterligare enheter efter behov. Om hello nyckel för tjänstregistrering försvinner när du har registrerat din första enhet, kan du generera en ny nyckel från hello StorSimple enheten Manager-tjänsten. Detta påverkar inte hello åtgärd för befintliga enheter.

När en enhet har registrerats använder token toocommunicate med Microsoft Azure. hello nyckel för tjänstregistrering används inte när registrering av enheten.

> [!NOTE]
> Vi rekommenderar att du återskapar hello nyckel för tjänstregistrering efter varje användning.


## <a name="protect-your-storsimple-solution-via-passwords"></a>Skydda din StorSimple-lösning via lösenord

Lösenord är en viktig del av datorsäkerhet och används överallt i hello StorSimple-lösningen toohelp kontrollerar du att dina data är endast tillgänglig tooauthorized användare. StorSimple kan tooconfigure hello följande lösenord:

* StorSimple-enhetens administratörslösenord
* Challenge Handshake Authentication Protocol (CHAP) initierare och mål-lösenord
* Lösenordet för StorSimple Snapshot Manager

### <a name="windows-powershell-for-storsimple-and-hello-storsimple-device-administrator-password"></a>Windows PowerShell för StorSimple och hello StorSimple-enhetens administratörslösenord

Windows PowerShell för StorSimple är ett kommandoradsgränssnitt som du kan använda toomanage hello StorSimple-enhet. Windows PowerShell för StorSimple har funktioner som gör att du tooregister enheten, konfigurera hello nätverksgränssnittet på din enhet, installera vissa typer av uppdateringar, felsöka enheten genom att öppna hello stöd sessionen och ändra hello enhetens tillstånd . Du kan komma åt Windows PowerShell för StorSimple anslutande toohello seriekonsolen på hello enhet eller med Windows PowerShell-fjärrkommunikation.

PowerShell-fjärrkommunikation kan ske över HTTPS eller HTTP. Om fjärrhantering över HTTPS är aktiverat måste toodownload hello fjärrhantering av certifikat från hello enhet och installera det på hello fjärrklienter. Mer information om PowerShell-fjärrkommunikation finns för[fjärransluta tooyour StorSimple-enhet](storsimple-8000-remote-connect.md).

När du använder Windows PowerShell för StorSimple tooconnect toohello-enhet behöver tooprovide hello enheten administratör lösenord toolog på toohello enhet.

![Enhetens administratörslösenord](./media/storsimple-security/DeviceAdminPW.png)

Behåll hello följa bästa praxis i åtanke:

* Fjärrhantering är inaktiverat som standard. Du kan använda hello StorSimple Enhetshanteraren service tooenable den. Som en säkerhetsåtgärd fjärråtkomst ska aktiveras endast när hello tidsperiod som faktiskt krävs.
* Om du ändrar hello lösenord kan vara att toonotify alla fjärranslutna användare så att de inte har ett oväntat anslutningen inte bryts.
* Hej StorSimple enheten Manager-tjänsten kan inte hämta befintliga lösenord: den kan bara återställa dem. Vi rekommenderar att du lagrar alla lösenord på en säker plats så att du inte har tooreset ett lösenord om det har glömt. Om du behöver tooreset ett lösenord, vara säker på att toonotify alla användare innan du återställa den.

Du kan komma åt hello Windows PowerShell-gränssnittet med hjälp av en seriell anslutning toohello enhet. Du kan också komma åt den via fjärranslutning via HTTP eller HTTPS, som ger ökad säkerhet. HTTPS ger högre säkerhet än en serie- eller HTTP-anslutning. Dock toouse HTTPS, du måste först installera ett certifikat på hello klientdator som kommer att använda hello enhet. Du kan hämta hello fjärråtkomst certifikat från konfigurationssidan för hello enheten i hello StorSimple enheten Manager-tjänsten. Om hello certifikat för fjärråtkomst tappas bort, måste du hämta ett nytt certifikat och sprida den tooall klienter som är auktoriserade toouse fjärrhantering.

### <a name="challenge-handshake-authentication-protocol-chap-initiator-and-target-passwords"></a>Challenge Handshake Authentication Protocol (CHAP) initierare och mål-lösenord

CHAP är ett autentiseringsschema som används av hello StorSimple enheten toovalidate hello identiteten för fjärrklienter. hello verifiering är baserad på ett delat lösenord. CHAP kan vara enkelriktade (enkelriktad) eller ömsesidig (dubbelriktad). Enkelriktade CHAP hello målet (hello StorSimple-enhet) som autentiserar en initierare (värd). Ömsesidig eller omvänd CHAP kräver att hello mål autentisera hello initieraren och sedan hello initieraren autentisera hello mål. Din StorSimple kan vara konfigurerade toouse båda metoderna.

Tänk på följande hello när du konfigurerar CHAP:

* hello CHAP-användarnamn måste innehålla färre än 233 tecken.
* hello CHAP-lösenord måste vara mellan 12 och 16 tecken. Försök toouse resulterar längre användarnamn eller lösenord i ett autentiseringsfel på hello Windows-värd.
* Du kan inte använda hello samma lösenord för både hello CHAP-initieraren och hello CHAP-målet.
* När du har angett hello lösenord kan ändras, men det går inte att hämta. Om hello lösenord har ändrats kan vara att toonotify alla fjärranslutna användare så att de kan ansluta toohello StorSimple-enhet.

Mer information om CHAP och hur tooconfigure den för din StorSimple-lösning för[konfigurera CHAP för din StorSimple-enhet](storsimple-8000-configure-chap.md).

### <a name="storsimple-snapshot-manager-password"></a>Lösenordet för StorSimple Snapshot Manager

StorSimple Snapshot Manager är en snapin-modul i Microsoft Management Console (MMC) som använder volymen grupper och hello Windows Volume Shadow Copy Service toogenerate programkonsekvent säkerhetskopiering. Du kan dessutom använda StorSimple Snapshot Manager toocreate scheman för säkerhetskopiering och klona eller återställa volymer.

När du konfigurerar en enhet toouse StorSimple Snapshot Manager, kommer du att lösenordet för nödvändiga tooprovide hello StorSimple Snapshot Manager. Lösenordet anges först i Windows PowerShell för StorSimple under registreringen. hello lösenord kan också ställa in och ändras från hello StorSimple enheten Manager-tjänsten. Det här lösenordet autentiseras hello enhet med StorSimple Snapshot Manager.

![Lösenordet för StorSimple Snapshot Manager](./media/storsimple-security/SnapshotMgrPassword.png)

hello lösenordet för StorSimple Snapshot Manager måste vara 14 too15 tecken och måste innehålla minst 3 av en kombination av versaler, gemener, siffror och särskilda tecken. När du ställer in hello lösenordet för StorSimple Snapshot Manager kan ändras, men det går inte att hämta. Om du ändrar hello lösenord kan vara att toonotify alla fjärranvändare.

Mer information om StorSimple Snapshot Manager finns för[vad är StorSimple Snapshot Manager?](storsimple-what-is-snapshot-manager.md)

### <a name="password-best-practices"></a>Metodtips för lösenord

Vi rekommenderar att du använder hello följande riktlinjer toohelp garanterar att StorSimple-lösenord är starkt och väl skyddade:

* Ändra ditt lösenord var tredje månad. Ändra hello lösenord tillämpas per år.
* Använd starka lösenord. Mer information finns för[skapa starkare lösenord och skydda dem](http://blogs.microsoft.com/cybertrust/2014/08/25/create-stronger-passwords-and-protect-them/).
* Alltid använda olika lösenord för olika mekanismer; var och en av hello-lösenord som du anger måste vara unika.
* Dela inte lösenord med personer som inte är auktoriserade tooaccess hello StorSimple-enhet.
* Tala om ett lösenord framför andra eller inte tipset på hello format för ett lösenord.
* Om du misstänker att ett konto eller lösenord har komprometterats bör rapportera hello incident tooyour information säkerhetsavdelningen.
* Hantera alla lösenord som känslig och konfidentiell information. 

## <a name="storsimple-data-protection"></a>Dataskydd för StorSimple

Det här avsnittet beskriver hello StorSimple säkerhetsfunktioner som skyddar data under överföring och lagrade data.

Enligt beskrivningen i andra avsnitt har använt tooauthorize lösenord och autentiserar användare innan de kan få åtkomst till tooyour StorSimple-lösningen. Ett annat övervägande för säkerhet skyddar data från obehöriga användare när det överförs mellan lagringssystem och samtidigt som det lagras. hello beskrivs följande avsnitt hello Dataskyddsfunktioner med StorSimple.

> [!NOTE]
> Deduplicering ger ytterligare skydd för data som lagras på hello StorSimple-enhet och i Microsoft Azure storage. När data är deduplicerad hello dataobjekt lagras separat från hello metadata som används toomap och komma åt dem: det finns inga tillgängliga lagringsnivå kontexten tooreconstruct hello data baserat på volymstruktur, filsystemet eller filnamn.


## <a name="protect-data-flowing-through-hello-service"></a>Skydda data som passerar genom hello-tjänsten

hello Huvudsyftet med hello StorSimple enheten Manager-tjänsten är toomanage och konfigurerar hello StorSimple-enhet. Hej StorSimple enheten Manager-tjänsten körs i Microsoft Azure. Du använder hello Azure portal tooenter konfigurationsdata för enheten och sedan Microsoft Azure använder hello StorSimple Enhetshanteraren service toosend hello data toohello enheten. StorSimple använder ett system med asymmetriska nyckelpar toohelp se till att en kompromettering av hello Azure-tjänsten inte resulterar i en kompromettering av lagras informationen.

![Kryptering av data som rör sig](./media/storsimple-security/DataEncryption.png)

hello asymmetriska nycklar system skyddar hello data som flödar genom hello-tjänsten på följande sätt:

1. Ett certifikat för kryptering av data som använder ett asymmetriskt offentliga och privata nyckelpar genereras på hello enheten och använda tooprotect hello data. hello nycklar genereras när hello första enheten är registrerad.
2. hello certifikat datakrypteringsnycklar exporteras till en Personal Information Exchange (.pfx)-fil som skyddas av hello krypteringsnyckel för tjänstdata, vilket är en stark 128-bitars nyckel som genereras slumpmässigt av hello första enheten under registreringen.
3. hello offentliga nyckeln för certifikatet för hello görs på ett säkert sätt tillgängliga toohello StorSimple enheten Manager-tjänsten och hello privata nyckeln sparas med hello enhet.
4. Data som anger hello-tjänsten är krypterat med hello offentliga nyckel och dekrypteras med hello privata nyckel som lagras på enheten hello säkerställer att hello Azure-tjänsten kan inte dekryptera hello data som flödar toohello enhet.

krypteringsnyckel för tjänstdata hello genereras på endast hello första enheten registrerad med hello-tjänsten. Alla efterföljande enheter som registreras med hello-tjänsten måste använda hello samma krypteringsnyckel för tjänstdata.

> [!IMPORTANT]
> Det är mycket viktigt toomake en kopia av krypteringsnyckeln för tjänstdata hello och spara den på en säker plats. En kopia av krypteringsnyckeln för tjänstdata hello bör lagras på ett sådant sätt att den kan användas av en behörig person och kan enkelt meddelas toohello enhetsadministratör.
> 
> Om hello krypteringsnyckel för tjänstdata tappas bort, kan en Microsoft-supporten hjälpa dig tooretrieve under förutsättning att du har minst en enhet i ett onlinetillstånd. Vi rekommenderar att du ändrar hello krypteringsnyckel för tjänstdata när den har hämtats. Anvisningar finns för[ändra hello krypteringsnyckel för tjänstdata](storsimple-service-dashboard.md#change-the-service-data-encryption-key).

Du kan ändra hello krypteringsnyckel för tjänstdata och hello-krypteringscertifikat för motsvarande data genom att välja hello **ändra krypteringsnyckel för tjänstdata** alternativet på instrumentpanelen för hello-tjänsten. tooensure att datasäkerheten inte komprometteras, måste du använda en fysiska StorSimple-enhet toochange hello krypteringsnyckel för tjänstdata. Ändra hello krypteringsnycklar kräver att alla enheter kan uppdateras med hello ny nyckel. Därför rekommenderar vi att du ändrar hello nyckel när alla enheter som är online. Om enheter som är offline, kan deras nycklar ändras vid en annan tid. hello-enheter med inaktuella nycklar kommer fortfarande att kunna toorun säkerhetskopieringar, men de kommer inte att kunna toorestore data förrän hello nyckeln uppdateras. Mer information finns för[Använd hello StorSimple Enhetshanteraren service instrumentpanelen](storsimple-8000-service-dashboard.md).

krypteringsnyckel för tjänstdata hello och hello-certifikat för kryptering av data ut inte. Vi rekommenderar dock att du ändrar hello service datakryptering viktiga årligen toohelp förhindra att nyckeln har komprometterats.

## <a name="protect-data-at-rest"></a>Skydda data i vila

Hej StorSimple-enheten hanterar data genom att lagra det i nivåerna lokalt och i hello moln, beroende på användning. Alla värdar för datorer som är anslutna toohello enheten skicka data toohello enheten, vilket flyttar data toohello moln, efter behov. Data överförs från hello enhet toohello moln på ett säkert sätt hello Internet. Varje enhet har en iSCSI-mål som hämtar alla volymer på den enheten. Alla data krypteras innan den skickas toocloud lagring. 

![Krypteringsnyckel för molnlagring](./media/storsimple-security/CloudStorageEncryption.png)

toohelp garantera hello säkerheten och integriteten hos data flyttas toohello molnet, StorSimple kan du toodefine cloud storage krypteringsnycklar på följande sätt:

* Du kan ange hello krypteringsnyckel för molnlagring när du skapar en volymbehållare. hello nyckel kan inte ändras eller läggs till senare.
* Alla volymer på en volym behållaren resurs hello samma krypteringsnyckel. Om du vill att en annan form av kryptering för en viss volym, rekommenderar vi att du skapar en ny volym behållaren toohost volymen.
* När du anger hello krypteringsnyckel för molnlagring i hello StorSimple Enhetshanteraren service krypteras hello nyckeln med hello offentliga delen av krypteringsnyckeln för tjänstdata hello och skickas sedan toohello enhet.
* krypteringsnyckel för molnlagring hello lagras inte någonstans i hello-tjänsten och är känd endast toohello enheten.
* Ange en krypteringsnyckel för molnlagring är valfritt. Du kan skicka data som har krypterats med hello toohello värdenhet.

### <a name="additional-security-best-practices"></a>Metodtips för ökad säkerhet

* Dela trafik: isolera din iSCSI SAN-nätverk från användartrafik i ett Företagsnätverk genom att distribuera ett helt separata nätverk och använda VLAN, där fysisk isolering inte är ett alternativ. Ett dedikerat nätverk för iSCSI-lagring garanterar hello säkerhet och prestanda för affärskritiska data. Blanda lagrings- och trafik via en LAN rekommenderas inte och ökar svarstiden och orsaka nätverksfel.
* Använda nätverksgränssnitt som stöder TCP/IP-avlastning Engine (TOE) för värden på klientsidan nätverkssäkerhet. TOE minskar CPU-belastningen genom att behandla TCP på hello nätverkskort.

## <a name="protect-data-via-storage-accounts"></a>Skydda data via storage-konton

Varje Microsoft Azure-prenumeration kan skapa en eller flera lagringskonton. (Ett lagringskonto ger ett unikt namnområde för att arbeta med data som lagras i hello Azure-molnet.) Storage-konto för åtkomst tooa styrs av hello prenumeration och åtkomst nycklar som är associerade med detta lagringskonto.

När du skapar ett lagringskonto genererar Microsoft Azure två 512-bitars lagring åtkomstnycklar, varav används för autentisering när hello StorSimple-enheten ansluter till hello storage-konto. Observera att endast en av dessa nycklar används. hello lagras andra nyckeln i reserv, så att du toorotate hello nycklar med jämna mellanrum. toorotate nycklar, gör du hello sekundärnyckeln active, och sedan ta bort hello primärnyckel. Du kan sedan skapa en ny nyckel för användning under hello nästa rotation. (Av säkerhetsskäl kräver många Datacenter viktiga rotation.)

Vi rekommenderar att du följer dessa bästa praxis för viktiga rotation:

* Du ska rotera lagringskontonycklar regelbundet toohelp se till att ditt lagringskonto inte kan nås av obehöriga användare.
* Administratören Azure bör med jämna mellanrum, ändra eller återskapa hello primära och sekundära nycklarna genom att använda hello lagring av hello Azure portal toodirectly åtkomst hello storage-konto.

## <a name="protect-data-via-encryption"></a>Skydda data via kryptering

StorSimple använder hello följande krypteringsalgoritmer tooprotect data lagras i eller reser mellan hello komponenter i din StorSimple-lösning.

| Algoritmen | Nyckellängd | Kommentarer-protokoll/program |
| --- | --- | --- |
| RSA |2048 |1 för RSA-PKCS-v1.5 används av hello Azure portal tooencrypt konfigurationsdata som skickas toohello enhet: till exempel lagring kontoautentiseringsuppgifter, StorSimple enhetskonfiguration och cloud storage krypteringsnycklar. |
| AES |256 |AES med CBC är används tooencrypt hello offentliga delen av krypteringsnyckeln för tjänstdata hello innan den skickas toohello Azure-portalen från hello StorSimple-enhet. Det används också av hello StorSimple enheten tooencrypt data innan hello data skickas toohello cloud storage-konto. |

## <a name="storsimple-cloud-appliance-security"></a>Säkerhet för StorSimple-enhet för molnet

[!INCLUDE [storsimple Cloud Appliance security](../../includes/storsimple-virtual-device-security.md)]

## <a name="frequently-asked-questions-faq"></a>Vanliga frågor och svar (FAQ)

hello följande är några frågor och svar om säkerhet och Microsoft Azure StorSimple.

**F:** min tjänst har komprometterats. Vad bör vara nästa steg om jag?

**S:** omedelbart bör du ändra hello krypteringsnyckel för tjänstdata och hello lagringskontonycklar för hello storage-konto som används för lagringsnivåer data. Anvisningar finns:

* [Ändra hello krypteringsnyckel för tjänstdata](storsimple-service-dashboard.md#change-the-service-data-encryption-key)
* [Viktiga rotation av storage-konton](storsimple-8000-manage-storage-accounts.md#key-rotation-of-storage-accounts)

**F:** jag har en ny StorSimple-enhet som frågar efter hello nyckel för tjänstregistrering. Hur hämtar den?

**S:** den här nyckeln skapas när du först skapade hello StorSimple enheten Manager-tjänsten. Du kan använda hello service Snabbstart sidan tooview eller generera hello nyckel för tjänstregistrering när du använder hello StorSimple Enhetshanteraren service tooconnect toohello enhet. Generera en ny nyckel för tjänstregistrering påverkar inte hello befintliga registrerade enheter. Anvisningar finns:

* [Visa eller återskapa hello nyckel för tjänstregistrering](storsimple-8000-manage-service.md##regenerate-the-service-registration-key)

**F:** försvann min krypteringsnyckel för tjänstdata. Vad gör jag nu?

**S:** kontaktar Microsoft Support. De kan logga in tooa support-session på enheten och hjälpen du hämta hello nyckel (förutsatt att minst en enhet är online). Omedelbart när du har fått hello krypteringsnyckel för tjänstdata, bör du ändra den tooensure känd hello nya nyckeln endast tooyou. Anvisningar finns:

* [Ändra hello krypteringsnyckel för tjänstdata](storsimple-service-dashboard.md#change-the-service-data-encryption-key)

**F:** jag behörighet en enhet för en service data encryption key ändring, men kunde inte starta hello ändringprocessen. Vad ska jag göra?

**S:** om hello timeout har upphört att gälla, kommer du behöver tooreauthorize hello enhet för ändringen för viktiga hello-service-kryptering och starta hello processen igen.

**F:** jag ändrade hello krypteringsnyckel för tjänstdata, men jag kunde inte tooupdate hello andra enheter inom fyra timmar. Måste jag toostart igen?

**S:** hello 4 timmar är endast för initiering av hello ändringen. När du börjar uppdateringsprocessen för hello på hello godkänd StorSimple-enhet, hello auktorisering är giltig förrän alla enheter har uppdaterats.

**F:** vår StorSimple-administratören har lämnat företaget hello. Vad ska jag göra?

**S:** och lösenordsåterställning hello lösenord som tillåter åtkomst toohello StorSimple-enhet och ändra hello tjänsten data encryption key tooensure att hello ny information inte är känt toounauthorized personal. Anvisningar finns:

* [Använd hello StorSimple Enhetshanteraren service toochange storsimple-lösenord](storsimple-8000-change-passwords.md)
* [Ändra hello krypteringsnyckel för tjänstdata](storsimple-service-dashboard.md#change-the-service-data-encryption-key)
* [Konfigurera CHAP för din StorSimple-enhet](storsimple-8000-configure-chap.md)

**F:** jag vill tooprovide hello StorSimple Snapshot Manager lösenord tooa värden som ansluter toohello StorSimple-enhet, men hello lösenord är inte tillgänglig. Vad kan jag göra?

**S:** om du har glömt hello lösenord, bör du skapa en ny. Glöm sedan tooinform alla befintliga användare som hello lösenord har ändrats och att de ska uppdatera sina klienter toouse hello nytt lösenord. Anvisningar finns:

* [Ändra hello lösenordet för StorSimple Snapshot Manager](storsimple-8000-change-passwords.md#set-the-storsimple-snapshot-manager-password)
* [Autentisera en enhet](storsimple-snapshot-manager-manage-devices.md#authenticate-a-device)

**F:** hello certifikat för fjärråtkomst toohello Windows PowerShell för StorSimple har ändrats på hello enhet. Hur uppdaterar jag min fjärråtkomstklienter?

**S:** du kan hämta hello nya certifikat från hello StorSimple enheten Manager-tjänsten och sedan tillhandahålla den toobe installerad i hello certifikatarkivet för fjärranslutna klienter. Anvisningar finns:

* [Importera certifikat cmdlet](https://technet.microsoft.com/library/hh848630.aspx)

**F:** är Mina data skyddas om hello StorSimple Enhetshanteraren service äventyras?

**S:** Service configuration data alltid krypteras med den offentliga nyckeln när du visar den i en webbläsare. Eftersom hello-tjänsten inte har åtkomst toohello privata nyckel, hello tjänsten kommer inte att kunna toosee några data. Om hello StorSimple Enhetshanteraren service äventyras, påverkas inte, eftersom det inte finns några nycklar som lagras i hello StorSimple enheten Manager-tjänsten.

**F:** om någon får tillgång toohello datakrypteringscertifikatet kommer Mina data äventyras?

**S:** Microsoft Azure lagrar hello kundens datakrypteringsnyckeln (.pfx-fil) i krypterat format. Eftersom hello .pfx-filen är krypterad och hello StorSimple-tjänsten har inte hello tjänsten data encryption key toodecrypt hello .pfx-fil kan gränssnittshändelsen bara få åtkomst till toohello .pfx-fil inte alla hemligheter skapas.

**F:** vad händer om en statliga entitet begär Microsoft Mina data?

**S:** eftersom alla hello data krypteras på hello-tjänsten och hello privata nyckeln sparas med hello enhet hello statliga entitet begära hello kunden hello data.

## <a name="next-steps"></a>Nästa steg

[Distribuera din StorSimple-enhet](storsimple-8000-deployment-walkthrough-u2.md).

