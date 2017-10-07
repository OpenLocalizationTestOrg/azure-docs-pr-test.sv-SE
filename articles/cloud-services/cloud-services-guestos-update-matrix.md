---
title: "aaaLearn om hello senaste Azure versioner för gästoperativsystem | Microsoft Docs"
description: "hello versionen nyheter och SDK-kompatibilitet för Azure Cloud Services Gästoperativsystem."
services: cloud-services
documentationcenter: na
author: raiye
manager: timlt
editor: 
ms.assetid: 6306cafe-1153-44c7-8554-623b03d59a34
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 8/24/2017
ms.author: raiye
ms.openlocfilehash: 7274f5a68a32ce91bdede77e1443cdb8053c07ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-guest-os-releases-and-sdk-compatibility-matrix"></a>Azure gäst-OS-versioner och SDK-kompatibilitetsmatris
Ger dig uppdaterad information om hello senaste Azure Gästoperativsystem släpper för molntjänster. Den här informationen hjälper dig att planera din uppgradering innan ett gäst-OS är inaktiverad. Om du konfigurerar roller-toouse *automatisk* Gästoperativsystem uppdateras enligt beskrivningen i [Azure gäst-OS uppdateringsinställningar][Azure Guest OS Update Settings], inte är det viktigt att du läser den här sidan.

> [!IMPORTANT]
> Den här sidan gäller tooCloud Services webb- och arbetsroller roller som körs ovanpå ett Gästoperativsystem. Det gör **inte** tooIaaS virtuella datorer.
>
>


> [!NOTE]
> hello RSS-Feed nyligen togs bort. Titta efter uppdateringar på en ny feed kommer snart!
>
>

Osäker på vilken hello Gästoperativsystem är eller hur hello Gästoperativsystem släpper arbete? Läs [detta](#how-it-works) avsnitt.

## <a name="news-updates"></a>Nyheter

###### <a name="august-24-2017"></a>**24 augusti 2017**
Augusti Gästoperativsystem har publicerat.

###### <a name="august-3-2017"></a>**3 augusti 2017**
Juli Gästoperativsystem har publicerat.

###### <a name="july-19-2017"></a>**19 juli 2017**
Juli gäst-OS-distributionen startar juli 19 och har en planerade version av 8 augusti.

###### <a name="july-7-2017"></a>**7 juli 2017**
Juni Gästoperativsystem har publicerat.

###### <a name="june-16-2017"></a>**16 juni 2017**
Juni gäst-OS-distributionen startar 16 juni och har en planerade version av 11 juli.

###### <a name="june-5-2017"></a>**5 juni 2017**
Gästoperativsystem kan har publicerat.

###### <a name="may-17-2017"></a>**Den 17 maj 2017**
På grund av tooa säkerhet programfel kan vi inaktiverar hello efter December 2016 och januari 2017 OS-versioner som inte har hello [åtgärda] från hello-portalen: WA-GÄST-OS-5.4_201612-01, WA-GÄST-OS-4.39_201612-01, WA-GUEST-OS-3.46_ 201612-01, WA-GUEST-OS-2.59_201701-01

###### <a name="may-12-2017"></a>**12 maj 2017**
Kan gäst-OS-distributionen startar 12 maj och har en planerade version av 13 juni.

###### <a name="april-18-2017"></a>**18 april 2017**
April gäst-OS-distributionen startar 18 April och har en planerade version av kan 9.

###### <a name="april-10-2017"></a>**10 april 2017**
Mars gäst-OS-installationen startade 14 mars 2017 och släpptes 10 April 2017.

###### <a name="january-10-2017"></a>**10 januari 2017**
hello januari Gästoperativsystem innehåller korrigeringar som påverkar endast OS-familjen 2 (Windows 2008 Server R2). Vi har därför ut endast hello OS-familjen 2 bilden (WA-GUEST-OS-2.59_201701-01) för den här månaden. För alla OS-familjer hello December OS (201612 - 01) förblir hello senaste.


## <a name="releases"></a>Versioner
## <a name="family-5-releases"></a>Familj 5 versioner
**Windows Server 2016**

.NET framework har installerats: 4.0, 4.5, 4.5.1, 4.5.2, 4.6, 4.6.1, 4.6.2

> [!NOTE]
> Datum med ett * är ämne toochange.
>
> hello RDP-lösenordet för OS-familjen 5 måste vara minst 10 tecken.
>

| Konfigurationssträngen | Utgivningsdatum | Inaktivera datum | Utgångna datum |
| --- | --- | --- | --- |
| WA-GUEST-OS-5.10_201708-01 |24 augusti 2017 |Post 5.12 |TBD |
| WA-GUEST-OS-5.9_201707-01 |3 augusti 2017 |Post 5.11 |TBD |
| WA-GUEST-OS-5.8_201706-01 |7 juli 2017 |Post 5.10 |TBD |
|~~WA-GUEST-OS-5.7_201705-01~~ |5 juni 2017 |24 augusti 2017 |TBD |
|~~WA-GUEST-OS-5.6_201704-01~~ |9 kan 2017 |3 augusti 2017 |TBD |
|~~WA-GUEST-OS-5.5_201703-01~~ |10 april 2017 |7 juli 2017 |TBD |
|~~WA-GUEST-OS-5.4_201612-01~~ |10 januari 2017 |5 juni 2017|TBD |
|~~WA-GUEST-OS-5.3_201611-01~~ |14 december 2016 |9 kan 2017 |TBD |
|~~WA-GUEST-OS-5.2_201610-02~~ |Den 1 november 2016 |10 april 2017 |TBD |

## <a name="family-4-releases"></a>Familj 4 versioner
**Windows Server 2012 R2**

Har stöd för .NET 4.0, 4.5, 4.5.1, 4.5.2

> [!NOTE]
> Datum med ett * är ämne toochange
>
>

| Konfigurationssträngen | Utgivningsdatum | Inaktivera datum | Utgångna datum |
| --- | --- | --- | --- |
| WA-GUEST-OS-4.45_201708-01 |24 augusti 2017 |Bokför 4.47 |TBD |
| WA-GUEST-OS-4.44_201707-01 |3 augusti 2017 |Bokför 4.46 |TBD |
| WA-GUEST-OS-4.43_201706-01 |7 juli 2017 |Bokför 4,45 |TBD |
|~~WA-GUEST-OS-4.42_201705-01~~ |5 juni 2017 |24 augusti 2017 |TBD |
|~~WA-GUEST-OS-4.41_201704-01~~ |9 kan 2017 |3 augusti 2017 |TBD |
|~~WA-GUEST-OS-4.40_201703-01~~ |10 april 2017 |7 juli 2017 |TBD |
|~~WA-GUEST-OS-4.39_201612-01~~ |10 januari 2017 |5 juni 2017 |TBD |
|~~WA-GUEST-OS-4.38_201611-01~~ |14 december 2016 |9 kan 2017 |TBD |
|~~WA-GUEST-OS-4.37_201610-02~~ |16 november 2016 |10 april 2017 |TBD |
|~~WA-GUEST-OS-4.36_201609-01~~ |13 oktober 2016 |14 januari 2017 |TBD |
|~~WA-GUEST-OS-4.35_201608-01~~ |13 september 2016 |16 december 2016 |TBD |
|~~WA-GUEST-OS-4.34_201607-01~~ |8 augusti 2016 |13 november 2016 |TBD |


## <a name="family-3-releases"></a>Familj 3-versioner
**Windows Server 2012**

Har stöd för .NET 4.0, 4.5, 4.5.1, 4.5.2

> [!NOTE]
> Datum med ett * är ämne toochange
>
>

| Konfigurationssträngen | Utgivningsdatum | Inaktivera datum | Utgångna datum |
| --- | --- | --- | --- |
| WA-GUEST-OS-3.52_201708-01 |24 augusti 2017 |Bokför 3.54 |TBD |
| WA-GUEST-OS-3.51_201707-01 |3 augusti 2017 |Bokför 3.53 |TBD |
| WA-GUEST-OS-3.50_201706-01 |7 juli 2017 |Post 3.52 |TBD |
|~~WA-GUEST-OS-3.49_201705-01~~ |5 juni 2017 |24 augusti 2017 |TBD |
|~~WA-GUEST-OS-3.48_201704-01~~ |9 kan 2017 |3 augusti 2017 |TBD |
|~~WA-GUEST-OS-3.47_201703-01~~ |10 april 2017 |7 juli 2017 |TBD |
|~~WA-GUEST-OS-3.46_201612-01~~ |10 januari 2017 |5 juni 2017 |TBD |
|~~WA-GUEST-OS-3.45_201611-01~~ |14 december 2016 |9 kan 2017 |TBD |
|~~WA-GUEST-OS-3.44_201610-02~~ |16 november 2016 |1 maj 2017 |TBD |
|~~WA-GUEST-OS-3.43_201609-01~~ |13 oktober 2016 |14 januari 2017 |TBD |
|~~WA-GUEST-OS-3.42_201608-01~~ |13 september 2016 |16 december 2016 |TBD |
|~~WA-GUEST-OS-3.41_201607-01~~ |8 augusti 2016 |13 november 2016 |TBD |


## <a name="family-2-releases"></a>Familj 2 versioner
**Windows Server 2008 R2 SP1**

Har stöd för .NET 3.5, 4.0, 4.5, 4.5.1, 4.5.2

> [!NOTE]
> Datum med ett * är ämne toochange
>
>

| Konfigurationssträngen | Utgivningsdatum | Inaktivera datum | Utgångna datum |
| --- | --- | --- | --- |
| WA-GUEST-OS-2.65_201708-01 |24 augusti 2017 |Bokför 2.67 |TBD |
| WA-GUEST-OS-2.64_201707-01 |3 augusti 2017 |Post 2,66 |TBD |
| WA-GUEST-OS-2.63_201706-01 |7 juli 2017 |Post 2.65 |TBD |
|~~WA-GUEST-OS-2.62_201705-01~~ |5 juni 2017 |24 augusti 2017 |TBD |
|~~WA-GUEST-OS-2.61_201704-01~~ |9 kan 2017 |3 augusti 2017 |TBD |
|~~WA-GUEST-OS-2.60_201703-01~~ |10 april 2017 |7 juli 2017 |TBD |
|~~WA-GUEST-OS-2.59_201701-01~~ |10 januari 2017 |5 juni 2017 |TBD |
|~~WA-GUEST-OS-2.58_201612-01~~ |10 januari 2017 |9 kan 2017|TBD |
|~~WA-GUEST-OS-2.57_201611-01~~ |14 december 2016 |10 april 2017 |TBD |
|~~WA-GUEST-OS-2.56_201610-02~~ |16 november 2016 |10 februari 2017 |TBD |
|~~WA-GUEST-OS-2.55_201609-01~~ |13 oktober 2016 |14 januari 2017 |TBD |
|~~WA-GUEST-OS-2.54_201608-01~~ |13 september 2016 |16 december 2016 |TBD |
|~~WA-GUEST-OS-2.53_201607-01~~ |8 augusti 2016 |13 november 2016 |TBD |



## <a name="msrc-patch-updates"></a>MSRC patch-uppdateringar
hello korrigeringsprogram som ingår i varje månad gäst-OS-version är tillgänglig [här][patches].

## <a name="sdk-support"></a>Stöd för SDK
Även om hello [pensionering princip för hello Azure SDK] [ retire policy sdk] anger att endast versioner ovan 2.2 är stöds, specifik gäst-OS-familjer som tillåter du toouse tidigare versioner. Du bör alltid använda hello senaste stöds SDK.

| OS-Gästfamiljen | Kompatibel SDK-versioner |
| --- | --- |
| 5 |Version 2.9.5.1+ |
| 4 |Version 2.1 + |
| 3 |Version 1.8 + |
| 2 |Version 1.3 + |
| 1 |Version 1.0 + |

## <a name="guest-os-release-information"></a>Information för gäst-OS-versionen
Det finns tre datum som är viktiga tooGuest OS-versioner: **släpper** datum, **inaktiverad** datum och **giltighetstid** datum. Ett gäst-OS anses vara tillgänglig när den är i hello Portal och kan väljas som hello mål Gästoperativsystem. När ett gäst-OS når hello **inaktiverad** datum, den tas bort från Azure. Alla Molntjänsten som mål som Gästoperativsystem fortfarande fungerar dock som vanligt.

hello fönstret mellan hello **inaktiverad** datum och hello **giltighetstid** datum ger dig en buffert tooeasily övergång från ett Gästoperativsystem tooone senare. Om du använder *automatisk* som din gäst-OS du alltid att senaste versionen av hello och du inte har tooworry om det upphör att gälla.

När hello **giltighetstid** datum överför alla Molntjänsten som fortfarande använder den Gästoperativsystem stoppas, borttagna eller framtvingad tooupgrade. Du kan läsa mer om hello pensionering principen [här][retirepolicy].

## <a name="guest-os-family-version-explanation"></a>Förklaring för gäst-OS-familjen-Version
hello gäst-OS-familjer baseras på versioner av Microsoft Windows Server. hello Gästoperativsystem är hello underliggande operativsystemet som Azure Cloud Services körs på. Varje Gästoperativsystem har en familj, version och utgåva nummer.

* **Gäst-OS-familjen**  
  En Windows Server operating system-versionen som en gäst-OS baseras på. Till exempel *familj 3* baseras på Windows Server 2012.
* **Gäst-OS-version**  
  Specifika tooa gäst-OS-familjen bild plus relevanta [Microsoft Security Response Center (MSRC)] [ msrc] korrigeringsprogram som är tillgängliga när hello-datum hello nya gäst-OS-version skapas. Inte alla korrigeringsfiler kan ingå.

    Siffror som börjar på 0 och öka med 1 varje gång en ny uppsättning uppdateringar har lagts till. Efterföljande nollor visas bara om det är viktigt. Version 2.10 är en annan, mycket senare version än version 2.1.
* **Gäst-OS-versionen**  
  En ny utgåva av en gäst-OS-version. En ny utgåva uppstår om Microsoft hittar problem under testningen; kräver ändringar. hello senaste versionen alltid ersätter alla tidigare versioner, offentlig eller inte. hello Azure-portalen kan användarna toopick hello senaste versionen för en viss version. Distributioner som körs på en tidigare version är vanligtvis inte kraft uppgraderas beroende på hello allvarlighetsgraden hello programfel.

I hello exemplet nedan 2 är hello familj, 12 är hello version och ”rel2” är hello-version.

**Gäst-OS-versionen** - 2,12 rel2

**Konfigurationssträngen för den här versionen** -WA-GUEST-OS-2.12_201208-02

hello konfigurationssträngen för ett gäst-OS har samma information inbäddat i den, tillsammans med ett datum som visar vilka MSRC säkerhetskorrigeringar ansågs för uppdateringen. I det här exemplet ansågs MSRC korrigeringsfiler produceras för Windows Server 2008 R2 in tooand inklusive augusti 2012 för införande. Endast korrigeringsprogram som är specifikt tillämpa toothat version av Windows Server ingår. Till exempel om en MSRC korrigeringsfil gäller tooMicrosoft Office, tas den inte med eftersom produkten inte är en del av hello basavbildningen för Windows Server.

## <a name="guest-os-system-update-process"></a>Uppdateringsprocessen för gäst-OS-System
Den här sidan innehåller information om kommande versioner för gästoperativsystem. Kunder har angett som de vill tooknow när en Versionspost beror på att deras molntjänstroller startas om de har angetts för ”automatisk” uppdatering. Gäst-OS-versioner sker vanligtvis minst fem (5) dagar efter hello MSRC uppdatering som uppstår på hello andra tisdagen varje månad. Nya versioner innehåller alla hello relevanta MSRC korrigeringar för varje gäst-OS-familjen.

Microsoft Azure lanserar ständigt uppdateringar. hello Gästoperativsystem är endast en sådan uppdatering i hello pipeline. En version som kan påverkas av flera faktorer för många toolist här. Dessutom körs Azure på hundratals med tusentals datorer. Detta innebär att det är omöjligt toogive ett exakt datum och tid när dina roller kommer att startas om. Vi arbetar på en plan toolimit eller tid omstarter.

Det kan ta tid toofully huvudkonfigurationsfil via Azure när en ny version av hello Gästoperativsystem publiceras. Eftersom tjänster uppdaterade toohello nya gäst-OS, de är omstartad respektera update domäner. Tjänster toouse ”automatisk” uppdateringar får en Versionspost första. Efter hello uppdateringen visas hello nya gäst-OS-version som anges för tjänsten i hello Azure-portalen. Här återutgivningarna kan uppstå under denna tid. Vissa versioner kan distribueras under längre tid och automatisk uppgradering omstarter inträffar för många veckor efter hello officiella versionen. När ett gäst-OS är tillgänglig, sedan du uttryckligen versionen från portalen hello eller i konfigurationsfilen.

En massa värdefull information på startas om och pekare toomore information tekniska detaljerna för gäst- och värd-OS-uppdateringar, finns hello MSDN blogg post med titeln [roll-instansen startas om på grund av tooOS uppgraderingar] [ restarts].

Om du manuellt uppdatera gäst-OS, se hello [Gästoperativsystem pensionering princip] [ retirepolicy] för ytterligare information.

## <a name="guest-os-supportability-and-retirement-policy"></a>Support för gäst-OS och tillbakadragning princip
hello Gästoperativsystem support och tillbakadragning princip förklaras [här][retirepolicy].

[Install .NET on a Cloud Service Role]: https://azure.microsoft.com/en-us/documentation/articles/cloud-services-dotnet-install-dotnet/?WT.mc_id=azurebg_email_Trans_963_RevisedNET_Update
[Azure Guest OS Update Settings]: cloud-services-how-to-configure.md
[ssl3 announcement]: http://azure.microsoft.com/blog/2014/12/09/azure-security-ssl-3-0-update/
[Microsoft Security Advisory 3009008]: https://technet.microsoft.com/library/security/3009008.aspx
[ssl3-fixit]: http://go.microsoft.com/?linkid=9863266
[MS14-066]: https://technet.microsoft.com/library/security/ms14-066.aspx
[MS14-046]: https://technet.microsoft.com/library/security/ms14-046.aspx
[retire policy sdk]: https://msdn.microsoft.com/library/dn479282.aspx
[server and gos]: https://msdn.microsoft.com/library/dn775043.aspx
[azuresupport]: http://azure.microsoft.com/support/options/
[net install pkg]: http://www.microsoft.com/download/details.aspx?id=42643
[msrc]: http://www.microsoft.com/security/msrc/default.aspx
[update guest os portal]: https://msdn.microsoft.com/library/gg433101.aspx
[update guest os svc]: https://msdn.microsoft.com/library/gg456324.aspx
[restarts]: http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx
[patches]: cloud-services-guestos-msrc-releases.md
[retirepolicy]: cloud-services-guestos-retirement-policy.md
[fam1retire]: cloud-services-guestos-family1-retirement.md
[åtgärda]: https://technet.microsoft.com/en-us/library/security/ms17-010.aspx
