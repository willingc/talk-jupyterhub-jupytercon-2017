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

Enough talk, now time for demos! 

(Note: I'll make GIFs for all of the demos, and decide if we're going to do it
live or use GIFs only based on discussion with other folks presenting. I'm
leaning towards just using GIFs!)

Demo #1 - Zero to JupyterHub in 2 minutes!

(pre-create k8s cluster with ingress + kube-lego + helm + jupyterhub repo added)
(create config.yaml & do helm install)
1. create config.yaml that describes the installation we wanna have
   ```
   hub:
     cookieSecret: blah blah 
   proxy:
     secretToken: blah blabh
   ingress:
     enabled: true
     host: jupytercon.hubs.yuvi.in
   ```
2. Install the jupyterhub chart
   ```
   helm repo add jupyterhub https://jupyterhub.github.io/helm-chart
   helm repo update
   helm install jupyterhub/jupyterhub --name=jupytercon --namespace=jupytercon \
                 --version=v0.4.1 -f config.yaml
   ```
3. tada! Note that you get automatic TLS here!

Demo #2 - Upgrade

1. Modify config.yaml, add dummy password
   ```
   auth:
     dummy:
       password: yesthereisahousingcrisis
   ```
2. Upgrade!
   ```
   helm upgrade jupytercon jupyterhub/jupyterhub --version=v0.4.1 -f config.yaml
   ```
3. w00t! Let's try again - set singleuser image to a jupyter docker stacks
   image.
4. Upgrade, w00t!

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
