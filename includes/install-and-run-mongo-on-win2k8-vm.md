<span data-ttu-id="67c51-101">Följ dessa steg om du vill installera och köra MongoDB på en virtuell dator som kör Windows Server.</span><span class="sxs-lookup"><span data-stu-id="67c51-101">Follow these steps to install and run MongoDB on a virtual machine running Windows Server.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="67c51-102">MongoDB säkerhetsfunktioner, till exempel autentisering och IP-adress bindningen är inte aktiverade som standard.</span><span class="sxs-lookup"><span data-stu-id="67c51-102">MongoDB security features, such as authentication and IP address binding, are not enabled by default.</span></span> <span data-ttu-id="67c51-103">Säkerhetsfunktioner ska aktiveras innan du distribuerar MongoDB till en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="67c51-103">Security features should be enabled before deploying MongoDB to a production environment.</span></span>  <span data-ttu-id="67c51-104">Mer information finns i [säkerhets- och](http://www.mongodb.org/display/DOCS/Security+and+Authentication).</span><span class="sxs-lookup"><span data-stu-id="67c51-104">For more information, see [Security and Authentication](http://www.mongodb.org/display/DOCS/Security+and+Authentication).</span></span>
>
>

1. <span data-ttu-id="67c51-105">När du har anslutit till den virtuella datorn via fjärrskrivbord, öppna Internet Explorer från den **starta** menyn på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="67c51-105">After you've connected to the virtual machine using Remote Desktop, open Internet Explorer from the **Start** menu on the virtual machine.</span></span>
2. <span data-ttu-id="67c51-106">Välj den **verktyg** i det övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="67c51-106">Select the **Tools** button in the upper right corner.</span></span>  <span data-ttu-id="67c51-107">I **Internetalternativ**, Välj den **säkerhet** och välj sedan den **tillförlitliga platser** ikon, och slutligen på den **platser** knappen.</span><span class="sxs-lookup"><span data-stu-id="67c51-107">In **Internet Options**, select the **Security** tab, and then select the **Trusted Sites** icon, and finally click the **Sites** button.</span></span> <span data-ttu-id="67c51-108">Lägg till *https://\*. mongodb.org* i listan över betrodda platser.</span><span class="sxs-lookup"><span data-stu-id="67c51-108">Add *https://\*.mongodb.org* to the list of trusted sites.</span></span>
3. <span data-ttu-id="67c51-109">Gå till [hämtas - MongoDB](https://www.mongodb.com/download-center#community).</span><span class="sxs-lookup"><span data-stu-id="67c51-109">Go to [Downloads - MongoDB](https://www.mongodb.com/download-center#community).</span></span>
4. <span data-ttu-id="67c51-110">Hitta den **aktuella stabil versionen** av **Community Server**, Välj senast **64-bitars** version i Windows-kolumnen.</span><span class="sxs-lookup"><span data-stu-id="67c51-110">Find the **Current Stable Release** of **Community Server**, select the latest **64-bit** version in the Windows column.</span></span> <span data-ttu-id="67c51-111">Hämta och kör sedan installationen av MSI.</span><span class="sxs-lookup"><span data-stu-id="67c51-111">Download, then run the MSI installer.</span></span>
5. <span data-ttu-id="67c51-112">MongoDB installeras normalt i C:\Program Files\MongoDB.</span><span class="sxs-lookup"><span data-stu-id="67c51-112">MongoDB is typically installed in C:\Program Files\MongoDB.</span></span> <span data-ttu-id="67c51-113">Sök efter miljövariabler på skrivbordet och lägga till sökvägen till MongoDB-binärfiler i PATH-variabeln.</span><span class="sxs-lookup"><span data-stu-id="67c51-113">Search for Environment Variables on the desktop and add the MongoDB binaries path to the PATH variable.</span></span> <span data-ttu-id="67c51-114">Du kan till exempel hitta binärfilerna på C:\Program Files\MongoDB\Server\3.4\bin på din dator.</span><span class="sxs-lookup"><span data-stu-id="67c51-114">For example, you might find the binaries at C:\Program Files\MongoDB\Server\3.4\bin on your machine.</span></span>
6. <span data-ttu-id="67c51-115">Skapa MongoDB data och loggfilen kataloger i datadisken (till exempel enhet **F:**) du skapade i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="67c51-115">Create MongoDB data and log directories in the data disk (such as drive **F:**) you created in the preceding steps.</span></span> <span data-ttu-id="67c51-116">Från **starta**väljer **kommandotolk** att öppna ett kommandotolksfönster.</span><span class="sxs-lookup"><span data-stu-id="67c51-116">From **Start**, select **Command Prompt** to open a command prompt window.</span></span>  <span data-ttu-id="67c51-117">Ange:</span><span class="sxs-lookup"><span data-stu-id="67c51-117">Type:</span></span>

        C:\> F:
        F:\> mkdir \MongoData
        F:\> mkdir \MongoLogs
7. <span data-ttu-id="67c51-118">Om du vill köra databasen kör du:</span><span class="sxs-lookup"><span data-stu-id="67c51-118">To run the database, run:</span></span>

        F:\> C:
        C:\> mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log

    <span data-ttu-id="67c51-119">Alla loggmeddelanden dirigeras till den *F:\MongoLogs\mongolog.log* filen som mongod.exe servern startar och preallocates journal-filer.</span><span class="sxs-lookup"><span data-stu-id="67c51-119">All log messages are directed to the *F:\MongoLogs\mongolog.log* file as mongod.exe server starts and preallocates journal files.</span></span> <span data-ttu-id="67c51-120">Det kan ta flera minuter för MongoDB Förallokera journal-filer och starta lyssna efter anslutningar.</span><span class="sxs-lookup"><span data-stu-id="67c51-120">It may take several minutes for MongoDB to preallocate the journal files and start listening for connections.</span></span> <span data-ttu-id="67c51-121">Kommandotolken förblir fokuserar på den här uppgiften när MongoDB-instansen körs.</span><span class="sxs-lookup"><span data-stu-id="67c51-121">The command prompt stays focused on this task while your MongoDB instance is running.</span></span>
8. <span data-ttu-id="67c51-122">Om du vill starta det administrativa gränssnittet MongoDB, öppnar du en annan kommandofönster från **starta** och Skriv följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="67c51-122">To start the MongoDB administrative shell, open another command window from **Start** and type the following commands:</span></span>

        C:\> cd \my_mongo_dir\bin  
        C:\my_mongo_dir\bin> mongo  
        >db  
        test
        > db.foo.insert( { a : 1 } )  
        > db.foo.find()  
        { _id : ..., a : 1 }  
        > show dbs  
        ...  
        > show collections  
        ...  
        > help  

    <span data-ttu-id="67c51-123">Databasen har skapats genom att infoga.</span><span class="sxs-lookup"><span data-stu-id="67c51-123">The database is created by the insert.</span></span>
9. <span data-ttu-id="67c51-124">Du kan också installera mongod.exe som en tjänst:</span><span class="sxs-lookup"><span data-stu-id="67c51-124">Alternatively, you can install mongod.exe as a service:</span></span>

        C:\> mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log --logappend  --install

    <span data-ttu-id="67c51-125">En tjänst har installerats med namnet MongoDB med en beskrivning av ”Mongo databas”.</span><span class="sxs-lookup"><span data-stu-id="67c51-125">A service is installed named MongoDB with a description of "Mongo DB".</span></span> <span data-ttu-id="67c51-126">Den `--logpath` alternativet måste användas för att ange en loggfil, eftersom tjänsten körs inte har ett kommandofönster om du vill visa utdata.</span><span class="sxs-lookup"><span data-stu-id="67c51-126">The `--logpath` option must be used to specify a log file, since the running service does not have a command window to display output.</span></span>  <span data-ttu-id="67c51-127">Den `--logappend` alternativet anger att en omstart av tjänsten gör att utdata ska läggas till i den befintliga loggfilen.</span><span class="sxs-lookup"><span data-stu-id="67c51-127">The `--logappend` option specifies that a restart of the service causes output to append to the existing log file.</span></span>  <span data-ttu-id="67c51-128">Den `--dbpath` alternativet anger platsen för datakatalog.</span><span class="sxs-lookup"><span data-stu-id="67c51-128">The `--dbpath` option specifies the location of the data directory.</span></span> <span data-ttu-id="67c51-129">För mer tjänstrelaterade kommandoradsalternativ, se [tjänstrelaterade kommandoradsalternativ][MongoWindowsSvcOptions].</span><span class="sxs-lookup"><span data-stu-id="67c51-129">For more service-related command-line options, see [Service-related command-line options][MongoWindowsSvcOptions].</span></span>

    <span data-ttu-id="67c51-130">Om du vill starta tjänsten, kör du kommandot:</span><span class="sxs-lookup"><span data-stu-id="67c51-130">To start the service, run this command:</span></span>

        C:\> net start MongoDB
10. <span data-ttu-id="67c51-131">Nu när MongoDB är installerad och körs, måste du öppna en port i Windows-brandväggen så att du kan fjärransluta till MongoDB.</span><span class="sxs-lookup"><span data-stu-id="67c51-131">Now that MongoDB is installed and running, you need to open a port in Windows Firewall so you can remotely connect to MongoDB.</span></span>  <span data-ttu-id="67c51-132">Från den **starta** väljer du **Administrationsverktyg** och sedan **Windows-brandväggen med avancerad säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="67c51-132">From the **Start** menu, select **Administrative Tools** and then **Windows Firewall with Advanced Security**.</span></span>
11. <span data-ttu-id="67c51-133">en) i den vänstra rutan, Välj **regler för inkommande trafik**.</span><span class="sxs-lookup"><span data-stu-id="67c51-133">a) In the left pane, select **Inbound Rules**.</span></span>  <span data-ttu-id="67c51-134">I den **åtgärder** rutan på höger, Välj **ny regel...** .</span><span class="sxs-lookup"><span data-stu-id="67c51-134">In the **Actions** pane on the right, select **New Rule...**.</span></span>

    ![Windows-brandväggen][Image1]

    <span data-ttu-id="67c51-136">b) i den **ny inkommande regel för**väljer **Port** och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="67c51-136">b) In the **New Inbound Rule Wizard**, select **Port** and then click **Next**.</span></span>

    ![Windows-brandväggen][Image2]

    <span data-ttu-id="67c51-138">c) Välj **TCP** och sedan **specifika lokala portar**.</span><span class="sxs-lookup"><span data-stu-id="67c51-138">c) Select **TCP** and then **Specific local ports**.</span></span>  <span data-ttu-id="67c51-139">Ange en port för ”27017” (standardporten MongoDB lyssnar på) och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="67c51-139">Specify a port of "27017" (the default port MongoDB listens on) and click **Next**.</span></span>

    ![Windows-brandväggen][Image3]

    <span data-ttu-id="67c51-141">d) Välj **tillåter anslutningen** och på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="67c51-141">d) Select **Allow the connection** and click **Next**.</span></span>

    ![Windows-brandväggen][Image4]

    <span data-ttu-id="67c51-143">e) Klicka på **nästa** igen.</span><span class="sxs-lookup"><span data-stu-id="67c51-143">e) Click **Next** again.</span></span>

    ![Windows-brandväggen][Image5]

    <span data-ttu-id="67c51-145">f) anger ett namn för regeln, till exempel ”MongoPort” och klickar på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="67c51-145">f) Specify a name for the rule, such as "MongoPort", and click **Finish**.</span></span>

    ![Windows-brandväggen][Image6]

12. <span data-ttu-id="67c51-147">Om du inte konfigurerar en slutpunkt för MongoDB när du skapade den virtuella datorn kan göra du det nu.</span><span class="sxs-lookup"><span data-stu-id="67c51-147">If you didn't configure an endpoint for MongoDB when you created the virtual machine, you can do it now.</span></span> <span data-ttu-id="67c51-148">Behöver du både brandväggsregeln och slutpunkten ska kunna fjärransluta till MongoDB.</span><span class="sxs-lookup"><span data-stu-id="67c51-148">You need both the firewall rule and the endpoint to be able to connect to MongoDB remotely.</span></span>

  <span data-ttu-id="67c51-149">I Azure-portalen klickar du på **virtuella datorer (klassisk)**, klicka på namnet på den nya virtuella datorn och klicka sedan på **slutpunkter**.</span><span class="sxs-lookup"><span data-stu-id="67c51-149">In the Azure portal, click **Virtual Machines (classic)**, click the name of your new virtual machine, and then click **Endpoints**.</span></span>

    ![Slutpunkter][Image7]

13. <span data-ttu-id="67c51-151">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="67c51-151">Click **Add**.</span></span>

14. <span data-ttu-id="67c51-152">Lägg till en slutpunkt med namnet ”Mongo”, protokollet **TCP**, och båda **offentliga** och **privata** portar inställd på ”27017”.</span><span class="sxs-lookup"><span data-stu-id="67c51-152">Add an endpoint with name "Mongo", protocol **TCP**, and both **Public** and **Private** ports set to "27017".</span></span> <span data-ttu-id="67c51-153">Öppnar den här porten kan MongoDB för fjärråtkomst.</span><span class="sxs-lookup"><span data-stu-id="67c51-153">Opening this port allows MongoDB to be accessed remotely.</span></span>

    ![Slutpunkter][Image9]

> [!NOTE]
> <span data-ttu-id="67c51-155">Porten 27017 är standardporten som används av MongoDB.</span><span class="sxs-lookup"><span data-stu-id="67c51-155">The port 27017 is the default port used by MongoDB.</span></span> <span data-ttu-id="67c51-156">Du kan ändra den här standardporten genom att ange den `--port` parameter när mongod.exe servern startas.</span><span class="sxs-lookup"><span data-stu-id="67c51-156">You can change this default port by specifying the `--port` parameter when starting the mongod.exe server.</span></span> <span data-ttu-id="67c51-157">Se till att ge samma portnummer i brandväggen och slutpunkten ”Mongo” i ovanstående instruktioner.</span><span class="sxs-lookup"><span data-stu-id="67c51-157">Make sure to give the same port number in the firewall and the "Mongo" endpoint in the preceding instructions.</span></span>
>
>

[MongoDownloads]: http://www.mongodb.org/downloads

[MongoWindowsSvcOptions]: http://www.mongodb.org/display/DOCS/Windows+Service


[Image1]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall1.png
[Image2]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall2.png
[Image3]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall3.png
[Image4]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall4.png
[Image5]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall5.png
[Image6]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall6.png
[Image7]: ./media/install-and-run-mongo-on-win2k8-vm/menusendpointadd.png
<!-- Removed 03/08/2017. Not in new portal. -->
<!-- [Image8]: ./media/install-and-run-mongo-on-win2k8-vm/WinVmAddEndpoint2.png
-->
[Image9]: ./media/install-and-run-mongo-on-win2k8-vm/newendpointdetails.png
