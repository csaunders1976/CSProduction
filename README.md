////// Bootstrap ///////////////////

*** Headings ***

<h1>h1. Bootstrap heading <small>Secondary text</small></h1>
<h2>h2. Bootstrap heading <small>Secondary text</small></h2>
<h3>h3. Bootstrap heading <small>Secondary text</small></h3>
<h4>h4. Bootstrap heading <small>Secondary text</small></h4>
<h5>h5. Bootstrap heading <small>Secondary text</small></h5>
<h6>h6. Bootstrap heading <small>Secondary text</small></h6>

*** Jumbotron ***

<div class="jumbotron">
  <h1>Hello, world!</h1>
  <p>...</p>
  <p><a class="btn btn-primary btn-lg" href="#" role="button">Learn more</a></p>
</div>

*** Columns ***
// Must be in container
<div class="row">
        <div class="col-md-2">.col-md-1</div>
        <div class="col-md-2">.col-md-1</div>
        <div class="col-md-2">.col-md-1</div>
        <div class="col-md-2">.col-md-1</div>
        <div class="col-md-2">.col-md-1</div>
        <div class="col-md-2">.col-md-1</div>
    </div>

*** Images ***

<img src="..." class="img-responsive" alt="Responsive image">

<img src="..." alt="..." class="img-rounded">
<img src="..." alt="..." class="img-circle">
<img src="..." alt="..." class="img-thumbnail">

*** Thumbnails ***

<div class="row">
  <div class="col-xs-6 col-md-3">
    <a href="#" class="thumbnail">
      <img src="..." alt="...">
    </a>
  </div>
  ...
</div>

////// Spin Up Information //////////

Production IP: 104.131.189.62
Staging IP: 107.170.104.232

Droplet Name: CS
512 MB Memory / 20 GB Disk / NYC2 - Ubuntu 12.04.5 x64

New Server Procedure

    Create Server
        Configured to the Development Guidelines specifications for this class
    SSH Into the Server
        ssh root@104.131.189.62
        Enter password
    Create Non-Root User
        adduser csaunders11
        adduser csaunders11 sudo
    End SSH Session
        exit
    Login as Non-Root User
        ssh caunders11@S104.131.189.62
        Enter password
    Update Package System
        sudo apt-get update
        Enter password
    Upgrade Package System
        sudo apt-get upgrade
    Update Packages for Newly Installed Version
        sudo apt-get update
    Update System level Packages
        sudo aptitude update
        sudo aptitude safe-upgrade
        sudo reboot
    Install Packages
        Git
            See Git: Install & Config
        Apache
            See Apache Install & Config

Git: Install & Config

    Install Git
        sudo apt-get install git-core
    Configure Git
        git config --global user.name csaunders1976
        git config --global user.email csaunders11@fullsail.edu
    Confirm Settings
        git config --list
    Create SSH Keys for Github Access
        ssh-keygen -t rsa -C csaunders11@fullsail.edu
            This will ask if you want to customize the name, you don’t. Just press enter.
        Enter a passphrase
        Re-enter the passphrase
    Put the RSA key on file with github.com so this server is treated as a trusted machine.
        less ~/.ssh/id_rsa.pub
            Copy ALL of the contents of this file to your clipboard.
        Add new SSH Key to your Github account under the Account Settings.
            Click the Add Key button
            Enter a title defining the server
            Paste the RSA file’s contents into text area marked “Key”
            Click Add Key


Apache Install & Config

    Install Apache 2
        sudo apt-get install apache2
    Configure ServerName
        Restart Server
            sudo service apache2 restart
        Failed to Restart
            apache2: Could not reliably determine the server's fully qualified domain name, using 127.0.0.1 for ServerName
        Configure ServerName
            sudo pico /etc/apache2/conf.d/security
                Add
                    ServerName localhost
            sudo service apache2 restart
            Apache should report a successful restart
    Restrict Access
        sudo pico /etc/apache2/conf.d/security
            Uncomment <Directory />
            Add
                Options FollowSymLinks
            sudo service apache2 restart
    Change Permissions to Allow Access
                sudo chown -R csaunders11 /var/www




 ////// BRANCH DEVELOPMENT ///////////

 Get the list of Current Branches

~ git branch

Create and Checkout a New Branch

~ git checkout master
~ git branch newFeatureName
~ git checkout newFeatureName

Merge Branch into Master

~ git checkout master
~ git pull origin master
~ git merge newFeatureName
~ git push origin master


Naming Conventions

When possible, use a camelCased naming conventions with an emphasis on description over abbreviation. For example, when naming a variable to represent a Category use the full word Category instead of the abbreviated cat as there is cause for confusion when another developer reviews, tests, modifies the code.

// Correct
var animalCategories = [‘large’, ‘small’];

// Avoid
var animalCats = [‘large’, ‘small’];


Versions Numbering

When releasing new versions the pattern to follow is Release Version (dot) Production Release ++ (dot) Staged Release ++. Each time a new feature is promoted to the staging server increment the Staged Release value by one. Each time a Production promotion occurs increment the Production Release Value by one. Additionally on Production Promotions reset the Staged Release value to zero.

Example:

    Staged Release: v0.1.13 turns into v0.1.14
    Production Release: v0.1.14 turns into v0.2.0

 /////// Configure Git //////////

 Configure Git Deployment

     Create Space for Repo on Live Server
         sudo mkdir /var/repos/
         sudo chown csaunders11 /var/repos
             Changing ownership of the Repos folder to your user instead of the Root user allows for your further manipulation of it’s content without superuser permissions.
         cd /var/repos && mkdir CSDevelopment.git && cd CSDevelopment.git
         git init --bare
             A bare git init means this folder will have no source files, only the version control structure of git.
     Configure Server Post Hook
         cd /var/repos/CSDevelopment.git/hooks/
         pico post-receive
             Save the file with the contents below. It separates the working tree and git dir into separate locations.

                 #!/bin/sh
                 GIT_WORK_TREE=/var/www git checkout -f


         chmod +x post-receive
             Give permissions to make the hook executable by the server.
     Configure Local Dev Environment
         It’s important to have a centralized space for your Projects and Deployments even if you’re not deploying multiple sites on this server.
         Open a new Terminal Window
         mkdir ~/Projects
         mkdir ~/Projects/SiteName.com && cd ~/Projects/SiteName.com
             Even if you did not purchase a domain name use a .com or .net etc of what you would like to use. In linux adding a .com to a folder name does not complicate things as it would on your laptop.
         git init
         git remote add prodServer ssh://YourUserName@IPAddress/var/repos/SiteName.git
     Further Reading
         http://toroid.org/ams/git-website-howto


