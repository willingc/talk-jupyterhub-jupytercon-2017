# Outline for JupyterCon - DRAFT

- [5 min] [00:00 - 04:59] Intro (**Carol,** Min, Yuvi)
  - Welcome
  - What is JupyterHub? High level concept
  - Three teaser real world examples of JupyterHub’s use in the wild
    - Gateways - Science/HPC
    - Data Science research - Industry/Data/Finance
    - Education - Berkeley
  - Preview agenda for rest of talk
- [8 min] [05:00 - 12:59] State of [JupyterHub](https://github.com/jupyterhub/jupyterhub) (**Carol,** Min)
  - Basic architecture
    - Pre 0.7 release
  - 0.7 overview | [changelog](https://jupyterhub.readthedocs.io/en/latest/changelog.html) | [announcement](https://groups.google.com/forum/#!topic/jupyter/OHCW6UODQGE)
  - Services - addition in 0.7 (foundation for adding functionality and customizing deployment)
- [8 min] [13:00 - 20:59] JupyterHub 0.8 - What’s new (**Min,** Carol)
  - OAuth
  - Multiple Services per User
  - Proxy abstraction
  - JupyterLab supported
  - Related project: [nbgrader](https://github.com/jupyter/nbgrader): Using services and enhancements (beneficial for education and corporate training/workshops) - sharing and collaboration
- [8 min] [21:00 - 28:59] JupyterHub and Kubernetes (**Yuvi**)
  - Berkeley: Data8 architecture overview
  - Demo
  - Close - Hook the audience - try it yourself (Zero to JupyterHub with Kubernetes)
- [5 min] [29:00 - 33:59] Looking toward future release (0.9) (**Min**)
  - [HubShare](https://github.com/jupyterhub/hubshare)
- [2 min][34:00 - 35:59] Conclusion (**Carol**, Min, Yuvi)
  - Recap of JupyterHub 0.8 and future direction
  - Community - Next steps - Deploy (Zero to JupyterHub), share knowledge (mailing lists), contribute (authenticators, spawners, services, etc.)
  - Thank you!
- [4 min][36:00 - 40:00] Q&A Slide with link for more information (Carol, Min, Yuvi)
  - This is buffer for timing => If over time, a break follows so Q&A can be hallway track.


Abstract - points 

- JupyterHub is a multiuser server for Jupyter notebooks. 
- discuss exciting recent additions and future plans for the project, 
  - including sharing notebooks with students and collaborators
- With Release 0.7, JupyterHub has added the concept of services,
  - adds the ability to deploy new applications with JupyterHub
  - Services provide a foundation for adding functionality and customizing your deployment to your specific needs
- 0.8 Looking to the future, JupyterHub’s internal authentication implementation is moving to OAuth. Using OAuth as the default for internal authentication makes it easier to integrate and offer services that already support OAuth. 
- As researchers, educators, and industry professionals, you frequently collaborate with peers and publish data for others to use. 
- share JupyterHub’s use to develop sharing tools for publishing and collaborating on notebooks and to enhance the distribution and collection of notebooks as educational or work assignments using tools like nbgrader.

