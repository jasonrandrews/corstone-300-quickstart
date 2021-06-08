# Software examples quickstart for Corstone-300 FVP

Below are a few software examples which can be run on the [Corstone-300 FVP](https://developer.arm.com/tools-and-software/open-source-software/arm-platforms-software/arm-ecosystem-fvps), some examples are for machine learning and use TensorFlow Lite Micro.

[Docker](https://docs.docker.com/get-docker/) is assumed to be installed. The examples run in a provided Docker container which includes the needed tools. Some examples use the Arm Compiler and some use gcc.

The [Arm Compiler](https://developer.arm.com/tools-and-software/embedded/arm-compiler) needs a license to run. The license is passed to the containers using an environment variable. The license can be post@host (floating). It can also be a file for a node-locked or any-host license.

## Simple getting started examples

These are simple, hello-world type examples to get started quickly. 

First, get the container from Docker Hub.

```bash
$ docker pull armswdev/arm-tools:corstone-300-fvp
```

Next, run the container. Set the ARMLMD_LICENSE_FILE to eithe port@host or a file path such as /home/ubuntu/license.dat

```bash
$ docker run --rm -it -e ARMLMD_LICENSE_FILE=port@host armswdev/arm-tools:corstone-300-fvp /bin/bash
```

Once inside the container, build and run a simple make based example.

```bash
$ git clone https://github.com/jasonrandrews/Corstone-300-hello.git
$ cd Corstone-300-hello/make
$ make
$ ./run.sh
```

The program output should be:
```console
Hello from Cortex-M55!

Sum is: 85344
```

Using the same GitHub project, build and run a simple [CMSIS-build](https://www.keil.com/pack/doc/CMSIS/Build/html/index.html) based example.

```bash
$ cd Corstone-300-hello/cbuild
$ cbuild.sh basic.Debug.cprj
$ ./run.sh
```

The program output should be:
```console
Basic CI example
```

The run.sh scripts contain the FVP command and parameters. The file ARMCM55_config.txt contains the parameters used.

To get a list of the options use:
```bash
$ FVP_Corstone_SSE-300_Ethos-U55 -h
```

The list of paramters which can be changed is large so write them to a file and look at the file.

```bash
$ FVP_Corstone_SSE-300_Ethos-U55 -l > paramfile
```

[GitHub software examples](https://github.com/jasonrandrews/Corstone-300-hello)

[Container on Docker Hub](https://hub.docker.com/r/armswdev/arm-tools)

[Files to build the Docker image](https://github.com/jasonrandrews/arm-tools)



## ML examples

The machine learning examples help to explain the Ethos-U55 and associated tools to prepare an application.

Get the containers from Docker Hub. There is a tag for gcc and a tag for Arm Compiler.

```bash
$ docker pull docker pull armswdev/tensorflow-lite-micro-rtos-fvp:gcc
$ docker pull docker pull armswdev/tensorflow-lite-micro-rtos-fvp:armclang
```

To run a FreeRTOS example using gcc (no license required).

```bash
$ docker run --rm -it -e ARMLMD_LICENSE_FILE=port@host armswdev/tensorflow-lite-micro-rtos-fvp:gcc /bin/bash 
```

Once in the container build the examples (this will take a little time).

```bash
$ ./linux_build_rtos_apps.sh -gcc
```

Run the person detection application.
```bash
$ ./run_demo_app.sh
```

It's also possible to run an example from the [ML Evaluation Toolkit](https://review.mlplatform.org/plugins/gitiles/ml/ethos-u/ml-embedded-evaluation-kit/+/HEAD/docs/documentation.md)

```bash
$ ./linux_build_eval_kit_apps.sh -gcc
```
The run details with docker are somewhat complex as the applications have an interactive prompt. 
The [README file](https://github.com/ARM-software/Tool-Solutions/blob/master/docker/tensorflow-lite-micro-rtos-fvp/README.md) is the best source of more details. 


One final note, there is an optional [docker_run.sh](https://github.com/ARM-software/Tool-Solutions/blob/master/docker/tensorflow-lite-micro-rtos-fvp/docker_run.sh) script which can be used directly to run. It has some options to specify the compiler and a shared folder and shows how to setup a Linux display.


[Container on Docker Hub](https://hub.docker.com/r/armswdev/tensorflow-lite-micro-rtos-fvp)

[Files to build the Docker image and use it](https://github.com/ARM-software/Tool-Solutions/tree/master/docker/tensorflow-lite-micro-rtos-fvp)
