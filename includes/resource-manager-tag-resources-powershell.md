<span data-ttu-id="eefcc-101">Version 3.0 av hello AzureRm.Resources modulen med viktiga ändringar i hur du arbetar med taggar.</span><span class="sxs-lookup"><span data-stu-id="eefcc-101">Version 3.0 of hello AzureRm.Resources module included significant changes in how you work with tags.</span></span> <span data-ttu-id="eefcc-102">Innan du fortsätter kontrollerar du din version:</span><span class="sxs-lookup"><span data-stu-id="eefcc-102">Before you proceed, check your version:</span></span>

```powershell
Get-Module -ListAvailable -Name AzureRm.Resources | Select Version
```

<span data-ttu-id="eefcc-103">Om resultatet visar version 3.0 eller senare, hello exemplen i det här avsnittet att arbeta med din miljö.</span><span class="sxs-lookup"><span data-stu-id="eefcc-103">If your results show version 3.0 or later, hello examples in this topic work with your environment.</span></span> <span data-ttu-id="eefcc-104">Om du inte har version 3.0 eller senare måste du [uppdatera versionen](/powershell/azureps-cmdlets-docs/) med hjälp av PowerShell-galleriet eller med Installationsprogram för webbplattform innan du fortsätter med det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="eefcc-104">If you do not have version 3.0 or later, [update your version](/powershell/azureps-cmdlets-docs/) by using PowerShell Gallery or Web Platform Installer before you proceed with this topic.</span></span>

```powershell
Version
-------
3.5.0
```

<span data-ttu-id="eefcc-105">toosee hello befintliga taggar för en *resursgruppen*, Använd:</span><span class="sxs-lookup"><span data-stu-id="eefcc-105">toosee hello existing tags for a *resource group*, use:</span></span>

```powershell
(Get-AzureRmResourceGroup -Name examplegroup).Tags
```

<span data-ttu-id="eefcc-106">Skriptet returnerar hello följande format:</span><span class="sxs-lookup"><span data-stu-id="eefcc-106">That script returns hello following format:</span></span>

```powershell
Name                           Value
----                           -----
Dept                           IT
Environment                    Test
```

<span data-ttu-id="eefcc-107">toosee hello befintliga taggar för en *resurs som har en viss resurs-ID*, Använd:</span><span class="sxs-lookup"><span data-stu-id="eefcc-107">toosee hello existing tags for a *resource that has a specified resource ID*, use:</span></span>

```powershell
(Get-AzureRmResource -ResourceId {resource-id}).Tags
```

<span data-ttu-id="eefcc-108">Eller toosee hello befintliga taggar för en *resurs som har en viss grupp av namn och resursen*, Använd:</span><span class="sxs-lookup"><span data-stu-id="eefcc-108">Or, toosee hello existing tags for a *resource that has a specified name and resource group*, use:</span></span>

```powershell
(Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup).Tags
```

<span data-ttu-id="eefcc-109">tooget *resursgrupper som har en specifik tagg*, Använd:</span><span class="sxs-lookup"><span data-stu-id="eefcc-109">tooget *resource groups that have a specific tag*, use:</span></span>

```powershell
(Find-AzureRmResourceGroup -Tag @{ Dept="Finance" }).Name 
```

<span data-ttu-id="eefcc-110">tooget *resurser som har en specifik tagg*, Använd:</span><span class="sxs-lookup"><span data-stu-id="eefcc-110">tooget *resources that have a specific tag*, use:</span></span>

```powershell
(Find-AzureRmResource -TagName Dept -TagValue Finance).Name
```

<span data-ttu-id="eefcc-111">Varje gång du tillämpar taggar tooa resurs eller en resursgrupp kan du skriva över hello befintliga taggar på denna resurs eller resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="eefcc-111">Every time you apply tags tooa resource or a resource group, you overwrite hello existing tags on that resource or resource group.</span></span> <span data-ttu-id="eefcc-112">Därför måste du använda en annan metod baserat på om hello resurs eller resursgrupp har befintliga taggar.</span><span class="sxs-lookup"><span data-stu-id="eefcc-112">Therefore, you must use a different approach based on whether hello resource or resource group has existing tags.</span></span> 

<span data-ttu-id="eefcc-113">tooadd taggar tooa *resursgruppen utan befintliga taggar*, Använd:</span><span class="sxs-lookup"><span data-stu-id="eefcc-113">tooadd tags tooa *resource group without existing tags*, use:</span></span>

```powershell
Set-AzureRmResourceGroup -Name examplegroup -Tag @{ Dept="IT"; Environment="Test" }
```

<span data-ttu-id="eefcc-114">tooadd taggar tooa *resursgrupp som har befintliga taggar*, hämta hello befintliga taggar, lägga till nya hello-tagg och tillämpa hello taggar:</span><span class="sxs-lookup"><span data-stu-id="eefcc-114">tooadd tags tooa *resource group that has existing tags*, retrieve hello existing tags, add hello new tag, and reapply hello tags:</span></span>

```powershell
$tags = (Get-AzureRmResourceGroup -Name examplegroup).Tags
$tags += @{Status="Approved"}
Set-AzureRmResourceGroup -Tag $tags -Name examplegroup
```

<span data-ttu-id="eefcc-115">tooadd taggar tooa *resursen utan befintliga taggar*, Använd:</span><span class="sxs-lookup"><span data-stu-id="eefcc-115">tooadd tags tooa *resource without existing tags*, use:</span></span>

```powershell
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test" } -ResourceName examplevnet -ResourceGroupName examplegroup
```

<span data-ttu-id="eefcc-116">tooadd taggar tooa *resurs som har befintliga taggar*, Använd:</span><span class="sxs-lookup"><span data-stu-id="eefcc-116">tooadd tags tooa *resource that has existing tags*, use:</span></span>

```powershell
$tags = (Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup).Tags
$tags += @{Status="Approved"}
Set-AzureRmResource -Tag $tags -ResourceName examplevnet -ResourceGroupName examplegroup
```

<span data-ttu-id="eefcc-117">tooapply alla taggar från resursen grupp tooits resurser och *inte behålla befintliga taggar på hello resurser*, använda hello följande skript:</span><span class="sxs-lookup"><span data-stu-id="eefcc-117">tooapply all tags from a resource group tooits resources, and *not retain existing tags on hello resources*, use hello following script:</span></span>

```powershell
$groups = Get-AzureRmResourceGroup
foreach ($g in $groups) 
{
    Find-AzureRmResource -ResourceGroupNameEquals $g.ResourceGroupName | ForEach-Object {Set-AzureRmResource -ResourceId $_.ResourceId -Tag $g.Tags -Force } 
}
```

<span data-ttu-id="eefcc-118">tooapply alla taggar från resursen grupp tooits resurser och *behåller befintliga taggar för resurser som inte är dubbletter*, använda hello följande skript:</span><span class="sxs-lookup"><span data-stu-id="eefcc-118">tooapply all tags from a resource group tooits resources, and *retain existing tags on resources that are not duplicates*, use hello following script:</span></span>

```powershell
$groups = Get-AzureRmResourceGroup
foreach ($g in $groups) 
{
    if ($g.Tags -ne $null) {
        $resources = Find-AzureRmResource -ResourceGroupNameEquals $g.ResourceGroupName 
        foreach ($r in $resources)
        {
            $resourcetags = (Get-AzureRmResource -ResourceId $r.ResourceId).Tags
            foreach ($key in $g.Tags.Keys)
            {
                if ($resourcetags.ContainsKey($key)) { $resourcetags.Remove($key) }
            }
            $resourcetags += $g.Tags
            Set-AzureRmResource -Tag $resourcetags -ResourceId $r.ResourceId -Force
        }
    }
}
```

<span data-ttu-id="eefcc-119">tooremove alla taggar skicka en tom hash-tabellen:</span><span class="sxs-lookup"><span data-stu-id="eefcc-119">tooremove all tags, pass an empty hash table:</span></span>

```powershell
Set-AzureRmResourceGroup -Tag @{} -Name examplegroup
```



