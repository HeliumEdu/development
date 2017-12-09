# Deployment Configuration

These Ansible roles and configurations are used to deploy projects to the appropriate nodes in various environments.

Each section in the `hosts` file should have a corresponding `.yml` file under `group_vars` for that environments
deployment to work properly.

These deployment configurations currently have two accepted risks:

* Reliance on Git for deployment (lack of a guarantee that is being deployed is what was tested)
* Deployments are done onto live nodes (chance of downtime)

## Reliance on Git

The current deployment process has live nodes request the tag from Git and clone it onto their box. Not only does this
introduce a dependency on Git and access from an external server to the project, it also introduces risk. While the new
code to be deployed is cloned and prepared in a staging area before moving it into place (thus making it live), this
method leaves a chance of downtime for any release, likely lasting less than a few seconds.

The proposed solution to this is to generate artifacts when Git tags are created use those for deployment instead. This
also eliminates the risk of what you deploy potentially being different than what you tested (for instance, if the Git
tag changed, the clone failed or was corrupted, etc.).

The current blocker is cost constraints for infrastructure and the financial cost that is incurred by this
process. It is not prioritized at this stage of the project.

## Zero Downtime Releases

Even if artifacts are used (as described in the above section), without rolling deployments, there is still the chance
of downtime during the deployment window.

There are a few possible solutions to this. One is to implement a rigid blue/green deployment infrastructure. Another
more cost efficient method that could probably be implemented sooner would be to prebuild AMIs (or similar containers)
and utilize rolling deployments through an ELB. 

The current blocker is cost constraints for engineering resources, infrastructure and the financial cost that is
incurred by this process. It is not prioritized at this stage of the project.