---
title: aaaMicrosoft Azure StorSimple Data Manager UI | Microsoft Docs
description: "Beskriver hur toouse StorSimple Data Manager-tjänsten Användargränssnittet (privat förhandsvisning)"
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/22/2016
ms.author: vidarmsft
ms.openlocfilehash: b0ee12b3e495400b54e48eb1a98c68b1af2e5f7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-using-hello-storsimple-data-manager-service-ui-private-preview"></a>Hantera med hjälp av hello StorSimple Data Manager-tjänsten Användargränssnittet (privat förhandsvisning)

Den här artikeln förklarar hur du kan använda hello omvandling av StorSimple Data Manager UI tooperform data på data som finns på hello StorSimple 8000-serien enheter. hello kan transformerade data sedan användas av andra Azure-tjänster, till exempel Azure Media Services, Azure HDInsight, Azure Machine Learning och Azure Search. 


## <a name="use-storsimple-data-transformation"></a>Använda StorSimple Data Transformation

Hej StorSimple Data Manager är hello resurs inom vilken Data Transformation kan initieras. hello Data Transformation service kan du flytta data från din StorSimple lokala enhet tooblobs i Azure-lagring. Därför i arbetsflödet måste toospecify hello information om dina StorSimple-enheten och hello data av intresse som du vill toomove toohello storage-konto.

### <a name="create-a-storsimple-data-manager-service"></a>Skapa en StorSimple Data Manager-tjänst

Utföra hello följande steg toocreate en StorSimple Data Manager-tjänsten.

1. Gå toocreate StorSimple Data Manager-tjänsten för[https://aka.ms/HybridDataManager](https://aka.ms/HybridDataManager)

2. Klicka på hello  **+**  ikon och sökning för StorSimple Data Manager. Klicka på din StorSimple Data Manager-tjänst och klicka sedan på **skapa**.

3. Om din prenumeration är aktiverad för att skapa den här tjänsten finns hello följande bladet.

    ![Skapa en resurs för StorSimple Data chefer](./media/storsimple-data-manager-ui/create-new-data-manager-service.png)

4. Ange hello indata och klicka på **skapa**. hello anges måste vara hello en som innehåller dina lagringskonton och StorSimple Manager-tjänsten. För närvarande stöds endast västra USA och Västeuropa regioner. Därför måste din StorSimple Manager-tjänsten, Data Manager-tjänsten och hello tillhörande storage-konto ska i hello föregående stöds regioner. Det tar en minut toocreate hello-tjänst.

### <a name="create-a-data-transformation-job-definition"></a>Skapa en data transformation jobbdefinition

Du måste toocreate data transformation jobbdefinitionen inom en StorSimple Data Manager-tjänsten. En jobbdefinition anger information om hello data som du vill flytta till ett lagringskonto i hello native-format. 

Utföra hello följande steg toocreate jobbdefinitionen för en ny omvandling.

1.  Navigera toohello tjänsten som du skapade. Klicka på **+ jobbet Definition**.

    ![Klicka på + jobbdefinitionen](./media/storsimple-data-manager-ui/click-add-job-definition.png)

2. hello nytt jobb definition blad öppnas. Namnge din jobbdefinitionen och klicka på **källa**. I hello **konfigurera datakällan** bladet ange hello information om din StorSimple-enhet och hello data av intresse.

    ![Skapa jobbdefinitionen](./media/storsimple-data-manager-ui//create-new-job-deifnition.png)

3. Eftersom det är en ny Data Manager-tjänst har inga databaser konfigurerats. tooadd din StorSimple Manager som en data-databas, klicka på **Lägg till ny** i hello data databasen listrutan och klicka sedan på **Lägg till Data databas**.

4. Välj **StorSimple 8000-serien Manager** som hello databasen skriver och anger hello egenskaperna för din **StorSimple Manager**. För hello **resurs-Id** fält måste tooenter hello nummer före hello **:** i hello registreringsnyckel av din StorSimple manager.

    ![Skapa datakälla](./media/storsimple-data-manager-ui/create-new-data-source.png)

5.  Klicka på **OK** när du är klar. Detta sparar data databasen och den här StorSimple Manager kan återanvändas i andra jobbdefinitioner utan att ange parametrarna igen. Det tar några sekunder efter att du klickar på **OK** för hello StorSimple Manager tooshow upp i hello listrutan.

6.  I hello **konfigurera datakällan** bladet ange hello enhetsnamnet och hello volymnamn som innehåller dina data av intresse.

7.  I hello **Filter** underavsnittets, ange hello rotkatalog som innehåller dina data av intresse (det här fältet måste börja med en `\`). Du kan också lägga till alla filfilter.

8.  hello data transformation-tjänsten fungerar på hello data som skickas in toohello Azure via ögonblicksbilder. När du kör det här jobbet kan välja du tootake en säkerhetskopia varje gång det här jobbet körs (toowork på senaste data) eller toouse hello senaste säkerhetskopian i hello molnet (om du arbetar med vissa arkiverade data).

    ![Information om ny datakälla](./media/storsimple-data-manager-ui/new-data-source-details.png)

9. Därefter måste hello Målinställningar toobe konfigurerats. Det finns 2 typer stöds mål – Azure Media Services och Azure Storage-konton. Välj lagringskonton tooput filer i BLOB på det kontot. Välj media services-konto tooput filer i tillgångar i kontot. Vi behöver igen, tooadd en databas. Välj i listrutan hello **Lägg till ny** och sedan **konfigurera inställningar för**.

    ![Skapa data sink](./media/storsimple-data-manager-ui/create-new-data-sink.png)

10. Här, kan du välja hello typ av databasen som du vill att tooadd och hello andra parametrar som är associerade med hello-databasen. I båda fallen skapas en kö för lagring när hello jobbet körs. Den här kön fylls med meddelanden om transformerade blobbar i takt med att de är klara. hello namnet på den här kön är hello samma som hello namnet på hello jobbdefinitionen. Om du väljer **Media Services** som hello lagringsplatsen typ, sedan kan du också ange lagringskontouppgifter där hello kön skapas.

    ![Nya data sink information](./media/storsimple-data-manager-ui/new-data-sink-details.png)

11. När du lägger till hello data databasen (som tar några sekunder) hittar du hello lagringsplatsen i hello listrutan hello **mål kontonamn**.  Välj hello-mål som du behöver.

12. Klicka på **OK** toocreate hello jobbdefinitionen. Din jobbdefinition har nu konfigurerats. Du kan använda den här jobbdefinitionen flera gånger via hello Användargränssnittet.

    ![Lägg till ny jobbdefinition](./media/storsimple-data-manager-ui/add-new-job-definition.png)

### <a name="run-hello-job-definition"></a>Kör hello jobbdefinitionen

När du behöver toomove data från StorSimple toohello storage-konto som du har angett i hello jobbdefinitionen måste tooinvoke den. Det finns vissa flexibilitet ändring hello parametrar varje gång du anropar hello jobb. hello stegen är följande:

1. Välj din StorSimple Data Manager-tjänsten och gå för**övervakning**. Klicka på **kör nu**.

    ![Utlösaren jobbdefinitionen](./media/storsimple-data-manager-ui/run-now.png)

2. Välj hello jobbdefinitionen som du vill toorun. Klicka på **körningsinställningar** toomodify alla inställningar som du kan toochange för det här jobbet körs.

    ![Kör jobbinställningar](./media/storsimple-data-manager-ui/run-settings.png)

3. Klicka på **OK** och klicka sedan på **kör** toolaunch ditt jobb. toomonitor detta jobb, gå toohello **jobb** sidan i din StorSimple Data Manager.

    ![Lista över jobb och status](./media/storsimple-data-manager-ui/jobs-list-and-status.png)

4. I tillägg toomonitoring i hello **jobb** bladet du kan också lyssna på hello storage-kö där ett meddelande läggs varje gång en fil flyttas från StorSimple toohello storage-konto.


## <a name="next-steps"></a>Nästa steg

[Använder .NET SDK toolaunch StorSimple Data Manager-jobb](storsimple-data-manager-dotnet-jobs.md).
