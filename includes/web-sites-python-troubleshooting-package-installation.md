<span data-ttu-id="3c046-101">En del paket kanske inte installeras med pip när det körs på Azure.</span><span class="sxs-lookup"><span data-stu-id="3c046-101">Some packages may not install using pip when run on Azure.</span></span>  <span data-ttu-id="3c046-102">Det kan vara att hello-paketet inte är tillgängligt på hello Python Package Index.</span><span class="sxs-lookup"><span data-stu-id="3c046-102">It may simply be that hello package is not available on hello Python Package Index.</span></span>  <span data-ttu-id="3c046-103">Det kan vara att det krävs en kompilator (en kompilator är inte tillgänglig på hello datorn hello webbapp som körs i Azure App Service).</span><span class="sxs-lookup"><span data-stu-id="3c046-103">It could be that a compiler is required (a compiler is not available on hello machine running hello web app in Azure App Service).</span></span>

<span data-ttu-id="3c046-104">I det här avsnittet ska vi titta på olika sätt toodeal med det här problemet.</span><span class="sxs-lookup"><span data-stu-id="3c046-104">In this section, we'll look at ways toodeal with this issue.</span></span>

### <a name="request-wheels"></a><span data-ttu-id="3c046-105">Begär hjul</span><span class="sxs-lookup"><span data-stu-id="3c046-105">Request wheels</span></span>
<span data-ttu-id="3c046-106">Om hello installationen kräver en kompilator, bör du kontakta hello paketet ägare toorequest hjul görs tillgängliga för hello-paketet.</span><span class="sxs-lookup"><span data-stu-id="3c046-106">If hello package installation requires a compiler, you should try contacting hello package owner toorequest that wheels be made available for hello package.</span></span>

<span data-ttu-id="3c046-107">Med hello senaste tillgänglighet för [Microsoft Visual C++-kompilatorn för Python 2.7][Microsoft Visual C++ Compiler for Python 2.7], är det nu enklare toobuild paket som har inbyggd kod för Python 2.7.</span><span class="sxs-lookup"><span data-stu-id="3c046-107">With hello recent availability of [Microsoft Visual C++ Compiler for Python 2.7][Microsoft Visual C++ Compiler for Python 2.7], it is now easier toobuild packages that have native code for Python 2.7.</span></span>

### <a name="build-wheels-requires-windows"></a><span data-ttu-id="3c046-108">Skapa hjul (kräver Windows)</span><span class="sxs-lookup"><span data-stu-id="3c046-108">Build wheels (requires Windows)</span></span>
<span data-ttu-id="3c046-109">Obs: När du använder det här alternativet, se till att toocompile hello paketet med en Python-miljö som matchar hello plattform/arkitektur/version som används på hello webbapp i Azure App Service (Windows/32-bit/2.7 eller 3.4).</span><span class="sxs-lookup"><span data-stu-id="3c046-109">Note: When using this option, make sure toocompile hello package using a Python environment that matches hello platform/architecture/version that is used on hello web app in Azure App Service (Windows/32-bit/2.7 or 3.4).</span></span>

<span data-ttu-id="3c046-110">Om hello paketet inte installeras eftersom den kräver en kompilator, kan du installera hello kompilatorn på den lokala datorn och skapa ett hjul för hello-paketet som du sedan inkluderar i databasen.</span><span class="sxs-lookup"><span data-stu-id="3c046-110">If hello package doesn't install because it requires a compiler, you can install hello compiler on your local machine and build a wheel for hello package, which you will then include in your repository.</span></span>

<span data-ttu-id="3c046-111">Mac/Linux-användare: Om du inte har åtkomst tooa Windows-dator, se [skapa en virtuell dator med Windows] [ Create a Virtual Machine Running Windows] för hur toocreate en virtuell dator på Azure.</span><span class="sxs-lookup"><span data-stu-id="3c046-111">Mac/Linux Users: If you don't have access tooa Windows machine, see [Create a Virtual Machine Running Windows][Create a Virtual Machine Running Windows] for how toocreate a VM on Azure.</span></span>  <span data-ttu-id="3c046-112">Du kan använda den toobuild hello hjul, lägga till dem toohello databasen och om du vill ta bort hello VM.</span><span class="sxs-lookup"><span data-stu-id="3c046-112">You can use it toobuild hello wheels, add them toohello repository, and discard hello VM if you like.</span></span> 

<span data-ttu-id="3c046-113">För Python 2.7, kan du installera [Microsoft Visual C++-kompilatorn för Python 2.7][Microsoft Visual C++ Compiler for Python 2.7].</span><span class="sxs-lookup"><span data-stu-id="3c046-113">For Python 2.7, you can install [Microsoft Visual C++ Compiler for Python 2.7][Microsoft Visual C++ Compiler for Python 2.7].</span></span>

<span data-ttu-id="3c046-114">För Python 3.4, kan du installera [Microsoft Visual C++ 2010 Express][Microsoft Visual C++ 2010 Express].</span><span class="sxs-lookup"><span data-stu-id="3c046-114">For Python 3.4, you can install [Microsoft Visual C++ 2010 Express][Microsoft Visual C++ 2010 Express].</span></span>

<span data-ttu-id="3c046-115">toobuild hjul, behöver du hjulpaketet hello:</span><span class="sxs-lookup"><span data-stu-id="3c046-115">toobuild wheels, you'll need hello wheel package:</span></span>

    env\scripts\pip install wheel

<span data-ttu-id="3c046-116">Du kommer att använda `pip wheel` toocompile ett beroende:</span><span class="sxs-lookup"><span data-stu-id="3c046-116">You'll use `pip wheel` toocompile a dependency:</span></span>

    env\scripts\pip wheel azure==0.8.4

<span data-ttu-id="3c046-117">Detta skapar en .whl-fil i hello \wheelhouse mapp.</span><span class="sxs-lookup"><span data-stu-id="3c046-117">This creates a .whl file in hello \wheelhouse folder.</span></span>  <span data-ttu-id="3c046-118">Lägg till hello \wheelhouse mapp och hjul filer tooyour databasen.</span><span class="sxs-lookup"><span data-stu-id="3c046-118">Add hello \wheelhouse folder and wheel files tooyour repository.</span></span>

<span data-ttu-id="3c046-119">Redigera din requirements.txt tooadd hello `--find-links` alternativet hello överst.</span><span class="sxs-lookup"><span data-stu-id="3c046-119">Edit your requirements.txt tooadd hello `--find-links` option at hello top.</span></span> <span data-ttu-id="3c046-120">Det instruerar pip toolook efter en exakt matchning i hello lokala mappen innan kommer toohello python package index.</span><span class="sxs-lookup"><span data-stu-id="3c046-120">This tells pip toolook for an exact match in hello local folder before going toohello python package index.</span></span>

    --find-links wheelhouse
    azure==0.8.4

<span data-ttu-id="3c046-121">Om du vill att alla beroenden i hello \wheelhouse mappen och inte använder hello python package index alls tooinclude kan du tvinga pip tooignore hello paketindexet genom att lägga till `--no-index` toohello början av din requirements.txt.</span><span class="sxs-lookup"><span data-stu-id="3c046-121">If you want tooinclude all your dependencies in hello \wheelhouse folder and not use hello python package index at all, you can force pip tooignore hello package index by adding `--no-index` toohello top of your requirements.txt.</span></span>

    --no-index

### <a name="customize-installation"></a><span data-ttu-id="3c046-122">Anpassa installationen</span><span class="sxs-lookup"><span data-stu-id="3c046-122">Customize installation</span></span>
<span data-ttu-id="3c046-123">Du kan anpassa hello distribution skriptet tooinstall ett paket i hello virtuell miljö med hjälp av ett alternativt installationsprogram som easy\_installera.</span><span class="sxs-lookup"><span data-stu-id="3c046-123">You can customize hello deployment script tooinstall a package in hello virtual environment using an alternate installer, such as easy\_install.</span></span>  <span data-ttu-id="3c046-124">Se deploy.cmd för ett exempel som har kommenterats bort.  Se till att paketen inte listats i requirements.txt tooprevent pip installerar dem..</span><span class="sxs-lookup"><span data-stu-id="3c046-124">See deploy.cmd for an example that is commented out.  Make sure that such packages aren't listed in requirements.txt, tooprevent pip from installing them.</span></span>

<span data-ttu-id="3c046-125">Lägg till den här toohello distributionsskriptet:</span><span class="sxs-lookup"><span data-stu-id="3c046-125">Add this toohello deployment script:</span></span>

    env\scripts\easy_install somepackage

<span data-ttu-id="3c046-126">Du kan också vara kan enkelt toouse\_installera tooinstall från ett exe-installationsprogram (en del är zip-kompatibla så easy\_install stöder dem).</span><span class="sxs-lookup"><span data-stu-id="3c046-126">You may also be able toouse easy\_install tooinstall from an exe installer (some are zip compatible, so easy\_install supports them).</span></span>  <span data-ttu-id="3c046-127">Lägg till hello installer tooyour databas och anropa easy\_installera genom att ange hello sökvägen toohello körbara.</span><span class="sxs-lookup"><span data-stu-id="3c046-127">Add hello installer tooyour repository, and invoke easy\_install by passing hello path toohello executable.</span></span>

<span data-ttu-id="3c046-128">Lägg till den här toohello distributionsskriptet:</span><span class="sxs-lookup"><span data-stu-id="3c046-128">Add this toohello deployment script:</span></span>

    env\scripts\easy_install "%DEPLOYMENT_SOURCE%\installers\somepackage.exe"

### <a name="include-hello-virtual-environment-in-hello-repository-requires-windows"></a><span data-ttu-id="3c046-129">Inkludera hello virtuella miljön i hello-databasen (kräver Windows)</span><span class="sxs-lookup"><span data-stu-id="3c046-129">Include hello virtual environment in hello repository (requires Windows)</span></span>
<span data-ttu-id="3c046-130">Obs: När du använder det här alternativet, se till att toouse en virtuell miljö som matchar hello plattform/arkitektur/version som används på hello webbapp i Azure App Service (Windows/32-bit/2.7 eller 3.4).</span><span class="sxs-lookup"><span data-stu-id="3c046-130">Note: When using this option, make sure toouse a virtual environment that matches hello platform/architecture/version that is used on hello web app in Azure App Service (Windows/32-bit/2.7 or 3.4).</span></span>

<span data-ttu-id="3c046-131">Om du inkluderar hello virtuella miljön i hello databas, kan du förhindra hello distributionsskriptet utför virtuell miljöhantering i Azure genom att skapa en tom fil:</span><span class="sxs-lookup"><span data-stu-id="3c046-131">If you include hello virtual environment in hello repository, you can prevent hello deployment script from doing virtual environment management on Azure by creating an empty file:</span></span>

    .skipPythonDeployment

<span data-ttu-id="3c046-132">Vi rekommenderar att du tar bort hello befintliga virtuella miljön i appen hello tooprevent överblivna filer från när hello virtuella miljön hanterades automatiskt.</span><span class="sxs-lookup"><span data-stu-id="3c046-132">We recommend that you delete hello existing virtual environment on hello app, tooprevent leftover files from when hello virtual environment was managed automatically.</span></span>

[Create a Virtual Machine Running Windows]: http://azure.microsoft.com/documentation/articles/virtual-machines-windows-hero-tutorial/
[Microsoft Visual C++ Compiler for Python 2.7]: http://aka.ms/vcpython27
[Microsoft Visual C++ 2010 Express]: http://go.microsoft.com/?linkid=9709949
