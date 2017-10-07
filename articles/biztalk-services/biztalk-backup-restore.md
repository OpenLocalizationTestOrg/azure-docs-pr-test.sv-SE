---
title: "aaaCreate och återställa en säkerhetskopia i BizTalk-tjänst | Microsoft Docs"
description: "BizTalk-tjänster omfattar säkerhetskopiering och återställning. Lär dig hur toocreate och återställa en säkerhetskopia och bestämma vad säkerhetskopieras. MABS WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 59f91173-4683-48df-abd5-41262bfce6df
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 32356ad870678fa5fd5bbbbf13d9377188f770a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="biztalk-services-backup-and-restore"></a>BizTalk Services: Säkerhetskopiering och återställning

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Azure BizTalk-tjänster inkluderar funktioner för säkerhetskopiering och återställning. Det här avsnittet beskrivs hur toobackup och återställning BizTalk-tjänster med hjälp av hello klassiska Azure-portalen.

Du kan också säkerhetskopiera BizTalk-tjänster med hjälp av hello [BizTalk Services REST API](http://go.microsoft.com/fwlink/p/?LinkID=325584). 

> [!NOTE]
> Hybridanslutningar säkerhetskopieras inte, oavsett hello Edition. Du måste återskapa din hybridanslutningar.


## <a name="before-you-begin"></a>Innan du börjar
* Säkerhetskopiering och återställning kanske inte tillgänglig för alla versioner. Se [BizTalk-tjänst: utgåvor diagram](biztalk-editions-feature-chart.md).
* Med hello klassiska Azure-portalen kan du skapa en säkerhetskopiering på begäran eller skapa en schemalagd säkerhetskopiering. 
* Säkerhetskopiera innehåll kan vara återställda toohello samma BizTalk Service eller tooa ny BizTalk Service. toorestore hello BizTalk Service med samma namn, hello befintlig BizTalk Service måste du ta bort hello och hello namn måste vara tillgänglig. När du tar bort en BizTalk Service kan det ta längre tid än ville för hello samma namn toobe tillgängliga. Om du inte kan vänta hello samma namn toobe tillgängliga, och sedan återställer tooa ny BizTalk Service.
* BizTalk-tjänst kan vara återställda toohello samma edition eller högre. Återställa BizTalk-tjänst tooa stöds lägre utgåva från när hello säkerhetskopian skapades, inte.
  
    Till exempel återställs med hello Basic-versionen kan vara en säkerhetskopia toohello Premium Edition. En säkerhetskopiering med hello Premium Edition måste återställas toohello Standard Edition.
* hello EDI-kontrollen siffror säkerhetskopieras toomaintain kontinuitet i hello kontrollen siffror. Meddelanden som bearbetas efter hello senaste säkerhetskopieringen, kan återställa säkerhetskopiering innehållet orsaka dubbla kontrollen siffror.
* Om en batch har aktiva meddelanden, bearbeta hello batch **innan** köra en säkerhetskopiering. När du skapar en säkerhetskopia (som krävs eller är schemalagda) lagras aldrig meddelanden i batchar. 
  
    **Om en säkerhetskopia görs med aktiva meddelanden i en grupp, säkerhetskopieras inte dessa meddelanden och därför går förlorade.**
* Valfritt: I hello BizTalk-Services-portalen, stoppa några hanteringsåtgärder.

## <a name="create-a-backup"></a>Skapa en säkerhetskopia
En säkerhetskopia kan vidtas när som helst och är helt styrs av du. Det här avsnittet innehåller hello steg toocreate säkerhetskopior med hello Azure klassiska portal, inklusive:

[På begäran-säkerhetskopia](#backupnow)

[Schemalägga en säkerhetskopia](#backupschedule)

#### <a name="backupnow"></a>På begäran-säkerhetskopia
1. Välj i hello klassiska Azure-portalen, **BizTalk-tjänst**, och sedan väljer hello BizTalk Service du vill toobackup.
2. I hello **instrumentpanelen** väljer **säkerhetskopiera** på hello hello sidans nederkant.
3. Ange ett namn på säkerhetskopia. Ange till exempel *myBizTalkService*för*datum*.
4. Välj ett blob storage-konto och välj hello markering toostart hello säkerhetskopiering.

När hello säkerhetskopieringen är klar skapas en behållare med hello namn du anger i hello storage-konto. Den här behållaren innehåller BizTalk Service konfigurationen för säkerhetskopieringen.

#### <a name="backupschedule"></a>Schemalägga en säkerhetskopia
1. Välj i hello klassiska Azure-portalen, **BizTalk-tjänst**, Välj hello BizTalk Service namn du vill tooschedule hello säkerhetskopiering och välj sedan hello **konfigurera** fliken.
2. Ange hello **Status för säkerhetskopiering** för**automatisk**. 
3. Välj hello **Lagringskonto** toostore Hej säkerhetskopiering, ange hello **frekvens** toocreate hello, och hur länge tookeep hello säkerhetskopior (**kvarhållning dagar**):
   
    ![][AutomaticBU]
   
    **Anteckningar**     
   
   * I **kvarhållning dagar**, hello kvarhållningsperioden måste vara större än säkerhetskopieringsfrekvensen för hello.
   * Välj **alltid ha minst en säkerhetskopia**, även om den tidigare hello Bevarandeperiod.
4. Välj **Spara**.

När en schemalagd säkerhetskopiering körs skapas en behållare (toostore säkerhetskopieringsdata) i hello storage-konto som du har angett. hello namnet på behållaren hello heter *BizTalk Service Name-tid*. 

Om hello BizTalk Service instrumentpanelen visar en **misslyckades** status:

![Senaste status för schemalagd säkerhetskopiering][BackupStatus] 

hello länken öppnar hello Management åtgärden tjänstloggar toohelp felsöka. Se [BizTalk-tjänst: felsökning med åtgärdsloggar](http://go.microsoft.com/fwlink/p/?LinkId=391211).

## <a name="restore"></a>Återställ
Du kan återställa säkerhetskopior från hello klassiska Azure-portalen eller hello [återställa BizTalk Service REST API](http://go.microsoft.com/fwlink/p/?LinkID=325582). Det här avsnittet innehåller hello steg toorestore med hello klassiska portalen.

#### <a name="before-restoring-a-backup"></a>Innan du återställer en säkerhetskopia
* Ny spårning, arkivering och övervakning lagrar kan anges när en BizTalk Service.
* hello samma EDI Körningsdata har återställts. hello EDI Runtime säkerhetskopiering lagrar hello kontrollen tal. hello återställts kontrollen siffror finns i sekvens från hello tid för hello säkerhetskopiering. Meddelanden som bearbetas efter hello senaste säkerhetskopieringen, kan återställa säkerhetskopiering innehållet orsaka dubbla kontrollen siffror.

#### <a name="restore-a-backup"></a>Återställ en säkerhetskopia
1. Välj i hello klassiska Azure-portalen, **ny** > **Apptjänster** > **BizTalk Service** > **återställa** :
   
    ![Återställ en säkerhetskopia][Restore]
2. I **säkerhetskopiering URL**hello mappikon och välj utöka hello Azure storage-konto som lagrar hello BizTalk Service konfigurationssäkerhetskopia. Utöka hello behållare och välj hello motsvarande säkerhetskopiera txt-fil i hello högra rutan. 
   <br/><br/>
   Välj **öppna**.
3. På hello **återställa BizTalk-tjänst** anger en **BizTalk tjänstnamnet** och kontrollera hello **domän-URL**, **Edition**, och **Region** för hello återställts BizTalk Service. **Skapa en ny SQL-databasinstans** för hello spårning av databasen:
   
    ![][RestoreBizTalkService]
   
    Välj hello nästa-pilen.
4. Verifierar hello namn hello SQL-databas, ange hello fysisk server där hello SQL-databasen skapas och ett användarnamn/lösenord för servern.

    Om du vill tooconfigure hello SQL database edition, storlek och andra egenskaper, Välj **konfigurera avancerade databasinställningar**. 

    Välj hello nästa-pilen.

1. Skapa ett nytt lagringskonto eller ange ett befintligt lagringskonto för hello BizTalk Service.
2. Välj hello markering toostart hello återställning.

När hello återställningen har slutförts visas en ny BizTalk Service i ett pausat tillstånd på hello BizTalk-tjänst sida i hello klassiska Azure-portalen.

### <a name="postrestore"></a>När du återställer en säkerhetskopia
hello BizTalk Service alltid återställs i ett **pausad** tillstånd. I det här tillståndet kan göra ändringar i konfigurationen innan nya hello-miljön är fungerar, inklusive:

* Om du har skapat BizTalk Service program med hjälp av hello Azure BizTalk Services SDK behöva tootooupdate hello Access Control (ACS) autentiseringsuppgifter i dessa program toowork med hello återställts-miljön.
* Du kan återställa en BizTalk Service tooreplicate en befintlig BizTalk Service-miljö. I det här fallet om det finns avtal som konfigurerats i hello ursprungliga BizTalk-Services-portalen och som använder källmappen FTP, kanske du måste tooupdate hello avtal i hello nyligen återställts miljö toouse en annan FTP-källmapp. Annars kan det finnas två olika avtal försök toopull hello samma meddelande.
* Om du har återställt toohave miljöer med flera BizTalk Service, kontrollera att du anpassar hello rätt miljö i hello Visual Studio-program, PowerShell-cmdlets, REST API: er eller handel Partner hantering OM API: er.
* Det är en bra idé tooconfigure automatisk säkerhetskopieringar på hello nyligen återställts BizTalk-miljö.

toostart hello BizTalk Service i hello klassiska Azure-portalen väljer hello återställts BizTalk Service och välj **återuppta** i hello Aktivitetsfältet. 

## <a name="what-gets-backed-up"></a>Vad säkerhetskopieras
När en säkerhetskopia skapas säkerhetskopieras hello följande:

<table border="1"> 
<tr bgcolor="FAF9F9">
<th> </th>
<TH>Objekt som har säkerhetskopierats</TH> 
</tr> 
<tr>
<td colspan="2">
 <strong>Azure BizTalk-Services-portalen</strong></td>
</tr> 
<tr>
<td>Konfiguration och körning</td> 
<td>
<ul>
<li>Information för partner och profil</li>
<li>Partner avtal</li>
<li>Anpassade sammansättningar som har distribuerats</li>
<li>Bryggor distribueras</li>
<li>Certifikat</li>
<li>Transformeringar distribueras</li>
<li>Pipelines</li>
<li>Mallar som har skapats och sparats i hello BizTalk-Services-portalen</li>
<li>X12 ST01 och GS01 mappningar</li>
<li>Kontrollen siffror (EDI)</li>
<li>AS2 meddelandet MIC värden</li>
</ul>
</td>
</tr> 

<tr>
<td colspan="2">
 <strong>Azure BizTalk-tjänst</strong></td>
</tr> 
<tr>
<td>SSL-certifikat</td> 
<td>
<ul>
<li>Data för SSL-certifikat</li>
<li>Lösenordet för SSL-certifikat</li>
</ul>
</td>
</tr> 
<tr>
<td>BizTalk-tjänst-inställningar</td> 
<td>
<ul>
<li>Skalningsantalet för enhet</li>
<li>Utgåva</li>
<li>Produktversion</li>
<li>Region/Datacenter</li>
<li>Access Control Service (ACS) namnområde och nyckel</li>
<li>Spåra databasanslutningssträng</li>
<li>Arkivering lagringsanslutningssträng för kontot</li>
<li>Övervaka lagringsanslutningssträng för kontot</li>
</ul>
</td>
</tr> 
<tr>
<td colspan="2">
 <strong>Ytterligare objekt</strong></td>
</tr> 
<tr>
<td>Spårning av databasen</td> 
<td>När hello BizTalk Service skapas registreras hello spårning databasinformation, inklusive hello Azure SQL-databasservern och hello spårning databasnamnet. hello spårningsdatabasen automatiskt säkerhetskopieras inte.
<br/><br/>
<strong>Viktigt</strong><br/>
Om hello spårningsdatabasen tas bort och hello databasen måste återställas, måste det finnas en tidigare säkerhetskopia. Om en säkerhetskopiering inte finns, är inte hello spårningsdatabasen och dess data återställas. I det här fallet skapar du en ny databas för spårning med hello samma databasnamn. GEO-replikering rekommenderas.</td>
</tr> 
</table>

## <a name="next"></a>Nästa
toocreate Azure BizTalk-tjänst i hello Azure klassiska portal, gå för[BizTalk-tjänst: etablering med hjälp av Azure klassiska portal](http://go.microsoft.com/fwlink/p/?LinkID=302280). för att skapa program, gå toostart[Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=235197).

## <a name="see-also"></a>Se även
* [Säkerhetskopiera BizTalk-tjänst](http://go.microsoft.com/fwlink/p/?LinkID=325584)
* [Återställa BizTalk-tjänst från en säkerhetskopia](http://go.microsoft.com/fwlink/p/?LinkID=325582)
* [BizTalk-tjänst: Utvecklare, Basic, Standard och Premium-utgåvor diagram](http://go.microsoft.com/fwlink/p/?LinkID=302279)
* [BizTalk-tjänst: Etablering med hjälp av Azure klassiska portal](http://go.microsoft.com/fwlink/p/?LinkID=302280)
* [BizTalk Services: Etablering av statusdiagram](http://go.microsoft.com/fwlink/p/?LinkID=329870)
* [BizTalk Services: Flikarna Instrumentpanel, Övervakare och Skalning](http://go.microsoft.com/fwlink/p/?LinkID=302281)
* [BizTalk Services: Begränsning](http://go.microsoft.com/fwlink/p/?LinkID=302282)
* [BizTalk Services: Utfärdarens namn och nyckel](http://go.microsoft.com/fwlink/p/?LinkID=303941)
* [Hur jag börja använda hello Azure BizTalk Services SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[BackupStatus]: ./media/biztalk-backup-restore/status-last-backup.png
[Restore]: ./media/biztalk-backup-restore/restore-ui.png
[AutomaticBU]: ./media/biztalk-backup-restore/AutomaticBU.png
[RestoreBizTalkService]: ./media/biztalk-backup-restore/RestoreBizTalkServiceWindow.png

