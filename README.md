# PHP Git Hooks

The Git hooks for applying in the local repository of the PHP project. Include the `pre-commit` hook.

## Installation

Clone repository:

```bash
cd /Volumes/Macintosh\ HD/var/www/psr
git clone https://github.com/vannh-ait/check_psr.git
cd check_psr
php -r "readfile('https://getcomposer.org/installer');" | php
./composer.phar install
```

Make symlink to the `pre-commit` file:

```bash
cd /Volumes/Macintosh\ HD/var/www/demo/.git/hooks
ln -s cd /Volumes/Macintosh\ HD/var/www/psr/check_psr/pre-commit pre-commit
```

## pre-commit

Checks the committed files:

* PHP Syntax on PHP-errors (with PHPLint)
* Fix style PSR-2 with php-fixer-cs
* Fix style PSR-2 with phpcbf
* Check PHPCS
* Check PHPMD

Based on `pre-commit` hook of [Carlos Buenosvinos](http://carlosbuenosvinos.com/write-your-git-hooks-in-php-and-keep-them-under-git-control/).


### Check psr but not commit
```bash
cd /Volumes/Macintosh\ HD/var/www/psr/check_psr/vendor/bin

---- Check ---------
php -l file_name_or_folder_name
phpcs --standard=PSR2 file_name_or_folder_name
phpmd file_name_or_folder_name  text cleancode,codesize,controversial,design,naming,unusedcode

----- Fix----
php-cs-fixer fix file_name_or_folder_name --level=psr2
./phpcbf file_name_or_folder_name --standard=PSR2
```

### Example of output

```bash
$ git ci -m "commit message"
Check Code Quality Tool
Fetching files
Running PHPLint
Checking code style
1) app/Controller/TestControllerTest.php (unused_use, eof_ending)

  [Exception]
  There are coding standards violations!
```

```bash
$ git ci -m "commit message"
Check Code Quality Tool
Fetching files
Running PHPLint
Checking code style
Checking code style with PHPCS
FILE: ...app/Controller/TestControllerTest.php
--------------------------------------------------------------------------------
FOUND 0 ERROR(S) AND 2 WARNING(S) AFFECTING 2 LINE(S)
--------------------------------------------------------------------------------
 197 | WARNING | Line exceeds 120 characters; contains 172 characters
 212 | WARNING | Line exceeds 120 characters; contains 128 characters
--------------------------------------------------------------------------------

  [Exception]
  There are PHPCS coding standards violations!
```

```bash
$ git ci -m "commit message"
Check Code Quality Tool
Fetching files
Running PHPLint
Checking code style
Checking code style with PHPCS
Good job dude!
[some-branch 0f5ea39] commit message
 10 files changed, 357 insertions(+), 17 deletions(-)
 create mode 120000 bin/php-cs-fixer
 create mode 120000 bin/phpcs
 create mode 120000 bin/phpmd
 create mode 120000 bin/php-fixer-cs
 create mode 120000 bin/phpcbf
 create mode 100644 app/Controller/TestControllerTest.php
```
