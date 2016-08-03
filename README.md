# Jupyter

Create the `parana/jupyter` Docker Image 

This **Dockerfile** is a [Automated build](https://hub.docker.com/r/parana/jupyter/) of [Docker Registry](https://hub.docker.com/).

## Building on boot2docker & Docker Machine

You need to configure swap space in boot2docker / Docker Machine prior the build:

1. Log into boot2docker / Docker Machine: `boot2docker ssh` or `docker-machine ssh default` (replace `default` if needed).
2. Create a file named `bootlocal.sh` in `/var/lib/boot2docker/` with the following content:

        #!/bin/sh

        SWAPFILE=/mnt/sda1/swapfile

        dd if=/dev/zero of=$SWAPFILE bs=1024 count=2097152
        mkswap $SWAPFILE && chmod 600 $SWAPFILE && swapon $SWAPFILE

3. Make this file executable: `chmod u+x /var/lib/boot2docker/bootlocal.sh`

After restarting boot2docker / Docker Machine, it will have increased swap size.

## How to use


```
docker run -i -t -h my-jupyter -p 8080:8080 -p 9999:9999 --rm parana/jupyter bash
```

The Container Bash shell will open and you can type:

```
# To run Jupyter Notebook, use the following command:
jupyter notebook --no-browser --port 9999 &
sleep 10
curl http://localhost:9999
```


### Using Jupyter Notebook

This Container have a Python 3.5.2 instalation provided by 
Continuum Analytics, Inc.

You can start a Jupyter Notebook server and interact with Anaconda via your 
browser:

```
docker run -i -t -p 8888:8888 continuumio/anaconda3 /bin/bash -c "/opt/conda/bin/conda install jupyter -y --quiet && mkdir /opt/notebooks && /opt/conda/bin/jupyter notebook --notebook-dir=/opt/notebooks --ip='*' --port=8888 --no-browser"
```

You can then view the Jupyter Notebook by opening http://localhost:8888 in 
your browser, or `http://<DOCKER-MACHINE-IP>:8888` if you are using a Docker 
Machine VM on macOS or Windows Operating Systems. `<DOCKER-MACHINE-IP>` is 
`localhost` if you are using the recently released version of Docker for macOS

Jupyter Notebook is a very useful tool if you need to create a live document with 
running code inside, much like Swift Playground avaiable on macOS / XCode.

