---
title:  "Programmatic Ansible with Python"
date:   2016-06-13 09:00:00
tags: ['tutorial', 'python', 'docker', 'ansible', 'automation', 'ansible-python-api']
categories: python ansible
permalink: :categories/:title
---
Hello!

As the title suggests, in this tutorial, we will write a python script to run Ansible programmatically.

Ansible is an IT automation tool written in Python. You can find out more [here](https://www.ansible.com/).
We will use [Ansible Python API](http://docs.ansible.com/ansible/developing_api.html) to write the script.
This API enables us to use Ansible programmatically through Python.

The play will be able to install docker and run a container in the local machine.

So let’s begin!
<br>
<br>

**First setup all the imports**

{% highlight python %}
from collections import namedtuple
from ansible.parsing.dataloader import DataLoader
from ansible.vars import VariableManager
from ansible.inventory import Inventory
from ansible.playbook.play import Play
from ansible.executor.task_queue_manager import TaskQueueManager
{% endhighlight %}

Here we have some important modules to set the ground for the play.

* The data loader is for loading data from a YAML or JSON formatted string/file. But in this play we don’t require it, so will initialize empty.
* The VariableManager is for setting different variables and specifying the inventory file.
* The Inventory is for initializing an inventory.
* The Play is used to set up the play.
* The TaskQueueManager is for actually running the play. 
<br>
<br>

**Now we will initialize the objects that are needed for the play.**

{% highlight python %}
Options = namedtuple('Options',
                ['connection', 'module_path', 'forks', 'become',
                 'become_method', 'become_user', 'check']
            )

#initialize needed objects

variable_manager = VariableManager()
loader = DataLoader()

options = Options(
    connection='local', module_path='', forks=100, become=True,
    become_method='sudo', become_user='root', check=False)
passwords = dict(vault_pass='secret')
{% endhighlight %}

Here we have set few options, such as the connection type, path to extra ansible modules (since we didn’t need here so set that empty), the number of forks to create.
We have also set become, become_method and become_user to run the command as root user. 
<br>
<br>

{% highlight python %}
#create inventory and pass to variable manager

inventory = Inventory(loader=loader, variable_manager=variable_manager,
                      host_list='localhost')
variable_manager.set_inventory(inventory)
{% endhighlight %}

Finally initiated an inventory with a list of hosts to run (here we are playing around with localhost). 
<br>
<br>

**Writing the play**

{% highlight python %}
#create play with tasks

play_src = dict(
    name="Install and run a Docker container",
    hosts="localhost",
    gather_facts="no",
    tasks=[
        # installing dependencies

        dict(name="Install Dependencies",
             action=dict(module="apt", args=dict(name="", update_cache="yes")),
             with_items=["python-dev", "python-setuptools"]),

        # installing pip

        dict(name="Install pip",
             action=dict(module="easy_install", args=dict(name="pip"))),

        # installing docker-py; it is required for docker_container module

        dict(name="Install docker-py",
             action=dict(module="pip", args=dict(name="docker-py", state="present"))),

        # add docker apt repository

        dict(name="Add docker apt-repo",
             action=dict(module="apt_repository",
                         args=dict(repo="deb https://apt.dockerproject.org/repo ubuntu-trusty main", state="present"))),

        # add docker repository key

        dict(name="Import the Docker repository key",
             action=dict(module="apt_key",
                         args=dict(url="https://apt.dockerproject.org/gpg", state="present", id="2C52609D"))),

        # install docker

        dict(name="Install Docker package",
             action=dict(module="apt",
                         args=dict(name="docker-engine", update_cache="yes"))),

        # create docker container

        dict(name="Create a data container",
             action=dict(module="docker_container",
                         args=dict(name="test", image="busybox", volumes="/data"))),

        # run a command iside the container

        dict(name="Run a command",
             action=dict(module="command", args="docker run busybox /bin/echo 'hello world'"),
             register="output"),

        # printing the output

        dict(name="Output", action=dict(module="debug", args=dict(var="output.stdout_lines"))),
    ],

    # handler for restarting docker

    handlers=[dict(name="restart docker",
                   action=dict(module="service", args=dict(name="docker", state="restarted")))]
)
{% endhighlight %}

Everything here is self explanatory. We create a dictionary for play with it’s name. hosts and tasks. tasks is a list of tasks we want to run.

Inside each list, there is a name for the task, an action to perform, and other things like `register` (if we want to cache some output in a variable),
`with_items` (to loop over items). Every action includes another dictionary with a module that will run in the task and it’s arguments.
<br>
<br>

**Configure the play**

{% highlight python %}
play = Play().load(play_src, variable_manager=variable_manager, loader=loader)
{% endhighlight %}

We have loaded the play with required configuration and the play data we wrote earlier. 
<br>
<br>

**Running the play**

{% highlight python %}
tqm = None
try:
    tqm = TaskQueueManager(
            inventory=inventory,
            variable_manager=variable_manager,
            loader=loader,
            options=options,
            passwords=passwords,
            stdout_callback="default",
        )
    result = tqm.run(play)
finally:
    if tqm is not None:
        tqm.cleanup()
{% endhighlight %}

Here the TaskQueueManager object is created with all the configurations we cooked earlier. Then we run the play using tqm.run(play).

If the TaskQueueManager was initiated successfully then it’s cleaned up in the end.

You can find the complete example in this [gist](https://gist.github.com/curioswati/55843fac0e0d251202006b68d756e245).

So we saw an example of how we can leverage the Python API to program ansible. Automation of an automation tool, sounds cool, No?

There are many other things we could do, we have just seen a small example to get started. You could dig in the [cli](https://github.com/ansible/ansible/tree/devel/lib/ansible) itself to find out the more cool stuff. 
<br>
<br>

### References

1. [http://rancher.com/using-ansible-with-docker-to-deploy-a-wordpress-service-on-rancher/](http://rancher.com/using-ansible-with-docker-to-deploy-a-wordpress-service-on-rancher/)
2. [http://docs.ansible.com/ansible/docker_container_module.html](http://docs.ansible.com/ansible/docker_container_module.html)
