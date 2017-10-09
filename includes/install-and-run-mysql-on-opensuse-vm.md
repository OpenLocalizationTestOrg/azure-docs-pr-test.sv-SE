
1. <span data-ttu-id="abae4-101">tooescalate privilegier, typ:</span><span class="sxs-lookup"><span data-stu-id="abae4-101">tooescalate privileges, type:</span></span>
   
        sudo -s
   
    <span data-ttu-id="abae4-102">Ange ditt lösenord</span><span class="sxs-lookup"><span data-stu-id="abae4-102">Enter your password.</span></span>
2. <span data-ttu-id="abae4-103">tooinstall MySQL Community Server edition, skriver du:</span><span class="sxs-lookup"><span data-stu-id="abae4-103">tooinstall MySQL Community Server edition, type:</span></span>
   
        zypper install mysql-community-server
   
    <span data-ttu-id="abae4-104">Vänta medan MySQL hämtas och installeras.</span><span class="sxs-lookup"><span data-stu-id="abae4-104">Wait while MySQL downloads and installs.</span></span>
3. <span data-ttu-id="abae4-105">tooset MySQL toostart när hello systemet startas, typ:</span><span class="sxs-lookup"><span data-stu-id="abae4-105">tooset MySQL toostart when hello system boots, type:</span></span>
   
        insserv mysql
4. <span data-ttu-id="abae4-106">Starta hello MySQL-daemon (mysqld) manuellt med det här kommandot:</span><span class="sxs-lookup"><span data-stu-id="abae4-106">Start hello MySQL daemon (mysqld) manually with this command:</span></span>
   
        rcmysql start
   
    <span data-ttu-id="abae4-107">toocheck hello status för hello MySQL daemon, skriver du:</span><span class="sxs-lookup"><span data-stu-id="abae4-107">toocheck hello status of hello MySQL daemon, type:</span></span>
   
        rcmysql status
   
    <span data-ttu-id="abae4-108">toostop hello MySQL daemon, skriver du:</span><span class="sxs-lookup"><span data-stu-id="abae4-108">toostop hello MySQL daemon, type:</span></span>
   
        rcmysql stop
   
   > [!IMPORTANT]
   > <span data-ttu-id="abae4-109">Hello MySQL rotlösenordet är tomt som standard efter installationen.</span><span class="sxs-lookup"><span data-stu-id="abae4-109">After installation, hello MySQL root password is empty by default.</span></span> <span data-ttu-id="abae4-110">Vi rekommenderar att du kör **mysql\_säker\_installation**, ett skript som hjälper till att säkra MySQL.</span><span class="sxs-lookup"><span data-stu-id="abae4-110">We recommended that you run **mysql\_secure\_installation**, a script that helps secure MySQL.</span></span> <span data-ttu-id="abae4-111">hello ombeds du toochange hello MySQL rotlösenordet, ta bort anonyma användarkonton, inaktivera fjärråtkomst rot-inloggningar, ta bort databaser för test och Läs in hello privilegier tabell.</span><span class="sxs-lookup"><span data-stu-id="abae4-111">hello script prompts you toochange hello MySQL root password, remove anonymous user accounts, disable remote root logins, remove test databases, and reload hello privileges table.</span></span> <span data-ttu-id="abae4-112">Vi rekommenderar att du svara Ja tooall av dessa alternativ och ändra hello rotlösenordet.</span><span class="sxs-lookup"><span data-stu-id="abae4-112">We recommended that you answer yes tooall of these options and change hello root password.</span></span>
   > 
   > 
5. <span data-ttu-id="abae4-113">Skriv toorun hello skriptet MySQL installationsskriptet:</span><span class="sxs-lookup"><span data-stu-id="abae4-113">Type this toorun hello script MySQL installation script:</span></span>
   
        mysql_secure_installation
6. <span data-ttu-id="abae4-114">Logga in tooMySQL:</span><span class="sxs-lookup"><span data-stu-id="abae4-114">Log in tooMySQL:</span></span>
   
        mysql -u root -p
   
    <span data-ttu-id="abae4-115">Ange rotlösenordet för hello MySQL (som du har ändrat i hello föregående steg) och visas med en uppmaning där du kan utfärda SQL-instruktioner toointeract med hello-databas.</span><span class="sxs-lookup"><span data-stu-id="abae4-115">Enter hello MySQL root password (which you changed in hello previous step) and you'll be presented with a prompt where you can issue SQL statements toointeract with hello database.</span></span>
7. <span data-ttu-id="abae4-116">toocreate en ny MySQL-användare kör hello följande på hello **mysql >** prompten:</span><span class="sxs-lookup"><span data-stu-id="abae4-116">toocreate a new MySQL user, run hello following at hello **mysql>** prompt:</span></span>
   
        CREATE USER 'mysqluser'@'localhost' IDENTIFIED BY 'password';
   
    <span data-ttu-id="abae4-117">Observera hello semikolon (;) hello slutet av hello raderna är avgörande för slutar hello-kommandon.</span><span class="sxs-lookup"><span data-stu-id="abae4-117">Note, hello semi-colons (;) at hello end of hello lines are crucial for ending hello commands.</span></span>
8. <span data-ttu-id="abae4-118">toocreate en databas och bevilja hello `mysqluser` behörigheter på den problemet hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="abae4-118">toocreate a database and grant hello `mysqluser` user permissions on it, issue hello following commands:</span></span>
   
        CREATE DATABASE testdatabase;
        GRANT ALL ON testdatabase.* too'mysqluser'@'localhost' IDENTIFIED BY 'password';
   
    <span data-ttu-id="abae4-119">Observera att databasen användarnamn och lösenord används endast av skript som ansluter toohello databas.</span><span class="sxs-lookup"><span data-stu-id="abae4-119">Note that database user names and passwords are only used by scripts connecting toohello database.</span></span>  <span data-ttu-id="abae4-120">Databasen användarkontonamn representerar inte nödvändigtvis faktiska användarkonton på hello system.</span><span class="sxs-lookup"><span data-stu-id="abae4-120">Database user account names do not necessarily represent actual user accounts on hello system.</span></span>
9. <span data-ttu-id="abae4-121">toolog i från en annan dator, typ:</span><span class="sxs-lookup"><span data-stu-id="abae4-121">toolog in from another computer, type:</span></span>
   
        GRANT ALL ON testdatabase.* too'mysqluser'@'<ip-address>' IDENTIFIED BY 'password';
   
    <span data-ttu-id="abae4-122">där `ip-address` är hello hello datorns som ansluter du tooMySQL IP-adress.</span><span class="sxs-lookup"><span data-stu-id="abae4-122">where `ip-address` is hello IP address of hello computer from which you will connect tooMySQL.</span></span>
10. <span data-ttu-id="abae4-123">tooexit hello MySQL-databas Administrationsverktyg för skriver du:</span><span class="sxs-lookup"><span data-stu-id="abae4-123">tooexit hello MySQL database administration utility, type:</span></span>
    
        quit

## <a name="add-an-endpoint"></a><span data-ttu-id="abae4-124">Lägga till en slutpunkt</span><span class="sxs-lookup"><span data-stu-id="abae4-124">Add an endpoint</span></span>
1. <span data-ttu-id="abae4-125">När MySQL installeras, måste tooconfigure en slutpunkt tooaccess MySQL på distans.</span><span class="sxs-lookup"><span data-stu-id="abae4-125">After MySQL is installed, you'll need tooconfigure an endpoint tooaccess MySQL remotely.</span></span> <span data-ttu-id="abae4-126">Logga in toohello [klassiska Azure-portalen][AzurePortal].</span><span class="sxs-lookup"><span data-stu-id="abae4-126">Log in toohello [Azure  classic portal][AzurePortal].</span></span> <span data-ttu-id="abae4-127">Klicka på **virtuella datorer**hello namnet på den nya virtuella datorn och klicka sedan på **slutpunkter**.</span><span class="sxs-lookup"><span data-stu-id="abae4-127">Click **Virtual Machines**, click hello name of your new virtual machine, and then click **Endpoints**.</span></span>
2. <span data-ttu-id="abae4-128">Klicka på **Lägg till** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="abae4-128">Click **Add** at hello bottom of hello page.</span></span>
3. <span data-ttu-id="abae4-129">Lägga till en slutpunkt med namnet ”MySQL” med protokollet **TCP**, och **offentliga** och **privata** portarna set för ”3306”.</span><span class="sxs-lookup"><span data-stu-id="abae4-129">Add an endpoint named "MySQL" with protocol **TCP**, and **Public** and **Private** ports set too"3306".</span></span>
4. <span data-ttu-id="abae4-130">tooremotely Anslut toohello virtuell dator från datorn, typ:</span><span class="sxs-lookup"><span data-stu-id="abae4-130">tooremotely connect toohello virtual machine from your computer, type:</span></span>
   
        mysql -u mysqluser -p -h <yourservicename>.cloudapp.net
   
    <span data-ttu-id="abae4-131">Till exempel använda hello virtuell dator som vi skapade i den här kursen, ange följande kommando:</span><span class="sxs-lookup"><span data-stu-id="abae4-131">For example, using hello virual machine we created in this tutorial, type this command:</span></span>
   
        mysql -u mysqluser -p -h testlinuxvm.cloudapp.net

[MySQLDocs]: http://dev.mysql.com/doc/
[AzurePortal]: http://manage.windowsazure.com

[Image9]: ./media/install-and-run-mysql-on-opensuse-vm/LinuxVmAddEndpointMySQL.png
