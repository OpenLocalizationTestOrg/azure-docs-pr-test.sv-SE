<!--author=SharS last changed: 9/17/15-->

#### <a name="to-connect-through-the-serial-console"></a><span data-ttu-id="74384-101">Anslut via seriekonsolen</span><span class="sxs-lookup"><span data-stu-id="74384-101">To connect through the serial console</span></span>
1. <span data-ttu-id="74384-102">Ansluta din seriekabel till enheten (direkt eller via en USB-serieadapter).</span><span class="sxs-lookup"><span data-stu-id="74384-102">Connect your serial cable to the device (directly or through a USB-serial adapter).</span></span>
2. <span data-ttu-id="74384-103">Öppna **Kontrollpanelen** och öppna sedan **Enhetshanteraren**.</span><span class="sxs-lookup"><span data-stu-id="74384-103">Open the **Control Panel**, and then open the **Device Manager**.</span></span>
3. <span data-ttu-id="74384-104">Identifiera COM-portarna som det visas i följande bild.</span><span class="sxs-lookup"><span data-stu-id="74384-104">Identify the COM port as shown in the following illustration.</span></span>
   
     ![Anslut via seriekonsol](./media/storsimple-use-putty/HCS_ConnectingDeviceS-include.png)
4. <span data-ttu-id="74384-106">Starta PuTTY.</span><span class="sxs-lookup"><span data-stu-id="74384-106">Start PuTTY.</span></span> 
5. <span data-ttu-id="74384-107">I den högra rutan, ändrar du **Anslutningstyp** till **Seriell**.</span><span class="sxs-lookup"><span data-stu-id="74384-107">In the right pane, change the **Connection type** to **Serial**.</span></span>
6. <span data-ttu-id="74384-108">I den högra rutan, skriver du in lämplig COM-port.</span><span class="sxs-lookup"><span data-stu-id="74384-108">In the right pane, type the appropriate COM port.</span></span> <span data-ttu-id="74384-109">Kontrollera att de parametrarna för seriekonfigurationen är inställda på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="74384-109">Make sure that the serial configuration parameters are set as follows:</span></span>
   
   * <span data-ttu-id="74384-110">Hastighet: 115 200</span><span class="sxs-lookup"><span data-stu-id="74384-110">Speed: 115,200</span></span>
   * <span data-ttu-id="74384-111">Databitar: 8</span><span class="sxs-lookup"><span data-stu-id="74384-111">Data bits: 8</span></span>
   * <span data-ttu-id="74384-112">Stoppbitar: 1</span><span class="sxs-lookup"><span data-stu-id="74384-112">Stop bits: 1</span></span>
   * <span data-ttu-id="74384-113">Paritet: ingen</span><span class="sxs-lookup"><span data-stu-id="74384-113">Parity: None</span></span>
   * <span data-ttu-id="74384-114">Flödeskontroll: ingen</span><span class="sxs-lookup"><span data-stu-id="74384-114">Flow control: None</span></span>
     
     <span data-ttu-id="74384-115">Inställningarna visas i följande bild.</span><span class="sxs-lookup"><span data-stu-id="74384-115">These settings are shown in the following illustration.</span></span>
     
     ![PuTTY-inställningar](./media/storsimple-use-putty/HCS_PuttyConfig-include.png) 
     
     > [!NOTE]
     > <span data-ttu-id="74384-117">Om standardinställningen för flödeskontroll inte fungerar, testa att ställa in flödeskontrollen på XON/XOFF.</span><span class="sxs-lookup"><span data-stu-id="74384-117">If the default flow control setting does not work, try setting the flow control to XON/XOFF.</span></span>
     > 
     > 
7. <span data-ttu-id="74384-118">Klicka på **Öppna**, för att starta en seriesession.</span><span class="sxs-lookup"><span data-stu-id="74384-118">Click **Open** to start a serial session.</span></span>

