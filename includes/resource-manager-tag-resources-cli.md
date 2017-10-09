tooadd en tagg tooa resursgrupp, Använd **azure har ställts in**. Om hello resursgruppen inte har några befintliga taggar kan överföra i hello-taggen.

```azurecli
azure group set -n tag-demo-group -t Dept=Finance
```

Taggar uppdateras som helhet. Om du vill tooadd en tagg tooa resursgrupp som har befintliga taggar måste klara alla hello-taggar. 

```azurecli
azure group set -n tag-demo-group -t Dept=Finance;Environment=Production;Project=Upgrade
```

Taggar ärvs inte av resurser i en resursgrupp. tooadd en tagg tooa resurs, Använd **azure resursuppsättningen**. Skicka hello API-versionsnumret för hello resurstyp som du lägger till hello tagg. Om du behöver tooretrieve hello API-version, Använd hello följande kommando med hello resource provider för hello-typ som du anger:

```azurecli
azure provider show -n Microsoft.Storage --json
```

Leta efter hello resurstyp som du vill ha i hello resultat.

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

Nu kan ange den API-versionen, resursgruppens namn, resurs, resurstyp och Taggvärde som parametrar.

```azurecli
azure resource set -g tag-demo-group -n storagetagdemo -r Microsoft.Storage/storageAccounts -t Dept=Finance -o 2016-01-01
```

Taggar finns direkt på resurser och resursgrupper. toosee hello befintliga taggar, hämta en resursgrupp och dess resurser med **azure-grupp visa**.

```azurecli
azure group show -n tag-demo-group --json
```

Som returnerar metadata om hello resursgrupp, inklusive eventuella märkningar tooit.

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

Du visar hello taggar för en viss resurs med hjälp av **azure-resurs visas**.

```azurecli
azure resource show -g tag-demo-group -n storagetagdemo -r Microsoft.Storage/storageAccounts -o 2016-01-01 --json
```

tooretrieve alla hello resurser med ett Taggvärde använder:

```azurecli
azure resource list -t Dept=Finance --json
```

tooretrieve Använd för alla resursgrupper för hello med ett Taggvärde:

```azurecli
azure group list -t Dept=Finance
```

Du kan visa befintliga hello-taggar i din prenumeration med hello följande kommando:

```azurecli
azure tag list
```
