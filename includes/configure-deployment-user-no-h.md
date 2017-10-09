Skapa autentiseringsuppgifter för distribution med hello [az webapp distributionsanvändare har angetts](/cli/azure/webapp/deployment/user#set) kommando.

En distribution av användare krävs för FTP- och lokal Git-distribution tooa webbapp. är kontonivå hello användarnamn och lösenord. _De skiljer sig från autentiseringsuppgifterna för din Azure-prenumeration._

Följande kommando, Ersätt i hello  *\<användarnamn >* och  *\<lösenord >* med ett nytt användarnamn och lösenord. hello användarnamnet måste vara unika. hello lösenord måste innehålla minst åtta tecken, med två av följande tre element hello: bokstäver, siffror, symboler. 

```azurecli-interactive
az webapp deployment user set --user-name <username> --password <password>
```

Om du får en ` 'Conflict'. Details: 409` fel, ändra hello användarnamn. Om du ser felet ` 'Bad Request'. Details: 400` ska du använda ett starkare lösenord.

Du behöver bara skapa distributionsanvändaren en gång. Du kan använda den för alla Azure-distributioner.

> [!NOTE]
> Posten hello användarnamn och lösenord. Du behöver dem. toodeploy hello webbprogrammet senare.
>
>