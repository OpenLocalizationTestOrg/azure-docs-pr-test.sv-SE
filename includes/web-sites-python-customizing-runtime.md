<span data-ttu-id="020a2-101">Azure fastställer hello version av Python toouse för sin virtuella miljö med hello efter prioritet:</span><span class="sxs-lookup"><span data-stu-id="020a2-101">Azure will determine hello version of Python toouse for its virtual environment with hello following priority:</span></span>

1. <span data-ttu-id="020a2-102">versionen som anges i runtime.txt i rotmappen för hello</span><span class="sxs-lookup"><span data-stu-id="020a2-102">version specified in runtime.txt in hello root folder</span></span>
2. <span data-ttu-id="020a2-103">versionen som anges i Python-inställningen i webbappkonfigurationen hello (hello **inställningar** > **programinställningar** bladet för din webbapp i hello Azure Portal)</span><span class="sxs-lookup"><span data-stu-id="020a2-103">version specified by Python setting in hello web app configuration (hello **Settings** > **Application Settings** blade for your web app in hello Azure Portal)</span></span>
3. <span data-ttu-id="020a2-104">Python-2.7 är hello standard om inget av ovanstående hello har angetts</span><span class="sxs-lookup"><span data-stu-id="020a2-104">python-2.7 is hello default if none of hello above are specified</span></span>

<span data-ttu-id="020a2-105">Giltiga värden för hello innehållet i</span><span class="sxs-lookup"><span data-stu-id="020a2-105">Valid values for hello contents of</span></span> 

    \runtime.txt

<span data-ttu-id="020a2-106">är:</span><span class="sxs-lookup"><span data-stu-id="020a2-106">are:</span></span>

* <span data-ttu-id="020a2-107">python-2.7</span><span class="sxs-lookup"><span data-stu-id="020a2-107">python-2.7</span></span>
* <span data-ttu-id="020a2-108">python-3.4</span><span class="sxs-lookup"><span data-stu-id="020a2-108">python-3.4</span></span>

<span data-ttu-id="020a2-109">Om hello mikroversion (tredje siffran) har angetts, ignoreras.</span><span class="sxs-lookup"><span data-stu-id="020a2-109">If hello micro version (third digit) is specified, it is ignored.</span></span>

