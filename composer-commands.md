# Composer Cheat Sheet for developers

Composer is the dependency manager for PHP and you are on a one-page-only documentation for this tool.
Official documentation is on the official website, this page just brings you the essential.

## One line installer
```
curl -sS https://getcomposer.org/installer | php
```
or

Make composer global
```
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"

php -r "if (hash_file('SHA384', 'composer-setup.php') === 'aa96f26c2b67226a324c27919f1eb05f21c248b987e6195cad9690d5c1ff713d53020a02ac8c217dbf90a7eacc9d141d') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"


php composer-setup.php  --install-dir=/usr/local/bin --filename=composer

php -r "unlink('composer-setup.php');"

```
composer.phar most needed command line


$ composer require vendor-name/package-name

Adds required packages to your composer.json and installs them.
The require command adds new packages to the composer.json file from the current directory.
After adding/changing the requirements, the modified requirements will be installed or updated.

$ composer install

Parses the composer.json file and downloads the needed dependencies.

$ composer update
$ composer update --lock
$ composer dump-autoload --optimize

### other commands

$ composer about
$ composer archive
$ composer browse
$ composer clear-cache
$ composer config --list
$ composer create-project symfony/standard-edition dir/
$ composer depends vendor-name/package-name
$ composer diagnose
$ composer exec
$ composer global
$ composer help
$ composer info
$ composer init
$ composer licenses
$ composer list
$ composer outdated
$ composer prohibits
$ composer remove
$ composer run-script
$ composer search my keywords
$ composer self-update
$ composer show
$ composer status
$ composer suggests
$ composer validate


### composer.json file

{
    "name": "vendor-name/project-name",
    "description": "This is a very cool package!",
    "version": "0.3.0",
    "type": "library",
    "keywords": ["logging", "cool", "awesome"],
    "homepage": "https://jolicode.com",
    "time": "2012-12-21",
    "license": "MIT",
    "authors": [
        {
            "name": "Xavier Lacot",
            "email": "xlacot@jolicode.com",
            "homepage": "http://www.lacot.org",
            "role": "Developer"
        },
        {
            "name": "Benjamin Clay",
            "email": "bclay@jolicode.com",
            "homepage": "https://github.com/ternel",
            "role": "Developer"
        }
    ],
    "support": {
        "email": "support@exemple.org",
        "issues": "https://github.com/jolicode/jane/issues",
        "forum": "http://www.my-forum.com/",
        "wiki": "http://www.my-wiki.com/",
        "irc": "irc://irc.freenode.org/composer",
        "source": "https://github.com/jolicode/jane",
        "docs": "https://github.com/jolicode/jane/wiki"
    },
    "require": {
        "monolog/monolog": "1.0.*",
        "joli/ternel": "@dev",
        "joli/ternel-bundle": "@stable",
        "joli/semver": "^2.0",
        "joli/package": ">=1.0 <1.1",
        "acme/foo": "dev-master#2eb0c097"
    },
    "require-dev": {
        "debug/dev-only": "1.0.*"
    },
    "conflict": {
        "another-vendor/conflict": "1.0.*"
    },
    "replace": {
        "debug/dev-only": "1.0.*"
    },
    "provide": {
        "debug/dev-only": "1.0.*"
    },
    "suggest": {
        "jolicode/gif-exception-bundle": "For fun!"
    },
    "autoload": {
        "psr-4": {
            "Monolog\\": "src/",
            "Vendor\\Namespace\\": ""
        },
        "psr-0": {
            "Monolog": "src/",
            "Vendor\\Namespace": ["src/", "lib/"],
            "Pear_Style": "src/",
            "": "src/"
        },
        "classmap": ["src/", "lib/", "Something.php"],
        "files": ["src/MyLibrary/functions.php"]
    },
    "autoload-dev": {
        "psr-0": {
            "MyPackage\\Tests": "test/"
        }
    },
    "target-dir": "Symfony/Component/Yaml",
    "minimum-stability": "stable",
    "repositories": [
        {
            "type": "composer",
            "url": "http://packages.example.com"
        },
        {
            "type": "vcs",
            "url": "https://github.com/Seldaek/monolog"
        },
        {
            "type": "pear",
            "url": "http://pear2.php.net"
        },
        {
            "type": "package",
            "package": {
              "name": "smarty/smarty",
              "version": "3.1.7",
              "dist": {
                "url": "http://www.smarty.net/Smarty-3.1.7.zip",
                "type": "zip"
              },
              "source": {
                "url": "http://smarty-php.googlecode.com/svn/",
                "type": "svn",
                "reference": "tags/Smarty_3_1_7/distribution/"
              }
            }
        },
        {
            "type": "artifact",
            "url": "path/to/directory/with/zips/"
        },
        {
            "type": "path",
            "url": "../../packages/my-package"
        }
    ],
    "config": {
        "process-timeout": 300,
        "use-include-path": false,
        "preferred-install": "auto",
        "store-auths": "prompt",
        "github-protocols": ["git", "https", "http"],
        "github-oauth": {"github.com": "oauthtoken"},
        "gitlab-oauth": {"gitlab.com": "oauthtoken"},
        "github-domains": ["entreprise-github.me.com"],
        "gitlab-domains": ["entreprise-gitlab.me.com"],
        "github-expose-hostname": true,
        "disable-tls": false,
        "cafile": "/var/certif.ca",
        "capath": "/var/",
        "http-basic": {"me.io":{"username":"foo","password":"bar"},
        "platform": {"php": "5.4", "ext-something": "4.0"},
        "vendor-dir": "vendor",
        "bin-dir": "bin",
        "data-dir": "/home/ternel/here",
        "cache-dir": "$home/cache",
        "cache-files-dir": "$cache-dir/files",
        "cache-repo-dir": "$cache-dir/repo",
        "cache-vcs-dir": "$cache-dir/vcs",
        "cache-files-ttl": 15552000,
        "cache-files-maxsize": "300MiB",
        "bin-compat": "auto",
        "prepend-autoloader": true,
        "autoloader-suffix": "pony",
        "optimize-autoloader": false,
        "sort-packages": false,
        "classmap-authoritative": false,
        "notify-on-install": true,
        "discard-changes": false,
        "archive-format": "tar",
        "archive-dir": "."
    },
    "archive": {
        "exclude": ["/foo/bar", "baz", "/*.test", "!/foo/bar/baz"]
    },
    "prefer-stable": true,
    "scripts": {
        "pre-install-cmd": "MyVendor\\MyClass::doSomething",
        "post-install-cmd": [
            "MyVendor\\MyClass::warmCache",
            "phpunit -c app/"
        ],
        "pre-update-cmd": "MyVendor\\MyClass::doSomething",
        "post-update-cmd": "MyVendor\\MyClass::doSomething",
        "pre-status-cmd": "MyVendor\\MyClass::doSomething",
        "post-status-cmd": "MyVendor\\MyClass::doSomething",
        "pre-package-install": "MyVendor\\MyClass::doSomething",
        "post-package-install": [
            "MyVendor\\MyClass::postPackageInstall"
        ],
        "pre-package-update": "MyVendor\\MyClass::doSomething",
        "post-package-update": "MyVendor\\MyClass::doSomething",
        "pre-package-uninstall": "MyVendor\\MyClass::doSomething",
        "post-package-uninstall": "MyVendor\\MyClass::doSomething",
        "pre-autoload-dump": "MyVendor\\MyClass::doSomething",
        "post-autoload-dump": "MyVendor\\MyClass::doSomething",
        "post-root-package-install": "MyVendor\\MyClass::doStuff",
        "post-create-project-cmd": "MyVendor\\MyClass::doThis",
        "pre-archive-cmd": "MyVendor\\MyClass::doSomething",
        "post-archive-cmd": "MyVendor\\MyClass::doSomething",
    },
    "extra": { "key": "value" },
    "bin": ["./bin/toto"]
}