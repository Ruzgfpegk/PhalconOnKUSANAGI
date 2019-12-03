# Phalcon on KUSANAGI
Short tutorial to build the Phalcon PHP Framework on a KUSANAGI-powered VPS.

## English
KUSANAGI provides a CentOS repository for its PHP versions, which are more up-to-date than the base CentOS ones.

To compile Phalcon (or any other module) on it, you need to use KUSANAGI's installed version of PHP, which thankfully include the libraries.

If you're not well-versed in RHEL-based systems or custom PHP versions, here's a quick guide.

0. Make sure all dependencies are met and install them if needed:
    * `$ sudo yum install gcc libtool pcre-devel`
1. Get the source code tarball URL for the wished Phalcon [release](https://github.com/phalcon/cphalcon/releases), for instance [the 3.4.5 one](https://github.com/phalcon/cphalcon/releases/tag/v3.4.5).
2. On the server, download/uncompress the relevant file and go in its build folder:
    * `$ wget https://github.com/phalcon/cphalcon/archive/v3.4.5.tar.gz`
    * `$ tar xvzf v3.4.5.tar.gz`
    * `$ cd cphalcon-3.4.5/build/`
3. You now need to localize the phpize and php-config files of the kusanagi PHP package:
    * `$ repoquery --list kusanagi-php7 | grep -E '(php-config|phpize)$'`
        * [ ] `/usr/local/php7/bin/php-config`
        * [ ] `/usr/local/php7/bin/phpize`
4. Still in the build folder, pass the relevant parameters to the install script (adjust if needed):
    * `# ./install --phpize /usr/local/php7/bin/phpize --php-config /usr/local/php7/bin/php-config`
5. After some time everything should be compiled fine and the files put in their right location ,
   but you still need to adjust your PHP configuration, so let's create a config file for Phalcon and restart PHP-FPM:

    * `# echo 'extension=phalcon.so' > /etc/php7.d/extensions/phalcon.ini`
    * `# systemctl restart php7-fpm.service`

8. And it's done! You can always call phpinfo() to check.



## 日本語

(translation welcome, given that KUSANAGI is mostly used in Japan)
