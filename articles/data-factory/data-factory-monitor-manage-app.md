---
title: aaaMonitor och hantera data pipelines - Azure | Microsoft Docs
description: "Lär dig hur toouse hello övervakning och hantering av app toomonitor och hantera Azure datafabriker och rörledningar."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: f3f07bc4-6dc3-4d4d-ac22-0be62189d578
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: spelluru
ms.openlocfilehash: 5e4ef6ec5fb8ebc9bda0be7899a39a51d58403d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-manage-azure-data-factory-pipelines-by-using-hello-monitoring-and-management-app"></a>Övervaka och hantera Azure Data Factory pipelines med hello övervakning och hantering av appen
> [!div class="op_single_selector"]
> * [Med hjälp av Azure portal/Azure PowerShell](data-factory-monitor-manage-pipelines.md)
> * [Med hjälp av övervakning och Management-appen](data-factory-monitor-manage-app.md)
>
>

Den här artikeln beskriver hur toouse hello övervakning och hantering av app toomonitor, hantera och felsöka din Data Factory pipelines. Det innehåller även information om hur toocreate aviseringar tooget meddelas när fel. Du kan komma igång med hjälp av programmet hello genom att titta på hello följande video:

> [!NOTE]
> hello gränssnitt som visas i hello video kanske inte stämmer exakt vad som visas i hello-portalen. Det är något äldre men begrepp förblir hello samma. 

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-Data-Factory-Monitoring-and-Managing-Big-Data-Piplines/player]
>

## <a name="launch-hello-monitoring-and-management-app"></a>Starta hello övervakning och hantering av appen
toolaunch hello Övervakare och Management-appen klickar du på hello **övervaka och hantera** panelen på hello **Data Factory** bladet för din data factory.

![Övervakning av panelen på startsidan för hello Data Factory](./media/data-factory-monitor-manage-app/MonitoringAppTile.png)

Du bör se hello övervakning och hantering av appen öppnas i ett separat fönster.  

![Övervaknings- och hanteringsapp](./media/data-factory-monitor-manage-app/AppLaunched.png)

> [!NOTE]
> Om du ser att hello webbläsare har ”auktorisera...” Rensa hello **blockerar cookies från tredje part och platsdata** kryssrutan-- eller behålla den har markerats kan du skapa ett undantag för **login.microsoftonline.com** , och försök sedan tooopen hello appen igen.


I hello aktivitet Windows lista i hello mittenrutan ser du en aktivitetsfönstret för varje körning av en aktivitet. Om du har varje timme hello schemalagd aktivitet toorun för fem timmar finns till exempel fem aktivitetsfönster som är associerade med fem datasektorer. Om du inte ser aktivitet windows hello listan längst ned hello hello följande:
 
- Uppdatera hello **starttid** och **sluttiden** filter på hello översta toomatch hello starta sluttider för din pipeline, och klicka sedan på hello **tillämpa** knappen.  
- hello aktivitet Windows listan uppdateras inte automatiskt. Klicka på hello **uppdatera** i verktygsfältet hello i hello **aktivitet Windows** lista.  

Om du inte har en Data Factory programmet tootest dessa steg med, hello Självstudier: [kopiera data från Blob Storage tooSQL databasen med hjälp av Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

## <a name="understand-hello-monitoring-and-management-app"></a>Förstå hello övervakning och hantering av appen
Det finns tre flikar hello vänster: **Resursläsaren**, **övervakning vyer**, och **aviseringar**. hello första fliken (**Resursläsaren**) väljs som standard.

### <a name="resource-explorer"></a>Resource Explorer
Du ser hello följande:

* Hej Resursläsaren **trädvy** hello vänster.
* Hej **diagramvyn** hello överst i hello mellersta rutan.
* Hej **aktivitet Windows** lista längst ned hello i hello mellersta rutan.
* Hej **egenskaper**, **aktivitet fönstret Explorer**, och **skriptet** flikar i hello till höger.

I resursutforskaren visas alla resurser (pipelines, datauppsättningar, länkade tjänster) i hello data factory i en trädvy. När du väljer ett objekt i Resursläsaren:

* hello associerade Data Factory entiteten är markerad i hello diagramvyn.
* [Associerade aktiviteten windows](data-factory-scheduling-and-execution.md) är markerade i hello aktivitet Windows lista längst ned hello.  
* hello egenskaper för hello valda objekt visas i fönstret Egenskaper för hello i hello högra rutan.
* hello JSON-definitionen av hello valda objekt visas, om tillämpligt. Exempel: en länkad tjänst, ett dataset eller en pipeline.

![Resource Explorer](./media/data-factory-monitor-manage-app/ResourceExplorer.png)

Se hello [schemaläggning och körning](data-factory-scheduling-and-execution.md) artikel detaljerad konceptuell information om aktiviteten windows.

### <a name="diagram-view"></a>Diagramvy
hello diagramvy för en datafabrik innehåller en enda om toomonitor och hantera en datafabrik och dess tillgångar. När du väljer en Data Factory-entitet (dataset/pipeline) i hello diagramvyn:

* hello data factory-entiteten är markerad i hello trädvyn.
* hello associerade aktiviteten windows är markerade i hello aktivitet Windows lista.
* hello egenskaper för hello valda objekt visas i fönstret Egenskaper för hello.

När hello pipeline aktiveras (inte i ett pausat tillstånd), visas det med en grön linje:

![Pipelinen körs](./media/data-factory-monitor-manage-app/PipelineRunning.png)

Du kan pausa, återuppta eller avsluta en pipeline genom att markera den i hello diagramvyn och hello knapparna i hello kommandofält.

![Pausa i hello kommandofält](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)
 
Det finns tre knapparna i fältet för hello pipeline i hello diagramvyn. Du kan använda hello andra knappen toopause hello pipeline. Pausa Avsluta inte hello pågående aktiviteter och kan fortsätta toocompletion. hello tredje knappen pausar hello pipeline och avslutar sin befintliga aktiviteter körs. hello första knappen återupptar hello pipeline. När din pipeline pausas ändras hello färgen för pipeline hello. Till exempel en pausad pipeline ser ut som i följande bild hello: 

![Pipeline pausats](./media/data-factory-monitor-manage-app/PipelinePaused.png)

Du kan välja flera två eller flera pipelines med hello Ctrl-tangenten. Du kan använda hello kommandot fältet knappar toopause/Fortsätt flera pipelines i taget.

Du kan också högerklicka på en pipeline och välja alternativ för toosuspend återuppta eller avsluta en pipeline. 

![Snabbmenyn för pipeline](./media/data-factory-monitor-manage-app/right-click-menu-for-pipeline.png)

Klicka på hello **öppna pipeline** alternativet toosee alla hello aktiviteter i hello pipeline. 

![Menyn Öppna pipeline](./media/data-factory-monitor-manage-app/OpenPipelineMenu.png)

I hello öppnas pipeline vyn visas alla aktiviteter i hello pipeline. I det här exemplet är bara en aktivitet: Kopieringsaktiviteten. 

![Öppna pipeline](./media/data-factory-monitor-manage-app/OpenedPipeline.png)

toogo bakifrån toohello tidigare, klicka på hello datafabriksnamnet i hello dynamiska menyn hello överst.

I hello pipeline vyn när du väljer en utdatamängd eller när du flyttar musen över hello utdatauppsättningen visas aktiviteten Windows hello popup-fönster för denna dataset.

![Aktiviteten Windows popup-fönster](./media/data-factory-monitor-manage-app/ActivityWindowsPopup.png)

Du kan klicka på en aktivitet fönstret toosee information för den i hello **egenskaper** fönster i hello till höger.

![Fönstret Egenskaper för aktivitet](./media/data-factory-monitor-manage-app/ActivityWindowProperties.png)

I högra fönstret hello växla toohello **aktivitet fönstret Explorer** fliken toosee mer information.

![Aktiviteten fönstret Explorer](./media/data-factory-monitor-manage-app/ActivityWindowExplorer.png)

Du också se **matcha variabler** för varje försök har misslyckats för en aktivitet i hello **försök** avsnitt.

![Matcha variabler](./media/data-factory-monitor-manage-app/ResolvedVariables.PNG)

Växla toohello **skriptet** fliken toosee hello JSON-skript definitionen av hello valda objekt.   

![Fliken skript](./media/data-factory-monitor-manage-app/ScriptTab.png)

Du kan se aktiviteten windows på tre platser:

* hello aktivitet Windows popup i hello diagramvyn (mellersta rutan).
* hello aktivitet fönstret Explorer i hello till höger.
* lista med hello aktivitet Windows hello längst ned i fönstret.

Du kan rulla toohello föregående vecka i hello aktivitet Windows popup-fönster och aktivitet Windows Explorer, och hello hello nästa vecka med hjälp av vänster och höger pilarna.

![Aktiviteten fönstret Explorer åt vänster och höger pilarna](./media/data-factory-monitor-manage-app/ActivityWindowExplorerLeftRightArrows.png)

Längst ned hello hello diagramvyn, visas dessa knappar: Zooma In, Zooma ut, Zooma tooFit Zooma 100% Lås layout. Hej **Lås layout** knappen förhindrar du av misstag flyttar tabeller och rörledningar i hello diagramvyn. Den är aktiverad som standard. Du kan stänga av den och flytta entiteter i hello diagram. När du inaktiverar den kan använda du hello senaste knappen tooautomatically position tabeller och rörledningar. Du kan zooma in eller ut genom att använda hello mushjulet.

![Diagram visa zoomning kommandon](./media/data-factory-monitor-manage-app/DiagramViewZoomCommands.png)

### <a name="activity-windows-list"></a>Lista över Windows-aktivitet
hello visas aktivitet Windows längst hello hello mellersta rutan alla aktiviteten windows hello dataset som du valde i hello Resursläsaren eller hello diagramvyn. Som standard är hello listan i fallande ordning, vilket innebär att du ser hello senaste aktivitetsfönstret hello överst.

![Lista över Windows-aktivitet](./media/data-factory-monitor-manage-app/ActivityWindowsList.png)

Den här listan uppdateras inte automatiskt, så Använd hello uppdateringsknappen på hello verktygsfältet toomanually uppdatera den.  

Aktiviteten windows kan ha hello följande statusar:

<table>
<tr>
    <th align="left">Status</th><th align="left">Substatus</th><th align="left">Beskrivning</th>
</tr>
<tr>
    <td rowspan="8">Väntar</td><td>ScheduleTime</td><td>hello tiden har inte inne för hello aktivitet fönstret toorun.</td>
</tr>
<tr>
<td>DatasetDependencies</td><td>Hej uppströmsberoendena är inte redo.</td>
</tr>
<tr>
<td>ComputeResources</td><td>hello beräkningsresurser är inte tillgängliga.</td>
</tr>
<tr>
<td>ConcurrencyLimit</td> <td>Alla hello aktivitetsinstanserna är upptagna med andra windows aktivitet.</td>
</tr>
<tr>
<td>ActivityResume</td><td>hello aktiviteten har pausats och kan inte köras hello aktivitet windows förrän den har återupptagits.</td>
</tr>
<tr>
<td>Försök igen</td><td>Hej aktivitetskörningen försöks.</td>
</tr>
<tr>
<td>Validering</td><td>Verifieringen har inte startat ännu.</td>
</tr>
<tr>
<td>ValidationRetry</td><td>Verifieringen är väntar toobe igen.</td>
</tr>
<tr>
<tr>
<td rowspan="2">InProgress</td><td>Verifiera</td><td>Verifiering pågår.</td>
</tr>
<td>-</td>
<td>hello aktivitetsfönstret bearbetas.</td>
</tr>
<tr>
<td rowspan="4">Det gick inte</td><td>För lång tid</td><td>hello aktivitetskörningen tog längre tid än vad som tillåts av hello-aktivitet.</td>
</tr>
<tr>
<td>Avbrutna</td><td>hello aktivitetsfönstret avbröts av en användare.</td>
</tr>
<tr>
<td>Validering</td><td>Verifieringen misslyckades.</td>
</tr>
<tr>
<td>-</td><td>Det gick inte att toobe genereras eller verifiera hello Aktivitetsfönstret.</td>
</tr>
<td>Redo</td><td>-</td><td>hello aktivitetsfönstret är redo för användning.</td>
</tr>
<tr>
<td>Hoppades över</td><td>-</td><td>hello aktivitetsfönstret bearbetas inte.</td>
</tr>
<tr>
<td>Ingen</td><td>-</td><td>En aktivitetsfönstret för tooexist med en annan status, men har återställts.</td>
</tr>
</table>


När du klickar på en aktivitetsfönstret i hello listan kan du se information om det i hello **aktivitet Utforskaren** eller hello **egenskaper** hello högra fönstret.

![Aktiviteten fönstret Explorer](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-2.png)

### <a name="refresh-activity-windows"></a>Uppdatera aktiviteten windows
hello information uppdateras inte automatiskt, så hello uppdateringsknappen (hello andra) på hello i kommandofältet toomanually uppdatering hello aktivitet windows i listan.  

### <a name="properties-window"></a>Egenskapsfönstret
hello egenskapsfönstret är hello längst till höger i fönstret hello övervakning och hantering av appen.

![Egenskapsfönstret](./media/data-factory-monitor-manage-app/PropertiesWindow.png)

Egenskaper för hello-objektet som du valde i hello Resursläsaren (trädvyn) visas diagramvyn eller aktivitet Windows lista.

### <a name="activity-window-explorer"></a>Aktiviteten fönstret Explorer
Hej **aktivitet fönstret Explorer** fönstret är hello längst till höger i fönstret hello övervakning och hantering av appen. Information om hello aktivitetsfönstret som du valde i hello aktivitet Windows popup-fönster eller hello aktivitet Windows listan visas.

![Aktiviteten fönstret Explorer](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-3.png)

Du kan växla tooanother aktivitetsfönstret genom att klicka på hello Kalender Visa hello överst. Du kan också använda hello vänstra pilen och höger pilknappar för på hello översta toosee aktivitet windows från hello föregående vecka eller hello nästa vecka.

Du kan använda hello knappar i Aktivitetsfönstret för hello nedre fönstret toorerun hello eller uppdatera hello information hello i fönstret.

### <a name="script"></a>Skript
Du kan använda hello **skriptet** fliken tooview hello JSON-definitionen av hello valda Data Factory-enhet (länkad tjänst, datauppsättningen eller pipeline).

![Fliken skript](./media/data-factory-monitor-manage-app/ScriptTab.png)

## <a name="use-system-views"></a>Använd systemvyer
hello övervakning och hantering av appen innehåller fördefinierade systemvyer (**senaste aktivitet windows**, **misslyckades aktivitet windows**, **pågående aktivitet windows**) som låter dig tooview senaste/misslyckades/pågående aktivitet windows för din data factory.

Växla toohello **övervakning vyer** fliken hello vänster genom att klicka på den.

![Övervaka vyer fliken](./media/data-factory-monitor-manage-app/MonitoringViewsTab.png)

Det finns tre systemvyer som stöds. Välj ett alternativ toosee senaste aktivitet windows, underkända aktiviteten windows eller pågående aktivitet windows hello aktivitet Windows listan (längst ned hello hello mellersta rutan).

När du väljer hello **senaste aktivitet windows** alternativet visas alla aktivitetsfönster för senaste i fallande ordning efter hello **tid för senaste försök**.

Du kan använda hello **misslyckades aktivitet windows** visa toosee alla misslyckades aktivitet windows hello-listan. Välj en misslyckad aktivitetsfönstret i hello toosee information om det i hello **egenskaper** fönster eller hello **aktivitet fönstret Explorer**. Du kan också hämta loggar för misslyckade Aktivitetsfönstret.

## <a name="sort-and-filter-activity-windows"></a>Sortera och filtrera aktiviteten windows
Ändra hello **starttid** och **sluttiden** inställningar i hello i kommandofältet toofilter aktivitet windows. Klicka på hello knappen Nästa toohello end tid toorefresh hello aktivitet Windows lista när du har ändrat hello starttid och sluttid.

![Start- och sluttider](./media/data-factory-monitor-manage-app/StartAndEndTimes.png)

> [!NOTE]
> Alla tider för närvarande i UTC-format i hello övervakning och hantering av app.
>
>

I hello **aktivitet Windows lista**, klicka på hello namnet på en kolumn (till exempel: Status).

![Kolumnen standardmenyn Windows lista](./media/data-factory-monitor-manage-app/ActivityWindowsListColumnMenu.png)

Du kan göra hello följande:

* Sortering i stigande ordning.
* Sortering i fallande ordning.
* Filtrera efter ett eller flera värden (klar, väntande och så vidare).

Hello filterknappen aktiverad för den kolumn som visar att hello värdena i kolumnen hello filtrerade värden visas när du anger ett filter på en kolumn.

![Filtrera efter en kolumn på aktiviteten Windows hello](./media/data-factory-monitor-manage-app/ActivityWindowsListFilterInColumn.png)

Du kan använda hello samma popup-fönster tooclear filter. tooclear alla filtrerar hello aktivitet Windows lista Klicka hello Rensa filter i hello kommandofältet.

![Ta bort alla filter för hello aktivitet Windows lista](./media/data-factory-monitor-manage-app/ClearAllFiltersActivityWindowsList.png)

## <a name="perform-batch-actions"></a>Utföra åtgärder för batch
### <a name="rerun-selected-activity-windows"></a>Kör vald aktivitet windows
Ett fönster med en aktivitet, klicka på hello NEDPIL för hello första fältet kommandoknapp och markera **kör** / **kör med uppströms i pipeline**. När du väljer hello **kör med uppströms i pipeline** alternativet den kör alla uppströmsaktivitet windows samt.
    ![Kör en aktivitetsfönstret](./media/data-factory-monitor-manage-app/ReRunSlice.png)

Du kan också Välj flera aktivitet windows hello listan och köra dem igen på hello samtidigt. Du kanske vill toofilter aktivitet windows-baserade på hello status (till exempel: **misslyckades**)-- och kör sedan hello misslyckades aktivitet windows när du har korrigerat hello problemet som orsakar hello aktivitet windows toofail. Se följande information om filtrering aktivitet windows hello listan hello.  

### <a name="pauseresume-multiple-pipelines"></a>Pausa/Fortsätt flera pipelines
Du kan multiselect två eller flera pipelines med hello Ctrl-tangenten. Du kan använda hello knapparna i fältet (som är markerade i hello röd rektangel i följande bild hello) toopause/återuppta dem.

![Pausa i hello kommandofält](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)

## <a name="create-alerts"></a>Skapa aviseringar
Hej **aviseringar** sidan kan du skapa en avisering och visa/redigera/ta bort befintliga aviseringar. Du kan också inaktivera/aktivera en avisering. toosee Hej sidan varningar, klicka på hello **aviseringar** fliken.

![Aviseringsfliken](./media/data-factory-monitor-manage-app/AlertsTab.png)

### <a name="toocreate-an-alert"></a>toocreate en avisering
1. Klicka på **Lägg till avisering** tooadd en avisering. Du ser hello **information** sidan.

    ![Skapa aviseringar - sidan](./media/data-factory-monitor-manage-app/CreateAlertDetailsPage.png)
2. Ange hello **namn** och **beskrivning** hello avisering och klicka på **nästa**. Du bör se hello **filter** sidan.

    ![Skapa aviseringar - sida](./media/data-factory-monitor-manage-app/CreateAlertFiltersPage.png)
3. Välj hello **händelse**, **status**, och **substatus** (valfritt) som du vill toocreate Data Factory-tjänsten avisering för och klicka på **nästa**. Du bör se hello **mottagare** sidan.

    ![Skapa aviseringar - mottagare sida](./media/data-factory-monitor-manage-app/CreateAlertRecipientsPage.png)
4. Välj hello **e-prenumerationsadministratörer** alternativet och/eller ange en **ytterligare administratör e-post**, och klicka på **Slutför**. Du bör se hello avisering i hello-listan.

    ![Lista över aviseringar](./media/data-factory-monitor-manage-app/AlertsList.png)

I hello aviseringslistan knapparna hello som är associerade med hello avisering tooedit/delete/Aktiverar/inaktiverar en avisering.

### <a name="eventstatussubstatus"></a>Substatus-händelse/status
hello innehåller följande tabell hello lista över tillgängliga händelser och status (och underordnad status).

| händelsenamnet | Status | Substatus |
| --- | --- | --- |
| Aktiviteten kör igång |Igång |Startar |
| Aktiviteten kör klar |Lyckades |Lyckades |
| Aktiviteten kör klar |Det gick inte |Misslyckade resursallokering<br/><br/>Misslyckade körning<br/><br/>Tidsgränsen uppnåddes<br/><br/>Inte kunde verifieras<br/><br/>Avbrutna |
| På begäran HDI-klustret skapa igång |Igång |-|
| På begäran HDI-klustret har skapats |Lyckades |-|
| På begäran HDI-klustret tas bort |Lyckades |-|

### <a name="tooedit-delete-or-disable-an-alert"></a>tooedit, ta bort eller inaktivera en avisering

Använd hello följande knappar (markerat i rött) tooedit, ta bort eller inaktivera en avisering.

![Aviseringar knappar](./media/data-factory-monitor-manage-app/AlertButtons.png)
