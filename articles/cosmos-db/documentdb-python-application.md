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
# <a name="build-a-python-flask-web-application-using-azure-cosmos-db"></a><span data-ttu-id="8fb41-105">Utveckla ett webbprogram i Python Flask med Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="8fb41-105">Build a Python Flask web application using Azure Cosmos DB</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="8fb41-106">.NET</span><span class="sxs-lookup"><span data-stu-id="8fb41-106">.NET</span></span>](documentdb-dotnet-application.md)
> * [<span data-ttu-id="8fb41-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="8fb41-107">Node.js</span></span>](documentdb-nodejs-application.md)
> * [<span data-ttu-id="8fb41-108">Java</span><span class="sxs-lookup"><span data-stu-id="8fb41-108">Java</span></span>](documentdb-java-application.md)
> * [<span data-ttu-id="8fb41-109">Python</span><span class="sxs-lookup"><span data-stu-id="8fb41-109">Python</span></span>](documentdb-python-application.md)
> 
> 

<span data-ttu-id="8fb41-110">Den här kursen visar hur toouse Azure Cosmos DB toostore och komma åt data från en Python-webbprogram i Azure och förutsätter att du har tidigare erfarenhet av Python och Azure websites.</span><span class="sxs-lookup"><span data-stu-id="8fb41-110">This tutorial shows you how toouse Azure Cosmos DB toostore and access data from a Python web application hosted on Azure and presumes that you have some prior experience using Python and Azure websites.</span></span>

<span data-ttu-id="8fb41-111">I den här självstudien om databaser tar vi upp följande:</span><span class="sxs-lookup"><span data-stu-id="8fb41-111">This database tutorial covers:</span></span>

1. <span data-ttu-id="8fb41-112">Skapa och etablera ett Cosmos-DB-konto.</span><span class="sxs-lookup"><span data-stu-id="8fb41-112">Creating and provisioning a Cosmos DB account.</span></span>
2. <span data-ttu-id="8fb41-113">Skapa en Python Flask-App.</span><span class="sxs-lookup"><span data-stu-id="8fb41-113">Creating a Python Flask application.</span></span>
3. <span data-ttu-id="8fb41-114">Ansluter tooand med Cosmos DB från ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="8fb41-114">Connecting tooand using Cosmos DB from your web application.</span></span>
4. <span data-ttu-id="8fb41-115">Distribuera hello web application tooAzure.</span><span class="sxs-lookup"><span data-stu-id="8fb41-115">Deploying hello web application tooAzure.</span></span>

<span data-ttu-id="8fb41-116">Genom att följa den här självstudiekursen skapar du en enkel röstningsapp där du toovote i en omröstning.</span><span class="sxs-lookup"><span data-stu-id="8fb41-116">By following this tutorial, you will build a simple voting application that allows you toovote for a poll.</span></span>

![Skärmbild som visar hello röstningsapp som skapats av den här database-självstudier](./media/documentdb-python-application/cosmos-db-pythonr-run-application.png)

## <a name="database-tutorial-prerequisites"></a><span data-ttu-id="8fb41-118">Förutsättningar för självstudien om databaser</span><span class="sxs-lookup"><span data-stu-id="8fb41-118">Database tutorial prerequisites</span></span>
<span data-ttu-id="8fb41-119">Innan du följer hello anvisningarna i den här artikeln bör du kontrollera att du har hello följande installerat:</span><span class="sxs-lookup"><span data-stu-id="8fb41-119">Before following hello instructions in this article, you should ensure that you have hello following installed:</span></span>

* <span data-ttu-id="8fb41-120">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="8fb41-120">An active Azure account.</span></span> <span data-ttu-id="8fb41-121">Om du inte har något konto kan du skapa ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="8fb41-121">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="8fb41-122">Mer information finns i [kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8fb41-122">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
 
    <span data-ttu-id="8fb41-123">ELLER</span><span class="sxs-lookup"><span data-stu-id="8fb41-123">OR</span></span> 

    <span data-ttu-id="8fb41-124">En lokal installation av hello [Azure Cosmos DB emulatorn](local-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="8fb41-124">A local installation of hello [Azure Cosmos DB Emulator](local-emulator.md).</span></span>
* <span data-ttu-id="8fb41-125">[Microsoft Visual Studio Community 2017](http://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="8fb41-125">[Microsoft Visual Studio Community 2017](http://www.visualstudio.com/).</span></span>  
* <span data-ttu-id="8fb41-126">[Python Tools för Visual Studio](https://github.com/Microsoft/PTVS/).</span><span class="sxs-lookup"><span data-stu-id="8fb41-126">[Python Tools for Visual Studio](https://github.com/Microsoft/PTVS/).</span></span>  
* <span data-ttu-id="8fb41-127">[Microsoft Azure SDK för Python 2.7](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="8fb41-127">[Microsoft Azure SDK for Python 2.7](https://azure.microsoft.com/downloads/).</span></span> 
* <span data-ttu-id="8fb41-128">[Python 2.7.13](https://www.python.org/downloads/windows/).</span><span class="sxs-lookup"><span data-stu-id="8fb41-128">[Python 2.7.13](https://www.python.org/downloads/windows/).</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="8fb41-129">Om du installerar Python 2.7 hello första gången måste du se till att i hello-skärmen anpassa Python 2.7.13, väljer du **Lägg till python.exe tooPath**.</span><span class="sxs-lookup"><span data-stu-id="8fb41-129">If you are installing Python 2.7 for hello first time, ensure that in hello Customize Python 2.7.13 screen, you select **Add python.exe tooPath**.</span></span>
> 
> ![Skärmbild som visar hello anpassa Python 2.7.11 skärmen där du behöver tooselect Lägg till python.exe tooPath](./media/documentdb-python-application/cosmos-db-python-install.png)
> 
> 

* <span data-ttu-id="8fb41-131">[Microsoft Visual C++-kompilatorn för Python 2.7](https://www.microsoft.com/en-us/download/details.aspx?id=44266).</span><span class="sxs-lookup"><span data-stu-id="8fb41-131">[Microsoft Visual C++ Compiler for Python 2.7](https://www.microsoft.com/en-us/download/details.aspx?id=44266).</span></span>

## <a name="step-1-create-an-azure-cosmos-db-database-account"></a><span data-ttu-id="8fb41-132">Steg 1: Skapa ett Azure Cosmos DB-databaskonto</span><span class="sxs-lookup"><span data-stu-id="8fb41-132">Step 1: Create an Azure Cosmos DB database account</span></span>
<span data-ttu-id="8fb41-133">Vi ska börja med att skapa ett Cosmos DB-konto.</span><span class="sxs-lookup"><span data-stu-id="8fb41-133">Let's start by creating an Cosmos DB account.</span></span> <span data-ttu-id="8fb41-134">Om du redan har ett konto eller om du använder hello Azure Cosmos DB-emulatorn för den här kursen kan du hoppa över för[steg 2: skapa en ny Python flask](#step-2-create-a-new-python-flask-web-application).</span><span class="sxs-lookup"><span data-stu-id="8fb41-134">If you already have an account or if you are using hello Azure Cosmos DB Emulator for this tutorial, you can skip too[Step 2: Create a new Python Flask web application](#step-2-create-a-new-python-flask-web-application).</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

<br/>
<span data-ttu-id="8fb41-135">Vi kommer att gå igenom hur toocreate en ny Python flask från hello bakgrund.</span><span class="sxs-lookup"><span data-stu-id="8fb41-135">We will now walk through how toocreate a new Python Flask web application from hello ground up.</span></span>

## <a name="step-2-create-a-new-python-flask-web-application"></a><span data-ttu-id="8fb41-136">Steg 2: Skapa en ny webbapp i Python Flask</span><span class="sxs-lookup"><span data-stu-id="8fb41-136">Step 2: Create a new Python Flask web application</span></span>
1. <span data-ttu-id="8fb41-137">I Visual Studio på hello **filen** -menyn, peka för**ny**, och klicka sedan på **projekt**.</span><span class="sxs-lookup"><span data-stu-id="8fb41-137">In Visual Studio, on hello **File** menu, point too**New**, and then click **Project**.</span></span>
   
    <span data-ttu-id="8fb41-138">Hej **nytt projekt** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="8fb41-138">hello **New Project** dialog box appears.</span></span>
2. <span data-ttu-id="8fb41-139">I hello till vänster och expanderar **mallar** och sedan **Python**, och klicka sedan på **Web**.</span><span class="sxs-lookup"><span data-stu-id="8fb41-139">In hello left pane, expand **Templates** and then **Python**, and then click **Web**.</span></span> 
3. <span data-ttu-id="8fb41-140">Välj **Flask-webbprojekt** i mittenfönstret hello sedan i hello **namn** skriver **kursen**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="8fb41-140">Select **Flask  Web Project** in hello center pane, then in hello **Name** box type **tutorial**, and then click **OK**.</span></span> <span data-ttu-id="8fb41-141">Kom ihåg att paketnamn i Python ska vara gemener, enligt beskrivningen i hello [Stilguide för Python-kod](https://www.python.org/dev/peps/pep-0008/#package-and-module-names).</span><span class="sxs-lookup"><span data-stu-id="8fb41-141">Remember that Python package names should be all lowercase, as described in hello [Style Guide for Python Code](https://www.python.org/dev/peps/pep-0008/#package-and-module-names).</span></span>
   
    <span data-ttu-id="8fb41-142">För de nya tooPython Flask är det en web development programramverk som hjälper dig att skapa webbappar i Python snabbare.</span><span class="sxs-lookup"><span data-stu-id="8fb41-142">For those new tooPython Flask, it is a web application development framework that helps you build web applications in Python faster.</span></span>
   
    ![Skärmbild som visar hello nytt projekt i Visual Studio med Python markerat hello vänster, Python Flask-webbprojekt valt i mitten hello och hello namnet tutorial i hello namn i fönstret](./media/documentdb-python-application/image9.png)
4. <span data-ttu-id="8fb41-144">I hello **Python Tools för Visual Studio** -fönstret klickar du på **installera i en virtuell miljö**.</span><span class="sxs-lookup"><span data-stu-id="8fb41-144">In hello **Python Tools for Visual Studio** window, click **Install into a virtual environment**.</span></span> 
   
    ![Skärmbild som visar hello databasen tutorial – Python Tools för Visual Studio-fönstret](./media/documentdb-python-application/python-install-virtual-environment.png)
5. <span data-ttu-id="8fb41-146">I hello **Lägg till virtuell miljö** och du kan acceptera hello standardinställningar och använda Python 2.7 som hello basmiljö eftersom PyDocumentDB för närvarande inte stöder Python 3.x. Klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="8fb41-146">In hello **Add Virtual Environment** window, you can accept hello defaults and use Python 2.7 as hello base environment because PyDocumentDB does not currently support Python 3.x, and then click **Create**.</span></span> <span data-ttu-id="8fb41-147">Detta konfigurerar hello krävs Python-miljön för projektet.</span><span class="sxs-lookup"><span data-stu-id="8fb41-147">This sets up hello required Python virtual environment for your project.</span></span>
   
    ![Skärmbild som visar hello databasen tutorial – Python Tools för Visual Studio-fönstret](./media/documentdb-python-application/image10_A.png)
   
    <span data-ttu-id="8fb41-149">hello utdata fönstret visas `Successfully installed Flask-0.10.1 Jinja2-2.8 MarkupSafe-0.23 Werkzeug-0.11.5 itsdangerous-0.24 'requirements.txt' was installed successfully.` när hello miljön har installerats.</span><span class="sxs-lookup"><span data-stu-id="8fb41-149">hello output window displays `Successfully installed Flask-0.10.1 Jinja2-2.8 MarkupSafe-0.23 Werkzeug-0.11.5 itsdangerous-0.24 'requirements.txt' was installed successfully.` when hello environment is successfully installed.</span></span>

## <a name="step-3-modify-hello-python-flask-web-application"></a><span data-ttu-id="8fb41-150">Steg 3: Ändra hello Python Flask-webbapp</span><span class="sxs-lookup"><span data-stu-id="8fb41-150">Step 3: Modify hello Python Flask web application</span></span>
### <a name="add-hello-python-flask-packages-tooyour-project"></a><span data-ttu-id="8fb41-151">Lägga till hello Python Flask-paket tooyour projekt</span><span class="sxs-lookup"><span data-stu-id="8fb41-151">Add hello Python Flask packages tooyour project</span></span>
<span data-ttu-id="8fb41-152">När projektet har konfigurerats, måste du tooadd hello krävs Flask-paket tooyour projekt, inklusive pydocumentdb, hello Python-paketet för DocumentDB.</span><span class="sxs-lookup"><span data-stu-id="8fb41-152">After your project is set up, you'll need tooadd hello required Flask packages tooyour project, including pydocumentdb, hello Python package for DocumentDB.</span></span>

1. <span data-ttu-id="8fb41-153">I Solution Explorer öppnar du hello-fil med namnet **requirements.txt** och Ersätt hello innehållet med hello följande:</span><span class="sxs-lookup"><span data-stu-id="8fb41-153">In Solution Explorer, open hello file named **requirements.txt** and replace hello contents with hello following:</span></span>
   
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
2. <span data-ttu-id="8fb41-154">Spara hello **requirements.txt** fil.</span><span class="sxs-lookup"><span data-stu-id="8fb41-154">Save hello **requirements.txt** file.</span></span> 
3. <span data-ttu-id="8fb41-155">Högerklicka på **env** i Solution Explorer och klicka på **Install from requirements.txt**.</span><span class="sxs-lookup"><span data-stu-id="8fb41-155">In Solution Explorer, right-click **env** and click **Install from requirements.txt**.</span></span>
   
    ![Skärmbild som visar env (Python 2.7) vald med Install från requirements.txt markerat i listan hello](./media/documentdb-python-application/cosmos-db-python-install-from-requirements.png)
   
    <span data-ttu-id="8fb41-157">Efter installationen visar utdatafönstret hello hello följande:</span><span class="sxs-lookup"><span data-stu-id="8fb41-157">After successful installation, hello output window displays hello following:</span></span>
   
        Successfully installed Babel-2.3.2 Tempita-0.5.2 WTForms-2.1 Whoosh-2.7.4 blinker-1.4 decorator-4.0.9 flask-0.9 flask-babel-0.8 flask-mail-0.7.6 flask-sqlalchemy-0.16 flask-whooshalchemy-0.55a0 flask-wtf-0.8.4 flup-1.0.2 pydocumentdb-1.6.1 pytz-2013b0 speaklater-1.3 sqlalchemy-0.7.9 sqlalchemy-migrate-0.7.2
   
   > [!NOTE]
   > <span data-ttu-id="8fb41-158">I sällsynta fall kan du se ett fel i utdatafönstret hello.</span><span class="sxs-lookup"><span data-stu-id="8fb41-158">In rare cases, you might see a failure in hello output window.</span></span> <span data-ttu-id="8fb41-159">Om det händer kan du kontrollera om hello fel är relaterade toocleanup.</span><span class="sxs-lookup"><span data-stu-id="8fb41-159">If this happens, check if hello error is related toocleanup.</span></span> <span data-ttu-id="8fb41-160">Ibland hello rensningen misslyckas, men kommer fortfarande att lyckas hello installation (Rulla uppåt i hello utdata fönstret tooverify detta).</span><span class="sxs-lookup"><span data-stu-id="8fb41-160">Sometimes hello cleanup fails, but hello installation will still be successful (scroll up in hello output window tooverify this).</span></span> <span data-ttu-id="8fb41-161">Du kan kontrollera installationen av [verifiera hello virtuell miljö](#verify-the-virtual-environment).</span><span class="sxs-lookup"><span data-stu-id="8fb41-161">You can check your installation by [Verifying hello virtual environment](#verify-the-virtual-environment).</span></span> <span data-ttu-id="8fb41-162">Om hello-installationen misslyckades men hello verifieringen lyckas, är det OK toocontinue.</span><span class="sxs-lookup"><span data-stu-id="8fb41-162">If hello installation failed but hello verification is successful, it's OK toocontinue.</span></span>
   > 
   > 

### <a name="verify-hello-virtual-environment"></a><span data-ttu-id="8fb41-163">Kontrollera hello virtuell miljö</span><span class="sxs-lookup"><span data-stu-id="8fb41-163">Verify hello virtual environment</span></span>
<span data-ttu-id="8fb41-164">Nu ska vi kontrollera att allt är korrekt installerat.</span><span class="sxs-lookup"><span data-stu-id="8fb41-164">Let's make sure that everything is installed correctly.</span></span>

1. <span data-ttu-id="8fb41-165">Skapa hello lösning genom att trycka på **Ctrl**+**SKIFT**+**B**.</span><span class="sxs-lookup"><span data-stu-id="8fb41-165">Build hello solution by pressing **Ctrl**+**Shift**+**B**.</span></span>
2. <span data-ttu-id="8fb41-166">När hello har byggts startar hello webbplats genom att trycka på **F5**.</span><span class="sxs-lookup"><span data-stu-id="8fb41-166">Once hello build succeeds, start hello website by pressing **F5**.</span></span> <span data-ttu-id="8fb41-167">Detta startar hello Flask-utvecklingsservern och din webbläsare.</span><span class="sxs-lookup"><span data-stu-id="8fb41-167">This launches hello Flask development server and starts your web browser.</span></span> <span data-ttu-id="8fb41-168">Du bör se hello följande sida.</span><span class="sxs-lookup"><span data-stu-id="8fb41-168">You should see hello following page.</span></span>
   
    ![hello tom Python Flask-webbutvecklingsprojektet visas i en webbläsare](./media/documentdb-python-application/image12.png)
3. <span data-ttu-id="8fb41-170">Stoppa felsökningen hello webbplats genom att trycka på **SKIFT**+**F5** i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8fb41-170">Stop debugging hello website by pressing **Shift**+**F5** in Visual Studio.</span></span>

### <a name="create-database-collection-and-document-definitions"></a><span data-ttu-id="8fb41-171">Skapa databas, samling och dokumentdefinitioner</span><span class="sxs-lookup"><span data-stu-id="8fb41-171">Create database, collection, and document definitions</span></span>
<span data-ttu-id="8fb41-172">Nu skapar vi röstningsappen genom att lägga till nya filer och uppdatera andra.</span><span class="sxs-lookup"><span data-stu-id="8fb41-172">Now let's create your voting application by adding new files and updating others.</span></span>

1. <span data-ttu-id="8fb41-173">I Solution Explorer högerklickar du på hello **kursen** projektet, klicka på **Lägg till**, och klicka sedan på **nytt objekt**.</span><span class="sxs-lookup"><span data-stu-id="8fb41-173">In Solution Explorer, right-click hello **tutorial** project, click **Add**, and then click **New Item**.</span></span> <span data-ttu-id="8fb41-174">Välj **tom Python-fil** och namnet hello **forms.py**.</span><span class="sxs-lookup"><span data-stu-id="8fb41-174">Select **Empty Python File** and name hello file **forms.py**.</span></span>  
2. <span data-ttu-id="8fb41-175">Lägg till hello följande kod toohello Forms.Fy och spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="8fb41-175">Add hello following code toohello forms.py file, and then save hello file.</span></span>

```python
from flask.ext.wtf import Form
from wtforms import RadioField

class VoteForm(Form):
    deploy_preference  = RadioField('Deployment Preference', choices=[
        ('Web Site', 'Web Site'),
        ('Cloud Service', 'Cloud Service'),
        ('Virtual Machine', 'Virtual Machine')], default='Web Site')
```


### <a name="add-hello-required-imports-tooviewspy"></a><span data-ttu-id="8fb41-176">Lägg till hello som krävs för import tooviews.py</span><span class="sxs-lookup"><span data-stu-id="8fb41-176">Add hello required imports tooviews.py</span></span>
1. <span data-ttu-id="8fb41-177">I Solution Explorer expanderar du hello **kursen** mapp och öppna hello **views.py** fil.</span><span class="sxs-lookup"><span data-stu-id="8fb41-177">In Solution Explorer, expand hello **tutorial** folder, and open hello **views.py** file.</span></span> 
2. <span data-ttu-id="8fb41-178">Lägg till följande importera instruktioner toohello överkant hello hello **views.py** och spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="8fb41-178">Add hello following import statements toohello top of hello **views.py** file, then save hello file.</span></span> <span data-ttu-id="8fb41-179">Dessa importera Cosmos DB s PythonSDK och hello Flask-paket.</span><span class="sxs-lookup"><span data-stu-id="8fb41-179">These import Cosmos DB's PythonSDK and hello Flask packages.</span></span>
   
    ```python
    from forms import VoteForm
    import config
    import pydocumentdb.document_client as document_client
    ```

### <a name="create-database-collection-and-document"></a><span data-ttu-id="8fb41-180">Skapa databas, samling och dokument</span><span class="sxs-lookup"><span data-stu-id="8fb41-180">Create database, collection, and document</span></span>
* <span data-ttu-id="8fb41-181">Fortfarande i **views.py**, lägga till hello efter koden toohello hello-filen.</span><span class="sxs-lookup"><span data-stu-id="8fb41-181">Still in **views.py**, add hello following code toohello end of hello file.</span></span> <span data-ttu-id="8fb41-182">Detta hand tar om skapar hello-databas som används av hello formuläret.</span><span class="sxs-lookup"><span data-stu-id="8fb41-182">This takes care of creating hello database used by hello form.</span></span> <span data-ttu-id="8fb41-183">Ta inte bort hello befintliga koden i **views.py**.</span><span class="sxs-lookup"><span data-stu-id="8fb41-183">Do not delete any of hello existing code in **views.py**.</span></span> <span data-ttu-id="8fb41-184">Lägg bara till denna toohello end.</span><span class="sxs-lookup"><span data-stu-id="8fb41-184">Simply append this toohello end.</span></span>

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


### <a name="read-database-collection-document-and-submit-form"></a><span data-ttu-id="8fb41-185">Läsa databas, samling och dokument samt skicka formulär</span><span class="sxs-lookup"><span data-stu-id="8fb41-185">Read database, collection, document, and submit form</span></span>
* <span data-ttu-id="8fb41-186">Fortfarande i **views.py**, lägga till hello efter koden toohello hello-filen.</span><span class="sxs-lookup"><span data-stu-id="8fb41-186">Still in **views.py**, add hello following code toohello end of hello file.</span></span> <span data-ttu-id="8fb41-187">Detta hand tar om hur du konfigurerar hello formuläret, läsning hello databas, samling och dokument.</span><span class="sxs-lookup"><span data-stu-id="8fb41-187">This takes care of setting up hello form, reading hello database, collection, and document.</span></span> <span data-ttu-id="8fb41-188">Ta inte bort hello befintliga koden i **views.py**.</span><span class="sxs-lookup"><span data-stu-id="8fb41-188">Do not delete any of hello existing code in **views.py**.</span></span> <span data-ttu-id="8fb41-189">Lägg bara till denna toohello end.</span><span class="sxs-lookup"><span data-stu-id="8fb41-189">Simply append this toohello end.</span></span>

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


### <a name="create-hello-html-files"></a><span data-ttu-id="8fb41-190">Skapa hello HTML-filer</span><span class="sxs-lookup"><span data-stu-id="8fb41-190">Create hello HTML files</span></span>
1. <span data-ttu-id="8fb41-191">I Solution Explorer i hello **kursen** mapp, höger Klicka på hello **mallar** mappen, klicka på **Lägg till**, och klicka sedan på **nytt objekt**.</span><span class="sxs-lookup"><span data-stu-id="8fb41-191">In Solution Explorer, in hello **tutorial** folder, right click hello **templates** folder, click **Add**, and then click **New Item**.</span></span> 
2. <span data-ttu-id="8fb41-192">Välj **HTML-sidan**, och klicka sedan på hello namn Skriv **create.html**.</span><span class="sxs-lookup"><span data-stu-id="8fb41-192">Select **HTML Page**, and then in hello name box type **create.html**.</span></span> 
3. <span data-ttu-id="8fb41-193">Upprepa steg 1 och 2 toocreate två ytterligare HTML-filer: results.html och vote.html.</span><span class="sxs-lookup"><span data-stu-id="8fb41-193">Repeat steps 1 and 2 toocreate two additional HTML files: results.html and vote.html.</span></span>
4. <span data-ttu-id="8fb41-194">Lägg till följande kod för hello**create.html** i hello `<body>` element.</span><span class="sxs-lookup"><span data-stu-id="8fb41-194">Add hello following code too**create.html** in hello `<body>` element.</span></span> <span data-ttu-id="8fb41-195">Ett meddelande visas om att vi har skapat en ny databas, en samling och ett dokument.</span><span class="sxs-lookup"><span data-stu-id="8fb41-195">It displays a message stating that we created a new database, collection, and document.</span></span>
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>{{ title }}.</h2>
    <h3>{{ message }}</h3>
    <p><a href="{{ url_for('vote') }}" class="btn btn-primary btn-large">Vote &raquo;</a></p>
    {% endblock %}
    ```
5. <span data-ttu-id="8fb41-196">Lägg till följande kod för hello**results.html** i hello `<body`> element.</span><span class="sxs-lookup"><span data-stu-id="8fb41-196">Add hello following code too**results.html** in hello `<body`> element.</span></span> <span data-ttu-id="8fb41-197">Den visar hello resultatet av omröstningen hello.</span><span class="sxs-lookup"><span data-stu-id="8fb41-197">It displays hello results of hello poll.</span></span>
   
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
6. <span data-ttu-id="8fb41-198">Lägg till följande kod för hello**vote.html** i hello `<body`> element.</span><span class="sxs-lookup"><span data-stu-id="8fb41-198">Add hello following code too**vote.html** in hello `<body`> element.</span></span> <span data-ttu-id="8fb41-199">Den visar hello omröstningen och godkänner hello röster.</span><span class="sxs-lookup"><span data-stu-id="8fb41-199">It displays hello poll and accepts hello votes.</span></span> <span data-ttu-id="8fb41-200">Om hur du registrerar hello röster skickas hello kontroll över tooviews.py där vi identifierar hello rösten cast och bifoga därefter hello dokumentet.</span><span class="sxs-lookup"><span data-stu-id="8fb41-200">On registering hello votes, hello control is passed over tooviews.py where we will recognize hello vote cast and append hello document accordingly.</span></span>
   
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
7. <span data-ttu-id="8fb41-201">I hello **mallar** Ersätt hello innehållet i mappen **index.html** med hello följande.</span><span class="sxs-lookup"><span data-stu-id="8fb41-201">In hello **templates** folder, replace hello contents of **index.html** with hello following.</span></span> <span data-ttu-id="8fb41-202">Detta fungerar som Landningssida för programmet hello.</span><span class="sxs-lookup"><span data-stu-id="8fb41-202">This serves as hello landing page for your application.</span></span>
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>Python + Azure Cosmos DB Voting Application.</h2>
    <h3>This is a sample Cosmos DB voting application using PyDocumentDB</h3>
    <p><a href="{{ url_for('create') }}" class="btn btn-primary btn-large">Create/Clear hello Voting Database &raquo;</a></p>
    <p><a href="{{ url_for('vote') }}" class="btn btn-primary btn-large">Vote &raquo;</a></p>
    {% endblock %}
    ```

### <a name="add-a-configuration-file-and-change-hello-initpy"></a><span data-ttu-id="8fb41-203">Lägga till en konfigurationsfil och ändra hello \_ \_init\_\_.py</span><span class="sxs-lookup"><span data-stu-id="8fb41-203">Add a configuration file and change hello \_\_init\_\_.py</span></span>
1. <span data-ttu-id="8fb41-204">I Solution Explorer högerklickar du på hello **kursen** projektet, klicka på **Lägg till**, klickar du på **nytt objekt**väljer **tom Python-fil**, och sedan namnet hello filen **config.py**.</span><span class="sxs-lookup"><span data-stu-id="8fb41-204">In Solution Explorer, right-click hello **tutorial** project, click **Add**, click **New Item**, select **Empty Python File**, and then name hello file **config.py**.</span></span> <span data-ttu-id="8fb41-205">Den här konfigurationsfilen krävs av formulär i Flask.</span><span class="sxs-lookup"><span data-stu-id="8fb41-205">This config file is required by forms in Flask.</span></span> <span data-ttu-id="8fb41-206">Du kan använda den tooprovide en hemlig nyckel.</span><span class="sxs-lookup"><span data-stu-id="8fb41-206">You can use it tooprovide a secret key as well.</span></span> <span data-ttu-id="8fb41-207">Nyckeln behövs inte i den här självstudien.</span><span class="sxs-lookup"><span data-stu-id="8fb41-207">This key is not needed for this tutorial though.</span></span>
2. <span data-ttu-id="8fb41-208">Lägg till hello följande kod tooconfig.py behöver du tooalter hello värdena för **DOCUMENTDB\_värden** och **DOCUMENTDB\_NYCKELN** i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="8fb41-208">Add hello following code tooconfig.py, you'll need tooalter hello values of **DOCUMENTDB\_HOST** and **DOCUMENTDB\_KEY** in hello next step.</span></span>
   
    ```python
    CSRF_ENABLED = True
    SECRET_KEY = 'you-will-never-guess'
   
    DOCUMENTDB_HOST = 'https://YOUR_DOCUMENTDB_NAME.documents.azure.com:443/'
    DOCUMENTDB_KEY = 'YOUR_SECRET_KEY_ENDING_IN_=='
   
    DOCUMENTDB_DATABASE = 'voting database'
    DOCUMENTDB_COLLECTION = 'voting collection'
    DOCUMENTDB_DOCUMENT = 'voting document'
    ```
3. <span data-ttu-id="8fb41-209">I hello [Azure-portalen](https://portal.azure.com/), navigera toohello **nycklar** bladet genom att klicka på **Bläddra**, **Azure Cosmos DB konton**, dubbelklicka på hello namn för hello toouse-kontot och klicka sedan på hello **nycklar** knapp i hello **Essentials** område.</span><span class="sxs-lookup"><span data-stu-id="8fb41-209">In hello [Azure portal](https://portal.azure.com/), navigate toohello **Keys** blade by clicking **Browse**, **Azure Cosmos DB Accounts**, double-click hello name of hello account toouse, and then click hello **Keys** button in hello **Essentials** area.</span></span> <span data-ttu-id="8fb41-210">I hello **nycklar** bladet, kopiera hello **URI** värdet och klistrar in den i hello **config.py** som hello värde för hello **DOCUMENTDB\_värden**  egenskapen.</span><span class="sxs-lookup"><span data-stu-id="8fb41-210">In hello **Keys** blade, copy hello **URI** value and paste it into hello **config.py** file, as hello value for hello **DOCUMENTDB\_HOST** property.</span></span> 
4. <span data-ttu-id="8fb41-211">Tillbaka i hello Azure-portalen i hello **nycklar** bladet kopiera hello värdet för hello **primärnyckel** eller hello **sekundärnyckeln**, och klistra in den i hello **config.py**  som hello värde för hello **DOCUMENTDB\_NYCKELN** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="8fb41-211">Back in hello Azure portal, in hello **Keys** blade, copy hello value of hello **Primary Key** or hello **Secondary Key**, and paste it into hello **config.py** file, as hello value for hello **DOCUMENTDB\_KEY** property.</span></span>
5. <span data-ttu-id="8fb41-212">I hello  **\_ \_init\_\_.py** lägger du till följande rad hello.</span><span class="sxs-lookup"><span data-stu-id="8fb41-212">In hello **\_\_init\_\_.py** file, add hello following line.</span></span> 
   
        app.config.from_object('config')
   
    <span data-ttu-id="8fb41-213">Så att hello innehållet hello-filen är:</span><span class="sxs-lookup"><span data-stu-id="8fb41-213">So that hello content of hello file is:</span></span>
   
    ```python
    from flask import Flask
    app = Flask(__name__)
    app.config.from_object('config')
    import tutorial.views
    ```
6. <span data-ttu-id="8fb41-214">När du lägger till alla filer som hello Solution Explorer bör se ut så här:</span><span class="sxs-lookup"><span data-stu-id="8fb41-214">After adding all hello files, Solution Explorer should look like this:</span></span>
   
    ![Skärmbild som visar hello Visual Studio Solution Explorer-fönstret](./media/documentdb-python-application/cosmos-db-python-solution-explorer.png)

## <a name="step-4-run-your-web-application-locally"></a><span data-ttu-id="8fb41-216">Steg 4: Kör webbappen lokalt</span><span class="sxs-lookup"><span data-stu-id="8fb41-216">Step 4: Run your web application locally</span></span>
1. <span data-ttu-id="8fb41-217">Skapa hello lösning genom att trycka på **Ctrl**+**SKIFT**+**B**.</span><span class="sxs-lookup"><span data-stu-id="8fb41-217">Build hello solution by pressing **Ctrl**+**Shift**+**B**.</span></span>
2. <span data-ttu-id="8fb41-218">När hello har byggts startar hello webbplats genom att trycka på **F5**.</span><span class="sxs-lookup"><span data-stu-id="8fb41-218">Once hello build succeeds, start hello website by pressing **F5**.</span></span> <span data-ttu-id="8fb41-219">Du bör se följande hello på skärmen.</span><span class="sxs-lookup"><span data-stu-id="8fb41-219">You should see hello following on your screen.</span></span>
   
    ![Skärmbild som visar hello Python + Azure Cosmos DB Röstningsappen visad i en webbläsare](./media/documentdb-python-application/cosmos-db-pythonr-run-application.png)
3. <span data-ttu-id="8fb41-221">Klicka på **skapa/Rensa hello röstning databasen** toogenerate hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="8fb41-221">Click **Create/Clear hello Voting Database** toogenerate hello database.</span></span>
   
    ![Skärmbild som visar hello Skapa sida för hello webbappen – utvecklingsdetaljer](./media/documentdb-python-application/cosmos-db-python-run-create-page.png)
4. <span data-ttu-id="8fb41-223">Klicka sedan på **Rösta** och välj ett alternativ.</span><span class="sxs-lookup"><span data-stu-id="8fb41-223">Then, click **Vote** and select your option.</span></span>
   
    ![Skärmbild som visar hello webbprogrammet med en röstningsfråga](./media/documentdb-python-application/cosmos-db-vote.png)
5. <span data-ttu-id="8fb41-225">För varje röst du lägger ökas hello aktuella räknaren.</span><span class="sxs-lookup"><span data-stu-id="8fb41-225">For every vote you cast, it increments hello appropriate counter.</span></span>
   
    ![Skärmbild som visar hello resultaten av hello sidan röstningsresultat](./media/documentdb-python-application/cosmos-db-voting-results.png)
6. <span data-ttu-id="8fb41-227">Stoppa felsökningen hello projektet genom att trycka på SKIFT + F5.</span><span class="sxs-lookup"><span data-stu-id="8fb41-227">Stop debugging hello project by pressing Shift+F5.</span></span>

## <a name="step-5-deploy-hello-web-application-tooazure"></a><span data-ttu-id="8fb41-228">Steg 5: Distribuera hello web application tooAzure</span><span class="sxs-lookup"><span data-stu-id="8fb41-228">Step 5: Deploy hello web application tooAzure</span></span>
<span data-ttu-id="8fb41-229">Nu när du har hello hela appen fungerar mot Cosmos DB vi toodeploy denna tooAzure.</span><span class="sxs-lookup"><span data-stu-id="8fb41-229">Now that you have hello complete application working correctly against Cosmos DB, we're going toodeploy this tooAzure.</span></span>

1. <span data-ttu-id="8fb41-230">Högerklicka på hello projektet i Solution Explorer (Kontrollera att du inte fortfarande körs lokalt) och välj **publicera**.</span><span class="sxs-lookup"><span data-stu-id="8fb41-230">Right-click hello project in Solution Explorer (make sure you're not still running it locally) and select **Publish**.</span></span>  
   
     ![Skärmbild som visar hello tutorial markerad i Solution Explorer med alternativet för hello publicera markerat](./media/documentdb-python-application/image20.png)
2. <span data-ttu-id="8fb41-232">I hello **publicera** dialogrutan **Microsoft Azure App Service**väljer **Skapa nytt**, och klicka sedan på **publicera**.</span><span class="sxs-lookup"><span data-stu-id="8fb41-232">In hello **Publish** dialog box, select **Microsoft Azure App Service**, select **Create New**, and then click **Publish**.</span></span>
   
    ![Skärmbild som visar hello Publicera webbplats fönstret med Microsoft Azure App Service markerat](./media/documentdb-python-application/cosmos-db-python-publish.png)
3. <span data-ttu-id="8fb41-234">I hello **skapa Apptjänst** dialogrutan Ange hello namn för ditt webbprogram tillsammans med din **prenumeration**, **resursgruppen**, och **App Service-Plan** , klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="8fb41-234">In hello **Create App Service** dialog box, enter hello name for your web app along with your **Subscription**, **Resource Group**, and **App Service Plan**, then click **Create**.</span></span>
   
    ![Skärmbild som visar hello Microsoft Azure Web Apps fönster fönster](./media/documentdb-python-application/cosmos-db-python-create-app-service.png)
4. <span data-ttu-id="8fb41-236">I några sekunder har Visual Studio publicerat din apptjänst och starta en webbläsare där du kan se ditt arbete som körs i Azure!</span><span class="sxs-lookup"><span data-stu-id="8fb41-236">In a few seconds, Visual Studio will finish publishing your app service and launch a browser where you can see your handiwork running in Azure!</span></span>

    ![Skärmbild som visar hello Microsoft Azure Web Apps fönster fönster](./media/documentdb-python-application/cosmos-db-python-appservice-created.png)

## <a name="troubleshooting"></a><span data-ttu-id="8fb41-238">Felsökning</span><span class="sxs-lookup"><span data-stu-id="8fb41-238">Troubleshooting</span></span>
<span data-ttu-id="8fb41-239">Om detta är hello första Python-appen du kör på datorn kontrollerar du att hello följande mappar (eller hello motsvarande installationsplatser) ingår i PATH-variabeln:</span><span class="sxs-lookup"><span data-stu-id="8fb41-239">If this is hello first Python app you've run on your computer, ensure that hello following folders (or hello equivalent installation locations) are included in your PATH variable:</span></span>

    C:\Python27\site-packages;C:\Python27\;C:\Python27\Scripts;

<span data-ttu-id="8fb41-240">Om du får ett felmeddelande på röstningssidan och du gav ditt projekt något annat än **kursen**, se till att  **\_ \_init\_\_.py** referenser hello rätt projektnamn i hello rad: `import tutorial.view`.</span><span class="sxs-lookup"><span data-stu-id="8fb41-240">If you receive an error on your vote page, and you named your project something other than **tutorial**, make sure that **\_\_init\_\_.py** references hello correct project name in hello line: `import tutorial.view`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8fb41-241">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8fb41-241">Next steps</span></span>
<span data-ttu-id="8fb41-242">Grattis!</span><span class="sxs-lookup"><span data-stu-id="8fb41-242">Congratulations!</span></span> <span data-ttu-id="8fb41-243">Du har precis slutfört din första Python-webbapp med Cosmos-DB och publicerat den tooAzure.</span><span class="sxs-lookup"><span data-stu-id="8fb41-243">You have just completed your first Python web application using Cosmos DB and published it tooAzure.</span></span>

<span data-ttu-id="8fb41-244">Vi uppdaterar och förbättrar ofta den här artikeln baserat på dina synpunkter.</span><span class="sxs-lookup"><span data-stu-id="8fb41-244">We update and improve this topic frequently based on your feedback.</span></span>  <span data-ttu-id="8fb41-245">När du har slutfört hello kursen ta med hello röstning knappar på hello upp och längst ned på sidan och vara säker på att tooinclude din feedback på vilka förbättringar som du vill att toosee som gjorts.</span><span class="sxs-lookup"><span data-stu-id="8fb41-245">Once you've completed hello tutorial, please using hello voting buttons at hello top and bottom of this page, and be sure tooinclude your feedback on what improvements you want toosee made.</span></span> <span data-ttu-id="8fb41-246">Om du vill att vi toocontact du direkt, anser ledigt tooinclude den e-postadress i dina kommentarer.</span><span class="sxs-lookup"><span data-stu-id="8fb41-246">If you'd like us toocontact you directly, feel free tooinclude your email address in your comments.</span></span>

<span data-ttu-id="8fb41-247">webbprogrammet för tooadd ytterligare funktioner tooyour, granska hello API: er som är tillgängliga i hello [Azure Cosmos DB Python SDK](documentdb-sdk-python.md).</span><span class="sxs-lookup"><span data-stu-id="8fb41-247">tooadd additional functionality tooyour web application, review hello APIs available in hello [Azure Cosmos DB Python SDK](documentdb-sdk-python.md).</span></span>

<span data-ttu-id="8fb41-248">Mer information om Azure, Visual Studio och Python finns hello [Python Developer Center](https://azure.microsoft.com/develop/python/).</span><span class="sxs-lookup"><span data-stu-id="8fb41-248">For more information about Azure, Visual Studio, and Python, see hello [Python Developer Center](https://azure.microsoft.com/develop/python/).</span></span> 

<span data-ttu-id="8fb41-249">Ytterligare självstudier om Python Flask finns [hello ingående självstudie om Flask, del I: Hello, World!](http://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world).</span><span class="sxs-lookup"><span data-stu-id="8fb41-249">For additional Python Flask tutorials, see [hello Flask Mega-Tutorial, Part I: Hello, World!](http://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world).</span></span> 

[Visual Studio Express]: http://www.visualstudio.com/products/visual-studio-express-vs.aspx
[2]: https://www.python.org/downloads/windows/
[3]: https://www.microsoft.com/download/details.aspx?id=44266
[Microsoft Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
[Azure portal]: http://portal.azure.com
