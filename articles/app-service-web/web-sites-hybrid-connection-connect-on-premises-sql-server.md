---
title: "Ansluta till lokal SQL Server från en webbapp i Azure App Service med Hybridanslutningar"
description: Skapa en webbapp i Microsoft Azure och koppla den till en lokal SQL Server-databas
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: mollybos
ms.assetid: 2b4e0539-1a0b-4aa1-8a69-b4b053c3b2e5
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2016
ms.author: cephalin
ms.openlocfilehash: 12456ef3e2aecfa7a03cca97de2ff6ffd9602357
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="connect-to-on-premises-sql-server-from-a-web-app-in-azure-app-service-using-hybrid-connections"></a><span data-ttu-id="22025-103">Ansluta till lokal SQL Server från en webbapp i Azure App Service med Hybridanslutningar</span><span class="sxs-lookup"><span data-stu-id="22025-103">Connect to on-premises SQL Server from a web app in Azure App Service using Hybrid Connections</span></span>
<span data-ttu-id="22025-104">Hybridanslutningar kan ansluta [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps till lokala resurser som använder en statisk TCP-port.</span><span class="sxs-lookup"><span data-stu-id="22025-104">Hybrid Connections can connect [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps to on-premises resources that use a static TCP port.</span></span> <span data-ttu-id="22025-105">Resurser som stöds är Microsoft SQL Server, MySQL, http-webb-API: er, användning och de flesta anpassade webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="22025-105">Supported resources include Microsoft SQL Server, MySQL, HTTP Web APIs, App Service, and most custom Web Services.</span></span>

<span data-ttu-id="22025-106">I kursen får du lära dig hur du skapar en App Service webbapp i den [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715), ansluta webbappen till lokala lokala SQL Server-databasen med hjälp av funktionen Hybridanslutning, skapa ett enkelt ASP.NET-program som ska använda hybridanslutning och distribuerar program till App Service webbapp.</span><span class="sxs-lookup"><span data-stu-id="22025-106">In this tutorial, you will learn how to create an App Service web app in the [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715), connect the web app to your local on-premises SQL Server database using the new Hybrid Connection feature, create a simple ASP.NET application that will use the hybrid connection, and deploy the application to the App Service web app.</span></span> <span data-ttu-id="22025-107">Slutförda webbappen i Azure lagrar autentiseringsuppgifter i en databas för medlemskap som är lokalt.</span><span class="sxs-lookup"><span data-stu-id="22025-107">The completed web app on Azure stores user credentials in a membership database that is on-premises.</span></span> <span data-ttu-id="22025-108">Kursen riktar sig till några tidigare erfarenheter med hjälp av Azure eller ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="22025-108">The tutorial assumes no prior experience using Azure or ASP.NET.</span></span>

> [!NOTE]
> <span data-ttu-id="22025-109">Om du vill komma igång med Azure Apptjänst innan du registrerar dig för ett Azure-konto kan du gå till [Prova Apptjänst](https://azure.microsoft.com/try/app-service/). Där kan du direkt skapa en tillfällig startwebbapp i Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="22025-109">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="22025-110">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="22025-110">No credit cards required; no commitments.</span></span>
> 
> <span data-ttu-id="22025-111">Web Apps-delen av funktionen Hybridanslutningar är endast tillgänglig i den [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="22025-111">The Web Apps portion of the Hybrid Connections feature is available only in the [Azure Portal](https://portal.azure.com).</span></span> <span data-ttu-id="22025-112">Om du vill skapa en anslutning i BizTalk-tjänst finns [Hybridanslutningar](http://go.microsoft.com/fwlink/p/?LinkID=397274).</span><span class="sxs-lookup"><span data-stu-id="22025-112">To create a connection in BizTalk Services, see [Hybrid Connections](http://go.microsoft.com/fwlink/p/?LinkID=397274).</span></span>  
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="22025-113">Krav</span><span class="sxs-lookup"><span data-stu-id="22025-113">Prerequisites</span></span>
<span data-ttu-id="22025-114">Den här kursen behöver du följande produkter.</span><span class="sxs-lookup"><span data-stu-id="22025-114">To complete this tutorial, you'll need the following products.</span></span> <span data-ttu-id="22025-115">Alla är tillgängliga i ledigt versioner, så du kan börja utveckla för Azure och helt kostnadsfritt.</span><span class="sxs-lookup"><span data-stu-id="22025-115">All are available in free versions, so you can start developing for Azure entirely for free.</span></span>

* <span data-ttu-id="22025-116">**Azure-prenumeration** – för en kostnadsfri prenumeration finns [kostnadsfri utvärderingsversion av Azure](/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="22025-116">**Azure subscription** - For a free subscription, see [Azure Free Trial](/pricing/free-trial/).</span></span>
* <span data-ttu-id="22025-117">**Visual Studio 2013** – om du vill hämta en kostnadsfri utvärderingsversion av Visual Studio 2013, se [Visual Studio-hämtningar](http://www.visualstudio.com/downloads/download-visual-studio-vs).</span><span class="sxs-lookup"><span data-stu-id="22025-117">**Visual Studio 2013** - To download a free trial version of Visual Studio 2013, see [Visual Studio Downloads](http://www.visualstudio.com/downloads/download-visual-studio-vs).</span></span> <span data-ttu-id="22025-118">Installera detta innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="22025-118">Install this before continuing.</span></span>
* <span data-ttu-id="22025-119">**Microsoft .NET Framework 3.5 servicepack 1** -om operativsystemet är Windows 8.1, Windows Server 2012 R2, Windows 8, Windows Server 2012, Windows 7 eller Windows Server 2008 R2, kan du aktivera det på Kontrollpanelen > program och funktioner > Aktivera Windows-funktioner på eller stänga av.</span><span class="sxs-lookup"><span data-stu-id="22025-119">**Microsoft .NET Framework 3.5 Service Pack 1** - If your operating system is Windows 8.1, Windows Server 2012 R2, Windows 8, Windows Server 2012, Windows 7, or Windows Server 2008 R2, you can enable this in Control Panel > Programs and Features > Turn Windows features on or off.</span></span> <span data-ttu-id="22025-120">Annars kan du hämta det från den [Microsoft Download Center](http://www.microsoft.com/download/en/details.aspx?displaylang=en&id=22).</span><span class="sxs-lookup"><span data-stu-id="22025-120">Otherwise, you can download it from the [Microsoft Download Center](http://www.microsoft.com/download/en/details.aspx?displaylang=en&id=22).</span></span>
* <span data-ttu-id="22025-121">**SQL Server 2014 Express med verktyg** -hämta Microsoft SQL Server Express kostnadsfritt på den [sidan Microsoft Web Platform databas](http://www.microsoft.com/web/platform/database.aspx).</span><span class="sxs-lookup"><span data-stu-id="22025-121">**SQL Server 2014 Express with Tools** - download Microsoft SQL Server Express for free at the [Microsoft Web Platform Database page](http://www.microsoft.com/web/platform/database.aspx).</span></span> <span data-ttu-id="22025-122">Välj den **Express** (inte LocalDB) version.</span><span class="sxs-lookup"><span data-stu-id="22025-122">Choose the **Express** (not LocalDB) version.</span></span> <span data-ttu-id="22025-123">Den **Express med verktyg** version omfattar SQL Server Management Studio, som du ska använda i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="22025-123">The **Express with Tools** version includes SQL Server Management Studio, which you will use in this tutorial.</span></span>
* <span data-ttu-id="22025-124">**SQL Server Management Studio Express** - detta ingår i SQL Server 2014 Express verktyg hämta som nämns ovan, men om du måste installera separat, du kan hämta och installera den från den [hämtningssidan för SQL Server Express](http://www.microsoft.com/web/platform/database.aspx).</span><span class="sxs-lookup"><span data-stu-id="22025-124">**SQL Server Management Studio Express** - This is included with the SQL Server 2014 Express with Tools download mentioned above, but if you need to install it separately, you can download and install it from the [SQL Server Express download page](http://www.microsoft.com/web/platform/database.aspx).</span></span>

<span data-ttu-id="22025-125">Kursen förutsätter att du har en Azure-prenumeration, att du har installerat Visual Studio 2013 och att du har installerat eller aktiverat .NET Framework 3.5.</span><span class="sxs-lookup"><span data-stu-id="22025-125">The tutorial assumes that you have an Azure subscription, that you have installed Visual Studio 2013, and that you have installed or enabled .NET Framework 3.5.</span></span> <span data-ttu-id="22025-126">Kursen visar hur du installerar SQL Server 2014 Express i en konfiguration som fungerar med funktionen Azure Hybridanslutningar (en standardinstans med en statisk TCP-port).</span><span class="sxs-lookup"><span data-stu-id="22025-126">The tutorial shows you how to install SQL Server 2014 Express in a configuration that works well with the Azure Hybrid Connections feature (a default instance with a static TCP port).</span></span> <span data-ttu-id="22025-127">Hämta SQL Server 2014 Express med verktyg från den plats som nämns ovan om du inte installerade SQL Server innan du påbörjar självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="22025-127">Before starting the tutorial, download SQL Server 2014 Express with Tools from the location mentioned above if you do not have SQL Server installed.</span></span>

### <a name="notes"></a><span data-ttu-id="22025-128">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="22025-128">Notes</span></span>
<span data-ttu-id="22025-129">Om du vill använda en lokal SQL Server eller SQL Server Express-databas med en hybridanslutning måste TCP/IP vara aktiverat på en statisk port.</span><span class="sxs-lookup"><span data-stu-id="22025-129">To use an on-premises SQL Server or SQL Server Express database with a hybrid connection, TCP/IP needs to be enabled on a static port.</span></span> <span data-ttu-id="22025-130">Standardinstanser på SQL Server använder statisk port 1433, inte namngivna instanser.</span><span class="sxs-lookup"><span data-stu-id="22025-130">Default instances on SQL Server use static port 1433, whereas named instances do not.</span></span>

<span data-ttu-id="22025-131">Datorn där du installerar lokal Hybridanslutningshanterare agenten:</span><span class="sxs-lookup"><span data-stu-id="22025-131">The computer on which you install the on-premises Hybrid Connection Manager agent:</span></span>

* <span data-ttu-id="22025-132">Måste ha en utgående anslutning till Azure via:</span><span class="sxs-lookup"><span data-stu-id="22025-132">Must have outbound connectivity to Azure over:</span></span>

| <span data-ttu-id="22025-133">Port</span><span class="sxs-lookup"><span data-stu-id="22025-133">Port</span></span> | <span data-ttu-id="22025-134">Varför</span><span class="sxs-lookup"><span data-stu-id="22025-134">Why</span></span> |
| --- | --- |
| <span data-ttu-id="22025-135">80</span><span class="sxs-lookup"><span data-stu-id="22025-135">80</span></span> |<span data-ttu-id="22025-136">**Krävs** för HTTP-porten för valideringen av servercertifikatet och eventuellt för dataanslutning.</span><span class="sxs-lookup"><span data-stu-id="22025-136">**Required** for HTTP port for certificate validation and optionally for data connectivity.</span></span> |
| <span data-ttu-id="22025-137">443</span><span class="sxs-lookup"><span data-stu-id="22025-137">443</span></span> |<span data-ttu-id="22025-138">**Valfria** för dataanslutning.</span><span class="sxs-lookup"><span data-stu-id="22025-138">**Optional** for data connectivity.</span></span> <span data-ttu-id="22025-139">Om utgående anslutning till 443 är tillgänglig, används TCP-port 80.</span><span class="sxs-lookup"><span data-stu-id="22025-139">If outbound connectivity to 443 is unavailable, TCP port 80 is used.</span></span> |
| <span data-ttu-id="22025-140">5671 och 9352</span><span class="sxs-lookup"><span data-stu-id="22025-140">5671 and 9352</span></span> |<span data-ttu-id="22025-141">**Rekommenderade** men valfritt för dataanslutning.</span><span class="sxs-lookup"><span data-stu-id="22025-141">**Recommended** but Optional for data connectivity.</span></span> <span data-ttu-id="22025-142">Observera att det här läget vanligtvis ger högre genomströmning.</span><span class="sxs-lookup"><span data-stu-id="22025-142">Note this mode usually yields higher throughput.</span></span> <span data-ttu-id="22025-143">Om utgående anslutning till dessa portar är tillgänglig, används TCP-port 443.</span><span class="sxs-lookup"><span data-stu-id="22025-143">If outbound connectivity to these ports is unavailable, TCP port 443 is used.</span></span> |

* <span data-ttu-id="22025-144">Måste kunna nå den *värdnamn*:*portnumber* på din lokala resursen.</span><span class="sxs-lookup"><span data-stu-id="22025-144">Must be able to reach the *hostname*:*portnumber* of your on-premises resource.</span></span>

<span data-ttu-id="22025-145">Stegen i den här artikeln förutsätter att du använder webbläsaren från den dator som är värd för agenten lokalt hybrid-anslutning.</span><span class="sxs-lookup"><span data-stu-id="22025-145">The steps in this article assume that you are using the browser from the computer that will host the on-premises hybrid connection agent.</span></span>

<span data-ttu-id="22025-146">Om du redan har SQL Server installeras i en konfiguration och i en miljö som uppfyller de villkor som beskrivs ovan kan du gå vidare och börja med [och skapa en SQL Server-databas lokalt](#CreateSQLDB).</span><span class="sxs-lookup"><span data-stu-id="22025-146">If you already have SQL Server installed in a configuration and in an environment that meets the conditions described above, you can skip ahead and start with [Create a SQL Server database on-premises](#CreateSQLDB).</span></span>

<a name="InstallSQL"></a>

## <a name="a-install-sql-server-express-enable-tcpip-and-create-a-sql-server-database-on-premises"></a><span data-ttu-id="22025-147">A.</span><span class="sxs-lookup"><span data-stu-id="22025-147">A.</span></span> <span data-ttu-id="22025-148">Installera SQL Server Express, aktivera TCP/IP och skapa en SQL Server-databas lokalt</span><span class="sxs-lookup"><span data-stu-id="22025-148">Install SQL Server Express, enable TCP/IP, and create a SQL Server database on-premises</span></span>
<span data-ttu-id="22025-149">Det här avsnittet visar hur du installerar SQL Server Express, aktivera TCP/IP och skapa en databas så att webbprogrammet fungerar med Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="22025-149">This section shows you how to install SQL Server Express, enable TCP/IP, and create a database so that your web application will work with the Azure Portal.</span></span>

### <a name="install-sql-server-express"></a><span data-ttu-id="22025-150">Installera SQL Server Express</span><span class="sxs-lookup"><span data-stu-id="22025-150">Install SQL Server Express</span></span>
1. <span data-ttu-id="22025-151">Installera SQL Server Express, köra den **SQLEXPRWT_x64_ENU.exe** eller **SQLEXPR_x86_ENU.exe** filen som du hämtade.</span><span class="sxs-lookup"><span data-stu-id="22025-151">To install SQL Server Express, run the **SQLEXPRWT_x64_ENU.exe** or **SQLEXPR_x86_ENU.exe** file that you downloaded.</span></span> <span data-ttu-id="22025-152">Guiden för SQL Server Installationscenter visas.</span><span class="sxs-lookup"><span data-stu-id="22025-152">The SQL Server Installation Center wizard appears.</span></span>
   
    ![SQL Server-installation][SQLServerInstall]
2. <span data-ttu-id="22025-154">Välj **fristående nya SQL Server-installation eller Lägg till funktioner i en befintlig installation**.</span><span class="sxs-lookup"><span data-stu-id="22025-154">Choose **New SQL Server stand-alone installation or add features to an existing installation**.</span></span> <span data-ttu-id="22025-155">Följ instruktionerna, accepterar du standardalternativen och inställningar, tills du kommer till den **instanskonfiguration** sidan.</span><span class="sxs-lookup"><span data-stu-id="22025-155">Follow the instructions, accepting the default choices and settings, until you get to the **Instance Configuration** page.</span></span>
3. <span data-ttu-id="22025-156">På den **instanskonfiguration** väljer **standardinstansen**.</span><span class="sxs-lookup"><span data-stu-id="22025-156">On the **Instance Configuration** page, choose **Default instance**.</span></span>
   
    ![Välj standardinstansen][ChooseDefaultInstance]
   
    <span data-ttu-id="22025-158">Som standard lyssnar standardinstansen av SQL Server för begäranden från SQL Server-klienter på statisk port 1433, vilket är Hybridanslutningar-funktionen ska fungera.</span><span class="sxs-lookup"><span data-stu-id="22025-158">By default, the default instance of SQL Server listens for requests from SQL Server clients on static port 1433, which is what the Hybrid Connections feature requires.</span></span> <span data-ttu-id="22025-159">Namngivna instanser använder dynamiska portar och UDP, vilket inte stöds av Hybridanslutningar.</span><span class="sxs-lookup"><span data-stu-id="22025-159">Named instances use dynamic ports and UDP, which are not supported by Hybrid Connections.</span></span>
4. <span data-ttu-id="22025-160">Godkänn standardinställningarna på den **serverkonfiguration** sidan.</span><span class="sxs-lookup"><span data-stu-id="22025-160">Accept the defaults on the **Server Configuration** page.</span></span>
5. <span data-ttu-id="22025-161">På den **konfiguration av databasmotor** sidan under **autentiseringsläge**, Välj **blandat läge (SQL Server-autentisering och Windows-autentisering)**, och ange ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="22025-161">On the **Database Engine Configuration** page, under **Authentication Mode**, choose **Mixed Mode (SQL Server authentication and Windows authentication)**, and provide a password.</span></span>
   
    ![Välj blandat läge][ChooseMixedMode]
   
    <span data-ttu-id="22025-163">I den här självstudiekursen kommer du att använda SQL Server-autentisering.</span><span class="sxs-lookup"><span data-stu-id="22025-163">In this tutorial, you will be using SQL Server authentication.</span></span> <span data-ttu-id="22025-164">Glöm inte att komma ihåg det lösenord som du anger, eftersom du behöver senare.</span><span class="sxs-lookup"><span data-stu-id="22025-164">Be sure to remember the password that you provide, because you will need it later.</span></span>
6. <span data-ttu-id="22025-165">Gå igenom resten av guiden för att slutföra installationen.</span><span class="sxs-lookup"><span data-stu-id="22025-165">Step through the rest of the wizard to complete the installation.</span></span>

### <a name="enable-tcpip"></a><span data-ttu-id="22025-166">Aktivera TCP/IP</span><span class="sxs-lookup"><span data-stu-id="22025-166">Enable TCP/IP</span></span>
<span data-ttu-id="22025-167">Om du vill aktivera TCP/IP, använder du SQL Server Configuration Manager, som installerades när du installerade SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="22025-167">To enable TCP/IP, you will use SQL Server Configuration Manager, which was installed when you installed SQL Server Express.</span></span> <span data-ttu-id="22025-168">Följ stegen i [aktivera nätverksprotokoll för TCP/IP för SQL Server](http://technet.microsoft.com/library/hh231672%28v=sql.110%29.aspx) innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="22025-168">Follow the steps in [Enable TCP/IP Network Protocol for SQL Server](http://technet.microsoft.com/library/hh231672%28v=sql.110%29.aspx) before continuing.</span></span>

<a name="CreateSQLDB"></a>

### <a name="create-a-sql-server-database-on-premises"></a><span data-ttu-id="22025-169">Skapa en SQL Server-databas lokalt</span><span class="sxs-lookup"><span data-stu-id="22025-169">Create a SQL Server database on-premises</span></span>
<span data-ttu-id="22025-170">Ditt webbprogram för Visual Studio kräver en databas för medlemskap som kan nås av Azure.</span><span class="sxs-lookup"><span data-stu-id="22025-170">Your Visual Studio web application requires a membership database that can be accessed by Azure.</span></span> <span data-ttu-id="22025-171">Detta kräver en SQL Server eller SQL Server Express-databas (inte LocalDB-databasen som MVC-mallen använder som standard), så ska du skapa medlemskapsdatabasen nästa.</span><span class="sxs-lookup"><span data-stu-id="22025-171">This requires a SQL Server or SQL Server Express database (not the LocalDB database that the MVC template uses by default), so you'll create the membership database next.</span></span>

1. <span data-ttu-id="22025-172">Anslut till SQL Server som du precis har installerat i SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="22025-172">In SQL Server Management Studio, connect to the SQL Server you just installed.</span></span> <span data-ttu-id="22025-173">(Om den **Anslut till Server** dialogrutan inte visas automatiskt, gå till **Object Explorer** i den vänstra rutan klickar du på **Anslut**, och klicka sedan på **databasmotorn**.) ![Ansluta till servern][SSMSConnectToServer]</span><span class="sxs-lookup"><span data-stu-id="22025-173">(If the **Connect to Server** dialog does not appear automatically, navigate to **Object Explorer** in the left pane, click **Connect**, and then click **Database Engine**.) ![Connect to Server][SSMSConnectToServer]</span></span>
   
    <span data-ttu-id="22025-174">För **servertyp**, Välj **databasmotorn**.</span><span class="sxs-lookup"><span data-stu-id="22025-174">For **Server type**, choose **Database Engine**.</span></span> <span data-ttu-id="22025-175">För **servernamn**, kan du använda **localhost** eller namnet på den dator som du använder.</span><span class="sxs-lookup"><span data-stu-id="22025-175">For **Server name**, you can use **localhost** or the name of the computer that you are using.</span></span> <span data-ttu-id="22025-176">Välj **SQL Server-autentisering**, och sedan logga in med sa-användarnamnet och lösenordet som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="22025-176">Choose **SQL Server authentication**, and then log in with the sa user name and the password that you created earlier.</span></span>
2. <span data-ttu-id="22025-177">Om du vill skapa en ny databas med hjälp av SQL Server Management Studio, högerklicka på **databaser** i Object Explorer och klicka sedan på **ny databas**.</span><span class="sxs-lookup"><span data-stu-id="22025-177">To create a new database by using SQL Server Management Studio, right-click **Databases** in Object Explorer, and then click **New Database**.</span></span>
   
    ![Skapa en ny databas][SSMScreateNewDB]
3. <span data-ttu-id="22025-179">I den **ny databas** dialogrutan Ange MembershipDB för databasens namn och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="22025-179">In the **New Database** dialog, enter MembershipDB for the database name, and then click **OK**.</span></span>
   
    ![Ange databasnamn][SSMSprovideDBname]
   
    <span data-ttu-id="22025-181">Observera att du inte göra några ändringar i databasen nu.</span><span class="sxs-lookup"><span data-stu-id="22025-181">Note that you do not make any changes to the database at this point.</span></span> <span data-ttu-id="22025-182">Information om gruppmedlemskap läggas till automatiskt senare av ditt webbprogram när den körs.</span><span class="sxs-lookup"><span data-stu-id="22025-182">The membership information will be added automatically later by your web application when you run it.</span></span>
4. <span data-ttu-id="22025-183">I Object Explorer, om du expanderar **databaser**, ser du att medlemskapsdatabasen har skapats.</span><span class="sxs-lookup"><span data-stu-id="22025-183">In Object Explorer, if you expand **Databases**, you will see that the membership database has been created.</span></span>
   
    ![MembershipDB skapas][SSMSMembershipDBCreated]

<a name="CreateSite"></a>

## <a name="b-create-a-web-app-in-the-azure-portal"></a><span data-ttu-id="22025-185">B.</span><span class="sxs-lookup"><span data-stu-id="22025-185">B.</span></span> <span data-ttu-id="22025-186">Skapa en webbapp i Azure Portal</span><span class="sxs-lookup"><span data-stu-id="22025-186">Create a web app in the Azure Portal</span></span>
> [!NOTE]
> <span data-ttu-id="22025-187">Om du redan har skapat en webbapp i Azure Portal som du vill använda för den här självstudiekursen, du kan gå vidare till [skapa en Hybrid-anslutning och en BizTalk Service](#CreateHC) och fortsätta därifrån.</span><span class="sxs-lookup"><span data-stu-id="22025-187">If you have already created a web app in the Azure Portal that you want to use for this tutorial, you can skip ahead to [Create a Hybrid Connection and a BizTalk Service](#CreateHC) and continue from there.</span></span>
> 
> 

1. <span data-ttu-id="22025-188">I den [Azure Portal](https://portal.azure.com), klickar du på **ny** > **webb + mobilt** > **webbapp**.</span><span class="sxs-lookup"><span data-stu-id="22025-188">In the [Azure Portal](https://portal.azure.com), click **New** > **Web + Mobile** > **Web app**.</span></span>
   
    ![Knappen Nytt][New]
2. <span data-ttu-id="22025-190">Konfigurera ditt webbprogram och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="22025-190">Configure your web app, and then click **Create**.</span></span>
   
    ![Namn på webbplats][WebsiteCreationBlade]
3. <span data-ttu-id="22025-192">Webbprogrammet har skapats och dess web app bladet visas efter en liten stund.</span><span class="sxs-lookup"><span data-stu-id="22025-192">After a few moments, the web app is created and its web app blade appears.</span></span> <span data-ttu-id="22025-193">Bladet är en lodrätt rullningsbara instrumentpanel där du kan hantera ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="22025-193">The blade is a vertically scrollable dashboard that lets you manage your web app.</span></span>
   
    ![Webbplatsen som kör][WebSiteRunningBlade]
   
    <span data-ttu-id="22025-195">För att verifiera webbappen är aktiv, kan du klicka på den **Bläddra** ikon för att visa sidan.</span><span class="sxs-lookup"><span data-stu-id="22025-195">To verify the web app is live, you can click the **Browse** icon to display the default page.</span></span>

<span data-ttu-id="22025-196">Därefter skapar du en hybridanslutning och BizTalk-tjänst för webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="22025-196">Next, you will create a hybrid connection and a BizTalk service for the web app.</span></span>

<a name="CreateHC"></a>

## <a name="c-create-a-hybrid-connection-and-a-biztalk-service"></a><span data-ttu-id="22025-197">C.</span><span class="sxs-lookup"><span data-stu-id="22025-197">C.</span></span> <span data-ttu-id="22025-198">Skapa en Hybridanslutning och BizTalk-tjänst</span><span class="sxs-lookup"><span data-stu-id="22025-198">Create a Hybrid Connection and a BizTalk Service</span></span>
1. <span data-ttu-id="22025-199">Tillbaka i portalen går du till inställningar och klickar på **nätverk** > **konfigurera slutpunkter för din hybridanslutning**.</span><span class="sxs-lookup"><span data-stu-id="22025-199">Back in the Portal, go to settings and click **Networking** > **Configure your hybrid connection endpoints**.</span></span>
   
    ![Hybridanslutningar][CreateHCHCIcon]
2. <span data-ttu-id="22025-201">Klicka på bladet Hybrid anslutningar **Lägg till** > **ny hybridanslutning**.</span><span class="sxs-lookup"><span data-stu-id="22025-201">On the Hybrid connections blade, click **Add** > **New hybrid connection**.</span></span>
3. <span data-ttu-id="22025-202">På den **skapa hybridanslutning** bladet:</span><span class="sxs-lookup"><span data-stu-id="22025-202">On the **Create hybrid connection** blade:</span></span>
   
   * <span data-ttu-id="22025-203">För **namn**, ange ett namn för anslutningen.</span><span class="sxs-lookup"><span data-stu-id="22025-203">For **Name**, provide a name for the connection.</span></span>
   * <span data-ttu-id="22025-204">För **värdnamn**, ange namnet på värddatorn för SQL Server.</span><span class="sxs-lookup"><span data-stu-id="22025-204">For **Hostname**, enter the computer name of your SQL Server host computer.</span></span>
   * <span data-ttu-id="22025-205">För **Port**, ange 1433 (standardporten för SQL Server).</span><span class="sxs-lookup"><span data-stu-id="22025-205">For **Port**, enter 1433 (the default port for SQL Server).</span></span>
   * <span data-ttu-id="22025-206">Klicka på **BizTalk Service** > **ny BizTalk-tjänst** och ange ett namn för BizTalk-tjänst.</span><span class="sxs-lookup"><span data-stu-id="22025-206">Click **BizTalk Service** > **New BizTalk Service** and enter a name for the BizTalk service.</span></span>
     
     ![Skapa en hybridanslutning][TwinCreateHCBlades]
4. <span data-ttu-id="22025-208">Klicka på **OK** två gånger.</span><span class="sxs-lookup"><span data-stu-id="22025-208">Click **OK** twice.</span></span>
   
    <span data-ttu-id="22025-209">När processen har slutförts i **meddelanden** område blinkar en grön **lyckade** och **hybridanslutning** ny hybridanslutning med status som visas i bladet **inte ansluten**.</span><span class="sxs-lookup"><span data-stu-id="22025-209">When the process completes, the **Notifications** area will flash a green **SUCCESS** and the **Hybrid connection** blade will show the new hybrid connection with the status as **Not connected**.</span></span>
   
    ![En hybridanslutning skapas][CreateHCOneConnectionCreated]

<span data-ttu-id="22025-211">Du har nu slutfört en viktig del av infrastrukturen för hybrid-anslutning.</span><span class="sxs-lookup"><span data-stu-id="22025-211">At this point, you have completed an important part of the cloud hybrid connection infrastructure.</span></span> <span data-ttu-id="22025-212">Därefter skapar du en motsvarande typ lokalt.</span><span class="sxs-lookup"><span data-stu-id="22025-212">Next, you will create a corresponding on-premises piece.</span></span>

<a name="InstallHCM"></a>

## <a name="d-install-the-on-premises-hybrid-connection-manager-to-complete-the-connection"></a><span data-ttu-id="22025-213">D.</span><span class="sxs-lookup"><span data-stu-id="22025-213">D.</span></span> <span data-ttu-id="22025-214">Installera lokal Hybridanslutningshanterare för att slutföra anslutningen</span><span class="sxs-lookup"><span data-stu-id="22025-214">Install the on-premises Hybrid Connection Manager to complete the connection</span></span>
[!INCLUDE [app-service-hybrid-connections-manager-install](../../includes/app-service-hybrid-connections-manager-install.md)]

<span data-ttu-id="22025-215">Anslutningen hybridinfrastruktur har slutförts, skapar du ett program som använder den.</span><span class="sxs-lookup"><span data-stu-id="22025-215">Now that the hybrid connection infrastructure is complete, you will create a web application that uses it.</span></span>

<a name="CreateASPNET"></a>

## <a name="e-create-a-basic-aspnet-web-project-edit-the-database-connection-string-and-run-the-project-locally"></a><span data-ttu-id="22025-216">E.</span><span class="sxs-lookup"><span data-stu-id="22025-216">E.</span></span> <span data-ttu-id="22025-217">Skapa ett grundläggande webbprojekt i ASP.NET, redigera anslutningssträng för databasen och köra projektet lokalt</span><span class="sxs-lookup"><span data-stu-id="22025-217">Create a basic ASP.NET web project, edit the database connection string, and run the project locally</span></span>
### <a name="create-a-basic-aspnet-project"></a><span data-ttu-id="22025-218">Skapa ett grundläggande ASP.NET-projekt</span><span class="sxs-lookup"><span data-stu-id="22025-218">Create a basic ASP.NET project</span></span>
1. <span data-ttu-id="22025-219">I Visual Studio på den **filen** menyn skapa ett nytt projekt:</span><span class="sxs-lookup"><span data-stu-id="22025-219">In Visual Studio, on the **File** menu, create a new Project:</span></span>
   
    ![Nytt projekt i Visual Studio][HCVSNewProject]
2. <span data-ttu-id="22025-221">I den **mallar** avsnitt i den **nytt projekt** markerar **Web** och välj **ASP.NET-webbprogram**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="22025-221">In the **Templates** section of the **New Project** dialog, select **Web** and choose **ASP.NET Web Application**, and then click **OK**.</span></span>
   
    ![Välj ASP.NET-webbprogram][HCVSChooseASPNET]
3. <span data-ttu-id="22025-223">I den **nytt ASP.NET-projekt** dialogrutan Välj **MVC**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="22025-223">In the **New ASP.NET Project** dialog, choose **MVC**, and then click **OK**.</span></span>
   
    ![Välja MVC][HCVSChooseMVC]
4. <span data-ttu-id="22025-225">När projektet har skapats visas appen viktig information på sidan.</span><span class="sxs-lookup"><span data-stu-id="22025-225">When the project has been created, the application readme page appears.</span></span> <span data-ttu-id="22025-226">Kör inte webbprojektet ännu.</span><span class="sxs-lookup"><span data-stu-id="22025-226">Do not run the web project yet.</span></span>
   
    ![Viktigt-sida][HCVSReadmePage]

### <a name="edit-the-database-connection-string-for-the-application"></a><span data-ttu-id="22025-228">Redigera anslutningssträng för databasen för programmet</span><span class="sxs-lookup"><span data-stu-id="22025-228">Edit the database connection string for the application</span></span>
<span data-ttu-id="22025-229">I det här steget kan redigera du den anslutningssträng som talar om programmet var du hittar den lokala SQL Server Express-databasen.</span><span class="sxs-lookup"><span data-stu-id="22025-229">In this step, you edit the connection string that tells your application where to find your local SQL Server Express database.</span></span> <span data-ttu-id="22025-230">Anslutningssträngen är i programmets Web.config-fil som innehåller konfigurationsinformation för programmet.</span><span class="sxs-lookup"><span data-stu-id="22025-230">The connection string is in the application's Web.config file, which contains configuration information for the application.</span></span>

> [!NOTE]
> <span data-ttu-id="22025-231">För att säkerställa att programmet använder databasen som du skapade i SQL Server Express och inte det i Visual Studio standard LocalDB, är det viktigt att du har slutfört det här steget innan du kör projektet.</span><span class="sxs-lookup"><span data-stu-id="22025-231">To ensure that your application uses the database that you created in SQL Server Express, and not the one in Visual Studio's default LocalDB, it is important that you complete this step before running your project.</span></span>
> 
> 

1. <span data-ttu-id="22025-232">Dubbelklicka på filen Web.config i Solution Explorer.</span><span class="sxs-lookup"><span data-stu-id="22025-232">In Solution Explorer, double-click the Web.config file.</span></span>
   
    ![Web.config][HCVSChooseWebConfig]
2. <span data-ttu-id="22025-234">Redigera den **connectionStrings** avsnittet att peka till SQL Server-databasen på din lokala dator enligt syntaxen i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="22025-234">Edit the **connectionStrings** section to point to the SQL Server database on your local machine, following the syntax in the following example:</span></span>
   
    ![Anslutningssträng][HCVSConnectionString]
   
    <span data-ttu-id="22025-236">Tänk på följande när du skriver anslutningssträngen:</span><span class="sxs-lookup"><span data-stu-id="22025-236">When composing the connection string, keep in mind the following:</span></span>
   
   * <span data-ttu-id="22025-237">Om du ansluter till en namngiven instans i stället för en standardinstans (till exempel YourServer\SQLEXPRESS), måste du konfigurera SQL Server för att använda statiska portar.</span><span class="sxs-lookup"><span data-stu-id="22025-237">If you are connecting to a named instance instead of a default instance (for example, YourServer\SQLEXPRESS), you must configure your SQL Server to use static ports.</span></span> <span data-ttu-id="22025-238">Information om hur du konfigurerar statiska portar finns [hur du konfigurerar SQL Server för att lyssna på en viss port](http://support.microsoft.com/kb/823938).</span><span class="sxs-lookup"><span data-stu-id="22025-238">For information on configuring static ports, see [How to configure SQL Server to listen on a specific port](http://support.microsoft.com/kb/823938).</span></span> <span data-ttu-id="22025-239">Som standard använder UDP- och dynamiska portar, vilket inte stöds av Hybridanslutningar för namngivna instanser.</span><span class="sxs-lookup"><span data-stu-id="22025-239">By default, named instances use UDP and dynamic ports, which are not supported by Hybrid Connections.</span></span>
   * <span data-ttu-id="22025-240">Vi rekommenderar att du anger porten (1433 som standard, som visas i exemplet) i anslutningssträngen så att du kan kontrollera att dina lokala SQL Server har TCP aktiverad och använder rätt port.</span><span class="sxs-lookup"><span data-stu-id="22025-240">It is recommended that you specify the port (1433 by default, as shown in the example) on the connection string so that you can be sure that your local SQL Server has TCP enabled and is using the correct port.</span></span>
   * <span data-ttu-id="22025-241">Kom ihåg att använda SQL Server-autentisering för att ansluta, ange användar-ID och lösenord i anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="22025-241">Remember to use SQL Server Authentication to connect, specifying the user ID and password in your connection string.</span></span>
3. <span data-ttu-id="22025-242">Klicka på **spara** i Visual Studio för att spara filen Web.config.</span><span class="sxs-lookup"><span data-stu-id="22025-242">Click **Save** in Visual Studio to save the Web.config file.</span></span>

### <a name="run-the-project-locally-and-register-a-new-user"></a><span data-ttu-id="22025-243">Köra projektet lokalt och registrera en ny användare</span><span class="sxs-lookup"><span data-stu-id="22025-243">Run the project locally and register a new user</span></span>
1. <span data-ttu-id="22025-244">Nu kan köra lokalt webbprojektet nya genom att klicka på bläddringsknappen under felsökning.</span><span class="sxs-lookup"><span data-stu-id="22025-244">Now, run your new web project locally by clicking the browse button under Debug.</span></span> <span data-ttu-id="22025-245">Det här exemplet använder Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="22025-245">This example uses Internet Explorer.</span></span>
   
    ![Kör projektet][HCVSRunProject]
2. <span data-ttu-id="22025-247">I övre högra standardwebbsidan, väljer **registrera** att registrera ett nytt konto:</span><span class="sxs-lookup"><span data-stu-id="22025-247">On the upper right of the default web page, choose **Register** to register a new account:</span></span>
   
    ![Registrera ett nytt konto][HCVSRegisterLocally]
3. <span data-ttu-id="22025-249">Ange ett användarnamn och lösenord:</span><span class="sxs-lookup"><span data-stu-id="22025-249">Enter a user name and password:</span></span>
   
    ![Ange användarnamn och lösenord][HCVSCreateNewAccount]
   
    <span data-ttu-id="22025-251">Detta skapar automatiskt en databas på den lokala SQL-servern som innehåller information om gruppmedlemskap för ditt program.</span><span class="sxs-lookup"><span data-stu-id="22025-251">This automatically creates a database on your local SQL Server that holds the membership information for your application.</span></span> <span data-ttu-id="22025-252">En av tabellerna (**dbo. AspNetUsers**) innehåller web app autentiseringsuppgifter med de som du har angett.</span><span class="sxs-lookup"><span data-stu-id="22025-252">One of the tables (**dbo.AspNetUsers**) holds web app user credentials like the ones that you just entered.</span></span> <span data-ttu-id="22025-253">Den här tabellen visas senare under kursen.</span><span class="sxs-lookup"><span data-stu-id="22025-253">You will see this table later in the tutorial.</span></span>
4. <span data-ttu-id="22025-254">Stäng webbläsarfönstret på webbsidan som standard.</span><span class="sxs-lookup"><span data-stu-id="22025-254">Close the browser window of the default web page.</span></span> <span data-ttu-id="22025-255">Detta hindrar program i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="22025-255">This stops the application in Visual Studio.</span></span>

<span data-ttu-id="22025-256">Du är nu redo för nästa steg, vilket är att publicera program till Azure och testa den.</span><span class="sxs-lookup"><span data-stu-id="22025-256">You are now ready for the next step, which is to publish the application to Azure and test it.</span></span>

<a name="PubNTest"></a>

## <a name="f-publish-the-web-application-to-azure-and-test-it"></a><span data-ttu-id="22025-257">F.</span><span class="sxs-lookup"><span data-stu-id="22025-257">F.</span></span> <span data-ttu-id="22025-258">Publicera webbappen till Azure och testa den</span><span class="sxs-lookup"><span data-stu-id="22025-258">Publish the web application to Azure and test it</span></span>
<span data-ttu-id="22025-259">Nu kan du publicera programmet till din App Service webbapp och sedan testa den för att se hur hybridanslutning som du tidigare har konfigurerat används för att ansluta ditt webbprogram till databasen på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="22025-259">Now, you'll publish your application to your App Service web app and then test it to see how the hybrid connection you configured earlier is being used to connect your web app to the database on your local machine.</span></span>

### <a name="publish-the-web-application"></a><span data-ttu-id="22025-260">Publicera webbprogrammet</span><span class="sxs-lookup"><span data-stu-id="22025-260">Publish the web application</span></span>
1. <span data-ttu-id="22025-261">Du kan ladda ned din publiceringsprofil för Apptjänst-webbapp i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="22025-261">You can download your publishing profile for the App Service web app in the Azure Portal.</span></span> <span data-ttu-id="22025-262">Klicka på bladet för din webbapp **Get publiceringsprofil**, och sedan spara filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="22025-262">On the blade for your web app, click **Get publish profile**, and then save the file to your computer.</span></span>
   
    ![Hämta Publicera profil][PortalDownloadPublishProfile]
   
    <span data-ttu-id="22025-264">Därefter importerar du den här filen till ditt webbprogram för Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="22025-264">Next, you will import this file into your Visual Studio web application.</span></span>
2. <span data-ttu-id="22025-265">Högerklicka på projektnamnet i Solution Explorer i Visual Studio och välj **publicera**.</span><span class="sxs-lookup"><span data-stu-id="22025-265">In Visual Studio, right-click the project name in Solution Explorer and select **Publish**.</span></span>
   
    ![Välj publicera][HCVSRightClickProjectSelectPublish]
3. <span data-ttu-id="22025-267">I den **Publicera webbplats** dialogrutan på den **profil** , Välj **importera**.</span><span class="sxs-lookup"><span data-stu-id="22025-267">In the **Publish Web** dialog, on the **Profile** tab, choose **Import**.</span></span>
   
    ![Importera][HCVSPublishWebDialogImport]
4. <span data-ttu-id="22025-269">Bläddra till din hämtade publiceringsprofilen markerar du den och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="22025-269">Browse to your downloaded publishing profile, select it, and then click **OK**.</span></span>
   
    ![Bläddra till profil][HCVSBrowseToImportPubProfile]
5. <span data-ttu-id="22025-271">Din publiceringsinformation har importerats och visas på den **anslutning** fliken i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="22025-271">Your publishing information is imported and displays on the **Connection** tab of the dialog.</span></span>
   
    ![Klicka på Publicera][HCVSClickPublish]
   
    <span data-ttu-id="22025-273">Klicka på **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="22025-273">Click **Publish**.</span></span>
   
    <span data-ttu-id="22025-274">När publiceringen är klar startar webbläsaren och visar nu välkända ASP.NET-programmet--förutom att det är nu live i Azure-molnet!</span><span class="sxs-lookup"><span data-stu-id="22025-274">When publishing completes, your browser will launch and show your now familiar ASP.NET application -- except that now it is live in the Azure cloud!</span></span>

<span data-ttu-id="22025-275">Sedan använder du webbprogrammet live för att visa dess Hybridanslutning i åtgärden.</span><span class="sxs-lookup"><span data-stu-id="22025-275">Next, you will use your live web application to see its Hybrid Connection in action.</span></span>

### <a name="test-the-completed-web-application-on-azure"></a><span data-ttu-id="22025-276">Testa det färdiga webbprogrammet på Azure</span><span class="sxs-lookup"><span data-stu-id="22025-276">Test the completed web application on Azure</span></span>
1. <span data-ttu-id="22025-277">Högst upp höger på din webbsida på Azure, Välj **logga in**.</span><span class="sxs-lookup"><span data-stu-id="22025-277">On the top right of your web page on Azure, choose **Log in**.</span></span>
   
    ![Testa inloggningen][HCTestLogIn]
2. <span data-ttu-id="22025-279">Din App Service web-appen är nu ansluten till ditt webbprogram medlemskap databas på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="22025-279">Your App Service web app is now connected to your web application's membership database on your local machine.</span></span> <span data-ttu-id="22025-280">Kontrollera detta genom att logga in med samma autentiseringsuppgifter som du angav tidigare i den lokala databasen.</span><span class="sxs-lookup"><span data-stu-id="22025-280">To verify this, log in with the same credentials that you entered in the local database earlier.</span></span>
   
    ![Hello hälsning][HCTestHelloContoso]
3. <span data-ttu-id="22025-282">Om du vill testa ytterligare en ny hybridanslutning, logga ut från ditt webbprogram för Azure och registrera som en annan användare.</span><span class="sxs-lookup"><span data-stu-id="22025-282">To further test your new hybrid connection, log off of your Azure web application and register as another user.</span></span> <span data-ttu-id="22025-283">Ange ett nytt användarnamn och lösenord och klicka sedan på **registrera**.</span><span class="sxs-lookup"><span data-stu-id="22025-283">Provide a new user name and password, and then click **Register**.</span></span>
   
    ![Testa registrera en annan användare][HCTestRegisterRelecloud]
4. <span data-ttu-id="22025-285">Öppna SQL Management Studio på den lokala datorn för att kontrollera att den nya användarens autentiseringsuppgifter har lagrats i den lokala databasen via hybridanslutningen.</span><span class="sxs-lookup"><span data-stu-id="22025-285">To verify that the new user's credentials have been stored in your local database through your hybrid connection, open SQL Management Studio on your local computer.</span></span> <span data-ttu-id="22025-286">I Object Explorer, expandera den **MembershipDB** databasen och expandera sedan **tabeller**.</span><span class="sxs-lookup"><span data-stu-id="22025-286">In Object Explorer, expand the **MembershipDB** database, and then expand **Tables**.</span></span> <span data-ttu-id="22025-287">Högerklicka på den **dbo. AspNetUsers** medlemskap tabell och välj **Välj de 1000 översta raderna** att visa resultat.</span><span class="sxs-lookup"><span data-stu-id="22025-287">Right-click the **dbo.AspNetUsers** membership table and choose **Select Top 1000 Rows** to view the results.</span></span>
   
    ![Visa resultaten][HCTestSSMSTree]
5. <span data-ttu-id="22025-289">Medlemskap i lokal tabellen visar nu båda kontona - konto som du skapade lokalt och ett som du skapade i Azure-molnet.</span><span class="sxs-lookup"><span data-stu-id="22025-289">Your local membership table now shows both accounts - the one that you created locally, and the one that you created in the Azure cloud.</span></span> <span data-ttu-id="22025-290">Det konto som du skapade i molnet har sparats till den lokala databasen via funktionen för Azures Hybridanslutning.</span><span class="sxs-lookup"><span data-stu-id="22025-290">The one that you created in the cloud has been saved to your on-premises database through Azure's Hybrid Connection feature.</span></span>
   
    ![Registrerade användare i en lokal databas][HCTestShowMemberDb]

<span data-ttu-id="22025-292">Du har nu skapats och distribuerats ASP.NET-webbprogram som använder en hybridanslutning mellan en webbapp i Azure-molnet och en lokal SQL Server-databas.</span><span class="sxs-lookup"><span data-stu-id="22025-292">You have now created and deployed an ASP.NET web application that uses a hybrid connection between a web app in the Azure cloud and an on-premises SQL Server database.</span></span> <span data-ttu-id="22025-293">Grattis!</span><span class="sxs-lookup"><span data-stu-id="22025-293">Congratulations!</span></span>

## <a name="see-also"></a><span data-ttu-id="22025-294">Se även</span><span class="sxs-lookup"><span data-stu-id="22025-294">See Also</span></span>
[<span data-ttu-id="22025-295">Översikt över hybrid-anslutningar</span><span class="sxs-lookup"><span data-stu-id="22025-295">Hybrid Connections overview</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=397274)

[<span data-ttu-id="22025-296">Josh Twist introducerar hybridanslutningar (Channel 9 video)</span><span class="sxs-lookup"><span data-stu-id="22025-296">Josh Twist introduces hybrid connections (Channel 9 video)</span></span>](http://channel9.msdn.com/Shows/Azure-Friday/Josh-Twist-introduces-hybrid-connections)

[<span data-ttu-id="22025-297">Översikt över hybrid-anslutningar</span><span class="sxs-lookup"><span data-stu-id="22025-297">Hybrid Connections overview</span></span>](/services/biztalk-services/)

[<span data-ttu-id="22025-298">BizTalk-tjänst: Instrumentpanelen, övervaka, skala, konfigurera och Hybridanslutning flikar</span><span class="sxs-lookup"><span data-stu-id="22025-298">BizTalk Services: Dashboard, Monitor, Scale, Configure, and Hybrid Connection tabs</span></span>](../biztalk-services/biztalk-dashboard-monitor-scale-tabs.md)

[<span data-ttu-id="22025-299">Skapa en verklig Hybridmoln med sömlös programmet Portability (Channel 9 video)</span><span class="sxs-lookup"><span data-stu-id="22025-299">Building a Real-World Hybrid Cloud with Seamless Application Portability (Channel 9 video)</span></span>](http://channel9.msdn.com/events/TechEd/NorthAmerica/2014/DCIM-B323#fbid=)

[<span data-ttu-id="22025-300">Åtkomst till lokala resurser genom att använda hybridanslutningar i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="22025-300">Access on-premises resources using hybrid connections in Azure App Service</span></span>](web-sites-hybrid-connection-get-started.md)

[<span data-ttu-id="22025-301">Översikt över ASP.NET-identitet</span><span class="sxs-lookup"><span data-stu-id="22025-301">ASP.NET Identity Overview</span></span>](http://www.asp.net/identity)

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

<!-- IMAGES -->
[SQLServerInstall]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A01SQLServerInstall.png
[ChooseDefaultInstance]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A02ChooseDefaultInstance.png
[ChooseMixedMode]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A03ChooseMixedMode.png
[SSMSConnectToServer]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A04SSMSConnectToServer.png
[SSMScreateNewDB]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A05SSMScreateNewDBlh.png
[SSMSprovideDBname]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A06SSMSprovideDBname.png
[SSMSMembershipDBCreated]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A07SSMSMembershipDBCreated.png
[New]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B01New.png
[NewWebsite]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B02NewWebsite.png
[WebsiteCreationBlade]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B03WebsiteCreationBlade.png
[WebSiteRunningBlade]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B04WebSiteRunningBlade.png
[Browse]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B05Browse.png
[DefaultWebSitePage]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B06DefaultWebSitePage.png
[CreateHCHCIcon]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C01CreateHCHCIcon.png
[CreateHCAddHC]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C02CreateHCAddHC.png
[TwinCreateHCBlades]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C03TwinCreateHCBlades.png
[CreateHCCreateBTS]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C04CreateHCCreateBTS.png
[CreateBTScomplete]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C05CreateBTScomplete.png
[CreateHCSuccessNotification]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C06CreateHCSuccessNotification.png
[CreateHCOneConnectionCreated]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C07CreateHCOneConnectionCreated.png
[HCIcon]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D01HCIcon.png
[NotConnected]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D02NotConnected.png
[NotConnectedBlade]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D03NotConnectedBlade.png
[ClickListenerSetup]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D04ClickListenerSetup.png
[ClickToInstallHCM]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D05ClickToInstallHCM.png
[ApplicationRunWarning]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D06ApplicationRunWarning.png
[UAC]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D07UAC.png
[HCMInstalling]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D08HCMInstalling.png
[HCMInstallComplete]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D09HCMInstallComplete.png
[HCStatusConnected]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D10HCStatusConnected.png
[HCVSNewProject]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E01HCVSNewProject.png
[HCVSChooseASPNET]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E02HCVSChooseASPNET.png
[HCVSChooseMVC]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E03HCVSChooseMVC.png
[HCVSReadmePage]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E04HCVSReadmePage.png
[HCVSChooseWebConfig]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E05HCVSChooseWebConfig.png
[HCVSConnectionString]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E06HCVSConnectionString.png
[HCVSRunProject]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E06HCVSRunProject.png
[HCVSRegisterLocally]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E07HCVSRegisterLocally.png
[HCVSCreateNewAccount]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E08HCVSCreateNewAccount.png
[PortalDownloadPublishProfile]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F01PortalDownloadPublishProfile.png
[HCVSPublishProfileInDownloadsFolder]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F02HCVSPublishProfileInDownloadsFolder.png
[HCVSRightClickProjectSelectPublish]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F03HCVSRightClickProjectSelectPublish.png
[HCVSPublishWebDialogImport]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F04HCVSPublishWebDialogImport.png
[HCVSBrowseToImportPubProfile]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F05HCVSBrowseToImportPubProfile.png
[HCVSClickPublish]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F06HCVSClickPublish.png
[HCTestLogIn]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F07HCTestLogIn.png
[HCTestHelloContoso]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F08HCTestHelloContoso.png
[HCTestRegisterRelecloud]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F09HCTestRegisterRelecloud.png
[HCTestSSMSTree]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F10HCTestSSMSTree.png
[HCTestShowMemberDb]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F11HCTestShowMemberDb.png
