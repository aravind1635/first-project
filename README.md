## first-project

QDM is responsible in deploying QuizUp services and its basic operations are 
- Any service instance launched by ASG having a _docker-manager_ init service. This service will pull the QDM image from the dockistry and launches **docker-manager** container
- _docker-manager_ container will initiate the below containers and it gets images tags from zookeeper
  - datadog 
  - dockergc
  - log_shipper
  - service container(service name will be pulled from the instance metadata)
- QDM always check the state of the image in zookeeper. Whenever there was a change in the tag, QDM will get the latest image and deploys it.
- Other way to deploy a different image specifically in one instance is, add the tag in _/quizup/service/docker_tag_ file. QDM will check this file first and if this file not present then it will get the image tag from zookeeper.

Recent changes added to QDM are
- Earlier,QDM(image tag is stable) code manages to deploy a single service in a instance and now it is modified(image tag is stable-multiservice-deploy) to deploy multiple services(list of serivces mentioned in metadata) in a single instance
- Removed the log_shipper container as we are not using ELK anymore

How to test and Modify the QDM next time?
- Launch a test server in staging with sample metadata to deploy services
- Login to the **docker-manager** container and make changes in QDM code. Test the code by stopping the service, other containers and see the deployment
- Once the code is stabilized without disturbing the deployment in the test server, replicate the same changes in this repository
- Use Makefile to push the docker image of latest code with a different tag
- _docker-manager_ service init script code needs to modified to take the new tag in the base AMI of the services instance
