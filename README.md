
## Instructions to deploy to Online Minikube Cluster (*Prefferred method*)

We can use a ready made `Minikube Cluster` deployed on cloud for us, if we don't want to install a minikube cluster locally. It's fairly simple, just go to this **[link]** (*open in new browser window*) and click on the blue 'Launch Terminal' button as below:
<br/><br/>

![image](https://user-images.githubusercontent.com/12583640/127970605-76a8c1c1-8655-47ac-b8bf-b0bd775dde00.png)

<br/><br/>
Make it full screen and after the cluster is ready just execute below commands:

```markup

$ git clone https://github.com/ankur512512/ecosia.git

$ cd ecosia

$ ./build.sh

```

Most of the comments that we will see during the execution of this script are self-explanatory. This script automatically tests the working of project as well using `curl` command. You can still verify again using below command:

```markup
$ curl http://local.ecosia.org/tree

```

You should get a result like this:

```markup
{"myFavouriteTree": "tulsi"}
```

<br/><br/>
### Instructions to deploy to your own minikube cluster

Provided you have a minikube cluster and git already installed on your local machine. Please run again below commands.

```markup

$ git clone https://github.com/ankur512512/ecosia.git

$ cd ecosia

$ ./build.sh

```

Most of the comments that we will see during the execution of this script are self-explanatory. This script automatically tests the working of project as well using `curl` command. You can still verify again using below command:

```markup
$ curl http://local.ecosia.org/tree

```

You should get a result like this:

```markup
{"myFavouriteTree": "tulsi"}
```

### ***(Bonus)*** Another cool way to see everything it in github actions

This is the fully automated way of deploying and testing this whole project, without even needing to manually execute any command.
We will be using minikube from ``github-actions``. To see more details feel free to explore the `.github/workflows/mini.yaml` file.

First of all, you need to sign in to your github account and then fork this project to your own repository, so that you are able to run the github-actions workflow. Then follow these steps:

  1. Go to your forked repository and then click on `Actions` button for your repo. 
     
     ![image](https://user-images.githubusercontent.com/12583640/128131036-ecabee6e-1328-47e7-860e-c15c016de441.png)
     
     'Accept/Agree' to the prompt which you get to use github-actions workflow.  
     
  3. Click on the blue 'Minikube start and deploy' button. And then click on 'Run workflow' dropdown, keep the branch as 'master' and then again click on the green 'Run worklow' button as shown below:
  
     ![Inkedtemp_LI](https://user-images.githubusercontent.com/12583640/128132213-0a8c2efd-cc3d-43ed-a36f-a77a402d17a5.jpg)
  3. After that, click on 'Minikube start and deploy' and on the next screen, click on 'Deploying Minikube' button. Refer to below screenshots:

     ![image](https://user-images.githubusercontent.com/12583640/127964047-86398361-f6b1-46a8-bc62-354f3d34174c.png)
     ****
     ![image](https://user-images.githubusercontent.com/12583640/127964742-8846817d-8314-4043-9806-146b0f80f9d3.png)
  4. You will see all the different jobs there, expand any job you want to see the output of. For this assignment, we are mainly concerned about 'Run everything' job and          inside that 'build.sh' script output, as shown below:
     
     ![image](https://user-images.githubusercontent.com/12583640/127965781-bab6aa4c-c3a3-4eb8-b0ff-18b8d06a8c87.png)

     You will see the output of curl command at the end of this script.
  


     
## Explanation
   
### Coding a web server

 A simple web server is created in Python using flask on port `5000` with below features:

- Only accepts GET requests, on the URL "/tree". Refer to file `./docker/resources/server.py` for details.
- Returns a JSON response of the form: `{"myFavouriteTree":tulsi}`. I have set `tulsi` as my favourite tree/plant.
- Has two software tests: One for HTTP Status Code check and the other for content verification. Refer to `./docker/resources/test.py`

### Automating deployment

This part of the assignment consists of creating the necessary configuration files and build scripts which allows us to build and deploy our web server into a Kubernetes cluster with a single command:

- A Dockerfile to build and package our application. `./docker/Dockerfile` is used to build the image and `./docker/resources/start.sh` acts as the startup script for this      container.
- Kubernetes manifests to deploy the application into our minikube cluster are placed in `./templates` directory consisting of:

  - ***deployment.yaml***: To create the deployment with one replica. Please note the `imagePullPolicy` is set to `IfNotPresent`, so that it will pickup the local image first. If you want to use a remote docker registry, please push the image `ecosia:latest` to your repo and change the `image` tag and `imagePullPolicy` accordingly.
  - ***service.yaml***: To create a `clusterIP` service, targetting and exposing the port `5000` of the application.
  - ***ingress.yaml***: To create an `ingress` resource, which routes to our web server only on the following URL: http://local.ecosia.org/tree

- A build script `./build.sh` which, in a single command, takes care of building, packaging and deploying our application and configuring our minikube cluster. At the end it tests the ingress as well using curl command. Please feel free to go through this script to see the steps used to deploy and configure this project or use these steps to manually deploy the manifests individually.

**Version Info**: Tested and verified on "Minikube cluster on Cloud". (the very first method in this README.md file)

- minikube v1.8.1 on Ubuntu 18.04
- Kubernetes v1.17.3 on Docker 19.03.6

Also, tested and verified in "github-actions":
- minikube v1.22.0 on Ubuntu 20.04
- Kubernetes v1.20.0 on Docker 20.10.7

*Cheers !!* :smile: :smile: :smile:

[link]: https://kubernetes.io/docs/tutorials/hello-minikube/#create-a-minikube-cluster

