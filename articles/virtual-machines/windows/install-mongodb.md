---
title: "aaaInstall MongoDB på en Windows-dator i Azure | Microsoft Docs"
description: "Lär dig hur du skapar tooinstall MongoDB på en Azure-dator som kör Windows Server 2012 R2 med hello Resource Manager-modellen."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 53faf630-8da5-4955-8d0b-6e829bf30cba
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: becd2c607d098e2bc806139e03f2c42f1f01f6f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-configure-mongodb-on-a-windows-vm-in-azure"></a><span data-ttu-id="40363-103">Installera och konfigurera MongoDB på en Windows-dator i Azure</span><span class="sxs-lookup"><span data-stu-id="40363-103">Install and configure MongoDB on a Windows VM in Azure</span></span>
<span data-ttu-id="40363-104">[MongoDB](http://www.mongodb.org) är en populär öppen källkod, högpresterande NoSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="40363-104">[MongoDB](http://www.mongodb.org) is a popular open-source, high-performance NoSQL database.</span></span> <span data-ttu-id="40363-105">Den här artikeln visar hur du installerar och konfigurerar MongoDB på en Windows Server 2012 R2 virtuell dator (VM) i Azure.</span><span class="sxs-lookup"><span data-stu-id="40363-105">This article guides you through installing and configuring MongoDB on a Windows Server 2012 R2 virtual machine (VM) in Azure.</span></span> <span data-ttu-id="40363-106">Du kan också [installera MongoDB på en Linux-VM i Azure](../linux/install-mongodb.md).</span><span class="sxs-lookup"><span data-stu-id="40363-106">You can also [install MongoDB on a Linux VM in Azure](../linux/install-mongodb.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="40363-107">Krav</span><span class="sxs-lookup"><span data-stu-id="40363-107">Prerequisites</span></span>
<span data-ttu-id="40363-108">Innan du installerar och konfigurerar MongoDB du behöver toocreate en virtuell dator och, helst lägga till en disk tooit för data.</span><span class="sxs-lookup"><span data-stu-id="40363-108">Before you install and configure MongoDB, you need toocreate a VM and, ideally, add a data disk tooit.</span></span> <span data-ttu-id="40363-109">Se följande artiklar toocreate en virtuell dator hello och Lägg till en datadisk:</span><span class="sxs-lookup"><span data-stu-id="40363-109">See hello following articles toocreate a VM and add a data disk:</span></span>

* <span data-ttu-id="40363-110">Skapa en virtuell Windows Server-dator med hjälp av [hello Azure-portalen](quick-create-portal.md) eller [Azure PowerShell](quick-create-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="40363-110">Create a Windows Server VM using [hello Azure portal](quick-create-portal.md) or [Azure PowerShell](quick-create-powershell.md).</span></span>
* <span data-ttu-id="40363-111">Koppla en data disk tooa virtuella Windows Server-datorn med [hello Azure-portalen](attach-managed-disk-portal.md) eller [Azure PowerShell](attach-disk-ps.md).</span><span class="sxs-lookup"><span data-stu-id="40363-111">Attach a data disk tooa Windows Server VM using [hello Azure portal](attach-managed-disk-portal.md) or [Azure PowerShell](attach-disk-ps.md).</span></span>

<span data-ttu-id="40363-112">toobegin installera och konfigurera MongoDB, [inloggning tooyour Windows Server VM](connect-logon.md) med hjälp av fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="40363-112">toobegin installing and configuring MongoDB, [log on tooyour Windows Server VM](connect-logon.md) by using Remote Desktop.</span></span>

## <a name="install-mongodb"></a><span data-ttu-id="40363-113">Installera MongoDB</span><span class="sxs-lookup"><span data-stu-id="40363-113">Install MongoDB</span></span>
> [!IMPORTANT]
> <span data-ttu-id="40363-114">MongoDB säkerhetsfunktioner, till exempel autentisering och IP-adress bindningen är inte aktiverade som standard.</span><span class="sxs-lookup"><span data-stu-id="40363-114">MongoDB security features, such as authentication and IP address binding, are not enabled by default.</span></span> <span data-ttu-id="40363-115">Säkerhetsfunktioner ska aktiveras innan du distribuerar MongoDB tooa produktionsmiljön.</span><span class="sxs-lookup"><span data-stu-id="40363-115">Security features should be enabled before deploying MongoDB tooa production environment.</span></span> <span data-ttu-id="40363-116">Mer information finns i [MongoDB säkerhet och autentisering](http://www.mongodb.org/display/DOCS/Security+and+Authentication).</span><span class="sxs-lookup"><span data-stu-id="40363-116">For more information, see [MongoDB Security and Authentication](http://www.mongodb.org/display/DOCS/Security+and+Authentication).</span></span>


1. <span data-ttu-id="40363-117">När du har anslutit tooyour VM med hjälp av fjärrskrivbord, öppna Internet Explorer från hello **starta** menyn på hello VM.</span><span class="sxs-lookup"><span data-stu-id="40363-117">After you've connected tooyour VM using Remote Desktop, open Internet Explorer from hello **Start** menu on hello VM.</span></span>
2. <span data-ttu-id="40363-118">Välj **Använd rekommenderade inställningar för säkerhet, sekretess och kompatibilitet** när Internet Explorer först öppnar och på **OK**.</span><span class="sxs-lookup"><span data-stu-id="40363-118">Select **Use recommended security, privacy, and compatibility settings** when Internet Explorer first opens, and click **OK**.</span></span>
3. <span data-ttu-id="40363-119">Förbättrad säkerhetskonfiguration i Internet Explorer är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="40363-119">Internet Explorer enhanced security configuration is enabled by default.</span></span> <span data-ttu-id="40363-120">Lägg till hello MongoDB webbplats toohello listan över tillåtna webbplatser:</span><span class="sxs-lookup"><span data-stu-id="40363-120">Add hello MongoDB website toohello list of allowed sites:</span></span>
   
   * <span data-ttu-id="40363-121">Välj hello **verktyg** i hello övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="40363-121">Select hello **Tools** icon in hello upper-right corner.</span></span>
   * <span data-ttu-id="40363-122">I **Internetalternativ**väljer hello **säkerhet** och välj hello **tillförlitliga platser** ikon.</span><span class="sxs-lookup"><span data-stu-id="40363-122">In **Internet Options**, select hello **Security** tab, and then select hello **Trusted Sites** icon.</span></span>
   * <span data-ttu-id="40363-123">Klicka på hello **platser** knappen.</span><span class="sxs-lookup"><span data-stu-id="40363-123">Click hello **Sites** button.</span></span> <span data-ttu-id="40363-124">Lägg till *https://\*. mongodb.org* toohello lista över betrodda platser och stänger sedan hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="40363-124">Add *https://\*.mongodb.org* toohello list of trusted sites, and then close hello dialog box.</span></span>
     
     ![Konfigurera säkerhetsinställningar för Internet Explorer](./media/install-mongodb/configure-internet-explorer-security.png)
4. <span data-ttu-id="40363-126">Bläddra toohello [MongoDB - hämtningar](http://www.mongodb.org/downloads) sida (http://www.mongodb.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="40363-126">Browse toohello [MongoDB - Downloads](http://www.mongodb.org/downloads) page (http://www.mongodb.org/downloads).</span></span>
5. <span data-ttu-id="40363-127">Om det behövs, Välj hello **Community Server** edition och välj sedan hello senaste aktuella stabil versionen för Windows Server 2008 R2 64-bitars och senare.</span><span class="sxs-lookup"><span data-stu-id="40363-127">If needed, select hello **Community Server** edition and then select hello latest current stable release for Windows Server 2008 R2 64-bit and later.</span></span> <span data-ttu-id="40363-128">toodownload Hej installationsprogrammet, klickar du på **DOWNLOAD (msi)**.</span><span class="sxs-lookup"><span data-stu-id="40363-128">toodownload hello installer, click **DOWNLOAD (msi)**.</span></span>
   
    ![Hämta MongoDB installer](./media/install-mongodb/download-mongodb.png)
   
    <span data-ttu-id="40363-130">Kör installationsprogrammet för hello när hello hämtningen är slutförd.</span><span class="sxs-lookup"><span data-stu-id="40363-130">Run hello installer after hello download is complete.</span></span>
6. <span data-ttu-id="40363-131">Läs och godkänn licensavtalet för hello.</span><span class="sxs-lookup"><span data-stu-id="40363-131">Read and accept hello license agreement.</span></span> <span data-ttu-id="40363-132">När du uppmanas, välja **Slutför** installera.</span><span class="sxs-lookup"><span data-stu-id="40363-132">When you're prompted, select **Complete** install.</span></span>
7. <span data-ttu-id="40363-133">På sista hello-skärmen klickar du på **installera**.</span><span class="sxs-lookup"><span data-stu-id="40363-133">On hello final screen, click **Install**.</span></span>

## <a name="configure-hello-vm-and-mongodb"></a><span data-ttu-id="40363-134">Konfigurera hello VM och MongoDB</span><span class="sxs-lookup"><span data-stu-id="40363-134">Configure hello VM and MongoDB</span></span>
1. <span data-ttu-id="40363-135">hello sökvägsvariabler uppdateras inte hello MongoDB Installer.</span><span class="sxs-lookup"><span data-stu-id="40363-135">hello path variables are not updated by hello MongoDB installer.</span></span> <span data-ttu-id="40363-136">Utan hello MongoDB `bin` plats i path-variabeln måste toospecify hello fullständig sökväg varje gång du använder en körbar fil MongoDB.</span><span class="sxs-lookup"><span data-stu-id="40363-136">Without hello MongoDB `bin` location in your path variable, you need toospecify hello full path each time you use a MongoDB executable.</span></span> <span data-ttu-id="40363-137">tooadd hello plats tooyour path-variabeln:</span><span class="sxs-lookup"><span data-stu-id="40363-137">tooadd hello location tooyour path variable:</span></span>
   
   * <span data-ttu-id="40363-138">Högerklicka på hello **starta** menyn och välj **System**.</span><span class="sxs-lookup"><span data-stu-id="40363-138">Right-click hello **Start** menu, and select **System**.</span></span>
   * <span data-ttu-id="40363-139">Klicka på **avancerade systeminställningar**, och klicka sedan på **miljövariabler**.</span><span class="sxs-lookup"><span data-stu-id="40363-139">Click **Advanced system settings**, and then click **Environment Variables**.</span></span>
   * <span data-ttu-id="40363-140">Under **systemvariabler**väljer **sökväg**, och klicka sedan på **redigera**.</span><span class="sxs-lookup"><span data-stu-id="40363-140">Under **System variables**, select **Path**, and then click **Edit**.</span></span>
     
     ![Konfigurera sökvägsvariabler](./media/install-mongodb/configure-path-variables.png)
     
     <span data-ttu-id="40363-142">Lägg till hello sökvägen tooyour MongoDB `bin` mapp.</span><span class="sxs-lookup"><span data-stu-id="40363-142">Add hello path tooyour MongoDB `bin` folder.</span></span> <span data-ttu-id="40363-143">MongoDB installeras normalt i *C:\Program Files\MongoDB*.</span><span class="sxs-lookup"><span data-stu-id="40363-143">MongoDB is typically installed in *C:\Program Files\MongoDB*.</span></span> <span data-ttu-id="40363-144">Kontrollera hello installationssökvägen på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="40363-144">Verify hello installation path on your VM.</span></span> <span data-ttu-id="40363-145">hello följande exempel läggs hello standard MongoDB installera platsen toohello `PATH` variabeln:</span><span class="sxs-lookup"><span data-stu-id="40363-145">hello following example adds hello default MongoDB install location toohello `PATH` variable:</span></span>
     
     ```
     ;C:\Program Files\MongoDB\Server\3.2\bin
     ```
     
     > [!NOTE]
     > <span data-ttu-id="40363-146">Vara säker på att tooadd hello inledande semikolon (`;`) att du lägger till en plats tooyour tooindicate `PATH` variabeln.</span><span class="sxs-lookup"><span data-stu-id="40363-146">Be sure tooadd hello leading semicolon (`;`) tooindicate that you are adding a location tooyour `PATH` variable.</span></span>

2. <span data-ttu-id="40363-147">Skapa MongoDB data och loggfilen kataloger på hårddisken data.</span><span class="sxs-lookup"><span data-stu-id="40363-147">Create MongoDB data and log directories on your data disk.</span></span> <span data-ttu-id="40363-148">Från hello **starta** väljer du **kommandotolk**.</span><span class="sxs-lookup"><span data-stu-id="40363-148">From hello **Start** menu, select **Command Prompt**.</span></span> <span data-ttu-id="40363-149">följande exempel hello skapa hello kataloger på enhet F:</span><span class="sxs-lookup"><span data-stu-id="40363-149">hello following examples create hello directories on drive F:</span></span>
   
    ```
    mkdir F:\MongoData
    mkdir F:\MongoLogs
    ```
3. <span data-ttu-id="40363-150">Starta en MongoDB-instansen med hello följande kommando, justera hello sökvägen tooyour data och logga kataloger i enlighet med detta:</span><span class="sxs-lookup"><span data-stu-id="40363-150">Start a MongoDB instance with hello following command, adjusting hello path tooyour data and log directories accordingly:</span></span>
   
    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log
    ```
   
    <span data-ttu-id="40363-151">Det kan ta flera minuter för MongoDB tooallocate hello journalfiler och börja lyssna efter anslutningar.</span><span class="sxs-lookup"><span data-stu-id="40363-151">It may take several minutes for MongoDB tooallocate hello journal files and start listening for connections.</span></span> <span data-ttu-id="40363-152">Alla loggmeddelanden är riktat toohello *F:\MongoLogs\mongolog.log* filen som `mongod.exe` servern startar och allokerar journal-filer.</span><span class="sxs-lookup"><span data-stu-id="40363-152">All log messages are directed toohello *F:\MongoLogs\mongolog.log* file as `mongod.exe` server starts and allocates journal files.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="40363-153">hello kommandotolk förblir fokuserar på den här uppgiften när MongoDB-instansen körs.</span><span class="sxs-lookup"><span data-stu-id="40363-153">hello command prompt stays focused on this task while your MongoDB instance is running.</span></span> <span data-ttu-id="40363-154">Lämna hello kommandotolk-fönster öppna toocontinue kör MongoDB.</span><span class="sxs-lookup"><span data-stu-id="40363-154">Leave hello command prompt window open toocontinue running MongoDB.</span></span> <span data-ttu-id="40363-155">Eller installera MongoDB som tjänst enligt anvisningarna i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="40363-155">Or, install MongoDB as service, as detailed in hello next step.</span></span>

4. <span data-ttu-id="40363-156">För en mer robusta MongoDB-upplevelse, installera hello `mongod.exe` som en tjänst.</span><span class="sxs-lookup"><span data-stu-id="40363-156">For a more robust MongoDB experience, install hello `mongod.exe` as a service.</span></span> <span data-ttu-id="40363-157">Skapa en tjänst innebär att du inte behöver tooleave Kommandotolken körs varje gång som du vill toouse MongoDB.</span><span class="sxs-lookup"><span data-stu-id="40363-157">Creating a service means you don't need tooleave a command prompt running each time you want toouse MongoDB.</span></span> <span data-ttu-id="40363-158">Skapa hello-tjänsten enligt följande justeras hello sökvägen tooyour data och loggfilen kataloger efter:</span><span class="sxs-lookup"><span data-stu-id="40363-158">Create hello service as follows, adjusting hello path tooyour data and log directories accordingly:</span></span>
   
    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log `
        --logappend  --install
    ```
   
    <span data-ttu-id="40363-159">hello skapar föregående kommando en tjänst med namnet MongoDB, med en beskrivning av ”Mongo databas”.</span><span class="sxs-lookup"><span data-stu-id="40363-159">hello preceding command creates a service named MongoDB, with a description of "Mongo DB".</span></span> <span data-ttu-id="40363-160">hello följande parametrar anges också:</span><span class="sxs-lookup"><span data-stu-id="40363-160">hello following parameters are also specified:</span></span>
   
   * <span data-ttu-id="40363-161">Hej `--dbpath` alternativet anger hello platsen för hello datakatalog.</span><span class="sxs-lookup"><span data-stu-id="40363-161">hello `--dbpath` option specifies hello location of hello data directory.</span></span>
   * <span data-ttu-id="40363-162">Hej `--logpath` alternativet måste vara används toospecify en loggfil, eftersom hello körs tjänsten inte har en toodisplay utdata från kommandot.</span><span class="sxs-lookup"><span data-stu-id="40363-162">hello `--logpath` option must be used toospecify a log file, because hello running service does not have a command window toodisplay output.</span></span>
   * <span data-ttu-id="40363-163">Hej `--logappend` alternativet anger att en omstart av hello tjänsten gör tooappend toohello befintliga utdataloggfilen.</span><span class="sxs-lookup"><span data-stu-id="40363-163">hello `--logappend` option specifies that a restart of hello service causes output tooappend toohello existing log file.</span></span>
   
   <span data-ttu-id="40363-164">toostart hello MongoDB, köra tjänsten hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="40363-164">toostart hello MongoDB service, run hello following command:</span></span>
   
    ```
    net start MongoDB
    ```
   
    <span data-ttu-id="40363-165">Mer information om hur du skapar hello MongoDB-tjänsten finns [konfigurera en Windows-tjänst för MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/#mongodb-as-a-windows-service).</span><span class="sxs-lookup"><span data-stu-id="40363-165">For more information about creating hello MongoDB service, see [Configure a Windows Service for MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/#mongodb-as-a-windows-service).</span></span>

## <a name="test-hello-mongodb-instance"></a><span data-ttu-id="40363-166">Testa hello MongoDB-instansen</span><span class="sxs-lookup"><span data-stu-id="40363-166">Test hello MongoDB instance</span></span>
<span data-ttu-id="40363-167">Med MongoDB körs som en enda instans eller installeras som en tjänst, kan du nu börja skapa och använda dina databaser.</span><span class="sxs-lookup"><span data-stu-id="40363-167">With MongoDB running as a single instance or installed as a service, you can now start creating and using your databases.</span></span> <span data-ttu-id="40363-168">toostart hello MongoDB administrativa shell, öppna en annan kommandotolk från hello **starta** -menyn och ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="40363-168">toostart hello MongoDB administrative shell, open another command prompt window from hello **Start** menu, and enter hello following command:</span></span>

```
mongo  
```

<span data-ttu-id="40363-169">Du kan ange hello databaser med hello `db` kommando.</span><span class="sxs-lookup"><span data-stu-id="40363-169">You can list hello databases with hello `db` command.</span></span> <span data-ttu-id="40363-170">Infoga data på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="40363-170">Insert some data as follows:</span></span>

```
db.foo.insert( { a : 1 } )
```

<span data-ttu-id="40363-171">Söka efter data på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="40363-171">Search for data as follows:</span></span>

```
db.foo.find()
```

<span data-ttu-id="40363-172">hello utdata är liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="40363-172">hello output is similar toohello following example:</span></span>

```
{ "_id" : "ObjectId("57f6a86cee873a6232d74842"), "a" : 1 }
```

<span data-ttu-id="40363-173">Avsluta hello `mongo` konsolen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="40363-173">Exit hello `mongo` console as follows:</span></span>

```
exit
```

## <a name="configure-firewall-and-network-security-group-rules"></a><span data-ttu-id="40363-174">Konfigurera brandväggen och Nätverkssäkerhetsgruppen regler</span><span class="sxs-lookup"><span data-stu-id="40363-174">Configure firewall and Network Security Group rules</span></span>
<span data-ttu-id="40363-175">Nu när MongoDB är installerade och körs, öppna en port i Windows-brandväggen så att du kan fjärransluta tooMongoDB.</span><span class="sxs-lookup"><span data-stu-id="40363-175">Now that MongoDB is installed and running, open a port in Windows Firewall so you can remotely connect tooMongoDB.</span></span> <span data-ttu-id="40363-176">toocreate en ny inkommande regel tooallow TCP-port 27017, öppna en administrativ PowerShell-Kommandotolken och ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="40363-176">toocreate a new inbound rule tooallow TCP port 27017, open an administrative PowerShell prompt and enter hello following command:</span></span>

```powerahell
New-NetFirewallRule `
    -DisplayName "Allow MongoDB" `
    -Direction Inbound `
    -Protocol TCP `
    -LocalPort 27017 `
    -Action Allow
```

<span data-ttu-id="40363-177">Du kan också skapa hello regel med hjälp av hello **Windows-brandväggen med avancerad säkerhet** grafiskt hanteringsverktyg.</span><span class="sxs-lookup"><span data-stu-id="40363-177">You can also create hello rule by using hello **Windows Firewall with Advanced Security** graphical management tool.</span></span> <span data-ttu-id="40363-178">Skapa en ny inkommande regel tooallow TCP-port 27017.</span><span class="sxs-lookup"><span data-stu-id="40363-178">Create a new inbound rule tooallow TCP port 27017.</span></span>

<span data-ttu-id="40363-179">Skapa en säkerhetsgrupp för nätverk regeln tooallow åtkomst tooMongoDB från utanför hello befintliga virtuella Azure-nätverket undernät vid behov.</span><span class="sxs-lookup"><span data-stu-id="40363-179">If needed, create a Network Security Group rule tooallow access tooMongoDB from outside of hello existing Azure virtual network subnet.</span></span> <span data-ttu-id="40363-180">Du kan skapa hello Nätverkssäkerhetsgruppen regler med hjälp av hello [Azure-portalen](nsg-quickstart-portal.md) eller [Azure PowerShell](nsg-quickstart-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="40363-180">You can create hello Network Security Group rules by using hello [Azure portal](nsg-quickstart-portal.md) or [Azure PowerShell](nsg-quickstart-powershell.md).</span></span> <span data-ttu-id="40363-181">Precis som med hello regler för Windows-brandväggen, kan TCP-port 27017 toohello virtuella nätverksgränssnittet av MongoDB-VM.</span><span class="sxs-lookup"><span data-stu-id="40363-181">As with hello Windows Firewall rules, allow TCP port 27017 toohello virtual network interface of your MongoDB VM.</span></span>

> [!NOTE]
> <span data-ttu-id="40363-182">TCP-port 27017 är hello standardport som används av MongoDB.</span><span class="sxs-lookup"><span data-stu-id="40363-182">TCP port 27017 is hello default port used by MongoDB.</span></span> <span data-ttu-id="40363-183">Du kan ändra den här porten med hjälp av hello `--port` parameter när du startar `mongod.exe` manuellt eller från en tjänst.</span><span class="sxs-lookup"><span data-stu-id="40363-183">You can change this port by using hello `--port` parameter when starting `mongod.exe` manually or from a service.</span></span> <span data-ttu-id="40363-184">Om du ändrar hello port gör att tooupdate hello Windows-brandväggen och Nätverkssäkerhetsgruppen regler i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="40363-184">If you change hello port, make sure tooupdate hello Windows Firewall and Network Security Group rules in hello preceding steps.</span></span>


## <a name="next-steps"></a><span data-ttu-id="40363-185">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="40363-185">Next steps</span></span>
<span data-ttu-id="40363-186">I kursen får du lärt dig hur tooinstall och konfigurera MongoDB på din Windows-VM.</span><span class="sxs-lookup"><span data-stu-id="40363-186">In this tutorial, you learned how tooinstall and configure MongoDB on your Windows VM.</span></span> <span data-ttu-id="40363-187">Du kan nu komma åt MongoDB på den virtuella datorn i Windows, av följande hello avancerade avsnitt i hello [MongoDB-dokumentation](https://docs.mongodb.com/manual/).</span><span class="sxs-lookup"><span data-stu-id="40363-187">You can now access MongoDB on your Windows VM, by following hello advanced topics in hello [MongoDB documentation](https://docs.mongodb.com/manual/).</span></span>

