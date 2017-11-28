## <a name="install-wordpress"></a><span data-ttu-id="7994a-101">Installera WordPress</span><span class="sxs-lookup"><span data-stu-id="7994a-101">Install WordPress</span></span>

<span data-ttu-id="7994a-102">Om du vill prova traven installerar du en exempelapp.</span><span class="sxs-lookup"><span data-stu-id="7994a-102">If you want to try your stack, install a sample app.</span></span> <span data-ttu-id="7994a-103">Exempelvis följande steg för att installera öppen källkod [WordPress](https://wordpress.org/) plattform för att skapa webbplatser och bloggar.</span><span class="sxs-lookup"><span data-stu-id="7994a-103">As an example, the following steps install the open source [WordPress](https://wordpress.org/) platform to create websites and blogs.</span></span> <span data-ttu-id="7994a-104">Andra arbetsbelastningar försöka ta [Drupal](http://www.drupal.org) och [Moodle](https://moodle.org/).</span><span class="sxs-lookup"><span data-stu-id="7994a-104">Other workloads to try include [Drupal](http://www.drupal.org) and [Moodle](https://moodle.org/).</span></span> 

<span data-ttu-id="7994a-105">Den här WordPress-installationen är för konceptbeviset.</span><span class="sxs-lookup"><span data-stu-id="7994a-105">This WordPress setup is for proof of concept.</span></span> <span data-ttu-id="7994a-106">Mer information och inställningar för produktion installation finns den [WordPress dokumentationen](https://codex.wordpress.org/Main_Page).</span><span class="sxs-lookup"><span data-stu-id="7994a-106">For more information and settings for production installation, see the [WordPress documentation](https://codex.wordpress.org/Main_Page).</span></span> 



### <a name="install-the-wordpress-package"></a><span data-ttu-id="7994a-107">Installera WordPress-paket</span><span class="sxs-lookup"><span data-stu-id="7994a-107">Install the WordPress package</span></span>

<span data-ttu-id="7994a-108">Kör följande kommando:</span><span class="sxs-lookup"><span data-stu-id="7994a-108">Run the following command:</span></span>

```bash
sudo apt install wordpress
```

### <a name="configure-wordpress"></a><span data-ttu-id="7994a-109">Konfigurera WordPress</span><span class="sxs-lookup"><span data-stu-id="7994a-109">Configure WordPress</span></span>

<span data-ttu-id="7994a-110">Konfigurera WordPress för att använda MySQL och PHP.</span><span class="sxs-lookup"><span data-stu-id="7994a-110">Configure WordPress to use MySQL and PHP.</span></span> <span data-ttu-id="7994a-111">Kör följande kommando för att öppna en textredigerare önskat och skapa filen `/etc/wordpress/config-localhost.php`:</span><span class="sxs-lookup"><span data-stu-id="7994a-111">Run the following command to open a text editor of your choice and create the file `/etc/wordpress/config-localhost.php`:</span></span>

```bash
sudo sensible-editor /etc/wordpress/config-localhost.php
```
<span data-ttu-id="7994a-112">Kopiera följande rader i filen, Ersätt ditt lösenord för *yourPassword* (lämna övriga värden oförändrade).</span><span class="sxs-lookup"><span data-stu-id="7994a-112">Copy the following lines to the file, substituting your database password for *yourPassword* (leave other values unchanged).</span></span> <span data-ttu-id="7994a-113">Spara sedan filen.</span><span class="sxs-lookup"><span data-stu-id="7994a-113">Then save the file.</span></span>

```php
<?php
define('DB_NAME', 'wordpress');
define('DB_USER', 'wordpress');
define('DB_PASSWORD', 'yourPassword');
define('DB_HOST', 'localhost');
define('WP_CONTENT_DIR', '/usr/share/wordpress/wp-content');
?>
```

<span data-ttu-id="7994a-114">Skapa en textfil i arbetskatalogen, `wordpress.sql` att konfigurera WordPress-databas:</span><span class="sxs-lookup"><span data-stu-id="7994a-114">In a working directory, create a text file `wordpress.sql` to configure the WordPress database:</span></span> 

```bash
sudo sensible-editor wordpress.sql
```

<span data-ttu-id="7994a-115">Lägg till följande kommandon och Ersätt ditt lösenord för *yourPassword* (lämna övriga värden oförändrade).</span><span class="sxs-lookup"><span data-stu-id="7994a-115">Add the following commands, substituting your database password for *yourPassword* (leave other values unchanged).</span></span> <span data-ttu-id="7994a-116">Spara sedan filen.</span><span class="sxs-lookup"><span data-stu-id="7994a-116">Then save the file.</span></span>

```sql
CREATE DATABASE wordpress;
GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP,ALTER
ON wordpress.*
TO wordpress@localhost
IDENTIFIED BY 'yourPassword';
FLUSH PRIVILEGES;
```


<span data-ttu-id="7994a-117">Kör följande kommando för att skapa databasen:</span><span class="sxs-lookup"><span data-stu-id="7994a-117">Run the following command to create the database:</span></span>

```bash
cat wordpress.sql | sudo mysql --defaults-extra-file=/etc/mysql/debian.cnf
```

<span data-ttu-id="7994a-118">När kommandot har slutförts kan du ta bort filen `wordpress.sql`.</span><span class="sxs-lookup"><span data-stu-id="7994a-118">After the command completes, delete the file `wordpress.sql`.</span></span>

<span data-ttu-id="7994a-119">Flytta WordPress-installationen till dokumentroten web server:</span><span class="sxs-lookup"><span data-stu-id="7994a-119">Move the WordPress installation to the web server document root:</span></span>

```bash
sudo ln -s /usr/share/wordpress /var/www/html/wordpress

sudo mv /etc/wordpress/config-localhost.php /etc/wordpress/config-default.php
```

<span data-ttu-id="7994a-120">Nu kan du slutför installationen av WordPress och publicera på plattformen.</span><span class="sxs-lookup"><span data-stu-id="7994a-120">Now you can complete the WordPress setup and publish on the platform.</span></span> <span data-ttu-id="7994a-121">Öppna en webbläsare och gå till `http://yourPublicIPAddress/wordpress`.</span><span class="sxs-lookup"><span data-stu-id="7994a-121">Open a browser and go to `http://yourPublicIPAddress/wordpress`.</span></span> <span data-ttu-id="7994a-122">Ersätt offentliga IP-adressen för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="7994a-122">Substitute the public IP address of your VM.</span></span> <span data-ttu-id="7994a-123">Det bör likna den här avbildningen.</span><span class="sxs-lookup"><span data-stu-id="7994a-123">It should look similar to this image.</span></span>

![Sidan för WordPress-installation](./media/virtual-machines-linux-tutorial-wordpress/wordpressstartpage.png)