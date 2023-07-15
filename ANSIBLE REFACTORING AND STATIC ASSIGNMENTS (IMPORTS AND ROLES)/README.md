### ANSIBLE REFACTORING AND STATIC ASSIGNMENTS (IMPORTS AND ROLES)
In this project you will continue working with ansible-config-mgt repository and make some improvements of your code. Now you need to refactor your Ansible code, create assignments, and learn how to use the imports functionality. Imports allow to effectively re-use previously created playbooks in a new playbook – it allows you to organize your tasks and reuse them when needed.

### For better understanding or Ansible artifacts re-use – read this article.

### Code Refactoring
Refactoring is a general term in computer programming. It means making changes to the source code without changing expected behaviour of the software. The main idea of refactoring is to enhance code readability, increase maintainability and extensibility, reduce complexity, add proper comments without affecting the logic.
In your case, you will move things around a little bit in the code, but the overall state of the infrastructure remains the same.
Let us see how you can improve your Ansible code!

### Step 1 – Jenkins job enhancement

Before we begin, let us make some changes to our Jenkins job – now every new change in the codes creates a separate directory which is not very convenient when we want to run some commands from one place. Besides, it consumes space on Jenkins serves with each subsequent change. Let us enhance it by introducing a new Jenkins project/job – we will require Copy Artifact plugin.

1. Go to your Jenkins-Ansible server and create a new directory called ansible-config-artifact – we will store there all artifacts after each build.

    `sudo mkdir /home/ubuntu/ansible-config-artifact`

2. Change permissions to this directory, so Jenkins could save files there – `chmod -R 0777 /home/ubuntu/ansible-config-artifact`

 ![chmod](./images/chmod.png)

3. Go to Jenkins web console -> Manage Jenkins -> Manage Plugins -> on Available tab search for Copy Artifact and install this plugin without restarting Jenkins

![plugin](./images/copy-artifact-plugin.png)

4. Create a new Freestyle projectand name it save_artifacts.

5. This project will be triggered by completion of your existing ansible project. Configure it accordingly:

![artifact](./images/save-artifact-1.png)

Note: You can configure number of builds to keep in order to save space on the server, for example, you might want to keep only last 2 or 5 build results. You can also make this change to your ansible job.

6. The main idea of save_artifacts project is to save artifacts into `/home/ubuntu/ansible-config-artifact` directory. To achieve this, create a Build step and choose Copy artifacts from other project, specify ansible as a source project and `/home/ubuntu/ansible-config-artifact` as a target directory.

![build](./images/build-steps.png)

7. Test your set up by making some change in README.MD file inside your ansible-config-mgt repository (right inside master branch).

![build](./images/build-successful.png)

If both Jenkins jobs have completed one after another – you shall see your files inside `/home/ubuntu/ansible-config-artifact` directory and it will be updated with every commit to your master branch. Now your Jenkins pipeline is more neat and clean.

![build](./images/home.png)

### Step 2 – Refactor Ansible code by importing other playbooks into site.yml

Before starting to refactor the codes, ensure that you have pulled down the latest code from master (main) branch, and created a new branch, name it refactor.

Let see code re-use in action by importing other playbooks.

1. Within playbooks folder, create a new file and name it site.yml – This file will now be considered as an entry point into the entire infrastructure configuration. Other playbooks will be included here as a reference. In other words, site.yml will become a parent to all other playbooks that will be developed. Including common.yml that you created previously. Dont worry, you will understand more what this means shortly.

2. Create a new folder in root of the repository and name it static-assignments. The static-assignments folder is where all other children playbooks will be stored. This is merely for easy organization of your work. It is not an Ansible specific concept, therefore you can choose how you want to organize your work. You will see why the folder name has a prefix of static very soon. For now, just follow along.

3.Move common.yml file into the newly created static-assignments folder.

4. Inside site.yml file, import common.yml playbook.
    
        ---
        - hosts: all
        - import_playbook: ../static-assignments/common.yml

The code above uses built in import_playbook Ansible module. Your folder structure should look like this;

    ├── static-assignments
    │   └── common.yml
    ├── inventory
        └── dev
        └── stage
        └── uat
        └── prod
    └── playbooks
        └── site.yml

   ![static](./images/static-assignments.png)

5. Run ansible-playbook command against the dev environment Since you need to apply some tasks to your dev servers and wireshark is already installed – you can go ahead and create another playbook under static-assignments and name it common-del.yml. In this playbook, configure deletion of wireshark utility.

        ---
        - name: update web and nfs server
        hosts: webservers, nfs
        remote_user: ec2-user
        become: yes
        become_user: root
        tasks:
        - name: delete wireshark
            yum:
            name: wireshark
            state: removed

        - name: update LB and db server
        hosts: lb, db
        remote_user: ubuntu
        become: yes
        become_user: root
        tasks:
        - name: delete wireshark
            apt:
            name: wireshark-qt
            state: absent
            autoremove: yes
            purge: yes
            autoclean: yes

6. update site.yml with - `import_playbook: ../static-assignments/common-del.yml` instead of common.yml and run it against dev servers.

7. Type the following commands:

`cd /home/ubuntu/ansible-config-artifact/`

`ansible-playbook -i inventory/dev.yml playbooks/site.yaml`