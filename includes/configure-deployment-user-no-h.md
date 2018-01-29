I Cloud Shell skapar du autentiseringsuppgifter för distribution med kommandot [az webapp deployment user set](/cli/azure/webapp/deployment/user?view=azure-cli-latest#az_webapp_deployment_user_set). En distributionsanvändare krävs för en FTP-distribution och en lokal git-distribution till en webbapp. Användarnamn och lösenord är på kontonivå. _De skiljer sig från autentiseringsuppgifterna för din Azure-prenumeration._

I följande exempel ersätter  *\<användarnamn >* och  *\<lösenord >* (inklusive hakparenteser) med ett nytt användarnamn och lösenord. Användarnamnet måste vara unikt. Lösenordet måste innehålla minst åtta tecken, med två av följande tre element: bokstäver, siffror, symboler. 

```azurecli-interactive
az webapp deployment user set --user-name <username> --password <password>
```

Om du ser felet ` 'Conflict'. Details: 409` ska du byta användarnamn. Om du ser felet ` 'Bad Request'. Details: 400` ska du använda ett starkare lösenord.

Du behöver bara skapa distributionsanvändaren en gång. Du kan använda den för alla Azure-distributioner.

> [!NOTE]
> Anteckna användarnamn och lösenord. Du kommer att behöva dem för att distribuera webbappen.
>
>
