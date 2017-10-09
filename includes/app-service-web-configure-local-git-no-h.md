Konfigurera lokal Git-distribution toohello webbapp med hello [az webapp distribution källa config-lokal-git](/cli/azure/webapp/deployment/source#config-local-git) kommando.

Apptjänst har stöd för flera olika sätt toodeploy innehåll tooa webbprogram, till exempel FTP, lokal Git, GitHub, Visual Studio Team Services och Bitbucket. I den här snabbstarten ska vi distribuera med hjälp av lokala Git. Det innebär att du distribuerar med en Git-kommandot toopush från en lokal databas tooa databas i Azure. 

Följande kommando, Ersätt i hello  *\<appnamn >* med namnet på ditt webbprogram.

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group myResourceGroup --query url --output tsv
```

hello utdata har hello följande format:

```bash
https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git
```

Hej `<username>` är hello [distribution användaren](#configure-a-deployment-user) som du skapade i föregående steg.

Kopiera hello URI visas; du ska använda i hello nästa steg.
