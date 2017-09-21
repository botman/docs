## Installation for macOS

- [Server Requirements](#requirements)
- [Preparations](#preparations)
- [Installation Botman](#installation)
- [NGROK to the rescue](#ngrok)

This is an installation for botman specifically on MacOS X. 

<a id="requirements"></a>
## Server Requirements
  - Mac Webserver (in my case I used MAMP) => https://www.mamp.info/
  - Terminal 
  - Composer
  - NGROK => https://ngrok.com/
  - HomeBrew => https://brew.sh/
  - MacOS running PHP 7.1
  - Admin access

<a id="preparations"></a>
## Preparations
  - Install your webserver in my case MAMP<br />
      `http://downloads1.mamp.info/MAMP-PRO/releases/4.2/MAMP_MAMP_PRO_4.2.pkg`
  - Installing Composer<br />
      `curl -sS https://getcomposer.org/installer | php`<br />
      `sudo mv composer.phar /usr/local/bin/composer`<br />      
  - Please check your PHP version by typing `php -v`
  - if you already running the PHP version 7.1 then you do not require this preparations.
  - to upgrade PHP for Mac you are recommended to used HomeBrew
  - install HomeBrew<br />
      `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`
  - On Terminal run this to begin PHP upgrade<br />
      `brew tap homebrew/dupes`<br />
      `brew tap homebrew/versions`<br />
      `brew tap homebrew/homebrew-php`<br />
      `brew unlink php56` only if you previously installing php using brew<br />
      `brew install php71`<br />
  - ALRIGHT you are done

<a id="installation"></a>
## Installation botman
  - Install botman studio<br />
      `composer global require "botman/installer"`
  - Once completed you need to edit `.bash_profile`
  - Used nano to edit it `sudo nano .bash_profile`
  - Add this line `export PATH="$PATH:$HOME/.composer/vendor/bin"`
  - In most cases once you are done botman studio are ready to be used
  - Go to your htdocs directory or your webserver root directory (MAMP directory `/Applications/MAMP/htdocs`)<br />
      `cd /Applications/MAMP/htdocs`
  - Perform `botman new weatherman`
  - Give a moment while Botman Studio complete its installation
  - Now you will need to edit your hosts<br />
      `sudo nano /etc/hosts`
  - Add `127.0.0.1  local.botman` and control-x to save
  - Now you need to edit your webserver to make sure it understand that `local.botman` vhost back to `/Applications/MAMP/htdocs/weatherman/public` as the root directory
  - If you do it correctly botman should now run on your server.
  - Call your botman from browser by typing http://local.botman

<a id="ngrok"></a>
## NGROK to the rescue
  - Download NGROK and unzip
  - Move ngrok to `mv ngrok /usr/local/bin/`
  - Run `ngrok http -host-header=rewrite local.botman:80`
  - You are now have https tunneling to setup hook for botman
  - CHEERS
    
    

