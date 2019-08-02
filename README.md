## first-project

QDM is responsible in deploying QuizUp services and its basic operations are 
- Any service instance launched by ASG having a `docker-manager` init service. This service will pull the QDM image from the dockistry and launches `docker-manager` container
- docker-manager container will initiate the below containers and it gets images tags from zookeeper
  - datadog 
  - dockergc
  - service container(service name will be pulled from the instance metadata)
- QDM always check the state of the image in zookeeper. Whenever there was a change in the tag, QDM will get the latest image and deploys it.
- Other way to deploy a different image specifically in one instance is, add the tag in /quizup/service/docker_tag file. QDM will check this file first and if this file not present then it will get the image tag from zookeeper.

Till 2019 March, QDM code manages to deploy single service in an instance. 
