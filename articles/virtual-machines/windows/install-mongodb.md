---
title: "Installera MongoDB på en Windows-dator i Azure | Microsoft Docs"
description: "Lär dig hur du installerar MongoDB på en Azure-dator som kör Windows Server 2012 R2 som skapats med Resource Manager-distributionsmodellen."
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
ms.openlocfilehash: db1a550b9273925b304fe4280f2a1b0e115f856d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="install-and-configure-mongodb-on-a-windows-vm-in-azure"></a><span data-ttu-id="8caa0-103">Installera och konfigurera MongoDB på en Windows-dator i Azure</span><span class="sxs-lookup"><span data-stu-id="8caa0-103">Install and configure MongoDB on a Windows VM in Azure</span></span>
<span data-ttu-id="8caa0-104">[MongoDB](http://www.mongodb.org) är en populär öppen källkod, högpresterande NoSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="8caa0-104">[MongoDB](http://www.mongodb.org) is a popular open-source, high-performance NoSQL database.</span></span> <span data-ttu-id="8caa0-105">Den här artikeln visar hur du installerar och konfigurerar MongoDB på en Windows Server 2012 R2 virtuell dator (VM) i Azure.</span><span class="sxs-lookup"><span data-stu-id="8caa0-105">This article guides you through installing and configuring MongoDB on a Windows Server 2012 R2 virtual machine (VM) in Azure.</span></span> <span data-ttu-id="8caa0-106">Du kan också [installera MongoDB på en Linux-VM i Azure](../linux/install-mongodb.md).</span><span class="sxs-lookup"><span data-stu-id="8caa0-106">You can also [install MongoDB on a Linux VM in Azure](../linux/install-mongodb.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8caa0-107">Krav</span><span class="sxs-lookup"><span data-stu-id="8caa0-107">Prerequisites</span></span>
<span data-ttu-id="8caa0-108">Innan du installerar och konfigurerar MongoDB, måste du skapa en virtuell dator och du bör lägga till en datadisk i den.</span><span class="sxs-lookup"><span data-stu-id="8caa0-108">Before you install and configure MongoDB, you need to create a VM and, ideally, add a data disk to it.</span></span> <span data-ttu-id="8caa0-109">Se följande artiklar för att skapa en virtuell dator och lägger till en datadisk:</span><span class="sxs-lookup"><span data-stu-id="8caa0-109">See the following articles to create a VM and add a data disk:</span></span>

* <span data-ttu-id="8caa0-110">Skapa en virtuell Windows Server-dator med hjälp av [Azure-portalen](quick-create-portal.md) eller [Azure PowerShell](quick-create-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="8caa0-110">Create a Windows Server VM using [the Azure portal](quick-create-portal.md) or [Azure PowerShell](quick-create-powershell.md).</span></span>
* <span data-ttu-id="8caa0-111">Ansluta en datadisk till en virtuell Windows Server-dator med hjälp av [Azure-portalen](attach-managed-disk-portal.md) eller [Azure PowerShell](attach-disk-ps.md).</span><span class="sxs-lookup"><span data-stu-id="8caa0-111">Attach a data disk to a Windows Server VM using [the Azure portal](attach-managed-disk-portal.md) or [Azure PowerShell](attach-disk-ps.md).</span></span>

<span data-ttu-id="8caa0-112">Att börja installera och konfigurera MongoDB [logga in på Windows Server-VM](connect-logon.md) med hjälp av fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="8caa0-112">To begin installing and configuring MongoDB, [log on to your Windows Server VM](connect-logon.md) by using Remote Desktop.</span></span>

## <a name="install-mongodb"></a><span data-ttu-id="8caa0-113">Installera MongoDB</span><span class="sxs-lookup"><span data-stu-id="8caa0-113">Install MongoDB</span></span>
> [!IMPORTANT]
> <span data-ttu-id="8caa0-114">MongoDB säkerhetsfunktioner, till exempel autentisering och IP-adress bindningen är inte aktiverade som standard.</span><span class="sxs-lookup"><span data-stu-id="8caa0-114">MongoDB security features, such as authentication and IP address binding, are not enabled by default.</span></span> <span data-ttu-id="8caa0-115">Säkerhetsfunktioner ska aktiveras innan du distribuerar MongoDB till en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="8caa0-115">Security features should be enabled before deploying MongoDB to a production environment.</span></span> <span data-ttu-id="8caa0-116">Mer information finns i [MongoDB säkerhet och autentisering](http://www.mongodb.org/display/DOCS/Security+and+Authentication).</span><span class="sxs-lookup"><span data-stu-id="8caa0-116">For more information, see [MongoDB Security and Authentication](http://www.mongodb.org/display/DOCS/Security+and+Authentication).</span></span>


1. <span data-ttu-id="8caa0-117">När du har anslutit till den virtuella datorn med hjälp av fjärrskrivbord, öppna Internet Explorer från den **starta** menyn på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="8caa0-117">After you've connected to your VM using Remote Desktop, open Internet Explorer from the **Start** menu on the VM.</span></span>
2. <span data-ttu-id="8caa0-118">Välj **Använd rekommenderade inställningar för säkerhet, sekretess och kompatibilitet** när Internet Explorer först öppnar och på **OK**.</span><span class="sxs-lookup"><span data-stu-id="8caa0-118">Select **Use recommended security, privacy, and compatibility settings** when Internet Explorer first opens, and click **OK**.</span></span>
3. <span data-ttu-id="8caa0-119">Förbättrad säkerhetskonfiguration i Internet Explorer är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="8caa0-119">Internet Explorer enhanced security configuration is enabled by default.</span></span> <span data-ttu-id="8caa0-120">Lägg till MongoDB-webbplatsen i listan över tillåtna platser:</span><span class="sxs-lookup"><span data-stu-id="8caa0-120">Add the MongoDB website to the list of allowed sites:</span></span>
   
   * <span data-ttu-id="8caa0-121">Välj den **verktyg** i det övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="8caa0-121">Select the **Tools** icon in the upper-right corner.</span></span>
   * <span data-ttu-id="8caa0-122">I **Internetalternativ**, Välj den **säkerhet** och välj sedan den **tillförlitliga platser** ikon.</span><span class="sxs-lookup"><span data-stu-id="8caa0-122">In **Internet Options**, select the **Security** tab, and then select the **Trusted Sites** icon.</span></span>
   * <span data-ttu-id="8caa0-123">Klicka på den **platser** knappen.</span><span class="sxs-lookup"><span data-stu-id="8caa0-123">Click the **Sites** button.</span></span> <span data-ttu-id="8caa0-124">Lägg till *https://\*. mongodb.org* i listan över betrodda platser och Stäng dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8caa0-124">Add *https://\*.mongodb.org* to the list of trusted sites, and then close the dialog box.</span></span>
     
     ![Konfigurera säkerhetsinställningar för Internet Explorer](./media/install-mongodb/configure-internet-explorer-security.png)
4. <span data-ttu-id="8caa0-126">Bläddra till den [MongoDB - hämtningar](http://www.mongodb.org/downloads) sida (http://www.mongodb.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="8caa0-126">Browse to the [MongoDB - Downloads](http://www.mongodb.org/downloads) page (http://www.mongodb.org/downloads).</span></span>
5. <span data-ttu-id="8caa0-127">Om det behövs, Välj den **Community Server** edition och välj sedan det senaste aktuella stabilt versionen för Windows Server 2008 R2 64-bitars och senare.</span><span class="sxs-lookup"><span data-stu-id="8caa0-127">If needed, select the **Community Server** edition and then select the latest current stable release for Windows Server 2008 R2 64-bit and later.</span></span> <span data-ttu-id="8caa0-128">För att hämta installationsprogrammet, klickar du på **DOWNLOAD (msi)**.</span><span class="sxs-lookup"><span data-stu-id="8caa0-128">To download the installer, click **DOWNLOAD (msi)**.</span></span>
   
    ![Hämta MongoDB installer](./media/install-mongodb/download-mongodb.png)
   
    <span data-ttu-id="8caa0-130">Kör installationsprogrammet när hämtningen är slutförd.</span><span class="sxs-lookup"><span data-stu-id="8caa0-130">Run the installer after the download is complete.</span></span>
6. <span data-ttu-id="8caa0-131">Läs och acceptera licensavtalet (EULA).</span><span class="sxs-lookup"><span data-stu-id="8caa0-131">Read and accept the license agreement.</span></span> <span data-ttu-id="8caa0-132">När du uppmanas, välja **Slutför** installera.</span><span class="sxs-lookup"><span data-stu-id="8caa0-132">When you're prompted, select **Complete** install.</span></span>
7. <span data-ttu-id="8caa0-133">Klicka på den sista sidan **installera**.</span><span class="sxs-lookup"><span data-stu-id="8caa0-133">On the final screen, click **Install**.</span></span>

## <a name="configure-the-vm-and-mongodb"></a><span data-ttu-id="8caa0-134">Konfigurera den virtuella datorn och MongoDB</span><span class="sxs-lookup"><span data-stu-id="8caa0-134">Configure the VM and MongoDB</span></span>
1. <span data-ttu-id="8caa0-135">Path-variabler uppdateras inte av installationsprogrammet MongoDB.</span><span class="sxs-lookup"><span data-stu-id="8caa0-135">The path variables are not updated by the MongoDB installer.</span></span> <span data-ttu-id="8caa0-136">Utan MongoDB `bin` plats i path-variabeln, måste du ange den fullständiga sökvägen varje gång du använder en körbar fil MongoDB.</span><span class="sxs-lookup"><span data-stu-id="8caa0-136">Without the MongoDB `bin` location in your path variable, you need to specify the full path each time you use a MongoDB executable.</span></span> <span data-ttu-id="8caa0-137">Lägga till platsen i path-variabeln:</span><span class="sxs-lookup"><span data-stu-id="8caa0-137">To add the location to your path variable:</span></span>
   
   * <span data-ttu-id="8caa0-138">Högerklicka på den **starta** menyn och välj **System**.</span><span class="sxs-lookup"><span data-stu-id="8caa0-138">Right-click the **Start** menu, and select **System**.</span></span>
   * <span data-ttu-id="8caa0-139">Klicka på **avancerade systeminställningar**, och klicka sedan på **miljövariabler**.</span><span class="sxs-lookup"><span data-stu-id="8caa0-139">Click **Advanced system settings**, and then click **Environment Variables**.</span></span>
   * <span data-ttu-id="8caa0-140">Under **systemvariabler**väljer **sökväg**, och klicka sedan på **redigera**.</span><span class="sxs-lookup"><span data-stu-id="8caa0-140">Under **System variables**, select **Path**, and then click **Edit**.</span></span>
     
     ![Konfigurera sökvägsvariabler](./media/install-mongodb/configure-path-variables.png)
     
     <span data-ttu-id="8caa0-142">Lägg till sökvägen till din MongoDB `bin` mapp.</span><span class="sxs-lookup"><span data-stu-id="8caa0-142">Add the path to your MongoDB `bin` folder.</span></span> <span data-ttu-id="8caa0-143">MongoDB installeras normalt i *C:\Program Files\MongoDB*.</span><span class="sxs-lookup"><span data-stu-id="8caa0-143">MongoDB is typically installed in *C:\Program Files\MongoDB*.</span></span> <span data-ttu-id="8caa0-144">Kontrollera installationssökvägen på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="8caa0-144">Verify the installation path on your VM.</span></span> <span data-ttu-id="8caa0-145">I följande exempel läggs standard MongoDB installera platsen till den `PATH` variabeln:</span><span class="sxs-lookup"><span data-stu-id="8caa0-145">The following example adds the default MongoDB install location to the `PATH` variable:</span></span>
     
     ```
     ;C:\Program Files\MongoDB\Server\3.2\bin
     ```
     
     > [!NOTE]
     > <span data-ttu-id="8caa0-146">Se till att lägga till inledande semikolon (`;`) att indikera att du lägger till en plats för att din `PATH` variabeln.</span><span class="sxs-lookup"><span data-stu-id="8caa0-146">Be sure to add the leading semicolon (`;`) to indicate that you are adding a location to your `PATH` variable.</span></span>

2. <span data-ttu-id="8caa0-147">Skapa MongoDB data och loggfilen kataloger på hårddisken data.</span><span class="sxs-lookup"><span data-stu-id="8caa0-147">Create MongoDB data and log directories on your data disk.</span></span> <span data-ttu-id="8caa0-148">Från den **starta** väljer du **kommandotolk**.</span><span class="sxs-lookup"><span data-stu-id="8caa0-148">From the **Start** menu, select **Command Prompt**.</span></span> <span data-ttu-id="8caa0-149">I följande exempel skapa kataloger på enhet F:</span><span class="sxs-lookup"><span data-stu-id="8caa0-149">The following examples create the directories on drive F:</span></span>
   
    ```
    mkdir F:\MongoData
    mkdir F:\MongoLogs
    ```
3. <span data-ttu-id="8caa0-150">Starta en MongoDB-instansen med följande kommando, justera sökvägen till dina data och logga kataloger i enlighet med detta:</span><span class="sxs-lookup"><span data-stu-id="8caa0-150">Start a MongoDB instance with the following command, adjusting the path to your data and log directories accordingly:</span></span>
   
    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log
    ```
   
    <span data-ttu-id="8caa0-151">Det kan ta flera minuter för MongoDB att allokera journal-filer och börja lyssna efter anslutningar.</span><span class="sxs-lookup"><span data-stu-id="8caa0-151">It may take several minutes for MongoDB to allocate the journal files and start listening for connections.</span></span> <span data-ttu-id="8caa0-152">Alla loggmeddelanden dirigeras till den *F:\MongoLogs\mongolog.log* filen som `mongod.exe` servern startar och allokerar journal-filer.</span><span class="sxs-lookup"><span data-stu-id="8caa0-152">All log messages are directed to the *F:\MongoLogs\mongolog.log* file as `mongod.exe` server starts and allocates journal files.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="8caa0-153">Kommandotolken förblir fokuserar på den här uppgiften när MongoDB-instansen körs.</span><span class="sxs-lookup"><span data-stu-id="8caa0-153">The command prompt stays focused on this task while your MongoDB instance is running.</span></span> <span data-ttu-id="8caa0-154">Lämna Kommandotolken öppen fortsätta att köra MongoDB.</span><span class="sxs-lookup"><span data-stu-id="8caa0-154">Leave the command prompt window open to continue running MongoDB.</span></span> <span data-ttu-id="8caa0-155">Eller installera MongoDB som tjänst enligt anvisningarna i nästa steg.</span><span class="sxs-lookup"><span data-stu-id="8caa0-155">Or, install MongoDB as service, as detailed in the next step.</span></span>

4. <span data-ttu-id="8caa0-156">En mer robusta MongoDB-upplevelse, installera den `mongod.exe` som en tjänst.</span><span class="sxs-lookup"><span data-stu-id="8caa0-156">For a more robust MongoDB experience, install the `mongod.exe` as a service.</span></span> <span data-ttu-id="8caa0-157">När du skapar en tjänst innebär du inte behöver lämna Kommandotolken körs varje gång som du vill använda MongoDB.</span><span class="sxs-lookup"><span data-stu-id="8caa0-157">Creating a service means you don't need to leave a command prompt running each time you want to use MongoDB.</span></span> <span data-ttu-id="8caa0-158">Skapa tjänsten på följande sätt justeras sökvägen till dina data och loggfilen kataloger efter:</span><span class="sxs-lookup"><span data-stu-id="8caa0-158">Create the service as follows, adjusting the path to your data and log directories accordingly:</span></span>
   
    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log `
        --logappend  --install
    ```
   
    <span data-ttu-id="8caa0-159">Föregående kommando skapar en tjänst med namnet MongoDB, med en beskrivning av ”Mongo databas”.</span><span class="sxs-lookup"><span data-stu-id="8caa0-159">The preceding command creates a service named MongoDB, with a description of "Mongo DB".</span></span> <span data-ttu-id="8caa0-160">Följande parametrar anges också:</span><span class="sxs-lookup"><span data-stu-id="8caa0-160">The following parameters are also specified:</span></span>
   
   * <span data-ttu-id="8caa0-161">Den `--dbpath` alternativet anger platsen för datakatalog.</span><span class="sxs-lookup"><span data-stu-id="8caa0-161">The `--dbpath` option specifies the location of the data directory.</span></span>
   * <span data-ttu-id="8caa0-162">Den `--logpath` alternativet måste användas för att ange en loggfil, eftersom tjänsten körs inte har ett kommandofönster om du vill visa utdata.</span><span class="sxs-lookup"><span data-stu-id="8caa0-162">The `--logpath` option must be used to specify a log file, because the running service does not have a command window to display output.</span></span>
   * <span data-ttu-id="8caa0-163">Den `--logappend` alternativet anger att en omstart av tjänsten gör att utdata ska läggas till i den befintliga loggfilen.</span><span class="sxs-lookup"><span data-stu-id="8caa0-163">The `--logappend` option specifies that a restart of the service causes output to append to the existing log file.</span></span>
   
   <span data-ttu-id="8caa0-164">Om du vill starta tjänsten MongoDB, kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="8caa0-164">To start the MongoDB service, run the following command:</span></span>
   
    ```
    net start MongoDB
    ```
   
    <span data-ttu-id="8caa0-165">Mer information om hur du skapar MongoDB-tjänsten finns [konfigurera en Windows-tjänst för MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/#mongodb-as-a-windows-service).</span><span class="sxs-lookup"><span data-stu-id="8caa0-165">For more information about creating the MongoDB service, see [Configure a Windows Service for MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/#mongodb-as-a-windows-service).</span></span>

## <a name="test-the-mongodb-instance"></a><span data-ttu-id="8caa0-166">Testa MongoDB-instansen</span><span class="sxs-lookup"><span data-stu-id="8caa0-166">Test the MongoDB instance</span></span>
<span data-ttu-id="8caa0-167">Med MongoDB körs som en enda instans eller installeras som en tjänst, kan du nu börja skapa och använda dina databaser.</span><span class="sxs-lookup"><span data-stu-id="8caa0-167">With MongoDB running as a single instance or installed as a service, you can now start creating and using your databases.</span></span> <span data-ttu-id="8caa0-168">Om du vill starta det administrativa gränssnittet MongoDB, öppna en annan kommandotolk från den **starta** -menyn och ange följande kommando:</span><span class="sxs-lookup"><span data-stu-id="8caa0-168">To start the MongoDB administrative shell, open another command prompt window from the **Start** menu, and enter the following command:</span></span>

```
mongo  
```

<span data-ttu-id="8caa0-169">Du kan visa en lista över databaser med den `db` kommando.</span><span class="sxs-lookup"><span data-stu-id="8caa0-169">You can list the databases with the `db` command.</span></span> <span data-ttu-id="8caa0-170">Infoga data på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="8caa0-170">Insert some data as follows:</span></span>

```
db.foo.insert( { a : 1 } )
```

<span data-ttu-id="8caa0-171">Söka efter data på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="8caa0-171">Search for data as follows:</span></span>

```
db.foo.find()
```

<span data-ttu-id="8caa0-172">Utdata ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="8caa0-172">The output is similar to the following example:</span></span>

```
{ "_id" : "ObjectId("57f6a86cee873a6232d74842"), "a" : 1 }
```

<span data-ttu-id="8caa0-173">Avsluta den `mongo` konsolen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="8caa0-173">Exit the `mongo` console as follows:</span></span>

```
exit
```

## <a name="configure-firewall-and-network-security-group-rules"></a><span data-ttu-id="8caa0-174">Konfigurera brandväggen och Nätverkssäkerhetsgruppen regler</span><span class="sxs-lookup"><span data-stu-id="8caa0-174">Configure firewall and Network Security Group rules</span></span>
<span data-ttu-id="8caa0-175">Nu när MongoDB är installerade och körs, öppna en port i Windows-brandväggen så att du kan fjärransluta till MongoDB.</span><span class="sxs-lookup"><span data-stu-id="8caa0-175">Now that MongoDB is installed and running, open a port in Windows Firewall so you can remotely connect to MongoDB.</span></span> <span data-ttu-id="8caa0-176">Om du vill skapa en ny inkommande regel för att tillåta TCP-port 27017 öppna en administrativ PowerShell-Kommandotolken och ange följande kommando:</span><span class="sxs-lookup"><span data-stu-id="8caa0-176">To create a new inbound rule to allow TCP port 27017, open an administrative PowerShell prompt and enter the following command:</span></span>

```powerahell
New-NetFirewallRule `
    -DisplayName "Allow MongoDB" `
    -Direction Inbound `
    -Protocol TCP `
    -LocalPort 27017 `
    -Action Allow
```

<span data-ttu-id="8caa0-177">Du kan också skapa regeln med hjälp av den **Windows-brandväggen med avancerad säkerhet** grafiskt hanteringsverktyg.</span><span class="sxs-lookup"><span data-stu-id="8caa0-177">You can also create the rule by using the **Windows Firewall with Advanced Security** graphical management tool.</span></span> <span data-ttu-id="8caa0-178">Skapa en ny inkommande regel för att tillåta TCP-port 27017.</span><span class="sxs-lookup"><span data-stu-id="8caa0-178">Create a new inbound rule to allow TCP port 27017.</span></span>

<span data-ttu-id="8caa0-179">Skapa en säkerhetsgrupp för nätverk-regel för att tillåta åtkomst till MongoDB från utanför det befintliga virtuella Azure-nätverket undernätet vid behov.</span><span class="sxs-lookup"><span data-stu-id="8caa0-179">If needed, create a Network Security Group rule to allow access to MongoDB from outside of the existing Azure virtual network subnet.</span></span> <span data-ttu-id="8caa0-180">Du kan skapa Nätverkssäkerhetsgruppen-regler med hjälp av den [Azure-portalen](nsg-quickstart-portal.md) eller [Azure PowerShell](nsg-quickstart-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="8caa0-180">You can create the Network Security Group rules by using the [Azure portal](nsg-quickstart-portal.md) or [Azure PowerShell](nsg-quickstart-powershell.md).</span></span> <span data-ttu-id="8caa0-181">Precis som med Windows-brandväggsreglerna Tillåt TCP-port 27017 till det virtuella nätverksgränssnittet av MongoDB-VM.</span><span class="sxs-lookup"><span data-stu-id="8caa0-181">As with the Windows Firewall rules, allow TCP port 27017 to the virtual network interface of your MongoDB VM.</span></span>

> [!NOTE]
> <span data-ttu-id="8caa0-182">TCP-port 27017 är standardporten som används av MongoDB.</span><span class="sxs-lookup"><span data-stu-id="8caa0-182">TCP port 27017 is the default port used by MongoDB.</span></span> <span data-ttu-id="8caa0-183">Du kan ändra den här porten med hjälp av den `--port` parameter när du startar `mongod.exe` manuellt eller från en tjänst.</span><span class="sxs-lookup"><span data-stu-id="8caa0-183">You can change this port by using the `--port` parameter when starting `mongod.exe` manually or from a service.</span></span> <span data-ttu-id="8caa0-184">Om du ändrar porten måste du se till att uppdatera Windows-brandväggen och Nätverkssäkerhetsgruppen reglerna i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="8caa0-184">If you change the port, make sure to update the Windows Firewall and Network Security Group rules in the preceding steps.</span></span>


## <a name="next-steps"></a><span data-ttu-id="8caa0-185">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8caa0-185">Next steps</span></span>
<span data-ttu-id="8caa0-186">I den här självstudiekursen beskrivs hur du installerar och konfigurerar MongoDB på din Windows-VM.</span><span class="sxs-lookup"><span data-stu-id="8caa0-186">In this tutorial, you learned how to install and configure MongoDB on your Windows VM.</span></span> <span data-ttu-id="8caa0-187">Du kan nu komma åt MongoDB på den virtuella datorn i Windows, genom att följa de avancerade ämnena i det [MongoDB-dokumentation](https://docs.mongodb.com/manual/).</span><span class="sxs-lookup"><span data-stu-id="8caa0-187">You can now access MongoDB on your Windows VM, by following the advanced topics in the [MongoDB documentation](https://docs.mongodb.com/manual/).</span></span>

