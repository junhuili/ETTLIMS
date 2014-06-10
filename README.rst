====================
ETTLIMS
====================

.. image:: https://travis-ci.org/BILS/SCGLIMS.svg?branch=master
  :target: https://travis-ci.org/BILS/SCGLIMS

.. image:: https://coveralls.io/repos/BILS/SCGLIMS/badge.png?branch=master
  :target: https://coveralls.io/r/BILS/SCGLIMS?branch=master


ETTLIMS is a Single Cell Genomics Lab Information Management System developed
for the `Ettema Lab`_ by `BILS`_.

The database implements the workflow of the Ettema Lab:

.. image:: http://raw.githubusercontent.com/BILS/ETTLIMS/master/docs/images/flowchartlab.png

If your lab has a similar workflow you might be able to use the ETTLIMS as is.
Otherwise an amout of tweaking might be required in which case we would
recommend forking the repository and only using certain elements.


* Documentation: `<http://scglims.rtfd.org>`_
* Demo: `<http://bit.ly/scglims-heroku>`_ (for admin user/pass inodb)
* Presentation: `<http://bit.ly/limstalk>`_
* GitHub: `<https://github.com/BILS/ETTLIMS/>`_
* Free software: GPLv3 License
.. * PyPI: Not yet available

.. _`Ettema Lab`: http://ettemalab.org
.. _`BILS`: http://bils.se

Requirements
-----------

* Python 2.7+
* Django 1.6.1
* Local: https://github.com/BILS/ETTLIMS/blob/master/lims_project/requirements/local.txt
* Development: https://github.com/BILS/ETTLIMS/blob/master/lims_project/requirements/development.txt

Installation
-------------

Clone the repository to your computer:

::
    
    git clone https://github.com/BILS/ETTLIMS

Local installation:

::
    
    pip install -r lims_project/requirements/local.txt


Running ETTLIMS
----------------

Locally with SQLite
*******************

Without example data:

::
        
    cd lims_project
    python manage.py syncdb --settings=lims_project.settings.local && \
    python manage.py runserver 127.0.0.1:8000 --settings=lims_project.settings.local

With all example data:
::
     
    cd lims_project
    python manage.py syncdb --settings=lims_project.settings.local && \
    python manage.py loaddata example barcode_example --settings=lims_project.settings.local && \
    python manage.py runserver 127.0.0.1:8000 --settings=lims_project.settings.local

Remove ``example`` from the ``loaddata`` command if you only want the
``barcode_example`` data and vice versa.

Empty the database:
::

    cd lims_project
    rm lims_project/lims_project.sqlite

Development with PostgreSQL database
************************************
First you need to `set up PostgreSQL`_. You'll have to create a database django
and user django, see the `development settings`_. Then the commands are similar to running it locally, but you change
``lims_project.settings.local`` to ``lims_project.settings.development`` e.g.:
::
    cd lims_project
    python manage.py syncdb --settings=lims_project.settings.development && \
    python manage.py loaddata example barcode_example --settings=lims_project.settings.development && \
    python manage.py runserver 127.0.0.1:8000 --settings=lims_project.settings.development

To empty the PostgreSQL database you can run:
::
    python manage.py sqlclear sessions admin lims auth contenttypes \
        --settings=lims_project.settings.development | \
    grep -v parent_id | \
    python manage.py dbshell --settings=lims_project.settings.development

The ``grep -v parent_id`` is necessary to remove a non-existing constraint from
the generated sql statements by django. This is a `known django bug`_.

.. _`known django bug`: https://code.djangoproject.com/ticket/22611
.. _`set up PostgreSQL`: http://www.techrepublic.com/article/set-up-a-postgresql-database-server-on-linux/
.. _`development settings`: http://github.com/BILS/ETTLIMS/blob/master/lims_project/lims_project/settings/development.py


Contribute
----------

Contributions are greatly appreciated, please read the `contribution instructions`_.

.. _`contribution instructions`: https://github.com/BILS/ETTLIMS/blob/master/CONTRIBUTORS.md
