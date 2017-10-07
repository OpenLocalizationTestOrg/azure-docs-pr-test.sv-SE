---
title: "aaaOverview för åtkomstkontroll i Data Lake Store | Microsoft Docs"
description: "Förstå åtkomstkontroll i Azure Data Lake Store"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: d16f8c09-c954-40d3-afab-c86ffa8c353d
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: 1cc5d578f22ef0a123a1547abebfb4795ea09139
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="access-control-in-azure-data-lake-store"></a>Åtkomstkontroll i Azure Data Lake Store

Azure Data Lake Store implementerar en åtkomstkontrollmodellen som härleds från HDFS, som i sin tur härleds från hello POSIX modellen för åtkomstkontroll. Den här artikeln sammanfattar hello grunderna i hello modellen för åtkomstkontroll för Data Lake Store. toolearn mer om hello HDFS åtkomstkontrollmodellen finns [HDFS behörigheter guiden](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html).

## <a name="access-control-lists-on-files-and-folders"></a>Åtkomstkontrollistor för filer och mappar

Det finns två sorters åtkomstkontrollistor (ACL:er), **Åtkomst-ACL:er** och **Standard-ACL:er**.

* **Åtkomst till ACL: er**: dessa kontrollobjekt åtkomst tooan. Både filer och mappar har åtkomst-ACL:er.

* **ACL: er som standard**: en ”mall” i ACL: er som är kopplade till en mapp som bestämmer hello åtkomst ACL för alla underordnade objekt som skapas i mappen. Filer har inte standard-ACL:er.

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-1.png)

Både åtkomst ACL: er och standard-ACL: er har hello samma struktur.

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-2.png)



> [!NOTE]
> Ändra hello standard ACL på en överordnad påverkar inte hello ACL Access eller standard ACL för underordnade objekt som redan finns.
>
>

## <a name="users-and-identities"></a>Användare och identiteter

Alla filer och mappar har olika behörigheter för dessa identiteter:

* hello ägande användare av hello-fil
* hello ägande grupp
* Namngivna användare
* Namngivna grupper
* Alla andra användare

hello identiteter för användare och grupper är identiteter med Azure Active Directory (AD Azure). Så om inget annat anges, ””, Hej kontexten för Data Lake Store kan antingen innebära en Azure AD-användare eller en Azure AD-säkerhetsgrupp.

## <a name="permissions"></a>Behörigheter

hello behörigheter för ett filsystem är **Läs**, **skriva**, och **kör**, och de kan användas för filer och mappar som visas i följande tabell hello:

|            |    Fil     |   Mapp |
|------------|-------------|----------|
| **Läsa (R)** | Kan läsa hello innehållet i en fil | Kräver **Läs** och **kör** toolist hello innehållet i hello-mappen|
| **Skriva (W)** | Kan skriva eller Lägg till tooa fil | Kräver **skriva** och **kör** toocreate underordnade objekt i en mapp |
| **Köra (X)** | Innebär inte någonting i hello kontexten för Data Lake Store | Nödvändiga tootraverse hello underordnade objekt i en mapp |

### <a name="short-forms-for-permissions"></a>Kortformat för behörigheter

**RWX** är används tooindicate **läsa + skriva + köra**. En mer komprimerad numeriska formatet finns där **Läs = 4**, **skriva = 2**, och **Execute = 1**, hello summan av som representerar hello behörigheter. Här följer några exempel.

| Numeriskt format | Kortformat |      Vad det innebär     |
|--------------|------------|------------------------|
| 7            | RWX        | Läsa + skriva + köra |
| 5            | R-X        | Läsa + köra         |
| 4            | R--        | Läsa                   |
| 0            | ---        | Inga behörigheter         |


### <a name="permissions-do-not-inherit"></a>Behörigheter ärvs inte

Behörigheter för ett objekt som lagras på själva hello objektnamnet i hello POSIX-typ modellen som används av Data Lake Store. Med andra ord kan behörighet för ett objekt inte ärvas från hello överordnade objekt.

## <a name="common-scenarios-related-toopermissions"></a>Vanliga scenarier relaterade toopermissions

Följande är några vanliga scenarier toohelp du förstår vilka behörigheter som krävs för tooperform vissa åtgärder på ett Data Lake Store-konto.

### <a name="permissions-needed-tooread-a-file"></a>Behörigheter som krävs för tooread en fil

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-3.png)

* Hello filen toobe läsa, hello i anroparen måste **Läs** behörigheter.
* För alla hello mapparna i mappstrukturen hello som innehåller hello filen hello anroparen måste **Execute** behörigheter.

### <a name="permissions-needed-tooappend-tooa-file"></a>Behörigheter som krävs för tooappend tooa fil

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-4.png)

* För hello filen toobe läggs till, hello anroparen måste **skriva** behörigheter.
* För alla hello mapparna som innehåller filen hello hello anroparen måste **Execute** behörigheter.

### <a name="permissions-needed-toodelete-a-file"></a>Behörigheter som krävs för toodelete en fil

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-5.png)

* För hello överordnade mappen hello anroparen måste **skriva + köra** behörigheter.
* För alla hello andra mappar i hello filsökväg hello anroparen måste **Execute** behörigheter.



> [!NOTE]
> Skriva behörigheterna för hello-fil inte är obligatoriska toodelete det så länge hello föregående två villkor är uppfyllda.
>
>

### <a name="permissions-needed-tooenumerate-a-folder"></a>Behörigheter som krävs för tooenumerate en mapp

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-6.png)

* Hello mappen tooenumerate hello i anroparen måste **Läs + Execute** behörigheter.
* För alla hello överordnade mappar hello anroparen måste **Execute** behörigheter.

## <a name="viewing-permissions-in-hello-azure-portal"></a>Visa behörigheter i hello Azure-portalen

Från hello **Data Explorer** bladet för hello Data Lake Store-konto, klickar du på **åtkomst** toosee hello ACL: er för en fil eller mapp. Klicka på **åtkomst** toosee hello ACL: er för hello **katalog** mapp under hello **mydatastore** konto.

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-show-acls-1.png)

På det här bladet visas hello överst en översikt över hello behörigheter som du har. (Hello skärmbild hello användare är Bob.) Efter att hello åtkomstbehörigheter visas. Efter att från och med hello **åtkomst** bladet, klickar du på **enkel vy** toosee hello enklare vy.

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-show-acls-simple-view.png)

Klicka på **Avancerat läge** toosee hello mer avancerat läge, där hello begreppet standard ACL: er, nätmask och en användare visas.

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-show-acls-advance-view.png)

## <a name="hello-super-user"></a>hello superanvändare

Superanvändare har hello de flesta rättigheter för alla hello användare i hello Data Lake Store. En superanvändare:

* Har RWX behörigheter för**alla** filer och mappar.
* Kan ändra hello behörigheter på en fil eller mapp.
* Ändra hello ägande användare eller äger grupp med en fil eller mapp.

I Azure har ett Data Lake Store-konto flera Azure-roller, inklusive:

* Ägare
* Deltagare
* Läsare

Alla i hello **ägare** rollen för ett Data Lake Store-konto är automatiskt Superanvändare för det kontot. Det finns fler toolearn [rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-configure.md).
Om du vill toocreate en anpassad roll-baserad-åtkomstkontroll (RBAC)-roll som har behörighet för en användare, måste toohave hello följande behörigheter:
- Microsoft.DataLakeStore/accounts/Superuser/action
- Microsoft.Authorization/roleAssignments/write


## <a name="hello-owning-user"></a>hello ägande användare

hello-användaren som skapade hello-objektet är automatiskt hello ägande användare hello-objektet. En ägande användare kan:

* Ändra hello behörigheter för en fil som ägs.
* Ändra hello äger grupp för en fil som ägs, så länge hello ägande användaren också är medlem i hello målgruppen.

> [!NOTE]
> hello ägande användare *kan* ändra hello ägande användare av en annan fil som företagsägda. Endast en användare kan ändra hello ägande användare av en fil eller mapp.
>
>

## <a name="hello-owning-group"></a>hello ägande grupp

I hello POSIX ACL: er, alla användare som är associerad med en ”primär grupp”. Användaren ”alice” kan till exempel hör toohello ”ekonomi” grupp. Alice kan också tillhör toomultiple grupper, men en grupp alltid är utsedd till sin primära grupp. I POSIX, när Anna skapar en fil anges hello äger gruppen av filen tooher primär grupp, som i det här fallet är ”ekonomi”.

När ett nytt filsystem-objekt skapas, tilldelar en värdet toohello äger gruppen Data Lake Store.

* **Fall 1**: hello rotmappen ”/”. Den här mappen skapas när ett Data Lake Store-konto skapas. I det här fallet in hello ägande gruppen toohello användaren som skapade hello-konto.
* **Fall 2** (alla andra fall): när ett nytt objekt skapas hello ägande grupp kopieras från hello överordnade mappen.

hello ägande grupp kan ändras av:
* Alla superanvändare.
* hello äger användaren, om hello ägande användaren också är medlem i hello målgruppen.

## <a name="access-check-algorithm"></a>Algoritm för åtkomstkontroll

Följande bild hello representerar hello åtkomst Kontrollera algoritmen för Data Lake Store-konton.

![ALC-algoritmer för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-algorithm.png)


## <a name="hello-mask-and-effective-permissions"></a>hello mask och ”gällande behörigheter”

Hej **mask** är en RWX värde som är används toolimit åtkomst för **namngivna användare**, hello **äger gruppen**, och **namngiven grupp** när du är Utför hello åtkomst Kontrollera algoritm. Här följer hello viktiga begrepp för hello mask.

* hello mask skapar ”gällande behörigheter”. Det vill säga ändrar hello behörigheten när hello åtkomstkontrollen.
* hello mask kan redigeras direkt av hello filens ägare och en användare.
* hello mask kan ta bort behörigheter toocreate hello gällande behörigheter. hello mask *kan* lägga till behörigheter toohello gällande behörigheter.

Låt oss titta på några exempel. I följande exempel hello, hello inställd för**RWX**, vilket innebär att hello mask tar inte bort alla behörigheter. hello gällande behörigheter för hello namngiven användare äger grupp och namngivna gruppen ändras inte under hello åtkomstkontrollen.

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-mask-1.png)

I följande exempel hello, hello inställd för**R-X**. Det innebär att den **inaktiverar hello skrivbehörighet** för **namngiven användare**, **äger gruppen**, och **med namnet grupp** när hello åtkomst Kontrollera.

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-mask-2.png)

För referens anger du det här är där hello mask för en fil eller mapp visas i hello Azure-portalen.

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-show-acls-mask-view.png)

> [!NOTE]
> För en ny Data Lake Store-konto, hello mask för hello ACL för åtkomst och standard ACL för hello rot (”/”) som standard tooRWX.
>
>

## <a name="permissions-on-new-files-and-folders"></a>Behörigheter för nya filer och mappar

När en ny fil eller mapp skapas under en befintlig mapp, anger hello standard ACL på hello överordnad mapp:

- En underordnad mapps standard-ACL och åtkomst-ACL.
- En underordnad fils åtkomst-ACL (filer har inte en standard-ACL).

### <a name="hello-access-acl-of-a-child-file-or-folder"></a>hello ACL för åtkomst av en underordnad fil eller mapp

När en underordnad fil eller mapp skapas, kopieras hello överordnade standard ACL som hello ACL för åtkomst av hello underordnade filen eller mappen. Även om **andra** användaren har behörighet för RWX i hello överordnade standard ACL, det tas bort från ACL för åtkomst av hello underordnade objekt.

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-child-items-1.png)

I de flesta fall är hello tidigare information du behöver tooknow om hur ACL för åtkomst av ett underordnat objekt fastställs. Men om du är bekant med POSIX-system och vill toounderstand djupgående hur transformationen uppnås avsnittet hello [Umasks roll i att skapa hello ACL för åtkomst för nya filer och mappar](#umasks-role-in-creating-the-access-acl-for-new-files-and-folders) senare i den här artikeln.


### <a name="a-child-folders-default-acl"></a>En underordnad mapps standard-ACL

När en underordnad mapp skapas under en överordnad mapp, kopieras hello överordnade mappen standard ACL över som standard ACL i toohello underordnad mapp.

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-child-items-2.png)

## <a name="advanced-topics-for-understanding-acls-in-data-lake-store"></a>Avancerad information för att förstå ACL:er i Data Lake Store

Följande är några avancerade ämnen toohelp du förstår hur ACL-listor fastställs för Data Lake Store-filer eller mappar.

### <a name="umasks-role-in-creating-hello-access-acl-for-new-files-and-folders"></a>Umasks roll i att skapa hello ACL för åtkomst för nya filer och mappar

I ett system med POSIX-kompatibla är hello allmänna begrepp som umask är ett 9-bitars värde på hello överordnad mapp som har använt tootransform hello behörighet för **ägande användare**, **äger gruppen**, och ** andra** på hello ACL för åtkomst av en ny underordnad fil eller mapp. hello bitarna i en umask identifiera vilka bits tooturn ut i ACL för åtkomst av hello underordnade objekt. Därför används tooselectively förhindra hello spridning av behörigheter för **ägande användare**, **äger gruppen**, och **andra**.

Hej umask är vanligtvis ett konfigurationsalternativ för hela platsen som styrs av administratörer i ett HDFS-system. Data Lake Store använder en **umask för konto** som inte kan ändras. följande tabell visar hello hello ta bort masken för Data Lake Store.

| Användargrupp  | Inställning | Effekt av nytt underordnade objekts åtkomst-ACL |
|------------ |---------|---------------------------------------|
| Ägande användare | ---     | Ingen effekt                             |
| Ägande grupp| ---     | Ingen effekt                             |
| Annat       | RWX     | Ta bort läsa + skriva + köra         |

hello följande bild visar den här umask i åtgärden. hello net effekten är tooremove **läsa + skriva + köra** för **andra** användare. Eftersom hello umask inte angav bits för **ägande användare** och **äger gruppen**, de behörigheterna inte omvandlas.

![ACL:er för Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-umask.png)

### <a name="hello-sticky-bit"></a>Fäst hello-bitars

Fäst hello-biten är en avancerad funktion för ett POSIX-filsystem. Hello kontexten för Data Lake Store är det osannolikt hello Fäst biten krävs.

hello visar följande tabell hur hello Fäst bitars fungerar i Data Lake Store.

| Användargrupp         | Fil    | Mapp |
|--------------------|---------|-------------------------|
| Sticky bit **AV** | Ingen effekt   | Ingen effekt.           |
| Sticky bit **PÅ**  | Ingen effekt   | Hindrar alla utom **överordnad användare** och hello **ägande användare** för ett underordnat objekt tar bort eller byta namn på det underordnade objektet.               |

Fäst hello-bitars visas inte i hello Azure-portalen.

## <a name="common-questions-about-acls-in-data-lake-store"></a>Vanliga frågor om ACL:er i Data Lake Store

Här är några frågor som ofta kommer upp rörande ACL:er i Data Lake Store.

### <a name="do-i-have-tooenable-support-for-acls"></a>Måste jag tooenable stöd för ACL: er?

Nej. Åtkomstkontroll via ACL:er är alltid aktiverat för ett Data Lake Store-konto.

### <a name="which-permissions-are-required-toorecursively-delete-a-folder-and-its-contents"></a>Vilka behörigheter som krävs toorecursively ta bort en mapp och dess innehåll?

* hello överordnad mapp måste ha **skriva + köra** behörigheter.
* Hej mappen toobe tas bort och alla mappar i denna kräver **läsa + skriva + köra** behörigheter.

> [!NOTE]
> Du behöver inte skrivbehörighet toodelete filer i mappar. Dessutom hello rotmappen ”/” kan **aldrig** tas bort.
>
>

### <a name="who-is-hello-owner-of-a-file-or-folder"></a>Vem är hello ägare till en fil eller mapp?

hello skapare av en fil eller mapp blir hello ägare.

### <a name="which-group-is-set-as-hello-owning-group-of-a-file-or-folder-at-creation"></a>Vilken grupp har angetts som hello äger en fil eller mapp på Skapa grupp?

hello ägande grupp kopieras från hello äger grupp hello överordnade mappen under vilka hello nya filen eller mappen har skapats.

### <a name="i-am-hello-owning-user-of-a-file-but-i-dont-have-hello-rwx-permissions-i-need-what-do-i-do"></a>Jag är hello ägande användare av en fil men du har inte hello RWX behörigheter som jag behöver. Vad gör jag nu?

hello ägande användaren kan ändra hello behörigheter för hello filen toogive själva RWX behörighet de behöver.

### <a name="when-i-look-at-acls-in-hello-azure-portal-i-see-user-names-but-through-apis-i-see-guids-why-is-that"></a>När jag tittar på ACL: er i hello Azure-portalen visas användarnamn men via API: er, visas GUID, varför?

Poster i hello ACL: er lagras som GUID som motsvarar toousers i Azure AD. hello API: er returnera hello GUID som är. hello Azure-portalen försöker toomake ACL: er enklare toouse genom att översätta hello GUID till egna namn när det är möjligt.

### <a name="why-do-i-sometimes-see-guids-in-hello-acls-when-im-using-hello-azure-portal"></a>Varför ibland visas GUID i hello ACL: er när jag använder hello Azure-portalen?

Ett GUID som visas när hello användaren inte finns i Azure AD längre. Detta inträffar vanligtvis när hello användaren har lämnat företaget hello eller om deras konto har tagits bort i Azure AD.

### <a name="does-data-lake-store-support-inheritance-of-acls"></a>Stöds arv av ACL:er i Data Lake Store?

Nej.

### <a name="what-is-hello-difference-between-mask-and-umask"></a>Vad är hello skillnaden mellan mask och umask?

| mask | umask|
|------|------|
| Hej **mask** egenskapen är tillgänglig på varje fil och mapp. | Hej **umask** är en egenskap hos hello Data Lake Store-konto. Det är därför bara en enda umask i hello Data Lake Store.    |
| hello ägande användare eller äger grupp för en fil eller en användare kan ändras hello egenskap i en fil eller mapp. | Hej umask egenskapen kan inte ändras av någon användare även superanvändare. Det är ett konstant värde som inte ändras.|
| hello egenskap används under hello åtkomst Kontrollera algoritmen vid körning toodetermine om en användare har rätt hello-tooperform på åtgärden på en fil eller mapp. hello roll hello mask är toocreate ”gällande behörigheter” när hello åtkomstkontrollen. | Hej umask används inte vid åtkomstkontrollen alls. Hej umask är används toodetermine hello ACL för åtkomst av den nya underordnade objekt i en mapp. |
| hello masken är ett 3-bitars RWX värde som gäller toonamed användare, namngiven grupp och ägande användare när hello åtkomstkontrollen.| Hej umask är ett 9-bitars värde som gäller toohello ägande användare, grupp, det ägande och **andra** av en ny underdomän.|

### <a name="where-can-i-learn-more-about-posix-access-control-model"></a>Var hittar jag mer information om POSIX-modellen för åtkomstkontroll?

* [POSIX-åtkomstkontrollistor på Linux](http://www.vanemery.com/Linux/ACL/POSIX_ACL_on_Linux.html)

* [Guide för HDFS-behörighet](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html)

* [Vanliga frågor och svar om POSIX](http://www.opengroup.org/austin/papers/posix_faq.html)

* [POSIX 1003.1 2008](http://standards.ieee.org/findstds/standard/1003.1-2008.html)

* [POSIX 1003.1 2013](http://pubs.opengroup.org/onlinepubs/9699919799.2013edition/)

* [POSIX 1003.1 2016](http://pubs.opengroup.org/onlinepubs/9699919799.2016edition/)

* [POSIX ACL på Ubuntu](https://help.ubuntu.com/community/FilePermissionsACLs)

* [ACL med åtkomstkontrollistor på Linux](http://bencane.com/2012/05/27/acl-using-access-control-lists-on-linux/)

## <a name="see-also"></a>Se även

* [Översikt över Azure Data Lake Store](data-lake-store-overview.md)
