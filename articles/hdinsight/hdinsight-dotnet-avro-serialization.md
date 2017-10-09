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
# <a name="serialize-data-in-hadoop-with-hello-microsoft-avro-library"></a>Serialisera data i Hadoop med hello Microsoft Avro Library

>[!NOTE]
>Hej Avro SDK stöds inte längre av Microsoft. hello-biblioteket är öppen källkod som stöds. hello källor för hello biblioteket finns i [Github](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro).

Det här avsnittet visar hur toouse hello [Microsoft Avro Library](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro) tooserialize objekt och andra data strukturer i dataströmmar toopersist dem toomemory, en databas eller en fil. Den visar även hur toodeserialize dem toorecover hello ursprungliga objekten.

[!INCLUDE [windows-only](../../includes/hdinsight-windows-only.md)]

## <a name="apache-avro"></a>Apache Avro
Hej <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsoft Avro Library</a> implementerar hello Apache Avro system för serialisering för hello Microsoft.NET miljö. Apache Avro ger en kompakta binära dataöverföringsformat för serialisering. Den använder <a href="http://www.json.org" target="_blank">JSON</a> toodefine ett språkoberoende schema som meddelar försäkringar språksamverkan. Data serialiseras på ett språk kan läsas i en annan. Stöds för närvarande C, C++, C#, Java, PHP, Python och Ruby. Detaljerad information om hello-format finns i hello <a href="http://avro.apache.org/docs/current/spec.html" target="_blank">specifikation för Apache Avro</a>. 

>[!NOTE]
>hello Microsoft Avro Library stöder inte hello fjärrproceduranrop (RPC-anrop)-anrop en del av den här specifikationen.
>

hello serialiseras representation av ett objekt i hello Avro system består av två delar: schemat och faktiskt värde. Hej Avro-schemats beskriver hello språkoberoende datamodellen hello serialiserade data med JSON. Den visas sida vid sida med en a binär representation av data. Med hello schemat separat från hello binär representation tillåter varje objekt toobe som skrivits med ingen per värde omkostnader, gör serialisering snabb och hello representation små.

## <a name="hello-hadoop-scenario"></a>Hej Hadoop scenario
hello Apache Avro-serialiseringsformatet används ofta i Azure HDInsight och andra Apache Hadoop-miljöer. Avro ger ett bekvämt sätt toorepresent komplicerade datastrukturer inom ett Hadoop-MapReduce-jobb. hello format Avro-filernas (Avro objektet behållaren-fil) har utformad toosupport hello distribuerade MapReduce-programmeringsmodellen. hello nyckelegenskap som aktiverar hello fördelningen är att hello filerna är ”delbara” i hello mening att söka efter valfri punkt i en fil och börja läsa från ett visst block.

## <a name="serialization-in-avro-library"></a>Serialisering i Avro Library
hello .NET-bibliotek för Avro stöder serialisering objekt på två sätt:

* **reflektion** -hello JSON-schema för hello typer skapas automatiskt från hello data kontraktet attribut för hello .NET typer toobe serialiseras.
* **allmän post** -A JSON-schema är explicit i en post som representeras av hello [ **AvroRecord** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) klassen när inga .NET-typer finns toodescribe hello schemat för hello data toobe serialiserad.

När hello dataschemat är känt tooboth hello skrivare och läsare hello dataströmmen, skickas hello data utan ett schema. I fall när en Avro objektet behållare fil används hello schemat lagras i hello-filen. Andra parametrar, till exempel hello codec som används för datakomprimering, kan anges. Dessa scenarier beskrivs i detalj och visas i följande kodexempel hello:

## <a name="install-avro-library"></a>Installera Avro Library
hello följande krävs innan du installerar hello-biblioteket:

* <a href="http://www.microsoft.com/download/details.aspx?id=17851" target="_blank">Microsoft .NET Framework 4</a>
* <a href="http://james.newtonking.com/json" target="_blank">Newtonsoft Json.NET</a> (6.0.4 eller senare)

Observera att hello Newtonsoft.Json.dll beroende hämtas automatiskt med hello installation av hello Microsoft Avro Library. hello proceduren finns i följande avsnitt hello:

hello Microsoft Avro Library distribueras som ett NuGet-paket som kan installeras från Visual Studio via hello nedan:

1. Välj hello **projekt** fliken -> **hantera NuGet-paket...**
2. Sök efter ”Microsoft.Hadoop.Avro” i hello **Sök Online** rutan.
3. Klicka på hello **installera** knappen för nästa**Microsoft Azure HDInsight Avro Library**.

Observera att hello Newtonsoft.Json.dll (> = 6.0.4) beroende hämtas även automatiskt med hello Microsoft Avro Library.

hello Microsoft Avro Library källkoden finns på [Github](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro).

## <a name="compile-schemas-using-avro-library"></a>Kompilera scheman med Avro Library
hello Microsoft Avro Library innehåller en kodgenerering verktyg som gör att skapa C# typer automatiskt baserat på hello tidigare definierats JSON-schema. hello code generation verktyget distribueras inte som en binär körbara, men är lätta att bygga via hello nedan:

1. Hämta hello ZIP-filen med hello senaste versionen av HDInsight SDK källkod från <a href="http://hadoopsdk.codeplex.com/SourceControl/latest#" target="_blank">Microsoft .NET SDK för Hadoop</a>. (Klicka på hello **hämta** och inte hello **hämtar** flik.)
2. Extrahera hello HDInsight SDK tooa katalog på hello datorn med .NET Framework 4 installerat och anslutet toohello Internet för att ladda ned nödvändiga beroendet NuGet-paket. Nedan, förutsätter vi att hello källkoden är extraherade tooC:\SDK.
3. Gå toohello mappen C:\SDK\src\Microsoft.Hadoop.Avro.Tools och kör build.bat. (hello filen anrop MSBuild från hello 32-bitars distribution av hello .NET Framework. Om du vill att toouse hello 64-bitarsversionen, redigera build.bat hello följande kommentarer i hello-fil.) Se till att hello build har lyckats. (På vissa datorer MSBuild kan generera varningar. Dessa varningar påverkar inte hello verktyget så länge det finns inga build-fel.)
4. hello kompileras som finns i C:\SDK\Bin\Unsigned\Release\Microsoft.Hadoop.Avro.Tools.

tooget bekant med hello kommandoradssyntaxen, kör hello följande kommando från hello mappen hello code generation verktyget:`Microsoft.Hadoop.Avro.Tools help /c:codegen`

tootest hello verktyg du kan skapa C#-klasser från hello JSON-schema exempelfilen medföljer hello källkoden. Kör följande kommando hello:

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:

Detta ska tooproduce två C#-filer i katalogen hello: SensorData.cs och Location.cs.

toounderstand hello logik som hello code generation verktyget använder vid konvertering av tooC # hello JSON schematyper, se hello fil GenerationVerification.feature i C:\SDK\src\Microsoft.Hadoop.Avro.Tools\Doc.

Namnområden extraheras från hello JSON-schema med hello logiken i hello-fil som nämns i hello föregående stycke. Namnområden som extraheras från hello schema företräde framför allt medföljer hello/n-parametern i kommandoraden för hello-verktyget. Om du vill toooverride hello namnområden som ingår i hello schemat använder du hello /nf-parametern. Till exempel toochange alla namnområden från hello SampleJSONSchema.avsc toomy.own.nspace köra hello följande kommando:

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:. /nf:my.own.nspace

## <a name="about-hello-samples"></a>Om hello-exempel
Sex exemplen i det här avsnittet visar olika scenarier som stöds av hello Microsoft Avro Library. hello Microsoft Avro Library är utformad toowork med en dataström. I det här exemplet ändras data via minne strömmar i stället filen dataströmmar eller databaser för enkelhetens skull. hello metod i en produktionsmiljö är beroende av hello exakt scenariokrav, datakällan och volymen, prestanda begränsningar och andra faktorer.

Hej första två exempel visas hur tooserialize och deserialisera data i dataströmmen minnesbuffertar med hjälp av reflektion och allmänna poster. hello schemat i båda fallen antas toobe delas mellan hello läsare och skrivare för out-of-band.

hello tredje och fjärde exempel visas hur tooserialize och deserialisera data med hjälp av hello Avro objektet behållarfiler. När data lagras i en fil för Avro-behållare, lagras dess schema alltid med det eftersom hello schemat måste vara delad för deserialisering.

hello prov som innehåller hello första fyra exempel kan hämtas från hello <a href="http://code.msdn.microsoft.com/Serialize-data-with-the-86055923" target="_blank">Azure kodexempel</a> plats.

Hej femte exempel visar hur toouse en anpassad komprimerings-codec för Avro objekt behållarfiler. Ett exempel som innehåller hello kod för det här exemplet kan hämtas från hello <a href="http://code.msdn.microsoft.com/Serialize-data-with-the-67159111" target="_blank">Azure kodexempel</a> plats.

hello sjätte exempel visas hur toouse Avro serialisering tooupload data tooAzure Blob storage och analysera den med hjälp av Hive med HDInsight (Hadoop)-kluster. Du kan hämta från hello <a href="https://code.msdn.microsoft.com/Using-Avro-to-upload-data-ae81b1e3" target="_blank">Azure kodexempel</a> plats.

Här följer länkar toohello sex exempel som beskrivs i avsnittet hello:

* <a href="#Scenario1">**Serialisering med reflektion** </a> -hello JSON-schema för typer toobe serialiseras skapas automatiskt från hello data kontraktet attribut.
* <a href="#Scenario2">**Serialisering med allmän post** </a> -hello JSON-schema är explicit i en post när ingen .NET-typ är tillgängligt för reflektion.
* <a href="#Scenario3">**Serialisering med reflektion objektet behållarfiler** </a> -hello JSON-schema har skapats automatiskt och delade tillsammans med hello serialiserade data via en Avro objektet behållare fil.
* <a href="#Scenario4">**Serialisering med allmän post objektet behållarfiler** </a> -hello JSON-schema är uttryckligen anges innan hello serialisering och delade tillsammans med hello data via en Avro objektet behållare fil.
* <a href="#Scenario5">**Serialisering med en anpassad komprimerings-codec objektet behållarfiler** </a> -hello exempel visas hur toocreate ett Avro objekt behållare fil med en anpassad .NET-implementering av hello Deflate data komprimerings-codec.
* <a href="#Scenario6">**Med hjälp av Avro tooupload data för hello tjänsten Microsoft Azure HDInsight** </a> -hello exemplet illustrerar hur Avro serialisering samverkar med hello HDInsight-tjänst. Ett aktivt Azure-prenumeration och åtkomst tooan Azure HDInsight-kluster måste toorun det här exemplet.

## <a name="Scenario1"></a>Exempel 1: Serialisering med reflektion
hello JSON-schema för hello typer kan automatiskt genereras av hello Microsoft Avro Library via reflektion från hello data kontraktet attribut för hello C# objekt toobe serialiseras. hello Microsoft Avro Library skapar en [ **IAvroSeralizer<T>**  ](http://msdn.microsoft.com/library/dn627341.aspx) tooidentify hello fält toobe serialiseras.

I det här exemplet objekt (en **SensorData** klass med en medlem **plats** struct) är serialiserade tooa minne dataströmmen och den här dataströmmen i sin tur avserialiseras. hello resultatet är jämfört med toohello första instansen tooconfirm som hello **SensorData** objekt som återställts är identiska toohello ursprungliga.

hello schemat i det här exemplet förutsätts toobe delas mellan hello läsare och skrivare, så hello Avro objektet behållares format inte krävs. Ett exempel på hur tooserialize och deserialisera data i minnesbuffertar med hjälp av reflektion med hello objektet behållares format när hello schemat måste delas med hello data finns <a href="#Scenario3">serialisering med reflektionbehållarfileriobjektet</a>.

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


## <a name="sample-2-serialization-with-a-generic-record"></a>Exempel 2: Serialisering med en allmän post
En JSON-schema kan uttryckligen anges i en allmän post när reflektion inte kan användas eftersom hello data inte kan representeras via .NET-klasser med ett datakontrakt. Den här metoden går långsammare än med hjälp av reflektion. I sådana fall kan hello schemat för hello data också vara dynamisk, som är inte känd vid kompileringen. Data visas i form av kommaavgränsade värden (CSV) filer vars schema är okänd tills det transformeras toohello Avro-formatet vid körning är ett exempel på den här typen av dynamisk scenario.

Det här exemplet illustrerar hur toocreate och använda en [ **AvroRecord** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) tooexplicitly ange en JSON-schema, hur toopopulate med hello data och sedan hur tooserialize och deserialisera det. hello resultatet jämförs sedan toohello inledande instans tooconfirm som hello post återställs är identiska toohello ursprungliga.

hello schemat i det här exemplet förutsätts toobe delas mellan hello läsare och skrivare, så hello Avro objektet behållares format inte krävs. Ett exempel på hur tooserialize och deserialisera data i minnesbuffertar med hjälp av en allmän post med hello objektet behållares format när hello schemat måste ingå i hello serialiserade data finns hello <a href="#Scenario4">serialisering med objektbehållare filer med allmän post</a> exempel.

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


## <a name="sample-3-serialization-using-object-container-files-and-serialization-with-reflection"></a>Exempel 3: Serialisering med reflektion objektet behållarfiler och serialisering
Det här exemplet är liknande toohello scenario i hello <a href="#Scenario1"> första exemplet</a>, där hello schema har angetts implicit med reflektion. hello skillnaden är att här hello schemat antas inte toobe kända toohello läsare som deserializes den. Hej **SensorData** objekt toobe serialiseras och deras implicit angivna schemat lagras i en Avro objektet behållaren-fil som representeras av hello [ **AvroContainer** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) klass.

hello data serialiseras i det här exemplet med [ **SequentialWriter<SensorData>**  ](http://msdn.microsoft.com/library/dn627340.aspx) och avserialiserat med [ **SequentialReader<SensorData>** ](http://msdn.microsoft.com/library/dn627340.aspx). hello resultatet blir sedan jämfört med toohello inledande instanser tooensure identitet.

hello i hello dataobjekt behållaren filer komprimeras via hello standard [ **Deflate** ] [ deflate-100] komprimerings-codec från .NET Framework 4. Se hello <a href="#Scenario5"> femte exempel</a> i det här avsnittet toolearn hur toouse en senare och högre version av hello [ **Deflate** ] [ deflate-110] komprimering codec som är tillgängliga i .NET Framework 4.5.

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


## <a name="sample-4-serialization-using-object-container-files-and-serialization-with-generic-record"></a>Exempel 4: Serialisering med objektet behållarfiler och serialisering med allmän post
Det här exemplet är liknande toohello scenario i hello <a href="#Scenario2"> andra exemplet</a>där hello schemat anges explicit med JSON. hello skillnaden är att här hello schemat antas inte toobe kända toohello läsare som deserializes den.

hello test datauppsättning har samlats in i en lista över [ **AvroRecord** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) objekt via ett uttryckligen definierade JSON-schema, och lagras sedan i en fil för behållaren av objektet som representeras av hello [ **AvroContainer** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) klass. Den här behållaren skapar en skrivare som har använt tooserialize hello data, okomprimerade, tooa minne dataström som sparas tooa fil. Hej [ **Codec.Null** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.codec.null.aspx) parametern används för att skapa hello reader anger inte komprimeras dessa data.

hello data sedan läsa från filen hello och avserialiseras till en samling objekt. Den här samlingen är jämfört med toohello första listan över Avro poster tooconfirm att de är identiska.

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




## <a name="sample-5-serialization-using-object-container-files-with-a-custom-compression-codec"></a>Exempel 5: Serialisering med en anpassad komprimerings-codec objektet behållarfiler
Hej femte exempel visar hur toouse en anpassad komprimerings-codec för Avro objekt behållarfiler. Ett exempel som innehåller hello kod för det här exemplet kan hämtas från hello [Azure kodexempel](http://code.msdn.microsoft.com/Serialize-data-with-the-67159111) plats.

Hej [Avro-specifikationen](http://avro.apache.org/docs/current/spec.html#Required+Codecs) tillåter användning av ett valfritt komprimerings-codec (förutom för**Null** och **Deflate** standardinställningarna). Det här exemplet inte implementerar en ny codec, till exempel Snappy (anges som en valfri codec i hello [Avro-specifikationen](http://avro.apache.org/docs/current/spec.html#snappy)). Den visar hur toouse hello .NET Framework 4.5 implementering av hello [ **Deflate** ] [ deflate-110] codec, vilket ger en bättre komprimeringsalgoritm baserat på hello [zlib](http://zlib.net/) Komprimeringsbiblioteket än hello standardversionen för .NET Framework 4.

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

## <a name="sample-6-using-avro-tooupload-data-for-hello-microsoft-azure-hdinsight-service"></a>Exempel 6: Använder Avro tooupload data för hello Microsoft Azure HDInsight-tjänst
hello exemplet sjätte vissa programming tekniker relaterade toointeracting med hello Azure HDInsight-tjänst. Ett exempel som innehåller hello kod för det här exemplet kan hämtas från hello [Azure kodexempel](https://code.msdn.microsoft.com/Using-Avro-to-upload-data-ae81b1e3) plats.

hello exempel hello följande uppgifter:

* Ansluter tooan befintliga service-kluster i HDInsight.
* Serialiserar flera CSV-filer och överför hello resultatet tooAzure Blob storage. (hello CSV-filer ska distribueras tillsammans med hello exempel och representerar ett utdrag ur AMEX börs historiska data som distribueras av [Infochimps](http://www.infochimps.com/) för hello period 1970 2010. hello exempel läser data från en CSV, konverterar hello poster tooinstances av hello **börs** klassen och Serialiserar dem med hjälp av reflektion. Typdefinitionen för lager skapas från ett JSON-schema via hello Microsoft Avro Library code generation verktyget.
* Skapar en ny extern tabell som kallas **lagerlista** i Hive, och länkar den toohello data på hello föregående steg.
* Kör en fråga med hjälp av Hive via hello **lagerlista** tabell.

Dessutom utför en rensning procedur hello exempel före och efter att ha genomfört större operationer. Under hello rensning relaterade alla hello Azure Blob-data och mappar tas bort och hello Hive-tabellen har släppts. Du kan även anropa hello Rensa proceduren från hello kommandorad.

hello exemplet har hello följande krav:

* En aktiv Microsoft Azure-prenumeration och dess prenumerations-ID.
* Ett certifikat för hello prenumeration med hello motsvarande privata nyckel. hello certifikatet ska installeras i hello aktuella användare privat lagring på hello datorn används toorun hello sampel.
* Ett aktivt HDInsight-kluster.
* Ett Azure Storage-konto länkas toohello HDInsight-kluster från hello tidigare nödvändiga tillsammans med hello motsvarande primära eller sekundära åtkomstnyckel.

Alla hello information från hello krav bör vara angivna toohello exempelkonfigurationsfilen innan hello exempel körs. Det finns två möjliga sätt toodo den:

* Redigera hello app.config-fil i rotkatalogen för hello exempel och skapa hello-exempel
* Skapa först hello exempel och sedan redigera AvroHDISample.exe.config i hello-katalogen

I båda fallen måste alla redigeringar bör göras i hello  **<appSettings>**  inställningar. Följ hello kommentarer i hello-filen.
hello körs exempel från kommandoraden hello genom att köra följande kommando (där hello ZIP-filen med hello exempel antogs toobe extraheras tooC:\AvroHDISample; om annars används hello relevanta filsökväg) hello:

    AvroHDISample run C:\AvroHDISample\Data

tooclean in hello klustret, kör hello följande kommando:

    AvroHDISample clean

[deflate-100]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.100).aspx
[deflate-110]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.110).aspx
