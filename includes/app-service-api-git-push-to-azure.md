Använd hello Azure CLI tooget hello Fjärrdistribution URL för API-appen. Följande kommando, Ersätt i hello  *\<appnamn >* med namnet på ditt webbprogram.

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group myResourceGroup --query url --output tsv
```

Konfigurera din lokala Git-distribution toobe kan toopush toohello fjärråtkomst.

```bash
git remote add azure <URI from previous step>
```

Skicka toohello Azure remote toodeploy din app. Du ombeds hello lösenord som du skapade tidigare när du skapade hello distribution användare. Kontrollera att du anger hello lösenordet du skapade i tidigare i hello quickstart och inte hello lösenord du använder toolog i toohello Azure-portalen.

```bash
git push azure master
```
