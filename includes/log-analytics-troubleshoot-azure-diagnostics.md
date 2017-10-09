### <a name="troubleshoot-azure-diagnostics"></a><span data-ttu-id="816ad-101">Felsöka Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="816ad-101">Troubleshoot Azure Diagnostics</span></span>

<span data-ttu-id="816ad-102">Om du får följande felmeddelande hello är hello Microsoft.insights resursprovidern inte registrerad:</span><span class="sxs-lookup"><span data-stu-id="816ad-102">If you receive hello following error message, hello Microsoft.insights resource provider is not registered:</span></span>

`Failed tooupdate diagnostics for 'resource'. {"code":"Forbidden","message":"Please register hello subscription 'subscription id' with Microsoft.Insights."}`

<span data-ttu-id="816ad-103">tooregister hello resursleverantör, utföra hello följa stegen i hello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="816ad-103">tooregister hello resource provider, perform hello following steps in hello Azure portal:</span></span>

1.  <span data-ttu-id="816ad-104">Hello navigeringsfönstret hello vänster, klicka på *prenumerationer*</span><span class="sxs-lookup"><span data-stu-id="816ad-104">In hello navigation pane on hello left, click *Subscriptions*</span></span>
2.  <span data-ttu-id="816ad-105">Välj hello-prenumeration som identifieras i hello felmeddelande</span><span class="sxs-lookup"><span data-stu-id="816ad-105">Select hello subscription identified in hello error message</span></span>
3.  <span data-ttu-id="816ad-106">Klicka på *Resursprovidrar*</span><span class="sxs-lookup"><span data-stu-id="816ad-106">Click *Resource Providers*</span></span>
4.  <span data-ttu-id="816ad-107">Hitta hello *Microsoft.insights* provider</span><span class="sxs-lookup"><span data-stu-id="816ad-107">Find hello *Microsoft.insights* provider</span></span>
5.  <span data-ttu-id="816ad-108">Klicka på hello *registrera* länk</span><span class="sxs-lookup"><span data-stu-id="816ad-108">Click hello *Register* link</span></span>

![Registrera resursprovidern microsoft.insights](./media/log-analytics-troubleshoot-azure-diagnostics/log-analytics-register-microsoft-diagnostics-resource-provider.png)

<span data-ttu-id="816ad-110">En gång hello *Microsoft.insights* resursprovidern har registrerats och försök sedan att konfigurera diagnostik.</span><span class="sxs-lookup"><span data-stu-id="816ad-110">Once hello *Microsoft.insights* resource provider is registered, retry configuring diagnostics.</span></span>


<span data-ttu-id="816ad-111">I PowerShell om du får följande felmeddelande hello, måste tooupdate din version av PowerShell:</span><span class="sxs-lookup"><span data-stu-id="816ad-111">In PowerShell, if you receive hello following error message, you need tooupdate your version of PowerShell:</span></span>

`Set-AzureRmDiagnosticSetting : A parameter cannot be found that matches parameter name 'WorkspaceId'.`

<span data-ttu-id="816ad-112">Uppdatera din version av PowerShell toohello November 2016 (v2.3.0) eller senare, frigöra använder hello instruktioner i hello [Kom igång med Azure PowerShell-cmdlets](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) artikel.</span><span class="sxs-lookup"><span data-stu-id="816ad-112">Update your version of PowerShell toohello November 2016 (v2.3.0), or later, release using hello instructions in hello [Get started with Azure PowerShell cmdlets](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) article.</span></span>
