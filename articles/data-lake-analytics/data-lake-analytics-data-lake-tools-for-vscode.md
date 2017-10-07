---
title: "Azure Data Lake-verktyg: Använd Azure Data Lake-verktyg för Visual Studio Code | Microsoft Docs"
description: "Lär dig hur toouse Azure Data Lake-verktyg för Visual Studio Code toocreate, testa och köra U-SQL-skript. "
Keywords: "VScode, Azure Data Lake-verktyg, lokal körning lokala felsökning lokala Debug Förhandsgranska lagringsfil överför toostorage sökväg"
services: data-lake-analytics
documentationcenter: 
author: jejiang
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/14/2017
ms.author: jejiang
ms.openlocfilehash: 77771c5d5dae3bfce4ad2df240ea6c6ef848f288
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-data-lake-tools-for-visual-studio-code"></a>Använda Azure Data Lake-verktyg för Visual Studio Code

Lär dig hur toouse Azure Data Lake-verktyg för Visual Studio-koden (VS) toocreate, testa och köra U-SQL-skript. hello information finns också i följande video hello:

<a href="https://www.youtube.com/watch?v=J_gWuyFnaGA&feature=youtu.be"><img src="./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-video.png"></a>

## <a name="prerequisites"></a>Krav

Data Lake-verktyg kan installeras på hello-plattformar som stöds av VS-kod. exempel på plattformar hello som stöds är Windows, Linux och MacOS. hello olika plattformar har hello följande krav:

- Windows

    - [Visual Studio Code]( https://www.visualstudio.com/products/code-vs.aspx).
    - [Java SE Runtime Environment version 8 update 77 eller senare](https://java.com/download/manual.jsp). Lägg till hello java.exe sökväg toohello miljö variabeln systemsökvägen. Konfigurationsanvisningar, se [hur jag ange eller ändra hello sökvägsvariabeln system?]( https://www.java.com/download/help/path.xml). hello sökvägen är liknande tooC:\Program Files\Java\jdk1.8.0_77\jre\bin.
    - [.NET core SDK 1.0.3 eller .NET Core 1.1 runtime](https://www.microsoft.com/net/download).
    
- Linux (Vi rekommenderar Ubuntu 14.04 LTS)

    - [Visual Studio Code]( https://www.visualstudio.com/products/code-vs.aspx). tooinstall Hej kod, ange hello följande kommando:

              sudo dpkg -i code_<version_number>_amd64.deb

    - [Monoljud 4.2.x](http://www.mono-project.com/docs/getting-started/install/linux/). 

        - tooupdate hello deb paketet källa, ange hello följande kommandon:

                sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF
                echo "deb http://download.mono-project.com/repo/debian wheezy/snapshots 4.2.4.4/main" | sudo tee /etc/apt/sources.list.d/mono-xamarin.list
                sudo apt-get update

        - tooinstall Mono, ange hello följande kommando:

                sudo apt-get install mono-complete

            > [!NOTE] 
            > Mono 4.6 stöds inte. Avinstallera 4.6 helt och hållet innan du installerar 4.2.x.  

        - [Java SE Runtime Environment version 8 update 77 eller senare](https://java.com/download/manual.jsp). Anvisningar för installation finns hello [Linux 64-bitars Installationsinstruktioner för Java]( https://java.com/en/download/help/linux_x64_install.xml) sidan.
        - [.NET core SDK 1.0.3 eller .NET Core 1.1 runtime](https://www.microsoft.com/net/download).
- MacOS

    - [Visual Studio Code]( https://www.visualstudio.com/products/code-vs.aspx).
    - [Mono 4.2.4](http://download.mono-project.com/archive/4.2.4/macos-10-x86/). 
    - [Java SE Runtime Environment version 8 update 77 eller senare](https://java.com/download/manual.jsp). Anvisningar för installation finns hello [Linux 64-bitars Installationsinstruktioner för Java](https://java.com/en/download/help/mac_install.xml) sidan.
    - [.NET core SDK 1.0.3 eller .NET Core 1.1 runtime](https://www.microsoft.com/net/download).

## <a name="install-data-lake-tools"></a>Installera Data Lake-verktyg

När du har installerat hello förutsättningar kan du installera Data Lake-verktyg för VS-kod.

**tooinstall Data Lake-verktyg**

1. Öppna Visual Studio-koden.
2. Välj Ctrl + P och ange sedan hello följande kommando:
```
ext install usql-vscode-ext
```
Du kan se en lista över tillägg för Visual Studio-koden. En av dem är **Azure Data Lake-verktyg**.

3. Välj **installera** nästa för**Azure Data Lake-verktyg**. Efter några sekunder hello **installera** knappen ändras för**ladda**.
4. Välj **ladda** tooactivate hello tillägg.
5. Välj **OK** tooconfirm. Du kan se Azure Data Lake-verktyg i hello **tillägg** fönstret.
    ![Data Lake-verktyg för Visual Studio Code-tillägg](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extensions.png)

## <a name="activate-azure-data-lake-tools"></a>Aktivera Azure Data Lake-verktyg
Skapa en ny .usql fil eller öppna ett befintligt .usql tooactivate hello filtillägg. 

## <a name="connect-tooazure"></a>Ansluta tooAzure

Innan du kan kompilera och köra U-SQL-skript i Data Lake Analytics, måste du ansluta tooyour Azure-konto.

**tooconnect tooAzure**

1.  Välj Ctrl + Skift + P tooopen hello kommandot palett. 
2.  Ange **ADL: inloggning**. hello inloggningsinformation visas i hello **utdata** fönstret.

    ![Data Lake-verktyg för Visual Studio Code kommandot paletten](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login.png)
    ![Data Lake-verktyg för Visual Studio Code enhetsinformation för inloggning](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-login-info.png)
3. Välj Ctrl + klicka på hello inloggnings-URL: https://aka.ms/devicelogin tooopen hello inloggningen webbsidan. Ange hello kod **G567LX42V** i textrutan för hello och välj sedan **Fortsätt**.

   ![Data Lake-verktyg för Visual Studio Code inloggningen klistra in koden](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login-paste-code.png )   
4.  Följ hello instruktioner toosign i från hello webbsidan. När du är ansluten visas namnet på ditt Azure hello statusfältet i hello nedre vänstra hörnet av hello **VS kod** fönster. 

    > [!NOTE] 
    > Om ditt konto har två faktorer är aktiverat, rekommenderar vi att du använder phone autentisering i stället för en PIN-kod.

toosign, ange hello kommando **ADL: Logga ut**.

## <a name="list-your-data-lake-analytics-accounts"></a>Visa en lista över dina Data Lake Analytics-konton

tootest hello anslutning, hämta en lista över dina Data Lake Analytics-konton.

**toolist hello Data Lake Analytics-konton under din Azure-prenumeration**

1. Välj Ctrl + Skift + P tooopen hello kommandot palett.
2. Ange **ADL: visa en lista över konton**. hello konton visas i hello **utdata** fönstret.

## <a name="open-hello-sample-script"></a>Öppna hello exempelskript
Öppna hello kommandot palett (Ctrl + Skift + P) och ange **ADL: öppna exempelskript**. Då öppnas en annan instans av det här exemplet. Du kan också redigera, konfigurera och skicka skript på den här instansen.

## <a name="work-with-u-sql"></a>Arbeta med U-SQL

Du måste öppna en U-SQL-fil eller en mapp toowork med U-SQL.

**tooopen en mapp för U-SQL-projekt**

1. Välj Visual Studio Code hello **filen** -menyn och välj sedan **öppna mappen**.
2. Ange en mapp och välj sedan **Välj mappen**.
3. Välj hello **filen** -menyn och välj sedan **ny**. Toohello projektet läggs en namnlös-1-filen.
4. Ange följande kod i filen hello Namnlös 1 hello:

        @departments  = 
            SELECT * FROM 
                (VALUES
                    (31,    "Sales"),
                    (33,    "Engineering"), 
                    (34,    "Clerical"),
                    (35,    "Marketing")
                ) AS 
                      D( DepID, DepName );
         
        OUTPUT @departments
            TO “/Output/departments.csv”

    hello skriptet skapar en departments.csv-fil med vissa data som ingår i hello/Output-mappen.

5. Spara hello som **myUSQL.usql** öppnats i ett hello mapp. En konfigurationsfil för adltools_settings.json läggs toohello projekt.
4. Öppna och konfigurera adltools_settings.json med hello följande egenskaper:

    - Konto: Ett Data Lake Analytics-konto under din Azure-prenumeration.
    - Databas: En databas med ditt konto. hello standardvärdet är **master**.
    - Schema: Ett schema under din databas. hello standardvärdet är **dbo**.
    - Valfria inställningar:
        - Prioritet: hello prioritetsintervall är 1 too1000 med 1 som hello högst prioritet. hello standardvärdet är **1000**.
        - Parallellitet: hello parallellitet intervall är 1 too150. hello standardvärdet är hello maximala parallellitet tillåts i ditt Azure Data Lake Analytics-konto. 
        
        > [!NOTE] 
        > Om hello inställningarna är ogiltig, används hello standardvärdena.

    ![Data Lake-verktyg för Visual Studio Code konfigurationsfil](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-configuration-file.png)

    En beräkning Data Lake Analytics-kontot är behövs toocompile och köra U-SQL-jobb. Du måste konfigurera hello datorkonto innan du kompilera och köra U-SQL-jobb.
    
När hello konfiguration sparas visas hello kontot, databas och schema information på hello statusfältet hello nedre vänstra hörn för hello motsvarande .usql-filen. 
 
 
Jämfört med tooopening en fil när du öppnar en mapp som du kan:

- Använda en fil med bakomliggande kod. Bakomliggande kod stöds inte i läget för hello-fil.
- Använd en konfigurationsfil. När du öppnar en mapp dela hello skript i hello arbetsmapp en enda konfigurationsfil.


hello U-SQL-skript kompilerar via fjärranslutning via hello Data Lake Analytics-tjänsten. När du skickar hello **Kompilera** kommandot hello U-SQL-skript skickas tooyour Data Lake Analytics-konto. Visual Studio-koden får senare hello Kompileringsresultatet. På grund av toohello remote kompilering kräver Visual Studio Code du lista hello information tooconnect tooyour Data Lake Analytics-kontot i hello konfigurationsfil.

**toocompile U-SQL-skript**

1. Välj Ctrl + Skift + P tooopen hello kommandot palett. 
2. Ange **ADL: kompilera skriptet**. hello kompilera visas resultaten i hello **utdata** fönster. Du kan också högerklicka på en skriptfil och sedan välja **ADL: kompilera skriptet** toocompile U-SQL-jobbet. hello Kompileringsresultatet visas i hello **utdata** fönstret.
 

**toosubmit U-SQL-skript**

1. Välj Ctrl + Skift + P tooopen hello kommandot palett. 
2. Ange **ADL: skicka jobbet**.  Du kan också högerklicka på en skriptfil och sedan välja **ADL: skicka jobbet**. 

När du skickar ett U-SQL-jobb hello skicka loggarna visas i hello **utdata** fönster i VS-kod. Om hello skicka lyckas visas hello jobbet URL också. Du kan öppna hello jobbet URL i en web webbläsare tootrack hello realtid jobbstatus.

Ange tooenable hello utdata från hello jobbinformation **jobInformationOutputPath** i hello **vs Platskod för hello u-sql_settings.json** fil.
 
## <a name="use-a-code-behind-file"></a>Använda en fil med bakomliggande kod

En fil med bakomliggande kod är ett C#-fil som är associerad med ett enda U-SQL-skript. Du kan definiera ett skript som är dedikerad tooUDO, UDA, UDT och UDF i hello bakomliggande kod-filen. hello kan UDO, UDA, UDT och UDF användas direkt i hello skript utan att först registrera hello sammansättningen. hello bakomliggande kod filen placeras i hello samma mapp som dess peering U-SQL-skriptfilen. Om hello skriptet heter xxx.usql, heter hello bakomliggande kod som xxx.usql.cs. Om du manuellt ta bort hello bakomliggande kod filen är hello bakomliggande kod funktionen inaktiverad för dess associerade U-SQL-skript. Mer information om hur du skriver kunden koden för U-SQL-skript finns [skrivning och använder anpassad kod i U-SQL: användardefinierade funktioner]( https://blogs.msdn.microsoft.com/visualstudio/2015/10/28/writing-and-using-custom-code-in-u-sql-user-defined-functions/).

toosupport bakomliggande kod, måste du öppna en arbetsmapp. 

**toogenerate en bakomliggande kod-fil**

1. Öppna en källfil. 
2. Välj Ctrl + Skift + P tooopen hello kommandot palett.
3. Ange **ADL: Generera kod bakom**. Bakomliggande kod skapas en fil i hello samma mapp. 

Du kan också högerklicka på en skriptfil och sedan välja **ADL: Generera kod bakom**. 

toocompile och skicka ett U-SQL-skript med en fil med bakomliggande kod är hello samma som med hello fristående U-SQL-skriptfil.

hello följande två skärmdumpar visar en fil med bakomliggande kod och dess associerade U-SQL-skriptfilen:
 
![Data Lake-verktyg för Visual Studio Code bakomliggande kod](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-code-behind.png)

![Data Lake-verktyg för Visual Studio Code bakomliggande kod skriptfilen](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-code-behind-call.png) 

## <a name="use-assemblies"></a>Använd sammansättningar

Information om hur du utvecklar sammansättningar finns [utveckla U-SQL-sammansättningar för Azure Data Lake Analytics-jobb](data-lake-analytics-u-sql-develop-assemblies.md).

Du kan använda Data Lake-verktyg tooregister anpassad kodsammansättningar i hello Data Lake Analytics-katalogen.

**tooregister en sammansättning**

Du kan registrera hello sammansättningen via hello **ADL: registrera sammansättningen** eller **ADL: registrera sammansättningen via konfiguration** kommandon.

**tooregister via hello ADL: registrera sammansättningen kommando**
1.  Välj Ctrl + Skift + P tooopen hello kommandot palett.
2.  Ange **ADL: registrera sammansättningen**. 
3.  Ange sökväg för hello lokala sammansättning. 
4.  Välj ett Data Lake Analytics-konto.
5.  Välj en databas.

Resultat: hello portalen öppnas i en webbläsare och visar hello sammansättningen registreringsprocessen.  

En annan praktiskt sätt tootrigger hello **ADL: registrera sammansättningen** kommandot är tooright Klicka hello DLL-filen i Utforskaren. 

**tooregister men hello ADL: registrera sammansättningen med hjälp av kommandot konfiguration**
1.  Välj Ctrl + Skift + P tooopen hello kommandot palett.
2.  Ange **ADL: registrera sammansättningen via konfiguration**. 
3.  Ange sökväg för hello lokala sammansättning. 
4.  hello JSON-fil visas. Granska och redigera hello paketberoenden och Resursparametrar, om det behövs. Instruktioner visas i hello **utdata** fönster. tooproceed toohello sammansättningen registrering, spara (Ctrl + S) hello JSON-fil.

![Data Lake-verktyg för Visual Studio Code bakomliggande kod](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-register-assembly-advance.png)
>[!NOTE]
>- Beroenden för sammansättningen: Azure Data Lake-verktyg autodetects om hello DLL har några beroenden. hello beroenden visas i hello JSON-fil när de identifieras. 
>- Resurser: Du kan överföra dina DLL-resurser (exempelvis txt, .png och CSV) som en del av hello regasm.exe för registrering. 

Ett annat sätt tootrigger hello **ADL: registrera sammansättningen via konfiguration** kommandot är tooright Klicka hello DLL-filen i Utforskaren. 

hello följande U-SQL-kod visar hur toocall en sammansättning. I exemplet hello hello sammansättningsnamnet är *testa*.

```
REFERENCE ASSEMBLY [test];

@a = 
    EXTRACT 
        Iid int,
    Starts DateTime,
    Region string,
    Query string,
    DwellTime int,
    Results string,
    ClickedUrls string 
    FROM @"Sample/SearchLog.txt" 
    USING Extractors.Tsv();

@d =
    SELECT DISTINCT Region 
    FROM @a;

@d1 = 
    PROCESS @d
    PRODUCE 
        Region string,
    Mkt string
    USING new USQLApplication_codebehind.MyProcessor();

OUTPUT @d1 
    too@"Sample/SearchLogtest.txt" 
    USING Outputters.Tsv();
```


## <a name="access-hello-data-lake-analytics-catalog"></a>Åtkomst hello Data Lake Analytics-katalog

När du har anslutit tooAzure kan använda du hello följande steg tooaccess hello U-SQL-katalogen.

**tooaccess hello Azure Data Lake Analytics-metadata**

1.  Välj Ctrl + Skift + P och ange **ADL: lista tabeller**.
2.  Välj en av hello Data Lake Analytics-konton.
3.  Välj en av hello Data Lake Analytics-databaser.
4.  Välj en av hello scheman. Du kan se hello listan över tabeller.

## <a name="view-data-lake-analytics-jobs"></a>Visa Data Lake Analytics-jobb

**tooview Data Lake Analytics-jobb**
1.  Öppna hello kommandot palett (Ctrl + Skift + P) och välj **ADL: Visa jobb**. 
2.  Välj en Data Lake Analytics eller lokalt konto. 
3.  Vänta tills hello jobb lista för hello konto tooappear.
4.  Välj ett jobb jobblistan, Data Lake-verktyg öppnas hello jobbinformation i hello Azure-portalen och visar hello Jobbinformations fil i VS-kod.

![Data Lake-verktyg för Visual Studio Code IntelliSense objekttyper](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-show-job.png)

## <a name="azure-data-lake-storage-integration"></a>Azure Data Lake Lagringsintegrering

Du kan använda lagring av Azure Data Lake-relaterade kommandon:
 - Bläddra igenom hello Azure Data Lake lagringsresurser. 
 - Förhandsgranska hello Azure Data Lake Storage-fil.  
 - Överför hello filen direkt tooAzure lagring av Data Lake i VS-kod. 

### <a name="list-hello-storage-path"></a>Sökvägen till listan hello lagring 
Du kan ange sökvägen till hello lagring via hello kommandot paletten eller högerklicka på.

**sökvägen till toolist hello lagring via hello kommandot paletten**

1.  Öppna hello kommandot palett (Ctrl + Skift + P) och ange **ADL: listan Lagringssökväg**.

    ![Data Lake-verktyg för Visual Studio Code lista lagringssökväg](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-storage.png)

2.  Välj din bästa sättet för att lista hello sökvägen för lagring. Den här övergången använder **ange en sökväg** som exempel.

    ![Data Lake-verktyg för Visual Studio Code enkelriktade toolist hello lagringssökväg](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account-selectoneway.png)

    > [!NOTE]
    >- VS-koden behålls hello senast besökt sökväg i varje Data Lake Analytics-konto. Till exempel: ss tt /.
    >- Webbläsaren från rotsökvägen: hello listan rotsökvägen från valda Data Lake Analytics-kontot eller en lokal sökväg.
    >- Ange en sökväg: visa en lista med en angiven sökväg från ditt valda Data Lake Analytics-konto eller en lokal sökväg.
    
3. Välj ett konto från hello lokal sökväg eller ett Data Lake Analytics-konto.

    ![Data Lake-verktyg för Visual Studio Code Välj Mer](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account.png)

4. Välj **mer** toolist mer Data Lake Analytics-konton och välj sedan ett Data Lake Analytics-konto.

    ![Data Lake-verktyg för Visual Studio Code välja ett konto](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-select-adla-account.png)

5.  Ange en sökväg till Azure storage. Till exempel/utdata.

    ![Data Lake-verktyg för Visual Studio Code ange sökvägen till lagring](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-input-path.png)

6.  Resultat: hello kommandot paletten visar hello sökvägsinformation baserat på dina uppgifter.

    ![Data Lake-verktyg för Visual Studio Code lista lagring sökväg resultat](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-path.png)

Det enklaste sättet toolist hello relativ sökväg är via hello högerklickar du på snabbmenyn.

**sökvägen till toolist hello lagring via högerklickar du på**

1.  Högerklicka på hello sökväg sträng tooselect **lista Lagringssökväg**.

       ![Data Lake-verktyg för Visual Studio Code högerklickar du på snabbmenyn](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-right-click-path.png)

2. hello valda relativa sökvägen visas i hello kommandot palett.

   ![Data Lake-verktyg för Visual Studio Code valda relativ sökväg](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-relative-path.png)

3.  Välj ett konto från hello lokal sökväg eller ett Data Lake Analytics-konto.

       ![Data Lake-verktyg för Visual Studio Code välja ett konto](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account.png)

4.  Resultat: hello kommandot paletten visar hello mappar och filer för hello aktuella sökvägen.

       ![Data Lake-verktyg för Visual Studio Code lista från hello aktuella sökvägen](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-current.png)

### <a name="preview-hello-storage-file"></a>Lagringsfilen för förhandsgranskning hello
Du kan förhandsgranska hello lagringsfilen via hello kommandot paletten eller högerklicka på.

**toopreview hello lagringsfilen via hello kommandot paletten**

1.  Öppna hello kommandot palett (Ctrl + Skift + P) och ange **ADL: Preview lagringsfilen**.

       ![Data Lake-verktyg för Visual Studio Code Förhandsgranska lagringsfilen](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-preview.png)

2.  Välj ett konto från hello lokal sökväg eller ett Data Lake Analytics-konto.

       ![Data Lake-verktyg för Visual Studio Code listan konto](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account.png)

3.  Välj **mer** toolist mer Data Lake Analytics-konton och välj sedan ett Data Lake Analytics-konto.

       ![Data Lake-verktyg för Visual Studio Code välja ett konto](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-select-adla-account.png)

4.  Ange ett Azure storage-sökvägen eller en fil. Till exempel /output/SearchLog.txt.

       ![Data Lake-verktyg för Visual Studio Code ange lagringssökväg och filnamn](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-input-preview-file.png)

5.  Resultat: hello kommandot paletten visar hello sökvägsinformation baserat på dina uppgifter.

       ![Data Lake-verktyg för Visual Studio Code förhandsgranska filen resultat](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-preview-results.png)

**sökvägen till toolist hello lagring via högerklickar du på**

1.  toopreview en fil, högerklicka på hello filsökväg.

   ![Data Lake-verktyg för Visual Studio Code högerklickar du på snabbmenyn](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-right-click-preview.png) 

2.  Välj ett konto från hello lokal sökväg eller ett Data Lake Analytics-konto.

       ![Data Lake-verktyg för Visual Studio Code välja ett konto](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account.png)

3.  Resultat: VS-kod visar hello förhandsvisning av resultat av hello-fil.

       ![Data Lake-verktyg för Visual Studio Code förhandsgranska filen resultat](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-preview-results.png)

### <a name="upload-a-file"></a>Överför en fil 

Du kan ladda upp filer genom att ange hello kommandon **ADL: ladda upp filen** eller **ADL: ladda upp filen via konfiguration**.

**tooupload filer men hello ADL: ladda upp fil (kommando)**
1. Välj Ctrl + Skift + P tooopen hello kommandot paletten eller högerklicka på hello Skriptredigeraren och ange sedan **överför filen**.
2.  tooupload hello fil, ange en lokal sökväg.

    ![Data Lake-verktyg för Visual Studio Code ange lokal sökväg](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-input-local-path.png)

3. Välj en av hello sätt att sökvägen till lista hello lagring. Den här övergången använder **ange en sökväg** som exempel.

    ![Data Lake-verktyg för Visual Studio Code lista lagringssökväg](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account-selectoneway.png)
    >[!NOTE]
    >- VS-koden behålls hello senast besökt sökväg i varje Data Lake Analytics-konto. Till exempel: ss tt /.
    >- Webbläsaren från rotsökvägen: hello listan rotsökvägen från valda Data Lake Analytics-kontot eller en lokal sökväg.
    >- Ange en sökväg: visa en lista med en angiven sökväg från ditt valda Data Lake Analytics-konto eller en lokal sökväg.

4. Välj ett konto från hello lokal sökväg eller ett Data Lake Analytics-konto.

    ![Data Lake-verktyg för Visual Studio Code Högerklicka på lagring](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account.png)

5. Ange en sökväg till Azure storage. Till exempel: / utdata.

       ![Data Lake Tools for Visual Studio Code enter storage path](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-input-preview-file.png)

6. Hitta din Azure storage-sökväg. Välj **Välj aktuella mappen**.

    ![Välj en mapp för data Lake-verktyg för Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-choose-current-folder.png)

7.  Resultat: hello **utdata** hello filen Överföringsstatusen visas i fönstret.

       ![Data Lake-verktyg för Visual Studio Code överför status](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-upload-status.png)    

**tooupload filer men hello ADL: ladda upp filen via Konfigurationskommando**
1.  Välj Ctrl + Skift + P tooopen hello kommandot paletten eller högerklicka på hello Skriptredigeraren och ange sedan **överför filen via konfiguration**.
2.  VS-kod visar en JSON-fil. Du kan ange sökvägar och överför flera filer på hello samtidigt. Instruktioner visas i hello **utdata** fönster. tooproceed tooupload hello fil spara (Ctrl + S) hello JSON-fil.

       ![Data Lake-verktyg för Visual Studio Code filsökväg](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-upload-file.png)

3.  Resultat: hello **utdata** hello filen Överföringsstatusen visas i fönstret.

       ![Data Lake-verktyg för Visual Studio Code överför status](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-upload-status.png)     

Ett annat sätt tooupload toostorage en fil är via hello högerklickar du på menyn på hello filens fullständiga sökväg eller hello relativa sökvägen i hello Skriptredigeraren. Ange hello lokal filsökväg och välj hello-konto. Hej **utdata** hello Överföringsstatusen visas i fönstret. 

### <a name="open-azure-storage-explorer"></a>Öppna Azure Lagringsutforskaren
Du kan öppna **Azure Lagringsutforskaren** genom att ange hello kommando **ADL: öppna Lagringsutforskaren för Web Azure** eller genom att välja den hello genom att högerklicka på snabbmenyn.

**tooopen Azure Lagringsutforskaren**

1. Välj Ctrl + Skift + P tooopen hello kommandot palett.
2. Ange **öppna Web Azure Lagringsutforskaren** eller högerklicka på en relativ eller hello fullständiga sökväg i hello Skriptredigeraren och välj sedan **öppna Web Azure Lagringsutforskaren**.
3. Välj ett Data Lake Analytics-konto.

Data Lake-verktyg öppnar hello Azure storage-sökvägen i hello Azure-portalen. Du hittar filen för hello-sökväg och förhandsgranska hello från hello webbserver.

### <a name="local-run-and-local-debug-for-windows-users"></a>Lokal körning och lokala debug för Windows användare
Lokal U-SQL kör testar dina lokala data och verifierar ditt skript lokalt, innan koden är publicerade tooData Lake Analytics. hello lokala felsökning kan du toocomplete hello följande uppgifter innan din kod har skickats tooData Lake Analytics: 
- Felsöka ditt C# bakomliggande kod. 
- Gå igenom hello kod. 
- Validera skriptet lokalt.

Anvisningar för lokal körning och lokala debug finns [lokal U-SQL kör och lokala debug med Visual Studio Code](data-lake-tools-for-vscode-local-run-and-debug.md).

## <a name="additional-features"></a>Ytterligare funktioner

Data Lake-verktyg för VS kod stöder hello följande funktioner:

-   IntelliSense automatisk komplettering: förslag visas i popup-fönster runt objekt, till exempel nyckelord, metoder och variabler. Olika ikoner representerar olika typer av hello objekt:

    - Scala-datatyp
    - Komplex datatyp
    - Inbyggda UDT-typer
    - .NET-samling och klasser
    - C#-uttryck
    - Inbyggda C# UDF: er och UDO UDAAGs 
    - U-SQL-funktioner
    - U-SQL-fönsterfunktion
 
    ![Data Lake-verktyg för Visual Studio Code IntelliSense objekttyper](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-objects.png)
 
-   IntelliSense automatisk komplettering på Data Lake Analytics metadata: Data Lake-verktyg hämtar hello Data Lake Analytics metadatainformation lokalt. hello IntelliSense funktionen fyller automatiskt objekt, inklusive hello-databasen, schemat, tabell, visa, tabellvärdesfunktion, procedurer och C#-sammansättningar, hello Data Lake Analytics metadata från.
 
    ![Data Lake-verktyg för Visual Studio Code IntelliSense metadata](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-metastore.png)

-   IntelliSense fel markör: Data Lake-verktyg understryker hello redigering fel för U-SQL och C#. 
-   Visar syntax: Data Lake-verktyg som använder olika färger toodifferentiate objekt, till exempel variabler, nyckelord, datatyp och funktioner. 

    ![Data Lake-verktyg för Visual Studio Code syntax visar](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-syntax-highlights.png)

## <a name="next-steps"></a>Nästa steg

- Lokal U-SQL-körning och lokala debug med Visual Studio Code finns [lokal U-SQL kör och lokala debug med Visual Studio Code](data-lake-tools-for-vscode-local-run-and-debug.md).
- Kom igång-information på Data Lake Analytics finns [Självstudier: Kom igång med Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md).
- Information om Data Lake-verktyg för Visual Studio finns [Självstudier: utveckla U-SQL-skript med hjälp av Data Lake-verktyg för Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- Information om hur du utvecklar sammansättningar finns [utveckla U-SQL-sammansättningar för Azure Data Lake Analytics-jobb](data-lake-analytics-u-sql-develop-assemblies.md).



