<span data-ttu-id="a10b7-101">Azure fastställer vilken Pythonversion det ska använda för sin virtuella miljö med följande prioritet:</span><span class="sxs-lookup"><span data-stu-id="a10b7-101">Azure will determine the version of Python to use for its virtual environment with the following priority:</span></span>

1. <span data-ttu-id="a10b7-102">versionen som anges i runtime.txt i rotmappen</span><span class="sxs-lookup"><span data-stu-id="a10b7-102">version specified in runtime.txt in the root folder</span></span>
2. <span data-ttu-id="a10b7-103">versionen som anges i Python-inställningen i webbappkonfigurationen (**Inställningar** > **Programinställningar**-bladet för din webbapp i Azure Portal)</span><span class="sxs-lookup"><span data-stu-id="a10b7-103">version specified by Python setting in the web app configuration (the **Settings** > **Application Settings** blade for your web app in the Azure Portal)</span></span>
3. <span data-ttu-id="a10b7-104">python-2.7 är standard om inget av ovanstående har angetts</span><span class="sxs-lookup"><span data-stu-id="a10b7-104">python-2.7 is the default if none of the above are specified</span></span>

<span data-ttu-id="a10b7-105">Giltiga värden för innehållet i</span><span class="sxs-lookup"><span data-stu-id="a10b7-105">Valid values for the contents of</span></span> 

    \runtime.txt

<span data-ttu-id="a10b7-106">är:</span><span class="sxs-lookup"><span data-stu-id="a10b7-106">are:</span></span>

* <span data-ttu-id="a10b7-107">python-2.7</span><span class="sxs-lookup"><span data-stu-id="a10b7-107">python-2.7</span></span>
* <span data-ttu-id="a10b7-108">python-3.4</span><span class="sxs-lookup"><span data-stu-id="a10b7-108">python-3.4</span></span>

<span data-ttu-id="a10b7-109">Om mikroversion (tredje siffran) har angetts, ignoreras den.</span><span class="sxs-lookup"><span data-stu-id="a10b7-109">If the micro version (third digit) is specified, it is ignored.</span></span>

