+++
date = "2016-08-28T13:47:08+02:00"
tags = ["ansible", "devops"]
categories = ["devops", "ansible"]
title = "Ansible playbooks"
description = "Ansible playbooks"
+++


Ansible  is a task execution engine. It exists to provide a method for operators, engineers, developers or whomever, to easily define one or more actions to be performed on one or more computers. 

This capability represents a step beyond simply just logging into each computer in question, and manually typing out the command. 

These tasks can target the local system Ansible is running from, as well as other systems Ansible can reach over the network.

In this post I will show how to work with Ansible that  are one of the core features of Ansible which tell to Ansible what we want to execute. 


## What are Playbooks?

Playbooks are YAML formatted files that collect one or more plays. 

Plays are one or more tasks linked to the hosts that they are to be executed on. 

Ansible playbook is the Ansible executable that is used to parse and execute the playbook with a provided inventory. To demonstrate playbooks, first I need to create an inventory. My inventory will just be a set of hosts in a couple of groups. Each host is uniquely named so that we can distinguish it in the output. In addition, we are going to define a variable for all the hosts that instructs Ansible to connect to these fake hosts locally. I'll need a playbook file, which I'll create with my text editor named demoplays.yaml. YAML files are simply text files, so any text editor will do. I happen to use Vim. After the traditional three dashes to indicate a YAML file, I'll create the first play. Plays require a name, a host pattern, and some tasks. I'll call this first play Do a demo. For a host pattern, I'll use the groupA set of hosts I just created. Next, I'll create a section for tasks. Like plays, tasks are expressed as a YAML list and each task should have a name. I'll give my first task a name of demo task one. Tasks require a module, and for demonstration purposes I'll make use of the debug module. The debug module can take a message argument to display provided text on the screen. I'll display this is task one. I'll duplicate this task for a second task, altering the name and the output string. Next, I'll add a second play with a similar name. Usings hosts from groupB, I'll copy all the tasks from the first play, and again, I'll alter the task names and the output strings to indicate their location in the file. First I'll copy this entire play. Change the name of the play to distinguish. Change the host to groupB. And change the task details to indicate that it's from the second play. Plays are executed from top to bottom and tasks are also executed from top to bottom So my play and task numbering will make sense. I'm going to save my playbook and execute it to show the output. I need to tell Ansible playbook the inventory file to use and the playbook file to parse. Inventory is provided with the -i argument and the playbook is a freeform argument. Each play is displayed on screen and as each host completes each task, the result is displayed on screen as well. An implicit task is show, the setup task, which gathers facts about each host.