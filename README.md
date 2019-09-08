# EB_NON_WORKIN_EXAMPLE
This sample Django application is to show the bug for AWS support
If you try to deploy this application through elastic beanstalk to any of the platfroms you will get the error which is described below
-Python 3.6 running on 64bit Amazon Linux/2.9.2
-Python 3.6 running on 64bit Amazon Linux/2.9.1
-Python 3.6 running on 64bit Amazon Linux/2.8.3

Error description:

During the deployment through EB the following error is raised when executing "source /opt/python/run/venv/bin/activate && python manage.py migrate --noinput" through container command:

Command failed on instance. Return code: 1 Output: (TRUNCATED)...gs]', add_help=False, allow_abbrev=False) File "/opt/python/run/venv/local/lib/python3.6/site-packages/django/core/management/base.py", line 48, in __init__ super().__init__(**kwargs) TypeError: __init__() got an unexpected keyword argument 'allow_abbrev'. container_command 01_migrate in .ebextensions/01-django_eb.config failed. For more detail, check /var/log/eb-activity.log using console or EB CLI.

Basically, the problem is with the argparse in venv and not with Django itself.
If argparse is used in venv it complains on 'allow_abbrev' argument.

Example:

import argparse
parser = argparse.ArgumentParser(prog='PROG', allow_abbrev=False)                                                         Traceback (most recent call last):                                                                                             File "<stdin>", line 1, in <module>                                                                                           TypeError: __init__() got an unexpected keyword argument 'allow_abbrev' 

This error does not happen when using argparse outside of venv
