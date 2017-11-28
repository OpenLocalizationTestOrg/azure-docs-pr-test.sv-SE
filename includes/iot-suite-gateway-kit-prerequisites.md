## <a name="prerequisites"></a><span data-ttu-id="47552-101">Krav</span><span class="sxs-lookup"><span data-stu-id="47552-101">Prerequisites</span></span>

<span data-ttu-id="47552-102">Du behöver en aktiv Azure-prenumeration för att kunna utföra stegen i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="47552-102">To complete this tutorial, you need an active Azure subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="47552-103">Om du inte har något konto kan du skapa ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="47552-103">If you don’t have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="47552-104">Mer information finns i [kostnadsfri utvärderingsversion av Azure][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="47552-104">For details, see [Azure Free Trial][lnk-free-trial].</span></span>

### <a name="required-software"></a><span data-ttu-id="47552-105">Programvara som krävs</span><span class="sxs-lookup"><span data-stu-id="47552-105">Required software</span></span>

<span data-ttu-id="47552-106">Du måste SSH-klienten på den stationära datorn så att du kan fjärransluta till kommandoraden på Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="47552-106">You need SSH client on your desktop machine to enable you to remotely access the command line on the Intel NUC.</span></span>

- <span data-ttu-id="47552-107">Windows innehåller inte en SSH-klient.</span><span class="sxs-lookup"><span data-stu-id="47552-107">Windows does not include an SSH client.</span></span> <span data-ttu-id="47552-108">Vi rekommenderar att du använder [PuTTY](http://www.putty.org/).</span><span class="sxs-lookup"><span data-stu-id="47552-108">We recommend using [PuTTY](http://www.putty.org/).</span></span>
- <span data-ttu-id="47552-109">De flesta distributioner för Linux och Mac OS inkluderar SSH-kommandoradsverktyget.</span><span class="sxs-lookup"><span data-stu-id="47552-109">Most Linux distributions and Mac OS include the command-line SSH utility.</span></span>

### <a name="required-hardware"></a><span data-ttu-id="47552-110">Nödvändig maskinvara</span><span class="sxs-lookup"><span data-stu-id="47552-110">Required hardware</span></span>

<span data-ttu-id="47552-111">En stationär dator så att du kan fjärransluta till kommandoraden på Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="47552-111">A desktop computer to enable you to connect remotely to the command line on the Intel NUC.</span></span>

<span data-ttu-id="47552-112">[IoT-kommersiella Gateway Kit][lnk-starter-kits].</span><span class="sxs-lookup"><span data-stu-id="47552-112">[IoT Commercial Gateway Kit][lnk-starter-kits].</span></span> <span data-ttu-id="47552-113">Den här kursen använder följande objekt från kit:</span><span class="sxs-lookup"><span data-stu-id="47552-113">This tutorial uses the following items from the kit:</span></span>

- <span data-ttu-id="47552-114">Intel® NUC Kit DE3815TYKE med 4G minne och Bluetooth expansion-kort</span><span class="sxs-lookup"><span data-stu-id="47552-114">Intel® NUC Kit DE3815TYKE with 4G Memory and Bluetooth expansion card</span></span>
- <span data-ttu-id="47552-115">Power adapter</span><span class="sxs-lookup"><span data-stu-id="47552-115">Power adaptor</span></span>

[lnk-starter-kits]: https://azure.microsoft.com/develop/iot/starter-kits/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/