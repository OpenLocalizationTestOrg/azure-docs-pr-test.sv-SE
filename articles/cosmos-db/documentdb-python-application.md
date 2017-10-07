---
title: "aaaPython Flask självstudien för Azure Cosmos DB | Microsoft Docs"
description: "Granska database-självstudier om hur du använder Azure Cosmos DB toostore och komma åt data från en Python flask finns i Azure. Hitta apputvecklingslösningar."
keywords: Application development, python flask, python web application, python web development
services: cosmos-db
documentationcenter: python
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 20ebec18-67c2-4988-a760-be7c30cfb745
ms.service: cosmos-db
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 08/09/2017
ms.author: mimig
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 87b73c656ed96a7efbd162843a1529d435f027f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-python-flask-web-application-using-azure-cosmos-db"></a>Utveckla ett webbprogram i Python Flask med Azure Cosmos DB
> [!div class="op_single_selector"]
> * [.NET](documentdb-dotnet-application.md)
> * [Node.js](documentdb-nodejs-application.md)
> * [Java](documentdb-java-application.md)
> * [Python](documentdb-python-application.md)
> 
> 

Den här kursen visar hur toouse Azure Cosmos DB toostore och komma åt data från en Python-webbprogram i Azure och förutsätter att du har tidigare erfarenhet av Python och Azure websites.

I den här självstudien om databaser tar vi upp följande:

1. Skapa och etablera ett Cosmos-DB-konto.
2. Skapa en Python Flask-App.
3. Ansluter tooand med Cosmos DB från ditt webbprogram.
4. Distribuera hello web application tooAzure.

Genom att följa den här självstudiekursen skapar du en enkel röstningsapp där du toovote i en omröstning.

![Skärmbild som visar hello röstningsapp som skapats av den här database-självstudier](./media/documentdb-python-application/cosmos-db-pythonr-run-application.png)

## <a name="database-tutorial-prerequisites"></a>Förutsättningar för självstudien om databaser
Innan du följer hello anvisningarna i den här artikeln bör du kontrollera att du har hello följande installerat:

* Ett aktivt Azure-konto. Om du inte har något konto kan du skapa ett kostnadsfritt utvärderingskonto på bara några minuter. Mer information finns i [kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).
 
    ELLER 

    En lokal installation av hello [Azure Cosmos DB emulatorn](local-emulator.md).
* [Microsoft Visual Studio Community 2017](http://www.visualstudio.com/).  
* [Python Tools för Visual Studio](https://github.com/Microsoft/PTVS/).  
* [Microsoft Azure SDK för Python 2.7](https://azure.microsoft.com/downloads/). 
* [Python 2.7.13](https://www.python.org/downloads/windows/). 

> [!IMPORTANT]
> Om du installerar Python 2.7 hello första gången måste du se till att i hello-skärmen anpassa Python 2.7.13, väljer du **Lägg till python.exe tooPath**.
> 
> ![Skärmbild som visar hello anpassa Python 2.7.11 skärmen där du behöver tooselect Lägg till python.exe tooPath](./media/documentdb-python-application/cosmos-db-python-install.png)
> 
> 

* [Microsoft Visual C++-kompilatorn för Python 2.7](https://www.microsoft.com/en-us/download/details.aspx?id=44266).

## <a name="step-1-create-an-azure-cosmos-db-database-account"></a>Steg 1: Skapa ett Azure Cosmos DB-databaskonto
Vi ska börja med att skapa ett Cosmos DB-konto. Om du redan har ett konto eller om du använder hello Azure Cosmos DB-emulatorn för den här kursen kan du hoppa över för[steg 2: skapa en ny Python flask](#step-2-create-a-new-python-flask-web-application).

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

<br/>
Vi kommer att gå igenom hur toocreate en ny Python flask från hello bakgrund.

## <a name="step-2-create-a-new-python-flask-web-application"></a>Steg 2: Skapa en ny webbapp i Python Flask
1. I Visual Studio på hello **filen** -menyn, peka för**ny**, och klicka sedan på **projekt**.
   
    Hej **nytt projekt** dialogrutan visas.
2. I hello till vänster och expanderar **mallar** och sedan **Python**, och klicka sedan på **Web**. 
3. Välj **Flask-webbprojekt** i mittenfönstret hello sedan i hello **namn** skriver **kursen**, och klicka sedan på **OK**. Kom ihåg att paketnamn i Python ska vara gemener, enligt beskrivningen i hello [Stilguide för Python-kod](https://www.python.org/dev/peps/pep-0008/#package-and-module-names).
   
    För de nya tooPython Flask är det en web development programramverk som hjälper dig att skapa webbappar i Python snabbare.
   
    ![Skärmbild som visar hello nytt projekt i Visual Studio med Python markerat hello vänster, Python Flask-webbprojekt valt i mitten hello och hello namnet tutorial i hello namn i fönstret](./media/documentdb-python-application/image9.png)
4. I hello **Python Tools för Visual Studio** -fönstret klickar du på **installera i en virtuell miljö**. 
   
    ![Skärmbild som visar hello databasen tutorial – Python Tools för Visual Studio-fönstret](./media/documentdb-python-application/python-install-virtual-environment.png)
5. I hello **Lägg till virtuell miljö** och du kan acceptera hello standardinställningar och använda Python 2.7 som hello basmiljö eftersom PyDocumentDB för närvarande inte stöder Python 3.x. Klicka sedan på **skapa**. Detta konfigurerar hello krävs Python-miljön för projektet.
   
    ![Skärmbild som visar hello databasen tutorial – Python Tools för Visual Studio-fönstret](./media/documentdb-python-application/image10_A.png)
   
    hello utdata fönstret visas `Successfully installed Flask-0.10.1 Jinja2-2.8 MarkupSafe-0.23 Werkzeug-0.11.5 itsdangerous-0.24 'requirements.txt' was installed successfully.` när hello miljön har installerats.

## <a name="step-3-modify-hello-python-flask-web-application"></a>Steg 3: Ändra hello Python Flask-webbapp
### <a name="add-hello-python-flask-packages-tooyour-project"></a>Lägga till hello Python Flask-paket tooyour projekt
När projektet har konfigurerats, måste du tooadd hello krävs Flask-paket tooyour projekt, inklusive pydocumentdb, hello Python-paketet för DocumentDB.

1. I Solution Explorer öppnar du hello-fil med namnet **requirements.txt** och Ersätt hello innehållet med hello följande:
   
        flask==0.9
        flask-mail==0.7.6
        sqlalchemy==0.7.9
        flask-sqlalchemy==0.16
        sqlalchemy-migrate==0.7.2
        flask-whooshalchemy==0.55a
        flask-wtf==0.8.4
        pytz==2013b
        flask-babel==0.8
        flup
        pydocumentdb>=1.0.0
2. Spara hello **requirements.txt** fil. 
3. Högerklicka på **env** i Solution Explorer och klicka på **Install from requirements.txt**.
   
    ![Skärmbild som visar env (Python 2.7) vald med Install från requirements.txt markerat i listan hello](./media/documentdb-python-application/cosmos-db-python-install-from-requirements.png)
   
    Efter installationen visar utdatafönstret hello hello följande:
   
        Successfully installed Babel-2.3.2 Tempita-0.5.2 WTForms-2.1 Whoosh-2.7.4 blinker-1.4 decorator-4.0.9 flask-0.9 flask-babel-0.8 flask-mail-0.7.6 flask-sqlalchemy-0.16 flask-whooshalchemy-0.55a0 flask-wtf-0.8.4 flup-1.0.2 pydocumentdb-1.6.1 pytz-2013b0 speaklater-1.3 sqlalchemy-0.7.9 sqlalchemy-migrate-0.7.2
   
   > [!NOTE]
   > I sällsynta fall kan du se ett fel i utdatafönstret hello. Om det händer kan du kontrollera om hello fel är relaterade toocleanup. Ibland hello rensningen misslyckas, men kommer fortfarande att lyckas hello installation (Rulla uppåt i hello utdata fönstret tooverify detta). Du kan kontrollera installationen av [verifiera hello virtuell miljö](#verify-the-virtual-environment). Om hello-installationen misslyckades men hello verifieringen lyckas, är det OK toocontinue.
   > 
   > 

### <a name="verify-hello-virtual-environment"></a>Kontrollera hello virtuell miljö
Nu ska vi kontrollera att allt är korrekt installerat.

1. Skapa hello lösning genom att trycka på **Ctrl**+**SKIFT**+**B**.
2. När hello har byggts startar hello webbplats genom att trycka på **F5**. Detta startar hello Flask-utvecklingsservern och din webbläsare. Du bör se hello följande sida.
   
    ![hello tom Python Flask-webbutvecklingsprojektet visas i en webbläsare](./media/documentdb-python-application/image12.png)
3. Stoppa felsökningen hello webbplats genom att trycka på **SKIFT**+**F5** i Visual Studio.

### <a name="create-database-collection-and-document-definitions"></a>Skapa databas, samling och dokumentdefinitioner
Nu skapar vi röstningsappen genom att lägga till nya filer och uppdatera andra.

1. I Solution Explorer högerklickar du på hello **kursen** projektet, klicka på **Lägg till**, och klicka sedan på **nytt objekt**. Välj **tom Python-fil** och namnet hello **forms.py**.  
2. Lägg till hello följande kod toohello Forms.Fy och spara hello-filen.

```python
from flask.ext.wtf import Form
from wtforms import RadioField

class VoteForm(Form):
    deploy_preference  = RadioField('Deployment Preference', choices=[
        ('Web Site', 'Web Site'),
        ('Cloud Service', 'Cloud Service'),
        ('Virtual Machine', 'Virtual Machine')], default='Web Site')
```


### <a name="add-hello-required-imports-tooviewspy"></a>Lägg till hello som krävs för import tooviews.py
1. I Solution Explorer expanderar du hello **kursen** mapp och öppna hello **views.py** fil. 
2. Lägg till följande importera instruktioner toohello överkant hello hello **views.py** och spara hello-filen. Dessa importera Cosmos DB s PythonSDK och hello Flask-paket.
   
    ```python
    from forms import VoteForm
    import config
    import pydocumentdb.document_client as document_client
    ```

### <a name="create-database-collection-and-document"></a>Skapa databas, samling och dokument
* Fortfarande i **views.py**, lägga till hello efter koden toohello hello-filen. Detta hand tar om skapar hello-databas som används av hello formuläret. Ta inte bort hello befintliga koden i **views.py**. Lägg bara till denna toohello end.

```python
@app.route('/create')
def create():
    """Renders hello contact page."""
    client = document_client.DocumentClient(config.DOCUMENTDB_HOST, {'masterKey': config.DOCUMENTDB_KEY})

    # Attempt toodelete hello database.  This allows this toobe used toorecreate as well as create
    try:
        db = next((data for data in client.ReadDatabases() if data['id'] == config.DOCUMENTDB_DATABASE))
        client.DeleteDatabase(db['_self'])
    except:
        pass

    # Create database
    db = client.CreateDatabase({ 'id': config.DOCUMENTDB_DATABASE })

    # Create collection
    collection = client.CreateCollection(db['_self'],{ 'id': config.DOCUMENTDB_COLLECTION })

    # Create document
    document = client.CreateDocument(collection['_self'],
        { 'id': config.DOCUMENTDB_DOCUMENT,
          'Web Site': 0,
          'Cloud Service': 0,
          'Virtual Machine': 0,
          'name': config.DOCUMENTDB_DOCUMENT 
        })

    return render_template(
       'create.html',
        title='Create Page',
        year=datetime.now().year,
        message='You just created a new database, collection, and document.  Your old votes have been deleted')
```


### <a name="read-database-collection-document-and-submit-form"></a>Läsa databas, samling och dokument samt skicka formulär
* Fortfarande i **views.py**, lägga till hello efter koden toohello hello-filen. Detta hand tar om hur du konfigurerar hello formuläret, läsning hello databas, samling och dokument. Ta inte bort hello befintliga koden i **views.py**. Lägg bara till denna toohello end.

```python
@app.route('/vote', methods=['GET', 'POST'])
def vote(): 
    form = VoteForm()
    replaced_document ={}
    if form.validate_on_submit(): # is user submitted vote  
        client = document_client.DocumentClient(config.DOCUMENTDB_HOST, {'masterKey': config.DOCUMENTDB_KEY})

        # Read databases and take first since id should not be duplicated.
        db = next((data for data in client.ReadDatabases() if data['id'] == config.DOCUMENTDB_DATABASE))

        # Read collections and take first since id should not be duplicated.
        coll = next((coll for coll in client.ReadCollections(db['_self']) if coll['id'] == config.DOCUMENTDB_COLLECTION))

        # Read documents and take first since id should not be duplicated.
        doc = next((doc for doc in client.ReadDocuments(coll['_self']) if doc['id'] == config.DOCUMENTDB_DOCUMENT))

        # Take hello data from hello deploy_preference and increment our database
        doc[form.deploy_preference.data] = doc[form.deploy_preference.data] + 1
        replaced_document = client.ReplaceDocument(doc['_self'], doc)

        # Create a model toopass tooresults.html
        class VoteObject:
            choices = dict()
            total_votes = 0

        vote_object = VoteObject()
        vote_object.choices = {
            "Web Site" : doc['Web Site'],
            "Cloud Service" : doc['Cloud Service'],
            "Virtual Machine" : doc['Virtual Machine']
        }
        vote_object.total_votes = sum(vote_object.choices.values())

        return render_template(
            'results.html', 
            year=datetime.now().year, 
            vote_object = vote_object)

    else :
        return render_template(
            'vote.html', 
            title = 'Vote',
            year=datetime.now().year,
            form = form)
```


### <a name="create-hello-html-files"></a>Skapa hello HTML-filer
1. I Solution Explorer i hello **kursen** mapp, höger Klicka på hello **mallar** mappen, klicka på **Lägg till**, och klicka sedan på **nytt objekt**. 
2. Välj **HTML-sidan**, och klicka sedan på hello namn Skriv **create.html**. 
3. Upprepa steg 1 och 2 toocreate två ytterligare HTML-filer: results.html och vote.html.
4. Lägg till följande kod för hello**create.html** i hello `<body>` element. Ett meddelande visas om att vi har skapat en ny databas, en samling och ett dokument.
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>{{ title }}.</h2>
    <h3>{{ message }}</h3>
    <p><a href="{{ url_for('vote') }}" class="btn btn-primary btn-large">Vote &raquo;</a></p>
    {% endblock %}
    ```
5. Lägg till följande kod för hello**results.html** i hello `<body`> element. Den visar hello resultatet av omröstningen hello.
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>Results of hello vote</h2>
        <br />
   
    {% for choice in vote_object.choices %}
    <div class="row">
        <div class="col-sm-5">{{choice}}</div>
            <div class="col-sm-5">
                <div class="progress">
                    <div class="progress-bar" role="progressbar" aria-valuenow="{{vote_object.choices[choice]}}" aria-valuemin="0" aria-valuemax="{{vote_object.total_votes}}" style="width: {{(vote_object.choices[choice]/vote_object.total_votes)*100}}%;">
                                {{vote_object.choices[choice]}}
                </div>
            </div>
            </div>
    </div>
    {% endfor %}
   
    <br />
    <a class="btn btn-primary" href="{{ url_for('vote') }}">Vote again?</a>
    {% endblock %}
    ```
6. Lägg till följande kod för hello**vote.html** i hello `<body`> element. Den visar hello omröstningen och godkänner hello röster. Om hur du registrerar hello röster skickas hello kontroll över tooviews.py där vi identifierar hello rösten cast och bifoga därefter hello dokumentet.
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>What is your favorite way toohost an application on Azure?</h2>
    <form action="" method="post" name="vote">
        {{form.hidden_tag()}}
            {{form.deploy_preference}}
            <button class="btn btn-primary" type="submit">Vote</button>
    </form>
    {% endblock %}
    ```
7. I hello **mallar** Ersätt hello innehållet i mappen **index.html** med hello följande. Detta fungerar som Landningssida för programmet hello.
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>Python + Azure Cosmos DB Voting Application.</h2>
    <h3>This is a sample Cosmos DB voting application using PyDocumentDB</h3>
    <p><a href="{{ url_for('create') }}" class="btn btn-primary btn-large">Create/Clear hello Voting Database &raquo;</a></p>
    <p><a href="{{ url_for('vote') }}" class="btn btn-primary btn-large">Vote &raquo;</a></p>
    {% endblock %}
    ```

### <a name="add-a-configuration-file-and-change-hello-initpy"></a>Lägga till en konfigurationsfil och ändra hello \_ \_init\_\_.py
1. I Solution Explorer högerklickar du på hello **kursen** projektet, klicka på **Lägg till**, klickar du på **nytt objekt**väljer **tom Python-fil**, och sedan namnet hello filen **config.py**. Den här konfigurationsfilen krävs av formulär i Flask. Du kan använda den tooprovide en hemlig nyckel. Nyckeln behövs inte i den här självstudien.
2. Lägg till hello följande kod tooconfig.py behöver du tooalter hello värdena för **DOCUMENTDB\_värden** och **DOCUMENTDB\_NYCKELN** i hello nästa steg.
   
    ```python
    CSRF_ENABLED = True
    SECRET_KEY = 'you-will-never-guess'
   
    DOCUMENTDB_HOST = 'https://YOUR_DOCUMENTDB_NAME.documents.azure.com:443/'
    DOCUMENTDB_KEY = 'YOUR_SECRET_KEY_ENDING_IN_=='
   
    DOCUMENTDB_DATABASE = 'voting database'
    DOCUMENTDB_COLLECTION = 'voting collection'
    DOCUMENTDB_DOCUMENT = 'voting document'
    ```
3. I hello [Azure-portalen](https://portal.azure.com/), navigera toohello **nycklar** bladet genom att klicka på **Bläddra**, **Azure Cosmos DB konton**, dubbelklicka på hello namn för hello toouse-kontot och klicka sedan på hello **nycklar** knapp i hello **Essentials** område. I hello **nycklar** bladet, kopiera hello **URI** värdet och klistrar in den i hello **config.py** som hello värde för hello **DOCUMENTDB\_värden**  egenskapen. 
4. Tillbaka i hello Azure-portalen i hello **nycklar** bladet kopiera hello värdet för hello **primärnyckel** eller hello **sekundärnyckeln**, och klistra in den i hello **config.py**  som hello värde för hello **DOCUMENTDB\_NYCKELN** egenskapen.
5. I hello  **\_ \_init\_\_.py** lägger du till följande rad hello. 
   
        app.config.from_object('config')
   
    Så att hello innehållet hello-filen är:
   
    ```python
    from flask import Flask
    app = Flask(__name__)
    app.config.from_object('config')
    import tutorial.views
    ```
6. När du lägger till alla filer som hello Solution Explorer bör se ut så här:
   
    ![Skärmbild som visar hello Visual Studio Solution Explorer-fönstret](./media/documentdb-python-application/cosmos-db-python-solution-explorer.png)

## <a name="step-4-run-your-web-application-locally"></a>Steg 4: Kör webbappen lokalt
1. Skapa hello lösning genom att trycka på **Ctrl**+**SKIFT**+**B**.
2. När hello har byggts startar hello webbplats genom att trycka på **F5**. Du bör se följande hello på skärmen.
   
    ![Skärmbild som visar hello Python + Azure Cosmos DB Röstningsappen visad i en webbläsare](./media/documentdb-python-application/cosmos-db-pythonr-run-application.png)
3. Klicka på **skapa/Rensa hello röstning databasen** toogenerate hello-databasen.
   
    ![Skärmbild som visar hello Skapa sida för hello webbappen – utvecklingsdetaljer](./media/documentdb-python-application/cosmos-db-python-run-create-page.png)
4. Klicka sedan på **Rösta** och välj ett alternativ.
   
    ![Skärmbild som visar hello webbprogrammet med en röstningsfråga](./media/documentdb-python-application/cosmos-db-vote.png)
5. För varje röst du lägger ökas hello aktuella räknaren.
   
    ![Skärmbild som visar hello resultaten av hello sidan röstningsresultat](./media/documentdb-python-application/cosmos-db-voting-results.png)
6. Stoppa felsökningen hello projektet genom att trycka på SKIFT + F5.

## <a name="step-5-deploy-hello-web-application-tooazure"></a>Steg 5: Distribuera hello web application tooAzure
Nu när du har hello hela appen fungerar mot Cosmos DB vi toodeploy denna tooAzure.

1. Högerklicka på hello projektet i Solution Explorer (Kontrollera att du inte fortfarande körs lokalt) och välj **publicera**.  
   
     ![Skärmbild som visar hello tutorial markerad i Solution Explorer med alternativet för hello publicera markerat](./media/documentdb-python-application/image20.png)
2. I hello **publicera** dialogrutan **Microsoft Azure App Service**väljer **Skapa nytt**, och klicka sedan på **publicera**.
   
    ![Skärmbild som visar hello Publicera webbplats fönstret med Microsoft Azure App Service markerat](./media/documentdb-python-application/cosmos-db-python-publish.png)
3. I hello **skapa Apptjänst** dialogrutan Ange hello namn för ditt webbprogram tillsammans med din **prenumeration**, **resursgruppen**, och **App Service-Plan** , klicka på **skapa**.
   
    ![Skärmbild som visar hello Microsoft Azure Web Apps fönster fönster](./media/documentdb-python-application/cosmos-db-python-create-app-service.png)
4. I några sekunder har Visual Studio publicerat din apptjänst och starta en webbläsare där du kan se ditt arbete som körs i Azure!

    ![Skärmbild som visar hello Microsoft Azure Web Apps fönster fönster](./media/documentdb-python-application/cosmos-db-python-appservice-created.png)

## <a name="troubleshooting"></a>Felsökning
Om detta är hello första Python-appen du kör på datorn kontrollerar du att hello följande mappar (eller hello motsvarande installationsplatser) ingår i PATH-variabeln:

    C:\Python27\site-packages;C:\Python27\;C:\Python27\Scripts;

Om du får ett felmeddelande på röstningssidan och du gav ditt projekt något annat än **kursen**, se till att  **\_ \_init\_\_.py** referenser hello rätt projektnamn i hello rad: `import tutorial.view`.

## <a name="next-steps"></a>Nästa steg
Grattis! Du har precis slutfört din första Python-webbapp med Cosmos-DB och publicerat den tooAzure.

Vi uppdaterar och förbättrar ofta den här artikeln baserat på dina synpunkter.  När du har slutfört hello kursen ta med hello röstning knappar på hello upp och längst ned på sidan och vara säker på att tooinclude din feedback på vilka förbättringar som du vill att toosee som gjorts. Om du vill att vi toocontact du direkt, anser ledigt tooinclude den e-postadress i dina kommentarer.

webbprogrammet för tooadd ytterligare funktioner tooyour, granska hello API: er som är tillgängliga i hello [Azure Cosmos DB Python SDK](documentdb-sdk-python.md).

Mer information om Azure, Visual Studio och Python finns hello [Python Developer Center](https://azure.microsoft.com/develop/python/). 

Ytterligare självstudier om Python Flask finns [hello ingående självstudie om Flask, del I: Hello, World!](http://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world). 

[Visual Studio Express]: http://www.visualstudio.com/products/visual-studio-express-vs.aspx
[2]: https://www.python.org/downloads/windows/
[3]: https://www.microsoft.com/download/details.aspx?id=44266
[Microsoft Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
[Azure portal]: http://portal.azure.com
