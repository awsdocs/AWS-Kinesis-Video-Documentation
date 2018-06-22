# Example: Streaming from an RTSP Source<a name="examples-rtsp"></a>

The [C\+\+ Producer Library](producer-sdk-cpp.md) contains a definition for a [Docker](https://www.docker.com/) container that connects to an RTSP \(Real Time Streaming Protocol\) network camera\. Using Docker standardizes the operating environment for Kinesis Video Streams, which greatly simplifies building and executing the application\.

To use the RTSP demo application, first install and build the [C\+\+ Producer Library](producer-sdk-cpp.md)\.

The following procedure demonstrates how to set up and use the RTSP demo application\.

**Topics**
+ [Prerequisites](#examples-rtsp-prerequisites)
+ [Build the Docker Image](#examples-rtsp-build)
+ [Run the RTSP Example Application](#examples-rtsp-procedure)

## Prerequisites<a name="examples-rtsp-prerequisites"></a>

To run the Kinesis Video Streams RTSP example application, you must have the following:
+ **Docker:** For information about installing and using Docker, see the following links:
  + [Docker download instructions](https://www.docker.com/community-edition#/download)
  + [Getting started with Docker](https://docs.docker.com/get-started/)
+ **RTSP network camera source:** For information about recommended cameras, see [System Requirements](system-requirements.md)\.

## Build the Docker Image<a name="examples-rtsp-build"></a>

First, you build the Docker image that the demo application will run inside\.

1. Create a new directory and copy the following files from the `docker_native_scripts` directory to the new directory:
   + `Dockerfile`
   + `start_rtsp_in_docker.sh`

1. Change to the directory that you created in the previous step\.

1. Build the Docker image using the following command\. This command creates the image and tags it as `rtspdockertest`\.

   ```
   docker build -t rtspdockertest .
   ```

1. Record the image ID that was returned in the previous step \(for example, *54f0d65f69b2*\)\.

## Run the RTSP Example Application<a name="examples-rtsp-procedure"></a>

Start the Kinesis Video Streams Docker container using the following command\. Provide the image ID from the previous step, your AWS credentials, the URL of your RTSP network camera, and the name of the Kinesis video stream to send the data\.

```
$ docker run -it <IMAGE_ID> <AWS_ACCESS_KEY_ID> <AWS_SECRET_ACCESS_KEY> <RTSP_URL> <STREAM_NAME>
```

To customize the application, comment or remove the `ENTRYPOINT` command in `Dockerfile`, and launch the container using the following command:

```
docker run -it <IMAGE_ID> bash
```

You are then prompted inside the Docker container to customize the sample application and start streaming\.