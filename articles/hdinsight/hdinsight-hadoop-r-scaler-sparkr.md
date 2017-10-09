---
title: aaaUse ScaleR och SparkR med Azure HDInsight | Microsoft Docs
description: "Använda ScaleR och SparkR med R Server och HDInsight"
services: hdinsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 5a76f897-02e8-4437-8f2b-4fb12225854a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: bradsev
ms.openlocfilehash: da732ff0235cf465a1452b81750c7cdd0351eed5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="combine-scaler-and-sparkr-in-hdinsight"></a><span data-ttu-id="06fae-103">Kombinera ScaleR och SparkR i HDInsight</span><span class="sxs-lookup"><span data-stu-id="06fae-103">Combine ScaleR and SparkR in HDInsight</span></span>

<span data-ttu-id="06fae-104">Den här artikeln visar hur toopredict flight ankomst fördröjningar med hjälp av en **ScaleR** logistic regressionsmodell från data på svarta fördröjningar och väder kopplas till **SparkR**.</span><span class="sxs-lookup"><span data-stu-id="06fae-104">This article shows how toopredict flight arrival delays using a **ScaleR** logistic regression model from data on flight delays and weather joined with **SparkR**.</span></span> <span data-ttu-id="06fae-105">Det här scenariot visar hello funktionerna i ScaleR för datamanipulering på Spark som används med Microsoft R Server för analys.</span><span class="sxs-lookup"><span data-stu-id="06fae-105">This scenario demonstrates hello capabilities of ScaleR for data manipulation on Spark used with Microsoft R Server for analytics.</span></span> <span data-ttu-id="06fae-106">hello kombination av dessa tekniker kan tooapply hello senaste funktionerna i distribuerad databehandling.</span><span class="sxs-lookup"><span data-stu-id="06fae-106">hello combination of these technologies enables you tooapply hello latest capabilities in distributed processing.</span></span>

<span data-ttu-id="06fae-107">Även om båda paketen körs på Hadoops motorn för körning av Spark, blockeras de från InMemory-Datadelning eftersom de varje kräver sin egen respektive Spark-sessioner.</span><span class="sxs-lookup"><span data-stu-id="06fae-107">Although both packages run on Hadoop’s Spark execution engine, they are blocked from in-memory data sharing as they each require their own respective Spark sessions.</span></span> <span data-ttu-id="06fae-108">Tills problemet åtgärdas i en kommande version av R-Server, är hello lösningen toomaintain inte överlappar Spark sessioner och tooexchange data via mellanliggande filer.</span><span class="sxs-lookup"><span data-stu-id="06fae-108">Until this issue is addressed in an upcoming version of R Server, hello workaround is toomaintain non-overlapping Spark sessions, and tooexchange data through intermediate files.</span></span> <span data-ttu-id="06fae-109">hello anvisningarna här visar att dessa krav är enkla tooachieve.</span><span class="sxs-lookup"><span data-stu-id="06fae-109">hello instructions here show that these requirements are straightforward tooachieve.</span></span>

<span data-ttu-id="06fae-110">Vi använder ett exempel här ursprungligen delas i en prata med skikt 2016 av Mario Inchiosa och Roni Burd som även är tillgängliga via hello webbseminariet [skapa en skalbar datavetenskap plattform med R](http://event.on24.com/eventRegistration/console/EventConsoleNG.jsp?uimode=nextgeneration&eventid=1160288&sessionid=1&key=8F8FB9E2EB1AEE867287CD6757D5BD40&contenttype=A&eventuserid=305999&playerwidth=1000&playerheight=650&caller=previewLobby&text_language_id=en&format=fhaudio). hello exemplet används SparkR toojoin hello välkända flygbolagen ankomst fördröjning datauppsättning med väder data vid utgångspunkten och ankomst flygplatser.</span><span class="sxs-lookup"><span data-stu-id="06fae-110">We use an example here initially shared in a talk at Strata 2016 by Mario Inchiosa and Roni Burd that is also available through hello webinar [Building a Scalable Data Science Platform with R](http://event.on24.com/eventRegistration/console/EventConsoleNG.jsp?uimode=nextgeneration&eventid=1160288&sessionid=1&key=8F8FB9E2EB1AEE867287CD6757D5BD40&contenttype=A&eventuserid=305999&playerwidth=1000&playerheight=650&caller=previewLobby&text_language_id=en&format=fhaudio). hello example uses SparkR toojoin hello well-known airlines arrival delay data set with weather data at departure and arrival airports.</span></span> <span data-ttu-id="06fae-111">hello data ansluten används sedan som inkommande tooa ScaleR logistic regressionsmodell för att förutsäga svarta ankomst fördröjning.</span><span class="sxs-lookup"><span data-stu-id="06fae-111">hello data joined is then used as input tooa ScaleR logistic regression model for predicting flight arrival delay.</span></span>

<span data-ttu-id="06fae-112">Hej kodhandledning vi ursprungligen skrevs för R-Server som körs på Spark i HDInsight-kluster i Azure.</span><span class="sxs-lookup"><span data-stu-id="06fae-112">hello code we walkthrough was originally written for R Server running on Spark in an HDInsight cluster on Azure.</span></span> <span data-ttu-id="06fae-113">Men hello begreppet blanda hello användningen av SparkR och ScaleR i ett skript är också i hello kontext för lokala miljöer.</span><span class="sxs-lookup"><span data-stu-id="06fae-113">But hello concept of mixing hello use of SparkR and ScaleR in one script is also valid in hello context of on-premises environments.</span></span> <span data-ttu-id="06fae-114">I följande hello, Vi förutsätter att en mellanliggande nivå av R och R hello [ScaleR](https://msdn.microsoft.com/microsoft-r/scaler-user-guide-introduction) bibliotek med R Server.</span><span class="sxs-lookup"><span data-stu-id="06fae-114">In hello following, we presume an intermediate level of knowledge of R and R hello [ScaleR](https://msdn.microsoft.com/microsoft-r/scaler-user-guide-introduction) library of R Server.</span></span> <span data-ttu-id="06fae-115">Vi introducerar också användning av [SparkR](https://spark.apache.org/docs/2.1.0/sparkr.html) när via det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="06fae-115">We also introduce use of [SparkR](https://spark.apache.org/docs/2.1.0/sparkr.html) while walking through this scenario.</span></span>

## <a name="hello-airline-and-weather-datasets"></a><span data-ttu-id="06fae-116">hello flygbolag och väder datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="06fae-116">hello airline and weather datasets</span></span>

<span data-ttu-id="06fae-117">Hej **AirOnTime08to12CSV** flygbolagen offentliga datamängden innehåller information om svarta ankomst och avvikelse information om alla kommersiella flygplan inom hello USA, från oktober 1987 tooDecember 2012.</span><span class="sxs-lookup"><span data-stu-id="06fae-117">hello **AirOnTime08to12CSV** airlines public dataset contains information on flight arrival and departure details for all commercial flights within hello USA, from October 1987 tooDecember 2012.</span></span> <span data-ttu-id="06fae-118">Det här är en stor datauppsättning: det finns poster som nästan 150 miljoner totalt.</span><span class="sxs-lookup"><span data-stu-id="06fae-118">This is a large dataset: there are nearly 150 million records in total.</span></span> <span data-ttu-id="06fae-119">Det är bara under 4 GB ska packas upp.</span><span class="sxs-lookup"><span data-stu-id="06fae-119">It is just under 4 GB unpacked.</span></span> <span data-ttu-id="06fae-120">Den är tillgänglig från hello [US government Arkiv](http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236).</span><span class="sxs-lookup"><span data-stu-id="06fae-120">It is available from hello [U.S. government archives](http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236).</span></span> <span data-ttu-id="06fae-121">Bekvämare, den är tillgänglig som en zip-fil (AirOnTimeCSV.zip) som innehåller en uppsättning 303 separata månatliga CSV-filer från hello [Revolution Analytics dataset-databasen](http://packages.revolutionanalytics.com/datasets/AirOnTime87to12/)</span><span class="sxs-lookup"><span data-stu-id="06fae-121">More conveniently, it is available as a zip file (AirOnTimeCSV.zip) containing a set of 303 separate monthly CSV files from hello [Revolution Analytics dataset repository](http://packages.revolutionanalytics.com/datasets/AirOnTime87to12/)</span></span>

<span data-ttu-id="06fae-122">toosee hello effekterna av väder på svarta fördröjningar, vi måste också hello väder data på varje hello flygplatser.</span><span class="sxs-lookup"><span data-stu-id="06fae-122">toosee hello effects of weather on flight delays, we also need hello weather data at each of hello airports.</span></span> <span data-ttu-id="06fae-123">Dessa data kan hämtas som zip-filer i obearbetat format per månad från hello [nationella oceaniskt och luften Administration databasen](http://www.ncdc.noaa.gov/orders/qclcd/).</span><span class="sxs-lookup"><span data-stu-id="06fae-123">This data can be downloaded as zip files in raw form, by month, from hello [National Oceanic and Atmospheric Administration repository](http://www.ncdc.noaa.gov/orders/qclcd/).</span></span> <span data-ttu-id="06fae-124">Hello enligt det här exemplet, vi hämtar väder data från maj 2007 – December 2012 och används hello timvis datafiler inom varje hello 68 månatliga komprimerade.</span><span class="sxs-lookup"><span data-stu-id="06fae-124">For hello purposes of this example, we pull weather data from May 2007 – December 2012 and used hello hourly data files within each of hello 68 monthly zips.</span></span> <span data-ttu-id="06fae-125">hello månatliga zip-filer kan du också innehålla en mappning (YYYYMMstation.txt) mellan hello väder station ID (WBAN), hello flygplats att den är associerad med (CallSign) och hello flygplatss Tidszonförskjutning från UTC (tidszonen).</span><span class="sxs-lookup"><span data-stu-id="06fae-125">hello monthly zip files also contain a mapping (YYYYMMstation.txt) between hello weather station ID (WBAN), hello airport that it is associated with (CallSign), and hello airport’s time zone offset from UTC (TimeZone).</span></span> <span data-ttu-id="06fae-126">All den här informationen behövs när du ansluter med hello flygbolag fördröjning och väder data.</span><span class="sxs-lookup"><span data-stu-id="06fae-126">All of this information is needed when joining with hello airline delay and weather data.</span></span>

## <a name="setting-up-hello-spark-environment"></a><span data-ttu-id="06fae-127">Konfigurera hello Spark-miljö</span><span class="sxs-lookup"><span data-stu-id="06fae-127">Setting up hello Spark environment</span></span>

<span data-ttu-id="06fae-128">hello första steget är tooset in hello Spark-miljön.</span><span class="sxs-lookup"><span data-stu-id="06fae-128">hello first step is tooset up hello Spark environment.</span></span> <span data-ttu-id="06fae-129">Vi börjar med pekar toohello katalog som innehåller våra indata kataloger, skapa en kontext för beräkning av Spark och skapa en loggningen för informativt loggning toohello konsolen:</span><span class="sxs-lookup"><span data-stu-id="06fae-129">We begin by pointing toohello directory that contains our input data directories, creating a Spark compute context, and creating a logging function for informational logging toohello console:</span></span>

```
workDir        <- '~'  
myNameNode     <- 'default' 
myPort         <- 0
inputDataDir   <- 'wasb://hdfs@myAzureAcccount.blob.core.windows.net'
hdfsFS         <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

# create a persistent Spark session tooreduce startup times 
#   (remember toostop it later!)
 
sparkCC        <- RxSpark(consoleOutput=TRUE, nameNode=myNameNode, port=myPort, persistentRun=TRUE)

# create working directories 

rxHadoopMakeDir('/user')
rxHadoopMakeDir('user/RevoShare')
rxHadoopMakeDir('user/RevoShare/remoteuser')

(dataDir <- '/share')
rxHadoopMakeDir(dataDir)
rxHadoopListFiles(dataDir) 

setwd(workDir)
getwd()

# version of rxRoc that runs in a local CC 
rxRoc <- function(...){
  rxSetComputeContext(RxLocalSeq())
  roc <- RevoScaleR::rxRoc(...)
  rxSetComputeContext(sparkCC)
  return(roc)
}

logmsg <- function(msg) { cat(format(Sys.time(), "%Y-%m-%d %H:%M:%S"),':',msg,'\n') } 
t0 <- proc.time() 

#..start 

logmsg('Start') 
(trackers <- system("mapred job -list-active-trackers", intern = TRUE))
logmsg(paste('Number of task nodes=',length(trackers)))
```

<span data-ttu-id="06fae-130">Nästa vi lägga till ”Spark_Home” toohello sökvägen för R-paket så att vi kan använda SparkR och initiera en SparkR session:</span><span class="sxs-lookup"><span data-stu-id="06fae-130">Next we add “Spark_Home” toohello search path for R packages so that we can use SparkR, and initialize a SparkR session:</span></span>

```
#..setup for use of SparkR  

logmsg('Initialize SparkR') 

.libPaths(c(file.path(Sys.getenv("SPARK_HOME"), "R", "lib"), .libPaths()))
library(SparkR)

sparkEnvir <- list(spark.executor.instances = '10',
                   spark.yarn.executor.memoryOverhead = '8000')

sc <- sparkR.init(
  sparkEnvir = sparkEnvir,
  sparkPackages = "com.databricks:spark-csv_2.10:1.3.0"
)

sqlContext <- sparkRSQL.init(sc)
```

## <a name="preparing-hello-weather-data"></a><span data-ttu-id="06fae-131">Förbereder hello väder data</span><span class="sxs-lookup"><span data-stu-id="06fae-131">Preparing hello weather data</span></span>

<span data-ttu-id="06fae-132">tooprepare hello väder data vi delmängd den toohello kolumner som behövs för modellering:</span><span class="sxs-lookup"><span data-stu-id="06fae-132">tooprepare hello weather data, we subset it toohello columns needed for modeling:</span></span> 

- <span data-ttu-id="06fae-133">”Synlighet”</span><span class="sxs-lookup"><span data-stu-id="06fae-133">"Visibility"</span></span>
- <span data-ttu-id="06fae-134">”DryBulbCelsius”</span><span class="sxs-lookup"><span data-stu-id="06fae-134">"DryBulbCelsius"</span></span>
- <span data-ttu-id="06fae-135">”DewPointCelsius”</span><span class="sxs-lookup"><span data-stu-id="06fae-135">"DewPointCelsius"</span></span>
- <span data-ttu-id="06fae-136">”RelativeHumidity”</span><span class="sxs-lookup"><span data-stu-id="06fae-136">"RelativeHumidity"</span></span>
- <span data-ttu-id="06fae-137">”Vindhastigheten”</span><span class="sxs-lookup"><span data-stu-id="06fae-137">"WindSpeed"</span></span>
- <span data-ttu-id="06fae-138">”Altimeter”</span><span class="sxs-lookup"><span data-stu-id="06fae-138">"Altimeter"</span></span>

<span data-ttu-id="06fae-139">Sedan vi lägga till en flygplats kod som är associerad med hello väder station och konvertera hello mätningar från tooUTC lokal tid.</span><span class="sxs-lookup"><span data-stu-id="06fae-139">Then we add an airport code associated with hello weather station and convert hello measurements from local time tooUTC.</span></span>

<span data-ttu-id="06fae-140">Vi börjar med att skapa en fil toomap hello väder station (WBAN) info tooan flygplats kod.</span><span class="sxs-lookup"><span data-stu-id="06fae-140">We begin by creating a file toomap hello weather station (WBAN) info tooan airport code.</span></span> <span data-ttu-id="06fae-141">Vi kan hämta detta korrelation från hello mappningsfilen medföljer hello väder data.</span><span class="sxs-lookup"><span data-stu-id="06fae-141">We could get this correlation from hello mapping file included with hello weather data.</span></span> <span data-ttu-id="06fae-142">Med mappning hello *CallSign* (till exempel LAX) fältet i datafilen för hello väder för*ursprung* i hello flygbolag data.</span><span class="sxs-lookup"><span data-stu-id="06fae-142">By mapping hello *CallSign* (for example, LAX) field in hello weather data file too*Origin* in hello airline data.</span></span> <span data-ttu-id="06fae-143">Men vi just har hänt toohave en annan mappning till hands som mappar *WBAN* för*AirportID* (till exempel 12892 för LAX) och innehåller *tidszonen* som har sparats tooa CSV-fil som heter ”wban-till-en flygplats-id-tz. CSV-fil ”som vi kan använda.</span><span class="sxs-lookup"><span data-stu-id="06fae-143">However, we just happened toohave another mapping on hand that maps *WBAN* too*AirportID* (for example, 12892 for LAX) and includes *TimeZone* that has been saved tooa CSV file called “wban-to-airport-id-tz.CSV” that we can use.</span></span> <span data-ttu-id="06fae-144">Exempel:</span><span class="sxs-lookup"><span data-stu-id="06fae-144">For example:</span></span>

| <span data-ttu-id="06fae-145">AirportID</span><span class="sxs-lookup"><span data-stu-id="06fae-145">AirportID</span></span> | <span data-ttu-id="06fae-146">WBAN</span><span class="sxs-lookup"><span data-stu-id="06fae-146">WBAN</span></span> | <span data-ttu-id="06fae-147">Tidszon</span><span class="sxs-lookup"><span data-stu-id="06fae-147">TimeZone</span></span>
|-----------|------|---------
| <span data-ttu-id="06fae-148">10685</span><span class="sxs-lookup"><span data-stu-id="06fae-148">10685</span></span> | <span data-ttu-id="06fae-149">54831</span><span class="sxs-lookup"><span data-stu-id="06fae-149">54831</span></span> | <span data-ttu-id="06fae-150">-6</span><span class="sxs-lookup"><span data-stu-id="06fae-150">-6</span></span>
| <span data-ttu-id="06fae-151">14871</span><span class="sxs-lookup"><span data-stu-id="06fae-151">14871</span></span> | <span data-ttu-id="06fae-152">24232</span><span class="sxs-lookup"><span data-stu-id="06fae-152">24232</span></span> | <span data-ttu-id="06fae-153">-8</span><span class="sxs-lookup"><span data-stu-id="06fae-153">-8</span></span>
| <span data-ttu-id="06fae-154">..</span><span class="sxs-lookup"><span data-stu-id="06fae-154">..</span></span> | <span data-ttu-id="06fae-155">..</span><span class="sxs-lookup"><span data-stu-id="06fae-155">..</span></span> | <span data-ttu-id="06fae-156">..</span><span class="sxs-lookup"><span data-stu-id="06fae-156">..</span></span>

<span data-ttu-id="06fae-157">hello följande kod läser varje hello timvis rådata väder data filerna delmängder toohello kolumner vi behöver, sammanfogar hello väder station mappningsfilen, justerar hello datum för mätningar tooUTC och skriver sedan ut en ny version av filen hello:</span><span class="sxs-lookup"><span data-stu-id="06fae-157">hello following code reads each of hello hourly raw weather data files, subsets toohello columns we need, merges hello weather station mapping file, adjusts hello date times of measurements tooUTC, and then writes out a new version of hello file:</span></span>

```
# Look up AirportID and Timezone for WBAN (weather station ID) and adjust time

adjustTime <- function(dataList)
{
  dataset0 <- as.data.frame(dataList)
  
  dataset1 <- base::merge(dataset0, wbanToAirIDAndTZDF1, by = "WBAN")

  if(nrow(dataset1) == 0) {
    dataset1 <- data.frame(
      Visibility = numeric(0),
      DryBulbCelsius = numeric(0),
      DewPointCelsius = numeric(0),
      RelativeHumidity = numeric(0),
      WindSpeed = numeric(0),
      Altimeter = numeric(0),
      AdjustedYear = numeric(0),
      AdjustedMonth = numeric(0),
      AdjustedDay = integer(0),
      AdjustedHour = integer(0),
      AirportID = integer(0)
    )
    
    return(dataset1)
  }
  
  Year <- as.integer(substr(dataset1$Date, 1, 4))
  Month <- as.integer(substr(dataset1$Date, 5, 6))
  Day <- as.integer(substr(dataset1$Date, 7, 8))
  
  Time <- dataset1$Time
  Hour <- ceiling(Time/100)
  
  Timezone <- as.integer(dataset1$TimeZone)
  
  adjustdate = as.POSIXlt(sprintf("%4d-%02d-%02d %02d:00:00", Year, Month, Day, Hour), tz = "UTC") + Timezone * 3600

  AdjustedYear = as.POSIXlt(adjustdate)$year + 1900
  AdjustedMonth = as.POSIXlt(adjustdate)$mon + 1
  AdjustedDay   = as.POSIXlt(adjustdate)$mday
  AdjustedHour  = as.POSIXlt(adjustdate)$hour
  
  AirportID = dataset1$AirportID
  Weather = dataset1[,c("Visibility", "DryBulbCelsius", "DewPointCelsius", "RelativeHumidity", "WindSpeed", "Altimeter")]
  
  data.set = data.frame(cbind(AdjustedYear, AdjustedMonth, AdjustedDay, AdjustedHour, AirportID, Weather))
  
  return(data.set)
}

wbanToAirIDAndTZDF <- read.csv("wban-to-airport-id-tz.csv")

colInfo <- list(
  WBAN = list(type="integer"),
  Date = list(type="character"),
  Time = list(type="integer"),
  Visibility = list(type="numeric"),
  DryBulbCelsius = list(type="numeric"),
  DewPointCelsius = list(type="numeric"),
  RelativeHumidity = list(type="numeric"),
  WindSpeed = list(type="numeric"),
  Altimeter = list(type="numeric")
)

weatherDF <- RxTextData(file.path(inputDataDir, "WeatherRaw"), colInfo = colInfo)

weatherDF1 <- RxTextData(file.path(inputDataDir, "Weather"), colInfo = colInfo,
                filesystem=hdfsFS)

rxSetComputeContext("localpar")
rxDataStep(weatherDF, outFile = weatherDF1, rowsPerRead = 50000, overwrite = T,
           transformFunc = adjustTime,
           transformObjects = list(wbanToAirIDAndTZDF1 = wbanToAirIDAndTZDF))
```

## <a name="importing-hello-airline-and-weather-data-toospark-dataframes"></a><span data-ttu-id="06fae-158">Importera hello flygbolag och väder data tooSpark DataFrames</span><span class="sxs-lookup"><span data-stu-id="06fae-158">Importing hello airline and weather data tooSpark DataFrames</span></span>

<span data-ttu-id="06fae-159">Vi använder inte hello SparkR [read.df()](https://docs.databricks.com/spark/latest/sparkr/functions/read.df.html) tooimport hello väder och flygbolag data tooSpark DataFrames att fungera.</span><span class="sxs-lookup"><span data-stu-id="06fae-159">Now we use hello SparkR [read.df()](https://docs.databricks.com/spark/latest/sparkr/functions/read.df.html) function tooimport hello weather and airline data tooSpark DataFrames.</span></span> <span data-ttu-id="06fae-160">Den här funktionen så många Spark-metoder utförs lazy, vilket innebär att de i kö för körning men inte köras förrän krävs.</span><span class="sxs-lookup"><span data-stu-id="06fae-160">This function, like many other Spark methods, are executed lazily, meaning that they are queued for execution but not executed until required.</span></span>

```
airPath     <- file.path(inputDataDir, "AirOnTime08to12CSV")
weatherPath <- file.path(inputDataDir, "Weather") # pre-processed weather data
rxHadoopListFiles(airPath) 
rxHadoopListFiles(weatherPath) 

# create a SparkR DataFrame for hello airline data

logmsg('create a SparkR DataFrame for hello airline data') 
# use inferSchema = "false" for more robust parsing
airDF <- read.df(sqlContext, airPath, source = "com.databricks.spark.csv", 
                 header = "true", inferSchema = "false")

# Create a SparkR DataFrame for hello weather data

logmsg('create a SparkR DataFrame for hello weather data') 
weatherDF <- read.df(sqlContext, weatherPath, source = "com.databricks.spark.csv", 
                     header = "true", inferSchema = "true")
```

## <a name="data-cleansing-and-transformation"></a><span data-ttu-id="06fae-161">Datarensning och omvandling</span><span class="sxs-lookup"><span data-stu-id="06fae-161">Data cleansing and transformation</span></span>

<span data-ttu-id="06fae-162">Nästa vi göra vissa rensning på hello flygbolag data har vi importerat toorename kolumner.</span><span class="sxs-lookup"><span data-stu-id="06fae-162">Next we do some cleanup on hello airline data we’ve imported toorename columns.</span></span> <span data-ttu-id="06fae-163">Vi bara hålla hello variabler som behövs och avrunda schemalagda avvikelse gånger ned toohello närmsta timmen tooenable sammanfoga hello senaste väder data på avvikelse:</span><span class="sxs-lookup"><span data-stu-id="06fae-163">We only keep hello variables needed, and round scheduled departure times down toohello nearest hour tooenable merging with hello latest weather data at departure:</span></span>

```
logmsg('clean hello airline data') 
airDF <- rename(airDF,
                ArrDel15 = airDF$ARR_DEL15,
                Year = airDF$YEAR,
                Month = airDF$MONTH,
                DayofMonth = airDF$DAY_OF_MONTH,
                DayOfWeek = airDF$DAY_OF_WEEK,
                Carrier = airDF$UNIQUE_CARRIER,
                OriginAirportID = airDF$ORIGIN_AIRPORT_ID,
                DestAirportID = airDF$DEST_AIRPORT_ID,
                CRSDepTime = airDF$CRS_DEP_TIME,
                CRSArrTime =  airDF$CRS_ARR_TIME
)

# Select desired columns from hello flight data. 
varsToKeep <- c("ArrDel15", "Year", "Month", "DayofMonth", "DayOfWeek", "Carrier", "OriginAirportID", "DestAirportID", "CRSDepTime", "CRSArrTime")
airDF <- select(airDF, varsToKeep)

# Apply schema
coltypes(airDF) <- c("character", "integer", "integer", "integer", "integer", "character", "integer", "integer", "integer", "integer")

# Round down scheduled departure time toofull hour.
airDF$CRSDepTime <- floor(airDF$CRSDepTime / 100)
```

<span data-ttu-id="06fae-164">Nu kan vi utföra liknande åtgärder på hello väder data:</span><span class="sxs-lookup"><span data-stu-id="06fae-164">Now we perform similar operations on hello weather data:</span></span>

```
# Average weather readings by hour
logmsg('clean hello weather data') 
weatherDF <- agg(groupBy(weatherDF, "AdjustedYear", "AdjustedMonth", "AdjustedDay", "AdjustedHour", "AirportID"), Visibility="avg",
                  DryBulbCelsius="avg", DewPointCelsius="avg", RelativeHumidity="avg", WindSpeed="avg", Altimeter="avg"
                  )

weatherDF <- rename(weatherDF,
                    Visibility = weatherDF$'avg(Visibility)',
                    DryBulbCelsius = weatherDF$'avg(DryBulbCelsius)',
                    DewPointCelsius = weatherDF$'avg(DewPointCelsius)',
                    RelativeHumidity = weatherDF$'avg(RelativeHumidity)',
                    WindSpeed = weatherDF$'avg(WindSpeed)',
                    Altimeter = weatherDF$'avg(Altimeter)'
)
```

## <a name="joining-hello-weather-and-airline-data"></a><span data-ttu-id="06fae-165">Koppla hello väder och flygbolag data</span><span class="sxs-lookup"><span data-stu-id="06fae-165">Joining hello weather and airline data</span></span>

<span data-ttu-id="06fae-166">Vi använder nu hello SparkR [join()](https://docs.databricks.com/spark/latest/sparkr/functions/join.html) fungerar toodo en vänster yttre koppling av hello flygbolag och väder data av avvikelse AirportID och datetime.</span><span class="sxs-lookup"><span data-stu-id="06fae-166">We now use hello SparkR [join()](https://docs.databricks.com/spark/latest/sparkr/functions/join.html) function toodo a left outer join of hello airline and weather data by departure AirportID and datetime.</span></span> <span data-ttu-id="06fae-167">hello yttre koppling kan vi tooretain alla hello flygbolag data innehåller även om det finns inga matchande väder-data.</span><span class="sxs-lookup"><span data-stu-id="06fae-167">hello outer join allows us tooretain all hello airline data records even if there is no matching weather data.</span></span> <span data-ttu-id="06fae-168">Hello koppling vi ta bort vissa redundant kolumner och Byt namn på hello hålls kolumner tooremove hello inkommande DataFrame prefix som introducerats av hello koppling.</span><span class="sxs-lookup"><span data-stu-id="06fae-168">Following hello join, we remove some redundant columns, and rename hello kept columns tooremove hello incoming DataFrame prefix introduced by hello join.</span></span>

```
logmsg('Join airline data with weather at Origin Airport')
joinedDF <- SparkR::join(
  airDF,
  weatherDF,
  airDF$OriginAirportID == weatherDF$AirportID &
    airDF$Year == weatherDF$AdjustedYear &
    airDF$Month == weatherDF$AdjustedMonth &
    airDF$DayofMonth == weatherDF$AdjustedDay &
    airDF$CRSDepTime == weatherDF$AdjustedHour,
  joinType = "left_outer"
)

# Remove redundant columns
vars <- names(joinedDF)
varsToDrop <- c('AdjustedYear', 'AdjustedMonth', 'AdjustedDay', 'AdjustedHour', 'AirportID')
varsToKeep <- vars[!(vars %in% varsToDrop)]
joinedDF1 <- select(joinedDF, varsToKeep)

joinedDF2 <- rename(joinedDF1,
                    VisibilityOrigin = joinedDF1$Visibility,
                    DryBulbCelsiusOrigin = joinedDF1$DryBulbCelsius,
                    DewPointCelsiusOrigin = joinedDF1$DewPointCelsius,
                    RelativeHumidityOrigin = joinedDF1$RelativeHumidity,
                    WindSpeedOrigin = joinedDF1$WindSpeed,
                    AltimeterOrigin = joinedDF1$Altimeter
)
```

<span data-ttu-id="06fae-169">Vi gå hello väder och flygbolag data baserat på ankomst AirportID och datetime på ett liknande sätt:</span><span class="sxs-lookup"><span data-stu-id="06fae-169">In a similar fashion, we join hello weather and airline data based on arrival AirportID and datetime:</span></span>

```
logmsg('Join airline data with weather at Destination Airport')
joinedDF3 <- SparkR::join(
  joinedDF2,
  weatherDF,
  airDF$DestAirportID == weatherDF$AirportID &
    airDF$Year == weatherDF$AdjustedYear &
    airDF$Month == weatherDF$AdjustedMonth &
    airDF$DayofMonth == weatherDF$AdjustedDay &
    airDF$CRSDepTime == weatherDF$AdjustedHour,
  joinType = "left_outer"
)

# Remove redundant columns
vars <- names(joinedDF3)
varsToDrop <- c('AdjustedYear', 'AdjustedMonth', 'AdjustedDay', 'AdjustedHour', 'AirportID')
varsToKeep <- vars[!(vars %in% varsToDrop)]
joinedDF4 <- select(joinedDF3, varsToKeep)

joinedDF5 <- rename(joinedDF4,
                    VisibilityDest = joinedDF4$Visibility,
                    DryBulbCelsiusDest = joinedDF4$DryBulbCelsius,
                    DewPointCelsiusDest = joinedDF4$DewPointCelsius,
                    RelativeHumidityDest = joinedDF4$RelativeHumidity,
                    WindSpeedDest = joinedDF4$WindSpeed,
                    AltimeterDest = joinedDF4$Altimeter
                    )
```

## <a name="save-results-toocsv-for-exchange-with-scaler"></a><span data-ttu-id="06fae-170">Spara resultaten tooCSV för exchange med ScaleR</span><span class="sxs-lookup"><span data-stu-id="06fae-170">Save results tooCSV for exchange with ScaleR</span></span>

<span data-ttu-id="06fae-171">Hello kopplingar måste toodo med SparkR är klar.</span><span class="sxs-lookup"><span data-stu-id="06fae-171">That completes hello joins we need toodo with SparkR.</span></span> <span data-ttu-id="06fae-172">Vi spara hello data från hello slutliga Spark DataFrame ”joinedDF5” tooa CSV för inkommande tooScaleR och stäng sedan ut hello SparkR session.</span><span class="sxs-lookup"><span data-stu-id="06fae-172">We save hello data from hello final Spark DataFrame “joinedDF5” tooa CSV for input tooScaleR and then close out hello SparkR session.</span></span> <span data-ttu-id="06fae-173">Vi uttryckligen begära SparkR toosave hello gällande CSV i 80 separata partitioner tooenable tillräckligt parallellitet i ScaleR bearbetningen:</span><span class="sxs-lookup"><span data-stu-id="06fae-173">We explicitly tell SparkR toosave hello resultant CSV in 80 separate partitions tooenable sufficient parallelism in ScaleR processing:</span></span>

```
logmsg('output hello joined data from Spark tooCSV') 
joinedDF5 <- repartition(joinedDF5, 80) # write.df below will produce this many CSVs

# write result toodirectory of CSVs
write.df(joinedDF5, file.path(dataDir, "joined5Csv"), "com.databricks.spark.csv", "overwrite", header = "true")

# We can shut down hello SparkR Spark context now
sparkR.stop()

# remove non-data files
rxHadoopRemove(file.path(dataDir, "joined5Csv/_SUCCESS"))
```

## <a name="import-tooxdf-for-use-by-scaler"></a><span data-ttu-id="06fae-174">Importera tooXDF för användning av ScaleR</span><span class="sxs-lookup"><span data-stu-id="06fae-174">Import tooXDF for use by ScaleR</span></span>

<span data-ttu-id="06fae-175">Vi kan använda hello CSV-fil för domänanslutna flygbolag och väder data som-är för modellering via en datakälla för ScaleR text.</span><span class="sxs-lookup"><span data-stu-id="06fae-175">We could use hello CSV file of joined airline and weather data as-is for modeling via a ScaleR text data source.</span></span> <span data-ttu-id="06fae-176">Men vi importera den tooXDF först, eftersom det är mer effektivt när du kör flera åtgärder på hello datauppsättningen:</span><span class="sxs-lookup"><span data-stu-id="06fae-176">But we import it tooXDF first, since it is more efficient when running multiple operations on hello dataset:</span></span>

```
logmsg('Import hello CSV toocompressed, binary XDF format') 

# set hello Spark compute context for R Server 
rxSetComputeContext(sparkCC)
rxGetComputeContext()

colInfo <- list(
  ArrDel15 = list(type="numeric"),
  Year = list(type="factor"),
  Month = list(type="factor"),
  DayofMonth = list(type="factor"),
  DayOfWeek = list(type="factor"),
  Carrier = list(type="factor"),
  OriginAirportID = list(type="factor"),
  DestAirportID = list(type="factor"),
  RelativeHumidityOrigin = list(type="numeric"),
  AltimeterOrigin = list(type="numeric"),
  DryBulbCelsiusOrigin = list(type="numeric"),
  WindSpeedOrigin = list(type="numeric"),
  VisibilityOrigin = list(type="numeric"),
  DewPointCelsiusOrigin = list(type="numeric"),
  RelativeHumidityDest = list(type="numeric"),
  AltimeterDest = list(type="numeric"),
  DryBulbCelsiusDest = list(type="numeric"),
  WindSpeedDest = list(type="numeric"),
  VisibilityDest = list(type="numeric"),
  DewPointCelsiusDest = list(type="numeric")
)

joinedDF5Txt <- RxTextData(file.path(dataDir, "joined5Csv"),
                           colInfo = colInfo, fileSystem = hdfsFS)
rxGetInfo(joinedDF5Txt) 

destData <- RxXdfData(file.path(dataDir, "joined5XDF"), fileSystem = hdfsFS)

rxImport(inData = joinedDF5Txt, destData, overwrite = TRUE)

rxGetInfo(destData, getVarInfo = T)

# File name: /user/RevoShare/dev/delayDataLarge/joined5XDF 
# Number of composite data files: 80 
# Number of observations: 148619655 
# Number of variables: 22 
# Number of blocks: 320 
# Compression type: zlib 
# Variable information: 
#   Var 1: ArrDel15, Type: numeric, Low/High: (0.0000, 1.0000)
# Var 2: Year
# 26 factor levels: 1987 1988 1989 1990 1991 ... 2008 2009 2010 2011 2012
# Var 3: Month
# 12 factor levels: 10 11 12 1 2 ... 5 6 7 8 9
# Var 4: DayofMonth
# 31 factor levels: 1 3 4 5 7 ... 29 30 2 18 31
# Var 5: DayOfWeek
# 7 factor levels: 4 6 7 1 3 2 5
# Var 6: Carrier
# 30 factor levels: PI UA US AA DL ... HA F9 YV 9E VX
# Var 7: OriginAirportID
# 374 factor levels: 15249 12264 11042 15412 13930 ... 13341 10559 14314 11711 10558
# Var 8: DestAirportID
# 378 factor levels: 13303 14492 10721 11057 13198 ... 14802 11711 11931 12899 10559
# Var 9: CRSDepTime, Type: integer, Low/High: (0, 24)
# Var 10: CRSArrTime, Type: integer, Low/High: (0, 2400)
# Var 11: RelativeHumidityOrigin, Type: numeric, Low/High: (0.0000, 100.0000)
# Var 12: AltimeterOrigin, Type: numeric, Low/High: (28.1700, 31.1600)
# Var 13: DryBulbCelsiusOrigin, Type: numeric, Low/High: (-46.1000, 47.8000)
# Var 14: WindSpeedOrigin, Type: numeric, Low/High: (0.0000, 81.0000)
# Var 15: VisibilityOrigin, Type: numeric, Low/High: (0.0000, 90.0000)
# Var 16: DewPointCelsiusOrigin, Type: numeric, Low/High: (-41.7000, 29.0000)
# Var 17: RelativeHumidityDest, Type: numeric, Low/High: (0.0000, 100.0000)
# Var 18: AltimeterDest, Type: numeric, Low/High: (28.1700, 31.1600)
# Var 19: DryBulbCelsiusDest, Type: numeric, Low/High: (-46.1000, 53.9000)
# Var 20: WindSpeedDest, Type: numeric, Low/High: (0.0000, 136.0000)
# Var 21: VisibilityDest, Type: numeric, Low/High: (0.0000, 88.0000)
# Var 22: DewPointCelsiusDest, Type: numeric, Low/High: (-43.0000, 29.0000)

finalData <- RxXdfData(file.path(dataDir, "joined5XDF"), fileSystem = hdfsFS)

```

## <a name="splitting-data-for-training-and-test"></a><span data-ttu-id="06fae-177">Dela data för träning och testning</span><span class="sxs-lookup"><span data-stu-id="06fae-177">Splitting data for training and test</span></span>

<span data-ttu-id="06fae-178">Vi använder rxDataStep toosplit hello 2012 uppgifter för att testa och hålla hello rest för träning:</span><span class="sxs-lookup"><span data-stu-id="06fae-178">We use rxDataStep toosplit out hello 2012 data for testing and keep hello rest for training:</span></span>

```
# split out hello training data

logmsg('split out training data as all data except year 2012')
trainDS <- RxXdfData( file.path(dataDir, "finalDataTrain" ),fileSystem = hdfsFS)

rxDataStep( inData = finalData, outFile = trainDS,
            rowSelection = ( Year != 2012 ), overwrite = T )

# split out hello testing data

logmsg('split out hello test data for year 2012') 
testDS <- RxXdfData( file.path(dataDir, "finalDataTest" ), fileSystem = hdfsFS)

rxDataStep( inData = finalData, outFile = testDS,
            rowSelection = ( Year == 2012 ), overwrite = T )

rxGetInfo(trainDS)
rxGetInfo(testDS)
```

## <a name="train-and-test-a-logistic-regression-model"></a><span data-ttu-id="06fae-179">Träna och testa en logistic regressionsmodell</span><span class="sxs-lookup"><span data-stu-id="06fae-179">Train and test a logistic regression model</span></span>

<span data-ttu-id="06fae-180">Vi är nu redo toobuild en modell.</span><span class="sxs-lookup"><span data-stu-id="06fae-180">Now we are ready toobuild a model.</span></span> <span data-ttu-id="06fae-181">toosee hello inverkan väder data på fördröjning i hello ankomsttid, vi använder Scaler's logistic regression rutinen.</span><span class="sxs-lookup"><span data-stu-id="06fae-181">toosee hello influence of weather data on delay in hello arrival time, we use ScaleR’s logistic regression routine.</span></span> <span data-ttu-id="06fae-182">Vi använder den toomodel om en ankomst fördröjning på mer än 15 minuter påverkas av hello väder vid hello utgångspunkten och ankomst flygplatser:</span><span class="sxs-lookup"><span data-stu-id="06fae-182">We use it toomodel whether an arrival delay of greater than 15 minutes is influenced by hello weather at hello departure and arrival airports:</span></span>

```
logmsg('train a logistic regression model for Arrival Delay > 15 minutes') 
formula <- as.formula(ArrDel15 ~ Year + Month + DayofMonth + DayOfWeek + Carrier +
                     OriginAirportID + DestAirportID + CRSDepTime + CRSArrTime + 
                     RelativeHumidityOrigin + AltimeterOrigin + DryBulbCelsiusOrigin +
                     WindSpeedOrigin + VisibilityOrigin + DewPointCelsiusOrigin + 
                     RelativeHumidityDest + AltimeterDest + DryBulbCelsiusDest +
                     WindSpeedDest + VisibilityDest + DewPointCelsiusDest
                   )

# Use hello scalable rxLogit() function but set max iterations too3 for hello purposes of 
# this exercise 

logitModel <- rxLogit(formula, data = trainDS, maxIterations = 3)

base::summary(logitModel)
```

<span data-ttu-id="06fae-183">Nu ska vi se hur den på hello testdata genom att göra vissa förutsägelser och titta ROC och AUC.</span><span class="sxs-lookup"><span data-stu-id="06fae-183">Now let’s see how it does on hello test data by making some predictions and looking at ROC and AUC.</span></span>

```
# Predict over test data (Logistic Regression).

logmsg('predict over hello test data') 
logitPredict <- RxXdfData(file.path(dataDir, "logitPredict"), fileSystem = hdfsFS)

# Use hello scalable rxPredict() function

rxPredict(logitModel, data = testDS, outData = logitPredict,
          extraVarsToWrite = c("ArrDel15"), 
          type = 'response', overwrite = TRUE)

# Calculate ROC and Area Under hello Curve (AUC).

logmsg('calculate hello roc and auc') 
logitRoc <- rxRoc("ArrDel15", "ArrDel15_Pred", logitPredict)
logitAuc <- rxAuc(logitRoc)
head(logitAuc)
logitAuc

plot(logitRoc)
```

## <a name="scoring-elsewhere"></a><span data-ttu-id="06fae-184">Bedömningen någon annanstans</span><span class="sxs-lookup"><span data-stu-id="06fae-184">Scoring elsewhere</span></span>

<span data-ttu-id="06fae-185">Vi kan också använda hello modellen för bedömningsprofil data på en annan plattform.</span><span class="sxs-lookup"><span data-stu-id="06fae-185">We can also use hello model for scoring data on another platform.</span></span> <span data-ttu-id="06fae-186">Genom att spara den tooan RDS-filen och överföra och importera den RDS till ett mål som bedömningen miljö, till exempel SQL Server R Services.</span><span class="sxs-lookup"><span data-stu-id="06fae-186">By saving it tooan RDS file and then transferring and importing that RDS into a destination scoring environment such as SQL Server R Services.</span></span> <span data-ttu-id="06fae-187">Det är viktigt tooensure som hello faktor nivåer av hello data toobe bedömas matchar de på vilka hello modellen har skapats.</span><span class="sxs-lookup"><span data-stu-id="06fae-187">It is important tooensure that hello factor levels of hello data toobe scored match those on which hello model was built.</span></span> <span data-ttu-id="06fae-188">Att matcha kan uppnås genom att extrahera och spara hello kolumninformation som är associerade med hello modellerar data via Scaler's `rxCreateColInfo()` funktionen och sedan använda den kolumnen toohello indata informationskällan för förutsägelse.</span><span class="sxs-lookup"><span data-stu-id="06fae-188">That match can be achieved by extracting and saving hello column infomation associated with hello modeling data via ScaleR’s `rxCreateColInfo()` function and then applying that column information toohello input data source for prediction.</span></span> <span data-ttu-id="06fae-189">I följande hello spara några rader i hello testdata vi och extrahera och använda hello kolumninformation från det här exemplet i hello förutsägelse skript:</span><span class="sxs-lookup"><span data-stu-id="06fae-189">In hello following we save a few rows of hello test dataset and extract and use hello column information from this sample in hello prediction script:</span></span>

```
# save hello model and a sample of hello test dataset 

logmsg('save serialized version of hello model and a sample of hello test data')
rxSetComputeContext('localpar') 
saveRDS(logitModel, file = "logitModel.rds")
testDF <- head(testDS, 1000)  
saveRDS(testDF    , file = "testDF.rds"    )
list.files()

rxHadoopListFiles(file.path(inputDataDir,''))
rxHadoopListFiles(dataDir)

# stop hello spark engine 
rxStopEngine(sparkCC) 

logmsg('Done.')
elapsed <- (proc.time() - t0)[3]
logmsg(paste('Elapsed time=',sprintf('%6.2f',elapsed),'(sec)\n\n'))
```

## <a name="summary"></a><span data-ttu-id="06fae-190">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="06fae-190">Summary</span></span>

<span data-ttu-id="06fae-191">I den här artikeln visas hur det är möjligt toocombine använda SparkR för datamanipulering med ScaleR för modellen utveckling i Hadoop Spark.</span><span class="sxs-lookup"><span data-stu-id="06fae-191">In this article, we’ve shown how it’s possible toocombine use of SparkR for data manipulation with ScaleR for model development in Hadoop Spark.</span></span> <span data-ttu-id="06fae-192">Det här scenariot måste du underhålla separata Spark-sessioner, kör endast en session i taget och utbyta data via CSV-filer.</span><span class="sxs-lookup"><span data-stu-id="06fae-192">This scenario requires that you maintain separate Spark sessions, only running one session at a time, and exchange data via CSV files.</span></span> <span data-ttu-id="06fae-193">Även om det är enkelt, ska den här processen vara enklare i en kommande R Server-versionen när SparkR och ScaleR kan dela en Spark-session och dela så Spark DataFrames.</span><span class="sxs-lookup"><span data-stu-id="06fae-193">Although straightforward, this process should be even easier in an upcoming R Server release, when SparkR and ScaleR can share a Spark session and so share Spark DataFrames.</span></span>

## <a name="next-steps-and-more-information"></a><span data-ttu-id="06fae-194">Nästa steg och mer information</span><span class="sxs-lookup"><span data-stu-id="06fae-194">Next steps and more information</span></span>

- <span data-ttu-id="06fae-195">Mer information om användning av R Server på Spark finns hello [komma igång-guiden på MSDN](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started)</span><span class="sxs-lookup"><span data-stu-id="06fae-195">For more information on use of R Server on Spark, see hello [Getting started guide on MSDN](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started)</span></span>

- <span data-ttu-id="06fae-196">Allmän information om R Server finns hello [Kom igång med R](https://msdn.microsoft.com/microsoft-r/microsoft-r-get-started-node) artikel.</span><span class="sxs-lookup"><span data-stu-id="06fae-196">For general information on R Server, see hello [Get started with R](https://msdn.microsoft.com/microsoft-r/microsoft-r-get-started-node) article.</span></span>

- <span data-ttu-id="06fae-197">Mer information om R Server på HDInsight finns [R Server på Azure HDInsight översikt](hdinsight-hadoop-r-server-overview.md) och [R Server på Azure HDInsight](hdinsight-hadoop-r-server-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="06fae-197">For information on R Server on HDInsight, see [R Server on Azure HDInsight overview](hdinsight-hadoop-r-server-overview.md) and [R Server on Azure HDInsight](hdinsight-hadoop-r-server-get-started.md).</span></span>

<span data-ttu-id="06fae-198">Mer information om användning av SparkR finns:</span><span class="sxs-lookup"><span data-stu-id="06fae-198">For more information on use of SparkR, see:</span></span>

- [<span data-ttu-id="06fae-199">Apache SparkR dokument</span><span class="sxs-lookup"><span data-stu-id="06fae-199">Apache SparkR document</span></span>](https://spark.apache.org/docs/2.1.0/sparkr.html)

- <span data-ttu-id="06fae-200">[Översikt över SparkR](https://docs.databricks.com/spark/latest/sparkr/overview.html) från Databricks</span><span class="sxs-lookup"><span data-stu-id="06fae-200">[SparkR Overview](https://docs.databricks.com/spark/latest/sparkr/overview.html) from Databricks</span></span>
