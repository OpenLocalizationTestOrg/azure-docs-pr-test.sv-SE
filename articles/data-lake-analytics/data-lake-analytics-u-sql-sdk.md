---
title: "aaaScale U-SQL lokalt köra och testa med Azure Data Lake U-SQL-SDK | Microsoft Docs"
description: "Lär dig hur toouse Azure Data Lake U-SQL-SDK tooscale U-SQL-jobb lokala körs och testa med kommandoraden och programmeringsgränssnitt på din lokala arbetsstationen."
services: data-lake-analytics
documentationcenter: 
author: 
manager: 
editor: 
ms.assetid: 
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/01/2017
ms.author: yanacai
ms.openlocfilehash: 2b0a16229789268e333f723ff6fc2c3efdc29905
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scale-u-sql-local-run-and-test-with-azure-data-lake-u-sql-sdk"></a>Skala lokal U-SQL-köra och testa med Azure Data Lake U-SQL-SDK

När du utvecklar U-SQL-skript, är det vanliga toorun och testar U-SQL-skript lokalt skicka in innan den toocloud. Azure Data Lake innehåller en Nuget-paketet kallas SDK för Azure Data Lake U-SQL i det här scenariot, via som du sedan kan du skala lokal U-SQL kör och test. Det är också möjligt toointegrate denna U-SQL testa med CI (kontinuerlig Integration) system tooautomate hello kompilera och test.

Om du bryr dig om hur toomanually lokala körs och felsöka U-SQL-skript med GUI-tooling kan du använda Azure Data Lake-verktyg för Visual Studio för den. Mer information från [här](data-lake-analytics-data-lake-tools-local-run.md).

## <a name="install-azure-data-lake-u-sql-sdk"></a>Installera Azure Data Lake U-SQL-SDK

Du kan hämta hello Azure Data Lake U-SQL-SDK [här](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/) på Nuget.org. Och innan du använder den toomake att du har beroenden på följande sätt.

### <a name="dependencies"></a>Beroenden

hello Data Lake U-SQL-SDK kräver hello följande beroenden:

- [Microsoft .NET Framework 4.6 eller nyare](https://www.microsoft.com/download/details.aspx?id=17851).
- Microsoft Visual C++-14 och Windows SDK 10.0.10240.0 eller senare (som kallas CppSDK i den här artikeln). Det finns två sätt tooget CppSDK:

    - Installera [Visual Studio Community Edition](https://developer.microsoft.com/downloads/vs-thankyou). Du har en \Windows Kits\10 mapp under hello mappen Program – till exempel C:\Program Files (x86) \Windows Kits\10\. Du hittar också hello SDK för Windows 10 version under \Windows Kits\10\Lib. Om du inte ser dessa mappar, installera om Visual Studio och vara säker på att tooselect hello SDK för Windows 10 under hello installationen. Om du har det installeras med Visual Studio hittar lokala hello U-SQL-kompileraren den automatiskt.

    ![Data Lake-verktyg för Visual Studio lokala kör Windows 10-SDK](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-windows-10-sdk.png)

    - Installera [Data Lake-verktyg för Visual Studio](http://aka.ms/adltoolsvs). Du kan hitta hello färdigförpackade Visual C++ och Windows SDK filer i C:\Program Files (x86) \Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\ADL Tools\X.X.XXXX.X\CppSDK. I det här fallet hittar lokala hello U-SQL-kompileraren inte hello beroenden automatiskt. Du behöver toospecify hello CppSDK sökvägen för den. Du kan kopiera hello tooanother platsen, eller så kan du använda den som den är.

## <a name="understand-basic-concepts"></a>Förstå grundläggande begrepp

### <a name="data-root"></a>Dataroten

hello data-rotmappen är ett ”lokalt Arkiv” för hello lokala beräkningskonto. Det är likvärdiga toohello Azure Data Lake Store-konto för Data Lake Analytics-konto. Växla tooa olika data-rotmappen som är precis som växlar tooa olika store-konto. Om du vill tooaccess ofta delas data med olika dataroten mappar, måste du använda absoluta sökvägar i skript. Skapa filen system symboliska länkar (till exempel **mklink** på NTFS) under hello-dataroten mappen toopoint toohello delade data.

hello data-rotmappen används för att:

- Lagra lokala metadata, inklusive databaser, tabeller, tabellvärdesfunktioner (Tabellvärdesfunktioner) och sammansättningar.
- Leta upp hello indata och utdata sökvägar som har definierats som relativa sökvägar i U-SQL. Använda relativa sökvägar gör det enklare toodeploy tooAzure dina U-SQL-projekt.

### <a name="file-path-in-u-sql"></a>Sökvägen i U-SQL

Du kan använda både en relativ sökväg och en lokal absolut sökväg i U-SQL-skript. hello relativ sökväg är relativ toohello angivna data-sökvägen till rotmappen. Vi rekommenderar att du använder ”/” som hello sökväg avgränsare toomake skripten som är kompatibel med hello på serversidan. Här följer några exempel på relativa sökvägar och deras motsvarande absoluta sökvägar. I det här exemplet är C:\LocalRunDataRoot hello data-rotmappen.

|Relativ sökväg|Absolut sökväg|
|-------------|-------------|
|/ABC/def/Input.csv |C:\LocalRunDataRoot\abc\def\input.csv|
|ABC/def/Input.csv  |C:\LocalRunDataRoot\abc\def\input.csv|
|D:/ABC/def/Input.csv |D:\abc\def\input.csv|

### <a name="working-directory"></a>Arbetskatalog

När du kör hello U-SQL-skript lokalt, skapas en arbetskatalog under kompilering under katalogen körs. Dessutom toohello kompilering utdata hello behövs runtime-filer för lokala körningen blir shadow kopierade toothis arbetskatalogen. hello working directory rotmappen kallas ”ScopeWorkDir” och hello filer under hello arbetskatalog är följande:

|Filen/katalogen|Filen/katalogen|Filen/katalogen|Definition|Beskrivning|
|--------------|--------------|--------------|----------|-----------|
|C6A101DDCB470506| | |Hash-sträng av körningsversion|Skuggkopia av runtime-filer som behövs för lokal körning|
| |Script_66AE4909AA0ED06C| |Script namn + hash-sträng med sökvägen för skriptet|Utdata för kompilering och körning steg loggning|
| | |\_skriptet\_.abr|Utdata för kompilator|Algebra fil|
| | |\_ScopeCodeGen\_. *|Utdata för kompilator|Genererade hanterad kod|
| | |\_ScopeCodeGenEngine\_. *|Utdata för kompilator|Genererade maskinkod|
| | |Refererade sammansättningar|Sammansättningsreferensen|Sammansättningsfiler|
| | |deployed_resources|Resurs-distribution|Resursfiler för distribution|
| | |xxxxxxxx.xxx[1..n]\_\*. *|Körningsloggen|Logg över utförande|


## <a name="use-hello-sdk-from-hello-command-line"></a>Använd hello SDK från hello kommandorad

### <a name="command-line-interface-of-hello-helper-application"></a>Kommandoradsgränssnitt för hello hjälpprogram

Under SDK directory\build\runtime är LocalRunHelper.exe hello kommandoradsverktyget hjälpprogram som innehåller gränssnitt toomost hello används vanligtvis för lokal kör funktioner. Observera att både hello kommandot och hello argumentväxlar är skiftlägeskänsliga. tooinvoke den:

    LocalRunHelper.exe <command> <Required-Command-Arguments> [Optional-Command-Arguments]

Kör LocalRunHelper.exe utan argument eller med hello **hjälp** växla tooshow hello hjälpinformation:

    > LocalRunHelper.exe help

        Command 'help' :  Show usage information
        Command 'compile' :  Compile hello script
        Required Arguments :
            -Script param
                    Script File Path
        Optional Arguments :
            -Shallow [default value 'False']
                    Shallow compile

I hello hjälpinformation:

-  **Kommandot** ger hello kommandots namn.  
-  **Argumentet är obligatoriskt** listas argument måste anges.  
-  **Valfria argumentet** listas argument som är valfria med standardvärden.  Valfri boolesk argument ha inte parametrar, och deras utseende innebär negativt tootheir standardvärdet.

### <a name="return-value-and-logging"></a>Returvärdet och loggning

hello hjälpprogram returnerar **0** för lyckad och **-1** för felet. Som standard skickar hello helper alla meddelanden toohello aktuella konsolen. De flesta av hello kommandon stöder dock hello **- MessageOut sökväg_till_loggfil** valfritt argument som omdirigerar hello matar ut tooa loggfilen.

### <a name="environment-variable-configuring"></a>Miljö variabeln konfigurera

Lokal U-SQL kör behov angivna data från en rotcertifikatutfärdare som konto för lokal lagring, samt en angiven CppSDK sökväg för beroenden. Du kan både set hello-argumentet i kommandorads- eller ange miljövariabeln för dessa.

- Ange hello **SCOPE_CPP_SDK** miljövariabeln.

    Om du får Microsoft Visual C++ och hello Windows SDK genom att installera Data Lake-verktyg för Visual Studio kan du kontrollera att du har hello följande mapp:

        C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\Microsoft Azure Data Lake Tools for Visual Studio 2015\X.X.XXXX.X\CppSDK

    Definiera en ny miljövariabel som heter **SCOPE_CPP_SDK** toopoint toothis directory. Kopiera hello mappen toohello annan plats och ange **SCOPE_CPP_SDK** som.

    Du kan ange hello i miljövariabeln tillägg toosetting hello, **- CppSDK** argumentet när du använder hello-kommandoraden. Det här argumentet skriver över dina standard CppSDK miljövariabeln.

- Ange hello **LOCALRUN_DATAROOT** miljövariabeln.

    Definiera en ny miljövariabel som heter **LOCALRUN_DATAROOT** som pekar toohello data rot.

    Du kan ange hello i miljövariabeln tillägg toosetting hello, **- DataRoot** argumentet med hello data-rotsökvägen när du använder en kommandorad. Det här argumentet skriver över dina standard dataroten miljövariabeln. Du måste tooadd argumentet tooevery kommandoraden du kör så att du kan skriva över standard hello-dataroten miljövariabeln för alla åtgärder.

### <a name="sdk-command-line-usage-samples"></a>SDK kommandoraden användning prover

#### <a name="compile-and-run"></a>Kompilera och kör

Hej **kör** kommandot används toocompile hello skript och köra kompilerade resultatet. Dess kommandoradsargument är en kombination av dessa från **Kompilera** och **köra**.

    LocalRunHelper run -Script path_to_usql_script.usql [optional_arguments]

hello följande är valfria argument för **kör**:


|Argumentet|Standardvärde|Beskrivning|
|--------|-------------|-----------|
|-Bakomliggande koden|False|hello skriptet har .cs koden bakom|
|-CppSDK| |CppSDK Directory|
|-DataRoot| DataRoot miljövariabel|DataRoot för lokala kör standard för miljövariabeln 'LOCALRUN_DATAROOT'|
|-MessageOut| |Dump meddelanden på konsolen tooa fil|
|-Parallell|1|Kör hello plan med hello angetts parallellitet|
|-Referenser| |Lista över sökvägar tooextra referenssammansättningar eller datafiler i koden bakom, avgränsade med ';'|
|-UdoRedirect|False|Generera Udo sammansättningen omdirigering config|
|-UseDatabase|master|Databasen toouse för koden bakom tillfällig sammansättning registrering|
|-Verbose|False|Visa detaljerade utdata från runtime|
|-WorkDir|Aktuell katalog|Katalogen för kompileraren användnings- och utdata|
|-RunScopeCEP|0|ScopeCEP läge toouse|
|-ScopeCEPTempPath|Temp|Sökvägen toouse för strömmande data|
|-OptFlags| |Kommaavgränsad lista över optimering flaggor|


Här är ett exempel:

    LocalRunHelper run -Script d:\test\test1.usql -WorkDir d:\test\bin -CodeBehind -References "d:\asm\ref1.dll;d:\asm\ref2.dll" -UseDatabase testDB –Parallel 5 -Verbose

Förutom att kombinera **Kompilera** och **köra**, kan du kompilera och köra körbara filer hello kompileras separat.

#### <a name="compile-a-u-sql-script"></a>Kompilera ett U-SQL-skript

Hej **Kompilera** kommandot är tooexecutables för används toocompile U-SQL-skript.

    LocalRunHelper compile -Script path_to_usql_script.usql [optional_arguments]

hello följande är valfria argument för **Kompilera**:


|Argumentet|Beskrivning|
|--------|-----------|
| -Bakomliggande koden [standardvärdet 'False']|hello skriptet har .cs koden bakom|
| -CppSDK [standardvärde '']|CppSDK Directory|
| -DataRoot [standardvärdet 'DataRoot miljövariabeln']|DataRoot för lokala kör standard för miljövariabeln 'LOCALRUN_DATAROOT'|
| -MessageOut [standardvärde '']|Dump meddelanden på konsolen tooa fil|
| -Hänvisar till [standardvärde '']|Lista över sökvägar tooextra referenssammansättningar eller datafiler i koden bakom, avgränsade med ';'|
| -Kort [standardvärdet 'False']|Lite kompilera|
| -UdoRedirect [standardvärdet 'False']|Generera Udo sammansättningen omdirigering config|
| -UseDatabase [standardvärdet 'master']|Databasen toouse för koden bakom tillfällig sammansättning registrering|
| -WorkDir [standardvärdet 'Katalogen']|Katalogen för kompileraren användnings- och utdata|
| -RunScopeCEP [standardvärdet '0']|ScopeCEP läge toouse|
| -ScopeCEPTempPath [standardvärdet 'temp']|Sökvägen toouse för strömmande data|
| -OptFlags [standardvärde '']|Kommaavgränsad lista över optimering flaggor|


Här följer några exempel på användning.

Kompilera ett U-SQL-skript:

    LocalRunHelper compile -Script d:\test\test1.usql

Kompilera ett U-SQL-skript och ange hello data-rotmappen. Observera att detta ersätter hello set-miljövariabeln.

    LocalRunHelper compile -Script d:\test\test1.usql –DataRoot c:\DataRoot

Kompilera ett U-SQL-skript och ange en arbetskatalog och referenssammansättning databasen:

    LocalRunHelper compile -Script d:\test\test1.usql -WorkDir d:\test\bin -References "d:\asm\ref1.dll;d:\asm\ref2.dll" -UseDatabase testDB

#### <a name="execute-compiled-results"></a>Köra kompilerade resultat

Hej **köra** kommandot är används tooexecute kompileras resultat.   

    LocalRunHelper execute -Algebra path_to_compiled_algebra_file [optional_arguments]

hello följande är valfria argument för **köra**:

|Argumentet|Beskrivning|
|--------|-----------|
|-DataRoot [standardvärde '']|Dataroten för körning av metadata. Standard toohello **LOCALRUN_DATAROOT** miljövariabeln.|
|-MessageOut [standardvärde '']|Dumpa meddelanden på hello tooa-fil.|
|-Parallell [standardvärdet '1']|Indikator toorun hello genereras lokala kör steg med hello angetts parallellitet nivå.|
|-Verbose [standardvärde 'False']|Indikator tooshow detaljerade utdata från körning.|

Här är ett exempel på användning:

    LocalRunHelper execute -Algebra d:\test\workdir\C6A101DDCB470506\Script_66AE4909AA0ED06C\__script__.abr –DataRoot c:\DataRoot –Parallel 5


## <a name="use-hello-sdk-with-programming-interfaces"></a>Använda hello SDK med programmeringsgränssnitt

hello programmeringsgränssnitt finns i hello LocalRunHelper.exe. Du kan använda dem toointegrate hello funktioner för hello U-SQL-SDK och hello C# test framework tooscale U-SQL-skript lokalt testet. I den här artikeln ska jag använda hello standard C# enhet test projektet tooshow hur toouse dessa gränssnitt tootest U-SQL-skript.

### <a name="step-1-create-c-unit-test-project-and-configuration"></a>Steg 1: Skapa C# enhet test-projekt och konfiguration

- Skapa ett C# enhet testprojekt via fil > Nytt > Projekt > Visual C# > Testa > enhet Testa projektet.
- Lägg till LocalRunHelper.exe som en referens för hello-projekt. Hej LocalRunHelper.exe finns i \build\runtime\LocalRunHelper.exe i Nuget-paketet.

    ![Azure Data Lake U-SQL-SDK Lägg till referens](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-add-reference.png)

- U-SQL-SDK **endast** x64 Stödmiljö, se till att tooset build platform mål som x64. Du kan ange som via projektegenskapen > Skapa > plattform mål.

    ![Azure Data Lake U-SQL-SDK konfigurera x64 projekt](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-configure-x64.png)

- Se till att tooset din testmiljö som x64. I Visual Studio kan du ändra det genom testning > Testa inställningar > standard processorarkitektur > x64.

    ![Azure Data Lake U-SQL-SDK konfigurera x64 testmiljö](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-configure-test-x64.png)

- Se till att toocopy alla beroende filer under NugetPackage\build\runtime\ tooproject arbetskatalogen som vanligtvis är under ProjectFolder\bin\x64\Debug.

### <a name="step-2-create-u-sql-script-test-case"></a>Steg 2: Skapa testfall för U-SQL-skript

Nedan visas hello exempelkod för U-SQL-skript. För att testa, behöver du tooprepare skript indata och utdata som förväntas-filer.

    using System;
    using Microsoft.VisualStudio.TestTools.UnitTesting;
    using System.IO;
    using System.Text;
    using System.Security.Cryptography;
    using Microsoft.Analytics.LocalRun;

    namespace UnitTestProject1
    {
        [TestClass]
        public class USQLUnitTest
        {
            [TestMethod]
            public void TestUSQLScript()
            {
                //Specify hello local run message output path
                StreamWriter MessageOutput = new StreamWriter("../../../log.txt");

                LocalRunHelper localrun = new LocalRunHelper(MessageOutput);

                //Configure hello DateRoot path, Script Path and CPPSDK path
                localrun.DataRoot = "../../../";
                localrun.ScriptPath = "../../../Script/Script.usql";
                localrun.CppSdkDir = "../../../CppSDK";

                //Run U-SQL script
                localrun.DoRun();

                //Script output 
                string Result = Path.Combine(localrun.DataRoot, "Output/result.csv");

                //Expected script output
                string ExpectedResult = "../../../ExpectedOutput/result.csv";

                Test.Helpers.FileAssert.AreEqual(Result, ExpectedResult);

                //Don't forget tooclose MessageOutput tooget logs into file
                MessageOutput.Close();
            }
        }
    }

    namespace Test.Helpers
    {
        public static class FileAssert
        {
            static string GetFileHash(string filename)
            {
                Assert.IsTrue(File.Exists(filename));

                using (var hash = new SHA1Managed())
                {
                    var clearBytes = File.ReadAllBytes(filename);
                    var hashedBytes = hash.ComputeHash(clearBytes);
                    return ConvertBytesToHex(hashedBytes);
                }
            }

            static string ConvertBytesToHex(byte[] bytes)
            {
                var sb = new StringBuilder();

                for (var i = 0; i < bytes.Length; i++)
                {
                    sb.Append(bytes[i].ToString("x"));
                }
                return sb.ToString();
            }

            public static void AreEqual(string filename1, string filename2)
            {
                string hash1 = GetFileHash(filename1);
                string hash2 = GetFileHash(filename2);

                Assert.AreEqual(hash1, hash2);
            }
        }
    }


### <a name="programming-interfaces-in-localrunhelperexe"></a>Programmeringsgränssnitt i LocalRunHelper.exe

LocalRunHelper.exe innehåller hello programmeringsgränssnitt för U-SQL lokalt kompilera kör, etc. hello gränssnitt visas nedan.

**Konstruktorn**

offentliga LocalRunHelper ([System.IO.TextWriter messageOutput = null])

|Parameter|Typ|Beskrivning|
|---------|----|-----------|
|messageOutput|System.IO.TextWriter|Ange toonull toouse konsolen för utgående meddelanden.|

**Egenskaper**

|Egenskap|Typ|Beskrivning|
|--------|----|-----------|
|AlgebraPath|Sträng|hello sökväg tooalgebra fil (algebra filen är en av hello Kompileringsresultatet)|
|CodeBehindReferences|Sträng|Hello skript finns ytterligare kod bakom referenser anger hello sökvägar avgränsade med ';'|
|CppSdkDir|Sträng|CppSDK directory|
|CurrentDir|Sträng|Aktuell katalog|
|DataRoot|Sträng|Rotsökvägen för data|
|DebuggerMailPath|Sträng|hello sökvägen toodebugger mailslot|
|GenerateUdoRedirect|bool|Om vi vill toogenerate sammansättning lästes in omdirigering åsidosätta config|
|HasCodeBehind|bool|Om hello skript har bakomliggande kod|
|InputDir|Sträng|Katalogen för indata|
|MessagePath|Sträng|Meddelandet dump sökväg|
|OutputDir|Sträng|Katalogen för utdata|
|Parallellitet|int|Parallellitet toorun hello algebra|
|ParentPid|int|Process-ID för hello överordnade på vilka hello tjänsten övervakar tooexit, ange too0 eller negativa tooignore|
|ResultPath|Sträng|Resultatet dump sökväg|
|RuntimeDir|Sträng|Runtime directory|
|ScriptPath|Sträng|Där toofind hello skript|
|Lite|bool|Ytlig kompilera eller inte|
|TempDir|Sträng|Temp-katalogen|
|UseDataBase|Sträng|Ange hello databasen toouse för koden bakom tillfällig sammansättning registrering, master som standard|
|WorkDir|Sträng|Prioriterade arbetskatalogen|


**Metoden**

|Metod|Beskrivning|Returnera|Parameter|
|------|-----------|------|---------|
|offentliga bool DoCompile()|Kompilera hello U-SQL-skript|SANT lyckades| |
|offentliga bool DoExec()|Köra hello kompileras resultat|SANT lyckades| |
|offentliga bool DoRun()|Kör hello U-SQL-skript (kompilera + Execute)|SANT lyckades| |
|offentliga bool IsValidRuntimeDir (sträng sökväg)|Kontrollera om hello angivna sökvägen är giltig runtime sökväg|Sant för giltigt|hello sökvägen till runtime-katalog|


## <a name="faq-about-common-issue"></a>Vanliga frågor och svar om vanliga problem

### <a name="error-1"></a>Fel 1:
E_CSC_SYSTEM_INTERNAL: Internt fel! Det gick inte att läsa in filen eller sammansättningen 'ScopeEngineManaged.dll' eller en av dess beroenden. Det gick inte att hitta hello angiven modul.

Kontrollera hello följande:

- Kontrollera att du har x64 miljö. hello build målplattform och hello testmiljö bör vara x64 finns för**steg 1: skapa C# enhet Testa projektet och konfiguration** ovan.
- Kontrollera att du har kopierat alla beroende filer under NugetPackage\build\runtime\ tooproject arbetskatalogen.


## <a name="next-steps"></a>Nästa steg

* toolearn U-SQL finns [Kom igång med Azure Data Lake Analytics U-SQL-språket](data-lake-analytics-u-sql-get-started.md).
* toolog diagnostikinformation, se [åtkomst till diagnostik loggar för Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md).
* toosee en mer komplex fråga, se [analysera webbplatsloggar med hjälp av Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
* tooview jobbinformation finns [Använd jobbet webbläsare och visa jobb för Azure Data Lake Analytics-jobb](data-lake-analytics-data-lake-tools-view-jobs.md).
* toouse hello vertex vy, se [Använd hello Vertex vy i Data Lake-verktyg för Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).
