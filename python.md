## install python on mac using homebrew

brew install python


## easy install
sudo python -m easy_install --upgrade pip

## install virtual environment

sudo pip install virtualenv

## create virtual environment

virtualenv myprojectenv

virtualenv -p /usr/local/bin/python2.7 venv-django

virtualenv --python=python3.5 venv_name

python3 -m venv --without-pip webapp

source myprojectenv/bin/activate

deactivate


## virtualenvwrapper

Working with multiple virtualenv can become really teddy and cumbersome: you have to switch and remember where is located the venv you want to use. This is where virtualenvwrapper comes to place: virtualenvwrapper is a set of extensions to virtualenv tool. You can easily list, create and delete virtual environments; all of your virtual environments are organized in one place.
Installing virtualenv is really simple, with your main Python:
pip install virtualenvwrapper
Next, you have to add source /usr/local/bin/virtualenvwrapper.sh to your shell startup file (on OS X, you can put this at the end of ~/.bash_profile).
Then, everything is set up and you can create a new venv:
$ mkvirtualenv env1
New python executable in env1/bin/python2.7
Also creating executable in env1/bin/python
Installing setuptools, pip…done.
(env1)~ $
By default, all your virtual environments will be created under ~/.virtualenvs/ but you can change it.
List all environnement:
$ workon
env1
env2
Switch on environnement env1:
$ workon env1



## install prosresql plugin

pip install psycopg2


## django project

django-admin.py startproject myproject .

python manage.py makemigrations

python manage.py migrate

python manage.py createsuperuser

python manage.py runserver 0.0.0.0:8000

## connect project to postgresql

from:

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}

to:

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': 'myproject',
        'USER': 'myprojectuser',
        'PASSWORD': 'password',
        'HOST': 'localhost',
        'PORT': '',
    }
}


## Shell scripts with Python
If you write shell scripts in Python, you can launch your script by simply call:
$ python myscript.py
This will execute your script, with the current python interpreter. If your script need a particular virtual environnement to run, you can run it in this virtual environment without having to activate it before. Simply hard code the python interpreter binary that ways:
$ /Users/jc/.virtualenvs/env1/bin/python myscript.py
This will execute the script myscript.py in the virtual environnement defined at /Users/jc/.virtualenvs/env1. All the packages that you have installed in this virtual environnement will be callable in the script.
If you want to call directly your script without typing python, add the proper permission on your script
$ chmod u+x myscript.py
And add the following shebang at the beginning of your script:
#!/usr/bin/env python
You can also point to a particular python interpreter if you want:
#!/Users/jc/.virtualenvs/env1/bin/python
Which is considerably less portable. #!/usr/bin/env python will use the current python interpreter of your shell session (which can be a virtualenv ones).
Finally, it’s a best practice to declare the encoding of your script file. On OS X, use UTF-8.
For example, the following script:
#!/usr/bin/env python


if __name__ == ’__main__’:
    print("Hello こんにちは!")
will produced this error when executed:
File "./testutf8.py", line 5
SyntaxError: Non-ASCII character ’\xe3’ in file ./testutf8.py on line 5, but no encoding declared; see http://www.python.org/peps/pep-0263.html for details
With the proper encoding hint, everything works as it should be (you can even use emojis if you want!):
#!/usr/bin/env python
# -*- coding: utf-8 -*-


if __name__ == ’__main__’:
    print("Hello こんにちは!")


$ ./testutf8.py
Hello こんにちは!


## Must-have packages
I’m not a Python expert, and everyone’s needs are different so take this list with a grain of salt.
* •	Requests: simply HTTP for humans. If you deals with HTTP requests, just use Request and your life will get a lot easier.
* •	BeautifulSoup: parsing HTML and XML data. So good.
* •	SQLAlchemy: efficient and high-performing SQL toolkit.
* •	Pillow: imaging library (fork of the venerable PIL). Just works.
* •	lxml: another HTML XML processing tookit. You can prefer BeautifulSoup APIs, but BeautifulSoup can also use lxml as a parser. It’s a matter of choice, I prefer BeautifulSoup.
* •	Jinga: full featured template engine.
* •	Pyments: generic syntax highlighter for general use in all kinds of software such as forum systems, wikis or other applications that need to prettify source code.
If you want to do web development, you could try:
* •	Django: unavoidable. Django is the behemoth of the Python web framework. I love it, for the documentation, the admin interface, the ORM, and the built-in authentication support. If you need something smaller, you can use Flask or Bottle.
* •	Flask: fast, simple and lightweight micro web framework.
* •	Bottle: fast, simple and lightweight micro web framework.
Flask and Bottle are both excellent. Give both a try before deciding which one to use.

## IDEs on OS X
You can write Python code with vim but there are good and complete Python IDEs on OS X.
* •	PyCharm: my Python IDE of choice. Starting at $99, it totally worths it. There is also a free edition. My only (minor) grip is that it isn’t a totally native OS X app (and there are too much preferences!).
* •	Exedore: much lighter than PyCharm and written entirely in Cocoa/Objective-C so it feels right at home on your Mac. Give-it a try (free trial, then $9.99).
* •	PyDev: if you like Eclipse, PyDev is for you. I’ve tried it but prefer PyCharm.

## Fabric
Fabric is a command-line tool for streamlining the use of SSH for application deployment or systems administration tasks. I use it for deploying this blog from my Mac for instance. With Fabric, you can launch command as if you were on your server, and execute arbitrary Python functions via the command line.
For exemple, in a fabfile.py file you code:
from fabric.api import local

def prepare_deploy():
    local("./manage.py test my_app")
    local("git add -p && git commit")
    local("git push")
And then on the command line, you can launch:
$ fab prepare_deploy
And the commands will be executed locally.
But the real power of Fabric it that it can run commands on your server, from your Mac! For instance:
env.hosts = [’example.com’]

def deploy():
    code_dir = ’/srv/django/myproject’
    with cd(code_dir):
        run("git pull")
        run("touch app.wsgi")
If you execute fab deploy, the commands will be sent from your Mac to your server (via SSH) and executed here! Fabric really simplify you deployment needs and you can deploy, run scripts on multiple servers easily. I use it to deploy the blog on our server, and we use it also to deploy our various Django projects.



eCommerce
    http://getsaleor.com/
    http://oscarcommerce.com/
    https://github.com/openlabs/nereid-webshop
    http://satchless.com/

CMS


DJANGO
        django-admin startproject
            - https://wagtail.io/
                wagtail_env
                pip install wagtail
                wagtail start mysite
                python manage.py createsuperuser
                python manage.py migrate
                python manage.py runserver

            - http://mezzanine.jupo.org/
                mezz_env
                pip install mezzanine
                mezzanine-project myproject
                python manage.py createdb
                python manage.py runserver

            - https://www.django-cms.org/en/
                djangocms_env
                pip install djangocms-installer
                djangocms mysite
                python manage.py runserver

            - https://django-edge.readthedocs.io/en/latest/
                https://github.com/arocks/edge
                edge

    WEB2PY
        python web2py.py
        python web2py.py -k myapp.key -c myapp.crt -i 0.0.0.0 -p 8082

    WEB.PY
        pip install web.py

    PYRAMID
        http://docs.pylonsproject.org/projects/pyramid/en/latest/narr/project.html
        pip install pyramid
        pcreate -s starter MyProject
        pip install -e .
        - running the test of application
            pip install -e ".[testing]"
            py.test -q
        - running the project application
            pserve development.ini

    TURBOGEARS
        http://www.turbogears.org/
        pip install tg.devtools
        gearbox quickstart example
        cd example/
        pip install -e .
        gearbox serve

    -SALEOR
    -SHUUP
    -TENSORFLOW
