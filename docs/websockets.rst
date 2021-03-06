===================================
Web Sockets
===================================


Why use Web Sockets?
=================================

.. images/image:: socket_vs_api.png


- WebSocket is a low-level protocol based on the concept of socket and port, which are the underlying transport mechanism, whereas REST is based on CRUD operation.
- WebSocket requires the use of IP address and Port details, which are lower-level details for any application, whereas RESTful application needs to design operation based on verbs and HTTP based.
- WebSocket is bi-directional in nature, i.e. both way operation from client to server and vice versa is possible, whereas REST follows a uni-directional approach.
- Short data transmission and higher performance
- Django 3.0 natively supports socket communication


Reactjs Intergation
=================================

I have forked a django react library, so I can modify the code and manage any customization.
I also forked this library due to it's compatibility with a newer create-react-app verions.

Intergration Steps
##################

#. Copy over (or create an app yourself) the reactjs application folder to your project's root. It's called 'frontend_react'
#. Copy over the react_helper into your Django project's root directory.
#. Open your settings.py file and add this app BEFORE 'django.contrib.staticfiles':

    ::

        INSTALLED_APPS = [
            ...........
            'react_helper', # Needs to be before staticfiles
            'django.contrib.staticfiles',
            '..........
        ]


#. Add the constant 'REACT_APPNAME' to your settings.py and specify the name of the react project folder:

    ::

        # The name of the folder containing the CRA project files,
        # relative to the base directory of the Django project (the folder containing manage.py):
        REACT_APPNAME = 'frontend_react' # Name of the React project inside our Django project

#. Open a second terminal and move into the react directory and start the create-react-app (assuming you have npm and yarn installed):

    .. code-block:: console

       $ cd react_frontend

       $ yarn start

#. Stop the reactjs server and create a build of the app's js and css:

    .. code-block:: console

       $ yarn build

#. Inside the html template file you want to use to connect to reactjs, add this:

    ::

        {% load react_helper_tags %}
        {% load static %}

        {% get_react_static_css %}
        <div id="react" ></div>
        <script>
            window.component = '{{ component }}';
        </script>
        {% get_react_static_js %}

#. Inside your main terminal, which is still at the root of your django project, run this custom command to collect the build into your project's 'static' directory:

    .. code-block:: console

       $ python manage.py collectstatic_react

#. Now you can start the server:

    .. code-block:: console

       $ python manage.py runserver

#. You should now see your react app loaded into the html template you loaded your code in


Validation
##################

You can validate from the terminal console outputs if the intergration went well, and that your Django application is aware that the React app is running.

    - This way Django know when to use the 'hot-reload' functionality of the react app. If react app server isn't running, then helper app will serve the build js and css files for when you have ran 'yarn build'.
    - Check to see if it outputs 'INFO:root:React liveserver is running':

    .. code-block:: console

        (upgrade_data_fluent) vascomeerman@vascos-mac-mini upgrade_data_fluent % python manage.py runserver
        INFO:root:http://localhost:3000
        INFO:root:React liveserver is running at: http://localhost:3000
        Watching for file changes with StatReloader
        .........



Websocket Communication
=================================

To use websocket comminumation within django, you do need to install uvicorn:

    .. code-block:: console

       $ pip install uvicorn

Furthermore, you can follow multiple tutorials on how do the full setup, which comes down to switching your project from WSGI to ASGI so it support asynchronous communication.
You can see my implementation under data_fluent_core in:

    - asgi.py
    - websocket.py


Read more about websocket intergration in Django 3.0 and above, |new_tab_link|.

.. |new_tab_link| raw:: html

   <a href="https://dev.to/alexoleshkevich/websockets-in-django-3-1-532o" target="_blank">works</a>
