# Berkeley Data Science Program

Hello everyone! I'm Yuvi Panda, Infrastructure Lead with the Berkeley Data Science Education Program. 

JupyterHub is a great way to get your students very quickly from 'I have a
browser' to 'I am writing code and exploring datasets!'. We at the Berkeley Data
Science Education Program believe this is a very important aspect of getting
datascience to everyone. Everyone should be able to set up & maintain
JupyterHubs of any size & complexity easily. On the cloud, in your campus data 
center, a bunch of raspberry pis - it should work anywhere. It should be as
cheap as possible, and require minimal upkeep - you wanna spend your time
teaching and doing data science, not debugging distributed systems running
JupyterHub.
 
The 'Zero to JupyterHub' guide is the result of our ongoing efforts at making 
all of these possible. It grew out of our experience building & maintaining a
1500+ user JupyterHub for our classes. We leverage industry standard open source
tools wherever possible - we use Kubernetes for its well designed cloud agnostic
high level abstractions, Helm the Kubernetes package manager for doing
deployments, and Docker for building images. The Kubernetes community has been
wonderful - it is a very diverse community that is innovating very fast, and we
are happy to consider ourselves part of it.

Enough talk, now time for demos! I've made animated GIFs of all my demos, because conference WiFi :)

Demo #1 - Zero to JupyterHub in 2 minutes!

0. Create a Kubernetes Cluster! We'll create a 3 node cluster with wth 14G of RAM each. This
   part is specific to your cloud provider - we'll use Google Cloud, but there are options for all
   popular cloud providers.

   gcloud container clusters create jupytercon-demo --num-nodes=3 --machine-type=n1-standard-2

1. create config.yaml that describes the installation we wanna have
   ```
   hub:
     cookieSecret: blah blah 
   proxy:
     secretToken: blah blabh
   ```
2. Install the jupyterhub chart
   ```
   helm repo add jupyterhub https://jupyterhub.github.io/helm-chart
   helm repo update
   helm install jupyterhub/jupyterhub --name=jupytercon --namespace=jupytercon \
                 --version=v0.4.1 -f config.yaml
   ```
3. Tada! In a few minutes you'll get an IP you can use to connect to your hub!

   ```
   watch kubectl --namespace=jupytercon get svc
   ```

Demo #2 - Upgrade

1. Modify config.yaml, add dummy password
   ```
   auth:
     dummy:
       password: yesthereisahousingcrisis
   ```
2. Upgrade!
   ```
   helm upgrade jupytercon jupyterhub/jupyterhub --version=v0.4 -f config.yaml
   ```
3. w00t! Let's try again - set singleuser image to a jupyter docker stacks
   image.
   ```
   singleuser:
      image:
         name: jupyter/r-notebook
         tag: 8f56e3c
   ```
4. Upgrade, w00t! Now we have our hub running a container image with R & Python!
   You can easily customize this by building your own images.

(Note: Pre-pull images earlier to make deployments faster)

Note that when we are upgrading, users who are already logged in see no
disruption! 

Demo #3: Resources

1. Give everyone 2G of RAM! This is a guarantee and a limit - they will always
   have upto 2G of RAM, but no more!
   ```
   singleuser:
     memory:
       requests: 2G
       limit: 2G
   ```
2. With 3 nodes of 14G RAM each, this means we can support upto 21 active users
   with this cluster. That is very easy to change! We can resize simply to add
   more nodes (use commandline to resize)
   
3. This is all fully automatable - there are several 'autoscaler'
   implementations that work to add / remove total number of nodes with demand.
   So it'll scale down at night when there are few people online, and scale back
   up during the day with more people coming on. Massive cost saver!
   

Demo #4: Possibile based on timing

All this automation allows us to set this up with the holy grail of
deployments - `continuous delivery`.
   
The entire state of our cluster at Berkeley is encapsulated in this git
repository. We can easily make a change simply by making a PR, and then merging
it on github - no commandlines necessary!

Let's say I wanna add a new library to the base image for users. It's as simple
as going in, editing the requirements.txt file, and then mergint the resulting
PR!

Travis will build the new image, push it, and deploy it. The same workflow works
for changing all other aspects of config!


# End #

Go forth and make more JupyterHubs! This is useful across education, research &
corporate setups. Come talk to us on Gitter or open issues on GitHub, and
patches are always welcome!
