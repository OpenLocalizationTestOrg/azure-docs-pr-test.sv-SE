Version 3.0 av hello AzureRm.Resources modulen med viktiga ändringar i hur du arbetar med taggar. Innan du fortsätter kontrollerar du din version:

```powershell
Get-Module -ListAvailable -Name AzureRm.Resources | Select Version
```

Om resultatet visar version 3.0 eller senare, hello exemplen i det här avsnittet att arbeta med din miljö. Om du inte har version 3.0 eller senare måste du [uppdatera versionen](/powershell/azureps-cmdlets-docs/) med hjälp av PowerShell-galleriet eller med Installationsprogram för webbplattform innan du fortsätter med det här avsnittet.

```powershell
Version
-------
3.5.0
```

toosee hello befintliga taggar för en *resursgruppen*, Använd:

```powershell
(Get-AzureRmResourceGroup -Name examplegroup).Tags
```

Skriptet returnerar hello följande format:

```powershell
Name                           Value
----                           -----
Dept                           IT
Environment                    Test
```

toosee hello befintliga taggar för en *resurs som har en viss resurs-ID*, Använd:

```powershell
(Get-AzureRmResource -ResourceId {resource-id}).Tags
```

Eller toosee hello befintliga taggar för en *resurs som har en viss grupp av namn och resursen*, Använd:

```powershell
(Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup).Tags
```

tooget *resursgrupper som har en specifik tagg*, Använd:

```powershell
(Find-AzureRmResourceGroup -Tag @{ Dept="Finance" }).Name 
```

tooget *resurser som har en specifik tagg*, Använd:

```powershell
(Find-AzureRmResource -TagName Dept -TagValue Finance).Name
```

Varje gång du tillämpar taggar tooa resurs eller en resursgrupp kan du skriva över hello befintliga taggar på denna resurs eller resursgrupp. Därför måste du använda en annan metod baserat på om hello resurs eller resursgrupp har befintliga taggar. 

tooadd taggar tooa *resursgruppen utan befintliga taggar*, Använd:

```powershell
Set-AzureRmResourceGroup -Name examplegroup -Tag @{ Dept="IT"; Environment="Test" }
```

tooadd taggar tooa *resursgrupp som har befintliga taggar*, hämta hello befintliga taggar, lägga till nya hello-tagg och tillämpa hello taggar:

```powershell
$tags = (Get-AzureRmResourceGroup -Name examplegroup).Tags
$tags += @{Status="Approved"}
Set-AzureRmResourceGroup -Tag $tags -Name examplegroup
```

tooadd taggar tooa *resursen utan befintliga taggar*, Använd:

```powershell
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test" } -ResourceName examplevnet -ResourceGroupName examplegroup
```

tooadd taggar tooa *resurs som har befintliga taggar*, Använd:

```powershell
$tags = (Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup).Tags
$tags += @{Status="Approved"}
Set-AzureRmResource -Tag $tags -ResourceName examplevnet -ResourceGroupName examplegroup
```

tooapply alla taggar från resursen grupp tooits resurser och *inte behålla befintliga taggar på hello resurser*, använda hello följande skript:

```powershell
$groups = Get-AzureRmResourceGroup
foreach ($g in $groups) 
{
    Find-AzureRmResource -ResourceGroupNameEquals $g.ResourceGroupName | ForEach-Object {Set-AzureRmResource -ResourceId $_.ResourceId -Tag $g.Tags -Force } 
}
```

tooapply alla taggar från resursen grupp tooits resurser och *behåller befintliga taggar för resurser som inte är dubbletter*, använda hello följande skript:

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

tooremove alla taggar skicka en tom hash-tabellen:

```powershell
Set-AzureRmResourceGroup -Tag @{} -Name examplegroup
```



