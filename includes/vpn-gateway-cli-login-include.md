Logga in tooyour Azure-prenumeration med hello [az inloggningen](/cli/azure/#login) kommando och följ hello på skärmen riktningar. Mer information om att logga in finns i [Kom igång med Azure CLI 2.0](/cli/azure/get-started-with-azure-cli).

```azurecli
az login
```

Om du har mer än en Azure-prenumeration, lista hello prenumerationer för hello-kontot.

```azurecli
az account list --all
```

Ange hello prenumeration som du vill toouse.

```azurecli
az account set --subscription <replace_with_your_subscription_id>
```