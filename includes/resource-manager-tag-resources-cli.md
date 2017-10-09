<span data-ttu-id="2a0cb-101">tooadd en tagg tooa resursgrupp, Använd **azure har ställts in**.</span><span class="sxs-lookup"><span data-stu-id="2a0cb-101">tooadd a tag tooa resource group, use **azure group set**.</span></span> <span data-ttu-id="2a0cb-102">Om hello resursgruppen inte har några befintliga taggar kan överföra i hello-taggen.</span><span class="sxs-lookup"><span data-stu-id="2a0cb-102">If hello resource group does not have any existing tags, pass in hello tag.</span></span>

```azurecli
azure group set -n tag-demo-group -t Dept=Finance
```

<span data-ttu-id="2a0cb-103">Taggar uppdateras som helhet.</span><span class="sxs-lookup"><span data-stu-id="2a0cb-103">Tags are updated as a whole.</span></span> <span data-ttu-id="2a0cb-104">Om du vill tooadd en tagg tooa resursgrupp som har befintliga taggar måste klara alla hello-taggar.</span><span class="sxs-lookup"><span data-stu-id="2a0cb-104">If you want tooadd a tag tooa resource group that has existing tags, pass all hello tags.</span></span> 

```azurecli
azure group set -n tag-demo-group -t Dept=Finance;Environment=Production;Project=Upgrade
```

<span data-ttu-id="2a0cb-105">Taggar ärvs inte av resurser i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="2a0cb-105">Tags are not inherited by resources in a resource group.</span></span> <span data-ttu-id="2a0cb-106">tooadd en tagg tooa resurs, Använd **azure resursuppsättningen**.</span><span class="sxs-lookup"><span data-stu-id="2a0cb-106">tooadd a tag tooa resource, use **azure resource set**.</span></span> <span data-ttu-id="2a0cb-107">Skicka hello API-versionsnumret för hello resurstyp som du lägger till hello tagg.</span><span class="sxs-lookup"><span data-stu-id="2a0cb-107">Pass hello API version number for hello resource type that you are adding hello tag to.</span></span> <span data-ttu-id="2a0cb-108">Om du behöver tooretrieve hello API-version, Använd hello följande kommando med hello resource provider för hello-typ som du anger:</span><span class="sxs-lookup"><span data-stu-id="2a0cb-108">If you need tooretrieve hello API version, use hello following command with hello resource provider for hello type you are setting:</span></span>

```azurecli
azure provider show -n Microsoft.Storage --json
```

<span data-ttu-id="2a0cb-109">Leta efter hello resurstyp som du vill ha i hello resultat.</span><span class="sxs-lookup"><span data-stu-id="2a0cb-109">In hello results, look for hello resource type you want.</span></span>

```azurecli
"resourceTypes": [
{
  "resourceType": "storageAccounts",
  ...
  "apiVersions": [
    "2016-01-01",
    "2015-06-15",
    "2015-05-01-preview"
  ]
}
...
```

<span data-ttu-id="2a0cb-110">Nu kan ange den API-versionen, resursgruppens namn, resurs, resurstyp och Taggvärde som parametrar.</span><span class="sxs-lookup"><span data-stu-id="2a0cb-110">Now, provide that API version, resource group name, resource name, resource type, and tag value as parameters.</span></span>

```azurecli
azure resource set -g tag-demo-group -n storagetagdemo -r Microsoft.Storage/storageAccounts -t Dept=Finance -o 2016-01-01
```

<span data-ttu-id="2a0cb-111">Taggar finns direkt på resurser och resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="2a0cb-111">Tags exist directly on resources and resource groups.</span></span> <span data-ttu-id="2a0cb-112">toosee hello befintliga taggar, hämta en resursgrupp och dess resurser med **azure-grupp visa**.</span><span class="sxs-lookup"><span data-stu-id="2a0cb-112">toosee hello existing tags, get a resource group and its resources with **azure group show**.</span></span>

```azurecli
azure group show -n tag-demo-group --json
```

<span data-ttu-id="2a0cb-113">Som returnerar metadata om hello resursgrupp, inklusive eventuella märkningar tooit.</span><span class="sxs-lookup"><span data-stu-id="2a0cb-113">Which returns metadata about hello resource group, including any tags applied tooit.</span></span>

```azurecli
{
  "id": "/subscriptions/4705409c-9372-42f0-914c-64a504530837/resourceGroups/tag-demo-group",
  "name": "tag-demo-group",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "location": "southcentralus",
  "tags": {
    "Dept": "Finance",
    "Environment": "Production",
    "Project": "Upgrade"
  },
  ...
}
```

<span data-ttu-id="2a0cb-114">Du visar hello taggar för en viss resurs med hjälp av **azure-resurs visas**.</span><span class="sxs-lookup"><span data-stu-id="2a0cb-114">You view hello tags for a particular resource by using **azure resource show**.</span></span>

```azurecli
azure resource show -g tag-demo-group -n storagetagdemo -r Microsoft.Storage/storageAccounts -o 2016-01-01 --json
```

<span data-ttu-id="2a0cb-115">tooretrieve alla hello resurser med ett Taggvärde använder:</span><span class="sxs-lookup"><span data-stu-id="2a0cb-115">tooretrieve all hello resources with a tag value, use:</span></span>

```azurecli
azure resource list -t Dept=Finance --json
```

<span data-ttu-id="2a0cb-116">tooretrieve Använd för alla resursgrupper för hello med ett Taggvärde:</span><span class="sxs-lookup"><span data-stu-id="2a0cb-116">tooretrieve all hello resource groups with a tag value, use:</span></span>

```azurecli
azure group list -t Dept=Finance
```

<span data-ttu-id="2a0cb-117">Du kan visa befintliga hello-taggar i din prenumeration med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="2a0cb-117">You can view hello existing tags in your subscription with hello following command:</span></span>

```azurecli
azure tag list
```
