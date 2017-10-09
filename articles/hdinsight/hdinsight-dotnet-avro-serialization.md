---
title: aaaSerialize data i Hadoop - Microsoft Avro Library - Azure | Microsoft Docs
description: "Lär dig hur tooserialize och deserialisera data i Hadoop i HDInsight med hjälp av hello Microsoft Avro Library toopersist toomemory, en databas eller fil."
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
ms.openlocfilehash: f364f8e855a54c0fc160e9a106ec8d5b30c6db23
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="serialize-data-in-hadoop-with-hello-microsoft-avro-library"></a><span data-ttu-id="8574e-104">Serialisera data i Hadoop med hello Microsoft Avro Library</span><span class="sxs-lookup"><span data-stu-id="8574e-104">Serialize data in Hadoop with hello Microsoft Avro Library</span></span>

>[!NOTE]
><span data-ttu-id="8574e-105">Hej Avro SDK stöds inte längre av Microsoft.</span><span class="sxs-lookup"><span data-stu-id="8574e-105">hello Avro SDK is no longer supported by Microsoft.</span></span> <span data-ttu-id="8574e-106">hello-biblioteket är öppen källkod som stöds.</span><span class="sxs-lookup"><span data-stu-id="8574e-106">hello library is open source community supported.</span></span> <span data-ttu-id="8574e-107">hello källor för hello biblioteket finns i [Github](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro).</span><span class="sxs-lookup"><span data-stu-id="8574e-107">hello sources for hello library are located in [Github](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro).</span></span>

<span data-ttu-id="8574e-108">Det här avsnittet visar hur toouse hello [Microsoft Avro Library](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro) tooserialize objekt och andra data strukturer i dataströmmar toopersist dem toomemory, en databas eller en fil.</span><span class="sxs-lookup"><span data-stu-id="8574e-108">This topic shows how toouse hello [Microsoft Avro Library](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro) tooserialize objects and other data structures into streams toopersist them toomemory, a database, or a file.</span></span> <span data-ttu-id="8574e-109">Den visar även hur toodeserialize dem toorecover hello ursprungliga objekten.</span><span class="sxs-lookup"><span data-stu-id="8574e-109">It also shows how toodeserialize them toorecover hello original objects.</span></span>

[!INCLUDE [windows-only](../../includes/hdinsight-windows-only.md)]

## <a name="apache-avro"></a><span data-ttu-id="8574e-110">Apache Avro</span><span class="sxs-lookup"><span data-stu-id="8574e-110">Apache Avro</span></span>
<span data-ttu-id="8574e-111">Hej <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsoft Avro Library</a> implementerar hello Apache Avro system för serialisering för hello Microsoft.NET miljö.</span><span class="sxs-lookup"><span data-stu-id="8574e-111">hello <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsoft Avro Library</a> implements hello Apache Avro data serialization system for hello Microsoft.NET environment.</span></span> <span data-ttu-id="8574e-112">Apache Avro ger en kompakta binära dataöverföringsformat för serialisering.</span><span class="sxs-lookup"><span data-stu-id="8574e-112">Apache Avro provides a compact binary data interchange format for serialization.</span></span> <span data-ttu-id="8574e-113">Den använder <a href="http://www.json.org" target="_blank">JSON</a> toodefine ett språkoberoende schema som meddelar försäkringar språksamverkan.</span><span class="sxs-lookup"><span data-stu-id="8574e-113">It uses <a href="http://www.json.org" target="_blank">JSON</a> toodefine a language-agnostic schema that underwrites language interoperability.</span></span> <span data-ttu-id="8574e-114">Data serialiseras på ett språk kan läsas i en annan.</span><span class="sxs-lookup"><span data-stu-id="8574e-114">Data serialized in one language can be read in another.</span></span> <span data-ttu-id="8574e-115">Stöds för närvarande C, C++, C#, Java, PHP, Python och Ruby.</span><span class="sxs-lookup"><span data-stu-id="8574e-115">Currently C, C++, C#, Java, PHP, Python, and Ruby are supported.</span></span> <span data-ttu-id="8574e-116">Detaljerad information om hello-format finns i hello <a href="http://avro.apache.org/docs/current/spec.html" target="_blank">specifikation för Apache Avro</a>.</span><span class="sxs-lookup"><span data-stu-id="8574e-116">Detailed information on hello format can be found in hello <a href="http://avro.apache.org/docs/current/spec.html" target="_blank">Apache Avro Specification</a>.</span></span> 

>[!NOTE]
><span data-ttu-id="8574e-117">hello Microsoft Avro Library stöder inte hello fjärrproceduranrop (RPC-anrop)-anrop en del av den här specifikationen.</span><span class="sxs-lookup"><span data-stu-id="8574e-117">hello Microsoft Avro Library does not support hello remote procedure calls (RPCs) part of this specification.</span></span>
>

<span data-ttu-id="8574e-118">hello serialiseras representation av ett objekt i hello Avro system består av två delar: schemat och faktiskt värde.</span><span class="sxs-lookup"><span data-stu-id="8574e-118">hello serialized representation of an object in hello Avro system consists of two parts: schema and actual value.</span></span> <span data-ttu-id="8574e-119">Hej Avro-schemats beskriver hello språkoberoende datamodellen hello serialiserade data med JSON.</span><span class="sxs-lookup"><span data-stu-id="8574e-119">hello Avro schema describes hello language-independent data model of hello serialized data with JSON.</span></span> <span data-ttu-id="8574e-120">Den visas sida vid sida med en a binär representation av data.</span><span class="sxs-lookup"><span data-stu-id="8574e-120">It is presented side by side with a binary representation of data.</span></span> <span data-ttu-id="8574e-121">Med hello schemat separat från hello binär representation tillåter varje objekt toobe som skrivits med ingen per värde omkostnader, gör serialisering snabb och hello representation små.</span><span class="sxs-lookup"><span data-stu-id="8574e-121">Having hello schema separate from hello binary representation permits each object toobe written with no per-value overheads, making serialization fast, and hello representation small.</span></span>

## <a name="hello-hadoop-scenario"></a><span data-ttu-id="8574e-122">Hej Hadoop scenario</span><span class="sxs-lookup"><span data-stu-id="8574e-122">hello Hadoop scenario</span></span>
<span data-ttu-id="8574e-123">hello Apache Avro-serialiseringsformatet används ofta i Azure HDInsight och andra Apache Hadoop-miljöer.</span><span class="sxs-lookup"><span data-stu-id="8574e-123">hello Apache Avro serialization format is widely used in Azure HDInsight and other Apache Hadoop environments.</span></span> <span data-ttu-id="8574e-124">Avro ger ett bekvämt sätt toorepresent komplicerade datastrukturer inom ett Hadoop-MapReduce-jobb.</span><span class="sxs-lookup"><span data-stu-id="8574e-124">Avro provides a convenient way toorepresent complex data structures within a Hadoop MapReduce job.</span></span> <span data-ttu-id="8574e-125">hello format Avro-filernas (Avro objektet behållaren-fil) har utformad toosupport hello distribuerade MapReduce-programmeringsmodellen.</span><span class="sxs-lookup"><span data-stu-id="8574e-125">hello format of Avro files (Avro object container file) has been designed toosupport hello distributed MapReduce programming model.</span></span> <span data-ttu-id="8574e-126">hello nyckelegenskap som aktiverar hello fördelningen är att hello filerna är ”delbara” i hello mening att söka efter valfri punkt i en fil och börja läsa från ett visst block.</span><span class="sxs-lookup"><span data-stu-id="8574e-126">hello key feature that enables hello distribution is that hello files are “splittable” in hello sense that one can seek any point in a file and start reading from a particular block.</span></span>

## <a name="serialization-in-avro-library"></a><span data-ttu-id="8574e-127">Serialisering i Avro Library</span><span class="sxs-lookup"><span data-stu-id="8574e-127">Serialization in Avro Library</span></span>
<span data-ttu-id="8574e-128">hello .NET-bibliotek för Avro stöder serialisering objekt på två sätt:</span><span class="sxs-lookup"><span data-stu-id="8574e-128">hello .NET Library for Avro supports two ways of serializing objects:</span></span>

* <span data-ttu-id="8574e-129">**reflektion** -hello JSON-schema för hello typer skapas automatiskt från hello data kontraktet attribut för hello .NET typer toobe serialiseras.</span><span class="sxs-lookup"><span data-stu-id="8574e-129">**reflection** - hello JSON schema for hello types is automatically built from hello data contract attributes of hello .NET types toobe serialized.</span></span>
* <span data-ttu-id="8574e-130">**allmän post** -A JSON-schema är explicit i en post som representeras av hello [ **AvroRecord** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) klassen när inga .NET-typer finns toodescribe hello schemat för hello data toobe serialiserad.</span><span class="sxs-lookup"><span data-stu-id="8574e-130">**generic record** - A JSON schema is explicitly specified in a record represented by hello [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) class when no .NET types are present toodescribe hello schema for hello data toobe serialized.</span></span>

<span data-ttu-id="8574e-131">När hello dataschemat är känt tooboth hello skrivare och läsare hello dataströmmen, skickas hello data utan ett schema.</span><span class="sxs-lookup"><span data-stu-id="8574e-131">When hello data schema is known tooboth hello writer and reader of hello stream, hello data can be sent without its schema.</span></span> <span data-ttu-id="8574e-132">I fall när en Avro objektet behållare fil används hello schemat lagras i hello-filen.</span><span class="sxs-lookup"><span data-stu-id="8574e-132">In cases when an Avro object container file is used, hello schema is stored within hello file.</span></span> <span data-ttu-id="8574e-133">Andra parametrar, till exempel hello codec som används för datakomprimering, kan anges.</span><span class="sxs-lookup"><span data-stu-id="8574e-133">Other parameters, such as hello codec used for data compression, can be specified.</span></span> <span data-ttu-id="8574e-134">Dessa scenarier beskrivs i detalj och visas i följande kodexempel hello:</span><span class="sxs-lookup"><span data-stu-id="8574e-134">These scenarios are outlined in more detail and illustrated in hello following code examples:</span></span>

## <a name="install-avro-library"></a><span data-ttu-id="8574e-135">Installera Avro Library</span><span class="sxs-lookup"><span data-stu-id="8574e-135">Install Avro Library</span></span>
<span data-ttu-id="8574e-136">hello följande krävs innan du installerar hello-biblioteket:</span><span class="sxs-lookup"><span data-stu-id="8574e-136">hello following are required before you install hello library:</span></span>

* <span data-ttu-id="8574e-137"><a href="http://www.microsoft.com/download/details.aspx?id=17851" target="_blank">Microsoft .NET Framework 4</a></span><span class="sxs-lookup"><span data-stu-id="8574e-137"><a href="http://www.microsoft.com/download/details.aspx?id=17851" target="_blank">Microsoft .NET Framework 4</a></span></span>
* <span data-ttu-id="8574e-138"><a href="http://james.newtonking.com/json" target="_blank">Newtonsoft Json.NET</a> (6.0.4 eller senare)</span><span class="sxs-lookup"><span data-stu-id="8574e-138"><a href="http://james.newtonking.com/json" target="_blank">Newtonsoft Json.NET</a> (6.0.4 or later)</span></span>

<span data-ttu-id="8574e-139">Observera att hello Newtonsoft.Json.dll beroende hämtas automatiskt med hello installation av hello Microsoft Avro Library.</span><span class="sxs-lookup"><span data-stu-id="8574e-139">Note that hello Newtonsoft.Json.dll dependency is downloaded automatically with hello installation of hello Microsoft Avro Library.</span></span> <span data-ttu-id="8574e-140">hello proceduren finns i följande avsnitt hello:</span><span class="sxs-lookup"><span data-stu-id="8574e-140">hello procedure is provided in hello following section:</span></span>

<span data-ttu-id="8574e-141">hello Microsoft Avro Library distribueras som ett NuGet-paket som kan installeras från Visual Studio via hello nedan:</span><span class="sxs-lookup"><span data-stu-id="8574e-141">hello Microsoft Avro Library is distributed as a NuGet package that can be installed from Visual Studio via hello following procedure:</span></span>

1. <span data-ttu-id="8574e-142">Välj hello **projekt** fliken -> **hantera NuGet-paket...**</span><span class="sxs-lookup"><span data-stu-id="8574e-142">Select hello **Project** tab -> **Manage NuGet Packages...**</span></span>
2. <span data-ttu-id="8574e-143">Sök efter ”Microsoft.Hadoop.Avro” i hello **Sök Online** rutan.</span><span class="sxs-lookup"><span data-stu-id="8574e-143">Search for "Microsoft.Hadoop.Avro" in hello **Search Online** box.</span></span>
3. <span data-ttu-id="8574e-144">Klicka på hello **installera** knappen för nästa**Microsoft Azure HDInsight Avro Library**.</span><span class="sxs-lookup"><span data-stu-id="8574e-144">Click hello **Install** button next too**Microsoft Azure HDInsight Avro Library**.</span></span>

<span data-ttu-id="8574e-145">Observera att hello Newtonsoft.Json.dll (> = 6.0.4) beroende hämtas även automatiskt med hello Microsoft Avro Library.</span><span class="sxs-lookup"><span data-stu-id="8574e-145">Note that hello Newtonsoft.Json.dll (>=6.0.4) dependency is also downloaded automatically with hello Microsoft Avro Library.</span></span>

<span data-ttu-id="8574e-146">hello Microsoft Avro Library källkoden finns på [Github](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro).</span><span class="sxs-lookup"><span data-stu-id="8574e-146">hello Microsoft Avro Library source code is available at [Github](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro).</span></span>

## <a name="compile-schemas-using-avro-library"></a><span data-ttu-id="8574e-147">Kompilera scheman med Avro Library</span><span class="sxs-lookup"><span data-stu-id="8574e-147">Compile schemas using Avro Library</span></span>
<span data-ttu-id="8574e-148">hello Microsoft Avro Library innehåller en kodgenerering verktyg som gör att skapa C# typer automatiskt baserat på hello tidigare definierats JSON-schema.</span><span class="sxs-lookup"><span data-stu-id="8574e-148">hello Microsoft Avro Library contains a code generation utility that allows creating C# types automatically based on hello previously defined JSON schema.</span></span> <span data-ttu-id="8574e-149">hello code generation verktyget distribueras inte som en binär körbara, men är lätta att bygga via hello nedan:</span><span class="sxs-lookup"><span data-stu-id="8574e-149">hello code generation utility is not distributed as a binary executable, but can be easily built via hello following procedure:</span></span>

1. <span data-ttu-id="8574e-150">Hämta hello ZIP-filen med hello senaste versionen av HDInsight SDK källkod från <a href="http://hadoopsdk.codeplex.com/SourceControl/latest#" target="_blank">Microsoft .NET SDK för Hadoop</a>.</span><span class="sxs-lookup"><span data-stu-id="8574e-150">Download hello .zip file with hello latest version of HDInsight SDK source code from <a href="http://hadoopsdk.codeplex.com/SourceControl/latest#" target="_blank">Microsoft .NET SDK For Hadoop</a>.</span></span> <span data-ttu-id="8574e-151">(Klicka på hello **hämta** och inte hello **hämtar** flik.)</span><span class="sxs-lookup"><span data-stu-id="8574e-151">(Click hello **Download** icon, not hello **Downloads** tab.)</span></span>
2. <span data-ttu-id="8574e-152">Extrahera hello HDInsight SDK tooa katalog på hello datorn med .NET Framework 4 installerat och anslutet toohello Internet för att ladda ned nödvändiga beroendet NuGet-paket.</span><span class="sxs-lookup"><span data-stu-id="8574e-152">Extract hello HDInsight SDK tooa directory on hello machine with .NET Framework 4 installed and connected toohello Internet for downloading necessary dependency NuGet packages.</span></span> <span data-ttu-id="8574e-153">Nedan, förutsätter vi att hello källkoden är extraherade tooC:\SDK.</span><span class="sxs-lookup"><span data-stu-id="8574e-153">Below, we assume that hello source code is extracted tooC:\SDK.</span></span>
3. <span data-ttu-id="8574e-154">Gå toohello mappen C:\SDK\src\Microsoft.Hadoop.Avro.Tools och kör build.bat.</span><span class="sxs-lookup"><span data-stu-id="8574e-154">Go toohello folder C:\SDK\src\Microsoft.Hadoop.Avro.Tools and run build.bat.</span></span> <span data-ttu-id="8574e-155">(hello filen anrop MSBuild från hello 32-bitars distribution av hello .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="8574e-155">(hello file calls MSBuild from hello 32-bit distribution of hello .NET Framework.</span></span> <span data-ttu-id="8574e-156">Om du vill att toouse hello 64-bitarsversionen, redigera build.bat hello följande kommentarer i hello-fil.) Se till att hello build har lyckats.</span><span class="sxs-lookup"><span data-stu-id="8574e-156">If you would like toouse hello 64-bit version, edit build.bat, following hello comments inside hello file.) Ensure that hello build is successful.</span></span> <span data-ttu-id="8574e-157">(På vissa datorer MSBuild kan generera varningar.</span><span class="sxs-lookup"><span data-stu-id="8574e-157">(On some systems, MSBuild may produce warnings.</span></span> <span data-ttu-id="8574e-158">Dessa varningar påverkar inte hello verktyget så länge det finns inga build-fel.)</span><span class="sxs-lookup"><span data-stu-id="8574e-158">These warnings do not affect hello utility as long as there are no build errors.)</span></span>
4. <span data-ttu-id="8574e-159">hello kompileras som finns i C:\SDK\Bin\Unsigned\Release\Microsoft.Hadoop.Avro.Tools.</span><span class="sxs-lookup"><span data-stu-id="8574e-159">hello compiled utility is located in C:\SDK\Bin\Unsigned\Release\Microsoft.Hadoop.Avro.Tools.</span></span>

<span data-ttu-id="8574e-160">tooget bekant med hello kommandoradssyntaxen, kör hello följande kommando från hello mappen hello code generation verktyget:`Microsoft.Hadoop.Avro.Tools help /c:codegen`</span><span class="sxs-lookup"><span data-stu-id="8574e-160">tooget familiar with hello command-line syntax, execute hello following command from hello folder where hello code generation utility is located: `Microsoft.Hadoop.Avro.Tools help /c:codegen`</span></span>

<span data-ttu-id="8574e-161">tootest hello verktyg du kan skapa C#-klasser från hello JSON-schema exempelfilen medföljer hello källkoden.</span><span class="sxs-lookup"><span data-stu-id="8574e-161">tootest hello utility, you can generate C# classes from hello sample JSON schema file provided with hello source code.</span></span> <span data-ttu-id="8574e-162">Kör följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="8574e-162">Execute hello following command:</span></span>

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:

<span data-ttu-id="8574e-163">Detta ska tooproduce två C#-filer i katalogen hello: SensorData.cs och Location.cs.</span><span class="sxs-lookup"><span data-stu-id="8574e-163">This is supposed tooproduce two C# files in hello current directory: SensorData.cs and Location.cs.</span></span>

<span data-ttu-id="8574e-164">toounderstand hello logik som hello code generation verktyget använder vid konvertering av tooC # hello JSON schematyper, se hello fil GenerationVerification.feature i C:\SDK\src\Microsoft.Hadoop.Avro.Tools\Doc.</span><span class="sxs-lookup"><span data-stu-id="8574e-164">toounderstand hello logic that hello code generation utility is using while converting hello JSON schema tooC# types, see hello file GenerationVerification.feature located in C:\SDK\src\Microsoft.Hadoop.Avro.Tools\Doc.</span></span>

<span data-ttu-id="8574e-165">Namnområden extraheras från hello JSON-schema med hello logiken i hello-fil som nämns i hello föregående stycke.</span><span class="sxs-lookup"><span data-stu-id="8574e-165">Namespaces are extracted from hello JSON schema, using hello logic described in hello file mentioned in hello previous paragraph.</span></span> <span data-ttu-id="8574e-166">Namnområden som extraheras från hello schema företräde framför allt medföljer hello/n-parametern i kommandoraden för hello-verktyget.</span><span class="sxs-lookup"><span data-stu-id="8574e-166">Namespaces extracted from hello schema take precedence over whatever is provided with hello /n parameter in hello utility command line.</span></span> <span data-ttu-id="8574e-167">Om du vill toooverride hello namnområden som ingår i hello schemat använder du hello /nf-parametern.</span><span class="sxs-lookup"><span data-stu-id="8574e-167">If you want toooverride hello namespaces contained within hello schema, use hello /nf parameter.</span></span> <span data-ttu-id="8574e-168">Till exempel toochange alla namnområden från hello SampleJSONSchema.avsc toomy.own.nspace köra hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="8574e-168">For example, toochange all namespaces from hello SampleJSONSchema.avsc toomy.own.nspace, execute hello following command:</span></span>

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:. /nf:my.own.nspace

## <a name="about-hello-samples"></a><span data-ttu-id="8574e-169">Om hello-exempel</span><span class="sxs-lookup"><span data-stu-id="8574e-169">About hello samples</span></span>
<span data-ttu-id="8574e-170">Sex exemplen i det här avsnittet visar olika scenarier som stöds av hello Microsoft Avro Library.</span><span class="sxs-lookup"><span data-stu-id="8574e-170">Six examples provided in this topic illustrate different scenarios supported by hello Microsoft Avro Library.</span></span> <span data-ttu-id="8574e-171">hello Microsoft Avro Library är utformad toowork med en dataström.</span><span class="sxs-lookup"><span data-stu-id="8574e-171">hello Microsoft Avro Library is designed toowork with any stream.</span></span> <span data-ttu-id="8574e-172">I det här exemplet ändras data via minne strömmar i stället filen dataströmmar eller databaser för enkelhetens skull.</span><span class="sxs-lookup"><span data-stu-id="8574e-172">In these examples, data is manipulated via memory streams rather than file streams or databases for simplicity and consistency.</span></span> <span data-ttu-id="8574e-173">hello metod i en produktionsmiljö är beroende av hello exakt scenariokrav, datakällan och volymen, prestanda begränsningar och andra faktorer.</span><span class="sxs-lookup"><span data-stu-id="8574e-173">hello approach taken in a production environment depends on hello exact scenario requirements, data source and volume, performance constraints, and other factors.</span></span>

<span data-ttu-id="8574e-174">Hej första två exempel visas hur tooserialize och deserialisera data i dataströmmen minnesbuffertar med hjälp av reflektion och allmänna poster.</span><span class="sxs-lookup"><span data-stu-id="8574e-174">hello first two examples show how tooserialize and deserialize data into memory stream buffers by using reflection and generic records.</span></span> <span data-ttu-id="8574e-175">hello schemat i båda fallen antas toobe delas mellan hello läsare och skrivare för out-of-band.</span><span class="sxs-lookup"><span data-stu-id="8574e-175">hello schema in these two cases is assumed toobe shared between hello readers and writers out-of-band.</span></span>

<span data-ttu-id="8574e-176">hello tredje och fjärde exempel visas hur tooserialize och deserialisera data med hjälp av hello Avro objektet behållarfiler.</span><span class="sxs-lookup"><span data-stu-id="8574e-176">hello third and fourth examples show how tooserialize and deserialize data by using hello Avro object container files.</span></span> <span data-ttu-id="8574e-177">När data lagras i en fil för Avro-behållare, lagras dess schema alltid med det eftersom hello schemat måste vara delad för deserialisering.</span><span class="sxs-lookup"><span data-stu-id="8574e-177">When data is stored in an Avro container file, its schema is always stored with it because hello schema must be shared for deserialization.</span></span>

<span data-ttu-id="8574e-178">hello prov som innehåller hello första fyra exempel kan hämtas från hello <a href="http://code.msdn.microsoft.com/Serialize-data-with-the-86055923" target="_blank">Azure kodexempel</a> plats.</span><span class="sxs-lookup"><span data-stu-id="8574e-178">hello sample containing hello first four examples can be downloaded from hello <a href="http://code.msdn.microsoft.com/Serialize-data-with-the-86055923" target="_blank">Azure code samples</a> site.</span></span>

<span data-ttu-id="8574e-179">Hej femte exempel visar hur toouse en anpassad komprimerings-codec för Avro objekt behållarfiler.</span><span class="sxs-lookup"><span data-stu-id="8574e-179">hello fifth example shows how toouse a custom compression codec for Avro object container files.</span></span> <span data-ttu-id="8574e-180">Ett exempel som innehåller hello kod för det här exemplet kan hämtas från hello <a href="http://code.msdn.microsoft.com/Serialize-data-with-the-67159111" target="_blank">Azure kodexempel</a> plats.</span><span class="sxs-lookup"><span data-stu-id="8574e-180">A sample containing hello code for this example can be downloaded from hello <a href="http://code.msdn.microsoft.com/Serialize-data-with-the-67159111" target="_blank">Azure code samples</a> site.</span></span>

<span data-ttu-id="8574e-181">hello sjätte exempel visas hur toouse Avro serialisering tooupload data tooAzure Blob storage och analysera den med hjälp av Hive med HDInsight (Hadoop)-kluster.</span><span class="sxs-lookup"><span data-stu-id="8574e-181">hello sixth sample shows how toouse Avro serialization tooupload data tooAzure Blob storage and then analyze it by using Hive with an HDInsight (Hadoop) cluster.</span></span> <span data-ttu-id="8574e-182">Du kan hämta från hello <a href="https://code.msdn.microsoft.com/Using-Avro-to-upload-data-ae81b1e3" target="_blank">Azure kodexempel</a> plats.</span><span class="sxs-lookup"><span data-stu-id="8574e-182">It can be downloaded from hello <a href="https://code.msdn.microsoft.com/Using-Avro-to-upload-data-ae81b1e3" target="_blank">Azure code samples</a> site.</span></span>

<span data-ttu-id="8574e-183">Här följer länkar toohello sex exempel som beskrivs i avsnittet hello:</span><span class="sxs-lookup"><span data-stu-id="8574e-183">Here are links toohello six samples discussed in hello topic:</span></span>

* <span data-ttu-id="8574e-184"><a href="#Scenario1">**Serialisering med reflektion** </a> -hello JSON-schema för typer toobe serialiseras skapas automatiskt från hello data kontraktet attribut.</span><span class="sxs-lookup"><span data-stu-id="8574e-184"><a href="#Scenario1">**Serialization with reflection**</a> - hello JSON schema for types toobe serialized is automatically built from hello data contract attributes.</span></span>
* <span data-ttu-id="8574e-185"><a href="#Scenario2">**Serialisering med allmän post** </a> -hello JSON-schema är explicit i en post när ingen .NET-typ är tillgängligt för reflektion.</span><span class="sxs-lookup"><span data-stu-id="8574e-185"><a href="#Scenario2">**Serialization with generic record**</a> - hello JSON schema is explicitly specified in a record when no .NET type is available for reflection.</span></span>
* <span data-ttu-id="8574e-186"><a href="#Scenario3">**Serialisering med reflektion objektet behållarfiler** </a> -hello JSON-schema har skapats automatiskt och delade tillsammans med hello serialiserade data via en Avro objektet behållare fil.</span><span class="sxs-lookup"><span data-stu-id="8574e-186"><a href="#Scenario3">**Serialization using object container files with reflection**</a> - hello JSON schema is automatically built and shared together with hello serialized data via an Avro object container file.</span></span>
* <span data-ttu-id="8574e-187"><a href="#Scenario4">**Serialisering med allmän post objektet behållarfiler** </a> -hello JSON-schema är uttryckligen anges innan hello serialisering och delade tillsammans med hello data via en Avro objektet behållare fil.</span><span class="sxs-lookup"><span data-stu-id="8574e-187"><a href="#Scenario4">**Serialization using object container files with generic record**</a> - hello JSON schema is explicitly specified before hello serialization and shared together with hello data via an Avro object container file.</span></span>
* <span data-ttu-id="8574e-188"><a href="#Scenario5">**Serialisering med en anpassad komprimerings-codec objektet behållarfiler** </a> -hello exempel visas hur toocreate ett Avro objekt behållare fil med en anpassad .NET-implementering av hello Deflate data komprimerings-codec.</span><span class="sxs-lookup"><span data-stu-id="8574e-188"><a href="#Scenario5">**Serialization using object container files with a custom compression codec**</a> - hello example shows how toocreate an Avro object container file with a customized .NET implementation of hello Deflate data compression codec.</span></span>
* <span data-ttu-id="8574e-189"><a href="#Scenario6">**Med hjälp av Avro tooupload data för hello tjänsten Microsoft Azure HDInsight** </a> -hello exemplet illustrerar hur Avro serialisering samverkar med hello HDInsight-tjänst.</span><span class="sxs-lookup"><span data-stu-id="8574e-189"><a href="#Scenario6">**Using Avro tooupload data for hello Microsoft Azure HDInsight service**</a> - hello example illustrates how Avro serialization interacts with hello HDInsight service.</span></span> <span data-ttu-id="8574e-190">Ett aktivt Azure-prenumeration och åtkomst tooan Azure HDInsight-kluster måste toorun det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="8574e-190">An active Azure subscription and access tooan Azure HDInsight cluster are required toorun this example.</span></span>

## <span data-ttu-id="8574e-191"><a name="Scenario1"></a>Exempel 1: Serialisering med reflektion</span><span class="sxs-lookup"><span data-stu-id="8574e-191"><a name="Scenario1"></a>Sample 1: Serialization with reflection</span></span>
<span data-ttu-id="8574e-192">hello JSON-schema för hello typer kan automatiskt genereras av hello Microsoft Avro Library via reflektion från hello data kontraktet attribut för hello C# objekt toobe serialiseras.</span><span class="sxs-lookup"><span data-stu-id="8574e-192">hello JSON schema for hello types can be automatically built by hello Microsoft Avro Library via reflection from hello data contract attributes of hello C# objects toobe serialized.</span></span> <span data-ttu-id="8574e-193">hello Microsoft Avro Library skapar en [ **IAvroSeralizer<T>**  ](http://msdn.microsoft.com/library/dn627341.aspx) tooidentify hello fält toobe serialiseras.</span><span class="sxs-lookup"><span data-stu-id="8574e-193">hello Microsoft Avro Library creates an [**IAvroSeralizer<T>**](http://msdn.microsoft.com/library/dn627341.aspx) tooidentify hello fields toobe serialized.</span></span>

<span data-ttu-id="8574e-194">I det här exemplet objekt (en **SensorData** klass med en medlem **plats** struct) är serialiserade tooa minne dataströmmen och den här dataströmmen i sin tur avserialiseras.</span><span class="sxs-lookup"><span data-stu-id="8574e-194">In this example, objects (a **SensorData** class with a member **Location** struct) are serialized tooa memory stream, and this stream is in turn deserialized.</span></span> <span data-ttu-id="8574e-195">hello resultatet är jämfört med toohello första instansen tooconfirm som hello **SensorData** objekt som återställts är identiska toohello ursprungliga.</span><span class="sxs-lookup"><span data-stu-id="8574e-195">hello result is then compared toohello initial instance tooconfirm that hello **SensorData** object recovered is identical toohello original.</span></span>

<span data-ttu-id="8574e-196">hello schemat i det här exemplet förutsätts toobe delas mellan hello läsare och skrivare, så hello Avro objektet behållares format inte krävs.</span><span class="sxs-lookup"><span data-stu-id="8574e-196">hello schema in this example is assumed toobe shared between hello readers and writers, so hello Avro object container format is not required.</span></span> <span data-ttu-id="8574e-197">Ett exempel på hur tooserialize och deserialisera data i minnesbuffertar med hjälp av reflektion med hello objektet behållares format när hello schemat måste delas med hello data finns <a href="#Scenario3">serialisering med reflektionbehållarfileriobjektet</a>.</span><span class="sxs-lookup"><span data-stu-id="8574e-197">For an example of how tooserialize and deserialize data into memory buffers by using reflection with hello object container format when hello schema must be shared with hello data, see <a href="#Scenario3">Serialization using object container files with reflection</a>.</span></span>

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
        //hello usage of Microsoft Avro Library
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

                    //Serialize hello data toohello specified stream
                    avroSerializer.Serialize(buffer, expected);


                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare hello stream for deserializing hello data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Deserialize data from hello stream and cast it toohello same type used for serialization
                    var actual = avroSerializer.Deserialize(buffer);

                    Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                    //Finally, verify that deserialized data matches hello original one
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

                //Serialization toomemory using reflection
                Sample.SerializeDeserializeObjectUsingReflection();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key tooexit.");
                Console.Read();
            }
        }
    }
    // hello example is expected toodisplay hello following output:
    // SERIALIZATION USING REFLECTION
    //
    // Serializing Sample Data Set...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // Result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key tooexit.


## <a name="sample-2-serialization-with-a-generic-record"></a><span data-ttu-id="8574e-198">Exempel 2: Serialisering med en allmän post</span><span class="sxs-lookup"><span data-stu-id="8574e-198">Sample 2: Serialization with a generic record</span></span>
<span data-ttu-id="8574e-199">En JSON-schema kan uttryckligen anges i en allmän post när reflektion inte kan användas eftersom hello data inte kan representeras via .NET-klasser med ett datakontrakt.</span><span class="sxs-lookup"><span data-stu-id="8574e-199">A JSON schema can be explicitly specified in a generic record when reflection cannot be used because hello data cannot be represented via .NET classes with a data contract.</span></span> <span data-ttu-id="8574e-200">Den här metoden går långsammare än med hjälp av reflektion.</span><span class="sxs-lookup"><span data-stu-id="8574e-200">This method is slower than using reflection.</span></span> <span data-ttu-id="8574e-201">I sådana fall kan hello schemat för hello data också vara dynamisk, som är inte känd vid kompileringen.</span><span class="sxs-lookup"><span data-stu-id="8574e-201">In such cases, hello schema for hello data may also be dynamic, that is, not known at compile time.</span></span> <span data-ttu-id="8574e-202">Data visas i form av kommaavgränsade värden (CSV) filer vars schema är okänd tills det transformeras toohello Avro-formatet vid körning är ett exempel på den här typen av dynamisk scenario.</span><span class="sxs-lookup"><span data-stu-id="8574e-202">Data represented as comma-separated values (CSV) files whose schema is unknown until it is transformed toohello Avro format at run time is an example of this sort of dynamic scenario.</span></span>

<span data-ttu-id="8574e-203">Det här exemplet illustrerar hur toocreate och använda en [ **AvroRecord** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) tooexplicitly ange en JSON-schema, hur toopopulate med hello data och sedan hur tooserialize och deserialisera det.</span><span class="sxs-lookup"><span data-stu-id="8574e-203">This example shows how toocreate and use an [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) tooexplicitly specify a JSON schema, how toopopulate it with hello data, and then how tooserialize and deserialize it.</span></span> <span data-ttu-id="8574e-204">hello resultatet jämförs sedan toohello inledande instans tooconfirm som hello post återställs är identiska toohello ursprungliga.</span><span class="sxs-lookup"><span data-stu-id="8574e-204">hello result is then compared toohello initial instance tooconfirm that hello record recovered is identical toohello original.</span></span>

<span data-ttu-id="8574e-205">hello schemat i det här exemplet förutsätts toobe delas mellan hello läsare och skrivare, så hello Avro objektet behållares format inte krävs.</span><span class="sxs-lookup"><span data-stu-id="8574e-205">hello schema in this example is assumed toobe shared between hello readers and writers, so hello Avro object container format is not required.</span></span> <span data-ttu-id="8574e-206">Ett exempel på hur tooserialize och deserialisera data i minnesbuffertar med hjälp av en allmän post med hello objektet behållares format när hello schemat måste ingå i hello serialiserade data finns hello <a href="#Scenario4">serialisering med objektbehållare filer med allmän post</a> exempel.</span><span class="sxs-lookup"><span data-stu-id="8574e-206">For an example of how tooserialize and deserialize data into memory buffers by using a generic record with hello object container format when hello schema must be included with hello serialized data, see hello <a href="#Scenario4">Serialization using object container files with generic record</a> example.</span></span>

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
    //hello usage of Microsoft Avro Library
    public class AvroSample
    {

        //Serialize and deserialize sample data set by using a generic record.
        //A generic record is a special class with hello schema explicitly defined in JSON.
        //All serialized data should be mapped toohello fields of hello generic record,
        //which in turn is then serialized.
        public void SerializeDeserializeObjectUsingGenericRecords()
        {
            Console.WriteLine("SERIALIZATION USING GENERIC RECORD\n");
            Console.WriteLine("Defining hello Schema and creating Sample Data Set...");

            //Define hello schema in JSON
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

            //Create a generic serializer based on hello schema
            var serializer = AvroSerializer.CreateGeneric(Schema);
            var rootSchema = serializer.WriterSchema as RecordSchema;

            //Create a memory stream buffer
            using (var stream = new MemoryStream())
            {
                //Create a generic record toorepresent hello data
                dynamic location = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location.Floor = 1;
                location.Room = 243;

                dynamic expected = new AvroRecord(serializer.WriterSchema);
                expected.Location = location;
                expected.Value = new byte[] { 1, 2, 3, 4, 5 };

                Console.WriteLine("Serializing Sample Data Set...");

                //Serialize hello data
                serializer.Serialize(stream, expected);

                stream.Seek(0, SeekOrigin.Begin);

                Console.WriteLine("Deserializing Sample Data Set...");

                //Deserialize hello data into a generic record
                dynamic actual = serializer.Deserialize(stream);

                Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                //Finally, verify hello results
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

            //Serialization toomemory using generic record
            Sample.SerializeDeserializeObjectUsingGenericRecords();

            Console.WriteLine(sectionDivider);
            Console.WriteLine("Press any key tooexit.");
            Console.Read();
        }
    }
    }
    // hello example is expected toodisplay hello following output:
    // SERIALIZATION USING GENERIC RECORD
    //
    // Defining hello Schema and creating Sample Data Set...
    // Serializing Sample Data Set...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // Result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key tooexit.


## <a name="sample-3-serialization-using-object-container-files-and-serialization-with-reflection"></a><span data-ttu-id="8574e-207">Exempel 3: Serialisering med reflektion objektet behållarfiler och serialisering</span><span class="sxs-lookup"><span data-stu-id="8574e-207">Sample 3: Serialization using object container files and serialization with reflection</span></span>
<span data-ttu-id="8574e-208">Det här exemplet är liknande toohello scenario i hello <a href="#Scenario1"> första exemplet</a>, där hello schema har angetts implicit med reflektion.</span><span class="sxs-lookup"><span data-stu-id="8574e-208">This example is similar toohello scenario in hello <a href="#Scenario1"> first example</a>, where hello schema is implicitly specified with reflection.</span></span> <span data-ttu-id="8574e-209">hello skillnaden är att här hello schemat antas inte toobe kända toohello läsare som deserializes den.</span><span class="sxs-lookup"><span data-stu-id="8574e-209">hello difference is that here, hello schema is not assumed toobe known toohello reader that deserializes it.</span></span> <span data-ttu-id="8574e-210">Hej **SensorData** objekt toobe serialiseras och deras implicit angivna schemat lagras i en Avro objektet behållaren-fil som representeras av hello [ **AvroContainer** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) klass.</span><span class="sxs-lookup"><span data-stu-id="8574e-210">hello **SensorData** objects toobe serialized and their implicitly specified schema are stored in an Avro object container file represented by hello [**AvroContainer**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) class.</span></span>

<span data-ttu-id="8574e-211">hello data serialiseras i det här exemplet med [ **SequentialWriter<SensorData>**  ](http://msdn.microsoft.com/library/dn627340.aspx) och avserialiserat med [ **SequentialReader<SensorData>** ](http://msdn.microsoft.com/library/dn627340.aspx).</span><span class="sxs-lookup"><span data-stu-id="8574e-211">hello data is serialized in this example with [**SequentialWriter<SensorData>**](http://msdn.microsoft.com/library/dn627340.aspx) and deserialized with [**SequentialReader<SensorData>**](http://msdn.microsoft.com/library/dn627340.aspx).</span></span> <span data-ttu-id="8574e-212">hello resultatet blir sedan jämfört med toohello inledande instanser tooensure identitet.</span><span class="sxs-lookup"><span data-stu-id="8574e-212">hello result then is compared toohello initial instances tooensure identity.</span></span>

<span data-ttu-id="8574e-213">hello i hello dataobjekt behållaren filer komprimeras via hello standard [ **Deflate** ] [ deflate-100] komprimerings-codec från .NET Framework 4.</span><span class="sxs-lookup"><span data-stu-id="8574e-213">hello data in hello object container file is compressed via hello default [**Deflate**][deflate-100] compression codec from .NET Framework 4.</span></span> <span data-ttu-id="8574e-214">Se hello <a href="#Scenario5"> femte exempel</a> i det här avsnittet toolearn hur toouse en senare och högre version av hello [ **Deflate** ] [ deflate-110] komprimering codec som är tillgängliga i .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="8574e-214">See hello <a href="#Scenario5"> fifth example</a> in this topic toolearn how toouse a more recent and superior version of hello [**Deflate**][deflate-110] compression codec available in .NET Framework 4.5.</span></span>

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
        //hello usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes hello sample data set by using reflection and Avro object container files.
            //Serialized data is compressed with hello Deflate codec.
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

                //Serializing and saving data toofile.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects toostream.
                    //Data is compressed using hello Deflate codec.
                    using (var w = AvroContainer.CreateWriter<SensorData>(buffer, Codec.Deflate))
                    {
                        using (var writer = new SequentialWriter<SensorData>(w, 24))
                        {
                            // Serialize hello data toostream by using hello sequential writer
                            testData.ForEach(writer.Write);
                        }
                    }

                    //Save stream toofile
                    Console.WriteLine("Saving serialized data toofile...");
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

                    //Prepare hello stream for deserializing hello data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Create a SequentialReader instance for type SensorData, which deserializes all serialized objects from hello given stream.
                    //It allows iterating over hello deserialized objects because it implements hello IEnumerable<T> interface.
                    using (var reader = new SequentialReader<SensorData>(
                        AvroContainer.CreateReader<SensorData>(buffer, true)))
                    {
                        var results = reader.Objects;

                        //Finally, verify that deserialized data matches hello original one
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

                //Delete hello file
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

            //Saving memory stream tooa new file with hello given path
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
                        Console.WriteLine("hello following exception was thrown during creation and writing toohello file \"{0}\"", path);
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

            //Reading a file content by using hello given path tooa memory stream
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
                    Console.WriteLine("hello following exception was thrown during reading from hello file \"{0}\"", path);
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
                        Console.WriteLine("hello following exception was thrown during deleting hello file \"{0}\"", path);
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

                //Serialization using reflection tooAvro object container file
                Sample.SerializeDeserializeUsingObjectContainersReflection();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key tooexit.");
                Console.Read();
            }
        }
    }
    // hello example is expected toodisplay hello following output:
    // SERIALIZATION USING REFLECTION AND AVRO OBJECT CONTAINER FILES
    //
    // Serializing Sample Data Set...
    // Saving serialized data toofile...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    // For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key tooexit.


## <a name="sample-4-serialization-using-object-container-files-and-serialization-with-generic-record"></a><span data-ttu-id="8574e-215">Exempel 4: Serialisering med objektet behållarfiler och serialisering med allmän post</span><span class="sxs-lookup"><span data-stu-id="8574e-215">Sample 4: Serialization using object container files and serialization with generic record</span></span>
<span data-ttu-id="8574e-216">Det här exemplet är liknande toohello scenario i hello <a href="#Scenario2"> andra exemplet</a>där hello schemat anges explicit med JSON.</span><span class="sxs-lookup"><span data-stu-id="8574e-216">This example is similar toohello scenario in hello <a href="#Scenario2"> second example</a>, where hello schema is explicitly specified with JSON.</span></span> <span data-ttu-id="8574e-217">hello skillnaden är att här hello schemat antas inte toobe kända toohello läsare som deserializes den.</span><span class="sxs-lookup"><span data-stu-id="8574e-217">hello difference is that here, hello schema is not assumed toobe known toohello reader that deserializes it.</span></span>

<span data-ttu-id="8574e-218">hello test datauppsättning har samlats in i en lista över [ **AvroRecord** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) objekt via ett uttryckligen definierade JSON-schema, och lagras sedan i en fil för behållaren av objektet som representeras av hello [ **AvroContainer** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) klass.</span><span class="sxs-lookup"><span data-stu-id="8574e-218">hello test data set is collected into a list of [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) objects via an explicitly defined JSON schema and then stored in an object container file represented by hello [**AvroContainer**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) class.</span></span> <span data-ttu-id="8574e-219">Den här behållaren skapar en skrivare som har använt tooserialize hello data, okomprimerade, tooa minne dataström som sparas tooa fil.</span><span class="sxs-lookup"><span data-stu-id="8574e-219">This container file creates a writer that is used tooserialize hello data, uncompressed, tooa memory stream that is then saved tooa file.</span></span> <span data-ttu-id="8574e-220">Hej [ **Codec.Null** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.codec.null.aspx) parametern används för att skapa hello reader anger inte komprimeras dessa data.</span><span class="sxs-lookup"><span data-stu-id="8574e-220">hello [**Codec.Null**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.codec.null.aspx) parameter used for creating hello reader specifies that this data is not compressed.</span></span>

<span data-ttu-id="8574e-221">hello data sedan läsa från filen hello och avserialiseras till en samling objekt.</span><span class="sxs-lookup"><span data-stu-id="8574e-221">hello data is then read from hello file and deserialized into a collection of objects.</span></span> <span data-ttu-id="8574e-222">Den här samlingen är jämfört med toohello första listan över Avro poster tooconfirm att de är identiska.</span><span class="sxs-lookup"><span data-stu-id="8574e-222">This collection is compared toohello initial list of Avro records tooconfirm that they are identical.</span></span>

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
        //hello usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes a sample data set by using a generic record and Avro object container files.
            //Serialized data is not compressed.
            public void SerializeDeserializeUsingObjectContainersGenericRecord()
            {
                Console.WriteLine("SERIALIZATION USING GENERIC RECORD AND AVRO OBJECT CONTAINER FILES\n");

                //Path for Avro object container file
                string path = "AvroSampleGenericRecordNullCodec.avro";

                Console.WriteLine("Defining hello Schema and creating Sample Data Set...");

                //Define hello schema in JSON
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

                //Create a generic serializer based on hello schema
                var serializer = AvroSerializer.CreateGeneric(Schema);
                var rootSchema = serializer.WriterSchema as RecordSchema;

                //Create a generic record toorepresent hello data
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

                //Serializing and saving data toofile.
                //Create a MemoryStream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects toostream.
                    //Data is not compressed (Null compression codec).
                    using (var writer = AvroContainer.CreateGenericWriter(Schema, buffer, Codec.Null))
                    {
                        using (var streamWriter = new SequentialWriter<object>(writer, 24))
                        {
                            // Serialize hello data toostream by using hello sequential writer
                            testData.ForEach(streamWriter.Write);
                        }
                    }

                    Console.WriteLine("Saving serialized data toofile...");

                    //Save stream toofile
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing hello data.
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

                    //Prepare hello stream for deserializing hello data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Create a SequentialReader instance for type SensorData, which deserializes all serialized objects from hello given stream.
                    //It allows iterating over hello deserialized objects because it implements hello IEnumerable<T> interface.
                    using (var reader = AvroContainer.CreateGenericReader(buffer))
                    {
                        using (var streamReader = new SequentialReader<object>(reader))
                        {
                            var results = streamReader.Objects;

                            Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                            //Finally, verify hello results
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

                //Delete hello file
                RemoveFile(path);
            }

            //
            //Helper methods
            //

            //Saving memory stream tooa new file with hello given path
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
                        Console.WriteLine("hello following exception was thrown during creation and writing toohello file \"{0}\"", path);
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

            //Reading a file content by using hello given path tooa memory stream
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
                    Console.WriteLine("hello following exception was thrown during reading from hello file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using hello given path
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
                        Console.WriteLine("hello following exception was thrown during deleting hello file \"{0}\"", path);
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

                //Create an instance of hello AvroSample class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using generic record tooAvro object container file
                Sample.SerializeDeserializeUsingObjectContainersGenericRecord();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key tooexit.");
                Console.Read();
            }
        }
    }
    // hello example is expected toodisplay hello following output:
    // SERIALIZATION USING GENERIC RECORD AND AVRO OBJECT CONTAINER FILES
    //
    // Defining hello Schema and creating Sample Data Set...
    // Serializing Sample Data Set...
    // Saving serialized data toofile...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    // For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key tooexit.




## <a name="sample-5-serialization-using-object-container-files-with-a-custom-compression-codec"></a><span data-ttu-id="8574e-223">Exempel 5: Serialisering med en anpassad komprimerings-codec objektet behållarfiler</span><span class="sxs-lookup"><span data-stu-id="8574e-223">Sample 5: Serialization using object container files with a custom compression codec</span></span>
<span data-ttu-id="8574e-224">Hej femte exempel visar hur toouse en anpassad komprimerings-codec för Avro objekt behållarfiler.</span><span class="sxs-lookup"><span data-stu-id="8574e-224">hello fifth example shows how toouse a custom compression codec for Avro object container files.</span></span> <span data-ttu-id="8574e-225">Ett exempel som innehåller hello kod för det här exemplet kan hämtas från hello [Azure kodexempel](http://code.msdn.microsoft.com/Serialize-data-with-the-67159111) plats.</span><span class="sxs-lookup"><span data-stu-id="8574e-225">A sample containing hello code for this example can be downloaded from hello [Azure code samples](http://code.msdn.microsoft.com/Serialize-data-with-the-67159111) site.</span></span>

<span data-ttu-id="8574e-226">Hej [Avro-specifikationen](http://avro.apache.org/docs/current/spec.html#Required+Codecs) tillåter användning av ett valfritt komprimerings-codec (förutom för**Null** och **Deflate** standardinställningarna).</span><span class="sxs-lookup"><span data-stu-id="8574e-226">hello [Avro Specification](http://avro.apache.org/docs/current/spec.html#Required+Codecs) allows usage of an optional compression codec (in addition too**Null** and **Deflate** defaults).</span></span> <span data-ttu-id="8574e-227">Det här exemplet inte implementerar en ny codec, till exempel Snappy (anges som en valfri codec i hello [Avro-specifikationen](http://avro.apache.org/docs/current/spec.html#snappy)).</span><span class="sxs-lookup"><span data-stu-id="8574e-227">This example is not implementing a new codec such as Snappy (mentioned as a supported optional codec in hello [Avro Specification](http://avro.apache.org/docs/current/spec.html#snappy)).</span></span> <span data-ttu-id="8574e-228">Den visar hur toouse hello .NET Framework 4.5 implementering av hello [ **Deflate** ] [ deflate-110] codec, vilket ger en bättre komprimeringsalgoritm baserat på hello [zlib](http://zlib.net/) Komprimeringsbiblioteket än hello standardversionen för .NET Framework 4.</span><span class="sxs-lookup"><span data-stu-id="8574e-228">It shows how toouse hello .NET Framework 4.5 implementation of hello [**Deflate**][deflate-110] codec, which provides a better compression algorithm based on hello [zlib](http://zlib.net/) compression library than hello default .NET Framework 4 version.</span></span>

    //
    // This code needs toobe compiled with hello parameter Target Framework set as ".NET Framework 4.5"
    // tooensure hello desired implementation of hello Deflate compression algorithm is used.
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
        //which are hello key ones for data compression.
        //tooenable a custom codec, one needs tooimplement these methods for hello required codec.

        #region Defining Compression and Decompression Streams
        //DeflateStream (class from System.IO.Compression namespace that implements Deflate algorithm)
        //cannot be directly used for Avro because it does not support vital operations like Seek.
        //Thus one needs tooimplement two classes inherited from stream
        //(one for compressed and one for decompressed stream)
        //that use Deflate compression and implement all required features.
        internal sealed class CompressionStreamDeflate45 : Stream
        {
            private readonly Stream buffer;
            private DeflateStream compressionStream;

            public CompressionStreamDeflate45(Stream buffer)
            {
                Debug.Assert(buffer != null, "Buffer is not allowed toobe null.");

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
        //Define hello actual codec class containing hello required methods for manipulating streams:
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
        //Define modified codec factory toobe used in hello reader.
        //It catches hello attempt toouse "Deflate" and provide  a custom codec.
        //For all other cases, it relies on hello base class (CodecFactory).
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
        //hello usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes sample data set by using reflection and Avro object container files.
            //Serialized data is compressed with hello custom compression codec (Deflate of .NET Framework 4.5).
            //
            //This sample uses memory stream for all operations related tooserialization, deserialization and
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

                //Serializing and saving data toofile.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects toostream.
                    //Here hello custom codec is introduced. For convenience, hello next commented code line shows how toouse built-in Deflate.
                    //Note that because hello sample deals with different IMPLEMENTATIONS of Deflate, built-in and custom codecs are interchangeable
                    //in read-write operations.
                    //using (var w = AvroContainer.CreateWriter<SensorData>(buffer, Codec.Deflate))
                    using (var w = AvroContainer.CreateWriter<SensorData>(buffer, new DeflateCodec45()))
                    {
                        using (var writer = new SequentialWriter<SensorData>(w, 24))
                        {
                            // Serialize hello data toostream using hello sequential writer
                            testData.ForEach(writer.Write);
                        }
                    }

                    //Save stream toofile
                    Console.WriteLine("Saving serialized data toofile...");
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

                    //Prepare hello stream for deserializing hello data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Because of SequentialReader<T> constructor signature, an AvroSerializerSettings instance is required
                    //when codec factory is explicitly specified.
                    //You may comment hello line below if you want toouse built-in Deflate (see next comment).
                    AvroSerializerSettings settings = new AvroSerializerSettings();

                    //Create a SequentialReader instance for type SensorData, which deserializes all serialized objects from hello given stream.
                    //It allows iterating over hello deserialized objects because it implements hello IEnumerable<T> interface.
                    //Here hello custom codec factory is introduced.
                    //For convenience, hello next commented code line shows how toouse built-in Deflate
                    //(no explicit Codec Factory parameter is required in this case).
                    //Note that because hello sample deals with different IMPLEMENTATIONS of Deflate, built-in and custom codecs are interchangeable
                    //in read-write operations.
                    //using (var reader = new SequentialReader<SensorData>(AvroContainer.CreateReader<SensorData>(buffer, true)))
                    using (var reader = new SequentialReader<SensorData>(
                        AvroContainer.CreateReader<SensorData>(buffer, true, settings, new CodecFactoryDeflate45())))
                    {
                        var results = reader.Objects;

                        //Finally, verify that deserialized data matches hello original one
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

                //Delete hello file
                RemoveFile(path);
            }
        #endregion

            #region Helper Methods

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }

            //Saving memory stream tooa new file with hello given path
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
                        Console.WriteLine("hello following exception was thrown during creation and writing toohello file \"{0}\"", path);
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

            //Reading file content by using hello given path tooa memory stream
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
                    Console.WriteLine("hello following exception was thrown during reading from hello file \"{0}\"", path);
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
                        Console.WriteLine("hello following exception was thrown during deleting hello file \"{0}\"", path);
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

                //Serialization using reflection tooAvro object container file using custom codec
                Sample.SerializeDeserializeUsingObjectContainersReflectionCustomCodec();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key tooexit.");
                Console.Read();
            }
        }
    }
    // hello example is expected toodisplay hello following output:
    // SERIALIZATION USING REFLECTION, AVRO OBJECT CONTAINER FILES AND CUSTOM CODEC
    //
    // Serializing Sample Data Set...
    // Saving serialized data toofile...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    //For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key tooexit.

## <a name="sample-6-using-avro-tooupload-data-for-hello-microsoft-azure-hdinsight-service"></a><span data-ttu-id="8574e-229">Exempel 6: Använder Avro tooupload data för hello Microsoft Azure HDInsight-tjänst</span><span class="sxs-lookup"><span data-stu-id="8574e-229">Sample 6: Using Avro tooupload data for hello Microsoft Azure HDInsight service</span></span>
<span data-ttu-id="8574e-230">hello exemplet sjätte vissa programming tekniker relaterade toointeracting med hello Azure HDInsight-tjänst.</span><span class="sxs-lookup"><span data-stu-id="8574e-230">hello sixth example illustrates some programming techniques related toointeracting with hello Azure HDInsight service.</span></span> <span data-ttu-id="8574e-231">Ett exempel som innehåller hello kod för det här exemplet kan hämtas från hello [Azure kodexempel](https://code.msdn.microsoft.com/Using-Avro-to-upload-data-ae81b1e3) plats.</span><span class="sxs-lookup"><span data-stu-id="8574e-231">A sample containing hello code for this example can be downloaded from hello [Azure code samples](https://code.msdn.microsoft.com/Using-Avro-to-upload-data-ae81b1e3) site.</span></span>

<span data-ttu-id="8574e-232">hello exempel hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="8574e-232">hello sample does hello following tasks:</span></span>

* <span data-ttu-id="8574e-233">Ansluter tooan befintliga service-kluster i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="8574e-233">Connects tooan existing HDInsight service cluster.</span></span>
* <span data-ttu-id="8574e-234">Serialiserar flera CSV-filer och överför hello resultatet tooAzure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="8574e-234">Serializes several CSV files and uploads hello result tooAzure Blob storage.</span></span> <span data-ttu-id="8574e-235">(hello CSV-filer ska distribueras tillsammans med hello exempel och representerar ett utdrag ur AMEX börs historiska data som distribueras av [Infochimps](http://www.infochimps.com/) för hello period 1970 2010.</span><span class="sxs-lookup"><span data-stu-id="8574e-235">(hello CSV files are distributed together with hello sample and represent an extract from AMEX Stock historical data distributed by [Infochimps](http://www.infochimps.com/) for hello period 1970-2010.</span></span> <span data-ttu-id="8574e-236">hello exempel läser data från en CSV, konverterar hello poster tooinstances av hello **börs** klassen och Serialiserar dem med hjälp av reflektion.</span><span class="sxs-lookup"><span data-stu-id="8574e-236">hello sample reads CSV file data, converts hello records tooinstances of hello **Stock** class, and then serializes them by using reflection.</span></span> <span data-ttu-id="8574e-237">Typdefinitionen för lager skapas från ett JSON-schema via hello Microsoft Avro Library code generation verktyget.</span><span class="sxs-lookup"><span data-stu-id="8574e-237">Stock type definition is created from a JSON schema via hello Microsoft Avro Library code generation utility.</span></span>
* <span data-ttu-id="8574e-238">Skapar en ny extern tabell som kallas **lagerlista** i Hive, och länkar den toohello data på hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="8574e-238">Creates a new external table called **Stocks** in Hive, and links it toohello data uploaded in hello previous step.</span></span>
* <span data-ttu-id="8574e-239">Kör en fråga med hjälp av Hive via hello **lagerlista** tabell.</span><span class="sxs-lookup"><span data-stu-id="8574e-239">Executes a query by using Hive over hello **Stocks** table.</span></span>

<span data-ttu-id="8574e-240">Dessutom utför en rensning procedur hello exempel före och efter att ha genomfört större operationer.</span><span class="sxs-lookup"><span data-stu-id="8574e-240">In addition, hello sample performs a clean-up procedure before and after performing major operations.</span></span> <span data-ttu-id="8574e-241">Under hello rensning relaterade alla hello Azure Blob-data och mappar tas bort och hello Hive-tabellen har släppts.</span><span class="sxs-lookup"><span data-stu-id="8574e-241">During hello clean-up, all of hello related Azure Blob data and folders are removed, and hello Hive table is dropped.</span></span> <span data-ttu-id="8574e-242">Du kan även anropa hello Rensa proceduren från hello kommandorad.</span><span class="sxs-lookup"><span data-stu-id="8574e-242">You can also invoke hello clean-up procedure from hello sample command line.</span></span>

<span data-ttu-id="8574e-243">hello exemplet har hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="8574e-243">hello sample has hello following prerequisites:</span></span>

* <span data-ttu-id="8574e-244">En aktiv Microsoft Azure-prenumeration och dess prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="8574e-244">An active Microsoft Azure subscription and its subscription ID.</span></span>
* <span data-ttu-id="8574e-245">Ett certifikat för hello prenumeration med hello motsvarande privata nyckel.</span><span class="sxs-lookup"><span data-stu-id="8574e-245">A management certificate for hello subscription with hello corresponding private key.</span></span> <span data-ttu-id="8574e-246">hello certifikatet ska installeras i hello aktuella användare privat lagring på hello datorn används toorun hello sampel.</span><span class="sxs-lookup"><span data-stu-id="8574e-246">hello certificate should be installed in hello current user private storage on hello machine used toorun hello sample.</span></span>
* <span data-ttu-id="8574e-247">Ett aktivt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="8574e-247">An active HDInsight cluster.</span></span>
* <span data-ttu-id="8574e-248">Ett Azure Storage-konto länkas toohello HDInsight-kluster från hello tidigare nödvändiga tillsammans med hello motsvarande primära eller sekundära åtkomstnyckel.</span><span class="sxs-lookup"><span data-stu-id="8574e-248">An Azure Storage account linked toohello HDInsight cluster from hello previous prerequisite, together with hello corresponding primary, or secondary access key.</span></span>

<span data-ttu-id="8574e-249">Alla hello information från hello krav bör vara angivna toohello exempelkonfigurationsfilen innan hello exempel körs.</span><span class="sxs-lookup"><span data-stu-id="8574e-249">All of hello information from hello prerequisites should be entered toohello sample configuration file before hello sample is run.</span></span> <span data-ttu-id="8574e-250">Det finns två möjliga sätt toodo den:</span><span class="sxs-lookup"><span data-stu-id="8574e-250">There are two possible ways toodo it:</span></span>

* <span data-ttu-id="8574e-251">Redigera hello app.config-fil i rotkatalogen för hello exempel och skapa hello-exempel</span><span class="sxs-lookup"><span data-stu-id="8574e-251">Edit hello app.config file in hello sample root directory and then build hello sample</span></span>
* <span data-ttu-id="8574e-252">Skapa först hello exempel och sedan redigera AvroHDISample.exe.config i hello-katalogen</span><span class="sxs-lookup"><span data-stu-id="8574e-252">First build hello sample, and then edit AvroHDISample.exe.config in hello build directory</span></span>

<span data-ttu-id="8574e-253">I båda fallen måste alla redigeringar bör göras i hello  **<appSettings>**  inställningar.</span><span class="sxs-lookup"><span data-stu-id="8574e-253">In both cases, all edits should be done in hello **<appSettings>** settings section.</span></span> <span data-ttu-id="8574e-254">Följ hello kommentarer i hello-filen.</span><span class="sxs-lookup"><span data-stu-id="8574e-254">Follow hello comments in hello file.</span></span>
<span data-ttu-id="8574e-255">hello körs exempel från kommandoraden hello genom att köra följande kommando (där hello ZIP-filen med hello exempel antogs toobe extraheras tooC:\AvroHDISample; om annars används hello relevanta filsökväg) hello:</span><span class="sxs-lookup"><span data-stu-id="8574e-255">hello sample is run from hello command line by executing hello following command (where hello .zip file with hello sample was assumed toobe extracted tooC:\AvroHDISample; if otherwise, use hello relevant file path):</span></span>

    AvroHDISample run C:\AvroHDISample\Data

<span data-ttu-id="8574e-256">tooclean in hello klustret, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="8574e-256">tooclean up hello cluster, run hello following command:</span></span>

    AvroHDISample clean

[deflate-100]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.100).aspx
[deflate-110]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.110).aspx
