.. _creating_structure:

Service Project Structure
=========================

Introduction
------------
In this section we describe the files created when you initialise a StackHut project using ``stackhut init baseos language`` (e.g. ``stackhut init fedora python``).

.. _creating_structure_hutfile:

Hutfile
-------

The ``Hutfile`` is a YAML file that describes the configuration of your StackHut service. It has several required fields that describe the project and let you control the packaging of your service.

Here's a sample,

.. code-block:: yaml

    # Name for the service - we recommend using snake-case
    name: demo-python

    # Service description
    description: Awesome service in python

    # GitHub source repo link (optional)
    github_url: www.github.com/StackHut/demo-python

    # The service dependencies, in terms of the base OS and language runtime
    baseos: fedora
    stack: python

    # Persist the service between requests
    persistent: False

    # Restrict service access to authenticated users
    private: False

Let's go through all the fields.

``name``
^^^^^^^^

*Required*

The name of the service - this does not need to be unique. We recommend keeping this the same as your project-s name in source control (e.g. on GitHub) and using snake-case.


``github_url``
^^^^^^^^^^^^^^

*Optional*

A link to the repository on GitHub - we use this to create links on the service homepage to the issue tracker and source, if available.

``description``
^^^^^^^^^^^^^^^

*Optional*

A short text description of the service.

``baseos``
^^^^^^^^^^

*Required*

The base OS to use when creating an image. Currently we support,

======  ===========
Name    Description    
======  ===========
fedora  `Fedora 22 <http://getfedora.org/>`_ 
alpine  `Alpine Linux <http://alpinelinux.org/>`_
======  ===========


.. _creating_structure_hutfile_stack:

``stack``
^^^^^^^^^

*Required*


The base language stack to use when creating an image. Currently we support,

=======     ===========
Name        Description    
=======     ===========
python      `Python 3 <http://python.org/>`_ 
nodejs      `Node.js <http://nodejs.org/>`_  (implemented using `io.js <http://iojs.org/>`_)
=======     ===========

.. note:: Currently we only support one language stack per service (although you could use this to call anothor language you've bundled into the image)

.. note:: If your chosen language stack has a package manager, e.g. ``pip``, ``npm``, etc., you can fill out the package file, e.g. ``requirements.txt``, ``package.json``, etc., and it will be installed automatically within your image.


``private``
^^^^^^^^^^^

*Required*

Boolean indicating if the service is private. If true only requests by an authenticated StackHut user are permitted. If false the service is public and requests to the service can be made by anyone.

``persistent``
^^^^^^^^^^^^^^

*Required*

Indicates whether the service persists between request or is terminated and restart on demand on each new request. Setting a service as persistent decreases response times and can hold state between requests, however will require more resources.

``files``
^^^^^^^^^

*Optional*

A list of files and directories within the project directory to include and bundle alongside the image. Useful for specifying resource files and binaries, for instance.

See `<http://www.github.com/StackHut/image-process>`_ for an example.

``os_deps``
^^^^^^^^^^^

*Optional*

A list of OS packages you wish to bundle with your service, i.e. those installable via ``yum``, or ``apt-get``, depending on your ``baseos``. You may need to check with your choosing base OS repository to find the names of the packages and their versions.

.. note:: you can also install language specific packages using your language package manager as described in the :ref:`creating_structure_hutfile_stack` subsection above.

See `<http://www.github.com/StackHut/pdf-tools>`_ for an example.


``docker_cmds``
^^^^^^^^^^^^^^^

*Optional*

If the default workflow is not flexible enough, you can specify any additional scripting commands as a list within this section. They will be run when building your container and follow the `Dockerfile builder specification <https://docs.docker.com/reference/builder/>`_.

See `<http://www.github.com/StackHut/browser>`_ for an example.

