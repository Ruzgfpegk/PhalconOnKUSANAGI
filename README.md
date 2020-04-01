# Phalcon on KUSANAGI
Short tutorial on how to build the Phalcon PHP Framework (v3/v4) on a [KUSANAGI](https://en.kusanagi.tokyo/document/)-powered VPS (Prime Strategy repository on top of CentOS).

## English
KUSANAGI provides a CentOS repository for its PHP versions, which are more up-to-date than the base CentOS ones.

To compile Phalcon (or any other module) on it, you need to use KUSANAGI's installed version of PHP,
which thankfully include the libraries.

If you're not well-versed in RHEL-based systems or custom PHP versions, here's a quick guide based on the general
instructions of each package.

0. If you're upgrading to Phalcon 4, before installing it you should rename the existing v3 library and adjust the ini file
   (adjust with the relevant filename) for an easy rollback if needed:
   * `# cd /usr/local/php7/lib/php/extensions/no-debug-non-zts-20180731/`
   * `# mv phalcon.so phalcon3.so`
   * `# perl -pi -e 's/phalcon\.so/phalcon3.so/' /etc/php7.d/extensions/50-phalcon.ini`
   * `# systemctl restart php7-fpm.service`
   * Two different versions of Phalcon cannot run in parallel, so you'll have to comment the Phalcon 3 line
     (add ; at the beginning of the line) before adding the Phalcon 4 one.
1. Make sure all dependencies are met and install them if needed:
    * `$ sudo yum install gcc libtool pcre-devel`
2. Get the source code tarball URL for the wished Phalcon [release](https://github.com/phalcon/cphalcon/releases),
   for instance [v3.4.5](https://github.com/phalcon/cphalcon/releases/tag/v3.4.5)
   or [v4.0.5](https://github.com/phalcon/cphalcon/releases/tag/v4.0.5).
3. On the server, download/uncompress the relevant Phalcon file and go in its build folder:
    * `$ wget https://github.com/phalcon/cphalcon/archive/v3.4.5.tar.gz`
    * `$ tar xvzf v3.4.5.tar.gz`
    * `$ cd cphalcon-3.4.5/build/`
4. You now need to locate the phpize and php-config files of the kusanagi PHP package:
    * `$ repoquery --list kusanagi-php7 | grep -E '(php-config|phpize)$'`
        * [ ] `/usr/local/php7/bin/php-config`
        * [ ] `/usr/local/php7/bin/phpize`
5. Still in the build folder, pass the relevant parameters to the install script (adjust if needed):
    * `# ./install --phpize /usr/local/php7/bin/phpize --php-config /usr/local/php7/bin/php-config`
6. Everything should then be compiled fine and the files put in their right location,
   but you still need to adjust your PHP configuration, so let's create a config file for Phalcon and restart PHP-FPM:
    * `# echo 'extension=phalcon.so' > /etc/php7.d/extensions/50-phalcon.ini`
    * `# systemctl restart php7-fpm.service`
7. If using Phalcon 4, you'll also have to download/compile/install the latest [PHP-PSR](https://github.com/jbboehr/php-psr),
   [v1.0.0](https://github.com/jbboehr/php-psr/releases/tag/v1.0.0) as of now, using a similar method.
    1. On the server, download/uncompress the relevant PHP-PSR file and go in its build folder:
       * `$ wget https://github.com/jbboehr/php-psr/archive/v1.0.0.tar.gz`
       * `$ tar xvzf v1.0.0.tar.gz`
       * `$ cd php-psr-1.0.0/`
    2. Compile and install using the phpize and php-config paths from step 3:
       * `$ /usr/local/php7/bin/phpize`
       * `$ ./configure --with-php-config=/usr/local/php7/bin/php-config`
       * `$ make`
       * `$ make test`
       * `# make install`
    3. Everything should then be compiled fine and the files put in their right location,
       but you still need to adjust your PHP configuration, so let's create a config file for PHP-PSR and restart PHP-FPM:
       * `# echo 'extension=psr.so' > /etc/php7.d/extensions/40-psr.ini`
       * `# systemctl restart php7-fpm.service`
    4. PHP-PSR needs to be loaded before Phalcon, that's why the number at the beginning of the PHP-PSR file is lower
       than the Phalcon one.
8. And it's done! You can always call phpinfo() to check.


## 日本語
(translation welcome, given that KUSANAGI is mostly used in Japan)


## Licence
Document under The Unlicense, so do what you want with it.
