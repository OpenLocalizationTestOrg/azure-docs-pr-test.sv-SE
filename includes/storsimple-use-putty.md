<!--author=SharS last changed: 9/17/15-->

#### <a name="tooconnect-through-hello-serial-console"></a><span data-ttu-id="3bcdb-101">tooconnect via hello seriekonsol</span><span class="sxs-lookup"><span data-stu-id="3bcdb-101">tooconnect through hello serial console</span></span>
1. <span data-ttu-id="3bcdb-102">Anslut den seriella kabel toohello enheten (direkt eller via en USB-Serieadapter).</span><span class="sxs-lookup"><span data-stu-id="3bcdb-102">Connect your serial cable toohello device (directly or through a USB-serial adapter).</span></span>
2. <span data-ttu-id="3bcdb-103">Öppna hello **Kontrollpanelen**, och sedan öppna hello **Enhetshanteraren**.</span><span class="sxs-lookup"><span data-stu-id="3bcdb-103">Open hello **Control Panel**, and then open hello **Device Manager**.</span></span>
3. <span data-ttu-id="3bcdb-104">Identifiera hello COM-port som visas i följande illustration hello.</span><span class="sxs-lookup"><span data-stu-id="3bcdb-104">Identify hello COM port as shown in hello following illustration.</span></span>
   
     ![Anslut via seriekonsol](./media/storsimple-use-putty/HCS_ConnectingDeviceS-include.png)
4. <span data-ttu-id="3bcdb-106">Starta PuTTY.</span><span class="sxs-lookup"><span data-stu-id="3bcdb-106">Start PuTTY.</span></span> 
5. <span data-ttu-id="3bcdb-107">I högra fönstret hello ändra hello **anslutningstypen** för**seriella**.</span><span class="sxs-lookup"><span data-stu-id="3bcdb-107">In hello right pane, change hello **Connection type** too**Serial**.</span></span>
6. <span data-ttu-id="3bcdb-108">Ange hello lämplig COM-port i hello högra rutan.</span><span class="sxs-lookup"><span data-stu-id="3bcdb-108">In hello right pane, type hello appropriate COM port.</span></span> <span data-ttu-id="3bcdb-109">Kontrollera att hello seriekonfigurationen är inställda på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="3bcdb-109">Make sure that hello serial configuration parameters are set as follows:</span></span>
   
   * <span data-ttu-id="3bcdb-110">Hastighet: 115 200</span><span class="sxs-lookup"><span data-stu-id="3bcdb-110">Speed: 115,200</span></span>
   * <span data-ttu-id="3bcdb-111">Databitar: 8</span><span class="sxs-lookup"><span data-stu-id="3bcdb-111">Data bits: 8</span></span>
   * <span data-ttu-id="3bcdb-112">Stoppbitar: 1</span><span class="sxs-lookup"><span data-stu-id="3bcdb-112">Stop bits: 1</span></span>
   * <span data-ttu-id="3bcdb-113">Paritet: ingen</span><span class="sxs-lookup"><span data-stu-id="3bcdb-113">Parity: None</span></span>
   * <span data-ttu-id="3bcdb-114">Flödeskontroll: ingen</span><span class="sxs-lookup"><span data-stu-id="3bcdb-114">Flow control: None</span></span>
     
     <span data-ttu-id="3bcdb-115">De här inställningarna visas i följande illustration hello.</span><span class="sxs-lookup"><span data-stu-id="3bcdb-115">These settings are shown in hello following illustration.</span></span>
     
     ![PuTTY-inställningar](./media/storsimple-use-putty/HCS_PuttyConfig-include.png) 
     
     > [!NOTE]
     > <span data-ttu-id="3bcdb-117">Om hello standardinställningen för flödeskontroll inte fungerar, försök med hello flödet kontrollen tooXON/XOFF.</span><span class="sxs-lookup"><span data-stu-id="3bcdb-117">If hello default flow control setting does not work, try setting hello flow control tooXON/XOFF.</span></span>
     > 
     > 
7. <span data-ttu-id="3bcdb-118">Klicka på **öppna** toostart en seriesession.</span><span class="sxs-lookup"><span data-stu-id="3bcdb-118">Click **Open** toostart a serial session.</span></span>

