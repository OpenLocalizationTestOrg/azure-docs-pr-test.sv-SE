---
title: Serialisera data i Hadoop - Microsoft Avro Library - Azure | Microsoft Docs
description: "Lär dig mer om att serialisera och deserialisera data i Hadoop i HDInsight med hjälp av Microsoft Avro Library ska sparas i minnet, en databas eller fil."
keywords: avro hadoop avro
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: c78dc20d-5d8d-4366-94ac-abbe89aaac58
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: jgao
ms.custom: hdiseo17may2017
ms.openlocfilehash: d06bf8ff4a21e4f4b29593bac32bfa2b32601fc4
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="serialize-data-in-hadoop-with-the-microsoft-avro-library"></a><span data-ttu-id="cfd60-104">Serialisera data i Hadoop med Microsoft Avro Library</span><span class="sxs-lookup"><span data-stu-id="cfd60-104">Serialize data in Hadoop with the Microsoft Avro Library</span></span>

>[!NOTE]
><span data-ttu-id="cfd60-105">Avro SDK stöds inte längre av Microsoft.</span><span class="sxs-lookup"><span data-stu-id="cfd60-105">The Avro SDK is no longer supported by Microsoft.</span></span> <span data-ttu-id="cfd60-106">Biblioteket är öppen källkod som stöds.</span><span class="sxs-lookup"><span data-stu-id="cfd60-106">The library is open source community supported.</span></span> <span data-ttu-id="cfd60-107">Källor för biblioteket finns i [Github](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro).</span><span class="sxs-lookup"><span data-stu-id="cfd60-107">The sources for the library are located in [Github](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro).</span></span>

<span data-ttu-id="cfd60-108">Det här avsnittet visar hur du använder den [Microsoft Avro Library](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro) att serialisera objekt och andra datastrukturer i dataströmmar att behålla dem till minne, en databas eller en fil.</span><span class="sxs-lookup"><span data-stu-id="cfd60-108">This topic shows how to use the [Microsoft Avro Library](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro) to serialize objects and other data structures into streams to persist them to memory, a database, or a file.</span></span> <span data-ttu-id="cfd60-109">Här visas också att deserialisera dem om du vill återställa de ursprungliga objekten.</span><span class="sxs-lookup"><span data-stu-id="cfd60-109">It also shows how to deserialize them to recover the original objects.</span></span>

[!INCLUDE [windows-only](../../includes/hdinsight-windows-only.md)]

## <a name="apache-avro"></a><span data-ttu-id="cfd60-110">Apache Avro</span><span class="sxs-lookup"><span data-stu-id="cfd60-110">Apache Avro</span></span>
<span data-ttu-id="cfd60-111">Den <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsoft Avro Library</a> implementerar Apache Avros data serialisering system för Microsoft.NET-miljö.</span><span class="sxs-lookup"><span data-stu-id="cfd60-111">The <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsoft Avro Library</a> implements the Apache Avro data serialization system for the Microsoft.NET environment.</span></span> <span data-ttu-id="cfd60-112">Apache Avro ger en kompakta binära dataöverföringsformat för serialisering.</span><span class="sxs-lookup"><span data-stu-id="cfd60-112">Apache Avro provides a compact binary data interchange format for serialization.</span></span> <span data-ttu-id="cfd60-113">Den använder <a href="http://www.json.org" target="_blank">JSON</a> att definiera ett språkoberoende schema som meddelar försäkringar språksamverkan.</span><span class="sxs-lookup"><span data-stu-id="cfd60-113">It uses <a href="http://www.json.org" target="_blank">JSON</a> to define a language-agnostic schema that underwrites language interoperability.</span></span> <span data-ttu-id="cfd60-114">Data serialiseras på ett språk kan läsas i en annan.</span><span class="sxs-lookup"><span data-stu-id="cfd60-114">Data serialized in one language can be read in another.</span></span> <span data-ttu-id="cfd60-115">Stöds för närvarande C, C++, C#, Java, PHP, Python och Ruby.</span><span class="sxs-lookup"><span data-stu-id="cfd60-115">Currently C, C++, C#, Java, PHP, Python, and Ruby are supported.</span></span> <span data-ttu-id="cfd60-116">Detaljerad information om formatet finns i den <a href="http://avro.apache.org/docs/current/spec.html" target="_blank">specifikation för Apache Avro</a>.</span><span class="sxs-lookup"><span data-stu-id="cfd60-116">Detailed information on the format can be found in the <a href="http://avro.apache.org/docs/current/spec.html" target="_blank">Apache Avro Specification</a>.</span></span> 

>[!NOTE]
><span data-ttu-id="cfd60-117">Microsoft Avro Library stöder inte RPC-anrop (RPC-anrop) en del av den här specifikationen.</span><span class="sxs-lookup"><span data-stu-id="cfd60-117">The Microsoft Avro Library does not support the remote procedure calls (RPCs) part of this specification.</span></span>
>

<span data-ttu-id="cfd60-118">Den serialiserade representationen av ett objekt i systemet Avro består av två delar: schemat och faktiskt värde.</span><span class="sxs-lookup"><span data-stu-id="cfd60-118">The serialized representation of an object in the Avro system consists of two parts: schema and actual value.</span></span> <span data-ttu-id="cfd60-119">Avro-schemats beskriver språkoberoende datamodellen serialiserade data med JSON.</span><span class="sxs-lookup"><span data-stu-id="cfd60-119">The Avro schema describes the language-independent data model of the serialized data with JSON.</span></span> <span data-ttu-id="cfd60-120">Den visas sida vid sida med en a binär representation av data.</span><span class="sxs-lookup"><span data-stu-id="cfd60-120">It is presented side by side with a binary representation of data.</span></span> <span data-ttu-id="cfd60-121">Med schemat separat från binär representation tillåter varje objekt som ska skrivas med ingen per värde omkostnader, gör serialisering snabb och representationen små.</span><span class="sxs-lookup"><span data-stu-id="cfd60-121">Having the schema separate from the binary representation permits each object to be written with no per-value overheads, making serialization fast, and the representation small.</span></span>

## <a name="the-hadoop-scenario"></a><span data-ttu-id="cfd60-122">Hadoop-scenario</span><span class="sxs-lookup"><span data-stu-id="cfd60-122">The Hadoop scenario</span></span>
<span data-ttu-id="cfd60-123">Apache Avro-serialiseringsformatet används ofta i Azure HDInsight och andra Apache Hadoop-miljöer.</span><span class="sxs-lookup"><span data-stu-id="cfd60-123">The Apache Avro serialization format is widely used in Azure HDInsight and other Apache Hadoop environments.</span></span> <span data-ttu-id="cfd60-124">Avro är ett bekvämt sätt att representera komplicerade datastrukturer inom ett Hadoop-MapReduce-jobb.</span><span class="sxs-lookup"><span data-stu-id="cfd60-124">Avro provides a convenient way to represent complex data structures within a Hadoop MapReduce job.</span></span> <span data-ttu-id="cfd60-125">Formatet för Avro-filernas (Avro objektet behållaren-fil) har utformats för att stödja distribuerade MapReduce-programmeringsmodellen.</span><span class="sxs-lookup"><span data-stu-id="cfd60-125">The format of Avro files (Avro object container file) has been designed to support the distributed MapReduce programming model.</span></span> <span data-ttu-id="cfd60-126">Nyckelegenskap som gör att distributionen är att filerna är ”delbara” i den mening att söka efter valfri punkt i en fil och börja läsa från ett visst block.</span><span class="sxs-lookup"><span data-stu-id="cfd60-126">The key feature that enables the distribution is that the files are “splittable” in the sense that one can seek any point in a file and start reading from a particular block.</span></span>

## <a name="serialization-in-avro-library"></a><span data-ttu-id="cfd60-127">Serialisering i Avro Library</span><span class="sxs-lookup"><span data-stu-id="cfd60-127">Serialization in Avro Library</span></span>
<span data-ttu-id="cfd60-128">.NET-bibliotek för Avro stöder serialisering objekt på två sätt:</span><span class="sxs-lookup"><span data-stu-id="cfd60-128">The .NET Library for Avro supports two ways of serializing objects:</span></span>

* <span data-ttu-id="cfd60-129">**reflektion** -JSON-schema för typer skapas automatiskt från data kontraktet attribut för .NET-typer för att kunna serialiseras.</span><span class="sxs-lookup"><span data-stu-id="cfd60-129">**reflection** - The JSON schema for the types is automatically built from the data contract attributes of the .NET types to be serialized.</span></span>
* <span data-ttu-id="cfd60-130">**allmän post** -A JSON-schema är explicit i en post som representeras av den [ **AvroRecord** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) klassen när det finns inga .NET-typer för att beskriva schemat för att data kan serialiserad.</span><span class="sxs-lookup"><span data-stu-id="cfd60-130">**generic record** - A JSON schema is explicitly specified in a record represented by the [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) class when no .NET types are present to describe the schema for the data to be serialized.</span></span>

<span data-ttu-id="cfd60-131">När dataschemat är känt både skrivare och reader strömmens, kan data skickas utan ett schema.</span><span class="sxs-lookup"><span data-stu-id="cfd60-131">When the data schema is known to both the writer and reader of the stream, the data can be sent without its schema.</span></span> <span data-ttu-id="cfd60-132">I fall när en Avro objektet behållare fil används lagras schemat i filen.</span><span class="sxs-lookup"><span data-stu-id="cfd60-132">In cases when an Avro object container file is used, the schema is stored within the file.</span></span> <span data-ttu-id="cfd60-133">Andra parametrar, till exempel den codec som används för datakomprimering, kan anges.</span><span class="sxs-lookup"><span data-stu-id="cfd60-133">Other parameters, such as the codec used for data compression, can be specified.</span></span> <span data-ttu-id="cfd60-134">Dessa scenarier beskrivs i detalj och visas i följande kodexempel:</span><span class="sxs-lookup"><span data-stu-id="cfd60-134">These scenarios are outlined in more detail and illustrated in the following code examples:</span></span>

## <a name="install-avro-library"></a><span data-ttu-id="cfd60-135">Installera Avro Library</span><span class="sxs-lookup"><span data-stu-id="cfd60-135">Install Avro Library</span></span>
<span data-ttu-id="cfd60-136">Följande krävs innan du installerar biblioteket:</span><span class="sxs-lookup"><span data-stu-id="cfd60-136">The following are required before you install the library:</span></span>

* <span data-ttu-id="cfd60-137"><a href="http://www.microsoft.com/download/details.aspx?id=17851" target="_blank">Microsoft .NET Framework 4</a></span><span class="sxs-lookup"><span data-stu-id="cfd60-137"><a href="http://www.microsoft.com/download/details.aspx?id=17851" target="_blank">Microsoft .NET Framework 4</a></span></span>
* <span data-ttu-id="cfd60-138"><a href="http://james.newtonking.com/json" target="_blank">Newtonsoft Json.NET</a> (6.0.4 eller senare)</span><span class="sxs-lookup"><span data-stu-id="cfd60-138"><a href="http://james.newtonking.com/json" target="_blank">Newtonsoft Json.NET</a> (6.0.4 or later)</span></span>

<span data-ttu-id="cfd60-139">Observera att Newtonsoft.Json.dll beroendet hämtas automatiskt med installationen av Microsoft Avro Library.</span><span class="sxs-lookup"><span data-stu-id="cfd60-139">Note that the Newtonsoft.Json.dll dependency is downloaded automatically with the installation of the Microsoft Avro Library.</span></span> <span data-ttu-id="cfd60-140">Proceduren finns i följande avsnitt:</span><span class="sxs-lookup"><span data-stu-id="cfd60-140">The procedure is provided in the following section:</span></span>

<span data-ttu-id="cfd60-141">Microsoft Avro Library distribueras som ett NuGet-paket som kan installeras från Visual Studio via följande procedur:</span><span class="sxs-lookup"><span data-stu-id="cfd60-141">The Microsoft Avro Library is distributed as a NuGet package that can be installed from Visual Studio via the following procedure:</span></span>

1. <span data-ttu-id="cfd60-142">Välj den **projekt** fliken -> **hantera NuGet-paket...**</span><span class="sxs-lookup"><span data-stu-id="cfd60-142">Select the **Project** tab -> **Manage NuGet Packages...**</span></span>
2. <span data-ttu-id="cfd60-143">Sök efter ”Microsoft.Hadoop.Avro” i den **Sök Online** rutan.</span><span class="sxs-lookup"><span data-stu-id="cfd60-143">Search for "Microsoft.Hadoop.Avro" in the **Search Online** box.</span></span>
3. <span data-ttu-id="cfd60-144">Klicka på den **installera** knappen bredvid **Microsoft Azure HDInsight Avro Library**.</span><span class="sxs-lookup"><span data-stu-id="cfd60-144">Click the **Install** button next to **Microsoft Azure HDInsight Avro Library**.</span></span>

<span data-ttu-id="cfd60-145">Observera att Newtonsoft.Json.dll (> = 6.0.4) beroende hämtas även automatiskt med Microsoft Avro Library.</span><span class="sxs-lookup"><span data-stu-id="cfd60-145">Note that the Newtonsoft.Json.dll (>=6.0.4) dependency is also downloaded automatically with the Microsoft Avro Library.</span></span>

<span data-ttu-id="cfd60-146">Källkoden Microsoft Avro Library finns på [Github](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro).</span><span class="sxs-lookup"><span data-stu-id="cfd60-146">The Microsoft Avro Library source code is available at [Github](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro).</span></span>

## <a name="compile-schemas-using-avro-library"></a><span data-ttu-id="cfd60-147">Kompilera scheman med Avro Library</span><span class="sxs-lookup"><span data-stu-id="cfd60-147">Compile schemas using Avro Library</span></span>
<span data-ttu-id="cfd60-148">Microsoft Avro Library innehåller ett code generation verktyg som kan skapa C# typer automatiskt baserat på den tidigare definierad JSON-scheman.</span><span class="sxs-lookup"><span data-stu-id="cfd60-148">The Microsoft Avro Library contains a code generation utility that allows creating C# types automatically based on the previously defined JSON schema.</span></span> <span data-ttu-id="cfd60-149">Verktyget code generation distribueras inte som en binär körbar fil, men är lätta att bygga via följande procedur:</span><span class="sxs-lookup"><span data-stu-id="cfd60-149">The code generation utility is not distributed as a binary executable, but can be easily built via the following procedure:</span></span>

1. <span data-ttu-id="cfd60-150">Hämta ZIP-filen med den senaste versionen av HDInsight SDK källkod från <a href="http://hadoopsdk.codeplex.com/SourceControl/latest#" target="_blank">Microsoft .NET SDK för Hadoop</a>.</span><span class="sxs-lookup"><span data-stu-id="cfd60-150">Download the .zip file with the latest version of HDInsight SDK source code from <a href="http://hadoopsdk.codeplex.com/SourceControl/latest#" target="_blank">Microsoft .NET SDK For Hadoop</a>.</span></span> <span data-ttu-id="cfd60-151">(Klicka på den **hämta** ikonen, inte den **hämtar** fliken.)</span><span class="sxs-lookup"><span data-stu-id="cfd60-151">(Click the **Download** icon, not the **Downloads** tab.)</span></span>
2. <span data-ttu-id="cfd60-152">Extrahera HDInsight SDK till en katalog på datorn med .NET Framework 4 installeras och ansluten till Internet för att ladda ned nödvändiga beroendet NuGet-paket.</span><span class="sxs-lookup"><span data-stu-id="cfd60-152">Extract the HDInsight SDK to a directory on the machine with .NET Framework 4 installed and connected to the Internet for downloading necessary dependency NuGet packages.</span></span> <span data-ttu-id="cfd60-153">Nedan, förutsätter vi att källkoden har extraherats till C:\SDK.</span><span class="sxs-lookup"><span data-stu-id="cfd60-153">Below, we assume that the source code is extracted to C:\SDK.</span></span>
3. <span data-ttu-id="cfd60-154">Gå till mappen C:\SDK\src\Microsoft.Hadoop.Avro.Tools och kör build.bat.</span><span class="sxs-lookup"><span data-stu-id="cfd60-154">Go to the folder C:\SDK\src\Microsoft.Hadoop.Avro.Tools and run build.bat.</span></span> <span data-ttu-id="cfd60-155">(Filen anropar MSBuild från 32-bitars fördelningen av .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="cfd60-155">(The file calls MSBuild from the 32-bit distribution of the .NET Framework.</span></span> <span data-ttu-id="cfd60-156">Om du vill använda 64-bitarsversionen redigera build.bat, efter kommentarerna i filen.) Kontrollera att versionen har lyckats.</span><span class="sxs-lookup"><span data-stu-id="cfd60-156">If you would like to use the 64-bit version, edit build.bat, following the comments inside the file.) Ensure that the build is successful.</span></span> <span data-ttu-id="cfd60-157">(På vissa datorer MSBuild kan generera varningar.</span><span class="sxs-lookup"><span data-stu-id="cfd60-157">(On some systems, MSBuild may produce warnings.</span></span> <span data-ttu-id="cfd60-158">Dessa varningar påverkar inte verktyget så länge det finns inga build-fel.)</span><span class="sxs-lookup"><span data-stu-id="cfd60-158">These warnings do not affect the utility as long as there are no build errors.)</span></span>
4. <span data-ttu-id="cfd60-159">Den kompilerade som finns i C:\SDK\Bin\Unsigned\Release\Microsoft.Hadoop.Avro.Tools.</span><span class="sxs-lookup"><span data-stu-id="cfd60-159">The compiled utility is located in C:\SDK\Bin\Unsigned\Release\Microsoft.Hadoop.Avro.Tools.</span></span>

<span data-ttu-id="cfd60-160">Om du vill bekanta dig med att kommandoradssyntaxen, kör du följande kommando från mappen där verktyget code generation finns:`Microsoft.Hadoop.Avro.Tools help /c:codegen`</span><span class="sxs-lookup"><span data-stu-id="cfd60-160">To get familiar with the command-line syntax, execute the following command from the folder where the code generation utility is located: `Microsoft.Hadoop.Avro.Tools help /c:codegen`</span></span>

<span data-ttu-id="cfd60-161">Du kan skapa C#-klasser från JSON-schema exempelfilen medföljer källkoden för att testa verktyget.</span><span class="sxs-lookup"><span data-stu-id="cfd60-161">To test the utility, you can generate C# classes from the sample JSON schema file provided with the source code.</span></span> <span data-ttu-id="cfd60-162">Kör följande kommando:</span><span class="sxs-lookup"><span data-stu-id="cfd60-162">Execute the following command:</span></span>

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:

<span data-ttu-id="cfd60-163">Detta ska skapa två C#-filer i den aktuella katalogen: SensorData.cs och Location.cs.</span><span class="sxs-lookup"><span data-stu-id="cfd60-163">This is supposed to produce two C# files in the current directory: SensorData.cs and Location.cs.</span></span>

<span data-ttu-id="cfd60-164">För att förstå den logik som använder verktyget code generation vid konvertering av JSON-schema för C#-typer finns i filen GenerationVerification.feature i C:\SDK\src\Microsoft.Hadoop.Avro.Tools\Doc.</span><span class="sxs-lookup"><span data-stu-id="cfd60-164">To understand the logic that the code generation utility is using while converting the JSON schema to C# types, see the file GenerationVerification.feature located in C:\SDK\src\Microsoft.Hadoop.Avro.Tools\Doc.</span></span>

<span data-ttu-id="cfd60-165">Namnområden extraheras från JSON-schema med hjälp av logiken i filen som nämns i föregående stycke.</span><span class="sxs-lookup"><span data-stu-id="cfd60-165">Namespaces are extracted from the JSON schema, using the logic described in the file mentioned in the previous paragraph.</span></span> <span data-ttu-id="cfd60-166">Namnområden som extraheras från schemat företräde framför allt har angetts med parametern/n på kommandoraden för verktyget.</span><span class="sxs-lookup"><span data-stu-id="cfd60-166">Namespaces extracted from the schema take precedence over whatever is provided with the /n parameter in the utility command line.</span></span> <span data-ttu-id="cfd60-167">Använd parametern /nf om du vill åsidosätta de namnområden som finns i schemat.</span><span class="sxs-lookup"><span data-stu-id="cfd60-167">If you want to override the namespaces contained within the schema, use the /nf parameter.</span></span> <span data-ttu-id="cfd60-168">Till exempel för att ändra alla namnområden från SampleJSONSchema.avsc till my.own.nspace, kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="cfd60-168">For example, to change all namespaces from the SampleJSONSchema.avsc to my.own.nspace, execute the following command:</span></span>

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:. /nf:my.own.nspace

## <a name="about-the-samples"></a><span data-ttu-id="cfd60-169">Om exemplen</span><span class="sxs-lookup"><span data-stu-id="cfd60-169">About the samples</span></span>
<span data-ttu-id="cfd60-170">Sex exemplen i det här avsnittet visar olika scenarier som stöds av Microsoft Avro Library.</span><span class="sxs-lookup"><span data-stu-id="cfd60-170">Six examples provided in this topic illustrate different scenarios supported by the Microsoft Avro Library.</span></span> <span data-ttu-id="cfd60-171">Microsoft Avro Library är avsedd att fungera med en dataström.</span><span class="sxs-lookup"><span data-stu-id="cfd60-171">The Microsoft Avro Library is designed to work with any stream.</span></span> <span data-ttu-id="cfd60-172">I det här exemplet ändras data via minne strömmar i stället filen dataströmmar eller databaser för enkelhetens skull.</span><span class="sxs-lookup"><span data-stu-id="cfd60-172">In these examples, data is manipulated via memory streams rather than file streams or databases for simplicity and consistency.</span></span> <span data-ttu-id="cfd60-173">Den metod som användes i en produktionsmiljö beror på de exakta scenariokrav datakälla och volymen, prestanda begränsningar och andra faktorer.</span><span class="sxs-lookup"><span data-stu-id="cfd60-173">The approach taken in a production environment depends on the exact scenario requirements, data source and volume, performance constraints, and other factors.</span></span>

<span data-ttu-id="cfd60-174">De första två exemplen visar hur du serialisera och deserialisera data i dataströmmen minnesbuffertar med hjälp av reflektion och allmänna poster.</span><span class="sxs-lookup"><span data-stu-id="cfd60-174">The first two examples show how to serialize and deserialize data into memory stream buffers by using reflection and generic records.</span></span> <span data-ttu-id="cfd60-175">Schemat i båda fallen antas delas mellan läsare och skrivare för out-of-band.</span><span class="sxs-lookup"><span data-stu-id="cfd60-175">The schema in these two cases is assumed to be shared between the readers and writers out-of-band.</span></span>

<span data-ttu-id="cfd60-176">Tredje och fjärde exemplen visar hur du serialisera och deserialisera data med hjälp av behållarfiler för Avro-objektet.</span><span class="sxs-lookup"><span data-stu-id="cfd60-176">The third and fourth examples show how to serialize and deserialize data by using the Avro object container files.</span></span> <span data-ttu-id="cfd60-177">När data lagras i en fil för Avro-behållare, lagras dess schema alltid med det eftersom schemat måste vara delad för deserialisering.</span><span class="sxs-lookup"><span data-stu-id="cfd60-177">When data is stored in an Avro container file, its schema is always stored with it because the schema must be shared for deserialization.</span></span>

<span data-ttu-id="cfd60-178">Provet som innehåller de fyra första exemplen kan hämtas från den <a href="http://code.msdn.microsoft.com/Serialize-data-with-the-86055923" target="_blank">Azure kodexempel</a> plats.</span><span class="sxs-lookup"><span data-stu-id="cfd60-178">The sample containing the first four examples can be downloaded from the <a href="http://code.msdn.microsoft.com/Serialize-data-with-the-86055923" target="_blank">Azure code samples</a> site.</span></span>

<span data-ttu-id="cfd60-179">Femte exempel visas hur du använder en anpassad komprimerings-codec för behållarfiler för Avro-objektet.</span><span class="sxs-lookup"><span data-stu-id="cfd60-179">The fifth example shows how to use a custom compression codec for Avro object container files.</span></span> <span data-ttu-id="cfd60-180">Ett exempel som innehåller koden för det här exemplet kan hämtas från den <a href="http://code.msdn.microsoft.com/Serialize-data-with-the-67159111" target="_blank">Azure kodexempel</a> plats.</span><span class="sxs-lookup"><span data-stu-id="cfd60-180">A sample containing the code for this example can be downloaded from the <a href="http://code.msdn.microsoft.com/Serialize-data-with-the-67159111" target="_blank">Azure code samples</a> site.</span></span>

<span data-ttu-id="cfd60-181">I sjätte exempel visas hur du använder Avro-serialisering för att överföra data till Azure Blob storage och analysera den med hjälp av Hive med HDInsight (Hadoop)-kluster.</span><span class="sxs-lookup"><span data-stu-id="cfd60-181">The sixth sample shows how to use Avro serialization to upload data to Azure Blob storage and then analyze it by using Hive with an HDInsight (Hadoop) cluster.</span></span> <span data-ttu-id="cfd60-182">Du kan hämta från den <a href="https://code.msdn.microsoft.com/Using-Avro-to-upload-data-ae81b1e3" target="_blank">Azure kodexempel</a> plats.</span><span class="sxs-lookup"><span data-stu-id="cfd60-182">It can be downloaded from the <a href="https://code.msdn.microsoft.com/Using-Avro-to-upload-data-ae81b1e3" target="_blank">Azure code samples</a> site.</span></span>

<span data-ttu-id="cfd60-183">Här är länkar till sex exemplen beskrivs i avsnittet:</span><span class="sxs-lookup"><span data-stu-id="cfd60-183">Here are links to the six samples discussed in the topic:</span></span>

* <span data-ttu-id="cfd60-184"><a href="#Scenario1">**Serialisering med reflektion** </a> -JSON-schema att kunna serialiseras skapas automatiskt från data kontraktet attribut.</span><span class="sxs-lookup"><span data-stu-id="cfd60-184"><a href="#Scenario1">**Serialization with reflection**</a> - The JSON schema for types to be serialized is automatically built from the data contract attributes.</span></span>
* <span data-ttu-id="cfd60-185"><a href="#Scenario2">**Serialisering med allmän post** </a> -JSON-schema är explicit i en post när ingen .NET-typ är tillgängligt för reflektion.</span><span class="sxs-lookup"><span data-stu-id="cfd60-185"><a href="#Scenario2">**Serialization with generic record**</a> - The JSON schema is explicitly specified in a record when no .NET type is available for reflection.</span></span>
* <span data-ttu-id="cfd60-186"><a href="#Scenario3">**Serialisering med reflektion objektet behållarfiler** </a> -JSON-schema har skapats automatiskt och delade tillsammans med serialiserade data via en Avro objektet behållare fil.</span><span class="sxs-lookup"><span data-stu-id="cfd60-186"><a href="#Scenario3">**Serialization using object container files with reflection**</a> - The JSON schema is automatically built and shared together with the serialized data via an Avro object container file.</span></span>
* <span data-ttu-id="cfd60-187"><a href="#Scenario4">**Serialisering med allmän post objektet behållarfiler** </a> -JSON-schema uttryckligen anges före serialisering och delade tillsammans med data via en Avro objektet behållare fil.</span><span class="sxs-lookup"><span data-stu-id="cfd60-187"><a href="#Scenario4">**Serialization using object container files with generic record**</a> - The JSON schema is explicitly specified before the serialization and shared together with the data via an Avro object container file.</span></span>
* <span data-ttu-id="cfd60-188"><a href="#Scenario5">**Serialisering med en anpassad komprimerings-codec objektet behållarfiler** </a> -exemplet visar hur du skapar en Avro objektet behållare fil med en anpassad .NET-implementering av Deflate data komprimerings-codec.</span><span class="sxs-lookup"><span data-stu-id="cfd60-188"><a href="#Scenario5">**Serialization using object container files with a custom compression codec**</a> - The example shows how to create an Avro object container file with a customized .NET implementation of the Deflate data compression codec.</span></span>
* <span data-ttu-id="cfd60-189"><a href="#Scenario6">**Använder Avro för att överföra data för tjänsten Microsoft Azure HDInsight** </a> -exemplet illustrerar hur Avro serialisering samverkar med HDInsight-tjänst.</span><span class="sxs-lookup"><span data-stu-id="cfd60-189"><a href="#Scenario6">**Using Avro to upload data for the Microsoft Azure HDInsight service**</a> - The example illustrates how Avro serialization interacts with the HDInsight service.</span></span> <span data-ttu-id="cfd60-190">En aktiv Azure-prenumeration och åtkomst till ett Azure HDInsight-kluster krävs för att köra det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="cfd60-190">An active Azure subscription and access to an Azure HDInsight cluster are required to run this example.</span></span>

## <span data-ttu-id="cfd60-191"><a name="Scenario1"></a>Exempel 1: Serialisering med reflektion</span><span class="sxs-lookup"><span data-stu-id="cfd60-191"><a name="Scenario1"></a>Sample 1: Serialization with reflection</span></span>
<span data-ttu-id="cfd60-192">JSON-schema för typer kan automatiskt genereras av Microsoft Avro Library via reflektion från data kontraktet attribut för C#-objekt för att kunna serialiseras.</span><span class="sxs-lookup"><span data-stu-id="cfd60-192">The JSON schema for the types can be automatically built by the Microsoft Avro Library via reflection from the data contract attributes of the C# objects to be serialized.</span></span> <span data-ttu-id="cfd60-193">Microsoft Avro Library skapar en [ **IAvroSeralizer<T>**  ](http://msdn.microsoft.com/library/dn627341.aspx) att identifiera de fält som ska serialiseras.</span><span class="sxs-lookup"><span data-stu-id="cfd60-193">The Microsoft Avro Library creates an [**IAvroSeralizer<T>**](http://msdn.microsoft.com/library/dn627341.aspx) to identify the fields to be serialized.</span></span>

<span data-ttu-id="cfd60-194">I det här exemplet objekt (en **SensorData** klass med en medlem **plats** struct) serialiseras till en dataström med minne och den här dataströmmen i sin tur avserialiseras.</span><span class="sxs-lookup"><span data-stu-id="cfd60-194">In this example, objects (a **SensorData** class with a member **Location** struct) are serialized to a memory stream, and this stream is in turn deserialized.</span></span> <span data-ttu-id="cfd60-195">Resultatet sedan jämfört med den första instansen att bekräfta att den **SensorData** objekt som återställts är identisk med ursprungligt.</span><span class="sxs-lookup"><span data-stu-id="cfd60-195">The result is then compared to the initial instance to confirm that the **SensorData** object recovered is identical to the original.</span></span>

<span data-ttu-id="cfd60-196">Schemat i det här exemplet förutsätts för att delas mellan läsare och skrivare, så Avro objektet behållares format inte krävs.</span><span class="sxs-lookup"><span data-stu-id="cfd60-196">The schema in this example is assumed to be shared between the readers and writers, so the Avro object container format is not required.</span></span> <span data-ttu-id="cfd60-197">Ett exempel på hur du serialisera och deserialisera data i minnesbuffertar med hjälp av reflektion med behållaren objektformatet när schemat måste delas med data som finns <a href="#Scenario3">serialisering med reflektionobjektetbehållarfiler</a>.</span><span class="sxs-lookup"><span data-stu-id="cfd60-197">For an example of how to serialize and deserialize data into memory buffers by using reflection with the object container format when the schema must be shared with the data, see <a href="#Scenario3">Serialization using object container files with reflection</a>.</span></span>

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serialize and deserialize sample data set represented as an object using reflection.
            //No explicit schema definition is required - schema of serialized objects is automatically built.
            public void SerializeDeserializeObjectUsingReflection()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION\n");
                Console.WriteLine("Serializing Sample Data Set...");

                //Create a new AvroSerializer instance and specify a custom serialization strategy AvroDataContractResolver
                //for serializing only properties attributed with DataContract/DateMember
                var avroSerializer = AvroSerializer.Create<SensorData>();

                //Create a memory stream buffer
                using (var buffer = new MemoryStream())
                {
                    //Create a data set by using sample class and struct
                    var expected = new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } };

                    //Serialize the data to the specified stream
                    avroSerializer.Serialize(buffer, expected);


                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Deserialize data from the stream and cast it to the same type used for serialization
                    var actual = avroSerializer.Deserialize(buffer);

                    Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                    //Finally, verify that deserialized data matches the original one
                    bool isEqual = this.Equal(expected, actual);

                    Console.WriteLine("Result of Data Set Identity Comparison is {0}", isEqual);

                }
            }

            //
            //Helper methods
            //

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }



            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample Class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization to memory using reflection
                Sample.SerializeDeserializeObjectUsingReflection();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION
    //
    // Serializing Sample Data Set...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // Result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


## <a name="sample-2-serialization-with-a-generic-record"></a><span data-ttu-id="cfd60-198">Exempel 2: Serialisering med en allmän post</span><span class="sxs-lookup"><span data-stu-id="cfd60-198">Sample 2: Serialization with a generic record</span></span>
<span data-ttu-id="cfd60-199">En JSON-schema kan uttryckligen anges i en allmän post när reflektion inte kan användas eftersom data inte kan representeras via .NET-klasser med ett datakontrakt.</span><span class="sxs-lookup"><span data-stu-id="cfd60-199">A JSON schema can be explicitly specified in a generic record when reflection cannot be used because the data cannot be represented via .NET classes with a data contract.</span></span> <span data-ttu-id="cfd60-200">Den här metoden går långsammare än med hjälp av reflektion.</span><span class="sxs-lookup"><span data-stu-id="cfd60-200">This method is slower than using reflection.</span></span> <span data-ttu-id="cfd60-201">I sådana fall kan schema för data också vara dynamisk, som är inte känd vid kompileringen.</span><span class="sxs-lookup"><span data-stu-id="cfd60-201">In such cases, the schema for the data may also be dynamic, that is, not known at compile time.</span></span> <span data-ttu-id="cfd60-202">Data som visas i form av fil med kommaavgränsade värden (CSV) filer vars schema är okänd tills omvandlas till formatet Avro vid körning är ett exempel på den här typen av dynamisk scenario.</span><span class="sxs-lookup"><span data-stu-id="cfd60-202">Data represented as comma-separated values (CSV) files whose schema is unknown until it is transformed to the Avro format at run time is an example of this sort of dynamic scenario.</span></span>

<span data-ttu-id="cfd60-203">Det här exemplet visar hur du skapar och använder en [ **AvroRecord** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) att explicit ange en JSON-schema, hur du fylla det med data och sedan hur du serialisera och deserialisera det.</span><span class="sxs-lookup"><span data-stu-id="cfd60-203">This example shows how to create and use an [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) to explicitly specify a JSON schema, how to populate it with the data, and then how to serialize and deserialize it.</span></span> <span data-ttu-id="cfd60-204">Resultatet är sedan jämfört med den första instansen att bekräfta att posten återställas är identisk med ursprungligt.</span><span class="sxs-lookup"><span data-stu-id="cfd60-204">The result is then compared to the initial instance to confirm that the record recovered is identical to the original.</span></span>

<span data-ttu-id="cfd60-205">Schemat i det här exemplet förutsätts för att delas mellan läsare och skrivare, så Avro objektet behållares format inte krävs.</span><span class="sxs-lookup"><span data-stu-id="cfd60-205">The schema in this example is assumed to be shared between the readers and writers, so the Avro object container format is not required.</span></span> <span data-ttu-id="cfd60-206">Ett exempel på hur du serialisera och deserialisera data i minnesbuffertar med hjälp av en allmän post med behållaren objektformatet när schemat måste ingå i den serialiserade informationen finns i <a href="#Scenario4">serialisering med objektet behållarfiler med allmän post</a> exempel.</span><span class="sxs-lookup"><span data-stu-id="cfd60-206">For an example of how to serialize and deserialize data into memory buffers by using a generic record with the object container format when the schema must be included with the serialized data, see the <a href="#Scenario4">Serialization using object container files with generic record</a> example.</span></span>

    namespace Microsoft.Hadoop.Avro.Sample
    {
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using System.Runtime.Serialization;
    using Microsoft.Hadoop.Avro.Container;
    using Microsoft.Hadoop.Avro.Schema;
    using Microsoft.Hadoop.Avro;

    //This class contains all methods demonstrating
    //the usage of Microsoft Avro Library
    public class AvroSample
    {

        //Serialize and deserialize sample data set by using a generic record.
        //A generic record is a special class with the schema explicitly defined in JSON.
        //All serialized data should be mapped to the fields of the generic record,
        //which in turn is then serialized.
        public void SerializeDeserializeObjectUsingGenericRecords()
        {
            Console.WriteLine("SERIALIZATION USING GENERIC RECORD\n");
            Console.WriteLine("Defining the Schema and creating Sample Data Set...");

            //Define the schema in JSON
            const string Schema = @"{
                                ""type"":""record"",
                                ""name"":""Microsoft.Hadoop.Avro.Specifications.SensorData"",
                                ""fields"":
                                    [
                                        {
                                            ""name"":""Location"",
                                            ""type"":
                                                {
                                                    ""type"":""record"",
                                                    ""name"":""Microsoft.Hadoop.Avro.Specifications.Location"",
                                                    ""fields"":
                                                        [
                                                            { ""name"":""Floor"", ""type"":""int"" },
                                                            { ""name"":""Room"", ""type"":""int"" }
                                                        ]
                                                }
                                        },
                                        { ""name"":""Value"", ""type"":""bytes"" }
                                    ]
                            }";

            //Create a generic serializer based on the schema
            var serializer = AvroSerializer.CreateGeneric(Schema);
            var rootSchema = serializer.WriterSchema as RecordSchema;

            //Create a memory stream buffer
            using (var stream = new MemoryStream())
            {
                //Create a generic record to represent the data
                dynamic location = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location.Floor = 1;
                location.Room = 243;

                dynamic expected = new AvroRecord(serializer.WriterSchema);
                expected.Location = location;
                expected.Value = new byte[] { 1, 2, 3, 4, 5 };

                Console.WriteLine("Serializing Sample Data Set...");

                //Serialize the data
                serializer.Serialize(stream, expected);

                stream.Seek(0, SeekOrigin.Begin);

                Console.WriteLine("Deserializing Sample Data Set...");

                //Deserialize the data into a generic record
                dynamic actual = serializer.Deserialize(stream);

                Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                //Finally, verify the results
                bool isEqual = expected.Location.Floor.Equals(actual.Location.Floor);
                isEqual = isEqual && expected.Location.Room.Equals(actual.Location.Room);
                isEqual = isEqual && ((byte[])expected.Value).SequenceEqual((byte[])actual.Value);
                Console.WriteLine("Result of Data Set Identity Comparison is {0}", isEqual);
            }
        }

        static void Main()
        {

            string sectionDivider = "---------------------------------------- ";

            //Create an instance of AvroSample class and invoke methods
            //illustrating different serializing approaches
            AvroSample Sample = new AvroSample();

            //Serialization to memory using generic record
            Sample.SerializeDeserializeObjectUsingGenericRecords();

            Console.WriteLine(sectionDivider);
            Console.WriteLine("Press any key to exit.");
            Console.Read();
        }
    }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING GENERIC RECORD
    //
    // Defining the Schema and creating Sample Data Set...
    // Serializing Sample Data Set...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // Result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


## <a name="sample-3-serialization-using-object-container-files-and-serialization-with-reflection"></a><span data-ttu-id="cfd60-207">Exempel 3: Serialisering med reflektion objektet behållarfiler och serialisering</span><span class="sxs-lookup"><span data-stu-id="cfd60-207">Sample 3: Serialization using object container files and serialization with reflection</span></span>
<span data-ttu-id="cfd60-208">Det här exemplet liknar ett scenario för den <a href="#Scenario1"> första exemplet</a>, där schemat implicit har angetts med reflektion.</span><span class="sxs-lookup"><span data-stu-id="cfd60-208">This example is similar to the scenario in the <a href="#Scenario1"> first example</a>, where the schema is implicitly specified with reflection.</span></span> <span data-ttu-id="cfd60-209">Skillnaden är att här antas schemat inte vara kända för läsare som deserializes den.</span><span class="sxs-lookup"><span data-stu-id="cfd60-209">The difference is that here, the schema is not assumed to be known to the reader that deserializes it.</span></span> <span data-ttu-id="cfd60-210">Den **SensorData** objekt som ska serialiseras och deras implicit angivna schemat lagras i en Avro objektet behållaren-fil som representeras av den [ **AvroContainer** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) klass.</span><span class="sxs-lookup"><span data-stu-id="cfd60-210">The **SensorData** objects to be serialized and their implicitly specified schema are stored in an Avro object container file represented by the [**AvroContainer**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) class.</span></span>

<span data-ttu-id="cfd60-211">Data serialiseras i det här exemplet med [ **SequentialWriter<SensorData>**  ](http://msdn.microsoft.com/library/dn627340.aspx) och avserialiserat med [ **SequentialReader<SensorData>**  ](http://msdn.microsoft.com/library/dn627340.aspx).</span><span class="sxs-lookup"><span data-stu-id="cfd60-211">The data is serialized in this example with [**SequentialWriter<SensorData>**](http://msdn.microsoft.com/library/dn627340.aspx) and deserialized with [**SequentialReader<SensorData>**](http://msdn.microsoft.com/library/dn627340.aspx).</span></span> <span data-ttu-id="cfd60-212">Resultatet sedan jämförs med inledande instanser att kontrollera identiteten.</span><span class="sxs-lookup"><span data-stu-id="cfd60-212">The result then is compared to the initial instances to ensure identity.</span></span>

<span data-ttu-id="cfd60-213">Data i filen objektet behållaren komprimeras via standard [ **Deflate** ] [ deflate-100] komprimerings-codec från .NET Framework 4.</span><span class="sxs-lookup"><span data-stu-id="cfd60-213">The data in the object container file is compressed via the default [**Deflate**][deflate-100] compression codec from .NET Framework 4.</span></span> <span data-ttu-id="cfd60-214">Finns det <a href="#Scenario5"> femte exempel</a> i det här avsnittet beskrivs hur du använder en senare och högre version av den [ **Deflate** ] [ deflate-110] komprimerings-codec tillgängligt i .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="cfd60-214">See the <a href="#Scenario5"> fifth example</a> in this topic to learn how to use a more recent and superior version of the [**Deflate**][deflate-110] compression codec available in .NET Framework 4.5.</span></span>

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes the sample data set by using reflection and Avro object container files.
            //Serialized data is compressed with the Deflate codec.
            public void SerializeDeserializeUsingObjectContainersReflection()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION AND AVRO OBJECT CONTAINER FILES\n");

                //Path for Avro object container file
                string path = "AvroSampleReflectionDeflate.avro";

                //Create a data set by using sample class and struct
                var testData = new List<SensorData>
                        {
                            new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } },
                            new SensorData { Value = new byte[] { 6, 7, 8, 9 }, Position = new Location { Room = 244, Floor = 1 } }
                        };

                //Serializing and saving data to file.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Data is compressed using the Deflate codec.
                    using (var w = AvroContainer.CreateWriter<SensorData>(buffer, Codec.Deflate))
                    {
                        using (var writer = new SequentialWriter<SensorData>(w, 24))
                        {
                            // Serialize the data to stream by using the sequential writer
                            testData.ForEach(writer.Write);
                        }
                    }

                    //Save stream to file
                    Console.WriteLine("Saving serialized data to file...");
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing data.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Create a SequentialReader instance for type SensorData, which deserializes all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    using (var reader = new SequentialReader<SensorData>(
                        AvroContainer.CreateReader<SensorData>(buffer, true)))
                    {
                        var results = reader.Objects;

                        //Finally, verify that deserialized data matches the original one
                        Console.WriteLine("Comparing Initial and Deserialized Data Sets...");
                        int count = 1;
                        var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = serialized, actual = deserialized });
                        foreach (var pair in pairs)
                        {
                            bool isEqual = this.Equal(pair.expected, pair.actual);
                            Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual);
                            count++;
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }

            //
            //Helper methods
            //

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading a file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using reflection to Avro object container file
                Sample.SerializeDeserializeUsingObjectContainersReflection();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION AND AVRO OBJECT CONTAINER FILES
    //
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    // For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


## <a name="sample-4-serialization-using-object-container-files-and-serialization-with-generic-record"></a><span data-ttu-id="cfd60-215">Exempel 4: Serialisering med objektet behållarfiler och serialisering med allmän post</span><span class="sxs-lookup"><span data-stu-id="cfd60-215">Sample 4: Serialization using object container files and serialization with generic record</span></span>
<span data-ttu-id="cfd60-216">Det här exemplet liknar ett scenario för den <a href="#Scenario2"> andra exemplet</a>, där schemat uttryckligen anges med JSON.</span><span class="sxs-lookup"><span data-stu-id="cfd60-216">This example is similar to the scenario in the <a href="#Scenario2"> second example</a>, where the schema is explicitly specified with JSON.</span></span> <span data-ttu-id="cfd60-217">Skillnaden är att här antas schemat inte vara kända för läsare som deserializes den.</span><span class="sxs-lookup"><span data-stu-id="cfd60-217">The difference is that here, the schema is not assumed to be known to the reader that deserializes it.</span></span>

<span data-ttu-id="cfd60-218">Datauppsättningen test har samlats in i en lista över [ **AvroRecord** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) objekt via ett uttryckligen definierade JSON-schema, och lagras sedan i en behållare-fil för objektet som representeras av den [  **AvroContainer** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) klass.</span><span class="sxs-lookup"><span data-stu-id="cfd60-218">The test data set is collected into a list of [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) objects via an explicitly defined JSON schema and then stored in an object container file represented by the [**AvroContainer**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) class.</span></span> <span data-ttu-id="cfd60-219">Den här behållaren skapar en skrivare som används för att serialisera data okomprimerade till en dataström i minnet som sparas till en fil.</span><span class="sxs-lookup"><span data-stu-id="cfd60-219">This container file creates a writer that is used to serialize the data, uncompressed, to a memory stream that is then saved to a file.</span></span> <span data-ttu-id="cfd60-220">Den [ **Codec.Null** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.codec.null.aspx) parametern används för att skapa läsaren anger inte komprimeras dessa data.</span><span class="sxs-lookup"><span data-stu-id="cfd60-220">The [**Codec.Null**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.codec.null.aspx) parameter used for creating the reader specifies that this data is not compressed.</span></span>

<span data-ttu-id="cfd60-221">Data läses från filen sedan och avserialiseras till en samling objekt.</span><span class="sxs-lookup"><span data-stu-id="cfd60-221">The data is then read from the file and deserialized into a collection of objects.</span></span> <span data-ttu-id="cfd60-222">Den här samlingen är jämfört med den ursprungliga listan över Avro-poster för att bekräfta att de är identiska.</span><span class="sxs-lookup"><span data-stu-id="cfd60-222">This collection is compared to the initial list of Avro records to confirm that they are identical.</span></span>

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro.Schema;
        using Microsoft.Hadoop.Avro;

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes a sample data set by using a generic record and Avro object container files.
            //Serialized data is not compressed.
            public void SerializeDeserializeUsingObjectContainersGenericRecord()
            {
                Console.WriteLine("SERIALIZATION USING GENERIC RECORD AND AVRO OBJECT CONTAINER FILES\n");

                //Path for Avro object container file
                string path = "AvroSampleGenericRecordNullCodec.avro";

                Console.WriteLine("Defining the Schema and creating Sample Data Set...");

                //Define the schema in JSON
                const string Schema = @"{
                                ""type"":""record"",
                                ""name"":""Microsoft.Hadoop.Avro.Specifications.SensorData"",
                                ""fields"":
                                    [
                                        {
                                            ""name"":""Location"",
                                            ""type"":
                                                {
                                                    ""type"":""record"",
                                                    ""name"":""Microsoft.Hadoop.Avro.Specifications.Location"",
                                                    ""fields"":
                                                        [
                                                            { ""name"":""Floor"", ""type"":""int"" },
                                                            { ""name"":""Room"", ""type"":""int"" }
                                                        ]
                                                }
                                        },
                                        { ""name"":""Value"", ""type"":""bytes"" }
                                    ]
                            }";

                //Create a generic serializer based on the schema
                var serializer = AvroSerializer.CreateGeneric(Schema);
                var rootSchema = serializer.WriterSchema as RecordSchema;

                //Create a generic record to represent the data
                var testData = new List<AvroRecord>();

                dynamic expected1 = new AvroRecord(rootSchema);
                dynamic location1 = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location1.Floor = 1;
                location1.Room = 243;
                expected1.Location = location1;
                expected1.Value = new byte[] { 1, 2, 3, 4, 5 };
                testData.Add(expected1);

                dynamic expected2 = new AvroRecord(rootSchema);
                dynamic location2 = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location2.Floor = 1;
                location2.Room = 244;
                expected2.Location = location2;
                expected2.Value = new byte[] { 6, 7, 8, 9 };
                testData.Add(expected2);

                //Serializing and saving data to file.
                //Create a MemoryStream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Data is not compressed (Null compression codec).
                    using (var writer = AvroContainer.CreateGenericWriter(Schema, buffer, Codec.Null))
                    {
                        using (var streamWriter = new SequentialWriter<object>(writer, 24))
                        {
                            // Serialize the data to stream by using the sequential writer
                            testData.ForEach(streamWriter.Write);
                        }
                    }

                    Console.WriteLine("Saving serialized data to file...");

                    //Save stream to file
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing the data.
                //Create a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Create a SequentialReader instance for type SensorData, which deserializes all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    using (var reader = AvroContainer.CreateGenericReader(buffer))
                    {
                        using (var streamReader = new SequentialReader<object>(reader))
                        {
                            var results = streamReader.Objects;

                            Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                            //Finally, verify the results
                            var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = (dynamic)serialized, actual = (dynamic)deserialized });
                            int count = 1;
                            foreach (var pair in pairs)
                            {
                                bool isEqual = pair.expected.Location.Floor.Equals(pair.actual.Location.Floor);
                                isEqual = isEqual && pair.expected.Location.Room.Equals(pair.actual.Location.Room);
                                isEqual = isEqual && ((byte[])pair.expected.Value).SequenceEqual((byte[])pair.actual.Value);
                                Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual.ToString());
                                count++;
                            }
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }

            //
            //Helper methods
            //

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading a file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using the given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of the AvroSample class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using generic record to Avro object container file
                Sample.SerializeDeserializeUsingObjectContainersGenericRecord();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING GENERIC RECORD AND AVRO OBJECT CONTAINER FILES
    //
    // Defining the Schema and creating Sample Data Set...
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    // For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.




## <a name="sample-5-serialization-using-object-container-files-with-a-custom-compression-codec"></a><span data-ttu-id="cfd60-223">Exempel 5: Serialisering med en anpassad komprimerings-codec objektet behållarfiler</span><span class="sxs-lookup"><span data-stu-id="cfd60-223">Sample 5: Serialization using object container files with a custom compression codec</span></span>
<span data-ttu-id="cfd60-224">Femte exempel visas hur du använder en anpassad komprimerings-codec för behållarfiler för Avro-objektet.</span><span class="sxs-lookup"><span data-stu-id="cfd60-224">The fifth example shows how to use a custom compression codec for Avro object container files.</span></span> <span data-ttu-id="cfd60-225">Ett exempel som innehåller koden för det här exemplet kan hämtas från den [Azure kodexempel](http://code.msdn.microsoft.com/Serialize-data-with-the-67159111) plats.</span><span class="sxs-lookup"><span data-stu-id="cfd60-225">A sample containing the code for this example can be downloaded from the [Azure code samples](http://code.msdn.microsoft.com/Serialize-data-with-the-67159111) site.</span></span>

<span data-ttu-id="cfd60-226">Den [Avro-specifikationen](http://avro.apache.org/docs/current/spec.html#Required+Codecs) tillåter användning av ett valfritt komprimerings-codec (förutom **Null** och **Deflate** standardinställningarna).</span><span class="sxs-lookup"><span data-stu-id="cfd60-226">The [Avro Specification](http://avro.apache.org/docs/current/spec.html#Required+Codecs) allows usage of an optional compression codec (in addition to **Null** and **Deflate** defaults).</span></span> <span data-ttu-id="cfd60-227">Det här exemplet inte implementerar en ny codec, till exempel Snappy (anges som en valfri codec i den [Avro-specifikationen](http://avro.apache.org/docs/current/spec.html#snappy)).</span><span class="sxs-lookup"><span data-stu-id="cfd60-227">This example is not implementing a new codec such as Snappy (mentioned as a supported optional codec in the [Avro Specification](http://avro.apache.org/docs/current/spec.html#snappy)).</span></span> <span data-ttu-id="cfd60-228">Den visar hur du använder .NET Framework 4.5-implementeringen av den [ **Deflate** ] [ deflate-110] codec, vilket ger en bättre komprimeringsalgoritm baserat på den [zlib ](http://zlib.net/) Komprimeringsbiblioteket än versionen som standard .NET Framework 4.</span><span class="sxs-lookup"><span data-stu-id="cfd60-228">It shows how to use the .NET Framework 4.5 implementation of the [**Deflate**][deflate-110] codec, which provides a better compression algorithm based on the [zlib](http://zlib.net/) compression library than the default .NET Framework 4 version.</span></span>

    //
    // This code needs to be compiled with the parameter Target Framework set as ".NET Framework 4.5"
    // to ensure the desired implementation of the Deflate compression algorithm is used.
    // Ensure your C# project is set up accordingly.
    //

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.IO;
        using System.IO.Compression;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        #region Defining objects for serialization
        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }
        #endregion

        #region Defining custom codec based on .NET Framework V.4.5 Deflate
        //Avro.NET codec class contains two methods,
        //GetCompressedStreamOver(Stream uncompressed) and GetDecompressedStreamOver(Stream compressed),
        //which are the key ones for data compression.
        //To enable a custom codec, one needs to implement these methods for the required codec.

        #region Defining Compression and Decompression Streams
        //DeflateStream (class from System.IO.Compression namespace that implements Deflate algorithm)
        //cannot be directly used for Avro because it does not support vital operations like Seek.
        //Thus one needs to implement two classes inherited from stream
        //(one for compressed and one for decompressed stream)
        //that use Deflate compression and implement all required features.
        internal sealed class CompressionStreamDeflate45 : Stream
        {
            private readonly Stream buffer;
            private DeflateStream compressionStream;

            public CompressionStreamDeflate45(Stream buffer)
            {
                Debug.Assert(buffer != null, "Buffer is not allowed to be null.");

                this.compressionStream = new DeflateStream(buffer, CompressionLevel.Fastest, true);
                this.buffer = buffer;
            }

            public override bool CanRead
            {
                get { return this.buffer.CanRead; }
            }

            public override bool CanSeek
            {
                get { return true; }
            }

            public override bool CanWrite
            {
                get { return this.buffer.CanWrite; }
            }

            public override void Flush()
            {
                this.compressionStream.Close();
            }

            public override long Length
            {
                get { return this.buffer.Length; }
            }

            public override long Position
            {
                get
                {
                    return this.buffer.Position;
                }

                set
                {
                    this.buffer.Position = value;
                }
            }

            public override int Read(byte[] buffer, int offset, int count)
            {
                return this.buffer.Read(buffer, offset, count);
            }

            public override long Seek(long offset, SeekOrigin origin)
            {
                return this.buffer.Seek(offset, origin);
            }

            public override void SetLength(long value)
            {
                throw new NotSupportedException();
            }

            public override void Write(byte[] buffer, int offset, int count)
            {
                this.compressionStream.Write(buffer, offset, count);
            }

            protected override void Dispose(bool disposed)
            {
                base.Dispose(disposed);

                if (disposed)
                {
                    this.compressionStream.Dispose();
                    this.compressionStream = null;
                }
            }
        }

        internal sealed class DecompressionStreamDeflate45 : Stream
        {
            private readonly DeflateStream decompressed;

            public DecompressionStreamDeflate45(Stream compressed)
            {
                this.decompressed = new DeflateStream(compressed, CompressionMode.Decompress, true);
            }

            public override bool CanRead
            {
                get { return true; }
            }

            public override bool CanSeek
            {
                get { return true; }
            }

            public override bool CanWrite
            {
                get { return false; }
            }

            public override void Flush()
            {
                this.decompressed.Close();
            }

            public override long Length
            {
                get { return this.decompressed.Length; }
            }

            public override long Position
            {
                get
                {
                    return this.decompressed.Position;
                }

                set
                {
                    throw new NotSupportedException();
                }
            }

            public override int Read(byte[] buffer, int offset, int count)
            {
                return this.decompressed.Read(buffer, offset, count);
            }

            public override long Seek(long offset, SeekOrigin origin)
            {
                throw new NotSupportedException();
            }

            public override void SetLength(long value)
            {
                throw new NotSupportedException();
            }

            public override void Write(byte[] buffer, int offset, int count)
            {
                throw new NotSupportedException();
            }

            protected override void Dispose(bool disposing)
            {
                base.Dispose(disposing);

                if (disposing)
                {
                    this.decompressed.Dispose();
                }
            }
        }
        #endregion

        #region Define Codec
        //Define the actual codec class containing the required methods for manipulating streams:
        //GetCompressedStreamOver(Stream uncompressed) and GetDecompressedStreamOver(Stream compressed).
        //Codec class uses classes for compressed and decompressed streams defined above.
        internal sealed class DeflateCodec45 : Codec
        {

            //We merely use different IMPLEMENTATIONS of Deflate, so CodecName remains "deflate"
            public static readonly string CodecName = "deflate";

            public DeflateCodec45()
                : base(CodecName)
            {
            }

            public override Stream GetCompressedStreamOver(Stream decompressed)
            {
                if (decompressed == null)
                {
                    throw new ArgumentNullException("decompressed");
                }

                return new CompressionStreamDeflate45(decompressed);
            }

            public override Stream GetDecompressedStreamOver(Stream compressed)
            {
                if (compressed == null)
                {
                    throw new ArgumentNullException("compressed");
                }

                return new DecompressionStreamDeflate45(compressed);
            }
        }
        #endregion

        #region Define modified Codec Factory
        //Define modified codec factory to be used in the reader.
        //It catches the attempt to use "Deflate" and provide  a custom codec.
        //For all other cases, it relies on the base class (CodecFactory).
        internal sealed class CodecFactoryDeflate45 : CodecFactory
        {

            public override Codec Create(string codecName)
            {
                if (codecName == DeflateCodec45.CodecName)
                    return new DeflateCodec45();
                else
                    return base.Create(codecName);
            }
        }
        #endregion

        #endregion

        #region Sample Class with demonstration methods
        //This class contains methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes sample data set by using reflection and Avro object container files.
            //Serialized data is compressed with the custom compression codec (Deflate of .NET Framework 4.5).
            //
            //This sample uses memory stream for all operations related to serialization, deserialization and
            //object container manipulation, though file stream could be easily used.
            public void SerializeDeserializeUsingObjectContainersReflectionCustomCodec()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION, AVRO OBJECT CONTAINER FILES AND CUSTOM CODEC\n");

                //Path for Avro object container file
                string path = "AvroSampleReflectionDeflate45.avro";

                //Create a data set by using sample class and struct
                var testData = new List<SensorData>
                        {
                            new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } },
                            new SensorData { Value = new byte[] { 6, 7, 8, 9 }, Position = new Location { Room = 244, Floor = 1 } }
                        };

                //Serializing and saving data to file.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Here the custom codec is introduced. For convenience, the next commented code line shows how to use built-in Deflate.
                    //Note that because the sample deals with different IMPLEMENTATIONS of Deflate, built-in and custom codecs are interchangeable
                    //in read-write operations.
                    //using (var w = AvroContainer.CreateWriter<SensorData>(buffer, Codec.Deflate))
                    using (var w = AvroContainer.CreateWriter<SensorData>(buffer, new DeflateCodec45()))
                    {
                        using (var writer = new SequentialWriter<SensorData>(w, 24))
                        {
                            // Serialize the data to stream using the sequential writer
                            testData.ForEach(writer.Write);
                        }
                    }

                    //Save stream to file
                    Console.WriteLine("Saving serialized data to file...");
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing data.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Because of SequentialReader<T> constructor signature, an AvroSerializerSettings instance is required
                    //when codec factory is explicitly specified.
                    //You may comment the line below if you want to use built-in Deflate (see next comment).
                    AvroSerializerSettings settings = new AvroSerializerSettings();

                    //Create a SequentialReader instance for type SensorData, which deserializes all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    //Here the custom codec factory is introduced.
                    //For convenience, the next commented code line shows how to use built-in Deflate
                    //(no explicit Codec Factory parameter is required in this case).
                    //Note that because the sample deals with different IMPLEMENTATIONS of Deflate, built-in and custom codecs are interchangeable
                    //in read-write operations.
                    //using (var reader = new SequentialReader<SensorData>(AvroContainer.CreateReader<SensorData>(buffer, true)))
                    using (var reader = new SequentialReader<SensorData>(
                        AvroContainer.CreateReader<SensorData>(buffer, true, settings, new CodecFactoryDeflate45())))
                    {
                        var results = reader.Objects;

                        //Finally, verify that deserialized data matches the original one
                        Console.WriteLine("Comparing Initial and Deserialized Data Sets...");
                        bool isEqual;
                        int count = 1;
                        var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = serialized, actual = deserialized });
                        foreach (var pair in pairs)
                        {
                            isEqual = this.Equal(pair.expected, pair.actual);
                            Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual.ToString());
                            count++;
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }
        #endregion

            #region Helper Methods

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }
            #endregion

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample Class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using reflection to Avro object container file using custom codec
                Sample.SerializeDeserializeUsingObjectContainersReflectionCustomCodec();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION, AVRO OBJECT CONTAINER FILES AND CUSTOM CODEC
    //
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    //For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.

## <a name="sample-6-using-avro-to-upload-data-for-the-microsoft-azure-hdinsight-service"></a><span data-ttu-id="cfd60-229">Exempel 6: Använder Avro för att överföra data för tjänsten Microsoft Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="cfd60-229">Sample 6: Using Avro to upload data for the Microsoft Azure HDInsight service</span></span>
<span data-ttu-id="cfd60-230">I sjätte exemplet vissa programmeringstekniker som rör interagerar med tjänsten Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="cfd60-230">The sixth example illustrates some programming techniques related to interacting with the Azure HDInsight service.</span></span> <span data-ttu-id="cfd60-231">Ett exempel som innehåller koden för det här exemplet kan hämtas från den [Azure kodexempel](https://code.msdn.microsoft.com/Using-Avro-to-upload-data-ae81b1e3) plats.</span><span class="sxs-lookup"><span data-stu-id="cfd60-231">A sample containing the code for this example can be downloaded from the [Azure code samples](https://code.msdn.microsoft.com/Using-Avro-to-upload-data-ae81b1e3) site.</span></span>

<span data-ttu-id="cfd60-232">Exemplet utför följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="cfd60-232">The sample does the following tasks:</span></span>

* <span data-ttu-id="cfd60-233">Ansluter till ett befintligt kluster i HDInsight-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="cfd60-233">Connects to an existing HDInsight service cluster.</span></span>
* <span data-ttu-id="cfd60-234">Serialiserar flera CSV-filer och överför resultatet till Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="cfd60-234">Serializes several CSV files and uploads the result to Azure Blob storage.</span></span> <span data-ttu-id="cfd60-235">(CSV-filer ska distribueras tillsammans med exemplet och representera ett utdrag ur AMEX börs historiska data som distribueras av [Infochimps](http://www.infochimps.com/) för perioden 1970 2010.</span><span class="sxs-lookup"><span data-stu-id="cfd60-235">(The CSV files are distributed together with the sample and represent an extract from AMEX Stock historical data distributed by [Infochimps](http://www.infochimps.com/) for the period 1970-2010.</span></span> <span data-ttu-id="cfd60-236">Exemplet läser data från en CSV, konverterar posterna till instanser av den **börs** klassen och Serialiserar dem med hjälp av reflektion.</span><span class="sxs-lookup"><span data-stu-id="cfd60-236">The sample reads CSV file data, converts the records to instances of the **Stock** class, and then serializes them by using reflection.</span></span> <span data-ttu-id="cfd60-237">Typdefinitionen för lager har skapats från ett JSON-schema via verktyget Microsoft Avro Library code generation.</span><span class="sxs-lookup"><span data-stu-id="cfd60-237">Stock type definition is created from a JSON schema via the Microsoft Avro Library code generation utility.</span></span>
* <span data-ttu-id="cfd60-238">Skapar en ny extern tabell som kallas **lagerlista** i Hive och länkar den till data överförs i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="cfd60-238">Creates a new external table called **Stocks** in Hive, and links it to the data uploaded in the previous step.</span></span>
* <span data-ttu-id="cfd60-239">Kör en fråga med hjälp av Hive via den **lagerlista** tabell.</span><span class="sxs-lookup"><span data-stu-id="cfd60-239">Executes a query by using Hive over the **Stocks** table.</span></span>

<span data-ttu-id="cfd60-240">Dessutom utför en rensning procedur exemplet före och efter att ha genomfört större operationer.</span><span class="sxs-lookup"><span data-stu-id="cfd60-240">In addition, the sample performs a clean-up procedure before and after performing major operations.</span></span> <span data-ttu-id="cfd60-241">Alla relaterade Azure Blob-data och mappar tas bort under rensningen, och Hive-tabellen har släppts.</span><span class="sxs-lookup"><span data-stu-id="cfd60-241">During the clean-up, all of the related Azure Blob data and folders are removed, and the Hive table is dropped.</span></span> <span data-ttu-id="cfd60-242">Du kan även anropa proceduren Rensa från kommandoraden exempel.</span><span class="sxs-lookup"><span data-stu-id="cfd60-242">You can also invoke the clean-up procedure from the sample command line.</span></span>

<span data-ttu-id="cfd60-243">Exemplet har följande krav:</span><span class="sxs-lookup"><span data-stu-id="cfd60-243">The sample has the following prerequisites:</span></span>

* <span data-ttu-id="cfd60-244">En aktiv Microsoft Azure-prenumeration och dess prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="cfd60-244">An active Microsoft Azure subscription and its subscription ID.</span></span>
* <span data-ttu-id="cfd60-245">Ett certifikat för prenumerationen med motsvarande privata nyckel.</span><span class="sxs-lookup"><span data-stu-id="cfd60-245">A management certificate for the subscription with the corresponding private key.</span></span> <span data-ttu-id="cfd60-246">Certifikatet ska installeras i den aktuella användaren privat lagringen på datorn som används för att köra exemplet.</span><span class="sxs-lookup"><span data-stu-id="cfd60-246">The certificate should be installed in the current user private storage on the machine used to run the sample.</span></span>
* <span data-ttu-id="cfd60-247">Ett aktivt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="cfd60-247">An active HDInsight cluster.</span></span>
* <span data-ttu-id="cfd60-248">Ett Azure Storage-konto länkas till HDInsight-kluster från de föregående nödvändiga tillsammans med motsvarande primära eller sekundära åtkomstnyckeln.</span><span class="sxs-lookup"><span data-stu-id="cfd60-248">An Azure Storage account linked to the HDInsight cluster from the previous prerequisite, together with the corresponding primary, or secondary access key.</span></span>

<span data-ttu-id="cfd60-249">All information från kraven bör anges till exempelkonfigurationsfilen innan exemplet körs.</span><span class="sxs-lookup"><span data-stu-id="cfd60-249">All of the information from the prerequisites should be entered to the sample configuration file before the sample is run.</span></span> <span data-ttu-id="cfd60-250">Det finns två möjliga sätt att göra det:</span><span class="sxs-lookup"><span data-stu-id="cfd60-250">There are two possible ways to do it:</span></span>

* <span data-ttu-id="cfd60-251">Redigera filen app.config i rotkatalogen exempel och bygga exemplet</span><span class="sxs-lookup"><span data-stu-id="cfd60-251">Edit the app.config file in the sample root directory and then build the sample</span></span>
* <span data-ttu-id="cfd60-252">Bygga exemplet först och sedan redigera AvroHDISample.exe.config i katalogen build</span><span class="sxs-lookup"><span data-stu-id="cfd60-252">First build the sample, and then edit AvroHDISample.exe.config in the build directory</span></span>

<span data-ttu-id="cfd60-253">I båda fallen måste alla redigeringar bör göras i den  **<appSettings>**  inställningar.</span><span class="sxs-lookup"><span data-stu-id="cfd60-253">In both cases, all edits should be done in the **<appSettings>** settings section.</span></span> <span data-ttu-id="cfd60-254">Följ kommentarer i filen.</span><span class="sxs-lookup"><span data-stu-id="cfd60-254">Follow the comments in the file.</span></span>
<span data-ttu-id="cfd60-255">Exemplet körs från kommandoraden genom att köra följande kommando (där ZIP-filen med exemplet antogs extraheras till C:\AvroHDISample; om annars använder relevanta filens sökväg):</span><span class="sxs-lookup"><span data-stu-id="cfd60-255">The sample is run from the command line by executing the following command (where the .zip file with the sample was assumed to be extracted to C:\AvroHDISample; if otherwise, use the relevant file path):</span></span>

    AvroHDISample run C:\AvroHDISample\Data

<span data-ttu-id="cfd60-256">Kör följande kommando för att rensa klustret:</span><span class="sxs-lookup"><span data-stu-id="cfd60-256">To clean up the cluster, run the following command:</span></span>

    AvroHDISample clean

[deflate-100]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.100).aspx
[deflate-110]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.110).aspx
