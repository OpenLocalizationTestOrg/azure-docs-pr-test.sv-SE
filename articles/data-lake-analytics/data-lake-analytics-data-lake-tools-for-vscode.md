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
# <a name="use-azure-data-lake-tools-for-visual-studio-code"></a><span data-ttu-id="ad8cf-104">Använda Azure Data Lake-verktyg för Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ad8cf-104">Use Azure Data Lake Tools for Visual Studio Code</span></span>

<span data-ttu-id="ad8cf-105">Lär dig hur toouse Azure Data Lake-verktyg för Visual Studio-koden (VS) toocreate, testa och köra U-SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-105">Learn how toouse Azure Data Lake Tools for Visual Studio Code (VS Code) toocreate, test, and run U-SQL scripts.</span></span> <span data-ttu-id="ad8cf-106">hello information finns också i följande video hello:</span><span class="sxs-lookup"><span data-stu-id="ad8cf-106">hello information is also covered in hello following video:</span></span>

<a href="https://www.youtube.com/watch?v=J_gWuyFnaGA&feature=youtu.be"><img src="./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-video.png"></a>

## <a name="prerequisites"></a><span data-ttu-id="ad8cf-107">Krav</span><span class="sxs-lookup"><span data-stu-id="ad8cf-107">Prerequisites</span></span>

<span data-ttu-id="ad8cf-108">Data Lake-verktyg kan installeras på hello-plattformar som stöds av VS-kod.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-108">Data Lake Tools can be installed on hello platforms supported by VS Code.</span></span> <span data-ttu-id="ad8cf-109">exempel på plattformar hello som stöds är Windows, Linux och MacOS.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-109">hello supported platforms include Windows, Linux, and MacOS.</span></span> <span data-ttu-id="ad8cf-110">hello olika plattformar har hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="ad8cf-110">hello different platforms have hello following prerequisites:</span></span>

- <span data-ttu-id="ad8cf-111">Windows</span><span class="sxs-lookup"><span data-stu-id="ad8cf-111">Windows</span></span>

    - <span data-ttu-id="ad8cf-112">[Visual Studio Code]( https://www.visualstudio.com/products/code-vs.aspx).</span><span class="sxs-lookup"><span data-stu-id="ad8cf-112">[Visual Studio Code]( https://www.visualstudio.com/products/code-vs.aspx).</span></span>
    - <span data-ttu-id="ad8cf-113">[Java SE Runtime Environment version 8 update 77 eller senare](https://java.com/download/manual.jsp).</span><span class="sxs-lookup"><span data-stu-id="ad8cf-113">[Java SE Runtime Environment version 8 update 77 or later](https://java.com/download/manual.jsp).</span></span> <span data-ttu-id="ad8cf-114">Lägg till hello java.exe sökväg toohello miljö variabeln systemsökvägen.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-114">Add hello java.exe path toohello system environment variable path.</span></span> <span data-ttu-id="ad8cf-115">Konfigurationsanvisningar, se [hur jag ange eller ändra hello sökvägsvariabeln system?]( https://www.java.com/download/help/path.xml).</span><span class="sxs-lookup"><span data-stu-id="ad8cf-115">For configuration instructions, see [How do I set or change hello Path system variable?]( https://www.java.com/download/help/path.xml).</span></span> <span data-ttu-id="ad8cf-116">hello sökvägen är liknande tooC:\Program Files\Java\jdk1.8.0_77\jre\bin.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-116">hello path is similar tooC:\Program Files\Java\jdk1.8.0_77\jre\bin.</span></span>
    - <span data-ttu-id="ad8cf-117">[.NET core SDK 1.0.3 eller .NET Core 1.1 runtime](https://www.microsoft.com/net/download).</span><span class="sxs-lookup"><span data-stu-id="ad8cf-117">[.NET Core SDK 1.0.3 or .NET Core 1.1 runtime](https://www.microsoft.com/net/download).</span></span>
    
- <span data-ttu-id="ad8cf-118">Linux (Vi rekommenderar Ubuntu 14.04 LTS)</span><span class="sxs-lookup"><span data-stu-id="ad8cf-118">Linux (We recommend Ubuntu 14.04 LTS)</span></span>

    - <span data-ttu-id="ad8cf-119">[Visual Studio Code]( https://www.visualstudio.com/products/code-vs.aspx).</span><span class="sxs-lookup"><span data-stu-id="ad8cf-119">[Visual Studio Code]( https://www.visualstudio.com/products/code-vs.aspx).</span></span> <span data-ttu-id="ad8cf-120">tooinstall Hej kod, ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ad8cf-120">tooinstall hello code, enter hello following command:</span></span>

              sudo dpkg -i code_<version_number>_amd64.deb

    - <span data-ttu-id="ad8cf-121">[Monoljud 4.2.x](http://www.mono-project.com/docs/getting-started/install/linux/).</span><span class="sxs-lookup"><span data-stu-id="ad8cf-121">[Mono 4.2.x](http://www.mono-project.com/docs/getting-started/install/linux/).</span></span> 

        - <span data-ttu-id="ad8cf-122">tooupdate hello deb paketet källa, ange hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="ad8cf-122">tooupdate hello deb package source, enter hello following commands:</span></span>

                sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF
                echo "deb http://download.mono-project.com/repo/debian wheezy/snapshots 4.2.4.4/main" | sudo tee /etc/apt/sources.list.d/mono-xamarin.list
                sudo apt-get update

        - <span data-ttu-id="ad8cf-123">tooinstall Mono, ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ad8cf-123">tooinstall Mono, enter hello following command:</span></span>

                sudo apt-get install mono-complete

            > [!NOTE] 
            > <span data-ttu-id="ad8cf-124">Mono 4.6 stöds inte.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-124">Mono 4.6 is not supported.</span></span> <span data-ttu-id="ad8cf-125">Avinstallera 4.6 helt och hållet innan du installerar 4.2.x.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-125">Uninstall version 4.6 entirely before you install 4.2.x.</span></span>  

        - <span data-ttu-id="ad8cf-126">[Java SE Runtime Environment version 8 update 77 eller senare](https://java.com/download/manual.jsp).</span><span class="sxs-lookup"><span data-stu-id="ad8cf-126">[Java SE Runtime Environment version 8 update 77 or later](https://java.com/download/manual.jsp).</span></span> <span data-ttu-id="ad8cf-127">Anvisningar för installation finns hello [Linux 64-bitars Installationsinstruktioner för Java]( https://java.com/en/download/help/linux_x64_install.xml) sidan.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-127">For instructions on installation, see hello [Linux 64-bit installation instructions for Java]( https://java.com/en/download/help/linux_x64_install.xml) page.</span></span>
        - <span data-ttu-id="ad8cf-128">[.NET core SDK 1.0.3 eller .NET Core 1.1 runtime](https://www.microsoft.com/net/download).</span><span class="sxs-lookup"><span data-stu-id="ad8cf-128">[.NET Core SDK 1.0.3 or .NET Core 1.1 runtime](https://www.microsoft.com/net/download).</span></span>
- <span data-ttu-id="ad8cf-129">MacOS</span><span class="sxs-lookup"><span data-stu-id="ad8cf-129">MacOS</span></span>

    - <span data-ttu-id="ad8cf-130">[Visual Studio Code]( https://www.visualstudio.com/products/code-vs.aspx).</span><span class="sxs-lookup"><span data-stu-id="ad8cf-130">[Visual Studio Code]( https://www.visualstudio.com/products/code-vs.aspx).</span></span>
    - <span data-ttu-id="ad8cf-131">[Mono 4.2.4](http://download.mono-project.com/archive/4.2.4/macos-10-x86/).</span><span class="sxs-lookup"><span data-stu-id="ad8cf-131">[Mono 4.2.4](http://download.mono-project.com/archive/4.2.4/macos-10-x86/).</span></span> 
    - <span data-ttu-id="ad8cf-132">[Java SE Runtime Environment version 8 update 77 eller senare](https://java.com/download/manual.jsp).</span><span class="sxs-lookup"><span data-stu-id="ad8cf-132">[Java SE Runtime Environment version 8 update 77 or later](https://java.com/download/manual.jsp).</span></span> <span data-ttu-id="ad8cf-133">Anvisningar för installation finns hello [Linux 64-bitars Installationsinstruktioner för Java](https://java.com/en/download/help/mac_install.xml) sidan.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-133">For instructions on installation, see hello [Linux 64-bit installation instructions for Java](https://java.com/en/download/help/mac_install.xml) page.</span></span>
    - <span data-ttu-id="ad8cf-134">[.NET core SDK 1.0.3 eller .NET Core 1.1 runtime](https://www.microsoft.com/net/download).</span><span class="sxs-lookup"><span data-stu-id="ad8cf-134">[.NET Core SDK 1.0.3 or .NET Core 1.1 runtime](https://www.microsoft.com/net/download).</span></span>

## <a name="install-data-lake-tools"></a><span data-ttu-id="ad8cf-135">Installera Data Lake-verktyg</span><span class="sxs-lookup"><span data-stu-id="ad8cf-135">Install Data Lake Tools</span></span>

<span data-ttu-id="ad8cf-136">När du har installerat hello förutsättningar kan du installera Data Lake-verktyg för VS-kod.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-136">After you install hello prerequisites, you can install Data Lake Tools for VS Code.</span></span>

<span data-ttu-id="ad8cf-137">**tooinstall Data Lake-verktyg**</span><span class="sxs-lookup"><span data-stu-id="ad8cf-137">**tooinstall Data Lake Tools**</span></span>

1. <span data-ttu-id="ad8cf-138">Öppna Visual Studio-koden.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-138">Open Visual Studio Code.</span></span>
2. <span data-ttu-id="ad8cf-139">Välj Ctrl + P och ange sedan hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ad8cf-139">Select Ctrl+P, and then enter hello following command:</span></span>
```
ext install usql-vscode-ext
```
<span data-ttu-id="ad8cf-140">Du kan se en lista över tillägg för Visual Studio-koden.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-140">You can see a list of Visual Studio code extensions.</span></span> <span data-ttu-id="ad8cf-141">En av dem är **Azure Data Lake-verktyg**.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-141">One of them is **Azure Data Lake Tools**.</span></span>

3. <span data-ttu-id="ad8cf-142">Välj **installera** nästa för**Azure Data Lake-verktyg**.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-142">Select **Install** next too**Azure Data Lake Tools**.</span></span> <span data-ttu-id="ad8cf-143">Efter några sekunder hello **installera** knappen ändras för**ladda**.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-143">After a few seconds, hello **Install** button changes too**Reload**.</span></span>
4. <span data-ttu-id="ad8cf-144">Välj **ladda** tooactivate hello tillägg.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-144">Select **Reload** tooactivate hello extension.</span></span>
5. <span data-ttu-id="ad8cf-145">Välj **OK** tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-145">Select **OK** tooconfirm.</span></span> <span data-ttu-id="ad8cf-146">Du kan se Azure Data Lake-verktyg i hello **tillägg** fönstret.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-146">You can see Azure Data Lake Tools in hello **Extensions** pane.</span></span>
    <span data-ttu-id="ad8cf-147">![Data Lake-verktyg för Visual Studio Code-tillägg](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extensions.png)</span><span class="sxs-lookup"><span data-stu-id="ad8cf-147">![Data Lake Tools for Visual Studio Code Extensions pane](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extensions.png)</span></span>

## <a name="activate-azure-data-lake-tools"></a><span data-ttu-id="ad8cf-148">Aktivera Azure Data Lake-verktyg</span><span class="sxs-lookup"><span data-stu-id="ad8cf-148">Activate Azure Data Lake Tools</span></span>
<span data-ttu-id="ad8cf-149">Skapa en ny .usql fil eller öppna ett befintligt .usql tooactivate hello filtillägg.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-149">Create a new .usql file or open an existing .usql file tooactivate hello extension.</span></span> 

## <a name="connect-tooazure"></a><span data-ttu-id="ad8cf-150">Ansluta tooAzure</span><span class="sxs-lookup"><span data-stu-id="ad8cf-150">Connect tooAzure</span></span>

<span data-ttu-id="ad8cf-151">Innan du kan kompilera och köra U-SQL-skript i Data Lake Analytics, måste du ansluta tooyour Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-151">Before you can compile and run U-SQL scripts in Data Lake Analytics, you must connect tooyour Azure account.</span></span>

<span data-ttu-id="ad8cf-152">**tooconnect tooAzure**</span><span class="sxs-lookup"><span data-stu-id="ad8cf-152">**tooconnect tooAzure**</span></span>

1.  <span data-ttu-id="ad8cf-153">Välj Ctrl + Skift + P tooopen hello kommandot palett.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-153">Select Ctrl+Shift+P tooopen hello command palette.</span></span> 
2.  <span data-ttu-id="ad8cf-154">Ange **ADL: inloggning**.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-154">Enter **ADL: Login**.</span></span> <span data-ttu-id="ad8cf-155">hello inloggningsinformation visas i hello **utdata** fönstret.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-155">hello login information appears in hello **Output** pane.</span></span>

    <span data-ttu-id="ad8cf-156">![Data Lake-verktyg för Visual Studio Code kommandot paletten](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login.png)
    ![Data Lake-verktyg för Visual Studio Code enhetsinformation för inloggning](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-login-info.png)</span><span class="sxs-lookup"><span data-stu-id="ad8cf-156">![Data Lake Tools for Visual Studio Code command palette](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login.png)
![Data Lake Tools for Visual Studio Code device login information](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-login-info.png)</span></span>
3. <span data-ttu-id="ad8cf-157">Välj Ctrl + klicka på hello inloggnings-URL: https://aka.ms/devicelogin tooopen hello inloggningen webbsidan.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-157">Select Ctrl+click on hello login URL: https://aka.ms/devicelogin tooopen hello login webpage.</span></span> <span data-ttu-id="ad8cf-158">Ange hello kod **G567LX42V** i textrutan för hello och välj sedan **Fortsätt**.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-158">Enter hello code **G567LX42V** into hello text box, and then select **Continue**.</span></span>

   ![Data Lake-verktyg för Visual Studio Code inloggningen klistra in koden](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login-paste-code.png )   
4.  <span data-ttu-id="ad8cf-160">Följ hello instruktioner toosign i från hello webbsidan.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-160">Follow hello instructions toosign in from hello webpage.</span></span> <span data-ttu-id="ad8cf-161">När du är ansluten visas namnet på ditt Azure hello statusfältet i hello nedre vänstra hörnet av hello **VS kod** fönster.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-161">When you're connected, your Azure account name appears on hello status bar in hello lower-left corner of hello **VS Code** window.</span></span> 

    > [!NOTE] 
    > <span data-ttu-id="ad8cf-162">Om ditt konto har två faktorer är aktiverat, rekommenderar vi att du använder phone autentisering i stället för en PIN-kod.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-162">If your account has two factors enabled, we recommend that you use phone authentication rather than using a PIN.</span></span>

<span data-ttu-id="ad8cf-163">toosign, ange hello kommando **ADL: Logga ut**.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-163">toosign out, enter hello command **ADL: Logout**.</span></span>

## <a name="list-your-data-lake-analytics-accounts"></a><span data-ttu-id="ad8cf-164">Visa en lista över dina Data Lake Analytics-konton</span><span class="sxs-lookup"><span data-stu-id="ad8cf-164">List your Data Lake Analytics accounts</span></span>

<span data-ttu-id="ad8cf-165">tootest hello anslutning, hämta en lista över dina Data Lake Analytics-konton.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-165">tootest hello connection, get a list of your Data Lake Analytics accounts.</span></span>

<span data-ttu-id="ad8cf-166">**toolist hello Data Lake Analytics-konton under din Azure-prenumeration**</span><span class="sxs-lookup"><span data-stu-id="ad8cf-166">**toolist hello Data Lake Analytics accounts under your Azure subscription**</span></span>

1. <span data-ttu-id="ad8cf-167">Välj Ctrl + Skift + P tooopen hello kommandot palett.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-167">Select Ctrl+Shift+P tooopen hello command palette.</span></span>
2. <span data-ttu-id="ad8cf-168">Ange **ADL: visa en lista över konton**.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-168">Enter **ADL: List Accounts**.</span></span> <span data-ttu-id="ad8cf-169">hello konton visas i hello **utdata** fönstret.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-169">hello accounts appear in hello **Output** pane.</span></span>

## <a name="open-hello-sample-script"></a><span data-ttu-id="ad8cf-170">Öppna hello exempelskript</span><span class="sxs-lookup"><span data-stu-id="ad8cf-170">Open hello sample script</span></span>
<span data-ttu-id="ad8cf-171">Öppna hello kommandot palett (Ctrl + Skift + P) och ange **ADL: öppna exempelskript**.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-171">Open hello command palette (Ctrl+Shift+P) and enter **ADL: Open Sample Script**.</span></span> <span data-ttu-id="ad8cf-172">Då öppnas en annan instans av det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-172">This opens another instance of this sample.</span></span> <span data-ttu-id="ad8cf-173">Du kan också redigera, konfigurera och skicka skript på den här instansen.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-173">You can also edit, configure, and submit script on this instance.</span></span>

## <a name="work-with-u-sql"></a><span data-ttu-id="ad8cf-174">Arbeta med U-SQL</span><span class="sxs-lookup"><span data-stu-id="ad8cf-174">Work with U-SQL</span></span>

<span data-ttu-id="ad8cf-175">Du måste öppna en U-SQL-fil eller en mapp toowork med U-SQL.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-175">You need open either a U-SQL file or a folder toowork with U-SQL.</span></span>

<span data-ttu-id="ad8cf-176">**tooopen en mapp för U-SQL-projekt**</span><span class="sxs-lookup"><span data-stu-id="ad8cf-176">**tooopen a folder for your U-SQL project**</span></span>

1. <span data-ttu-id="ad8cf-177">Välj Visual Studio Code hello **filen** -menyn och välj sedan **öppna mappen**.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-177">From Visual Studio Code, select hello **File** menu, and then select **Open Folder**.</span></span>
2. <span data-ttu-id="ad8cf-178">Ange en mapp och välj sedan **Välj mappen**.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-178">Specify a folder, and then select **Select Folder**.</span></span>
3. <span data-ttu-id="ad8cf-179">Välj hello **filen** -menyn och välj sedan **ny**.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-179">Select hello **File** menu, and then select **New**.</span></span> <span data-ttu-id="ad8cf-180">Toohello projektet läggs en namnlös-1-filen.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-180">An Untitled-1 file is added toohello project.</span></span>
4. <span data-ttu-id="ad8cf-181">Ange följande kod i filen hello Namnlös 1 hello:</span><span class="sxs-lookup"><span data-stu-id="ad8cf-181">Enter hello following code into hello Untitled-1 file:</span></span>

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

    <span data-ttu-id="ad8cf-182">hello skriptet skapar en departments.csv-fil med vissa data som ingår i hello/Output-mappen.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-182">hello script creates a departments.csv file with some data included in hello /output folder.</span></span>

5. <span data-ttu-id="ad8cf-183">Spara hello som **myUSQL.usql** öppnats i ett hello mapp.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-183">Save hello file as **myUSQL.usql** in hello opened folder.</span></span> <span data-ttu-id="ad8cf-184">En konfigurationsfil för adltools_settings.json läggs toohello projekt.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-184">A adltools_settings.json configuration file is also added toohello project.</span></span>
4. <span data-ttu-id="ad8cf-185">Öppna och konfigurera adltools_settings.json med hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="ad8cf-185">Open and configure adltools_settings.json with hello following properties:</span></span>

    - <span data-ttu-id="ad8cf-186">Konto: Ett Data Lake Analytics-konto under din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-186">Account:  A Data Lake Analytics account under your Azure subscription.</span></span>
    - <span data-ttu-id="ad8cf-187">Databas: En databas med ditt konto.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-187">Database: A database under your account.</span></span> <span data-ttu-id="ad8cf-188">hello standardvärdet är **master**.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-188">hello default is **master**.</span></span>
    - <span data-ttu-id="ad8cf-189">Schema: Ett schema under din databas.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-189">Schema: A schema under your database.</span></span> <span data-ttu-id="ad8cf-190">hello standardvärdet är **dbo**.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-190">hello default is **dbo**.</span></span>
    - <span data-ttu-id="ad8cf-191">Valfria inställningar:</span><span class="sxs-lookup"><span data-stu-id="ad8cf-191">Optional settings:</span></span>
        - <span data-ttu-id="ad8cf-192">Prioritet: hello prioritetsintervall är 1 too1000 med 1 som hello högst prioritet.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-192">Priority: hello priority range is from 1 too1000 with 1 as hello highest priority.</span></span> <span data-ttu-id="ad8cf-193">hello standardvärdet är **1000**.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-193">hello default value is **1000**.</span></span>
        - <span data-ttu-id="ad8cf-194">Parallellitet: hello parallellitet intervall är 1 too150.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-194">Parallelism: hello parallelism range is from 1 too150.</span></span> <span data-ttu-id="ad8cf-195">hello standardvärdet är hello maximala parallellitet tillåts i ditt Azure Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-195">hello default value is hello maximum parallelism allowed in your Azure Data Lake Analytics account.</span></span> 
        
        > [!NOTE] 
        > <span data-ttu-id="ad8cf-196">Om hello inställningarna är ogiltig, används hello standardvärdena.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-196">If hello settings are invalid, hello default values are used.</span></span>

    ![Data Lake-verktyg för Visual Studio Code konfigurationsfil](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-configuration-file.png)

    <span data-ttu-id="ad8cf-198">En beräkning Data Lake Analytics-kontot är behövs toocompile och köra U-SQL-jobb.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-198">A compute Data Lake Analytics account is needed toocompile and run U-SQL jobs.</span></span> <span data-ttu-id="ad8cf-199">Du måste konfigurera hello datorkonto innan du kompilera och köra U-SQL-jobb.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-199">You must configure hello computer account before you can compile and run U-SQL jobs.</span></span>
    
<span data-ttu-id="ad8cf-200">När hello konfiguration sparas visas hello kontot, databas och schema information på hello statusfältet hello nedre vänstra hörn för hello motsvarande .usql-filen.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-200">After hello configuration is saved, hello account, database, and schema information appears on hello status bar at hello bottom-left corner of hello corresponding .usql file.</span></span> 
 
 
<span data-ttu-id="ad8cf-201">Jämfört med tooopening en fil när du öppnar en mapp som du kan:</span><span class="sxs-lookup"><span data-stu-id="ad8cf-201">Compared tooopening a file, when you open a folder you can:</span></span>

- <span data-ttu-id="ad8cf-202">Använda en fil med bakomliggande kod.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-202">Use a code-behind file.</span></span> <span data-ttu-id="ad8cf-203">Bakomliggande kod stöds inte i läget för hello-fil.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-203">In hello single-file mode, code-behind is not supported.</span></span>
- <span data-ttu-id="ad8cf-204">Använd en konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-204">Use a configuration file.</span></span> <span data-ttu-id="ad8cf-205">När du öppnar en mapp dela hello skript i hello arbetsmapp en enda konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-205">When you open a folder, hello scripts in hello working folder share a single configuration file.</span></span>


<span data-ttu-id="ad8cf-206">hello U-SQL-skript kompilerar via fjärranslutning via hello Data Lake Analytics-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-206">hello U-SQL script compiles remotely through hello Data Lake Analytics service.</span></span> <span data-ttu-id="ad8cf-207">När du skickar hello **Kompilera** kommandot hello U-SQL-skript skickas tooyour Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-207">When you issue hello **compile** command, hello U-SQL script is sent tooyour Data Lake Analytics account.</span></span> <span data-ttu-id="ad8cf-208">Visual Studio-koden får senare hello Kompileringsresultatet.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-208">Later, Visual Studio Code receives hello compilation result.</span></span> <span data-ttu-id="ad8cf-209">På grund av toohello remote kompilering kräver Visual Studio Code du lista hello information tooconnect tooyour Data Lake Analytics-kontot i hello konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-209">Due toohello remote compilation, Visual Studio Code requires that you list hello information tooconnect tooyour Data Lake Analytics account in hello configuration file.</span></span>

<span data-ttu-id="ad8cf-210">**toocompile U-SQL-skript**</span><span class="sxs-lookup"><span data-stu-id="ad8cf-210">**toocompile a U-SQL script**</span></span>

1. <span data-ttu-id="ad8cf-211">Välj Ctrl + Skift + P tooopen hello kommandot palett.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-211">Select Ctrl+Shift+P tooopen hello command palette.</span></span> 
2. <span data-ttu-id="ad8cf-212">Ange **ADL: kompilera skriptet**.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-212">Enter **ADL: Compile Script**.</span></span> <span data-ttu-id="ad8cf-213">hello kompilera visas resultaten i hello **utdata** fönster.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-213">hello compile results appear in hello **Output** window.</span></span> <span data-ttu-id="ad8cf-214">Du kan också högerklicka på en skriptfil och sedan välja **ADL: kompilera skriptet** toocompile U-SQL-jobbet.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-214">You can also right-click a script file, and then select **ADL: Compile Script** toocompile a U-SQL job.</span></span> <span data-ttu-id="ad8cf-215">hello Kompileringsresultatet visas i hello **utdata** fönstret.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-215">hello compilation result appears in hello **Output** pane.</span></span>
 

<span data-ttu-id="ad8cf-216">**toosubmit U-SQL-skript**</span><span class="sxs-lookup"><span data-stu-id="ad8cf-216">**toosubmit a U-SQL script**</span></span>

1. <span data-ttu-id="ad8cf-217">Välj Ctrl + Skift + P tooopen hello kommandot palett.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-217">Select Ctrl+Shift+P tooopen hello command palette.</span></span> 
2. <span data-ttu-id="ad8cf-218">Ange **ADL: skicka jobbet**.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-218">Enter **ADL: Submit Job**.</span></span>  <span data-ttu-id="ad8cf-219">Du kan också högerklicka på en skriptfil och sedan välja **ADL: skicka jobbet**.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-219">You can also right-click a script file, and then select **ADL: Submit Job**.</span></span> 

<span data-ttu-id="ad8cf-220">När du skickar ett U-SQL-jobb hello skicka loggarna visas i hello **utdata** fönster i VS-kod.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-220">After you submit a U-SQL job, hello submission logs appear in hello **Output** window in VS Code.</span></span> <span data-ttu-id="ad8cf-221">Om hello skicka lyckas visas hello jobbet URL också.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-221">If hello submission is successful, hello job URL appears as well.</span></span> <span data-ttu-id="ad8cf-222">Du kan öppna hello jobbet URL i en web webbläsare tootrack hello realtid jobbstatus.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-222">You can open hello job URL in a web browser tootrack hello real-time job status.</span></span>

<span data-ttu-id="ad8cf-223">Ange tooenable hello utdata från hello jobbinformation **jobInformationOutputPath** i hello **vs Platskod för hello u-sql_settings.json** fil.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-223">tooenable hello output of hello job details, set **jobInformationOutputPath** in hello **vs code for hello u-sql_settings.json** file.</span></span>
 
## <a name="use-a-code-behind-file"></a><span data-ttu-id="ad8cf-224">Använda en fil med bakomliggande kod</span><span class="sxs-lookup"><span data-stu-id="ad8cf-224">Use a code-behind file</span></span>

<span data-ttu-id="ad8cf-225">En fil med bakomliggande kod är ett C#-fil som är associerad med ett enda U-SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-225">A code-behind file is a C# file associated with a single U-SQL script.</span></span> <span data-ttu-id="ad8cf-226">Du kan definiera ett skript som är dedikerad tooUDO, UDA, UDT och UDF i hello bakomliggande kod-filen.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-226">You can define a script dedicated tooUDO, UDA, UDT, and UDF in hello code-behind file.</span></span> <span data-ttu-id="ad8cf-227">hello kan UDO, UDA, UDT och UDF användas direkt i hello skript utan att först registrera hello sammansättningen.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-227">hello UDO, UDA, UDT, and UDF can be used directly in hello script without registering hello assembly first.</span></span> <span data-ttu-id="ad8cf-228">hello bakomliggande kod filen placeras i hello samma mapp som dess peering U-SQL-skriptfilen.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-228">hello code-behind file is put in hello same folder as its peering U-SQL script file.</span></span> <span data-ttu-id="ad8cf-229">Om hello skriptet heter xxx.usql, heter hello bakomliggande kod som xxx.usql.cs.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-229">If hello script is named xxx.usql, hello code-behind is named as xxx.usql.cs.</span></span> <span data-ttu-id="ad8cf-230">Om du manuellt ta bort hello bakomliggande kod filen är hello bakomliggande kod funktionen inaktiverad för dess associerade U-SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-230">If you manually delete hello code-behind file, hello code-behind feature is disabled for its associated U-SQL script.</span></span> <span data-ttu-id="ad8cf-231">Mer information om hur du skriver kunden koden för U-SQL-skript finns [skrivning och använder anpassad kod i U-SQL: användardefinierade funktioner]( https://blogs.msdn.microsoft.com/visualstudio/2015/10/28/writing-and-using-custom-code-in-u-sql-user-defined-functions/).</span><span class="sxs-lookup"><span data-stu-id="ad8cf-231">For more information about writing customer code for U-SQL script, see [Writing and Using Custom Code in U-SQL: User-Defined Functions]( https://blogs.msdn.microsoft.com/visualstudio/2015/10/28/writing-and-using-custom-code-in-u-sql-user-defined-functions/).</span></span>

<span data-ttu-id="ad8cf-232">toosupport bakomliggande kod, måste du öppna en arbetsmapp.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-232">toosupport code-behind, you must open a working folder.</span></span> 

<span data-ttu-id="ad8cf-233">**toogenerate en bakomliggande kod-fil**</span><span class="sxs-lookup"><span data-stu-id="ad8cf-233">**toogenerate a code-behind file**</span></span>

1. <span data-ttu-id="ad8cf-234">Öppna en källfil.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-234">Open a source file.</span></span> 
2. <span data-ttu-id="ad8cf-235">Välj Ctrl + Skift + P tooopen hello kommandot palett.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-235">Select Ctrl+Shift+P tooopen hello command palette.</span></span>
3. <span data-ttu-id="ad8cf-236">Ange **ADL: Generera kod bakom**.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-236">Enter **ADL: Generate Code Behind**.</span></span> <span data-ttu-id="ad8cf-237">Bakomliggande kod skapas en fil i hello samma mapp.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-237">A code-behind file is created in hello same folder.</span></span> 

<span data-ttu-id="ad8cf-238">Du kan också högerklicka på en skriptfil och sedan välja **ADL: Generera kod bakom**.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-238">You can also right-click a script file, and then select **ADL: Generate Code Behind**.</span></span> 

<span data-ttu-id="ad8cf-239">toocompile och skicka ett U-SQL-skript med en fil med bakomliggande kod är hello samma som med hello fristående U-SQL-skriptfil.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-239">toocompile and submit a U-SQL script with a code-behind file is hello same as with hello standalone U-SQL script file.</span></span>

<span data-ttu-id="ad8cf-240">hello följande två skärmdumpar visar en fil med bakomliggande kod och dess associerade U-SQL-skriptfilen:</span><span class="sxs-lookup"><span data-stu-id="ad8cf-240">hello following two screenshots show a code-behind file and its associated U-SQL script file:</span></span>
 
![Data Lake-verktyg för Visual Studio Code bakomliggande kod](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-code-behind.png)

![Data Lake-verktyg för Visual Studio Code bakomliggande kod skriptfilen](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-code-behind-call.png) 

## <a name="use-assemblies"></a><span data-ttu-id="ad8cf-243">Använd sammansättningar</span><span class="sxs-lookup"><span data-stu-id="ad8cf-243">Use assemblies</span></span>

<span data-ttu-id="ad8cf-244">Information om hur du utvecklar sammansättningar finns [utveckla U-SQL-sammansättningar för Azure Data Lake Analytics-jobb](data-lake-analytics-u-sql-develop-assemblies.md).</span><span class="sxs-lookup"><span data-stu-id="ad8cf-244">For information on developing assemblies, see [Develop U-SQL assemblies for Azure Data Lake Analytics jobs](data-lake-analytics-u-sql-develop-assemblies.md).</span></span>

<span data-ttu-id="ad8cf-245">Du kan använda Data Lake-verktyg tooregister anpassad kodsammansättningar i hello Data Lake Analytics-katalogen.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-245">You can use Data Lake Tools tooregister custom code assemblies in hello Data Lake Analytics catalog.</span></span>

<span data-ttu-id="ad8cf-246">**tooregister en sammansättning**</span><span class="sxs-lookup"><span data-stu-id="ad8cf-246">**tooregister an assembly**</span></span>

<span data-ttu-id="ad8cf-247">Du kan registrera hello sammansättningen via hello **ADL: registrera sammansättningen** eller **ADL: registrera sammansättningen via konfiguration** kommandon.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-247">You can register hello assembly through hello **ADL: Register Assembly** or **ADL: Register Assembly through Configuration** commands.</span></span>

<span data-ttu-id="ad8cf-248">**tooregister via hello ADL: registrera sammansättningen kommando**</span><span class="sxs-lookup"><span data-stu-id="ad8cf-248">**tooregister through hello ADL: Register Assembly command**</span></span>
1.  <span data-ttu-id="ad8cf-249">Välj Ctrl + Skift + P tooopen hello kommandot palett.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-249">Select Ctrl+Shift+P tooopen hello command palette.</span></span>
2.  <span data-ttu-id="ad8cf-250">Ange **ADL: registrera sammansättningen**.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-250">Enter **ADL: Register Assembly**.</span></span> 
3.  <span data-ttu-id="ad8cf-251">Ange sökväg för hello lokala sammansättning.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-251">Specify hello local assembly path.</span></span> 
4.  <span data-ttu-id="ad8cf-252">Välj ett Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-252">Select a Data Lake Analytics account.</span></span>
5.  <span data-ttu-id="ad8cf-253">Välj en databas.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-253">Select a database.</span></span>

<span data-ttu-id="ad8cf-254">Resultat: hello portalen öppnas i en webbläsare och visar hello sammansättningen registreringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-254">Results: hello portal is opened in a browser and displays hello assembly registration process.</span></span>  

<span data-ttu-id="ad8cf-255">En annan praktiskt sätt tootrigger hello **ADL: registrera sammansättningen** kommandot är tooright Klicka hello DLL-filen i Utforskaren.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-255">Another convenient way tootrigger hello **ADL: Register Assembly** command is tooright-click hello .dll file in File Explorer.</span></span> 

<span data-ttu-id="ad8cf-256">**tooregister men hello ADL: registrera sammansättningen med hjälp av kommandot konfiguration**</span><span class="sxs-lookup"><span data-stu-id="ad8cf-256">**tooregister though hello ADL: Register Assembly through Configuration command**</span></span>
1.  <span data-ttu-id="ad8cf-257">Välj Ctrl + Skift + P tooopen hello kommandot palett.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-257">Select Ctrl+Shift+P tooopen hello command palette.</span></span>
2.  <span data-ttu-id="ad8cf-258">Ange **ADL: registrera sammansättningen via konfiguration**.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-258">Enter **ADL: Register Assembly through Configuration**.</span></span> 
3.  <span data-ttu-id="ad8cf-259">Ange sökväg för hello lokala sammansättning.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-259">Specify hello local assembly path.</span></span> 
4.  <span data-ttu-id="ad8cf-260">hello JSON-fil visas.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-260">hello JSON file is displayed.</span></span> <span data-ttu-id="ad8cf-261">Granska och redigera hello paketberoenden och Resursparametrar, om det behövs.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-261">Review and edit hello assembly dependencies and resource parameters, if needed.</span></span> <span data-ttu-id="ad8cf-262">Instruktioner visas i hello **utdata** fönster.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-262">Instructions are displayed in hello **Output** window.</span></span> <span data-ttu-id="ad8cf-263">tooproceed toohello sammansättningen registrering, spara (Ctrl + S) hello JSON-fil.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-263">tooproceed toohello assembly registration, save (Ctrl+S) hello JSON file.</span></span>

![Data Lake-verktyg för Visual Studio Code bakomliggande kod](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-register-assembly-advance.png)
>[!NOTE]
>- <span data-ttu-id="ad8cf-265">Beroenden för sammansättningen: Azure Data Lake-verktyg autodetects om hello DLL har några beroenden.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-265">Assembly dependencies: Azure Data Lake Tools autodetects whether hello DLL has any dependencies.</span></span> <span data-ttu-id="ad8cf-266">hello beroenden visas i hello JSON-fil när de identifieras.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-266">hello dependencies are displayed in hello JSON file after they are detected.</span></span> 
>- <span data-ttu-id="ad8cf-267">Resurser: Du kan överföra dina DLL-resurser (exempelvis txt, .png och CSV) som en del av hello regasm.exe för registrering.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-267">Resources: You can upload your DLL resources (for example, .txt, .png, and .csv) as part of hello assembly registration.</span></span> 

<span data-ttu-id="ad8cf-268">Ett annat sätt tootrigger hello **ADL: registrera sammansättningen via konfiguration** kommandot är tooright Klicka hello DLL-filen i Utforskaren.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-268">Another way tootrigger hello **ADL: Register Assembly through Configuration** command is tooright-click hello .dll file in File Explorer.</span></span> 

<span data-ttu-id="ad8cf-269">hello följande U-SQL-kod visar hur toocall en sammansättning.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-269">hello following U-SQL code demonstrates how toocall an assembly.</span></span> <span data-ttu-id="ad8cf-270">I exemplet hello hello sammansättningsnamnet är *testa*.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-270">In hello sample, hello assembly name is *test*.</span></span>

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


## <a name="access-hello-data-lake-analytics-catalog"></a><span data-ttu-id="ad8cf-271">Åtkomst hello Data Lake Analytics-katalog</span><span class="sxs-lookup"><span data-stu-id="ad8cf-271">Access hello Data Lake Analytics catalog</span></span>

<span data-ttu-id="ad8cf-272">När du har anslutit tooAzure kan använda du hello följande steg tooaccess hello U-SQL-katalogen.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-272">After you have connected tooAzure, you can use hello following steps tooaccess hello U-SQL catalog.</span></span>

<span data-ttu-id="ad8cf-273">**tooaccess hello Azure Data Lake Analytics-metadata**</span><span class="sxs-lookup"><span data-stu-id="ad8cf-273">**tooaccess hello Azure Data Lake Analytics metadata**</span></span>

1.  <span data-ttu-id="ad8cf-274">Välj Ctrl + Skift + P och ange **ADL: lista tabeller**.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-274">Select Ctrl+Shift+P, and then enter **ADL: List Tables**.</span></span>
2.  <span data-ttu-id="ad8cf-275">Välj en av hello Data Lake Analytics-konton.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-275">Select one of hello Data Lake Analytics accounts.</span></span>
3.  <span data-ttu-id="ad8cf-276">Välj en av hello Data Lake Analytics-databaser.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-276">Select one of hello Data Lake Analytics databases.</span></span>
4.  <span data-ttu-id="ad8cf-277">Välj en av hello scheman.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-277">Select one of hello schemas.</span></span> <span data-ttu-id="ad8cf-278">Du kan se hello listan över tabeller.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-278">You can see hello list of tables.</span></span>

## <a name="view-data-lake-analytics-jobs"></a><span data-ttu-id="ad8cf-279">Visa Data Lake Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="ad8cf-279">View Data Lake Analytics jobs</span></span>

<span data-ttu-id="ad8cf-280">**tooview Data Lake Analytics-jobb**</span><span class="sxs-lookup"><span data-stu-id="ad8cf-280">**tooview Data Lake Analytics jobs**</span></span>
1.  <span data-ttu-id="ad8cf-281">Öppna hello kommandot palett (Ctrl + Skift + P) och välj **ADL: Visa jobb**.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-281">Open hello command palette (Ctrl+Shift+P) and select **ADL: Show Job**.</span></span> 
2.  <span data-ttu-id="ad8cf-282">Välj en Data Lake Analytics eller lokalt konto.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-282">Select a Data Lake Analytics or local account.</span></span> 
3.  <span data-ttu-id="ad8cf-283">Vänta tills hello jobb lista för hello konto tooappear.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-283">Wait for hello jobs list for hello account tooappear.</span></span>
4.  <span data-ttu-id="ad8cf-284">Välj ett jobb jobblistan, Data Lake-verktyg öppnas hello jobbinformation i hello Azure-portalen och visar hello Jobbinformations fil i VS-kod.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-284">Select a job from job list, Data Lake Tools opens hello job details in hello Azure portal and displays hello JobInfo file in VS Code.</span></span>

![Data Lake-verktyg för Visual Studio Code IntelliSense objekttyper](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-show-job.png)

## <a name="azure-data-lake-storage-integration"></a><span data-ttu-id="ad8cf-286">Azure Data Lake Lagringsintegrering</span><span class="sxs-lookup"><span data-stu-id="ad8cf-286">Azure Data Lake Storage integration</span></span>

<span data-ttu-id="ad8cf-287">Du kan använda lagring av Azure Data Lake-relaterade kommandon:</span><span class="sxs-lookup"><span data-stu-id="ad8cf-287">You can use Azure Data Lake Storage-related commands to:</span></span>
 - <span data-ttu-id="ad8cf-288">Bläddra igenom hello Azure Data Lake lagringsresurser.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-288">Browse through hello Azure Data Lake Storage resources.</span></span> 
 - <span data-ttu-id="ad8cf-289">Förhandsgranska hello Azure Data Lake Storage-fil.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-289">Preview hello Azure Data Lake Storage file.</span></span>  
 - <span data-ttu-id="ad8cf-290">Överför hello filen direkt tooAzure lagring av Data Lake i VS-kod.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-290">Upload hello file directly tooAzure Data Lake Storage in VS Code.</span></span> 

### <a name="list-hello-storage-path"></a><span data-ttu-id="ad8cf-291">Sökvägen till listan hello lagring</span><span class="sxs-lookup"><span data-stu-id="ad8cf-291">List hello storage path</span></span> 
<span data-ttu-id="ad8cf-292">Du kan ange sökvägen till hello lagring via hello kommandot paletten eller högerklicka på.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-292">You can list hello storage path through hello command palette or through right-click.</span></span>

<span data-ttu-id="ad8cf-293">**sökvägen till toolist hello lagring via hello kommandot paletten**</span><span class="sxs-lookup"><span data-stu-id="ad8cf-293">**toolist hello storage path through hello command palette**</span></span>

1.  <span data-ttu-id="ad8cf-294">Öppna hello kommandot palett (Ctrl + Skift + P) och ange **ADL: listan Lagringssökväg**.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-294">Open hello command palette (Ctrl+Shift+P) and enter **ADL: List Storage Path**.</span></span>

    ![Data Lake-verktyg för Visual Studio Code lista lagringssökväg](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-storage.png)

2.  <span data-ttu-id="ad8cf-296">Välj din bästa sättet för att lista hello sökvägen för lagring.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-296">Select your preferred way for listing hello storage path.</span></span> <span data-ttu-id="ad8cf-297">Den här övergången använder **ange en sökväg** som exempel.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-297">This passage uses **Enter a path** as an example.</span></span>

    ![Data Lake-verktyg för Visual Studio Code enkelriktade toolist hello lagringssökväg](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account-selectoneway.png)

    > [!NOTE]
    >- <span data-ttu-id="ad8cf-299">VS-koden behålls hello senast besökt sökväg i varje Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-299">VS Code keeps hello last-visited path in every Data Lake Analytics account.</span></span> <span data-ttu-id="ad8cf-300">Till exempel: ss tt /.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-300">For example: /tt/ss.</span></span>
    >- <span data-ttu-id="ad8cf-301">Webbläsaren från rotsökvägen: hello listan rotsökvägen från valda Data Lake Analytics-kontot eller en lokal sökväg.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-301">Browser from root path: hello list root path from your selected Data Lake Analytics account or a local path.</span></span>
    >- <span data-ttu-id="ad8cf-302">Ange en sökväg: visa en lista med en angiven sökväg från ditt valda Data Lake Analytics-konto eller en lokal sökväg.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-302">Enter a path: List a specified path from your selected Data Lake Analytics account or a local path.</span></span>
    
3. <span data-ttu-id="ad8cf-303">Välj ett konto från hello lokal sökväg eller ett Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-303">Select an account from hello local path or a Data Lake Analytics account.</span></span>

    ![Data Lake-verktyg för Visual Studio Code Välj Mer](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account.png)

4. <span data-ttu-id="ad8cf-305">Välj **mer** toolist mer Data Lake Analytics-konton och välj sedan ett Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-305">Select **more** toolist more Data Lake Analytics accounts, and then select a Data Lake Analytics account.</span></span>

    ![Data Lake-verktyg för Visual Studio Code välja ett konto](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-select-adla-account.png)

5.  <span data-ttu-id="ad8cf-307">Ange en sökväg till Azure storage.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-307">Enter an Azure storage path.</span></span> <span data-ttu-id="ad8cf-308">Till exempel/utdata.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-308">For example, /output.</span></span>

    ![Data Lake-verktyg för Visual Studio Code ange sökvägen till lagring](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-input-path.png)

6.  <span data-ttu-id="ad8cf-310">Resultat: hello kommandot paletten visar hello sökvägsinformation baserat på dina uppgifter.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-310">Results: hello command palette lists hello path information based on your entries.</span></span>

    ![Data Lake-verktyg för Visual Studio Code lista lagring sökväg resultat](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-path.png)

<span data-ttu-id="ad8cf-312">Det enklaste sättet toolist hello relativ sökväg är via hello högerklickar du på snabbmenyn.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-312">A more convenient way toolist hello relative path is through hello right-click context menu.</span></span>

<span data-ttu-id="ad8cf-313">**sökvägen till toolist hello lagring via högerklickar du på**</span><span class="sxs-lookup"><span data-stu-id="ad8cf-313">**toolist hello storage path through right-click**</span></span>

1.  <span data-ttu-id="ad8cf-314">Högerklicka på hello sökväg sträng tooselect **lista Lagringssökväg**.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-314">Right-click hello path string tooselect **List Storage Path**.</span></span>

       ![Data Lake-verktyg för Visual Studio Code högerklickar du på snabbmenyn](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-right-click-path.png)

2. <span data-ttu-id="ad8cf-316">hello valda relativa sökvägen visas i hello kommandot palett.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-316">hello selected relative path appears in hello command palette.</span></span>

   ![Data Lake-verktyg för Visual Studio Code valda relativ sökväg](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-relative-path.png)

3.  <span data-ttu-id="ad8cf-318">Välj ett konto från hello lokal sökväg eller ett Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-318">Select an account from hello local path or a Data Lake Analytics account.</span></span>

       ![Data Lake-verktyg för Visual Studio Code välja ett konto](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account.png)

4.  <span data-ttu-id="ad8cf-320">Resultat: hello kommandot paletten visar hello mappar och filer för hello aktuella sökvägen.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-320">Results: hello command palette lists hello folders and files for hello current path.</span></span>

       ![Data Lake-verktyg för Visual Studio Code lista från hello aktuella sökvägen](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-current.png)

### <a name="preview-hello-storage-file"></a><span data-ttu-id="ad8cf-322">Lagringsfilen för förhandsgranskning hello</span><span class="sxs-lookup"><span data-stu-id="ad8cf-322">Preview hello storage file</span></span>
<span data-ttu-id="ad8cf-323">Du kan förhandsgranska hello lagringsfilen via hello kommandot paletten eller högerklicka på.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-323">You can preview hello storage file through hello command palette or through right-click.</span></span>

<span data-ttu-id="ad8cf-324">**toopreview hello lagringsfilen via hello kommandot paletten**</span><span class="sxs-lookup"><span data-stu-id="ad8cf-324">**toopreview hello storage file through hello command palette**</span></span>

1.  <span data-ttu-id="ad8cf-325">Öppna hello kommandot palett (Ctrl + Skift + P) och ange **ADL: Preview lagringsfilen**.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-325">Open hello command palette (Ctrl+Shift+P) and enter **ADL: Preview Storage File**.</span></span>

       ![Data Lake-verktyg för Visual Studio Code Förhandsgranska lagringsfilen](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-preview.png)

2.  <span data-ttu-id="ad8cf-327">Välj ett konto från hello lokal sökväg eller ett Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-327">Select an account from hello local path or a Data Lake Analytics account.</span></span>

       ![Data Lake-verktyg för Visual Studio Code listan konto](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account.png)

3.  <span data-ttu-id="ad8cf-329">Välj **mer** toolist mer Data Lake Analytics-konton och välj sedan ett Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-329">Select **more** toolist more Data Lake Analytics accounts, and then select a Data Lake Analytics account.</span></span>

       ![Data Lake-verktyg för Visual Studio Code välja ett konto](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-select-adla-account.png)

4.  <span data-ttu-id="ad8cf-331">Ange ett Azure storage-sökvägen eller en fil.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-331">Enter an Azure storage path or file.</span></span> <span data-ttu-id="ad8cf-332">Till exempel /output/SearchLog.txt.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-332">For example, /output/SearchLog.txt.</span></span>

       ![Data Lake-verktyg för Visual Studio Code ange lagringssökväg och filnamn](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-input-preview-file.png)

5.  <span data-ttu-id="ad8cf-334">Resultat: hello kommandot paletten visar hello sökvägsinformation baserat på dina uppgifter.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-334">Results: hello command palette lists hello path information based on your entries.</span></span>

       ![Data Lake-verktyg för Visual Studio Code förhandsgranska filen resultat](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-preview-results.png)

<span data-ttu-id="ad8cf-336">**sökvägen till toolist hello lagring via högerklickar du på**</span><span class="sxs-lookup"><span data-stu-id="ad8cf-336">**toolist hello storage path through right-click**</span></span>

1.  <span data-ttu-id="ad8cf-337">toopreview en fil, högerklicka på hello filsökväg.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-337">toopreview a file, right-click hello file path.</span></span>

   ![Data Lake-verktyg för Visual Studio Code högerklickar du på snabbmenyn](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-right-click-preview.png) 

2.  <span data-ttu-id="ad8cf-339">Välj ett konto från hello lokal sökväg eller ett Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-339">Select an account from hello local path or a Data Lake Analytics account.</span></span>

       ![Data Lake-verktyg för Visual Studio Code välja ett konto](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account.png)

3.  <span data-ttu-id="ad8cf-341">Resultat: VS-kod visar hello förhandsvisning av resultat av hello-fil.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-341">Results: VS Code displays hello preview results of hello file.</span></span>

       ![Data Lake-verktyg för Visual Studio Code förhandsgranska filen resultat](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-preview-results.png)

### <a name="upload-a-file"></a><span data-ttu-id="ad8cf-343">Överför en fil</span><span class="sxs-lookup"><span data-stu-id="ad8cf-343">Upload a file</span></span> 

<span data-ttu-id="ad8cf-344">Du kan ladda upp filer genom att ange hello kommandon **ADL: ladda upp filen** eller **ADL: ladda upp filen via konfiguration**.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-344">You can upload files by entering hello commands **ADL: Upload File** or **ADL: Upload File through Configuration**.</span></span>

<span data-ttu-id="ad8cf-345">**tooupload filer men hello ADL: ladda upp fil (kommando)**</span><span class="sxs-lookup"><span data-stu-id="ad8cf-345">**tooupload files though hello ADL: Upload File command**</span></span>
1. <span data-ttu-id="ad8cf-346">Välj Ctrl + Skift + P tooopen hello kommandot paletten eller högerklicka på hello Skriptredigeraren och ange sedan **överför filen**.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-346">Select Ctrl+Shift+P tooopen hello command palette or right-click hello script editor, and then enter **Upload File**.</span></span>
2.  <span data-ttu-id="ad8cf-347">tooupload hello fil, ange en lokal sökväg.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-347">tooupload hello file, enter a local path.</span></span>

    ![Data Lake-verktyg för Visual Studio Code ange lokal sökväg](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-input-local-path.png)

3. <span data-ttu-id="ad8cf-349">Välj en av hello sätt att sökvägen till lista hello lagring.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-349">Select one of hello ways of listing hello storage path.</span></span> <span data-ttu-id="ad8cf-350">Den här övergången använder **ange en sökväg** som exempel.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-350">This passage uses **Enter a path** as an example.</span></span>

    ![Data Lake-verktyg för Visual Studio Code lista lagringssökväg](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account-selectoneway.png)
    >[!NOTE]
    >- <span data-ttu-id="ad8cf-352">VS-koden behålls hello senast besökt sökväg i varje Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-352">VS Code keeps hello last-visited path in every Data Lake Analytics account.</span></span> <span data-ttu-id="ad8cf-353">Till exempel: ss tt /.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-353">For example: /tt/ss.</span></span>
    >- <span data-ttu-id="ad8cf-354">Webbläsaren från rotsökvägen: hello listan rotsökvägen från valda Data Lake Analytics-kontot eller en lokal sökväg.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-354">Browser from root path: hello list root path from your selected Data Lake Analytics account or a local path.</span></span>
    >- <span data-ttu-id="ad8cf-355">Ange en sökväg: visa en lista med en angiven sökväg från ditt valda Data Lake Analytics-konto eller en lokal sökväg.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-355">Enter a path: List a specified path from your selected Data Lake Analytics account or a local path.</span></span>

4. <span data-ttu-id="ad8cf-356">Välj ett konto från hello lokal sökväg eller ett Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-356">Select an account from hello local path or a Data Lake Analytics account.</span></span>

    ![Data Lake-verktyg för Visual Studio Code Högerklicka på lagring](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account.png)

5. <span data-ttu-id="ad8cf-358">Ange en sökväg till Azure storage.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-358">Enter an Azure storage path.</span></span> <span data-ttu-id="ad8cf-359">Till exempel: / utdata.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-359">For example: /output.</span></span>

       ![Data Lake Tools for Visual Studio Code enter storage path](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-input-preview-file.png)

6. <span data-ttu-id="ad8cf-360">Hitta din Azure storage-sökväg.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-360">Find your Azure storage path.</span></span> <span data-ttu-id="ad8cf-361">Välj **Välj aktuella mappen**.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-361">Select **Choose current folder**.</span></span>

    ![Välj en mapp för data Lake-verktyg för Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-choose-current-folder.png)

7.  <span data-ttu-id="ad8cf-363">Resultat: hello **utdata** hello filen Överföringsstatusen visas i fönstret.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-363">Results: hello **Output** window displays hello file upload status.</span></span>

       ![Data Lake-verktyg för Visual Studio Code överför status](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-upload-status.png)    

<span data-ttu-id="ad8cf-365">**tooupload filer men hello ADL: ladda upp filen via Konfigurationskommando**</span><span class="sxs-lookup"><span data-stu-id="ad8cf-365">**tooupload files though hello ADL: Upload File through Configuration command**</span></span>
1.  <span data-ttu-id="ad8cf-366">Välj Ctrl + Skift + P tooopen hello kommandot paletten eller högerklicka på hello Skriptredigeraren och ange sedan **överför filen via konfiguration**.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-366">Select Ctrl+Shift+P tooopen hello command palette or right-click hello script editor, and then enter **Upload File through Configuration**.</span></span>
2.  <span data-ttu-id="ad8cf-367">VS-kod visar en JSON-fil.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-367">VS Code displays a JSON file.</span></span> <span data-ttu-id="ad8cf-368">Du kan ange sökvägar och överför flera filer på hello samtidigt.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-368">You can enter file paths and upload multiple files at hello same time.</span></span> <span data-ttu-id="ad8cf-369">Instruktioner visas i hello **utdata** fönster.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-369">Instructions are displayed in hello **Output** window.</span></span> <span data-ttu-id="ad8cf-370">tooproceed tooupload hello fil spara (Ctrl + S) hello JSON-fil.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-370">tooproceed tooupload hello file, save (Ctrl+S) hello JSON file.</span></span>

       ![Data Lake-verktyg för Visual Studio Code filsökväg](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-upload-file.png)

3.  <span data-ttu-id="ad8cf-372">Resultat: hello **utdata** hello filen Överföringsstatusen visas i fönstret.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-372">Results: hello **Output** window displays hello file upload status.</span></span>

       ![Data Lake-verktyg för Visual Studio Code överför status](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-upload-status.png)     

<span data-ttu-id="ad8cf-374">Ett annat sätt tooupload toostorage en fil är via hello högerklickar du på menyn på hello filens fullständiga sökväg eller hello relativa sökvägen i hello Skriptredigeraren.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-374">Another way tooupload a file toostorage is through hello right-click menu on hello file's full path or hello file's relative path in hello script editor.</span></span> <span data-ttu-id="ad8cf-375">Ange hello lokal filsökväg och välj hello-konto.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-375">Enter hello local file path, and then select hello account.</span></span> <span data-ttu-id="ad8cf-376">Hej **utdata** hello Överföringsstatusen visas i fönstret.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-376">hello **Output** window displays hello upload status.</span></span> 

### <a name="open-azure-storage-explorer"></a><span data-ttu-id="ad8cf-377">Öppna Azure Lagringsutforskaren</span><span class="sxs-lookup"><span data-stu-id="ad8cf-377">Open Azure Storage Explorer</span></span>
<span data-ttu-id="ad8cf-378">Du kan öppna **Azure Lagringsutforskaren** genom att ange hello kommando **ADL: öppna Lagringsutforskaren för Web Azure** eller genom att välja den hello genom att högerklicka på snabbmenyn.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-378">You can open **Azure Storage Explorer** by entering hello command **ADL: Open Web Azure Storage Explorer** or by selecting it from hello right-click context menu.</span></span>

<span data-ttu-id="ad8cf-379">**tooopen Azure Lagringsutforskaren**</span><span class="sxs-lookup"><span data-stu-id="ad8cf-379">**tooopen Azure Storage Explorer**</span></span>

1. <span data-ttu-id="ad8cf-380">Välj Ctrl + Skift + P tooopen hello kommandot palett.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-380">Select Ctrl+Shift+P tooopen hello command palette.</span></span>
2. <span data-ttu-id="ad8cf-381">Ange **öppna Web Azure Lagringsutforskaren** eller högerklicka på en relativ eller hello fullständiga sökväg i hello Skriptredigeraren och välj sedan **öppna Web Azure Lagringsutforskaren**.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-381">Enter **Open Web Azure Storage Explorer** or right-click on a relative path or hello full path in hello script editor, and then select **Open Web Azure Storage Explorer**.</span></span>
3. <span data-ttu-id="ad8cf-382">Välj ett Data Lake Analytics-konto.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-382">Select a Data Lake Analytics account.</span></span>

<span data-ttu-id="ad8cf-383">Data Lake-verktyg öppnar hello Azure storage-sökvägen i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-383">Data Lake Tools opens hello Azure storage path in hello Azure portal.</span></span> <span data-ttu-id="ad8cf-384">Du hittar filen för hello-sökväg och förhandsgranska hello från hello webbserver.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-384">You can find hello path and preview hello file from hello web.</span></span>

### <a name="local-run-and-local-debug-for-windows-users"></a><span data-ttu-id="ad8cf-385">Lokal körning och lokala debug för Windows användare</span><span class="sxs-lookup"><span data-stu-id="ad8cf-385">Local run and local debug for Windows users</span></span>
<span data-ttu-id="ad8cf-386">Lokal U-SQL kör testar dina lokala data och verifierar ditt skript lokalt, innan koden är publicerade tooData Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-386">U-SQL local run tests your local data and validates your script locally, before your code is published tooData Lake Analytics.</span></span> <span data-ttu-id="ad8cf-387">hello lokala felsökning kan du toocomplete hello följande uppgifter innan din kod har skickats tooData Lake Analytics:</span><span class="sxs-lookup"><span data-stu-id="ad8cf-387">hello local debug feature enables you toocomplete hello following tasks before your code is submitted tooData Lake Analytics:</span></span> 
- <span data-ttu-id="ad8cf-388">Felsöka ditt C# bakomliggande kod.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-388">Debug your C# code-behind.</span></span> 
- <span data-ttu-id="ad8cf-389">Gå igenom hello kod.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-389">Step through hello code.</span></span> 
- <span data-ttu-id="ad8cf-390">Validera skriptet lokalt.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-390">Validate your script locally.</span></span>

<span data-ttu-id="ad8cf-391">Anvisningar för lokal körning och lokala debug finns [lokal U-SQL kör och lokala debug med Visual Studio Code](data-lake-tools-for-vscode-local-run-and-debug.md).</span><span class="sxs-lookup"><span data-stu-id="ad8cf-391">For instructions on local run and local debug, see [U-SQL local run and local debug with Visual Studio Code](data-lake-tools-for-vscode-local-run-and-debug.md).</span></span>

## <a name="additional-features"></a><span data-ttu-id="ad8cf-392">Ytterligare funktioner</span><span class="sxs-lookup"><span data-stu-id="ad8cf-392">Additional features</span></span>

<span data-ttu-id="ad8cf-393">Data Lake-verktyg för VS kod stöder hello följande funktioner:</span><span class="sxs-lookup"><span data-stu-id="ad8cf-393">Data Lake Tools for VS Code supports hello following features:</span></span>

-   <span data-ttu-id="ad8cf-394">IntelliSense automatisk komplettering: förslag visas i popup-fönster runt objekt, till exempel nyckelord, metoder och variabler.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-394">IntelliSense auto-complete: Suggestions appear in pop-up windows around items, such as keywords, methods, and variables.</span></span> <span data-ttu-id="ad8cf-395">Olika ikoner representerar olika typer av hello objekt:</span><span class="sxs-lookup"><span data-stu-id="ad8cf-395">Different icons represent different types of hello objects:</span></span>

    - <span data-ttu-id="ad8cf-396">Scala-datatyp</span><span class="sxs-lookup"><span data-stu-id="ad8cf-396">Scala data type</span></span>
    - <span data-ttu-id="ad8cf-397">Komplex datatyp</span><span class="sxs-lookup"><span data-stu-id="ad8cf-397">Complex data type</span></span>
    - <span data-ttu-id="ad8cf-398">Inbyggda UDT-typer</span><span class="sxs-lookup"><span data-stu-id="ad8cf-398">Built-in UDTs</span></span>
    - <span data-ttu-id="ad8cf-399">.NET-samling och klasser</span><span class="sxs-lookup"><span data-stu-id="ad8cf-399">.NET collection and classes</span></span>
    - <span data-ttu-id="ad8cf-400">C#-uttryck</span><span class="sxs-lookup"><span data-stu-id="ad8cf-400">C# expressions</span></span>
    - <span data-ttu-id="ad8cf-401">Inbyggda C# UDF: er och UDO UDAAGs</span><span class="sxs-lookup"><span data-stu-id="ad8cf-401">Built-in C# UDFs, UDOs, and UDAAGs</span></span> 
    - <span data-ttu-id="ad8cf-402">U-SQL-funktioner</span><span class="sxs-lookup"><span data-stu-id="ad8cf-402">U-SQL functions</span></span>
    - <span data-ttu-id="ad8cf-403">U-SQL-fönsterfunktion</span><span class="sxs-lookup"><span data-stu-id="ad8cf-403">U-SQL windowing function</span></span>
 
    ![Data Lake-verktyg för Visual Studio Code IntelliSense objekttyper](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-objects.png)
 
-   <span data-ttu-id="ad8cf-405">IntelliSense automatisk komplettering på Data Lake Analytics metadata: Data Lake-verktyg hämtar hello Data Lake Analytics metadatainformation lokalt.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-405">IntelliSense auto-complete on Data Lake Analytics metadata: Data Lake Tools downloads hello Data Lake Analytics metadata information locally.</span></span> <span data-ttu-id="ad8cf-406">hello IntelliSense funktionen fyller automatiskt objekt, inklusive hello-databasen, schemat, tabell, visa, tabellvärdesfunktion, procedurer och C#-sammansättningar, hello Data Lake Analytics metadata från.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-406">hello IntelliSense feature automatically populates objects, including hello database, schema, table, view, table-valued function, procedures, and C# assemblies, from hello Data Lake Analytics metadata.</span></span>
 
    ![Data Lake-verktyg för Visual Studio Code IntelliSense metadata](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-metastore.png)

-   <span data-ttu-id="ad8cf-408">IntelliSense fel markör: Data Lake-verktyg understryker hello redigering fel för U-SQL och C#.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-408">IntelliSense error marker: Data Lake Tools underlines hello editing errors for U-SQL and C#.</span></span> 
-   <span data-ttu-id="ad8cf-409">Visar syntax: Data Lake-verktyg som använder olika färger toodifferentiate objekt, till exempel variabler, nyckelord, datatyp och funktioner.</span><span class="sxs-lookup"><span data-stu-id="ad8cf-409">Syntax highlights: Data Lake Tools uses different colors toodifferentiate items, such as variables, keywords, data type, and functions.</span></span> 

    ![Data Lake-verktyg för Visual Studio Code syntax visar](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-syntax-highlights.png)

## <a name="next-steps"></a><span data-ttu-id="ad8cf-411">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ad8cf-411">Next steps</span></span>

- <span data-ttu-id="ad8cf-412">Lokal U-SQL-körning och lokala debug med Visual Studio Code finns [lokal U-SQL kör och lokala debug med Visual Studio Code](data-lake-tools-for-vscode-local-run-and-debug.md).</span><span class="sxs-lookup"><span data-stu-id="ad8cf-412">For U-SQL local run and local debug with Visual Studio Code, see [U-SQL local run and local debug with Visual Studio Code](data-lake-tools-for-vscode-local-run-and-debug.md).</span></span>
- <span data-ttu-id="ad8cf-413">Kom igång-information på Data Lake Analytics finns [Självstudier: Kom igång med Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="ad8cf-413">For getting-started information on Data Lake Analytics, see [Tutorial: Get started with Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md).</span></span>
- <span data-ttu-id="ad8cf-414">Information om Data Lake-verktyg för Visual Studio finns [Självstudier: utveckla U-SQL-skript med hjälp av Data Lake-verktyg för Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="ad8cf-414">For information about Data Lake Tools for Visual Studio, see [Tutorial: Develop U-SQL scripts by using Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span>
- <span data-ttu-id="ad8cf-415">Information om hur du utvecklar sammansättningar finns [utveckla U-SQL-sammansättningar för Azure Data Lake Analytics-jobb](data-lake-analytics-u-sql-develop-assemblies.md).</span><span class="sxs-lookup"><span data-stu-id="ad8cf-415">For information on developing assemblies, see [Develop U-SQL assemblies for Azure Data Lake Analytics jobs](data-lake-analytics-u-sql-develop-assemblies.md).</span></span>



