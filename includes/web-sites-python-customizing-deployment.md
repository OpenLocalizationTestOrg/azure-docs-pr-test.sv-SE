<span data-ttu-id="ec4b3-101">Azure kommer att fastställa att ditt program använder Python **om bägge följande villkor stämmer**:</span><span class="sxs-lookup"><span data-stu-id="ec4b3-101">Azure will determine that your application uses Python **if both of these conditions are true**:</span></span>

* <span data-ttu-id="ec4b3-102">Requirements.txt filen i rotmappen för hello</span><span class="sxs-lookup"><span data-stu-id="ec4b3-102">requirements.txt file in hello root folder</span></span>
* <span data-ttu-id="ec4b3-103">någon .py-fil i rotmappen för hello eller en runtime.txt som specificerar python</span><span class="sxs-lookup"><span data-stu-id="ec4b3-103">any .py file in hello root folder OR a runtime.txt that specifies python</span></span>

<span data-ttu-id="ec4b3-104">När så är fallet hello används en Python-specifikt distributionsskript vilket utför hello standardsynkronisering av filer såväl som ytterligare Python-åtgärder som:</span><span class="sxs-lookup"><span data-stu-id="ec4b3-104">When that's hello case, it will use a Python specific deployment script, which performs hello standard synchronization of files, as well as additional Python operations such as:</span></span>

* <span data-ttu-id="ec4b3-105">Automatisk hantering av virtuell miljö</span><span class="sxs-lookup"><span data-stu-id="ec4b3-105">Automatic management of virtual environment</span></span>
* <span data-ttu-id="ec4b3-106">Installation av paket som listas i requirements.txt med pip</span><span class="sxs-lookup"><span data-stu-id="ec4b3-106">Installation of packages listed in requirements.txt using pip</span></span>
* <span data-ttu-id="ec4b3-107">Skapa hello lämplig web.config baserat på hello valda Python-versionen.</span><span class="sxs-lookup"><span data-stu-id="ec4b3-107">Creation of hello appropriate web.config based on hello selected Python version.</span></span>
* <span data-ttu-id="ec4b3-108">Samla in statiska filer för Django-program</span><span class="sxs-lookup"><span data-stu-id="ec4b3-108">Collect static files for Django applications</span></span>

<span data-ttu-id="ec4b3-109">Du kan kontrollera vissa aspekter av hello standard-distributionsstegen utan att behöva toocustomize hello skript.</span><span class="sxs-lookup"><span data-stu-id="ec4b3-109">You can control certain aspects of hello default deployment steps without having toocustomize hello script.</span></span>

<span data-ttu-id="ec4b3-110">Om du vill tooskip alla Python-specifika distributionssteg, kan du skapa den här tomma filen:</span><span class="sxs-lookup"><span data-stu-id="ec4b3-110">If you want tooskip all Python specific deployment steps, you can create this empty file:</span></span>

    \.skipPythonDeployment

<span data-ttu-id="ec4b3-111">För mer kontroll över distributionen kan du åsidosätta hello standard-distributionsskriptet genom att skapa hello följande filer:</span><span class="sxs-lookup"><span data-stu-id="ec4b3-111">For more control over deployment, you can override hello default deployment script by creating hello following files:</span></span>

    \.deployment
    \deploy.cmd

<span data-ttu-id="ec4b3-112">Du kan använda hello [Azure-kommandoradsgränssnittet] [ Azure command-line interface] toocreate hello filer.</span><span class="sxs-lookup"><span data-stu-id="ec4b3-112">You can use hello [Azure command-line interface][Azure command-line interface] toocreate hello files.</span></span>  <span data-ttu-id="ec4b3-113">Använd det här kommandot från din projektmapp:</span><span class="sxs-lookup"><span data-stu-id="ec4b3-113">Use this command from your project folder:</span></span>

    azure site deploymentscript --python

<span data-ttu-id="ec4b3-114">Om de här filerna inte finns så skapar Azure ett tillfälligt distributionsskript och kör det.</span><span class="sxs-lookup"><span data-stu-id="ec4b3-114">When these files don't exist, Azure creates a temporary deployment script and runs it.</span></span>  <span data-ttu-id="ec4b3-115">Det är identiska toohello som du har skapat med hello kommandot ovan.</span><span class="sxs-lookup"><span data-stu-id="ec4b3-115">It is identical toohello one you create with hello command above.</span></span>

[Azure command-line interface]: http://azure.microsoft.com/downloads/
