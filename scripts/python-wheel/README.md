# Pip installable wheel for Malmo #

Malmo can be built from source ([Build from source](https://github.com/Microsoft/malmo)) or easily installed from a pre-build version 
[Install from archive](https://github.com/Microsoft/malmo/releases)) it is often simpler to install Malmo as a python native platform wheel 
for Linux, Windows and MacOS.

In order to �pip3 install malmo� there some environment and dependency requirements:

1.	JAVA_HOME environment variable must be set up for Java8
2.	MALMO_XSD_PATH must be defined (see below)

3.	There are a few OS specific dependencies that must be pre-installed. 

    ..*	For Ubuntu Linux these are follows:

        git, python3-pip, openjdk-8-jdk, ffmpeg

        We�ll add more Linux flavours here but the Malmo/scripts/docker build scripts are a good place to start.

    ..*	Windows - please use the powershell scripts to install dependencies (python3, ffmpeg, 7zip, Java8). You also need to have git installed.

    ..*	MacOS  - please see [MacOs](ttps://github.com/Microsoft/malmo/blob/master/doc/install_macosx.md) You also need git.

If you are unsure of how to install for your Linux flavour, the Malmo docker build files might be a good place to start 

Rather than installing these dependencies manually it�s simper to launch a docker container using this docker image.

The docker image already has the Malmo Python wheel as well as the source code installed.

The docker container will launch the Malmo Minecraft Mod and a Jupyter server on start up, 
as well as allowing remove access via the VNC remote desktop display protocol so you can see the Minecraft game inside the container.

```docker run -it -p 5901:5901 -p 6901:6901 -p8888:8888 -e VNC_PW=vncpassword1 headless```

You can also add a ```-v drive:/somedir:/somedir``` to the docker run command to mount a directory for sharing your coding project files.

Then simply browse to http://localhost:6901/?password=vncpassword1 (or connect to port 5901 using a VNC client). 
Once Minecraft is launched (which can take some minutes) you should see it in the VNC desktop browser tab.

After launching Minecraft a Jupiter server is started and a connection advise hint is written on the docker container�s output. 
Please follow the advice to cut & paste the url into a browser but substituting ```localhost``` for ```0.0.0.0``` 
(bridging port 8888 to the docker container).

The advise looks something like this:

```
Copy/paste this URL into your browser when you connect for the first time,
    to login with a token:
        http://0.0.0.0:8888/?token=1c6390221431ca75146946c52e253f063431b6488420bbac
```
(Here you should have used: ```http://localhost:8888/?token=1c6390221431ca75146946c52e253f063431b6488420bbac```)

If you would rather install Malmo locally you can do that (after satisfying the OS & environment variable requirements) using:

```
pip3 install malmo
```

Create or change into a working directory where you would like Malmo to be installed and do:

```
python3 -c  'import malmo.minecraftbootstrap; malmo.minecraftbootstrap.download()'
```

This will create a new directory (called MalmoPlatform) containing the Malmo GitHub project in your (current) working directory.

Setup the MALMO_XSD_PATH environment variable to point to the MalmoPlatform/Schemas directory. 
i.e. full path of working dir and MalmoPlaftorm/Schema.

Alternatively you could set it in Python temporarily with a ```malmo.minecraftbootstrap.set_xsd_path();``` after the 
```import malmo.minecraftbootstrap;``` in python commands below.

You can now launch Minecraft from your working directory:

```
python3 -c  'import malmo.minecraftbootstrap; malmo.minecraftbootstrap.launch_minecraft()'
```

This may take some time (minutes if it�s the first run).

The malmo package includes a simple test mission which you can run as follows:

```
python3 -c 'from malmo.run_mission import run; run()'
```

(again add a ```malmo.minecraftbootstrap.set_xsd_path()``` statement if you have not set up the MALMO_XSD_PATH.)

You can also run the mission from Jupyter. Simply create a Python3 notebook and 
add ```from malmo.run_mission import run; run()``` to it and execute it.

To de-install delete the MalmoPlatform directory (and it�s contents) and do a ```pip3 uninstall malmo```.
