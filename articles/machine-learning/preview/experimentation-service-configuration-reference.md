---
title: Azure Machine Learning experiment Service configuration-filer
description: "Det här dokumentet beskriver konfigurationsinställningarna för Azure ML experiment Service."
services: machine-learning
author: gokhanuluderya-msft
ms.author: gokhanu
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/28/2017
ms.openlocfilehash: 16c72f8c22307a124fdb670aabca771084c0d1ec
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/05/2017
---
# <a name="azure-machine-learning-experimentation-service-configuration-files"></a>Azure Machine Learning experiment Service configuration-filer

När du kör ett skript i Azure Machine Learning (Azure ML) arbetsstationen beteendet för körningen styrs av filer i den **aml_config** mapp. Denna mapp är under mappen projektroten. Det är viktigt att förstå innehållet i filerna för att uppnå önskat utfall för din körning optimalt.

Följande är relevanta filer under den här mappen:
- conda_dependencies.yml
- spark_dependencies.yml
- Beräkna med filer
    - \<Beräkna målnamn > .compute
- Kör konfigurationsfiler
    - \<Kör Konfigurationsnamnet > .runconfig

>[!NOTE]
>Du normalt har en beräkning målfilen och kör konfigurationsfilen för varje compute-mål som du skapar. Du kan dock skapa dessa filer oberoende och har flera kör konfigurationsfiler som pekar på samma mål för beräkning.

## <a name="condadependenciesyml"></a>conda_dependencies.yml
Den här filen är en [conda miljö filen](https://conda.io/docs/using/envs.html#create-environment-file-by-hand) som anger Python körtidsversion och paket som är beroende av din kod. När Azure ML-arbetsstationen kör ett skript i ett dockerbehållare eller HDInsight-kluster, skapas en [conda miljö](https://conda.io/docs/using/envs.html) att köra skriptet. 

Ange Python-paket som behöver för ditt skript för körning i den här filen. Azure ML experiment tjänsten skapar conda miljön i Docker-avbildningen enligt din lista över beroenden. Här listan paket måste kunna nås av motorn för körning. Därför måste paket som ska visas i kanaler:

* [continuum.IO](https://anaconda.org/conda-forge/repo)
* [PyPI](https://pypi.python.org/pypi)
* en offentligt tillgänglig slutpunkt (URL)
* eller en lokal filsökväg
* andra kan nås av motorn för körning

>[!NOTE]
>När du kör på HDInsight-kluster skapar en conda miljö för din Azure ML-arbetsstationen. Detta gör att olika användare ska köras på olika python-miljöer i samma kluster.  

Här är ett exempel på en typisk **conda_dependencies.yml** fil.
```yaml
name: project_environment
dependencies:
  # Python version
  - python=3.5.2
  
  # some conda packages
  - scikit-learn
  - cryptography
  
  # use pip to install some more packages
  - pip:
     # a package in PyPi
     - azure-storage
     
     # a package hosted in a public URL endpoint
     - https://cntk.ai/PythonWheel/CPU-Only/cntk-2.1-cp35-cp35m-win_amd64.whl
     
     # a wheel file available locally on disk (this only works if you are executing against local target)
     - C:\temp\my_private_python_pkg.whl
```

Azure ML-arbetsstationen använder samma conda miljö utan att återskapa så länge den **conda_dependencies.yml** intakt. Om något ändras i den här filen resulterar det dock i återskapa Docker-bild.

>[!NOTE]
>Om du riktar körning mot _lokala_ compute sammanhang **conda_dependencies.yml** filen **inte** används. Paketberoenden för din lokala Azure ML-arbetsstationen Python-miljö måste installeras manuellt.

## <a name="sparkdependenciesyml"></a>spark_dependencies.yml
Den här filen anger namnet på Spark-programmet när du skickar ett PySpark-skript och Spark-paket som måste installeras. Du kan också ange någon offentlig Maven databasen samt Spark-paket som finns i dessa Maven-databaser.

Här är ett exempel:

```yaml
configuration:
  # Spark application name
  "spark.app.name": "ClassifyingIris"
  
repositories:
  # Maven repository hosted in Azure CDN
  - "https://mmlspark.azureedge.net/maven"
  
  # Maven repository hosted in spark-packages.org
  - "https://spark-packages.org/packages"
  
packages:
  # MMLSpark package hosted in the Azure CDN Maven
  - group: "com.microsoft.ml.spark"
    artifact: "mmlspark_2.11"
    version: "0.5"
    
  # spark-sklearn packaged hosted in the spark-packages.org Maven
  - group: "databricks"
    artifact: "spark-sklearn"
    version: "0.2.0"
```

>[!NOTE]
>Prestandajustering klusterparametrar, till exempel worker storlek, kärnor ska gå till avsnittet ”configuration” i filen spark_dependecies.yml 

>[!NOTE]
>Om du kör skriptet i Python-miljö, *spark_dependencies.yml* ignoreras. Det har endast effekt om du kör mot Spark (antingen i Docker eller HDInsight-kluster).

## <a name="run-configuration"></a>Kör konfigurationen
Om du vill ange en specifik kör konfiguration krävs ett par med filer. De genereras vanligtvis med kommandot CLI. Men du kan också klona avslutas viktiga, byta namn på dem och redigera dem.

```azurecli
# create a compute target pointing to a VM via SSH
$ az ml computetarget attach remotedocker -n <compute target name> -a <IP address or FQDN of VM> -u <username> -w <password>

# create a compute context pointing to an HDI cluster head-node via SSH
$ az ml computetarget attach cluster -n <compute target name> -a <IP address or FQDN of HDI cluster> -u <username> -w <password> 
```

Detta kommando skapar ett par filer baserat på den angivna beräkning måltypen. Anta att du namngivna beräknings-målet _foo_. Det här kommandot genererar _foo.compute_ och _foo.runconfig_ i din **aml_config** mapp.

>[!NOTE]
> _lokala_ eller _docker_ namn för kör konfigurationsfilerna är valfri. Azure ML-arbetsstationen lägger till dessa två kör konfigurationer när du skapar ett tomt projekt för din bekvämlighet. Du kan byta namn på ”<run configuration name>.runconfig” filer som medföljer projektmallen eller skapa nya med vilket namn som helst.

### <a name="compute-target-namecompute"></a>\<Beräkna målnamn > .compute
_\<Beräkna målnamn > .compute_ filen anger information om anslutning och konfiguration för beräknings-målet. Det är en lista över namn / värde-par. Följande är inställningarna som stöds.

**typen**: typ av beräknings-miljö. Värden som stöds är:
  - lokala
  - Docker
  - remotedocker
  - kluster

**baseDockerImage**: den Docker-bild som används för att köra skriptet Python/PySpark. Standardvärdet är _microsoft/mmlspark:plus-0.7.91_. Vi stöder också en bild: _microsoft/mmlspark:plus-gpu-0.7.91_, vilket ger dig GPU åtkomst till värddatorn (om GPU finns).

**adressen**: IP-adress eller FQDN (fullständigt kvalificerade domännamn) på den virtuella datorn eller HDInsight-kluster huvudnod.

**användarnamnet**: SSH användarnamnet för åtkomst till den virtuella datorn eller HDInsight-huvudnod.

**lösenordet**: det krypterade lösenordet för SSH-anslutning.

**sharedVolumes**: flagga för att signalera att motorn för körning ska använda Docker delad volym funktionen att leverera projektfiler fram och tillbaka. Med den här flaggan aktiverat kan snabba upp körningen eftersom Docker kan komma åt projekt direkt utan att behöva kopiera dem. Det är bäst att ange _FALSKT_ om Docker-motorn körs på Windows eftersom volymen delning för Docker i Windows kan vara flaky. Ange det till _SANT_ om den körs på macOS- eller Linux.

**nvidiaDocker**: den här flaggan när _SANT_, talar om tjänsten Azure ML försök att använda _nvidia docker_ kommandot, till skillnad från vanliga _docker_ kommandot för att starta Docker-bild. Den _nvidia docker_ motorn kan dockerbehållare åtkomst GPU maskinvara. Inställningen är obligatorisk om du vill köra GPU Docker-behållare. Har stöd för Linux-värd _nvidia docker_. Till exempel Linux-baserade DSVM i Azure levereras med _nvidia docker_. _NVIDIA docker_ från och med nu stöds inte i Windows.

**nativeSharedDirectory**: den här egenskapen anger baskatalogen (till exempel: _~/.azureml/share/_) där filer kan sparas för att delas mellan körs på samma mål för beräkning. Om den här inställningen används när körs på en dockerbehållare med _sharedVolumes_ måste anges till true. Annars misslyckas körning.

### <a name="run-configuration-namerunconfig"></a>\<Kör Konfigurationsnamnet > .runconfig
_\<Kör Konfigurationsnamnet > .runconfig_ anger Azure ML experimentera körningsbeteende. Du kan konfigurera körning beteenden, till exempel spårning körningshistorik eller vad compute mål för att använda tillsammans med många andra. Namnen på de kör konfigurationsfilerna används för att fylla i körningen kontexten listrutan i Azure ML-arbetsstationen skrivbordsprogram.

**ArgumentVector**: det här avsnittet anger skriptet ska köras som en del av den här körning och parametrar för skriptet. Om du har följande kodavsnitt i till exempel din ”<run configuration name>.runconfig” fil 

```
 "ArgumentVector":[
  - "myscript.py"
  - 234
  - "-v" 
 ] 
```
_”az ml experiment skicka foo.runconfig”_ automatiskt kör kommandot med _myscript.py_ filen skickar i 234 som en parameter och anger--verbose-flaggan.

**Målet**: den här parametern är namnet på den _.compute_ filen som den _runconfig_ filen referenser. Det vanligtvis pekar på _foo.compute_ filen men du kan redigera den att peka till en annan beräknings-mål.

**Miljövariabler**: det här avsnittet kan du ange miljövariabler som en del av deras körs. Användaren kan ange miljövariablerna med hjälp av namn / värde-par i följande format:
```
EnvironmentVariables:
"EXAMPLE_ENV_VAR1": "Example Value1"
"EXAMPLE_ENV_VAR2": "Example Value2"
```

De här miljövariablerna kan nås i användarens kod. Till exempel skrivs phyton koden miljövariabel som heter ”EXAMPLE_ENV_VAR”
```
print(os.environ.get("EXAMPLE_ENV_VAR1"))
```

**Framework**: den här egenskapen anger om Azure ML-arbetsstationen ska starta ett Spark-session för att köra skriptet. Standardvärdet är _PySpark_. Ange det till _Python_ om du inte kör PySpark-kod, som kan hjälpa dig att starta jobbet snabbare med lägre kostnader.

**CondaDependenciesFile**: den här egenskapen pekar på den fil som anger conda miljö beroenden i den *aml_config* mapp. Om värdet _null_, pekar på standardvärdet **conda_dependencies.yml** fil.

**SparkDependenciesFile**: den här egenskapen pekar på den fil som anger Spark-beroenden i den **aml_config** mapp. Den är inställd på _null_ som standard och den pekar till standardvärdet **spark_dependencies.yml** fil.

**PrepareEnvironment**: den här egenskapen har värdet _SANT_, talar om tjänsten undersökningar för att förbereda miljön conda baserat på conda beroenden som angetts som en del av din första gången. Den här egenskapen är effektivt endast när du kör mot en Docker-miljö. Den här inställningen har ingen effekt om du kör mot en _lokala_ miljö. 

**TrackedRun**: den här flaggan signalerar tjänsten experiment om att spåra körs i Azure ML-arbetsstationen kör historik infrastruktur eller inte. Standardvärdet är _SANT_. 

**UseSampling**: _UseSampling_ anger om de aktiva provdatauppsättningar för datakällor som används för körning. Om värdet _FALSKT_, datakällor mata in och använda fullständig data läses från datalagret. Om värdet _SANT_, aktiva exempel används. Användarna kan använda den ** DataSourceSettings ”att ange vilka specifika provdatauppsättningar ska användas om de vill åsidosätta active exemplet. 

**DataSourceSettings**: konfigurationsavsnittet anger inställningarna för datakälla. I det här avsnittet anger användaren vilka befintliga data exemplet för en viss datakälla används som en del av körningen. 

Följande Konfigurationsinställningen anger det exemplet med namnet ”MySample” används för datakällan med namnet ”MyDataSource”
```
DataSourceSettings:
    MyDataSource.dsource:
    Sampling:
    Sample: MySample
```

**DataSourceSubstitutions**: datakälla ersättningar kan användas när användaren vill växla från en datakälla till en annan utan att ändra koden. Användare kan till exempel växla från en provtagning ned, lokal fil till den ursprungliga, större datamängd som lagras i Azure Blob genom att ändra referens för datakälla. När en ersättning används körs Azure ML-arbetsstationen dina datakällor och data förberedelse paket som refererar till datakällan substitute.

I följande exempel ersätter ”mylocal.datasource”-referenser i Azure ML-datakällor och paket för förberedelse av data med ”myremote.dsource”. 
 
```
DataSourceSubstitutions:
    myocal.dsource: myremote.dsource
```

Baserat på ovanstående ersättningen läser följande kodexempel nu från ”myremote.dsource” i stället för ”mylocal.dsource” utan användare ändra sina kod.
```
df = datasource.load_datasource('mylocal.dsource')
```
## <a name="next-steps"></a>Nästa steg
Lär dig mer om [experiment tjänstkonfiguration](experimentation-service-configuration.md).
