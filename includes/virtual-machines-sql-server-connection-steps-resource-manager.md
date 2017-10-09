### <a name="configure-a-dns-label-for-hello-public-ip-address"></a><span data-ttu-id="2bbb9-101">Konfigurera en DNS-etikett för hello offentlig IP-adress</span><span class="sxs-lookup"><span data-stu-id="2bbb9-101">Configure a DNS Label for hello public IP address</span></span>

<span data-ttu-id="2bbb9-102">tooconnect toohello SQL Server Database Engine från hello Internet, kan du skapa en DNS-etikett för din offentliga IP-adress.</span><span class="sxs-lookup"><span data-stu-id="2bbb9-102">tooconnect toohello SQL Server Database Engine from hello Internet, consider creating a DNS Label for your public IP address.</span></span> <span data-ttu-id="2bbb9-103">Du kan ansluta med IP-adress, men hello DNS-etikett skapar en A-post som är enklare tooidentify och sammanfattningar hello underliggande offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="2bbb9-103">You can connect by IP address, but hello DNS Label creates an A Record that is easier tooidentify and abstracts hello underlying public IP address.</span></span>

> [!NOTE]
> <span data-ttu-id="2bbb9-104">DNS-etiketter krävs inte om planen tooonly ansluta toohello SQL Server-instansen inom hello samma virtuella nätverk eller bara lokalt.</span><span class="sxs-lookup"><span data-stu-id="2bbb9-104">DNS Labels are not required if you plan tooonly connect toohello SQL Server instance within hello same Virtual Network or only locally.</span></span>

<span data-ttu-id="2bbb9-105">toocreate en DNS-etikett först välja **virtuella datorer** hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="2bbb9-105">toocreate a DNS Label, first select **Virtual machines** in hello portal.</span></span> <span data-ttu-id="2bbb9-106">Välj din SQL Server-VM toobring fram egenskaperna.</span><span class="sxs-lookup"><span data-stu-id="2bbb9-106">Select your SQL Server VM toobring up its properties.</span></span>

1. <span data-ttu-id="2bbb9-107">I hello-översikt över virtuella datorer, Välj din **offentliga IP-adressen**.</span><span class="sxs-lookup"><span data-stu-id="2bbb9-107">In hello virtual machine overview, select your **Public IP address**.</span></span>

    ![offentlig IP-adress](./media/virtual-machines-sql-server-connection-steps/rm-public-ip-address.png)

1. <span data-ttu-id="2bbb9-109">Expandera i hello egenskaper för din offentliga IP-adress, **Configuration**.</span><span class="sxs-lookup"><span data-stu-id="2bbb9-109">In hello properties for your Public IP address, expand **Configuration**.</span></span>

1. <span data-ttu-id="2bbb9-110">Ange ett namn för DNS-etiketten.</span><span class="sxs-lookup"><span data-stu-id="2bbb9-110">Enter a DNS Label name.</span></span> <span data-ttu-id="2bbb9-111">Det här är en A-post som kan använda tooconnect tooyour SQL Server-VM via namn istället för IP-adressen direkt.</span><span class="sxs-lookup"><span data-stu-id="2bbb9-111">This name is an A Record that can be used tooconnect tooyour SQL Server VM by name instead of by IP Address directly.</span></span>

1. <span data-ttu-id="2bbb9-112">Klicka på hello **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="2bbb9-112">Click hello **Save** button.</span></span>

    ![dns-etikett](./media/virtual-machines-sql-server-connection-steps/rm-dns-label.png)

### <a name="connect-toohello-database-engine-from-another-computer"></a><span data-ttu-id="2bbb9-114">Ansluta toohello databasmotorn från en annan dator</span><span class="sxs-lookup"><span data-stu-id="2bbb9-114">Connect toohello Database Engine from another computer</span></span>

1. <span data-ttu-id="2bbb9-115">På en dator ansluten toohello internet, Öppna SQL Server Management Studio (SSMS).</span><span class="sxs-lookup"><span data-stu-id="2bbb9-115">On a computer connected toohello internet, open SQL Server Management Studio (SSMS).</span></span> <span data-ttu-id="2bbb9-116">Om du inte har SQL Server Management Studio kan du ladda ned den [här](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).</span><span class="sxs-lookup"><span data-stu-id="2bbb9-116">If you do not have SQL Server Management Studio, you can download it [here](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).</span></span>

1. <span data-ttu-id="2bbb9-117">I hello **ansluta tooServer** eller **ansluta tooDatabase motorn** dialogrutan, redigera hello **servernamn** värde.</span><span class="sxs-lookup"><span data-stu-id="2bbb9-117">In hello **Connect tooServer** or **Connect tooDatabase Engine** dialog box, edit hello **Server name** value.</span></span> <span data-ttu-id="2bbb9-118">Ange hello IP-adress eller fullständigt DNS-namn för hello virtuella datorn (bestäms i hello föregående åtgärd).</span><span class="sxs-lookup"><span data-stu-id="2bbb9-118">Enter hello IP address or full DNS name of hello virtual machine (determined in hello previous task).</span></span> <span data-ttu-id="2bbb9-119">Du kan också lägga till ett kommatecken och ange TCP-porten för SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2bbb9-119">You can also add a comma and provide SQL Server's TCP port.</span></span> <span data-ttu-id="2bbb9-120">Till exempel `mysqlvmlabel.eastus.cloudapp.azure.com,1433`.</span><span class="sxs-lookup"><span data-stu-id="2bbb9-120">For example, `mysqlvmlabel.eastus.cloudapp.azure.com,1433`.</span></span>

1. <span data-ttu-id="2bbb9-121">I hello **autentisering** väljer **SQL Server-autentisering**.</span><span class="sxs-lookup"><span data-stu-id="2bbb9-121">In hello **Authentication** box, select **SQL Server Authentication**.</span></span>

1. <span data-ttu-id="2bbb9-122">I hello **inloggning** rutan, ange hello namnet på en giltig SQL-inloggning.</span><span class="sxs-lookup"><span data-stu-id="2bbb9-122">In hello **Login** box, type hello name of a valid SQL login.</span></span>

1. <span data-ttu-id="2bbb9-123">I hello **lösenord** rutan, typen hello lösenordet till hello-inloggning.</span><span class="sxs-lookup"><span data-stu-id="2bbb9-123">In hello **Password** box, type hello password of hello login.</span></span>

1. <span data-ttu-id="2bbb9-124">Klicka på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="2bbb9-124">Click **Connect**.</span></span>

    ![ssms anslut](./media/virtual-machines-sql-server-connection-steps/rm-ssms-connect.png)