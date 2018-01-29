---
title: "Azure Machine Learning modellen Management kommandoradsgränssnittet referens | Microsoft Docs"
description: "Azure Machine Learning modellen Management kommandoradsgränssnittet referens."
services: machine-learning
author: raymondl
ms.author: raymondl, aashishb
manager: neerajkh
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 11/08/2017
ms.openlocfilehash: 373abb8f40a8acf557b7cd4a0d0b3fb55f4a545c
ms.sourcegitcommit: 3ee36b8a4115fce8b79dd912486adb7610866a7c
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/15/2017
---
# <a name="model-management-command-line-interface-reference"></a>Referens för modellen management-kommandoradsgränssnittet

## <a name="base-cli-concepts"></a>Grundläggande CLI-begrepp:

    account : Manage model management accounts. 
    env     : Manage compute environments.
    image   : Manage operationalization images.
    manifest: Manage operationalization manifests.
    model   : Manage operationalization models.
    service : Manage operationalized services.

## <a name="account-commands"></a>Kontot kommandon
Ett konto för hantering av modellen krävs för att använda de tjänster som gör att du kan distribuera och hantera modeller. Använd `az ml account modelmanagement -h` att se i följande lista:

    create: Create a Model Management Account.
    delete: Delete a specified Model Management Account.
    list  : Gets the Model Management Accounts in the current subscriptiong.
    set   : Set the active Model Management Account.
    show  : Show a Model Management Account.
    update: Update an existing Model Management Account.

**Skapa modellen Management-konto**

Skapa ett konto för hantering av modellen med följande kommando. Det här kontot används för fakturering.

`az ml account modelmanagement create --location [Azure region e.g. eastus2] --name [new account name] --resource-group [resource group name to store the account in]`

Lokala argument:

    --location -l       [Required]: Resource location.
    --name -n           [Required]: Name of the model management account.
    --resource-group -g [Required]: Resource group to create the model management account in.
    --description -d              : Description of the model management account.
    --sku-instances               : Number of instances of the selected SKU. Must be between 1 and
                                    16 inclusive.  Default: 1.
    --sku-name                    : SKU name. Valid names are S1|S2|S3|DevTest.  Default: S1.
    --tags -t                     : Tags for the model management account.  Default: {}.
    -v                            : Verbosity flag.

## <a name="environment-commands"></a>Miljö kommandon

    cluster        : Switch the current execution context to 'cluster'.
    delete         : Delete an MLCRP-provisioned resource.
    get-credentials: List the keys for an environment.
    list           : List all environments in the current subscription.
    local          : Switch the current execution context to 'local'.
    set            : Set the active MLC environment.
    setup          : Sets up an MLC environment.
    show           : Show an MLC resource; if resource_group or cluster_name are not provided, shows
                     the active MLC env.

**Konfigurera en miljö för distribution**

Installationskommandot måste du ha deltagare åtkomst till prenumerationen. Om du inte har som behöver du minst deltagare åtkomst till den resursgrupp som du distribuerar till. Om du vill göra det senare, måste du ange resursgruppens namn som en del av installationen kommando med `-g` flaggan. 

Det finns två alternativ för distributionen: *lokala* och *klustret*. Ange den `--cluster` (eller `-c`) flaggan gör Klusterdistribution som etablerar en ACS-kluster. Grundläggande inställningar syntax är följande:

`az ml env setup [-c] --location [location of environment resources] --name[name of environment]`

Detta initierar din Azure maskininlärning miljö med en storage-konto och ACR registret App Insights-tjänsten som skapats i din prenumeration. Som standard har i miljön initierats för lokala distributioner endast (ingen ACS) om någon flagga har angetts. Om du behöver skala tjänst anger du den `--cluster` (eller `-c`) flagga för att skapa en ACS-kluster.

Information om kommandot:

    --location -l        [Required]: Location for environment resources; an Azure region, e.g. eastus2.
    --name -n            [Required]: Name of environment to provision.
    --acr -r                       : ARM ID of ACR to associate with this environment.
    --agent-count -z               : Number of agents to provision in the ACS cluster. Default: 3.
    --cert-cname                   : CNAME of certificate.
    --cert-pem                     : Path to .pem file with certificate bytes.
    --cluster -c                   : Flag to provision ACS cluster. Off by default; specify this to force an ACS cluster deployment.
    --key-pem                      : Path to .pem file with certificate key.
    --master-count -m              : Number of master nodes to provision in the ACS cluster. Acceptable values: 1, 3, 5. Default: 1.
    --resource-group -g            : Resource group in which to create compute resource. Will be created if it does not exist.
                                     If not provided, resource group will be created with 'rg' appended to 'name.'.
    --service-principal-app-id -a  : App ID of service principal to use for configuring ML compute.
    --service-principal-password -p: Password associated with service principal.
    --storage -s                   : ARM ID of storage account to associate with this environment.
    --yes -y                       : Flag to answer 'yes' to any prompts. Command will fail if user is not logged in.

Globala argument
```
    --debug                        : Increase logging verbosity to show all debug logs.
    --help -h                      : Show this help message and exit.
    --output -o                    : Output format.  Allowed values: json, jsonc, table, tsv. Default: json.
    --query                        : JMESPath query string. See http://jmespath.org/ for more information and examples.
    --verbose                      : Increase logging verbosity. Use --debug for full debug logs.
```
## <a name="model-commands"></a>Modell-kommandon

    list
    register
    show

**Registrera en modell**

Kommandot för att registrera modellen.

`az ml model register --model [path to model file] --name [model name]`

Information om kommandot:

    --model -m [Required]: Model to register.
    --name -n  [Required]: Name of model to register.
    --description -d     : Description of the model.
    --tag -t             : Tags for the model. Multiple tags can be specified with additional -t arguments.
    -v                   : Verbosity flag.

Globala argument

    --debug              : Increase logging verbosity to show all debug logs.
    --help -h            : Show this help message and exit.
    --output -o          : Output format.  Allowed values: json, jsonc, table, tsv.  Default: json.
    --query              : JMESPath query string. See http://jmespath.org/ for more information and
                           examples.
    --verbose            : Increase logging verbosity. Use --debug for full debug logs.

## <a name="manifest-commands"></a>Manifestet kommandon

    create: Create an Operationalization Manifest. This command has two different
            sets of required arguments, depending on if you want to use previously registered
            model/s.
    list
    show

**Skapa manifestet**

Skapar en manifestfil för modellen. 

`az ml manifest create --manifest-name [your new manifest name] -f [path to code file] -r [runtime for the image, e.g. spark-py]`

Information om kommandot:

    --manifest-name -n [Required]: Name of the manifest to create.
    -f                 [Required]: The code file to be deployed.
    -r                 [Required]: Runtime of the web service. Valid runtimes are spark-py|python.
    --conda-file -c              : Path to Conda Environment file.
    --dependency -d              : Files and directories required by the service. Multiple
                                   dependencies can be specified with additional -d arguments.
    --manifest-description       : Description of the manifest.
    --schema-file -s             : Schema file to add to the manifest.
    -p                           : A pip requirements.txt file needed by the code file.
    -v                           : Verbosity flag.

Registrerad modell argument

    --model-id -i                : [Required] Id of previously registered model to add to manifest.
                                   Multiple models can be specified with additional -i arguments.

Avregistrera modellen argument

    --model-file -m              : [Required] Model file to register. If used, must be combined with
                                   model name.

Globala argument

    --debug                      : Increase logging verbosity to show all debug logs.
    --help -h                    : Show this help message and exit.
    --output -o                  : Output format.  Allowed values: json, jsonc, table, tsv.
                                   Default: json.
    --query                      : JMESPath query string. See http://jmespath.org/ for more
                                   information and examples.
    --verbose                    : Increase logging verbosity. Use --debug for full debug logs.


## <a name="image-commands"></a>Bildkommandon

    create: Creates a docker image with the model and its dependencies. This command has two different sets of
            required arguments, depending on if you want to use a previously created manifest.
    list
    show
    usage

**Skapa avbildning**

Du kan skapa en avbildning med alternativet för att ha skapat manifestet innan. 

`az ml image create -n [image name] --manifest-id [the manifest ID]`

Men du kan skapa manifestet och bild med ett enda kommando. 

`az ml image create -n [image name] --model-file [model file or folder path] -f [code file, e.g. the score.py file] -r [the runtime eg.g. spark-py which is the Docker container image base]`

Information om kommandot:

    --image-name -n [Required]: The name of the image being created.
    --image-description       : Description of the image.
    --image-type              : The image type to create. Defaults to "Docker".
    -v                        : Verbosity flag.

Registrerade manifestet argument

    --manifest-id             : [Required] Id of previously registered manifest to use in image creation.

Avregistrera manifestet argument

    --conda-file -c           : Path to Conda Environment file.
    --dependency -d           : Files and directories required by the service. Multiple dependencies can
                                be specified with additional -d arguments.
    --model-file -m           : [Required] Model file to register.
    --schema-file -s          : Schema file to add to the manifest.
    -f                        : [Required] The code file to be deployed.
    -p                        : A pip requirements.txt file needed by the code file.
    -r                        : [Required] Runtime of the web service. Valid runtimes are python|spark-py.


## <a name="service-commands"></a>Tjänsten kommandon
Följande kommandon stöds för tjänsten. Använd alternativet -h om du vill visa parametrar för varje kommando. Till exempel använda `az ml service create realtime -h` att se Skapa information om kommandot.

    create
    delete
    keys
    list
    logs
    run
    show
    update
    usage

**Skapa en tjänst**

Om du vill skapa en tjänst med en tidigare skapad avbildning, använder du följande kommando:

`az ml service create realtime --image-id [image to deploy] -n [service name]`

Om du vill skapa en tjänst använder manifestet och bild med ett enda kommando du följande kommando:

`az ml service create realtime --model-file [path to model file(s)] -f [path to model scoring file, e.g. score.py] -n [service name] -r [run time included in the image, e.g. spark-py]`

Information om kommandon:

    -n                                : [Required] Webservice name.
    --autoscale-enabled               : Enable automatic scaling of service replicas based on request demand.
                                        Allowed values: true, false. False if omitted.  Default: false.
    --autoscale-max-replicas          : If autoscale is enabled - sets the maximum number of replicas.
    --autoscale-min-replicas          : If autoscale is enabled - sets the minimum number of replicas.
    --autoscale-refresh-period-seconds: If autoscale is enabled - the interval of evaluating scaling demand.
    --autoscale-target-utilization    : If autoscale is enabled - target utilization of replicas time.
    --collect-model-data              : Enable model data collection. Allowed values: true, false. False if omitted.  Default: false.
    --cpu                             : Reserved number of CPU cores per service replica (can be fraction).
    --enable-app-insights -l          : Enable app insights. Allowed values: true, false. False if omitted.  Default: false.
    --memory                          : Reserved amount of memory per service replica, in M or G. (ex. 1G, 300M).
    --replica-max-concurrent-requests : Maximum number of concurrent requests that can be routed to a service replica.
    -v                                : Verbosity flag.
    -z                                : Number of replicas for a Kubernetes service.  Default: 1.

Registrerade avbildningen argument

    --image-id                        : [Required] Image to deploy to the service.

Avregistrera avbildningen argument

    --conda-file -c                   : Path to Conda Environment file.
    --image-type                      : The image type to create. Defaults to "Docker".
    --model-file -m                   : [Required] The model to be deployed.
    -d                                : Files and directories required by the service. Multiple dependencies can be specified
                                        with additional -d arguments.
    -f                                : [Required] The code file to be deployed.
    -p                                : A pip requirements.txt file of package needed by the code file.
    -r                                : [Required] Runtime of the web service. Valid runtimes are python|spark-py.
    -s                                : Input and output schema of the web service.

Globala argument

    --debug                           : Increase logging verbosity to show all debug logs.
    --help -h                         : Show this help message and exit.
    --output -o                       : Output format.  Allowed values: json, jsonc, table, tsv. Default: json.
    --query                           : JMESPath query string. See http://jmespath.org/ for more information and examples.
    --verbose                         : Increase logging verbosity. Use --debug for full debug logs.


Tänk på den `-d` flagga för att bifoga beroenden: Om du skickar namnet på en katalog som inte redan är paketerade (zip, tar osv.), katalogen automatiskt hämtar tar'ed och skickas tillsammans sedan automatiskt avskiljs maskinvara på andra sidan. 

Om du skickar i en katalog som redan är paketerade vi behandla det som en fil och överför den längs är. Det är inte separata automatiskt. du förväntas hantera som i koden.

**Hämta information om tjänst**

Hämta information om tjänsten inklusive URL, användning (inklusive exempeldata om ett schema har skapats).

`az ml service show realtime --name [service name]`

Information om kommandot:

    --id -i    : The service id to show.
    --name -n  : Webservice name.
    -v         : Verbosity flag.

Globala argument

    --debug    : Increase logging verbosity to show all debug logs.
    --help -h  : Show this help message and exit.
    --output -o: Output format.  Allowed values: json, jsonc, table, tsv.  Default: json.
    --query    : JMESPath query string. See http://jmespath.org/ for more information and examples.
    --verbose  : Increase logging verbosity. Use --debug for full debug logs.

**Tjänsten körs**

`az ml service run realtime -n [service name] -d [input_data]`

Information om kommandot:

    --id -i    : The service id to show.
    --name -n  : Webservice name.
    -v         : Verbosity flag.

Globala argument

    --debug    : Increase logging verbosity to show all debug logs.
    --help -h  : Show this help message and exit.
    --output -o: Output format.  Allowed values: json, jsonc, table, tsv.  Default: json.
    --query    : JMESPath query string. See http://jmespath.org/ for more information and examples.
    --verbose  : Increase logging verbosity. Use --debug for full debug logs.

Kommando

    az ml service run realtime

Argument--id -i: [krävs] tjänst-id att poängsätta mot.
-d: data som ska användas för att anropa webbtjänsten.
-v: detaljnivå flaggan.

Globala argument

    --debug    : Increase logging verbosity to show all debug logs.
    --help -h  : Show this help message and exit.
    --output -o: Output format.  Allowed values: json, jsonc, table, tsv. Default: json.
    --query    : JMESPath query string. See http://jmespath.org/ for more information and examples.
    --verbose  : Increase logging verbosity. Use --debug for full debug logs.
