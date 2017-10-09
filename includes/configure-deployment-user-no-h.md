<span data-ttu-id="a6aec-101">Skapa autentiseringsuppgifter för distribution med hello [az webapp distributionsanvändare har angetts](/cli/azure/webapp/deployment/user#set) kommando.</span><span class="sxs-lookup"><span data-stu-id="a6aec-101">Create deployment credentials with hello [az webapp deployment user set](/cli/azure/webapp/deployment/user#set) command.</span></span>

<span data-ttu-id="a6aec-102">En distribution av användare krävs för FTP- och lokal Git-distribution tooa webbapp.</span><span class="sxs-lookup"><span data-stu-id="a6aec-102">A deployment user is required for FTP and local Git deployment tooa web app.</span></span> <span data-ttu-id="a6aec-103">är kontonivå hello användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="a6aec-103">hello user name and password are account level.</span></span> <span data-ttu-id="a6aec-104">_De skiljer sig från autentiseringsuppgifterna för din Azure-prenumeration._</span><span class="sxs-lookup"><span data-stu-id="a6aec-104">_They are different from your Azure subscription credentials._</span></span>

<span data-ttu-id="a6aec-105">Följande kommando, Ersätt i hello  *\<användarnamn >* och  *\<lösenord >* med ett nytt användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="a6aec-105">In hello following command, replace *\<user-name>* and *\<password>* with a new user name and password.</span></span> <span data-ttu-id="a6aec-106">hello användarnamnet måste vara unika.</span><span class="sxs-lookup"><span data-stu-id="a6aec-106">hello user name must be unique.</span></span> <span data-ttu-id="a6aec-107">hello lösenord måste innehålla minst åtta tecken, med två av följande tre element hello: bokstäver, siffror, symboler.</span><span class="sxs-lookup"><span data-stu-id="a6aec-107">hello password must be at least eight characters long, with two of hello following three elements: letters, numbers, symbols.</span></span> 

```azurecli-interactive
az webapp deployment user set --user-name <username> --password <password>
```

<span data-ttu-id="a6aec-108">Om du får en ` 'Conflict'. Details: 409` fel, ändra hello användarnamn.</span><span class="sxs-lookup"><span data-stu-id="a6aec-108">If you get a ` 'Conflict'. Details: 409` error, change hello username.</span></span> <span data-ttu-id="a6aec-109">Om du ser felet ` 'Bad Request'. Details: 400` ska du använda ett starkare lösenord.</span><span class="sxs-lookup"><span data-stu-id="a6aec-109">If you get a ` 'Bad Request'. Details: 400` error, use a stronger password.</span></span>

<span data-ttu-id="a6aec-110">Du behöver bara skapa distributionsanvändaren en gång. Du kan använda den för alla Azure-distributioner.</span><span class="sxs-lookup"><span data-stu-id="a6aec-110">You create this deployment user only once; you can use it for all your Azure deployments.</span></span>

> [!NOTE]
> <span data-ttu-id="a6aec-111">Posten hello användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="a6aec-111">Record hello user name and password.</span></span> <span data-ttu-id="a6aec-112">Du behöver dem. toodeploy hello webbprogrammet senare.</span><span class="sxs-lookup"><span data-stu-id="a6aec-112">You need them toodeploy hello web app later.</span></span>
>
>