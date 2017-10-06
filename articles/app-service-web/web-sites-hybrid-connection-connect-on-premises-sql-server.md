---
title: "aaaConnect tooon lokal SQL Server från en webbapp i Azure App Service med Hybridanslutningar"
description: Skapa en webbapp i Microsoft Azure och koppla den tooan lokala SQL Server-databas
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
ms.openlocfilehash: 2e8f8f7e0b9733cfb0433697615faba4358c6023
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooon-premises-sql-server-from-a-web-app-in-azure-app-service-using-hybrid-connections"></a><span data-ttu-id="c116a-103">Ansluta tooon lokal SQL Server från en webbapp i Azure App Service med Hybridanslutningar</span><span class="sxs-lookup"><span data-stu-id="c116a-103">Connect tooon-premises SQL Server from a web app in Azure App Service using Hybrid Connections</span></span>
<span data-ttu-id="c116a-104">Hybridanslutningar kan ansluta [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps tooon lokala resurser som använder en statisk TCP-port.</span><span class="sxs-lookup"><span data-stu-id="c116a-104">Hybrid Connections can connect [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps tooon-premises resources that use a static TCP port.</span></span> <span data-ttu-id="c116a-105">Resurser som stöds är Microsoft SQL Server, MySQL, http-webb-API: er, användning och de flesta anpassade webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="c116a-105">Supported resources include Microsoft SQL Server, MySQL, HTTP Web APIs, App Service, and most custom Web Services.</span></span>

<span data-ttu-id="c116a-106">I den här kursen får du lära dig hur toocreate en App Service web app i hello [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715), ansluta hello web app tooyour lokala lokala SQL Server-databas med hjälp av hello ny Hybridanslutning funktion, skapa en enkel ASP.NET program som ska använda hello hybridanslutning och distribuera hello programmet toohello App Service webbapp.</span><span class="sxs-lookup"><span data-stu-id="c116a-106">In this tutorial, you will learn how toocreate an App Service web app in hello [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715), connect hello web app tooyour local on-premises SQL Server database using hello new Hybrid Connection feature, create a simple ASP.NET application that will use hello hybrid connection, and deploy hello application toohello App Service web app.</span></span> <span data-ttu-id="c116a-107">hello slutförts webbapp på Azure lagrar autentiseringsuppgifter i en databas för medlemskap som är lokalt.</span><span class="sxs-lookup"><span data-stu-id="c116a-107">hello completed web app on Azure stores user credentials in a membership database that is on-premises.</span></span> <span data-ttu-id="c116a-108">hello självstudiekursen förutsätter några tidigare erfarenheter med hjälp av Azure eller ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c116a-108">hello tutorial assumes no prior experience using Azure or ASP.NET.</span></span>

> [!NOTE]
> <span data-ttu-id="c116a-109">Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service.</span><span class="sxs-lookup"><span data-stu-id="c116a-109">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="c116a-110">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="c116a-110">No credit cards required; no commitments.</span></span>
> 
> <span data-ttu-id="c116a-111">hello Web Apps del av hello Hybridanslutningar funktionen är endast tillgänglig i hello [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c116a-111">hello Web Apps portion of hello Hybrid Connections feature is available only in hello [Azure Portal](https://portal.azure.com).</span></span> <span data-ttu-id="c116a-112">toocreate en anslutning i BizTalk-tjänst finns [Hybridanslutningar](http://go.microsoft.com/fwlink/p/?LinkID=397274).</span><span class="sxs-lookup"><span data-stu-id="c116a-112">toocreate a connection in BizTalk Services, see [Hybrid Connections](http://go.microsoft.com/fwlink/p/?LinkID=397274).</span></span>  
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="c116a-113">Krav</span><span class="sxs-lookup"><span data-stu-id="c116a-113">Prerequisites</span></span>
<span data-ttu-id="c116a-114">toocomplete den här kursen behöver du hello följande produkter.</span><span class="sxs-lookup"><span data-stu-id="c116a-114">toocomplete this tutorial, you'll need hello following products.</span></span> <span data-ttu-id="c116a-115">Alla är tillgängliga i ledigt versioner, så du kan börja utveckla för Azure och helt kostnadsfritt.</span><span class="sxs-lookup"><span data-stu-id="c116a-115">All are available in free versions, so you can start developing for Azure entirely for free.</span></span>

* <span data-ttu-id="c116a-116">**Azure-prenumeration** – för en kostnadsfri prenumeration finns [kostnadsfri utvärderingsversion av Azure](/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c116a-116">**Azure subscription** - For a free subscription, see [Azure Free Trial](/pricing/free-trial/).</span></span>
* <span data-ttu-id="c116a-117">**Visual Studio 2013** -toodownload en kostnadsfri utvärderingsversion av Visual Studio 2013 finns [Visual Studio-hämtningar](http://www.visualstudio.com/downloads/download-visual-studio-vs).</span><span class="sxs-lookup"><span data-stu-id="c116a-117">**Visual Studio 2013** - toodownload a free trial version of Visual Studio 2013, see [Visual Studio Downloads](http://www.visualstudio.com/downloads/download-visual-studio-vs).</span></span> <span data-ttu-id="c116a-118">Installera detta innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="c116a-118">Install this before continuing.</span></span>
* <span data-ttu-id="c116a-119">**Microsoft .NET Framework 3.5 servicepack 1** -om operativsystemet är Windows 8.1, Windows Server 2012 R2, Windows 8, Windows Server 2012, Windows 7 eller Windows Server 2008 R2, kan du aktivera det på Kontrollpanelen > program och funktioner > Aktivera Windows-funktioner på eller stänga av.</span><span class="sxs-lookup"><span data-stu-id="c116a-119">**Microsoft .NET Framework 3.5 Service Pack 1** - If your operating system is Windows 8.1, Windows Server 2012 R2, Windows 8, Windows Server 2012, Windows 7, or Windows Server 2008 R2, you can enable this in Control Panel > Programs and Features > Turn Windows features on or off.</span></span> <span data-ttu-id="c116a-120">Annars kan du hämta det från hello [Microsoft Download Center](http://www.microsoft.com/download/en/details.aspx?displaylang=en&id=22).</span><span class="sxs-lookup"><span data-stu-id="c116a-120">Otherwise, you can download it from hello [Microsoft Download Center](http://www.microsoft.com/download/en/details.aspx?displaylang=en&id=22).</span></span>
* <span data-ttu-id="c116a-121">**SQL Server 2014 Express med verktyg** -hämta Microsoft SQL Server Express kostnadsfritt på hello [sidan Microsoft Web Platform databas](http://www.microsoft.com/web/platform/database.aspx).</span><span class="sxs-lookup"><span data-stu-id="c116a-121">**SQL Server 2014 Express with Tools** - download Microsoft SQL Server Express for free at hello [Microsoft Web Platform Database page](http://www.microsoft.com/web/platform/database.aspx).</span></span> <span data-ttu-id="c116a-122">Välj hello **Express** (inte LocalDB) version.</span><span class="sxs-lookup"><span data-stu-id="c116a-122">Choose hello **Express** (not LocalDB) version.</span></span> <span data-ttu-id="c116a-123">Hej **Express med verktyg** version omfattar SQL Server Management Studio, som du ska använda i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="c116a-123">hello **Express with Tools** version includes SQL Server Management Studio, which you will use in this tutorial.</span></span>
* <span data-ttu-id="c116a-124">**SQL Server Management Studio Express** - detta ingår i SQL Server 2014 Express hello verktyg hämta som nämns ovan, men om du behöver tooinstall den separat, du kan hämta och installera det från hello [SQL Server Express hämtningssidan](http://www.microsoft.com/web/platform/database.aspx).</span><span class="sxs-lookup"><span data-stu-id="c116a-124">**SQL Server Management Studio Express** - This is included with hello SQL Server 2014 Express with Tools download mentioned above, but if you need tooinstall it separately, you can download and install it from hello [SQL Server Express download page](http://www.microsoft.com/web/platform/database.aspx).</span></span>

<span data-ttu-id="c116a-125">hello kursen förutsätter att du har en Azure-prenumeration, att du har installerat Visual Studio 2013 och att du har installerat eller aktiverat .NET Framework 3.5.</span><span class="sxs-lookup"><span data-stu-id="c116a-125">hello tutorial assumes that you have an Azure subscription, that you have installed Visual Studio 2013, and that you have installed or enabled .NET Framework 3.5.</span></span> <span data-ttu-id="c116a-126">hello kursen visar hur tooinstall SQL Server 2014 Express i en konfiguration som fungerar bra med hello Azure Hybridanslutningar funktion (en standardinstans med en statisk TCP-port).</span><span class="sxs-lookup"><span data-stu-id="c116a-126">hello tutorial shows you how tooinstall SQL Server 2014 Express in a configuration that works well with hello Azure Hybrid Connections feature (a default instance with a static TCP port).</span></span> <span data-ttu-id="c116a-127">Hämta SQL Server 2014 Express med verktyg hello-plats som nämns ovan om du inte installerade SQL Server innan du startar hello kursen.</span><span class="sxs-lookup"><span data-stu-id="c116a-127">Before starting hello tutorial, download SQL Server 2014 Express with Tools from hello location mentioned above if you do not have SQL Server installed.</span></span>

### <a name="notes"></a><span data-ttu-id="c116a-128">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="c116a-128">Notes</span></span>
<span data-ttu-id="c116a-129">toouse en lokal SQL Server eller SQL Server Express-databas med en hybridanslutning, måste TCP/IP toobe aktiverad på en statisk port.</span><span class="sxs-lookup"><span data-stu-id="c116a-129">toouse an on-premises SQL Server or SQL Server Express database with a hybrid connection, TCP/IP needs toobe enabled on a static port.</span></span> <span data-ttu-id="c116a-130">Standardinstanser på SQL Server använder statisk port 1433, inte namngivna instanser.</span><span class="sxs-lookup"><span data-stu-id="c116a-130">Default instances on SQL Server use static port 1433, whereas named instances do not.</span></span>

<span data-ttu-id="c116a-131">hello-dator som du installerar hello lokal Hybridanslutningshanterare agent:</span><span class="sxs-lookup"><span data-stu-id="c116a-131">hello computer on which you install hello on-premises Hybrid Connection Manager agent:</span></span>

* <span data-ttu-id="c116a-132">Måste ha utgående anslutning tooAzure över:</span><span class="sxs-lookup"><span data-stu-id="c116a-132">Must have outbound connectivity tooAzure over:</span></span>

| <span data-ttu-id="c116a-133">Port</span><span class="sxs-lookup"><span data-stu-id="c116a-133">Port</span></span> | <span data-ttu-id="c116a-134">Varför</span><span class="sxs-lookup"><span data-stu-id="c116a-134">Why</span></span> |
| --- | --- |
| <span data-ttu-id="c116a-135">80</span><span class="sxs-lookup"><span data-stu-id="c116a-135">80</span></span> |<span data-ttu-id="c116a-136">**Krävs** för HTTP-porten för valideringen av servercertifikatet och eventuellt för dataanslutning.</span><span class="sxs-lookup"><span data-stu-id="c116a-136">**Required** for HTTP port for certificate validation and optionally for data connectivity.</span></span> |
| <span data-ttu-id="c116a-137">443</span><span class="sxs-lookup"><span data-stu-id="c116a-137">443</span></span> |<span data-ttu-id="c116a-138">**Valfria** för dataanslutning.</span><span class="sxs-lookup"><span data-stu-id="c116a-138">**Optional** for data connectivity.</span></span> <span data-ttu-id="c116a-139">Om utgående anslutning too443 inte är tillgänglig används TCP-port 80.</span><span class="sxs-lookup"><span data-stu-id="c116a-139">If outbound connectivity too443 is unavailable, TCP port 80 is used.</span></span> |
| <span data-ttu-id="c116a-140">5671 och 9352</span><span class="sxs-lookup"><span data-stu-id="c116a-140">5671 and 9352</span></span> |<span data-ttu-id="c116a-141">**Rekommenderade** men valfritt för dataanslutning.</span><span class="sxs-lookup"><span data-stu-id="c116a-141">**Recommended** but Optional for data connectivity.</span></span> <span data-ttu-id="c116a-142">Observera att det här läget vanligtvis ger högre genomströmning.</span><span class="sxs-lookup"><span data-stu-id="c116a-142">Note this mode usually yields higher throughput.</span></span> <span data-ttu-id="c116a-143">Om utgående anslutning toothese portar är tillgänglig, används TCP-port 443.</span><span class="sxs-lookup"><span data-stu-id="c116a-143">If outbound connectivity toothese ports is unavailable, TCP port 443 is used.</span></span> |

* <span data-ttu-id="c116a-144">Måste vara kan tooreach hello *värdnamn*:*portnumber* på din lokala resursen.</span><span class="sxs-lookup"><span data-stu-id="c116a-144">Must be able tooreach hello *hostname*:*portnumber* of your on-premises resource.</span></span>

<span data-ttu-id="c116a-145">hello stegen i den här artikeln förutsätter att du använder hello webbläsaren från hello-dator som är värd för hello lokalt hybrid anslutning agent.</span><span class="sxs-lookup"><span data-stu-id="c116a-145">hello steps in this article assume that you are using hello browser from hello computer that will host hello on-premises hybrid connection agent.</span></span>

<span data-ttu-id="c116a-146">Om du redan har SQL Server installeras i en konfiguration och i en miljö som uppfyller villkoren för hello som beskrivs ovan kan du gå vidare och börja med [och skapa en SQL Server-databas lokalt](#CreateSQLDB).</span><span class="sxs-lookup"><span data-stu-id="c116a-146">If you already have SQL Server installed in a configuration and in an environment that meets hello conditions described above, you can skip ahead and start with [Create a SQL Server database on-premises](#CreateSQLDB).</span></span>

<a name="InstallSQL"></a>

## <a name="a-install-sql-server-express-enable-tcpip-and-create-a-sql-server-database-on-premises"></a><span data-ttu-id="c116a-147">A.</span><span class="sxs-lookup"><span data-stu-id="c116a-147">A.</span></span> <span data-ttu-id="c116a-148">Installera SQL Server Express, aktivera TCP/IP och skapa en SQL Server-databas lokalt</span><span class="sxs-lookup"><span data-stu-id="c116a-148">Install SQL Server Express, enable TCP/IP, and create a SQL Server database on-premises</span></span>
<span data-ttu-id="c116a-149">Det här avsnittet visar hur tooinstall SQL Server Express, aktivera TCP/IP och skapa en databas så att webbprogrammet fungerar med hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c116a-149">This section shows you how tooinstall SQL Server Express, enable TCP/IP, and create a database so that your web application will work with hello Azure Portal.</span></span>

### <a name="install-sql-server-express"></a><span data-ttu-id="c116a-150">Installera SQL Server Express</span><span class="sxs-lookup"><span data-stu-id="c116a-150">Install SQL Server Express</span></span>
1. <span data-ttu-id="c116a-151">tooinstall SQL Server Express, kör hello **SQLEXPRWT_x64_ENU.exe** eller **SQLEXPR_x86_ENU.exe** filen som du hämtade.</span><span class="sxs-lookup"><span data-stu-id="c116a-151">tooinstall SQL Server Express, run hello **SQLEXPRWT_x64_ENU.exe** or **SQLEXPR_x86_ENU.exe** file that you downloaded.</span></span> <span data-ttu-id="c116a-152">hello SQL Server Installationscenter guiden visas.</span><span class="sxs-lookup"><span data-stu-id="c116a-152">hello SQL Server Installation Center wizard appears.</span></span>
   
    ![SQL Server-installation][SQLServerInstall]
2. <span data-ttu-id="c116a-154">Välj **fristående nya SQL Server-installation eller Lägg till en befintlig installation av funktioner tooan**.</span><span class="sxs-lookup"><span data-stu-id="c116a-154">Choose **New SQL Server stand-alone installation or add features tooan existing installation**.</span></span> <span data-ttu-id="c116a-155">Följ instruktionerna för hello, acceptera hello standardalternativen och inställningar, tills du får toohello **instanskonfiguration** sidan.</span><span class="sxs-lookup"><span data-stu-id="c116a-155">Follow hello instructions, accepting hello default choices and settings, until you get toohello **Instance Configuration** page.</span></span>
3. <span data-ttu-id="c116a-156">På hello **instanskonfiguration** väljer **standardinstansen**.</span><span class="sxs-lookup"><span data-stu-id="c116a-156">On hello **Instance Configuration** page, choose **Default instance**.</span></span>
   
    ![Välj standardinstansen][ChooseDefaultInstance]
   
    <span data-ttu-id="c116a-158">Som standard lyssnar hello standardinstansen av SQL Server för begäranden från SQL Server-klienter på statisk port 1433, vilket är vilka hello Hybridanslutningar funktionen kräver.</span><span class="sxs-lookup"><span data-stu-id="c116a-158">By default, hello default instance of SQL Server listens for requests from SQL Server clients on static port 1433, which is what hello Hybrid Connections feature requires.</span></span> <span data-ttu-id="c116a-159">Namngivna instanser använder dynamiska portar och UDP, vilket inte stöds av Hybridanslutningar.</span><span class="sxs-lookup"><span data-stu-id="c116a-159">Named instances use dynamic ports and UDP, which are not supported by Hybrid Connections.</span></span>
4. <span data-ttu-id="c116a-160">Acceptera hello standardinställningar på hello **serverkonfiguration** sidan.</span><span class="sxs-lookup"><span data-stu-id="c116a-160">Accept hello defaults on hello **Server Configuration** page.</span></span>
5. <span data-ttu-id="c116a-161">På hello **konfiguration av databasmotor** sidan under **autentiseringsläge**, Välj **blandat läge (SQL Server-autentisering och Windows-autentisering)**, och tillhandahålla ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="c116a-161">On hello **Database Engine Configuration** page, under **Authentication Mode**, choose **Mixed Mode (SQL Server authentication and Windows authentication)**, and provide a password.</span></span>
   
    ![Välj blandat läge][ChooseMixedMode]
   
    <span data-ttu-id="c116a-163">I den här självstudiekursen kommer du att använda SQL Server-autentisering.</span><span class="sxs-lookup"><span data-stu-id="c116a-163">In this tutorial, you will be using SQL Server authentication.</span></span> <span data-ttu-id="c116a-164">Vara säker på att tooremember hello lösenord som du anger eftersom du behöver senare.</span><span class="sxs-lookup"><span data-stu-id="c116a-164">Be sure tooremember hello password that you provide, because you will need it later.</span></span>
6. <span data-ttu-id="c116a-165">Gå igenom hello resten av hello guiden toocomplete hello installation.</span><span class="sxs-lookup"><span data-stu-id="c116a-165">Step through hello rest of hello wizard toocomplete hello installation.</span></span>

### <a name="enable-tcpip"></a><span data-ttu-id="c116a-166">Aktivera TCP/IP</span><span class="sxs-lookup"><span data-stu-id="c116a-166">Enable TCP/IP</span></span>
<span data-ttu-id="c116a-167">tooenable TCP/IP, ska du använda SQL Server Configuration Manager, som installerades när du installerade SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="c116a-167">tooenable TCP/IP, you will use SQL Server Configuration Manager, which was installed when you installed SQL Server Express.</span></span> <span data-ttu-id="c116a-168">Gör så hello i [aktivera nätverksprotokoll för TCP/IP för SQL Server](http://technet.microsoft.com/library/hh231672%28v=sql.110%29.aspx) innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="c116a-168">Follow hello steps in [Enable TCP/IP Network Protocol for SQL Server](http://technet.microsoft.com/library/hh231672%28v=sql.110%29.aspx) before continuing.</span></span>

<a name="CreateSQLDB"></a>

### <a name="create-a-sql-server-database-on-premises"></a><span data-ttu-id="c116a-169">Skapa en SQL Server-databas lokalt</span><span class="sxs-lookup"><span data-stu-id="c116a-169">Create a SQL Server database on-premises</span></span>
<span data-ttu-id="c116a-170">Ditt webbprogram för Visual Studio kräver en databas för medlemskap som kan nås av Azure.</span><span class="sxs-lookup"><span data-stu-id="c116a-170">Your Visual Studio web application requires a membership database that can be accessed by Azure.</span></span> <span data-ttu-id="c116a-171">Detta kräver en SQL Server eller SQL Server Express-databas (inte hello LocalDB-databas som hello MVC-mallen använder som standard), så ska du skapa hello medlemskapsdatabasen nästa.</span><span class="sxs-lookup"><span data-stu-id="c116a-171">This requires a SQL Server or SQL Server Express database (not hello LocalDB database that hello MVC template uses by default), so you'll create hello membership database next.</span></span>

1. <span data-ttu-id="c116a-172">Anslut toohello SQL Server som du precis har installerat i SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="c116a-172">In SQL Server Management Studio, connect toohello SQL Server you just installed.</span></span> <span data-ttu-id="c116a-173">(Om hello **ansluta tooServer** dialogrutan inte visas automatiskt, navigera för**Object Explorer** i hello vänstra rutan klickar du på **Anslut**, och klicka sedan på **Database Engine**.) ![Ansluta tooServer][SSMSConnectToServer]</span><span class="sxs-lookup"><span data-stu-id="c116a-173">(If hello **Connect tooServer** dialog does not appear automatically, navigate too**Object Explorer** in hello left pane, click **Connect**, and then click **Database Engine**.) ![Connect tooServer][SSMSConnectToServer]</span></span>
   
    <span data-ttu-id="c116a-174">För **servertyp**, Välj **databasmotorn**.</span><span class="sxs-lookup"><span data-stu-id="c116a-174">For **Server type**, choose **Database Engine**.</span></span> <span data-ttu-id="c116a-175">För **servernamn**, kan du använda **localhost** eller hello namnet på hello-dator som du använder.</span><span class="sxs-lookup"><span data-stu-id="c116a-175">For **Server name**, you can use **localhost** or hello name of hello computer that you are using.</span></span> <span data-ttu-id="c116a-176">Välj **SQL Server-autentisering**, och sedan logga in med användarnamnet för hello sa och hello lösenord som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="c116a-176">Choose **SQL Server authentication**, and then log in with hello sa user name and hello password that you created earlier.</span></span>
2. <span data-ttu-id="c116a-177">toocreate en ny databas med hjälp av SQL Server Management Studio, högerklicka på **databaser** i Object Explorer och klicka sedan på **ny databas**.</span><span class="sxs-lookup"><span data-stu-id="c116a-177">toocreate a new database by using SQL Server Management Studio, right-click **Databases** in Object Explorer, and then click **New Database**.</span></span>
   
    ![Skapa en ny databas][SSMScreateNewDB]
3. <span data-ttu-id="c116a-179">I hello **ny databas** dialogrutan Ange MembershipDB för hello databasens namn och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c116a-179">In hello **New Database** dialog, enter MembershipDB for hello database name, and then click **OK**.</span></span>
   
    ![Ange databasnamn][SSMSprovideDBname]
   
    <span data-ttu-id="c116a-181">Observera att du inte göra några ändringar toohello databasen nu.</span><span class="sxs-lookup"><span data-stu-id="c116a-181">Note that you do not make any changes toohello database at this point.</span></span> <span data-ttu-id="c116a-182">information om medlemskap i hello läggas till automatiskt senare av ditt webbprogram när den körs.</span><span class="sxs-lookup"><span data-stu-id="c116a-182">hello membership information will be added automatically later by your web application when you run it.</span></span>
4. <span data-ttu-id="c116a-183">I Object Explorer, om du expanderar **databaser**, ser du att hello medlemskapsdatabasen har skapats.</span><span class="sxs-lookup"><span data-stu-id="c116a-183">In Object Explorer, if you expand **Databases**, you will see that hello membership database has been created.</span></span>
   
    ![MembershipDB skapas][SSMSMembershipDBCreated]

<a name="CreateSite"></a>

## <a name="b-create-a-web-app-in-hello-azure-portal"></a><span data-ttu-id="c116a-185">B.</span><span class="sxs-lookup"><span data-stu-id="c116a-185">B.</span></span> <span data-ttu-id="c116a-186">Skapa en webbapp i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="c116a-186">Create a web app in hello Azure Portal</span></span>
> [!NOTE]
> <span data-ttu-id="c116a-187">Om du redan har skapat ett webbprogram i hello Azure-portalen som du vill toouse för den här självstudiekursen, du kan gå vidare för[skapa en Hybrid-anslutning och en BizTalk Service](#CreateHC) och fortsätta därifrån.</span><span class="sxs-lookup"><span data-stu-id="c116a-187">If you have already created a web app in hello Azure Portal that you want toouse for this tutorial, you can skip ahead too[Create a Hybrid Connection and a BizTalk Service](#CreateHC) and continue from there.</span></span>
> 
> 

1. <span data-ttu-id="c116a-188">I hello [Azure Portal](https://portal.azure.com), klickar du på **ny** > **webb + mobilt** > **webbapp**.</span><span class="sxs-lookup"><span data-stu-id="c116a-188">In hello [Azure Portal](https://portal.azure.com), click **New** > **Web + Mobile** > **Web app**.</span></span>
   
    ![Knappen Nytt][New]
2. <span data-ttu-id="c116a-190">Konfigurera ditt webbprogram och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="c116a-190">Configure your web app, and then click **Create**.</span></span>
   
    ![Namn på webbplats][WebsiteCreationBlade]
3. <span data-ttu-id="c116a-192">Hello webbprogram skapas och dess web app bladet visas efter en liten stund.</span><span class="sxs-lookup"><span data-stu-id="c116a-192">After a few moments, hello web app is created and its web app blade appears.</span></span> <span data-ttu-id="c116a-193">hello bladet är en lodrätt rullningsbara instrumentpanel där du kan hantera ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="c116a-193">hello blade is a vertically scrollable dashboard that lets you manage your web app.</span></span>
   
    ![Webbplatsen som kör][WebSiteRunningBlade]
   
    <span data-ttu-id="c116a-195">tooverify hello webbprogrammet är aktiv, kan du klicka på hello **Bläddra** ikonen toodisplay hello standardsida.</span><span class="sxs-lookup"><span data-stu-id="c116a-195">tooverify hello web app is live, you can click hello **Browse** icon toodisplay hello default page.</span></span>

<span data-ttu-id="c116a-196">Därefter skapar du en hybridanslutning och BizTalk-tjänst för hello webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="c116a-196">Next, you will create a hybrid connection and a BizTalk service for hello web app.</span></span>

<a name="CreateHC"></a>

## <a name="c-create-a-hybrid-connection-and-a-biztalk-service"></a><span data-ttu-id="c116a-197">C.</span><span class="sxs-lookup"><span data-stu-id="c116a-197">C.</span></span> <span data-ttu-id="c116a-198">Skapa en Hybridanslutning och BizTalk-tjänst</span><span class="sxs-lookup"><span data-stu-id="c116a-198">Create a Hybrid Connection and a BizTalk Service</span></span>
1. <span data-ttu-id="c116a-199">Tillbaka i hello Portal, gå toosettings och klicka på **nätverk** > **konfigurera slutpunkter för din hybridanslutning**.</span><span class="sxs-lookup"><span data-stu-id="c116a-199">Back in hello Portal, go toosettings and click **Networking** > **Configure your hybrid connection endpoints**.</span></span>
   
    ![Hybridanslutningar][CreateHCHCIcon]
2. <span data-ttu-id="c116a-201">Klicka på hello Hybrid anslutningar bladet **Lägg till** > **ny hybridanslutning**.</span><span class="sxs-lookup"><span data-stu-id="c116a-201">On hello Hybrid connections blade, click **Add** > **New hybrid connection**.</span></span>
3. <span data-ttu-id="c116a-202">På hello **skapa hybridanslutning** bladet:</span><span class="sxs-lookup"><span data-stu-id="c116a-202">On hello **Create hybrid connection** blade:</span></span>
   
   * <span data-ttu-id="c116a-203">För **namn**, ange ett namn för hello-anslutning.</span><span class="sxs-lookup"><span data-stu-id="c116a-203">For **Name**, provide a name for hello connection.</span></span>
   * <span data-ttu-id="c116a-204">För **värdnamn**, ange hello datornamnet på värddatorn för SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c116a-204">For **Hostname**, enter hello computer name of your SQL Server host computer.</span></span>
   * <span data-ttu-id="c116a-205">För **Port**, ange 1433 (hello-standardporten för SQL Server).</span><span class="sxs-lookup"><span data-stu-id="c116a-205">For **Port**, enter 1433 (hello default port for SQL Server).</span></span>
   * <span data-ttu-id="c116a-206">Klicka på **BizTalk Service** > **ny BizTalk-tjänst** och ange ett namn för hello BizTalk-tjänst.</span><span class="sxs-lookup"><span data-stu-id="c116a-206">Click **BizTalk Service** > **New BizTalk Service** and enter a name for hello BizTalk service.</span></span>
     
     ![Skapa en hybridanslutning][TwinCreateHCBlades]
4. <span data-ttu-id="c116a-208">Klicka på **OK** två gånger.</span><span class="sxs-lookup"><span data-stu-id="c116a-208">Click **OK** twice.</span></span>
   
    <span data-ttu-id="c116a-209">När hello processen har slutförts hello **meddelanden** område blinkar en grön **lyckade** och hello **hybridanslutning** bladet visar hello ny hybridanslutning med Hej status som **inte ansluten**.</span><span class="sxs-lookup"><span data-stu-id="c116a-209">When hello process completes, hello **Notifications** area will flash a green **SUCCESS** and hello **Hybrid connection** blade will show hello new hybrid connection with hello status as **Not connected**.</span></span>
   
    ![En hybridanslutning skapas][CreateHCOneConnectionCreated]

<span data-ttu-id="c116a-211">Du har nu slutfört en viktig del av hello molninfrastruktur hybrid-anslutning.</span><span class="sxs-lookup"><span data-stu-id="c116a-211">At this point, you have completed an important part of hello cloud hybrid connection infrastructure.</span></span> <span data-ttu-id="c116a-212">Därefter skapar du en motsvarande typ lokalt.</span><span class="sxs-lookup"><span data-stu-id="c116a-212">Next, you will create a corresponding on-premises piece.</span></span>

<a name="InstallHCM"></a>

## <a name="d-install-hello-on-premises-hybrid-connection-manager-toocomplete-hello-connection"></a><span data-ttu-id="c116a-213">D.</span><span class="sxs-lookup"><span data-stu-id="c116a-213">D.</span></span> <span data-ttu-id="c116a-214">Installera hello lokal Hybridanslutningshanterare toocomplete hello anslutning</span><span class="sxs-lookup"><span data-stu-id="c116a-214">Install hello on-premises Hybrid Connection Manager toocomplete hello connection</span></span>
[!INCLUDE [app-service-hybrid-connections-manager-install](../../includes/app-service-hybrid-connections-manager-install.md)]

<span data-ttu-id="c116a-215">Nu hello hybrid anslutning infrastrukturen är klar skapar du ett program som använder den.</span><span class="sxs-lookup"><span data-stu-id="c116a-215">Now that hello hybrid connection infrastructure is complete, you will create a web application that uses it.</span></span>

<a name="CreateASPNET"></a>

## <a name="e-create-a-basic-aspnet-web-project-edit-hello-database-connection-string-and-run-hello-project-locally"></a><span data-ttu-id="c116a-216">E.</span><span class="sxs-lookup"><span data-stu-id="c116a-216">E.</span></span> <span data-ttu-id="c116a-217">Skapa ett grundläggande webbprojekt i ASP.NET, redigera hello databasanslutningssträng och kör hello projektet lokalt</span><span class="sxs-lookup"><span data-stu-id="c116a-217">Create a basic ASP.NET web project, edit hello database connection string, and run hello project locally</span></span>
### <a name="create-a-basic-aspnet-project"></a><span data-ttu-id="c116a-218">Skapa ett grundläggande ASP.NET-projekt</span><span class="sxs-lookup"><span data-stu-id="c116a-218">Create a basic ASP.NET project</span></span>
1. <span data-ttu-id="c116a-219">I Visual Studio på hello **filen** menyn skapa ett nytt projekt:</span><span class="sxs-lookup"><span data-stu-id="c116a-219">In Visual Studio, on hello **File** menu, create a new Project:</span></span>
   
    ![Nytt projekt i Visual Studio][HCVSNewProject]
2. <span data-ttu-id="c116a-221">I hello **mallar** avsnitt i hello **nytt projekt** markerar **Web** och välj **ASP.NET-webbprogram**, och klicka sedan på  **OK**.</span><span class="sxs-lookup"><span data-stu-id="c116a-221">In hello **Templates** section of hello **New Project** dialog, select **Web** and choose **ASP.NET Web Application**, and then click **OK**.</span></span>
   
    ![Välj ASP.NET-webbprogram][HCVSChooseASPNET]
3. <span data-ttu-id="c116a-223">I hello **nytt ASP.NET-projekt** dialogrutan Välj **MVC**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c116a-223">In hello **New ASP.NET Project** dialog, choose **MVC**, and then click **OK**.</span></span>
   
    ![Välja MVC][HCVSChooseMVC]
4. <span data-ttu-id="c116a-225">Hello programmet viktigt-sidan visas när hello projektet har skapats.</span><span class="sxs-lookup"><span data-stu-id="c116a-225">When hello project has been created, hello application readme page appears.</span></span> <span data-ttu-id="c116a-226">Kör inte hello webbprojekt ännu.</span><span class="sxs-lookup"><span data-stu-id="c116a-226">Do not run hello web project yet.</span></span>
   
    ![Viktigt-sida][HCVSReadmePage]

### <a name="edit-hello-database-connection-string-for-hello-application"></a><span data-ttu-id="c116a-228">Redigera hello databasanslutningssträng för hello program</span><span class="sxs-lookup"><span data-stu-id="c116a-228">Edit hello database connection string for hello application</span></span>
<span data-ttu-id="c116a-229">I det här steget kan du redigera hello anslutningssträngen som talar om ditt program där toofind dina lokala SQLServer Express databasen.</span><span class="sxs-lookup"><span data-stu-id="c116a-229">In this step, you edit hello connection string that tells your application where toofind your local SQL Server Express database.</span></span> <span data-ttu-id="c116a-230">hello anslutningssträngen är i hello programmets Web.config-fil som innehåller konfigurationsinformation för hello-programmet.</span><span class="sxs-lookup"><span data-stu-id="c116a-230">hello connection string is in hello application's Web.config file, which contains configuration information for hello application.</span></span>

> [!NOTE]
> <span data-ttu-id="c116a-231">tooensure som används för programmet hello-databas som du skapade i SQL Server Express och inte hello något i Visual Studio standard LocalDB, är det viktigt att du har slutfört det här steget innan du kör projektet.</span><span class="sxs-lookup"><span data-stu-id="c116a-231">tooensure that your application uses hello database that you created in SQL Server Express, and not hello one in Visual Studio's default LocalDB, it is important that you complete this step before running your project.</span></span>
> 
> 

1. <span data-ttu-id="c116a-232">I Solution Explorer dubbelklickar du på hello Web.config-filen.</span><span class="sxs-lookup"><span data-stu-id="c116a-232">In Solution Explorer, double-click hello Web.config file.</span></span>
   
    ![Web.config][HCVSChooseWebConfig]
2. <span data-ttu-id="c116a-234">Redigera hello **connectionStrings** avsnittet toopoint toohello SQL Server-databas på den lokala datorn hello syntaxen i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="c116a-234">Edit hello **connectionStrings** section toopoint toohello SQL Server database on your local machine, following hello syntax in hello following example:</span></span>
   
    ![Anslutningssträng][HCVSConnectionString]
   
    <span data-ttu-id="c116a-236">När du skriver hello anslutningssträngen, Kom ihåg hello följande:</span><span class="sxs-lookup"><span data-stu-id="c116a-236">When composing hello connection string, keep in mind hello following:</span></span>
   
   * <span data-ttu-id="c116a-237">Om du ansluter tooa namngivna instans i stället för en standardinstans (till exempel YourServer\SQLEXPRESS), måste du konfigurera din SQL Server-toouse statiska portar.</span><span class="sxs-lookup"><span data-stu-id="c116a-237">If you are connecting tooa named instance instead of a default instance (for example, YourServer\SQLEXPRESS), you must configure your SQL Server toouse static ports.</span></span> <span data-ttu-id="c116a-238">Information om hur du konfigurerar statiska portar finns [hur tooconfigure SQL Server toolisten på en viss port](http://support.microsoft.com/kb/823938).</span><span class="sxs-lookup"><span data-stu-id="c116a-238">For information on configuring static ports, see [How tooconfigure SQL Server toolisten on a specific port](http://support.microsoft.com/kb/823938).</span></span> <span data-ttu-id="c116a-239">Som standard använder UDP- och dynamiska portar, vilket inte stöds av Hybridanslutningar för namngivna instanser.</span><span class="sxs-lookup"><span data-stu-id="c116a-239">By default, named instances use UDP and dynamic ports, which are not supported by Hybrid Connections.</span></span>
   * <span data-ttu-id="c116a-240">Vi rekommenderar att du anger hello port (1433 som standard, som visas i exemplet hello) på hello anslutningssträngen så att du kan vara säker på att dina lokala SQL Server har TCP aktiverad och använder hello rätt port.</span><span class="sxs-lookup"><span data-stu-id="c116a-240">It is recommended that you specify hello port (1433 by default, as shown in hello example) on hello connection string so that you can be sure that your local SQL Server has TCP enabled and is using hello correct port.</span></span>
   * <span data-ttu-id="c116a-241">Kom ihåg toouse SQL Server-autentisering tooconnect, att ange hello användar-ID och lösenord i anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="c116a-241">Remember toouse SQL Server Authentication tooconnect, specifying hello user ID and password in your connection string.</span></span>
3. <span data-ttu-id="c116a-242">Klicka på **spara** i Visual Studio toosave hello Web.config-fil.</span><span class="sxs-lookup"><span data-stu-id="c116a-242">Click **Save** in Visual Studio toosave hello Web.config file.</span></span>

### <a name="run-hello-project-locally-and-register-a-new-user"></a><span data-ttu-id="c116a-243">Kör hello projektet lokalt och registrera en ny användare</span><span class="sxs-lookup"><span data-stu-id="c116a-243">Run hello project locally and register a new user</span></span>
1. <span data-ttu-id="c116a-244">Nu kan köra lokalt webbprojektet nya genom att klicka på bläddringsknappen hello under felsökning.</span><span class="sxs-lookup"><span data-stu-id="c116a-244">Now, run your new web project locally by clicking hello browse button under Debug.</span></span> <span data-ttu-id="c116a-245">Det här exemplet använder Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="c116a-245">This example uses Internet Explorer.</span></span>
   
    ![Kör projektet][HCVSRunProject]
2. <span data-ttu-id="c116a-247">Välj på hello övre högra hörnet av hello standardwebbsidan, **registrera** tooregister ett nytt konto:</span><span class="sxs-lookup"><span data-stu-id="c116a-247">On hello upper right of hello default web page, choose **Register** tooregister a new account:</span></span>
   
    ![Registrera ett nytt konto][HCVSRegisterLocally]
3. <span data-ttu-id="c116a-249">Ange ett användarnamn och lösenord:</span><span class="sxs-lookup"><span data-stu-id="c116a-249">Enter a user name and password:</span></span>
   
    ![Ange användarnamn och lösenord][HCVSCreateNewAccount]
   
    <span data-ttu-id="c116a-251">Detta skapar automatiskt en databas på den lokala SQL-servern som innehåller information om hello gruppmedlemskap för ditt program.</span><span class="sxs-lookup"><span data-stu-id="c116a-251">This automatically creates a database on your local SQL Server that holds hello membership information for your application.</span></span> <span data-ttu-id="c116a-252">En av tabellerna hello (**dbo. AspNetUsers**) innehåller web app användarautentiseringsuppgifter som hello som du angav.</span><span class="sxs-lookup"><span data-stu-id="c116a-252">One of hello tables (**dbo.AspNetUsers**) holds web app user credentials like hello ones that you just entered.</span></span> <span data-ttu-id="c116a-253">Den här tabellen visas senare i självstudiekursen hello.</span><span class="sxs-lookup"><span data-stu-id="c116a-253">You will see this table later in hello tutorial.</span></span>
4. <span data-ttu-id="c116a-254">Stäng webbläsarfönstret för hello av hello standardwebbsidan.</span><span class="sxs-lookup"><span data-stu-id="c116a-254">Close hello browser window of hello default web page.</span></span> <span data-ttu-id="c116a-255">Detta stoppar hello program i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c116a-255">This stops hello application in Visual Studio.</span></span>

<span data-ttu-id="c116a-256">Du är nu redo för nästa steg i hello, vilket är toopublish hello programmet tooAzure och testa den.</span><span class="sxs-lookup"><span data-stu-id="c116a-256">You are now ready for hello next step, which is toopublish hello application tooAzure and test it.</span></span>

<a name="PubNTest"></a>

## <a name="f-publish-hello-web-application-tooazure-and-test-it"></a><span data-ttu-id="c116a-257">F.</span><span class="sxs-lookup"><span data-stu-id="c116a-257">F.</span></span> <span data-ttu-id="c116a-258">Publicera hello web application tooAzure och testa den</span><span class="sxs-lookup"><span data-stu-id="c116a-258">Publish hello web application tooAzure and test it</span></span>
<span data-ttu-id="c116a-259">Nu kan du publicera dina program tooyour App Service web-appen och sedan testa den toosee hur hello hybridanslutning som du tidigare har konfigurerat är att använda tooconnect web app toohello databasen på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="c116a-259">Now, you'll publish your application tooyour App Service web app and then test it toosee how hello hybrid connection you configured earlier is being used tooconnect your web app toohello database on your local machine.</span></span>

### <a name="publish-hello-web-application"></a><span data-ttu-id="c116a-260">Publicera hello-webbprogram</span><span class="sxs-lookup"><span data-stu-id="c116a-260">Publish hello web application</span></span>
1. <span data-ttu-id="c116a-261">Du kan ladda ned din publiceringsprofil för hello Apptjänst-webbapp i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c116a-261">You can download your publishing profile for hello App Service web app in hello Azure Portal.</span></span> <span data-ttu-id="c116a-262">Hello bladet för ditt webbprogram, klicka på **Get publiceringsprofil**, och sedan spara hello filen tooyour dator.</span><span class="sxs-lookup"><span data-stu-id="c116a-262">On hello blade for your web app, click **Get publish profile**, and then save hello file tooyour computer.</span></span>
   
    ![Hämta Publicera profil][PortalDownloadPublishProfile]
   
    <span data-ttu-id="c116a-264">Därefter importerar du den här filen till ditt webbprogram för Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c116a-264">Next, you will import this file into your Visual Studio web application.</span></span>
2. <span data-ttu-id="c116a-265">Högerklicka på hello projektnamnet i Solution Explorer i Visual Studio och välj **publicera**.</span><span class="sxs-lookup"><span data-stu-id="c116a-265">In Visual Studio, right-click hello project name in Solution Explorer and select **Publish**.</span></span>
   
    ![Välj publicera][HCVSRightClickProjectSelectPublish]
3. <span data-ttu-id="c116a-267">I hello **Publicera webbplats** dialogrutan på hello **profil** , Välj **importera**.</span><span class="sxs-lookup"><span data-stu-id="c116a-267">In hello **Publish Web** dialog, on hello **Profile** tab, choose **Import**.</span></span>
   
    ![Importera][HCVSPublishWebDialogImport]
4. <span data-ttu-id="c116a-269">Bläddra tooyour hämtas publiceringsprofilen markerar du den och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c116a-269">Browse tooyour downloaded publishing profile, select it, and then click **OK**.</span></span>
   
    ![Bläddra tooprofile][HCVSBrowseToImportPubProfile]
5. <span data-ttu-id="c116a-271">Din publiceringsinformation har importerats och visar på hello **anslutning** fliken hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c116a-271">Your publishing information is imported and displays on hello **Connection** tab of hello dialog.</span></span>
   
    ![Klicka på Publicera][HCVSClickPublish]
   
    <span data-ttu-id="c116a-273">Klicka på **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="c116a-273">Click **Publish**.</span></span>
   
    <span data-ttu-id="c116a-274">När publiceringen är klar startar webbläsaren och visar nu välkända ASP.NET-programmet--förutom att den är nu aktiv i hello Azure-molnet!</span><span class="sxs-lookup"><span data-stu-id="c116a-274">When publishing completes, your browser will launch and show your now familiar ASP.NET application -- except that now it is live in hello Azure cloud!</span></span>

<span data-ttu-id="c116a-275">Därefter använder du din live web application toosee dess Hybridanslutning i åtgärden.</span><span class="sxs-lookup"><span data-stu-id="c116a-275">Next, you will use your live web application toosee its Hybrid Connection in action.</span></span>

### <a name="test-hello-completed-web-application-on-azure"></a><span data-ttu-id="c116a-276">Testa hello slutförts webbprogram på Azure</span><span class="sxs-lookup"><span data-stu-id="c116a-276">Test hello completed web application on Azure</span></span>
1. <span data-ttu-id="c116a-277">Hello överst höger på din webbsida på Azure, Välj **logga in**.</span><span class="sxs-lookup"><span data-stu-id="c116a-277">On hello top right of your web page on Azure, choose **Log in**.</span></span>
   
    ![Testa inloggningen][HCTestLogIn]
2. <span data-ttu-id="c116a-279">Din App Service webbappen är nu ansluten tooyour webbprogrammets medlemskapsdatabasen på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="c116a-279">Your App Service web app is now connected tooyour web application's membership database on your local machine.</span></span> <span data-ttu-id="c116a-280">tooverify detta, logga in med samma autentiseringsuppgifter som du angett i hello lokal databas tidigare hello.</span><span class="sxs-lookup"><span data-stu-id="c116a-280">tooverify this, log in with hello same credentials that you entered in hello local database earlier.</span></span>
   
    ![Hello hälsning][HCTestHelloContoso]
3. <span data-ttu-id="c116a-282">toofurther testa nya hybridanslutningen, logga ut från ditt webbprogram för Azure och registrera som en annan användare.</span><span class="sxs-lookup"><span data-stu-id="c116a-282">toofurther test your new hybrid connection, log off of your Azure web application and register as another user.</span></span> <span data-ttu-id="c116a-283">Ange ett nytt användarnamn och lösenord och klicka sedan på **registrera**.</span><span class="sxs-lookup"><span data-stu-id="c116a-283">Provide a new user name and password, and then click **Register**.</span></span>
   
    ![Testa registrera en annan användare][HCTestRegisterRelecloud]
4. <span data-ttu-id="c116a-285">tooverify att hello den nya användarens autentiseringsuppgifter har lagrats i den lokala databasen via din hybridanslutning Öppna SQL Management Studio på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="c116a-285">tooverify that hello new user's credentials have been stored in your local database through your hybrid connection, open SQL Management Studio on your local computer.</span></span> <span data-ttu-id="c116a-286">I Object Explorer, expandera hello **MembershipDB** databasen och expandera sedan **tabeller**.</span><span class="sxs-lookup"><span data-stu-id="c116a-286">In Object Explorer, expand hello **MembershipDB** database, and then expand **Tables**.</span></span> <span data-ttu-id="c116a-287">Högerklicka på hello **dbo. AspNetUsers** medlemskap tabell och välj **Välj de 1000 översta raderna** tooview hello resultat.</span><span class="sxs-lookup"><span data-stu-id="c116a-287">Right-click hello **dbo.AspNetUsers** membership table and choose **Select Top 1000 Rows** tooview hello results.</span></span>
   
    ![Visa hello resultat][HCTestSSMSTree]
5. <span data-ttu-id="c116a-289">Medlemskap i lokal tabellen visar nu båda kontona - hello skapats lokalt och hello som du skapade i hello Azure-molnet.</span><span class="sxs-lookup"><span data-stu-id="c116a-289">Your local membership table now shows both accounts - hello one that you created locally, and hello one that you created in hello Azure cloud.</span></span> <span data-ttu-id="c116a-290">hello som du skapade i hello molnet har sparats tooyour lokala-databasen via funktionen för Azures Hybridanslutning.</span><span class="sxs-lookup"><span data-stu-id="c116a-290">hello one that you created in hello cloud has been saved tooyour on-premises database through Azure's Hybrid Connection feature.</span></span>
   
    ![Registrerade användare i en lokal databas][HCTestShowMemberDb]

<span data-ttu-id="c116a-292">Du har nu skapats och distribuerats ASP.NET-webbprogram som använder en hybridanslutning mellan ett webbprogram i hello Azure-molnet och en lokal SQL Server-databas.</span><span class="sxs-lookup"><span data-stu-id="c116a-292">You have now created and deployed an ASP.NET web application that uses a hybrid connection between a web app in hello Azure cloud and an on-premises SQL Server database.</span></span> <span data-ttu-id="c116a-293">Grattis!</span><span class="sxs-lookup"><span data-stu-id="c116a-293">Congratulations!</span></span>

## <a name="see-also"></a><span data-ttu-id="c116a-294">Se även</span><span class="sxs-lookup"><span data-stu-id="c116a-294">See Also</span></span>
[<span data-ttu-id="c116a-295">Översikt över hybrid-anslutningar</span><span class="sxs-lookup"><span data-stu-id="c116a-295">Hybrid Connections overview</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=397274)

[<span data-ttu-id="c116a-296">Josh Twist introducerar hybridanslutningar (Channel 9 video)</span><span class="sxs-lookup"><span data-stu-id="c116a-296">Josh Twist introduces hybrid connections (Channel 9 video)</span></span>](http://channel9.msdn.com/Shows/Azure-Friday/Josh-Twist-introduces-hybrid-connections)

[<span data-ttu-id="c116a-297">Översikt över hybrid-anslutningar</span><span class="sxs-lookup"><span data-stu-id="c116a-297">Hybrid Connections overview</span></span>](/services/biztalk-services/)

[<span data-ttu-id="c116a-298">BizTalk-tjänst: Instrumentpanelen, övervaka, skala, konfigurera och Hybridanslutning flikar</span><span class="sxs-lookup"><span data-stu-id="c116a-298">BizTalk Services: Dashboard, Monitor, Scale, Configure, and Hybrid Connection tabs</span></span>](../biztalk-services/biztalk-dashboard-monitor-scale-tabs.md)

[<span data-ttu-id="c116a-299">Skapa en verklig Hybridmoln med sömlös programmet Portability (Channel 9 video)</span><span class="sxs-lookup"><span data-stu-id="c116a-299">Building a Real-World Hybrid Cloud with Seamless Application Portability (Channel 9 video)</span></span>](http://channel9.msdn.com/events/TechEd/NorthAmerica/2014/DCIM-B323#fbid=)

[<span data-ttu-id="c116a-300">Åtkomst till lokala resurser genom att använda hybridanslutningar i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="c116a-300">Access on-premises resources using hybrid connections in Azure App Service</span></span>](web-sites-hybrid-connection-get-started.md)

[<span data-ttu-id="c116a-301">Översikt över ASP.NET-identitet</span><span class="sxs-lookup"><span data-stu-id="c116a-301">ASP.NET Identity Overview</span></span>](http://www.asp.net/identity)

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
