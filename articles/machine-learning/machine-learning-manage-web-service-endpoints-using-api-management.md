---
title: "aaaLearn hur toomanage AzureML web services med hjälp av API-hantering | Microsoft Docs"
description: "En guide som visar hur toomanage AzureML web services med hjälp av API-hantering."
keywords: Machine learning api-hantering
services: machine-learning
documentationcenter: 
author: roalexan
manager: jhubbard
editor: 
ms.assetid: 05150ae1-5b6a-4d25-ac67-fb2f24a68e8d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/19/2017
ms.author: roalexan
ms.openlocfilehash: 6e764fbfd71be6cc908a1c8d3d8889969fc651a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="learn-how-toomanage-azureml-web-services-using-api-management"></a>Lär dig hur toomanage AzureML web services med hjälp av API-hantering
## <a name="overview"></a>Översikt
Den här guiden visar hur tooquickly komma igång med API Management toomanage AzureML-webbtjänster.

## <a name="what-is-azure-api-management"></a>Vad är Azure API Management?
Azure API Management är en Azure-tjänst som låter dig hantera REST API-slutpunkter genom att definiera användaråtkomst, begränsning och instrumentpanelen för övervakning. Klicka på [här](https://azure.microsoft.com/services/api-management/) information om Azure API Management. Klicka på [här](../api-management/api-management-get-started.md) för en översikt över hur tooget igång med Azure API Management. Den här andra guiden som den här guiden bygger på omfattar flera ämnen, t.ex. meddelande konfigurationer, nivå prissättning, svar hantering, autentisering av användare, skapa produkter, developer prenumerationer och användning dashboarding.

## <a name="what-is-azureml"></a>Vad är AzureML?
AzureML är en Azure-tjänst för machine learning som gör att du tooeasily skapa, distribuera och dela avancerade Analyslösningar. Klicka på [här](https://azure.microsoft.com/services/machine-learning/) information om AzureML.

## <a name="prerequisites"></a>Krav
toocomplete den här guiden behöver du:

* Ett Azure-konto. Om du inte har ett Azure-konto, klickar du på [här](https://azure.microsoft.com/pricing/free-trial/) mer information om hur toocreate ett kostnadsfritt utvärderingskonto.
* AzureML-konto. Om du inte har en AzureML-konto, klickar du på [här](https://studio.azureml.net/) mer information om hur toocreate ett kostnadsfritt utvärderingskonto.
* hello arbetsytan, tjänst och api_key för en AzureML-experiment som distribueras som en webbtjänst. Klicka på [här](machine-learning-create-experiment.md) för information om hur toocreate en AzureML experiment. Klicka på [här](machine-learning-publish-a-machine-learning-web-service.md) för information om hur toodeploy en AzureML experiment som en webbtjänst. Bilaga A har också instruktioner för hur toocreate och testa en enkel AzureML experimentera och distribuera det som en webbtjänst.

## <a name="create-an-api-management-instance"></a>Skapa en API Management-instans
Nedan finns hello steg för att använda API Management toomanage AzureML webbtjänsten. Först skapa en instans av tjänsten. Logga in toohello [klassiska portalen](https://manage.windowsazure.com/) och på **ny** > **Apptjänster** > **API Management**  >  **Skapa**.

![Skapa instans](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-instance.png)

Ange ett unikt **URL**. Den här guiden använder **demoazureml** – du behöver toochoose någonting annat. Välj önskad hello **prenumeration** och **Region** för service-instans. När du har gjort dina val klickar du på knappen för nästa hello.

![skapa-tjänsten-1](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-service-1.png)

Ange ett värde för hello **organisationsnamn**. Den här guiden använder **demoazureml** – du behöver toochoose någonting annat. Ange din e-postadress i hello **administratör e-post** fältet. E-postadressen används för meddelanden från hello API Management-systemet.

![skapa-tjänsten-2](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-service-2.png)

Klicka på hello kryssrutan toocreate service-instans. *Den tar toothirty minuter för en ny service toobe skapade*.

## <a name="create-hello-api"></a>Skapa hello-API
När du har skapat hello tjänstinstans är hello nästa steg toocreate hello API. Ett API består av en uppsättning åtgärder som kan anropas från ett klientprogram. API: et är via proxy tooexisting webbtjänster. Den här guiden skapar API: er som proxy toohello befintliga AzureML RRS och BES webbtjänster.

API: er skapas och konfigureras från hello API publisher-portalen som öppnas via hello klassiska Azure-portalen. tooreach hello publisher portal, väljer din service-instans.

![Välj tjänstinstans](./media/machine-learning-manage-web-service-endpoints-using-api-management/select-service-instance.png)

Klicka på **hantera** i hello klassiska Azure-portalen för API Management-tjänsten.

![hantera service](./media/machine-learning-manage-web-service-endpoints-using-api-management/manage-service.png)

Klicka på **API: er** från hello **API Management** menyn på hello vänster och klicka sedan på **lägga till API**.

![management-API-menyn](./media/machine-learning-manage-web-service-endpoints-using-api-management/api-management-menu.png)

Typen **AzureML Demo API** som hello **Web API-namnet**. Typen **https://ussouthcentral.services.azureml.net** som hello **-webbtjänstens URL**. Typen **azureml-demo** som hello **URL för Web API-suffix**. Kontrollera **HTTPS** som hello **URL för Web API** schema. Välj **Starter** som **produkter**. När du är klar klickar du på **spara** toocreate hello API.

![Lägg till-nya-api](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-new-api.png)

## <a name="add-hello-operations"></a>Lägga till hello åtgärder
Klicka på **lägga till åtgärden** tooadd operations toothis API.

![lägga till åtgärden](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-operation.png)

Hej **nya åtgärden** fönster visas och hello **signatur** fliken väljs som standard.

## <a name="add-rrs-operation"></a>Lägg till RR-åtgärd
Först skapa en åtgärd för hello AzureML RRS-tjänsten. Välj **POST** som hello **HTTP-verbet**. Typen **/workspaces/ {arbetsytan} /services/ {tjänsten} / execute? api-version = {apiversion} & information = {information}** som hello **URL mallen**. Typen **Resursposter köra** som hello **visningsnamn**.

![Lägg till RR-åtgärden-signaturer](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-rrs-operation-signature.png)

Klicka på **svar** > **lägga till** på hello vänster och välj **200 OK**. Klicka på **spara** toosave den här åtgärden.

![Lägg till-RR-åtgärden-svar](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-rrs-operation-response.png)

## <a name="add-bes-operations"></a>Lägga till BES-åtgärder
Skärmbilder anges inte för hello BES åtgärder som de är mycket lik toothose för att lägga till hello Resursposter igen.

### <a name="submit-but-not-start-a-batch-execution-job"></a>Skicka (men inte startar) ett Batch Execution-jobb
Klicka på **lägga till åtgärden** tooadd hello AzureML BES åtgärden toohello API. Välj **POST** för hello **HTTP-verbet**. Typen **/workspaces/ {arbetsytan} /services/ {tjänsten} / jobb? api-version = {apiversion}** för hello **URL mallen**. Typen **BES skicka** för hello **visningsnamn**. Klicka på **svar** > **lägga till** på hello vänster och välj **200 OK**. Klicka på **spara** toosave den här åtgärden.

### <a name="start-a-batch-execution-job"></a>Starta ett jobb i Batch Execution
Klicka på **lägga till åtgärden** tooadd hello AzureML BES åtgärden toohello API. Välj **POST** för hello **HTTP-verbet**. Typen **/workspaces/ {arbetsytan} /services/ {tjänsten} /jobs/ {jobid} / start? api-version = {apiversion}** för hello **URL mallen**. Typen **BES starta** för hello **visningsnamn**. Klicka på **svar** > **lägga till** på hello vänster och välj **200 OK**. Klicka på **spara** toosave den här åtgärden.

### <a name="get-hello-status-or-result-of-a-batch-execution-job"></a>Hämta hello status eller resultatet av ett Batch Execution-jobb
Klicka på **lägga till åtgärden** tooadd hello AzureML BES åtgärden toohello API. Välj **hämta** för hello **HTTP-verbet**. Typen **/workspaces/ {arbetsytan} /services/ {tjänsten} /jobs/ {jobid}? api-version = {apiversion}** för hello **URL mallen**. Typen **BES Status** för hello **visningsnamn**. Klicka på **svar** > **lägga till** på hello vänster och välj **200 OK**. Klicka på **spara** toosave den här åtgärden.

### <a name="delete-a-batch-execution-job"></a>Ta bort en Batch Execution-jobb
Klicka på **lägga till åtgärden** tooadd hello AzureML BES åtgärden toohello API. Välj **ta bort** för hello **HTTP-verbet**. Typen **/workspaces/ {arbetsytan} /services/ {tjänsten} /jobs/ {jobid}? api-version = {apiversion}** för hello **URL mallen**. Typen **BES ta bort** för hello **visningsnamn**. Klicka på **svar** > **lägga till** på hello vänster och välj **200 OK**. Klicka på **spara** toosave den här åtgärden.

## <a name="call-an-operation-from-hello-developer-portal"></a>Anropa en åtgärd från hello Developer-portalen
Åtgärder kan anropas direkt från hello Developer-portalen, vilket ger ett bekvämt sätt tooview och testa hello driften av ett API. I den här guiden steg du anropar hello **Resursposter köra** metod som har lagts till toohello **AzureML Demo API**. Klicka på **Developer-portalen** hello-menyn på hello uppifrån höger om hello klassiska portalen.

![Developer-portalen](./media/machine-learning-manage-web-service-endpoints-using-api-management/developer-portal.png)

Klicka på **API: er** hello översta menyn och klicka sedan på **AzureML Demo API** toosee hello-åtgärder som är tillgängliga.

![demoazureml-api](./media/machine-learning-manage-web-service-endpoints-using-api-management/demoazureml-api.png)

Välj **Resursposter köra** för hello åtgärden. Klicka på **prova**.

![Försök it](./media/machine-learning-manage-web-service-endpoints-using-api-management/try-it.png)

De begäranparametrarna, Skriv din **arbetsytan**, **service**, **2.0** för hello **apiversion**, och **true**för hello **information**. Du kan hitta din **arbetsytan** och **service** hello AzureML web service instrumentpanelen (finns **testa hello webbtjänsten** i bilaga A).

Huvuden för begäran, klickar du på **Lägg till sidhuvud** och skriv **Content-Type** och **application/json**, klicka på **Lägg till sidhuvud** och skriv **auktorisering** och **ägar <YOUR AZUREML SERVICE API-KEY>** . Du hittar din **api-nyckel** hello AzureML web service instrumentpanelen (se **testa hello-webbtjänsten** i bilaga A).

Typen **{”indata”: {”input1”: {”ColumnNames”: [”Col2”], ”värden”: [[”det här är en bra dag”]]}}, ”GlobalParameters”: {}}** för hello frågans brödtext.

![azureml-demo-api](./media/machine-learning-manage-web-service-endpoints-using-api-management/azureml-demo-api.png)

Klicka på **skicka**.

![Skicka](./media/machine-learning-manage-web-service-endpoints-using-api-management/send.png)

När en åtgärd har anropats hello developer-portalen visar hello **begärda URL: en** från hello backend-tjänst hello **svarsstatusen**, hello **svarshuvuden**, och alla **svar innehåll**.

![Response-status](./media/machine-learning-manage-web-service-endpoints-using-api-management/response-status.png)

## <a name="appendix-a---creating-and-testing-a-simple-azureml-web-service"></a>Bilaga A – skapa och testa en enkel AzureML webbtjänsten
### <a name="creating-hello-experiment"></a>Skapa hello experiment
Nedan finns hello steg för att skapa ett enkelt experiment AzureML och distribuera den som en webbtjänst. Hej web service tar som indata för en kolumn med valfri text och returnerar en uppsättning funktioner som visas i form av heltal. Exempel:

| Text | Hashformaterats Text |
| --- | --- |
| Det här är en bra dag |1 1 2 2 0 2 0 1 |

Börja med en webbläsare, navigera till: [https://studio.azureml.net/](https://studio.azureml.net/) och ange dina autentiseringsuppgifter toolog i. Skapa sedan ett nytt tomt experiment.

![Sök-experiment-mallar](./media/machine-learning-manage-web-service-endpoints-using-api-management/search-experiment-templates.png)

Byt namn på den för**SimpleFeatureHashingExperiment**. Expandera **sparade datauppsättningar** och dra **Book granskningar från Amazon** på experimentet.

![enkel-funktionen-hash-experiment](./media/machine-learning-manage-web-service-endpoints-using-api-management/simple-feature-hashing-experiment.png)

Expandera **Dataomvandling** och **manipulering** och dra **Välj kolumner i datauppsättning** på experimentet. Ansluta **Book granskningar från Amazon** för**Välj kolumner i datauppsättning**.

![select-columns](./media/machine-learning-manage-web-service-endpoints-using-api-management/project-columns.png)

Klicka på **Välj kolumner i datauppsättning** och klicka sedan på **starta kolumnväljaren** och välj **Col2**. Klicka på hello markering tooapply ändringarna.

![select-columns](./media/machine-learning-manage-web-service-endpoints-using-api-management/select-columns.png)

Expandera **textanalys** och dra **funktions-hashning** till hello experiment. Ansluta **Välj kolumner i datauppsättning** för**hash-funktionen**.

![ansluta projektkolumner](./media/machine-learning-manage-web-service-endpoints-using-api-management/connect-project-columns.png)

Typen **3** för hello **Hashing bitsize**. Detta skapar 8 (23) kolumner.

![hash-bitsize](./media/machine-learning-manage-web-service-endpoints-using-api-management/hashing-bitsize.png)

Nu vill du kanske tooclick **kör** tootest hello experiment.

![Kör](./media/machine-learning-manage-web-service-endpoints-using-api-management/run.png)

### <a name="create-a-web-service"></a>Skapa en webbtjänst
Nu skapa en webbtjänst. Expandera **webbtjänsten** och dra **indata** på experimentet. Ansluta **indata** för**hash-funktionen**. Också dra **utdata** på experimentet. Ansluta **utdata** för**hash-funktionen**.

![utdata-till-funktionen för-hashing](./media/machine-learning-manage-web-service-endpoints-using-api-management/output-to-feature-hashing.png)

Klicka på **publicerar webbtjänst**.

![Publicera webbtjänst](./media/machine-learning-manage-web-service-endpoints-using-api-management/publish-web-service.png)

Klicka på **Ja** toopublish hello experiment.

![Ja för att publicera](./media/machine-learning-manage-web-service-endpoints-using-api-management/yes-to-publish.png)

### <a name="test-hello-web-service"></a>Testa hello-webbtjänst
En AzureML-webbtjänst består av RSS-(begäranden/svar-tjänsten) och slutpunkter för BES (batch execution service). RSS är för synkron körning. BES är asynkron jobbkörningen. tootest webbtjänst med hello Python-exempelkälla nedan, du kan behöva toodownload och installera hello Azure SDK för Python (se: [hur tooinstall Python](../python-how-to-install.md)).

Du måste också hello **arbetsytan**, **service**, och **api_key** av experimentet för hello-exempelkälla nedan. Du kan hitta hello arbetsytan och tjänsten genom att klicka på antingen **frågor och svar** eller **Batch Execution** för experimentet hello web service instrumentpanelen.

![Sök-arbetsytan-och-tjänst](./media/machine-learning-manage-web-service-endpoints-using-api-management/find-workspace-and-service.png)

Du kan hitta hello **api_key** genom att klicka på experimentet hello web service instrumentpanelen.

![Sök-api-nyckel](./media/machine-learning-manage-web-service-endpoints-using-api-management/find-api-key.png)

#### <a name="test-rrs-endpoint"></a>Testa Resursposter slutpunkt
##### <a name="test-button"></a>Knappen Testa
Ett enkelt sätt tootest hello RR-slutpunkten är tooclick **Test** på instrumentpanelen för hello web service.

![Test](./media/machine-learning-manage-web-service-endpoints-using-api-management/test.png)

Typen **detta är en bra dag** för **col2**. Klicka på hello markering.

![Ange data](./media/machine-learning-manage-web-service-endpoints-using-api-management/enter-data.png)

Du ser något som liknar

![exempel på-utdata](./media/machine-learning-manage-web-service-endpoints-using-api-management/sample-output.png)

##### <a name="sample-code"></a>Exempelkod
Ett annat sätt tootest din RRS är från din klientkod. Om du klickar på **frågor och svar** på hello instrumentpanelen och rulla toohello längst ned i visas exempelkod för C#, Python och R. Du kan även se hello syntax för hello Resursposter förfrågan, inklusive hello Begärd URI, rubriker och brödtext.

Den här guiden visar en fungerande Python exempel. Du behöver toomodify med hello **arbetsytan**, **service**, och **api_key** av experimentet.

    import urllib2
    import json
    workspace = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE WORKSPACE ID>"
    service = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE SERVICE ID>"
    api_key = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE API KEY>"
    data = {
    "Inputs": {
        "input1": {
            "ColumnNames": ["Col2"],
            "Values": [ [ "This is a good day" ] ]
        },
    },
    "GlobalParameters": { }
    }
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace + "/services/" + service + "/execute?api-version=2.0&details=true"
    headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ api_key)}
    body = str.encode(json.dumps(data))
    try:
        req = urllib2.Request(url, body, headers)
        response = urllib2.urlopen(req)
        result = response.read()
        print "result:" + result
            except urllib2.HTTPError, error:
        print("hello request failed with status code: " + str(error.code))
        print(error.info())
        print(json.loads(error.read()))

#### <a name="test-bes-endpoint"></a>Testa BES slutpunkt
Klicka på **Batch-körningen** hello instrumentpanelen och rulla toohello längst ned. Du kommer att se exempelkod för C#, Python och R. Du kan även se hello syntaxen för hello BES begäranden toosubmit ett jobb, starta ett jobb, hämta hello status eller resultatet av ett jobb och ta bort ett jobb.

Den här guiden visar en fungerande Python exempel. Du behöver toomodify med hello **arbetsytan**, **service**, och **api_key** av experimentet. Dessutom måste toomodify hello **lagringskontonamnet**, **lagringskontonyckel**, och **namnet på lagringsbehållaren**. Slutligen måste toomodify hello platsen för hello **indatafilen** och hello plats för hello **utdatafilen**.

    import urllib2
    import json
    import time
    from azure.storage import *
    workspace = "<REPLACE WITH YOUR WORKSPACE ID>"
    service = "<REPLACE WITH YOUR SERVICE ID>"
    api_key = "<REPLACE WITH hello API KEY FOR YOUR WEB SERVICE>"
    storage_account_name = "<REPLACE WITH YOUR AZURE STORAGE ACCOUNT NAME>"
    storage_account_key = "<REPLACE WITH YOUR AZURE STORAGE KEY>"
    storage_container_name = "<REPLACE WITH YOUR AZURE STORAGE CONTAINER NAME>"
    input_file = "<REPLACE WITH hello LOCATION OF YOUR INPUT FILE>" # Example: C:\\mydata.csv
    output_file = "<REPLACE WITH hello LOCATION OF YOUR OUTPUT FILE>" # Example: C:\\myresults.csv
    input_blob_name = "mydatablob.csv"
    output_blob_name = "myresultsblob.csv"
    def printHttpError(httpError):
    print("hello request failed with status code: " + str(httpError.code))
    print(httpError.info())
    print(json.loads(httpError.read()))
    return
    def saveBlobToFile(blobUrl, resultsLabel):
    print("Reading hello result from " + blobUrl)
    try:
        response = urllib2.urlopen(blobUrl)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    with open(output_file, "w+") as f:
        f.write(response.read())
    print(resultsLabel + " have been written toohello file " + output_file)
    return
    def processResults(result):
    first = True
    results = result["Results"]
    for outputName in results:
        result_blob_location = results[outputName]
        sas_token = result_blob_location["SasBlobToken"]
        base_url = result_blob_location["BaseLocation"]
        relative_url = result_blob_location["RelativeLocation"]
        print("hello results for " + outputName + " are available at hello following Azure Storage location:")
        print("BaseLocation: " + base_url)
        print("RelativeLocation: " + relative_url)
        print("SasBlobToken: " + sas_token)
        if (first):
            first = False
            url3 = base_url + relative_url + sas_token
            saveBlobToFile(url3, "hello results for " + outputName)
    return

    def invokeBatchExecutionService():
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace +"/services/" + service +"/jobs"
    blob_service = BlobService(account_name=storage_account_name, account_key=storage_account_key)
    print("Uploading hello input tooblob storage...")
    data_to_upload = open(input_file, "r").read()
    blob_service.put_blob(storage_container_name, input_blob_name, data_to_upload, x_ms_blob_type="BlockBlob")
    print "Uploaded hello input tooblob storage"
    input_blob_path = "/" + storage_container_name + "/" + input_blob_name
    connection_string = "DefaultEndpointsProtocol=https;AccountName=" + storage_account_name + ";AccountKey=" + storage_account_key
    payload =  {
        "Input": {
            "ConnectionString": connection_string,
            "RelativeLocation": input_blob_path
        },
        "Outputs": {
            "output1": { "ConnectionString": connection_string, "RelativeLocation": "/" + storage_container_name + "/" + output_blob_name },
        },
        "GlobalParameters": {
        }
    }
        body = str.encode(json.dumps(payload))
    headers = { "Content-Type":"application/json", "Authorization":("Bearer " + api_key)}
    print("Submitting hello job...")
    # submit hello job
    req = urllib2.Request(url + "?api-version=2.0", body, headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    result = response.read()
    job_id = result[1:-1] # remove hello enclosing double-quotes
    print("Job ID: " + job_id)
    # start hello job
    print("Starting hello job...")
    req = urllib2.Request(url + "/" + job_id + "/start?api-version=2.0", "", headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    url2 = url + "/" + job_id + "?api-version=2.0"

    while True:
        print("Checking hello job status...")
        # If you are using Python 3+, replace urllib2 with urllib.request in hello follwing code
        req = urllib2.Request(url2, headers = { "Authorization":("Bearer " + api_key) })
        try:
            response = urllib2.urlopen(req)
        except urllib2.HTTPError, error:
            printHttpError(error)
            return
        result = json.loads(response.read())
        status = result["StatusCode"]
        print "status:" + status
        if (status == 0 or status == "NotStarted"):
            print("Job " + job_id + " not yet started...")
        elif (status == 1 or status == "Running"):
            print("Job " + job_id + " running...")
        elif (status == 2 or status == "Failed"):
            print("Job " + job_id + " failed!")
            print("Error details: " + result["Details"])
            break
        elif (status == 3 or status == "Cancelled"):
            print("Job " + job_id + " cancelled!")
            break
        elif (status == 4 or status == "Finished"):
            print("Job " + job_id + " finished!")
            processResults(result)
            break
        time.sleep(1) # wait one second
    return
    invokeBatchExecutionService()
