<span data-ttu-id="8850c-101">Följ dessa steg tooinstall och kör MongoDB på en virtuell dator som kör Windows Server.</span><span class="sxs-lookup"><span data-stu-id="8850c-101">Follow these steps tooinstall and run MongoDB on a virtual machine running Windows Server.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8850c-102">MongoDB säkerhetsfunktioner, till exempel autentisering och IP-adress bindningen är inte aktiverade som standard.</span><span class="sxs-lookup"><span data-stu-id="8850c-102">MongoDB security features, such as authentication and IP address binding, are not enabled by default.</span></span> <span data-ttu-id="8850c-103">Säkerhetsfunktioner ska aktiveras innan du distribuerar MongoDB tooa produktionsmiljön.</span><span class="sxs-lookup"><span data-stu-id="8850c-103">Security features should be enabled before deploying MongoDB tooa production environment.</span></span>  <span data-ttu-id="8850c-104">Mer information finns i [säkerhets- och](http://www.mongodb.org/display/DOCS/Security+and+Authentication).</span><span class="sxs-lookup"><span data-stu-id="8850c-104">For more information, see [Security and Authentication](http://www.mongodb.org/display/DOCS/Security+and+Authentication).</span></span>
>
>

1. <span data-ttu-id="8850c-105">När du har anslutit toohello virtuell dator med hjälp av fjärrskrivbord, öppna Internet Explorer från hello **starta** menyn på hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="8850c-105">After you've connected toohello virtual machine using Remote Desktop, open Internet Explorer from hello **Start** menu on hello virtual machine.</span></span>
2. <span data-ttu-id="8850c-106">Välj hello **verktyg** i hello övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="8850c-106">Select hello **Tools** button in hello upper right corner.</span></span>  <span data-ttu-id="8850c-107">I **Internetalternativ**väljer hello **säkerhet** och välj hello **tillförlitliga platser** ikon, och slutligen på hello **platser** knappen.</span><span class="sxs-lookup"><span data-stu-id="8850c-107">In **Internet Options**, select hello **Security** tab, and then select hello **Trusted Sites** icon, and finally click hello **Sites** button.</span></span> <span data-ttu-id="8850c-108">Lägg till *https://\*. mongodb.org* toohello lista över betrodda platser.</span><span class="sxs-lookup"><span data-stu-id="8850c-108">Add *https://\*.mongodb.org* toohello list of trusted sites.</span></span>
3. <span data-ttu-id="8850c-109">Gå för[hämtas - MongoDB](https://www.mongodb.com/download-center#community).</span><span class="sxs-lookup"><span data-stu-id="8850c-109">Go too[Downloads - MongoDB](https://www.mongodb.com/download-center#community).</span></span>
4. <span data-ttu-id="8850c-110">Hitta hello **aktuella stabil versionen** av **Community Server**väljer hello senaste **64-bitars** version i hello Windows kolumnen.</span><span class="sxs-lookup"><span data-stu-id="8850c-110">Find hello **Current Stable Release** of **Community Server**, select hello latest **64-bit** version in hello Windows column.</span></span> <span data-ttu-id="8850c-111">Hämta och kör sedan hello MSI installer.</span><span class="sxs-lookup"><span data-stu-id="8850c-111">Download, then run hello MSI installer.</span></span>
5. <span data-ttu-id="8850c-112">MongoDB installeras normalt i C:\Program Files\MongoDB.</span><span class="sxs-lookup"><span data-stu-id="8850c-112">MongoDB is typically installed in C:\Program Files\MongoDB.</span></span> <span data-ttu-id="8850c-113">Sök efter miljövariabler på hello skrivbordet och Lägg till hello MongoDB binärfiler toohello sökväg sökvägsvariabeln.</span><span class="sxs-lookup"><span data-stu-id="8850c-113">Search for Environment Variables on hello desktop and add hello MongoDB binaries path toohello PATH variable.</span></span> <span data-ttu-id="8850c-114">Exempelvis kanske hello binärfiler i C:\Program Files\MongoDB\Server\3.4\bin på din dator.</span><span class="sxs-lookup"><span data-stu-id="8850c-114">For example, you might find hello binaries at C:\Program Files\MongoDB\Server\3.4\bin on your machine.</span></span>
6. <span data-ttu-id="8850c-115">Skapa MongoDB data och loggfilen kataloger i hello datadisk (till exempel enhet **F:**) du skapade i föregående steg hello.</span><span class="sxs-lookup"><span data-stu-id="8850c-115">Create MongoDB data and log directories in hello data disk (such as drive **F:**) you created in hello preceding steps.</span></span> <span data-ttu-id="8850c-116">Från **starta**väljer **kommandotolk** tooopen Kommandotolkens fönster.</span><span class="sxs-lookup"><span data-stu-id="8850c-116">From **Start**, select **Command Prompt** tooopen a command prompt window.</span></span>  <span data-ttu-id="8850c-117">Ange:</span><span class="sxs-lookup"><span data-stu-id="8850c-117">Type:</span></span>

        C:\> F:
        F:\> mkdir \MongoData
        F:\> mkdir \MongoLogs
7. <span data-ttu-id="8850c-118">toorun hello databas, kör:</span><span class="sxs-lookup"><span data-stu-id="8850c-118">toorun hello database, run:</span></span>

        F:\> C:
        C:\> mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log

    <span data-ttu-id="8850c-119">Alla loggmeddelanden är riktat toohello *F:\MongoLogs\mongolog.log* filen som mongod.exe servern startar och preallocates journal-filer.</span><span class="sxs-lookup"><span data-stu-id="8850c-119">All log messages are directed toohello *F:\MongoLogs\mongolog.log* file as mongod.exe server starts and preallocates journal files.</span></span> <span data-ttu-id="8850c-120">Det kan ta flera minuter för MongoDB toopreallocate hello journalfiler och börja lyssna efter anslutningar.</span><span class="sxs-lookup"><span data-stu-id="8850c-120">It may take several minutes for MongoDB toopreallocate hello journal files and start listening for connections.</span></span> <span data-ttu-id="8850c-121">hello kommandotolk förblir fokuserar på den här uppgiften när MongoDB-instansen körs.</span><span class="sxs-lookup"><span data-stu-id="8850c-121">hello command prompt stays focused on this task while your MongoDB instance is running.</span></span>
8. <span data-ttu-id="8850c-122">toostart hello MongoDB administrativa shell, öppnar du en annan kommandofönster från **starta** och typen hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="8850c-122">toostart hello MongoDB administrative shell, open another command window from **Start** and type hello following commands:</span></span>

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

    <span data-ttu-id="8850c-123">hello-databasen har skapats med hello insert.</span><span class="sxs-lookup"><span data-stu-id="8850c-123">hello database is created by hello insert.</span></span>
9. <span data-ttu-id="8850c-124">Du kan också installera mongod.exe som en tjänst:</span><span class="sxs-lookup"><span data-stu-id="8850c-124">Alternatively, you can install mongod.exe as a service:</span></span>

        C:\> mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log --logappend  --install

    <span data-ttu-id="8850c-125">En tjänst har installerats med namnet MongoDB med en beskrivning av ”Mongo databas”.</span><span class="sxs-lookup"><span data-stu-id="8850c-125">A service is installed named MongoDB with a description of "Mongo DB".</span></span> <span data-ttu-id="8850c-126">Hej `--logpath` alternativet måste vara används toospecify en loggfil, eftersom hello kör tjänsten har inte en toodisplay utdata från kommandot.</span><span class="sxs-lookup"><span data-stu-id="8850c-126">hello `--logpath` option must be used toospecify a log file, since hello running service does not have a command window toodisplay output.</span></span>  <span data-ttu-id="8850c-127">Hej `--logappend` alternativet anger att en omstart av hello tjänsten gör tooappend toohello befintliga utdataloggfilen.</span><span class="sxs-lookup"><span data-stu-id="8850c-127">hello `--logappend` option specifies that a restart of hello service causes output tooappend toohello existing log file.</span></span>  <span data-ttu-id="8850c-128">Hej `--dbpath` alternativet anger hello platsen för hello datakatalog.</span><span class="sxs-lookup"><span data-stu-id="8850c-128">hello `--dbpath` option specifies hello location of hello data directory.</span></span> <span data-ttu-id="8850c-129">För mer tjänstrelaterade kommandoradsalternativ, se [tjänstrelaterade kommandoradsalternativ][MongoWindowsSvcOptions].</span><span class="sxs-lookup"><span data-stu-id="8850c-129">For more service-related command-line options, see [Service-related command-line options][MongoWindowsSvcOptions].</span></span>

    <span data-ttu-id="8850c-130">toostart hello-tjänst, köra det här kommandot:</span><span class="sxs-lookup"><span data-stu-id="8850c-130">toostart hello service, run this command:</span></span>

        C:\> net start MongoDB
10. <span data-ttu-id="8850c-131">Nu när MongoDB är installerad och körs, behöver du tooopen en port i Windows-brandväggen så du kan fjärransluta tooMongoDB.</span><span class="sxs-lookup"><span data-stu-id="8850c-131">Now that MongoDB is installed and running, you need tooopen a port in Windows Firewall so you can remotely connect tooMongoDB.</span></span>  <span data-ttu-id="8850c-132">Från hello **starta** väljer du **Administrationsverktyg** och sedan **Windows-brandväggen med avancerad säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="8850c-132">From hello **Start** menu, select **Administrative Tools** and then **Windows Firewall with Advanced Security**.</span></span>
11. <span data-ttu-id="8850c-133">en) i hello till vänster och välj **regler för inkommande trafik**.</span><span class="sxs-lookup"><span data-stu-id="8850c-133">a) In hello left pane, select **Inbound Rules**.</span></span>  <span data-ttu-id="8850c-134">I hello **åtgärder** panelen på hello höger, väljer **ny regel...** .</span><span class="sxs-lookup"><span data-stu-id="8850c-134">In hello **Actions** pane on hello right, select **New Rule...**.</span></span>

    ![Windows-brandväggen][Image1]

    <span data-ttu-id="8850c-136">b) i hello **ny inkommande regel för**väljer **Port** och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="8850c-136">b) In hello **New Inbound Rule Wizard**, select **Port** and then click **Next**.</span></span>

    ![Windows-brandväggen][Image2]

    <span data-ttu-id="8850c-138">c) Välj **TCP** och sedan **specifika lokala portar**.</span><span class="sxs-lookup"><span data-stu-id="8850c-138">c) Select **TCP** and then **Specific local ports**.</span></span>  <span data-ttu-id="8850c-139">Ange en port för ”27017” (hello standardporten MongoDB lyssnar på) och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="8850c-139">Specify a port of "27017" (hello default port MongoDB listens on) and click **Next**.</span></span>

    ![Windows-brandväggen][Image3]

    <span data-ttu-id="8850c-141">d) Välj **Tillåt hello anslutning** och på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="8850c-141">d) Select **Allow hello connection** and click **Next**.</span></span>

    ![Windows-brandväggen][Image4]

    <span data-ttu-id="8850c-143">e) Klicka på **nästa** igen.</span><span class="sxs-lookup"><span data-stu-id="8850c-143">e) Click **Next** again.</span></span>

    ![Windows-brandväggen][Image5]

    <span data-ttu-id="8850c-145">f) anger ett namn för hello regeln, exempelvis ”MongoPort” och klickar på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="8850c-145">f) Specify a name for hello rule, such as "MongoPort", and click **Finish**.</span></span>

    ![Windows-brandväggen][Image6]

12. <span data-ttu-id="8850c-147">Om du inte konfigurerar en slutpunkt för MongoDB när du skapade hello virtuell dator kan göra du det nu.</span><span class="sxs-lookup"><span data-stu-id="8850c-147">If you didn't configure an endpoint for MongoDB when you created hello virtual machine, you can do it now.</span></span> <span data-ttu-id="8850c-148">Behöver du både hello brandväggsregel och hello endpoint toobe kan tooconnect tooMongoDB via fjärranslutning.</span><span class="sxs-lookup"><span data-stu-id="8850c-148">You need both hello firewall rule and hello endpoint toobe able tooconnect tooMongoDB remotely.</span></span>

  <span data-ttu-id="8850c-149">I hello Azure-portalen klickar du på **virtuella datorer (klassisk)**hello namnet på den nya virtuella datorn och klicka sedan på **slutpunkter**.</span><span class="sxs-lookup"><span data-stu-id="8850c-149">In hello Azure portal, click **Virtual Machines (classic)**, click hello name of your new virtual machine, and then click **Endpoints**.</span></span>

    ![Slutpunkter][Image7]

13. <span data-ttu-id="8850c-151">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="8850c-151">Click **Add**.</span></span>

14. <span data-ttu-id="8850c-152">Lägg till en slutpunkt med namnet ”Mongo”, protokollet **TCP**, och båda **offentliga** och **privata** set-portarna för ”27017”.</span><span class="sxs-lookup"><span data-stu-id="8850c-152">Add an endpoint with name "Mongo", protocol **TCP**, and both **Public** and **Private** ports set too"27017".</span></span> <span data-ttu-id="8850c-153">Öppnar den här porten kan MongoDB toobe fjärråtkomst.</span><span class="sxs-lookup"><span data-stu-id="8850c-153">Opening this port allows MongoDB toobe accessed remotely.</span></span>

    ![Slutpunkter][Image9]

> [!NOTE]
> <span data-ttu-id="8850c-155">hello port 27017 är hello standardport som används av MongoDB.</span><span class="sxs-lookup"><span data-stu-id="8850c-155">hello port 27017 is hello default port used by MongoDB.</span></span> <span data-ttu-id="8850c-156">Du kan ändra den här standardporten genom att ange hello `--port` parameter när du startar hello mongod.exe server.</span><span class="sxs-lookup"><span data-stu-id="8850c-156">You can change this default port by specifying hello `--port` parameter when starting hello mongod.exe server.</span></span> <span data-ttu-id="8850c-157">Kontrollera att toogive hello samma portnummer i hello-brandväggen och hello ”Mongo” slutpunkt i hello föregående anvisningar.</span><span class="sxs-lookup"><span data-stu-id="8850c-157">Make sure toogive hello same port number in hello firewall and hello "Mongo" endpoint in hello preceding instructions.</span></span>
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
