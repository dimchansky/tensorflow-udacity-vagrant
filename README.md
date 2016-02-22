# Running TensorFlow Docker image under Windows

This Vagrant configuration allows you to run TensorFlow Docker image (from the Udacity [Deep Learning course](https://www.udacity.com/course/viewer#!/c-ud730) authors) on your Windows machine and share the `assignments` folder on the Windows machine with the TensorFlow Docker container, so that all your work will stay on your Windows machine.

## Prerequisites

As Docker runs on Linux containers we would need to run a Linux Virtual Machine(VM) to be able to run Docker images. So first install [Vagrant](https://www.vagrantup.com/).

Before you run VM, make sure you have enough RAM and CPUs on your Windows machine. Current Vagrant configuration will use 2 CPUs and 8GB of RAM to run VM (Deep Learning course authors [recommend using 8G](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/examples/udacity/README.md)). You can fix these numbers in [Vagrantfile](Vagrantfile#L26).

Make sure the following ports are not occupied by any other process and are free to use by VM:

- 2222 - used to SSH into VM
- 6006 - reserved for [TensorBoard](https://www.tensorflow.org/versions/master/how_tos/summaries_and_tensorboard/index.html)
- 8888 - used by Jupyter Notebook

## Preparing image for first use

1. Run VM with the Docker image (all Vagrant commands need to be run from the folder you've cloned repository to):

   ```sh
   > vagrant up tensorflow-udacity
   ```

2. When VM is ready, open [http://localhost:8888/](http://localhost:8888/) in your browser.
3. At this point TensorFlow is almost ready, but you need to copy the assignments from Docker image to shared working folder (`assignments` folder on your Windows machine). So press the *New* button on the TensorFlow page in your browser and then press *Terminal*. Type the following commands in the *Terminal*:

   ```sh
   # cp -a /notebooks/. /assignments
   # chmod -R 666 /assignments
   # exit
   ```

   After this you can close the *Terminal* page and goto [http://localhost:8888/](http://localhost:8888/). Now you should see all the assignments and are ready to get to work.
   
## Shutdown/run the VM with the Docker image

### Shutdown

When you're done with you work you can destroy docker container and halt the VM:

```sh
> vagrant destroy tensorflow-udacity
> vagrant halt
```

Please note that all your files will stay in `./assignments` folder on your Windows machine.

### Run

Next time you can run Docker container in the VM just by running the command:

```sh
> vagrant up tensorflow-udacity
```

The second time it will start faster, because it's already configured. When VM is ready just open [http://localhost:8888/](http://localhost:8888/) in your browser and you'll see all your files from the assignments folder.

## Tips & tricks

### Logs

```sh
> vagrant docker-logs
```

command can be used to see the logs of a running TensorFlow Notebook. It can be useful to troubleshoot TensorFlow problems.

### SSH

You can SSH into the VM by running the command (you should have ssh.exe in your Windows path):

```sh
> ssh vagrant@localhost -p 2222
```

The password is `vagrant`.

After you SSH into the VM you can run `bash` in the TensorFlow docker container by running the command:

```sh
$ docker exec -it tensorflow-udacity bash
```

### Fully destroy the image of VM

The VM takes about 3GB on your disk (check your `%USERPROFILE%/VirtualBox VMs` folder). You can destroy this image when you're done with the course. All your assignments will stay in `./assignments` folder on your Windows machine.

```sh
> vagrant destroy
```

You can also remove vagrant box that had been used to create a VM. The following command will show you all the boxes installed on your machine:

```sh
> vagrant box list
```

You should see something like:

```
phusion/ubuntu-14.04-amd64 (virtualbox, 2014.04.30)
```

Run the following command to remove the `phusion/ubuntu-14.04-amd64` box:

```sh
> vagrant box remove phusion/ubuntu-14.04-amd64
```
