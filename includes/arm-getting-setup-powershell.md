## <a name="setting-up-powershell-for-resource-manager-templates"></a><span data-ttu-id="a25e3-101">Konfigurera PowerShell för Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="a25e3-101">Setting up PowerShell for Resource Manager templates</span></span>
<span data-ttu-id="a25e3-102">Innan du kan använda Azure PowerShell med Resource Manager, måste toohave hello rätt Windows PowerShell och Azure PowerShell-versioner.</span><span class="sxs-lookup"><span data-stu-id="a25e3-102">Before you can use Azure PowerShell with Resource Manager, you will need toohave hello right Windows PowerShell and Azure PowerShell versions.</span></span>

### <a name="verify-powershell-versions"></a><span data-ttu-id="a25e3-103">Kontrollera att PowerShell-versioner</span><span class="sxs-lookup"><span data-stu-id="a25e3-103">Verify PowerShell versions</span></span>
<span data-ttu-id="a25e3-104">Kontrollera att du har Windows PowerShell version 3.0 eller 4.0.</span><span class="sxs-lookup"><span data-stu-id="a25e3-104">Verify you have Windows PowerShell version 3.0 or 4.0.</span></span> <span data-ttu-id="a25e3-105">toofind hello version av Windows PowerShell, Skriv följande kommando i Kommandotolken för Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a25e3-105">toofind hello version of Windows PowerShell, type this command at a Windows PowerShell command prompt.</span></span>

    $PSVersionTable

<span data-ttu-id="a25e3-106">Du får hello efter typ av information:</span><span class="sxs-lookup"><span data-stu-id="a25e3-106">You will receive hello following type of information:</span></span>

    Name                           Value
    ----                           -----
    PSVersion                      3.0
    WSManStackVersion              3.0
    SerializationVersion           1.1.0.1
    CLRVersion                     4.0.30319.18444
    BuildVersion                   6.2.9200.16481
    PSCompatibleVersions           {1.0, 2.0, 3.0}
    PSRemotingProtocolVersion      2.2


<span data-ttu-id="a25e3-107">Kontrollera att hello-värdet för **PSVersion** 3.0 eller 4.0.</span><span class="sxs-lookup"><span data-stu-id="a25e3-107">Verify that hello value of **PSVersion** is 3.0 or 4.0.</span></span> <span data-ttu-id="a25e3-108">Om inte, se [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) eller [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).</span><span class="sxs-lookup"><span data-stu-id="a25e3-108">If not, see [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) or [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).</span></span>

### <a name="set-your-azure-account-and-subscription"></a><span data-ttu-id="a25e3-109">Konfigurera Azure-kontot och Azure-prenumerationen</span><span class="sxs-lookup"><span data-stu-id="a25e3-109">Set your Azure account and subscription</span></span>
<span data-ttu-id="a25e3-110">Om du inte redan har en Azure-prenumeration, kan du aktivera din [MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) eller registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a25e3-110">If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

<span data-ttu-id="a25e3-111">Öppna en Azure PowerShell-kommandotolk och logga in tooAzure med det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="a25e3-111">Open an Azure PowerShell command prompt and log on tooAzure with this command.</span></span>

    Login-AzureRmAccount

<span data-ttu-id="a25e3-112">Om du har flera Azure-prenumerationer, kan du visa din Azure-prenumerationer med det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="a25e3-112">If you have multiple Azure subscriptions, you can list your Azure subscriptions with this command.</span></span>

    Get-AzureRmSubscription

<span data-ttu-id="a25e3-113">Du får hello efter typ av information:</span><span class="sxs-lookup"><span data-stu-id="a25e3-113">You will receive hello following type of information:</span></span>

    SubscriptionId            : fd22919d-eaca-4f2b-841a-e4ac6770g92e
    SubscriptionName          : Visual Studio Ultimate with MSDN
    Environment               : AzureCloud
    SupportedModes            : AzureServiceManagement,AzureResourceManager
    DefaultAccount            : johndoe@contoso.com
    Accounts                  : {johndoe@contoso.com}
    IsDefault                 : True
    IsCurrent                 : True
    CurrentStorageAccountName :
    TenantId                  : 32fa88b4-86f1-419f-93ab-2d7ce016dba7

<span data-ttu-id="a25e3-114">Du kan ange hello aktuella Azure-prenumeration genom att köra dessa kommandon i Kommandotolken för hello Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a25e3-114">You can set hello current Azure subscription by running these commands at hello Azure PowerShell command prompt.</span></span> <span data-ttu-id="a25e3-115">Ersätt allt inom hello citattecken, inklusive Hej < och > tecken, med hello rätt namn.</span><span class="sxs-lookup"><span data-stu-id="a25e3-115">Replace everything within hello quotes, including hello < and > characters, with hello correct name.</span></span>

    $subscr="<SubscriptionName from hello display of Get-AzureRmSubscription>"
    Select-AzureRmSubscription -SubscriptionName $subscr -Current

<span data-ttu-id="a25e3-116">Läs mer om Azure-prenumerationer och konton [så här: ansluta tooyour prenumeration](/powershell/azureps-cmdlets-docs#step-3-connect).</span><span class="sxs-lookup"><span data-stu-id="a25e3-116">For more information about Azure subscriptions and accounts, see [How to: Connect tooyour subscription](/powershell/azureps-cmdlets-docs#step-3-connect).</span></span>

