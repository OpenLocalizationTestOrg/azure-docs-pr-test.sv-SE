---
title: "aaaFlask och Azure Table Storage på Azure med Python Tools 2.2 för Visual Studio"
description: "Lär dig hur toouse hello Python Tools för Visual Studio toocreate en Flask-webbapp som lagrar data i Azure Table Storage och distribuera den tooAzure App Service Web Apps."
services: app-service\web
tags: python
documentationcenter: python
author: huguesv
manager: erikre
editor: 
ms.assetid: d8e70a29-aca1-4010-95f5-cfe769e3be06
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 07/07/2016
ms.author: huvalo
ms.openlocfilehash: 1a09d4cc78078a00492ba4fe7e2075df96fb0380
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="flask-and-azure-table-storage-on-azure-with-python-tools-22-for-visual-studio"></a>Flask och Azure Table Storage på Azure med Python Tools 2.2 för Visual Studio
I den här kursen använder vi [Python Tools för Visual Studio] toocreate ett enkelt avfrågar webbapp med någon av exempelmallarna i hello PTVS. Den här kursen finns också tillgänglig som en [video](https://www.youtube.com/watch?v=qUtZWtPwbTk).

Hej webbapp definierar en abstraktion för dess databas, så du kan enkelt växla mellan olika typer av databaser (i minnet, Azure Table Storage MongoDB).

Vi lära dig hur toocreate ett Azure Storage-konto, hur tooconfigure hello web app toouse Azure Table Storage och hur toopublish hello webbprogrammet för[Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).

Se hello [Python Developer Center] fler artiklar om utvecklingen av Azure App Service Web Apps med PTVS med hjälp av Bottle, Flask och Django webbramverken med MongoDB, Azure Table Storage, MySQL och SQL Database-tjänster. Den här artikeln fokuserar på App Service, hello stegen är liknande när du utvecklar [Azure Cloud Services].

## <a name="prerequisites"></a>Krav
* Visual Studio 2015
* [Python Tools 2.2 för Visual Studio]
* [Python Tools 2.2 för Visual Studio Samples VSI]
* [Azure SDK Tools för VS 2015]
* [Python 2.7 32-bitars] eller [Python 3.4 32-bitars]

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service. Inget kreditkort krävs, och du gör inga åtaganden.
> 
> 

## <a name="create-hello-project"></a>Skapa hello projekt
I det här avsnittet skapar vi ett Visual Studio-projekt utifrån en exempelmall. Vi skapa en virtuell miljö och installera nödvändiga paket. Sedan kör vi hello programmet lokalt med hjälp av hello standarddatabas i minnet.

1. I Visual Studio väljer du **Arkiv**, **Nytt projekt**.
2. Hej projektmallar från hello [Python Tools 2.2 för Visual Studio Samples VSI] är tillgängliga under **Python**, **exempel**. Välj **avsökningar Flask-webbprojekt** och klicka på OK toocreate hello projektet.
   
     ![Dialogrutan Nytt projekt](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskNewProject.png)
3. Du kommer att tillfrågas tooinstall externa paket. Välj **Install into a virtual environment** (Installera i en virtuell miljö).
   
     ![Dialogrutan Externa paket](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskExternalPackages.png)
4. Välj **Python 2.7** eller **Python 3.4** som hello bastolk.
   
     ![Dialogrutan Add Virtual Environment (Lägg till virtuell miljö)](./media/web-sites-python-ptvs-flask-table-storage/PollsCommonAddVirtualEnv.png)
5. Bekräfta att hello programmet fungerar genom att trycka på `F5`. Som standard använder programmet hello en InMemory-lagringsplats som inte kräver någon konfiguration. Alla data går förlorade när hello webbservern har stoppats.
6. Klicka på **skapa Exempelomröstningar**, klicka på en omröstning och rösta.
   
     ![Webbläsare](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskInMemoryBrowser.png)

## <a name="create-an-azure-storage-account"></a>Skapa ett Azure Storage-konto
toouse lagringsåtgärder du behöver ett Azure storage-konto. Du kan skapa ett lagringskonto genom att följa dessa steg.

1. Logga in på hello [Azure Portal](https://portal.azure.com/).
2. Klicka på hello **ny** ikonen hello längst upp till vänster i hello Portal och klicka sedan på **Data + lagring** > **Lagringskonto**. Klicka på **skapa**, och ge hello storage-konto ett unikt namn och skapa en ny [resursgruppen](../azure-resource-manager/resource-group-overview.md) för den.
   
      ![Snabbregistrering](./media/web-sites-python-ptvs-flask-table-storage/PollsCommonAzureStorageCreate.png)
   
    När hello storage-konto har skapats, hello **meddelanden** knappen blinkar en grön **lyckade** och hello lagringskontots blad är öppet tooshow det hör toohello ny resurs gruppen du Skapa.
3. Klicka på hello **åtkomstnycklar** ingår i hello lagringskontots bladet. Anteckna hello kontonamnet och hello key1.
   
      ![Nycklar](./media/web-sites-python-ptvs-flask-table-storage/PollsCommonAzureStorageKeys.png)
   
    Vi behöver denna information tooconfigure ditt projekt i hello nästa avsnitt.

## <a name="configure-hello-project"></a>Konfigurera hello projekt
I det här avsnittet konfigurerar vi våra program toouse hello lagringskonto vi just skapade. Vi se hur tooobtain anslutningsinställningar från hello Azure-portalen. Sedan ska vi kör hello program lokalt.

1. Högerklicka på projektnoden i Solution Explorer i Visual Studio och välj **egenskaper**. Klicka på hello **felsöka** fliken.
   
     ![Inställningar för felsökning av projektet](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskAzureTableStorageProjectDebugSettings.png)
2. Ange hello värden för miljövariabler som krävs av programmet hello i **kommandot Debug Server**, **miljö**.
   
       REPOSITORY_NAME=azuretablestorage
       STORAGE_NAME=<storage account name>
       STORAGE_KEY=<primary access key>
   
   Detta anger hello miljövariabler när du **Start Debugging**. Om du vill ange hello variabler toobe när du **Start utan Debugging**, ange hello samma värden under **kommandot Kör servern** samt.
   
   Du kan också definiera miljövariablerna med hjälp av hello på Kontrollpanelen. Detta är ett bättre alternativ om du vill lagra autentiseringsuppgifter i källkoden tooavoid / project filen. Observera att du måste toorestart Visual Studio hello nya miljö värden toobe tillgängliga toohello programmet.
3. hello-kod som implementerar hello Azure Table Storage databasen är i **models/azuretablestorage.py**. Se hello [dokumentationen] för mer information om hur toouse Tabelltjänsten från Python.
4. Kör programmet hello med `F5`. Omröstningar som skapas med **skapa Exempelomröstningar** och hello-data som skickats vid röstning serialiseras i Azure Table Storage.
   
   > [!NOTE]
   > hello virtuell miljö för Python 2.7 kan orsaka ett undantag break i Visual Studio.  Tryck på `F5` toocontinue inläsning hello webbprojekt.
   > 
   > 
5. Bläddra toohello **om** sidan tooverify som hello program använder hello **Azure Table Storage** databasen.
   
     ![Webbläsare](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskAzureTableStorageAbout.png)

## <a name="explore-hello-azure-table-storage"></a>Utforska hello Azure-tabellagring
Det är enkelt tooview och redigera storage-tabeller med Cloud Explorer i Visual Studio. I det här avsnittet använder vi Server Explorer tooview hello innehållet i hello avsökningar programmet tabeller.

> [!NOTE]
> Detta kräver Microsoft Azure-verktyg toobe installerat, som är tillgängliga som en del av hello [Azure SDK för .NET].
> 
> 

1. Öppna **Cloud Explorer**. Expandera **Lagringskonton**, ditt lagringskonto, sedan **tabeller**.
   
     ![Cloud Explorer](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonServerExplorer.png)
2. Dubbelklicka på hello **polls** eller **val** tabell tooview hello innehållet i hello tabell i ett dokumentfönster, samt lägga till eller ta bort/redigera entiteter.
   
     ![Tabell frågeresultat](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonServerExplorerTable.png)

## <a name="publish-hello-web-app-tooazure-app-service"></a>Publicera hello web app tooAzure Apptjänst
hello Azure .NET SDK innehåller ett enkelt sätt toodeploy din web app tooAzure Apptjänst.

1. I **Solution Explorer**, högerklicka på hello projektnoden och väljer **publicera**.
   
     ![Dialogrutan Publicera webbplats](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonPublishWebSiteDialog.png)
2. Klicka på **Microsoft Azure Web Apps**.
3. Klicka på **ny** toocreate ett nytt webbprogram.
4. Fyll i följande fält hello och på **skapa**.
   
   * **Webbappens namn**
   * **App Service-plan**
   * **Resursgrupp**
   * **Region**
   * Lämna **databasserver** ställa in också**ingen databas**
5. Godkänn alla standardvärden och klicka på **Publicera**.
6. Webbläsaren öppnas automatiskt toohello publicerade webbprogram. Om du bläddrar toohello om sidan ser du att den använder hello **i minnet** databasen, inte hello **Azure Table Storage** databasen.
   
   Det beror på att hello miljövariablerna inte anges på instansen för hello Web Apps i Azure App Service, så att den använder hello standardvärden som anges i **settings.py**.

## <a name="configure-hello-web-apps-instance"></a>Konfigurera hello Web Apps instans
I det här avsnittet konfigurerar vi miljövariabler för hello Web Apps-instansen.

1. I [Azure Portal](https://portal.azure.com), öppna hello webbapps blad genom att klicka på **Bläddra** > **Apptjänster** > din webbprogrammets namn.
2. I din webbapps blad klickar du på **alla inställningar**, klicka på **programinställningar**.
3. Bläddra nedåt toohello **appinställningar** och ange hello värden för **databasen\_namn**, **lagring\_namn** och  **LAGRING\_NYCKELN** enligt beskrivningen i hello **konfigurera hello projektet** ovan.
   
     ![App-inställningar](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonWebSiteConfigureSettingsTableStorage.png)
4. Klicka på **Spara**. När du har fått hello meddelanden att hello ändringarna har tillämpats, klickar du på **Bläddra** från hello Web app huvudblad.
5. Du bör se hello web app fungerar som förväntat, med hjälp av hello **Azure Table Storage** databasen.
   
   Grattis!
   
     ![Webbläsare](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskAzureBrowser.png)

## <a name="next-steps"></a>Nästa steg
Följ dessa länkar toolearn mer om Python Tools för Visual Studio, Flask och Azure Table Storage.

* [Dokumentation för Python Tools för Visual Studio]
  * [Webbprojekt]
  * [Molntjänstprojekt]
  * [Fjärrfelsökning i Microsoft Azure]
* [Flask-dokumentation]
* [Azure Storage]
* [Azure SDK för Python]
* [Hur tooUse hello Table Storage-tjänsten från Python]

## <a name="whats-changed"></a>Nyheter
* En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)

<!--Link references-->
[Python Developer Center]: /develop/python/
[Azure Cloud Services]: ../cloud-services/cloud-services-python-ptvs.md
[dokumentationen]:../cosmos-db/table-storage-how-to-use-python.md
[Hur tooUse hello Table Storage-tjänsten från Python]:../cosmos-db/table-storage-how-to-use-python.md

<!--External Link references-->
[Azure Portal]: https://portal.azure.com
[Azure SDK för .NET]: http://azure.microsoft.com/downloads/
[Python Tools för Visual Studio]: http://aka.ms/ptvs
[Python Tools 2.2 för Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Python Tools 2.2 för Visual Studio Samples VSI]: http://go.microsoft.com/fwlink/?LinkID=624025
[Azure SDK Tools för VS 2015]: http://go.microsoft.com/fwlink/?linkid=518003
[Python 2.7 32-bitars]: http://go.microsoft.com/fwlink/?LinkId=517190
[Python 3.4 32-bitars]: http://go.microsoft.com/fwlink/?LinkId=517191
[Dokumentation för Python Tools för Visual Studio]: http://aka.ms/ptvsdocs
[Flask-dokumentation]: http://flask.pocoo.org/
[Fjärrfelsökning i Microsoft Azure]: http://go.microsoft.com/fwlink/?LinkId=624026
[Webbprojekt]: http://go.microsoft.com/fwlink/?LinkId=624027
[Molntjänstprojekt]: http://go.microsoft.com/fwlink/?LinkId=624028
[Azure Storage]: http://azure.microsoft.com/documentation/services/storage/
[Azure SDK för Python]: https://github.com/Azure/azure-sdk-for-python
