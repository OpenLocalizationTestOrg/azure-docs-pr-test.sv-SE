<span data-ttu-id="718dd-101">Azure kommer att fastställa att ditt program använder Python **om bägge följande villkor stämmer**:</span><span class="sxs-lookup"><span data-stu-id="718dd-101">Azure will determine that your application uses Python **if both of these conditions are true**:</span></span>

* <span data-ttu-id="718dd-102">requirements.txt filen i rotmappen</span><span class="sxs-lookup"><span data-stu-id="718dd-102">requirements.txt file in the root folder</span></span>
* <span data-ttu-id="718dd-103">någon .py-fil i rotmappen ELLER en runtime.txt som specificerar python</span><span class="sxs-lookup"><span data-stu-id="718dd-103">any .py file in the root folder OR a runtime.txt that specifies python</span></span>

<span data-ttu-id="718dd-104">När så är fallet så kommer det att använda ett Python-specifikt distributionsskript vilket utför standardsynkronisering av filer såväl som ytterligare Python-åtgärder som:</span><span class="sxs-lookup"><span data-stu-id="718dd-104">When that's the case, it will use a Python specific deployment script, which performs the standard synchronization of files, as well as additional Python operations such as:</span></span>

* <span data-ttu-id="718dd-105">Automatisk hantering av virtuell miljö</span><span class="sxs-lookup"><span data-stu-id="718dd-105">Automatic management of virtual environment</span></span>
* <span data-ttu-id="718dd-106">Installation av paket som listas i requirements.txt med pip</span><span class="sxs-lookup"><span data-stu-id="718dd-106">Installation of packages listed in requirements.txt using pip</span></span>
* <span data-ttu-id="718dd-107">Skapa en lämplig web.config baserat på den valda Python-versionen.</span><span class="sxs-lookup"><span data-stu-id="718dd-107">Creation of the appropriate web.config based on the selected Python version.</span></span>
* <span data-ttu-id="718dd-108">Samla in statiska filer för Django-program</span><span class="sxs-lookup"><span data-stu-id="718dd-108">Collect static files for Django applications</span></span>

<span data-ttu-id="718dd-109">Du kan kontrollera vissa aspekter av standard-distributionsstegen utan att behöva anpassa skriptet.</span><span class="sxs-lookup"><span data-stu-id="718dd-109">You can control certain aspects of the default deployment steps without having to customize the script.</span></span>

<span data-ttu-id="718dd-110">Om du vill hoppa över alla Python-specifika distributionssteg så kan du skapa den här tomma filen:</span><span class="sxs-lookup"><span data-stu-id="718dd-110">If you want to skip all Python specific deployment steps, you can create this empty file:</span></span>

    \.skipPythonDeployment

<span data-ttu-id="718dd-111">Om du vill hoppa över insamlingen av statiska filer för Django-programmet:</span><span class="sxs-lookup"><span data-stu-id="718dd-111">If you want to skip collection of static files for your Django application:</span></span>

    \.skipDjango 

<span data-ttu-id="718dd-112">För ytterligare kontroll över distributionen så kan du även åsidosätta standard-distributionsskriptet genom att skapa följande filer:</span><span class="sxs-lookup"><span data-stu-id="718dd-112">For more control over deployment, you can override the default deployment script by creating the following files:</span></span>

    \.deployment
    \deploy.cmd

<span data-ttu-id="718dd-113">Du kan använda den [Azure-kommandoradsgränssnittet] [ Azure command-line interface] att skapa filerna.</span><span class="sxs-lookup"><span data-stu-id="718dd-113">You can use the [Azure command-line interface][Azure command-line interface] to create the files.</span></span>  <span data-ttu-id="718dd-114">Använd det här kommandot från din projektmapp:</span><span class="sxs-lookup"><span data-stu-id="718dd-114">Use this command from your project folder:</span></span>

    azure site deploymentscript --python

<span data-ttu-id="718dd-115">Om de här filerna inte finns så skapar Azure ett tillfälligt distributionsskript och kör det.</span><span class="sxs-lookup"><span data-stu-id="718dd-115">When these files don't exist, Azure creates a temporary deployment script and runs it.</span></span>  <span data-ttu-id="718dd-116">Det är identiskt med det som du skapar med kommandot ovan.</span><span class="sxs-lookup"><span data-stu-id="718dd-116">It is identical to the one you create with the command above.</span></span>

[Azure command-line interface]: http://azure.microsoft.com/downloads/
