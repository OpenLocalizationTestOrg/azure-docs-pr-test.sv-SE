<!--author=SharS last changed: 12/01/15-->

#### <a name="tooenter-maintenance-mode"></a><span data-ttu-id="55c20-101">tooenter underhållsläge</span><span class="sxs-lookup"><span data-stu-id="55c20-101">tooenter Maintenance mode</span></span>
1. <span data-ttu-id="55c20-102">I menyn för seriekonsolen av hello väljer du alternativ 1, **logga in med fullständig åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="55c20-102">In hello serial console menu, choose option 1, **Log in with full access**.</span></span>
2. <span data-ttu-id="55c20-103">Ange hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="55c20-103">Type hello password.</span></span> <span data-ttu-id="55c20-104">hello standardlösenordet är **Password1**.</span><span class="sxs-lookup"><span data-stu-id="55c20-104">hello default password is **Password1**.</span></span>
3. <span data-ttu-id="55c20-105">I hello kommandotolk, skriver du:</span><span class="sxs-lookup"><span data-stu-id="55c20-105">At hello command prompt, type</span></span>
   
     `Enter-HcsMaintenanceMode`
4. <span data-ttu-id="55c20-106">Visas ett varningsmeddelande som talar om att underhållsläge ska störa alla i/o-begäranden och sever hello anslutning toohello klassiska Azure-portalen och du uppmanas att bekräfta.</span><span class="sxs-lookup"><span data-stu-id="55c20-106">You will see a warning message telling you that Maintenance mode will disrupt all I/O requests and sever hello connection toohello Azure classic portal, and you will be prompted for confirmation.</span></span> <span data-ttu-id="55c20-107">Typen **Y** tooenter underhållsläge.</span><span class="sxs-lookup"><span data-stu-id="55c20-107">Type **Y** tooenter Maintenance mode.</span></span>
   
    <span data-ttu-id="55c20-108">Både domänkontrollanter startas om.</span><span class="sxs-lookup"><span data-stu-id="55c20-108">Both controllers will restart.</span></span> <span data-ttu-id="55c20-109">När hello omstarten är klar, visas ett annat meddelande som anger att hello-enheten är i underhållsläge.</span><span class="sxs-lookup"><span data-stu-id="55c20-109">When hello restart is complete, another message will appear indicating that hello device is in Maintenance mode.</span></span>

