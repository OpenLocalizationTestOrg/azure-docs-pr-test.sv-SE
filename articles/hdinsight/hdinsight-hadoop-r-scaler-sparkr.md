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
# <a name="combine-scaler-and-sparkr-in-hdinsight"></a>Kombinera ScaleR och SparkR i HDInsight

Den här artikeln visar hur toopredict flight ankomst fördröjningar med hjälp av en **ScaleR** logistic regressionsmodell från data på svarta fördröjningar och väder kopplas till **SparkR**. Det här scenariot visar hello funktionerna i ScaleR för datamanipulering på Spark som används med Microsoft R Server för analys. hello kombination av dessa tekniker kan tooapply hello senaste funktionerna i distribuerad databehandling.

Även om båda paketen körs på Hadoops motorn för körning av Spark, blockeras de från InMemory-Datadelning eftersom de varje kräver sin egen respektive Spark-sessioner. Tills problemet åtgärdas i en kommande version av R-Server, är hello lösningen toomaintain inte överlappar Spark sessioner och tooexchange data via mellanliggande filer. hello anvisningarna här visar att dessa krav är enkla tooachieve.

Vi använder ett exempel här ursprungligen delas i en prata med skikt 2016 av Mario Inchiosa och Roni Burd som även är tillgängliga via hello webbseminariet [skapa en skalbar datavetenskap plattform med R](http://event.on24.com/eventRegistration/console/EventConsoleNG.jsp?uimode=nextgeneration&eventid=1160288&sessionid=1&key=8F8FB9E2EB1AEE867287CD6757D5BD40&contenttype=A&eventuserid=305999&playerwidth=1000&playerheight=650&caller=previewLobby&text_language_id=en&format=fhaudio). hello exemplet används SparkR toojoin hello välkända flygbolagen ankomst fördröjning datauppsättning med väder data vid utgångspunkten och ankomst flygplatser. hello data ansluten används sedan som inkommande tooa ScaleR logistic regressionsmodell för att förutsäga svarta ankomst fördröjning.

Hej kodhandledning vi ursprungligen skrevs för R-Server som körs på Spark i HDInsight-kluster i Azure. Men hello begreppet blanda hello användningen av SparkR och ScaleR i ett skript är också i hello kontext för lokala miljöer. I följande hello, Vi förutsätter att en mellanliggande nivå av R och R hello [ScaleR](https://msdn.microsoft.com/microsoft-r/scaler-user-guide-introduction) bibliotek med R Server. Vi introducerar också användning av [SparkR](https://spark.apache.org/docs/2.1.0/sparkr.html) när via det här scenariot.

## <a name="hello-airline-and-weather-datasets"></a>hello flygbolag och väder datauppsättningar

Hej **AirOnTime08to12CSV** flygbolagen offentliga datamängden innehåller information om svarta ankomst och avvikelse information om alla kommersiella flygplan inom hello USA, från oktober 1987 tooDecember 2012. Det här är en stor datauppsättning: det finns poster som nästan 150 miljoner totalt. Det är bara under 4 GB ska packas upp. Den är tillgänglig från hello [US government Arkiv](http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236). Bekvämare, den är tillgänglig som en zip-fil (AirOnTimeCSV.zip) som innehåller en uppsättning 303 separata månatliga CSV-filer från hello [Revolution Analytics dataset-databasen](http://packages.revolutionanalytics.com/datasets/AirOnTime87to12/)

toosee hello effekterna av väder på svarta fördröjningar, vi måste också hello väder data på varje hello flygplatser. Dessa data kan hämtas som zip-filer i obearbetat format per månad från hello [nationella oceaniskt och luften Administration databasen](http://www.ncdc.noaa.gov/orders/qclcd/). Hello enligt det här exemplet, vi hämtar väder data från maj 2007 – December 2012 och används hello timvis datafiler inom varje hello 68 månatliga komprimerade. hello månatliga zip-filer kan du också innehålla en mappning (YYYYMMstation.txt) mellan hello väder station ID (WBAN), hello flygplats att den är associerad med (CallSign) och hello flygplatss Tidszonförskjutning från UTC (tidszonen). All den här informationen behövs när du ansluter med hello flygbolag fördröjning och väder data.

## <a name="setting-up-hello-spark-environment"></a>Konfigurera hello Spark-miljö

hello första steget är tooset in hello Spark-miljön. Vi börjar med pekar toohello katalog som innehåller våra indata kataloger, skapa en kontext för beräkning av Spark och skapa en loggningen för informativt loggning toohello konsolen:

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

Nästa vi lägga till ”Spark_Home” toohello sökvägen för R-paket så att vi kan använda SparkR och initiera en SparkR session:

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

## <a name="preparing-hello-weather-data"></a>Förbereder hello väder data

tooprepare hello väder data vi delmängd den toohello kolumner som behövs för modellering: 

- ”Synlighet”
- ”DryBulbCelsius”
- ”DewPointCelsius”
- ”RelativeHumidity”
- ”Vindhastigheten”
- ”Altimeter”

Sedan vi lägga till en flygplats kod som är associerad med hello väder station och konvertera hello mätningar från tooUTC lokal tid.

Vi börjar med att skapa en fil toomap hello väder station (WBAN) info tooan flygplats kod. Vi kan hämta detta korrelation från hello mappningsfilen medföljer hello väder data. Med mappning hello *CallSign* (till exempel LAX) fältet i datafilen för hello väder för*ursprung* i hello flygbolag data. Men vi just har hänt toohave en annan mappning till hands som mappar *WBAN* för*AirportID* (till exempel 12892 för LAX) och innehåller *tidszonen* som har sparats tooa CSV-fil som heter ”wban-till-en flygplats-id-tz. CSV-fil ”som vi kan använda. Exempel:

| AirportID | WBAN | Tidszon
|-----------|------|---------
| 10685 | 54831 | -6
| 14871 | 24232 | -8
| .. | .. | ..

hello följande kod läser varje hello timvis rådata väder data filerna delmängder toohello kolumner vi behöver, sammanfogar hello väder station mappningsfilen, justerar hello datum för mätningar tooUTC och skriver sedan ut en ny version av filen hello:

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

## <a name="importing-hello-airline-and-weather-data-toospark-dataframes"></a>Importera hello flygbolag och väder data tooSpark DataFrames

Vi använder inte hello SparkR [read.df()](https://docs.databricks.com/spark/latest/sparkr/functions/read.df.html) tooimport hello väder och flygbolag data tooSpark DataFrames att fungera. Den här funktionen så många Spark-metoder utförs lazy, vilket innebär att de i kö för körning men inte köras förrän krävs.

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

## <a name="data-cleansing-and-transformation"></a>Datarensning och omvandling

Nästa vi göra vissa rensning på hello flygbolag data har vi importerat toorename kolumner. Vi bara hålla hello variabler som behövs och avrunda schemalagda avvikelse gånger ned toohello närmsta timmen tooenable sammanfoga hello senaste väder data på avvikelse:

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

Nu kan vi utföra liknande åtgärder på hello väder data:

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

## <a name="joining-hello-weather-and-airline-data"></a>Koppla hello väder och flygbolag data

Vi använder nu hello SparkR [join()](https://docs.databricks.com/spark/latest/sparkr/functions/join.html) fungerar toodo en vänster yttre koppling av hello flygbolag och väder data av avvikelse AirportID och datetime. hello yttre koppling kan vi tooretain alla hello flygbolag data innehåller även om det finns inga matchande väder-data. Hello koppling vi ta bort vissa redundant kolumner och Byt namn på hello hålls kolumner tooremove hello inkommande DataFrame prefix som introducerats av hello koppling.

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

Vi gå hello väder och flygbolag data baserat på ankomst AirportID och datetime på ett liknande sätt:

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

## <a name="save-results-toocsv-for-exchange-with-scaler"></a>Spara resultaten tooCSV för exchange med ScaleR

Hello kopplingar måste toodo med SparkR är klar. Vi spara hello data från hello slutliga Spark DataFrame ”joinedDF5” tooa CSV för inkommande tooScaleR och stäng sedan ut hello SparkR session. Vi uttryckligen begära SparkR toosave hello gällande CSV i 80 separata partitioner tooenable tillräckligt parallellitet i ScaleR bearbetningen:

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

## <a name="import-tooxdf-for-use-by-scaler"></a>Importera tooXDF för användning av ScaleR

Vi kan använda hello CSV-fil för domänanslutna flygbolag och väder data som-är för modellering via en datakälla för ScaleR text. Men vi importera den tooXDF först, eftersom det är mer effektivt när du kör flera åtgärder på hello datauppsättningen:

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

## <a name="splitting-data-for-training-and-test"></a>Dela data för träning och testning

Vi använder rxDataStep toosplit hello 2012 uppgifter för att testa och hålla hello rest för träning:

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

## <a name="train-and-test-a-logistic-regression-model"></a>Träna och testa en logistic regressionsmodell

Vi är nu redo toobuild en modell. toosee hello inverkan väder data på fördröjning i hello ankomsttid, vi använder Scaler's logistic regression rutinen. Vi använder den toomodel om en ankomst fördröjning på mer än 15 minuter påverkas av hello väder vid hello utgångspunkten och ankomst flygplatser:

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

Nu ska vi se hur den på hello testdata genom att göra vissa förutsägelser och titta ROC och AUC.

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

## <a name="scoring-elsewhere"></a>Bedömningen någon annanstans

Vi kan också använda hello modellen för bedömningsprofil data på en annan plattform. Genom att spara den tooan RDS-filen och överföra och importera den RDS till ett mål som bedömningen miljö, till exempel SQL Server R Services. Det är viktigt tooensure som hello faktor nivåer av hello data toobe bedömas matchar de på vilka hello modellen har skapats. Att matcha kan uppnås genom att extrahera och spara hello kolumninformation som är associerade med hello modellerar data via Scaler's `rxCreateColInfo()` funktionen och sedan använda den kolumnen toohello indata informationskällan för förutsägelse. I följande hello spara några rader i hello testdata vi och extrahera och använda hello kolumninformation från det här exemplet i hello förutsägelse skript:

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

## <a name="summary"></a>Sammanfattning

I den här artikeln visas hur det är möjligt toocombine använda SparkR för datamanipulering med ScaleR för modellen utveckling i Hadoop Spark. Det här scenariot måste du underhålla separata Spark-sessioner, kör endast en session i taget och utbyta data via CSV-filer. Även om det är enkelt, ska den här processen vara enklare i en kommande R Server-versionen när SparkR och ScaleR kan dela en Spark-session och dela så Spark DataFrames.

## <a name="next-steps-and-more-information"></a>Nästa steg och mer information

- Mer information om användning av R Server på Spark finns hello [komma igång-guiden på MSDN](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started)

- Allmän information om R Server finns hello [Kom igång med R](https://msdn.microsoft.com/microsoft-r/microsoft-r-get-started-node) artikel.

- Mer information om R Server på HDInsight finns [R Server på Azure HDInsight översikt](hdinsight-hadoop-r-server-overview.md) och [R Server på Azure HDInsight](hdinsight-hadoop-r-server-get-started.md).

Mer information om användning av SparkR finns:

- [Apache SparkR dokument](https://spark.apache.org/docs/2.1.0/sparkr.html)

- [Översikt över SparkR](https://docs.databricks.com/spark/latest/sparkr/overview.html) från Databricks
