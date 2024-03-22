# mqtt_docker_broker
A Docker server for MQTT broker. Here are the files for setting up a simple MQTT Broker that provides user and password authentication with various roles. 

## Built With

* [Docker](https://www.docker.com) - To run the server in a container.
* [HiveMq](https://www.hivemq.com) - MQTT for IoT messaging.

# Getting Started

Notice that we have a [Dockerfile](Dockerfile) in our folder. In case of not understanding what is a [Dockerfile](Dockerfile) for.


*A Dockerfile is a text document that contains all of the commands that the user needs to run on the command line to build an image. A Dockerfile allows users to design an automated build that performs many command-line instructions in succession.*


<br>

Next we need to understand, what is the lines doing in the Docker file.

```
FROM hivemq/hivemq4:latest
```

This line is to pull and download the latest version of hivemq image to our local.

```
COPY config.xml /opt/hivemq/conf/config.xml
```

This is to copy the [config.xml](config.xml) to the container files, from which the container will be built later.

```
COPY hivemq-file-rbac-extension/ /opt/hivemq/extensions/hivemq-file-rbac-extension
```
This is to copy the extension that supports user and password authentication for MQTT, which we will download later also.

### First Step :
We will need to download the [HiveMQ Extension File RBAC extension](https://www.hivemq.com/products/extensions/file-rbac-extension/) file first. You can obtain the extension file from the website listed above, or you can download it by clicking [here](https://github.com/hivemq/hivemq-file-rbac-extension/releases/download/4.5.3/hivemq-file-rbac-extension-4.5.3.zip).

After that you will get a file name as **hivemq-file-rbac-extension**. Move the files into this project root folder. Such as:
```
mqtt_docker_broker
|___config.xml
|___Dockerfile
|___LICENSE.md
|___README.md
|___hivemq-file-rbac-extension (MOVE HERE!!)
```
If the file names differ from this, you must manually update the docker file when copying the files into the container.

### Second Step:
Build the images from the Dockerfile by running:
```
docker build -t [YOUR_IMAGE_NAME] [YOUR_DOCKERFILE_SRC_PATH]
```

**YOUR_IMAGE_NAME** - Any name you choose to give your Docker image. 

**YOUR_DOCKERFILE_SRC_PATH** - Your Dockerfile path, commonly known as the [Dockerfile] (Dockerfile) in the root project. ***\* Provide the entire path to the file from your system's root folder.***

Example:
```
docker build -t nexstream_mqqt /Users/zachary/java_mqqt_example_gui
```

<br>

After that, proceed to check if the images is ready by running 
```
docker images
```

If you see your image with your supplied name, it means that we will now just execute the images as a container in Docker by doing:

```
docker run -p 8080:8080 -p 1883:1883 [YOUR_IMAGE_NAME]
```

**YOUR_IMAGE_NAME** - The name where you provided earlier. 

The port 8080 is reserve for hiveMQ user console center, and the port 1883 is for the HiveMQ broker.

## FAQ
**Why we copy config.xml even though HiveMQ itself already have one inside conf folder?**

To support different configuration for the HiveMQ. Understand more by clicking the reference below :

- [General Configuration Information](https://docs.hivemq.com/hivemq/latest/user-guide/configuration.html)
- [Security](https://docs.hivemq.com/hivemq/latest/user-guide/security.html)

<br>

**How can i update the authentication username and password, including the roles?**

You'll have to manually navigate to the rbac extension we downloaded previously. Look for the ***credential.xml*** file and customise it to your liking. There is now a sample of two users and two roles.

<br>

**The container failed to run because of port 8080 is being used. What can I do?**

Just simply rerun your container by mapping to a different port.
```
docker run -p [YOUR_AVAILABLE_PORT]:8080 -p [YOUR_AVAILABLE_PORT]:1883 [YOUR_IMAGE_NAME]
```

The port in front comes from your device, while the port behind is for mapping to the container itself. Please do not alter the connection port unless you understand what you are doing.

## Authors

* **Zachary** - [YuLiangGoh](https://github.com/YuLiangGoh)

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details