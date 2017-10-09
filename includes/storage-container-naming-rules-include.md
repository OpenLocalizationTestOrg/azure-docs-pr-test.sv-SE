<span data-ttu-id="c7843-101">Varje blobb i Azure-lagring måste befinna sig i en behållare.</span><span class="sxs-lookup"><span data-stu-id="c7843-101">Every blob in Azure storage must reside in a container.</span></span> <span data-ttu-id="c7843-102">hello behållaren utgör en del av blobbnamnet hello.</span><span class="sxs-lookup"><span data-stu-id="c7843-102">hello container forms part of hello blob name.</span></span> <span data-ttu-id="c7843-103">Till exempel `mycontainer` är hello namnet på hello-behållaren i de här exempelblobb URI: er:</span><span class="sxs-lookup"><span data-stu-id="c7843-103">For example, `mycontainer` is hello name of hello container in these sample blob URIs:</span></span>

    https://storagesample.blob.core.windows.net/mycontainer/blob1.txt
    https://storagesample.blob.core.windows.net/mycontainer/photos/myphoto.jpg

<span data-ttu-id="c7843-104">Ett behållarnamn måste vara ett giltigt DNS-namn, förväntad toohello följande namngivningsregler:</span><span class="sxs-lookup"><span data-stu-id="c7843-104">A container name must be a valid DNS name, conforming toohello following naming rules:</span></span>

1. <span data-ttu-id="c7843-105">Behållarnamn måste inledas med en bokstav eller siffra och får bara innehålla bokstäver, siffror, och hello streck (-).</span><span class="sxs-lookup"><span data-stu-id="c7843-105">Container names must start with a letter or number, and can contain only letters, numbers, and hello dash (-) character.</span></span>
2. <span data-ttu-id="c7843-106">Varje bindestreck (-) måste föregås och följas av en bokstav eller siffra. Flera bindestreck i följd är inte tillåtna i behållarnamn.</span><span class="sxs-lookup"><span data-stu-id="c7843-106">Every dash (-) character must be immediately preceded and followed by a letter or number; consecutive dashes are not permitted in container names.</span></span>
3. <span data-ttu-id="c7843-107">Alla bokstäver i ett behållarnamn måste vara gemener.</span><span class="sxs-lookup"><span data-stu-id="c7843-107">All letters in a container name must be lowercase.</span></span>
4. <span data-ttu-id="c7843-108">Behållarnamn måste vara mellan 3 och 63 tecken långa.</span><span class="sxs-lookup"><span data-stu-id="c7843-108">Container names must be from 3 through 63 characters long.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c7843-109">Observera att hello namnet på en behållare måste alltid vara gemener.</span><span class="sxs-lookup"><span data-stu-id="c7843-109">Note that hello name of a container must always be lowercase.</span></span> <span data-ttu-id="c7843-110">Om du inkluderar en versal i ett behållarnamn, eller något annat sätt kränka hello behållaren namngivningsregler, kan det hända att ett 400-fel (felaktig begäran).</span><span class="sxs-lookup"><span data-stu-id="c7843-110">If you include an upper-case letter in a container name, or otherwise violate hello container naming rules, you may receive a 400 error (Bad Request).</span></span> 
> 
> 

